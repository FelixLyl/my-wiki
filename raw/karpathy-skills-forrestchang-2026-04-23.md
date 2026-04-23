# andrej-karpathy-skills — CLAUDE.md 编码准则

来源：https://github.com/forrestchang/andrej-karpathy-skills/tree/main
作者：forrestchang（Jiayuan）
抓取日期：2026-04-23

---

## 核心定位

一个单文件 CLAUDE.md，用于改善 Claude Code 行为，内容源自 Andrej Karpathy 对 LLM 编码缺陷的观察。

也可作为 Claude Code 插件安装：
```
/plugin marketplace add forrestchang/andrej-karpathy-skills
/plugin install andrej-karpathy-skills@karpathy-skills
```

---

## Karpathy 原始观察

> "The models make wrong assumptions on your behalf and just run along with them without checking. They don't manage their confusion, don't seek clarifications, don't surface inconsistencies, don't present tradeoffs, don't push back when they should."

> "They really like to overcomplicate code and APIs, bloat abstractions, don't clean up dead code... implement a bloated construction over 1000 lines when 100 would do."

> "They still sometimes change/remove comments and code they don't sufficiently understand as side effects, even if orthogonal to the task."

---

## 四条准则

### 1. Think Before Coding（编码前先思考）
针对：隐藏假设、混乱、遗漏权衡

- 显式陈述假设，不确定时问而不猜
- 存在歧义时呈现多种解读，不要静默选择
- 若有更简单方案存在，主动说出来
- 遇到困惑时停下来，说清楚什么地方不明确

### 2. Simplicity First（简洁优先）
针对：过度工程化、膨胀抽象

- 只做被要求的功能，不加任何推测性功能
- 单次使用的代码不加抽象层
- 不加没被要求的"灵活性"或"可配置性"
- 不为不可能发生的场景写错误处理
- 200 行能写成 50 行就重写
- 测试标准：资深工程师看完会说"这太复杂了吗？" 如果是，就简化

### 3. Surgical Changes（手术式修改）
针对：正交性编辑、改了不该改的地方

编辑已有代码时：
- 不"改进"相邻代码、注释或格式
- 不重构没坏的东西
- 匹配现有代码风格，即使你会用不同写法
- 如果发现无关的死代码，提及它，不要删它

当你的改动产生孤儿时：
- 删除你的改动造成的未使用 import/变量/函数
- 不删除已有的死代码，除非被要求

测试标准：每行改动都应直接对应用户的请求

### 4. Goal-Driven Execution（目标驱动执行）
针对：弱成功标准、需要频繁澄清

将命令式任务转化为可验证目标：

| 原始指令 | 转化为 |
|---------|--------|
| "Add validation" | "为无效输入写测试，再让测试通过" |
| "Fix the bug" | "写一个重现 bug 的测试，再让测试通过" |
| "Refactor X" | "确保重构前后测试都通过" |

多步任务时陈述简短计划：
1. [步骤] → 验证：[检查点]
2. [步骤] → 验证：[检查点]

> "LLMs are exceptionally good at looping until they meet specific goals... Don't tell it what to do, give it success criteria and watch it go."（Karpathy）

---

## 适用场景

- 这些准则偏向谨慎而非速度
- 对于简单任务（明显的一行修改），不需要全套流程
- 目的是减少非简单工作中的代价高昂的错误

---

## 安装方式

**Option A: Claude Code Plugin（推荐，跨项目）**
```
/plugin marketplace add forrestchang/andrej-karpathy-skills
/plugin install andrej-karpathy-skills@karpathy-skills
```

**Option B: CLAUDE.md（单项目）**
```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/forrestchang/andrej-karpathy-skills/main/CLAUDE.md
```

**Cursor 支持：** 仓库内置 `.cursor/rules/karpathy-guidelines.mdc`

---

## 关联项目

- 作者另有项目 [Multica](https://github.com/multica-ai/multica)：开源平台，用于运行和管理带有可复用 Skills 的编码 Agent
