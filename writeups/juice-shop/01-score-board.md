# 01 – Score Board

| | |
|---|---|
| **Target** | OWASP Juice Shop (local lab) |
| **Difficulty** | ⭐ (1/6) |
| **Category** | Miscellaneous / Security through obscurity |
| **OWASP Top 10** | A01:2021 – Broken Access Control |

## Goal

Find the hidden Score Board page, which is not linked anywhere in the application menu.

## Approach

1. Opened the browser DevTools (`F12`) → **Sources** tab.
2. Searched the main JavaScript bundle (`main.js`) for `scor`. This gave 71 results,
   mostly labels and text strings. The code is minified, which makes it hard to read.
3. Narrowed the search to the route definitions pattern used by Angular (`path:`),
   then searched for `score-board`.
4. Found a route entry similar to `{path:"score-board", ...}`.
5. Opened `http://localhost:3000/#/score-board` and the challenge was solved.

## Why this is a security issue

The page was "hidden" only by not linking to it. However, **all frontend code is sent to
the user's browser**, so anyone can read it and discover every route.

Hiding something is not the same as protecting it (*security through obscurity*).
In a real application, the same mistake could expose an admin panel or internal tools.

## Remediation

- Enforce **access control on the server side**: check whether the user is allowed to see a resource.
- Don't rely on hidden URLs or missing links as a security measure.
- Treat everything in client-side code as public.

## What I learned

- How to use DevTools to inspect a web application's JavaScript
- Minified code is hard to read, so searching for **patterns** (`path:`) works better than single words
- Why client-side "hiding" is not a security control