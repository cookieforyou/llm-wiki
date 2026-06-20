---
title: "AI Agent"
type: concept
created: 2026-06-10
updated: 2026-06-20
tags: [ai, agent, architecture, llm, system-design, enterprise, orchestrator, multi-agent]
sources: [ai-agent-10-core-concepts, enterprise-agent-guide, enterprise-skill-guide]
aliases: [AI 智能体, 自主智能体, Intelligent Agent, Agentic AI]
---

# AI Agent

AI Agent（智能体）是一种能够自主感知环境、制定决策、执行行动并根据反馈调整策略的智能系统。它不是某个单一技术，而是多种核心概念的**融合体**——以 LLM 为大脑，通过工具与外部世界交互，借助记忆维持连续性，运用规划和推理完成复杂任务。

---

## 企业级定义

### Chatbot → Copilot → Agent 的演进

| 阶段 | 特征 | 人类角色 | 典型代表 |
|------|------|----------|----------|
| **Chatbot** | 单轮/多轮对话，被动问答 | 全程主导 | 客服机器人 |
| **Copilot** | 辅助生成，人做最终决策 | 审核确认 | GitHub Copilot |
| **Agent** | 自主规划、调用工具、闭环执行 | 监督兜底 | 本体系所建系统 |

**核心区别**：Agent 不只是"回答问题"，而是**理解目标 → 拆解计划 → 调用工具 → 执行动作 → 验证结果 → 输出交付物**，形成完整的 **Action Loop（行动闭环）**。

### 企业级 AI Agent 定义

> 以 LLM 为"大脑"，以 MCP/API 工具为"手脚"，以记忆系统为"经验库"，以 Guardrails 为"安全边界"，在编排引擎的统一调度下，**自主完成特定业务场景的端到端任务执行**的软件系统。

五条企业级标准：

| 维度 | 要求 |
|------|------|
| **可靠性** | 99.9%+ 可用率，任务成功率可量化 |
| **可观测性** | 每一步可追溯、可审计 |
| **安全性** | 权限隔离、数据脱敏、防注入 |
| **可扩展性** | 支持并发、可水平扩展 |
| **合规性** | 符合数据保护法规、操作留痕 |

---

## 六层架构视图

企业级 Agent 可自底向上分解为六大核心层：

```
╔══════════════════════════════════════════════════════════════╗
║                    用户/事件 接入层                            ║
║          (Web Hook / API Gateway / Message Queue)            ║
╠══════════════════════════════════════════════════════════════╣
║                    编排引擎层 (Orchestrator)                   ║
║        LangGraph / Temporal / 状态机                          ║
║   ┌──────────┬──────────┬──────────┬──────────┐             ║
║   │ 任务规划  │ 状态管理  │ 流程控制  │ 错误恢复  │             ║
║   └──────────┴──────────┴──────────┴──────────┘             ║
╠══════════════════════════════════════════════════════════════╣
║                    智能核心层 (LLM Brain)                      ║
║   ┌──────────┬──────────┬──────────┬──────────┐             ║
║   │ 主力推理  │ 辅助模型  │ Embedding│ 重排序器  │             ║
║   │(Sonnet/  │(Haiku/   │  模型    │(Reranker)│             ║
║   │ GPT-4o)  │ 4o-mini) │          │          │             ║
║   └──────────┴──────────┴──────────┴──────────┘             ║
╠══════════════════════════════════════════════════════════════╣
║                    能力层 (Tools / Skills)                     ║
║   ┌────────┬────────┬────────┬────────┬────────┐            ║
║   │OCR解析  │数据查重  │政策RAG  │API调用  │ERP写入  │            ║
║   └────────┴────────┴────────┴────────┴────────┘            ║
║              ↕ MCP Protocol (标准化工具接口)                    ║
╠══════════════════════════════════════════════════════════════╣
║                    记忆层 (Memory System)                      ║
║   ┌──────────┬──────────┬──────────────┐                    ║
║   │ 工作记忆  │ 情景记忆  │ 语义记忆（知识库）│                    ║
║   │(当前状态) │(历史案例) │(政策/规则文档) │                    ║
║   └──────────┴──────────┴──────────────┘                    ║
╠══════════════════════════════════════════════════════════════╣
║                    安全与治理层 (Guardrails)                    ║
║   ┌──────────┬──────────┬──────────┬──────────┐             ║
║   │输入过滤   │输出校验   │权限控制   │审计日志   │             ║
║   └──────────┴──────────┴──────────┴──────────┘             ║
╚══════════════════════════════════════════════════════════════╝
```

