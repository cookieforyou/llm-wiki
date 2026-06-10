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

Model Context Protocol（模型上下文协议）是由 Anthropic 提出的开放标准协议，旨在标准化 AI 模型与外部数据源、工具和服务之间的通信接口。

> 本文结合了 PDF 资料与 [[wiki/sources/ai-agent-10-core-concepts|AI Agent 十大核心概念深度剖析]]（第 6 节）的内容。

## 核心思想

MCP 解决的是 AI 生态中面临的 **M×N 集成问题**——有 M 个 AI 应用和 N 个外部工具时，传统方式需要 M×N 个定制集成。MCP 引入一个标准协议层，将 M×N 问题降维为 **M+N 问题**——每个 AI 应用只需实现一次 MCP 客户端，每个工具只需实现一次 MCP 服务端。

类比：MCP 就像 **AI 世界的 USB 标准**。在 USB 出现之前，每个外设需要不同接口和驱动。USB 统一了连接方式。同理，MCP 统一了 AI 模型与所有外部工具的连接方式。

## 工作流程

MCP 的交互分为四个阶段：

1. **连接建立**：Client 初始化并协商协议版本和能力
2. **工具发现**：Client 请求 Server 列出所有可用工具
3. **工具调用**：Client 调用指定工具并传递参数，Server 执行并返回结果
4. **资源读取**：Client 按需读取 Server 暴露的数据资源

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

## MCP vs 其他集成方式

| 维度 | MCP | OpenAI Plugins | LangChain Tools | 定制 API 集成 |
|------|-----|---------------|-----------------|------------|
| 标准化程度 | ✅ 开放标准 | ❌ 平台私有 | ⚠️ 框架绑定 | ❌ 无标准 |
| 集成复杂度 | O(M+N) | O(M+N) 但封闭 | O(M+N) 但框架锁定 | O(M×N) |
| 跨平台互操作 | ✅ 任何 MCP 客户端 | ❌ 仅 ChatGPT | ⚠️ 仅 LangChain | ❌ 一对一 |
| 安全机制 | ✅ 协议层统一 | ✅ 平台管控 | ⚠️ 自行实现 | ⚠️ 自行实现 |
| 传输方式 | stdio / HTTP+SSE | HTTP | Python 函数调用 | 各异 |

## 应用场景

1. **代码开发** — LLM 通过 MCP 访问代码库、文档和开发工具
2. **数据分析** — 连接数据库和数据处理工具
3. **知识管理** — 与本笔记库中 [[wiki/concepts/llm-wiki-methodology|LLM Wiki 方法论]] 结合，作为搜索和检索工具
4. **DevOps** — 访问监控系统、部署管道等
5. **Skill 工具层** — 为 [[wiki/concepts/skill-system|Skill 系统]] 提供标准化的外部服务连接
6. **IDE 集成** — Cursor、Windsurf 通过 MCP 连接数据库/Git/文档，无需切换窗口

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

## 未来展望

MCP 解决了"AI 与工具的标准化连接"，而 Google 的 **A2A（Agent-to-Agent）** 协议解决"Agent 与 Agent 的标准化通信"。两者成熟后，可能出现"Agent 互联网"——数十亿 Agent 像网站一样互联互通。

## 安全最佳实践

- **最小权限原则**：MCP Server 只暴露必要的资源和工具
- **协议版本协商**：initialize 阶段严格校验版本兼容性
- **异步处理**：长时间运行的工具调用使用异步模式或进度通知
- **避免硬编码凭证**：MCP Server 通过环境变量统一管理鉴权

## 相关概念

- [[wiki/concepts/claude-md-configuration|CLAUDE.md 配置系统]]
- [[wiki/concepts/llm-wiki-methodology|LLM Wiki 方法论]]
- [[wiki/concepts/skill-system|Skill 系统]] — MCP 作为 Skill 的外部服务连接层
- [[wiki/concepts/ai-agent|AI Agent]] — MCP 作为 Agent 架构的标准化工具协议层

## 参考源

- [[wiki/sources/mcp-paper|Agent 工具与 MCP 协议]]
- [[wiki/sources/skill-practical-guide|Skill 实战经验手册]] — 第 6 章详细讨论 MCP vs HTTP
- [[wiki/sources/ai-agent-10-core-concepts|AI Agent 十大核心概念深度剖析]] — 第 6 节详细剖析 MCP
