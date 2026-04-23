---
name: linked-source-routing
description: Classify user-supplied links and choose the first acquisition path. Use when the input already contains one or more URLs and the agent must decide between direct fetch, web-access CDP, video-to-summary, or browser troubleshooting before content processing starts.
license: MIT
metadata:
  openclaw:
    emoji: "🧭"
    category: "routing"
    tags: ["link", "url", "routing", "web-access", "cdp"]
  hermes:
    tags: ["link", "url", "routing", "browser", "web"]
    requires_toolsets: [web]
---

# Linked Source Routing

Turn supplied links into an acquisition plan before the main skill starts.

## Core Rules

1. This skill is only for requests that already contain URLs.
2. Route for acquisition first, processing second.
3. Prefer the cheapest path that returns real content, not just any `200 OK`.
4. If static fetch returns challenge HTML, empty shells, or partial content, escalate immediately.
5. Distinguish source problems from browser/CDP problems.

## Route Matrix

| Link type | Preferred path | Fallback |
| --- | --- | --- |
| public article / docs / normal webpage | direct fetch | `web-access` |
| GitHub repo / docs / changelog | direct fetch | `web-access` |
| JS-heavy page | `web-access` CDP | alternate source |
| anti-bot or verification page | `web-access` CDP | alternate source |
| login-gated page | `web-access` with logged-in browser | ask for access |
| video URL | `video-to-summary` | `web-access` page extraction |
| short-video page | `web-access` first | `video-to-summary` with exported asset |
| browser/CDP failing | `browser-troubleshooting` | user intervention |

## Static Fetch Failure Rule

Treat these as acquisition failures:

- JavaScript verification pages
- challenge or captcha HTML
- empty app shells
- obviously partial HTML where the important fields are missing

Do not keep retrying the same static layer once one of those appears.

## Workflow

### 1. Classify the link

- identify whether the URL is page, video, app shell, login page, or dynamic platform
- check whether the supplied link is already the canonical source

### 2. Pick the first acquisition path

- direct fetch for public static pages
- `web-access` CDP for dynamic, login-gated, or anti-bot pages
- `video-to-summary` for video and short-video sources
- `browser-troubleshooting` only when the browser path itself is broken

### 3. Validate the payload

- did the chosen path return the real content?
- are the key fields present?
- was the response only a challenge page or shell?

### 4. Hand off

Return a short routing note with:

- URL type
- preferred acquisition path
- fallback path
- why the route was chosen
- the downstream skill to call next

## References

- `references/route-matrix.md`
  Use when you need concrete examples and escalation triggers.
