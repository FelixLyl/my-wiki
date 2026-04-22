# addyosmani/agent-skills

> 来源: https://github.com/addyosmani/agent-skills
> 抓取时间: 2026-04-22

## 项目简介

Production-grade engineering skills for AI coding agents. 由 Addy Osmani（Google Chrome 工程总监）发布。

Skills encode the workflows, quality gates, and best practices that senior engineers use when building software. These ones are packaged so AI agents follow them consistently across every phase of development.

## 开发生命周期 6 阶段

```
DEFINE  PLAN   BUILD  VERIFY  REVIEW  SHIP
/spec  /plan  /build  /test  /review  /ship
```

7 个 slash 命令覆盖完整开发周期：

| 命令 | 阶段 | 核心原则 |
|------|------|---------|
| /spec | Define | Spec before code |
| /plan | Plan | Small, atomic tasks |
| /build | Build | One slice at a time |
| /test | Verify | Tests are proof |
| /review | Review | Improve code health |
| /code-simplify | Review | Clarity over cleverness |
| /ship | Ship | Faster is safer |

## 20 核心 Skills

### Define
- idea-refine — 结构化发散/收敛思维，把模糊想法变成具体提案
- spec-driven-development — 写 PRD（目标/命令/结构/代码风格/测试/边界）再写代码

### Plan
- planning-and-task-breakdown — 把 spec 拆成可验证的小任务，含接受标准和依赖排序

### Build
- incremental-implementation — 薄垂直切片，实现→测试→验证→提交，feature flags，回滚友好
- test-driven-development — Red-Green-Refactor，测试金字塔(80/15/5)，DAMP over DRY，Beyonce Rule
- context-engineering — 给 Agent 喂正确的信息，rules files, context packing, MCP integrations
- source-driven-development — 每个框架决策都基于官方文档，带引用来源，标记未验证内容
- frontend-ui-engineering — 组件架构，设计系统，状态管理，响应式设计，WCAG 2.1 AA
- api-and-interface-design — 合约优先设计，Hyrum's Law，One-Version Rule，错误语义，边界验证

### Verify
- browser-testing-with-devtools — Chrome DevTools MCP，DOM 检查、控制台日志、网络追踪、性能分析
- debugging-and-error-recovery — 五步分诊：复现/定位/缩减/修复/防护，stop-the-line 规则

### Review
- code-review-and-quality — 五轴 Review，~100 行变更大小，严重等级标签(Nit/Optional/FYI)
- code-simplification — Chesterton's Fence，Rule of 500，降低复杂性同时保留精确行为
- security-and-hardening — OWASP Top 10，auth 模式，secrets 管理，依赖审计，三层边界系统
- performance-optimization — 先度量：Core Web Vitals 目标，性能分析工作流，bundle 分析

### Ship
- git-workflow-and-versioning — trunk-based development，原子提交，~100 行变更大小，commit-as-save-point
- ci-cd-and-automation — Shift Left，Faster is Safer，feature flags，quality gate pipeline
- deprecation-and-migration — code-as-liability 心态，强制 vs 建议性废弃，迁移模式，僵尸代码清除
- documentation-and-adrs — Architecture Decision Records，API docs，内联文档标准，记录为什么
- shipping-and-launch — 上线前检查清单，feature flag 生命周期，分阶段 rollout，回滚程序，监控设置

## 3 个专家 Agent 人设

- code-reviewer — 高级 Staff 工程师，五轴 code review
- test-engineer — QA 专家，测试策略、覆盖分析
- security-auditor — 安全工程师，漏洞检测、威胁建模、OWASP 评估

## 4 份参考清单

- testing-patterns.md — 测试结构/命名/mocking/React/API/E2E 示例
- security-checklist.md — 提交前检查，auth，输入验证，headers，CORS，OWASP Top 10
- performance-checklist.md — Core Web Vitals 目标，前/后端清单
- accessibility-checklist.md — 键盘导航，屏幕阅读器，视觉设计，ARIA，测试工具

## Skill 解剖结构

每个 SKILL.md 包含：
- Frontmatter（name/description/use-when）
- Overview → 做什么
- When to Use → 触发条件
- Process → 分步工作流
- Rationalizations → 常见借口 + 反驳
- Red Flags → 出错信号
- Verification → 证据要求

**核心设计原则：**
- Process, not prose — 工作流，不是参考文档
- Anti-rationalization — 每个 skill 都有借口→反驳表（如"我以后再加测试"）
- Verification is non-negotiable — 每个 skill 以证据要求结束，"看起来对"永远不够
- Progressive disclosure — SKILL.md 是入口，辅助参考文档按需加载

## 工程文化背景

Skills 融入了 Google 工程文化（《Software Engineering at Google》SWE Book 和 Google 工程实践指南）：
- Hyrum's Law（API 设计）
- Beyonce Rule + 测试金字塔（测试）
- 变更大小与 Review 速度规范（Code Review）
- Chesterton's Fence（简化）
- trunk-based development（Git 工作流）
- Shift Left + feature flags（CI/CD）
- code-as-liability 心态（废弃迁移）

## 与 Superpowers 的差异

| 维度 | agent-skills (addyosmani) | Superpowers (obra) |
|------|--------------------------|-------------------|
| 覆盖范围 | 全生命周期20 Skills | 工程执行7 Skills |
| 触发方式 | 自动触发 + 手动 /命令 | 全部自动触发 |
| 专家 Agent | 3 个专职 persona | 无 |
| 工程文化 | Google SWE Book | TDD/YAGNI/DRY |
| 安装 | 市场 + 本地 | 市场 + 本地 |

## 安装方式

```bash
# Claude Code（推荐）
/plugin marketplace add addyosmani/agent-skills
/plugin install agent-skills@addy-agent-skills

# Gemini CLI
gemini skills install https://github.com/addyosmani/agent-skills.git --path skills

# 本地开发
git clone https://github.com/addyosmani/agent-skills.git
claude --plugin-dir /path/to/agent-skills
```

支持：Claude Code、Cursor、Gemini CLI、Windsurf、OpenCode、GitHub Copilot、Kiro IDE

## 项目信息

- GitHub: https://github.com/addyosmani/agent-skills
- 作者: Addy Osmani (Google Chrome 工程总监)
- License: MIT
