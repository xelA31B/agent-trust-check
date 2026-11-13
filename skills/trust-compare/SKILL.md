---
name: trust-compare
description: >
  This skill should be used when the user asks "which MCP server is safest for X", "compare these MCP
  servers", "find a trustworthy alternative to this plugin", "best secure MCP server for GitHub / Slack /
  database", or wants several MCP servers, plugins or skills for the same job compared on trust and
  security. Also triggers on the same requests in Russian ("какой MCP-сервер безопаснее", "сравни
  плагины") or Hungarian.
metadata:
  version: "0.1.0"
---

# Trust Compare

Compare 2–5 components that do the same job and recommend the safest fit.

## Rules

- Language of the user's request. Concise, professional, no filler.
- Same evidence standard and sources as `trust-check` (`../trust-check/references/data-sources.md`, `../trust-check/references/scoring.md`).
- Read-only; never install or run candidates.

## Steps

1. **Candidates.** Use the user's list, or find up to 5 candidates for the job in the official MCP Registry and other public catalogues (state where each came from).
2. **Check each** with the `trust-check` method (signals + capability review), in a lighter form if more than 3 candidates: Scorecard, deps.dev advisories, maintenance, install scripts, tool-definition review.
3. **Fit.** Note which candidate needs the narrowest permissions for the user's task.
4. **Report:**

```
# Trust Compare — [job] · [date]

**Recommendation:** [component] — one-line reason

| Component | Trust Score | Verdict | Scorecard | Advisories | Maintained | Permissions needed | Notes |

## Why not the others
- one line each
```

Offer to run a full `trust-check` on the recommended component.

## Branded report

If the environment can render HTML artifacts or files, present the full report as a static HTML page (no scripts) in the house style below; otherwise use the markdown format above. Keep the chat reply to a short summary either way.

House style:
- Header band with a diagonal gradient from deep navy #1C1E4E to teal #0E7480, white text: small uppercase plugin name with a gold ✦ (#FFC448), the report title, then subject and date.
- Light background #F6F7FB; content in white cards with a thin #E3E6EF border and rounded corners (about 16 px).
- Summary card first: a circular score ring (teal arc proportional to the score, number in the centre in navy), the verdict and 2–3 key points.
- Status chips as small rounded labels: green #1F9D63 pass, amber #E0A000 warning, red #D64545 fail, grey #8A90A2 not checked.
- Tables with muted header text #5B6275 and thin row lines; evidence in small monospace.
- Footer line with the plugin name.
