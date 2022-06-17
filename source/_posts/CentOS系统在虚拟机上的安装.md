title: CentOS系统在虚拟机上的安装
author: lwl
cover: https://cdn.jsdelivr.net/gh/code-anan/image/20220331190341.png
tags:
  - 虚拟机
  - Vmware
  - CentOS
categories:
  - 记录
my: post/CentOSInstall
---

# 前言

无论是学习linux还是想要部署项目，虚拟机的使用以及linux以及centos操作系统安装都是必不可少的，这里记录一下安装教程

# Vmware安装

首先是官网的下载地址：->[传送门](https://www.vmware.com/go/getworkstation-win)，这里我们下载的是16版本的，下载完成之后双击运行即可（这里可能会需要重启一下电脑）前面一直下一步即可

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331133612.png)

这里我们最好更改一下安装路径 默认安装在C盘，然后下一步一直到输入许可证的地方![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331133952.png)

输入许可证，这里感谢[小翁同学]([还在用旧版本VMWare？不如安装最新的16.0！_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1eo4y1o77M?spm_id_from=333.337.search-card.all.click))提供的三个可使用密钥

`ZF3R0-FHED2-M80TY-8QYGC-NPKYF`

`YF390-0HF8P-M81RQ-2DXQE-M2UT6`

`ZF71R-DMX85-08DQY-8YMNC-PPHV8`

然后输入即安装成功

# 镜像文件下载

1. centos8版本光驱可以在MSDN的[原版软件 (itellyou.cn)](https://next.itellyou.cn/Original/#cbp=Product?ID=dfb4715d-5e52-ea11-bd34-b025aa28351d))下载
2. 但是我们更多用的还是centosOS7的版本，7的版本可以在我的百度网盘进行下载链接：https://pan.baidu.com/s/119ageklrJ4YOFoZ99iFy4Q 
   提取码：20yx
3. 清华大学[清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/))进行下载

# 创新新的虚拟机

vmware安装好之后，我们就可以在上面创建我们的Linux或者其他的系统了

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331134430.png)

然后选择自定义![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331134527.png)

硬盘兼容性选择16.x就行![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331134644.png)

然后选择我们事先准备好的光驱，光驱可以在MSDN的[新官网]([原版软件 (itellyou.cn)](https://next.itellyou.cn/Original/#cbp=Product?ID=dfb4715d-5e52-ea11-bd34-b025aa28351d))下载

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331134747.png)

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331135009.png)

复制到迅雷下载即可![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331135056.png)

继续下一步，设置好账号密码![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331135143.png)

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331135312.png)

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331135512.png)

这里处理器数量根据你电脑的情况选择![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331135557.png)

这里也是一样 如果你电脑内存只有8g还是选择1g即可![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331135700.png)

这里说一下网络类型，根据你的需求进行选择：

桥接网络适用场景：当虚拟机需要被其它电脑访问的时候。例如：虚拟机作为项目的开发环境或者测试环境等

使用网络地址转换适用场景：学习研究技术使用。如：公司的开发环境虚拟机软件环境已经安装妥当，自己又不想在自己的电脑上安装新的环境。这个时候可以把公司的开发环境拷贝一份放到自己的虚拟机上来使用

这里我们选择桥接网络，然后直接无脑下一步即可

# 使用SSH工具连接

上面创建完毕之后，确保宿主机跟我们的虚拟机都可以互相ping通，如果遇到其他问题可以看羊哥的这篇[人手一套Linux环境之：Windows版本教程 (qq.com)](https://mp.weixin.qq.com/s/onVwwEQ1DAwbvK7qS2YNxg))

首先在虚拟机上使用`ifconfig`指令查看分配的ip地址![](https://cdn.jsdelivr.net/gh/code-anan/image/20220331162347.png)

在SSH工具上即可进行连接 账号密码即我们创建虚拟机的账号密码 这样我们就可以实现文件互传啦 虚拟机创建完成还有一些开发需要必要的配置后面再说(￣▽￣)~*