# Wiki 主索引

> 由 AI 自动维护。每次入库后更新。最后更新：2026-04-29

## 统计

| 指标 | 数量 |
|------|------|
| 总文章数 | 72 |
| 已入库素材 | 34 |
| 最后入库时间 | 2026-04-26 |
| 待入库素材 | 0 |
| 最后做梦时间 | 2026-04-29 |

## 目录

### 🤖 Agent工程 (Agent工程/)

- [[Claude-Agent-Teams]] — Claude 多 Agent 协作团队模式，混合模型策略，多龙虾实践
- [[Claude-Code-Game-Studios]] — 49 Agent + 72 Skills 的游戏工作室配置，三层层级（Directors/Leads/Specialists），含 Godot/Unity/Unreal 引擎支持
- [[The-Agency-Agents]] — 全业务领域 AI 专家 Agent 人设库（工程/设计/营销/销售），原生支持 OpenClaw，含飞书集成专家和中国市场专家
- [[OpenSpec]] — Spec-Driven Development 框架，编码前先锁定变更工件（proposal+specs+design+tasks），让 AI coding assistant 不偏离需求，支持 20+ 工具
- [[Superpowers]] — obra 开源的 AI Coding Agent 软件工程方法论，Skills 自动触发，Subagent-Driven Development，TDD 强制执行，已上架 Anthropic 官方市场
- [[Agent-Skills-addyosmani]] — Addy Osmani（Google Chrome 工程总监）出品，生产级 AI Coding Agent 工程 Skills，20 个 Skills 覆盖完整开发生命周期，内嵌 Google SWE Book 实践
- [[autoresearch]] — GitHub Issue 驱动的全自动化开发工具，多 Agent 轮转交叉审核，双轨质量门禁（Build/Test + LLM 评分≥85），全自动 Issue→PR→合并→关闭闭环
- [[Code-Simplifier]] — Anthropic 官方 Claude Code 插件，自动简化最近修改的代码，聚焦可读性
- [[Claude-Sub-Agents]] — 动态创建的隔离型子 Agent，处理耗时/脏活任务
- [[CLAUDE-md配置方法论]] — 项目根目录配置文件，植入 AI 的三层思维逻辑；含 Agent 行为规范 4 条黄金准则 + Karpathy 四条编码准则
- [[Karpathy-LLM编码准则]] — Karpathy 观察 LLM 编码三大缺陷提炼的四条准则：Think Before Coding / Simplicity First / Surgical Changes / Goal-Driven Execution
- [[Claude-Code源码架构]] — Claude Code 开源源码全栈拆解：AI 核心、工具系统、Agent 编排、安全、终端 UI、记忆
- [[Claude-Code-Powerup教程]] — Claude Code v2.1.90 内置 /powerup 交互式教程，10 课覆盖 @引用/模式切换/撤销/后台/CLAUDE.md/MCP/Skills+Hooks/子代理/远程控制/模型调节
- [[Hermes-Agent]] — Nous Research 开源 AI 智能体框架，OpenClaw 首个真正竞争对手，~3万 Stars
- [[AI内容创作工作流]] — 公众号全自动流水线 / YouTube 知识化 / GEO-SEO

### 🧠 知识管理 (知识管理/)

- [[个人知识库编译器模式]] — Wiki Compiler Pattern，LLM 替代人工维护知识库
- [[LLM知识库三层架构]] — Raw（不可变来源）→ Wiki（LLM 维护）→ Schema（配置驱动）
- [[知识沉淀双轨机制]] — 经验资产化，私有轨 vs 共享轨；进阶：自动复盘与长期记忆
- [[做梦机制]] — 空闲时产生增量价值：夜间巡逻 + 灵感碰撞 + 健康检查
- [[Agent记忆持久化]] — lossless-claw 树状摘要，解决 Agent 跨会话失忆问题
- [[GBrain-世界知识脑]] — Garry Tan 开源的 AI Agent 世界知识持久化系统，Postgres+pgvector 混合检索，brain-agent loop 每天复利积累
- [[Claude-Mem]] — Claude Code 专属跨 Session 记忆系统，生命周期 Hooks 自动捕获工具观测值，SQLite+Chroma 混合搜索，渐进式披露，原生支持 OpenClaw
- [[Wiki-vs-RAG]] — 知识存储模式对比：RAG 无积累，Wiki 复利增长
- [[幂等防重模式]] — 只处理新增/修改文件，增量永远优于全量
- [[自学习复盘模式]] — self-improving-agent 自动记录错误知识卡片，self-evolving-skill 代码自修复

