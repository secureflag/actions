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

- `target_repo`: (Required) The full name of the repository (e.g., org/repo), auto-replaced by `${{ github.repository }}`.
- `sec_token`: (Required) GitHub token with permission to:
  - Read repository security advisories.
  - Read the Pull Request's description
  - Read the commits messages
  - Create a comment in a Pull Request
- `api_endpoint`: (Required) SecureFlag API endpoint URL.
- `api_token`: (Required) API token to authenticate with SecureFlag.

Store all sensitive values as GitHub Secrets in your repository or organization.
