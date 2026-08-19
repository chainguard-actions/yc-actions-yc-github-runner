<!-- markdownlint-disable -->

# Hardening Report: yc-actions--yc-github-runner/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **yc-actions--yc-github-runner/v2.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions using mutable tags instead of pinned full-length SHA commits. This exposes the workflow to supply-chain attacks if the tag is moved to a different (potentially malicious) commit. Failing references:
- .github/workflows/check-dist.yml: `actions/checkout@v4`, `actions/setup-node@v4.4.0`, `actions/upload-artifact@v4`
- .github/workflows/test.yml: `actions/checkout@v4`
All should be replaced with their corresponding 40-character hex commit SHAs.

Locations:

- `.github/workflows/check-dist.yml:20`
- `.github/workflows/check-dist.yml:23`
- `.github/workflows/check-dist.yml:43`
- `.github/workflows/test.yml:13`

### missing-permissions (severity: medium)

Neither workflow file declares a top-level `permissions:` block, and no job in either file has a job-level `permissions:` block. Without explicit permissions, GitHub Actions grants the default token permissions (which can include write access to repository contents and other scopes depending on organization settings). Each workflow should declare minimal required permissions explicitly.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. check-dist.yml:
   - Added top-level `permissions: contents: read` block
   - Pinned actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 # v4
   - Pinned actions/setup-node@v4.4.0 → @49933ea5288caeca8642d1e84afbd3f7d6820020 # v4.4.0
   - Pinned actions/upload-artifact@v4 → @ea165f8d65b6e75b540449e92b4886f43607fa02 # v4

2. test.yml:
   - Added top-level `permissions: contents: read` block
   - Pinned actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 # v4

All SHAs were resolved using lookup_action_sha against the actual upstream repositories.

