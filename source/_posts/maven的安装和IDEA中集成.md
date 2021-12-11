title: maven的安装和IDEA中集成
author: lwl
cover: 'https://cdn.jsdelivr.net/gh/code-anan/image/20211003220215.png'
tags:
  - Maven
categories:
  - maven
my: post/maveninstall
date: 2021-10-03 22:45:31
---
# 前言
maven一直是一款非常热门的java项目构建系统或者说是一个工具，最通俗易懂且最强大的功能就是它可以帮助我们管理各种各样的jar包，不用像之前一样需要一个jar包就要自己拖到lib目录下，当然他还有其他的功能、这些只有在项目中自己体会才能理解，下面直接开始介绍安装过程！
> 本文主要参考[碎冰的文章](https://www.cnblogs.com/iceb/p/7097850.html)

# 安装步骤

## maven下载
首先我们需要来到maven的官网进行下载->[传送门](http://maven.apache.org/download.cgi)
版本我们选择最新版就可以(*^▽^*)，这里是3.8.2版本![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003225228.png)
这里我们选择下载压缩包，下载完之后这里建议解压到一个新的目录下（即我们maven安装的目录），这个目录下我们存放我们下载的maven和`本地仓库`（这里我选择解压到新建的一个文件夹`maven`下），本地仓库的概念后面会讲解
![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003225437.png)
![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003225620.png)
## 配置环境变量
环境变量位置：此电脑->属性->`高级系统设置`->`环境变量`，然后`新建`![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003225922.png)
变量名：`M2_HOME`
变量值：刚才解压的maven位置，例如`D:\maven\apache-maven-3.8.2`
![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003230140.png)
然后是系统变量`path`中添加`%M2_HOME%\bin`或者`D:\maven\apache-maven-3.8.2\bin`，这两种方式都可以输入完之后确定退出
![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003230429.png)
## 测试一下是否安装成功
来到我们的cmd中输入`mvn -v`或者`mvn -version`，出现版本号等信息安装完成!![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003230710.png)
## 本地仓库的配置
如果这里我们不设置的话，默认的仓库位置是`C：用户\name\.m2\`，这样太占用我们c盘的空间，所以这里我们选择设置到我们刚才新建的D盘下的maven文件夹下，先新建一个文件夹`repository`作为我们本地的仓库位置![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003231013.png)
然后找到安装的maven下的conf下的`settings.xml`,![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003231307.png)
打开找到`localrepository`,目录为我们刚才新建的文件夹repository，本地仓库的目的是我们从中心库下载一些jar包等都存放在这里，项目需要的jar包都会先从这里找，这里找不到的话就会从中心库下载，中心库下载也会下载到我们的本地仓库![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003231458.png)
## 配置中央仓库
maven默认下载中央仓库的位置在国外，为了加快速度，我们在`settings.xml`中添加国内的阿里云镜像，即在`mirrors`标签中新增加一个镜像标签：
```
<mirror>
      <id>alimaven</id>
      <mirrorOf>central</mirrorOf>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
</mirror>
```
如下图：![](https://cdn.jsdelivr.net/gh/code-anan/image/20211004000112.png)
这样我们本地需要的就可以在阿里云下载 速度会快很多

# IDEA中集成maven
实际上idea中内置了maven项目，但是功能不够强大，所以可以选择集成我们自己下载安装的，来到idea中`File`->`settings`->`Build,Execution,Deployment`->`Build tools`->`Maven`，进行如下修改![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003233633.png)
然后我们创建一个maven项目看看是否会下载jar包到本地仓库,项目类型我们选择maven，创建一个web项目![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003234205.png)一直下一步可以看到一直在下载web项目需要的各类jar包
{% note info danger %}
注意：创建项目的时候选择maven的位置和目录也要跟我们在设置里面设置的一致
{% endnote %}
![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003234309.png)创建之后就可以看到我们的本地仓库已经有了很多文件了，这也证明了我们的结论![](https://cdn.jsdelivr.net/gh/code-anan/image/20211003234354.png)