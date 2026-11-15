# 02 – DOM XSS

| | |
|---|---|
| **Target** | OWASP Juice Shop (local lab) |
| **Difficulty** | ⭐ (1/6) |
| **Category** | XSS (Cross-Site Scripting) |
| **OWASP Top 10** | A03:2021 – Injection |

## Goal

Execute JavaScript in the browser through the product search feature.

## Approach

1. **Reconnaissance:** searched for `apple`. The search term was reflected on the page
   ("Search Results - apple"). User input is displayed back, which is a potential injection point.
2. **HTML injection test:** searched for `<b>test</b>`. The word was rendered **bold**,
   so the input is inserted into the page as HTML, not as plain text.
3. **Attempt 1:** `<script>alert(1)</script>`. Nothing happened. Browsers don't execute
   `<script>` tags inserted dynamically into the page this way.
4. **Attempt 2:** `<iframe src="https://...">`. An iframe appeared on the page (with an error
   icon because the URL didn't load). This confirmed that HTML elements can be injected.
5. **Final payload:**

   ```html
   <iframe src="javascript:alert(`xss`)">
   ```

   The browser executed the `javascript:` URL and an alert box with `xss` appeared.
   Challenge solved.

## Why this is a security issue

The payload ends up in the URL, so an attacker can send a crafted link to a victim.
When the victim opens it, the attacker's JavaScript runs **in the victim's browser, in their
logged-in session**. This can be used to steal session data, perform actions as the victim
or show a fake login form.

Because this is **DOM-based** XSS, the injection happens entirely in the browser (client-side
JavaScript), so server logs may not show any trace of the attack. This makes it harder to detect.

## Remediation

- Treat user input as **text, not HTML**: use output encoding / safe rendering APIs.
- Don't bypass the framework's built-in sanitization (Angular sanitizes by default).
- Add a **Content Security Policy (CSP)** to block inline scripts and `javascript:` URLs.

## What I learned

- How to identify a reflection point and test for HTML injection safely
- Why `<script>` didn't work and how an alternative vector (`<iframe>` + `javascript:`) bypasses that
- Difference between DOM-based XSS and server-side injection, and why it matters for detection (SOC)
