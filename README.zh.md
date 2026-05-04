# Project Coding Skills

一组用于 AI 编码团队的项目级 skills，帮助 AI 在不同项目中自动加载各自的项目上下文和编码规则。

[English](./README.md) | 简体中文

这个项目的核心思想很简单：

> Skill 是全局通用的，项目规则是项目本地的。

## 为什么需要这个项目

AI 编码工具很强，但真实团队的项目往往有自己的规则：

- 架构风格
- 包结构
- Controller / Service / Repository 边界
- 测试习惯
- API 约定
- 已有 AI 生成的设计文档和计划文档
- Code Review 要求

这些规则不应该散落在每个开发者自己的长 prompt 里，而应该放在项目仓库中，由团队共享。

## MVP Skills

当前第一版只包含两个核心 skill：

```text
skills/
  project-context-init/
  project-feature-dev/
```

### `project-context-init`

团队口令：`develop:init`

用于初始化或刷新当前项目的 AI 编码上下文。

它会扫描当前项目，以源码为第一事实来源，读取已有 AI 生成文档作为辅助上下文，并在业务项目中生成：

```text
docs/ai-coding/
  project-profile.md
  architecture-summary.md
  coding-rules.md
  ai-context-sources.md
  feature-prompt-context.md
  open-questions.md
```

这些文件属于目标业务项目，不属于本 skill 仓库。

### `project-feature-dev`

团队口令：`develop:feature`

用于功能开发前加载当前项目的上下文。

它会先确定当前项目根目录，只读取当前项目自己的 `docs/ai-coding/`，再阅读相关源码、寻找相似实现，并带着当前项目的规则进入功能开发。

它不替代 brainstorming 或设计流程。它的职责是在设计和编码前加载项目上下文。

## 核心原则

```text
源码是第一事实来源。
AI 生成文档只是上下文，不是权威。
每个项目拥有自己的 docs/ai-coding/。
禁止复用其他项目的上下文。
docs/ai-coding/ 应提交到目标项目的 Git 仓库。
```

## 安装方式

### 使用 Skills CLI 安装

```bash
npx skills add huajiexiewenfeng/project-coding-skills
```

如果在本仓库根目录做本地开发：

```bash
npx skills add .
```

安装后，重启 Codex 或你的 Agent 运行环境，让 skills 被重新发现。

### 手动安装

如果你的工具不支持从 GitHub 安装 skills，可以将这两个 skill 目录复制到本地 skills 目录。

对于 Codex 风格的本地 skills，请保持目录结构：

```text
skills/
  project-context-init/
    SKILL.md
    references/

  project-feature-dev/
    SKILL.md
    references/
```

## 使用方式

### 1. 初始化业务项目

在业务项目仓库中执行：

```text
使用 develop:init 初始化这个项目的 AI 编码上下文。
```

skill 会在当前业务项目中生成：

```text
docs/ai-coding/
```

架构师或负责人重点 review：

```text
docs/ai-coding/open-questions.md
docs/ai-coding/coding-rules.md
docs/ai-coding/feature-prompt-context.md
```

确认后，将 `docs/ai-coding/` 提交到业务项目 Git 仓库，确保团队所有成员和 AI Agent 使用同一份上下文。

### 2. 开发功能

在同一个业务项目中执行：

```text
使用 develop:feature 帮我实现这个需求：...
```

skill 会加载当前项目自己的 `docs/ai-coding/`，并按照当前项目的编码风格和规则推进功能开发。

## 已有 AI 文档

`project-context-init` 可以读取已有 AI 生成上下文，例如：

```text
docs/superpowers/specs/
docs/superpowers/plans/
graphify-out/GRAPH_REPORT.md
graphify-out/graph.json
docs/ai-coding/
*.design.md
*.plan.md
```

注意：这些文档只是辅助上下文。如果它们和源码冲突，以源码为准。

## 仓库结构

```text
skills/
  project-context-init/
    SKILL.md
    references/
      output-templates.md
      source-scan-guide.md
      ai-doc-discovery.md

  project-feature-dev/
    SKILL.md
    references/
      feature-workflow.md
      final-response-template.md
```

## 当前状态

本项目目前处于 MVP 阶段。

当前重点：

- 项目上下文初始化
- 功能开发前加载项目本地上下文
- 源码优先
- 项目隔离
- Git 共享团队上下文

后续可以扩展 bugfix、review、database change、risk check、context refresh 等 skills。
