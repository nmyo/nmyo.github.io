---
title: Alpine搭建XrayR
date: 2024-2-16
categories: VPS
tags: Linux，Alpine
description: Alpine搭建XrayR简易教程
---

### 安装依赖项

```bash
apk update
apk add wget unzip openrc
```

### 安装XrayR

```bash
wget https://github.com/XrayR-project/XrayR/releases/download/v0.9.0/XrayR-linux-64.zip
unzip XrayR-linux-64.zip -d /etc/XrayR
chmod +x /etc/XrayR/XrayR
#创建软链接
ln -s /etc/XrayR/XrayR /usr/bin/XrayR 
```

### 创建XrayR服务文件

```bash
cat > /etc/init.d/XrayR <<EOF
#!/sbin/openrc-run

depend() {
    need net
}

start() {
    ebegin "Starting XrayR"
    start-stop-daemon --start --exec /usr/bin/XrayR -- -config /etc/XrayR/config.yml
    eend $?
}

stop() {
    ebegin "Stopping XrayR"
    start-stop-daemon --stop --exec /usr/bin/XrayR
    eend $?
}

restart() {
    ebegin "Restarting XrayR"
    start-stop-daemon --stop --exec /usr/bin/XrayR
    sleep 1
    start-stop-daemon --start --exec /usr/bin/XrayR -- -config /etc/XrayR/config.yml
    eend $?
}
EOF
```

### 加入OpenRC自启动

```bash
#添加执行权限
chmod +x /etc/init.d/XrayR
#添加到开机启动项中
rc-update add XrayR default
/etc/init.d/XrayR restart
```

