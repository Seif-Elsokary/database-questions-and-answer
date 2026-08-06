# 2356. Number of Unique Subjects Taught by Each Teacher

**Problem Link:** https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher/

---

# Problem Overview

The `Teacher` table contains information about teachers and the subjects they teach.

Each row represents:

* A teacher.
* A subject taught by that teacher.

The goal is to find the number of **unique subjects** taught by each teacher.

The result should contain:

* `teacher_id`
* Number of unique subjects as `cnt`

---

# Database Table

## Teacher Table

| teacher_id | subject_id | dept_id |
| ---------- | ---------- | ------- |
| 1          | 2          | 3       |
| 1          | 2          | 4       |
| 1          | 3          | 3       |
| 2          | 1          | 1       |
| 2          | 4          | 2       |

---

# Expected Result

| teacher_id | cnt |
| ---------- | --- |
| 1          | 2   |
| 2          | 2   |

---

# Explanation

Teacher `1` teaches:

```text
subject_id:
2
2
3
```

The subject `2` appears twice, but we count it only once.

Unique subjects:

```text
2, 3
```

Count:

```text
2
```

---

Teacher `2` teaches:

```text
1, 4
```

Unique subjects:

```text
1, 4
```

Count:

```text
2
```

---

# New Concepts

This problem introduces:

* `GROUP BY`
* `COUNT()`
* `DISTINCT`

---

# COUNT()

`COUNT()` is an aggregate function used to count rows.

Example:

```sql
SELECT COUNT(subject_id)
FROM Teacher;
```

It counts all values in `subject_id`.

Example data:

| subject_id |
| ---------- |
| 2          |
| 2          |
| 3          |

Result:

```text
3
```

Because there are three rows.

---

# DISTINCT

`DISTINCT` removes duplicate values.

Example:

```sql
SELECT DISTINCT subject_id
FROM Teacher;
```

Input:

| subject_id |
| ---------- |
| 2          |
| 2          |
| 3          |

Output:

| subject_id |
| ---------- |
| 2          |
| 3          |

The duplicated value appears only once.

---

# COUNT(DISTINCT)

Combining both:

```sql
COUNT(DISTINCT subject_id)
```

means:

1. Remove duplicated subjects.
2. Count the remaining values.

Example:

| subject_id |
| ---------- |
| 2          |
| 2          |
| 3          |

After `DISTINCT`:

| subject_id |
| ---------- |
| 2          |
| 3          |

Result:

```text
2
```

---

# GROUP BY

`GROUP BY` groups rows that have the same value.

Example:

```sql
SELECT teacher_id
FROM Teacher
GROUP BY teacher_id;
```

It creates a separate group for each teacher.

Example:

| teacher_id | subject_id |
| ---------- | ---------- |
| 1          | 2          |
| 1          | 3          |
| 2          | 1          |

Groups:

```text
Teacher 1
    subject 2
    subject 3

Teacher 2
    subject 1
```

---

# SQL Solution

```sql
SELECT 
    teacher_id,
    COUNT(DISTINCT subject_id) AS cnt
FROM Teacher
GROUP BY teacher_id;
```

---

# Solution Breakdown

## Step 1: SELECT

```sql
SELECT 
    teacher_id,
    COUNT(DISTINCT subject_id) AS cnt
```

Returns:

* Teacher ID.
* Number of unique subjects.

---

## Step 2: FROM

```sql
FROM Teacher
```

Reads data from the `Teacher` table.

---

## Step 3: GROUP BY

```sql
GROUP BY teacher_id;
```

Creates a separate calculation for every teacher.

---

# Why Do We Use DISTINCT?

Because the same teacher can teach the same subject multiple times.

Without:

```sql
COUNT(subject_id)
```

Duplicate subjects will be counted.

With:

```sql
COUNT(DISTINCT subject_id)
```

Only unique subjects are counted.

---

# Time Complexity

**Time Complexity:** `O(n)`

The database scans the table and groups rows by `teacher_id`.

---

# Key Concepts

* SELECT
* GROUP BY
* COUNT()
* DISTINCT
* Aggregate Functions
* Removing Duplicates
* Data Grouping
