---
type: project
maturity: draft
date: 2026-04-18
updated: 2026-04-18
tags: [agent, personality, multi-domain, claude-code, openclaw, marketing, engineering, sales, design]
aliases: [The Agency, agency-agents, AI Agency, 专家Agent库]
sources: ["raw/agency-agents-2026-04-18.md"]
related: ["[[GStack-虚拟工程团队]]", "[[Claude-Code-Game-Studios]]", "[[Claude-Agent-Teams]]", "[[OpenClaw-Skill生态]]"]
---

# The Agency — 全业务领域 AI 专家 Agent 集合

The Agency（msitarzewski/agency-agents）是一个持续增长的 AI Agent 人设集合库，以「组建梦之队」为比喻：每个 Agent 都是不知疲倦的专家，有独特的声音、工作流程和可交付成果。起源于一个 Reddit 帖子，经过数月迭代形成。

## 核心区别于通用 Prompt 模板

每个 Agent 具备四个维度：
1. **专业化** — 深耕特定领域，非通用助手
2. **人格驱动** — 独特的声音与沟通风格
3. **交付物导向** — 真实代码、流程、可量化结果
4. **生产就绪** — 经过实战验证的工作流和成功指标

## 部门结构与规模

| 部门 | Agent 数 | 覆盖领域 |
|------|---------|---------|
| Engineering | ~28 | 前端/后端/移动/AI/DevOps/安全/嵌入式/区块链 |
| Design | 8 | UI/UX/品牌/视觉叙事/AI 图像提示词 |
| Paid Media | 7 | Google/Meta/Amazon 广告、GTM 追踪 |
| Sales | 9 | Outbound/成单分析/提案/管道/客户成功 |
| Marketing | 26 | 西方平台 + 中国生态（小红书/抖音/微信/微博）+ 跨平台策略 |
| Specialized | 若干 | Product Manager、Reality Checker、Reddit Ninja 等 |

## Agent 文件格式

统一 Markdown 格式，每个文件包含：身份与性格特征、核心使命与工作流、带代码示例的交付物、成功指标与沟通风格。可直接复制到任何工具的 agents 目录使用。

## 多工具原生支持（10+ 工具）

```bash
./scripts/convert.sh          # 生成各工具格式
./scripts/install.sh          # 自动检测已安装工具并安装
./scripts/install.sh --tool openclaw   # 指定工具
./scripts/install.sh --tool claude-code
```

支持：Claude Code、OpenClaw、Cursor、Gemini CLI、OpenCode、GitHub Copilot、Antigravity、Aider、Windsurf、Kimi Code。

## 对主人工作直接相关的 Agent

- **Feishu Integration Developer** — 飞书 Open Platform 专家（Bot、工作流、API 集成）
- **Autonomous Optimization Architect** — LLM 路由 + 成本优化（与 [[Prompt-Caching降本]] 方向吻合）
- **AI Engineer** — ML 模型部署、AI 集成
- **Reality Checker** — 专门对抗 AI Slop，防止 AI 内容脱离现实

## 与同类项目的谱系定位

| 项目 | 路线 | 特点 |
|------|------|------|
| **The Agency** | 人格库（水平扩张）| 宽度最大，人格最强，工具兼容最广 |
| **[[GStack-虚拟工程团队]]** | 流程库（纵深）| 软件工程 sprint 全流程闭环 |
| **[[Claude-Code-Game-Studios]]** | 垂直层级制 | 游戏开发三层工作室架构 |

三者构成 AI Agent 团队配置的完整谱系：**人格定义 → 流程编排 → 组织层级**。

## 来源

- GitHub: https://github.com/msitarzewski/agency-agents
- License: MIT
- 来源：Reddit 帖子 + 数月社区迭代
