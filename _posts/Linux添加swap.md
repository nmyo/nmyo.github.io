**小内存vps，增加或删除swap分区方法：**

**检查分区：`free -h`**

```vbnet
              total        used        free      shared  buff/cache   available
Mem:           470M         69M        271M        3.6M        129M        386M
Swap:          1.0G          0B        1.0G
```

**如果出现上面字样说明你的vps是存在swap分区的，这个时候想调整分区大小，俺们就要先删除原有分区，再创建。**

**找到swapfile文件： `find / -name swapfile`**

**cd到存放swapfile的文件夹**

**停用swap空间： `swapoff swapfile`**

**删除swapfile文件： `rm swapfile`**

**上面完事后就等于删除swap分区了，接下来创建swap分区。**

```bash
cd / && mkdir swap && cd swap
```

**创建swap文件，后面的2048是分区大小2g，自己可以根据需要调整。**

```bash
dd if=/dev/zero of=swapfile bs=1M count=102
```

**将文件标记为交换空间：**

```bash
mkswap swapfile
```

**启用该交换文件:**

```bash
swapon swapfile
```

**另建议给swapfile文件权限为600，也就是root权限，以免出现安全隐患:**

```bash
chmod 600 swapfile
```

**再次检查一下swap分区是否可用：**

```bash
swapon --show
```

**最后，设置swap分区为开机自动挂载：**

```bash
echo "/swap/swapfile none swap sw 0 0" >> /etc/fstab
```

**建议swap分区大小和RAM大小保持差不多。**