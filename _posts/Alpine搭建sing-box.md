### 首先先安装singbox,还有vim

```bash
echo "@testing https://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories
apk add sing-box@testing vim
```

### 设置自启 
编辑`vim /etc/init.d/singboxtunnel`

```bash
#!/sbin/openrc-run
 
command="/usr/bin/sing-box"
command_args="run -c /root/config.json" #是您的配置文件位置
description="SingboxTunnel service"
 
depend() {
  need net
  use logger
}
 
start() {
  ebegin "Starting SingboxTunnel"
  start-stop-daemon --start --background --exec $command -- $command_args
  eend $?
}
 
stop() {
  ebegin "Stopping SingboxTunnel"
  start-stop-daemon --stop --exec $command
  eend $?
}
```

### 然后加入OpenRC自启动

```bash
chmod +x /etc/init.d/singboxtunnel
rc-update add singboxtunnel default
rc-service singboxtunnel start
```

