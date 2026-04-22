---
type: project
maturity: draft
date: 2026-04-22
updated: 2026-04-22
tags: [skills, skill-installer, cli, auto-detect, claude-code, skills.sh, tooling]
aliases: [autoskills, midudev/autoskills, npx-autoskills]
sources: ["raw/autoskills-midudev-2026-04-22.md"]
related: ["[[OpenClaw-Skill生态]]", "[[skills-manage]]", "[[Agent-Skills-addyosmani]]", "[[CLAUDE-md配置方法论]]"]
---

# autoskills

midudev/autoskills 是一个一行命令的 AI Agent Skills 自动安装工具：扫描项目技术栈，从 [skills.sh](https://skills.sh) 自动安装最匹配的 Skills，并为 Claude Code 生成 `CLAUDE.md`。

> One command. Your entire AI skill stack. Installed.

```bash
npx autoskills
```

## 工作原理

1. 扫描项目根目录下的 `package.json`、Gradle 文件及各类配置文件，识别技术栈
2. 从 [skills.sh](https://skills.sh) 查找并安装最匹配的 AI Agent Skills
3. 若检测到 Claude Code（或通过 `-a claude-code` 指定），自动生成 `CLAUDE.md`，汇总 `.claude/skills/` 下所有已安装 markdown

零配置，开箱即用，Node.js >= 22。

## CLI 参数

| 参数 | 说明 |
|------|------|
| `-y, --yes` | 跳过确认提示，直接安装 |
| `--dry-run` | 预览将安装什么，不实际安装 |
| `-a claude-code` | 指定目标 Agent，触发 CLAUDE.md 生成 |
| `-h, --help` | 显示帮助 |

## 支持的技术栈（自动检测）

覆盖现代全栈开发主流技术：

- **前端框架**：React, Next.js, Vue, Nuxt, Svelte, Angular, Astro, Tailwind CSS, shadcn/ui, GSAP, Three.js
- **语言与运行时**：TypeScript, Node.js, Go, Bun, Deno, Dart
- **后端/API**：Express, Hono, NestJS, Spring Boot
- **移动/桌面**：Expo, React Native, Flutter, SwiftUI, Android, Kotlin Multiplatform, Tauri, Electron
- **数据与存储**：Supabase, Neon, Prisma, Drizzle ORM, Zod, React Hook Form
- **认证/支付**：Better Auth, Clerk, Stripe
- **测试**：Vitest, Playwright
- **云/基础设施**：Vercel, Cloudflare (含 Agents/AI/Durable Objects), AWS, Azure, Terraform
- **工具链**：Turborepo, Vite, oxlint
- **媒体/AI**：Remotion, ElevenLabs

## 与 skills-manage 的关系

两者解决同一生态问题的不同角度：

| 工具 | 定位 | 使用场景 |
|------|------|---------|
| **autoskills**（本文） | 项目级自动安装 CLI | 新项目初始化，一行命令完成配置 |
| **[[skills-manage]]** | 跨平台可视化管理桌面应用 | 长期维护、跨工具统一管理 Skills 库 |

两者可以互补：autoskills 快速安装到项目，skills-manage 负责全局库的可视化管理和跨平台同步。

## 来源

- GitHub: https://github.com/midudev/autoskills
- 官网: https://autoskills.sh
- Skills 源: https://skills.sh
- 作者: midudev（西班牙知名前端开发者/YouTuber，500K+ YouTube 订阅）
- License: CC BY-NC 4.0
