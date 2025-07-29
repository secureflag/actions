# SecureFlag Training GitHub Action

This GitHub Action helps enforce secure development practices by checking whether contributors have completed relevant security training.

When a Pull Request (PR) is submitted or updated, the action:
- Looks for references to existing repos' Security Advisories (GHSA) in commit messages, pull request title or description and fetches the GHSA title(s).
- Gets the committer's email from the PR.
- Queries Secureflag's database with the above email and the GHSA title(s) to verify if the author has completed the required SecureFlag training related to those advisories.
- If the author hasn't passed training for any of advisories, the action creates a PR comment with information and the URL(s) for the required training and the PR is blocked.

When the training is completed, the action can be re-run to unblock the PR.

## How to Use

### Add the following snippet to your workflow file
(e.g., `.github/workflows/security-check.yml`)

```yaml
name: Vulnerability Training Check

on:
  push:
    branches:
      - '*'
      - '!main'

jobs:
  security-approval:
    runs-on: ubuntu-latest
    steps:
      - uses: secureflag/actions/.github/actions/advisory_training_check@main
        with:
          target_repo: ${{ github.repository }}
          gh_token: ${{ secrets.SEC_TOKEN }}
          api_endpoint: ${{ secrets.API_ENDPOINT }}
          api_token: ${{ secrets.API_TOKEN }}
```

#### Required Variables
To run the action, set the following inputs:

- `target_repo`: The full name of the repository (e.g., org/repo). `${{ github.repository }}` sets the current org/repo
- `gh_token`: (Required) GitHub token with permission to:
  - Read repository security advisories.
  - Read the Pull Request's description
  - Read the commits messages
  - Create a comment in a Pull Request
- `api_endpoint`: SecureFlag API endpoint URL.
- `api_token`: (Required) API token to authenticate with SecureFlag.

### Set Branch Protection rule to enforce blocking of the Pull Request
Repo Settings
-> Branches
-> Add Branch protection rule
  -> Set branch name pattern (eg. "main")
  -> Select "Require status checks to pass before merging".
  -> Select "Require branches to be up to date before merging"
  -> Below that, set "security-approval" as Status Check 
-> Save

Store all sensitive values as GitHub Secrets in your repository or organization.
