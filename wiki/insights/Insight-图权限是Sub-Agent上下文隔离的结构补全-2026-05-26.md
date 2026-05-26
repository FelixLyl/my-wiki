---
type: insight
maturity: draft
date: 2026-05-26
sources: ["[[Claude子Agent机制]]", "[[OpenClaw-Agent安全体系]]", "[[Graphify-代码知识图谱]]"]
tags: [agent, security, knowledge-graph, sub-agent, zero-trust, insight]
---

# 图权限是 Sub Agent 上下文隔离的结构补全

## 碰撞来源

三篇风马牛不相及的文章：[[Claude子Agent机制]]（每任务 fresh context，隔离脏活）、[[OpenClaw-Agent安全体系]]（零信任、最小权限）、[[Graphify-代码知识图谱]]（代码库的图结构，函数调用关系用节点+边表达）。

## 洞见

Sub Agent 的"上下文隔离"目前只解决了**认知层**的隔离（防止主上下文被污染），但没有解决**权限层**的隔离（Sub Agent 理论上可以访问整个代码库、整套工具集）。

[[Graphify-代码知识图谱|Graphify]] 的图结构提供了一个尚未被利用的基础设施：**代码库中任意一个 Sub Agent 的任务边界，可以精确地用一个"节点子图"来表达**。

具体地说：
- 一个 Sub Agent 负责"重构 `auth/` 模块的单元测试"，其权限域就是 `auth/` 相关节点及其一跳依赖
- 一个 Sub Agent 负责"数据库迁移"，其权限域就是 `db/` 节点集合及所有 `import` 该模块的调用方节点

这使得[[OpenClaw-Agent安全体系]]中"最小权限原则"从**策略声明**变成了**可计算的结构约束**：

```
Sub Agent 权限 = Graphify.subgraph(task_root_nodes, depth=N)
```

## 与当前实践的差距

[[Claude子Agent机制]] 的现有设计中，"单一职责"是靠提示词约束的——主 Agent 在 Prompt 里告诉 Sub Agent "只做这件事"。但 Prompt 是软约束，Sub Agent 依然可以读取全部上下文传入的文件、调用全部工具。

引入图权限后，可以在调用 Sub Agent 时传入的不是"全量代码库路径"，而是 Graphify 导出的"最小可执行子图"（相关文件 + 接口定义 + 类型声明），天然过滤不相关节点。

## 尚未回答的问题

1. 图权限边界应该由谁来划定？主 Agent 自动推断，还是人工在 Graphify 里预先标注？
2. 子图深度参数 `N` 如何设定？深度太浅 Sub Agent 缺少必要上下文；太深则退化为全量传入。
3. 与[[OpenClaw-Agent安全体系]]中"高危操作拦截"的关系：图权限管的是知识边界，高危操作拦截管的是行为边界，两者正交、互补，需要同时部署。

## 结构意义

这个洞见的价值在于：**安全体系（零信任）+ 上下文隔离（Sub Agent）+ 结构化代码理解（图谱），三个独立演化的方向，在"图权限"这个概念上找到了一个共同的实现锚点。**

它不是某一个工具的功能升级，而是三个工具组合出了一个目前尚不存在的能力：**可计算、可审计、可最小化的 Sub Agent 执行边界**。
