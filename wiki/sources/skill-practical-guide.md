---
title: "Skill 实战经验手册"
type: source
created: 2026-06-10
updated: 2026-06-10
tags: [skill, advanced, best-practice, claude, anthropic]
source: https://zhuanlan.zhihu.com/p/2045872710375428908
---

# Skill 实战经验手册

> **原始位置：** `raw/clippings/如何写好 Skill：一份实战经验手册.md`
> **来源：** [知乎 - 腾讯技术工程](https://zhuanlan.zhihu.com/p/2045872710375428908)
> **作者：** jackjchou（腾讯技术工程）
> **剪藏日期：** 2026-06-10

## 内容摘要

腾讯技术工程团队编写的系统性 Skill 编写指南，整合了团队踩过的坑和 Anthropic 官方的最佳实践。

### 核心内容

1. **Skill 的本质**：结构化的 Prompt Engineering——通过标准文件格式，把分散在人脑中的领域知识、操作流程和最佳实践转化为 AI 可理解、可执行的指令集。

2. **Skill 的三层加载机制（Level 1/2/3）**：
   - Level 1（元数据）：name + description，始终驻留在 AI 上下文中
   - Level 2（正文）：SKILL.md 主体，Skill 匹配触发时加载
   - Level 3（资源）：脚本和参考资料，执行中按需读取
   - **Token 成本意识**：20 个 Skill 的 Level 1 就要消耗 1000-3000 Token

3. **编写关键技巧**：
   - Description 精准化（可做"触发评估"测试）
   - 开头说清做什么、为什么、怎么做判断
   - 祈使句下指令 + 解释"为什么"
   - Before/After 对比（注释标注、完整文件对比、Diff 格式）
   - Few-Shot 示例（3-5 个，覆盖正常/边界/错误）
   - 可视化表达（表格、ASCII 流程图、决策树）

4. **模块化设计**：超过 500 行 → 拆分为主 Skill + 子 Skill，每个子 Skill 可独立使用，主 Skill 负责编排。

5. **MCP vs HTTP**：MCP 管连接，Skill 管流程，HTTP 脚本兜底。MCP 适合高频复用服务，HTTP 适合一次性简单调用。

6. **安全规范**：不硬编码密钥、危险操作加确认、数据库操作先备份、防范 Prompt 注入。

7. **Skill Creator 与工程化评估**：Anthropic 官方的"写 Skill 的 Skill"，新增了触发评估和效果评估能力，让 Skill 质量有数据可依。

8. **调试与排错**：70% 以上的问题出在"Skill 没加载"或"Description 不够好"。

9. **反模式**（六种常见）：
   - 大杂烩 Skill（一个干三件事）
   - Description 写内部黑话
   - 只有指令没有示例
   - 步骤之间没有验证点
   - 写死具体数值
   - SKILL.md 当 Wiki 写

### 关键观点

- "Skill 不是免费的——每加载一个 Skill 都会占用上下文窗口"
- "一个 Skill 只管一件事"
- "Rule 是底线，Skill 是技能"
- "Skills 应该像代码一样被 Review"

## 相关页面

- [[wiki/concepts/skill-system|Skill 系统]] — 由本篇和入门教程共同支持的 Skill 概念页面
- [[wiki/concepts/model-context-protocol|Model Context Protocol (MCP)]] — 与 Skill 的 MCP vs HTTP 选择有关
- [[wiki/sources/skill-beginners-tutorial|Skill 保姆级入门教程]] — 同一主题的入门参考
