---
type: concept
maturity: draft
date: 2026-04-06
updated: 2026-04-06
tags: [content-creation, wechat, seo, youtube, workflow, automation, wewrite]
aliases: [内容创作, AI写作, 公众号流水线, wewrite, AI内容自动化]
sources: ["raw/ai-research-list-2026-04-06.md"]
related: ["[[OpenClaw-Skill生态]]", "[[内部系统Agent集成]]"]
---

# AI 内容创作工作流

AI 辅助内容创作已从"辅助写作"演进为端到端全自动流水线：从热点选题到最终发布，可由 Agent 一次性完成。调研清单中涉及的方案覆盖公众号、YouTube 知识库、SEO 三个主要场景。

## 公众号全自动流水线

### 龙虾公众号流水线（第二弹）

9步全自动流程：热点发现 → 选题确认 → 内容写作 → SEO 优化 → 视觉配图 → 排版 → 推送草稿箱。整个流程由 OpenClaw 串联执行，人工介入点只有"选题确认"（可选）。

### wewrite（oaker-io）

功能覆盖公众号全流程的 AI Skill：热点→选题→写作→SEO→视觉→排版→微信草稿箱，GitHub 611⭐。与龙虾流水线思路一致，封装为标准 Skill，接入成本更低。

### MiniMax Office 文档生成 Skills

MiniMax 开源 Skills 库，聚焦 Office 文档生成（Word/PPT/Excel），具备**自我进化系统**——可根据用户反馈自动优化生成逻辑。文档生成的自进化机制有亮点，值得关注。

## YouTube 内容知识化

**YouTube to NotebookLM（Chrome 插件）**：一键批量抓取 YouTube 字幕，自动导入 NotebookLM 构建知识库。适合竞品分析、行业横扫场景——批量消化竞品/行业 YouTube 内容，沉淀为可查询的知识库。

**baoyu-youtube-transcript**：输入 YouTube URL 抓取字幕，生成带章节、发言人、封面图的结构化文档，无需 API Key。与上一个工具定位略有不同：baoyu 聚焦单视频精处理，插件方案聚焦批量导入。

## SEO 优化方向

**geo-seo-claude（zubair-trabzada）**：GEO-first SEO 审计 Skill，专为 AI 搜索优化设计：
- AI 可引用性评分
- AI 爬虫分析（ChatGPT/Claude/Perplexity 的访问分析）
- 品牌权威度评估

与传统 SEO 工具的区别在于：针对"被 AI 引用"而非"被人类点击"优化，这是 GEO（Generative Engine Optimization）方向的核心转变。

## 情报聚合工具

**last30days-skill（全网风口聚合器）**：输入关键词，自动聚合过去 30 天 X/Reddit/YouTube/HN 等 10 个社区讨论，并整合 Polymarket 预测市场赔率（本周 GitHub Trending 第一，7349⭐）。

**AI-Search-Hub（minsight-ai-info）**：聚合 Gemini/Grok/豆包/元宝等平台原生 AI 搜索，可触达微信公众号、抖音、微博数据，免维护爬虫，直接复用大厂搜索能力。

**multi-search-engine-2-0-1**：聚合 17 个搜索引擎的免费工具，信息覆盖面广。

## 工作流设计原则

内容创作流水线的关键设计选择是**人工介入点**的设置：全自动降低人力成本，但风险是低质输出；在选题和最终审核两个节点保留人工判断，可以在效率和质量间取得平衡。
