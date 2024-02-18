---
title: Tcp-Brutal搭建
date: 2024-2-16
categories: VPS
tags: Linux
description: Tcp-Brutal不推荐使用，数值设置不好只会适得其反
---

### 官方安装脚本
```bash
bash <(curl -fsSL https://tcp.hy2.sh/)
#安装头文件
Ubuntu: apt install linux-headers-$(uname -r)
```

### 手动安装
```bash
#安装依赖
apt -y update
apt -y install build-essential linux-headers-$(uname -r) dkms dh-make git
```
```bash
#获取项目代码、创建dkms压缩包
git clone https://github.com/apernet/tcp-brutal.git
cd tcp-brutal
make dkms-tarball
```
```bash
#看一下dkms.conf文件的内容
cat dkms.conf
#有以下内容就正常
PACKAGE_NAME="tcp-brutal"
PACKAGE_VERSION="1.0.1"
```
```bash
#根据查看到的PACKAGE_NAME和PACKAGE_VERSION创建相应的目录
mkdir -p /usr/src/tcp-brutal-1.0.1
```
```bash
#把压缩包文件解压到相应的目录
tar -xzf dkms.tar.gz --strip-components=2 -C /usr/src/tcp-brutal-1.0.1
```
```bash
#安装brutal内核模块
dkms install -m tcp-brutal -v 1.0.1
#查看是否安装成功
lsmod | grep brutal
```
