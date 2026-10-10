<!-- markdownlint-disable -->

# Hardening Report: GuillaumeFalourd--git-commit-push/v1.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **GuillaumeFalourd--git-commit-push/v1.3** was hardened automatically. 26 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Step 'Git push and commit origin' directly interpolates ${{ inputs.* }} and ${{ github.* }} expressions inside the run: shell script (rule a). Attacker-controlled inputs are substituted into shell commands before the shell parses them, enabling command injection. Offending lines include: `TARGET_BRANCH=${{ inputs.target_branch }}` (unquoted), `if [ "${{ inputs.force }}" != "0" ]`, `echo "  password ${{ inputs.access_token }}"`, `git config --local user.email "${{ inputs.email }}"`, `git config --local user.name "${{ inputs.name }}"`, `git add ${{ inputs.files }} -v` (unquoted), `git commit -m "${{ inputs.commit_message }}"`, `git branch git-commit-push-action-${{ github.run_id }}-${{ github.job }}`, `git fetch "${{ inputs.remote_repository }}"`, and `git push "${{ inputs.remote_repository }}" ...`. All of these must be moved to env: variables and then referenced as double-quoted shell variables.

Locations:

- `action.yml:44`

### script-injection (severity: high)

Step 'Git push and commit remote' directly interpolates ${{ inputs.* }} and ${{ github.* }} expressions inside the run: shell script (rule a). Offending lines include: `if [ -z "${{ inputs.access_token }}" ]`, `if [[ ${{ inputs.remote_repository }} =~ $REGEX ]]` (unquoted), `git config --global user.email "${{ inputs.email }}"`, `git config --global user.name "${{ inputs.name }}"`, `git clone "https://${{ inputs.access_token }}@github.com/..."`, `cp -rvf ${{ inputs.files }} $CLONE_DIRECTORY` (unquoted), `REMOTE_URL=https://...:${{ inputs.access_token }}@github.com/...`, `git commit --message "${{ inputs.commit_message }}"`, `git branch git-commit-push-action-${{ github.run_id }}-${{ github.job }}`, `BE=$(git ls-remote --heads origin ${{ inputs.target_branch }} | wc -l)`, `git checkout -b ${{ inputs.target_branch }}`, `git checkout ${{ inputs.target_branch }}`, and `git push -f -u origin "${{ inputs.target_branch }}"`.

Locations:

- `action.yml:89`

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

Fixed all 26 script injection findings in action.yml by moving every ${{ inputs.* }} and ${{ github.* }} expression from run: shell scripts into env: blocks for both steps ('Git push and commit origin' and 'Git push and commit remote'). Shell scripts now reference plain env vars ($INPUT_TARGET_BRANCH, $INPUT_FORCE, $INPUT_EMPTY, $INPUT_TAGS, $INPUT_ACCESS_TOKEN, $INPUT_EMAIL, $INPUT_NAME, $INPUT_FILES, $INPUT_COMMIT_MESSAGE, $INPUT_REMOTE_REPOSITORY, $GITHUB_RUN_ID_VAL, $GITHUB_JOB_VAL). The 'files' input (a space-separated list) is tokenized using the xargs/read-loop idiom (with guard for empty value) to preserve argument boundaries and honor quoted values. All other inputs use double-quoted variable references. The ${{ }} expressions remaining in the file are only in safe locations: env: block values, if: conditions, outputs: values, and input defaults.

