title: 关于hexo部署到github上一直报错的原因（已更新）
author: lwl
tags:
  - 部署报错
cover: 'https://cdn.jsdelivr.net/gh/code-anan/image/20210616135114.png'
categories:
  - Hexo
  - 记录
my: post/hexo error
date: 2022-02-22 19:27:52
---
# 前言
每次写完博客想要部署到github上的时候总是会出现各种各样的原因，对于这些原因我找过很多种答案，其实对于大部分同学而言，部署不上去的一个最大的原因就是因为github上不稳定，所以解决这个问题大部分情况都可以迎刃而解，通过这种方式修改访问github相关的网站也会变得飞快！
# 必要的修改
修改的方式同样是熟悉的`hosts`文件，来到目录`C:\Windows\System32\drivers\etc`，需要是管理员权限才可以进行修改，对于要加入的东西，好多人给的都是一大串的，其实很多都是不必要的，其实只需要添加两个东西就可以:
```
xxxxxx  github.com
xxxxxx  github.global.ssl.fastly.net 
```
例如我添加的
```
140.82.114.3 github.com 
199.232.69.194  github.global.ssl.fastly.net 
```
把前面的`ip`换成你本机查到的才有效，这也是为什么直接复制出错的原因，对于这两个网址ip的查找方式有两种：
1. 通过网址查找：->[传送](https://www.ipaddress.com/ip-lookup)
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210616133559.png)
依次输入`github.com`和`github.global.ssl.fastly.net `然后点击`Lookuo`即可查到相应的ip
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210616133755.png)
2. 第二种方式比较简单 直接打开cmd在任意目录下 通过`ping`命令即可获取到两个网址的ip
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210616133912.png)
# 补充
如果以上步骤还是访问缓慢，可尝试`hosts`再在下面添加一些代码
```
#github related website
140.82.112.4 github.com
151.101.185.194 github.global.ssl.fastly.net
203.98.7.65 gist.github.com
13.229.189.0 codeload.github.com
185.199.109.153 desktop.github.com
185.199.108.153 guides.github.com
185.199.108.153 blog.github.com
18.204.240.114 status.github.com
185.199.108.153 developer.github.com
185.199.108.153 services.github.com
192.30.253.175 enterprise.github.com
34.195.49.195 education.github.com
185.199.108.153 pages.github.com
34.196.237.103 classroom.github.com

140.82.112.3 github.com
```
> 还有个常用的指令`ipconfig/flushdns`用来刷新DNS缓存，可以在配置完之后刷新一下重新进入浏览器
# 最新解决方案
经过测试 以上的方案存在各种缺陷且不稳定，最近发现一个完美解决这个问题的方案，即下载[网易UU加速器](https://uu.163.com/)下载之后搜索`学术资源`然后进行加速即可 无论是上传到github还是访问github 速度都得到质的提升！
通过上面步骤访问github的速度变得飞快，并且部署也不会出问题了(￣▽￣)~*