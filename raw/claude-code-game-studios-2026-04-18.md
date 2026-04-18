# Claude Code Game Studios — 用 Claude Code 模拟完整游戏开发工作室

来源：GitHub README + 项目目录结构
收集日期：2026-04-18
GitHub: https://github.com/Donchitos/Claude-Code-Game-Studios
License: MIT
作者: Donchitos

---

## 核心定位

**口号：Turn a single Claude Code session into a full game development studio. 49 agents. 72 skills. One coordinated AI team.**

Claude Code Game Studios 是一个 Claude Code 配置模板项目，解决的核心问题是：**单个 AI 对话没有结构**——没人阻止你硬编码魔法数字、跳过设计文档、写意大利面代码，也没有 QA 流程和设计评审。

解决思路：把真实游戏工作室的组织结构映射到 Claude Code 的 Sub-Agent 系统，用 49 个专职 Agent 替代一个万能助手。每个 Agent 有明确职责、上报路径和质量关卡。

---

## 核心数字

| 类别 | 数量 | 说明 |
|------|------|------|
| Agents | 49 | 覆盖设计、编程、美术、音频、叙事、QA、发行 |
| Skills (Slash Commands) | 72 | `/start`、`/create-stories`、`/dev-story` 等全流程命令 |
| Hooks | 12 | commit/push/资源变更/Session 生命周期自动校验 |
| Rules | 11 | 按路径限定的编码规范（gameplay/engine/AI/UI/网络） |
| Templates | 39 | GDD、UX spec、ADR、Sprint 计划、HUD 设计等文档模板 |

---

## Studio 层级架构

```
Tier 1 — Directors（使用 Opus 模型）
  creative-director  technical-director  producer

Tier 2 — Department Leads（使用 Sonnet）
  game-designer     lead-programmer    art-director
  audio-director    narrative-director qa-lead
  release-manager   localization-lead

Tier 3 — Specialists（使用 Sonnet/Haiku）
  gameplay-programmer  engine-programmer  ai-programmer
  network-programmer   tools-programmer   ui-programmer
  systems-designer     level-designer     economy-designer
  technical-artist     sound-designer     writer
  world-builder        ux-designer        prototyper
  performance-analyst  devops-engineer    analytics-engineer
  security-engineer    qa-tester          accessibility-specialist
  live-ops-designer    community-manager
```

**引擎专家集合（三选一）：**

| 引擎 | 主 Agent | 子专家 |
|------|---------|--------|
| Godot 4 | godot-specialist | GDScript、Shaders、GDExtension |
| Unity | unity-specialist | DOTS/ECS、Shaders/VFX、Addressables、UI Toolkit |
| Unreal Engine 5 | unreal-specialist | GAS、Blueprints、Replication、UMG/CommonUI |

---

## Agent 协调机制

1. **垂直委派** — Directors → Leads → Specialists，上级委派下级，下级不能反向覆盖决策
2. **水平咨询** — 同级 Agent 可互相咨询，但不能做有约束力的决定
3. **质量关卡** — 每个里程碑和 Sprint 结束前强制触发 `/gate-check`
4. **升级路径** — 专家遇到超出范围的问题时，必须升级给对应 Lead

---

## 72 个 Slash Commands（按阶段）

**入门 & 导航**
`/start` `/help` `/project-stage-detect` `/setup-engine` `/adopt`

**游戏设计**
`/brainstorm` `/map-systems` `/design-system` `/quick-design` `/review-all-gdds` `/propagate-design-change`

**美术 & 资产**
`/art-bible` `/asset-spec` `/asset-audit`

**UX & 界面设计**
`/ux-design` `/ux-review`

**架构**
`/create-architecture` `/architecture-decision` `/architecture-review` `/create-control-manifest`

**Stories & Sprint**
`/create-epics` `/create-stories` `/dev-story` `/sprint-plan` `/sprint-status` `/story-readiness` `/story-done` `/estimate`

