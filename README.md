# docmd deploy · GitHub Action

[![Marketplace](https://img.shields.io/badge/GitHub_Marketplace-docmd_deploy-blue?style=for-the-badge&logo=github)](https://github.com/marketplace/actions/docmd-deploy)

Build and deploy [docmd](https://docmd.io) documentation to GitHub Pages — zero config, one step.

## Usage

Add to any workflow:

```yaml
jobs:
  docs:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deploy.outputs.page_url }}
    steps:
      - uses: actions/checkout@v4

      - uses: docmd-io/deploy@v1
        id: build

      - uses: actions/upload-pages-artifact@v3
        with:
          path: ${{ steps.build.outputs.site-dir }}

      - uses: actions/deploy-pages@v4
        id: deploy
```

Or use the reusable workflow for even less boilerplate:

```yaml
on:
  push:
    branches: [main]

jobs:
  docs:
    uses: docmd-io/deploy/.github/workflows/deploy.yml@v1
```

## What it does

- Detects `docmd.config.json` anywhere in the repo (including subdirectories)
- Runs `npx @docmd/core init` if no config is found
- Installs dependencies — `npm ci` if a `package.json` exists, otherwise installs `@docmd/core` directly
- Builds the site and outputs the path via `site-dir`

## Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `node` | `20` | Node.js version |

## Outputs

| Output | Description |
|--------|-------------|
| `site-dir` | Path to the built site directory |

## First-time setup

GitHub Pages must be set to deploy from **GitHub Actions** (not a branch).
Go to **Settings → Pages → Source → GitHub Actions**.

This is a one-time step per repo.

[docmd.io](https://docmd.io) · [Documentation](https://docs.docmd.io) · [MIT License](LICENSE)