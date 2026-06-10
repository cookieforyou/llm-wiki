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
