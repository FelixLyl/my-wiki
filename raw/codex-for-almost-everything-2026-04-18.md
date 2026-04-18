# Codex：全能型助手（重大版本更新）

来源：https://openai.com/zh-Hans-CN/index/codex-for-almost-everything/
作者：OpenAI 官方
收录时间：2026-04-18

---

## 核心摘要

OpenAI 发布 Codex 重大更新，目标是打造"全能型协作伙伴"。全球已有超过 300 万名开发者每周使用 Codex。

---

## 新功能清单

### 1. 后台计算机使用（Computer Use）
- Codex 能像人类一样通过视觉识别、点击、输入，自主操控电脑上的各类应用
- 多个 Agent 并行工作，互不干扰用户日常操作
- 适用场景：前端效果调试、应用测试、操作无 API 的应用

### 2. 内置浏览器 + Comment Mode
- 应用内嵌浏览器，支持直接在页面标注，为 Agent 提供精准操作指令
- 已在前端及游戏开发中展现核心价值
- 未来规划：从 localhost 扩展到全浏览器控制

### 3. 图像生成（gpt-image-1.5）
- 调用 gpt-image-1.5，生成并迭代图像
- 结合截图与代码，支持产品原型、前端设计、视觉效果图、游戏开发

### 4. 90+ 新插件
- 新增插件包括：Atlassian Rovo（JIRA）、CircleCI、CodeRabbit、GitLab Issues、Microsoft 套件、Neon by Databricks、Remotion、Render、Superpowers 等
- 通过技能、应用集成及 MCP 服务端扩展上下文获取能力

### 5. 开发者工作流深化
- 支持处理 GitHub 评审意见
- 多标签终端视图
- SSH 连接远程开发机（Devbox，Alpha）
- 侧边栏直接打开文件，富文本预览（PDF/Excel/PPT/文档）
- 摘要面板：实时追踪 Agent 计划、来源、产出

### 6. 自动化（Automation）增强
- 重复使用既有对话线程，保留历史上下文
- 自主规划未来工作，自动唤醒执行长期任务（跨天/跨周）
- 已有团队用于：合入挂起 PR、跟进待办、监控 Slack/Gmail/Notion 动态

### 7. 记忆功能（Memory，预览版）
- 记住个人偏好、历史修正、费时获取的信息
- 显著加快后续任务速度，减少"自定义指令"依赖

### 8. 主动建议后续工作
- 结合项目背景、插件、记忆，建议开启新一天的工作或继续旧项目
- 示例：识别 Google Docs 未处理评论，从 Slack/Notion/代码库提取背景，输出优先级行动清单

---

## 开发者使用趋势

从"单纯编写代码"演进为：
- 剖析系统架构
- 调取上下文信息
- 评审代码
- 排查故障
- 协同团队工作
- 推动长期项目持续演进

---

## 可用性

- 登录 ChatGPT 账号的 Codex 桌面用户即日起陆续收到更新
- Computer Use 首发 macOS
- 个性化功能（上下文感知 + 记忆）即将向 Enterprise、Edu 及欧盟/英国开放
