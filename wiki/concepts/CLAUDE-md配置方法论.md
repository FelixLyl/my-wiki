---
type: concept
maturity: draft
date: 2026-04-05
updated: 2026-04-06
tags: [claude-code, claude-md, llm-engineering, configuration, agent-behavior]
aliases: [CLAUDE.md, Claude配置文件, claude.md行为规范]
sources: ["raw/AI编程.pdf", "raw/ai-research-list-2026-04-06.md"]
related: ["[[Claude-Agent-Teams]]", "[[Claude-Sub-Agents]]", "[[知识沉淀双轨机制]]", "[[OpenClaw-Skill生态]]"]
---

# CLAUDE.md 配置方法论

CLAUDE.md 是放置在项目根目录的配置文件，用于向 Claude Code 声明项目的工作协议。一份设计良好的 CLAUDE.md 相当于给 AI 植入了"思维逻辑"，让其从"推一下动一下"变为自主判断执行策略。

## 三层结构

### 决策层 — 编排策略

定义任务复杂度的评估标准和对应的执行模式：

| 复杂度 | 模式 | 说明 |
|--------|------|------|
| 复杂/全栈任务 | 自动组建 [[Claude-Agent-Teams]] | 多模块、多维度审查 |
| 耗时/隔离任务 | 动态创建 [[Claude-Sub-Agents]] | 上下文隔离、脏活累活 |
| 简单任务 | 直接执行 | 单文件修改、简单问答 |

### 执行层 — 技能与规范

- **Skills 优先**：执行特定任务前，先检索 `skills/` 目录下的技能文档
- **Rules 红线**：严格遵守 `rules/` 目录下的代码规范，任何代码生成必须通过 Rules 检查

### 进化层 — 知识沉淀

参见 [[知识沉淀双轨机制]]。

## 通用开发准则（建议写入）

- **代码质量**：简洁、模块化、可测试
- **错误处理**：不忽略错误，不使用 TODO 占位符，必须实现完整逻辑
- **文件操作**：修改现有文件保持原有风格；新建文件确保目录结构清晰
- **沟通风格**：专业、直接、以结果为导向，不过度解释显而易见的代码

## 使用方式

1. 将配置内容保存为项目根目录下的 `CLAUDE.md`
2. 在 `.claude/settings.json` 中开启 Agent Teams 功能
3. 用复杂任务验证：如"帮我开发一个用户积分系统，包括数据库设计和前后端接口"

## Agent 行为规范（4 条黄金准则）

来自调研素材中的高质量提示词实践，适合直接写入 CLAUDE.md：

1. **拒绝假设**：遇到不确定时，先提问而不是猜测
2. **追求最优**：不满足于"能用"，主动寻找最佳解法
3. **追根溯源**：遇到 Bug 先找根因，不打补丁
4. **极致简洁**：回复直击要点，杜绝冗余

这 4 条规范写入 CLAUDE.md 即刻生效，直接影响 Agent 输出质量，成本为零。

## 与 Skills 的关系

[[OpenClaw-Skill生态]] 中的 Skills 是 CLAUDE.md "执行层"的具体实现形式。CLAUDE.md 声明"执行某类任务时优先调用对应 Skill"，Skills 则提供具体执行能力（如 prd、prd-to-issues、sql-optimization 等）。两者配合构成完整的 AI 工程师工作台。

## 设计哲学

CLAUDE.md 的本质是**让 AI 成为项目的智能研发主管**，而非单纯的代码生成工具。它通过声明式配置，将项目的工程规范、协作模式和知识沉淀策略内化为 AI 的行为准则。
