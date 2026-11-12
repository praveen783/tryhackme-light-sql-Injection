# 4. SQLite Database Identification

## Objective

Identify the database engine used by the application and determine its version.

## What is SQLite?

SQLite is a lightweight relational database engine used by applications to store and retrieve structured data.

Other database management systems include MySQL, PostgreSQL, Microsoft SQL Server, and Oracle Database.

## Function used

SQLite provides the built-in function:

```sql
sqlite_version()
```

It returns the version of the SQLite database engine.

Example:

```sql
SELECT sqlite_version();
```

## Observed result

```text
3.31.1
```

## Investigation findings

| Property | Result |
|---|---|
| Database engine | SQLite |
| Database version | 3.31.1 |
| Function used | `sqlite_version()` |

## Why identify the database engine?

Database fingerprinting helps an investigator understand which database-specific syntax, functions, and features may be available.

Different database engines have different functions and SQL dialects. Knowing the engine helps guide further investigation.

Examples of version-identification functions include:

| Database | Version function |
|---|---|
| SQLite | `sqlite_version()` |
| MySQL | `VERSION()` |
| PostgreSQL | `version()` |
| SQL Server | `@@VERSION` |

## Key takeaway

Identifying the database engine and version is known as database fingerprinting. It is an early step in understanding an application's database technology.
