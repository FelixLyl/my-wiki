---
type: person
maturity: draft
date: 2026-04-06
updated: 2026-04-06
tags: [ai-researcher, llm, deep-learning, tesla, openai]
aliases: [Karpathy, Andrej Karpathy]
sources: ["raw/karpathy-llm-wiki-pattern.md", "raw/zhangpelf-wiki-compiler-v2.md"]
related: ["[[个人知识库编译器模式]]", "[[Wiki-vs-RAG]]", "[[LLM知识库三层架构]]"]
---

# Andrej Karpathy

Andrej Karpathy 是深度学习领域的研究者，曾任 OpenAI 联合创始人之一，后加入特斯拉负责自动驾驶 AI，2023 年离开特斯拉后独立从事 AI 教育与研究。他以 YouTube 系列课程（如 nanoGPT、micrograd）广为人知，受众覆盖大量自学者。

## 个人知识库理念

Karpathy 在一篇 Gist 中系统阐述了用 LLM 构建个人知识库的模式，核心主张是将 LLM 定位为 wiki 的维护者而非查询工具。

"I rarely touch the wiki directly. It's the domain of the LLM." — Andrej Karpathy

他指出 [[Wiki-vs-RAG]] 的根本差异在于积累：RAG 每次查询都从头重新发现知识，wiki 则是复利增长的持久化产物。这一理念被 [[zhangpelf/wiki-compiler]] 等项目 1:1 实现，也影响了 [[farzaa Personal Wiki Skill]] 等变体。

## 历史渊源的主张

Karpathy 将这种个人知识库实践与 Vannevar Bush 1945 年提出的 Memex 联系起来。Memex 构想了一个具有文档间关联路径的个人知识存储设备，Bush 未能解决的问题是由谁来做维护工作；Karpathy 认为 LLM 处理了这个问题。
