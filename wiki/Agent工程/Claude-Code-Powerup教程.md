---
type: concept
maturity: draft
date: 2026-04-26
updated: 2026-04-26
tags: [claude-code, tutorial, powerup, onboarding, slash-commands, mcp, skills, hooks, subagent]
aliases: [/powerup, powerup, Claude Code Powerup, Claude Code 教程]
sources: ["raw/面试官："你 Claude Code 用得这么 6？" 我暗喜："我早把 powerup 命令的流程都做了"，他：你牛逼.md"]
related: ["[[Claude-Code源码架构]]", "[[CLAUDE-md配置方法论]]", "[[Claude-Sub-Agents]]", "[[OpenClaw-Skill生态]]"]
---

# Claude Code /powerup 教程

Claude Code v2.1.90 新增 `/powerup` 命令，在终端内置了一套交互式教程，共 10 个课程，带动画演示，覆盖从基础操作到进阶技巧的完整学习路径。

## 10 个课程概览

### 01 Talk to your codebase：@ 引用文件

用 `@` 符号精准指定 Claude 要看的文件，避免自动搜索吃满上下文。

- `@./src/App.tsx` — 引用单个文件
- `@./src/components/` — 引用整个目录（获取文件列表）
- 多文件用空格隔开

配合 `.claude/settings.json` 的 `permissions.deny` 可屏蔽敏感文件（.env、secrets 等），防止浪费上下文或泄露敏感信息。

### 02 Steer with modes：Shift+Tab 切换模式

四种工作模式对应四种信任级别：

| 模式 | 行为 | 适用场景 |
|------|------|---------|
| Normal（默认） | 每步操作都征求同意 | 不确定 Claude 会做什么时 |
| Accept Edits | 文件编辑自动放行，命令仍需确认 | 日常写代码的"黄金档位" |
| Plan | 只看不改，出方案后等人确认 | 复杂重构、大功能规划 |
| Auto | 所有操作全自动，靠 Claude 安全分类器判断 | 完全信任的长任务、批量操作 |

### 03 Undo anything：撤销机制

- 连按两次 Esc 或输入 `/rewind` 打开回滚菜单
- 回滚不仅恢复文件内容，对话上下文也一起回滚
- 回滚只覆盖 Claude 直接创建/编辑的文件，`npm install` 等命令产生的文件需靠 Git 管理

### 04 Run in the background：后台任务

用自然语言告诉 Claude "在后台运行"即可，Claude 自动使用 Bash 工具的后台模式。用 `/tasks` 查看后台任务状态。适用于构建、测试、安装依赖等耗时长但不需盯着的任务。

### 05 Teach Claude your rules：CLAUDE.md 与记忆

[[CLAUDE-md配置方法论]] 的入门版。CLAUDE.md 是项目根目录的配置文件，Claude Code 每次启动自动读取。

三个层级（优先级递增）：
1. **用户级**：`~/.claude/CLAUDE.md`，所有项目生效
2. **项目级**：`CLAUDE.md` 或 `.claude/CLAUDE.md`，可提交 Git 团队共享
3. **个人项目级**：`CLAUDE.local.md`，加入 .gitignore，私人偏好

`/init` 一键生成初始 CLAUDE.md。`/memory` 管理跨项目个人记忆。用自然语言"请记住：xxx"也可写入 auto memory。

原则：**越精准越好，不是越长越好**。太长会占用上下文，反而降低效率。

### 06 Extend with tools：MCP 扩展

MCP（Model Context Protocol）是 Claude Code 的外挂接口，让 Claude 调用外部工具能力。

安装方式：
- 命令行：`claude mcp add <名字> -- <启动命令>`
- 配置文件：`.claude/settings.json` 或 `~/.claude/settings.json` 的 `mcpServers` 字段
- 团队共享：项目根目录 `.mcp.json` 提交 Git

管理：`/mcp` 查看管理界面，`claude mcp list` 列出已装 server，`claude mcp remove <名字>` 卸载。

### 07 Automate your workflow：Skills 与 Hooks

**Skills** 是 Claude Code 的插件/技能包机制。一份 Skill 就是 `SKILL.md` 文件加辅助材料，放到 `~/.claude/skills/` 或 `.claude/skills/` 即可。安装打包好的插件用 `/plugin install <名字>@<市场>`。

**Hooks** 是自动化钩子，在工具执行前后触发自定义脚本：
- `PreToolUse` — 工具执行前触发，适合输入校验、拦截不安全操作
- `PostToolUse` — 工具执行后触发，适合自动格式化、自动测试

Hook 脚本通过 stdin 接收 JSON（含 tool_input.file_path 等字段），不是通过环境变量。

### 08 Multiply yourself：子代理

[[Claude-Sub-Agents]] 的用户向解读。用 `/agents` 创建子代理，每个子代理有独立上下文窗口，与主对话隔离。

两个核心价值：
1. **独立视角** — 子代理不知道代码是谁写的，审查更客观
2. **保护主会话上下文** — 子代理消耗自己的上下文空间，只汇报精炼结果

### 09 Code from anywhere：远程控制与传送

- `/remote-control`（别名 `/rc`）— 让本地终端会话进入可远程控制状态，从 claude.ai 网页操作
- `/teleport`（别名 `/tp`）— 将 claude.ai 网页上的会话传送到本地终端继续（需 claude.ai 订阅）

两个命令形成双向通道：一个向外暴露，一个向内传送。

### 10 Dial the model：模型与思考深度

`/model` 切换模型：Sonnet（平衡）、Opus（最强推理）、Haiku（最快最便宜）。

`/effort` 设置思考档位，5 档：

| 档位 | 特点 | 持久性 |
|------|------|--------|
| low | 最少思考，最快响应 | 跨 session 记住 |
| medium | 成本敏感折中 | 跨 session 记住 |
| high | 默认行为，复杂推理可扛 | 跨 session 记住 |
| xhigh | 甜点档，长跑编码任务推荐默认 | 跨 session 记住 |
| max | 能力上限，容易过度思考 | 一次性，会话结束自动失效 |

`ultrathink` 关键词可临时将当前轮次顶到 high 思考。新版 Claude Code 中只有 ultrathink 真正生效，其他关键词（think、think hard、think harder）只被当作普通提示词。

## 补充高频命令

| 命令 | 效果 | 适用场景 |
|------|------|---------|
| `/context` | 查看上下文占用 | Claude 开始犯迷糊时 |
| `/compact` | 压缩对话保留关键信息 | 上下文超 70% 时 |
| `/clear` | 彻底清空对话 | 切换不相关任务 |
| `claude --resume` | 恢复历史对话 | 关掉终端后继续 |
| `claude -c` | 恢复最近一次对话 | 快速接续上次工作 |

## 来源

- 小林coding（2026-04-26）：微信文章 https://mp.weixin.qq.com/s/tO15UKQG0WtTBTNz8QLQjQ
- Claude Code v2.1.90+ 内置功能
