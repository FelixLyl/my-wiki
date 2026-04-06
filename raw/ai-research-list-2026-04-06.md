# AI 调研清单（钉钉 AI 表格导出）

来源：https://alidocs.dingtalk.com/i/nodes/gvNG4YZ7JneL9XyNTqAlQ1ZpV2LD0oRE
导出时间：2026-04-06
总记录数：82 条

---

## 🔴 高优先级

| 主题 | 描述 | 标签 | 优先级理由 |
|------|------|------|-----------|
| skill-builder（流程打包成Skill） | 把手动做过的流程告诉 Agent，自动打包成独立 Skill | #自动化 #skill生成 #工作流 #低代码 | 把重复流程自动固化为 Skill，是 Skills 库建设的核心加速器 |
| 调研 SecureClaw 安全性评估 | 调研 SecureClaw 的安全能力、适用场景，评估是否引入团队 | #安全 #SecureClaw #选型 | 纯调研成本低，结论直接影响密钥/审计方案选择 |
| 如何用好vercel（vercel-composition-patterns） | vercel 最佳实践 | — | 主人标记高优先 |
| 自动化测试相关（scoutqa-test） | 来源：skills.sh/github/awesome-copilot/scoutqa-test | — | 主人标记高优先 |
| 慢雾 OpenClaw 安全实践指南 | 专为高权限自主 Agent 定制，零信任架构，可直接喂给 Agent 自动部署 | #安全 #零信任 #PromptInjection #供应链安全 | 慢雾出品，安全团队必读 |
| Prompt Caching 降本方案 | 利用 Prompt Caching 降低重复上下文 Token 成本，可降低高达 90% | #降本 #Token #PromptCaching | 重复上下文成本降 90%，高频调用场景收益极大 |
| github skills（gh-cli） | 来源：skills.sh/github/awesome-copilot/gh-cli | — | 主人标记高优先 |
| last30days-skill（全网风口聚合器） | 输入关键词自动搜集过去30天 X/Reddit/YouTube/HN 等10个社区讨论，整合 Polymarket 预测市场赔率 | #情报 #趋势分析 #风口 | 本周 GitHub Trending 第一，7349 Stars |
| AI-Search-Hub（minsight-ai-info） | 聚合 Gemini/Grok/豆包/元宝等平台原生 AI 搜索，可触达微信公众号、抖音、微博数据 | #搜索 #AI搜索 #爬虫 #数据采集 | 免维护爬虫，直接复用大厂搜索能力 |
| awesome-openclaw-agents（187个Agent模板） | 收录 187 个生产可用 OpenClaw Agent 模板，覆盖 19 个类别 | #Agent模板 #SOUL.md #OpenClaw | 187个即用模板，调研成本极低收益极高 |
| openclaw-ops-guardrails（高危操作二次授权） | 服务器运维护栏，rm -rf/防火墙修改等高危操作前强制触发拦截申请二次授权 | #安全 #运维 #高危操作 | 线上环境必装 |
| Skill + Swagger 访问内部系统（含登录鉴权方案） | 通过 Swagger 文档自动生成 Skill，让 OpenClaw 访问内部系统，解决登录/鉴权 | #skills #swagger #内部系统集成 #鉴权 | 直接打通 AI 与内部系统，是团队 AI 落地的核心路径 |
| OpenClaw 24小时AI团队实战（微信文章） | 多龙虾协作团队实战教程，涵盖多 Agent 分工、任务调度 | #OpenClaw #多Agent #实战 | 直接对应团队 AI 落地场景 |
| AIClient-2-API（Kiro/Gemini/Qwen转API） | 将 Gemini CLI/Kiro/Qwen Code 等免费大模型封装为本地 OpenAI 兼容接口，支持账号池管理 | #降本 #API代理 #免费模型 | 免费用高级模型，阮一峰周刊推荐 |
| self-improving-agent（自动复盘） | 自动记录纠正过的错误、踩过的坑，整理成知识卡片供后续任务主动调用 | #自学习 #复盘 #记忆 #Agent | 让 Agent 越用越聪明，自动沉淀团队经验 |
| 有整套AI工程SOP（mattpocock/skills） | PRD写作/TDD/架构优化/代码质量等全套 AI 工程 Skills | #工程SOP #claude-code | 主人标记高优先 |
| prd（awesome-copilot/prd） | 来源：skills.sh/github/awesome-copilot/prd | — | 主人标记高优先 |
| prd-to-issues（mattpocock/skills） | 来源：skills.sh/mattpocock/skills/prd-to-issues | — | 主人标记高优先 |
| sql 优化器（awesome-copilot/sql-optimization） | SQL 优化 Skill | — | 主人标记高优先 |
| claude.md 行为规范提示词 | 4条 Agent 行为规范：拒绝假设/追求最优/追根溯源/极致简洁 | #提示词 #claude.md #行为规范 | 直接影响 Agent 输出质量，写入即生效 |
| bb-browser（本地浏览器登录态复用） | 调用本地浏览器登录态让 Agent 操作网页，免扫码，支持小红书等 | #浏览器 #登录态 #自动化 | 免扫码免管 session |
| SecureClaw（安全扫描工具） | 扫描已安装 Skills 的安全风险，能拦截已知攻击模式 | #安全 #扫描 #skill安检 | npm install -g secureclaw 即可使用 |
| skill-vetter（安装前安检） | 安装新 Skill 前扫描安全风险：网络外发/敏感环境变量/写系统目录/可疑base64 | #安全 #skill安检 #审计 | 安全基线工具 |
| skill-guard（技能包安全扫描） | 安装新技能前自动扫描 .js/.sh 文件，检测是否藏有后门 | #安全 #skill #扫描 #供应链 | 供应链安全第一道闸门 |
| OpenClaw 记忆优化（飞书Wiki） | 记忆优化方案文档，涵盖 OpenClaw 记忆管理最佳实践 | #记忆 #优化 #OpenClaw | 记忆是 Agent 核心痛点，调研成本极低 |
| lossless-claw（长期记忆） | 给 OpenClaw 装长期记忆，对话持久化存数据库，后台自动压缩成树状摘要控制 Token | #记忆 #持久化 #Token优化 | OpenClaw 作者推荐，解决失忆痛点 |
| auto-monitor（服务器健康监控） | CPU 飙高、内存快满、服务挂掉，第一时间发消息报警 | #监控 #服务器 #报警 | 线上部署必备 |
| cron-claw（定时任务） | 给 OpenClaw 装定时任务，设好时间和指令自动按时执行 | #定时任务 #自动化 #cron | 无人值守自动化的核心组件 |
| auto-workflow（多动作串联工作流） | 把多个动作串起来，如：看新闻→转PDF→发Discord | #工作流 #自动化 #多步骤 | 自动化工作流核心组件 |
| larksuite/cli | 飞书开放平台命令行工具，200+ 命令，含 19 个 AI Agent Skills | #飞书 #CLI #Agent | GitHub 534⭐，官方出品，团队主用飞书 |
| OpenCLI / jackwener（网站逆向成CLI） | 把任意网站/Electron App 转成 CLI 接口，复用本地浏览器登录态 | #CLI #浏览器自动化 #多平台 | 内部系统集成通用解法 |
| 龙虾公众号流水线实战（第二弹） | 完整公众号自动化流水线，9步全自动 | #公众号 #内容创作 #工作流 #全自动 | 团队有内容运营需求时优先落地 |
| Docker/主机 OpenClaw 密钥防泄露方案 | 调研 .env / Secret Manager / Vault 等密钥安全存储方案 | #安全 #密钥管理 #docker | 安全基线问题，线上部署前必须解决 |
| 调研 openclaw-shield | 调研 openclaw-shield 的防护机制和适用场景 | #安全 #shield #选型 | 安全类工具调研成本低 |
| 如何用好vercel（vercel-react-best-practices） | Vercel + React 最佳实践 | — | 主人标记高优先 |

