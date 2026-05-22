# 部署 / CI-CD 规则

## GitHub Actions

- [ ] **push = 自动部署。** 推到 main 分支就自动部署到服务器，不需要手动操作。
- [ ] **用 rsync 部署。** 不要用 scp-action（文件可能传不全），用 rsync 更可靠。
- [ ] **SSH 密钥认证。** 服务器用 SSH key 登录，密钥存在 GitHub Secrets 里。
- [ ] **不要在 Actions 里 clone 仓库到服务器。** 直接 rsync 文件过去，避免私有仓库认证问题。

## 服务器部署

- [ ] **先查端口占用再分配。** `ss -tlnp` 或 `netstat` 确认端口没被占。
- [ ] **用 systemd 管理服务。** 不要用 nohup 或 screen，要能开机自启、崩溃重启。
- [ ] **Nginx 做反向代理。** 前端静态文件 + API 代理统一走一个端口。
- [ ] **虚拟环境隔离 Python。** 每个项目独立 venv，不污染系统 Python。

## 踩坑记录

- [ ] **sshd_config 里 PubkeyAuthentication 可能被注释掉。** 新服务器记得检查并启用。
- [ ] **端口 8000 可能被其他项目占。** 不要假设端口空闲，先查。
- [ ] **腾讯云域名需要 ICP 备案才能用。** 如果域名没备案，直接用 IP:端口 访问。
- [ ] **rsync 目标目录要提前创建好。** 否则首次部署可能失败。
