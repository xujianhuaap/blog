---
title: linux-reinstall
date: 2017-12-06 15:36:38
tags: usb-booter
---
```
1. 准备的材料u盘 iso文件
2. fdisk 查看路径egg: /dev/sdb1
3. umount /dev/sdb1
4. mkfs.vfat /dev/sdb1
5. sudo dd if=/path/*.iso of=/dev/sdb
ps: of=/dev/sdb 与/dev/sdb1无关
    u盘如果以前做过启动盘（利用大白菜，毛猴桃
	等工具），必须利用ud分区工具删除分区
```
