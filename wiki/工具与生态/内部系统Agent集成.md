---
type: concept
maturity: draft
date: 2026-04-06
updated: 2026-04-06
tags: [internal-systems, agent, swagger, cli, browser-automation, authentication, integration]
aliases: [内部系统集成, Swagger Agent, CLI集成, 鉴权方案, bb-browser]
sources: ["raw/ai-research-list-2026-04-06.md"]
related: ["[[OpenClaw-Skill生态]]", "[[OpenClaw-Agent安全体系]]", "[[CLAUDE-md配置方法论]]", "[[n8n]]", "[[Coolify]]"]
---

# 内部系统 Agent 集成

让 AI Agent 操作内部系统是团队 AI 落地的核心路径。挑战在于内部系统通常没有公开 API，有登录鉴权门槛，且接口格式多样。调研中的方案从三个角度攻克这个问题。

## 方案一：Swagger 自动生成 Skill

**Skill + Swagger 访问内部系统**是成本最低的方案：只要内部系统有 Swagger（OpenAPI）文档，就可以让 Agent 读取文档，自动生成对应的 Skill。Skill 内封装鉴权逻辑（Bearer Token / Cookie 注入），Agent 调用 Skill 时自动携带认证信息。

流程：`Swagger 文档 → Skill 自动生成 → Agent 通过 Skill 调用 → 业务逻辑执行`

登录鉴权方案可选：
- Session Cookie 注入（适合 Web 系统）
- API Key / Bearer Token（适合 REST API）
- OAuth 代理（复杂场景）

## 方案二：CLI 化任意系统

将没有 API 的系统转换为 CLI 接口，供 Agent 调用。

| 工具 | 特点 |
|------|------|
| **OpenCLI / jackwener** | 把任意网站/Electron App 转成 CLI 接口，复用本地浏览器登录态 |
| **CLI-Anything（HKUDS）** | 将任意工具/平台转换为统一 CLI 接口，香港大学出品 |

两者思路相近，都是"把无 API 的系统变成 CLI"，差异在实现细节和支持平台。适合 Electron 类桌面应用（钉钉、飞书本地端等）。

## 方案三：浏览器登录态复用

**bb-browser**：Agent 直接复用本地浏览器的登录态操作网页，无需重新扫码或管理 Session。支持小红书等有反爬机制的平台。

**Agent-Reach**：打通 Twitter/Reddit/YouTube/GitHub/B站/小红书的读取和搜索，零 API 费用（GitHub ⭐11,208）。不需要爬虫维护，直接复用平台内容。

**ai-web-automation**：针对需要填验证码、拖动滑块的复杂网页自动化，是 bb-browser 的补充，处理高对抗性平台。

## 方案选型建议

| 场景 | 推荐方案 |
|------|---------|
| 内部系统有 Swagger 文档 | Swagger → Skill 自动生成 |
| Electron 桌面应用 | OpenCLI / CLI-Anything |
| 有反爬的公开平台 | bb-browser / Agent-Reach |
| 强验证码系统 | ai-web-automation |

## 工作流自动化中间层

[[n8n]] 是连接 AI Agent 与内部/外部系统的工作流自动化平台（400+ 集成，原生 LangChain），适合做 AI Agent 工具调用的编排中间层。搭配 [[Coolify]] 可私有化自托管，数据不出境。两者构成「AI Agent 集成基础设施」的低成本自托管方案。

## 浏览器方案的两个维度

上述浏览器方案解决的是"能进门"（登录态复用）问题。2026-04-18，OpenAI Codex 推出的内嵌浏览器 + Comment Mode 解决了另一个维度——"看得准"（UI 感知精度）：点击页面元素 → 自动截图 + DOM 捕获 → 精准上下文注入，消除欠规格 Prompt。参见 [[Codex-InApp-Browser]]。

两者互补：bb-browser 解决鉴权，Codex 内嵌浏览器解决 UI 感知精度。

## 安全注意事项

内部系统集成涉及鉴权信息（Cookie、Token），应结合 [[OpenClaw-Agent安全体系]] 中的密钥管理方案，避免鉴权信息硬编码在 Skill 文件中。
