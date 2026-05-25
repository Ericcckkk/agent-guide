# 服务器环境信息

## 腾讯云轻量服务器

- **IP:** 101.42.94.249
- **OS:** Ubuntu (具体版本需确认)
- **登录:** root 用户，SSH 密钥认证
- **Python:** 3.12

## 端口分配

| 端口 | 用途 | 项目 |
|------|------|------|
| 8000 | FastAPI | ai-interviewer |
| 8001 | FastAPI | 买房决策系统 API |
| 8080 | Nginx | 买房决策系统（前端+API代理） |
| 8443 | HTTPS | ai-interviewer |

## 目录结构

| 路径 | 内容 |
|------|------|
| `/opt/house-system/` | 买房决策系统 |
| `/opt/house-system/repo/` | rsync 同步的代码 |
| `/opt/house-system/server/` | FastAPI 后端 |
| `/opt/house-system/data/` | SQLite 数据库 |
| `/opt/house-system/venv/` | Python 虚拟环境 |
| `/opt/ai-interviewer/` | AI 面试项目 |

## 域名

- **eric-ai.top** — 注册在阿里云/万网，DNS 由 HiChina 解析
- 腾讯云要求 ICP 备案才能通过域名访问，目前未备案，用 IP 直连

## 注意事项

- [ ] **`/opt/ai-interviewer/` 是活跃开发项目（mocky·），可以修改但必须先读 DEVLOG.md。** [更新 2025-07-14] 此项目已进入持续迭代，不再是"不要动"的状态。
- [ ] 不要占用 8000、8443 端口（分别被 ai-interviewer 和 nginx HTTPS 使用）
- [ ] 数据库文件在 `/opt/house-system/data/house.db`，部署时不要覆盖
- [ ] **所有项目的工作状态记录在各项目根目录的 `DEVLOG.md` 中，详见 `projects.md`。**
