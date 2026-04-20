---
type: insight
maturity: draft
date: 2026-04-20
sources: ["[[Claude-Code源码架构]]", "[[OpenClaw-Agent安全体系]]", "[[GStack-虚拟工程团队]]"]
tags: [agent-security, defense-in-depth, zero-trust, cso, claude-code, openclaw, gstack]
---

# Agent 安全的三层纵深防御

Agent 系统的安全问题在三个完全不同的抽象层上被独立解决，但这三层恰好形成了一套完整的纵深防御体系。将它们并排观察时，隐含的互补结构变得清晰。

## 三层防御的来源

**第一层：运行时内核层（Claude Code 源码架构）**

Claude Code 的 BashTool 在源码层面实现了四层安全防线和 7 步权限决策管线。权限逻辑是硬编码在 Agent 核心中的，不依赖外部检查——它更接近操作系统的内核态安全，无法被绕过，但也无法被配置者轻易扩展。这是 Agent 的"本体安全"。

**第二层：平台运行时层（OpenClaw Agent 安全体系）**

OpenClaw 的安全体系在 Agent 之外建了一层运行时拦截：`rm -rf`、防火墙修改等高危操作触发二次授权，Skill 安装前经过供应链扫描，密钥从不进入代码。这一层的核心价值是"将人工审批插入危险操作路径"，而非信任 Agent 的自我判断。它是 Agent 运行环境的安全，而非 Agent 本身的安全。

**第三层：专家 Agent 层（GStack /cso）**

GStack 的 `/cso` 把安全变成了一个可召唤的专家角色：OWASP Top 10 + STRIDE 威胁建模，8/10+ 置信度门槛，17 项误报过滤。安全不再是护栏，而是流程中的一个节点——在 build 之后、ship 之前必经的专家评审。这是"安全即服务"，可以随时召唤，也可以与其他角色并行。

## 三层之间的关系

三层的覆盖范围互不重叠：

| 层级 | 覆盖范围 | 拦截时机 | 灵活性 |
|------|---------|---------|--------|
| 内核层（Claude Code）| Agent 自身行为 | 执行前 | 低（硬编码）|
| 平台层（OpenClaw）| Agent 运行环境 | 执行前/后 | 中（配置化）|
| 专家层（GStack /cso）| 代码与设计决策 | 代码提交前 | 高（可对话）|

传统软件安全的纵深防御（边界→主机→应用）在 Agent 世界被映射为：**本体安全→运行时安全→决策安全**。

## 隐含的空白

三层防御覆盖了大部分攻击面，但存在一个共同盲区：**跨 Agent 调用链的信任传递**。

当 Agent A 调用 Agent B（如 GStack 的多 Agent 并行），内核层只管自己的沙箱，平台层难以感知跨 Agent 的意图链，专家 Agent 也只审查单次代码输出。Prompt Injection 在长调用链中的传播，目前没有任何一层能系统性覆盖。

这是下一代 Agent 安全体系需要解决的问题：调用链的信任 provenance 追踪，类似于软件供应链的 SBOM（Software Bill of Materials），但针对的是 Agent 调用意图，而非代码依赖。

## 实践建议

不需要选择其中一层部署——三层同时存在是正常的。部署优先级按成本/收益排序：

1. **内核层**：选用本身有权限系统的 Agent 框架（Claude Code、OpenClaw），开箱即得
2. **平台层**：部署 `openclaw-ops-guardrails`，解决最高频的误操作风险
3. **专家层**：在代码 review 流程中引入 `/cso` 或等价的安全检查节点

三层齐备时，攻击者需要同时穿透本体、环境和决策三道防线，攻击成本显著提高。
