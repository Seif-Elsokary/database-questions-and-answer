
# 175. Combine Two Tables

## Problem Overview

This problem requires retrieving data from **two different tables**, which means we need to use a **JOIN**.

The `Person` table contains personal information, while the `Address` table contains each person's address. Both tables are related using the `personId` column.

The goal is to return the **first name, last name, city, and state** for every person.

If a person does not have a matching address, the `city` and `state` should be returned as **NULL**.

---

# JOIN Types

Assume we have the following tables.

## Person

| personId | firstName |
| -------- | --------- |
| 1        | Allen     |
| 2        | Bob       |
| 3        | Charlie   |

## Address

| personId | city       |
| -------- | ---------- |
| 2        | New York   |
| 3        | California |
| 4        | Texas      |

| JOIN Type           | Description                                                                                                                                   | Example Result                                                                         |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| **INNER JOIN**      | Returns only the matching rows from both tables.                                                                                              | (2, Bob, New York)<br>(3, Charlie, California)                                         |
| **LEFT JOIN**       | Returns all rows from the left table and the matching rows from the right table. If no match exists, the right table columns become **NULL**. | (1, Allen, NULL)<br>(2, Bob, New York)<br>(3, Charlie, California)                     |
| **RIGHT JOIN**      | Returns all rows from the right table and the matching rows from the left table. If no match exists, the left table columns become **NULL**.  | (2, Bob, New York)<br>(3, Charlie, California)<br>(4, NULL, Texas)                     |
| **FULL OUTER JOIN** | Returns all rows from both tables. Rows without a match return **NULL**. *(Not supported directly in MySQL.)*                                 | (1, Allen, NULL)<br>(2, Bob, New York)<br>(3, Charlie, California)<br>(4, NULL, Texas) |

---

# Why LEFT JOIN?

The problem asks us to return **every person** from the **Person** table.

That means:

* If a person has an address, return the corresponding **city** and **state**.
* If a person does not have an address, return **NULL** instead.

Since we must keep **all rows from the Person table**, the correct choice is **LEFT JOIN**.

---

# Database Tables

## Person

| personId | firstName | lastName |
| -------- | --------- | -------- |
| 1        | Allen     | Wang     |
| 2        | Bob       | Alice    |

## Address

| addressId | personId | city          | state      |
| --------- | -------- | ------------- | ---------- |
| 1         | 2        | New York City | New York   |
| 2         | 3        | Leetcode      | California |

---

# Relationship Between Tables

The relationship between the two tables is:

```text
Person.personId = Address.personId
```

* `Person.personId` is the **Primary Key**.
* `Address.personId` is the **Foreign Key**.
* We use these columns to combine the two tables.

---

# Expected Result

| firstName | lastName | city          | state    |
| --------- | -------- | ------------- | -------- |
| Allen     | Wang     | NULL          | NULL     |
| Bob       | Alice    | New York City | New York |

### Explanation

* **Allen Wang** does not have a matching address, so `city` and `state` are returned as **NULL**.
* **Bob Alice** has a matching address, so the corresponding `city` and `state` are returned.
* The record with `personId = 3` exists only in the `Address` table, so it is ignored because the query starts from the `Person` table.

---

# SQL Solution

```sql
SELECT
    p.firstName,
    p.lastName,
    a.city,
    a.state
FROM Person p
LEFT JOIN Address a
ON p.personId = a.personId;
```

---

# Solution Breakdown

### Step 1: SELECT

```sql
SELECT
    p.firstName,
    p.lastName,
    a.city,
    a.state
```

Selects the required columns from both tables.

---

### Step 2: FROM

```sql
FROM Person p
```

Starts from the **Person** table because the problem requires returning **every person**.

---

### Step 3: LEFT JOIN

```sql
LEFT JOIN Address a
```

Combines the `Person` table with the `Address` table while keeping **all records** from the `Person` table.

---

### Step 4: ON

```sql
ON p.personId = a.personId
```

Matches each person with their address using the `personId` column.

If no matching address exists, MySQL automatically returns **NULL** for `city` and `state`.

---

# Result

| firstName | lastName | city          | state    |
| --------- | -------- | ------------- | -------- |
| Allen     | Wang     | NULL          | NULL     |
| Bob       | Alice    | New York City | New York |

---

# Time Complexity

**Time Complexity:** `O(n)`

The database scans the `Person` table and joins it with the `Address` table using the `personId` column. With proper indexing on the join key, the operation is efficient.

---

# Key Concepts

* SELECT
* FROM
* LEFT JOIN
* ON
* Primary Key
* Foreign Key
* NULL
* Table Relationships
* Combining Tables
