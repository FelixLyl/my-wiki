---
type: concept
maturity: draft
date: 2026-04-19
updated: 2026-04-19
tags: [repo-analyzer, skill, claude-code, openclaw, codex, architecture-analysis, subagent, mermaid, open-source]
aliases: [repo-analyzer, 开源项目架构分析, 仓库分析 Skill]
sources: ["raw/repo-analyzer-2026-04-19.md"]
related: ["[[OpenClaw-Skill生态]]", "[[Claude-Code源码架构]]", "[[Claude-Sub-Agents]]", "[[markdown-viewer-skills]]"]
---

# repo-analyzer

repo-analyzer（yzddmr6，MIT）是一个 AI coding agent skill，通过一句自然语言指令完成开源项目深度架构分析，输出带 Mermaid 图的专业级报告。兼容 Claude Code、Codex、OpenClaw 及任何支持 Skills 格式的 Agent。

## 安装

```bash
npx skills add yzddmr6/repo-analyzer
```

也支持 Plugin Marketplace 安装和手动 Git Clone。

## 核心设计理念

repo-analyzer 的核心是"Why > What"——报告聚焦设计决策背后的原因和权衡，而不是代码功能列表。这使它区别于单纯的静态分析工具。

**自适应**是另一个核心原则：报告结构不采用固定模板，而是根据每个项目的特征动态设计章节。

## 8 阶段工作流

| 阶段 | 动作 |
|------|------|
| 1. Clone & Scan | 克隆仓库，按模块统计有效代码行数 |
| 2. External Research | Web 搜索评测文章、架构讨论；爬取官网 |
| 3. Adaptive Q&A | 根据项目特征生成针对性问题（非固定检查清单） |
| 4. Dynamic Report Structure | 根据回答动态设计章节布局 |
| 5. Parallel Deep Analysis | 为每个核心模块 spawn sub-agent，并发分析 |
| 6. Cross-Validation | 跨模块验证结论，检查代码覆盖率 |
| 7. Multi-Source Fusion | 融合调研 + 模块分析 + 洞见为完整叙事 |
| 8. Final Report | 输出单个 Markdown 文件（含 Mermaid 图） |

第 5 阶段的**并行 Sub-agent 分析**是这个工作流的关键创新——将大型代码库的模块分析并发化，参见 [[Claude-Sub-Agents]]。

## 三种分析深度

| 模式 | 核心覆盖 | 适用 |
|------|---------|------|
| Quick | ≥30% | 快速了解架构 |
| Standard（默认）| ≥60% | 常规分析 |
| Deep | ≥90% | 研究每个设计决策 |

## 报告章节

- **问题背景**：解决什么问题？现有方案为何不足？
- **竞品定位**：设计哲学差异（非功能清单）
- **项目全景**：架构总览
- **深度模块分析**：Why > What，含权衡和行业对比
- **评估与启示**：优缺点诚实评估 + 可借鉴经验
- **架构图**：Mermaid 系统总览 + 各模块时序图

报告保存至 `~/repo-analyses/{project-name}-{date}/ANALYSIS_REPORT.md`。

## 触发词

自动激活条件：提到「分析项目」「分析仓库」「源码分析」「架构分析」「对比两个项目」「学习这个项目」等。

## 使用场景

- **技术选型**：对比 express vs fastify 等同类框架的设计哲学
- **代码库学习**：快速理解 ollama、ruff 等大型开源项目架构
- **架构研究**：深入研究某个设计模式的真实工程实现

## 在 Skill 生态中的定位

repo-analyzer 是 [[OpenClaw-Skill生态]] 中"代码理解"方向的代表性工具，与 [[markdown-viewer-skills]]（可视化输出）配合使用效果更佳——repo-analyzer 生成 Mermaid 图，markdown-viewer-skills 提供多引擎渲染。

它也是研究 [[Claude-Code源码架构]] 等复杂代码库的高效入口：用 repo-analyzer 先生成架构报告，再针对性深入阅读源码。
