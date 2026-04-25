# nashsu/llm_wiki — Karpathy LLM Wiki 模式的完整桌面应用实现

来源：https://github.com/nashsu/llm_wiki
收录：2026-04-25

## 核心定位

"A personal knowledge base that builds itself."

LLM Wiki 是一个跨平台桌面应用，把文档自动转化为结构化、互联的知识库。
与传统 RAG 不同：知识一次编译、持续维护，不在每次查询时重新推导。

基于 Karpathy 的 LLM Wiki pattern（https://gist.github.com/karpathy/442a6bf555914893e9891c11519read94f）
实现为完整桌面应用，并做了大量扩展。

## 架构忠实遵循 Karpathy 设计

- 三层架构：Raw Sources（不可变）→ Wiki（LLM 生成）→ Schema（规则配置）
- 三核心操作：Ingest、Query、Lint
- index.md + log.md
- [[wikilink]] 双链语法
- YAML frontmatter
- Obsidian 兼容（wiki/ 目录即 Obsidian vault）
- "Human curates, LLM maintains"分工

## 核心扩展（超出原始模式的部分）

### 1. purpose.md（新增第四层）
原始设计有 Schema（how）但没有 why。
purpose.md 定义：目标、关键问题、研究范围、演进论点。
LLM 在每次 ingest 和 query 时读取它。
本质区别：schema 是结构规则，purpose 是方向意图。

### 2. 两步 Chain-of-Thought Ingest（质量提升）
原始：LLM 读+写同时进行（单步）
扩展：拆分为两次独立 LLM 调用

Step 1（分析）：
- 关键实体、概念、论点
- 与现有 wiki 内容的关联
- 矛盾与张力
- wiki 结构建议

Step 2（生成）：
- 来源摘要页（含 frontmatter）
- 实体/概念页（含交叉引用）
- 更新 index.md、log.md、overview.md
- 需人工判断的 review items
- Deep Research 搜索查询

其他 ingest 增强：
- SHA256 增量缓存（内容哈希，跳过未变文件）
- 持久化队列（崩溃恢复，自动重试3次）
- 目录导入（保留目录结构，路径作为 LLM 分类提示）
- overview.md 自动更新（每次 ingest 后重新生成全局摘要）
- 语言感知（中英文）

### 3. 知识图谱（全新）
4信号相关性模型：
- 直接链接 ×3.0（[[wikilinks]]）
- 来源重叠 ×4.0（共享相同 raw source）
- Adamic-Adar ×1.5（共享邻居，按邻居度加权）
- 类型亲和 ×1.0（同类型页面加分）

图可视化：sigma.js + graphology + ForceAtlas2

### 4. Louvain 社区检测（全新）
自动发现知识聚类，独立于预定义页面类型。
内聚度评分（实际边/可能边密度）。
低内聚聚类标记为需关注。

### 5. 图谱洞见（全新）
惊喜连接：跨社区边、跨类型链接、外围↔核心耦合
知识空白：孤立页面（度≤1）、稀疏社区
点击洞见卡片 → 高亮图谱节点 → 一键 Deep Research

### 6. 多阶段查询管线（升级）
Phase 1：分词搜索（英文词切分+停用词/中文CJK bigram，标题匹配+10分）
Phase 1.5：向量语义搜索（可选，LanceDB，OpenAI-compatible endpoint，召回率58.2%→71.4%）
Phase 2：图谱扩展（top结果为种子节点，4信号模型，2跳衰减）
Phase 3：预算控制（4K→1M token可配，60%wiki/20%历史/5%索引/15%系统）
Phase 4：上下文组装（编号页面+完整内容，LLM按编号引用 [1][2]）

向量搜索默认关闭，可在设置中启用，独立 endpoint/key/model 配置。

### 7. Multi-conversation（升级）
独立会话、会话侧栏、每对话持久化（.llm-wiki/chats/{id}.json）
引用面板（每条回答显示使用了哪些 wiki 页面）
Save to Wiki（把有价值的回答归档到 wiki/queries/，再自动 ingest）

### 8. 异步 Review 系统（全新）
LLM 在 ingest 时标记需人工判断的项目。
预定义动作类型（Create Page / Deep Research / Skip）——约束防幻觉。
搜索查询在 ingest 时预生成。
用户随时处理，不阻塞 ingest 流程。

### 9. Deep Research（全新）
Tavily API 多查询 web 搜索 + 全文提取（无截断）。
从图谱洞见触发时：LLM 读 overview.md + purpose.md 生成领域专属 topic 和查询词。
确认对话框可编辑 topic 和搜索词。
LLM 综合结果为 wiki research 页面，含交叉引用。
结果自动 ingest 提取实体/概念。

### 10. Chrome Web Clipper（全新）
Manifest V3 扩展，Mozilla Readability.js 提取正文，Turndown.js HTML→Markdown。
本地 HTTP API（port 19827）与桌面 App 通信。
一键剪藏 → 自动 ingest 进入两步流程。

### 11. 文件格式支持
PDF（pdf-extract Rust）、DOCX（docx-rs）、PPTX（ZIP+XML）
XLSX/XLS/ODS（calamine）、图片、视频/音频

### 12. 级联删除（全新）
删除来源文件 → 自动删除对应摘要页。
3方法匹配相关 wiki 页：frontmatter sources[]、摘要页名、frontmatter 引用。
多来源共享的实体页：只移除该来源记录，不删整页。
自动清理死 [[wikilinks]]。

## 项目目录结构
```
my-wiki/
├── purpose.md          # Goals, key questions, research scope
├── schema.md           # Wiki structure rules, page types
├── raw/
│   ├── sources/        # Uploaded documents (immutable)
│   └── assets/
├── wiki/
│   ├── index.md        # Content catalog
│   ├── log.md          # Operation history
│   ├── overview.md     # Global summary (auto-updated)
│   ├── entities/       # People, organizations, products
│   ├── concepts/       # Theories, methods, techniques
│   ├── sources/        # Source summaries
│   ├── queries/        # Saved chat answers + research
│   ├── synthesis/      # Cross-source analysis
│   └── comparisons/    # Side-by-side comparisons
├── .obsidian/          # Auto-generated
└── .llm-wiki/          # App config, chats, review items
```

## 技术栈
- Desktop: Tauri v2（Rust 后端）
- Frontend: React 19 + TypeScript + Vite
- UI: shadcn/ui + Tailwind CSS v4
- Editor: Milkdown（ProseMirror WYSIWYG）
- Graph: sigma.js + graphology + ForceAtlas2
- Vector DB: LanceDB（Rust，内嵌，可选）
- i18n: react-i18next（中英文）
- State: Zustand
- LLM: OpenAI、Anthropic、Google、Ollama、Custom
- Web Search: Tavily API

## 安装
Releases 提供：macOS .dmg（Apple Silicon + Intel）、Windows .msi、Linux .deb/.AppImage
