# Reflected Cross-Site Scripting (XSS)

## Lab Overview

This PortSwigger Web Security Academy lab demonstrated a classic Reflected Cross-Site Scripting (XSS) vulnerability.

The application accepted user-controlled input through a URL parameter and reflected that input directly into the HTML response without performing any output encoding or sanitization.

Because the input was rendered directly by the browser, malicious JavaScript could be executed within the context of the application.

---

## Objective

Identify and exploit a Reflected XSS vulnerability by injecting JavaScript into a user-controlled input field.

---

## Vulnerability Explanation

Reflected XSS occurs when:

1. User input is sent to the application.
2. The application immediately includes that input in the HTTP response.
3. The browser interprets the reflected content as executable code.

If output encoding is not applied, attackers can inject JavaScript that executes when the page is rendered.

---

## Approach

While testing the application's input field, I observed that user-supplied data was reflected directly back into the HTML response.

To verify whether the input was properly sanitized, a simple JavaScript payload was submitted.

The payload was reflected into the page and executed successfully, confirming the presence of a Reflected XSS vulnerability.

---

## Payload Used

```html
<script>alert(1)</script>
```

---

## Why the Payload Worked

The application inserted user-controlled input directly into the HTML response without encoding special characters.

As a result:

```html
<p>User Search: <script>alert(1)</script></p>
```

The browser interpreted the injected `<script>` tag as legitimate page content and executed the JavaScript code.

The browser cannot distinguish between code written by the developer and code supplied by an attacker if both are delivered as executable HTML.

---

## Impact

A successful Reflected XSS vulnerability can allow attackers to:

- Execute arbitrary JavaScript
- Steal session tokens
- Perform actions on behalf of users
- Modify page content
- Redirect users to malicious websites

The actual impact depends on the application's context and security controls.

---

## Key Learnings

- Reflected XSS can arise from a single unsanitized input field.
- Browsers execute injected scripts when applications fail to encode output properly.
- User input should never be trusted, regardless of where it originates.
- Output encoding is one of the most effective defenses against XSS.

---

## Security Takeaways

- Encode user-controlled data before rendering it in HTML.
- Apply context-aware output encoding.
- Use Content Security Policy (CSP) where appropriate.
- Validate and sanitize user input on the server side.
- Follow secure coding practices throughout the application lifecycle.

---

## Skills Practiced

- Cross-Site Scripting (XSS)
- Input reflection analysis
- Payload testing
- Browser behavior understanding
- Web application security testing

---

## Platform

PortSwigger Web Security Academy
