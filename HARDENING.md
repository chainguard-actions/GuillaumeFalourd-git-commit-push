<!-- markdownlint-disable -->

# Hardening Report: GuillaumeFalourd--git-commit-push/v1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **GuillaumeFalourd--git-commit-push/v1.2** was hardened automatically. 27 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Step 'Git push and commit origin': Multiple ${{ inputs.* }} and ${{ github.* }} expressions are interpolated directly inside the run: shell script, violating rule (a). Attacker-controlled inputs flow directly into shell commands without any env: indirection. Offending lines include:
- `TARGET_BRANCH=${{ inputs.target_branch }}` (unquoted — shell parses the value directly)
- `if [ "${{ inputs.force }}" != "0" ]`
- `if [ "${{ inputs.empty }}" != "0" ]`
- `if [ "${{ inputs.tags }}" != "0" ]`
- `echo "  password ${{ inputs.access_token }}"` (×2, written to .netrc)
- `git config --local user.email "${{ inputs.email }}"`
- `git config --local user.name "${{ inputs.name }}"`
- `git add ${{ inputs.files }} -v` (unquoted)
- `git commit -m "${{ inputs.commit_message }}"`
- `git branch push-and-commit-action-${{ github.run_id }}-${{ github.job }}`
- `git fetch "${{ inputs.remote_repository }}"`
- `git merge push-and-commit-action-${{ github.run_id }}-${{ github.job }}`
- `git branch -d push-and-commit-action-${{ github.run_id }}-${{ github.job }}`
- `git push "${{ inputs.remote_repository }}"`
Any of these inputs can contain shell metacharacters (`;`, `|`, `$(...)`, etc.) injected by a caller.

Locations:

- `action.yml:64`
- `action.yml:69`
- `action.yml:72`
- `action.yml:75`
- `action.yml:81`
- `action.yml:85`
- `action.yml:87`
- `action.yml:88`
- `action.yml:91`
- `action.yml:92`
- `action.yml:93`
- `action.yml:94`
- `action.yml:96`
- `action.yml:97`
- `action.yml:99`

### script-injection (severity: high)

Step 'Git push and commit remote': Multiple ${{ inputs.* }} and ${{ github.* }} expressions are interpolated directly inside the run: shell script, violating rule (a). Offending lines include:
- `if [ -z "${{ inputs.access_token }}" ]`
- `if [[ ${{ inputs.remote_repository }} =~ $REGEX ]]` (unquoted — regex match with attacker-controlled value)
- `git config --global user.email "${{ inputs.email }}"`
- `git config --global user.name "${{ inputs.name }}"`
- `git clone "https://${{ inputs.access_token }}@github.com/..."`
- `cp -rvf "${{ inputs.files }}" "$CLONE_DIRECTORY"`
- `REMOTE_URL=https://$DESTINATION_OWNER:${{ inputs.access_token }}@...` (unquoted)
- `git add ${{ inputs.files }}` (unquoted)
- `git commit --message "${{ inputs.commit_message }}"`
- `git branch copy-push-files-action-${{ github.run_id }}-${{ github.job }}`
- `BE=$(git ls-remote --heads origin ${{ inputs.target_branch }} | wc -l)` (unquoted)
- `git checkout -b ${{ inputs.target_branch }}` (unquoted)
- `git checkout ${{ inputs.target_branch }}` (unquoted)
- `git merge -X theirs copy-push-files-action-${{ github.run_id }}-${{ github.job }}`
- `git branch -d copy-push-files-action-${{ github.run_id }}-${{ github.job }}`
- `git push -f -u origin "${{ inputs.target_branch }}"`
An attacker controlling any of these inputs can inject arbitrary shell commands.

Locations:

- `action.yml:106`
- `action.yml:113`
- `action.yml:120`
- `action.yml:121`
- `action.yml:125`
- `action.yml:130`
- `action.yml:133`
- `action.yml:136`
- `action.yml:139`
- `action.yml:141`
- `action.yml:144`
- `action.yml:147`
- `action.yml:149`
- `action.yml:151`
- `action.yml:152`
- `action.yml:155`
- `action.yml:161`

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

Fixed all script injection findings in action.yml by:
1. Added env: blocks to both 'Git push and commit origin' and 'Git push and commit remote' steps, mapping all ${{ inputs.* }} and ${{ github.* }} expressions to environment variables (INPUT_TARGET_BRANCH, INPUT_FORCE, INPUT_EMPTY, INPUT_TAGS, INPUT_ACCESS_TOKEN, INPUT_EMAIL, INPUT_NAME, INPUT_FILES, INPUT_COMMIT_MESSAGE, INPUT_REMOTE_REPOSITORY, GITHUB_RUN_ID_VAL, GITHUB_JOB_VAL).
2. Replaced all inline ${{ }} expressions in run: blocks with their corresponding $ENV_VAR references, properly double-quoted.
3. The 'files' input (space-separated list) is tokenized using the xargs/while-read-NUL pattern to preserve argument boundaries and handle quoted paths correctly.
4. Branch names using run_id and job are now constructed from shell variables: "push-and-commit-action-${GITHUB_RUN_ID_VAL}-${GITHUB_JOB_VAL}".
5. The regex match for remote_repository now uses $INPUT_REMOTE_REPOSITORY instead of the inline expression.
6. The REMOTE_URL construction in the remote step now uses properly quoted shell variables.

