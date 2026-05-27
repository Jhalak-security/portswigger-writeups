# Blind SQL Injection with Conditional Responses

## Lab Overview

This PortSwigger Web Security Academy lab demonstrated how Blind SQL Injection vulnerabilities can be exploited using conditional responses to extract sensitive information from the database.

Unlike visible SQL Injection scenarios, the application did not directly display query results or database errors. Instead, the application's response behavior changed depending on whether injected conditions evaluated as true or false.

---

## Objective

Exploit a Blind SQL Injection vulnerability to extract the administrator password character by character using conditional responses.

---

## Vulnerability Explanation

The application processed user-controlled input within a backend SQL query without proper sanitization.

Although query results were not directly visible, the application responded differently when injected conditions evaluated to true versus false. This behavior allowed information to be inferred indirectly.

This is commonly referred to as Blind SQL Injection.

---

## Approach

The injection point was first identified by testing conditional payloads and observing changes in the application's response.

Once confirmed, the next goal was extracting the administrator password one character at a time.

The intended approach was to automate extraction using:

- Burp Suite Intruder
- Cluster Bomb attack strategy
- A Python automation script shared by Rana Khalil

However:

- Burp Suite Community Edition significantly slowed down large-scale attacks
- The Python automation script did not behave as expected within my environment

Because of these limitations, the password was ultimately extracted manually using a Sniper-based enumeration approach for all 20 password characters.

---

## Example Payload Concept

```sql
' AND SUBSTRING(password,1,1)='a'--
```

This type of payload checks whether a specific character at a specific position matches the guessed value.

The application's response behavior indicated whether the condition evaluated as true or false.

---

## Why the Technique Worked

Even though the application did not reveal database output directly, the response differences acted as an indirect information channel.

By repeatedly testing conditions character by character, it became possible to reconstruct the administrator password.

This demonstrated how dangerous Blind SQL Injection vulnerabilities can be, even when applications suppress visible errors.

---

## Challenges Faced

- Slow attack execution in Burp Suite Community Edition
- Python automation script compatibility/debugging issues
- Time-consuming manual enumeration process

These challenges required adapting the testing methodology instead of relying entirely on automation.

---

## Key Learnings

- Blind SQL Injection often requires patience and careful observation.
- Automation tools are helpful, but understanding the manual process is essential.
- Real-world testing frequently involves troubleshooting tools and adapting techniques.
- Response-based inference can still leak sensitive information even without visible SQL output.

---

## Security Takeaways

- Use parameterized queries and prepared statements.
- Avoid directly embedding user input into SQL queries.
- Implement proper input validation and output handling.
- Use monitoring and rate limiting to reduce automated exploitation attempts.

---

## Skills Practiced

- Blind SQL Injection
- Conditional response analysis
- Manual enumeration techniques
- Burp Suite Intruder usage
- Payload testing and debugging

---

## Platform

PortSwigger Web Security Academy
