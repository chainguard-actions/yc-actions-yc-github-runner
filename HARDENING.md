<!-- markdownlint-disable -->

# Hardening Report: yc-actions--yc-github-runner/v1.6.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **yc-actions--yc-github-runner/v1.6.0** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple `uses:` references in workflow files are pinned to mutable tags or version strings instead of immutable 40-character SHA commit hashes. This exposes the workflow to supply-chain attacks if the referenced action tag is moved or compromised. Failing references: check-dist.yml — `actions/checkout@v4`, `actions/setup-node@v4.0.2`, `actions/upload-artifact@v4`; test.yml — `actions/checkout@v4`.

Locations:

- `.github/workflows/check-dist.yml:21`
- `.github/workflows/check-dist.yml:24`
- `.github/workflows/check-dist.yml:40`
- `.github/workflows/test.yml:13`

### missing-permissions (severity: medium)

Neither workflow file defines a top-level `permissions:` key, and no individual job within either file defines a `permissions:` block. Without explicit permissions, workflows run with the default (potentially broad) GITHUB_TOKEN permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/check-dist.yml:1`
- `.github/workflows/test.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed both workflow files: (1) Pinned all `uses:` references to full 40-character SHA hashes with original tags preserved as comments — actions/checkout@v4 → SHA 11d5960a..., actions/setup-node@v4.0.2 → SHA 60edb5dd..., actions/upload-artifact@v4 → SHA ea165f8d... (2) Added top-level `permissions: contents: read` block to both check-dist.yml and test.yml, granting only the minimum permission needed for the checkout step.