**评审 & 分析**
`/design-review` `/code-review` `/balance-check` `/content-audit` `/scope-check` `/perf-profile` `/tech-debt` `/gate-check` `/consistency-check`

**QA & 测试**
`/qa-plan` `/smoke-check` `/soak-test` `/regression-suite` `/test-setup` `/test-helpers` `/test-evidence-review` `/test-flakiness` `/skill-test` `/skill-improve`

**生产管理**
`/milestone-review` `/retrospective` `/bug-report` `/bug-triage` `/reverse-document` `/playtest-report`

**发行**
`/release-checklist` `/launch-checklist` `/changelog` `/patch-notes` `/hotfix`

**创意 & 内容**
`/prototype` `/onboard` `/localize`

**Team 编排**（多 Agent 协作单个 feature）
`/team-combat` `/team-narrative` `/team-ui` `/team-release` `/team-polish` `/team-audio` `/team-level` `/team-live-ops` `/team-qa`

---

## 项目文件结构

```
CLAUDE.md                       # 主配置文件
.claude/
  settings.json                 # Hooks、权限、安全规则
  agents/                       # 49 个 Agent 定义（Markdown + YAML frontmatter）
  skills/                       # 72 个 Slash Command（每个命令一个子目录）
  hooks/                        # 12 个 Hook 脚本（Bash，跨平台）
  rules/                        # 11 个路径限定的编码规范
  statusline.sh                 # 状态行（context%、模型、阶段、Epic 面包屑）
  docs/
    workflow-catalog.yaml       # 7 阶段流水线定义（供 /help 读取）
    templates/                  # 39 个文档模板
src/                            # 游戏源代码
assets/                         # 美术、音频、VFX、着色器、数据文件
design/                         # GDD、叙事文档、关卡设计
docs/                           # 技术文档和 ADR
tests/                          # 测试套件（单元、集成、性能、游戏测试）
prototypes/                     # 一次性原型（与 src/ 隔离）
production/                     # Sprint 计划、里程碑、发行追踪
```

---

## 设计哲学

与 GStack（Garry Tan 的虚拟工程团队）相同的思路，但专注游戏开发领域：
- **角色分工比 Prompt 规则更可靠** — 每个 Agent 有固定职责，而不是一个 Agent 靠规则限制
- **流程结构是协调语言** — 72 个命令覆盖从创意到发行的完整 7 阶段流水线
- **Hooks 做自动校验** — 不依赖人为触发，commit/push 自动跑质量检查
- **用户仍然掌控决策** — 所有 Agent 的目标是"提出正确问题"，而不是替代用户判断

---

## 与 GStack 的对比

| 维度 | GStack（Garry Tan） | Claude Code Game Studios |
|------|-------------------|--------------------------|
| 领域 | 通用软件工程 | 游戏开发 |
| Agent 数量 | 23 个 | 49 个 |
| Skills 数量 | ~8 个 OpenClaw Skill | 72 个 Slash Command |
| 层级深度 | 浅（Leads + Specialists） | 深（Directors + Leads + Specialists） |
| 自动化 | 无 Hooks | 12 个 Hooks |
| 引擎支持 | 无 | Godot/Unity/Unreal 三套 |
| 模板 | 无 | 39 个文档模板 |

---

## 使用方式

```bash
git clone https://github.com/Donchitos/Claude-Code-Game-Studios.git my-game
cd my-game
claude
# 在 Claude Code 中运行 /start
```

环境要求：Git + Claude Code（`npm install -g @anthropic-ai/claude-code`）；可选：jq、Python 3（用于 Hook 校验）

---

## 战略意义

这是「把组织结构编码进 AI 系统」思路的游戏领域落地版本。核心价值不在于 Agent 的智能，而在于**流程结构的约束力**——角色分工、质量关卡、升级路径这些组织制度，比任何 Prompt 规则都更难绕过。

与 [[GStack-虚拟工程团队]] 构成同一模式的不同垂直实现，共同指向一个方向：**AI 团队协作的关键是组织设计，而非模型能力**。
