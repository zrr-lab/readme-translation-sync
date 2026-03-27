# README Translation Sync(test)
[English](README.md) | [中文](README.zh-CN.md)

一个自动同步 README 翻译的 GitHub Action，使用 GitHub Models AI。

## 特性

- 🔄 自动检测源 README 的变化
- 🤖 通过 [actions/ai-inference](https://github.com/actions/ai-inference) 使用 GitHub Models AI 进行翻译
- 📝 使用 AI 模型翻译 README 文件
- 🌍 可配置的目标语言和文件
- ⚙️ 支持自定义模型和令牌限制

## 使用方法

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
        # token 是可选的，默认为 ${{ github.token }}

      # 可选：自定义翻译设置
      # - uses: zrr-lab/readme-translation-sync@v1
      #   with:
      #     source-file: README.md
      #     target-file: README.zh-CN.md
      #     target-language: Chinese
      #     model: openai/gpt-4o
      #     max-tokens: 8192

      - uses: peter-evans/create-pull-request@v7
        with:
          commit-message: "📝 同步 README.zh-CN.md 翻译"
          title: "📝 同步 README.zh-CN.md 翻译"
          branch: readme-translation-sync
          delete-branch: true
```

## 输入

| 名称 | 描述 | 默认 |
|------|-------------|---------|
| `source-file` | 待翻译的源 README 文件 | `README.md` |
| `target-file` | 目标翻译后的 README 文件 | `README.zh-CN.md` |
| `target-language` | 翻译的目标语言 | `Chinese` |
| `model` | 要使用的 GitHub Models 模型 | `openai/gpt-4o` |
| `token` | 具有 models:read 权限的 GitHub 令牌 | `${{ github.token }}` |
| `github-token` | GitHub 令牌（已弃用，请使用 `token` 替代） | - |
| `max-tokens` | 生成的最大令牌数 | `8192` |

## 输出

| 名称 | 描述 |
|------|-------------|
| `has-changes` | 翻译是否有变更 |

## 工作原理

这个 action 使用 [actions/ai-inference](https://github.com/actions/ai-inference) 来翻译 README 文件：

1. **检测变更**：检查源 README 文件是否被修改
2. **翻译**：使用 GitHub Models AI 将源文件翻译成目标语言
3. **保存结果**：将翻译的内容写入目标文件
4. **报告变更**：输出翻译文件是否被修改

该 action 自动处理 markdown 格式，保留代码块，并维护原始文件的结构。

## 需求

- **权限**：工作流需要 `models: read` 权限才能使用 GitHub Models AI
- **GitHub Models**：需要访问 GitHub Models（可在 GitHub Enterprise Cloud 或具有适当访问权限的情况下获得）

## 许可

MIT
