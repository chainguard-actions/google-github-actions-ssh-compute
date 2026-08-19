<!-- markdownlint-disable -->

# Hardening Report: google-github-actions--ssh-compute/v2.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **google-github-actions--ssh-compute/v2.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Two workflow files reference reusable workflows using mutable version tags (`@v3`) instead of full 40-character commit SHAs. This means a supply-chain compromise of the referenced repository could silently alter what code runs. Both are annotated `# ratchet:exclude`, which intentionally bypasses SHA-pinning enforcement, but the references remain unpinned.

- `.github/workflows/draft-release.yml`: `uses: 'google-github-actions/.github/.github/workflows/draft-release.yml@v3'`
- `.github/workflows/release.yml`: `uses: 'google-github-actions/.github/.github/workflows/release.yml@v3'`

Locations:

- `.github/workflows/draft-release.yml:18`
- `.github/workflows/release.yml:8`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned both reusable workflow references from the mutable '@v3' tag to the full commit SHA '29c6d38eeb974133b4b66401985f7c70cf4a6681' (resolved via git ls-remote). Updated files:
- .github/workflows/draft-release.yml: pinned draft-release.yml@v3 → @29c6d38eeb974133b4b66401985f7c70cf4a6681 # v3
- .github/workflows/release.yml: pinned release.yml@v3 → @29c6d38eeb974133b4b66401985f7c70cf4a6681 # v3
The '# ratchet:exclude' annotations were removed since the references are now properly pinned by SHA.

