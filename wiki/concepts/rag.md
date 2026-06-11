---
title: "RAG"
type: concept
created: 2026-06-11
updated: 2026-06-11
tags: [ai, llm, rag, retrieval-augmented-generation, knowledge-retrieval]
sources: [rag-practical-guide, ai-agent-10-core-concepts]
aliases: [检索增强生成, Retrieval-Augmented Generation, RAG 系统]
---

# RAG（检索增强生成）

RAG（Retrieval-Augmented Generation，检索增强生成）是一种通过 **外部知识检索** 来增强大语言模型生成质量的技术架构。它将"知识存储"和"知识推理"解耦——知识存入可实时更新的外部存储（如向量数据库），LLM 专注于推理和生成，从而有效解决 LLM 的知识截止、事实幻觉和私有数据不可见三大瓶颈。

## 核心价值

| 价值 | 说明 |
|------|------|
| **动态知识注入** | 实时接入最新文档、数据库、API |
| **事实锚定** | 答案基于真实证据，大幅降低幻觉率 |
| **数据主权保障** | 私有数据无需上传至公有云模型 |
| **可解释性增强** | 支持答案溯源，提升用户信任 |

## 六大核心模块

完整的 RAG 系统由以下六个阶段构成，每个阶段都直接影响最终效果：

```
原始数据 → 预处理 → 文本切分 → 向量化 → 向量存储 → 检索 → 提示构造 → 生成
                                                          ↑
                                                      用户问题 → 问题向量化
```

### 1. 数据预处理
将多源异构数据（PDF、Word、网页、数据库、音视频等）统一转换为结构化的干净文本，并保留元数据（来源、页码、时间、分类等）用于后续过滤与溯源。

### 2. 文本切分（Chunking）
将长文本分割为适合向量化的片段。核心权衡：**切太碎丢失上下文，切太长超出模型窗口**。推荐策略为 **递归切分**（优先按段落→句子→token），chunk_size 通常 256–1024 tokens，overlap 设 10%–20%。

### 3. 向量化（Embedding）
将文本片段转换为稠密向量，使语义相近的文本在向量空间中距离更近。中文项目推荐 `bge-large-zh-v1.5`（1024 维，中文 SOTA）。工程优化包括批量编码、向量缓存、PCA 维度压缩。

### 4. 检索（Retrieval）
从向量库中找出与用户问题最相关的 Top-K 文档片段。**高级策略**包括：
- **混合检索**：BM25 关键词匹配 + 向量相似度加权融合
- **重排序（Reranking）**：粗召回 Top-100 → Cross-encoder 精排 Top-5
- **元数据过滤**：按时间、类别、来源等维度精确过滤

### 5. 提示构造（Prompt Engineering）
将检索结果有效注入 LLM 上下文。关键技巧：引用标注、指令强化、上下文压缩。

### 6. 生成与后处理
Temperature=0 提升确定性，Faithfulness 验证防止幻觉，溯源展示增强可信，失败回退机制兜底。

## RAG 的进阶演进

```
基础 RAG ─→ Self-RAG ─→ Corrective RAG ─→ Graph RAG
  （检索+生成）  （自反思判断）   （验证修正）    （知识图谱推理）
```

| 变体 | 核心思想 | 关键突破 |
|------|---------|---------|
| **Self-RAG** (ICLR 2024) | LLM 自主判断是否检索、结果是否相关、是否使用 | 按需检索，减少不必要的检索开销 |
| **Corrective RAG** | 对检索结果做验证与修正（如搜索引擎核实） | 提高检索结果的准确性和可靠性 |
| **Graph RAG**（微软开源） | 将知识建为图谱，支持多跳推理 | 解决需要跨文档推理的复杂问题 |

## 评估指标

| 阶段 | 指标 | 说明 |
|------|------|------|
| 检索 | Hit Rate@K、MRR | 能否召回相关文档 |
| 生成 | Faithfulness、Answer Relevance | 答案是否基于文档、是否切题 |
| 端到端 | Task Success Rate、User Satisfaction | 用户是否得到满意答案 |

推荐使用 [Ragas](https://github.com/explodinggradients/ragas) 评估框架。

## 典型应用场景

- **企业知识库问答** — "差旅报销标准是多少？"
- **智能客服** — "我的订单 #12345 什么时候发货？"
- **科研助手** — "近五年关于 CRISPR-Cas9 的综述有哪些？"
- **合规审查** — "该合同条款是否符合《数据安全法》？"

## 反模式与避坑

✅ **必须做**：使用与训练一致的 embedding 模型、保留元数据、设置 fallback 机制、敏感数据脱敏
❌ **禁止做**：直接将整篇长文档塞入 LLM、忽略分块策略硬切、不评估就上线

## 相关概念

- [[wiki/concepts/ai-agent|AI Agent]] — RAG 作为 Agent 能力扩展层（"实时更新的外挂大脑"）
- [[wiki/concepts/llm-wiki-methodology|LLM Wiki 方法论]] — 与传统 RAG 相对的知识编译范式
- [[wiki/concepts/model-context-protocol|Model Context Protocol (MCP)]] — 平行互补：MCP 管工具连接，RAG 管知识检索
- [[wiki/concepts/skill-system|Skill 系统]] — 可封装 RAG 能力为可复用模块

## 参考源

- [[wiki/sources/rag-practical-guide|RAG 全流程深度实战指南]] — 本文主要工程参考，含完整代码
- [[wiki/sources/ai-agent-10-core-concepts|AI Agent 十大核心概念深度剖析]] — RAG 的概念解构
