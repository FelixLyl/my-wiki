---
type: project
maturity: draft
date: 2026-04-22
updated: 2026-04-22
tags: [claude-code, skills, engineering, tdd, google-swe, methodology, workflow, production-grade]
aliases: [addyosmani/agent-skills, agent-skills, Addy-Osmani-Skills]
sources: ["raw/addyosmani-agent-skills-2026-04-22.md"]
related: ["[[Superpowers]]", "[[GStack-虚拟工程团队]]", "[[OpenClaw-Skill生态]]", "[[markdown-viewer-skills]]"]
---

# Agent-Skills (addyosmani)

addyosmani/agent-skills 是 Google Chrome 工程总监 Addy Osmani 开源的生产级 AI Coding Agent 工程 Skills 包。核心主张：AI Agent 默认走最短路径、跳过工程规范，Skills 把高级工程师的判断力打包成工作流，让 Agent 在每个开发阶段都有结构化流程约束。

## 核心洞见

AI coding agents 的默认行为是拿到需求就写代码——跳过规格说明、忽视测试、缺乏计划，导致原型级而非生产级的输出。agent-skills 把 Google 多年工程实践（SWE Book + 工程实践指南）封装成 Skills，让 Agent 自动遵循。

> Skills encode the workflows, quality gates, and best practices that senior engineers use when building software.

## 6 阶段开发生命周期

```
DEFINE → PLAN → BUILD → VERIFY → REVIEW → SHIP
/spec   /plan  /build   /test   /review   /ship
```

7 个 slash 命令覆盖完整生命周期（含 /code-simplify）：

| 命令 | 阶段 | 核心原则 |
|------|------|---------|
| /spec | Define | Spec before code |
| /plan | Plan | Small, atomic tasks |
| /build | Build | One slice at a time |
| /test | Verify | Tests are proof |
| /review | Review | Improve code health |
| /code-simplify | Review | Clarity over cleverness |
| /ship | Ship | Faster is safer |

## 20 个核心 Skills

### Define（定义）
- **idea-refine** — 结构化发散/收敛，把模糊想法变具体提案
- **spec-driven-development** — 写 PRD（目标/命令/结构/代码风格/测试/边界）再写代码

### Plan（计划）
- **planning-and-task-breakdown** — 把 spec 拆成可验证小任务，含接受标准和依赖排序

### Build（构建）
- **incremental-implementation** — 薄垂直切片，feature flags，回滚友好
- **test-driven-development** — Red-Green-Refactor，测试金字塔(80/15/5)，DAMP over DRY，Beyonce Rule
- **context-engineering** — 给 Agent 喂正确信息，rules files，context packing，MCP integrations
- **source-driven-development** — 每个框架决策基于官方文档，标注引用来源
- **frontend-ui-engineering** — 组件架构，设计系统，WCAG 2.1 AA 无障碍
- **api-and-interface-design** — 合约优先，Hyrum's Law，One-Version Rule

### Verify（验证）
- **browser-testing-with-devtools** — Chrome DevTools MCP，DOM/控制台/网络/性能分析
- **debugging-and-error-recovery** — 五步分诊：复现→定位→缩减→修复→防护，stop-the-line 规则

### Review（Review）
- **code-review-and-quality** — 五轴 Review，~100 行/次，严重等级标签(Nit/Optional/FYI)
- **code-simplification** — Chesterton's Fence，Rule of 500，降低复杂性保留精确行为
- **security-and-hardening** — OWASP Top 10，secrets 管理，三层边界系统
- **performance-optimization** — 先度量：Core Web Vitals，bundle 分析，反模式检测

### Ship（发布）
- **git-workflow-and-versioning** — trunk-based development，原子提交，commit-as-save-point
- **ci-cd-and-automation** — Shift Left，feature flags，quality gate pipeline
- **deprecation-and-migration** — code-as-liability，强制 vs 建议性废弃，僵尸代码清除
- **documentation-and-adrs** — Architecture Decision Records，记录"为什么"
- **shipping-and-launch** — 上线前检查清单，分阶段 rollout，监控设置

## 专家 Agent 人设（3 个）

| Agent | 角色 | 评审视角 |
|-------|------|---------|
| code-reviewer | Senior Staff Engineer | 五轴 code review，staff 级标准 |
| test-engineer | QA 专家 | 测试策略、覆盖分析 |
| security-auditor | 安全工程师 | 漏洞检测、威胁建模、OWASP |

## Skill 解剖结构

每个 SKILL.md 遵循统一格式，确保 Agent 可执行而非只能阅读：

- **Process, not prose** — 工作流，不是参考文档；每步有检查点和退出标准
- **Anti-rationalization** — 每个 skill 包含借口→反驳表（如"我以后再加测试" → "稍后永远不来"）
- **Verification is non-negotiable** — 每个 skill 以证据要求结束，"看起来对"永远不够
- **Progressive disclosure** — SKILL.md 是入口，辅助参考文档按需加载，减少 token 消耗

## Google 工程文化嵌入

Skills 融入《Software Engineering at Google》和 Google 工程实践指南的核心概念：

| 概念 | 嵌入位置 |
|------|---------|
| Hyrum's Law | api-and-interface-design |
| Beyonce Rule + 测试金字塔 | test-driven-development |
| 变更大小 + Review 速度规范 | code-review-and-quality |
| Chesterton's Fence | code-simplification |
| trunk-based development | git-workflow-and-versioning |
| Shift Left + feature flags | ci-cd-and-automation |
| code-as-liability | deprecation-and-migration |

## 与 [[Superpowers]] 的对比

两者都是 AI Coding Agent Skills 包，定位有差异：

| 维度 | agent-skills | [[Superpowers]] |
|------|-------------|----------------|
| 覆盖范围 | 完整生命周期 20 Skills | 工程执行 7 Skills |
| 作者背景 | Google 工程总监 | Prime Radiant 独立开发者 |
| 工程文化底座 | Google SWE Book | TDD/YAGNI/DRY |
| 专家 Agent | 3 个专职 persona | 无 |
| 子 Agent 派发 | 无专项 skill | 核心机制（Subagent-Driven Dev） |
| 参考资料 | 4 份独立 checklist | 无 |

两者互补：agent-skills 提供宽覆盖的工程规范框架，[[Superpowers]] 提供深度的 Agent 派发与执行约束机制。

## 安装方式

```bash
# Claude Code（推荐，官方市场）
/plugin marketplace add addyosmani/agent-skills
/plugin install agent-skills@addy-agent-skills

# Gemini CLI
gemini skills install https://github.com/addyosmani/agent-skills.git --path skills

# 本地开发
git clone https://github.com/addyosmani/agent-skills.git
claude --plugin-dir /path/to/agent-skills
```

支持：Claude Code、Cursor、Gemini CLI、Windsurf、OpenCode、GitHub Copilot、Kiro IDE & CLI

## 来源

- GitHub: https://github.com/addyosmani/agent-skills
- 作者：Addy Osmani（Google Chrome 工程总监）
- License: MIT
