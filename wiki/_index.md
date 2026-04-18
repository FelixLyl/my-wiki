# Wiki 主索引

> 由 AI 自动维护。每次入库后更新。最后更新：2026-04-18

## 统计

| 指标 | 数量 |
|------|------|
| 总文章数 | 42 |
| 已入库素材 | 14 |
| 最后入库时间 | 2026-04-18 |
| 待入库素材 | 1（raw/AI编程.md，待主人确认） |
| 最后做梦时间 | 2026-04-15 |

## 目录

### 🤖 Agent工程 (Agent工程/)

- [[Claude-Agent-Teams]] — Claude 多 Agent 协作团队模式，混合模型策略，多龙虾实践
- [[Claude-Code-Game-Studios]] — 49 Agent + 72 Skills 的游戏工作室配置，三层层级（Directors/Leads/Specialists），含 Godot/Unity/Unreal 引擎支持
- [[The-Agency-Agents]] — 全业务领域 AI 专家 Agent 人设库（工程/设计/营销/销售），原生支持 OpenClaw，含飞书集成专家和中国市场专家
- [[Superpowers]] — obra 开源的 AI Coding Agent 软件工程方法论，Skills 自动触发，Subagent-Driven Development，TDD 强制执行，已上架 Anthropic 官方市场
- [[Claude-Sub-Agents]] — 动态创建的隔离型子 Agent，处理耗时/脏活任务
- [[CLAUDE-md配置方法论]] — 项目根目录配置文件，植入 AI 的三层思维逻辑；含 Agent 行为规范 4 条黄金准则
- [[Claude-Code源码架构]] — Claude Code 开源源码全栈拆解：AI 核心、工具系统、Agent 编排、安全、终端 UI、记忆
- [[Hermes-Agent]] — Nous Research 开源 AI 智能体框架，OpenClaw 首个真正竞争对手，~3万 Stars
- [[AI内容创作工作流]] — 公众号全自动流水线 / YouTube 知识化 / GEO-SEO

### 🧠 知识管理 (知识管理/)

- [[个人知识库编译器模式]] — Wiki Compiler Pattern，LLM 替代人工维护知识库
- [[LLM知识库三层架构]] — Raw（不可变来源）→ Wiki（LLM 维护）→ Schema（配置驱动）
- [[知识沉淀双轨机制]] — 经验资产化，私有轨 vs 共享轨；进阶：自动复盘与长期记忆
- [[做梦机制]] — 空闲时产生增量价值：夜间巡逻 + 灵感碰撞 + 健康检查
- [[Agent记忆持久化]] — lossless-claw 树状摘要，解决 Agent 跨会话失忆问题
- [[GBrain-世界知识脑]] — Garry Tan 开源的 AI Agent 世界知识持久化系统，Postgres+pgvector 混合检索，brain-agent loop 每天复利积累
- [[Wiki-vs-RAG]] — 知识存储模式对比：RAG 无积累，Wiki 复利增长
- [[幂等防重模式]] — 只处理新增/修改文件，增量永远优于全量
- [[自学习复盘模式]] — self-improving-agent 自动记录错误知识卡片，self-evolving-skill 代码自修复

### 🛠️ 工具与生态 (工具与生态/)

- [[OpenClaw-Skill生态]] — Skill 生成/社区来源/自治能力/187 个生产模板；含视频制作 Skills（HyperFrames）
- [[HyperFrames]] — HeyGen 开源的 HTML-to-Video 框架，AI Agent 原生，HTML+GSAP 写合成，无头 Chrome 渲染 MP4
- [[GStack-虚拟工程团队]] — Garry Tan 的 Claude Code 虚拟工程配置，23 个专家角色 + 8 工具技能，think→plan→build→review→test→ship 完整 sprint 流程
- [[markdown-viewer-skills]] — 面向 AI Agent 的图表/可视化 Skills 库，15 个 Skills，7 种渲染引擎（PlantUML/Mermaid/Vega/Graphviz 等）
- [[内部系统Agent集成]] — Swagger 自动生成 Skill / CLI 化 / 浏览器登录态复用
- [[Prompt-Caching降本]] — Prompt Caching 降低高达 90% 重复 Token 成本

### 🔒 安全 (安全/)

- [[OpenClaw-Agent安全体系]] — SecureClaw/skill-vetter/高危拦截/零信任，Agent 安全全栈

### 👤 人物 (人物/)

- [[Andrej-Karpathy]] — 深度学习研究者，LLM 个人知识库模式的提出者

### 💡 洞见 (洞见/)

- [[Insight-AI系统的三层记忆架构-2026-04-06]] — AI 的上下文隔离、记忆分层与声明式配置，映射人类三层记忆架构
- [[Insight-流水线即知识复利机-2026-04-07]] — 内容创作流水线双轨输出：发布轨 + 入库轨，幂等防重保证知识不重复积累
- [[Insight-分布式Agent与知识孤岛-2026-04-08]] — Agent Teams 上下文隔离引入知识碎片化风险，需做梦机制异步蒸馏跨 Agent 经验
- [[Insight-错误记忆的缓存友好编码-2026-04-09]] — 错误知识卡片应采用"仅追加、永不修改"策略，使 Agent 记忆既能复利积累，又能最大化 Prompt Cache 命中率
- [[Insight-经验缓存化是Agent框架的护城河-2026-04-10]] — 自学习经验层（L1/L2）天然缓存友好，构成 Agent 框架对抗开源竞品的真正壁垒
- [[Insight-Memex闭环-2026-04-11]] — Memex 闭环洞见
- [[Insight-不确定性显性化-2026-04-12]] — 不确定性显性化洞见
- [[Insight-安全体系的自进化闭环-2026-04-13]] — 静态安全规则 × 流程化安全评审 × 自学习复盘 → 安全知识卡片记忆化 + 动态更新防护规则
- [[Insight-流程结构即Agent协调语言-2026-04-14]] — GStack sprint + Agent安全 + 做梦机制三角碰撞：流程结构比 Prompt 规则更可靠，是多 Agent 系统的真正协调语言
- [[Insight-反重复发现原则-2026-04-15]] — CLAUDE.md × Wiki × Prompt Caching 三层共享同一底层逻辑：已知的东西不必重新发现，知识固化是认知经济学的核心优化
- [[Insight-安全审计即做梦机制的镜像-2026-04-16]] — 安全体系与做梦机制的底层同构：空闲时自检、不确定性显性化、三层自省闭环（知识层 + 代码层 + 运行层）

### 📖 来源 (来源/)

- [[karpathy-llm-wiki-pattern]] — Karpathy Gist：Wiki vs RAG 核心论述，三层架构与三个工作流
- [[zhangpelf-wiki-compiler-v2]] — zhangpelf V2：幂等防重、做梦机制、零幻觉健康检查、Map-Reduce 综述
- [[farzaa-personal-wiki-skill]] — farzaa 个人 wiki：针对生活数据的变体，作家视角，五大工作流
- [[ai-research-list-2026-04-06]] — AI 工具调研清单，82 条记录，覆盖安全/降本/Skills/集成/内容/工程/记忆 7 个方向
- [[claude-code-source-study]] — luyao618 的 25 篇 Claude Code 源码深度分析，覆盖 AI 核心/工具/Agent/安全/UI/记忆全栈
