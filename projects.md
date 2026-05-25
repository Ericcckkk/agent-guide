# 项目协作协议

## 核心规则

我有多个在服务器上运行的项目。每个项目根目录下有两个文件：

- `README.md` — 架构文档（技术栈、目录结构、常用命令）
- `DEVLOG.md` — 活的进度日志（已完成、待做、变更历史）

**任何 agent 接手某个项目时，必须：**

1. SSH 到对应服务器，`cat` 项目的 `DEVLOG.md`，了解当前状态
2. 基于 DEVLOG 中的信息 + 用户指令开始工作
3. 工作完成后，更新 `DEVLOG.md`（标记完成项、新增待办、记录今天的变更）

**不要问用户"项目做到哪了"——自己去服务器上看。**

---

## 项目清单

| 项目名 | 服务器 | 路径 | 说明 |
|--------|--------|------|------|
| mocky· | root@101.42.94.249 | /opt/ai-interviewer/ | AI 模拟面试工具，语音实时交互 |
| 买房决策系统 | root@101.42.94.249 | /opt/house-system/ | 买房数据分析平台 |

### mocky·

- **技术栈：** FastAPI + WebSocket + MiniMax Realtime API（语音）
- **前端：** 原生 HTML/CSS/JS，在 /opt/ai-interviewer/ui/
- **部署：** systemd（ai-interviewer.service），项目里有 docker-compose.yml 但未启用
- **CI/CD：** GitHub Actions + SCP（见 .github/workflows/deploy.yml）
- **域名：** interview.eric-ai.top:8443（Nginx SSL，Let's Encrypt）
- **端口：** 8000（uvicorn）→ 8443（nginx HTTPS 反代）
- **关键注意：** .env 里有三个 MiniMax API key 轮换；重启用 `systemctl restart ai-interviewer`

### 买房决策系统

- **技术栈：** FastAPI + SQLite
- **部署：** systemd，Nginx 反代 8080 → 8001
- **端口：** 8001（FastAPI）、8080（Nginx 前端+API 代理）
- **数据库：** /opt/house-system/data/house.db（部署时不要覆盖）
- **同步：** rsync

---

## DEVLOG.md 格式规范

```markdown
# 项目名 开发日志

> **给任何接手的 Agent：** 每次开始工作前先读此文件；每次完成工作后更新此文件。

---

## YYYY-MM-DD 最新状态

### 已完成
- [x] 功能描述

### 待做 / 已知问题
- [ ] 待办描述

### 技术要点备忘
- 关键配置、端口、API 路径等

---

## 变更历史

### YYYY-MM-DD
- 具体改了什么
```

---

## 跨设备工作场景

用户会在不同设备（公司 CatDesk、家里龙虾、其他电脑的 Claude 等）上工作。这些客户端之间没有共享记忆。

唯一的共享状态就是服务器上的文件。所以：

- **不要依赖本地文件或聊天历史来了解项目状态**
- **不要问用户"上次做到哪了"**
- **直接 SSH 去看 DEVLOG.md**

用户的起手式通常就是一句话，比如：
- "继续搞 mocky"
- "mocky 加个历史记录功能"
- "看看 101.42.94.249 上的 ai-interviewer，把移动端适配做了"

你要做的：解析出项目 → 查项目清单找到服务器和路径 → SSH cat DEVLOG.md → 开工。

---

## 新设备首次接入

如果你在一台新设备上，SSH 连接服务器失败（没有密钥），按以下流程操作：

1. **生成密钥：** `ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -N ""`
2. **输出公钥：** `cat ~/.ssh/id_ed25519.pub`
3. **把公钥内容发给用户**，说明需要将其添加到服务器的 `~/.ssh/authorized_keys`
4. 用户会通过已有权限的设备帮你加上去（或者直接告诉你密码）
5. 加完后重试 SSH 连接

**注意：** 这是一次性操作，每台设备只需做一次。不要反复尝试连接或自行寻找其他方式绕过。

---

## 项目技术速查

### 买房决策系统

| 项 | 值 |
|----|-----|
| GitHub | https://github.com/Ericcckkk/house-decision |
| 域名 | house.eric-ai.top（未备案，目前用 IP:8080 访问） |
| 前端 | Tailwind + Alpine.js + ECharts，PWA，静态文件在 /opt/house-system/dashboard/ |
| 后端 | FastAPI + SQLite，端口 8001（仅 127.0.0.1） |
| 反代 | Nginx 监听 8080，/ → 静态文件，/api/ → 8001 |
| 服务 | systemd: house-api |
| 数据库 | /opt/house-system/data/house.db（部署时不要覆盖） |
| 定时任务 | cron 每日 6:00 跑 /opt/house-system/run_scraper.sh |
| 部署方式 | GitHub Actions → rsync 到 /opt/house-system/repo/ → restart service |
| Python | 3.12，venv 在 /opt/house-system/venv/ |

**注意事项：**
- [ ] **不要占用 8001 端口。** 这是买房系统 API 的端口。
- [ ] **不要覆盖 house.db。** 部署时 rsync 应排除 data/ 目录。
- [ ] **Nginx 配置在 /etc/nginx/sites-enabled/house。** 改完要 nginx -t && systemctl reload nginx。
