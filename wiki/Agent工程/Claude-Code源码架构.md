---
type: concept
maturity: draft
date: 2026-04-07
updated: 2026-04-07
tags: [claude-code, agent-architecture, source-code, bun, typescript, ink, ai-engineering]
aliases: [Claude Code Architecture, Claude Code 源码, Claude Code 架构]
sources: ["raw/articles/claude-code-source-study-github.md"]
related: ["[[CLAUDE-md配置方法论]]", "[[Prompt-Caching降本]]", "[[Agent记忆持久化]]", "[[Claude-Agent-Teams]]", "[[Claude-Sub-Agents]]"]
---

# Claude Code 源码架构

Claude Code 是 Anthropic 推出的 AI 命令行编程助手，其开源源码约 1900 个文件，是目前可公开研究的最完整的生产级 AI Agent 实现之一。luyao618 的源码分析系列（25 篇）对其进行了逐模块拆解，提供了从架构到实现细节的完整技术图谱。

## 技术栈

Claude Code 选择了 Bun + TypeScript + Ink 的组合：
- Bun 作为运行时，实现毫秒级 CLI 启动（侧效果前置、Dead Code Elimination、API 预连接）
- TypeScript 提供类型安全
- Ink（基于 React 的终端 UI 框架）驱动交互界面，通过自定义 React Reconciler 和 Yoga 布局实现 60fps 终端渲染

## 五层架构

### 1. AI 核心层

对话循环采用 AsyncGenerator 状态机驱动，实现完整的 AI 交互流程。上下文管理通过 Token 预算和 Auto-compact 机制支撑无限长度对话。System Prompt 采用分段构建策略，配合缓存边界设计优化成本（参见 [[Prompt-Caching降本]]）。Thinking 模块支持 ThinkingConfig、Effort 级别和 ultrathink 三档推理控制。

### 2. 工具与 Agent 层

工具系统基于 `buildTool()` builder 模式，支持三层条件注册。BashTool 是最复杂的单一工具（12400 行），实现了四层安全防线和沙箱执行。命令系统采用六源聚合的斜杠命令架构。Agent 系统支持从单体到多智能体协作（参见 [[Claude-Agent-Teams]]），通过 context 隔离保证子 Agent 独立性（参见 [[Claude-Sub-Agents]]）。内置 Agent 包括 Explore、Plan、Verification 三种设计模式。任务系统定义了 7 种 TaskType，配备并发执行引擎。

### 3. 安全与配置层

权限系统实现七种权限模式和 7 步决策管线。Settings 系统采用 5+1 层配置合并（参见 [[CLAUDE-md配置方法论]]），支持 MDM 集成。Hooks 系统暴露 27 个事件和 4 种 Hook 类型，允许外部扩展。Feature Flag 通过 `feature()` 函数实现编译期 DCE，同一份代码可构建两个不同产品。

### 4. 终端 UI 层

基于 Ink 框架深度定制，包含自定义 React Reconciler、Yoga 布局引擎。设计系统提供终端主题和 80+ 色彩 token。

### 5. 记忆与知识层

Memory 系统实现五层记忆架构，支持跨会话持久化（参见 [[Agent记忆持久化]]）。MCP 协议实现支持 6 种传输层和 OAuth/XAA 认证。

## 可迁移的设计模式

源码分析总结了 7 个可直接复用到其他 AI Agent 项目的设计模式，涵盖状态管理（35 行极简 Store 桥接 React 与非 React）、API 错误恢复（withRetry + 过载处理 + 流式降级）、编译期优化等方面。

## 学习价值

这份源码的独特价值在于它是真实生产级产品而非 demo。每个模块的设计决策都经过了大规模用户验证，提供了 AI Agent 工程化的参考标准。

学习路径建议：先用 [[repo-analyzer]] Skill 对仓库运行 Deep 模式分析，生成架构报告作为地图，再针对感兴趣的模块深入阅读源码。
