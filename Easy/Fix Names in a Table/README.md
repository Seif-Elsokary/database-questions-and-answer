# 1667. Fix Names in a Table

**Problem Link:** https://leetcode.com/problems/fix-names-in-a-table/

---

# Problem Overview

The `Users` table contains information about users.

The `name` column contains only uppercase and lowercase characters.

The goal is to fix the names so that:

* The first character is uppercase.
* All remaining characters are lowercase.

The result should be ordered by `user_id`.

---

# Database Table

## Users Table

| user_id | name  |
| ------- | ----- |
| 1       | aLice |
| 2       | bOB   |

---

# Expected Result

| user_id | name  |
| ------- | ----- |
| 1       | Alice |
| 2       | Bob   |

---

# Explanation

The required format:

* The first character should always be uppercase.
* The rest of the name should be lowercase.

Examples:

```
aLice → Alice
bOB   → Bob
```

---

# New Concepts

This problem introduces several SQL string functions:

* `UPPER()`
* `LOWER()`
* `LEFT()`
* `SUBSTRING()`
* `CONCAT()`

---

# UPPER()

`UPPER()` converts characters into uppercase.

Example:

```sql
SELECT UPPER('alice');
```

Result:

```
ALICE
```

---

# LOWER()

`LOWER()` converts characters into lowercase.

Example:

```sql
SELECT LOWER('BOB');
```

Result:

```
bob
```

---

# LEFT()

`LEFT()` returns characters from the beginning of a string.

Syntax:

```sql
LEFT(string, number_of_characters)
```

Example:

```sql
SELECT LEFT('alice', 1);
```

Result:

```
a
```

In this problem:

```sql
LEFT(name, 1)
```

gets the first character of the name.

---

# SUBSTRING()

`SUBSTRING()` extracts part of a string starting from a specific position.

Syntax:

```sql
SUBSTRING(string, start_position)
```

Example:

```sql
SELECT SUBSTRING('alice', 2);
```

Result:

```
lice
```

In this problem:

```sql
SUBSTRING(name, 2)
```

gets all characters after the first character.

---

# CONCAT()

`CONCAT()` combines multiple strings into one string.

Example:

```sql
SELECT CONCAT('A', 'lice');
```

Result:

```
Alice
```

---

# SQL Solution

```sql
SELECT 
    user_id,
    CONCAT(
        UPPER(
            LEFT(name, 1)
        ),
        LOWER(
            SUBSTRING(name, 2)
        )
    ) AS name
FROM Users
ORDER BY user_id;
```

---

# Solution Breakdown

## Step 1: Get User ID

```sql
SELECT user_id
```

Returns the user's identifier.

---

## Step 2: Extract the First Character

```sql
LEFT(name, 1)
```

Example:

Input:

```
aLice
```

Returns:

```
a
```

Then:

```sql
UPPER('a')
```

Returns:

```
A
```

---

## Step 3: Extract Remaining Characters

```sql
SUBSTRING(name, 2)
```

Example:

Input:

```
aLice
```

Returns:

```
Lice
```

Then:

```sql
LOWER('Lice')
```

Returns:

```
lice
```

---

## Step 4: Combine Both Parts

```sql
CONCAT('A', 'lice')
```

Result:

```
Alice
```

---

## Step 5: Order Result

```sql
ORDER BY user_id;
```

Sorts the output by user ID.

---

# Alternative Solution

Another approach is converting the whole name to lowercase first:

```sql
SELECT 
    user_id,
    CONCAT(
        UPPER(LEFT(LOWER(name), 1)),
        SUBSTRING(LOWER(name), 2)
    ) AS name
FROM Users
ORDER BY user_id;
```

---

# Time Complexity

**Time Complexity:** `O(n)`

The database processes each row once and applies string functions to the name column.

---

# Key Concepts

* SELECT
* ORDER BY
* UPPER()
* LOWER()
* LEFT()
* SUBSTRING()
* CONCAT()
* String Manipulation
