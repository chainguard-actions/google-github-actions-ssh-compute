<!-- markdownlint-disable -->

# Hardening Report: google-github-actions--ssh-compute/v1.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **google-github-actions--ssh-compute/v1.1.2** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions/reusable workflows by mutable tag refs instead of full 40-character SHA commit hashes, making them vulnerable to supply-chain attacks if the tag is moved.

Failing references:
- `.github/workflows/unit.yml`: `actions/checkout@v4` and `actions/setup-node@v3`
- `.github/workflows/draft-release.yml`: `google-github-actions/.github/.github/workflows/draft-release.yml@v0`
- `.github/workflows/release.yml`: `google-github-actions/.github/.github/workflows/release.yml@v1` (the `# ratchet:exclude` comment does not make the ref safe)

Locations:

- `.github/workflows/unit.yml:27`
- `.github/workflows/unit.yml:29`
- `.github/workflows/draft-release.yml:17`
- `.github/workflows/release.yml:12`

### missing-permissions (severity: medium)

Three workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, the GITHUB_TOKEN is granted its default (broad) permissions, violating the principle of least privilege.

- `.github/workflows/unit.yml`: no permissions declared at any level
- `.github/workflows/draft-release.yml`: no permissions declared at any level
- `.github/workflows/release.yml`: no permissions declared at any level

Locations:

- `.github/workflows/unit.yml:1`
- `.github/workflows/draft-release.yml:1`
- `.github/workflows/release.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed all three workflow files:

1. `.github/workflows/unit.yml`: Pinned `actions/checkout@v4` to SHA `11d5960a326750d5838078e36cf38b85af677262` and `actions/setup-node@v3` to SHA `3235b876344d2a9aa001b8d1453c930bba69e610`. Added `permissions: {}` at the top level.

2. `.github/workflows/draft-release.yml`: Pinned `google-github-actions/.github/.github/workflows/draft-release.yml@v0` to SHA `7db31c4dda4d67c9f66fc070137881f1cb4c7c37`. Added `permissions: {}` at the top level.

3. `.github/workflows/release.yml`: Pinned `google-github-actions/.github/.github/workflows/release.yml@v1` to SHA `6900f1ed495961bca1d6c2e6cb679e7ce7e23a88` (removing the `# ratchet:exclude` comment). Added `permissions: {}` at the top level.

All original tags are preserved as inline comments (e.g., `# v4`) for readability.

