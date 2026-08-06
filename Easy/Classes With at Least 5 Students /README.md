# 596. Classes With at Least 5 Students

**Problem Link:** https://leetcode.com/problems/classes-more-than-5-students/

---

# Problem Overview

The `Courses` table contains information about students and the classes they are enrolled in.

The goal is to find all classes that have **at least 5 students**.

To solve this problem, we need to:

1. Group students by their class.
2. Count the number of students in each class.
3. Return only classes where the count is greater than or equal to 5.

---

# Database Table

## Courses Table

| student | class    |
| ------- | -------- |
| A       | Math     |
| B       | English  |
| C       | Math     |
| D       | Biology  |
| E       | Math     |
| F       | Computer |
| G       | Math     |
| H       | Math     |
| I       | Math     |

---

# Expected Result

| class |
| ----- |
| Math  |

---

# Explanation

After counting students in each class:

| class    | count |
| -------- | ----- |
| Math     | 6     |
| English  | 1     |
| Biology  | 1     |
| Computer | 1     |

Only `Math` has at least 5 students, so it is returned.

---

# New Concept: GROUP BY

`GROUP BY` is used to combine rows that have the same value in a specific column.

It is commonly used with aggregate functions such as:

* `COUNT()`
* `SUM()`
* `AVG()`
* `MAX()`
* `MIN()`

---

## Example

Table:

| student | class   |
| ------- | ------- |
| A       | Math    |
| B       | Math    |
| C       | English |

Query:

```sql
SELECT class
FROM Courses
GROUP BY class;
```

Result:

| class   |
| ------- |
| Math    |
| English |

The rows are grouped based on the `class` column.

---

# New Concept: COUNT()

`COUNT()` returns the number of rows.

Example:

```sql
SELECT class, COUNT(*) 
FROM Courses
GROUP BY class;
```

Result:

| class   | COUNT(*) |
| ------- | -------- |
| Math    | 2        |
| English | 1        |

---

# New Concept: HAVING

`HAVING` is used to filter groups after using `GROUP BY`.

The difference:

| WHERE                        | HAVING                        |
| ---------------------------- | ----------------------------- |
| Filters rows before grouping | Filters groups after grouping |

Example:

```sql
SELECT class, COUNT(*)
FROM Courses
GROUP BY class
HAVING COUNT(*) >= 5;
```

This returns only classes that have 5 or more students.

---

# SQL Solution

```sql
SELECT
    class
FROM Courses
GROUP BY class
HAVING COUNT(student) >= 5;
```

---

# Solution Breakdown

## Step 1: SELECT

```sql
SELECT class
```

Returns the class name.

---

## Step 2: FROM

```sql
FROM Courses
```

Reads data from the `Courses` table.

---

## Step 3: GROUP BY

```sql
GROUP BY class
```

Groups all students who belong to the same class.

Example:

Before grouping:

| student | class |
| ------- | ----- |
| A       | Math  |
| C       | Math  |
| E       | Math  |

After grouping:

| class |
| ----- |
| Math  |

---

## Step 4: HAVING

```sql
HAVING COUNT(student) >= 5;
```

Counts students in each class and keeps only classes with 5 or more students.

---

# Time Complexity

**Time Complexity:** `O(n)`

The database scans all rows once and groups them by class.

---

# Key Concepts

* SELECT
* FROM
* GROUP BY
* HAVING
* COUNT()
* Aggregate Functions
* Filtering Groups
