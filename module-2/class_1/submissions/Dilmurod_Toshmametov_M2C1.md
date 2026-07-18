# Python Warmup -- Module 2, Class 1

This notebook covers the Python fundamentals you need for data work:
- Variables and data types
- Functions
- Loops

Each section has examples first, then TODO exercises for you to complete.

---
## 1. Variables and Data Types

Python has four core data types you will use constantly:
- **int** -- whole numbers (subscriber count, age)
- **float** -- decimal numbers (bill amount, accuracy)
- **str** -- text (customer name, region)
- **bool** -- True or False (is_active, payment_on_time)
- **list** -- ordered collection (monthly bills, feature names)


```python
# --- EXAMPLES ---

# Integers
subscriber_count = 10_000_000
age = 25

# Floats
monthly_bill = 89_500.50
accuracy = 0.95

# Strings
customer_name = "Abdullayev Jasur"
region = "Tashkent"

# Booleans
is_active = True
payment_on_time = False

# Lists
monthly_bills = [85000, 92000, 78000, 95000, 88000]
regions = ["Tashkent", "Samarkand", "Bukhara", "Fergana"]

# Check types
print(f"subscriber_count: {subscriber_count} (type: {type(subscriber_count).__name__})")
print(f"monthly_bill:     {monthly_bill} (type: {type(monthly_bill).__name__})")
print(f"customer_name:    {customer_name} (type: {type(customer_name).__name__})")
print(f"is_active:        {is_active} (type: {type(is_active).__name__})")
print(f"monthly_bills:    {monthly_bills} (type: {type(monthly_bills).__name__})")
```


```python
# --- List operations ---

# Access by index (0-based)
print(f"First bill: {monthly_bills[0]}")
print(f"Last bill:  {monthly_bills[-1]}")

# Slicing
print(f"First 3 bills: {monthly_bills[:3]}")

# Length
print(f"Number of bills: {len(monthly_bills)}")

# Append
monthly_bills.append(91000)
print(f"After append: {monthly_bills}")
```


```python
# --- TODO: Variables exercise ---
# Create variables for a telecom customer profile:

# TODO: Create an integer variable for the customer's age
customer_age = 25

# TODO: Create a float variable for their monthly data usage in GB
data_usage_gb = 15.5

# TODO: Create a string variable for their city
city = 'Tashkent'

# TODO: Create a boolean for whether they have a contract (vs prepaid)
has_contract = False

# TODO: Create a list of their last 6 monthly bills (make up reasonable numbers in soum)
bills = [450_000, 350_000, 200_000, 300_000, 320_000, 330_000]

# TODO: Print all variables with their types (follow the example pattern above)

print(f"customer_age: {customer_age} (type: {type(customer_age).__name__})")
print(f"data_usage_gb:     {data_usage_gb} (type: {type(data_usage_gb).__name__})")
print(f"city:    {city} (type: {type(city).__name__})")
print(f"has_contract:        {has_contract} (type: {type(has_contract).__name__})")
print(f"bills:    {bills} (type: {type(bills).__name__})")


```

---
## 2. Dictionaries

A dictionary stores key-value pairs. Think of it as a single row in a database table -- keys are column names, values are the data.


```python
# --- EXAMPLE ---

customer = {
    "id": "UZT-001234",
    "name": "Karimov Sherzod",
    "region": "Tashkent",
    "monthly_spend": 125000,
    "is_active": True,
    "plan": "Premium"
}

# Access values by key
print(f"Customer: {customer['name']}")
print(f"Region:   {customer['region']}")
print(f"Spend:    {customer['monthly_spend']} soum")

# Add a new key
customer["tenure_months"] = 24
print(f"\nFull record: {customer}")
```


```python
from os import name
# --- TODO: Dictionary exercise ---
# Create a dictionary representing a product from an e-commerce store
# Include at least 5 keys: name, category, price, quantity_sold, rating

# TODO: Create the product dictionary
product = {
    "name": "Milk",
    "category": "Dairy",
    "price": 13000,
    "quantity_sold": 15,
    "rating": 4.8
}

# TODO: Print the product name and price
print(f"Product name: {product['name']}")
print(f"Product price: {product['price']} soum")
# TODO: Add a new key "discount" with a value between 0 and 1

product['discount'] = 0.5
# TODO: Print all keys using product.keys()
print(product.keys())


```

---
## 3. Functions

Functions take inputs, perform an operation, and return an output. They let you write reusable, modular code.

In ML, every step of a pipeline is typically a function: load_data(), clean_data(), train_model(), evaluate().


```python
# --- EXAMPLE: calculate_average ---

def calculate_average(numbers):
    """Calculate the average of a list of numbers."""
    total = sum(numbers)
    count = len(numbers)
    return total / count

bills = [85000, 92000, 78000, 95000, 88000, 91000]
avg_bill = calculate_average(bills)
print(f"Average monthly bill: {avg_bill:,.0f} soum")
```


```python
# --- EXAMPLE: function with multiple returns ---

def describe_bills(bills):
    """Return basic statistics for a list of bills."""
    avg = sum(bills) / len(bills)
    minimum = min(bills)
    maximum = max(bills)
    return avg, minimum, maximum

avg, low, high = describe_bills(bills)
print(f"Average: {avg:,.0f} | Min: {low:,.0f} | Max: {high:,.0f}")
```


