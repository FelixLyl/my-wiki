---
type: pattern
maturity: draft
date: 2026-04-06
updated: 2026-04-06
tags: [knowledge-management, rag, wiki, llm-engineering, information-architecture]
aliases: [Wiki vs RAG, RAG vs Wiki, 知识存储模式]
sources: ["raw/karpathy-llm-wiki-pattern.md", "raw/nashsu-llm-wiki-desktop-2026-04-25.md"]
related: ["[[个人知识库编译器模式]]", "[[LLM知识库三层架构]]", "[[Andrej-Karpathy]]", "[[LLM-Wiki-Desktop]]"]
---

# Wiki vs RAG：两种知识存储模式

Wiki 和 RAG（检索增强生成）是用 LLM 处理个人知识库的两种截然不同的架构选择，差异不在于技术实现，而在于**知识是否积累**。

## 核心差异

| 维度 | RAG | Wiki |
|------|-----|------|
| 查询方式 | 每次从原始文档检索相关块 | 读取 LLM 已整理的 wiki 页面 |
| 积累性 | 无——每次都在重新发现知识 | 有——交叉引用、矛盾标记已在库中 |
| 维护主体 | 不需要维护（也没有积累） | LLM 持续维护，复利增长 |
| 适用场景 | 临时性查询、大型文档库 | 长期个人知识管理 |

[[Andrej-Karpathy]] 明确指出这一差异的关键在于"持久化"：wiki 是一个持久的、复利增长的产物，交叉引用已经在那里，矛盾已经被标记，综合已经反映了你读过的一切。

## 为何选择 Wiki

RAG 没有积累的性质意味着每次问同样的问题，LLM 都要重新发现同样的知识。好的分析、发现的关联、对比结论——这些有价值的内容消失在聊天历史里。

Wiki 模式下，每增加一个来源，每问一个问题，wiki 变得更丰富。这是复利，而非线性增长。

## 索引替代嵌入

在中等规模（约 100 个来源、数百个页面）下，用 `_index.md`（每个页面一行摘要）配合 LLM 先读索引再深入的方式，效果出奇好，可以避免基于嵌入的 RAG 基础设施需求。这一发现挑战了"知识库必须用向量数据库"的默认假设。

## 混合方案：Wiki + 向量搜索

[[LLM-Wiki-Desktop]] 的实践给出了一个中间路线：以 Wiki 为主干，可选接入向量搜索（LanceDB）作为补充检索层，两者融合后召回率从 58.2% 提升到 71.4%。

这表明 Wiki 和 RAG 的对立并非绝对——Wiki 提供积累和结构，向量搜索补充语义跨越关键词盲区。RAG 在纯检索场景的优势可以叠加在 Wiki 的复利基础上。

## RAG 的适用场景

RAG 在以下场景仍有优势：
- 需要检索的文档数量远超 LLM 上下文窗口
- 知识库频繁更新，维护 wiki 成本高
- 一次性查询，不需要积累
