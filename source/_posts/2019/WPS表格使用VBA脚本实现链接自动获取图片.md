title: WPS表格使用VBA脚本实现链接自动获取图片
date: '2019-02-01 22:06:52'
updated: '2021-01-05 16:38:55'
tags: [WPS, VBA, 脚本]
permalink: /articles/2019/02/01/1549029322040.html
cover: https://tmx.fishpi.cn/img/road-5089188_1920.jpg
categories: 
- 菜鸡修炼手册
---
![5ac1e4a0e7bce7250515c894.jpg](https://tmx.fishpi.cn/img/road-5089188_1920.jpg)
WPS表格使用VBA脚本实现链接自动获取图片
记录代码防止忘记

1. 安装vba
   一般用户的wps是不带vba的，无法启用宏，需要单独安装，搜索下载安装就行
2. 写代码
   切换到开发工具，打开VB编辑器，选择当前的工作表，贴入代码，收工

```
Private Sub Worksheet_Change(ByVal Target As Range)
    With Target
        If .Column = 1 And .Count = 1 Then '修改的单元格在A列，且只修改一个单元格
            If Left(.Value, 7) = "http://" Then '如果单元格内容为网址
                '添加网络图片，并设置为图片大小位置随单元格变化而变化
                ActiveSheet.Shapes.AddPicture(.Value, msoCTrue, msoCTrue, .Left, .Top, .Width, .Height).Placement = xlMoveAndSize
                .WrapText = True '单元格设置为自动换行，以隐藏网址
            End If
        End If  
    End With   
  End Sub
```

3. 实现效果
   在第一列插入`http://`开头的文本，会自动下载图片，插入对象，并自动设为单元格大小

