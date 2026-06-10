# Stored XSS into Anchor href Attribute

## Lab Overview

This PortSwigger Web Security Academy lab demonstrated how Stored Cross-Site Scripting (XSS) vulnerabilities can occur when user input is stored and later rendered inside HTML attributes.

Unlike basic Reflected XSS, the payload was not injected into normal HTML content. Instead, user-controlled data was stored by the application and later placed inside an anchor (`<a>`) tag's `href` attribute.

This required understanding the context in which the input was rendered before crafting an appropriate payload.

---

## Objective

Exploit a Stored XSS vulnerability by injecting a payload into an anchor tag's `href` attribute.

---

## Vulnerability Explanation

The application allowed users to submit comments containing a website field.

The submitted value was:

- Stored by the application
- Rendered later for other users
- Inserted directly into an anchor tag's `href` attribute

Because the application failed to properly validate or sanitize the input, attacker-controlled values could be stored and executed when users interacted with the generated link.

---

## Approach

To understand how the application handled input:

1. An alphanumeric value was submitted.
2. The request was intercepted using Burp Suite.
3. The rendered output was examined to determine where the input appeared.

After confirming that the input was being placed inside the anchor's `href` attribute, the comment submission request was modified to include a JavaScript URI payload.

---

## Payload Used

```html
javascript:alert(1)
```

---

## Why the Payload Worked

The application generated a link similar to:

```html
<a href="javascript:alert(1)">Website</a>
```

When a user clicked the link:

- The browser interpreted `javascript:` as a valid URI scheme.
- Everything following the scheme was executed as JavaScript.
- The injected code executed within the application's context.

No `<script>` tags were required because the browser executed the payload directly from the attribute value.

---

## Impact

A successful Stored XSS vulnerability can allow attackers to:

- Execute arbitrary JavaScript
- Steal user sessions
- Perform actions on behalf of victims
- Modify page content
- Deliver malicious payloads to multiple users

Because the payload is stored by the application, every user who interacts with the affected content may be impacted.

---

## Key Learnings

- XSS is heavily dependent on context.
- The location where input is rendered determines the appropriate payload.
- HTML attributes introduce different attack vectors than normal page content.
- Understanding browser behavior is often more important than memorizing payloads.

---

## Security Takeaways

- Validate and sanitize user input before storage.
- Restrict dangerous URI schemes such as `javascript:`.
- Apply context-aware output encoding.
- Use Content Security Policy (CSP) to reduce XSS impact.
- Perform server-side validation rather than relying solely on client-side controls.

---

## Skills Practiced

- Stored Cross-Site Scripting (XSS)
- Attribute-context injection
- Burp Suite request interception
- Payload crafting
- Browser execution behavior analysis

---

## Tools Used

- Burp Suite Community Edition
- PortSwigger Web Security Academy

---

## Platform

PortSwigger Web Security Academy
