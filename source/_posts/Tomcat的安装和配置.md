title: Tomcat的安装和配置
author: lwl
date: 2021-10-04 21:02:40
tags:
  - tomcat
categories:
  - 服务器
cover: https://gcore.jsdelivr.net/gh/code-anan/image/u=1870340309,3725703990&fm=26&fmt=auto.webp
my: post/tomcatinstall
---
# Tomcat简单介绍
{% note info simple %}
简单的来说tomcat就是个轻量级的服务器，我们往javaweb方面学习时候经常会用到它，这里记录一下安装的步骤和简单的配置（注意安装tomcat之前jdk必须安装配置完成）
{% endnote %}

# 安装步骤
下载地址可以选择官方网站：->[传送门](https://tomcat.apache.org/download-80.cgi)![](https://gcore.jsdelivr.net/gh/code-anan/image/20211004211139.png)
左边为版本 core中为压缩包 选择下载解压即可
或者也可以选择从我的网盘进行下载安装包文件
链接：`https://pan.baidu.com/s/1Z4qtzsugPZUAmfC3qeEEGw` 
提取码：`8pht`![](https://gcore.jsdelivr.net/gh/code-anan/image/20211004211628.png)选择一个版本安装即可
安装过程只需要一直下一步就可以
{% note danger simple %}
需要注意的是从官网下载的压缩包直接解压得到的即为安装完成，我网盘中的为.exe格式的安装包，不要混淆
{% endnote %}
最后出现以下目录结构如下表示安装完成
![](https://gcore.jsdelivr.net/gh/code-anan/image/20211004212112.png)
# 环境变量配置
来到我们的高级系统设置->环境变量->新建变量，其中变量名为`CATALINA_HOME`，值为我们上面安装的tomcat所在的绝对路径，然后是JAVA_HOME也要写上jdk安装的位置，如下图![](https://gcore.jsdelivr.net/gh/code-anan/image/20211004215533.png)
最后还要在path中加上`%CATALINA_HOME%\bin`或者tomcat下bin的绝对路径，这两种方式都是一样的![](https://gcore.jsdelivr.net/gh/code-anan/image/20211004215735.png)
到这里基本上就配置完成了 如果不配置的话会发现我们本地想要启动tomcat会闪退无法启动
# 测试
以上步骤都操作好了之后，可以在本地测试一下，来到tomcat安装的目录下的bin下，双击startup.bat![](https://gcore.jsdelivr.net/gh/code-anan/image/20211004215829.png)如果这里出现闪退 一定是`环境变量配置`那里没有配置好 然后在本地看一下tomcat,在浏览器上输入`http://localhost:8080`,出现下图表示安装成功！![](https://gcore.jsdelivr.net/gh/code-anan/image/20211004220019.png)最后不要忘记双击bin目录下的shutdown.bat来关闭tomcat服务，不然会一直占用资源(￣▽￣)~*