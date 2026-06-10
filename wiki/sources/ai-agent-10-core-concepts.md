---
title: "AI Agent 十大核心概念深度剖析"
type: source
created: 2026-06-10
updated: 2026-06-10
tags: [ai, agent, llm, prompt, rag, mcp, memory, react, planning, skill]
source: raw/articles/AI Agent 十大核心概念深度剖析.md
---

# AI Agent 十大核心概念深度剖析

> **原始位置：** `raw/articles/AI Agent 十大核心概念深度剖析.md`
> **字数：** 约 12.8 万字（2999 行）
> **剪藏日期：** 2026-06-10

## 内容概述

本文以"全息知识解构"方法，对 AI Agent 的十大核心概念进行六维深度剖析，分为三层：

- **第一层（独立解构）**：10 个概念逐一六维剖析
- **第二层（关联映射）**：概念间的依赖与协作关系
- **第三层（聚合升华）**：十大概念融合为完整 Agent 架构

## 十大概念速览

| # | 概念 | 一句话本质 | 核心解决问题 |
|---|------|-----------|-------------|
| 1 | **LLM** | 通用智能推理引擎 | 让机器理解和生成自然语言 |
| 2 | **Prompt** | 意图到指令的翻译器 | 将模糊需求转为精确指令 |
| 3 | **Function Calling** | AI 连接物理世界的双手 | 打破 LLM 只能生成文本的限制 |
| 4 | **RAG** | 实时更新的外挂大脑 | 解决知识截止和幻觉问题 |
| 5 | **Memory** | 跨越时空的状态保持器 | 让无状态 LLM 具备连续能力 |
| 6 | **MCP** | AI 工具生态的 USB 统一接口 | 解决 M×N 工具集成难题 |
| 7 | **ReAct** | 知行合一的决策心跳 | 推理与行动交替进行 |
| 8 | **Planning** | 复杂任务的降维拆解器 | 将大目标拆为可执行步骤 |
| 9 | **Skills** | 可插拔的模块化能力商店 | 能力解耦与复用 |
| 10 | **Agent** | 九大概念融合的自主数字员工 | 从被动响应到主动自主 |

## 六维解构框架

每个概念都按统一格式剖析：

1. **概念破壁** — 神级类比 + 专业定义 + 第一性原理
2. **知识图谱** — 子概念表 + 内部运作机制图 (Mermaid)
3. **底层原理深潜** — Sequence Diagram + 公式 + 竞品对比表
4. **真实应用** — 工业级场景 + 反模式与避坑
5. **上手实现** — Python 代码示例（可直接运行的工程代码）
6. **认知升华** — 学习路径 + 终极灵魂拷问

## 依赖关系全景

文章第二层梳理了概念间的依赖网络：

```
基础设施层: LLM → Prompt
能力扩展层: Function Calling → MCP, RAG → Memory
状态管理层: Memory
决策控制层: ReAct → Planning
能力封装层: Skills → FC → MCP
终极融合层: Agent（依赖所有上层概念）
```

## 从零构建路线图

文章第三层提供了 4 阶段落地路线图：

- **Phase 1（1 周）**：LLM + Prompt + 基础 Function Calling
- **Phase 2（2 周）**：RAG + Memory + ReAct 循环
- **Phase 3（4 周）**：Planning + Skills + MCP + 安全护栏
- **Phase 4（持续）**：Multi-Agent + 自我反思 + 技能市场

## 核心观点

- "Agent 不是魔法，是计算机科学、认知科学与系统工程学的极致结晶"
- "不要迷信大一统模型—模块化和分层规划才是工程化的正道"
- "没有 Guardrails 和 Human-in-the-loop 的 Agent 在生产环境就是定时炸弹"

## 相关页面

- [[wiki/concepts/ai-agent|AI Agent]] — 十大概念聚合概念页面
- [[wiki/concepts/model-context-protocol|Model Context Protocol (MCP)]] — 文章第 6 节详细讨论了 MCP
- [[wiki/concepts/skill-system|Skill 系统]] — 文章第 9 节提供了 Skills 的系统视角
