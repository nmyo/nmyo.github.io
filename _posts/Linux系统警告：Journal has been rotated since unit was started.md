Linux系统 `systemctl status xxx` 显示：
Warning: Journal has been rotated since unit was started. Log output is incomplete or unavailable.

原因：
journald 的磁盘使用率达到最大值，默认10%

首先可以使用 journalctl 命令查看日志占用：

```bash
journalctl --disk-usage
```

### 手动清理 journal 日志

如果要去清理 journal 日志，可以先执行 rotate 命令：

```bash
journalctl --rotate && \
systemctl restart systemd-journald
```

删除两天前的日志：
`journalctl --vacuum-time=2days`

删除两个礼拜前的日志：
`journalctl --vacuum-time=2weeks`

或者删除超出文件大小的日志：
`journalctl --vacuum-size=20M`

### 修改限制 journal 日志最大占用

修改配置文件：
`vi /etc/systemd/journald.conf`

修改其中的两项：

```ini
SystemMaxUse=100M
RuntimeMaxUse=100M
```

SystemMaxUse 设置 `/var/log/journal`， RuntimeMaxUse 设置 `/run/log/journal`

然后使设置生效：
`systemctl daemon-reload`