# 踩坑速查索引

遇到问题先查这里，按症状查找对应解决方案。

---

## SSH / 连接类

| 症状 | 原因 | 解决 |
|------|------|------|
| SSH 连接被拒绝 / Permission denied | 本机没有密钥 | 见 `projects.md` → "新设备首次接入" |
| SSH 连接超时 | 腾讯云安全组没放行端口 22 | 去腾讯云控制台 → 防火墙 → 放行 22 端口 |
| SSH 密钥认证不生效 | sshd_config 里 PubkeyAuthentication 被注释 | `vim /etc/ssh/sshd_config`，取消注释并设为 yes，`systemctl restart sshd` |

## 端口 / 服务类

| 症状 | 原因 | 解决 |
|------|------|------|
| 服务启动报端口已占用 | 其他进程占了这个端口 | `ss -tlnp \| grep :端口号` 找到占用的 PID，确认后 kill 或换端口 |
| 网页打不开但服务在跑 | Nginx 没配反代 / 没 reload | 检查 `/etc/nginx/sites-enabled/` 下的配置，`nginx -t && systemctl reload nginx` |
| HTTPS 证书报错 | Let's Encrypt 证书过期 | `certbot renew` 然后 `systemctl reload nginx` |
| 服务崩了没自动重启 | systemd 没配 Restart=always | 编辑 service 文件加上 `Restart=always`，`systemctl daemon-reload` |

## 部署类

| 症状 | 原因 | 解决 |
|------|------|------|
| rsync 首次部署失败 | 目标目录不存在 | 先 `mkdir -p /opt/项目名/` |
| 部署后数据丢了 | rsync 覆盖了数据库文件 | rsync 命令里必须 `--exclude data/`；恢复看备份 |
| 部署后 import 报错 | WorkingDirectory 和代码实际位置不一致 | 检查 systemd service 文件里的 WorkingDirectory 路径 |
| GitHub Actions 部署失败 | SSH key 过期或 Secrets 没配 | 检查 GitHub repo → Settings → Secrets 里的密钥是否正确 |

## Python / 后端类

| 症状 | 原因 | 解决 |
|------|------|------|
| ModuleNotFoundError | 没激活 venv 或依赖没装 | `source /opt/项目名/venv/bin/activate && pip install -r requirements.txt` |
| cron 定时任务不执行 | cron 环境里没有 PATH 和 venv | 脚本里写绝对路径，开头加 `source /opt/项目名/venv/bin/activate` |

## 前端类

| 症状 | 原因 | 解决 |
|------|------|------|
| 样式/JS 没更新 | 浏览器缓存 | 文件名加版本号参数（如 `app.js?v=2`）或 Ctrl+Shift+R 强刷 |
| PWA 图标生成失败 | cairosvg 在 Windows/服务器上装不上 | 不用 cairosvg，用纯 Python（struct+zlib）直接写 PNG 字节流 |

## 域名 / HTTPS 类

| 症状 | 原因 | 解决 |
|------|------|------|
| 域名访问被拦截 / 白屏 | 没有 ICP 备案 | 腾讯云要求备案，暂时直接用 IP:端口 访问 |
| 证书申请失败 | 80 端口没放行或 DNS 没解析 | 确保 Nginx 在 80 端口响应，DNS A 记录指向服务器 IP |
