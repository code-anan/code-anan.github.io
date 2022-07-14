title: 关于PLSql Developer连接数据库报无法锁定OCI dll的错误解决方案
author: lwl
date: 2021-07-14 21:27:29
tags:
  - PLSQL Developer
categories:
 - 数据库
 - 工具
my: post/oci.dll
cover: https://gcore.jsdelivr.net/gh/code-anan/image/src=http---i0.hdslb.com-bfs-article-22e5fc1e71bcb4ba2c24560ef8ab15c1dc84f5bb.jpg&refer=http---i0.hdslb.jpg
---
# 前言
最近出差，有时候会用到数据库查一些数据。其实，之前我一直用的`Navicat Premium 12`,但是今天有同事远程我的电脑时，他习惯使用`PLsql`,但是打开我的plsql的时候发现连接不上数据库，并且报了一下错误，好在后来成功解决(*^▽^*)![](https://gcore.jsdelivr.net/gh/code-anan/image/20210714212511.png)

# 解决方案
我们连接不成功还是可以取消登录进入到主页面![](https://gcore.jsdelivr.net/gh/code-anan/image/20210714213145.png)
选中圈中的小图标进入到`首选项`：![](https://gcore.jsdelivr.net/gh/code-anan/image/20210715082741.png)
这两项需要分别填入oracle客户端实例以及对应的`oci.dll`文件![](https://gcore.jsdelivr.net/gh/code-anan/image/20210715082953.png)
然后点击`应用`->`确定`，如果该文件没有下载的话，可以到[上一篇](/post/ora28547)中查看下载地址，然后关闭plsql重新进入，可以发现登录页也发生了一些变化![](https://gcore.jsdelivr.net/gh/code-anan/image/20210715083408.png)这里需要注意的是数据库的填写格式为`主机ip`+`端口号`+`服务名`，然后发现可以正常连接了！
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210715083739.png)

# 最后
这篇绝对不是为了充数写的(￣▽￣)~*



