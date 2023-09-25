title: React Native debug正常，release闪退
date: '2019-03-14 20:35:49'
updated: '2021-01-05 16:39:41'
tags: [ReactNative, Android]
permalink: /articles/2019/03/14/1552566949418.html
cover: https://tmx.fishpi.cn/img/leaves-6932148_1920.jpg
categories: 
- 菜鸡修炼手册
---
![5ab204abca886.jpg](https://tmx.fishpi.cn/img/leaves-6932148_1920.jpg)

今天准备打包一版react native app
debug的时候，那么进行打包release
打包成功，安装打开后闪退
查看日志，显示未能加载index.android.js
所以，解决方案：在android工程assets下生成相应的bundle文件

在工程目录下输入命令如下：

```
react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res/
```

继续打包，正常

