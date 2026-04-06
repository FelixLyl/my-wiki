---
type: concept
maturity: draft
date: 2026-04-06
updated: 2026-04-06
tags: [security, openclaw, agent, zero-trust, supply-chain, skill-security, audit]
aliases: [Agent安全, SecureClaw, OpenClaw安全, 供应链安全, 零信任Agent]
sources: ["raw/ai-research-list-2026-04-06.md"]
related: ["[[OpenClaw-Skill生态]]", "[[内部系统Agent集成]]", "[[Claude-Agent-Teams]]"]
---

# OpenClaw Agent 安全体系

高权限自主 Agent 的安全问题不同于传统软件安全：Agent 可以自主执行命令、访问文件、调用外部服务，其风险面包含恶意 Skill（供应链攻击）、Prompt Injection（提示词注入）、密钥泄露和误操作高危命令。一套完整的 Agent 安全体系需要覆盖这几个维度。

## 安全工具矩阵

### 安装前防线：Skill 供应链安全

| 工具 | 职责 |
|------|------|
| **skill-vetter** | 安装新 Skill 前扫描：网络外发/敏感环境变量/写系统目录/可疑 base64 |
| **skill-guard** | 自动扫描 `.js/.sh` 文件，检测是否藏有后门 |
| **SecureClaw** | `npm install -g secureclaw`，扫描已安装 Skills 的安全风险，能拦截已知攻击模式 |

三工具覆盖安装前（vetter/guard）和安装后（SecureClaw）两个阶段。

### 运行时防线：高危操作拦截

**openclaw-ops-guardrails**：线上环境必装的运维护栏。`rm -rf`、防火墙修改等高危操作前强制触发拦截，申请二次授权后方可执行。这一机制的核心价值是将"人工审批"插入 Agent 的危险操作路径，而非寄希望于 Agent 自身判断。

### 密钥管理

线上部署 OpenClaw 前必须解决密钥安全问题。可选方案：
- `.env` 文件（最低成本，但仍有泄露风险）
- Secret Manager（云平台托管，推荐）
- Vault（HashiCorp，自托管，复杂度高）

Docker 容器场景需额外注意：密钥不可硬编码到镜像，通过环境变量或 Mount 注入。

### 专项防护

- **openclaw-shield**：防护机制待调研，适用场景关注 Prompt Injection 和 Agent 越权行为
- **慢雾 OpenClaw 安全实践指南**：专为高权限自主 Agent 定制，零信任架构，涵盖 Prompt Injection 防御和供应链安全。慢雾出品，可直接喂给 Agent 部署

## 安全审计

**在 Docker 中构建专职安全审计员**：设计一个独立的 OpenClaw 实例，专职定期检查同环境其他实例的安全隐患。建议在密钥管理方案确定（openclaw-ops-guardrails 部署）后再实施，避免审计员本身成为攻击面。

## 零信任原则

高权限 Agent 的安全设计应遵循零信任：
1. **不信任任何输入**：外部内容（网页、邮件、文档）均可能含 Prompt Injection
2. **最小权限**：Agent 只获得完成任务所需的最小权限
3. **操作可逆**：优先 `trash` 而非 `rm`，优先干跑（dry-run）而非直接执行
4. **人在回路**：高危操作强制二次确认

## 优先部署顺序

1. skill-vetter（安装任何新 Skill 前先扫描）
2. openclaw-ops-guardrails（线上必装）
3. 密钥管理方案（上线前）
4. SecureClaw（定期巡检）
5. 慢雾安全指南（深入阅读）
6. openclaw-shield / 安全审计员（视需要）
