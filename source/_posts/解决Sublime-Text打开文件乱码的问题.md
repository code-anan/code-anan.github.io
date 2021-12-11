title: 解决Sublime Text打开文件乱码的问题
author: lwl
date: 2021-10-30 18:04:34
tags:
  - Sublime Text
categories:
 - 工具
my: post/sublimeText3Error
cover: https://cdn.jsdelivr.net/gh/code-anan/image/src=http---hbimg.b0.upaiyun.com-06de8f75371e7a55721fbc84976a12f2a34b7f805c86-kRWYUW_fw658&refer=http---hbimg.b0.upaiyun.jpg
---
# 前言
我们知道`Sunlime Text`是一款非常好用的文本编辑器、我们打开各种java文件、xml文件、properties等类型的文件时非常的好用、但是他也有自己的小bug、例如我打开一个springboot的application.properties文件时，当文件含有中文他就会乱码、如下图![](https://cdn.jsdelivr.net/gh/code-anan/image/20211030180745.png)

# 解决方案
1. 首先需要给sublime安装`Package Control`
 打开text3、然后`view`->`show console`或者快捷键`ctrl`+`~`![](https://cdn.jsdelivr.net/gh/code-anan/image/20211030181650.png)
 输入以下指令
 ```
 import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())

 ```
2. 安装好之后可以看到`preferences`中有了package Control,然后输入`install package`![](https://cdn.jsdelivr.net/gh/code-anan/image/20211030182007.png)
3. 等待几秒 在插件安装页输入`ConvertToUTF8`和`GBK Support`
![](https://cdn.jsdelivr.net/gh/code-anan/image/20211030182147.png)
![](https://cdn.jsdelivr.net/gh/code-anan/image/20211030182455.png)
4. 等待插件安装好之后重启 打开文件
 ![](https://cdn.jsdelivr.net/gh/code-anan/image/20211030183014.png)
{% note danger simple %}
注意：这里你的idea项目编码方式一定要是`utf-8`，否则还会发现乱码，如下图
{% endnote %}
![](https://cdn.jsdelivr.net/gh/code-anan/image/20211030183523.png)