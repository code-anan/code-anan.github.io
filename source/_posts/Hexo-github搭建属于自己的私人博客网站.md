title: Hexo+github搭建属于自己的私人博客网站
author: lwl
cover: 'https://fastly.jsdelivr.net/gh/code-anan/image/hexo.jpg'
tags:
  - Hexo
  - Github
categories:
  - Hexo
  - 记录
description: 梦开始的地方~
sticky: 10
my: post/hexo+github
date: 2021-05-25 20:20:17
---
<meta name="referrer" content="no-referrer" />

# 前言

能够看到这篇文章，说明你也已经对如何搭建个人博客有了一定的兴趣，阅读本文即可快速搭建起来。本文主要来源是B站[羊哥](https://space.bilibili.com/384068749?from=search&seid=6701903827907687402)的教学视频，但是羊哥使用的是Mac操作系统，我的是{% label win10  %}而且我在安装的时候也遇到了一些问题，所以写下这篇文章，希望能够帮助跟我遇到一样问题的朋友，好了废话不多说，下面步入正题。

# 准备工作

1. 如果要部署到github上，那么需要先注册一个[github账号](https://github.com/login)
2. 登录以后，创建一个新仓库：<img src="/img/repository.jpg" ><img src="/img/s2.jpg" >
  {% note danger simple %}
  注意：仓库名称必须为 {% label 用户名  green%}.github.io
  {% endnote %}
3. 由于Hexo都是基于Node的实现的，所以需要先下载:[Node](https://nodejs.org/en/),这里下载稳定版（LTS）即可，一直下一步就可以，同时他还会把后面需要的npm也自动安装好，下载好在Dos窗口（快捷键{% label win+R pink %}）测试:
``` 
 node -v
```
出现{% label vxx.xx.x  pink %}表示安装正常
4. 这里也可以选择安装淘宝的cnpm的管理器(据说是可以加快速度)：
```
npm install -g cnpm --registry=http://registry.npm.taobao.org
cnpm -v
```
5. 然后开始安装Hexo框架
```
cnpm install -g hexo-cli
hexo -v
```
# 正式开始

6. 安装成功之后可以开始创建hexo项目
```
hexo init blog
```
这里blog也可以改成你想要的名字，博客的配置文件等都会放在它的子目录下
7. 创建好blog文件夹之后 需要进入到blog目录下`cd blog`然后输入`hexo s`,在浏览器上输入{% label localhost:4000  %}就可以查看本地博客了<img src="/img/landscape.jpg" >
8. 下面开始部署到github上，这样别人也可以访问我们的博客了：
    + 首先配置blog目录下的{% label _config.yml  %}文件，打开找到下面的代码修改为：
    ```
      deploy:
       type: git
    repo: https://github.com/YourGithubName/YourGithubName.github.io.git
    branch: master
    ```
    {% note danger simple %}
     上面代码中的{% label YourGithubName green  %}改成你自己的{% label YourGithubName green  %}，不要全部复制
    {% endnote %}
    + 本地部署之后，下面就要正式开始部署到github上了，这里需要用到一个工具[git](https://git-scm.com/downloads),没下载的可以下载一下也很简单，下载安装之后会发现鼠标右键会多出来两项<img src="/img/git.jpg" >然后还需要安装一个非常重要的插件，在blog目录下输入`cnpm install --save hexo-deployer-git`,把项目部署到github上需要用到。
9. 最后三步就完成了部署
```
hexo clean
hexo g
hexo d
```
在浏览器上输入{% label YourGithubName.github.io green  %}就可以看到你的博客了![](https://fastly.jsdelivr.net/gh/code-anan/image/20210625155527.png)
10. 本文只介绍基本的安装和部署，另外关于文章的语法和其他的主题设置等会在其他文章中介绍，如果遇到了任何问题欢迎和我交流
# 常见问题

{% note danger simple %}
  经常遇到的问题是同步到github上时会出错，有一种原因是同步前没有`hexo clean`+`hexo g`，第二种是需要在`hexo d`之前需要再输入一遍github的邮箱和用户名
```
git config --global user.email xxx
git config --global user.name xxx
```
xxx分别填上你github的邮箱 github名称 再重试即可

{% endnote %}
