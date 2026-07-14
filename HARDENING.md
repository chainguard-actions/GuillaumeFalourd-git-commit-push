<!-- markdownlint-disable -->

# Hardening Report: GuillaumeFalourd--git-commit-push/v1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **GuillaumeFalourd--git-commit-push/v1** was hardened automatically. 15 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The `run:` block in action.yml directly interpolates multiple `${{ inputs.* }}` and `${{ github.* }}` expressions into shell commands (sub-rule a). This allows a caller to inject arbitrary shell commands. Specific offending lines include:
- `TARGET_BRANCH=${{ inputs.target_branch }}` — unquoted, direct interpolation into shell variable assignment
- `if [ "${{ inputs.force }}" != "0" ]` — direct interpolation in test expression
- `if [ "${{ inputs.empty }}" != "0" ]` — direct interpolation in test expression
- `if [ "${{ inputs.tags }}" != "0" ]` — direct interpolation in test expression
- `echo "  password ${{ inputs.access_token }}"` — direct interpolation (appears twice)
- `git config --local user.email "${{ inputs.email }}"` — direct interpolation
- `git config --local user.name "${{ inputs.name }}"` — direct interpolation
- `git add ${{ inputs.files }} -v` — unquoted, direct interpolation
- `git commit -m "${{ inputs.commit_message }}"` — direct interpolation
- `git branch push-and-commit-action-${{ github.run_id }}-${{ github.job }}` — direct interpolation (used 3 times)
- `git fetch "${{ inputs.remote_repository }}"` — direct interpolation
- `git push "${{ inputs.remote_repository }}"` — direct interpolation
All inputs should be passed via `env:` variables and then referenced as quoted `"$VAR"` shell variables.

Locations:

- `action.yml:44`

### unpinned-uses (severity: high)

All three workflow files reference actions by mutable tags/branches instead of full 40-character commit SHAs, making them vulnerable to supply-chain attacks:
- `actions/checkout@v2.3.4` (tag, not SHA) — used in all three workflow files
- `GuillaumeFalourd/git-commit-push@main` (branch, not SHA) — used in all three workflow files
All `uses:` references should be pinned to a full SHA, e.g. `actions/checkout@<40-char-sha> # v2.3.4`.

Locations:

- `.github/workflows/macos_action_test.yml:13`
- `.github/workflows/macos_action_test.yml:15`
- `.github/workflows/macos_action_test.yml:20`
- `.github/workflows/macos_action_test.yml:22`
- `.github/workflows/ubuntu_action_test.yml:13`
- `.github/workflows/ubuntu_action_test.yml:15`
- `.github/workflows/ubuntu_action_test.yml:20`
- `.github/workflows/ubuntu_action_test.yml:22`
- `.github/workflows/windows_action_test.yml:13`
- `.github/workflows/windows_action_test.yml:15`
- `.github/workflows/windows_action_test.yml:20`
- `.github/workflows/windows_action_test.yml:22`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` key, and none of the individual jobs define job-level `permissions:` keys. Without explicit permissions, workflows run with the default (often `write-all`) token permissions, granting unnecessary access. Each workflow should declare minimal required permissions (e.g. `contents: write` for a commit-push workflow).

Locations:

- `.github/workflows/macos_action_test.yml:1`
- `.github/workflows/ubuntu_action_test.yml:1`
- `.github/workflows/windows_action_test.yml:1`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.target_branch }}" appears directly in run: block of step "Git push and commit"; move to env: map

Locations:

- `action.yml:61`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.force }}" appears directly in run: block of step "Git push and commit"; move to env: map

Locations:

- `action.yml:66`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.empty }}" appears directly in run: block of step "Git push and commit"; move to env: map

Locations:

- `action.yml:69`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.tags }}" appears directly in run: block of step "Git push and commit"; move to env: map

Locations:

- `action.yml:72`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.access_token }}" appears directly in run: block of step "Git push and commit"; move to env: map

Locations:

- `action.yml:78`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.access_token }}" appears directly in run: block of step "Git push and commit"; move to env: map

Locations:

- `action.yml:82`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.email }}" appears directly in run: block of step "Git push and commit"; move to env: map

Locations:

- `action.yml:84`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.name }}" appears directly in run: block of step "Git push and commit"; move to env: map

Locations:

- `action.yml:85`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.files }}" appears directly in run: block of step "Git push and commit"; move to env: map

Locations:

- `action.yml:86`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.commit_message }}" appears directly in run: block of step "Git push and commit"; move to env: map

Locations:

- `action.yml:87`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.remote_repository }}" appears directly in run: block of step "Git push and commit"; move to env: map

Locations:

- `action.yml:90`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.remote_repository }}" appears directly in run: block of step "Git push and commit"; move to env: map

Locations:

- `action.yml:95`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed action.yml: moved all ${{ inputs.* }} and ${{ github.run_id }}/${{ github.job }} expressions from the run: block into an env: map, then referenced them as shell variables ($INPUT_TARGET_BRANCH, $INPUT_FORCE, $INPUT_EMPTY, $INPUT_TAGS, $INPUT_ACCESS_TOKEN, $INPUT_EMAIL, $INPUT_NAME, $INPUT_FILES, $INPUT_COMMIT_MESSAGE, $INPUT_REMOTE_REPOSITORY, $GITHUB_RUN_ID_VAL, $GITHUB_JOB_VAL). Fixed all three workflow files: pinned actions/checkout@v2.3.4 to SHA 5a4ac9002d0be2fb38bd78e4b4dbde5606d7042f and GuillaumeFalourd/git-commit-push@main to SHA 4597be00dc9a434a1763ed9725856de509b1403b, and added top-level `permissions: contents: write` to each workflow.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed three script injection vulnerabilities in action.yml:
1. Line 56: Added double-quotes around `$CURRENT_BRANCH` in the `case` statement subject: `case "$CURRENT_BRANCH" in`
2. Line 59: Added double-quotes around `$TARGET_BRANCH` in the `case` statement subject: `case "$TARGET_BRANCH" in`
3. Line 72: Replaced unquoted `git add $INPUT_FILES -v` with a safe array expansion: `read -ra FILES_ARRAY <<< "$INPUT_FILES"` followed by `git add "${FILES_ARRAY[@]}" -v`. This prevents word-splitting attacks and glob expansion while still correctly handling space-separated file lists as intended by the action's design.

