# 1795. Rearrange Products Table

**Problem Link:** https://leetcode.com/problems/rearrange-products-table/

---

# Problem Overview

The `Products` table stores product prices in different stores.

Each store has its own column:

* `store1`
* `store2`
* `store3`

The goal is to transform the table from a **wide format** into a **normal table format**.

Instead of having each store as a separate column, we need:

* `product_id`
* `store`
* `price`

Only include rows where the product has a price in that store.

---

# Database Table

## Products Table

| product_id | store1 | store2 | store3 |
| ---------- | ------ | ------ | ------ |
| 0          | 95     | 100    | 105    |
| 1          | 70     | NULL   | 80     |

---

# Expected Result

| product_id | store  | price |
| ---------- | ------ | ----- |
| 0          | store1 | 95    |
| 0          | store2 | 100   |
| 0          | store3 | 105   |
| 1          | store1 | 70    |
| 1          | store3 | 80    |

---

# Explanation

The original table stores each store as a column:

```
product_id | store1 | store2 | store3
```

We need to convert it into rows:

```
product_id | store  | price
```

Example:

Before:

| product_id | store1 | store2 |
| ---------- | ------ | ------ |
| 0          | 95     | 100    |

After:

| product_id | store  | price |
| ---------- | ------ | ----- |
| 0          | store1 | 95    |
| 0          | store2 | 100   |

---

# New Concepts

This problem introduces:

* `UNION ALL`
* Column Aliases
* Filtering NULL values

---

# UNION ALL

`UNION ALL` combines the results of multiple SELECT statements.

Syntax:

```sql
SELECT column1, column2
FROM table1

UNION ALL

SELECT column1, column2
FROM table2;
```

Example:

Query 1:

| id |
| -- |
| 1  |
| 2  |

Query 2:

| id |
| -- |
| 3  |
| 4  |

Using:

```sql
SELECT id FROM A
UNION ALL
SELECT id FROM B;
```

Result:

| id |
| -- |
| 1  |
| 2  |
| 3  |
| 4  |

---

# Difference Between UNION and UNION ALL

## UNION

Removes duplicate rows.

```sql
SELECT id FROM A
UNION
SELECT id FROM B;
```

---

## UNION ALL

Keeps all rows, including duplicates.

```sql
SELECT id FROM A
UNION ALL
SELECT id FROM B;
```

In this problem we use `UNION ALL` because every store price is a separate record.

---

# Column Alias

Alias gives a temporary name to a column.

Example:

```sql
SELECT store1 AS price
FROM Products;
```

The output column will be named:

```
price
```

---

# IS NOT NULL

Used to exclude empty values.

Example:

```sql
WHERE store1 IS NOT NULL
```

Means:

Return only products that have a price in `store1`.

---

# SQL Solution

```sql
SELECT 
    product_id,
    'store1' AS store,
    store1 AS price
FROM Products
WHERE store1 IS NOT NULL

UNION ALL

SELECT 
    product_id,
    'store2' AS store,
    store2 AS price
FROM Products
WHERE store2 IS NOT NULL

UNION ALL

SELECT 
    product_id,
    'store3' AS store,
    store3 AS price
FROM Products
WHERE store3 IS NOT NULL;
```

---

# Solution Breakdown

## First SELECT

```sql
SELECT 
    product_id,
    'store1' AS store,
    store1 AS price
```

Creates rows for `store1`.

Example:

Input:

| product_id | store1 |
| ---------- | ------ |
| 0          | 95     |

Output:

| product_id | store  | price |
| ---------- | ------ | ----- |
| 0          | store1 | 95    |

---

## Second SELECT

```sql
SELECT 
    product_id,
    'store2' AS store,
    store2 AS price
```

Creates rows for `store2`.

---

## Third SELECT

```sql
SELECT 
    product_id,
    'store3' AS store,
    store3 AS price
```

Creates rows for `store3`.

---

## WHERE Condition

Example:

```sql
WHERE store2 IS NOT NULL
```

Prevents returning rows where the product does not exist in that store.

Example:

```
store2 = NULL
```

will not be included.

---

# Why Do We Use UNION ALL?

Because we are combining three different store columns into one column:

```
store1
store2
store3
```

Each SELECT creates a part of the final result, and `UNION ALL` combines them.

---

# Time Complexity

**Time Complexity:** `O(n)`

The database scans the table three times, once for each store column.

---

# Key Concepts

* SELECT
* UNION ALL
* Alias AS
* IS NOT NULL
* Data Transformation
* Wide Table to Normalized Table
