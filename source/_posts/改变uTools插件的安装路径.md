title: 改变uTools插件的安装路径
author: lwl
date: 2021-11-20 16:39:16
tags:
  - uTools
categories:
  - uTools
cover: https://gcore.jsdelivr.net/gh/code-anan/image/src=http---app.tentech.club-wp-content-uploads-2020-02-utools.png&refer=http---app.tentech.jpg
my: post/utools
---
# 前言
我们知道uTools是一款好用至极的软件，可以自由选择各种插件、提高工作效率
# 下载
下载地址百度搜索`utools`或者点击我的[链接](http://www.u.tools/)即可进行下载、唯一不好的地方我们双击下载的exe文件，他会自动帮我们下载完毕、这里非常不友好、但是本身只有100多M，所以我们也就忍了（安装完成之后会出现提示页面，按住`alt`+`space`即可呼出）![](https://gcore.jsdelivr.net/gh/code-anan/image/20211120164504.png)

# 改变插件安装路径
软件安装位置我们改变不了、安装的插件位置一定不能还在C盘了、这样体积太大、直接开始介绍方法：
1. `win`+`R`,输入`%APPDATA%`,找到我们的`uTools`文件夹，然后进行剪切![](https://gcore.jsdelivr.net/gh/code-anan/image/20211120164833.png)并记下他的路径`C:\Users\admin\AppData\Roaming\uTools`(这个admin是你电脑自己起的名字记得更换~)
2. 剪切到我们想要放的位置、这里我选择D盘存放，粘贴到D盘![](https://gcore.jsdelivr.net/gh/code-anan/image/20211120165020.png)
同样记下文件路径，我这里为`D:\uTools`

3. 最后进入cmd输入以下指令`mklink /d "C:\Users\admin\AppData\Roaming/uTools" "D:\uTools"`更换为你的实际路径
![](https://gcore.jsdelivr.net/gh/code-anan/image/20211120165345.png)
出现以下符号即为创建完成！
# 好用的插件介绍
 * maven&gradle
![](https://gcore.jsdelivr.net/gh/code-anan/image/20211120165554.png)
使用该插件之后，输入想要的依赖自动copy我们需要的依赖地址，例如gson，选择某一个版本点击之后赋值到我们的项目中![](https://gcore.jsdelivr.net/gh/code-anan/image/20211120165914.png)
![](https://gcore.jsdelivr.net/gh/code-anan/image/20211120165949.png)
这样就不用我们到网站上搜索了
 * 内网穿透（现在已经下架，之前非常好用现在只能用花生壳了）
 * 本地搜索 （轻易检索到本地文件只需要一点点信息）
![](https://gcore.jsdelivr.net/gh/code-anan/image/20211120170134.png)
当然还有其他很多优秀的插件、这里就不介绍了值得你慢慢探索！