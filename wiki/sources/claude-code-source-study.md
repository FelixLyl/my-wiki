---
type: source
maturity: draft
date: 2026-04-07
updated: 2026-04-07
tags: [claude-code, source-code-analysis, agent-architecture, ai-engineering]
aliases: []
sources: ["raw/articles/claude-code-source-study-github.md"]
related: ["[[Claude-Code源码架构]]", "[[Prompt-Caching降本]]", "[[Agent记忆持久化]]", "[[CLAUDE-md配置方法论]]"]
---

# Claude Code Source Study（luyao618）

来源：https://github.com/luyao618/Claude-Code-Source-Study
作者：luyao618 | Stars：968 | License：MIT

25 篇中文深度源码分析，逐文件、逐函数拆解 Claude Code 的完整技术栈。每篇精确到源码行号，附关键代码片段，结尾提炼 2-3 个可迁移设计模式。

## 内容覆盖

- 全局架构：项目全景、启动优化（毫秒级 CLI）、状态管理（35 行极简 Store）
- AI 核心：System Prompt 工程、对话循环（AsyncGenerator 状态机）、上下文管理（Token 预算 + Auto-compact）、Prompt Cache、Thinking 推理控制
- 工具与 Agent：buildTool() builder 模式、BashTool 四层安全防线、六源聚合命令系统、多 Agent 协作、7 种 TaskType、MCP 协议（6 种传输层）
- 安全与工程：七种权限模式、5+1 层 Settings 合并、27 事件 Hooks 系统、Feature Flag 编译期 DCE、API 错误恢复
- 终端 UI：Ink 自定义 Reconciler、80+ 色彩 token 设计系统
- 知识管理：五层 Memory 架构、跨会话持久化

## 推荐阅读路线

- 入门（7 篇）：项目全景 → 启动优化 → 状态管理 → 对话循环 → 工具系统 → Agent 系统 → 架构模式总结
- AI 工程（9 篇）：加入 System Prompt、上下文管理、Thinking、内置 Agent 设计模式
- 完整（25 篇）：按顺序通读
