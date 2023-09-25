title: 使用nodejs和adb shell 操作手机
date: '2019-05-13 21:47:53'
updated: '2021-01-06 11:03:37'
tags: [Node.Js, JavaScript]
permalink: /articles/2019/05/13/1557755273356.html
cover: https://tmx.fishpi.cn/img/flamingo-6922032_1920.jpg
categories: 
- 菜鸡修炼手册
---
![201904091554816947676263.jpg](https://tmx.fishpi.cn/img/flamingo-6922032_1920.jpg)

### intro

&emsp;&emsp;一直在玩一款叫《云裳羽衣》的换装手游（不要问我为什么一个糙汉子玩换装游戏，只是比较喜欢古风和收集元素而已，所有非常合适）
&emsp;&emsp;然而游戏中有一个的日常叫评选赛，就是点评别人的搭配。本来也没什么，奖励是固定的，随便点评就是了，问题出在数量上，一天一共要点评60次，再加上难熬加载的时间，使得这个日常烦人且无趣。

<img src="https://tmx.fishpi.cn/img/RYw_Screenshot20190513212630-05aacbfa.jpg" width='30%'/>

### 分析

&emsp;&emsp;考虑到流程，每次出现需要点评的时候，先点击底部的评分，一般固定4分，然后点击跳过，加载下一条，然后循环操作。操作是固定的，那么就用电脑通过adb来替我们操作

### 知识点

1. 使用node 的child_process来执行shell命令

> * `child_process.exec(command[, options][, callback]) `
>   启动子进程来执行shell命令,可以通过回调参数来获取脚本shell执行结果

* `child_process.execfile(file[, args][, options][, callback]) `
  与exec类型不同的是，它执行的不是shell命令而是一个可执行文件
* `child_process.spawn(command[, args][, options])`
  仅仅执行一个shell命令，不需要获取执行结果
* `child_process.fork(modulePath[, args][, options])`
  可以用node 执行的.js文件，也不需要获取执行结果。fork出来的子进程一定是node进程

在这里，我们使用exec就可以了
2. 使用adb shell 操作手机
由于我们只需要点击操作，所以shell语句如下
`adb shell input tap x y`
最后两个为点击点的x坐标和y坐标
对于坐标，可以在开发者选项中打开**指针位置**，手机会顶部显示当前触控点的坐标

### 上代码

好了，内容就这些，剩下的就是写个循环，定时执行shell分别点击两个坐标点就行了，完整代码如下，测试后大概没啥问题🤣🤣🤣

```
var process = require('child_process');

const exec = (shell) => {
  process.exec(shell, (error, stdout, stderr) => {
    if (error != null) {
      console.log('exec error:' + error);
    }
  })
}

const pinxuanTask = () => {
  console.log("------开始评选------");
  let i = 1;
  pinxuanAction(i);
  let timer = setInterval(() => {
    //操作一次
    i++;
    pinxuanAction(i);
    if (i == 35) {
      clearInterval(timer);
    }
  }, 6000);
}

const pinxuanAction = (i) => {
  console.log("----第" + i + "次");
  console.log("-----点4星----");
  exec(`adb shell input tap 689 1803`)
  setTimeout(() => {
    console.log("-----点跳过----");
    exec(`adb shell input tap 1003 1844`)
  }, 1000)
}

pinxuanTask();
```

---

### bat批处理版本

20190527更新
好吧 很多小伙伴没装node，那就给一个bat批处理吧

```
@echo off

chcp 65001
echo [开始评选]
echo ------------------
rem 这里次数定了35
for /l %%i in (1,1,35) do ( 
	echo 第%%i次
	rem 这里是4星的位置
	adb shell input tap 689 1803  
	rem 靠ping来延时，自行调整数字大小
	ping 0.0.0.0  -n 1 > null   
	rem 这里是跳过的位置
	adb shell input tap 1003 1844
	rem 靠ping来延时，自行调整数字大小
	ping 0.0.0.0  -n 6 > null	
)
pause
```

复制后新建个文本文档，贴进去保存为bat格式运行，请先插上手机并启动adb，我的习惯是执行一遍adb devices，当然，前提是电脑上有adb……