### 🛠️ 工具与生态 (工具与生态/)

- [[OpenClaw-Skill生态]] — Skill 生成/社区来源/自治能力/187 个生产模板；含视频制作 Skills（HyperFrames）
- [[Coolify]] — 开源自托管 Heroku/Vercel 替代品，SSH 管理多服务器，AI 基础设施私有化部署底座
- [[n8n]] — 面向技术团队的工作流自动化（400+ 集成，原生 LangChain），AI Agent 集成中间层，Fair-code
- [[HyperFrames]] — HeyGen 开源的 HTML-to-Video 框架，AI Agent 原生视频渲染 MP4
- [[HyperFrames]] — HeyGen 开源的 HTML-to-Video 框架，AI Agent 原生，HTML+GSAP 写合成，无头 Chrome 渲染 MP4
- [[Codex-InApp-Browser]] — OpenAI Codex 内嵌浏览器 + Comment Mode，点击页面元素 → 自动截图 + DOM 捕获 → 精准上下文注入，消除欠规格 Prompt
- [[Codex-全能助手]] — OpenAI Codex 2026 重大更新：Computer Use + 内嵌浏览器 + 记忆 + 自动化 + 90+插件，全流程 Agent 协作伙伴
- [[repo-analyzer]] — 一句话生成开源项目深度架构报告；8 阶段工作流 + 并发 sub-agent + Mermaid 图；兼容 Claude Code / Codex / OpenClaw
- [[GStack-虚拟工程团队]] — Garry Tan 的 Claude Code 虚拟工程配置，23 个专家角色 + 8 工具技能，think→plan→build→review→test→ship 完整 sprint 流程
- [[markdown-viewer-skills]] — 面向 AI Agent 的图表/可视化 Skills 库，15 个 Skills，7 种渲染引擎（PlantUML/Mermaid/Vega/Graphviz 等）
- [[autoskills]] — `npx autoskills` 一行命令，自动扫描项目技术栈并安装最匹配 Skills，支持 Claude Code CLAUDE.md 自动生成，70+ 技术栈
- [[skills-manage]] — Tauri 桌面应用，统一管理 27 个 AI 工具平台 Skills（含 OpenClaw 龙虾系列），中央库+符号链接，支持 Marketplace/GitHub 导入
- [[内部系统Agent集成]] — Swagger 自动生成 Skill / CLI 化 / 浏览器登录态复用
- [[LLM-Wiki-Desktop]] — nashsu/llm_wiki：Karpathy 模式的完整桌面应用实现，两步 CoT Ingest + 4信号知识图谱 + Louvain 社区检测 + 向量搜索 + Deep Research + Chrome Clipper
- [[Prompt-Caching降本]] — Prompt Caching 降低高达 90% 重复 Token 成本
- [[Graphify-代码知识图谱]] — 将代码库转化为可查询知识图谱，AST + 语义双轨，支持论文与代码跨模态融合，可集成 Claude Code / Codex

### 🔒 安全 (安全/)

- [[OpenClaw-Agent安全体系]] — SecureClaw/skill-vetter/高危拦截/零信任，Agent 安全全栈

### 👤 人物 (人物/)

- [[Andrej-Karpathy]] — 深度学习研究者，LLM 个人知识库模式与 LLM 编码缺陷观察的提出者

### 💡 洞见 (洞见/)

