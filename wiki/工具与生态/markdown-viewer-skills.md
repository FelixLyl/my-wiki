---
type: concept
maturity: draft
date: 2026-04-11
updated: 2026-04-11
tags: [skills, diagram, visualization, plantuml, mermaid, vega, agent-skills, markdown-viewer]
aliases: [markdown-viewer/skills, 图表Skills库, AI图表生成, markdown可视化]
sources: ["raw/markdown-viewer-skills.md"]
related: ["[[OpenClaw-Skill生态]]", "[[AI内容创作工作流]]", "[[个人知识库编译器模式]]"]
---

# markdown-viewer/skills：AI Agent 图表生成 Skills 库

markdown-viewer/skills 是一套面向 AI 编程 Agent 的图表/可视化 Skills 集合，15 个 Skills 覆盖 7 种渲染引擎。核心理念是让 Agent 直接在 Markdown 中生成高质量图表，无需外部工具或人工渲染。遵循 [Agent Skills](https://agentskills.io/) 格式，安装后即可被 Claude Code、Codex、Cursor 等主流 Agent 识别。

```
npx skills add markdown-viewer/skills
```

## 技术架构：三类渲染方式

### 1. PlantUML 系（文本图表引擎）

代码围栏 ` ```plantuml ` / ` ```puml `，输出 SVG：

| Skill | 领域 |
|-------|------|
| uml | 14 种 UML 图 + 9500 mxgraph 图标，软件建模 |
| cloud | AWS/Azure/GCP/阿里云/IBM/K8s 云架构 |
| network | 网络拓扑，Cisco/Citrix 设备图标 |
| security | IAM/加密/防火墙/威胁检测/合规审计 |
| archimate | 企业架构，ArchiMate 业务/应用/技术分层 |
| bpmn | 业务流程，EIP 企业集成模式，精益价值流图 |
| data-analytics | ETL/ELT 数据管道，数仓，ML 工作流 |
| iot | IoT 设备/传感器/边缘计算/数字孪生 |

### 2. 独立渲染引擎系（各有专属代码围栏）

| Skill | 代码围栏 | 适合场景 |
|-------|---------|---------|
| mermaid | ` ```mermaid ` | 流程图/时序图/甘特图，14+ 类型，最常用 |
| vega | ` ```vega-lite ` / ` ```vega ` | 数据驱动图表：柱/线/散点/热图/雷达/词云 |
| infographic | ` ```infographic ` | 70+ 预设 YAML 模板：KPI/时间线/路线图/SWOT/漏斗 |
| canvas | ` ```canvas ` | JSON Canvas 格式空间节点图：思维导图/知识图谱 |
| graphviz | ` ```dot ` | DOT 语言复杂有向图：依赖树/包关系/调用图 |

### 3. HTML/CSS 直接嵌入系（无代码围栏）

| Skill | 模板规模 | 适合场景 |
|-------|---------|---------|
| architecture | 13 布局 × 12 风格 | 分层架构图，色彩编码，网格组件布局 |
| infocard | 13 布局 × 14 风格 | 编辑级信息卡片，杂志级排版，数据高亮 |

## 场景快速选型

| 场景 | 推荐 Skill |
|------|-----------|
| 流程图、API 序列图 | mermaid |
| 复杂状态机、类图 | uml |
| 数据可视化（柱/线/散点） | vega |
| KPI 仪表板、路线图 | infographic |
| 思维导图、知识图谱 | canvas |
| 依赖树、模块关系图 | graphviz |
| 系统分层架构图 | architecture |
| 知识摘要卡片 | infocard |
| AWS/GCP/Azure/阿里云架构 | cloud |
| 网络拓扑 LAN/WAN | network |
| 威胁模型、零信任架构 | security |
| BPMN 工作流、EIP 集成模式 | bpmn |
| ETL 管道、ML 工作流 | data-analytics |
| IoT 传感器网络、边缘计算 | iot |
| 企业架构 ArchiMate | archimate |

## Skill 文件结构

每个 Skill 包含：
- `SKILL.md`：Agent 指令，含 YAML frontmatter、关键语法规范、常见错误防范
- `examples/`：示例图（PlantUML 系）
- `references/`：语法规范参考（mermaid/canvas/graphviz/vega/infographic）
- `layouts/`：布局模板（architecture/infocard）
- `styles/`：风格模板（architecture/infocard）

## 与 OpenClaw Skill 生态的关联

该库遵循标准 Agent Skills 格式，理论上可通过 [[OpenClaw-Skill生态]] 中的 skill-builder 路径改造或适配为 OpenClaw Skill。其多引擎分层设计（PlantUML / 独立引擎 / HTML 直出）是大型 Skills 库的标准架构参考。

## 渲染依赖

图表最终渲染依赖 Markdown Viewer 扩展（https://docu.md），在 Chrome、Firefox、VS Code 中均有对应版本。脱离该扩展，图表代码围栏在普通 Markdown 渲染器中只显示为代码块。
