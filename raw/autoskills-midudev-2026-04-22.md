# midudev/autoskills

> 来源: https://github.com/midudev/autoskills
> 官网: https://autoskills.sh
> 抓取时间: 2026-04-22

## 简介

One command. Your entire AI skill stack. Installed.

一条命令，扫描项目技术栈，自动安装最适合的 AI Agent Skills。

```bash
npx autoskills
```

## 工作原理

1. 在项目根目录运行 `npx autoskills`
2. 扫描 `package.json`、Gradle 文件、各类配置文件，检测技术栈
3. 从 [skills.sh](https://skills.sh) 自动安装最匹配的 AI Agent Skills
4. 若检测到 Claude Code，自动生成 `CLAUDE.md`（汇总 `.claude/skills/` 下所有已安装 markdown）

零配置，开箱即用。

## CLI 参数

| 参数 | 说明 |
|------|------|
| -y, --yes | 跳过确认提示 |
| --dry-run | 预览将安装什么，不实际安装 |
| -h, --help | 显示帮助 |
| -a claude-code | 指定目标 agent，触发 CLAUDE.md 生成 |

## 支持的技术栈

**Frameworks & UI:** React, Next.js, Vue, Nuxt, Svelte, Angular, Astro, Tailwind CSS, shadcn/ui, GSAP, Three.js

**Languages & Runtimes:** TypeScript, Node.js, Go, Bun, Deno, Dart

**Backend & APIs:** Express, Hono, NestJS, Spring Boot

**Mobile & Desktop:** Expo, React Native, Flutter, SwiftUI, Android, Kotlin Multiplatform, Tauri, Electron

**Data & Storage:** Supabase, Neon, Prisma, Drizzle ORM, Zod, React Hook Form

**Auth & Billing:** Better Auth, Clerk, Stripe

**Testing:** Vitest, Playwright

**Cloud & Infrastructure:** Vercel, Vercel AI SDK, Cloudflare, Durable Objects, Cloudflare Agents, Cloudflare AI, AWS, Azure, Terraform

**Tooling:** Turborepo, Vite, oxlint

**Media & AI:** Remotion, ElevenLabs

## 项目信息

- GitHub: https://github.com/midudev/autoskills
- 官网: https://autoskills.sh
- Skills 源: https://skills.sh
- 作者: midudev（西班牙知名前端开发者/YouTuber）
- License: CC BY-NC 4.0
- 前置条件: Node.js >= 22
