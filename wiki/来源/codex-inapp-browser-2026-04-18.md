---
type: source
maturity: draft
date: 2026-04-18
updated: 2026-04-18
tags: [codex, openai, in-app-browser, comment-mode, agent-ux, dom-capture]
aliases: [Codex 内嵌浏览器, Codex Comment Mode]
sources: ["raw/codex-inapp-browser-2026-04-18.md"]
related: ["[[Codex-InApp-Browser]]", "[[内部系统Agent集成]]", "[[Claude-Code源码架构]]"]
---

# Codex In-App Browser（2026-04-18）

来源：James Sun（@JamesZmSun），OpenAI Codex 团队，2026-04-18

## 核心内容

Codex 推出内嵌浏览器 + Comment Mode：用户点击页面任意元素，系统自动截图、捕获 DOM 元素，将其作为精准上下文注入下一轮对话。

解决的问题：不再需要切换浏览器、拖截图、写模糊 Prompt。

## 意义

"指哪打哪"（point and click）的交互范式，让 Agent 能直接"看到"UI 上下文，而不依赖人工描述。这是将浏览器操作与 AI 编程工作流深度融合的标志性步骤。

参见 [[Codex-InApp-Browser]] 了解技术细节与行业影响。
