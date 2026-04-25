---
type: concept
maturity: draft
date: 2026-04-06
updated: 2026-04-06
tags: [knowledge-management, llm-engineering, wiki, architecture]
aliases: [知识库三层架构, Raw-Wiki-Schema, 三层知识架构]
sources: ["raw/karpathy-llm-wiki-pattern.md", "raw/nashsu-llm-wiki-desktop-2026-04-25.md"]
related: ["[[个人知识库编译器模式]]", "[[Wiki-vs-RAG]]", "[[CLAUDE-md配置方法论]]", "[[Andrej-Karpathy]]", "[[LLM-Wiki-Desktop]]"]
---

# LLM 知识库三层架构

[[Andrej-Karpathy]] 提出的 LLM 个人知识库由三层构成，每层有不同的所有者和修改权限。这一分层设计是 [[个人知识库编译器模式]] 可持续运行的基础。

## 三层结构

### 第一层：原始来源（Raw Sources）
存储你精心挑选的源文档：文章、论文、图片、数据文件。**不可变**——LLM 从中读取但从不修改。这是整个系统的真相来源（Source of Truth）。

**所有者**：人类  
**路径约定**：`raw/`

### 第二层：Wiki
LLM 生成和维护的 Markdown 文件集合：摘要、实体页面、概念页面、比较、综合。LLM 完全拥有这一层——创建页面、更新交叉引用、保持一致性。

**所有者**：LLM  
**路径约定**：`wiki/`

人类读 wiki，LLM 写 wiki。

### 第三层：Schema（配置文件）
告诉 LLM wiki 如何结构化、约定是什么、执行什么工作流的文档（如 CLAUDE.md 或 AGENTS.md）。这是让 LLM 成为有纪律的 wiki 维护者而非通用聊天机器人的关键配置。

**所有者**：人类和 LLM 共同演化  
**路径约定**：项目根目录的配置文件

## 两个特殊文件

### _index.md（内容导向）
wiki 中所有内容的目录，每个页面有链接、一行摘要和可选元数据。LLM 在每次入库时更新。查询时，LLM 先读索引找相关页面，再深入阅读。

### _log.md（时间导向）
只追加的操作记录——入库、查询、健康检查过程。每个条目以一致前缀开始（如 `## [2026-04-06] ingest | 文件名`），可用简单工具解析。

## 分层的意义

所有权边界是这套架构的核心价值：LLM 不能修改原始素材（防止幻觉污染来源），人类不需要手动维护 wiki（维护负担由 LLM 承担）。Schema 层则保证 LLM 的行为是可预期的、有纪律的。

## 第四层扩展：purpose.md（意图层）

[[LLM-Wiki-Desktop]] 在三层架构上新增了 purpose.md，定义知识库的目标、关键问题、研究范围和演进论点。LLM 在每次 ingest 和 query 时读取它，确保行为对准用户意图。

这一扩展揭示了原始三层架构的一个盲区：Schema 告诉 LLM "怎么维护"，但没有告诉它"为什么维护/维护的方向"。purpose.md 补充了方向意图这一维度。

## 与 CLAUDE.md 的关系

Schema 层在实践中通常就是 [[CLAUDE-md配置方法论]] 中的 CLAUDE.md 文件。wiki-compiler 系统依赖这个文件来约束 LLM 的行为模式，包括编译规范、防重逻辑、写作风格等。
