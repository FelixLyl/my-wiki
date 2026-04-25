---
type: concept
maturity: draft
date: 2026-04-11
updated: 2026-04-11
tags: [engineering-team, claude-code, skills, ai-coding, garry-tan, gstack, openclaw, product-methodology]
aliases: [GStack, gstack, 虚拟工程团队, Garry-Tan-Stack]
sources: ["raw/garrytan-gstack.md", "raw/openspec-superpowers-gstack-2026-04-25.md"]
related: ["[[OpenClaw-Skill生态]]", "[[Claude-Sub-Agents]]", "[[Claude-Agent-Teams]]", "[[GBrain-世界知识脑]]", "[[OpenClaw-Agent安全体系]]", "[[Claude-Code-Game-Studios]]", "[[OpenSpec]]", "[[Superpowers]]"]
---

# GStack：虚拟工程团队

GStack（garrytan/gstack）是 Y Combinator CEO Garry Tan 的开源 Claude Code 工程配置，23 个专家角色 + 8 个工具技能，全部 Markdown slash command，MIT 授权。

"I don't think I've typed like a line of code probably since December." — Andrej Karpathy，2026 年 3 月

Garry Tan 本人在过去 60 天内输出 60 万行生产代码（35% 测试），每天 1-2 万行，兼职状态下运行 YC。

## 核心设计原则：流程而非工具集

gstack 是一个 sprint 流程，不是独立工具的集合：

```
Think → Plan → Build → Review → Test → Ship → Reflect
```

每个 skill 的输出喂给下一个：`/office-hours` 写 design doc → `/plan-ceo-review` 读取 → `/plan-eng-review` 生成测试计划 → `/qa` 执行测试。没有信息孤岛，没有遗漏环节。

## 23 个专家角色（按 sprint 流程排列）

### 规划阶段

| 命令 | 专家角色 | 核心价值 |
|------|---------|---------|
| /office-hours | YC Office Hours | 6 个逼问题，重构产品定义，从"我想建日历 app"推导出"你在建个人 chief of staff AI" |
| /plan-ceo-review | CEO/Founder | 4 种模式（扩张/缩减/锁定/重做），找隐藏的 10 星产品 |
| /plan-eng-review | 工程经理 | 架构/数据流/ASCII 图/边界情况/测试矩阵，强制隐藏假设浮出水面 |
| /plan-design-review | 高级设计师 | 0-10 评分各设计维度，AI Slop 检测，交互式逐项改进 |
| /plan-devex-review | DX Lead | 开发者体验基准，TTHW 计时，竞品对比，20-45 个强制问题 |
| /autoplan | 全流程协调 | 一键运行 CEO→设计→工程评审，只上浮需要品味决策的节点 |

### 设计阶段

| 命令 | 核心价值 |
|------|---------|
| /design-shotgun | 生成 4-6 个 GPT Image mockup 变体，浏览器对比板，口味记忆迭代，直到满意 |
| /design-html | mockup → 可发布 HTML/CSS，Pretext 计算布局，30KB 零依赖，输出是可以发布的，不是 demo |

### 构建/评审阶段

| 命令 | 核心价值 |
|------|---------|
| /review | 找到通过 CI 但在生产崩溃的 bug |
| /cso | OWASP Top 10 + STRIDE 威胁模型，8/10+ 置信度门槛，17 项误报过滤 |
| /codex | OpenAI Codex CLI 第二意见，跨模型分析（Claude + OpenAI 双重评审） |
| /investigate | 铁律：无调查不修复，3 次失败后停止 |

### 测试/交付阶段

| 命令 | 核心价值 |
|------|---------|
| /qa | 真实浏览器测试，找 bug，自动生成回归测试，"/qa 让我从 6 个并行 worker 升到 12 个" |
| /ship | 同步 main、跑测试、审计覆盖率、推送、开 PR；自动引导测试框架 |
| /land-and-deploy | 合并 PR → 等 CI → 等部署 → 验证生产健康，一条命令端到端 |
| /document-release | 更新所有文档匹配最新发布，/ship 自动触发 |

## 与 OpenClaw 的集成

gstack 通过 ACP 与 OpenClaw 集成：OpenClaw 通过 `sessions_spawn(runtime=acp)` 启动 Claude Code 会话，Claude Code 内调用 gstack skill。对话触发示例：

- "Run a security audit" → OpenClaw 派发 Claude Code，执行 `/cso`
- "Build me a feature" → `/autoplan` → implement → `/ship`

ClawHub 上有 4 个纯对话版 OpenClaw Skill（无需 Claude Code 进程）：`gstack-openclaw-office-hours`、`gstack-openclaw-ceo-review`、`gstack-openclaw-investigate`、`gstack-openclaw-retro`。

## 多 Agent 并行

gstack 与 [Conductor](https://conductor.build)（并行 Claude Code 会话管理器）配合使用时，可同时运行 10-15 个 sprint：一个做规划，一个做评审，一个实现功能，一个测试 staging。流程化结构（think→plan→build→review→test→ship）是并行不混乱的关键——每个 Agent 知道自己在哪个阶段，该停在哪里等待人工决策。

这是 [[Claude-Agent-Teams]] 模式的具体落地：不是一个 Agent 做所有事，而是多个专家 Agent 各管一段流程。

## 与 [[OpenClaw-Agent安全体系]] 的关系

`/cso` 内置 OWASP Top 10 + STRIDE，是 gstack 的安全专家角色。`/careful`、`/freeze`、`/guard` 是安全护栏工具，对应 OpenClaw 的高危拦截场景。两套体系互补：gstack 关注代码安全，OpenClaw 安全体系关注 Agent 运行时安全。

## 安装

```bash
# Claude Code 个人安装
git clone --single-branch --depth 1 \
  https://github.com/garrytan/gstack.git ~/.claude/skills/gstack
cd ~/.claude/skills/gstack && ./setup

# 团队共享（写入 repo，自动更新）
./setup --team
~/.claude/skills/gstack/bin/gstack-team-init required
git add .claude/ CLAUDE.md && git commit -m "require gstack"
```

支持 8 个 AI Coding Agent：Claude Code / OpenAI Codex CLI / OpenCode / Cursor / Factory Droid / Slate / Kiro / OpenClaw。MIT 免费永久，无付费层。

## 在三件套中的位置

笨小葱的文章将 gstack 与 [[OpenSpec]]、[[Superpowers]] 并称为 AI 增强开发三件套，gstack 负责"谁来做"——角色分工与 sprint 流程，另两者分别管规格（OpenSpec）和执行纪律（Superpowers）。

典型组合链：
```
OpenSpec /opsx:propose   → 锁定需求、生成 tasks
GStack /office-hours     → 产品层 review，产出 design doc
GStack /plan-eng-review  → 工程层 review，生成测试矩阵
Superpowers brainstorming → 每任务边界案例
Superpowers subagent-dev → 子 Agent 逐任务执行
GStack /review + /qa     → 评审 + 真实浏览器测试
GStack /ship             → 同步、CI、PR 一键交付
```

三者互补而不重叠：gstack 不强制 TDD（那是 Superpowers），不管规格变更追踪（那是 OpenSpec），专注于**流程结构化和专家视角切换**。

## 同类实现：[[Claude-Code-Game-Studios]]

Donchitos 的 Claude Code Game Studios 是同一模式在游戏开发领域的垂直实现：49 个 Agent、72 个 Slash Command、12 个 Hooks、39 个文档模板，三层工作室层级（Directors → Leads → Specialists），并支持 Godot/Unity/Unreal 三套引擎专家集合。规模更大、自动化程度更高，但领域更窄。
