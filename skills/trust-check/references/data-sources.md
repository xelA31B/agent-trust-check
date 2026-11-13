# Free public data sources

All sources below are free and use simple GET requests that return JSON. No API keys are required. Respect rate limits; if a request fails, mark the source "not available" and continue.

Some Claude apps refuse URLs that were not shown in the conversation or found by search. Each source therefore also lists a human-readable page that web search can find; see the fallback steps in the trust-check skill.

## OpenSSF Scorecard

- Endpoint: `https://api.scorecard.dev/projects/github.com/<owner>/<repo>`
- Use: `score` (0–10) and checks. Most relevant checks: `Maintained`, `Code-Review`, `Branch-Protection`, `Dangerous-Workflow`, `Pinned-Dependencies`, `Signed-Releases`, `Vulnerabilities`, `Security-Policy`, `License`, `Binary-Artifacts`.
- Coverage: repositories in the weekly scan or that publish their results. Missing repository = "not available", not a failure.
- Data licence: CDLA Permissive 2.0.
- Viewer for humans: `https://scorecard.dev/viewer/?uri=github.com/<owner>/<repo>`

## deps.dev (Open Source Insights)

- Human page: `https://deps.dev/<system>/<name>`
- Package: `https://api.deps.dev/v3/systems/<system>/packages/<name>` (system: `npm`, `pypi`, `go`, `maven`, `cargo`, `nuget`). Percent-encode the name (`@scope/pkg` → `%40scope%2Fpkg`).
- Version: `https://api.deps.dev/v3/systems/<system>/packages/<name>/versions/<version>` — licenses, `advisoryKeys`, links, publish date.
- Advisory: `https://api.deps.dev/v3/advisories/<id>` — details from OSV.
- Use the default version unless the user's config pins another.

## GitHub repository metadata

- Endpoint: `https://api.github.com/repos/<owner>/<repo>`
- Human page: `https://github.com/<owner>/<repo>`
- Use: `owner.type`, `created_at`, `pushed_at`, `archived`, `stargazers_count`, `open_issues_count`, `license`, `default_branch`.
- Unauthenticated limit is low (about 60 requests per hour); use sparingly.

## Package registries

- Human pages: `https://www.npmjs.com/package/<name>`, `https://pypi.org/project/<name>/`
- npm: `https://registry.npmjs.org/<name>` — `maintainers`, `time` (release history), `versions[v].scripts` (look for `preinstall`, `install`, `postinstall`), `repository`.
- PyPI: `https://pypi.org/pypi/<name>/json` — `info` (author, project URLs, license), `releases`.

## Official MCP Registry

- Search: `https://registry.modelcontextprotocol.io/v0.1/servers?search=<query>&version=latest`
- Server version: `https://registry.modelcontextprotocol.io/v0.1/servers/<url-encoded name>/versions/latest`
- Use: `repository`, `packages`, `remotes`, `version`, and registry metadata under `_meta` (status, published dates). A reverse-DNS namespace (`io.github.<user>/…` or a domain) indicates verified publisher ownership.
- If the API path has changed, find the current one on modelcontextprotocol.io.

## OWASP references (for mapping risks)

- OWASP Top 10 for LLM Applications
- OWASP Top 10 for MCP (verify the current version and item names by web search)
