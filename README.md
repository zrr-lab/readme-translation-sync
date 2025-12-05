# README Translation Sync
English | [中文](README.zh-CN.md)

A GitHub Action that automatically syncs README translations using GitHub Models AI.

## Features

- 🔄 Automatically detects changes in source README
- 🤖 Uses GitHub Models AI via [actions/ai-inference](https://github.com/actions/ai-inference) for translation
- 📝 Translates README files using AI models
- 🌍 Configurable target language and files
- ⚙️ Supports custom models and token limits

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
        # token is optional, defaults to ${{ github.token }}

      # Optional: Customize translation settings
      # - uses: zrr-lab/readme-translation-sync@v1
      #   with:
      #     source-file: README.md
      #     target-file: README.zh-CN.md
      #     target-language: Chinese
      #     model: openai/gpt-4o
      #     max-tokens: 8192

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
| `model` | GitHub Models model to use | `openai/gpt-4o` |
| `token` | GitHub token with models:read permission | `${{ github.token }}` |
| `github-token` | GitHub token (deprecated, use `token` instead) | - |
| `max-tokens` | Maximum number of tokens to generate | `8192` |

## Outputs

| Name | Description |
|------|-------------|
| `has-changes` | Whether the translation has changes |

## How It Works

This action uses [actions/ai-inference](https://github.com/actions/ai-inference) to translate README files:

1. **Detects changes**: Checks if the source README file has been modified
2. **Translates**: Uses GitHub Models AI to translate the source file to the target language
3. **Saves result**: Writes the translated content to the target file
4. **Reports changes**: Outputs whether the translation file was modified

The action automatically handles markdown formatting, preserves code blocks, and maintains the structure of the original file.

## Requirements

- **Permissions**: The workflow needs `models: read` permission to use GitHub Models AI
- **GitHub Models**: Requires access to GitHub Models (available in GitHub Enterprise Cloud or with appropriate access)

## License

MIT