---

## 🟡 中优先级

| 主题 | 描述 | 标签 | 优先级理由 |
|------|------|------|-----------|
| 已训练 OpenClaw Skills/配置如何复制给其他实例 | 调研如何将一个 OpenClaw 实例的 Skills/Memory/配置等迁移/同步到其他实例 | #skills #知识迁移 #多agent | 团队规模化复用的关键 |
| 调研 openclaw-claude-code-skill | 了解 openclaw-claude-code-skill 的能力边界和使用场景 | #skills #claude #coding | 研发效能方向 |
| multi-search-engine-2-0-1（17搜索引擎聚合） | 聚合 17 个搜索引擎的免费搜索工具 | #搜索 #聚合 #多引擎 #免费 | 17 引擎聚合，信息覆盖面广 |
| YouTube to NotebookLM（Chrome插件实战） | Chrome 插件，一键批量抓取 YouTube 字幕，自动导入 NotebookLM 构建知识库 | #YouTube #NotebookLM #知识库 | 免费插件，竞品分析/领域横扫必备 |
| wewrite（oaker-io） | 公众号文章全流程 AI Skill：热点→选题→写作→SEO→视觉→排版→微信草稿箱 | #内容创作 #公众号 #SEO | GitHub 611⭐ |
| MiniMax Office 文档生成 Skills | MiniMax 开源 Skills 库，聚焦 Office 文档生成，具备自我进化系统 | #MiniMax #Office #文档生成 | 自我进化系统有亮点 |
| 使用 Skills 代替数据库 MCP | 探索用 Skill 封装数据库访问逻辑，替代直接使用 MCP 连数据库 | #skills #mcp #架构 | 有创新价值，建议等 Skills 库建设有基础后再做 |
| 调研 openclaw-token-optimizer（省钱模型） | 调研 openclaw-token-optimizer 的优化策略 | #降本 #token #优化 | 直接影响运营成本 |
| baoyu-youtube-transcript（YouTube字幕抓取） | 输入 YouTube URL 抓取字幕，生成带章节/发言人/封面图的文档 | #YouTube #字幕 #文档生成 | 免 API Key |
| CLI-Anything（HKUDS） | 将任意工具/平台转换为统一 CLI 接口供 AI Agent 调用 | #CLI #Agent #工具集成 | 香港大学出品，与 OpenCLI 同类方向 |
| Product-Manager-Skills（deanpeters） | 基于实战方法论的 PM Skills 框架，46 个 Skills/6 个命令 | #产品经理 #PM #方法论 | 开箱即用 PM Skills |
| geo-seo-claude（zubair-trabzada） | GEO-first SEO 审计 Skill，AI 可引用性评分/AI 爬虫分析/品牌权威度 | #SEO #GEO #AI搜索优化 | 针对 ChatGPT/Claude/Perplexity 优化 |
| control-center（多龙虾仪表盘） | 多 OpenClaw 实例管理面板，查看 Token 消耗/实例健康状态 | #多Agent #监控 #管理 | 多龙虾规模化后必备 |
| slavingia/skills（极简创业方法论） | 基于《The Minimalist Entrepreneur》的 Claude Code Skills | #创业 #方法论 #claude-code | GitHub 4747⭐ |
| opencode 调研 | 调研 opencode 的能力与适用场景 | #coding #Agent #开发工具 | 主人标记待调研 |
| openwork 调研 | 调研 openwork 的能力与适用场景 | #工作流 #Agent #自动化 | 主人标记待调研 |
| Agent-Reach：CLI打通多平台内容读取（零API费用） | 打通 Twitter/Reddit/YouTube/GitHub/B站/小红书的读取和搜索，零 API 费用 | #CLI #多平台 #内容聚合 #零成本 | ⭐11,208 |
| ShopClawMart AI数字员工（调研） | 调研 shopclawmart.com 和 clawmart.cn，AI 数字员工产品/平台 | #AI数字员工 #选型 #商业化 | 了解市场方向和竞品格局 |
| 调研 openclaw-backup（x2） | 调研 openclaw-backup 的备份策略和恢复能力 | #备份 #运维 #可靠性 | 运维保障类，规模化部署前需要 |
| 在 Docker 中构建 OpenClaw 安全审计员 | 设计专职安全审计 OpenClaw，定期检查同环境其他实例安全隐患 | #安全 #审计 #docker #多agent | 建议 #004 密钥方案确定后再做 |
| self-evolving-skill（技能自升级） | 定期巡检已装技能，通过阅读报错日志自动修改代码适配新环境 | #自进化 #自修复 #skill | 减少 Skill 维护成本 |
| claw-router（多龙虾智能调度） | 根据任务类型自动分配给最合适的 OpenClaw 实例 | #多Agent #路由 #调度 | 多 Agent 协作调度层 |
| openclaw-cli（让龙虾精通自身CLI） | 让 OpenClaw 完全精通自身命令行工具，能自己查日志/修复断线/配端口 | #CLI #自运维 #OpenClaw | 自运维能力，减少人工干预 |
| ai-web-automation（复杂网页自动化） | 专门处理需要填验证码、拖动滑块的复杂网页自动化操作 | #浏览器 #验证码 #自动化 | bb-browser 的补充 |
| lu-auto-deploy（自动部署） | 代码审查完直接一行指令推向服务器并重启 Docker | #部署 #Docker #CI/CD | 研发效能工具 |
| 龙虾导航 claw123.com | OpenClaw 生态导航站，聚合 Skills/教程/资源等入口 | #导航 #OpenClaw #生态 | 生态导航，定期浏览发现新 Skills |
| 安装宝玉老师 baoyu-skills（封面生成+插图生成） | baoyu-cover-image（16种渲染风格）+ baoyu-article-illustrator（8种视觉风格） | #skills #生图 #封面 | clawhub install 一条命令 |
| afrexai-productivity-system（个人生产力OS） | 完整个人生产力系统：日程排期/自动规划子任务/到点催进度 | #生产力 #任务管理 | 个人效率方向 |
| PHP-Code-Audit-Skill（0xShe） | 专注 PHP 安全审计，支持漏洞检测/代码审查 | #安全 #PHP #代码审计 | GitHub 175⭐ |
| 调研 frontend-slides（Claude Code 网页幻灯片 Skill） | 生成富有动效的单文件 HTML 幻灯片，支持 PPT 转换/12种视觉风格/零依赖 | #skills #前端 #演示文稿 | 团队演示/汇报场景 |

