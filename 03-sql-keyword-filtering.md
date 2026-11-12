# 3. SQL Keyword Filtering

## What is SQL keyword filtering?

Some applications attempt to prevent SQL injection by blocking particular SQL keywords, such as:

- `SELECT`
- `UNION`
- `DROP`

This is an input-filtering approach. It is not a reliable SQL injection defence.

## SQL keywords are generally case-insensitive

The following SQL keywords are equivalent:

```sql
SELECT
select
SeLeCt
```

SQL generally recognises these as the same keyword, regardless of capitalization.

## Python string comparisons are case-sensitive by default

Example:

```python
keyword = "SELECT"
user_input = "SeLeCt"

print(keyword in user_input)
```

Output:

```text
False
```

Python compares the characters as written. Uppercase and lowercase letters are different characters in a normal string comparison.

## Example of a weak filter

```python
user_input = input("Enter your input: ")

if "SELECT" in user_input:
    print("Blocked!")
else:
    print("Accepted!")
```

A check that only looks for the exact uppercase spelling may fail to recognise mixed-case variants.

## Investigation observation

During the Light challenge, the application blocked keywords such as `SELECT` and `UNION`. Mixed-case SQL keywords were accepted during testing.

Example observed in the investigation:

```sql
UniOn SeLeCt
```

This demonstrated a weakness in the application's keyword filtering mechanism.

## Improving case handling is not enough

A simple normalization improvement is:

```python
user_input = input("Enter your input: ")

if "SELECT" in user_input.upper():
    print("Blocked!")
else:
    print("Accepted!")
```

Calling `.upper()` converts the input to uppercase before the comparison.

However, even normalized keyword filtering is not a sufficient SQL injection defence. SQL syntax has many forms, and blocking selected words does not fix unsafe query construction.

## Security lesson

Do not rely on keyword filtering as the primary SQL injection defence. Use parameterized queries so user input cannot change the intended SQL query structure.
