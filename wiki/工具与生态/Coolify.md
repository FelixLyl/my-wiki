---
type: project
maturity: draft
date: 2026-04-18
updated: 2026-04-18
tags: [self-host, deployment, infrastructure, heroku, vercel, devops, docker, ssh]
aliases: [coolify, 自托管部署平台, Heroku替代]
sources: ["raw/coolify-2026-04-18.md"]
related: ["[[内部系统Agent集成]]", "[[OpenClaw-Agent安全体系]]"]
---

# Coolify

Coolify 是 Heroku/Netlify/Vercel 的开源自托管替代品，让你用自己的服务器（VPS、裸金属、树莓派等）获得云平台体验，只需 SSH 连接，无需额外配置。

## 核心价值

**无供应商锁定**：所有应用配置存储在自己的服务器上，停用 Coolify 后资源仍能继续运行。数据不经过第三方云，私有化部署合规友好。

安装只需一行：
```bash
curl -fsSL https://cdn.coollabs.io/coolify/install.sh | bash
```

## 主要功能

- 管理 VPS、裸金属、树莓派等任意 SSH 可达服务器
- 一键部署应用（Docker Compose、Dockerfile、Nixpacks）
- 自动 SSL 证书（Let's Encrypt）
- Git push 触发自动部署（GitHub/GitLab/Gitea）
- 数据库一键启动（PostgreSQL、MySQL、Redis、MongoDB 等）
- 实时日志与资源监控
- 多服务器/多团队管理

## 商业模式

开源自托管版完全免费，功能无阉割。付费 Cloud 版（app.coolify.io）提供高可用、免费邮件通知和更好支持。

推荐配置：一台 ~$5/月的 VPS 跑 Coolify 本身，其余服务器用来跑实际应用。

## 与 AI 基础设施的关联

Coolify 是 AI Agent 私有化部署的理想底座：

- 自托管 OpenClaw Gateway、Claude Code 相关服务
- 部署 [[n8n]] 工作流自动化（与 AI Agent 集成）
- 部署向量数据库（Chroma、Weaviate）、LLM 推理服务
- 管理各类 AI Agent 依赖的数据库和 API 服务

对于要求数据不出境或需要控制成本的场景，Coolify + 自有 VPS 是替代 AWS/GCP/Vercel 的可行方案。
