# Authentication Bypass via SQL Injection

## Lab Overview
This PortSwigger Web Security Academy lab demonstrated how improper handling of user input in SQL queries can lead to authentication bypass vulnerabilities.

The application directly embedded user-controlled input into the SQL query without proper sanitization or parameterized handling.

---

## Objective
Gain unauthorized access to the application by manipulating the login query using SQL Injection.

---

## Vulnerability Explanation

The login functionality was vulnerable because the backend SQL query dynamically included user input.

Example of a vulnerable query:

```sql
SELECT * FROM users WHERE username = 'input' AND password = 'input';
```

Since user input was not sanitized, crafted SQL payloads could modify the query logic.

---

## Approach

I tested SQL Injection payloads in the login field to observe how the application handled special characters and query manipulation.

By injecting crafted input, the authentication logic was altered, allowing login without valid credentials.

---

## Payload Used

```sql
' OR 1=1 --
```

---

## Why the Payload Worked

The payload modified the SQL query condition to always evaluate as true.

Example:

```sql
SELECT * FROM users WHERE username = '' OR 1=1 --' AND password = '';
```

- `OR 1=1` always returns true
- `--` comments out the remaining part of the query

As a result, the application bypassed authentication checks.

---

## Key Takeaways

- Applications should never directly concatenate user input into SQL queries.
- Parameterized queries and prepared statements are critical defenses against SQL Injection.
- Input validation alone is not sufficient protection.
- Even simple login forms can become high-risk attack surfaces if query handling is insecure.

---

## Skills Practiced

- SQL Injection basics
- Authentication bypass techniques
- Understanding query manipulation
- Web application testing methodology

---

## Platform

PortSwigger Web Security Academy
