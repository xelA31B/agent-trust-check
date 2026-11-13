# Agent Trust Check — Claude plugin

Before you install an MCP server, plugin or skill, know who made it, what it can do and whether it is safe. Agent Trust Check combines public security databases with a review of the component's tools, permissions and code — and gives a clear verdict.

## Install

**Claude app (web, desktop):** Plugins → add a marketplace from a GitHub repository → `xelA31B/agent-trust-check` → install **Agent Trust Check**. Once approved in the Claude directory, search for **Agent Trust Check** in Plugins.

**Claude Code (VS Code or terminal):**

```
/plugin marketplace add xelA31B/agent-trust-check
/plugin install agent-trust-check@agent-trust-check
```

## Commands

Type `/` in Claude and pick a command:

| Command | What it does |
|---|---|
| `/trust-check [MCP server, package, repo or URL]` | Pre-install trust verdict: ✅ Install · ⚠️ With conditions · ⛔ Do not install |
| `/trust-compare [job or list]` | Compare 2–5 options for the same job and recommend the safest |
| `/trust-watch [scope]` | Re-check installed components for new advisories and changes |

## Data sources (free, public)

| Source | What it adds |
|---|---|
| OpenSSF Scorecard | Security health score of the repository (maintenance, code review, branch protection, pinned dependencies, signed releases…) |
| deps.dev | Versions, licences and known vulnerabilities of the exact package version |
| GitHub | Owner, age, activity, archived status |
| npm / PyPI | Maintainers, release history, install scripts |
| Official MCP Registry | Verified publisher namespace, declared packages and remotes |

Risks are mapped to the OWASP Top 10 for MCP and for LLM Applications. Every finding comes with evidence.

## Where it works best

Full data checks work best in **Claude Code** (VS Code or terminal). In the Claude web and mobile apps some direct API requests can be blocked; the plugin then uses web search, asks you to open a link, and marks unchecked sources in the report.

## Principles

Read-only: never installs, runs or connects to what it checks. Concise reports in the language of your request, as a branded HTML report where supported. Schedule regular re-checks on your own frequency, day and time.

## License

MIT
