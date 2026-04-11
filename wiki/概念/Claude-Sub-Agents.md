---
type: concept
maturity: draft
date: 2026-04-05
updated: 2026-04-05
tags: [claude-code, agent, sub-agent, llm-engineering]
aliases: [Sub Agents, 动态Sub Agent]
sources: ["raw/AI编程.pdf"]
related: ["[[Claude-Agent-Teams]]", "[[CLAUDE-md配置方法论]]"]
---

# Claude Sub Agents

Sub Agents 是在任务执行过程中**动态创建**的一次性 Agent，用于处理需要上下文隔离或批量重复性的子任务。

## 核心价值

主 Agent 的上下文是有限且宝贵的。当某个子任务涉及大量文件读取或复杂计算时，在主上下文里做会"污染"后续的推理质量。Sub Agent 提供了一个隔离的沙箱。

## 触发场景

**上下文隔离需求：**
- 任务涉及大量文件读取
- 复杂计算可能污染主上下文

**脏活累活：**
- 编写全套单元测试
- 大规模重命名
- 数据迁移
- 任何重复性、机械性操作

## 使用原则

创建 Sub Agent 时需明确指定其**单一职责**，完成后返回摘要报告给主 Agent，而不是全量输出。这样既保持隔离，又能将结果汇总。

## 与 Agent Teams 的关系

Sub Agents 可以在 [[Claude-Agent-Teams]] 中的任意成员内部创建，实现嵌套结构：

```
Team Lead (Opus)
├── 核心成员 A (Sonnet)
│   └── Sub Agent: 单元测试编写
└── 核心成员 B (Sonnet)
    └── Sub Agent: 数据库迁移脚本
```
