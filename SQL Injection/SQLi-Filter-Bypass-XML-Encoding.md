# SQL Injection with Filter Bypass via XML Encoding

## Lab Overview

This PortSwigger Web Security Academy lab demonstrated how SQL Injection payloads can sometimes be blocked by application filters and input validation mechanisms, requiring alternative encoding techniques to bypass protections.

In this scenario, normal SQL Injection payloads were identified and blocked by the application before they could successfully reach the backend database query.

---

## Objective

Bypass the application's filtering mechanism and successfully perform SQL Injection using encoded payloads.

---

## Vulnerability Explanation

The application attempted to detect and block malicious SQL Injection patterns within incoming requests.

As a result:

- Standard SQL Injection payloads were filtered
- Requests containing suspicious characters or patterns were rejected
- Direct exploitation attempts failed

Although filtering mechanisms were present, the backend application still processed user-controlled input insecurely after decoding the request.

This created an opportunity to bypass the filter using encoded payloads.

---

## Approach

To bypass the filtering mechanism, the payload was encoded using XML encoding techniques.

Following the lab guidance, the **Hackvertor** Burp Suite extension was used to automatically encode the SQL Injection payload into an XML-compatible format.

The encoded payload successfully bypassed the application's filtering layer and was processed by the backend SQL query, allowing the vulnerability to be exploited.

---

## Example Payload Concept

Original payload:

```sql
' UNION SELECT username, password FROM users--
```

Encoded representation (conceptual example):

```xml
&#x27; UNION SELECT username, password FROM users--
```

The encoding altered how the payload appeared to the filtering mechanism while still being interpreted correctly by the backend system after decoding.

---

## Why the Technique Worked

The application's filtering logic inspected the raw request input before processing.

Because the payload was XML-encoded:

- The filter failed to recognize the malicious SQL pattern
- The backend application decoded the payload during processing
- The SQL Injection payload ultimately reached the database query

This demonstrated how encoding techniques can sometimes bypass weak filtering implementations.

---

## Key Learnings

- Input filtering alone is not a reliable defense against SQL Injection.
- Attackers may use encoding techniques to evade detection mechanisms.
- Understanding how applications process and decode input is important during security testing.
- Security controls must validate input after decoding and normalization.

---

## Security Takeaways

- Use parameterized queries and prepared statements instead of relying on filters.
- Perform validation after decoding and normalization steps.
- Avoid blacklist-based filtering approaches alone.
- Implement layered defenses and secure query handling practices.

---

## Skills Practiced

- SQL Injection filter bypass techniques
- XML encoding concepts
- Burp Suite extension usage
- Payload encoding and request manipulation
- Web application testing methodology

---

## Tools Used

- Burp Suite Community Edition
- Hackvertor Extension

---

## Platform

PortSwigger Web Security Academy
