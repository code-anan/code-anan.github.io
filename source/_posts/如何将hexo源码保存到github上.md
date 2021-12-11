title: 如何将hexo源码保存到github上
author: lwl
tags:
  - 文章路径
categories:
  - Git
  - 上传
my: post/savetoGithub
cover: >-
  https://cdn.jsdelivr.net/gh/code-anan/image/src=http---hbimg.b0.upaiyun.com-46ba6037c7a82a8f245cb3b77ae0bd487e9199da1bdf2-8ho4nc_fw658&refer=http---hbimg.b0.upaiyun.jpg
date: 2021-06-17 15:49:46
---

# 前言
众所周知，我们使用`hexo d`将项目部署到github上的时候，代码其实已经被重新渲染过了，都变成了`html`文件，如下所示，可以看到目录结构和我们在本地的项目已经完全不同，而且里面并没有我们所写的md文件![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617155143.png)
再加上我曾经重装过一次系统，所以上次的网站已经找不回来了，现在看到的这个站其实是我的第二个网站了，但是吃一堑长一智，学会备份源码是一件很重要的事情，下面来看看怎么在github上备份本地代码吧！

# 前提
阅读本文之前我默认为你已经有了github账号，git安装过了，如果没有{% btn '/post/hexo+github/',可以在这里找到它们的安装步骤,far fa-hand-point-right,outline %}

# 开始操作
1. 创建一个仓库用来存放源码，这个没什么好说的，创建`public`不要勾选添加md文件，并且复制仓库的地址
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617160804.png)
2. `cmd`进入到你的根目录下
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617160723.png)
3. 这里我们全程使用的是git的常见指令
  + 首先输入`git init`(建仓)
  + 然后是`git add *`(添加代码到本地仓库)
  出现`warning`是正常的
  ![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617161144.png)
  + 然后输入`git commit -m "first commit"`(提交到本地缓存，引号是提交信息)![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617161352.png)
  + （首次使用会提示：please tell me who you are）
  ![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617161501.png)
  如果看到上面的提示，就在cmd里面继续敲这两行：（如果没有的话可以直接忽略）

>git config --global user.name "xxx@xxx.com(你的github邮箱)"   

>git config --global user.email "你的github用户名"（敲完之后，继续上面的commit这一步）
 
 + 第四步是`git remote add origin https://github.com/code-anan/hexo-codeResource.git`,注意替换成你的地址，第一步的时候应该复制下来了（这一步是提交到远程github上）
 + 最后一步是`git push -u origin master `(push到master分支)
 这一步由于github的连接情况很容易爆出各种错误，可以把它改成`git push -u origin master -f`强制上传，不过这样还是有可能上传失败，可以多试几次ヽ(￣▽￣)ﾉ
 # 大功告成
 ![](https://cdn.jsdelivr.net/gh/code-anan/image/20210617162145.png)
 不用担心不小心丢失源码再也写不了博客了ヽ(￣▽￣)ﾉ
 # 补充
   +   git add -A  提交所有变化

   +  git add -u  提交被修改(modified)和被删除(deleted)文件，不包括新文件(new)

   + git add .  提交新文件(new)和被修改(modified)文件，不包括被删除(deleted)文件