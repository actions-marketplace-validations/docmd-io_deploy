# Build & Deploy docmd · GitHub Actions

[![Marketplace](https://img.shields.io/badge/actions-build_&_deploy_with_docmd-blue?style=flat-square&logo=github)](https://github.com/marketplace/actions/build-and-deploy-documentation-with-docmd)
[![Release](https://img.shields.io/github/v/release/docmd-io/deploy?style=flat-square&color=brightgreen)](https://github.com/docmd-io/deploy/releases)

Build and deploy your [docmd](https://github.com/docmd-io/docmd) documentation site to GitHub Pages. Zero config, with automatic project configuration detection.

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
      - uses: docmd-io/deploy@v1.1
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
    uses: docmd-io/deploy/.github/workflows/deploy.yml@v1.1
```

## What it does

- Detects `docmd.config.json` anywhere in the repo
- Falls back to `npx @docmd/core init` if no config is found
- Installs dependencies: runs `npm ci` if a `package.json` is present, otherwise installs `@docmd/core` directly
- Builds the site and outputs the path via `site-dir`

## Inputs

| Input   | Default | Description     |
|---------|---------|-----------------|
| `node`  | `20`    | Node.js version |

## Outputs

| Output     | Description                      |
|------------|----------------------------------|
| `site-dir` | Path to the built site directory |

## First-time setup

GitHub Pages must be set to deploy from **GitHub Actions** (not a branch).

Go to **Settings → Pages → Source → GitHub Actions**.

This only needs to be configured once per repository.

#### [Website](https://docmd.io) • [Documentation](https://docs.docmd.io) • [MIT License](LICENSE)
