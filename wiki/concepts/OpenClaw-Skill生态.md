---
type: concept
maturity: draft
date: 2026-04-06
updated: 2026-04-06
tags: [skills, openclaw, ecosystem, automation, skill-builder, clawhub]
aliases: [Skills库, OpenClaw Skills, Skill生态, 技能生态, clawhub]
sources: ["raw/ai-research-list-2026-04-06.md"]
related: ["[[CLAUDE-md配置方法论]]", "[[OpenClaw-Agent安全体系]]", "[[内部系统Agent集成]]", "[[自学习复盘模式]]", "[[markdown-viewer-skills]]"]
---

# OpenClaw Skill 生态

Skill 是 OpenClaw 的功能扩展单元，本质上是一段描述"如何完成某类任务"的指令文档（通常是 Markdown）。Skills 的价值在于可复用、可分发、可组合——把一次性完成的流程固化为可反复调用的能力模块。

## Skills 库建设路径

### 自动生成（skill-builder）

把手动做过的流程描述给 Agent，由 Agent 自动打包成标准格式的独立 Skill。这是 Skills 库建设的核心加速器：把人工经验转换为可复用模块，边做边积累。

### 社区来源

主要来源渠道（来自调研清单）：

| 来源 | 亮点 |
|------|------|
| **awesome-openclaw-agents** | 187 个生产可用 OpenClaw Agent 模板，覆盖 19 个类别 |
| **larksuite/cli** | 飞书官方出品，200+ 命令，含 19 个 AI Agent Skills |
| **mattpocock/skills** | 全套 AI 工程 SOP：PRD 写作/TDD/架构优化/代码质量 |
| **baoyu-skills** | 封面生成（16 种渲染风格）+ 插图生成（8 种视觉风格） |
| **Product-Manager-Skills** | 46 个 PM Skills，基于实战方法论 |
| **slavingia/skills** | 极简创业方法论 Skills，GitHub 4747⭐ |
| **skills.sh** | gh-cli、prd、prd-to-issues、sql-optimization 等工程向 Skills |

### 高价值单体 Skills

- **prd**：PRD 写作，来源 awesome-copilot
- **prd-to-issues**：PRD 自动拆分为 GitHub Issues
- **sql-optimization**：SQL 语句优化
- **scoutqa-test**：自动化测试相关
- **frontend-slides**：生成单文件 HTML 幻灯片，12 种视觉风格，零依赖
- **geo-seo-claude**：GEO-first SEO 审计，针对 ChatGPT/Claude/Perplexity 优化

## Skill 自治能力

### self-evolving-skill（技能自升级）

定期巡检已安装 Skills，通过读取报错日志自动修改代码适配新环境。这是 Skills 基础设施的自我维护机制，减少人工干预。参见 [[自学习复盘模式]]。

### skill-scanner（主动推荐）

巡检工作区所有环境和代码库，主动推荐适合当前场景的 Skills。从"人找 Skill"转变为"Skill 找人"。

## Skills 与 MCP 的关系

一个设计探索：用 Skill 封装数据库访问逻辑，替代直接使用 MCP 连数据库。优势是 Skill 可以嵌入业务语义（如"查询近30天的活跃用户"），而非暴露原始 SQL 接口。适合在 Skills 库建设有一定基础后尝试。

## 竞品生态

[[Hermes-Agent]]（Nous Research，2026年2月开源）是社区认定的 OpenClaw 首个真正竞争对手，上线不到两个月 GitHub Stars 接近三万，值得持续关注。

## 图表可视化 Skills

[[markdown-viewer-skills]] 是一个专注图表/可视化的 Skills 库（MIT 开源），15 个 Skills 覆盖 PlantUML、Mermaid、Vega、Graphviz 等 7 种渲染引擎，遵循标准 Agent Skills 格式，可作为图表能力模块的参考实现。

## 生态导航

`claw123.com`（龙虾导航）是 OpenClaw 生态的聚合导航站，定期浏览可发现新 Skills 和资源。
