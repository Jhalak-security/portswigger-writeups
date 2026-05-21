# SQL Injection to Determine Oracle Database Version

## Lab Overview

This PortSwigger Web Security Academy lab demonstrated how SQL Injection can be used to identify the type and version of the backend database system.

The application was vulnerable to SQL Injection within a query, allowing additional SQL statements to be executed through crafted input.

---

## Objective

Identify the backend database type and retrieve its version information using SQL Injection techniques.

---

## Vulnerability Explanation

The application failed to properly sanitize user-controlled input before including it in an SQL query.

This allowed crafted payloads to manipulate the original query and perform UNION-based attacks to extract information from the database.

---

## Approach

The first step was identifying how many columns the query returned and determining which columns accepted visible output.

After confirming the injectable point, a UNION-based payload was used to query Oracle database version information.

---

## Payload Used

```sql
' UNION SELECT banner, NULL FROM v$version--
```

---

## Why the Payload Worked

Oracle databases store version details in the `v$version` table.

The injected UNION query combined the original query results with attacker-controlled output, allowing database version information to be displayed within the application's response.

Example concept:

```sql
SELECT column1, column2 FROM products
UNION SELECT banner, NULL FROM v$version--
```

This exposed information about the Oracle database version running on the backend.

---

## Key Learnings

- Information gathering is a critical phase in SQL Injection exploitation.
- Understanding the backend database type helps craft more accurate payloads.
- UNION-based SQL Injection can expose sensitive internal information.
- Misconfigured applications may unintentionally reveal database details.

---

## Security Takeaways

- Use parameterized queries and prepared statements.
- Avoid exposing database error messages or system information.
- Validate and sanitize all user-controlled input.
- Apply least-privilege access controls to database accounts.

---

## Skills Practiced

- UNION-based SQL Injection
- Database enumeration
- Oracle database fingerprinting
- Query structure analysis

---

## Platform

PortSwigger Web Security Academy
