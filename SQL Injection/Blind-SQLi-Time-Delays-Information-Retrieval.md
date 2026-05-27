# Blind SQL Injection with Time Delays and Information Retrieval

## Lab Overview

This PortSwigger Web Security Academy lab demonstrated how time-based Blind SQL Injection can be used to confirm vulnerabilities and extract sensitive information from a backend database.

Unlike traditional SQL Injection scenarios, the application did not return visible database errors or query results. Instead, response delays were used as an indicator that injected SQL conditions were being processed by the backend database.

---

## Objective

Exploit a Blind SQL Injection vulnerability using time delays to identify sensitive database information and retrieve the administrator password.

---

## Vulnerability Explanation

The application incorporated user-controlled input into a backend SQL query without proper sanitization.

Although the application suppressed visible SQL output, conditional queries could still trigger measurable response delays from the database server.

This created a timing-based side channel that allowed information to be extracted indirectly.

---

## Approach

The vulnerability was first confirmed by injecting payloads designed to intentionally delay the database response.

By comparing response times between true and false conditions, it became clear that the backend database was processing the injected SQL statements.

After confirming the injection point:

- The database structure was analyzed
- The existence of a `users` table was identified
- An administrator account was confirmed

Once the structure was understood, Burp Suite Intruder with a Cluster Bomb attack was used to enumerate the administrator password character by character.

---

## Example Payload Concept

```sql
'; IF (1=1) WAITFOR DELAY '0:0:5'--
```

This payload introduces a delay when the condition evaluates as true.

For character enumeration, conditional checks were used to determine whether a guessed character matched a specific position in the password.

---

## Why the Technique Worked

Although the application did not expose query results directly, database response timing created an observable side channel.

When injected conditions evaluated as true, the database intentionally delayed its response.

By repeatedly testing conditions and measuring response times, sensitive information could be inferred one step at a time.

---

## Key Learnings

- Blind SQL Injection can still be highly exploitable even without visible output.
- Time-based techniques are useful when applications suppress database errors and query responses.
- Structured enumeration methods allow attackers to gradually reconstruct sensitive information.
- Combining timing analysis with automation tools significantly improves efficiency.

---

## Security Takeaways

- Use parameterized queries and prepared statements.
- Never directly embed user-controlled input into SQL statements.
- Apply strict input validation and output handling.
- Monitor for abnormal query execution times and repeated request patterns.
- Implement rate limiting and anomaly detection mechanisms.

---

## Skills Practiced

- Time-based Blind SQL Injection
- Response timing analysis
- Database enumeration
- Burp Suite Intruder usage
- Password extraction techniques

---

## Platform

PortSwigger Web Security Academy
