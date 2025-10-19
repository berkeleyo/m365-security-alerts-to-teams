# Example Adaptive Card (redacted)

This is a safe, redacted example of the two-section card your workflow posts.

```json
{{ read_file('EXAMPLE_CARD.json') }}

> Note: MkDocs Material supports `read_file()` by default when `mkdocs-material` is installed. If it doesn’t render, we’ll just keep the JSON file (below) and you can paste the JSON inline.

## A4) GitHub Actions to build & publish the docs site
> Add this as `.github/workflows/docs.yml`. It builds MkDocs and publishes to a `gh-pages` branch.

```yaml
name: Build docs site

on:
  push:
    branches: [ main ]
    paths:
      - 'docs/**'
      - 'mkdocs.yml'
  workflow_dispatch:

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version: '3.x'
      - name: Install MkDocs + theme
        run: |
          python -m pip install --upgrade pip
          pip install mkdocs mkdocs-material
      - name: Deploy to gh-pages
        run: mkdocs gh-deploy --force
