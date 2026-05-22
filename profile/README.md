# Ozark Security Labs

We build deterministic application-security tooling for developers and AppSec teams.

## Products

- [**deterministic-deps**](https://github.com/Ozark-Security-Labs/deterministic-deps) — GitHub Action that flags non-deterministic dependency declarations across 9 ecosystems.
- [**AuthMap**](https://github.com/Ozark-Security-Labs/AuthMap) — authorization coverage mapping across routes, handlers, and data mutations.
- [**SessionScope**](https://github.com/Ozark-Security-Labs/SessionScope) — session, cookie, JWT, and token lifecycle auditor.
- [**PkgWarden**](https://github.com/Ozark-Security-Labs/PkgWarden) — package-manager hardening advisor for dependency-ingestion controls.
- [**rulepath**](https://github.com/Ozark-Security-Labs/rulepath) — deterministic analysis of business-logic flaws and invariant enforcement.

## Internal standard library

Every external dependency consumed by an Ozark product lives inside this org as an owned, trimmed `osl-`prefixed fork. We patch CVEs and bugs directly in the fork rather than waiting on upstream, and we never auto-sync — upstream commits are cherry-picked deliberately.

The full policy, the fork-and-trim runbook, and the current index of forks live in this repo:

- [`docs/ozark-stdlib.md`](docs/ozark-stdlib.md) — index of forks + the pinned SHA each consumer is on.
- [`docs/fork-and-trim-workflow.md`](docs/fork-and-trim-workflow.md) — the procedural runbook for forking, trimming, and consuming a new dep.
- [`docs/fork-proposal-template.md`](docs/fork-proposal-template.md) — the template coding agents must use to propose a new external dependency.

If you found this org through a CVE advisory or are reviewing how we handle a specific vulnerability, the fork's `CHANGELOG-OZARK.md` is the per-dep history.
