---
type: project
maturity: draft
date: 2026-04-18
updated: 2026-04-18
tags: [claude-code, methodology, tdd, sub-agent, skills, automatic, workflow, engineering]
aliases: [superpowers, obra/superpowers, Subagent-Driven-Development]
sources: ["raw/superpowers-obra-2026-04-18.md"]
related: ["[[Claude-Sub-Agents]]", "[[Claude-Agent-Teams]]", "[[GStack-虚拟工程团队]]", "[[Claude-Code-Game-Studios]]", "[[CLAUDE-md配置方法论]]"]
---

# Superpowers

Superpowers（obra/superpowers）是 Jesse Vincent（Prime Radiant）开源的 AI Coding Agent 软件开发方法论，以可组合的 Skills 包形式发布。已上架 Anthropic 官方 Claude 插件市场。

## 核心洞见

AI Coding Agent 的默认行为是拿到需求就写代码——跳过规格澄清、忽视测试、缺乏拆分计划，导致大量返工。Superpowers 把 TDD、YAGNI、DRY、系统化调试等工程最佳实践封装成 Skills，在合适时机**自动触发**，而非靠开发者手动调用。

> 不需要做任何特殊操作。你的 coding agent 就是有了 Superpowers。

## 7 步自动化工作流

| 步骤 | Skill | 触发时机 | 作用 |
|------|-------|---------|------|
| 1 | brainstorming | 写代码前 | Socratic 对话提炼需求，分段确认设计，保存设计文档 |
| 2 | using-git-worktrees | 设计确认后 | 新分支隔离工作区，验证测试基线 |
| 3 | writing-plans | 设计确认后 | 拆分为 2-5 分钟/任务，含精确文件路径和验证步骤 |
| 4 | subagent-driven-development | 计划就绪后 | 每任务派发新子 Agent，两阶段 Review（规格+代码质量） |
| 5 | test-driven-development | 实现过程中 | 强制 RED-GREEN-REFACTOR，删除先于测试的代码 |
| 6 | requesting-code-review | 任务间 | 按严重等级报告，Critical 阻塞推进 |
| 7 | finishing-a-development-branch | 任务完成后 | 验证、merge/PR 决策、清理工作区 |

**Skills 强制执行，不是建议**。Agent 在每个任务前自动检查相关 Skills。

## Subagent-Driven Development

这是 Superpowers 的核心创新：

1. 主 Agent 负责设计和拆解计划
2. 每个任务派发**全新子 Agent**（fresh context）
3. 子 Agent 完成后，主 Agent 做两阶段 Review
4. Claude 可以自主连续工作数小时不偏离计划

本质是 [[Claude-Sub-Agents]] 在工程场景的高密度系统化应用——每个子任务都有独立上下文，避免主 Agent 上下文污染。

## Skills 全清单

**Testing：** test-driven-development

**Debugging：** systematic-debugging（4阶段根因分析）、verification-before-completion

**Collaboration：** brainstorming、writing-plans、executing-plans、dispatching-parallel-agents、requesting-code-review、receiving-code-review、using-git-worktrees、finishing-a-development-branch、subagent-driven-development

**Meta：** writing-skills、using-superpowers

## 设计哲学

TDD 优先 / 系统化流程优于临时应对 / 简洁是首要目标 / 证据优于声明

## 与同类项目的关键区别

| 项目 | 核心机制 | 自动触发 |
|------|---------|---------|
| **Superpowers** | 方法论 + 自动 Skills | ✅ |
| [[GStack-虚拟工程团队]] | 23 专家角色 sprint 流程 | ❌ 手动 |
| [[Claude-Code-Game-Studios]] | 49 Agent 层级架构 + Hooks | 部分 |
| [[The-Agency-Agents]] | 人格驱动专家库 | ❌ 手动 |

Superpowers 不靠 Agent 数量取胜，靠**自动化流程约束**。单个 Agent + 自动触发 Skills + 子 Agent 派发，比多 Agent 人工协调更流畅。

## 安装

```bash
# Claude Code（官方市场，推荐）
/plugin install superpowers@claude-plugins-official

# Claude Code（Superpowers 自有市场）
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace

# Gemini CLI
gemini extensions install https://github.com/obra/superpowers
```

还支持：Cursor、GitHub Copilot、OpenAI Codex CLI/App、OpenCode。

## 来源

- GitHub: https://github.com/obra/superpowers
- 作者：Jesse Vincent，Prime Radiant（https://primeradiant.com）
- 发布文章：https://blog.fsck.com/2025/10/09/superpowers/
- License: MIT
