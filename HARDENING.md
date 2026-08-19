<!-- markdownlint-disable -->

# Hardening Report: yc-actions--yc-github-runner/v1.3.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **yc-actions--yc-github-runner/v1.3.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Both workflow files reference GitHub Actions using mutable tags instead of full 40-character SHA digests, making them vulnerable to supply-chain attacks if the referenced tags are moved or compromised.

In .github/workflows/check-dist.yml:
- `uses: actions/checkout@v3` (tag, not SHA)
- `uses: actions/setup-node@v3.6.0` (tag, not SHA)
- `uses: actions/upload-artifact@v3` (tag, not SHA)

In .github/workflows/test.yml:
- `uses: actions/checkout@v3` (tag, not SHA)

All should be pinned to their full commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:44`
- `.github/workflows/test.yml:13`

### missing-permissions (severity: medium)

Neither .github/workflows/check-dist.yml nor .github/workflows/test.yml defines a top-level `permissions:` block, and no job within either file defines job-level permissions. Without explicit permissions, workflows inherit the default repository permissions (which may be broad, e.g. write access to contents). A minimal `permissions: {}` or specific scopes (e.g. `contents: read`) should be declared at the top level or per job.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. **unpinned-uses** — Pinned all 4 action references to full 40-character SHA digests with original tags preserved as comments:
   - `actions/checkout@v3` → `@a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3` (in both check-dist.yml and test.yml)
   - `actions/setup-node@v3.6.0` → `@64ed1c7eab4cce3362f8c340dee64e5eaeef8f7c # v3.6.0` (check-dist.yml)
   - `actions/upload-artifact@v3` → `@ff15f0306b3f739f7b6fd43fb5d26cd321bd4de5 # v3` (check-dist.yml)

2. **missing-permissions** — Added `permissions: contents: read` at the top level of both `.github/workflows/check-dist.yml` and `.github/workflows/test.yml`. The workflows only need to read repository contents for checkout, so `contents: read` is the minimal required permission.

