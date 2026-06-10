---
title: "Skill 系统"
type: concept
created: 2026-06-10
updated: 2026-06-10
tags: [skill, claude, anthropic, prompt-engineering, best-practice]
sources: [skill-beginners-tutorial, skill-practical-guide]
aliases: [Skill, Skills, Claude Skill, SKILL.md, 技能包]
---

# Skill 系统

Skill 是为 AI 编程助手（Claude Code、CodeBuddy、Cursor 等）添加的"能力包"——一种 **结构化的 Prompt Engineering**，通过标准文件格式将领域知识、操作流程和最佳实践转化为 AI 可理解、可执行的指令集。

## 核心概念

### Skill 与提示词的区别

| | 提示词（Prompt） | Skill |
|---|---|---|
| 范围 | 管"当前这一次" | 管"这一类任务" |
| 复用性 | 每次重新写 | 一次写好，反复调用 |
| 输出稳定性 | 不稳定 | 每次输出稳定 |
| 触发方式 | 手动输入 | AI 根据 description 自动匹配 |

### 什么时候该写 Skill

一件事最近一周做了 **3 次以上**，且做法基本固定，就值得写成 Skill。

### Skill 与 Rule 的区别

- **Rule** 是"底线"——全局约束，每次对话都自动加载（如编码规范、安全红线）
- **Skill** 是"技能"——按需触发的能力包（如迁移流程、审查模板）

## 三层加载机制（Level 1/2/3）

| 层级 | 加载时机 | 内容 | Token 成本参考 |
|------|---------|------|---------------|
| Level 1 | 常驻（每次对话） | name + description（100 字以内） | 约 50-150 Token / 个 |
| Level 2 | 匹配触发时加载 | SKILL.md 正文（建议 ≤ 500 行） | 约 2,000-5,000 Token |
| Level 3 | 执行中按需读取 | 脚本、参考文档、模板 | 按实际引用大小 |

> ⚠️ **Skill 不是免费的**——每个 Skill 的 Level 1 元数据始终占用上下文。20 个 Skill 光 Level 1 就要吃掉 1000-3000 Token。

**核心原则**：Level 1 越精准越好（决定触发时机），Level 2 越精简越好（减少 Token 消耗），Level 3 放心放（按需加载不占常驻空间）。

## 文件结构

### 最小结构
```
skill-name/
└── SKILL.md
```

### 标准结构
```
skill-name/
├── SKILL.md              # 核心指令文件（必需）
├── scripts/              # 可执行脚本（可选）
├── references/           # 参考文档（可选）
└── assets/               # 静态资源（可选）
```

### 模块化结构（复杂场景）
```
project-migration/                  # 主 Skill：流程总览与编排
├── SKILL.md
└── steps/                          # 子步骤文档
    ├── 00-environment-setup.md
    ├── 01-dependency-update.md
    └── 02-api-migration.md
```

## Description 编写

Description 是 Skill 最重要的字段——它决定了 AI 什么时候触发这个 Skill。

### 公式
```
好的 description = 功能描述 + 触发关键词
```

### 示例
```yaml
# ✅ 好的 description
description: |
  将故事文本转换为AI视频生成所需的分镜脚本。
  当用户说"做分镜"、"分镜脚本"、"故事转分镜"时触发。

# ❌ 太模糊，AI 不知道什么时候该用
description: 帮忙处理文档

# ❌ 太宽泛，什么写作任务都会触发
description: 帮助用户写文章
```

### 触发评估
一个实用技巧：自己想 20 个问题（一半该触发、一半不该触发），测试 AI 是否每次都能正确判断。

## SKILL.md 编写规范

### 多模块骨架（9 个关键问题）

1. **目标**是什么
2. **什么情况下触发**
3. **什么情况下不要触发**
4. 开始前先**收集什么信息**
5. 按什么**顺序**干活
6. **输出**必须长什么样
7. 做到什么程度算**完成**
8. 搞不定的**时候怎么处理**
9. 什么时候去**读参考文件**

