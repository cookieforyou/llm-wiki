---
title: "Model Context Protocol (MCP)"
type: concept
created: 2026-06-10
updated: 2026-06-10
tags: [ai, llm, mcp, protocol, anthropic]
sources: [mcp-paper]
aliases: [MCP, 模型上下文协议]
---

# Model Context Protocol (MCP)

Model Context Protocol（模型上下文协议）是由 Anthropic 提出的开放协议，旨在标准化应用程序如何向 LLM 提供上下文信息。

> **注意：** 本页面基于源文件名和已有知识编写。PDF 原文的具体内容在进一步提取后可精细化。

## 核心思想

MCP 解决的是 LLM 应用中一个根本性问题：如何让模型安全、高效地访问所需的外部数据和工具。在 MCP 之前，每个 LLM 集成都需要自定义的集成方案。MCP 提供了一个通用标准，类似 USB 为外设提供的标准化接口。

## 关键概念

### MCP 架构

MCP 采用客户端-服务器架构：
- **MCP 客户端** — LLM 应用（如 Claude Code），发起请求
- **MCP 服务器** — 提供数据和工具的后端服务
- **MCP 协议** — 定义通信格式和流程的标准

### 资源（Resources）
- 服务器暴露的上下文数据（文件、数据库记录、API 响应等）
- 客户端可通过协议读取

### 工具（Tools）
- 服务器提供的可执行功能
- LLM 可以动态调用以执行操作
- 有明确的输入/输出模式

### 提示（Prompts）
- 预定义的提示模板
- 帮助标准化常见交互模式

## 应用场景

1. **代码开发** — LLM 通过 MCP 访问代码库、文档和开发工具
2. **数据分析** — 连接数据库和数据处理工具
3. **知识管理** — 与本笔记库中 [[wiki/concepts/llm-wiki-methodology|LLM Wiki 方法论]] 结合，作为搜索和检索工具
4. **DevOps** — 访问监控系统、部署管道等
5. **Skill 工具层** — 为 [[wiki/concepts/skill-system|Skill 系统]] 提供标准化的外部服务连接

## MCP 与 Skill 的关系

在 [[wiki/concepts/skill-system|Skill 系统]] 中，MCP 作为 AI Agent 的标准化工具协议，与 Skill 配合使用：

| | MCP | HTTP 直接调用 |
|---|-----|-------------|
| 定位 | AI Agent 的标准化工具协议 | 通用网络通信协议 |
| 适用场景 | 高频复用、多平台共享、需统一鉴权 | 一次性简单调用、对接老系统 |
| Skill 中的写法 | 只说"做什么"，MCP 负责连接 | 在脚本中写完整请求代码 |

**最佳实践**：MCP 管连接，Skill 管流程，HTTP 脚本兜底处理 MCP 顾不上的场景。优先使用 MCP 生态已有的 Server，避免为每个 API 都封装 MCP Server。

## 与 LLM Wiki 的关系

MCP 可以作为 LLM Wiki 架构中的工具层：
- MCP 服务器可以为 LLM 提供本地搜索能力（如 [[qmd|qmd 搜索引擎]]）
- 标准化的协议使得工具接入更模块化
- LLM 可以通过 MCP 工具更高效地维护维基

## 相关概念

- [[wiki/concepts/claude-md-configuration|CLAUDE.md 配置系统]]
- [[wiki/concepts/llm-wiki-methodology|LLM Wiki 方法论]]
- [[wiki/concepts/skill-system|Skill 系统]] — MCP 作为 Skill 的外部服务连接层

## 参考源

- [[wiki/sources/mcp-paper|Agent 工具与 MCP 协议]]
- [[wiki/sources/skill-practical-guide|Skill 实战经验手册]] — 第 6 章详细讨论 MCP vs HTTP
