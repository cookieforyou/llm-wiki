# wiki 操作日志

## 记录日志的日志

```markdown
## [YYYY-MM-DD] 操作类型 | 标题

- **操作：** ingest | query | lint | update
- **涉及页面：** [[链接]]
- **要点：** 操作摘要
```

---

## [2026-06-10] init | 初始化知识库结构

- **操作：** init
- **涉及页面：** [[wiki/index.md]], [[wiki/log.md]], [[wiki/concepts/llm-wiki-methodology]], [[wiki/concepts/model-context-protocol]], [[wiki/concepts/claude-md-configuration]], [[wiki/sources/llm-wiki-idea]], [[wiki/sources/claude-memory-guide]], [[wiki/sources/mcp-paper]]
- **要点：** 基于 LLM Wiki 方法论初始化知识库，创建三层架构（raw/wiki/schema），迁移 3 个源文件，建立索引和日志系统。

## [2026-06-10] update | 翻译 LLM Wiki + 生成 CLAUDE.md

- **操作：** update
- **涉及页面：** [[LLM Wiki 方法论]], [[CLAUDE.md]], [[.claude/CLAUDE.md]], [[wiki/index.md]]
- **要点：** 翻译 LLM Wiki.md 为中文并创建 LLM Wiki 方法论.md；生成根级 CLAUDE.md 作为主 Schema，简化 .claude/CLAUDE.md 为导入结构；更新索引。

## [2026-06-10] ingest | 两篇 Skill 相关文章

- **操作：** ingest
- **涉及页面：** [[wiki/sources/skill-beginners-tutorial]], [[wiki/sources/skill-practical-guide]], [[wiki/concepts/skill-system]], [[wiki/concepts/model-context-protocol]], [[wiki/index.md]]
- **要点：** 处理 raw/clippings/ 中新增的两篇 Skill 教程文章（保姆级入门教程 + 腾讯实战经验手册）。创建 2 个源摘要页面，创建 Skill 系统概念页面（涵盖三层加载机制、description 编写、反模式、安全规范、工程化评估等），更新 MCP 页面补充 Skill-MCP 关系。

## [2026-06-10] ingest | AI Agent 十大核心概念深度剖析

- **操作：** ingest
- **涉及页面：** [[wiki/sources/ai-agent-10-core-concepts]], [[wiki/concepts/ai-agent]], [[wiki/concepts/model-context-protocol]], [[wiki/concepts/skill-system]], [[wiki/index.md]]
- **要点：** 处理 raw/articles/ 中新增的 AI Agent 深度剖析文章（12.8 万字，10 个概念 × 6 维度）。创建源摘要页面，创建 AI Agent 概念页面作为十大概念的聚合枢纽（含分层架构、完整工作流、落地路线图），更新 MCP 页面补充 M×N 问题框架和跨方案对比，更新 Skill 页面补充 Agent 架构定位和生态视角。

## [2026-06-20] ingest | 两篇企业级 AI Agent 文章

- **操作：** ingest
- **涉及页面：** [[wiki/sources/enterprise-skill-guide]], [[wiki/sources/enterprise-agent-guide]], [[wiki/concepts/skill-system]], [[wiki/concepts/ai-agent]], [[wiki/index.md]]
- **要点：** 处理 raw/articles/ 中新增的两篇文章。《企业级 AI Agent Skill 深度构建指南》补充了 Skill 系统的五大设计模式、企业级标准对比、工作流识别与拆解方法、生命周期治理等内容。《从零到一搭建企业级 AI Agent 完全指南》以报销审核为场景，补充了 AI Agent 的六层架构、编排引擎选型、LLM 分层策略、Guardrails 设计、Multi-Agent 协作模式、评估指标和生产级考量。创建 2 个源摘要页面，扩展 Skill 和 AI Agent 两个概念页面。

## [2026-06-11] ingest | RAG 全流程深度实战指南

- **操作：** ingest
- **涉及页面：** [[wiki/sources/rag-practical-guide]], [[wiki/concepts/rag]], [[wiki/queries/rag-vs-llm-wiki]], [[wiki/concepts/ai-agent]], [[wiki/concepts/llm-wiki-methodology]], [[wiki/index.md]]
- **要点：** 处理 raw/clippings/ 中新增的 RAG 实战指南（CSDN，9500 字）。创建源摘要页面（含六大模块核心摘要与代码要点）；创建 RAG 概念页面（涵盖六大模块链路、进阶演进、评估指标、应用场景）；创建 RAG vs LLM Wiki 对比查询页面（含决策树与混合架构建议）；扩展 AI Agent 页面的 RAG 子章节（补充切分策略、检索策略、进阶演进）；更新 LLM Wiki 页面引用链接至新 RAG 概念页和对比查询页。
