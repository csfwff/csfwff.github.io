---
title: Shopify皮肤修改记录
date: 2022-08-03 13:23:22
tags: [CSS,前端,Shopify]
categories: 
- 菜鸡修炼手册
permalink: /articles/2022/08/03/1659504202231.html
top_img: https://tmx.fishpi.cn/img/flowers-7233987_1920.jpg
cover: https://tmx.fishpi.cn/img/flowers-7233987_1920.jpg
---
![1628345033618.jpg](https://tmx.fishpi.cn/img/flowers-7233987_1920.jpg)

## 顶部广告图修改
皮肤自定义里**Header Default**，编辑**Top Message Text**，内容如下`{表示说明}`，改的时候去掉大括号
``` html
<a href="{换成点击跳转地址}" style="display:block">		 
<picture>
  <source media="(max-width: 500px)" srcset="{换成手机端图片地址}">
  <source media="(min-width: 501px)" srcset="{换成PC端图片地址}">
  <img src="{换成PC端图片地址}">
</picture>
</a>
```

## logo和nav居中修改
`theme-styles-responsive.scss.liquid` 6712行附近
``` css
  .site-nav {
        margin: 0;
        display:flex;  /*新增加*/
        justify-content:center; /*新增加*/

        .icon-dropdown,
        .menu-mb-title {
            display: none;
        }
```
`header.liquid` 24行，`container`增加`max-width`
``` html
<div class="header-bottom" data-sticky-mb>
    <div class="container" style="max-width:1350px;"> <!-- 这里加max-width -->
       <div class="wrapper-header-bt">
```


