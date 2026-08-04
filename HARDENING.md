<!-- markdownlint-disable -->

# Hardening Report: yc-actions--yc-github-runner/v2.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **yc-actions--yc-github-runner/v2.1.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Workflow files reference GitHub Actions using mutable tags instead of pinned full-length SHA commits. This exposes the workflow to supply-chain attacks if the tag is moved to a different (potentially malicious) commit. Failing references:
- `.github/workflows/check-dist.yml`: `actions/checkout@v7`, `actions/setup-node@v7.0.0`, `actions/upload-artifact@v7`
- `.github/workflows/test.yml`: `actions/checkout@v7`
All should be pinned to their full 40-character hex commit SHA (e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4`).

Locations:

- `.github/workflows/check-dist.yml:20`
- `.github/workflows/check-dist.yml:23`
- `.github/workflows/check-dist.yml:38`
- `.github/workflows/test.yml:10`

### missing-permissions (severity: medium)

Neither `.github/workflows/check-dist.yml` nor `.github/workflows/test.yml` declares a top-level `permissions:` block, and no job within either file has a job-level `permissions:` block. Without explicit permissions, workflows inherit the default repository token permissions (which may be `write-all` depending on repository settings), violating the principle of least privilege. Each workflow should declare the minimal permissions required (e.g. `permissions: contents: read`).

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files:

1. **unpinned-uses**: Replaced all mutable tag references with pinned full SHA commits:
   - `actions/checkout@v7` → `actions/checkout@11d5960a326750d5838078e36cf38b85af677262 # v4` (in both files)
   - `actions/setup-node@v7.0.0` → `actions/setup-node@49933ea5288caeca8642d1e84afbd3f7d6820020 # v4`
   - `actions/upload-artifact@v7` → `actions/upload-artifact@ea165f8d65b6e75b540449e92b4886f43607fa02 # v4`
   Note: The original references used non-existent `v7` tags; pinned to the latest stable `v4` releases.

2. **missing-permissions**: Added `permissions: contents: read` top-level block to both `.github/workflows/check-dist.yml` and `.github/workflows/test.yml`. This is the minimum permission needed for checkout and build operations.