此架构与下文"十大核心概念架构"互为补充——六层架构侧重工程部署视角，十大概念侧重能力抽象视角。

---

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

### 一、基础设施层

#### 1. LLM（大语言模型）
基于 Transformer 架构的通用推理引擎。Agent 的"大脑"，负责语义理解、逻辑推理和内容生成。

#### 2. Prompt（提示词工程）
人机交互的核心接口层。关键模式：System Prompt（角色边界）、Few-shot（示例教学）、Chain-of-Thought（逐步推理）、ReAct（推理-行动交替）。

### 二、能力扩展层

#### 3. Function Calling（函数调用）
LLM 与外部工具交互的协议机制。LLM 输出结构化函数调用请求，由运行时执行并返回结果。

**核心流程**：
1. 系统 Prompt 中注入工具描述（name, description, parameters）
2. LLM 推理后返回工具调用指令（tool_calls）
3. 编排引擎拦截并执行对应函数
4. 执行结果回传给 LLM 继续推理
5. 循环直到 LLM 返回最终文本

> ⚠️ **关键理解**：LLM **从不直接执行**任何操作，它只是"说"要调用什么工具、传什么参数。真正的执行由编排引擎完成——这是 Agent 安全可控的基础。

#### 4. RAG（检索增强生成）
通过外部知识检索增强 LLM 生成质量。详见 [[wiki/concepts/rag|RAG]]。

#### 5. Memory（记忆系统）

企业级 Agent 采用三层记忆架构：

| 层级 | 存储内容 | 生命周期 | 实现方案 | 人类类比 |
|------|----------|----------|----------|----------|
| **工作记忆** | 当前任务上下文、中间状态、对话历史 | 单次任务 | LangGraph State / Redis | "此刻正在想的事" |
| **情景记忆** | 历史案例、成功/失败模式、纠错记录 | 长期持久化 | PostgreSQL + 向量数据库 | "过去的经历和教训" |
| **语义记忆** | 政策文档、财务制度、FAQ、知识库 | 长期持久化，定期更新 | RAG（向量数据库 + 文档分块） | "知识和常识" |

#### 6. MCP（模型上下文协议）
Anthropic 提出的开放标准协议，标准化 LLM 与外部工具的连接方式。详见 [[wiki/concepts/model-context-protocol|MCP]]。

**MCP vs Function Calling**：

| 维度 | Function Calling | MCP |
|------|-----------------|-----|
| 工具定义 | 每家 LLM 格式不同 | 统一 JSON Schema |
| 工具发现 | 硬编码在应用中 | 运行时动态发现 |
| 权限管理 | 自行实现 | 协议层面内置 |
| 复用性 | 绑定特定应用 | Server 可被任何 Agent 复用 |
| 生态 | 碎片化 | 统一生态，工具市场化 |

### 三、决策控制层

#### 7. ReAct（推理-行动框架）
将推理（Reasoning）和行动（Acting）交错执行的决策框架。核心循环：**Thought** → **Action** → **Observation** → 循环。

#### 8. Planning（任务规划）
将复杂高层目标自动分解为有序子任务的机制。常见策略：Plan-and-Solve（先规划后执行）、Adaptive Planning（边执行边规划）。

### 四、能力封装层

#### 9. Skills（技能系统）
可组合、可复用的能力封装单元。每个 Skill 是独立的功能模块。详见 [[wiki/concepts/skill-system|Skill 系统]]。

---

## 编排引擎选型

编排引擎（Orchestrator）负责管理 Agent 的任务执行流程，是"神经系统"：

