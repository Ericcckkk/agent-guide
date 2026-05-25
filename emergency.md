# 应急与回滚

---

## 绝对不能做的事

- **不要删除或覆盖数据库文件**（house.db、任何 .db / .sqlite 文件）
- **不要 `rm -rf` 项目根目录**
- **不要直接 `systemctl stop nginx`**（会导致所有站点同时挂掉）
- **不要改 sshd 配置后不测试就 restart sshd**（可能锁死自己）
- **不要动 .env 文件里的 API key 格式**（MiniMax key 有特定格式要求）
- **不要在没有 `--exclude data/` 的情况下 rsync 整个项目目录**

---

## 改坏了怎么恢复

### 服务挂了

```bash
# 查看服务状态和报错
systemctl status 服务名
journalctl -u 服务名 --since "5 min ago"

# 重启
systemctl restart 服务名
```

### Nginx 配置改坏了

```bash
# 先测试语法
nginx -t

# 如果报错，恢复备份
cp /etc/nginx/sites-enabled/配置文件.bak /etc/nginx/sites-enabled/配置文件
nginx -t && systemctl reload nginx
```

**规则：每次改 Nginx 配置前，先 `cp 配置文件 配置文件.bak`。**

### 前端改坏了（页面白屏/报错）

项目代码在 GitHub 上有备份。最快的恢复方式：

```bash
# mocky
cd /opt/ai-interviewer/ui/
git checkout -- .

# 买房决策系统
cd /opt/house-system/repo/
git checkout -- .
```

如果没有 git 历史（直接 SCP 上传的），找用户要之前的版本或从 GitHub Actions 的部署记录回溯。

### 数据库损坏

SQLite 数据库如果损坏，没有自动备份的话数据会丢。所以：

- **部署前确认 rsync 排除了 data/ 目录**
- 如果真的坏了，尝试：`sqlite3 house.db ".recover" | sqlite3 recovered.db`

---

## 操作前的安全习惯

每次做有风险的操作前，养成这些习惯：

1. **改配置前先备份：** `cp 文件 文件.bak`
2. **改 Nginx 后先测试：** `nginx -t`（通过了再 reload）
3. **改 systemd 后：** `systemctl daemon-reload`（再 restart）
4. **大范围改文件前：** 先确认 git status 干净，或者手动备份关键文件
5. **不确定的命令：** 先 `--dry-run`（rsync 支持）或 echo 出来看看
