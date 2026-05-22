# Ozark internal standard library — index

Single source of truth for every `osl-`prefixed fork in the `Ozark-Security-Labs` org and which Ozark product is currently consuming which pinned SHA.

The fork itself is canonical for security history; this index is for cross-cutting visibility ("which version of `osl-glob` is `deterministic-deps` on right now?", "is anyone else consuming `osl-js-yaml`?").

## Update protocol

Update this file in the same change that bumps a consumer's pinned SHA. During the pilot this is manual; if it becomes a frequent enough operation it will be automated via a workflow that reads the consumer's manifest on PR merge.

## Forks

| Fork | Upstream | Type | First forked | Consumers (pinned SHA) |
|---|---|---|---|---|
| [osl-actions-core](https://github.com/Ozark-Security-Labs/osl-actions-core) | [`actions/toolkit` `packages/core/`](https://github.com/actions/toolkit/tree/main/packages/core) | npm package | _pending pilot_ | `deterministic-deps` (TBD) |
| [osl-glob](https://github.com/Ozark-Security-Labs/osl-glob) | [`isaacs/node-glob`](https://github.com/isaacs/node-glob) | npm package | _pending pilot_ | `deterministic-deps` (TBD) |
| [osl-js-yaml](https://github.com/Ozark-Security-Labs/osl-js-yaml) | [`nodeca/js-yaml`](https://github.com/nodeca/js-yaml) | npm package | _pending pilot_ | `deterministic-deps` (TBD) |
| [osl-minimatch](https://github.com/Ozark-Security-Labs/osl-minimatch) | [`isaacs/minimatch`](https://github.com/isaacs/minimatch) | npm package | _pending pilot_ | `deterministic-deps` (TBD) |

## Open fork proposals

Coding agents working in Ozark product repos cannot add external dependencies directly. Instead they file a proposal under `.ozark/fork-proposals/<dep>.md` in the consuming repo; an auto-issue workflow mirrors each proposal here as an issue titled `Fork request: osl-<dep>`. Triage happens in this repo's [Issues tab](https://github.com/Ozark-Security-Labs/.github/issues).
