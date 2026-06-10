---
title: "AI Agent"
type: concept
created: 2026-06-10
updated: 2026-06-10
tags: [ai, agent, architecture, llm, system-design]
sources: [ai-agent-10-core-concepts]
aliases: [AI 智能体, 自主智能体, Intelligent Agent]
---

# AI Agent

AI Agent（智能体）是一种能够自主感知环境、制定决策、执行行动并根据反馈调整策略的智能系统。它不是某个单一技术，而是十大核心概念的**终极融合体**——以 LLM 为大脑，通过工具与外部世界交互，借助记忆维持连续性，运用规划和推理完成复杂任务。

## 十大核心概念架构

AI Agent 的能力由十个基础概念分层构建：

```
┌─────────────────────────────────────────────────┐
│            Agent  智能体（终极融合）                │
│  ┌───────────────────────────────────────────┐  │
│  │  Skills  技能系统（能力封装层）               │  │
│  │  ┌─────────────────────────────────────┐  │  │
│  │  │  ReAct + Planning  规划与执行层      │  │  │
│  │  │  ┌───────────────────────────────┐  │  │  │
│  │  │  │  Memory  记忆层（状态管理层）    │  │  │  │
│  │  │  │  ┌─────────────────────────┐  │  │  │  │
│  │  │  │  │  FC + MCP + RAG  扩展层 │  │  │  │  │
│  │  │  │  │  ┌───────────────────┐  │  │  │  │  │
│  │  │  │  │  │  LLM + Prompt  基础│  │  │  │  │  │
│  │  │  │  │  └───────────────────┘  │  │  │  │  │
│  │  │  │  └─────────────────────────┘  │  │  │  │
│  │  │  └───────────────────────────────┘  │  │  │
│  │  └─────────────────────────────────────┘  │  │
│  └───────────────────────────────────────────┘  │
└─────────────────────────────────────────────────┘
```

## 一、基础设施层

### 1. LLM（大语言模型）
基于 Transformer 架构的通用推理引擎。Agent 的"大脑"，负责语义理解、逻辑推理和内容生成。核心能力包括：自注意力机制建立全局语义关联、涌现能力（规模驱动的质变）、上下文学习（In-Context Learning）。

### 2. Prompt（提示词工程）
人机交互的核心接口层。将用户模糊需求转化为 LLM 可精确执行的指令。关键模式包括：System Prompt（角色边界）、Few-shot（示例教学）、Chain-of-Thought（逐步推理）、ReAct（推理-行动交替）。

## 二、能力扩展层

### 3. Function Calling（函数调用）
LLM 与外部工具交互的协议机制。LLM 输出结构化函数调用请求，由运行时执行并返回结果。核心组件：Tool Schema（工具描述）、Tool Selection（工具选择）、Parameter Extraction（参数提取）。

### 4. RAG（检索增强生成）
通过外部知识检索增强 LLM 生成质量。将"知识存储"和"知识推理"解耦——知识存入可实时更新的向量数据库，LLM 专注于推理和生成。关键策略：混合检索（Dense + Sparse）、Reranker 精排、Query 改写。

### 5. Memory（记忆系统）
分层信息存储与检索机制，模拟人类记忆：
- **短期记忆**：当前对话上下文（工作台）
- **长期记忆**：跨会话持久化信息（档案柜）
- **情景记忆**：完整交互事件记录（日记本）
- **工作记忆**：当前任务中间状态（草稿纸）

参见 [[wiki/concepts/claude-md-configuration|CLAUDE.md 配置系统]] 中关于 Auto Memory 的具体实现。

### 6. MCP（模型上下文协议）
Anthropic 提出的开放标准协议，标准化 LLM 与外部工具的连接方式。将 M×N 集成问题降维为 M+N 问题。详见 [[wiki/concepts/model-context-protocol|Model Context Protocol (MCP)]]。

## 三、决策控制层

### 7. ReAct（推理-行动框架）
将推理（Reasoning）和行动（Acting）交错执行的决策框架。核心循环：**Thought**（分析当前状态）→ **Action**（选择并执行操作）→ **Observation**（获取结果）→ 循环直到信息足够。解决了纯推理"闭门造车"和纯行动"盲目试错"的矛盾。

### 8. Planning（任务规划）
将复杂高层目标自动分解为有序子任务的机制。包括：任务分解（Decomposition）、依赖分析（Dependency Graph）、动态重规划（Replanning）。常见策略：Plan-and-Solve（先规划后执行）、Adaptive Planning（边执行边规划）。

## 四、能力封装层

### 9. Skills（技能系统）
可组合、可复用的能力封装单元。每个 Skill 是独立的功能模块，包含输入/输出接口、执行逻辑和元数据。详见 [[wiki/concepts/skill-system|Skill 系统]]。

## 五、架构体系

### 完整工作流
```
1. 接收任务  → 解析输入 + 检索记忆（Prompt + Memory）
2. 理解任务  → LLM 推理 + RAG 知识检索（LLM + RAG）
3. 制定计划  → 任务分解 + 技能识别（Planning + Skills）
4. 逐步执行  → ReAct 循环 + 工具调用（ReAct + FC + MCP）
5. 知识增强  → 按需检索补充信息（RAG）
6. 状态维护  → 实时更新记忆（Memory）
7. 结果输出  → 综合生成最终回复（LLM + Prompt）
8. 反思学习  → 提取经验存入长期记忆（Memory）
```

### Agent 类型对比

| 类型 | 架构 | 适用场景 | 代表系统 |
|------|------|---------|---------|
| 单 Agent | 一个处理所有任务 | 个人助手 | AutoGPT |
| Multi-Agent | 多个专业化 Agent 协作 | 团队任务 | CrewAI, MetaGPT |
| Hierarchical Agent | 上级调度下级 | 企业流程 | LangGraph |
| Swarm | Agent 自组织 | 大规模分布式 | OpenAI Swarm |

### 落地路线图
- **Phase 1（1 周 MVP）**：LLM + Prompt + 基础 FC（3-5 个工具）
- **Phase 2（2 周）**：RAG + Memory + ReAct 循环
- **Phase 3（4 周生产化）**：Planning + Skills + MCP + 安全护栏
- **Phase 4（持续进阶）**：Multi-Agent + 自我反思 + 技能市场

## 安全与工程化

- **Guardrails**：安全护栏，防止 Agent 执行高风险操作
- **Human-in-the-loop**：关键操作需人工确认
- **成本监控**：设置单次任务 token/费用上限
- **审计日志**：记录所有决策和执行记录

## 相关概念

- [[wiki/concepts/llm-wiki-methodology|LLM Wiki 方法论]] — 本知识库的方法论基础
- [[wiki/concepts/model-context-protocol|Model Context Protocol (MCP)]]
- [[wiki/concepts/skill-system|Skill 系统]]
- [[wiki/concepts/claude-md-configuration|CLAUDE.md 配置系统]]

## 参考源

- [[wiki/sources/ai-agent-10-core-concepts|AI Agent 十大核心概念深度剖析]]
