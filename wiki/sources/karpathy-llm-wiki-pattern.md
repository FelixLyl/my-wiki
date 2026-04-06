---
type: source
maturity: draft
date: 2026-04-06
updated: 2026-04-06
tags: [knowledge-management, llm-engineering, wiki, karpathy]
aliases: [Karpathy Wiki Gist]
sources: ["raw/karpathy-llm-wiki-pattern.md"]
related: ["[[Andrej-Karpathy]]", "[[个人知识库编译器模式]]", "[[Wiki-vs-RAG]]", "[[LLM知识库三层架构]]"]
---

# 来源摘要：Karpathy — 用 LLM 构建个人知识库的模式

**原始来源**：https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f  
**作者**：[[Andrej-Karpathy]]  
**抓取时间**：2026-04-06

## 核心主张

[[Wiki-vs-RAG]]：不要用 RAG 临时检索，而是让 LLM 增量构建和维护持久化 wiki。wiki 是复利增长的产物，每加一个来源，知识库就更丰富。

## 关键概念

- [[LLM知识库三层架构]]：Raw（不可变来源）→ Wiki（LLM 维护）→ Schema（配置驱动行为）
- 三个工作流：Ingest（摄入）、Query（查询）、Lint（健康检查）
- 两个特殊文件：`_index.md`（内容导向目录）和 `_log.md`（时间导向追加日志）
- 中等规模下（~100 来源）用索引代替嵌入 RAG 的可行性

## 核心金句

"I rarely touch the wiki directly. It's the domain of the LLM."

## 影响

直接启发了 zhangpelf/wiki-compiler、farzaa Personal Wiki Skill 等实现。将 1945 年 Vannevar Bush 的 Memex 构想落地为可操作的 LLM 工作流。
