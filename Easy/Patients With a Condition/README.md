# 1527. Patients With a Condition

**Problem Link:** https://leetcode.com/problems/patients-with-a-condition/

---

# Problem Overview

The `Patients` table contains information about hospital patients.

The `conditions` column stores one or more medical condition codes separated by spaces.

The goal is to find patients who have **Type I Diabetes**.

Type I Diabetes codes always start with:

```text
DIAB1
```

We need to return:

* `patient_id`
* `patient_name`
* `conditions`

for patients whose conditions contain a code starting with `DIAB1`.

---

# Database Table

## Patients Table

| patient_id | patient_name | conditions   |
| ---------- | ------------ | ------------ |
| 1          | Daniel       | YFEV COUGH   |
| 2          | Alice        |              |
| 3          | Bob          | DIAB100 MYOP |
| 4          | George       | ACNE DIAB100 |
| 5          | Alain        | DIAB201      |

---

# Expected Result

| patient_id | patient_name | conditions   |
| ---------- | ------------ | ------------ |
| 3          | Bob          | DIAB100 MYOP |
| 4          | George       | ACNE DIAB100 |

---

# Explanation

* **Bob**

  His conditions are:

```text
DIAB100 MYOP
```

The condition `DIAB100` starts with `DIAB1`, so he is included.

---

* **George**

  His conditions are:

```text
ACNE DIAB100
```

The condition `DIAB100` appears after a space and starts with `DIAB1`, so he is included.

---

* **Alain**

  His condition is:

```text
DIAB201
```

It starts with `DIAB2`, not `DIAB1`, so he is not included.

---

# New Concept: LIKE Operator

`LIKE` is used to search for a pattern inside string values.

## Syntax

```sql
column LIKE pattern
```

Example:

```sql
SELECT *
FROM Patients
WHERE conditions LIKE 'DIAB1%';
```

This finds values that start with `DIAB1`.

---

# Wildcard %

The `%` symbol represents zero or more characters.

Example:

```sql
WHERE patient_name LIKE 'A%'
```

Matches names such as:

```
Alice
Ahmed
Andrew
```

because they start with the letter `A`.

---

# Important Point in This Problem

The condition `DIAB1` can appear:

At the beginning:

```text
DIAB100 MYOP
```

or after another condition:

```text
ACNE DIAB100
```

Therefore, checking only:

```sql
conditions LIKE 'DIAB1%'
```

is not enough because it only finds conditions at the beginning.

We also need to check if `DIAB1` appears after a space.

---

# SQL Solution

```sql
SELECT
    patient_id,
    patient_name,
    conditions
FROM Patients
WHERE conditions LIKE 'DIAB1%'
   OR conditions LIKE '% DIAB1%';
```

---

# Solution Breakdown

## Step 1: SELECT

```sql
SELECT
    patient_id,
    patient_name,
    conditions
```

Returns the required columns.

---

## Step 2: FROM

```sql
FROM Patients
```

Specifies the table that contains patient information.

---

## Step 3: WHERE

First condition:

```sql
conditions LIKE 'DIAB1%'
```

Checks if the conditions start with `DIAB1`.

Example:

```text
DIAB100 MYOP
```

Matches this condition.

---

Second condition:

```sql
conditions LIKE '% DIAB1%'
```

Checks if `DIAB1` appears after a space.

Example:

```text
ACNE DIAB100
```

Matches this condition.

---

# Alternative Solution - Using CONCAT()

Another way is to add spaces around the whole string and search for `DIAB1`.

```sql
SELECT
    patient_id,
    patient_name,
    conditions
FROM Patients
WHERE CONCAT(' ', conditions, ' ') LIKE '% DIAB1%';
```

---

# CONCAT() Explanation

`CONCAT()` combines multiple strings together.

Example:

Original value:

```text
ACNE DIAB100
```

After:

```sql
CONCAT(' ', conditions, ' ')
```

It becomes:

```text
 ACNE DIAB100 
```

Now `DIAB1` always has a space before it, making the search easier.

---

# Time Complexity

**Time Complexity:** `O(n)`

The database scans the conditions column for each patient.

---

# Key Concepts

* SELECT
* FROM
* WHERE
* LIKE
* Wildcard `%`
* OR
* CONCAT()
* String Searching
* Pattern Matching
