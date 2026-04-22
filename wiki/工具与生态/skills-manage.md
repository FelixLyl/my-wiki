---
type: project
maturity: draft
date: 2026-04-22
updated: 2026-04-22
tags: [skills, skill-manager, desktop-app, tauri, openclaw, cross-platform, gui, sqlite]
aliases: [skills-manage, iamzhihuix/skills-manage, Skills管理桌面应用]
sources: ["raw/skills-manage-iamzhihuix-2026-04-22.md"]
related: ["[[OpenClaw-Skill生态]]", "[[autoskills]]", "[[Agent-Skills-addyosmani]]", "[[Hermes-Agent]]"]
---

# skills-manage

iamzhihuix/skills-manage 是一个 Tauri 桌面应用，用于从一个地方统一管理跨 20+ AI Coding Agent 平台的 Skills。遵循 Agent Skills 开放标准，以 `~/.agents/skills/` 为中央目录，通过符号链接同步到各平台，实现「一个来源，驱动所有工具」。

## 核心功能

- **中央 Skill 库**：统一管理，按平台安装/卸载
- **Skill 详情页**：Markdown 预览 + raw 源码 + AI 解释生成（调用本地 LLM API）
- **Collections（合集）**：按主题组织 Skills，批量一键安装到指定平台
- **Discover scan**：扫描本地磁盘，发现已有的项目级 Skill 库
- **Marketplace 浏览**：从市场或 GitHub 仓库导入 Skills（支持认证和重试）
- **快速搜索**：延迟查询 + 懒加载 + 虚拟化，大型 Skill 库也流畅
- **双语 UI**（中/英）+ Catppuccin 主题 + 自定义强调色

## 支持平台（27个）

### Coding 类（21个）

Claude Code、Codex CLI、Cursor、Gemini CLI、Trae、Factory Droid、Junie、Qwen、Trae CN、Windsurf、Qoder、Augment、OpenCode、KiloCode、OB1、Amp、Kiro、CodeBuddy、Hermes、Copilot、Aider

### 龙虾系列（Lobster，6个）⭐

| 平台 | 目录 |
|------|------|
| **OpenClaw (开爪)** | ~/.openclaw/skills/ |
| QClaw (千爪) | ~/.qclaw/skills/ |
| EasyClaw (简爪) | ~/.easyclaw/skills/ |
| EasyClaw V2 | ~/.easyclaw-20260322-01/skills/ |
| AutoClaw | ~/.openclaw-autoclaw/skills/ |
| WorkBuddy (打工搭子) | ~/.workbuddy/skills-marketplace/skills/ |

可在 Settings 中添加自定义平台。

## 隐私与安全设计

- **本地优先**：所有数据存在 `~/.skillsmanage/db.sqlite`，不上传
- **无遥测**：无分析、崩溃报告、使用追踪
- **按需联网**：仅在主动操作（Marketplace 同步、GitHub 导入、AI 解释）时联网
- **凭据本地**：GitHub PAT 和 AI API Key 存在本地 SQLite（未加密，注意勿截图泄露）

## 技术栈

| 层 | 技术 |
|----|------|
| 桌面框架 | Tauri v2 |
| 前端 | React 19, TypeScript, Tailwind CSS 4 |
| UI | shadcn/ui, Lucide icons |
| 状态 | Zustand |
| i18n | react-i18next |
| 主题 | Catppuccin 4-flavor palette |
| 后端 | Rust (serde, sqlx, chrono, uuid) |
| 数据库 | SQLite via sqlx (WAL 模式) |
| 路由 | react-router-dom v7 |

## 安装与运行

- **下载**：[最新 Release](https://github.com/iamzhihuix/skills-manage/releases/latest)（当前仅 Apple Silicon macOS .dmg/.app.zip）
- **其他平台**：从源码构建（需要 Node.js LTS + pnpm + Rust stable）
- **macOS Gatekeeper 问题**（未公证）：`xattr -dr com.apple.quarantine "/Applications/skills-manage.app"`

## 与 autoskills 的关系

两者在 Skills 生态中定位互补：

| 维度 | skills-manage | [[autoskills]] |
|------|--------------|---------------|
| 形态 | 桌面 GUI 应用 | CLI 工具（npx） |
| 范围 | 全局跨平台 Skill 库管理 | 项目级自动检测安装 |
| 使用频率 | 持续维护 | 项目初始化时一次性 |
| 平台支持 | 20+ AI 工具（含龙虾系列） | Claude Code 为主 |

## 意义

skills-manage 的出现标志着 AI Agent Skills 生态已经进入「工具链配套」阶段——不再只是写 Skill、用 Skill，而是需要专门的管理工具来处理跨平台安装、版本同步、发现浏览等基础设施问题。龙虾系列（OpenClaw 族）被单独列为一个品类，说明 OpenClaw 生态在 Skill 工具链中已有独立的存在感。

## 来源

- GitHub: https://github.com/iamzhihuix/skills-manage
- 中文文档: https://github.com/iamzhihuix/skills-manage/blob/main/README_CN.md
- License: Apache 2.0
