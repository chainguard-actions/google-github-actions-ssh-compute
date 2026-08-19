<!-- markdownlint-disable -->

# Hardening Report: google-github-actions--ssh-compute/v1.1.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **google-github-actions--ssh-compute/v1.1.5** was hardened automatically. 1 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Two workflow files reference reusable workflows by a mutable tag (`@v3`) rather than a full 40-character commit SHA. This means a compromised or modified tag could silently swap in malicious workflow code. The `# ratchet:exclude` annotation suppresses the pinning tool but does not mitigate the supply-chain risk.

Failing references:
- `.github/workflows/draft-release.yml`: `uses: 'google-github-actions/.github/.github/workflows/draft-release.yml@v3'`
- `.github/workflows/release.yml`: `uses: 'google-github-actions/.github/.github/workflows/release.yml@v3'`

Locations:

- `.github/workflows/draft-release.yml:17`
- `.github/workflows/release.yml:9`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses

**Notes:**

Pinned both reusable workflow references in .github/workflows/draft-release.yml and .github/workflows/release.yml from the mutable '@v3' tag to the full commit SHA '29c6d38eeb974133b4b66401985f7c70cf4a6681' (resolved via git ls-remote). The '# ratchet:exclude' annotation was replaced with '# v3' to preserve readability while eliminating the supply-chain risk.

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses

**Notes:**

Fixed both workflow-examples/ssh-command-example.yml and workflow-examples/ssh-script-example.yml:

1. script-injection: Moved ${{ steps.ssh.outputs.stdout }} and ${{ steps.ssh.outputs.stderr }} from inline shell interpolation into the step's env: block (as STDOUT and STDERR), then referenced them as plain env vars ($STDOUT, $STDERR) in the run: block.

2. unpinned-uses: Pinned all three action references to full commit SHAs:
   - actions/checkout@v3 → @f43a0e5ff2bd294095638e18286ca9a3d1956744 # v3
   - google-github-actions/auth@v2 → @c200f3691d83b41bf9bbd8638997a462592937ed # v2
   - google-github-actions/ssh-compute@v1 → @a748312ad1aca8faf0a14c468e25b04d80bfb018 # v1

