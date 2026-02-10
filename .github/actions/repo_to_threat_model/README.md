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
          SECUREFLAG_MODEL_UUID: ${{ vars.SECUREFLAG_MODEL_UUID }}
          # AI Provider - choose ONE of the following options:
          # Option 1 - Anthropic:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
          # Option 2 - OpenAI:
          # OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
          # Option 3 - Azure OpenAI:
          # AZURE_OPENAI_KEY: ${{ secrets.AZURE_OPENAI_KEY }}
          # AZURE_OPENAI_ENDPOINT: ${{ secrets.AZURE_OPENAI_ENDPOINT }}
          # AZURE_OPENAI_DEPLOYMENT: ${{ secrets.AZURE_OPENAI_DEPLOYMENT }}
```

#### Required Variables

- `SECUREFLAG_API_KEY`: SecureFlag API key for authentication.
- `SECUREFLAG_MODEL_UUID`: SecureFlag model UUID identifying the target threat model.

#### AI Provider Variables (choose one)

**Option 1 - Anthropic:**
- `ANTHROPIC_API_KEY`: Anthropic API key for AI-powered analysis.
- `ANTHROPIC_MODEL`: (Optional) Model name. Default: `claude-sonnet-4-20250514`

**Option 2 - OpenAI:**
- `OPENAI_API_KEY`: OpenAI API key for AI-powered analysis.
- `OPENAI_MODEL`: (Optional) Model name. Default: `gpt-4o`

**Option 3 - Azure OpenAI:**
- `AZURE_OPENAI_KEY`: Azure OpenAI API key.
- `AZURE_OPENAI_ENDPOINT`: Azure endpoint URL (e.g., `https://your-resource.openai.azure.com/`)
- `AZURE_OPENAI_DEPLOYMENT`: Azure deployment name.
- `AZURE_OPENAI_API_VERSION`: (Optional) API version. Default: `2024-02-15-preview`

#### Optional Variables

- `SECUREFLAG_COMMANDS`: Commands to run. Defaults to `model-repo`.
- `SECUREFLAG_REPO_PATH`: Restrict analysis to a specific directory within the repository.
- `SECUREFLAG_COMPONENT_LIMIT`: Hinted number of nodes in ThreatCanvas diagrams.

Store all sensitive values as GitHub Secrets in your repository or organization.
