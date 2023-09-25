title: Auto.js自动评选赛复杂食用教程
date: '2019-11-06 13:46:31'
updated: '2021-01-06 11:15:05'
tags: [Auto.js, JavaScript]
permalink: /articles/2019/11/06/1573019191477.html
cover: https://tmx.fishpi.cn/img/squirrel-6935155_1920.jpg
categories: 
- 菜鸡修炼手册

---
![美女334.jpg](https://tmx.fishpi.cn/img/squirrel-6935155_1920.jpg)

&emsp;&emsp;前几天写了一个[自动点击器简单食用教程](https://sszsj.cc/articles/2019/10/15/1571118414618.html)，然后有人推荐我使用Auto.js，🆗，那就去瞅一瞅。百度一番，发现目前只有pro版的安装包，开源版的被下架了，其他人打包的不放心，那就fork一份自己打包，顺便再来个教程。
&emsp;&emsp;本来准备打包成apk的，但是一直找不到打包插件的仓库地址，么的编译，而且考虑到可能会有修改坐标的需求，就不打包了

## 安装apk

点[这里](https://tmx.fishpi.cn/other/org.autojs.autojs.apk)下载apk并安装，注意浏览器劫持，不要下错了!
安装后如图

<img src=https://tmx.fishpi.cn/img/20210104164809631.jpg style="width: 300px;">

## 设置权限，开启无障碍

打开APP会自动弹窗，打开相应权限。然后设置无障碍，点击去设置，选择Auto.js，打开开关。

<img src=https://tmx.fishpi.cn/img/20210104164947818.jpg style="width: 300px;" >

<img src=https://tmx.fishpi.cn/img/20210104165034303.jpg style="width: 300px;" >

<img src=https://tmx.fishpi.cn/img/20210104165111850.jpg style="width: 300px;" >

## 新建项目，导入文件，运行脚本

首先，点[这里](https://tmx.fishpi.cn/other/%E4%BA%91%E8%A3%B3%E7%BE%BD%E8%A1%A3%E8%87%AA%E5%8A%A8%E8%AF%84%E9%80%89.zip)下载需要的脚本文件，然后解压，共5个文件
![QQ截图20191106132018a1a4ebc3.png](https://tmx.fishpi.cn/img/20210104165253553.png)

点击右下角加号，选择项目，差不多这么写，内容随便来

<img src=https://tmx.fishpi.cn/img/20210104165327428.jpg style="width: 300px;">

<img src=https://tmx.fishpi.cn/img/20210104165404100.jpg style="width: 300px;">

创建完成后点击进入，点击右下角，依次导入刚刚加压的5个文件

<img src=https://tmx.fishpi.cn/img/20210104165449443.jpg style="width: 300px;"/>

<img src=https://tmx.fishpi.cn/img/20210104165528615.jpg style="width: 300px;"/>

完成后如图，最后点击这个三角形运行脚本，会出现如下弹窗

<img src=https://tmx.fishpi.cn/img/20210104165621193.jpg style="width: 300px;"/>

<img src=https://tmx.fishpi.cn/img/20210104165713459.jpg style="width: 300px;"/>

然后打开游戏到这个页面，点击对应的选项就行（也可以先开游戏再运行脚本）

<img src=https://tmx.fishpi.cn/img/20210104165752318.jpg style="width: 300px;"/>

最终的效果是自动进入相应的两个评选赛，完成点评，领取奖励后回到这里

## 一些参数设置

考虑到不通手机分辨率不一样，虽然已经做了相应的适配，但不排除仍有问题，所以我把坐标都单独抽了出来，在`configSet.js`文件中，点击文件右边的铅笔✏即可修改，具体含义有注释说明。)

<img src=https://tmx.fishpi.cn/img/20210104165837272.png  style="width: 500px;"/>

至于如何获取坐标，需要打开`开发人员选项`中`指针位置`，顶部即可看到触摸点的坐标

<img src=https://tmx.fishpi.cn/img/20210104165912740.jpg style="width: 300px;"/>

## 小问题

1. 目前已知的问题是有时候会出现无障碍已开启但并未运行的提示，只能通过关闭打开无障碍开关，重启app解决，也可以尝试在电源管理中允许自启动。
2. 小云有时候有活动奖励会加在评选赛奖励里头，所以领取礼包的时候点击次数就少了一次，导致错乱，修改`pxtask`第18行，把`4`（领取金币，祈玉，评选币，关闭弹窗共点击4次）改成`5`就可以了，压缩包里就懒得改了。

## EOF

么有了

