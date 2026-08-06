# 1693. Daily Leads and Partners

**Problem Link:** https://leetcode.com/problems/daily-leads-and-partners/

---

# Problem Overview

The `DailySales` table contains information about products sold on different dates.

Each row contains:

* The sale date.
* The product name.
* The lead ID.
* The partner ID.

The goal is to find, for each:

* `date_id`
* `make_name`

the number of:

* Unique leads.
* Unique partners.

---

# Database Table

## DailySales Table

| date_id    | make_name | lead_id | partner_id |
| ---------- | --------- | ------- | ---------- |
| 2020-12-08 | toyota    | 0       | 1          |
| 2020-12-08 | toyota    | 1       | 0          |
| 2020-12-08 | toyota    | 1       | 2          |
| 2020-12-08 | honda     | 1       | 2          |
| 2020-12-08 | honda     | 2       | 1          |

---

# Expected Result

| date_id    | make_name | unique_leads | unique_partners |
| ---------- | --------- | ------------ | --------------- |
| 2020-12-08 | toyota    | 2            | 3               |
| 2020-12-08 | honda     | 2            | 2               |

---

# Explanation

For each combination of:

```text
date_id + make_name
```

we count the number of different:

* `lead_id`
* `partner_id`

Example:

For:

```text
2020-12-08 + toyota
```

Leads:

```text
0, 1
```

Number of unique leads:

```text
2
```

Partners:

```text
1, 0, 2
```

Number of unique partners:

```text
3
```

---

# New Concepts

This problem introduces:

* `GROUP BY`
* `COUNT()`
* `DISTINCT`

---

# GROUP BY

`GROUP BY` combines rows that have the same values in specific columns.

Example:

```sql
SELECT make_name
FROM DailySales
GROUP BY make_name;
```

Rows with the same `make_name` are placed into one group.

---

# COUNT()

`COUNT()` counts the number of rows.

Example:

```sql
SELECT 
    make_name,
    COUNT(*)
FROM DailySales
GROUP BY make_name;
```

It returns the number of records for each product.

---

# DISTINCT

`DISTINCT` removes duplicated values before counting.

Example:

Data:

| lead_id |
| ------- |
| 1       |
| 1       |
| 2       |

Using:

```sql
COUNT(lead_id)
```

Result:

```text
3
```

Because all rows are counted.

Using:

```sql
COUNT(DISTINCT lead_id)
```

Result:

```text
2
```

Because duplicate values are counted once.

---

# Why Do We Use COUNT(DISTINCT)?

The problem asks for:

> number of distinct lead_id's and distinct partner_id's

So duplicates should not be counted.

Example:

```text
lead_id:

1
1
2
```

The answer should be:

```text
2
```

not:

```text
3
```

---

# SQL Solution

```sql
SELECT
    d.date_id AS date_id,
    d.make_name AS make_name,

    COUNT(DISTINCT d.lead_id) AS unique_leads,
    COUNT(DISTINCT d.partner_id) AS unique_partners

FROM DailySales d

GROUP BY
    d.date_id,
    d.make_name;
```

---

# Solution Breakdown

## Step 1: SELECT

```sql
SELECT
    d.date_id,
    d.make_name
```

Returns the date and product name.

---

## Step 2: Count Unique Leads

```sql
COUNT(DISTINCT d.lead_id) AS unique_leads
```

Counts different lead IDs for each group.

---

## Step 3: Count Unique Partners

```sql
COUNT(DISTINCT d.partner_id) AS unique_partners
```

Counts different partner IDs for each group.

---

## Step 4: FROM

```sql
FROM DailySales d
```

Reads data from the `DailySales` table.

---

## Step 5: GROUP BY

```sql
GROUP BY
    d.date_id,
    d.make_name
```

Creates groups based on:

* Date
* Product name

Each group gets its own counts.

---

# Time Complexity

**Time Complexity:** `O(n)`

The database scans all rows and groups them by date and product name.

---

# Key Concepts

* SELECT
* FROM
* GROUP BY
* COUNT()
* DISTINCT
* Aggregate Functions
* Grouping Data
