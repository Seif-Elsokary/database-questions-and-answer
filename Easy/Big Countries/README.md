# 595. Big Countries

**Problem Link:** https://leetcode.com/problems/big-countries/

---

# Problem Overview

The `World` table contains information about different countries, including their name, population, and area.

A country is considered **big** if:

* Its **population** is greater than or equal to **25,000,000**, **OR**
* Its **area** is greater than or equal to **3,000,000**.

The goal is to return the country's **name**, **population**, and **area**.

---

# Database Table

## World Table

| name        | continent |    area | population |          gdp |
| ----------- | --------- | ------: | ---------: | -----------: |
| Afghanistan | Asia      |  652230 |   25500100 |  20343000000 |
| Albania     | Europe    |   28748 |    2831741 |  12960000000 |
| Algeria     | Africa    | 2381741 |   37100000 | 188681000000 |
| Andorra     | Europe    |     468 |      78115 |   3712000000 |
| Angola      | Africa    | 1246700 |   20609294 | 100990000000 |

---

# Expected Result

| name        | population |    area |
| ----------- | ---------: | ------: |
| Afghanistan |   25500100 |  652230 |
| Algeria     |   37100000 | 2381741 |

---

# Explanation

* **Afghanistan**

  * Population = **25,500,100** ≥ **25,000,000**
  * Area = **652,230**
  * Returned because it satisfies the population condition.

* **Algeria**

  * Population = **37,100,000** ≥ **25,000,000**
  * Area = **2,381,741**
  * Returned because it satisfies the population condition.

* The other countries do not satisfy any of the required conditions, so they are not included.

---

# New Concept: ORDER BY

`ORDER BY` is used to sort the result after retrieving data from the database.

## Syntax

```sql
SELECT column1, column2
FROM table_name
ORDER BY column_name [ASC | DESC];
```

---

## ASC (Ascending)

Sorts data from smallest to largest.

Examples:

* Numbers → Smallest to Largest
* Text → A to Z
* Dates → Oldest to Newest

Example:

```sql
SELECT name, population
FROM World
ORDER BY population ASC;
```

---

## DESC (Descending)

Sorts data from largest to smallest.

Examples:

* Numbers → Largest to Smallest
* Text → Z to A
* Dates → Newest to Oldest

Example:

```sql
SELECT name, population
FROM World
ORDER BY population DESC;
```

---

## Note

If no direction is specified:

```sql
ORDER BY population;
```

SQL uses:

```sql
ORDER BY population ASC;
```

by default.

---

# SQL Solution

```sql
SELECT
    w.name,
    w.population,
    w.area
FROM World w
WHERE population >= 25000000
   OR area >= 3000000
ORDER BY w.population;
```

---

# Alternative Solutions

## Solution 2

```sql
SELECT
    name,
    population,
    area
FROM World
WHERE population >= 25000000
   OR area >= 3000000
ORDER BY population;
```

### Explanation

This solution removes the table alias because only one table is used.

Both queries return the same result.

---

## Solution 3

```sql
SELECT
    name,
    population,
    area
FROM World
WHERE area >= 3000000
   OR population >= 25000000
ORDER BY population;
```

### Explanation

The order of conditions inside `OR` does not change the result.

The database checks whether at least one condition is true.

---

# Solution Breakdown

## Step 1: SELECT

```sql
SELECT
    w.name,
    w.population,
    w.area
```

Selects the required columns from the table.

---

## Step 2: FROM

```sql
FROM World w
```

Specifies the table that contains the data.

`w` is a table alias used to make the query shorter.

---

## Step 3: WHERE

```sql
WHERE population >= 25000000
   OR area >= 3000000
```

Filters countries that satisfy at least one condition:

* Population is at least 25 million.
* Area is at least 3 million.

The `OR` operator means one condition is enough.

---

## Step 4: ORDER BY

```sql
ORDER BY w.population;
```

Sorts the final result by population.

Since no direction is specified, the default is ascending order.

---

# Time Complexity

**Time Complexity:** `O(n log n)`

* The database scans the table to apply the filter.
* Sorting the result requires additional time.

---

# Key Concepts

* SELECT
* FROM
* WHERE
* OR
* ORDER BY
* ASC
* DESC
* Table Alias
* Filtering Data
* Sorting Data
