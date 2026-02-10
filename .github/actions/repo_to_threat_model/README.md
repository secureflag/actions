# SecureFlag Repo To Threat Model GitHub Action

This GitHub Action analyzes your codebase and generates threat models using SecureFlag's ThreatCanvas.

When the action runs, it:
- Scans your repository's codebase to understand its architecture and components.
- Uses AI to identify potential security threats and attack vectors.
- Generates a threat model in SecureFlag's ThreatCanvas format.

## How to Use
### Add the following snippet to your workflow file
(e.g., `.github/workflows/threatcanvas.yml`)

```yaml
name: Generate Threat Model with ThreatCanvas

on:
  push:
    tags:
      - '*'

jobs:
  threat-model:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: secureflag/actions/.github/actions/repo_to_threat_model@main
        with:
          SECUREFLAG_API_KEY: ${{ secrets.SECUREFLAG_API_KEY }}
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
          SECUREFLAG_MODEL_UUID: ${{ vars.SECUREFLAG_MODEL_UUID }}
```

#### Required Variables
The job uses the following inputs:

- `SECUREFLAG_API_KEY`: (Required) SecureFlag API key for authentication.
- `ANTHROPIC_API_KEY`: (Required) Anthropic API key for AI-powered analysis.
- `SECUREFLAG_MODEL_UUID`: (Required) SecureFlag model UUID identifying the target threat model.

#### Optional Variables

- `SECUREFLAG_COMMANDS`: Commands to run. Defaults to `model-repo`.
- `SECUREFLAG_REPO_PATH`: Restrict analysis to a specific directory within the repository.
- `SECUREFLAG_COMPONENT_LIMIT`: Hinted number of nodes in ThreatCanvas diagrams.

Store all sensitive values as GitHub Secrets in your repository or organization.
