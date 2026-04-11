# garrytan/gbrain — AI Agent 持久知识脑

来源：https://github.com/garrytan/gbrain
抓取时间：2026-04-11

## 一句话定位

你的 AI Agent 很聪明，但对你的生活一无所知。GBrain 解决这个问题。
会议、邮件、推文、日历、语音通话、原始想法……全部流入一个可检索的知识库，Agent 每次回答前先读取，每次对话后写入。Agent 每天变得更聪明。

## 核心架构

```
Brain Repo (git)          GBrain (检索层)         AI Agent (读写层)
────────────────    ─►   ──────────────────   ◄──►  ─────────────────
markdown files            Postgres + pgvector         skills 定义
= source of truth         hybrid search               HOW to use brain
human always              (vector + keyword            entity detect
can read & edit           + RRF)                      enrich / ingest
```

### 知识页面格式：compiled truth + timeline

```markdown
---
type: concept
title: Do Things That Don't Scale
tags: [startups, growth]
---

（compiled truth — 当前最佳理解，新证据出现时重写）

---

- 2013-07-01: Published on paulgraham.com
- 2024-11-15: Referenced in batch W25 kickoff talk
（timeline — 只追加，永不编辑）
```

compiled truth 是答案，timeline 是证据链。

## 技术实现

### 存储引擎（可插拔）

| 引擎 | 场景 |
|------|------|
| PGLite（默认） | 本地嵌入式 Postgres 17.5（via WASM），零配置，~2秒就绪 |
| Supabase Pro（$25/月） | 1000+ 文件，多设备，远程 MCP，8GB 存储 |

双向迁移：`gbrain migrate --to supabase` / `--to pglite`

### 数据库结构（10 张表）

- **pages**：核心内容，slug/type/compiled_truth/timeline/frontmatter(JSONB)/search_vector
- **content_chunks**：分块内容 + 1536 维向量嵌入（HNSW 索引）
- **links**：页面间有类型的双向链接
- **timeline_entries**：结构化时间线事件
- **page_versions**：版本历史快照
- **raw_data**：外部 API 原始数据旁存

### 检索管线（混合搜索）

```
Query → 多查询扩展（Claude Haiku）
         ↓
    向量搜索 + 关键词搜索
         ↓
    RRF 融合（score = Σ 1/(60+rank)）
         ↓
    4 层去重（余弦相似度/类型多样性/分块上限）
```

### 分块策略（三种）

- **递归**：5 级分隔层级，300词/块，50词重叠（快，用于批量导入）
- **语义**：计算句子间余弦相似度 + 平滑找主题边界（质量最高）
- **LLM 引导**：Claude Haiku 识别主题边界（最贵，按需）

## 核心循环（brain-agent loop）

```
信号到达（会议/邮件/推文/链接）
  → Agent 实体识别（人/公司/想法）
  → READ：先查脑库（gbrain search / get）
  → 携带完整上下文回应
  → WRITE：更新 brain 页面
  → Sync：索引变更
```

每次循环都在积累知识。复利效应每天都在发生。

## 集成 Recipes

| Recipe | 需要 | 功能 |
|--------|------|------|
| ngrok-tunnel | — | 固定 URL 供 MCP + 语音使用 |
| credential-gateway | — | Gmail + Calendar 鉴权 |
| voice-to-brain | ngrok | 电话通话 → brain 页面 |
| email-to-brain | credential-gateway | Gmail → 实体页面 |
| x-to-brain | — | Twitter 时间线 + 提及 → brain |
| calendar-to-brain | credential-gateway | Google Calendar → 可搜索日历页 |
| meeting-sync | — | Circleback 会议记录 → brain 含参会人 |

## 与 OpenClaw 记忆的分层关系

| 层 | 存什么 | 查询方式 |
|----|--------|---------|
| **GBrain** | 人物/公司/会议/想法/媒体（世界知识） | gbrain search/query/get |
| **Agent memory** | 偏好/决策/配置（运行状态） | memory_search |
| **Session context** | 当前对话 | 自动 |

三层都应查询。互补，不竞争。

## 主要 CLI 命令

```bash
# 初始化
gbrain init                # PGLite 本地
gbrain init --supabase     # Supabase 向导

# 读写
gbrain get <slug>          # 读页面（支持模糊匹配）
gbrain put <slug>          # 写/更新页面
gbrain delete <slug>       # 删页面

# 检索
gbrain search <query>      # 关键词搜索
gbrain query <question>    # 混合搜索（向量+关键词+RRF）

# 导入
gbrain import <dir>        # 导入 markdown 目录（幂等）
gbrain sync --repo <path>  # 增量 git 同步
gbrain embed --stale       # 生成/刷新向量嵌入

# 图谱
gbrain link <from> <to>    # 创建有类型链接
gbrain graph <slug>        # 遍历链接图（递归 CTE，默认深度 5）

# 健康
gbrain doctor              # 健康检查
gbrain health              # 嵌入覆盖率/过期/孤儿页统计
gbrain stats               # 脑库统计
```

## Skill 文件

| Skill | 功能 |
|-------|------|
| ingest/SKILL.md | 导入会议/文档，更新 compiled truth，追加 timeline，跨实体链接 |
| query/SKILL.md | 3 层检索 + 综合 + 引用溯源 |
| maintain/SKILL.md | 定期健康：矛盾/过期/孤儿/死链 |
| enrich/SKILL.md | 外部 API 丰富页面 |
| briefing/SKILL.md | 每日简报：会议准备/活跃 deals/待办线程 |
| migrate/SKILL.md | 从 Obsidian/Notion/Logseq/Roam 迁移 |

## 规模数据（真实部署）

- 10,000+ markdown 文件，3,000+ 人物完整档案
- 13 年日历数据，280+ 会议记录，300+ 原始想法
- 7,500 页约占 750MB（含嵌入向量）
- 初始嵌入成本约 $4-5（OpenAI text-embedding-3-large）

## 安装与依赖

```bash
# 快速安装
bun add github:garrytan/gbrain
gbrain init
gbrain import ~/git/brain/
gbrain query "what themes show up across my notes?"
```

| 依赖 | 用途 | 获取 |
|------|------|------|
| OpenAI API Key | 向量嵌入 | platform.openai.com |
| Anthropic API Key | 多查询扩展 + LLM 分块 | console.anthropic.com |
| Supabase Pro $25/mo | 生产规模存储 | supabase.com |

无 OpenAI Key → 关键词搜索仍可用（无向量搜索）

## MCP 接入

```json
// Claude Code (~/.claude/server.json)
{
  "mcpServers": {
    "gbrain": {
      "command": "gbrain",
      "args": ["serve"]
    }
  }
}
```

30 个 MCP 工具：get_page, put_page, search, query, add_link, traverse_graph, sync_brain 等。

## 许可证
MIT
