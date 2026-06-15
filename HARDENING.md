<!-- markdownlint-disable -->

# Hardening Report: GuillaumeFalourd--git-commit-push/v1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **GuillaumeFalourd--git-commit-push/v1.2** was hardened automatically. 26 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Both `run:` steps in action.yml directly interpolate `${{ inputs.* }}` and `${{ github.* }}` expressions inside shell command strings. This means attacker-controlled values are substituted into the shell script before the shell parses them, enabling command injection. Affected expressions include (but are not limited to):

**Step 1 – "Git push and commit origin"** (sub-rule a):
- `TARGET_BRANCH=${{ inputs.target_branch }}` (line 62) — unquoted, directly sets a shell variable from an expression
- `if [ "${{ inputs.force }}" != "0" ]` (line 65)
- `if [ "${{ inputs.empty }}" != "0" ]` (line 68)
- `if [ "${{ inputs.tags }}" != "0" ]` (line 71)
- `echo "  password ${{ inputs.access_token }}"` (lines 75, 78)
- `git config --local user.email "${{ inputs.email }}"` (line 80)
- `git config --local user.name "${{ inputs.name }}"` (line 81)
- `git add ${{ inputs.files }} -v` (line 83) — unquoted
- `git commit -m "${{ inputs.commit_message }}"` (line 84)
- `push-and-commit-action-${{ github.run_id }}-${{ github.job }}` (lines 85, 88, 89)
- `git fetch "${{ inputs.remote_repository }}"` (line 86)

**Step 2 – "Git push and commit remote"** (sub-rule a):
- `if [ -z "${{ inputs.access_token }}" ]` (line 99)
- `if [[ ${{ inputs.remote_repository }} =~ $REGEX ]]` (line 105) — unquoted
- `git config --global user.email "${{ inputs.email }}"` (line 118)
- `git config --global user.name "${{ inputs.name }}"` (line 119)
- `git clone "https://${{ inputs.access_token }}@github.com/..."` (line 121)
- `cp -rvf "${{ inputs.files }}" "$CLONE_DIRECTORY"` (line 126)
- `REMOTE_URL=https://$DESTINATION_OWNER:${{ inputs.access_token }}@...` (line 129) — unquoted
- `git add ${{ inputs.files }}` (line 132) — unquoted
- `git commit --message "${{ inputs.commit_message }}"` (line 134)
- `copy-push-files-action-${{ github.run_id }}-${{ github.job }}` (lines 135, 143, 144)
- `git ls-remote --heads origin ${{ inputs.target_branch }}` (line 138) — unquoted
- `git checkout -b ${{ inputs.target_branch }}` (line 140) — unquoted
- `git checkout ${{ inputs.target_branch }}` (line 142) — unquoted
- `git push -f -u origin "${{ inputs.target_branch }}"` (line 146)

All of these allow a calling workflow to inject arbitrary shell commands via crafted input values (e.g. `inputs.commit_message`, `inputs.files`, `inputs.target_branch`, `inputs.remote_repository`). The fix is to route all inputs through `env:` variables and reference them as properly double-quoted `"$VAR"` shell variables.

Locations:

- `action.yml:62`
- `action.yml:65`
- `action.yml:68`
- `action.yml:71`
- `action.yml:75`
- `action.yml:78`
- `action.yml:80`
- `action.yml:81`
- `action.yml:83`
- `action.yml:84`
- `action.yml:85`
- `action.yml:99`
- `action.yml:105`
- `action.yml:118`
- `action.yml:119`
- `action.yml:121`
- `action.yml:126`
- `action.yml:129`
- `action.yml:132`
- `action.yml:134`
- `action.yml:138`
- `action.yml:140`
- `action.yml:142`
- `action.yml:146`

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

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Moved all ${{ inputs.* }} and ${{ github.* }} expressions out of run: shell strings and into env: blocks for both steps in action.yml. Step 1 ('Git push and commit origin') and Step 2 ('Git push and commit remote') now declare INPUT_TARGET_BRANCH, INPUT_FORCE, INPUT_EMPTY, INPUT_TAGS, INPUT_ACCESS_TOKEN, INPUT_EMAIL, INPUT_NAME, INPUT_FILES, INPUT_COMMIT_MESSAGE, INPUT_REMOTE_REPOSITORY, GITHUB_RUN_ID_VAL, and GITHUB_JOB_VAL in their respective env: blocks. All shell commands reference these as plain $VAR environment variables, eliminating the command injection vectors.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed all script injection vulnerabilities in action.yml:

1. 'Git push and commit origin' step:
   - Quoted `$CURRENT_BRANCH` in `case "$CURRENT_BRANCH" in` to prevent shell metacharacter injection
   - Quoted `$TARGET_BRANCH` in `case "$TARGET_BRANCH" in` to prevent shell metacharacter injection
   - Changed `git add $INPUT_FILES -v` to `git add "$INPUT_FILES" -v` to prevent word-splitting and glob expansion
   - Changed `git commit ... $EMPTY` to use `${EMPTY:+"$EMPTY"}` conditional expansion (optional flag)
   - Changed `git push ... $FORCE $TAGS` to use `${FORCE:+"$FORCE"} ${TAGS:+"$TAGS"}` conditional expansion so empty optional flags don't produce empty arguments

2. 'Git push and commit remote' step:
   - Changed `[[ $DESTINATION_REPOSITORY == *'.git'* ]]` to `[[ "$DESTINATION_REPOSITORY" == *'.git'* ]]` to properly quote the variable
   - Changed `git add $INPUT_FILES` to `git add "$INPUT_FILES"` to prevent word-splitting and glob expansion

