---
type: person
maturity: draft
date: 2026-04-06
updated: 2026-04-23
tags: [ai-researcher, llm, deep-learning, tesla, openai, coding-agent, claude-code]
aliases: [Karpathy, Andrej Karpathy]
sources: ["raw/karpathy-llm-wiki-pattern.md", "raw/zhangpelf-wiki-compiler-v2.md", "raw/karpathy-skills-forrestchang-2026-04-23.md"]
related: ["[[个人知识库编译器模式]]", "[[Wiki-vs-RAG]]", "[[LLM知识库三层架构]]", "[[Karpathy-LLM编码准则]]"]
---

# Andrej Karpathy

Andrej Karpathy 是深度学习领域的研究者，曾任 OpenAI 联合创始人之一，后加入特斯拉负责自动驾驶 AI，2023 年离开特斯拉后独立从事 AI 教育与研究。他以 YouTube 系列课程（如 nanoGPT、micrograd）广为人知，受众覆盖大量自学者。

## 个人知识库理念

Karpathy 在一篇 Gist 中系统阐述了用 LLM 构建个人知识库的模式，核心主张是将 LLM 定位为 wiki 的维护者而非查询工具。

"I rarely touch the wiki directly. It's the domain of the LLM." — Andrej Karpathy

他指出 [[Wiki-vs-RAG]] 的根本差异在于积累：RAG 每次查询都从头重新发现知识，wiki 则是复利增长的持久化产物。这一理念被 [[个人知识库编译器模式|zhangpelf/wiki-compiler]] 等项目 1:1 实现，也影响了 [[来源/farzaa-personal-wiki-skill|farzaa Personal Wiki Skill]] 等变体。

## 历史渊源的主张

Karpathy 将这种个人知识库实践与 Vannevar Bush 1945 年提出的 Memex 联系起来。Memex 构想了一个具有文档间关联路径的个人知识存储设备，Bush 未能解决的问题是由谁来做维护工作；Karpathy 认为 LLM 处理了这个问题。

## LLM 编码缺陷的系统性观察

Karpathy 在 X/Twitter 上总结了 LLM 在编码任务中的三类结构性问题：隐藏假设、过度复杂化、正交性破坏。这些观察被 forrestchang 提炼为四条可操作准则（[[Karpathy-LLM编码准则]]），封装为可安装的 Claude Code 插件。

核心论断是：LLM 天然倾向于"选一种解读静默执行"而非"暴露不确定性"，需要用行为约束文件（CLAUDE.md）从外部纠正。这与他在知识库领域的立场一致：LLM 的价值不在于单次生成，而在于通过持续约束产生可积累的产物。
