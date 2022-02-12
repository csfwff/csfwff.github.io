title: Gradle编译 Faile to resolve:support-media-compat
date: '2019-04-09 13:41:48'
updated: '2021-01-05 16:41:02'
tags: [Gradle, Android]
permalink: /articles/2019/04/09/1554788508900.html
cover: https://tmx.fishpi.cn/img/rose-6898032_1920.jpg
categories: 
- 菜鸡修炼手册
---
![af7232906f9315d61fae4ce7b0a554fa.jpg](https://tmx.fishpi.cn/img/rose-6898032_1920.jpg)

![QQ截图20190409110406.png](https://tmx.fishpi.cn/img/20201231105400691.png)
原因是中央仓库里没找到相应的包
解决方案是在项目的build.gradle中

```
allprojects {
    repositories {
        google()
        jcenter()
    }
}
```

把google挪到jcenter前就可以了

