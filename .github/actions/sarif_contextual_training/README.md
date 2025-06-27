# SecureFlag Knowledge Base for GitHub Actions SARIF

This is a GitHub Action for augmenting SARIF files (e.g. from CodeQL and/or files uploaded to GitHub Code Scanning) with links to relevant SecureFlag training labs.

## Inputs

### `sarif_path`

The directory of SARIF files to be augmented with SecureFlag training links.

## Example workflow with CodeQL

```yaml
...

- name: Perform CodeQL Analysis
  uses: github/codeql-action/analyze@v3
  with:
    category: "/language:${{matrix.language}}"
    upload: "failure-only"

- name: Add SecureFlag training links
  uses: secureflag/actions/.github/actions/sarif_contextual_training@main

- name: Upload SARIF
  uses: github/codeql-action/upload-sarif@v3

...
```
