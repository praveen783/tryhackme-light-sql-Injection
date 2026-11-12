# 2. Boolean-Based SQL Injection

## What is Boolean-Based SQL Injection?

Boolean-based SQL injection is a technique that uses conditions evaluating to TRUE or FALSE. An attacker observes differences in the application's responses to infer information about the database.

The technique relies on the application behaving differently depending on whether a query condition is true or false.

## Understanding TRUE and FALSE

```sql
1=1
```

This condition is TRUE.

```sql
1=2
```

This condition is FALSE.

## SQL logical operators

| Operator | Meaning |
|---|---|
| `AND` | Both conditions must be TRUE |
| `OR` | At least one condition must be TRUE |
| `NOT` | Reverses a condition |

Examples:

```sql
TRUE AND TRUE
```

Result: TRUE.

```sql
TRUE AND FALSE
```

Result: FALSE.

```sql
TRUE OR FALSE
```

Result: TRUE.

```sql
NOT TRUE
```

Result: FALSE.

## Investigation observations

### Test 1

```sql
' OR '1'='1
```

**Observation:** The application returned a password value.

**Interpretation:** The injected TRUE condition changed the behaviour of the original SQL query.

### Test 2

```sql
'1'='1
```

**Observation:** The application returned an SQL syntax error.

**Interpretation:** Quote placement and the structure of the original query matter. An input that does not fit the query's syntax can produce an error.

### Test 3

```sql
smokey' AND 1=1 AND '1
```

**Observation:** The input worked. The application appended a closing quote, producing a valid SQL expression in the observed query context.

In SQLite, a nonzero numeric value such as `'1'` evaluates as true in a Boolean context.

Conceptually, the conditions evaluate as:

```text
TRUE AND TRUE AND TRUE = TRUE
```

Changing the middle condition to `1=2` would make the combined AND expression false, assuming the other conditions remain true.

## Why this matters

If an application returns distinguishable responses for TRUE and FALSE conditions, an attacker may be able to infer information without the database directly displaying the requested value.

## Key takeaway

Boolean-based SQL injection uses TRUE/FALSE conditions and observable application behaviour to infer database information.
