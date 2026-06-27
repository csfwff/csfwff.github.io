---
title: 墨流 ---- 针对 hexo+github pages 的写作客户端
date: 2026-06-27 10:51:23
tags: [hexo]
categories: [菜鸡修炼手册]
permalink: /articels/2026/06/27/1782528683.html
top_img: "https://file.fishpi.cn/2026/06/202606270949462938-98d6dcbd.jpg"
cover: "https://file.fishpi.cn/2026/06/202606270949462938-98d6dcbd.jpg"
---
![20260627094946_29_38.jpg](https://file.fishpi.cn/2026/06/202606270949462938-98d6dcbd.jpg)

墨流，是一款针对hexo+github pages的写作客户端
目标是支持web，linux，win，mac，android（iOS：？先等等吧）
本应用支持从github同步文章，编辑，发布，草稿，图床支持全流程

使用本应用的前提是，你已经在github上创建了账号，创建了仓库，并在仓库里配置好了hexo的自动部署，并且当前能够正常访问

**本应用不负责搭建Hexo以及配置自动部署！！！**
**本应用不负责搭建Hexo以及配置自动部署！！！**
**本应用不负责搭建Hexo以及配置自动部署！！！**

如有需要，请阅读这里的这些教程

[Hexo 博客搭建教程（一）——搭建篇](https://fishpi.cn/article/1781253951404)
[使用hexo框架在github.io上搭建博客网站.](https://cooooing.github.io/%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/%E4%BD%BF%E7%94%A8hexo%E6%A1%86%E6%9E%B6%E5%9C%A8github-io%E4%B8%8A%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2%E7%BD%91%E7%AB%99/)

关于自动部署，你可以直接跟AI说：
```
帮我写一个github action的workflow，自动部署hexo到github pages
```

## 使用步骤

### 配置github

打开应用，点击右上角齿轮进入设置，切换到github设置页
填入token，token获取方式如下
1. 访问[这里](https://github.com/settings/personal-access-tokens)
2. 点击`fine-grained personal access tokens `右侧的 `generate new token`
3. 填入token name，随意填，例如`inkflow`
4. resource owner 选你自己
5. expiration 选中 no expiration
6. repository access 选择 only select repositories，选择你hexo所在的仓库，如果你准备用github图床，加上你图床的仓库
7. permissions 添加contents, read and write，应该会默认带上metadata
8. 点击generate token 生成token，注意及时复制保存，后续将看不到token

填入token后，输入Owner，也就是你的用户名
点击仓库右侧按钮加载仓库列表，选择你hexo所在的仓库，如果没有同步出来，可以手动输入
选择仓库后选择对应的分支
文章目录格式按需配置

### 配置图床

#### github图床

图床中选择github，填入图片仓库和路径，自定义域名
其他按需配置即可

#### 又拍云

图床中选择又拍云，填入空间名称，操作员和操作员密码，具体从[这里](https://console.upyun.com/account/operators/)获取
域名填你空间绑定的域名，其他的按需配置即可

## 同步文章

配置完成后，回到首页，点击右上角同步，如果文章较多，首次同步耗时较久，请耐心等待
后续同步可以增量同步，缩短时间，同步完成后，将展示文章数据

## 编辑文章

点击文章或新增文章进入编辑，右上角四个按钮分别是 元数据编辑，保存本地草稿，保存仓库草稿，正式发布
元数据编辑可以配置元数据，比如标签，分类，如果有未列出的元数据，可以使用自定义字段添加
保存本地草稿将只在本地保存，不推送远程仓库
保存仓库草稿将推送文章指drafts目录
正式发布将推送到posts目录

## 下载地址

请访问仓库的release页面下载
https://github.com/csfwff/inkflow/releases

web版访问下面的地址直接使用
https://inkflow.sszsj.com

当然，现在都还在测试调整阶段，会频繁改动
注意备份远程仓库分支，防止数据丢失！！！

## 问题反馈

请在[摸鱼派](https://fishpi.cn)发帖并@csfwff
如果你没有账号，点[这个链接](https://fishpi.cn/register?r=csfwff)注册

## 最后
本文由墨流inkflow发布
