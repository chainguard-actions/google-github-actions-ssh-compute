<!-- markdownlint-disable -->

# Hardening Report: google-github-actions--ssh-compute/v1.1.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **google-github-actions--ssh-compute/v1.1.3** was hardened automatically. 5 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

The workflow delegates to an external reusable workflow using a mutable tag ref '@v3' instead of a full 40-character commit SHA. If the tag is moved or the upstream repository is compromised, the workflow will silently execute different code. Affected reference: 'google-github-actions/.github/.github/workflows/draft-release.yml@v3'.

Locations:

- `.github/workflows/draft-release.yml:17`

### unpinned-uses (severity: high)

The workflow delegates to an external reusable workflow using a mutable tag ref '@v3' instead of a full 40-character commit SHA. If the tag is moved or the upstream repository is compromised, the workflow will silently execute different code. Affected reference: 'google-github-actions/.github/.github/workflows/release.yml@v3'.

Locations:

- `.github/workflows/release.yml:10`

### missing-permissions (severity: medium)

The workflow has no top-level 'permissions:' key and the single job also has no 'permissions:' key. Without explicit permissions, the job inherits the default repository token permissions, which may be broader than necessary.

Locations:

- `.github/workflows/draft-release.yml:1`

### missing-permissions (severity: medium)

The workflow has no top-level 'permissions:' key and the single job also has no 'permissions:' key. Without explicit permissions, the job inherits the default repository token permissions, which may be broader than necessary.

Locations:

- `.github/workflows/release.yml:1`

### missing-permissions (severity: medium)

The workflow has no top-level 'permissions:' key and none of its jobs define a 'permissions:' key. Without explicit permissions, all jobs inherit the default repository token permissions, which may be broader than necessary.

Locations:

- `.github/workflows/unit.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions

**Notes:**

Fixed 5 findings across 3 workflow files:
1. draft-release.yml: Pinned 'google-github-actions/.github/.github/workflows/draft-release.yml@v3' to full SHA '29c6d38eeb974133b4b66401985f7c70cf4a6681' and added 'permissions: {}' at the top level.
2. release.yml: Pinned 'google-github-actions/.github/.github/workflows/release.yml@v3' to full SHA '29c6d38eeb974133b4b66401985f7c70cf4a6681' and added 'permissions: {}' at the top level.
3. unit.yml: Added 'permissions: {}' at the top level (no unpinned-uses finding here, existing action refs were already pinned to full SHAs).

