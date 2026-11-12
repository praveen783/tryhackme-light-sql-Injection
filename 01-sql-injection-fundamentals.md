# 1. SQL Injection Fundamentals

## What is SQL Injection?

SQL injection (SQLi) is a vulnerability that occurs when an application incorporates untrusted user input into an SQL query without safely separating data from SQL instructions.

An attacker may supply SQL syntax instead of ordinary input, potentially changing the meaning of the original query.

## Example of vulnerable Python code

```python
username = input("Enter username: ")

query = "SELECT password FROM users WHERE username = '" + username + "'"
```

The problem is string concatenation: the application directly combines user input with SQL code.

## Example input

```sql
' OR '1'='1
```

The expression `1=1` is always TRUE. When input is incorporated unsafely into a query, an injected condition may change the query's logic.

## Root cause

**Insecure SQL query construction.**

The application does not reliably distinguish user-supplied data from SQL instructions.

## Prevention: Parameterized queries

A safer approach is to use parameterized queries (prepared statements).

```python
username = input("Enter username: ")

query = "SELECT password FROM users WHERE username = ?"

cursor.execute(query, (username,))
```

### Understanding the code

- `?` is a placeholder for a parameter.
- `username` is supplied separately from the SQL statement.
- `(username,)` is a one-element Python tuple. The comma makes it a tuple.
- The database treats the supplied value as data rather than executable SQL syntax.

## Key takeaway

SQL injection is caused by unsafe query construction. Parameterized queries separate SQL instructions from user-supplied data and are the primary defence.
