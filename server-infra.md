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

- [ ] 不要动 `/opt/ai-interviewer/` 下的任何东西
- [ ] 不要占用 8000、8443 端口
- [ ] 数据库文件在 `/opt/house-system/data/house.db`，部署时不要覆盖
