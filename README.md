# TryHackMe – Light | SQL Injection Investigation

## 1. Overview

This repository documents my investigation of the **Light** room on TryHackMe, focusing on SQL injection vulnerabilities and SQLite database enumeration.

The investigation covered SQL injection fundamentals, Boolean-based SQL injection, SQL keyword filtering, SQLite database identification, database schema discovery, and UNION-based SQL injection.

> **Scope:** All testing documented here was performed against the authorised TryHackMe challenge environment.

## 2. Lab Environment

| Component | Details |
|---|---|
| Platform | TryHackMe |
| Challenge | Light |
| Category | Web / SQL Injection |
| Operating system | Kali Linux |
| Database engine identified | SQLite |
| Database version identified | 3.31.1 |
| Connection tool | Netcat (`nc`) |

Connect using the target IP currently assigned by the room:

```bash
nc <TARGET_IP> 1337
```

The target IP may change when the room machine is restarted.

## 3. Investigation Methodology

1. Connect to the challenge application.
2. Examine how the application responds to supplied input.
3. Test SQL injection behaviour.
4. Study Boolean conditions and application responses.
5. Examine the keyword filter and its case sensitivity.
6. Identify the database engine and version.
7. Discover database tables and columns.
8. Use UNION-based SQL injection to retrieve the information requested by the challenge.

## 4. Findings at a Glance

| Investigation | Finding |
|---|---|
| Database engine | SQLite |
| Database version | 3.31.1 |
| Discovered table | `admintable` |
| Discovered columns | `id`, `username`, `password` |
| Boolean condition testing | TRUE/FALSE conditions affected query behaviour |
| Keyword filtering | Mixed-case SQL keywords were accepted during testing |
| UNION-based extraction | Retrieved the administrator username, password information, and the requested flag |

## 5. Topic Write-ups

- [01 – SQL Injection Fundamentals](01-sql-injection-fundamentals.md)
- [02 – Boolean-Based SQL Injection](02-boolean-based-sqli.md)
- [03 – SQL Keyword Filtering](03-sql-keyword-filtering.md)
- [04 – SQLite Database Identification](04-sqlite-database-identification.md)
- [05 – Database Schema Discovery](05-database-schema-discovery.md)
- [06 – UNION-Based SQL Injection](06-union-based-sqli.md)

## 6. Security Lessons

- Constructing SQL queries through unsafe string concatenation can allow user input to change query logic.
- Boolean-based SQL injection uses differences between TRUE and FALSE responses to infer information.
- SQL keyword filtering is not a reliable SQL injection defence.
- Database fingerprinting and schema enumeration can reveal information about an application's backend.
- UNION-based SQL injection can expose data from other tables when query structure and output conditions permit it.
- Parameterized queries (prepared statements) are the primary defence against SQL injection.
- Database accounts should follow the principle of least privilege.

## 7. Conclusion

This investigation demonstrated how unsafe SQL query construction can expose application data. Starting with basic SQL injection concepts, I examined Boolean-based behaviour, weak keyword filtering, SQLite fingerprinting, database schema discovery, and UNION-based data extraction.

The key security lesson is to keep SQL instructions separate from untrusted user input by using parameterized queries.

## References

- [TryHackMe – Light](https://tryhackme.com/room/light)
- [SQLite Documentation](https://www.sqlite.org/docs.html)
- [SQLite SQL Language Reference](https://www.sqlite.org/lang.html)
- [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)

## Screenshots

Place your own screenshots from the investigation in the `screenshots/` directory. Do not include real credentials or sensitive values in public screenshots.

**Challenge status:** The three requested challenge questions were answered during the investigation documented in this repository. Only the findings actually observed are recorded here.
