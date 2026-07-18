---
jupyter:
  colab:
  kernelspec:
    display_name: Python 3
    name: python3
  language_info:
    name: python
  nbformat: 4
  nbformat_minor: 0
---

::: {.cell .markdown id="2uhxtHagM5dI"}
# Data Preparation Lab \-- Module 2, Class 2

**Dataset:** Superstore Sales

In this lab you will:

1.  Load and inspect data
2.  Handle missing values
3.  Remove duplicates
4.  Convert data types
5.  Create derived features

The first 3 tasks are pre-built. The rest are TODO for you.
:::

::: {.cell .markdown id="yanYi8r2M5dK"}

------------------------------------------------------------------------

## Setup: Load the Dataset

Option A: Upload from Kaggle (download from
<https://www.kaggle.com/datasets/vivek468/superstore-dataset-final>)

Option B: Use the URL loader below.
:::

::: {.cell .code execution_count="3" colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":741,\"status\":\"ok\",\"timestamp\":1777023597649,\"user\":{\"displayName\":\"Abu Muslim\",\"userId\":\"05575646951325550270\"},\"user_tz\":-300}" id="E2bKUyWaM5dL" outputId="6bd5d16f-07dd-4cda-f434-8c0f7d5f5eda"}
``` python
import pandas as pd
import numpy as np
#importing libraries

# Option A: Upload in Colab
# from google.colab import files
# uploaded = files.upload()  # upload your CSV
# df = pd.read_csv('SampleSuperstore.csv')

# Option B: Load from a public URL
# If the URL does not work, use Option A.
url = "https://raw.githubusercontent.com/leonism/sample-superstore/master/data/superstore.csv"
#getting data set from a link
try:
    df = pd.read_csv(url, encoding='latin-1')
    # assigning data set to df
    # prints number of rows and columns if loaded successfully
    print(f"Loaded from URL: {df.shape[0]} rows, {df.shape[1]} columns")
except Exception as e:
    # gives error message if loading failed
    print(f"URL failed ({e}). Use Option A: upload the CSV manually.")
    # Fallback: upload manually
    # uploading dataset manually
    from google.colab import files
    uploaded = files.upload()
    filename = list(uploaded.keys())[0]
    df = pd.read_csv(filename, encoding='latin-1')
    print(f"Loaded from upload: {df.shape[0]} rows, {df.shape[1]} columns")
```

::: {.output .stream .stdout}
    Loaded from URL: 10800 rows, 21 columns
:::
:::

::: {.cell .markdown id="B2IRzb0BM5dM"}

------------------------------------------------------------------------

## Task 1: Inspect the Data (pre-built)

Always look at your data before doing anything to it.
:::

::: {.cell .code execution_count="4" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":985}" executionInfo="{\"elapsed\":87,\"status\":\"ok\",\"timestamp\":1777023600582,\"user\":{\"displayName\":\"Abu Muslim\",\"userId\":\"05575646951325550270\"},\"user_tz\":-300}" id="P47O6aXGM5dM" outputId="4c9f132a-cea8-4d8a-f9c6-465161a6b4f7"}
``` python
# First 5 rows
#df.head() is by default shows 5 rows
df.head(10)
```

::: {.output .execute_result execution_count="4"}
``` json
{"type":"dataframe","variable_name":"df"}
```
:::
:::

::: {.cell .code execution_count="5" colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":12,\"status\":\"ok\",\"timestamp\":1777023605179,\"user\":{\"displayName\":\"Abu Muslim\",\"userId\":\"05575646951325550270\"},\"user_tz\":-300}" id="r1rcew6ZM5dN" outputId="4ad00c87-0f21-48ee-996b-926b9d774ac6"}
``` python
# Shape: rows x columns
#shows all data
print(f"Shape: {df.shape[0]} rows, {df.shape[1]} columns")
```

::: {.output .stream .stdout}
    Shape: 10800 rows, 21 columns
:::
:::

::: {.cell .code execution_count="6" colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":37,\"status\":\"ok\",\"timestamp\":1777023609627,\"user\":{\"displayName\":\"Abu Muslim\",\"userId\":\"05575646951325550270\"},\"user_tz\":-300}" id="yd2Lg_oAM5dN" outputId="9e37e5ff-94dd-4922-f227-951eeda34a4d"}
``` python
# Data types and non-null counts
df.info()
```

::: {.output .stream .stdout}
    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 10800 entries, 0 to 10799
    Data columns (total 21 columns):
     #   Column         Non-Null Count  Dtype  
    ---  ------         --------------  -----  
     0   Row ID         10800 non-null  object 
     1   Order ID       10800 non-null  object 
     2   Order Date     9994 non-null   object 
     3   Ship Date      9994 non-null   object 
     4   Ship Mode      9994 non-null   object 
     5   Customer ID    9994 non-null   object 
     6   Customer Name  9994 non-null   object 
     7   Segment        9994 non-null   object 
     8   Country        9994 non-null   object 
     9   City           9994 non-null   object 
     10  State          9994 non-null   object 
     11  Postal Code    9983 non-null   float64
     12  Region         9994 non-null   object 
     13  Product ID     9994 non-null   object 
     14  Category       9994 non-null   object 
     15  Sub-Category   9994 non-null   object 
     16  Product Name   9994 non-null   object 
     17  Sales          9994 non-null   float64
     18  Quantity       9994 non-null   float64
     19  Discount       9994 non-null   float64
     20  Profit         9994 non-null   float64
    dtypes: float64(5), object(16)
    memory usage: 1.7+ MB
:::
:::

::: {.cell .code execution_count="7" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":300}" executionInfo="{\"elapsed\":68,\"status\":\"ok\",\"timestamp\":1777023613127,\"user\":{\"displayName\":\"Abu Muslim\",\"userId\":\"05575646951325550270\"},\"user_tz\":-300}" id="sjdKsY-kM5dN" outputId="3c98e99b-7a18-4def-b590-551282c1edd6"}
``` python
# Summary statistics for numerical columns
df.describe()
```

::: {.output .execute_result execution_count="7"}
``` json
{"summary":"{\n  \"name\": \"df\",\n  \"rows\": 8,\n  \"fields\": [\n    {\n      \"column\": \"Postal Code\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 35894.261900444144,\n        \"min\": 1040.0,\n        \"max\": 99301.0,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          55245.23329660423,\n          57103.0,\n          9983.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Sales\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 8197.010918685499,\n        \"min\": 0.444,\n        \"max\": 22638.48,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          229.85800083049827,\n          54.489999999999995,\n          9994.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Quantity\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 3531.848471644344,\n        \"min\": 1.0,\n        \"max\": 9994.0,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          3.789573744246548,\n          3.0,\n          9994.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Discount\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 3533.3336684667293,\n        \"min\": 0.0,\n        \"max\": 9994.0,\n        \"num_unique_values\": 6,\n        \"samples\": [\n          9994.0,\n          0.15620272163297977,\n          0.8\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"Profit\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 5288.326642672474,\n        \"min\": -6599.978,\n        \"max\": 9994.0,\n        \"num_unique_values\": 8,\n        \"samples\": [\n          28.65689630778467,\n          8.6665,\n          9994.0\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}","type":"dataframe"}
```
:::
:::

::: {.cell .markdown id="FoM_zY2vM5dO"}

------------------------------------------------------------------------

## Task 2: Missing Values (pre-built)

Check which columns have missing values and how many.
:::

::: {.cell .code execution_count="8" colab="{\"base_uri\":\"https://localhost:8080/\",\"height\":645}" executionInfo="{\"elapsed\":42,\"status\":\"ok\",\"timestamp\":1777023678276,\"user\":{\"displayName\":\"Abu Muslim\",\"userId\":\"05575646951325550270\"},\"user_tz\":-300}" id="EAD0PBO3M5dO" outputId="ad5081fc-be6e-4f1c-d395-c6909bce5fca"}
``` python
# Count missing values per column
missing = df.isnull().sum()
missing_pct = (df.isnull().sum() / len(df) * 100).round(2)

missing_report = pd.DataFrame({
    'missing_count': missing,
    'missing_pct': missing_pct
})
#showing number of columns where a values are missing and how many
# Show only columns with missing values
missing_report[missing_report['missing_count'] > 0]
```

::: {.output .execute_result execution_count="8"}
``` json
{"summary":"{\n  \"name\": \"missing_report[missing_report['missing_count'] > 0]\",\n  \"rows\": 19,\n  \"fields\": [\n    {\n      \"column\": \"missing_count\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 2,\n        \"min\": 806,\n        \"max\": 817,\n        \"num_unique_values\": 2,\n        \"samples\": [\n          817,\n          806\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    },\n    {\n      \"column\": \"missing_pct\",\n      \"properties\": {\n        \"dtype\": \"number\",\n        \"std\": 0.0229415733870561,\n        \"min\": 7.46,\n        \"max\": 7.56,\n        \"num_unique_values\": 2,\n        \"samples\": [\n          7.56,\n          7.46\n        ],\n        \"semantic_type\": \"\",\n        \"description\": \"\"\n      }\n    }\n  ]\n}","type":"dataframe"}
```
:::
:::

::: {.cell .code execution_count="10" colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":39,\"status\":\"ok\",\"timestamp\":1777023904290,\"user\":{\"displayName\":\"Abu Muslim\",\"userId\":\"05575646951325550270\"},\"user_tz\":-300}" id="EbwBSWxBM5dO" outputId="0c08b1ec-b6ab-427a-858e-b0515f484403"}
``` python
# Fill missing numerical values with median (robust to outliers)

numerical_cols = df.select_dtypes(include=[np.number]).columns
for col in numerical_cols:
    if df[col].isnull().sum() > 0:
        median_val = df[col].median()
        df[col].fillna(median_val, inplace=True)
        print(f"Filled {col} missing values with median: {median_val}")

# Fill missing categorical values with mode
categorical_cols = df.select_dtypes(include=['object']).columns
for col in categorical_cols:
    if df[col].isnull().sum() > 0:
        mode_val = df[col].mode()[0]
        df[col].fillna(mode_val, inplace=True)
        print(f"Filled {col} missing values with mode: {mode_val}")

# Verify: no more missing values
print(f"\nTotal missing values remaining: {df.isnull().sum().sum()}")
```

::: {.output .stream .stdout}

    Total missing values remaining: 0
:::
:::

::: {.cell .markdown id="MUjCwGqtM5dP"}

------------------------------------------------------------------------

## Task 3: Remove Duplicates (pre-built)
:::

::: {.cell .code execution_count="11" colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":72,\"status\":\"ok\",\"timestamp\":1777024062822,\"user\":{\"displayName\":\"Abu Muslim\",\"userId\":\"05575646951325550270\"},\"user_tz\":-300}" id="c1hUqYnxM5dP" outputId="9b92ddac-4dd9-4420-a024-900683256201"}
``` python
# Check for duplicates
n_duplicates = df.duplicated().sum()
print(f"Duplicate rows found: {n_duplicates}")

# Remove duplicates
if n_duplicates > 0:
    df = df.drop_duplicates()
    print(f"After removal: {df.shape[0]} rows remain")
else:
    print("No duplicates to remove.")
```

::: {.output .stream .stdout}
    Duplicate rows found: 504
    After removal: 10296 rows remain
:::
:::

::: {.cell .markdown id="XotDZN4zM5dP"}

------------------------------------------------------------------------

## Task 4: Convert Date Columns (TODO)

The `Order Date` and `Ship Date` columns are stored as strings. Convert
them to proper datetime objects.

Hint: Use `pd.to_datetime()`. After conversion, verify with `.dtypes`.
:::

::: {.cell .code execution_count="12" colab="{\"base_uri\":\"https://localhost:8080/\"}" executionInfo="{\"elapsed\":49,\"status\":\"ok\",\"timestamp\":1777024240019,\"user\":{\"displayName\":\"Abu Muslim\",\"userId\":\"05575646951325550270\"},\"user_tz\":-300}" id="EojalWbkM5dP" outputId="01b0580f-e152-4c3d-81f8-3d528553eda1"}
``` python
# Check current types of date columns
print("Before conversion:")
for col in df.columns:
    if 'date' in col.lower() or 'Date' in col:
        print(f"  {col}: {df[col].dtype}")
        print(f"  Sample value: {df[col].iloc[0]}")
```

::: {.output .stream .stdout}
    Before conversion:
      Order Date: object
      Sample value: 11/8/2017
      Ship Date: object
      Sample value: 11/11/2017
:::
:::

::: {.cell .code id="YZwOkRC2M5dQ"}
``` python
# TODO: Convert date columns to datetime
# Look at the column names printed above and convert them.
# The column names may vary depending on the dataset version.
#
# Example pattern:
# df['Column Name'] = pd.to_datetime(df['Column Name'])

# Your code here:
```
:::

::: {.cell .code id="SGut3SFeM5dQ"}
``` python
# TODO: Verify the conversion worked
# Print dtypes for the date columns to confirm they are datetime64

# Your code here:
```
:::

::: {.cell .markdown id="dkXpeGYSM5dQ"}

------------------------------------------------------------------------

## Task 5: Derived Features (TODO)

Create customer-level summary features. These are the building blocks
for customer segmentation (Activity 4).

You need to create:

-   **total_spending**: Total sales per customer
-   **order_frequency**: Number of orders per customer
-   **avg_order_value**: Average sales amount per order per customer

Hint: Use `df.groupby('Customer ID')` (or whatever the customer ID
column is named).
:::

::: {.cell .code id="8DyF2Mt0M5dQ"}
``` python
# First, identify the right column names
print("All columns:")
for col in df.columns:
    print(f"  {col}")
```
:::

::: {.cell .code id="FMdS7IFsM5dR"}
``` python
# TODO: Create total_spending per customer
# Hint: df.groupby('Customer ID')['Sales'].sum()
#
# Replace column names below with the actual names from your dataset.

# customer_spending = df.groupby('???')['???'].sum()
# customer_spending.name = 'total_spending'

# Your code here:
```
:::

::: {.cell .code id="XhgljgUCM5dR"}
``` python
# TODO: Create order_frequency per customer
# Hint: Count the number of rows (orders) per customer.
# Use .groupby(...).size() or .groupby(...)['some_col'].count()

# Your code here:
```
:::

::: {.cell .code id="OGHqnnCtM5dR"}
``` python
# TODO: Create avg_order_value per customer
# Hint: Use .groupby(...)['Sales'].mean()

# Your code here:
```
:::

::: {.cell .code id="l2xYppK3M5dR"}
``` python
# TODO: Combine all three into a single customer-level DataFrame
# Hint: Use pd.concat([series1, series2, series3], axis=1)
# or create them all at once with .groupby(...).agg(...)

# customer_summary = pd.concat([customer_spending, order_freq, avg_order], axis=1)
# customer_summary.columns = ['total_spending', 'order_frequency', 'avg_order_value']

# Your code here:
```
:::

::: {.cell .code id="rgm-r01PM5dS"}
``` python
# TODO: Display the first 10 rows of your customer summary and .describe()

# Your code here:
```
:::

::: {.cell .markdown id="8WwHUp2PM5dS"}

------------------------------------------------------------------------

## Task 6: Save Cleaned Data (TODO)

Save the cleaned DataFrame to a new CSV file. Never overwrite the
original.
:::

::: {.cell .code id="6u3ZzSchM5dS"}
``` python
# TODO: Save the cleaned main DataFrame
# df.to_csv('superstore_cleaned.csv', index=False)

# TODO: Save the customer summary DataFrame
# customer_summary.to_csv('customer_summary.csv')

# Your code here:
```
:::

::: {.cell .markdown id="1al-mI6OM5dS"}

------------------------------------------------------------------------

## Reflection

Answer these in a text cell or comments:

1.  Why did we use median instead of mean for filling numerical missing
    values?
2.  What is the difference between the row-level DataFrame (one row per
    order) and the customer-level summary? When would you use each?
3.  If two rows have identical values in every column, are they always
    true duplicates? When might they not be?
:::
