---
type: project
maturity: draft
date: 2026-04-18
updated: 2026-04-18
tags: [memory, claude-code, persistence, hooks, mcp, sqlite, vector-search, openclaw, session]
aliases: [claude-mem, thedotmack/claude-mem, 跨Session记忆]
sources: ["raw/claude-mem-2026-04-18.md"]
related: ["[[Agent记忆持久化]]", "[[GBrain-世界知识脑]]", "[[Prompt-Caching降本]]", "[[Claude-Code源码架构]]"]
---

# Claude-Mem

Claude-Mem（thedotmack/claude-mem）是专为 Claude Code 构建的跨 Session 持久化记忆压缩系统，当前版本 6.5.0，License AGPL 3.0。通过 Claude Code 生命周期 Hooks 自动捕获工具调用观测值，生成语义摘要，在新 Session 中按需注入历史上下文。

## 解决的问题

Claude Code 每次新 Session 从零开始，无法记住上一次的操作、遇到的问题、已做的决策。Claude-Mem 在工具调用（PostToolUse）和 Session 结束（SessionEnd）时自动写入观测记录，使 Agent 在下次 Session 开始时可以感知项目历史。

## 系统架构

**5 个生命周期 Hooks：** SessionStart（注入历史）/ UserPromptSubmit / PostToolUse（捕获观测）/ Stop / SessionEnd（生成摘要）

**存储层：**
- SQLite + FTS5 全文搜索 — 存储 sessions、observations、summaries
- Chroma 向量数据库 — 语义搜索

**服务层：** Worker Service（Bun，端口 37777），含 Web Viewer UI 和 10 个搜索端点

## MCP 搜索：3 层渐进式披露（~10x Token 节省）

| 层 | 工具 | Token/结果 | 作用 |
|----|------|-----------|------|
| 1 | `search` | 50-100 | 获取带 ID 的紧凑索引 |
| 2 | `timeline` | 中等 | 时间轴上下文 |
| 3 | `get_observations` | 500-1000 | 按 ID 拉取完整详情 |

先搜索拿 ID，再按需拉详情——避免全量历史塞入上下文。

## Progressive Disclosure 设计哲学

记忆不是一次性全量注入，而是分层渐进披露：摘要先于细节，Agent 看到 Token 成本后自主决定是否深挖。这让记忆系统在 Token 效率和信息完整性之间取得平衡。

## 安装

```bash
npx claude-mem install              # Claude Code（推荐）
npx claude-mem install --ide gemini-cli
/plugin install claude-mem          # Claude Code 插件市场

# OpenClaw Gateway（一行安装）
curl -fsSL https://install.cmem.ai/openclaw.sh | bash
```

OpenClaw 脚本自动处理依赖、插件配置、Worker 启动，以及可选的 Telegram/Discord/Slack 实时推送。

## 在记忆方案谱系中的定位

| 方案 | 存储内容 | 场景 |
|------|---------|------|
| **Claude-Mem** | 工具调用观测 + 摘要 | Claude Code 工程操作历史 |
| [[GBrain-世界知识脑]] | 结构化世界知识 | 通用知识积累 |
| MEMORY.md（OpenClaw）| 对话关键事件 | 个人 Agent 长期记忆 |
| [[个人知识库编译器模式|wiki-compiler]] | 整理后的知识文章 | 个人知识库 |

Claude-Mem 填补了「Claude Code 工程操作层记忆」的空白——记录的不是"知识"，而是"做了什么、遇到了什么问题"。

## 注意事项

- License 为 AGPL 3.0（非 MIT），商业使用需评估合规性
- 需要 Node.js >= 18
- Chroma 向量数据库需要额外安装
- OpenClaw 用户用 `curl` 安装脚本最简单
