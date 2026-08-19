<!-- markdownlint-disable -->

# Hardening Report: yc-actions--yc-github-runner/v1.5.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **yc-actions--yc-github-runner/v1.5.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions using mutable version tags instead of full 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the referenced tag is moved or the upstream action is compromised.

In .github/workflows/check-dist.yml:
  - uses: actions/checkout@v4  (line ~20)
  - uses: actions/setup-node@v4.0.2  (line ~23)
  - uses: actions/upload-artifact@v4  (line ~43)

In .github/workflows/test.yml:
  - uses: actions/checkout@v4  (line ~13)

All should be pinned to their full SHA digest, e.g. actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4

Locations:

- `.github/workflows/check-dist.yml:20`
- `.github/workflows/check-dist.yml:23`
- `.github/workflows/check-dist.yml:43`
- `.github/workflows/test.yml:13`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` block, and no job within either file defines job-level permissions. Without explicit permissions, workflows run with the default token permissions (which may include write access to repository contents, pull requests, etc.), violating the principle of least privilege.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. check-dist.yml:
   - Added `permissions: {}` top-level block
   - Pinned actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 # v4
   - Pinned actions/setup-node@v4.0.2 → @60edb5dd545a775178f52524783378180af0d1f8 # v4.0.2
   - Pinned actions/upload-artifact@v4 → @ea165f8d65b6e75b540449e92b4886f43607fa02 # v4

2. test.yml:
   - Added `permissions: {}` top-level block
   - Pinned actions/checkout@v4 → @11d5960a326750d5838078e36cf38b85af677262 # v4

All SHAs were resolved using lookup_action_sha against the actual upstream repositories.

