---
title: "Claude 记忆机制指南"
type: source
created: 2026-05-16
updated: 2026-06-10
tags: [claude, configuration, memory]
source: https://code.claude.com/docs/zh-CN/memory
---

# Claude 记忆机制指南

> **原始位置：** `raw/clippings/Claude 如何记住你的项目.md`
> **来源：** [Claude Code 官方文档](https://code.claude.com/docs/zh-CN/memory)
> **剪藏日期：** 2026-05-16

## 内容摘要

Claude Code 通过两种互补机制跨会话传递知识：

### 1. CLAUDE.md 文件系统

由人类编写的持久化指令，按范围分为四个层级：托管策略（组织级）→ 用户指令 → 项目指令 → 本地指令。每个 CLAUDE.md 文件目标在 200 行以下，使用结构化 markdown。还支持通过 `@` 语法导入其他文件，以及通过 `.claude/rules/` 实现路径范围规则。

### 2. 自动记忆（Auto Memory）

Claude 在交互过程中自动积累的知识笔记。存储在项目特定的 `MEMORY.md` 文件中，前 200 行/25KB 在每次会话加载。主题文件（如 `debugging.md`）按需读取。

### 关键细节

- CLAUDE.md 文件通过从工作目录向上遍历目录树来发现和加载
- 路径范围规则通过 YAML frontmatter 中的 `paths` 字段限定适用范围
- 自动记忆默认开启，可通过 `/memory` 命令管理
- 文件支持符号链接跨项目共享

## 与本知识库的关系

本文是理解 [[wiki/concepts/claude-md-configuration|CLAUDE.md 配置系统]] 的主要参考源。在 [[wiki/concepts/llm-wiki-methodology|LLM Wiki 方法论]] 框架中，CLAUDE.md 被用作"模式层"的具体实现。
