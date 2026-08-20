<!-- markdownlint-disable -->

# Hardening Report: Azure--use-kubelogin/v1.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Azure--use-kubelogin/v1.3** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow file `.github/workflows/test-kubelogin.yaml` contains an unpinned `uses:` reference: `uses: actions/checkout@v6`. This is a mutable tag reference rather than an immutable 40-character commit SHA, making it vulnerable to supply-chain attacks if the tag is moved. The second checkout step in the same file correctly uses a SHA pin (`actions/checkout@de0fac2e4500dabe0009e67214ff5f5447ce83dd`), but the first step in the `test-with-github-token` job does not.

Locations:

- `.github/workflows/test-kubelogin.yaml:57`

### missing-permissions (severity: medium)

The workflow file `.github/workflows/integration-test.yaml` has no top-level `permissions:` key, and two of its five jobs — `test-specified-versions-with-github-token` and `test-github-api-override` — also have no job-level `permissions:` key. Without explicit permissions, these jobs inherit the default repository permissions (which may include `write` access to contents and other scopes), violating the principle of least privilege. The other three jobs in the same file do specify `permissions: contents: read`.

Locations:

- `.github/workflows/integration-test.yaml:38`
- `.github/workflows/integration-test.yaml:52`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

1. Fixed unpinned-uses in .github/workflows/test-kubelogin.yaml: replaced `actions/checkout@v6` with `actions/checkout@d23441a48e516b6c34aea4fa41551a30e30af803 # v6` (resolved via lookup_action_sha). 2. Fixed missing-permissions in .github/workflows/integration-test.yaml: added `permissions: contents: read` to both `test-specified-versions-with-github-token` (line 38) and `test-github-api-override` (line 52) jobs, consistent with the other three jobs in the same file that already had this permission.

