---
name: trust-watch
description: >
  This skill should be used when the user asks to "monitor my MCP servers", "re-check installed plugins",
  "any new vulnerabilities in my MCP servers", "watch list for agent tools", "weekly security check of my
  AI tools", or wants installed MCP servers, plugins and skills re-checked for new advisories, ownership
  or version changes. Also triggers on the same requests in Russian ("следи за моими MCP-серверами",
  "проверь установленные плагины на новые уязвимости") or Hungarian.
metadata:
  version: "0.1.0"
---

# Trust Watch

Re-check installed agent components and report only what changed since the last check.

## Rules

- Language of the user's request. Concise, professional, no filler.
- Read-only. Never update, uninstall or reconfigure anything; recommend actions.
- Same sources and scoring as `trust-check`.

## Steps

1. **Inventory.** Ask the user for the list of installed MCP servers, plugins and skills (name, source, version), or use the plugin and MCP list that the agent itself displays. Use names and versions only; do not open configuration files.
2. **Baseline.** Load `TRUST_WATCH_BASELINE.md` if it exists; otherwise this run creates the first one (ask before saving).
3. **Re-check each component:** new advisories (deps.dev), Scorecard change, new releases, repository archived or transferred, maintainer changes, newly added install scripts, MCP Registry status change.
4. **Report:**

```
# Trust Watch — [scope] · [date]

**Status:** 🟢 No new risks / 🟡 Review needed / 🔴 Action required

## New risks
| Component | Change | Severity | Evidence | Recommended action |

## Updates available
| Component | Installed | Latest | Safe to upgrade? |

## No change
- N components unchanged
```

5. **Offer** to update the baseline (ask first).

## Branded report

If the environment can render HTML artifacts or files, deliver the full report using `references/report-template.md` (fill the placeholders, keep the design and colours; use the score ring for the main score and status chips for pass / warn / fail / not checked). Otherwise use the markdown format above. Keep the chat reply to a short summary either way.

## Recurring run

Recommended cadence: weekly.

Let the user choose their own schedule. Ask in one short message, with the recommendation pre-filled:

- **Frequency:** daily · weekly · every 2 weeks · monthly · quarterly · custom
- **Day:** e.g. Monday, Friday, 1st of the month
- **Time and time zone:** e.g. 09:00 Europe/Budapest (use the user's time zone if known)
- **Delivery:** in this chat, a new chat, or a document

If the environment offers scheduled tasks (a scheduling tool is available), create the task only after the user confirms these choices. Write the scheduled prompt so it is self-contained: what to check, which skill to run, the report language, and where to deliver the result. Confirm what was scheduled in one line. The user can change or cancel the schedule at any time; when asked, update or remove it.

If no scheduling tool is available, state the chosen or recommended cadence and suggest a calendar reminder.
