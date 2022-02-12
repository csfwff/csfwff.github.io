---
title: 使用bat同时在多台设备安装应用
date: 2022-01-05 15:30:08
tags: [bat,Android,adb]
categories: 
- 菜鸡修炼手册
permalink: /articles/2022/01/05/1641367808000.html
top_img: https://tmx.fishpi.cn/img/ranunkeln-g3e434f4c9_1920.jpg
cover: https://tmx.fishpi.cn/img/ranunkeln-g3e434f4c9_1920.jpg
---

![图 1](https://tmx.fishpi.cn/img/ranunkeln-g3e434f4c9_1920.jpg)  

现在公司在调试一款Android设备，平时都是adb远程连上去，然后在Android Stuido里直接运行就行了
但是，现在一排设备摆在那里，要安装的时候，我得一个一个连上，然后一个一个安装，就很麻烦
或者我打好包，再到每个设备上安装，就很不优雅
百度一番，结合自己情况稍微修改一下，完美解决

## 先上jio本
`adb_connect.bat`

```
adb connect 192.168.1.51
::adb connect 192.168.1.52
adb connect 192.168.1.53
adb connect 192.168.1.54
adb connect 192.168.1.55
::adb connect 192.168.1.56
```

`multi.bat`
```
@echo off

echo --------------------------------------------------------
echo Get devices
adb devices > devices.txt
type devices.txt

echo --------------------------------------------------------
echo start install %1 %2

for /f  "skip=1 tokens=1 delims=	" %%i in (devices.txt) do (
start adb_install %%i %1 %2
)

del devices.txt
echo --------------------------------------------------------
pause
```
`adb_install.bat`
```
echo start install apk to %1
adb -s %1 install -r %2
adb -s %1 shell am start %3
exit
```

使用方法
1. 编辑`adb_connect.bat`，输入要安装的设备ip
2. 复制apk到当前目录下
3. powershell运行`.\adb_connect.bat`连接设备
4. powershell运行`.\multi.bat .\test.apk com.xiamo.text/.MainActivity`安装并启动(第一个参数apk路径，第二个为启动的Activity)
5. 弹出一堆黑框，各自安装，安装完退出，全部退出表示全部安装完成
6. 后面是我凑字数的，不用看了


## 连接设备
建个文件夹，保存相关脚本文件(注：后续命令中的{}表示这里是个参数，需更换成对应内容)
要安装，首先要连到设备，使用命令`adb connect {ip}:{port}`
一般来说，端口都是5555，可以省略，所以直接`adb connect {ip}`就行
例如：`adb connect 192.168.1.10` (不一定局域网哟，只要能连到就行，可以用端口映射把设备5555端口映射出来)
一两台设备可以手动连，多了就直接写个`adb_connect.bat`(`::`表示注释)
```
adb connect 192.168.1.51
::adb connect 192.168.1.52
adb connect 192.168.1.53
adb connect 192.168.1.54
adb connect 192.168.1.55
::adb connect 192.168.1.56
```
然后打开powershell，运行`.\adb_connect.bat`就行了
这种方式时串行的，需要连到第一个一会才会连第二个，如果设备网络出现问题，就会等连接失败再进行下一个连接
当然你也可以改成并行的

## 获取设备
如果只有一个设备连接，直接使用`adb install {apk}`就可以安装
如果连接了多个设备，执行上述命令会提示超过一个设备连接，无法安装，因此安装的时候需要指定设备，即`adb -s {device} install {apk}`
因此我们需要当前连接的设备列表
执行`adb devices`即可，获取到设备后，然后再挨个执行安装就可以了
写个bat保存设备列表到`devices.txt`中
```adb devices > devices.txt```

## 安装应用
然后就是一个for循环，挨个安装，因为我们的目标是并行安装，所以另外写了一个`adb_install.bat`用于安装
```
for /f  "skip=1 tokens=1 delims=	" %%i in (devices.txt) do (
start adb_install %%i %1 %2
)
```
跳过第一行，因为第一行是`List of devices attached`

## 启动应用
因为`adb install`只负责安装，不负责启动
所以启动我们还需要另外的命令
`adb shell am start {package/activity}`
同样需要指定设备，即`adb -s {device} shell am start {package/activity}`


## 就这样吧
突然发现不知道怎么写，有点乱，所以让你们不要看了的，jio本拿去好了的……




