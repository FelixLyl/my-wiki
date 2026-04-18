---
type: project
maturity: draft
date: 2026-04-18
updated: 2026-04-18
tags: [claude-code, game-dev, multi-agent, studio, hierarchy, skills, hooks, godot, unity, unreal]
aliases: [Game Studios, 游戏工作室配置, CCGS]
sources: ["raw/claude-code-game-studios-2026-04-18.md"]
related: ["[[GStack-虚拟工程团队]]", "[[Claude-Agent-Teams]]", "[[Claude-Sub-Agents]]", "[[CLAUDE-md配置方法论]]", "[[OpenClaw-Skill生态]]"]
---

# Claude Code Game Studios

Claude Code Game Studios（CCGS）是 Donchitos 开源的 Claude Code 配置模板，把真实游戏工作室的组织层级映射到 Claude Code 的 Sub-Agent 系统中，用 49 个专职 Agent 替代单一通用助手。

## 核心问题

独自用 AI 开发游戏时，单个对话 Session 缺乏结构约束：没有设计评审阻止硬编码魔法数字，没有 QA 流程，没有人追问"这符合游戏愿景吗"。CCGS 给 AI 会话植入真实工作室的组织纪律。

## 规模

| 类别 | 数量 |
|------|------|
| Agents | 49 |
| Skills（Slash Commands） | 72 |
| Hooks | 12 |
| Rules | 11 |
| Document Templates | 39 |

## 三层组织架构

```
Tier 1 — Directors（Opus 模型）
  creative-director  technical-director  producer

Tier 2 — Department Leads（Sonnet）
  game-designer      lead-programmer    art-director
  audio-director     narrative-director qa-lead
  release-manager    localization-lead

Tier 3 — Specialists（Sonnet/Haiku）
  gameplay-programmer  engine-programmer  ai-programmer
  network-programmer   tools-programmer   ui-programmer
  systems-designer     level-designer     economy-designer
  technical-artist     sound-designer     writer
  world-builder        ux-designer        prototyper
  performance-analyst  devops-engineer    analytics-engineer
  security-engineer    qa-tester          accessibility-specialist
  live-ops-designer    community-manager
```

层级规则：垂直委派（上级→下级），水平咨询（同级参考，不强制），向上升级（问题超出范围时必须上报）。

## 引擎专家集合

| 引擎 | Lead Agent | 子专家 |
|------|-----------|--------|
| Godot 4 | godot-specialist | GDScript、Shaders、GDExtension |
| Unity | unity-specialist | DOTS/ECS、Shaders/VFX、Addressables、UI Toolkit |
| Unreal Engine 5 | unreal-specialist | GAS、Blueprints、Replication、UMG/CommonUI |

## 72 个 Slash Commands（开发全流程覆盖）

从游戏创意到发行，7 个阶段：

- **入门**：`/start` `/project-stage-detect` `/setup-engine` `/adopt`
- **设计**：`/brainstorm` `/map-systems` `/design-system` `/quick-design`
- **架构**：`/create-architecture` `/architecture-decision` `/architecture-review`
- **Stories/Sprint**：`/create-epics` `/create-stories` `/dev-story` `/sprint-plan` `/story-done`
- **评审**：`/code-review` `/design-review` `/gate-check` `/balance-check` `/tech-debt`
- **QA**：`/qa-plan` `/smoke-check` `/regression-suite` `/test-flakiness`
- **发行**：`/release-checklist` `/launch-checklist` `/changelog` `/hotfix`
- **Team 编排**（多 Agent 协作单 feature）：`/team-combat` `/team-narrative` `/team-ui` `/team-release` 等 9 个

## 12 个 Hooks（自动化校验）

在 commit、push、资源变更、Session 生命周期节点自动触发：Agent 审计追踪、gap 检测、编码规范校验。不依赖人工触发，强制自动执行质量门槛。

## 与 [[GStack-虚拟工程团队]] 的对比

| 维度 | GStack | CCGS |
|------|--------|------|
| 领域 | 通用软件工程 | 游戏开发 |
| Agent 数 | 23 | 49 |
| Skills 数 | ~8（OpenClaw）| 72 |
| 自动化 Hooks | 无 | 12 个 |
| 模板 | 无 | 39 个文档模板 |
| 引擎支持 | 无 | Godot/Unity/Unreal 三套 |

两者都是「把组织结构编码进 AI 系统」模式在不同垂直领域的落地，核心哲学相同：**角色分工和流程约束比 Prompt 规则更可靠**。参见 [[Insight-流程结构即Agent协调语言]]。

## 使用方式

```bash
git clone https://github.com/Donchitos/Claude-Code-Game-Studios.git my-game
cd my-game
claude       # 启动 Claude Code
# 输入 /start 开始
```

环境：Git + Claude Code（`npm install -g @anthropic-ai/claude-code`）；可选 jq、Python 3 用于 Hook 校验。

## 来源

- GitHub: https://github.com/Donchitos/Claude-Code-Game-Studios
- License: MIT
- `.claude/` 目录：agents/ skills/ hooks/ rules/ docs/templates/
