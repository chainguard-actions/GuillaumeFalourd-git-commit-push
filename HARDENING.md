<!-- markdownlint-disable -->

# Hardening Report: GuillaumeFalourd--git-commit-push/v1.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **GuillaumeFalourd--git-commit-push/v1.3** was hardened automatically. 26 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Step 'Git push and commit origin': Multiple ${{ inputs.* }} and ${{ github.* }} expressions are interpolated directly into the run: shell script (sub-rule a). This allows an attacker-controlled value to break out of the shell context and execute arbitrary commands. Offending lines include:
- `TARGET_BRANCH=${{ inputs.target_branch }}` (unquoted, direct interpolation)
- `if [ "${{ inputs.force }}" != "0" ]`
- `if [ "${{ inputs.empty }}" != "0" ]`
- `if [ "${{ inputs.tags }}" != "0" ]`
- `echo "  password ${{ inputs.access_token }}"`  (×2)
- `git config --local user.email "${{ inputs.email }}"`
- `git config --local user.name "${{ inputs.name }}"`
- `git add ${{ inputs.files }} -v` (unquoted)
- `git commit -m "${{ inputs.commit_message }}"`
- `git branch git-commit-push-action-${{ github.run_id }}-${{ github.job }}`
- `git fetch "${{ inputs.remote_repository }}"`
- `git merge git-commit-push-action-${{ github.run_id }}-${{ github.job }}`
- `git push "${{ inputs.remote_repository }}" ...`
All these values should be passed via env: variables and then referenced as quoted shell variables (e.g., "$VAR").

Locations:

- `action.yml:57`
- `action.yml:60`
- `action.yml:63`
- `action.yml:66`
- `action.yml:70`
- `action.yml:73`
- `action.yml:75`
- `action.yml:76`
- `action.yml:79`
- `action.yml:80`
- `action.yml:81`
- `action.yml:82`
- `action.yml:84`
- `action.yml:85`
- `action.yml:87`

### script-injection (severity: high)

Step 'Git push and commit remote': Multiple ${{ inputs.* }} and ${{ github.* }} expressions are interpolated directly into the run: shell script (sub-rule a). This allows an attacker-controlled value to break out of the shell context and execute arbitrary commands. Offending lines include:
- `if [ -z "${{ inputs.access_token }}" ]`
- `if [[ ${{ inputs.remote_repository }} =~ $REGEX ]]` (unquoted, direct interpolation)
- `git config --global user.email "${{ inputs.email }}"`
- `git config --global user.name "${{ inputs.name }}"`
- `git clone "https://${{ inputs.access_token }}@github.com/..."`
- `cp -rvf ${{ inputs.files }} $CLONE_DIRECTORY` (unquoted)
- `REMOTE_URL=https://$DESTINATION_OWNER:${{ inputs.access_token }}@...`
- `git commit --message "${{ inputs.commit_message }}"`
- `git branch git-commit-push-action-${{ github.run_id }}-${{ github.job }}`
- `BE=$(git ls-remote --heads origin ${{ inputs.target_branch }} | wc -l)` (unquoted)
- `git checkout -b ${{ inputs.target_branch }}` (unquoted)
- `git checkout ${{ inputs.target_branch }}` (unquoted)
- `git merge -X theirs git-commit-push-action-${{ github.run_id }}-${{ github.job }}`
- `git push -f -u origin "${{ inputs.target_branch }}"`
All these values should be passed via env: variables and then referenced as quoted shell variables.

Locations:

- `action.yml:98`
- `action.yml:103`
- `action.yml:112`
- `action.yml:113`
- `action.yml:115`
- `action.yml:121`
- `action.yml:126`
- `action.yml:131`
- `action.yml:132`
- `action.yml:135`
- `action.yml:138`
- `action.yml:140`
- `action.yml:142`
- `action.yml:147`

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

- `action.yml:136`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.access_token }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:142`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.commit_message }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:149`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.target_branch }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:154`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.target_branch }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:157`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.target_branch }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:159`

### static-inline-injection (severity: high)

shell injection: expression "${{ inputs.target_branch }}" appears directly in run: block of step "Git push and commit remote"; move to env: map

Locations:

- `action.yml:166`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, static-inline-injection

**Notes:**

Fixed all script injection findings in action.yml by adding env: blocks to both composite steps ('Git push and commit origin' and 'Git push and commit remote'). All ${{ inputs.* }} and ${{ github.* }} expressions were moved from run: shell scripts into env: maps, and the shell scripts were updated to reference them as plain environment variables ($INPUT_TARGET_BRANCH, $INPUT_FORCE, $INPUT_EMPTY, $INPUT_TAGS, $INPUT_ACCESS_TOKEN, $INPUT_EMAIL, $INPUT_NAME, $INPUT_FILES, $INPUT_COMMIT_MESSAGE, $INPUT_REMOTE_REPOSITORY, $GH_RUN_ID, $GH_JOB). No ${{ }} expressions remain in any run: block.

### Iteration 2

**Fixes applied:** script-injection

**Notes:**

Fixed all script injection vulnerabilities in action.yml:

1. 'Git push and commit origin' step:
   - Added quotes around $CURRENT_BRANCH and $TARGET_BRANCH in case statements
   - Replaced unquoted `git add $INPUT_FILES -v` with safe array expansion: `read -ra FILES_ARRAY <<< "$INPUT_FILES"` then `git add "${FILES_ARRAY[@]}" -v`

2. 'Git push and commit remote' step:
   - Added quotes around $INPUT_REMOTE_REPOSITORY in regex test: `[[ "$INPUT_REMOTE_REPOSITORY" =~ $REGEX ]]`
   - Replaced unquoted `cp -rvf $INPUT_FILES $CLONE_DIRECTORY` with safe array expansion: `read -ra FILES_ARRAY <<< "$INPUT_FILES"` then `cp -rvf "${FILES_ARRAY[@]}" "$CLONE_DIRECTORY"`
   - Properly quoted REMOTE_URL assignment and usage: `REMOTE_URL="https://..."` and `git remote set-url origin "$REMOTE_URL"`

All user-controlled inputs ($INPUT_FILES, $INPUT_REMOTE_REPOSITORY, $TARGET_BRANCH, $REMOTE_URL) are now properly quoted or handled through array expansion to prevent shell metacharacter injection.

