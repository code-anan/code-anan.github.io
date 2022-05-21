title: 如何在本地运行一个非maven项目
author: lwl
date: 2021-10-30 18:54:48
tags:
  - Maven
cover: https://fastly.jsdelivr.net/gh/code-anan/image/src=http---www.aspku.com-uploads-allimg-180815-153453D21-0.jpg&refer=http---www.aspku.jpg
my: post/oldproject
categories:
  - Maven
---
# 前言
非maven的项目我们可以统称为老项目了、一般现在的maven项目，除了必要的配置文件中的地址需要改动一下，其他就没有什么需要改的了、但是老项目却不是这样，配置起来非常麻烦、这里记录一下老项目的启动配置过程

# 必要配置
1. 第一步当然是把代码拉下来 然后`check out`（需要先下载[Tortoise SVN](https://osdn.net/projects/tortoisesvn)）![](https://fastly.jsdelivr.net/gh/code-anan/image/20211030185845.png)
2. 确保整个项目的jdk正常![](https://fastly.jsdelivr.net/gh/code-anan/image/20211030191611.png)
3. 删除无用爆红的jar包![](https://fastly.jsdelivr.net/gh/code-anan/image/20211030191708.png)
4. 添加web目录下的jar包一般放在web-inf中的lib目录下![](https://fastly.jsdelivr.net/gh/code-anan/image/20211030191911.png)
5. 添加tomcat的jar包![](https://fastly.jsdelivr.net/gh/code-anan/image/20211030192004.png)
6. 创建web（在Facets和modules中都一样）![](https://fastly.jsdelivr.net/gh/code-anan/image/20211030192240.png)
7. Artifact创建好之后，把缺失的文件都添上

# Tomcat配置
最后就是必须的Tomcat配置了、非maven的项目都不会内置tomcat、所以这里我们需要手动配置![](https://fastly.jsdelivr.net/gh/code-anan/image/20211030192540.png)注意不要选成tomee
![](https://fastly.jsdelivr.net/gh/code-anan/image/20211030192637.png)
选择你电脑上的tomcat进行配置
然后Deployment（太简单了不用说了）
然后启动成功！
![](https://fastly.jsdelivr.net/gh/code-anan/image/20211030193248.png)

# 最后
如果以上配置还不能正常启动、那就是一些必要的配置没有配置好、例如我这个项目的`frame-plugin.xml`中的两项启动方式就需要手动改一下，这里可以询问你的同事他们一定知道、完结撒花！(￣▽￣)／