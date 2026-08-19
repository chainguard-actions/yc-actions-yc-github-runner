<!-- markdownlint-disable -->

# Hardening Report: yc-actions--yc-github-runner/v1.7.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **yc-actions--yc-github-runner/v1.7.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files use mutable tag refs instead of full 40-character SHA commit digests, making the action vulnerable to supply-chain attacks if the referenced tag is moved or overwritten.

In `.github/workflows/check-dist.yml`:
- `uses: actions/checkout@v4` (tag, not SHA)
- `uses: actions/setup-node@v4.1.0` (tag, not SHA)
- `uses: actions/upload-artifact@v4` (tag, not SHA)

In `.github/workflows/test.yml`:
- `uses: actions/checkout@v4` (tag, not SHA)

All should be pinned to their full 40-character commit SHA, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`.

Locations:

- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:43`
- `.github/workflows/test.yml:13`

### missing-permissions (severity: medium)

Neither `.github/workflows/check-dist.yml` nor `.github/workflows/test.yml` declares a top-level `permissions:` key, and no job within either file defines its own `permissions:` block. Without explicit permissions, workflows run with the default repository token permissions, which may be broader than necessary (e.g. write access to contents, pull-requests, etc.). Each workflow should declare minimal required permissions at the top level or per-job.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. check-dist.yml:
   - Added top-level `permissions: contents: read`
   - Pinned actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 # v4
   - Pinned actions/setup-node@v4.1.0 → @39370e3970a6d050c480ffad4ff0ed4d3fdee5af # v4.1.0
   - Pinned actions/upload-artifact@v4 → @ea165f8d65b6e75b540449e92b4886f43607fa02 # v4

2. test.yml:
   - Added top-level `permissions: contents: read`
   - Pinned actions/checkout@v4 → @34e114876b0b11c390a56381ad16ebd13914f8d5 # v4

All SHAs were resolved using lookup_action_sha to ensure accuracy.

