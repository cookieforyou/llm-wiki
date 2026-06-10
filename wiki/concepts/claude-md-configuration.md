---
title: "CLAUDE.md 配置系统"
type: concept
created: 2026-06-10
updated: 2026-06-10
tags: [claude, configuration, memory, llm, productivity]
sources: [claude-memory-guide]
aliases: [CLAUDE.md, Claude 记忆, Auto Memory]
---

# CLAUDE.md 配置系统

Claude Code 的持久化指令配置机制，包含两个互补的记忆系统：CLAUDE.md 文件（由人类编写）和自动记忆（由 Claude 自动积累）。

## 两个记忆系统

| 维度 | CLAUDE.md 文件 | 自动记忆 |
|------|---------------|---------|
| **谁编写** | 你 | Claude |
| **内容** | 指令和规则 | 学习和模式 |
| **范围** | 项目、用户或组织 | 每个工作树 |
| **加载时机** | 每个会话 | 每个会话（前 200 行/25KB） |
| **用途** | 编码标准、工作流、架构 | 构建命令、调试见解、偏好 |

## CLAUDE.md 文件层级

| 范围 | 位置 | 目的 |
|------|------|------|
| 托管策略 | 系统级路径 | 组织范围指令 |
| 用户指令 | `~/.claude/CLAUDE.md` | 所有项目的个人偏好 |
| 项目指令 | `./.claude/CLAUDE.md` | 项目团队共享指令 |
| 本地指令 | `./CLAUDE.local.md` | 个人项目特定偏好 |

### 编写有效指令

- **大小**：目标 200 行以下
- **结构**：Markdown 标题和项目符号
- **具体性**：可验证的指令（如"使用 2 空格缩进"而非"正确格式化代码"）
- **一致性**：避免矛盾规则

## 路径范围规则 (`.claude/rules/`)

通过 YAML frontmatter 中的 `paths` 字段，将指令范围限定到特定文件类型或目录：

```yaml
---
paths:
  - "src/api/**/*.ts"
---
```

这使得指令按需加载，节省上下文空间。

## 自动记忆机制

- 默认开启
- 存储在 `~/.claude/projects/<project>/memory/`
- `MEMORY.md` 作为索引，前 200 行/25KB 在每个会话加载
- 主题文件（如 `debugging.md`）按需读取
- 可通过 `/memory` 命令查看和编辑

## 与 LLM Wiki 的关系

在 [[wiki/concepts/llm-wiki-methodology|LLM Wiki 方法论]] 中，CLAUDE.md 充当**模式层（Schema）** 的具体实现，定义了：
- 维基的结构和惯例
- 摄取、查询和健康检查的工作流
- 页面的格式规范

这正是本 NotesMatrix 知识库的搭建方式——`.claude/CLAUDE.md` 就是整个知识库的"宪法"。

## 参考源

- [[wiki/sources/claude-memory-guide|Claude 记忆机制指南]]
