title: TortoiseGit和Git使用教程
author: lwl
date: 2021-06-17 09:45:58
tags:
 - TortoiseGit
categories:
 - Git
 - 工具
cover: https://cdn.jsdelivr.net/gh/code-anan/image/20210617110058.png
my: post/TortoiseGit
---
# 环境安装
1. 首先git肯定是已经下载好的，如果没有需要先下载->[传送门](https://git-scm.com/downloads)
2. TortoiseGit的下载地址->[传送门](https://tortoisegit.org/download/)
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617095428.png)
根据你的电脑情况下载对应的版本，并且下载对应的汉化包，下载完一直下一步就可以，`注意汉化包需要后安装`
3. 相关的配置
+ 最好先选定一个文件夹用来存放以后Git的项目，例如在D盘下创建一个文件夹
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617095758.png)
+ 然后鼠标右键，进入`Settings`中
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617100039.png)
汉化包下载之后，就可以看到语言栏有了中文，选择中文，应用确定菜单就都变成中文的了![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617100136.png)
+ 右键菜单可以根据你的习惯进行设置，本人设置如下
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617100803.png)

# 使用TortoiseGit从Github上拉取代码
上面的都配置好之后，在任意文件位置右键选择`Git克隆`![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617101121.png)
输入对应的`url`和要存放的目录位置即可，这里为了演示我选择克隆我的Github仓库![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617101239.png)
找到你想要克隆的github上的项目
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617102309.png)
复制链接到我们的`url`上![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617102423.png)

点击确定
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617102243.png)
可以看到正在从github上进行克隆到本地

![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617102347.png)
克隆成功

![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617102549.png)
可以在本地查看代码了
# 创建本地版本库
在这里创建一个版本库
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617104232.png)
注意这里不要勾选，直接确定
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617104330.png)
然后就可以看到一个`.git`的文件夹（看不到打开隐藏的项目）
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617104558.png)
这个文件夹创建了就可以，不需要我们修改




