title: 使用Mailu搭建自己的域名邮箱
date: '2023-03-06 18:49:18'
updated: '2023-03-06 18:49:18'
tags: [Mailu, 邮箱]
permalink: /articles/2023/03/06/1678099758000.html
cover: https://tmx.fishpi.cn/img/20230306_girl-g7cba9628f_1920_75.jpg
categories: 
- 菜鸡修炼手册
---
![图 1](https://tmx.fishpi.cn/img/20230306_girl-g7cba9628f_1920_75.jpg)  

[【活动】送服务器啦！蓝易云征文大赛](https://fishpi.cn/article/1677580643943)


## 1. intro
平时用域名邮箱，一般都选择现有的企业邮箱服务，比如阿里的，腾讯的，
但是这些免费的只能绑一个域名，腾讯的还得注册企业微信，
如果有多个域名，这些就不够用了。
当然，如果只是收信，可以选择使用clouflare的转发服务，这里就不细说了。
因为我有多个域名邮箱的需求，这里选择自己搭建一个邮箱服务。
我搜索对比了MailCow 和 Mailu，
种种尝试使用MailCow搭建失败后，最后选择了用Mailu

## 2. 准备
首先，要有一台服务器（废话），然后确认25端口是否打开
检查方式是用 telnet 测一下 比如 `telnet mx1.qiye.aliyun.com 25`(把中间的域名换成你的IP)
正常情况下，你会是一直 trying，因为大部分厂商都是把25端口关掉的
这时候你需要去控制台提工单或者联系客服开通
开通成功后再测试，就会是
```
Trying 140.205.96.211...
Connected to mx1.qiye.aliyun.com.
Escape character is '^]'.
220 mx1.aliyun-inc.com MX AliMail Server
Connection closed by foreign host.
```
只要不是一直trying

## 3. 域名
登录域名解析控制台
创建一个A记录，记录为 `mail`，值为你的IP
然后创建一个MX，记录为 `@`, 值为之前创建的`mail`
大概就是这样
![图 2](https://tmx.fishpi.cn/img/20230306_pic_1678100971176_98.png)  
然后登陆服务器，修改hostname为mx记录的值
一般在 /etc/hostname 编辑文件即可

## 4. docker
mailu使用docker部署，所以需要安装docker，另外还需要docker compose
已安装的可以跳过，没安装的搜索一下安装

## 5. Mailu
mailu 提供了一个网页来辅助生成配置，访问[https://setup.mailu.io/](https://setup.mailu.io/)
![图 3](https://tmx.fishpi.cn/img/20230306_pic_1678101292839_35.png)  
选择Compose，下一步，填上基本信息，然后勾上启用admin UI
![图 4](https://tmx.fishpi.cn/img/20230306_pic_1678101577553_51.png)  
然后选择邮箱网页样式
![图 5](https://tmx.fishpi.cn/img/20230306_pic_1678101638978_83.png)  
下面三个选项分别是防病毒，webdva，邮件代收，看情况选择，配置不高的可以不开杀毒那个
然后是配置网络，ipv4填上公网IP，内网地址不用动，v6一般不用开，最下面的域名填mx记录的值
![图 6](https://tmx.fishpi.cn/img/20230306_pic_1678101779997_48.png)  
数据库选默认的sqlite就行了，当然也可以换别的
![图 7](https://tmx.fishpi.cn/img/20230306_pic_1678101872254_55.png)  
点击setup

## 6. 安装
回到服务器上，在你想要的地方创建 `mailu`目录，并下载配置文件
![图 8](https://tmx.fishpi.cn/img/20230306_pic_1678101977077_50.png)  
下载完成后检查下配置，当然一般没啥事，直接进入下一步
![图 9](https://tmx.fishpi.cn/img/20230306_pic_1678102060051_35.png)  
顺利的话，应该能看到后台页面了

## 7. 顺利？
顺利？对我来说那是必然不可能的，我在启动到时候，发现mailu-front就是起不来
报错内容为Error starting userland proxy: listen tcp xx.xx.xx.xx:995: bind: cannot assign requested address.
这时候，需要打开之前下载的`docker-compose.yml`
修改front中ports部分，去掉前面的IP
然后再启动就行了
![图 10](https://tmx.fishpi.cn/img/20230306_pic_1678103654058_98.png)  

## 8. 再次配置解析
登陆邮箱后台，点击左侧邮件域
点击域名列表左侧的详细信息
![图 11](https://tmx.fishpi.cn/img/20230306_pic_1678103764332_28.png)  
然后点击右上角生成密钥
然后在域名解析控制台添加对应的解析
这一步，主要是为了防止其他邮箱，将我们发送的邮件归入垃圾邮件
至此，配置基本完成

## 9. 测试一下
尝试使用域名邮箱给自己的其他邮箱发送邮件，测试一下能不能发送成功
收到后再回复一下，测试能不能收到邮件
如果25端口没开，基本是发不出去，收不到
完了以后，可以点击左侧客户端设置
可以在手机上邮件应用里登陆邮箱