```python
# --- TODO: Write your own functions ---

# TODO 1: Write a function called `classify_customer` that takes monthly_spend as input
#         and returns:
#         - "premium" if spend > 150000
#         - "standard" if spend is between 50000 and 150000 (inclusive)
#         - "basic" if spend < 50000


def classify_customer(monthly_spend):
    # TODO: implement this function
    if monthly_spend > 15000:
     return "premium"
    elif 50000 <= monthly_spend <= 150000:
      "standart"
    else:
        return "basic"

# Test your function:
# print(classify_customer(200000))  # should print "premium"
# print(classify_customer(75000))   # should print "standard"
# print(classify_customer(30000))   # should print "basic"
```


```python
# TODO 2: Write a function called `calculate_churn_rate` that takes two arguments:
#         - total_customers (int)
#         - churned_customers (int)
#         Returns the churn rate as a percentage (float).

def calculate_churn_rate(total_customers, churned_customers):
    # TODO: implement this function
    def calculate_churn_rate(total_customers, churned_customers):
      if total_customers == 0:
        return 0.0  # avoid division by zero
    
    return (churned_customers / total_customers) * 100

# Test:
# print(calculate_churn_rate(10000, 350))  # should print 3.5
```

---
## 4. Loops

Loops let you repeat an operation across a collection. In data work, you iterate over rows, columns, files, or parameter values constantly.


```python
# --- EXAMPLE: basic for loop ---

regions = ["Tashkent", "Samarkand", "Bukhara", "Fergana", "Namangan"]

for region in regions:
    print(f"Processing data for: {region}")
```


```python
# --- EXAMPLE: enumerate (index + value) ---
# enumerate gives you both the position and the value

monthly_bills = [85000, 92000, 78000, 95000, 88000, 91000]

for i, bill in enumerate(monthly_bills):
    print(f"Month {i + 1}: {bill:,} soum")
```


```python
# --- EXAMPLE: loop with conditional ---

customers = [
    {"name": "Alisher", "spend": 200000},
    {"name": "Dilnoza", "spend": 45000},
    {"name": "Bobur",   "spend": 120000},
    {"name": "Malika",  "spend": 35000},
    {"name": "Rustam",  "spend": 180000},
]

print("High-value customers (spend > 100,000):")
for c in customers:
    if c["spend"] > 100000:
        print(f"  {c['name']}: {c['spend']:,} soum")
```


```python

# --- TODO: Loop exercises ---

# TODO 1: Using the `customers` list above, loop through and classify each customer
#         using your classify_customer function from Section 3.
#         Print: "Name: <name>, Tier: <tier>"
for c in customers:
    tier = classify_customer(c["spend"])
    print(f"Name: {c['name']}, Tier: {tier}")
# Your code here:
```


```python
# TODO 2: Given the list of scores below, use enumerate to print each score
#         with its rank (starting from 1), and mark scores above 90 with " -- HIGH"
#
# Expected output:
#   Rank 1: 72
#   Rank 2: 95 -- HIGH
#   Rank 3: 88
#   ...

scores = [72, 95, 88, 61, 93, 77, 85, 91, 68, 99]

# Your code here:
for index, score in enumerate(scores, start=1):
    if score > 90:
        print(f"Rank {index}: {score} -- HIGH")
    else:
        print(f"Rank {index}: {score}")
```


```python
# TODO 3: Write a loop that computes the total and average of the scores list.
#         Do NOT use sum() or len() -- compute manually with a loop.
#         Print: "Total: <total>, Average: <average>"

# Your code here:total = 0
count = 0

for score in scores:
    total += score
    count += 1

average = total / count

print(f"Total: {total}, Average: {average}")

```

---
## 5. Putting It All Together

Combine variables, functions, and loops to solve a mini data task.


```python
# --- TODO: Final exercise ---
#
# You have a dataset of 8 customers (list of dictionaries).
# Write code that:
# 1. Loops through each customer
# 2. Classifies them into tiers (use classify_customer or write inline logic)
# 3. Counts how many customers are in each tier
# 4. Prints a summary: "Premium: X, Standard: Y, Basic: Z"

customers_data = [
    {"id": 1, "name": "Anvar",   "monthly_spend": 210000},
    {"id": 2, "name": "Nilufar", "monthly_spend": 55000},
    {"id": 3, "name": "Sardor",  "monthly_spend": 180000},
    {"id": 4, "name": "Gulnora", "monthly_spend": 32000},
    {"id": 5, "name": "Timur",   "monthly_spend": 95000},
    {"id": 6, "name": "Madina",  "monthly_spend": 160000},
    {"id": 7, "name": "Otabek",  "monthly_spend": 47000},
    {"id": 8, "name": "Zarina",  "monthly_spend": 125000},
]

# Your code here:
premium = 0
standard = 0
basic = 0

for c in customers_data:
    spend = c["monthly_spend"]
    
    if spend > 150000:
        premium += 1
    elif 50000 <= spend <= 150000:
        standard += 1
    else:
        basic += 1

print(f"Premium: {premium}, Standard: {standard}, Basic: {basic}")

```
