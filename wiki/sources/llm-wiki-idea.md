---
title: "LLM Wiki 思想文档"
type: source
created: 2026-06-10
updated: 2026-06-10
tags: [methodology, knowledge-management, llm]
source: LLM Wiki.md
---

# LLM Wiki 思想文档

> **位置：** `LLM Wiki.md`（库根目录）
> **类型：** 方法论蓝图 / 思想文档
> **目的：** 描述基于 LLM 构建持久化个人知识库的通用模式

## 核心论点

传统 RAG 系统在每次查询时从原始文档中重新发现知识，没有知识积累。本文提出了一种不同的方法：让 LLM 增量构建和维护一个持久化的维基——一个结构化的、相互链接的 markdown 文件集合——位于用户和原始源之间。

## 关键概念

参见 [[wiki/concepts/llm-wiki-methodology|LLM Wiki 方法论]]。

## 结构

- **三层架构**：原始源 → 维基 → 模式
- **三个操作**：Ingest（摄取）、Query（查询）、Lint（健康检查）
- **两个特殊文件**：index.md（内容目录），log.md（操作日志）

## 引用的工具

- **Obsidian Web Clipper** — 浏览器扩展，将网页转为 markdown
- **Obsidian 图视图** — 查看维基的连接结构
- **Marp** — Markdown 幻灯片格式
- **Dataview** — Obsidian 插件，对 frontmatter 执行查询
- **qmd** — 本地 markdown 搜索引擎（BM25/向量混合搜索）
- **Git** — 版本控制

## 思想先驱

本文提到了 Vannevar Bush 1945 年的 Memex 概念——一个有联想路径的个人知识库。Bush 未能解决维护问题，而 LLM 解决了这个问题。

## 相关页面

- [[wiki/concepts/llm-wiki-methodology|LLM Wiki 方法论]] — 由本文衍生的核心概念页面
- [[wiki/concepts/claude-md-configuration|CLAUDE.md 配置系统]] — 模式层的具体实现
