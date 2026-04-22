# iamzhihuix/skills-manage

> 来源: https://github.com/iamzhihuix/skills-manage
> 抓取时间: 2026-04-22

## 简介

skills-manage 是一个 Tauri 桌面应用，用于从一个地方统一管理跨多平台的 AI Coding Agent Skills。

支持 Claude Code、Cursor、Gemini CLI、Codex、OpenCode、OpenClaw 等 20+ 平台，遵循 Agent Skills 开放标准（`~/.agents/skills/` 作为中央目录），通过符号链接安装到各平台。

## 核心功能

- **中央 skill 库** + 各平台独立安装/卸载流程
- **完整 Skill 详情**：Markdown 预览、raw 源码、AI 解释生成
- **Collections（合集）**：组织 Skills 并批量安装到平台
- **Discover scan**：扫描本地磁盘上的项目级 Skill 库
- **Marketplace 浏览**：GitHub 仓库导入，支持认证请求和重试回退
- **快速搜索**：延迟查询 + 懒加载索引 + 虚拟化，支持大型 Skill 库
- **双语 UI**（中/英）+ Catppuccin 主题 + 强调色 + 引导流程 + 响应式导航

## 支持的平台（27个）

### Coding 类
| 平台 | Skills 目录 |
|------|------------|
| Claude Code | ~/.claude/skills/ |
| Codex CLI | ~/.agents/skills/ |
| Cursor | ~/.cursor/skills/ |
| Gemini CLI | ~/.gemini/skills/ |
| Trae | ~/.trae/skills/ |
| Factory Droid | ~/.factory/skills/ |
| Junie | ~/.junie/skills/ |
| Qwen | ~/.qwen/skills/ |
| Trae CN | ~/.trae-cn/skills/ |
| Windsurf | ~/.windsurf/skills/ |
| Qoder | ~/.qoder/skills/ |
| Augment | ~/.augment/skills/ |
| OpenCode | ~/.opencode/skills/ |
| KiloCode | ~/.kilocode/skills/ |
| OB1 | ~/.ob1/skills/ |
| Amp | ~/.amp/skills/ |
| Kiro | ~/.kiro/skills/ |
| CodeBuddy | ~/.codebuddy/skills/ |
| Hermes | ~/.hermes/skills/ |
| Copilot | ~/.copilot/skills/ |
| Aider | ~/.aider/skills/ |

### Lobster 类（龙虾系列！）
| 平台 | Skills 目录 |
|------|------------|
| OpenClaw (开爪) | ~/.openclaw/skills/ |
| QClaw (千爪) | ~/.qclaw/skills/ |
| EasyClaw (简爪) | ~/.easyclaw/skills/ |
| EasyClaw V2 | ~/.easyclaw-20260322-01/skills/ |
| AutoClaw | ~/.openclaw-autoclaw/skills/ |
| WorkBuddy (打工搭子) | ~/.workbuddy/skills-marketplace/skills/ |

### Central
| 平台 | Skills 目录 |
|------|------------|
| Central Skills | ~/.agents/skills/ |

支持自定义平台（通过 Settings 添加）。

## 隐私与安全

- **本地优先**：元数据/Collections/扫描结果/设置/AI 解释缓存全部存在 `~/.skillsmanage/db.sqlite`
- **无遥测**：无分析、崩溃报告或使用追踪
- **按需联网**：仅在显式使用 Marketplace 同步/下载、GitHub 导入、AI 解释生成时才发起请求
- **凭据本地存储**：GitHub PAT 和 AI API Key 存在 SQLite settings 表中，未加密

## 技术栈

| 层 | 技术 |
|----|------|
| 桌面框架 | Tauri v2 |
| 前端 | React 19, TypeScript, Tailwind CSS 4 |
| UI 组件 | shadcn/ui, Lucide icons |
| 状态管理 | Zustand |
| i18n | react-i18next |
| 主题 | Catppuccin 4-flavor palette |
| 后端 | Rust (serde, sqlx, chrono, uuid) |
| 数据库 | SQLite via sqlx (WAL mode) |
| 路由 | react-router-dom v7 |

## 安装

- 下载最新 Release（当前仅 Apple Silicon macOS .dmg/.app.zip）
- macOS Gatekeeper 问题：`xattr -dr com.apple.quarantine "/Applications/skills-manage.app"`
- 其他平台：从源码构建

## 项目信息

- GitHub: https://github.com/iamzhihuix/skills-manage
- License: Apache 2.0
- 数据库: ~/.skillsmanage/db.sqlite
