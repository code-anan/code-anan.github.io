title: 一劳永逸！这次真的可以实现打开github不卡顿了
author: lwl
tags:
  - github加速
cover: >-
  https://cdn.jsdelivr.net/gh/code-anan/image/src=http---oss.heipg.cn-2020-03-1583939289-25b2916b5c49db6.jpg&refer=http---oss.heipg-1.jpg
categories:
  - Github
my: post/switchhosts
sticky: 9
date: 2021-10-08 22:05:42
---
# 前言
相信很多人包括我在很长一段时间内，都遇到过这种经历，今天想去访问一下github，结果发现github怎么都进不去，于是我们翻了各种方案各种方法，例如更改hosts文件、刷新路由配置等等。。。然后这种方案虽然有时候确实能暂短的解决，但是往往第二天会发现又进不去了，归根结底还是因为github的ip不是固定的而且受到国内的限制导致地址一直不是固定的往往就会访问到错误的节点从而打不开或是很缓慢，不过好消息是现在终于有了一劳永逸的方案

# 解决方案
1. 下载软件
这里我们需要用到的软件是[Swithchhosts](https://github.com/oldj/SwitchHosts/releases)（点击即可进行跳转），下载步骤感觉就不用了多说了，选择适合你的版本进行下载即可![](https://cdn.jsdelivr.net/gh/code-anan/image/20211008221533.png)
2. 下载完之后安装到合适的目录然后打开样子如下图所示![](https://cdn.jsdelivr.net/gh/code-anan/image/20211008221810.png)
3. 填写如下配置即可![](https://cdn.jsdelivr.net/gh/code-anan/image/20211008221926.png)
url:`https://cdn.jsdelivr.net/gh/isevenluo/github-hosts/hosts`
4. 然后打开![](https://cdn.jsdelivr.net/gh/code-anan/image/20211008222042.png)

# 总结
这个软件的本质是可以动态的帮我们改变hosts文件，虽然也是改hosts文件但是却不用我们手动写 只要打开软件即可 所以还是很方便的(￣▽￣)／
最后：虽然它能动态的帮我们改写地址 但是偶尔还是会发现网址打不开 但是相较于之前已经方便了许多 总体来说还是很值得一试的