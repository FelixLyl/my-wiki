# Claude-Mem — Claude Code 跨 Session 持久化记忆压缩系统

来源：GitHub README
收集日期：2026-04-18
GitHub: https://github.com/thedotmack/claude-mem
作者: thedotmack
License: AGPL 3.0
当前版本: 6.5.0
文档: https://docs.claude-mem.ai/

---

## 核心定位

**"Persistent memory compression system built for Claude Code."**

Claude-Mem 通过自动捕获工具使用观测值（tool usage observations）、生成语义摘要，并在未来 Session 中注入上下文，让 Claude Code 在 Session 结束或重连后仍能保持项目连续性。

**核心解决的问题**：Claude Code 每次新 Session 都从零开始，没有项目历史记忆。Claude-Mem 是专门针对 Claude Code 的跨 Session 记忆层。

---

## 核心特性

- **持久记忆（Persistent Memory）** — 上下文跨 Session 存活
- **渐进式披露（Progressive Disclosure）** — 分层记忆检索，显示 Token 成本
- **Skill-Based 搜索** — mem-search skill 用自然语言查询项目历史
- **Web Viewer UI** — http://localhost:37777 实时记忆流
- **隐私控制** — `<private>` 标签排除敏感内容
- **自动运行** — 无需人工干预
- **引用（Citations）** — 通过 ID 引用历史观测值
- **OpenClaw 原生集成** — 一行命令安装为 OpenClaw Gateway 插件

---

## 系统架构

### 核心组件

1. **5 个生命周期 Hooks**（6 个 Hook 脚本）
   - SessionStart — Session 开始时注入历史上下文
   - UserPromptSubmit — 用户提交 Prompt 时
   - PostToolUse — 工具调用后捕获观测值
   - Stop — 暂停时
   - SessionEnd — Session 结束时生成摘要

2. **Worker Service** — Bun 管理的 HTTP API（端口 37777），含 Web Viewer UI 和 10 个搜索端点

3. **SQLite 数据库** — 存储 sessions、observations、summaries

4. **Chroma 向量数据库** — 语义 + 关键词混合搜索

5. **mem-search Skill** — 自然语言查询，Progressive Disclosure 策略

### MCP 搜索工具（3 层工作流，~10x Token 节省）

| 层 | 工具 | Token 消耗 | 用途 |
|----|------|-----------|------|
| 1 | `search` | ~50-100 tokens/结果 | 获取带 ID 的紧凑索引 |
| 2 | `timeline` | 中等 | 获取特定结果的时间轴上下文 |
| 3 | `get_observations` | ~500-1000 tokens/结果 | 按 ID 获取完整详情 |

先 search 拿 ID 列表，再用 get_observations 只取感兴趣的条目，避免无差别加载历史。

---

## 安装方式

```bash
# Claude Code（推荐）
npx claude-mem install

# Gemini CLI
npx claude-mem install --ide gemini-cli

# OpenCode
npx claude-mem install --ide opencode

# Claude Code 插件市场
/plugin marketplace add thedotmack/claude-mem
/plugin install claude-mem

# OpenClaw Gateway（一行安装）
curl -fsSL https://install.cmem.ai/openclaw.sh | bash
```

OpenClaw 安装脚本自动处理：依赖安装、插件配置、AI 提供商配置、Worker 启动、可选的实时观测推送（Telegram/Discord/Slack）。

---

## Progressive Disclosure 哲学

不在 Session 开始时把所有历史一次性塞入上下文（高 Token 成本），而是分层按需加载：
- 先注入摘要（低成本）
- Agent 认为需要详情时再按 ID 拉取
- 每次都展示 Token 成本，让 Agent 做出 Token 效率决策

这与 [[GBrain-世界知识脑]] 的混合检索思路类似，但专注于 Session 级别的工程记忆，而非通用知识库。

---

## 与已有记忆方案的对比

| 方案 | 存储内容 | 检索方式 | 场景 |
|------|---------|---------|------|
| **Claude-Mem** | 工具调用观测值 + 语义摘要 | 混合搜索（SQLite FTS5 + Chroma） | Claude Code 工程记忆 |
| **GBrain** | 世界知识（Postgres + pgvector） | 语义检索 | 通用知识积累 |
| **MEMORY.md（OpenClaw）** | 对话关键事件 | 文本读取 | 个人 Agent 长期记忆 |
| **wiki-compiler** | 整理后的知识文章 | 索引 + 双链 | 个人知识库 |

Claude-Mem 填补了一个特定缺口：**Claude Code 在工程项目上的跨 Session 操作记忆**（做了什么、遇到了什么问题、调用了哪些工具）。

---

## 战略价值

对主人工作的直接意义：
- 作为 OpenClaw 生态中的记忆插件，与 QMD（当前使用的记忆后端）形成互补
- QMD 保存对话层面的长期记忆，Claude-Mem 保存工具操作层面的工程历史
- 适合在 Claude Code 场景下长期维护同一个代码库时使用

与 [[Agent记忆持久化]] 课题相关，是该方向的一个具体落地工具。

---

## 来源

- GitHub: https://github.com/thedotmack/claude-mem
- 文档: https://docs.claude-mem.ai/
- License: AGPL 3.0（注意：非 MIT，商业使用需注意）
- 版本: 6.5.0，Node.js >= 18
