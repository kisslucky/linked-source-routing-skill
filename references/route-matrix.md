# Route Matrix

## Quick Examples

| Example input | Preferred route | Why |
| --- | --- | --- |
| company blog post | direct fetch | public static content |
| Google result page with JS verification | `web-access` CDP or alternate search source | static fetch is already invalid |
| Bing result page | direct fetch or search tool | often HTML-friendly enough for discovery |
| `old.reddit.com` thread | direct fetch | often better static retrieval than the modern shell |
| YouTube watch page | `video-to-summary` or `web-access` depending on need | video/transcript workflow |
| Douyin short-video page | `web-access` CDP | dynamic, interaction-heavy page |
| login-gated SaaS dashboard | `web-access` with logged-in browser | fetch layer will not help |

## Escalation Reminder

Escalate to `web-access` CDP when:

- the content is rendered client-side
- the source needs clicks, scrolling, login, or transcript expansion
- the fetch layer returns challenge HTML
- the source is known to behave badly under static retrieval
