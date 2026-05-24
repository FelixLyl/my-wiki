---
type: project
maturity: draft
date: 2026-05-24
updated: 2026-05-24
tags: [code-analysis, knowledge-graph, claude-code, multi-agent, interactive-dashboard, codebase-onboarding, karpathy-wiki]
aliases: [Understand-Anything, 代码库理解工具, 交互式知识图谱]
sources: ["https://github.com/Lum1104/Understand-Anything"]
related: ["[[Graphify-代码知识图谱]]", "[[LLM-Wiki桌面工具]]", "[[Claude-Code源码架构]]", "[[Karpathy-LLM编码准则]]", "[[代码仓库分析器]]", "[[GBrain-世界知识脑]]"]
---

# Understand-Anything

Understand-Anything 是一个将代码库、知识库或文档转化为交互式知识图谱的 Claude Code 插件，MIT 开源。支持 Claude Code、Codex、Cursor、Copilot、Gemini CLI、OpenClaw 等 13 个平台。

> Turn any codebase, knowledge base, or docs into an interactive knowledge graph you can explore, search, and ask questions about.

> The goal isn't a graph that wows you with how complex your codebase is — it's a graph that quietly teaches you how every piece fits together.

GitHub Trending 项目，官网：https://understand-anything.com，提供可在线交互的 Demo。

## 核心场景

**大型陌生代码库的快速上手**：加入新团队，面对 20 万行代码，不知从何下手。Understand-Anything 通过多 Agent 管道分析项目，构建包含每个文件、函数、类和依赖的知识图谱，以交互式 Dashboard 呈现，可点击、可搜索、可缩放。

## 主要功能

**代码知识图谱（/understand）**：多 Agent 管道扫描项目，提取所有文件/函数/类/依赖，构建知识图谱存至 `.understand-anything/knowledge-graph.json`。支持按架构层（API/Service/Data/UI/Utility）自动分组、颜色标记。

**交互式 Dashboard（/understand-dashboard）**：在浏览器中打开可视化图谱，节点可点击查看代码、关系和纯英文说明。

**领域视图（/understand-domain）**：切换到业务领域视图，将代码映射为真实业务流程——域（Domain）、流程（Flow）、步骤（Step）横向图展示。由专属 `domain-analyzer` Agent 驱动。

**Wiki 知识库图谱（/understand-knowledge）**：指向 Karpathy 模式的 LLM wiki，生成力导向知识图谱并带社区聚类。确定性解析器提取 `_index.md` 中的 wikilink 和分类，LLM Agent 进一步发现隐式关系、提取实体、挖掘 claim——将 wiki 变成可导航的思想图谱。

**变更影响分析（/understand-diff）**：分析当前修改影响哪些系统模块，在 commit 前看清改动的波及范围。

**对话式查询（/understand-chat）**：对代码库提问，如"payment flow 是怎么工作的"。

**自动导览（/understand-onboard）**：生成按依赖顺序排列的架构走读，按正确顺序学习代码库。按用户角色（初级开发/PM/高级用户）调整详细程度。

**12 种编程模式识别**：在 context 中自动标注泛型、闭包、装饰器等 12 种模式并附解释。

## 多 Agent 架构

`/understand` 命令编排 5 个专属 Agent（`/understand-domain` 追加第 6 个）：

| Agent | 职责 |
|-------|------|
| project-scanner | 发现文件，检测语言和框架 |
| file-analyzer | 提取函数/类/import，生成节点和边 |
| architecture-analyzer | 识别架构层 |
| tour-builder | 生成引导式学习路径 |
| graph-reviewer | 验证图谱完整性和引用完整性 |
| domain-analyzer | 提取业务域、流程和步骤 |
| article-analyzer | 从 wiki 文章提取实体、claim 和隐式关系 |

文件分析器并行执行（最多 5 并发，每批 20-30 文件），支持增量更新，只重新分析自上次运行后变更的文件。

## 静态分析 + LLM 双轨

**Tree-sitter（确定性）**：将源代码解析为具体语法树，提取结构事实：import、export、函数/类定义、调用点、继承关系。同一代码始终输出相同结果，也为增量更新提供指纹检测。

**LLM（语义）**：读取解析结构和原始代码，产出解析器无法给出的内容：纯英文摘要、标签、架构层归属、业务域映射、引导游览、语言概念标注。

这一分工使图谱在结构侧可复现（相同代码始终产生相同边），在语义侧捕获意图（文件的用途，而非仅仅它 import 了什么）。与 [[Graphify-代码知识图谱]] 的 AST + 语义双轨设计高度同构。

## 安装方式

**Claude Code**（原生）：
```
/plugin marketplace add Lum1104/Understand-Anything
/plugin install understand-anything
```

**其他平台**（macOS/Linux）：
```bash
curl -fsSL https://raw.githubusercontent.com/Lum1104/Understand-Anything/main/install.sh | bash -s <platform>
# platform: codex | openclaw | gemini | vscode | hermes | cline | kimi ...
```

**Cursor/VS Code**：克隆仓库后自动发现，无需手动安装。

## 图谱即文档（Graphs-as-Code）

图谱以 JSON 存储（`.understand-anything/knowledge-graph.json`），可以 commit 进仓库。新成员加入时直接跳过分析管道，适用于 onboarding、PR review 和文档即代码场景。大图谱（10MB+）建议用 git-lfs 追踪。启用 `/understand --auto-update` 可在每次 commit 后通过 post-commit hook 增量更新图谱。

## 与相关工具的对比

与 [[Graphify-代码知识图谱]] 的关键差异：
- Graphify 专注图谱数据本身，提供命令行查询接口，强项是论文与代码跨模态融合。
- Understand-Anything 强调**交互式 Dashboard 体验**和多 Agent 分析，额外支持业务域视图、wiki 知识库图谱、变更影响分析，定位更偏向"团队协作与 onboarding 工具"。

与 [[代码仓库分析器]] 的差异：代码仓库分析器生成静态文本架构报告（含 Mermaid 图），Understand-Anything 生成可交互的可视化图谱，并持续随代码库演化而增量更新。

`/understand-knowledge` 命令将 Karpathy wiki 模式与知识图谱结合，是 [[LLM-Wiki桌面工具]] 中"4信号知识图谱 + Louvain 社区检测"路线的一个轻量变体，区别在于直接集成于 AI 编码助手而非独立桌面应用。
