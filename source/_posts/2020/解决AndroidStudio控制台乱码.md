title: 解决Android Studio控制台乱码
date: '2020-07-03 13:04:07'
updated: '2021-01-05 17:00:03'
tags: [Android]
permalink: /articles/2020/07/03/1593752646951.html
cover: https://tmx.fishpi.cn/img/road-4730553_1920.jpg
categories: 
- 菜鸡修炼手册

---
![201904091554817248554774.jpg](https://tmx.fishpi.cn/img/road-4730553_1920.jpg)

最近升级了AS，突然发现控制台的中文乱码了，搜了下解决方案，记录下省的再找。

---

依次点开 Help->Edit Custom VM Options...
![image.png](https://tmx.fishpi.cn/img/20210105091340942.png)

在编辑器中输入`-Dfile.encoding=UTF-8`
![image.png](https://tmx.fishpi.cn/img/20210105091441332.png)

重启AS，输出正常中文

