---
title: Alpine搭建sing-box
date: 2024-2-16
categories: VPS
tags: Linux，Alpine
description: Alpine搭建sing-box，收集自网上教程
---

### 下载运行文件（自行去下面代码网址替换适合自己机器架构的版本）：

```bash
wget -O realm.tar.gz https://github.com/zhboner/realm/releases/download/v2.5.3/realm-x86_64-unknown-linux-gnu.tar.gz && tar -xvf realm.tar.gz && chmod +x realm
```

### 去`/root`文件夹下新建`config.toml`文件，并写入（具体远程IP和端口，本机端口自己改）：

```toml
cat > config.toml <<EOF
[network]
# 开启UDP
use_udp = true
tcp_timeout = 5
udp_timeout = 30

[[endpoints]]
listen = "0.0.0.0:8022"
remote = "[240d:1a:116e:c800::46]:10888"

[[endpoints]]
listen = "0.0.0.0:8033"
remote = "[240d:1a:116e:c800::46]:10999"

[[endpoints]]
listen = "0.0.0.0:8044"
remote = "jp-osaka-oracle-d9311f.ip1.shop:33053"
EOF
```

### 去`/etc/systemd/system`文件夹下新建`realm.service`文件，并写入：

```json
cat > /etc/systemd/system/realm.service <<EOF
[Unit]
Description=realm
After=network-online.target
Wants=network-online.target systemd-networkd-wait-online.service

[Service]
Type=simple
User=root
Restart=on-failure
RestartSec=5s
DynamicUser=true
WorkingDirectory=/root
ExecStart=/root/realm -c /root/config.toml

[Install]
WantedBy=multi-user.target
EOF
```

### 启动服务：

```bash
systemctl daemon-reload
systemctl enable realm
systemctl start realm
systemctl restart realm
systemctl status realm
```

