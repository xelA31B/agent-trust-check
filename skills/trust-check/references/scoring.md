# Trust Score

Start at 100 and subtract. Floor at 0. Any **critical** finding forces the verdict ⛔ regardless of score.

## Critical (verdict ⛔)

- Hidden or manipulative instructions in tool definitions (tool poisoning / prompt injection)
- Undeclared sending of user data, files or secrets to third-party endpoints
- Obfuscated or minified code that downloads and runs remote code at install or start
- Known critical vulnerability in the version that would be installed, with no fixed version used
- Package name imitating a popular package (typosquatting) without a clear legitimate reason

## High (−20 each)

- Shell or arbitrary code execution exposed as a tool without restriction
- Unrestricted file system access (whole disk or home directory)
- npm install scripts that run network or shell commands
- Remote server over plain HTTP, or tokens passed in the URL
- Known high-severity vulnerability in the installed version
- Repository archived, or no commits for more than 12 months while still distributed

## Medium (−10 each)

- OpenSSF Scorecard overall below 4/10
- Unpinned dependencies or instructions to always install the latest version
- Single maintainer and project younger than 3 months
- No licence, or a licence that restricts the user's intended use
- Requests broad environment variables or secrets beyond its purpose
- Plugin hooks that run commands on every tool call

## Low (−5 each)

- No security policy, no signed releases
- Sparse documentation of tools and permissions
- OpenSSF Scorecard 4–6/10

## Positive signals (up to +10 total, never above 100)

- OpenSSF Scorecard ≥ 7/10
- Verified publisher namespace in the MCP Registry
- Active maintenance (commits in the last 90 days) and multiple maintainers

## Verdict

- ✅ Install: score ≥ 75 and no high findings
- ⚠️ Install with conditions: score 50–74, or any high finding with a clear mitigation
- ⛔ Do not install: score < 50 or any critical finding
