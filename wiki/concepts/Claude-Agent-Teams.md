---
type: concept
maturity: draft
date: 2026-04-05
updated: 2026-04-05
tags: [claude-code, agent, multi-agent, llm-engineering]
aliases: [Agent Teams, Claude多Agent协作]
sources: ["raw/AI编程.pdf"]
related: ["[[Claude-Sub-Agents]]", "[[CLAUDE-md配置方法论]]", "[[知识沉淀双轨机制]]"]
---

# Claude Agent Teams

Claude Agent Teams 是一种在复杂任务中自动组建多 Agent 协作小组的执行策略，适用于涉及多个独立模块或需要多维度审查的全栈任务。

## 核心思路

单一 Agent 对于复杂任务（如"开发登录功能"，同时涉及前端、后端、数据库）效率低且上下文容易混乱。Agent Teams 的思路是按职责分工，让不同模型承担不同角色，并行推进。

## 推荐团队配置

| 角色 | 模型 | 职责 |
|------|------|------|
| Team Lead | Opus | 统筹、架构设计、最终审查 |
| 核心成员 | Sonnet | 具体模块开发（前端/后端） |
| 辅助成员 | Haiku | 文档编写、格式检查、简单重构 |

这种"混合模型策略"在保证质量的同时控制成本——高价模型做高价值工作，低价模型做低价值工作。

## 触发条件

- 任务涉及多个独立模块
- 需要多维度审查（如安全审查 + 代码审查同时进行）
- 全栈类任务（前后端 + 数据库同时开发）

## 协作原则

成员之间通过消息系统直接沟通依赖问题（如接口定义），保持并行工作，不串行等待。

## 与 Sub Agents 的区别

Agent Teams 是**预先组建**的协作团队，适合已知需要多角色的任务。[[Claude-Sub-Agents]] 是**动态创建**的一次性 Agent，适合执行过程中遇到的隔离型子任务。两者可以嵌套使用。

## 配置方式

在 `CLAUDE.md` 中声明编排策略，并确保 `.claude/settings.json` 开启 Agent Teams 功能。参见 [[CLAUDE-md配置方法论]]。
