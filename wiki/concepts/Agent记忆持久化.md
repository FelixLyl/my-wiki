---
type: concept
maturity: draft
date: 2026-04-06
updated: 2026-04-06
tags: [memory, persistence, openclaw, agent, long-term-memory, token-optimization]
aliases: [长期记忆, lossless-claw, Agent记忆, 记忆持久化, 对话持久化]
sources: ["raw/ai-research-list-2026-04-06.md", "raw/articles/claude-code-source-study-github.md"]
related: ["[[知识沉淀双轨机制]]", "[[自学习复盘模式]]", "[[Prompt-Caching降本]]", "[[个人知识库编译器模式]]", "[[Claude-Code源码架构]]", "[[GBrain-世界知识脑]]"]
---

# Agent 记忆持久化

Agent 的记忆问题是当前 LLM 应用的核心痛点：每次会话重新开始，过去学到的经验、确认的上下文、用户偏好全部丢失。记忆持久化方案在技术上解决"如何记得住"这个问题，与 [[知识沉淀双轨机制]] 的"什么值得记"形成互补。

## 核心挑战

- **上下文窗口有限**：无法将所有历史对话塞入每次请求
- **跨会话断层**：新会话中 Agent 无法感知过去的决策和偏好
- **记忆膨胀**：随时间积累，原始对话记录体量越来越大，直接传入会超出 Token 预算

## lossless-claw（树状摘要方案）

OpenClaw 作者推荐方案。核心机制：
1. 对话内容持久化存入数据库（不丢失原始数据）
2. 后台异步任务自动将对话压缩成**树状摘要**
3. 新会话只传入压缩后的摘要，控制 Token 消耗
4. 需要细节时可回溯原始数据库记录

"树状摘要"的优势在于保留层级结构——近期记忆保留细节，远期记忆压缩为要点，形似人类的记忆衰减曲线。

## OpenClaw 记忆优化实践

OpenClaw 记忆优化文档涵盖多种方案：
- **文件记忆**（当前默认）：`MEMORY.md` + 每日 `memory/YYYY-MM-DD.md`，简单但受限于 Agent 主动写入习惯
- **结构化存储**：将关键实体（人、项目、决策）写入可查询的结构化格式
- **优先级摘要**：按重要性分层存储，高优先级记忆在每次会话中必然注入

## 与个人知识库的关系

[[个人知识库编译器模式]] 是记忆持久化的一个特化形态：wiki 本质上是一个由 LLM 维护的长期记忆库，以结构化文档而非原始对话的形式持久化。两者的区别在于粒度——wiki 存储提炼后的知识，lossless-claw 存储对话轨迹。

## Claude Code 的五层记忆架构

Claude Code 源码（参见 [[Claude-Code源码架构]]）实现了五层记忆架构，支持跨会话持久化。这是目前可公开研究的最完整的 Agent 记忆实现之一，将记忆从短期上下文到长期持久化分层管理，每层有不同的生命周期和访问策略。这一实现为 lossless-claw 等方案提供了工程化参考。

## GBrain：世界知识层

[[GBrain-世界知识脑]]（Garry Tan）是记忆持久化的一个完整工程实现，在 OpenClaw memory 之上增加了"世界知识层"：存储人物/公司/会议/想法等结构化知识，通过 Postgres + pgvector 实现混合检索。两者定位互补——GBrain 管外部世界知识，OpenClaw memory 管 Agent 运行状态。

## 自动化记忆维护

`self-improving-agent`（参见 [[自学习复盘模式]]）可以作为记忆持久化的上层应用：自动识别值得记住的错误和教训，触发写入记忆库的操作，而不需要人工判断。