| 方案 | 原理 | 适用场景 | 优势 | 劣势 |
|------|------|----------|------|------|
| **LangGraph** | 有向图状态机，节点=动作，边=条件转移 | 中等复杂度工作流 | 灵活、Human-in-the-loop、Streaming | 学习曲线陡 |
| **Temporal** | 持久化工作流引擎，代码即流程 | 企业级长运行任务 | 强一致性、重试、版本管理 | 运维复杂 |
| **纯 LLM ReAct** | LLM 自主决定下一步 | 简单/探索性任务 | 最灵活 | 不可控、不稳定 |
| **Dify/Coze** | 拖拽式 DAG 工作流 | 快速原型/业务人员 | 低代码 | 定制性差 |

**LangGraph 核心概念**：
- **Node（节点）** = 一个具体的处理动作（LLM 调用、API 调用、纯代码逻辑）
- **Edge（边）** = 节点间的流转关系
- **Conditional Edge（条件边）** = 根据运行时状态动态决定下一个节点
- **State（状态）** = 在节点间流转共享的数据结构
- **Checkpointer** = 持久化状态检查点，任务中断可恢复

---

## LLM 分层使用策略

企业级 Agent 的关键认知：**不同子任务用不同模型**，这是成本与质量的最优平衡。

| 层级 | 模型 | 用途 | 特点 |
|------|------|------|------|
| **Tier 1 主力推理** | Claude Sonnet / GPT-4o | 复杂推理、政策解读、最终决策 | 最强能力，最高成本，调用最少 |
| **Tier 2 辅助模型** | Claude Haiku / GPT-4o-mini | 字段提取、格式转换、简单分类 | 速度快，成本低，批量调用 |
| **Tier 3 专用模型** | OCR 模型 / Embedding / 分类器 | 图片文字识别、向量化、规则匹配 | 确定性输出，毫秒级响应 |

### 结构化输出

使用 Pydantic / JSON Schema 约束 LLM 输出格式，消除 LLM 输出的不确定性：

```python
class InvoiceExtraction(BaseModel):
    invoice_number: str = Field(description="发票号码")
    amount: float = Field(description="金额（元）")
    confidence: float = Field(ge=0, le=1, description="提取置信度")

result = llm.with_structured_output(InvoiceExtraction).invoke(text)
```

---

## Guardrails 安全架构

Guardrails 是在 LLM 输入和输出两端设置的"安全护栏"：

```
           ┌─────────── Guardrails 架构 ───────────┐
           │                                        │
  输入 ──→ │ 输入侧 Guardrails                        │
           │ • Prompt Injection 检测                 │
           │ • PII（敏感信息）脱敏                     │
           │ • 输入格式校验                           │
           │ • 权限检查                               │
           │          │                              │
           │          ▼                              │
           │     Agent Core                          │
           │          │                              │
           │          ▼                              │
           │ 输出侧 Guardrails                        │
           │ • 业务规则校验（金额上限、审批层级等）       │
           │ • 决策格式校验                            │
           │ • 敏感操作拦截                            │
           │ • 置信度阈值（< 0.7 → 人工复核）           │
           │ • 幻觉检测（结果是否与证据一致）            │
           └────────────────────────────────────────┘
```

---

## Multi-Agent 协作模式

当场景复杂度增长，单 Agent 可能不堪重负。四种协作模式：

### 1. 管道式 (Pipeline)
```
Agent A → Agent B → Agent C → 结果
```
适用：流程固定、步骤间有明确先后关系。如：解析 Agent → 审核 Agent → 执行 Agent

### 2. 监督者模式 (Supervisor)
```
         ┌── 工具Agent A
   主管 ─┼── 工具Agent B
   Agent └── 工具Agent C
```
适用：主管 Agent 动态分配任务给专业 Agent

### 3. 辩论式 (Debate)
```
Agent A ⟷ Agent B → 裁判 Agent → 结论
```
适用：需要多视角判断的复杂决策

### 4. 层级式 (Hierarchical)
```
战略Agent → 战术Agent → 执行Agent
```
适用：大型组织的复杂审批流程

### 演进路径

```
Phase 1: 单 Agent + 多工具（一人完成所有步骤）
Phase 2: 监督者模式（主管分配，专业 Agent 各司其职）
Phase 3: 全链路 Multi-Agent（新增合规、风控、客服等专业 Agent）
```

