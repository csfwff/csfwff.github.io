---
title: Scrcpy常用快捷键
date: 2021-11-19 11:53:03
tags: [Android,adb,Scrcpy]
categories: 
- 菜鸡修炼手册
permalink: /articles/2021/11/19/1637293983000.html
top_img: https://tmx.fishpi.cn/img/smartphone-5601536_1920.jpg
cover: https://tmx.fishpi.cn/img/smartphone-5601536_1920.jpg
---
![1628345033618.jpg](https://tmx.fishpi.cn/img/smartphone-5601536_1920.jpg)

又是一个，菜鸡懒得搜，copy一份记录

Scrcpy是一款在电脑上通过adb控制手机的软件，主要是我现在开发很多时候设备没在身边，不好操作，所以找到这么一款软件

在下列表格中，MOD是热键的修饰键， 默认是（左）Alt或者（左）Super。
您可以使用 --shortcut-mod后缀来修改它。
可选的按键有lctrl、rctrl、 lalt、ralt、lsuper和rsuper。如下例：
使用右侧的Ctrl键scrcpy --shortcut-mod=rctrl
使用左侧的Ctrl键、Alt键或Super键scrcpy --shortcut-mod=lctrl+lalt,lsuper
一般来说，Super就是Windows或者Cmd。


|操作|快捷键|
|---|---|
|全屏|	MOD+f|
向左旋转屏幕|	MOD+← (左)
将窗口大小重置为1:1 |(像素优先)	MOD+g
将窗口大小重置为消除黑边|	MOD+w or 双击1
点按 主屏幕 |	MOD+h or 点击鼠标中键
|点按 返回|	MOD+b or 点击鼠标右键2|
点按 切换应用|	MOD+s
点按 菜单 （解锁屏幕）|	MOD+m
点按 音量+	|MOD+↑ (up)
点按 音量-	|MOD+↓ (down)
点按 电源	|MOD+p
打开屏幕	|点击鼠标右键2
关闭设备屏幕（但继续在电脑上显示）|	MOD+o
打开设备屏幕	|MOD+Shift+o
旋转设备屏幕	|MOD+r
展开通知面板	|MOD+n
展开快捷操作	|MOD+Shift+n
复制到剪贴板3	|MOD+c
剪切到剪贴板3	|MOD+x
同步剪贴板并黏贴3|	MOD+v
导入电脑剪贴板文本|	MOD+Shift+v
打开/关闭FPS显示（在 stdout)|	MOD+i
捏拉缩放	|Ctrl+点按并移动鼠标




1. 双击黑色边界以关闭黑色边界 
2. 点击鼠标右键将在屏幕熄灭时点亮屏幕，其余情况则视为按下 返回键 。
3. 需要安卓版本 Android >= 7。(这条似乎不需要)
