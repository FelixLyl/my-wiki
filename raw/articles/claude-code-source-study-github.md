# 深入 Claude Code 源码

> 来源：https://github.com/luyao618/Claude-Code-Source-Study
> 收录时间：2026-04-07

**从 Anthropic 的 AI 编程助手源码中，学会构建 AI Agent 应用的全栈技术**

Claude Code 是 Anthropic 推出的 AI 命令行编程助手，也是目前最好的 AI Coding 产品之一。它的开源源码包含约 1900 个文件，覆盖了 System Prompt 工程、多 Agent 编排、工具系统、权限安全、终端 UI 等完整技术栈。

**这是一份 25 篇、覆盖全部核心模块的深度源码分析。**

不是泛泛而谈的架构概览，而是逐文件、逐函数的拆解——每篇都精确到源码行号，附关键代码片段，并总结可迁移到你自己项目的设计模式。

## 为什么值得读？
- 🔍 真实产品，不是 demo — 从真实的生产级 AI 产品中学习
- 🏗️ 全栈覆盖 — 从编译期优化到运行时状态管理，从 Prompt Cache 到终端渲染
- 🎯 面向实战 — 每篇结尾提炼 2-3 个可直接复用的设计模式
- 📖 中文友好 — 正文中文，技术术语保留英文原文

## 目录

### Part 1: 全局架构
01. 项目全景 — 技术栈选型（Bun + TypeScript + Ink）、启动链路、模块依赖全景
02. 启动优化 — 毫秒级 CLI 启动：侧效果前置、DCE、API 预连接
03. 状态管理 — 35 行极简 Store 桥接 React 与非 React 世界

### Part 2: AI 核心
04. System Prompt 工程 — 分段构建、缓存边界、行为引导技巧
05. 对话循环 — AsyncGenerator 状态机驱动的完整 AI 交互
06. 上下文管理 — Token 预算、Auto-compact、无限对话的秘密
07. Prompt Cache — 跨模块缓存策略如何降低 API 成本
08. Thinking 与推理控制 — ThinkingConfig、Effort 级别、ultrathink

### Part 3: 工具、命令与 Agent 系统
09. 工具系统设计 — buildTool() builder 模式、三层条件注册
10. BashTool 深度剖析 — 四层安全防线、沙箱执行、12400 行的最复杂工具
11. 命令系统 — 六源聚合的斜杠命令架构
12. Agent 系统 — 从单体到多智能体协作、context 隔离
13. 内置 Agent 设计模式 — Explore/Plan/Verification 的 Prompt 设计
14. 任务系统 — 7 种 TaskType、并发执行引擎
15. MCP 协议实现 — 6 种传输层、OAuth/XAA 认证

### Part 4: 安全与工程
16. 权限系统 — 七种权限模式、7 步决策管线
17. Settings 系统 — 5+1 层配置合并、MDM 集成
18. Hooks 系统 — 27 个事件、4 种 Hook 类型
19. Feature Flag 与编译期优化 — feature() DCE、同一份代码构建两个产品
20. API 调用与错误恢复 — withRetry、过载处理、流式降级

### Part 5: 终端 UI 与知识管理
21. Ink 框架深度定制 — 自定义 React Reconciler、Yoga 布局、60fps 渲染
22. 设计系统 — 终端主题系统、80+ 色彩 token
23. Memory 系统 — 五层记忆架构、跨会话持久化

### 附录
24. Skill/Plugin 开发实战 — 自定义 Agent/Skill/Plugin 编写指南
25. 架构模式总结 — 7 个可迁移到你自己项目的设计模式

## 推荐阅读路线
- ⚡ 入门路线（7 篇）：1 → 2 → 3 → 5 → 9 → 12 → 25
- 🤖 AI 工程路线（9 篇）：1 → 3 → 4 → 5 → 6 → 8 → 9 → 12 → 13
- 📚 完整路线（25 篇）：按顺序通读

## 项目信息
- 作者：luyao618
- Stars：968
- License：MIT
