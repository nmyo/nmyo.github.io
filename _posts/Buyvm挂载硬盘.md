## BuyVM/frantech Block Storage（数据盘）挂载方法如下:

1. 购买 Block Storage (Las Vegas)

2. 进入 Storage Volumes 后台，将 Block Storage 附加到（Attached To） VPS

3. 查看数据盘标号/名称

   > 输入`ls /dev/disk/by-id/`
   >
   > > **会看到如下结果:**
   > > ata-QEMU_DVD-ROM_QM00004  scsi-0BUYVM_SLAB_VOLUME-5263
   > > scsi-0BUYVM_SLAB_VOLUME-5263 就是数据盘，5263 是数据盘 id，后台也能看到。

4. 格式化
   `mkfs.ext4 -F /dev/disk/by-id/scsi-0BUYVM_SLAB_VOLUME-5263`

5. 创建加载文件夹[^1]
   `mkdir -p /Bigpan`

6. 挂载
   `mount -o discard,defaults /dev/disk/by-id/scsi-0BUYVM_SLAB_VOLUME-5263 /Bigpan`

7. 开机/重启自动挂载
   `echo '/dev/disk/by-id/scsi-0BUYVM_SLAB_VOLUME-5263 /Bigpan ext4 defaults,nofail,discard 0 0' | sudo tee -a /etc/fstab`

8. 查看硬盘分配情况
   `df -h`

[^1]: 加载路径请根据实际需要创建。