---

## 🟢 低优先级

| 主题 | 描述 | 标签 |
|------|------|------|
| file-organizer-zh（文件自动归类） | 自动把电脑文件按时间/类型/项目归类整理 | #文件管理 #整理 #自动化 |
| yt-search-download（joeseesun） | YouTube 视频搜索与下载 Skill | #YouTube #下载 #媒体 |
| visual-file-sorter（视觉归类文件） | 调用视觉能力直接看图片内容，按内容自动归类 | #视觉AI #文件管理 #图片分类 |
| skill-scanner（主动推荐技能） | 定期巡检工作区所有环境和代码库，主动推荐适合当前场景的技能 | #推荐 #skill #巡检 |

---

## ✅ 已完成

| 主题 | 标签 |
|------|------|
| 如何低成本构建内部 Skills 库 | #skills #基础设施 #降本 |

## ⏸ 暂缓

| 主题 | 标签 |
|------|------|
| Docker/主机 OpenClaw 密钥防泄露方案 | #安全 #密钥管理 #docker |

---

## 标签全景

**安全类**：#安全 #SecureClaw #零信任 #PromptInjection #供应链安全 #密钥管理 #shield #skill安检 #审计 #运维

**降本类**：#降本 #Token #PromptCaching #API代理 #免费模型 #token优化

**Skills 建设**：#skills #基础设施 #skill生成 #自动化 #知识迁移 #多agent

**内部系统集成**：#swagger #鉴权 #CLI #浏览器自动化 #多平台

**内容创作**：#公众号 #内容创作 #SEO #YouTube #知识库

**AI 工程**：#claude.md #行为规范 #工程SOP #TDD #架构

**记忆/学习**：#记忆 #持久化 #自学习 #复盘 #自进化
