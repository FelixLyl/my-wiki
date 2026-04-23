---
type: concept
maturity: draft
date: 2026-04-23
updated: 2026-04-23
tags: [claude-code, llm-engineering, coding-agent, karpathy, best-practices, claude-md]
aliases: [karpathy-skills, Karpathy编码准则, LLM编码黄金准则]
sources: ["raw/karpathy-skills-forrestchang-2026-04-23.md"]
related: ["[[Andrej-Karpathy]]", "[[CLAUDE-md配置方法论]]", "[[Agent-Skills-addyosmani]]", "[[Superpowers]]", "[[Code-Simplifier]]"]
---

# Karpathy LLM 编码准则

Andrej Karpathy 对 LLM 在编码任务中的系统性缺陷进行了总结，并由 forrestchang 将其提炼为四条可执行准则，封装为单文件 `CLAUDE.md`，可作为 Claude Code 插件跨项目安装。

## Karpathy 的核心观察

LLM 在编码中存在三类结构性问题：

**问题一：隐藏假设**
> "The models make wrong assumptions on your behalf and just run along with them without checking. They don't manage their confusion, don't seek clarifications, don't surface inconsistencies, don't present tradeoffs, don't push back when they should."

**问题二：过度复杂化**
> "They really like to overcomplicate code and APIs, bloat abstractions, don't clean up dead code... implement a bloated construction over 1000 lines when 100 would do."

**问题三：正交性破坏**
"They still sometimes change/remove comments and code they don't sufficiently understand as side effects, even if orthogonal to the task."

## 四条准则

### 1. Think Before Coding — 编码前先思考

针对：隐藏假设、混乱、遗漏权衡

- 不确定时问，而非猜测
- 存在歧义时呈现多种解读，不静默选择
- 若有更简单方案，主动提出
- 遇到困惑，停下来说清楚什么地方不明确

### 2. Simplicity First — 简洁优先

针对：过度工程化、膨胀抽象

- 只做被要求的功能，不加推测性功能
- 单次使用的代码不加抽象层
- 不加没被要求的"灵活性"或"可配置性"
- 不为不可能发生的场景写错误处理
- 200 行能写成 50 行就重写
- **测试标准**：资深工程师会说"这太复杂了吗？"如果是，就简化

### 3. Surgical Changes — 手术式修改

针对：正交性编辑

编辑已有代码时：
- 不"改进"相邻代码、注释或格式
- 不重构没坏的东西
- 匹配现有代码风格
- 如果发现无关的死代码，提及它，不要删它

当改动产生孤儿时：
- 删除自己的改动造成的未使用 import/变量/函数
- 不删除已有的死代码，除非被要求

**测试标准**：每行改动都应直接对应用户的请求

### 4. Goal-Driven Execution — 目标驱动执行

针对：弱成功标准、频繁澄清

将命令式任务转化为可验证目标：

| 原始指令 | 转化为 |
|---------|--------|
| "Add validation" | "为无效输入写测试，再让测试通过" |
| "Fix the bug" | "写一个重现 bug 的测试，再让测试通过" |
| "Refactor X" | "确保重构前后测试都通过" |

多步任务陈述简短计划，每步附带验证点。

> "LLMs are exceptionally good at looping until they meet specific goals... Don't tell it what to do, give it success criteria and watch it go." — Karpathy

## 与其他 Skills 的对比

这四条准则与 [[Agent-Skills-addyosmani]] 形成互补关系：

| 维度 | Karpathy 准则 | addyosmani/agent-skills |
|------|-------------|-------------------------|
| 形态 | 单文件 CLAUDE.md | 20 个 Skills 包 |
| 覆盖 | 通用行为约束 | 完整开发生命周期 |
| 颗粒度 | 原则级（"为什么"） | 工作流级（"怎么做"） |
| 来源 | Karpathy 直接观察 | Google SWE Book 实践 |

[[Superpowers]] 中的"Subagent-Driven Development"与"Goal-Driven Execution"高度同构：两者都将任务定义为可循环验证的目标，而非命令序列。

[[Code-Simplifier]] 是"Simplicity First"在工具层的对应实现：Anthropic 官方插件，自动扫描并简化最近修改的代码。

## 安装方式

```
/plugin marketplace add forrestchang/andrej-karpathy-skills
/plugin install andrej-karpathy-skills@karpathy-skills
```

或单项目使用：
```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md
```
