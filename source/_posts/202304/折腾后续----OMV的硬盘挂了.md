title: 折腾后续----OMV的硬盘挂了
date: 2023-04-12 14:52:06
tags: [OMV,NAS]
categories: 
- 菜鸡修炼手册
permalink: /articles/2023/04/12/1681282326000.html
top_img: https://tmx.fishpi.cn/img/20230412_soldering-7897827_1920_20.jpg
cover: https://tmx.fishpi.cn/img/20230412_soldering-7897827_1920_20.jpg
---
![图 1](https://tmx.fishpi.cn/img/20230412_soldering-7897827_1920_20.jpg)  

## 前情提要
看这里->[周末折腾----记OMV升级失败](https://www.sszsj.cc/articles/2022/11/07/1667798327000.html)
那一次手贱升级失败后，被迫换了硬盘重新安装
之前还想着没机会再拆开了
可惜啊可惜啊，现在又不得不拆开了

## 全是报错
在那之后，我又部署了docker，然后部署了rust desk，ddns等服务，
本来没啥事，用着也很顺畅
最近不知怎么的，老是挂掉，开盖一看全是这个报错
```
ata1.00: status: { DRDY ERR }
ata1.00: error:  { UNC }
ata1.00: failed command: READ FPDM QUEUED
```
以及一些IO错误
![图 2](https://tmx.fishpi.cn/img/20230412_518AB28DCE9AAA8FCEA9C3452B3A3CFB_0.jpg)  

## 百度一下
以我目前的水平，掉进的坑肯定有前人掉进去过
所以，不管怎么说，先百度一下
差不多就是因为异常断电导致硬盘坏了
仔细想想也雀食是这么一回事，有次断电后就经常出现这个现象

## 拆机拔硬盘
简单想了想，ovm上直接检测坏道之类的用不习惯
那就直接拆机拔硬盘吧，反正几个螺丝拧一下
顺便可以拆开看看这台笔记本里面的结构
《原装正品》
![图 3](https://tmx.fishpi.cn/img/20230412_QQ图片20230412155508_24.jpg)  
主板和硬盘，小巧玲珑的风扇
![图 4](https://tmx.fishpi.cn/img/20230412_QQ图片20230412155733_10.jpg)  
百度了一下，是记忆内存出品的2G内存条（只有一个内存插槽）
![图 5](https://tmx.fishpi.cn/img/20230412_B112C58E2F7E7EBB35536494B5FFEC87_44.jpg)  

## 坏道检测
拔出硬盘，装入上次买的硬盘盒
![图 6](https://tmx.fishpi.cn/img/20230412_7E3EFB07D0DB5CC738866E97E5818AC9_78.jpg)  
插上电脑，打开diskgenius(奇怪，为什么我电脑里会有这个？)
开始坏道检测
一看要好几个小时，直接去睡觉了
一觉醒来，跑完了
![图 7](https://tmx.fishpi.cn/img/20230412_8B27337571593666FA5D81ADC987B6EE_23.jpg)  
凉凉，好多红的
因为修复坏道可能损坏数据，所以暂时不做修复

## 然后呢？
然后，当然是搬出另外一个淘汰的笔记本，顶替这个辣鸡捡来的笔记本
![图 8](https://tmx.fishpi.cn/img/20230412_C6AFD118BAD0CC20E21EB4B292ACAD97_8.jpg)  
![图 9](https://tmx.fishpi.cn/img/20230412_105235A3B4BD3B129489B3F298F79BB9_8.jpg)  
这个笔记本是领导上大学的时候用的
经历了汤汤水水的洗礼，键盘部分按键一直处于按下的状态
无奈，我只能拆开，把键盘的排线拔了
之前的硬盘装上，准备重新安装系统
然后另外下单了一个光驱硬盘支架，到时候把有坏道的硬盘数据弄出来修复一下，直接上光驱位
（不得不说，thinkpad的设计是真的舒服，清灰换硬盘加内存换光驱，都是几个螺丝的事，哪像我的acer，清个灰直接拆成碎片）
![图 10](https://tmx.fishpi.cn/img/20230412_36F5E4CBCF380A3546BAB23FE877D450_43.jpg) 
反正不存重要数据，几块钱都无所谓

就酱，准备开干

