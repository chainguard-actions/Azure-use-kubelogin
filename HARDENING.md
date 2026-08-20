<!-- markdownlint-disable -->

# Hardening Report: Azure--use-kubelogin/v1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Azure--use-kubelogin/v1.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference GitHub Actions using mutable version tags (e.g. @v3) instead of full 40-character commit SHAs. This exposes the workflow to supply-chain attacks where a tag could be silently moved to point to malicious code.

Failing references:
- .github/workflows/test-kubelogin.yaml: `uses: actions/checkout@v3` (lines 62 and 80)
- .github/workflows/unit-test.yaml: `uses: actions/checkout@v3` (line 12), `uses: actions/setup-node@v3` (line 13)

These should be pinned to full SHA digests, e.g. `actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v3`.

Locations:

- `.github/workflows/test-kubelogin.yaml:62`
- `.github/workflows/test-kubelogin.yaml:80`
- `.github/workflows/unit-test.yaml:12`
- `.github/workflows/unit-test.yaml:13`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` block, and no individual job within them defines job-level permissions. Without explicit permissions, GitHub Actions defaults to the repository's default token permissions (which may be `write-all` for older repositories), granting broader access than necessary. All three files should declare minimal required permissions (e.g. `permissions: read-all` or specific scopes).

Affected files:
- .github/workflows/integration-test.yaml
- .github/workflows/test-kubelogin.yaml
- .github/workflows/unit-test.yaml

Locations:

- `.github/workflows/integration-test.yaml:1`
- `.github/workflows/test-kubelogin.yaml:1`
- `.github/workflows/unit-test.yaml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all findings across three workflow files:

1. **unpinned-uses**: Pinned all mutable action references to full commit SHAs:
   - `actions/checkout@v3` → `actions/checkout@a37ce9120846195fa4ece8f58b268e6043cb2f26 # v3` (both occurrences in test-kubelogin.yaml, and in unit-test.yaml)
   - `actions/setup-node@v3` → `actions/setup-node@3235b876344d2a9aa001b8d1453c930bba69e610 # v3` (in unit-test.yaml)

2. **missing-permissions**: Added top-level `permissions: contents: read` block to all three workflow files:
   - `.github/workflows/integration-test.yaml`
   - `.github/workflows/test-kubelogin.yaml`
   - `.github/workflows/unit-test.yaml`

   `contents: read` is the minimum needed for checkout operations. The integration-test.yaml only calls reusable workflows so it also only needs read access.

