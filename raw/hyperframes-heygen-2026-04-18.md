# HyperFrames — HeyGen 开源的 HTML 视频渲染框架

来源：GitHub README + 官方文档 + 分析文章
收集日期：2026-04-18
GitHub: https://github.com/heygen-com/hyperframes
官方文档：https://hyperframes.heygen.com/introduction
License: Apache 2.0

---

## 核心定位

**口号：Write HTML. Render video. Built for agents.**

HyperFrames 是 HeyGen 于 2026年4月17日开源的视频渲染框架。核心思想：HTML 是视频的 source of truth。把一个 HTML 文件加上 `data-*` 时间轴属性，就能渲染成 MP4/MOV/WebM。

传统视频工具（After Effects、DaVinci Resolve）用专有二进制格式和复杂 JSON 场景图，AI Agent 几乎无法操作。HyperFrames 的洞见是：LLM 在 HTML/CSS/JS 上受过海量训练，天然擅长写动效代码。**把 HTML 变成视频格式，AI Agent 就成了视频编辑器。**

---

## 工作原理

1. **写 HTML** — 用 `data-start`、`data-duration`、`data-track-index` 属性定义时间轴
2. **浏览器预览** — `npx hyperframes preview` 实时热更新
3. **渲染 MP4** — `npx hyperframes render` → 无头 Chrome 逐帧捕获 → FFmpeg 编码

渲染方式：Chrome 的 `beginFrame` API 逐帧截图，`frame = floor(time * fps)`，确定性渲染，同一输入永远产出相同输出。

### 最简 HTML 示例

```html
<div id="root" data-composition-id="demo" data-start="0" data-width="1920" data-height="1080">
  <video id="clip-1" data-start="0" data-duration="5" data-track-index="0" src="intro.mp4" muted playsinline></video>
  <h1 id="title" class="clip" data-start="1" data-duration="4" data-track-index="1" style="font-size:72px;color:white;">
    Welcome to Hyperframes
  </h1>
  <audio id="bg-music" data-start="0" data-duration="5" data-track-index="2" data-volume="0.5" src="music.wav"></audio>
</div>
```

---

## 核心属性

| 属性 | 作用 |
|------|------|
| `data-composition-id` | 视频合成的唯一 ID |
| `data-width` / `data-height` | 输出分辨率（像素） |
| `data-start` | 场景开始时间（秒） |
| `data-duration` | 场景持续时间（秒） |
| `data-track-index` | 轨道层叠顺序（同轨不能重叠） |

---

## 包结构

| 包名 | 作用 |
|------|------|
| `hyperframes`（CLI） | 创建、预览、lint、渲染合成 |
| `@hyperframes/core` | 类型、HTML 解析、运行时、linter |
| `@hyperframes/engine` | 可定位的页面转视频捕获引擎（Puppeteer + FFmpeg） |
| `@hyperframes/producer` | 完整渲染管道（捕获 + 编码 + 混音） |
| `@hyperframes/studio` | 浏览器端可视化合成编辑 UI |
| `@hyperframes/player` | 可嵌入的 `<hyperframes-player>` Web Component |
| `@hyperframes/shader-transitions` | WebGL 着色器场景转场 |

---

## 功能特性

- **AI Agent 原生** — CLI 默认非交互式，全 flag 驱动，纯文本输出，fail-fast，Agent 无需解析交互界面
- **Frame Adapter 模式** — 支持任意动画运行时：GSAP、Lottie、CSS、Three.js
- **50+ 预制组件** — 社交 overlay、shader 转场、数据可视化、电影特效（`npx hyperframes add <block>`）
- **TTS 配音** — 内置 Kokoro-82M 文字转语音
- **字幕同步** — 字幕/歌词逐字与音频对齐（karaoke 风格）
- **音频响应式动效** — 动画随音乐节拍脉冲/发光
- **WCAG 对比度检测** — `npx hyperframes validate` 自动截图检测文字对比度

---

## 与 AI Agent 的集成

HyperFrames 以 Skills 包形式发布，一条命令安装：

```bash
npx skills add heygen-com/hyperframes
```

安装后 Claude Code/Cursor/Codex/Gemini CLI 等 Agent 即获得视频制作能力。在 Claude Code 中注册为 slash 命令：
- `/hyperframes` — 合成编写
- `/hyperframes-cli` — CLI 操作
- `/gsap` — 动画帮助

### Agent 使用示例
```
Using /hyperframes, create a 10-second product intro with a fade-in title, a background video, and background music.

Summarize the attached PDF into a 45-second pitch video using /hyperframes.

Turn this CSV into an animated bar chart race using /hyperframes.

Make a 9:16 TikTok-style hook video about [topic], with bouncy captions synced to a TTS narration.
```

---

## 环境要求

- Node.js >= 22
- FFmpeg

---

## 战略意义

HyperFrames 开源的背景：HeyGen 是头部 AI 视频生成平台，通过开源底层渲染框架，可以：
1. 建立 HTML-to-Video 的技术标准
2. 吸引开发者生态，形成围绕 HeyGen 平台的工具链
3. 让 AI Agent 能批量自动生产结构化视频内容

这是「把 AI 的原生语言（HTML）变成视频格式」的思路，与 Remotion（React-to-video）路线类似，但更强调 Agent 友好性和确定性渲染。

---

## 相关链接

- GitHub: https://github.com/heygen-com/hyperframes
- 官方文档: https://hyperframes.heygen.com/introduction
- 组件目录: https://hyperframes.heygen.com/catalog
- Launch video 源码: https://github.com/heygen-com/hyperframes-launch-video
- Skills: https://skills.sh/heygen-com/hyperframes
