---
type: project
maturity: draft
date: 2026-04-25
updated: 2026-04-25
tags: [knowledge-management, llm-wiki, desktop-app, karpathy, knowledge-graph, tauri, obsidian, ingest, rag-alternative]
aliases: [llm_wiki, nashsu/llm_wiki, LLM Wiki Desktop, llm-wiki-desktop]
sources: ["raw/nashsu-llm-wiki-desktop-2026-04-25.md"]
related: ["[[个人知识库编译器模式]]", "[[LLM知识库三层架构]]", "[[Wiki-vs-RAG]]", "[[Andrej-Karpathy]]", "[[幂等防重模式]]", "[[做梦机制]]", "[[GBrain-世界知识脑]]", "[[Claude-Mem]]"]
---

# LLM Wiki Desktop（nashsu/llm_wiki）

nashsu/llm_wiki 是 [[Andrej-Karpathy]] LLM Wiki 模式的完整桌面应用实现，将"文档自动编译为结构化知识库"从一份抽象设计文档变成可安装的跨平台应用（macOS / Windows / Linux）。

> "A personal knowledge base that builds itself."

核心主张与 RAG 的区别：知识**编译一次、持续维护**，不在每次查询时重新推导。参见 [[Wiki-vs-RAG]]。

## 忠实实现的 Karpathy 基础层

| 特性 | 说明 |
|------|------|
| 三层架构 | Raw Sources（不可变）→ Wiki（LLM 生成）→ Schema（规则） |
| 双链语法 | `[[wikilink]]` 跨引用 |
| YAML frontmatter | 每页必须 |
| index.md + log.md | 内容导航 + 操作记录 |
| Obsidian 兼容 | wiki/ 目录直接作 Obsidian vault 打开 |
| 分工原则 | Human curates, LLM maintains |

## 关键扩展

### purpose.md——意图层（新增）

原始 Karpathy 设计有 Schema（how）但没有 why。llm_wiki 新增 purpose.md，定义目标、关键问题、研究范围、演进论点。

Schema 是结构规则，purpose 是方向意图——LLM 在每次 ingest 和 query 时读取它，确保生成内容始终对准用户意图而不是通用知识整理。这是对 [[LLM知识库三层架构]] 的实质性扩展：三层变四层。

### 两步 Chain-of-Thought Ingest

原始模式 LLM 读写同步（单步），llm_wiki 拆成两次独立 LLM 调用：

**Step 1（分析）：** 关键实体、与现有 wiki 的关联、矛盾与张力、结构建议。

**Step 2（生成）：** 来源摘要页、实体/概念页（含交叉引用）、更新 index.md 和 overview.md、生成待人工 review 的项目 + Deep Research 搜索词。

两步分离的核心价值：分析不被生成约束，生成有结构化分析作为输入——质量显著提升。

其他 ingest 增强：
- **SHA256 增量缓存**：内容哈希，跳过未变文件（对应 [[幂等防重模式]]）
- **持久化队列**：崩溃恢复，失败自动重试 3 次
- **目录导入**：保留目录结构，路径作为 LLM 分类提示（"papers > energy" 帮助分类）
- **overview.md 自动更新**：每次 ingest 后重新生成全局摘要

### 4 信号知识图谱

原始模式只有 `[[wikilinks]]`，llm_wiki 构建了完整的知识图谱可视化和相关性引擎：

| 信号 | 权重 | 说明 |
|------|------|------|
| 直接链接 | ×3.0 | `[[wikilinks]]` |
| 来源重叠 | ×4.0 | 共享相同 raw source（frontmatter sources[]） |
| Adamic-Adar | ×1.5 | 共享邻居，按邻居度加权 |
| 类型亲和 | ×1.0 | 同类型页面加分 |

图可视化：sigma.js + graphology + ForceAtlas2，节点颜色按页面类型或社区，边厚度按权重。

**Louvain 社区检测**：自动发现知识聚类，独立于预定义页面类型。内聚度评分识别低凝聚力社区。

