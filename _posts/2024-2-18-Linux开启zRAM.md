---
title: Linux开启zRAM
date: 2024-2-16
categories: VPS
tags: Linux
description: 此片文章是在逛论坛时无意看到，觉得很有意思，遂搬运过来
---

### 开启/加载 zRAM 模块

zRAM 是 Linux 内核的模块，需要使用 `modprobe` 命令加载 zRAM 模块。具体为 `modprobe zram num_devices=1` 

zRAM 模块的参数为num_devices，`zram num_devices=1` 表示创建一个设备文件，该文件将会保存在设备目录，文件名称是 /dev/zram0

这里，如果 num_devices 的数值不等于 1，内核就会创建多个 zram 文件，具体为： /dev/zram{0,1,2,3...}

为了检查 zRAM 是否开启，我们需要使用 lsmod 命令 ：使用 `lsmod  | grep zram` 确认是否成功加载，如果成功开启，将会打印类似这样的消息

```
zram                   40960  2
zsmalloc               32768  1 zram
```

完整命令：

```bash
sudo modprobe zram num_devices=1
lsmod  | grep zram
```

### 开机自动开启/加载 zRAM 模块

modprobe 加载的模块仅在当前运行时可用，重新启动计算机后，会消失。为了自动加载 zRAM 模块，需要创建内核模块载入文件和模块的配置文件

载入 zRAM 模块。需要在 /etc/modules-load.d/ 目录创建文件 zram.conf，运行命令 `echo "zram" | sudo tee -a /etc/modules-load.d/zram.conf`

然后创建模块的配置文件 zram.conf。这个文件需要在目录 /etc/modules-load.d/，运行命令 `echo "options zram num_devices=1" | sudo tee -a /etc/modprobe.d/zram.conf` 

完整命令：

```bash
echo "zram" | sudo tee -a /etc/modules-load.d/zram.conf

echo "options zram num_devices=1" | sudo tee -a /etc/modprobe.d/zram.conf
```

### 配置 zRAM

通常 zRAM 最常见的配置是大小 disksize 和压缩算法 comp_algorithm

控制 zRAM 的大小的文件是 `/sys/block/zram0/disksize`，压缩算法文件是 `/sys/block/zram0/comp_algorithm`

可以运行 cat 命令查看两个文件来确定 zRAM 文件的大小和压缩算法，例如命令 `cat /sys/block/zram0/disksize` 查看 zRAM 大小(**zRAM 大小以实际 RAM 的 1-1.5倍 为宜，当然可以更大**)

同样也可以通过两个文件设置 zRAM 文件的大小和压缩算法，例如命令`echo "8G" | sudo tee /sys/block/zram0/disksize`修改 zRAM 的大小(**对于压缩算法这里推荐zstd压缩算法，各种算法区别具体可以Google**)

**注意：** 请务必先选择压缩算法再选择 zRAM 文件大小，不然会出现设备繁忙的问题，也就是无法修改压缩算法了，需要卸载 zRAM 后重新配置。

完整命令：

```bash
cat /sys/block/zram0/disksize
cat /sys/block/zram0/comp_algorithm

echo "zstd" | sudo tee /sys/block/zram0/comp_algorithm
echo "8G" | sudo tee /sys/block/zram0/disksize
```

### 自动进行 zRAM 配置

由于 /sys 目录是基于内存的文件系统，因此同样的，重启系统后，我们设置的 zRAM 参数将不再存在。为了实现开机自动加载 zRAM 的参数，需要使用 udev 进行设置。

udev 是一个用户空间系统(我的理解是，它就是 Linux 设备管理器)。它使操作系统管理员能够为设备事件运行用户指定的程序或者脚本，也可以在添加设备时指定设备参数。

要设置 zRAM 的大小，可以在 udev 规则文件的 ATTR 指定 zRAM 大小 disksize，ATTR{disksize}="8G" 表示设置 zRAM 的大小是 8G。

要设置 zRAM 的其它属性，可以在 udev 规则文件指定多个 ATTR 属性设置设备参数。例如设置 zRAM 压缩算法 ，可以添加属性 ATTR{comp_algorithm}="zstd"。

完整命令：

```bash
echo 'KERNEL=="zram0", ATTR{disksize}="512M",TAG+="systemd"' | sudo tee  /etc/udev/rules.d/99-zram.rules

echo 'KERNEL=="zram0", ATTR{comp_algorithm}="zstd", ATTR{disksize}="512M", TAG+="systemd"' | sudo tee  /etc/udev/rules.d/99-zram.rules
```

### 挂载 zRAM

要在当前运行时挂载 zRAM ，使用方式类似于 Swap 。首先需要将 zRAM 文件进行格式化，运行命令 `sudo mkswap /dev/zram0`

当格式化完成后，为了让系统识别 zRAM 文件，因此还需要启用 zRAM 文件。可以运行命令 `sudo swapon /dev/zram0`启用 zRAM 文件

如果你系统已经存在 Swap ，那么总的交换空间的大小是zRAM的大小加 Swap 的大小

完整命令:

```bash
sudo mkswap /dev/zram0
sudo swapon /dev/zram0
```

### 自动挂载 zRAM

同样的，为了避免重启后 zRAM 没有自动挂载，我们需要使用`systemd`命令，在系统启动时自动激活 zRAM 文件并作为交换空间挂载

使用你喜欢的方式创建文件`/etc/systemd/system/zram.service`并且编辑它。由于我是 vim 党，所以这里我使用vim 创建并编辑文件 SystemD 单元文件

编辑完成后，保存文件并退出 vim 编辑器，然后运行命令`sudo systemctl enable zram`启用 zram 服务，最后重启计算机

完整命令：

```bash
sudo vim /etc/systemd/system/zram.service
sudo systemctl enable zram
```

zram.service内容：

```json
[Unit]
Description=Swap with zram
After=multi-user.target

[Service]
Type=oneshot
RemainAfterExit=true
ExecStartPre=/sbin/mkswap /dev/zram0
ExecStart=/sbin/swapon /dev/zram0
ExecStop=/sbin/swapoff /dev/zram0

[Install]
WantedBy=multi-user.target
```

### 停止并禁用 zram 的服务

说完了基本使用，接下俩就要讲讲停用与卸载

我们上面将 zram 作为服务运行，所以可以通过 `systemd` 进行控制。以下命令可以停止并禁用 zram 服务：

```bash
sudo systemctl stop zramswap.service
sudo systemctl disable zramswap.service
```

### 卸载 zram 模块

可以使用 `rmmod`命令完全卸载zram模块。首先，确保没有任何 zram 设备在使用中：

```bash
sudo swapoff /dev/zram0
```

然后，卸载 zram 内核模块：

```bash
sudo rmmod zram
```

### 移除/注释相关的 fstab 条目

如果 /etc/fstab 文件中有关于 zram 的条目，应该将其注释掉或删除。打开 /etc/fstab 文件：

```bash
sudo vim /etc/fstab
```

找到类似下面的行：

```
/dev/zram0 none swap defaults 0 0
```

将其注释掉（在行首添加 #）或删除
