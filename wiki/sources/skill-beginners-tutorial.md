---
title: "Skill 保姆级入门教程"
type: source
created: 2026-06-10
updated: 2026-06-10
tags: [skill, tutorial, beginner, claude]
source: https://www.uisdc.com/skill
---

# Skill 保姆级入门教程

> **原始位置：** `raw/clippings/保姆级教程！手把手教你写出好用又安全的专属 Skill.md`
> **来源：** [优设网](https://www.uisdc.com/skill)
> **作者：** 阿真
> **剪藏日期：** 2026-06-10

## 内容摘要

这是一篇面向小白的 Claude Skill 编写教程，核心观点是：**与其到处装别人的 Skill 担心安全风险，不如自己掌握写 Skill 的方法。**

### 核心内容

1. **Skill 与提示词的区别**：提示词管"当前这一次"，Skill 管"这一类任务"。一次写好，反复调用，输出稳定。
2. **什么时候该写 Skill**：一件事最近一周做了 3 次以上，做法基本固定。
3. **Description 的重要性**：description 决定了 AI 什么时候触发 Skill。好的 description = 功能描述 + 触发关键词。
4. **Skill 的文件结构**：最小结构是 `skill-name/SKILL.md`，复杂场景可加 `scripts/`、`references/`、`assets/`。
5. **多模块骨架**：9 个关键问题（目标、触发条件、排除条件、信息收集、执行顺序、输出格式、完成标准、异常处理、参考文件）。
6. **SKILL.md 正文原则**：
   - 只写 AI 不知道的东西
   - 流程拆成步骤 + 决策分支
   - 用示例代替解释
   - 写任务指令，不堆身份设定
   - 信息分层，按需加载
   - 复杂流程加验证环节
7. **Skill 访谈式构建器**——本文最大亮点：一个元 Skill，通过分步访谈引导用户理清需求，自动生成完整的 Skill 文件包。包含 12 个结构化问题（核心意图 → 运行环境 → 输出契约），支持三种交付方式。

### 关键观点

- "别人写的 Skill 我们未必看得懂它到底干了什么"
- "自己写的 Skill 最安全，也最贴合我们的需求"
- "写好 Skill = 精准触发 + 清晰流程 + 输出契约 + 可验证标准"

## 相关页面

- [[wiki/concepts/skill-system|Skill 系统]] — 由本篇和实战手册共同支持的 Skill 概念页面
- [[wiki/sources/skill-practical-guide|Skill 实战经验手册]] — 同一主题的系统性参考
