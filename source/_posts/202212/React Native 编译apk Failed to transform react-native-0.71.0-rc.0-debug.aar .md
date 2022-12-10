title: React Native 编译apk Failed to transform react-native-0.71.0-rc.0-debug.aar 
date: '2022-12-10 11:30:20'
updated: '2022-12-10 11:30:20'
tags: [React Native, Android]
permalink: /articles/2022/12/10/1670643020000.html
cover: https://tmx.fishpi.cn/img/20221210_photographer-gb73056019_1920_16.jpg
categories: 
- 菜鸡修炼手册
---
![图 1](https://tmx.fishpi.cn/img/20221210_photographer-gb73056019_1920_16.jpg)  

一个老项目，突然要修改图标和名称，只能打开重新打包
打包的时候报错了
`Failed to transform react-native-0.71.0-rc.0-debug.aar`
如图
![图 2](https://tmx.fishpi.cn/img/20221210_pic_1670643657419_7.png)  


搜索一番发现是react native版本太旧了
指定回旧版本即可

修改`android\build.gradle`
在开头加入
```
def REACT_NATIVE_VERSION = new File(['node', '--print',"JSON.parse(require('fs').readFileSync(require.resolve('react-native/package.json'), 'utf-8')).version"].execute(null, rootDir).text.trim())
```

在`allprojects`中加入
```
	 configurations.all {
        resolutionStrategy {
            // Remove this override in 0.66, as a proper fix is included in react-native itself.
            force "com.facebook.react:react-native:" + REACT_NATIVE_VERSION
        }
    }
```
![图 3](https://tmx.fishpi.cn/img/20221210_pic_1670643833320_67.png)  

重新打包，完事收工