---

## 评估指标体系

### 业务指标
| 指标 | 目标 | 说明 |
|------|------|------|
| 自动处理率 | ≥ 70% | 无需人工介入的比例 |
| 审核准确率 | ≥ 95% | Agent 决策 vs 最终正确决策 |
| 平均处理时长 | ≤ 30s/笔 | 端到端延迟 |
| 人力节省率 | 持续提升 | (原人力 - 现人力) / 原人力 |

### 技术指标
| 指标 | 目标 |
|------|------|
| LLM 调用成功率 | ≥ 99.5% |
| 工具调用成功率 | ≥ 99% |
| 结构化输出解析成功率 | ≥ 98% |
| 端到端可用性 | ≥ 99.9% |
| P95 延迟 | ≤ 15s |

### 安全指标
| 指标 | 目标 |
|------|------|
| Prompt Injection 拦截率 | 100% |
| 幻觉率 | ≤ 2% |
| Guardrails 触发率 | 监控异常波动 |

---

## 生产级考量

### 容错与重试
- 工具调用带指数退避重试（最多 3 次）
- 关键工具提供降级方案（如发票验真 API 不可用 → 标记"待人工验证"而非直接失败）

### 成本控制
| 策略 | 预期节省 |
|------|----------|
| **模型分层** | 60-70% |
| **Prompt 缓存** | 50%（Claude） |
| **结果缓存** | 避免重复调用 |
| **Early Exit** | 失败尽早返回，节省后续 LLM 调用 |

### 可观测性三支柱
- **Traces（链路追踪）**：LangFuse / LangSmith 追踪每次 Agent 执行的全链路
- **Metrics（指标监控）**：任务成功率、平均处理时长、Token 消耗量
- **Logs（日志）**：结构化 JSON 日志，每次 LLM 调用的完整 prompt 和 response

### 灰度发布
```
影子模式（并行不执行）→ 10% 低风险 → 50% 中等 → 100% 全量
```

---

## 完整工作流

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

---

## Agent 的本质：决策-执行循环

从根本上说，AI Agent 就是一个 LLM 驱动的决策-执行循环：

```
while not done:
    observation = perceive(state, environment)     # 1. 感知
    thought, action = llm.reason(observation, ...)  # 2. 思考
    result = execute(action)                        # 3. 行动
    state.update(result)                            # 4. 更新
    done = is_complete(state, goal)                 # 5. 判断
```

理解了这一点，所有技术选择背后的逻辑就清晰了：
- **RAG** → 让"感知"更丰富
- **Structured Output** → 让"思考"更可控
- **Tool Calling** → 让"行动"更强大
- **Memory** → 让"经验"可积累
- **Guardrails** → 让"行动"更安全

---

## 相关概念

- [[wiki/concepts/skill-system|Skill 系统]] — Agent 的能力封装层
- [[wiki/concepts/model-context-protocol|Model Context Protocol (MCP)]] — Agent 的工具标准化接口
- [[wiki/concepts/rag|RAG]] — Agent 的知识检索技术
- [[wiki/concepts/claude-md-configuration|CLAUDE.md 配置系统]] — Agent 的持久化配置机制
- [[wiki/concepts/llm-wiki-methodology|LLM Wiki 方法论]] — 本知识库的方法论基础

## 参考源

- [[wiki/sources/enterprise-agent-guide|从零到一搭建企业级 AI Agent 完全指南]] — 企业级 Agent 实施方法论（报销审核场景）
- [[wiki/sources/ai-agent-10-core-concepts|AI Agent 十大核心概念深度剖析]] — 十大核心概念全息解构
- [[wiki/sources/enterprise-skill-guide|企业级 AI Agent Skill 深度构建指南]] — 企业级 Skill 构建方法论
- [[wiki/sources/rag-practical-guide|RAG 全流程深度实战指南]] — RAG 工程实践
- [[wiki/sources/claude-memory-guide|Claude 记忆机制指南]] — 记忆机制官方文档
- [[wiki/sources/mcp-paper|Agent 工具与 MCP 协议]] — MCP 协议资料
