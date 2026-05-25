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
- [ ] **rsync 要排除数据文件。** SQLite 数据库、用户上传文件等不能被部署覆盖，用 `--exclude` 排除 data/ 目录。
- [ ] **PWA 图标不要用 cairosvg 生成。** 服务器和 Windows 上装 cairo 都很麻烦，用纯 Python（struct + zlib）直接写 PNG 字节流更可靠。
- [ ] **systemd 服务的 WorkingDirectory 要和代码实际位置一致。** 如果 rsync 同步到 repo/ 子目录，WorkingDirectory 也要指向那里，否则 import 找不到模块。
- [ ] **Nginx 改完配置必须 nginx -t 再 reload。** 不要直接 restart，语法错误会导致整个 Nginx 挂掉影响所有站点。
- [ ] **cron 定时任务里要写绝对路径并激活 venv。** 不要依赖 PATH 环境变量，cron 的环境和 shell 不一样。
- [ ] **GitHub Actions 可做轻量定时任务。** 不需要服务器 cron 的场景（如每天抓新闻+调 API），直接用 Actions cron 触发 + workflow_dispatch 手动补跑。省服务器资源。
- [ ] **MiniMax M2.7 模型会输出 `<think>` 标签。** 调用 MiniMax API 解析 JSON 结果前，必须先 `re.sub(r'<think>[\s\S]*?</think>', '', response).strip()` 剥掉思考过程，否则 json.loads 会炸。
- [ ] **API 调用超时要给够。** 大量文本让 LLM 处理时（50+ 条新闻），timeout 至少设 300 秒，120 秒大概率超时。
- [ ] **MiniMax Token Plan 的 key 可跨项目复用。** 一个 plan（199元/月）的 key 可以给多个 GitHub repo 的 Secrets 用，不需要每个项目单独买。
