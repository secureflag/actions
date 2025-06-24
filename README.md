# SecureFlag Training GitHub Action

This GitHub Action helps enforce secure development practices by checking whether contributors have completed relevant security training.

When a Pull Request is submitted or updated, the action looks for references to existing repos' Security Advisories in commit messages or the pull request description, and verifies if the author has completed the required SecureFlag training related to those advisories.

## How to Use

Add the following to your workflow file (e.g., `.github/workflows/security-check.yml`):

```yaml
name: Vulnerability Training Check

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
    types: [opened, synchronize, reopened, ready_for_review]

jobs:
  security-approval:
    runs-on: ubuntu-latest
    steps:
      - uses: secureflag/actions/.github/actions/advisory_training_check@main
        with:
          target_repo: ${{ github.repository }}
          sec_token: ${{ secrets.SEC_TOKEN }}
          api_endpoint: ${{ secrets.API_ENDPOINT }}
          api_token: ${{ secrets.API_TOKEN }}
```

## Required Variables

To run the action, set the following inputs:

- `target_repo`: (Required) The full name of the repository (e.g., org/repo), usually `${{ github.repository }}`.
- `sec_token`: (Required) GitHub token with permission to read repository security advisories.
- `api_endpoint`: (Required) SecureFlag API endpoint URL.
- `api_token`: (Required) API token to authenticate with SecureFlag.

Store all sensitive values as GitHub Secrets in your repository or organization.
