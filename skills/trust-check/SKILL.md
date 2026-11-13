---
name: trust-check
description: >
  This skill should be used when the user asks "is this MCP server safe to install", "can I trust this
  plugin", "vet this MCP server", "check this agent skill before installing", "security check of an MCP
  package", "should I install this Claude plugin", or shares an MCP server name, npm/PyPI package, GitHub
  repository, remote MCP URL, plugin or skill and wants a trust verdict. Also triggers on the same
  requests in Russian ("безопасно ли ставить этот MCP-сервер", "проверь плагин перед установкой") or
  Hungarian.
metadata:
  version: "0.1.0"
---

# Trust Check

Give a pre-install trust verdict for an MCP server, Claude plugin or agent skill, based on public security data and a review of what the component can do.

## Style and evidence rules

- Language of the user's request. Concise, professional, no filler.
- Every finding follows **source → what was checked → evidence** (API field, file and line, URL). No evidence, no finding.
- Mark each check **pass / warn / fail / not available**. Data that a source does not have is "not available", never a pass.
- Read-only. Never install, run or connect to the component being checked.
- Never print sensitive values found in code; report the file name only.

## Step 1 — Identify the component

Accept any of: MCP Registry name, npm or PyPI package, GitHub repository, remote MCP URL, plugin marketplace repository, skill folder. Resolve to: repository, package(s), version, transport (local command or remote URL), publisher.

Use the official MCP Registry when the input is an MCP server (see `references/data-sources.md`).

## Step 2 — Collect external signals

Query the free public sources in `references/data-sources.md` with web fetch (GET requests, JSON):

1. **OpenSSF Scorecard** — overall score and key checks for the repository.
2. **deps.dev** — package versions, default version, publish dates, licenses, known advisories for the version that would be installed.
3. **GitHub repository metadata** — owner type, age, last push, archived, stars, open issues, license.
4. **Package registry** — npm or PyPI: maintainers, release history, install scripts (npm `preinstall` / `install` / `postinstall`).
5. **MCP Registry entry** — status, publisher namespace verification, declared packages and remotes.

Record the exact URL queried for each result. If a source is unreachable or has no data, say so.

**If direct API requests are refused** (some Claude apps only open URLs that appear in the conversation or in search results):

1. In Claude Code, use the environment's web fetch or an HTTP GET from the terminal instead.
2. Otherwise, find the same data through web search and fetch the result pages: the Scorecard viewer (`scorecard.dev/viewer/?uri=github.com/<owner>/<repo>`), the package page on deps.dev, the GitHub repository page, the npm or PyPI package page.
3. If that also fails, give the user the exact links in one message and ask them to open them and paste the result; continue with what is available meanwhile.
4. In the report, list every source that could not be checked under "Not available" and note that a full check works best in Claude Code.

## Step 3 — Review capabilities and code

Read the manifest and source (repository files) and check, mapping each risk to the current **OWASP Top 10 for MCP** and **OWASP Top 10 for LLM Applications** (verify current list names by web search if unsure):

- **Tool definitions** — hidden or manipulative instructions in tool names, descriptions or schemas (tool poisoning, prompt injection)
- **Capabilities** — file system scope, shell or code execution, network destinations, access to sensitive user data
- **Data flow** — where data can be sent; undeclared outbound endpoints
- **Supply chain** — unpinned versions, auto-install of latest, install scripts, recently transferred ownership, typosquatting of popular names
- **Transport and auth** — HTTPS for remote servers, authentication data in URLs, OAuth support
- **Plugins and skills** — hooks that run commands, instructions that disable confirmations or widen access, bundled executables

## Step 4 — Score and verdict

Use `references/scoring.md` to compute the **Trust Score (0–100)** and the verdict:

- ✅ **Install** — no high risks, healthy maintenance
- ⚠️ **Install with conditions** — list the exact conditions (pin version X, restrict paths, dedicated account, review hook)
- ⛔ **Do not install** — any critical finding

## Step 5 — Report

Report format (see Branded report for the HTML version):

```
# Trust Check — [component] [version] · [date]

**Verdict:** ✅/⚠️/⛔ … · **Trust Score:** X/100
One-line reason.

## Conditions (if any)
- …

## Signals
| Source | Result | Evidence |
(OpenSSF Scorecard · deps.dev · GitHub · npm/PyPI · MCP Registry)

## Findings
| # | Severity | Risk (OWASP ref) | Evidence | Fix / mitigation |

## What it can do
| Capability | Scope | Risk |

## Not available
- source/check — why
```

Close with one line offering: compare alternatives (`trust-compare`) or add it to the watch list (`trust-watch`).

## Recurring run

At the end of the report, offer a recurring run. Recommended cadence: weekly re-check of installed components (`trust-watch`), plus before every upgrade.

Let the user choose their own schedule. Ask in one short message, with the recommendation pre-filled:

- **Frequency:** daily · weekly · every 2 weeks · monthly · quarterly · custom
- **Day:** e.g. Monday, Friday, 1st of the month
- **Time and time zone:** e.g. 09:00 Europe/Budapest (use the user's time zone if known)
- **Delivery:** in this chat, a new chat, or a document

If the environment offers scheduled tasks (a scheduling tool is available), create the task only after the user confirms these choices. Write the scheduled prompt so it is self-contained: what to check, which skill to run, the report language, and where to deliver the result. Confirm what was scheduled in one line. The user can change or cancel the schedule at any time; when asked, update or remove it.

If no scheduling tool is available, state the chosen or recommended cadence and suggest a calendar reminder.
