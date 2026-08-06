
# 183. Customers Who Never Order

**Problem Link:** https://leetcode.com/problems/customers-who-never-order/

---

# Problem Overview

This problem requires finding **customers who have never placed an order**.

The `Customers` table contains information about all customers, while the `Orders` table contains only customers who have placed at least one order.

The goal is to return the names of customers who **do not have any matching record** in the `Orders` table.

---

# Why LEFT JOIN?

The problem asks us to return **all customers**, including those who have never placed an order.

A **LEFT JOIN** keeps every row from the `Customers` table.

* If a customer has placed an order, a matching row is returned.
* If a customer has never placed an order, the columns from the `Orders` table become **NULL**.

By filtering these `NULL` values, we can identify customers who never ordered.

---

# Database Tables

## Customers

| id | name  |
| -- | ----- |
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |

## Orders

| id | customerId |
| -- | ---------- |
| 1  | 3          |
| 2  | 1          |

---

# Relationship Between Tables

The relationship between the two tables is:

```text
Customers.id = Orders.customerId
```

* `Customers.id` is the **Primary Key**.
* `Orders.customerId` is the **Foreign Key**.
* These columns are used to connect the two tables.

---

# Result After LEFT JOIN

| Customer ID | Name  | customerId |
| ----------- | ----- | ---------- |
| 1           | Joe   | 1          |
| 2           | Henry | NULL       |
| 3           | Sam   | 3          |
| 4           | Max   | NULL       |

Notice that **Henry** and **Max** have `NULL` because no matching order exists.

---

# Expected Result

| Customers |
| --------- |
| Henry     |
| Max       |

---

# SQL Solution

```sql
SELECT
    c.name AS Customers
FROM Customers c
LEFT JOIN Orders o
ON c.id = o.customerId
WHERE o.customerId IS NULL;
```

---

# Solution Breakdown

### Step 1: SELECT

```sql
SELECT
    c.name AS Customers
```

Returns the customer name.

The alias **Customers** is required because it matches the expected output column name.

---

### Step 2: FROM

```sql
FROM Customers c
```

Starts with the `Customers` table because we need to check **every customer**.

---

### Step 3: LEFT JOIN

```sql
LEFT JOIN Orders o
```

Combines the `Customers` table with the `Orders` table while keeping all customers.

---

### Step 4: ON

```sql
ON c.id = o.customerId
```

Matches each customer with their orders using the customer ID.

---

### Step 5: WHERE

```sql
WHERE o.customerId IS NULL;
```

Filters only customers who do not have a matching order.

If a customer has never placed an order, the `customerId` from the `Orders` table becomes `NULL`, so that customer is included in the final result.

---

# Time Complexity

**Time Complexity:** `O(n)`

The database scans the `Customers` table and joins it with the `Orders` table using the indexed `customerId` column. Filtering `NULL` values is performed during query execution.

---

# Key Concepts

* SELECT
* LEFT JOIN
* WHERE
* NULL
* Primary Key
* Foreign Key
* Table Relationships
* Filtering Missing Records
