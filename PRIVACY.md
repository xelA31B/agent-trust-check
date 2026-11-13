# Privacy Policy — Agent Trust Check

_Last updated: 13 November 2026_

This plugin is a set of instructions (skills) that runs inside Claude. It has no servers of its own.

- **No data collection.** The plugin author does not receive, store or process any data or reports.
- **Public lookups.** To check a component, Claude sends its public identifiers (package name, version, repository name) to free public APIs: OpenSSF Scorecard (api.scorecard.dev), deps.dev (api.deps.dev), GitHub (api.github.com), npm (registry.npmjs.org), PyPI (pypi.org) and the official MCP Registry (registry.modelcontextprotocol.io). No personal data, files or secrets are sent.
- **Configuration reading.** For `trust-watch`, Claude reads the list of installed components from agent configuration in your session. Secret values are never printed or sent.
- **Optional file.** A baseline report (`TRUST_WATCH_BASELINE.md`) is saved only with your confirmation and stays in your project.
- **No tracking, cookies or analytics.**

Questions: https://github.com/xelA31B/agent-trust-check/issues
