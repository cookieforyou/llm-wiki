# llm-wiki 知识库

> 基于 **LLM Wiki 方法论**构建的持久化知识库。  
> 核心理念：LLM 增量构建和维护维基，人类负责策展和探索。

## 📚 知识库概述

这是一个由 LLM 维护的持久化维基系统，遵循三层架构：

| 层 | 路径 | 描述 | 读写 |
|---|------|------|------|
| **原始源** | `raw/` | 不可变的源文档（文章、PDF、剪藏等） | LLM 只读 |
| **维基** | `wiki/` | LLM 生成和维护的 markdown 页面 | LLM 读写 |
| **模式** | `CLAUDE.md` + `.claude/` | 定义结构、约定和工作流的配置文件 | 人类+LLM 共同演进 |

**核心原则：** 知识被编译一次并持续更新，而不是每次查询时重新推导。

---

## 📁 目录结构

```
llm-wiki/
│
├── 📄 CLAUDE.md                   # 🧠 模式：本文件 — 知识库宪法
├── 📄 LLM Wiki.md                 # 方法论英文原版
├── 📄 LLM Wiki 方法论.md            # 方法论中文版
│
├── 📂 .claude/
│   ├── 📄 CLAUDE.md               # Claude Code 指令（导入根 CLAUDE.md）
│   └── 📂 rules/                  # 路径范围规则（可扩展）
│
├── 📂 raw/                        # 📥 原始源（不可变）
│   ├── 📂 clippings/              # Obsidian Web Clipper 剪藏
│   ├── 📂 articles/               # 手动保存的文章
│   ├── 📂 papers/                 # 学术论文、PDF、报告
│   └── 📂 assets/                 # 图片、附件
│
└── 📂 wiki/                       # 🏗️ 维基（LLM 生成维护）
    ├── 📂 concepts/               # 概念页面
    ├── 📂 sources/                # 源摘要页面
    ├── 📂 queries/                # 查询结果/专题分析
    ├── 📄 index.md                # 📇 内容目录
    └── 📄 log.md                  # 📋 操作日志
```

---

## 📐 页面约定

### YAML Frontmatter

每个 wiki 页面必须包含：

```yaml
---
title: "页面标题"
type: concept | source | query
created: YYYY-MM-DD
updated: YYYY-MM-DD
tags: [tag1, tag2]
sources: [source-name]      # 可选：关联的源
aliases: [别名]              # 可选
---
```

### 命名规则

- **文件名**：英文小写、连字符分隔（如 `retrieval-augmented-generation.md`）
- **标题**：页面首个 H1 `#` 标题，与 frontmatter `title` 一致
- **正文语言**：中文为主，英文术语保留原文

### 内部链接

- 使用 Obsidian Wiki-link：`[[wiki/concepts/some-concept]]`
- 首次提及重要概念时立即建立链接
- 可选的显示文本：`[[wiki/concepts/概念|显示名]]`

### 标签

小写英文标签，按领域/类型/状态分类：

| 类别 | 示例 |
|------|------|
| 领域 | `ai`, `llm`, `mcp`, `psychology`, `productivity` |
| 类型 | `methodology`, `tutorial`, `reference`, `case-study` |
| 状态 | `needs-review`, `growing`, `mature` |

---

## ⚙️ 工作流

### 1. Ingest（源摄取）

当新源文件添加到 `raw/` 时：

1. **读取源** — 完整读取源文件内容
2. **讨论** — 与用户讨论关键要点，确认重点
3. **创建/更新源摘要** → `wiki/sources/<source-name>.md`
   - 元数据 + 核心论点摘要（300-500字中文）
   - 与现有知识的交叉引用和矛盾标注
4. **更新概念页面** — 创建或更新相关概念页面
5. **更新索引** → 在 `wiki/index.md` 中添加条目
6. **记录日志** → 在 `wiki/log.md` 追加记录

> 建议一次摄取一个源，获得更好的讨论质量。

### 2. Query（查询）

当用户提问时：

1. **查索引** → 读取 `wiki/index.md` 定位相关页面
2. **读页面** → 深入阅读相关 wiki 页面
3. **合成答案** → 基于维基内容，标注引用源
4. **归档** → 有价值答案存入 `wiki/queries/`，更新索引和日志

答案格式按问题类型选择：
- 简单问答 → 对话回复
- 比较分析 → 表格
- 深度分析 → 新 wiki 页面
- 演示 → Marp 幻灯片

### 3. Lint（健康检查）

定期检查：

1. 页面间矛盾声明
2. 被新源覆盖的陈旧信息
3. 孤儿页面（无入链）
4. 被提到但缺独立页面的概念
5. 缺失的交叉引用
6. 可探索的新方向和可搜索的新源

---

## 📇 索引与日志

### index.md

按类别列出所有 wiki 页面。每次变更后更新首行的"最后更新"日期。

```markdown
# wiki 索引

> 最后更新：YYYY-MM-DD
> 页面总数：N

## 概念
- [[wiki/concepts/name]] — 一句话摘要

## 源
- [[wiki/sources/name]] — 一句话摘要

## 查询
- [[wiki/queries/name]] — 一句话摘要
```

### log.md

追加记录格式。使用一致前缀方便 grep 快速检索：

```markdown
## [YYYY-MM-DD] 操作类型 | 标题

- **操作：** ingest | query | lint | update | init
- **涉及页面：** [[链接1]], [[链接2]]
- **要点：** 操作摘要
```

> 快速查看最近记录：`grep "^## \[" wiki/log.md | tail -5`

---

## 🗄️ 文件管理规则

- **`raw/` 不可变** — 源文件放入后 LLM 不得修改
- **附件路径** — Obsidian 已配置为 `raw/assets/`
- **图片** — 优先下载到本地后嵌入（`![[image.png]]`）而非 URL
- **LLM 读图限制** — LLM 无法一次读取内嵌图片；先读文本，再单独查看图片获取额外上下文

---

## 🔄 版本控制

整个 vault 是 git 仓库。维基页面有版本历史和分支能力。每次操作后不需要主动提交，但鼓励定期提交。提交信息格式：

```
docs: 操作摘要
```

---

## 🧭 快速参考

| 操作 | 对 LLM 说的话 |
|------|-------------|
| 摄取新源 | "摄取这个源" / "处理这篇新文章" |
| 提问 | "MCP 是什么？" / "帮我分析..." |
| 健康检查 | "检查维基健康" / "运行 Lint" |
| 查找源 | "帮我找关于 X 的资料" |
