# 跨平台配置指南

换设备/换账号时，按下面的步骤配置，让 agent 自动遵守规则。

## 核心提示词（通用）

所有平台都用这段话作为系统指令：

```
先读一下 https://github.com/Ericcckkk/agent-guide 里的所有 .md 文件，了解我的偏好和规则，整个对话过程中严格遵守。
如果过程中踩了坑或者我表达了新的偏好，对话结束前问我要不要更新 agent-guide。
```

## 各平台配置方法

### CatDesk

创建自定义命令，新对话输入 `/guide` 即可注入规则：

```bash
catdesk commands create --name "guide" --content "# Agent 协作规则

请先访问 https://github.com/Ericcckkk/agent-guide 阅读所有 .md 文件，了解我的偏好和规则。整个对话过程中严格遵守以下核心原则：

- [ ] 不要自作主张，改动前先确认
- [ ] 代码风格遵守 coding-style.md
- [ ] 部署相关遵守 deployment.md（GitHub Actions + 我的服务器配置）
- [ ] 前端项目遵守 frontend.md（移动优先、Tailwind、无 UI 库）
- [ ] 踩坑后主动提议更新 agent-guide（只追加不覆盖）

如果过程中踩了坑或者我表达了新的偏好，对话结束前问我要不要更新 agent-guide。"
```

### Claude Projects

1. 打开 claude.ai → 左侧 Projects → 新建或选择项目
2. 点击项目设置（齿轮图标）→ Custom Instructions
3. 粘贴上面的「核心提示词」
4. 该项目下所有对话自动生效

### ChatGPT

1. 打开 chatgpt.com → 左下角头像 → Settings
2. Personalization → Custom Instructions
3. 在 "How would you like ChatGPT to respond?" 里粘贴「核心提示词」
4. 所有新对话自动生效（全局）

### Cursor

在项目根目录创建 `.cursorrules` 文件（本仓库已提供模板 `cursorrules-template`）：

```bash
cp cursorrules-template /你的项目路径/.cursorrules
```

Cursor 会自动读取该文件作为项目级系统提示词。

### 其他 Agent（Copilot、Gemini 等）

找到对应平台的「系统提示词 / Custom Instructions / System Prompt」入口，粘贴「核心提示词」即可。

## 配置检查清单

换新环境后确认以下事项：

- [ ] GitHub 仓库 https://github.com/Ericcckkk/agent-guide 可公开访问
- [ ] 对应平台已配置系统提示词
- [ ] 新对话中 agent 能正确读取并遵守规则（可以问它"你读到了哪些规则？"验证）
