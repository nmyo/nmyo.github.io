---
title: Debian安装指定内核
date: 2024-2-16
categories: VPS
tags: Linux
description: Debian安装指定内核
---

### 某些程序在个别版本的内核上无法正常运行，这时候就需要安装指定版本的内核

#### 通过命令 `grep -e 'menuentry ' -e 'subm' /boot/grub/grub.cfg | awk -F'--' '{print $1}'` 找到需要启动的内核菜单名称

![示例](https://cdn.jsdelivr.net/gh/nmyo/pictures@main/new202312071639080.png)

#### 假设上图中的 `Debian GNU/Linux, with Linux 5.10.0-13-amd64` 是我们期望使用的内核，则需要引用图示 1 和 2 修改 `/etc/default/grub`

#### 修改 `/etc/default/grub` 中的 `GRUB_DEFAULT=0` 为如下值

```bash
GRUB_DEFAULT="Advanced options for Debian GNU/Linux>Debian GNU/Linux, with Linux 6.6.10-bbrplus"
```

#### 修改完成后执行命令 `update-grub`，成功后`reboot`即可
