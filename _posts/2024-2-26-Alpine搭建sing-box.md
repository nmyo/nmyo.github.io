---
title: Alpine搭建sing-box
date: 2024-2-16
categories: VPS
tags: Linux，Alpine
description: Alpine搭建sing-box，收集自网上教程
---

### 首先先安装singbox,还有vim

```bash
echo "@testing https://dl-cdn.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories
apk add sing-box@testing vim
```

### 设置自启 
编辑`vim /etc/init.d/singboxtunnel`

```json
cat > /etc/init.d/singbox <<EOF
#!/sbin/openrc-run

name=$RC_SVCNAME
description="sing-box service"
supervisor="supervise-daemon"
command="/usr/bin/sing-box"
command_args="-D /var/lib/sing-box -C /etc/sing-box run"

depend() {
        after net dns 
}

reload() {
        ebegin "Reloading $RC_SVCNAME"
        /bin/kill -HUP $MAINPID
        eend $?
}
EOF
```

### 然后加入OpenRC自启动

```bash
chmod +x /etc/init.d/singbox
rc-update add singbox default
rc-service singbox start
```
