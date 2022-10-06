---
title: Ubuntu实现开机自动挂载硬盘
date: 2022-10-06 15:19:42
tags: [Ubuntu,Linux]
categories: 
- 菜鸡修炼手册
permalink: /articles/2022/10/06/1665040782325.html
top_img: https://tmx.fishpi.cn/img/red-maple-leaves-g3378b9232_1920.jpg
cover: https://tmx.fishpi.cn/img/red-maple-leaves-g3378b9232_1920.jpg
---
![1628345033618.jpg](https://tmx.fishpi.cn/img/red-maple-leaves-g3378b9232_1920.jpg)


## 正常手动挂载

1. 首先查看有哪些硬盘
```
fdisk -l
```

2. 挂载
```
mount [-参数] [设备名称] [挂载点] 
```

例如
```
mount /dev/vdb1 /devdata
```
其中，设备名称是`/dev/vdb1`， 挂载节点是`/devdata`

## 开机自动挂载
按上面那种方式挂载，重启后需要重新挂载，所以需要将硬盘uuid写入`/etc/fstab`

1. 查看硬盘uuid
```
blkid
```

2. 编辑`/etc/fstab`
```
vim /etc/fstab
```
加入新硬盘的记录
例如
```
UUID=cacec1a9-eb31-4924-b6db-df0070da7bf4 /devdata ext4 defaults 0 0
```

具体格式为
```
<fs spec> <fs file> <fs vfstype> <fs mntops> <fs freq> <fs passno>
具体说明:
<fs spec> :分区定位，可以给UUID或LABEL，例如：UUID=cacec1a9-eb31-4924-b6db-df0070da7bf4或LABEL=software
<fs file> : 具体挂载点的位置，例如：/devdata
<fs vfstype> : 挂载磁盘类型，linux分区一般为ext4，windows分区一般为ntfs
<fs mntops> : 多个选项间使用逗号分隔, async, sync, _netdev, defaults(rw,  suid, dev, exec, auto, nouser, async, and relatime)
<fs freq> : 转存频率， 0：从不备份, 1：每日备份, 2：每隔一天备份; 该方法已过时，默认选0即可；
<fs passno> : 自检次序：0: 不自检，1：首先自检，通常只能被/使用；2：等数字为1的自检完成后，再进行自检；一般磁盘可以选2，如果不需要自检就选择0；
```

3. 验证配置是否正确
```
mount -a
```
如果配置不正确，可能导致启动失败

4. 重启系统，正常挂载
