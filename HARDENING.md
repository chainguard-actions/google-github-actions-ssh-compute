<!-- markdownlint-disable -->

# Hardening Report: google-github-actions--ssh-compute/v1.1.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **google-github-actions--ssh-compute/v1.1.4** was hardened automatically. 2 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions/reusable workflows by mutable tags instead of full 40-character commit SHAs, making them vulnerable to supply-chain attacks if the tag is moved.

.github/workflows/draft-release.yml:
  - uses: 'google-github-actions/.github/.github/workflows/draft-release.yml@v3' (line 17)

.github/workflows/release.yml:
  - uses: 'google-github-actions/.github/.github/workflows/release.yml@v3' (line 10)

workflow-examples/ssh-command-example.yml:
  - uses: 'actions/checkout@v3' (line 31)
  - uses: 'google-github-actions/auth@v2' (line 36)
  - uses: 'google-github-actions/ssh-compute@v1' (line 42)

workflow-examples/ssh-script-example.yml:
  - uses: 'actions/checkout@v3' (line 31)
  - uses: 'google-github-actions/auth@v2' (line 36)
  - uses: 'google-github-actions/ssh-compute@v1' (line 42)

Locations:

- `.github/workflows/draft-release.yml:17`
- `.github/workflows/release.yml:10`
- `workflow-examples/ssh-command-example.yml:31`
- `workflow-examples/ssh-command-example.yml:36`
- `workflow-examples/ssh-command-example.yml:42`
- `workflow-examples/ssh-script-example.yml:31`
- `workflow-examples/ssh-script-example.yml:36`
- `workflow-examples/ssh-script-example.yml:42`

### script-injection (severity: high)

Sub-rule (a): GitHub Actions expressions are interpolated directly inside run: shell command strings. In both workflow example files, the 'Show Output' step uses echo '${{ steps.ssh.outputs.stdout }}' and echo '${{ steps.ssh.outputs.stderr }}' inside a run: block. The values of steps.ssh.outputs.stdout and steps.ssh.outputs.stderr are workflow-controllable (they come from the SSH command output, which could be attacker-influenced) and are expanded by the YAML template engine before the shell ever sees them, enabling script injection. These should be passed via env: variables and double-quoted in the shell instead.

Locations:

- `workflow-examples/ssh-command-example.yml:50`
- `workflow-examples/ssh-command-example.yml:51`
- `workflow-examples/ssh-script-example.yml:50`
- `workflow-examples/ssh-script-example.yml:51`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection

**Notes:**

Fixed all 8 unpinned action references by resolving them to full 40-character commit SHAs using lookup_action_sha: actions/checkout@v3→a37ce9120846195fa4ece8f58b268e6043cb2f26, google-github-actions/auth@v2→c200f3691d83b41bf9bbd8638997a462592937ed, google-github-actions/ssh-compute@v1→a748312ad1aca8faf0a14c468e25b04d80bfb018, google-github-actions/.github@v3→29c6d38eeb974133b4b66401985f7c70cf4a6681. Fixed script injection in both workflow example files by moving steps.ssh.outputs.stdout and steps.ssh.outputs.stderr into env: block variables (STDOUT/STDERR) and referencing them as double-quoted shell variables in the run: block.

