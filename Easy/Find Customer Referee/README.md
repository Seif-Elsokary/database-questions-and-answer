# 584. Find Customer Referee

**Problem Link:** https://leetcode.com/problems/find-customer-referee/

---

# Problem Overview

The `Customer` table stores each customer's information along with the ID of the customer who referred them.

The goal is to return the names of customers who:

* Were **not referred** by the customer with `id = 2`.
* **Were not referred by anyone**, meaning `referee_id` is `NULL`.

---

# Database Table

| id | name | referee_id |
| -- | ---- | ---------- |
| 1  | Will | NULL       |
| 2  | Jane | NULL       |
| 3  | Alex | 2          |
| 4  | Bill | NULL       |
| 5  | Zack | 1          |
| 6  | Mark | 2          |

---

# Expected Result

| name |
| ---- |
| Will |
| Jane |
| Bill |
| Zack |

---

# SQL Solution

```sql
SELECT
    name
FROM Customer
WHERE referee_id <> 2
   OR referee_id IS NULL;
```

---

# Alternative Solution

The following query is equivalent in **MySQL**.

```sql
SELECT
    name
FROM Customer
WHERE referee_id != 2
   OR referee_id IS NULL;
```

### Note

* `<>` is the **standard SQL** operator for **not equal**.
* `!=` is also supported in **MySQL** and produces the same result.
* Many developers prefer `<>` because it follows the SQL standard and is more portable across different database systems.

---

# Solution Breakdown

### Step 1: SELECT

```sql
SELECT name
```

Returns the customer's name.

---

### Step 2: FROM

```sql
FROM Customer
```

Reads data from the `Customer` table.

---

### Step 3: WHERE

```sql
WHERE referee_id <> 2
   OR referee_id IS NULL;
```

* `referee_id <> 2`

  * Selects customers whose referee is **not** customer `2`.

* `referee_id IS NULL`

  * Includes customers who were **not referred by anyone**.

The `OR` operator ensures that customers satisfying either condition are included in the result.

---

# Time Complexity

**Time Complexity:** `O(n)`

The database scans the `Customer` table once and applies the filtering condition.

---

# Key Concepts

* SELECT
* FROM
* WHERE
* OR
* NULL
* Comparison Operators
* Filtering Data
