---
type: concept
maturity: draft
date: 2026-04-11
updated: 2026-04-11
tags: [knowledge-brain, agent-memory, pgvector, hybrid-search, gbrain, garry-tan, openclaw]
aliases: [GBrain, gbrain, 世界知识脑, brain-agent-loop, Garry-Tan-Brain]
sources: ["raw/garrytan-gbrain.md"]
related: ["[[Agent记忆持久化]]", "[[个人知识库编译器模式]]", "[[LLM知识库三层架构]]", "[[做梦机制]]", "[[Andrej-Karpathy]]", "[[GStack-虚拟工程团队]]"]
---

# GBrain：世界知识脑

GBrain（garrytan/gbrain）是 Garry Tan（Y Combinator CEO）开源的 AI Agent 知识持久化系统。核心命题是：Agent 每次对话前从知识库读取上下文，每次对话后将新知写入，形成一个持续复利的知识循环。

"Your AI agent is smart but it doesn't know anything about your life. GBrain fixes that."

## Brain-Agent Loop（核心循环）

```
信号到达（会议/邮件/推文/链接）
  → 实体识别（人/公司/想法）
  → READ：先查脑库（gbrain search / get）
  → 携带完整上下文回应
  → WRITE：更新 brain 页面
  → Sync：索引变更，供下次查询
```

每次循环都在积累。一个没有此循环的 Agent 永远从零开始；有了此循环，它每天变得更聪明。差异随时间复利增长。

## 知识模型：Compiled Truth + Timeline

每个 brain 页面遵循固定格式：

- **Compiled Truth**（上方）：当前最佳理解，有新证据时**整体重写**，而非追加
- **Timeline**（下方，`---` 分隔）：时间顺序的证据链，**只追加，永不修改**

Compiled truth 是答案，timeline 是证据。这一结构让知识既能更新（重写顶部），又能溯源（追加底部），不需要在两者之间取舍。

## 与 OpenClaw 记忆的分层定位

GBrain 与 [[Agent记忆持久化]] 中的 OpenClaw memory 是互补而非竞争的关系：

| 层 | 存什么 | 工具 |
|----|--------|------|
| **GBrain** | 世界知识：人/公司/会议/想法/媒体 | gbrain search / query / get |
| **Agent memory** | 运行状态：偏好/决策/配置 | memory_search |
| **Session context** | 当前对话 | 自动注入 |

三层都应在每次回应前查询，各司其职。

## 技术实现

### 存储引擎（可插拔）

- **PGLite**（默认）：嵌入式 Postgres 17.5（via WASM），零配置，~2 秒就绪，约 750MB 容纳 7,500 页
- **Supabase Pro**（$25/月）：1000+ 文件、多设备、远程 MCP 时迁移

双向迁移：`gbrain migrate --to supabase / --to pglite`

### 混合检索管线

```
查询
  → 多查询扩展（Claude Haiku 生成同义变体）
  → 向量搜索（HNSW cosine）+ 关键词搜索（tsvector ts_rank）
  → RRF 融合：score = Σ 1/(60 + rank)
  → 4 层去重（余弦相似度 / 类型多样性 / 分块上限）
```

关键词搜索找不到语义相似但措辞不同的内容（如用"ignore conventional wisdom"找不到"Bus Ticket Theory"），向量搜索会因周围文本稀释精确短语。RRF 融合兼顾两者。

## 与 [[个人知识库编译器模式]] 的关系

GBrain 与 wiki-compiler 模式的技术路线不同，但理念高度吻合：

| 维度 | GBrain | Wiki Compiler |
|------|--------|---------------|
| 存储格式 | Postgres + pgvector | Markdown 文件 + git |
| 检索方式 | SQL 混合搜索 | LLM 读文件 |
| 维护者 | Agent 读写循环 | LLM 批量编译 |
| 来源理念 | Garry Tan 实践 | Karpathy 提出 |

两者都遵循"原始素材不可变 + 提炼知识可更新"的原则。参见 [[LLM知识库三层架构]]。

## 做梦循环（Dream Cycle）

GBrain 在夜间运行 cron 任务（参见 [[做梦机制]]的类比）：扫描所有对话，丰富遗漏实体，修复断裂引用，整合记忆。"我睡觉的时候 Agent 还在工作，早上起来脑库比睡前更聪明。"

## 数据规模参考（真实部署）

Garry Tan 自己的 brain：10,000+ markdown 文件，3,000+ 人物完整档案，13 年日历，280+ 会议记录，300+ 原始想法。7,500 页约占 750MB（含嵌入向量），初始嵌入成本约 $4-5。

## 集成 Recipes

7 个开箱即用的数据接入配方：语音转 brain（Twilio + OpenAI Realtime）、邮件转 brain（Gmail）、Twitter 转 brain（时间线 + 提及）、日历转 brain（Google Calendar）、会议记录同步（Circleback）等。每个 Recipe 是一个 Markdown 文件，Agent 读取后自行要求 API Key、验证、连线。

## 安装

```bash
bun add github:garrytan/gbrain
gbrain init          # PGLite 本地，~2 秒就绪
gbrain import ~/notes/   # 导入 markdown（幂等）
gbrain query "what themes show up across my notes?"
```

MIT 开源。
