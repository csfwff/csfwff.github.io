title: 使用Caddy在非443端口申请SSL证书
date: '2023-03-07 11:18:38'
updated: '2023-03-07 11:18:38'
tags: [Caddy, https, SSL]
permalink: /articles/2023/03/07/1678159118000.html
cover: https://tmx.fishpi.cn/img/20230307_music-player-g1a3e4cc3f_1920_94.jpg
categories: 
- 菜鸡修炼手册
---
![图 1](https://tmx.fishpi.cn/img/20230307_music-player-g1a3e4cc3f_1920_94.jpg)  

## Caddy
众所周知，Caddy是一个用Go实现的web服务器
（虽然我只是拿来当nginx用）
而Caddy有一个重要的特性
就是会自动帮你申请SSL证书
>当符合下面一些合理的标准时，Caddy会自动为所有站点启用HTTPS:
> - 主机名
>    - 不为空
>    - 不是localhost
>    - 不是一个IP地址
>    - 不超过一个通配符（*）
>    - 通配符必须是最左边的标签
> - 没有显式指定端口为80
> - 没有显式指定使用http协议
> - TLS没有在站点的定义中被关闭
> - 不是你自己提供的证书和密钥
> - Caddy能够绑定到端口80和443（除非使用DNS验证）

## 非443端口
众所周知，https的默认端口是443
那如果我准备用别的端口呢？
caddy提供了各种dns验证的模块
[All Modules](https://caddyserver.com/docs/modules/)
在这里找到相应的dns提供商
大部分都有，没有就用`lego_deprecated`
我的在dnspod，所以选择`dns.providers.dnspod`

## 安装module
首先安装好caddy
```
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```
然后安装module
```
 sudo caddy add-package github.com/caddy-dns/dnspod
```

## 获取appid和token
登陆dnspod，点击右上角头像，选择API密钥
选择DNSPod Token
创建一个密钥，记下id和token

## Caddyfile
编辑 `/etc/Caddyfile`
在开头写上
```
{
	http_port 1999  # http端口
	https_port 2000  # https端口
}
```
用于指定端口

然后创建一个站点，加入一个tls，同时填上appid和token
```
test.sszsj.cc {
	reverse_proxy localhost:3001
	tls {
 		dns dnspod {换成appid},{换成apptoken}  # 例如 11111,dsadsadassdadsadsa
	}
}
```

## 启动caddy
执行命令
```
caddy start --config /etc/caddy/Caddyfile
```
理论上应该能在控制台看到申请证书的过程
这时候https访问2000端口，应该就能正常看到页面了

