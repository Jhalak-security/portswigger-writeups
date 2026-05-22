# SQL Injection UNION Attack to Retrieve Data from Other Tables

## Lab Overview

This PortSwigger Web Security Academy lab demonstrated how SQL Injection UNION attacks can be used to retrieve data from other database tables.

The application was vulnerable because user-controlled input was directly included in a backend SQL query without proper sanitization.

---

## Objective

Exploit a SQL Injection vulnerability to retrieve sensitive data from another table within the database.

---

## Vulnerability Explanation

The product filter parameter appeared to interact directly with a backend SQL query.

While testing the parameter with special characters and crafted input, the application's response changed, indicating that the query structure could potentially be manipulated.

After confirming the injection point, a UNION-based SQL Injection technique was used to combine the original query with attacker-controlled queries.

---

## Approach

The first step involved determining:

- Whether the parameter was injectable
- The number of columns returned by the original query
- Which columns accepted visible output

Once the query structure was understood, a UNION SELECT statement was crafted to retrieve data from another table in the database.

---

## Example Payload

```sql
' UNION SELECT username, password FROM users--
```

---

## Why the Payload Worked

The `UNION` operator combines the results of two SQL queries into a single response.

By matching the correct number of columns and compatible data types, it became possible to append a malicious query to the original SQL statement.

Conceptually:

```sql
SELECT name, description FROM products
UNION
SELECT username, password FROM users--
```

This caused the application to display data retrieved from the `users` table.

---

## Key Learnings

- Understanding query structure is critical for successful UNION attacks.
- Column count and data type matching are important during exploitation.
- SQL Injection vulnerabilities can expose highly sensitive information from unrelated database tables.
- Small differences in application responses can reveal injection points.

---

## Security Takeaways

- Use parameterized queries and prepared statements.
- Never directly concatenate user input into SQL queries.
- Apply strict server-side validation.
- Restrict database permissions using the principle of least privilege.

---

## Skills Practiced

- UNION-based SQL Injection
- Query structure analysis
- Database enumeration
- Sensitive data extraction techniques

---

## Platform

PortSwigger Web Security Academy
