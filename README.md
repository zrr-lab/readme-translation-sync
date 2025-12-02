# README Translation Sync

A GitHub Action that automatically syncs README translations using GitHub Models AI.

## Features

- 🔄 Automatically detects changes in source README
- 🤖 Uses GitHub Models (GPT-4o-mini) for translation
- 📝 Supports incremental updates based on diffs
- 🌍 Configurable target language and files

## Usage

```yaml
name: Sync README Translation

on:
  push:
    branches: [main]
    paths:
      - README.md

permissions:
  contents: write
  pull-requests: write
  models: read

jobs:
  translate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - uses: zrr-lab/readme-translation-sync@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}

      - uses: peter-evans/create-pull-request@v7
        with:
          commit-message: "📝 Sync README.zh-CN.md translation"
          title: "📝 Sync README.zh-CN.md translation"
          branch: readme-translation-sync
          delete-branch: true
```

## Inputs

| Name | Description | Default |
|------|-------------|---------|
| `source-file` | Source README file to translate | `README.md` |
| `target-file` | Target translated README file | `README.zh-CN.md` |
| `target-language` | Target language for translation | `Chinese` |
| `model` | GitHub Models model to use | `openai/gpt-4o-mini` |
| `github-token` | GitHub token with models:read permission | **Required** |

## Outputs

| Name | Description |
|------|-------------|
| `has-changes` | Whether the translation has changes |

## License

MIT
