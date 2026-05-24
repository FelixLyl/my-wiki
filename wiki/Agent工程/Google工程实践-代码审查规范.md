---
type: concept
maturity: draft
date: 2026-05-24
updated: 2026-05-24
tags: [code-review, engineering-practices, google, swe, team-collaboration, cl, lgtm]
aliases: [Google代码审查, eng-practices, Google工程实践, CL审查规范]
sources: ["https://github.com/google/eng-practices"]
related: ["[[Karpathy-LLM编码准则]]", "[[CLAUDE-md配置方法论]]", "[[超能力技能集]]", "[[Agent技能-addyosmani]]", "[[自学习复盘模式]]"]
---

# Google 工程实践 — 代码审查规范

google/eng-practices 是 Google 将内部工程最佳实践公开的文档集，目前包含代码审查（Code Review）指南，分为审查者视角（Reviewer's Guide）和提交者视角（CL Author's Guide）两套文档。CC-By 3.0 开源。

> Google has many generalized engineering practices that cover all languages and all projects. These documents represent our collective experience of various best practices that we have developed over time.

**核心术语：**
- CL（Changelist）：一个提交单元，等同于 PR（Pull Request）或 patch
- LGTM（Looks Good to Me）：审查者表示批准

## 代码审查的根本目标

代码审查的首要目的是持续改善代码库的整体健康度（code health）。所有工具和流程都服务于这一目标。核心准则：

> In general, reviewers should favor approving a CL once it is in a state where it definitely improves the overall code health of the system being worked on, even if the CL isn't perfect.

**没有"完美"代码，只有"更好"的代码。** 审查者不应要求作者打磨每一处细节，应在推进进展与提出改进意见之间保持平衡。

## 审查者指南

### 审查什么

按优先级：

1. **设计（Design）**：CL 的整体设计是否合理？各模块交互是否合理？是否属于该代码库还是应进库？时机是否合适？
2. **功能（Functionality）**：CL 是否实现了开发者意图？意图对用户是否有益？需考虑边界情况、并发问题。
3. **复杂度（Complexity）**：是否比必要的更复杂？"复杂"= 阅读者无法快速理解，或调用/修改时容易引入 bug。特别警惕过度工程化（over-engineering）：解决未来可能的问题，而非当前已知的问题。
4. **测试（Tests）**：是否有合适的单元/集成/端到端测试？测试本身是否正确、有意义、不会产生假阳性？测试也是需要维护的代码。
5. **命名与注释（Naming & Comments）**：命名是否清晰？注释是否解释"为什么"而非"是什么"？代码能自解释时不需注释。
6. **风格（Style）**：遵循 Google 风格指南，非必要风格建议加 `Nit:` 前缀。
7. **文档（Documentation）**：构建方式、测试方式、发布方式如有变化，需同步更新文档。

### 逐行审查原则

> Look at every line of code you have been assigned to review.

对于人工编写的类、函数、代码块，不能跳过扫视。如果代码难以理解，要求作者澄清——让代码可读，是在帮助未来所有读代码的人。

### 注释写法

- 对事不对人：评论代码，不评论开发者。
- 解释"为什么"：说明你的建议如何改善代码健康度。
- 分级标注：
  - `Nit:` — 小问题，可忽略
  - `Optional:` / `Consider:` — 建议但非强制
  - `FYI:` — 供参考，本 CL 无需处理
- 鼓励好的实践，不仅指出问题。

### 审查速度

> We optimize for the speed at which a team of developers can produce a product together, as opposed to optimizing for the speed at which an individual developer can write code.

单次审查响应不超过一个工作日（次日上班第一件事）。慢速审查会导致：团队整体速度下降、开发者对审查流程产生抵触、代码质量反而因此下滑（被迫降低标准）。

**专注与响应的平衡：** 如果正在深度编码中，不要中断自己——等待一个自然断点再去审查。研究表明从中断中恢复需要大量时间，打断自己对团队的代价比让对方等一等更高。

**LGTM with Comments：** 当剩余意见都是非阻塞性修改，或跨时区协作时，可以在留注释的同时给出 LGTM，避免对方等待一整天。

### 处理 Pushback

- 开发者对代码更熟悉，若其论点有道理，接受并放弃原意见。
- 若审查者仍确信改进有价值，继续解释，但保持礼貌。
- "稍后再改"通常不会兑现——如果引入了新的复杂度，应要求在本 CL 完成，而非留下技术债。

## 作者指南

### 小 CL 原则

> Reviewers rarely complain about getting CLs that are too small.

小 CL 的优点：审查更快、审查更彻底、更不容易引入 bug、被拒绝浪费更少、更易合并、更易回滚。

**CL 大小参考：** 100 行通常合理，1000 行通常太大。不存在绝对数字，取决于审查者判断。

**拆分策略：**
- 堆叠（Stacking）：先提交 CL-1，在其基础上写 CL-2，两者可并行审查
- 按文件拆分：不同模块发给不同审查者
- 水平拆分（Horizontal）：按技术层分（proto → service → client）
- 垂直拆分（Vertical）：按功能特性分（乘法 vs 除法各一个 CL）
- 重构和功能变更分开：先重构再功能，方便审查者理解变化意图

### CL 描述写法

CL 描述是版本历史的永久记录，可能被数百人在未来数年内阅读。

**第一行规范：**
- 简短，完整句，祈使语气（"Delete the FizzBuzz RPC..."）
- 独立成行即可被理解，无需阅读全文

**正文：**
- 说明改变了什么（what）和为什么（why）
- 包含不反映在代码中的决策背景
- 引用 bug 编号、benchmark 结果、设计文档

**反例：**"Fix bug"、"Add patch"、"Moving code from A to B" — 均不足，无法从历史中检索。

### 处理审查意见

- 不要把审查意见当成人身攻击，审查者是在帮助代码库。
- 如果代码让审查者困惑，首先修改代码本身，其次加注释，最后才在审查工具中解释（审查工具中的解释对未来读者无帮助）。
- 遇到不认同的意见：思考而非防御，表述自己的论据和权衡，与审查者协作找到更好方案。

## 与 AI 编码的关联

Google 工程实践中"代码健康度持续改善"的目标，与 [[Karpathy-LLM编码准则]] 中的"Simplicity First"和"Surgical Changes"高度同构：
- Karpathy 针对 LLM 编码的三大缺陷（隐藏假设、过度复杂化、正交性破坏），恰好对应 Google 代码审查关注的：功能正确性（隐藏假设）、复杂度（过度复杂化）、手术式修改（正交性破坏）。
- [[CLAUDE-md配置方法论]] 的"植入三层思维逻辑"正是在 Agent 层面复现 Google 代码审查中对开发者的行为约束。

[[Agent技能-addyosmani]] 直接援引 Google SWE Book（Google 软件工程实践的系统化总结），eng-practices 是其代码审查部分的公开版本。

`"Cleaning It Up Later"` 原则与 [[自学习复盘模式]] 中"错误卡片即时记录"逻辑一致：都在阻止技术债/知识债被推迟——推迟就是永远不做。
