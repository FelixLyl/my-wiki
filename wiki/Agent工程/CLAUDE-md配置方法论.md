---
type: concept
maturity: draft
date: 2026-04-05
updated: 2026-04-26
tags: [claude-code, claude-md, llm-engineering, configuration, agent-behavior]
aliases: [CLAUDE.md, Claude配置文件, claude.md行为规范]
sources: ["raw/AI编程.pdf", "raw/ai-research-list-2026-04-06.md", "raw/articles/claude-code-source-study-github.md", "raw/karpathy-skills-forrestchang-2026-04-23.md", "raw/AI 增强开发的三件套：OpenSpec + Superpowers + gstack.md"]
related: ["[[Claude-Agent-Teams]]", "[[Claude-Sub-Agents]]", "[[知识沉淀双轨机制]]", "[[OpenClaw-Skill生态]]", "[[Claude-Code源码架构]]", "[[Karpathy-LLM编码准则]]", "[[OpenSpec]]", "[[Superpowers]]", "[[GStack-虚拟工程团队]]"]
---

# CLAUDE.md 配置方法论

CLAUDE.md 是放置在项目根目录的配置文件，用于向 Claude Code 声明项目的工作协议。一份设计良好的 CLAUDE.md 相当于给 AI 植入了"思维逻辑"，让其从"推一下动一下"变为自主判断执行策略。

## 三层结构

### 决策层 — 编排策略

定义任务复杂度的评估标准和对应的执行模式：

| 复杂度 | 模式 | 说明 |
|--------|------|------|
| 复杂/全栈任务 | 自动组建 [[Claude-Agent-Teams]] | 多模块、多维度审查 |
| 耗时/隔离任务 | 动态创建 [[Claude-Sub-Agents]] | 上下文隔离、脏活累活 |
| 简单任务 | 直接执行 | 单文件修改、简单问答 |

### 执行层 — 技能与规范

- **Skills 优先**：执行特定任务前，先检索 `skills/` 目录下的技能文档
- **Rules 红线**：严格遵守 `rules/` 目录下的代码规范，任何代码生成必须通过 Rules 检查

### 进化层 — 知识沉淀

参见 [[知识沉淀双轨机制]]。

## 通用开发准则（建议写入）

- **代码质量**：简洁、模块化、可测试
- **错误处理**：不忽略错误，不使用 TODO 占位符，必须实现完整逻辑
- **文件操作**：修改现有文件保持原有风格；新建文件确保目录结构清晰
- **沟通风格**：专业、直接、以结果为导向，不过度解释显而易见的代码

## 使用方式

1. 将配置内容保存为项目根目录下的 `CLAUDE.md`
2. 在 `.claude/settings.json` 中开启 Agent Teams 功能
3. 用复杂任务验证：如"帮我开发一个用户积分系统，包括数据库设计和前后端接口"

## Agent 行为规范（4 条黄金准则）

来自调研素材中的高质量提示词实践，适合直接写入 CLAUDE.md：

1. **拒绝假设**：遇到不确定时，先提问而不是猜测
2. **追求最优**：不满足于"能用"，主动寻找最佳解法
3. **追根溯源**：遇到 Bug 先找根因，不打补丁
4. **极致简洁**：回复直击要点，杜绝冗余

这 4 条规范写入 CLAUDE.md 即刻生效，直接影响 Agent 输出质量，成本为零。

## Karpathy 四条编码准则

[[Karpathy-LLM编码准则]] 是对上述黄金准则的深化，由 Andrej Karpathy 对 LLM 编码缺陷的直接观察提炼而来，聚焦于：

- **Think Before Coding** — 假设显性化，不确定性先问
- **Simplicity First** — 最小代码，禁止推测性功能
- **Surgical Changes** — 只改请求范围内的代码，不顺手"改进"
- **Goal-Driven Execution** — 把指令转化为可验证目标，让 LLM 自主循环直到达成

与上述黄金准则的区别在于颗粒度：Karpathy 准则有具体的"测试标准"（如"资深工程师会说这太复杂吗？"），可直接作为 CLAUDE.md 内容使用。

## 与 Skills 的关系

[[OpenClaw-Skill生态]] 中的 Skills 是 CLAUDE.md "执行层"的具体实现形式。CLAUDE.md 声明"执行某类任务时优先调用对应 Skill"，Skills 则提供具体执行能力（如 prd、prd-to-issues、sql-optimization 等）。两者配合构成完整的 AI 工程师工作台。

## 源码级实现：System Prompt 工程

Claude Code 源码分析（参见 [[Claude-Code源码架构]]）揭示了 CLAUDE.md 在产品内部的实际处理方式。System Prompt 采用分段构建策略：静态规范部分作为 Prompt Cache 的缓存前缀，动态注入的项目配置（即 CLAUDE.md 内容）按优先级排列。Settings 系统实现了 5+1 层配置合并，CLAUDE.md 是其中面向用户的声明层，最终与内置规则、MDM 策略等合并为完整的行为指令。

## 多 Skill 裁决配置实例

笨小葱在三件套实战中提出：要让 [[OpenSpec]] + [[Superpowers]] + [[GStack-虚拟工程团队]] 真正协同运行，CLAUDE.md 需要写明**裁决规则**，告诉 Claude 遇到什么情况该走哪个 Skill。核心原则包括：

1. **规范先行**：任何需求变更必须先过 OpenSpec（/opsx:propose），产出 proposal + design + tasks 三件套
2. **流程归 Superpowers**：brainstorm、plan、debug、TDD、verify、code review 走 Superpowers
3. **执行归 gstack**：浏览器、QA、ship、deploy、canary 走 gstack
4. **独立 reviewer 通道**：verification 和 code-review 分两个 pass，不在同一上下文合并
5. **证据优先**：没有测试/截图/QA 报告不算完成
6. **最短路径优先**：能用一个 skill 解决的，不升级为完整闭环

任务分流建议：只读任务直接处理；轻量任务跳过完整流程直接实现；中任务走 OpenSpec → 简短 brainstorming → 实现 → 验证；大任务走完整闭环含 worktrees + TDD + code-review + /ship + /canary。

## 设计哲学

CLAUDE.md 的本质是让 AI 成为项目的智能研发主管，而非单纯的代码生成工具。它通过声明式配置，将项目的工程规范、协作模式和知识沉淀策略内化为 AI 的行为准则。
