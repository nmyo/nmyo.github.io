---
title: Linux手动安装BBRplus
date: 2024-2-16
categories: VPS
tags: Linux
description: Linux手动安装BBRplus,安装其余内核可以用类似方法
---

### 设置目录

```bash
mkdir bbr&&cd bbr
```

```
#Centos 7
wget https://slink.ltd/https://github.com/UJX6N/bbrplus-6.x_stable/releases/download/6.7.4-bbrplus/CentOS-7_Optional_kernel-headers-6.7.4-bbrplus.el7.x86_64.rpm
https://github.com/UJX6N/bbrplus-6.x_stable/releases/download/6.7.4-bbrplus/CentOS-7_Required_kernel-6.7.4-bbrplus.el7.x86_64.rpm
```

### AMD64 / IA32（x86_64）

```
#Centos 8
wget https://github.com/UJX6N/bbrplus-5.17/releases/download/5.17.15-bbrplus/Debian-Ubuntu_Required_linux-headers-5.17.15-bbrplus_5.17.15-bbrplus-1_amd64.deb
wget https://github.com/UJX6N/bbrplus-5.17/releases/download/5.17.15-bbrplus/Debian-Ubuntu_Required_linux-image-5.17.15-bbrplus_5.17.15-bbrplus-1_amd64.deb
#Debian/Ubuntu
wget https://github.com/UJX6N/bbrplus-6.x_stable/releases/download/6.7.4-bbrplus/CentOS-Stream-8_Optional_kernel-headers-6.7.4-bbrplus.el8.x86_64.rpm
wget https://github.com/UJX6N/bbrplus-6.x_stable/releases/download/6.7.4-bbrplus/CentOS-Stream-8_Required_kernel-6.7.4-bbrplus.el8.x86_64.rpm
```

### ARM64 / AARCH64

```
#Centos 8
wget https://github.com/UJX6N/bbrplus-6.x_stable/releases/download/6.7.4-bbrplus/CentOS-Stream-8_Optional_kernel-headers-6.7.4-bbrplus.el8.aarch64.rpm
wget https://github.com/UJX6N/bbrplus-6.x_stable/releases/download/6.7.4-bbrplus/CentOS-Stream-8_Required_kernel-6.7.4-bbrplus.el8.aarch64.rpm
#Debian/Ubuntu
wget https://github.com/UJX6N/bbrplus-6.x_stable/releases/download/6.7.4-bbrplus/Debian-Ubuntu_Optional_linux-headers-6.7.4-bbrplus_6.7.4-bbrplus-1_amd64.deb
wget https://github.com/UJX6N/bbrplus-6.x_stable/releases/download/6.7.4-bbrplus/Debian-Ubuntu_Required_linux-image-6.7.4-bbrplus_6.7.4-bbrplus-1_amd64.deb
```

### 安装内核

```bash
sudo dpkg -i *.deb
```

### 检查内核（是否已被安装）

```bash
dpkg -l|grep linux-image
```

