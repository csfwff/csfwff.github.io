---
title: Android 发布lib到JitPack
date: 2021-09-09 18:41:13
tags: [Android,JitPack]
categories: 
- 菜鸡修炼手册
permalink: /articles/2021/09/09/1631184077130.html
top_img: https://tmx.fishpi.cn/img/stones-6660734_1920.jpg
cover: https://tmx.fishpi.cn/img/stones-6660734_1920.jpg
---
![1628345033618.jpg](https://tmx.fishpi.cn/img/stones-6660734_1920.jpg)

## =.= 
之前一直以为发布到Jitpack，jcenter（这个要没了）这种中央仓库很麻烦，要注册账号啥的，所以一直不愿意搞
结果搜了一下，似乎不需要，只要配置一下就好了的
虽然不麻烦，但还是遇到了几个坑，简单记录一下

## \*.\*
首先建好github仓库，把代码传上去
这一步正常操作，没有问题

## 0.0
在项目的根目录下的 `build.gradle` 文件里面添加Jitpack 插件
```
classpath 'com.github.dcendents:android-maven-gradle-plugin:2.1'
```
没问题

在 repositories 下面添加
```
maven { url "https://jitpack.io" }
```
问题来了
升级到`gradle:7.0.0`后，项目的`build.gradle`里已经没有了`allprojects`，所以也没有`repositories`
如果强行添加，sync就会报
```
Build was configured to prefer settings repositories over project repositories but repository 'maven' was added by build
```
其实是因为这个配置已经移到了`setting.gradle`
打开，加上，解决

## >.<
按照百毒搜的，要在library module的build.gradle加插件
```
apply plugin: 'com.github.dcendents.android-maven'
```
或者
```
plugins {
    ...
    id 'com.github.dcendents.android-maven'
}
```
这时候，sync会报
```
Unable to load class 'org.gradle.api.publication.maven.internal.MavenPomMetaInfoProvider'.
```
什么的
搜索一番，发现需要把插件换成`maven-publish`，也就是
```
plugins {
    ...
    id 'maven-publish'
}
```

然后配置好相关信息
```
project.afterEvaluate {
    publishing {
        publications {
            // Creates a Maven publication called "release".
            release(MavenPublication) {
                // Applies the component for the release build variant.
                from components.release

                // You can then customize attributes of the publication as shown below.
                groupId = 'com.xiamo'
                artifactId = 'example'
                version = '1.0'
            }
        }
    }
}

```
完成后push

## >.0
去github创建一个release，填好tag，然后release
然后就可以去[JitPack](https://jitpack.io/)
输入你的仓库地址，点look up
就能查到啦