- [[Insight-AI系统的三层记忆架构-2026-04-06]] — AI 的上下文隔离、记忆分层与声明式配置，映射人类三层记忆架构
- [[Insight-流水线即知识复利机-2026-04-07]] — 内容创作流水线双轨输出：发布轨 + 入库轨，幂等防重保证知识不重复积累
- [[Insight-分布式Agent与知识孤岛-2026-04-08]] — Agent Teams 上下文隔离引入知识碎片化风险，需做梦机制异步蒸馏跨 Agent 经验
- [[Insight-错误记忆的缓存友好编码-2026-04-09]] — 错误知识卡片应采用"仅追加、永不修改"策略，使 Agent 记忆既能复利积累，又能最大化 Prompt Cache 命中率
- [[Insight-经验缓存化是Agent框架的护城河-2026-04-10]] — 自学习经验层（L1/L2）天然缓存友好，构成 Agent 框架对抗开源竞品的真正壁垒
- [[Insight-不确定性显性化-2026-04-12]] — 不确定性显性化洞见
- [[Insight-安全体系的自进化闭环-2026-04-13]] — 静态安全规则 × 流程化安全评审 × 自学习复盘 → 安全知识卡片记忆化 + 动态更新防护规则
- [[Insight-流程结构即Agent协调语言-2026-04-14]] — GStack sprint + Agent安全 + 做梦机制三角碰撞：流程结构比 Prompt 规则更可靠，是多 Agent 系统的真正协调语言
- [[Insight-反重复发现原则-2026-04-15]] — CLAUDE.md × Wiki × Prompt Caching 三层共享同一底层逻辑：已知的东西不必重新发现，知识固化是认知经济学的核心优化
- [[Insight-安全审计即做梦机制的镜像-2026-04-16]] — 安全体系与做梦机制的底层同构：空闲时自检、不确定性显性化、三层自省闭环（知识层 + 代码层 + 运行层）
- [[Insight-自主系统的内建怀疑论-2026-04-17]] — 自主系统洞见
- [[Insight-Agent系统的三类熵增防线-2026-04-18]] — Agent 系统熵增防线洞见
- [[Insight-意图翻译损耗-2026-04-19]] — 意图翻译损耗洞见
- [[Insight-Agent安全的三层纵深防御-2026-04-20]] — Claude Code内核安全 × OpenClaw运行时安全 × GStack专家Agent安全，三层互补形成完整纵深防御体系，揭示跨Agent调用链信任传递为共同盲区
- [[Insight-可信Agent的根基是制度-2026-04-21]] — 可信 Agent 系统的底层是制度而非技术洞见
- [[Insight-知识编译器即安全的最后一公里-2026-04-23]] — 知识编译器与安全体系的底层同构洞见
- [[Insight-人格即防熵锚点-2026-04-24]] — Agent 人格文件是行为熵的第四维防线，Karpathy 持续约束论 × The Agency 人格驱动 × 三类熵增防线的跨领域洞见
- [[Insight-Agent工具链的信任边界悖论-2026-04-27]] — 安全体系 × 可视化 Skills × 错误记忆缓存编码，揭示信任边界三种扩散路径及"守卫的守卫"悖论
- [[Insight-知识资本化是AI效率游戏的分成门票-2026-04-28]] — autoresearch多Agent自动化 × GBrain知识脑复利积累 × AI角色转变，揭示效率红利的真正获取路径
- [[Insight-自主权即复利的前提-2026-04-29]] — Coolify自托管 × Karpathy wiki模式 × 做梦机制，揭示控制权是时间复利积累的必要前提，三层自主权框架

### 💭 价值观与方法论 (philosophies/)

- [[AI与商业角色转变]] — AI 效率红利流向由商业链路角色决定；工具使用不等于角色升级（stub，待补充）

### 📖 来源 (来源/)

- [[karpathy-llm-wiki-pattern]] — Karpathy Gist：Wiki vs RAG 核心论述，三层架构与三个工作流
- [[zhangpelf-wiki-compiler-v2]] — zhangpelf V2：幂等防重、做梦机制、零幻觉健康检查、Map-Reduce 综述
- [[farzaa-personal-wiki-skill]] — farzaa 个人 wiki：针对生活数据的变体，作家视角，五大工作流
- [[ai-research-list-2026-04-06]] — AI 工具调研清单，82 条记录，覆盖安全/降本/Skills/集成/内容/工程/记忆 7 个方向
- [[claude-code-source-study]] — luyao618 的 25 篇 Claude Code 源码深度分析，覆盖 AI 核心/工具/Agent/安全/UI/记忆全栈
