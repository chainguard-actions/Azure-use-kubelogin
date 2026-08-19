<!-- markdownlint-disable -->

# Hardening Report: Azure--use-kubelogin/v1.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Azure--use-kubelogin/v1.1** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags (@v3) instead of immutable 40-character SHA commit digests. This exposes the workflow to supply-chain attacks if the tag is moved to a malicious commit. Affected references: `actions/checkout@v3` and `actions/setup-node@v3` in unit-test.yaml; `actions/checkout@v3` (twice) in test-kubelogin.yaml.

Locations:

- `.github/workflows/unit-test.yaml:10`
- `.github/workflows/unit-test.yaml:11`
- `.github/workflows/test-kubelogin.yaml:57`
- `.github/workflows/test-kubelogin.yaml:80`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` block, and no individual jobs define job-level `permissions:` blocks. Without explicit permissions, workflows run with the default repository token permissions, which may be overly broad (e.g., write access to contents). All three workflow files are affected: integration-test.yaml, test-kubelogin.yaml, and unit-test.yaml.

Locations:

- `.github/workflows/integration-test.yaml:1`
- `.github/workflows/test-kubelogin.yaml:1`
- `.github/workflows/unit-test.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all 4 unpinned action references by resolving them to full 40-character SHA digests: actions/checkout@v3 → @a37ce9120846195fa4ece8f58b268e6043cb2f26 (3 occurrences across unit-test.yaml and test-kubelogin.yaml) and actions/setup-node@v3 → @3235b876344d2a9aa001b8d1453c930bba69e610 (1 occurrence in unit-test.yaml). Added `permissions: {}` top-level blocks to all three workflow files (unit-test.yaml, test-kubelogin.yaml, integration-test.yaml) to enforce least-privilege access.

