---
type: project
maturity: draft
date: 2026-05-24
updated: 2026-05-24
tags: [geo, seo, content-engineering, rag, distribution, wordpress, php, laravel, open-source]
aliases: [GEOFlow, GEO内容工程, 多站点分发, GEO优化系统]
sources: ["https://github.com/yaojingang/GEOFlow"]
related: ["[[AI内容创作工作流]]", "[[Wiki与RAG的对比]]", "[[Prompt-Caching降本]]", "[[n8n]]", "[[Coolify]]"]
---

# GEOFlow

GEOFlow 是面向 GEO（生成式引擎优化）的开源内容工程与多站点分发系统。由 yaojingang 开发，Apache 2.0 开源。

> GEOFlow 是一套专门面向 GEO（生成式引擎优化）的开源智能内容工程与多站点分发系统。它把知识库、素材库、提示词、AI 生成任务、审核发布、数据分析、GEOFlow Agent 目标站点包、WordPress REST 渠道和远端静态页面分发串联为一条可持续运营的工作链路。

技术栈：PHP 8.2+（Laravel）、PostgreSQL（pgvector）、Redis、Docker Compose。

## 核心定位

GEOFlow 的目标是把可信资料沉淀为可管理、可发布、可追踪、可同步到多端的 GEO 内容资产。它不是批量生成工具，而是面向长期运营设计的内容工程系统，知识库建设优先于自动化。

**GEO 与 SEO 的区别**：传统 SEO 优化"被人类点击"，GEO 优化"被 AI 引用"。GEOFlow 内置 AI 爬虫识别、llms.txt/TXT 地图生成和 Schema 输出，直接服务 AI 搜索引擎可信度。

## 系统架构

```
后台管理页面
  ↓
AI 配置 / 素材库 / 提示词 / 任务配置
  ↓
调度器 / 队列 / Worker 执行 AI 生成
  ↓
草稿 / 审核 / 发布
  ↓
本地前台文章与 SEO 页面
  ↓
分发队列 / 目标站点 Agent
  ↓
远端静态首页、详情页、sitemap、TXT 地图与 llms.txt
```

分层结构：

| 层级 | 说明 |
|------|------|
| Web / Admin | Laravel 路由；前台站点、Blade 后台、数据分析、分发管理 |
| API / Agent | 本地 API 与目标站点 PHP Agent；负责分发健康检查、文章接收、远端静态生成 |
| Scheduler / Queue / Reverb | Scheduler 入队；queue:work 消费生成与分发任务；Reverb 按需启用 |
| Persistence | PostgreSQL（pgvector）+ Redis + 目标站点本地 JSON/静态文件 |

## 主要功能

**多模型内容生成**：兼容 OpenAI 风格接口和 Gemini 原生接口，支持 chat / embedding 模型、Provider URL 自动适配、智能模型切换、失败重试和调用统计。

**知识库与 RAG**：支持结构化规则切片、可选 LLM 语义规划和稳定回退。LLM 只规划边界，最终切片从原文稳定重建。配置 embedding 模型后写入向量，在文章生成时召回相关资料。与 [[Wiki与RAG的对比]] 中的"RAG 无积累"不同，GEOFlow 的知识库设计要求先建设真实、可验证的业务资料，知识库才是系统价值的核心。

**素材与提示词体系**：标题库、关键词库、图片库、作者库、知识库、正文提示词、特殊提示词集中管理。

**任务自动化**：支持生成数量、草稿池、审核开关、发布节奏、队列执行、失败重试、发布范围控制。

**多站点分发**：支持 GEOFlow Agent（PHP 包）与 WordPress REST 渠道。为每个渠道生成预配置 PHP Agent 包，内置首页、详情页、静态资源、sitemap、llms.txt 和 Schema。

**数据分析**：系统总览、单站内容运营、多站分发、访问日志、Top 内容、AI 爬虫识别和趋势图，集中于 `/admin/analytics`。

## 典型使用场景

- **独立 GEO 官网**：围绕产品资料、FAQ、案例构建持续更新内容系统，提升 AI 搜索可见度。
- **官网中的 GEO 子频道**：不重构主站，快速上线独立内容频道。
- **独立 GEO 信源站点**：面向特定行业沉淀高质量文章、榜单、指南，构建可信外部内容资产。
- **内部 GEO 内容管理后台**：统一管理模型、素材、审核、发布，服务内容/增长/品牌团队。
- **多站点/多栏目部署**：同一套系统管理多个内容出口。

## 部署方式

Docker Compose 一键拉起（PostgreSQL+pgvector、Redis、应用、队列、调度、Reverb、Nginx/php-fpm）：

```bash
git clone https://github.com/yaojingang/GEOFlow.git
cp .env.example .env
docker compose build && docker compose up -d
```

生产环境使用 `docker-compose.prod.yml`（Nginx + PHP-FPM）。提供参考部署脚本，支持自动环境自检、Docker 检测、.env 生成和部署后健康检查。

默认端口：前台 18080，WebSocket 18081（均可通过环境变量配置）。后台登录路径由 `ADMIN_BASE_PATH` 控制，默认 `/geo_admin/login`。

## 设计哲学

GEOFlow 内置一条明确的使用原则：先建设知识库，再建设自动化流程；先确保内容真实、可核验、可维护，再用模型和任务能力提效。系统不鼓励批量制造低质内容，"知识库建设应该始终排在最前面"。

这与 [[Wiki与RAG的对比]] 中"Wiki 复利增长"的判断高度一致：GEO 内容资产的价值取决于知识库质量，而非生成速度。也与 [[AI内容创作工作流]] 中"人工介入点设置"的设计原则形成互补——GEOFlow 通过审核开关和发布范围控制，在效率与内容质量间保留人工介入空间。

## 2.0 主要变化

- 后台首页改为运营导航，按单站点/多站点/配套 skill 组织入口
- Gemini 与 OpenAI-compatible 接入更完整
- 知识库支持语义切片规划
- 数据分析独立成页（`/admin/analytics`）
- 分发管理进入可运行闭环（支持两种渠道、远端设置同步、静态文件生成）
- 任务发布范围更清晰（本地+渠道 / 仅渠道 / 仅本站）
- 后台多语言覆盖补齐（中/英/日/西/俄/葡）
