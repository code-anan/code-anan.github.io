title: 使用PicGo+Github作为你的免费图床
author: lwl
tags:
  - PicGo
  - Github
cover: 'https://fastly.jsdelivr.net/gh/code-anan/image/picgocover.jpg'
categories:
  - Hexo
  - 图床
description: 必备设置，不要忘记！
my: post/PicGo
date: 2021-06-16 08:47:54
---
# 前言
{% note info simple %}
其实我之前也没有使用图床的习惯，一直都是放在source/img目录下，这样的弊端就是图片加载速度慢，体验不好。后来看到花猪的文章感受到了图床的魅力，现在已经真香了ヽ(￣▽￣)ﾉ
{% endnote %}
# 安装过程
## 下载PicGo
下载地址：->[传送](https://github.com/Molunerfinn/PicGo/releases)选择你喜欢的版本下载，建议不要太高版本，这里我使用的是[2.2.2版本](https://github.com/Molunerfinn/PicGo/releases/download/v2.2.2/PicGo-Setup-2.2.2.exe)，安装步骤没什么好说的就是位置别选c盘就行，安装后的样子如下
![](https://fastly.jsdelivr.net/gh/code-anan/image/20210616142836.png)
## github配置
+ 来到我们的[github](https://github.com/)创建仓库，注意不要添加md文件，默认分支会改变![](https://fastly.jsdelivr.net/gh/code-anan/image/20210616144142.png)
+ 然后是获取`token`,右上角点击头像->`Settings`->`Developer settings`->`Personal access tokens`->`Generate new token`，然后`note`中填写我们刚才创建的仓库名称`images`![](https://fastly.jsdelivr.net/gh/code-anan/image/20210616145604.png)，并且勾选上`repo`，拉到最下面点击`Generate token`，这样就获取到我们的token了，点击复制![](https://fastly.jsdelivr.net/gh/code-anan/image/20210616150107.png)
## 图床设置
上面仓库 token获取到之后就可以回到我们的PicGo中，点击`图床设置`->`Github图床`，这下知道我们上面的步骤的作用了
![](https://fastly.jsdelivr.net/gh/code-anan/image/20210616150307.png)

+ 设定仓库名：GitHub用户名/仓库名(例如code-anan/images)
+ 设定分支名：默认的master（前面不要添加md文件的情况下）
+ 设定Token：上一步咱们获取到的复制上去
+ 指定存储路径：建议不写，写的话也可以
+ 设定自定义域名：建议写成`https://fastly.jsdelivr.net/gh/GitHub账号名/仓库名`(例如`https://fastly.jsdelivr.net/gh/code-anan/images`),当然不写的话也可以，写成这种格式可以jsDelivr加速
然后点击一下`设为默认图床`，`确定`就完成了所有的配置，下面找到拖动区放个图片试试吧，另外还有`PicGo设置`中可以开启上传前重命名，这样取个合适的名字容易分辨

# 遇到的问题
我遇到的问题是上传之后发现相册区域图片不能预览，不过后来百度知道了解决方案，找到咱们的`hosts`文件，路径为`C:\Windows\System32\drivers\etc`,在最下方输入以下内容：
```
# GitHub Start 
192.30.253.112 github.com 
192.30.253.119 gist.github.com
151.101.184.133 assets-cdn.github.com
151.101.184.133 raw.githubusercontent.com
151.101.184.133 gist.githubusercontent.com
151.101.184.133 cloud.githubusercontent.com
151.101.184.133 camo.githubusercontent.com
151.101.184.133 avatars0.githubusercontent.com
151.101.184.133 avatars1.githubusercontent.com
151.101.184.133 avatars2.githubusercontent.com
151.101.184.133 avatars3.githubusercontent.com
151.101.184.133 avatars4.githubusercontent.com
151.101.184.133 avatars5.githubusercontent.com
151.101.184.133 avatars6.githubusercontent.com
151.101.184.133 avatars7.githubusercontent.com
151.101.184.133 avatars8.githubusercontent.com
```
修改之后就可以正常预览图片了，效果如下：
![](https://fastly.jsdelivr.net/gh/code-anan/image/yulan.png)
如果你按照上述步骤还是不能预览，那么就是上面的github.com的ip需要修改，修改步骤参考>{% btn '/post/hexo error/',修改github.com的ip,far fa-hand-point-right,outline %}
使用PicGo图床还有一个好处是我们使用它里面的图片时，直接点击红圈下方最左边的小图标就可以直接帮我们复制成`Markdown`的语法格式，然后粘贴就可以在文章里面使用图片了，到这里就大功告成了~



# 特别注意

当以上步骤配置完之后很可能会发现图片上传不上去，这时候建议关闭picGo然后等一会再进行上传