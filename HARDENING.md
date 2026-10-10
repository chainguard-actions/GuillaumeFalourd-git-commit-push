<!-- markdownlint-disable -->

# Hardening Report: GuillaumeFalourd--git-commit-push/v1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **GuillaumeFalourd--git-commit-push/v1** was hardened automatically. 13 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

The `run:` block in action.yml (the single composite step 'Git push and commit') directly interpolates numerous `${{ inputs.* }}` and `${{ github.* }}` expressions inside shell commands, violating sub-rule (a). Before the shell ever sees the script, GitHub Actions performs YAML template substitution of these expressions, allowing an attacker-controlled value to inject arbitrary shell commands.

Affected lines (all sub-rule a unless noted):
- Line 57: `TARGET_BRANCH=${{ inputs.target_branch }}` — unquoted direct interpolation (also sub-rule b)
- Line 61: `if [ "${{ inputs.force }}" != "0" ]`
- Line 65: `if [ "${{ inputs.empty }}" != "0" ]`
- Line 69: `if [ "${{ inputs.tags }}" != "0" ]`
- Line 74: `echo "  password ${{ inputs.access_token }}"`
- Line 77: `echo "  password ${{ inputs.access_token }}"`
- Line 80: `git config --local user.email "${{ inputs.email }}"`
- Line 81: `git config --local user.name "${{ inputs.name }}"`
- Line 82: `git add ${{ inputs.files }} -v` — unquoted (also sub-rule b)
- Line 83: `git commit -m "${{ inputs.commit_message }}"`
- Line 85: `git branch push-and-commit-action-${{ github.run_id }}-${{ github.job }}`
- Line 86: `git fetch "${{ inputs.remote_repository }}"`
- Line 88: `git merge push-and-commit-action-${{ github.run_id }}-${{ github.job }}`
- Line 89: `git branch -d push-and-commit-action-${{ github.run_id }}-${{ github.job }}`
- Line 91: `git push "${{ inputs.remote_repository }}" ...`

Fix: Move all expression values into `env:` variables and reference them as quoted shell variables (e.g., `"$INPUT_FILES"`) inside the `run:` block. Never interpolate `${{ }}` expressions directly in shell scripts.

Locations:

- `action.yml:57`
- `action.yml:61`
- `action.yml:65`
- `action.yml:69`
- `action.yml:74`
- `action.yml:77`
- `action.yml:80`
- `action.yml:81`
- `action.yml:82`
- `action.yml:83`
- `action.yml:85`
- `action.yml:86`
- `action.yml:88`
- `action.yml:89`
- `action.yml:91`

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

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Moved all ${{ inputs.* }} and ${{ github.* }} expressions from the run: block into the step's env: block. Added env vars: INPUT_TARGET_BRANCH, INPUT_FORCE, INPUT_EMPTY, INPUT_TAGS, INPUT_ACCESS_TOKEN, INPUT_EMAIL, INPUT_NAME, INPUT_FILES, INPUT_COMMIT_MESSAGE, INPUT_REMOTE_REPOSITORY. Replaced ${{ github.run_id }} and ${{ github.job }} with the built-in GITHUB_RUN_ID and GITHUB_JOB env vars. Applied xargs-based tokenization for INPUT_FILES (a space-separated list input) using the NUL-delimited read loop pattern with a guard for empty values.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed two unquoted case statement variables in hardened/action/action.yml:
1. Line 57: Changed `case $CURRENT_BRANCH in` to `case "$CURRENT_BRANCH" in`
2. Line 60: Changed `case $TARGET_BRANCH in` to `case "$TARGET_BRANCH" in`

Both variables are derived from workflow-controlled data (GITHUB_REF and inputs.target_branch respectively), so quoting them prevents shell metacharacter injection.

