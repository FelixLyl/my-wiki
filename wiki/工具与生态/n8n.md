---
type: project
maturity: draft
date: 2026-04-18
updated: 2026-04-18
tags: [workflow-automation, no-code, ai-agent, langchain, integration, self-host, fair-code]
aliases: [n8n, nodemation, 工作流自动化]
sources: ["raw/n8n-2026-04-18.md"]
related: ["[[内部系统Agent集成]]", "[[Coolify]]", "[[OpenClaw-Skill生态]]"]
---

# n8n

n8n 是面向技术团队的工作流自动化平台，定位于「代码的灵活性 + no-code 的速度」。400+ 集成、900+ 现成模板、原生 AI/LangChain 支持，支持自托管。名称来自 "nodemation"（Node.js + automation）缩写。

## 核心能力

- **代码 + No-Code 混合** — 节点化可视化编排，同时支持写 JavaScript/Python 和加载 npm 包
- **AI 原生** — 内置 LangChain 节点，可构建多步骤 AI Agent 工作流，接入自有数据和模型
- **400+ 集成** — Slack、飞书、MySQL、HTTP、Webhook、Google Sheets 等
- **完全控制** — 自托管或云版本，Enterprise 版支持 SSO 和离线部署

## 快速启动

```bash
npx n8n           # 本地直接运行
# 或 Docker
docker run -it --rm --name n8n -p 5678:5678 \
  -v n8n_data:/home/node/.n8n docker.n8n.io/n8nio/n8n
```

编辑器地址：http://localhost:5678

## License 说明（重要）

n8n 使用 **Fair-code（Sustainable Use License）**，非完全开源（不是 MIT/Apache）：
- 源码可见，可自托管
- 商业使用有限制，详见 LICENSE
- 企业功能需 Enterprise License

## 在 AI Agent 体系中的定位

n8n 天然适合作为 AI Agent 的**工具调用和集成中间层**：

- 替代手写 Webhook/集成代码，快速连通飞书、钉钉、数据库、内部 API
- LangChain 节点让 AI Agent 可以在工作流中调用 LLM、做 RAG、处理文档
- 与 [[内部系统Agent集成]] 调研课题直接相关（#003 Skill + 内部系统）
- 搭配 [[Coolify]] 私有化部署，数据不出境

n8n 是「把外部世界和 AI 连起来」的管道，而 OpenClaw Skills 是「告诉 AI 怎么调用这些管道」——两者互补。