### 正文编写原则

1. **只写 AI 不知道的东西**——不需要教 AI 已知的概念
2. **流程拆成步骤，带上决策分支**——"如果 XX 情况，如何处理"
3. **用示例代替解释**——一个正例 + 一个反例胜过长篇说明
4. **写任务指令，不要堆身份设定**——"你是一个资深 XX 专家"效果不稳定
5. **信息分层，按需加载**——核心规则放主文件，参考资料放独立文件
6. **复杂流程加验证环节**——关键步骤后插入检查点
7. **先跑起来，再慢慢打磨**——很难一次做到完美

### 示例对比的三种方式

```
方式一：注释标注（适合简单变更）
// Before
import oldhttp "github.com/example/old-http-client"
// After
import uhttp "github.com/example/unified-httpclient"

方式二：完整文件对比（适合复杂变更）
...（完整的 Before/After 代码块）

方式三：Diff 格式（最直观）
--- a/pkg/request/client.go
+++ b/pkg/request/client.go
@@ -3,7 +3,7 @@
-import oldhttp "github.com/example/old-http-client"
+import uhttp "github.com/example/unified-httpclient"
```

### 安全性规范

- 绝不硬编码敏感信息（使用环境变量）
- 危险操作必须加确认
- 数据库操作先备份再变更
- 防范 Prompt 注入——区分"指令"和"数据"
- 网络请求使用 HTTPS 并设置超时

## 常用反模式

| 反模式 | 症状 | 解法 |
|--------|------|------|
| 大杂烩 Skill | 一个 Skill 干三件事 | 拆分 |
| Description 写黑话 | 全是内部术语 | 通用语言 + 技术关键词 |
| 没有示例 | 纯文字描述 | 加 Few-Shot |
| 没有验证点 | 做完才检查 | 关键步骤加检查点 |
| 写死数值 | 硬编码配置 | 给判断规则和参考范围 |
| 当 Wiki 写 | 背景 300 行正文 50 行 | 背景放 references/ |

## 生命周期管理

- **版本控制**：metadata 中标版本号，语义化版本（修复→1.1，新增→1.2，重写→2.0）
- **跨项目复用**：从项目级同步到用户级 `~/.claude/skills/`
- **团队协作**：像代码一样 PR Review，维护 CHANGELOG
- **持续优化**：从具体反馈中总结规律，越精简越好

## 工程化评估

Anthropic Skill Creator 提供系统化评估：

1. **触发评估**：自动生成正例/反例/边界用例，计算触发准确率和召回率
2. **效果评估**：基于测试用例，对比"有 Skill"和"无 Skill"的输出质量
3. **达标标准**：触发准确率 ≥ 85%，效果通过率 ≥ 80%

## 与 MCP 的关系

Skill 需要调用外部服务时有两种选择：

| | MCP | HTTP 直接调用 |
|---|-----|-------------|
| 定位 | AI Agent 的标准化工具协议 | 通用网络通信协议 |
| 适用 | 高频复用、多平台共享、需统一鉴权 | 一次性简单调用、对接老系统 |
| 写法 | Skill 中只说"做什么"，MCP 负责连接 | 在脚本中写完整请求代码 |

**最佳实践**：MCP 管连接，Skill 管流程，HTTP 脚本兜底。

## 相关概念

- [[wiki/concepts/claude-md-configuration|CLAUDE.md 配置系统]] — Skill 的配置文件体系
- [[wiki/concepts/model-context-protocol|Model Context Protocol (MCP)]] — Skill 外部服务调用的标准化方式
- [[wiki/concepts/llm-wiki-methodology|LLM Wiki 方法论]] — 本知识库的方法论基础

## 参考源

- [[wiki/sources/skill-beginners-tutorial|Skill 保姆级入门教程]]
- [[wiki/sources/skill-practical-guide|Skill 实战经验手册]]
