---
type: project
maturity: draft
date: 2026-04-18
updated: 2026-04-18
tags: [video, html, ai-agent, open-source, heygen, gsap, rendering]
aliases: [hyperframes, HTML转视频, HTML-to-video]
sources: ["raw/hyperframes-heygen-2026-04-18.md"]
related: ["[[OpenClaw-Skill生态]]", "[[内部系统Agent集成]]", "[[AI内容创作工作流]]"]
---

# HyperFrames

HyperFrames 是 HeyGen 于 2026年4月17日开源的 HTML 视频渲染框架，口号"Write HTML. Render video. Built for agents."概括了它的定位：让 AI Agent 用 HTML 写视频。

## 核心洞见

传统视频工具（After Effects、DaVinci Resolve）存储格式对 AI 不友好：专有二进制格式、嵌套 JSON 场景图、训练数据极少。LLM 恰恰在 HTML/CSS/JS/GSAP 上受过海量训练，天然擅长写出可视化动效代码。HyperFrames 在 HTML 上增加少量 `data-*` 时间轴属性，把"定义网页"和"定义视频"统一为同一种操作。

> "HTML is the source of truth for video."

## 工作原理

一个 HyperFrames 视频就是一个 HTML 文件，核心属性定义时间轴：

| 属性 | 作用 |
|------|------|
| `data-composition-id` | 合成唯一标识 |
| `data-start` | 片段开始时间（秒） |
| `data-duration` | 片段持续时间（秒） |
| `data-track-index` | 轨道编号，同轨不重叠 |
| `data-width` / `data-height` | 输出分辨率 |

渲染管道：无头 Chrome 通过 `beginFrame` API 逐帧截图 → FFmpeg 编码输出 MP4/MOV/WebM。确定性设计：`frame = floor(time * fps)`，同一输入永远产出相同输出，适合自动化流水线。

## 包结构

| 包 | 职责 |
|----|------|
| `hyperframes`（CLI） | 创建、预览、lint、渲染 |
| `@hyperframes/core` | 类型、解析、运行时、linter |
| `@hyperframes/engine` | 逐帧捕获引擎（Puppeteer + FFmpeg） |
| `@hyperframes/producer` | 完整渲染管道（捕获 + 编码 + 混音） |
| `@hyperframes/studio` | 浏览器端可视化编辑 UI |
| `@hyperframes/player` | 可嵌入 Web Component |
| `@hyperframes/shader-transitions` | WebGL 着色器场景转场 |

## 主要能力

- **动画** — 通过 GSAP、Lottie、CSS 或任意可 seek 的运行时（Frame Adapter 模式）
- **字幕/歌词** — 逐字与音频对齐（karaoke 风格）
- **TTS 配音** — 内置 Kokoro-82M 文字转语音
- **音频响应式** — 动效随音乐节拍脉冲/发光
- **50+ 预制组件** — `npx hyperframes add <block>`，含社交 overlay、shader 转场、数据图表
- **WCAG 检测** — `npx hyperframes validate` 自动截图检测文字对比度

## 与 AI Agent 的集成

以 [[OpenClaw-Skill生态|Skills]] 包形式发布，一条命令安装：

```bash
npx skills add heygen-com/hyperframes
```

安装后 Claude Code/Cursor/Codex/Gemini CLI 等 Agent 即获得完整视频制作能力。CLI 默认非交互、全 flag 驱动、fail-fast，专为 Agent 自动驾驶设计。

## 竞品与定位

与 Remotion（React-to-video）同属"代码定义视频"赛道，区别在于 HyperFrames 更强调：
1. **Agent 友好性** — CLI 非交互，Skill 直接安装
2. **确定性** — 渲染结果可复现，适合 CI/自动化批量生产
3. **零依赖 DSL** — 标准 HTML，无框架绑定

HeyGen 通过开源底层框架，意图建立 HTML-to-Video 技术标准，吸引开发者生态围绕其平台聚集。

## 环境要求

Node.js >= 22，FFmpeg

## 来源

- GitHub: https://github.com/heygen-com/hyperframes
- 文档: https://hyperframes.heygen.com/introduction
- 组件目录: https://hyperframes.heygen.com/catalog
- 开源日期: 2026-04-17，License: Apache 2.0
