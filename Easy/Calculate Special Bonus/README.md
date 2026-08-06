# 1873. Calculate Special Bonus

**Problem Link:** https://leetcode.com/problems/calculate-special-bonus/

---

# Problem Overview

The `Employees` table contains information about employees.

The goal is to calculate the bonus for each employee.

An employee receives a bonus equal to their salary if:

1. The `employee_id` is odd.
2. The employee's name does not start with the letter `M`.

Otherwise, the bonus should be `0`.

The result should contain:

* `employee_id`
* `bonus`

The result must be ordered by `employee_id`.

---

# Database Table

## Employees Table

| employee_id | name    | salary |
| ----------- | ------- | ------ |
| 2           | Meir    | 3000   |
| 3           | Michael | 3800   |
| 9           | Addilyn | 7400   |
| 8           | Juan    | 6100   |
| 7           | Kannon  | 7700   |

---

# Expected Result

| employee_id | bonus |
| ----------- | ----- |
| 2           | 0     |
| 3           | 0     |
| 7           | 7700  |
| 8           | 0     |
| 9           | 7400  |

---

# Explanation

Employee `9`:

* `employee_id = 9` is odd.
* Name is `Addilyn`, which does not start with `M`.
* Bonus = salary (`7400`).

Employee `7`:

* `employee_id = 7` is odd.
* Name is `Kannon`, which does not start with `M`.
* Bonus = salary (`7700`).

The remaining employees do not satisfy both conditions, so their bonus is `0`.

---

# New Concepts

This problem introduces:

* `IF()`
* `CASE WHEN`
* Modulo operator `%`
* `LIKE`
* `NOT LIKE`
* `AND`
* `ORDER BY`

---

# IF() Function

`IF()` is used to apply conditional logic in MySQL.

## Syntax

```sql
IF(condition, value_if_true, value_if_false)
```

Example:

```sql
IF(salary > 5000, salary, 0)
```

Meaning:

* If salary is greater than 5000 → return salary.
* Otherwise → return 0.

---

# Modulo Operator %

The `%` operator returns the remainder after division.

Example:

```sql
7 % 2
```

Result:

```text
1
```

Because:

```text
7 = 2 * 3 + 1
```

To check if a number is odd:

```sql
employee_id % 2 = 1
```

or:

```sql
employee_id % 2 != 0
```

---

# LIKE Operator

`LIKE` searches for a specific pattern in text.

Example:

```sql
name LIKE 'M%'
```

Means:

The name starts with `M`.

Examples:

```text
Mike
Mary
Michael
```

---

# NOT LIKE

`NOT LIKE` returns values that do not match the pattern.

Example:

```sql
name NOT LIKE 'M%'
```

Means:

The name does not start with `M`.

Examples:

```text
John
Alex
Kannon
Addilyn
```

---

# AND Operator

`AND` requires all conditions to be true.

Example:

```sql
condition1 AND condition2
```

Both conditions must be satisfied.

In this problem:

```sql
employee_id % 2 != 0
AND
name NOT LIKE 'M%'
```

The employee must:

* Have an odd ID.
* Have a name that does not start with `M`.

---

# ORDER BY

`ORDER BY` is used to sort the result.

Syntax:

```sql
ORDER BY column_name;
```

Example:

```sql
ORDER BY employee_id;
```

This sorts employees by their ID in ascending order.

---

# SQL Solution 1 - Using IF()

```sql
SELECT 
    employee_id,
    IF(
        employee_id % 2 != 0 
        AND name NOT LIKE 'M%',
        salary,
        0
    ) AS bonus
FROM Employees
ORDER BY employee_id;
```

---

# Solution Breakdown

## Step 1: Select Employee ID

```sql
SELECT employee_id
```

Returns the employee identifier.

---

## Step 2: Calculate Bonus

```sql
IF(
    employee_id % 2 != 0 
    AND name NOT LIKE 'M%',
    salary,
    0
)
```

Checks the conditions:

If:

```text
employee_id is odd
AND
name does not start with M
```

Then:

```text
bonus = salary
```

Otherwise:

```text
bonus = 0
```

---

## Step 3: Sort Result

```sql
ORDER BY employee_id;
```

Orders the final output by employee ID.

---

# Alternative Solution - Using CASE WHEN

`CASE WHEN` is the standard SQL way to write conditional logic.

```sql
SELECT
    employee_id,
    CASE
        WHEN employee_id % 2 != 0
             AND name NOT LIKE 'M%'
        THEN salary
        ELSE 0
    END AS bonus
FROM Employees
ORDER BY employee_id;
```

---

# CASE WHEN Explanation

Syntax:

```sql
CASE
    WHEN condition THEN value
    ELSE value
END
```

Example:

```sql
CASE
    WHEN salary > 5000 THEN salary
    ELSE 0
END
```

---

# IF() vs CASE WHEN

## IF()

```sql
IF(condition, true_value, false_value)
```

* Short syntax.
* Specific to MySQL.
* Good for simple conditions.

---

## CASE WHEN

```sql
CASE
    WHEN condition THEN value
    ELSE value
END
```

* Standard SQL.
* Works with many database systems.
* Better for multiple conditions.

---

# Time Complexity

**Time Complexity:** `O(n)`

The database scans every employee row once to calculate the bonus and sort the result.

---

# Key Concepts

* SELECT
* IF()
* CASE WHEN
* %
* LIKE
* NOT LIKE
* AND
* ORDER BY
* Conditional Logic
* Calculated Columns
