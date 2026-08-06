# 627. Swap Salary

**Problem Link:** https://leetcode.com/problems/swap-salary/

---

# Problem Overview

The `Salary` table contains information about employees.

The goal is to swap all values in the `sex` column:

* Change `m` → `f`
* Change `f` → `m`

The problem requires:

* Using **one UPDATE statement only**.
* No temporary tables.
* No SELECT statement.

---

# Database Table

## Salary Table

| id | name | sex | salary |
| -- | ---- | --- | ------ |
| 1  | A    | m   | 2500   |
| 2  | B    | f   | 1500   |
| 3  | C    | m   | 5500   |
| 4  | D    | f   | 500    |

---

# Expected Result

| id | name | sex | salary |
| -- | ---- | --- | ------ |
| 1  | A    | f   | 2500   |
| 2  | B    | m   | 1500   |
| 3  | C    | f   | 5500   |
| 4  | D    | m   | 500    |

---

# New Concept: UPDATE

`UPDATE` is used to modify existing data inside a table.

## Syntax

```sql
UPDATE table_name
SET column_name = value
WHERE condition;
```

Example:

```sql
UPDATE Salary
SET salary = 3000
WHERE id = 1;
```

This changes the salary of the employee whose `id` is `1`.

---

# New Concept: CASE WHEN

`CASE` is used to apply different values depending on conditions.

It works like `if / else` in programming languages.

## Syntax

```sql
CASE
    WHEN condition THEN value
    ELSE value
END
```

Example:

```sql
CASE
    WHEN sex = 'm' THEN 'f'
    ELSE 'm'
END
```

Meaning:

* If `sex` is `m`, change it to `f`.
* Otherwise, change it to `m`.

---

# SQL Solution 1 - Using CASE WHEN

```sql
UPDATE Salary
SET sex =
    CASE
        WHEN sex = 'm' THEN 'f'
        ELSE 'm'
    END;
```

---

# Solution Breakdown

## Step 1: UPDATE

```sql
UPDATE Salary
```

Specifies the table that will be modified.

---

## Step 2: SET

```sql
SET sex =
```

Defines the column that will be updated.

---

## Step 3: CASE

```sql
CASE
    WHEN sex = 'm' THEN 'f'
    ELSE 'm'
END
```

Checks the current value:

* If the value is `m`, replace it with `f`.
* If the value is `f`, replace it with `m`.

---

# Alternative Solution - Using IF()

MySQL provides the `IF()` function, which works like a simple `if/else` condition.

```sql
UPDATE Salary
SET sex = IF(sex = 'm', 'f', 'm');
```

---

# IF() Explanation

Syntax:

```sql
IF(condition, value_if_true, value_if_false)
```

Example:

```sql
IF(sex = 'm', 'f', 'm')
```

Means:

* If `sex` equals `m` → update it to `f`.
* Otherwise → update it to `m`.

---

# CASE WHEN vs IF()

## CASE WHEN

```sql
CASE
    WHEN sex = 'm' THEN 'f'
    ELSE 'm'
END
```

* Standard SQL.
* Works with most database systems.
* Better when there are multiple conditions.

---

## IF()

```sql
IF(sex = 'm', 'f', 'm')
```

* MySQL-specific function.
* Shorter for simple conditions.
* Not supported in all SQL databases.

---

# Why No WHERE?

Normally, `UPDATE` uses `WHERE` to update specific rows.

Example:

```sql
UPDATE Salary
SET sex = 'f'
WHERE id = 1;
```

But this problem requires changing **all employees**, so we update the whole table without using `WHERE`.

---

# Time Complexity

**Time Complexity:** `O(n)`

The database scans all rows and updates the `sex` value for each employee.

---

# Key Concepts

* UPDATE
* SET
* CASE WHEN
* IF()
* ENUM
* Conditional Logic
* Modifying Data
