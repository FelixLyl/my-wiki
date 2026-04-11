# markdown-viewer/skills — AI Agent 图表生成 Skills 库

来源：https://github.com/markdown-viewer/skills
抓取时间：2026-04-11

## 概述

面向 AI 编程 Agent 的图表/可视化 Skills 集合，15 个 Skills 覆盖 7 种渲染引擎。
支持 Claude Code、Codex、Cursor 等主流 AI Agent。
遵循 [Agent Skills](https://agentskills.io/) 格式。

安装方式：
```
npx skills add markdown-viewer/skills
```

## 技术架构（三类）

### 1. PlantUML 系（文本图表引擎）
代码围栏：```plantuml 或 ```puml

| Skill | 用途 |
|-------|------|
| uml | 14 种 UML 图 + 9500 mxgraph 图标 |
| cloud | AWS/Azure/GCP/阿里云/IBM/OpenStack/K8s |
| network | 网络拓扑，Cisco/Citrix 设备图标 |
| security | IAM/加密/防火墙/威胁检测/合规 |
| archimate | 企业架构，ArchiMate 分层建模 |
| bpmn | 业务流程，EIP，精益价值流图 |
| data-analytics | ETL/数仓/ML 工作流 |
| iot | IoT 设备/传感器/边缘计算 |

### 2. 独立渲染引擎系

| Skill | 代码围栏 | 用途 |
|-------|---------|------|
| mermaid | ```mermaid | 流程图/时序图/甘特图，14+ 类型 |
| vega | ```vega-lite / ```vega | 数据驱动图表：柱/线/散点/热图/词云 |
| infographic | ```infographic | 70+ 预设 YAML 模板：KPI/时间线/路线图/SWOT/漏斗 |
| canvas | ```canvas | JSON Canvas 格式空间节点图：思维导图/知识图谱 |
| graphviz | ```dot | DOT 语言复杂有向/无向图：依赖树/调用图 |

### 3. HTML/CSS 直接嵌入系（无代码围栏，直接写 HTML）

| Skill | 模板数 | 用途 |
|-------|--------|------|
| architecture | 13 布局 × 12 风格 | 分层架构图，微服务，企业应用 |
| infocard | 13 布局 × 14 风格 | 编辑级信息卡片，数据高亮，活动公告 |

## Skill 文件结构

```
skills/
├── <skill-name>/
│   ├── SKILL.md        # Agent 指令（含 YAML frontmatter）
│   ├── examples/       # 示例图（PlantUML 系）
│   ├── references/     # 语法规范（mermaid/canvas/graphviz/vega/infographic）
│   ├── layouts/        # 布局模板（architecture/infocard）
│   └── styles/         # 颜色风格（architecture/infocard）
└── README.md
```

## 使用场景快速导航

| 场景 | 推荐 Skill |
|------|-----------|
| 流程图/API 序列图 | mermaid |
| 状态机（复杂） | uml |
| 柱/线/散点图 | vega |
| KPI 仪表板/路线图 | infographic |
| 思维导图/知识图谱 | canvas |
| 依赖树/模块关系 | graphviz |
| 系统分层架构 | architecture |
| 知识摘要卡片 | infocard |
| AWS/GCP/Azure 架构 | cloud |
| 网络拓扑 | network |
| 威胁模型/零信任 | security |
| UML 类图/组件图 | uml |
| BPMN 工作流 | bpmn |
| ETL/ML 流水线 | data-analytics |
| IoT 传感器网络 | iot |
| 企业架构（ArchiMate） | archimate |

## 相关链接
- Markdown Viewer 扩展：https://docu.md
- Agent Skills 格式：https://agentskills.io/
- Chrome 扩展：https://chromewebstore.google.com/detail/markdown-viewer/jekhhoflgcfoikceikgeenibinpojaoi
- Firefox 扩展：https://addons.mozilla.org/firefox/addon/markdown-viewer-extension/
- VS Code 扩展：https://marketplace.visualstudio.com/items?itemName=xicilion.markdown-viewer-extension

## 许可证
MIT
