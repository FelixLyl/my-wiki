---
type: concept
maturity: draft
date: 2026-04-18
updated: 2026-04-18
tags: [codex, openai, in-app-browser, comment-mode, agent-ux, dom-capture, browser-automation]
aliases: [Codex 内嵌浏览器, Codex Comment Mode, 点评模式]
sources: ["raw/codex-inapp-browser-2026-04-18.md", "raw/codex-for-almost-everything-2026-04-18.md"]
related: ["[[Codex-全能助手]]", "[[Claude-Code源码架构]]", "[[内部系统Agent集成]]", "[[Superpowers]]", "[[Code-Simplifier]]"]
---

# Codex In-App Browser（内嵌浏览器 + Comment Mode）

OpenAI Codex 于 2026-04-18 推出内嵌浏览器功能，配合 Comment Mode，将"看到 UI → 精确描述 → 修改代码"的三步动作压缩为一次点击。

## 工作原理

用户在内嵌浏览器中点击页面任意元素，Codex 自动完成三件事：

1. **截图**：捕获当前页面视觉状态
2. **DOM 捕获**：定位并提取被点击的 DOM 元素（标签、属性、层级路径）
3. **上下文注入**：将截图 + DOM 信息作为精准 context 注入下一轮对话

这消除了"切换浏览器 → 手动截图 → 拖进对话框 → 文字描述问题位置"的繁琐流程。

## 解决的痛点

传统 AI 编程工作流依赖人工"翻译"：开发者需要用语言描述视觉问题（"右上角那个按钮样式不对"），而 LLM 无法直接看到。这种翻译过程引入模糊性，导致 Agent 误操作频率高。

内嵌浏览器通过结构化捕获（截图 + DOM），将视觉问题转化为机器可精确处理的输入，根本上解决了"欠规格 Prompt"（underspecified prompt）问题。

## 行业意义

这是 AI 编程工具向"Agent 感知真实 UI"演进的具体实现。与 [[内部系统Agent集成]] 中的 bb-browser / Agent-Reach 模式同属"让 Agent 直接操作浏览器"路径，区别在于：

- bb-browser 解决的是鉴权复用（Agent 复用人类登录态）
- Codex 内嵌浏览器解决的是 UI 感知精度（Agent 获得精确 DOM 上下文）

两者互补：前者解决"能进门"，后者解决"看得准"。

## 官方路线图

2026-04-18 官方博客确认：当前内嵌浏览器聚焦 localhost Web 应用；未来规划扩展至全浏览器控制（所有操作场景）。参见 [[Codex-全能助手]] 了解完整更新。

## 相关工具

- [[Codex-全能助手]] — Codex 2026 重大更新全貌，内嵌浏览器为其中一项
- [[Claude-Code源码架构]] — Claude Code 的 MCP 协议和工具系统，类似方向的 Anthropic 实现
- [[Code-Simplifier]] — Anthropic 的 Claude Code 插件，代码简化流；与 Codex 功能形成竞品关系
- [[Superpowers]] — AI Coding Agent 方法论；point-and-click Agent 指令是其"减少 Prompt 认知负担"主张的实例
