<!-- markdownlint-disable -->

# Hardening Report: GuillaumeFalourd--git-commit-push/v1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **GuillaumeFalourd--git-commit-push/v1.2** was hardened automatically. 28 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Both `run:` blocks in action.yml directly interpolate `${{ inputs.* }}` and `${{ github.* }}` expressions inside shell commands without routing through env vars or quoting, violating sub-rule (a). An attacker who controls these inputs can inject arbitrary shell commands. Examples include:
- `TARGET_BRANCH=${{ inputs.target_branch }}` (unquoted direct interpolation, step 1)
- `if [ "${{ inputs.force }}" != "0" ]` (step 1)
- `echo "  password ${{ inputs.access_token }}"` (step 1)
- `git config --local user.email "${{ inputs.email }}"` (step 1)
- `git add ${{ inputs.files }} -v` (unquoted, step 1)
- `git commit -m "${{ inputs.commit_message }}"` (step 1)
- `git branch push-and-commit-action-${{ github.run_id }}-${{ github.job }}` (step 1)
- `if [ -z "${{ inputs.access_token }}" ]` (step 2)
- `if [[ ${{ inputs.remote_repository }} =~ $REGEX ]]` (unquoted, step 2)
- `git clone "https://${{ inputs.access_token }}@github.com/..."` (step 2)
- `cp -rvf "${{ inputs.files }}" "$CLONE_DIRECTORY"` (step 2)
- `REMOTE_URL=https://$DESTINATION_OWNER:${{ inputs.access_token }}@...` (step 2)
- `git add ${{ inputs.files }}` (unquoted, step 2)
- `git checkout -b ${{ inputs.target_branch }}` (step 2)
- `git push -f -u origin "${{ inputs.target_branch }}"` (step 2)

Locations:

- `action.yml:55`
- `action.yml:97`

### unpinned-uses (severity: high)

All three workflow files use `uses:` references pinned to mutable tags or branches rather than full 40-character commit SHAs. Specifically:
- `actions/checkout@v2.3.4` (version tag, not a SHA) appears in all three workflow files.
- `GuillaumeFalourd/git-commit-push@main` (branch name, not a SHA) appears in all three workflow files.
These can be silently updated by the upstream repository owner to inject malicious code.

Locations:

- `.github/workflows/macos_action_test.yml:14`
- `.github/workflows/macos_action_test.yml:16`
- `.github/workflows/ubuntu_action_test.yml:14`
- `.github/workflows/ubuntu_action_test.yml:16`
- `.github/workflows/windows_action_test.yml:14`
- `.github/workflows/windows_action_test.yml:16`

### missing-permissions (severity: medium)

None of the three workflow files define a top-level `permissions:` key, and no individual job within any of these files defines a `permissions:` key either. Without explicit permissions, workflows run with the default (often broad) token permissions, violating the principle of least privilege.

Locations:

- `.github/workflows/macos_action_test.yml:1`
- `.github/workflows/ubuntu_action_test.yml:1`
- `.github/workflows/windows_action_test.yml:1`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.target_branch }}" appears directly in run: block of step "Git push and commit origin"; move to env: map

Locations:

- `action.yml:62`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.force }}" appears directly in run: block of step "Git push and commit origin"; move to env: map

Locations:

- `action.yml:67`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.empty }}" appears directly in run: block of step "Git push and commit origin"; move to env: map

Locations:

- `action.yml:70`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.tags }}" appears directly in run: block of step "Git push and commit origin"; move to env: map

Locations:

- `action.yml:73`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.access_token }}" appears directly in run: block of step "Git push and commit origin"; move to env: map

Locations:

- `action.yml:79`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.access_token }}" appears directly in run: block of step "Git push and commit origin"; move to env: map

Locations:

- `action.yml:83`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.email }}" appears directly in run: block of step "Git push and commit origin"; move to env: map

Locations:

- `action.yml:85`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.name }}" appears directly in run: block of step "Git push and commit origin"; move to env: map

Locations:

- `action.yml:86`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.files }}" appears directly in run: block of step "Git push and commit origin"; move to env: map

Locations:

- `action.yml:89`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.commit_message }}" appears directly in run: block of step "Git push and commit origin"; move to env: map

Locations:

- `action.yml:90`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.remote_repository }}" appears directly in run: block of step "Git push and commit origin"; move to env: map

Locations:

- `action.yml:92`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.remote_repository }}" appears directly in run: block of step "Git push and commit origin"; move to env: map

Locations:

- `action.yml:97`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.access_token }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:106`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.remote_repository }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:113`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.email }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:127`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.name }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:128`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.access_token }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:130`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.files }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:135`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.access_token }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:139`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.files }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:143`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.commit_message }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:146`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.target_branch }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:151`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.target_branch }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:154`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.target_branch }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:156`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.target_branch }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:163`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all findings in action.yml and the three workflow files:

1. **script-injection / static-inline-injection** (action.yml): All ${{ inputs.* }} and ${{ github.* }} expressions in both run: blocks (steps 'Git push and commit origin' and 'Git push and commit remote') were moved to env: blocks. Variables are now referenced as plain shell env vars (INPUT_TARGET_BRANCH, INPUT_FORCE, INPUT_EMPTY, INPUT_TAGS, INPUT_ACCESS_TOKEN, INPUT_EMAIL, INPUT_NAME, INPUT_FILES, INPUT_COMMIT_MESSAGE, INPUT_REMOTE_REPOSITORY, GITHUB_RUN_ID_VAL, GITHUB_JOB_VAL).

2. **unpinned-uses** (macos_action_test.yml, ubuntu_action_test.yml, windows_action_test.yml): Pinned actions/checkout@v2.3.4 to SHA 5a4ac9002d0be2fb38bd78e4b4dbde5606d7042f and GuillaumeFalourd/git-commit-push@main to SHA 954983f1f22b9695ffee7d2456c00d100763ada6.

3. **missing-permissions** (all three workflow files): Added top-level 'permissions: contents: write' to each workflow file, as the workflows need to push commits to the repository.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed four unquoted variable expansions in action.yml that could allow shell word splitting and glob expansion on attacker-controlled inputs:
1. `case $CURRENT_BRANCH in` → `case "$CURRENT_BRANCH" in` (step 1, prevents glob expansion)
2. `case $TARGET_BRANCH in` → `case "$TARGET_BRANCH" in` (step 1, line 63, prevents glob expansion on workflow-controllable `inputs.target_branch`)
3. `git add $INPUT_FILES -v` → `git add "$INPUT_FILES" -v` (step 1, line 82, prevents word splitting/glob on `inputs.files`)
4. `git add $INPUT_FILES` → `git add "$INPUT_FILES"` (step 2, line 143, prevents word splitting/glob on `inputs.files`)

All `$INPUT_FILES` and `$TARGET_BRANCH` expansions are now properly double-quoted throughout both run blocks.

