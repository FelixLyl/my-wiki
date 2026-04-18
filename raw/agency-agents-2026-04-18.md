# The Agency — 覆盖全业务领域的 AI 专家 Agent 集合

来源：GitHub README
收集日期：2026-04-18
GitHub: https://github.com/msitarzewski/agency-agents
作者: msitarzewski
License: MIT
起源：Reddit 帖子 + 数月迭代

---

## 核心定位

**"A complete AI agency at your fingertips"**

The Agency 是一个持续增长的 AI Agent 人设（personality）集合库。区别于通用 Prompt 模板，每个 Agent 有：

- **专业化（Specialized）** — 深耕特定领域，而非万能助手
- **人格驱动（Personality-Driven）** — 独特的声音、沟通风格和处事方式
- **交付物导向（Deliverable-Focused）** — 真实代码、流程、可衡量结果
- **生产就绪（Production-Ready）** — 经过实战验证的工作流程和成功指标

定位是：**组建梦之队，成员都是不知疲倦、绝不抱怨的 AI 专家。**

---

## 支持工具列表（原生多平台）

一键安装脚本，支持 10+ 工具：

```bash
./scripts/install.sh --tool claude-code   # 推荐
./scripts/install.sh --tool openclaw
./scripts/install.sh --tool cursor
./scripts/install.sh --tool gemini-cli
./scripts/install.sh --tool opencode
./scripts/install.sh --tool copilot
./scripts/install.sh --tool antigravity
./scripts/install.sh --tool aider
./scripts/install.sh --tool windsurf
./scripts/install.sh --tool kimi
```

`convert.sh` 先生成各工具的格式文件，`install.sh` 再自动检测并安装。

---

## Agent 部门架构

### 💻 Engineering Division（工程部，28 个 Agent）

| Agent | 专长 |
|-------|------|
| Frontend Developer | React/Vue/Angular，Core Web Vitals |
| Backend Architect | API、数据库架构、微服务 |
| Mobile App Builder | iOS/Android，React Native，Flutter |
| AI Engineer | ML 模型、数据管道、AI 集成 |
| DevOps Automator | CI/CD、基础设施自动化 |
| Rapid Prototyper | 快速 POC、MVP |
| Senior Developer | Laravel/Livewire、复杂架构 |
| Security Engineer | 威胁建模、安全代码审查 |
| Autonomous Optimization Architect | LLM 路由、成本优化、影子测试 |
| Incident Response Commander | 故障管理、事后复盘 |
| Solidity Smart Contract Engineer | EVM 合约、DeFi |
| Codebase Onboarding Engineer | 代码库快速上手、只读探索 |
| Technical Writer | 开发文档、API 参考 |
| Threat Detection Engineer | SIEM 规则、威胁狩猎、ATT&CK |
| WeChat Mini Program Developer | 微信小程序、支付集成 |
| Code Reviewer | PR 审查、代码质量门禁 |
| Database Optimizer | PostgreSQL/MySQL 调优 |
| Git Workflow Master | 分支策略、Conventional Commits |
| Software Architect | DDD、系统设计、架构模式 |
| SRE | SLO、错误预算、可观测性 |
| AI Data Remediation Engineer | 自愈数据管道、语义聚类 |
| Data Engineer | 数据湖、ETL/ELT |
| **Feishu Integration Developer** | **飞书 Open Platform、Bot、工作流** |
| CMS Developer | WordPress & Drupal |
| Email Intelligence Engineer | 邮件解析、结构化数据 |
| Voice AI Integration Engineer | 语音转文字、Whisper、说话人分离 |
| Embedded Firmware Engineer | 裸机/RTOS、ESP32/STM32 |
| Filament Optimization Specialist | Filament PHP Admin UX |

### 🎨 Design Division（设计部，8 个 Agent）

| Agent | 专长 |
|-------|------|
| UI Designer | 可视化设计、组件库、设计系统 |
| UX Researcher | 用户测试、行为分析 |
| UX Architect | 技术架构、CSS 系统 |
| Brand Guardian | 品牌一致性、定位 |
| Visual Storyteller | 视觉叙事、多媒体内容 |
| Whimsy Injector | 个性、愉悦感、彩蛋 |
| Image Prompt Engineer | AI 图片提示词（Midjourney/DALL-E/SD） |
| Inclusive Visuals Specialist | 文化代表性、AI 图像偏见消除 |

### 💰 Paid Media Division（付费媒体部，7 个 Agent）

Google/Meta/Amazon 广告账户管理、搜索词分析、广告创意策略、GTM 追踪。

### 💼 Sales Division（销售部，9 个 Agent）

Outbound 策略、发现对话教练、MEDDPICC 成单分析、提案策略、管道分析、客户成功。

### 📢 Marketing Division（市场部，26 个 Agent）

分为西方平台（TikTok/Instagram/Twitter/Reddit）、中国生态（小红书/抖音/微信/微博）、跨平台策略师。

### 🔖 Specialized Agents（专项 Agent）

独立专家：Product Manager、Reddit Community Ninja、Reality Checker（防 AI Slop）、Sales Outreach、Researcher Analyst 等。

---

## Agent 文件格式

每个 Agent 是一个 Markdown 文件，包含：
- 身份与性格特征（Identity & personality traits）
- 核心使命与工作流（Core mission & workflows）
- 带代码示例的交付物（Technical deliverables with code examples）
- 成功指标与沟通风格（Success metrics & communication style）

---

## 与同类项目的对比

| 项目 | 领域 | Agent 特点 | 多工具支持 |
|------|------|-----------|-----------|
| **The Agency** | 全业务（工程/设计/营销/销售）| 人格驱动，交付物导向 | 10+ 工具（含 OpenClaw） |
| **GStack** | 通用软件工程 | 流程驱动（sprint 闭环）| Claude Code 原生 |
| **CCGS** | 游戏开发 | 层级驱动（Director→Lead→Specialist） | Claude Code 原生 |

The Agency 的独特性在于：**宽度最大**（从工程师到广告买手到微信小程序开发者），**人格最强**（每个 Agent 有独特的声音和风格），**工具兼容最广**（覆盖所有主流 AI Coding 工具）。

---

## 值得关注的特殊 Agent

- **Feishu Integration Developer** — 飞书 Open Platform 专家，对主人的工作直接相关
- **Autonomous Optimization Architect** — LLM 路由 + 成本优化，与降本方向吻合（参见 Prompt-Caching 相关研究）
- **Reality Checker** — 专门对抗 AI Slop，防止生成内容脱离现实
- **Whimsy Injector** — 向产品注入个性和愉悦感的设计专家
- **Reddit Community Ninja** — Reddit 社区运营专家

---

## 安装使用

```bash
git clone https://github.com/msitarzewski/agency-agents.git
cd agency-agents

# 生成工具兼容格式（如需非 claude-code 工具）
./scripts/convert.sh

# 安装到指定工具
./scripts/install.sh --tool claude-code
# 或
./scripts/install.sh --tool openclaw

# 只安装特定部门
cp engineering/*.md ~/.claude/agents/
```

---

## 战略意义

The Agency 代表了 Agent 人设库的「水平扩张」路线：不追求单领域的深度流程化，而是用统一格式（Markdown + 人格定义）覆盖尽可能多的业务场景，最大化单个仓库对不同角色用户的覆盖率。

结合 [[GStack-虚拟工程团队]] 的纵深和 [[Claude-Code-Game-Studios]] 的垂直深度，三者构成了 AI Agent 团队配置的完整谱系：人格库 → 工程流程 → 垂直领域层级制。