**图谱洞见**自动浮现惊喜连接（跨社区边、跨类型链接）和知识空白（孤立页面、稀疏社区）。点击洞见卡片 → 高亮图谱节点 → 一键 Deep Research。

### 多阶段查询管线

原始模式单步读取相关页面，llm_wiki 构建四阶段管线：

1. **分词搜索**：英文词切分/中文 CJK bigram，标题匹配加 10 分
2. **向量语义搜索**（可选，LanceDB）：召回率从 58.2% 提升到 71.4%
3. **图谱扩展**：top 结果为种子节点，4 信号模型，2 跳衰减
4. **预算控制**：4K→1M token 可配，60% wiki / 20% 历史 / 5% 索引 / 15% 系统

向量搜索默认关闭，独立配置。向量层与关键词+图谱结合构成 hybrid retrieval。

### 异步 Review 系统

LLM 在 ingest 时标记需人工判断的项目，预定义动作类型（Create Page / Deep Research / Skip）——通过约束防止幻觉。搜索查询在 ingest 时预生成，用户随时处理，不阻塞 ingest 流程。

### Deep Research

从图谱洞见触发时，LLM 读取 overview.md + purpose.md 生成领域专属 topic 和搜索词（而非通用关键词）。Tavily API 多查询 web 搜索 + 全文提取 → LLM 综合为 wiki research 页面 → 自动 ingest 提取实体/概念。

### Chrome Web Clipper

Manifest V3 扩展，Mozilla Readability.js 提取正文，Turndown.js HTML→Markdown。本地 HTTP API（port 19827）与桌面 App 通信。一键剪藏 → 两步 ingest 流程自动启动。

## 与其他知识库实现的对比

| 实现 | 形态 | 知识图谱 | 向量搜索 | Deep Research | 说明 |
|------|------|---------|---------|--------------|------|
| **llm_wiki** | 桌面 App | ✅ 4信号+Louvain | ✅ 可选 LanceDB | ✅ | 最完整实现 |
| [[GBrain-世界知识脑]] | Postgres+pgvector | ❌ | ✅ 原生 | ❌ | Garry Tan，brain-agent loop |
| [[Claude-Mem]] | Claude Code 插件 | ❌ | ✅ Chroma | ❌ | 跨 Session 记忆，生命周期 Hooks |
| zhangpelf/wiki-compiler | LLM Agent Skill | ❌ | ❌ | ❌ | 最接近 Karpathy 原始形态 |
| 本 Wiki（小的的知识库） | LLM Agent Skill | ❌（手动双链） | ❌ | ❌ | 主人的私有实现 |

llm_wiki 是面向**普通用户**的最重量级实现，不需要懂代码，直接下载安装即用。其 Louvain 社区检测和图谱洞见是其他实现都没有的。

## 目录结构（扩展 Karpathy 设计）

```
my-wiki/
├── purpose.md          # 方向意图（新增）
├── schema.md           # 结构规则
├── raw/sources/        # 原始文档（不可变）
├── wiki/
│   ├── index.md        # 内容目录
│   ├── log.md          # 操作历史
│   ├── overview.md     # 全局摘要（自动更新，新增）
│   ├── entities/       # 人物、组织、产品
│   ├── concepts/       # 理论、方法、技术
│   ├── sources/        # 来源摘要
│   ├── queries/        # 保存的问答（新增）
│   ├── synthesis/      # 跨来源分析（新增）
│   └── comparisons/    # 对比页（新增）
├── .obsidian/          # 自动生成
└── .llm-wiki/          # App 状态：会话、review items、配置
```

## 技术栈

Tauri v2（Rust）+ React 19 + TypeScript + shadcn/ui + Milkdown（ProseMirror）+ sigma.js + LanceDB（可选）+ react-i18next（中英文）

LLM：OpenAI / Anthropic / Google / Ollama / Custom endpoint 均支持。

## 来源

- GitHub: https://github.com/nashsu/llm_wiki（MIT）
- 基于：https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f（[[Andrej-Karpathy]]）
