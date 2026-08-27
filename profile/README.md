# Ozark Security Labs

Trust infrastructure for the agent internet.

The internet is filling up with machine actors. OSL builds the observatory and evidence layer so those interactions can be inspected, attributed, and defended.

**Glimmer** is the first product: a commercial threat-intelligence publisher for MCP and x402. We run the Decoys. Customers receive signed Feeds and lookup APIs, not raw telemetry. Glimmer is pre-launch — architecture confirmed, public access gated on legal and hosting-provider clearance.

- [ozarksecuritylabs.com](https://ozarksecuritylabs.com)
- [Glimmer](https://ozarksecuritylabs.com/glimmer)

## Public tools

This org also hosts open-source product-security tools. They are not Glimmer.

- [**deterministic-deps**](https://github.com/Ozark-Security-Labs/deterministic-deps) — GitHub Action that flags non-deterministic dependency declarations across 9 ecosystems.
- [**AuthMap**](https://github.com/Ozark-Security-Labs/AuthMap) — authorization coverage mapping across routes, handlers, and data mutations.
- [**SessionScope**](https://github.com/Ozark-Security-Labs/SessionScope) — session, cookie, JWT, and token lifecycle auditor.
- [**PkgWarden**](https://github.com/Ozark-Security-Labs/PkgWarden) — package-manager hardening advisor for dependency-ingestion controls.
- [**rulepath**](https://github.com/Ozark-Security-Labs/rulepath) — deterministic analysis of business-logic flaws and invariant enforcement.

## Internal standard library

Owned, trimmed `osl-` forks of external dependencies live in this org. Policy, runbook, and the current index:

- [`docs/ozark-stdlib.md`](docs/ozark-stdlib.md)
- [`docs/fork-and-trim-workflow.md`](docs/fork-and-trim-workflow.md)
- [`docs/fork-proposal-template.md`](docs/fork-proposal-template.md)

If you found this org through a CVE advisory, the fork's `CHANGELOG-OZARK.md` is the per-dep history.
