# Fork-and-trim workflow

The procedure for bringing a new external dependency into the Ozark internal standard library. Followed verbatim every time a fork proposal in a consumer repo is accepted.

## Prerequisites

- A `.ozark/fork-proposals/<dep>.md` file exists in the consuming repo, the mirrored issue in `Ozark-Security-Labs/.github` is approved, and the `proposed_fork` name uses the `osl-` prefix.
- You are a member of `Ozark-Security-Labs` with repo-create rights on the org.

## Steps

### 1. Fork the upstream repo

```bash
gh repo fork <upstream-owner>/<upstream-repo> \
  --org Ozark-Security-Labs \
  --fork-name osl-<dep> \
  --default-branch-only \
  --remote=false
```

If the upstream is a monorepo and only a sub-package is wanted (e.g., `actions/toolkit` for `osl-actions-core`), fork the whole repo and prune after step 4.

### 2. Anchor the fork point

Clone the new fork locally, then tag the commit you forked from before any Ozark changes land. This is what future CVE cherry-picks diff against.

```bash
gh repo clone Ozark-Security-Labs/osl-<dep>
cd osl-<dep>
git tag upstream/$(git rev-parse HEAD)
git push origin upstream/$(git rev-parse HEAD)
```

### 3. Rename the package

Update the in-package identity to `osl-<dep>`:

- **npm:** `package.json` `"name"` field → `osl-<dep>`. Bump `"version"` to `0.0.1` (Ozark versioning restarts independent of upstream).
- **Cargo:** `Cargo.toml` `[package] name = "osl-<dep>"`. Update any internal `path = "..."` references and re-run `cargo check`.
- **Go:** `go.mod` `module github.com/Ozark-Security-Labs/osl-<dep>`. Run `go mod tidy`.

### 4. Trim

Delete code paths the consumer does not use. The fork-proposal's `surface_used` field is the target.

- Delete tests for removed code; the remaining test suite must still pass.
- Record every removal in `OZARK-NOTES.md` (see template below).
- If the upstream was a monorepo, this is also when you prune unrelated packages.

### 5. Lock the fork's own supply chain

Inside the fork, apply the same discipline `deterministic-deps` recommends:

- Pin every dep exactly. Commit the lockfile.
- SHA-pin every GitHub Action used in the fork's CI.
- Add the standard fork CI workflow (`.github/workflows/ci.yml`) — at minimum: install, lint, test, and run `deterministic-deps` itself against the fork as a self-check.

### 6. Add the required Ozark files

- `OZARK-NOTES.md` — what was kept, what was removed, the upstream commit forked from, the date.
- `CHANGELOG-OZARK.md` — header only at this point; future entries log each Ozark-side patch.
- `LICENSE-UPSTREAM` — verbatim copy of the upstream license. The fork's own `LICENSE` may add Ozark headers but must not weaken upstream terms.
- `README.md` — replace upstream's with a short notice that this is an Ozark internal fork; link to upstream and to this profile repo.

### 7. Push and capture the SHA

```bash
git checkout -b ozark/initial-trim
git add .
git commit -m "ozark: initial trim and rename to osl-<dep>"
git push -u origin ozark/initial-trim
gh pr create --fill --base main
# After review + merge, capture the merge commit SHA:
gh api repos/Ozark-Security-Labs/osl-<dep>/branches/main --jq .commit.sha
```

### 8. Open the consumer PR

In the consuming repo:

- Update the manifest entry to `"osl-<dep>": "github:Ozark-Security-Labs/osl-<dep>#<full-merge-sha>"` (npm syntax shown; see plan for Cargo/Go equivalents).
- Update every import / require / use site in source code from `<dep>` to `osl-<dep>`.
- Regenerate the lockfile.
- Update `docs/ozark-stdlib.md` in `Ozark-Security-Labs/.github`: set the consumer + SHA column for this fork.
- Both repos' CI must be green before merging.

### 9. Close the proposal

In the consumer PR description, link the proposal file and the auto-created fork-request issue. Close the issue when the PR merges.

## CVE / patch lifecycle

When a CVE or logic bug is announced upstream:

1. Read the upstream patch.
2. Cherry-pick the relevant lines into the fork — **do not** merge upstream wholesale (that re-introduces trimmed code).
3. Add a `CHANGELOG-OZARK.md` entry: date, CVE / bug reference, summary, author.
4. PR + merge in the fork; capture the new HEAD SHA.
5. Bump the SHA in every consumer's manifest; update `docs/ozark-stdlib.md`.

## OZARK-NOTES.md template

```markdown
# OZARK-NOTES

**Forked from:** <upstream-org>/<upstream-repo>@<full-sha> (<short-date>)
**Anchor tag in this repo:** upstream/<full-sha>

## Surface kept

- <module / function / API surface the consumer actually uses>

## Surface removed

- <path / feature> — <one-sentence reason>

## Build / runtime notes

- <anything unusual: build scripts, native bindings, optional peer deps, etc.>
```

## CHANGELOG-OZARK.md template

```markdown
# CHANGELOG-OZARK

All Ozark-side patches applied on top of the upstream fork point.

## YYYY-MM-DD — <one-line summary>

- **Type:** CVE patch | bug fix | new feature | trim
- **Reference:** CVE-XXXX-XXXX / upstream PR # / internal issue
- **Author:** <github-handle>
- **Notes:** <free text>
```
