---
type: insight
maturity: draft
date: 2026-04-11
sources: ["[[Wiki-vs-RAG]]", "[[Andrej-Karpathy]]", "[[claude-code-source-study]]"]
tags: [knowledge-management, agent-architecture, memex, llm-engineering, memory]
---

# Memex 闭环：从 1945 年愿景到 2026 年工程实现

Vannevar Bush 1945 年在《As We May Think》中描述了 Memex——一台能在文档之间建立关联路径、供人类知识工作者使用的个人知识存储设备。他的愿景中有一个未解决的问题：谁来做维护？

[[Andrej-Karpathy]] 提出，LLM 处理了这个问题——LLM 可以担任 wiki 的维护者，而不只是查询工具。这是对 Memex 愿景的概念补全。

但真正有意思的是：[[Claude-Code源码架构]] 的工程实现，几乎是对这一愿景的代码级落地。

## 三个层次的对应

Claude Code 的五层 Memory 架构（CLAUDE.md / Project docs / Conversation history / Tool results / Auto-compact cache）与 Karpathy 描述的 wiki 三层架构高度同构：

| Karpathy 的 wiki 层 | Claude Code 的对应实现 |
|-------------------|--------------------|
| 持久化的 wiki 页面 | CLAUDE.md + Project docs（跨会话存活） |
| 当前对话上下文 | Conversation history（Token 预算管理） |
| 动态检索与综合 | AsyncGenerator 对话循环 + Auto-compact |

这不是巧合——Claude Code 的设计者在解决同一个问题：如何让一个 LLM Agent 的知识能跨会话积累，而不是每次重头开始。

## [[Wiki-vs-RAG]] 的工程诠释

[[Wiki-vs-RAG]] 指出 RAG 的根本缺陷是"每次都在重新发现知识"。Claude Code 的 Prompt Cache 机制是一个工程层面的直接回应：

- **KV Cache** 让频繁出现的上下文（System Prompt、CLAUDE.md）只被编码一次
- **Auto-compact** 在 Token 预算耗尽时将对话历史压缩为摘要，保留精华
- 两者合力实现了"知识积累而非每次重新发现"

Memex 的愿景是知识关联路径的持久化；Claude Code 的实现是 Token 经济学约束下的知识压缩和复用。

## 闭环成立的条件

Bush → Karpathy → Claude Code 这条线能形成闭环，依赖一个前提：**知识维护的摩擦足够低**。

Memex 失败（停留在概念层面）的原因之一是人工建立关联路径的成本过高。wiki 编译器模式降低了这一成本：LLM 自动识别关联、更新文章、维护索引。Claude Code 的 27 事件 Hooks 系统则进一步降低了"知识进入系统"的触发门槛——任何工具调用都可以成为知识入口。

当维护摩擦趋近于零，Bush 的愿景才真正落地。

## 延伸问题

- 五层 Memory 的"Auto-compact 摘要"会丢失哪类知识？与人类记忆的睡眠压缩机制有何相似之处？
- 个人 wiki 编译器是否可以借鉴 Claude Code 的 Hooks 机制，实现"任意工具调用自动触发入库"？
