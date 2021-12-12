title: Hexo博客中接入docsify文档管理系统
author: lwl
cover: >-
  https://cdn.jsdelivr.net/gh/code-anan/image/src=http---cdn.dealmango.com-wp-content-uploads-2018-05-Docsify-appsumo-lifetime-deal.jpeg&refer=http---cdn.dealmango.jpg
tags:
  - Docsify
categories:
  - 记录
my: post/docsify
date: 2021-12-12 16:50:26
---

# 前言

从鱼皮那里看到了一个很好看的文档管理系统，后来知道叫做[docsify](https://docsify.js.org/#/)看起来非常不错，这里记录一下接入hexo的过程 

# 准备工作

## 安装docsify(前提是安装了[Node.js](https://nodejs.org/zh-cn/download/))
   > npm i docsify-cli -g

## 新建仓库
在github上新建一个仓库命名格式为`doc.域名`
   > 例如我的域名为 weilong98.com 所以我的仓库名称为doc.weilong98.com

![](https://cdn.jsdelivr.net/gh/code-anan/image/20211211195623.png)

## 添加文件
给新建的仓库添加一个文件 内容为从刚才的仓库名![](https://cdn.jsdelivr.net/gh/code-anan/image/20211211200200.png)
## 初始化文档(cmd窗口中)
   > docsify init docs
   > 
   > Initialization succeeded! Please run docsify serve docs

出现succeeded即为初始化成功，初始化成功后会生成三个文件

![](https://cdn.jsdelivr.net/gh/code-anan/image/20211211201310.png)

本地预览

> docsify serve docs

与hexo类似 本地启动之后访问`http://localhost:3000`![](https://cdn.jsdelivr.net/gh/code-anan/image/20211211201503.png)

启动成功 但是现在还没有内容

# 搭建博客

- 设置封面

打开刚才新生成文件的`index.html`，这里我们使用`sublime Text`打开 或者别的都可以![](https://cdn.jsdelivr.net/gh/code-anan/image/20211211201846.png)在图中所示位置添加`coverpage: true`

新建一个`_coverpage.md`文件在`docs`文件夹下

![](https://cdn.jsdelivr.net/gh/code-anan/image/20211211202309.png)

然后在这里写我们封面的内容，下面为示例

```markdown
<img width="180px" style="border-radius: 50%" bor src="https://nodejsred.oss-cn-shanghai.aliyuncs.com/nodejs_roadmap-logo.jpeg?x-oss-process=style/may">

# 我的记录文档

- 这是我的学习记录文档

[![stars](https://badgen.net/github/stars/Q-Angelo/Nodejs-Roadmap?icon=github&color=4ab8a1)](https://github.com/Q-Angelo/Nodejs-Roadmap) [![forks](https://badgen.net/github/forks/Q-Angelo/Nodejs-Roadmap?icon=github&color=4ab8a1)](https://github.com/Q-Angelo/Nodejs-Roadmap)

[主站](http://www.weilong98.com)
[开始阅读](README.md)

```

效果![](https://cdn.jsdelivr.net/gh/code-anan/image/20211212152705.png)

- 定制导航栏

官方有两种支持导航的方式，一种是在html里面设置但是以#/开头，但是我们还是更喜欢Markdown的方式,首先配置`loadNavbar: true`,同样也要创建`_navbar.md`文件与上面类似

```markdown
* 首页
    * [首页](/README.md)

* 新的内容
    * [新内容](/nav/newpage.md)
```

<br/>

其他更多设置可以查看官网[docsify](https://docsify.js.org/#/zh-cn/)

# 把文档接入Github pages

## 拷贝项目
首先  拷贝我们新建的仓库到本地
> git clone https://github.com/code-anan/doc.weilong98.com.git
git clone后面为我们创建的仓库地址

## 拷贝文件
然后把`docs`也就是刚才初始化的docsify项目中的所有文件copy到本地仓库`doc.weilong98.com`中

![](https://cdn.jsdelivr.net/gh/code-anan/image/20211212154220.png)

并把刚才的`docs`文件夹删除

## 上传仓库
把项目上传到仓库（输入一些常用的命令即可）
   
   ![](https://cdn.jsdelivr.net/gh/code-anan/image/20211212155022.png)

## 解析配置
然后还需要来到我们的域名控制台-解析设置中添加以下记录（记录值为我们hexo的域名）
   
   ![](https://cdn.jsdelivr.net/gh/code-anan/image/20211212161259.png)
## 分支保存
来到仓库-settigns-pages选择我们的分支默认为main分支保存即可![](https://cdn.jsdelivr.net/gh/code-anan/image/20211212160759.png)然后我们的Custom domain也自动输入上去了
## 仓库名访问
然后可以直接访问我们的docsif文档了
   
   ![](https://cdn.jsdelivr.net/gh/code-anan/image/20211212161442.png)

# hexo博客中引入doscify

 通过上面的步骤我们只需要在hexo中添加个导航栏 加上docsify的地址即可 打开`_config.butterfly.yml`文档找到导航区域

![](https://cdn.jsdelivr.net/gh/code-anan/image/20211212164050.png)

<br/>

这样就能完美接入docsify了  剩下的就是慢慢完善docsify的页面布局等ヽ(￣▽￣)ﾉ
