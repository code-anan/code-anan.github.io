title: 添加valine评论工具
author: lwl
tags:
  - valine
  - LeanCloud
cover: 'https://fastly.jsdelivr.net/gh/code-anan/image/leancloud.jpg'
my: post/valine
categories:
  - Hexo
  - 修改记录
date: 2021-05-29 15:28:15
---
<meta name="referrer" content="no-referrer" />

# 前言
> [官方](https://butterfly.js.org/)文档推荐了好多种评论工具，其实大部分我都尝试过，但是有的是因为某些原因现在用不了了，还有的是不好配置导致了我花了好多时间也没搞好最后就放弃了，最后觉得valine还不错，而且还可以绑定qq邮箱及时通知，效果在下方就可以看到

# 操作步骤
## 注册LeanCloud
建议注册[LeanCloud国际版](https://console.leancloud.app/register),以后添加说说功能用国际版的会比较方便，注册完之后创建个应用选择开发版，名字无所谓
<img src="/img/posts/leancloud.png">
## 找到AppID和AppKey
进入应用->设置->应用Keys，可以看到`ID`和`Key`,这个是后面需要用到的
<img src="/img/posts/idandkey.png">
## 修改主题配置文件
1. `comments`下的`use`，一定要把前面的#注释打开，不然修改不成功
```yaml
comments:
  # Up to two comments system, the first will be shown as default
  # Choose: Disqus/Disqusjs/Livere/Gitalk/Valine/Waline/Utterances/Facebook Comments/Twikoo
  use:
    - Valine
```
2. 继续找到下面的`valine`配置：
```yaml
# valine
# https://valine.js.org
valine:
  appId: xxxxxxxxxxxxxxxxxxxxxxxxxxxx
  appKey: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
  pageSize: 20 # comment list page size
  avatar: wavatar # gravatar style https://valine.js.org/#/avatar
  lang: en # i18n: zh-CN/zh-TW/en/ja
  placeholder: ヾﾉ≧∀≦)o 来呀！快活呀！~ # valine comment input placeholder (like: Please leave your footprints)
  guest_info: nick,mail,link # valine comment header info (nick/mail/link)
  recordIP: false # Record reviewer IP
  serverURLs: # This configuration is suitable for domestic custom domain name users, overseas version will be automatically detected (no need to manually fill in)
  bg: # valine background
  emojiCDN: # emoji CDN
  enableQQ: false # enable the Nickname box to automatically get QQ Nickname and QQ Avatar
  requiredFields: nick,mail # required fields (nick/mail)
  visitor: false
  option:
```
`appId`和`appKey`依次改为你在`LeadCloud`中创建应用的`AppID`和`AppKey`
3. 重新清理启动一下查看效果
```
hexo clean
hexo g
hexo s
```
效果图：
<img src="/img/posts/comment.png">
{% note info simple %}
如果你不想随时接收到评论信息，到这里就完成了，在[应用](https://console.leancloud.app/apps/rW4vvYAhgn12o4M7UaOgUQAX-MdYXbMMI/)中的`结构化数据`就可以查看评论
{% endnote %}

# 添加QQ邮箱提醒
通过上面的步骤评论已经可以使用，但是为了能及时收到别人的评论，所以最好绑定自己的邮箱，这里我绑定的是`QQ`邮箱
+ 进入到咱们的`应用`->`云引擎`->`WEB`->`设置`->`添加新变量`，添加以下变量
![](https://fastly.jsdelivr.net/gh/code-anan/image/20210618163012.png)
注意设置QQ邮箱的话需要`SMTP_PASS`，它的获取需要到QQ邮箱中的`设置`->`账户`中，找到`SMTP`进行开启，这里需要发个短信![](https://fastly.jsdelivr.net/gh/code-anan/image/20210618163240.png)

获取并填写完之后，就可以来到部署页了![](https://fastly.jsdelivr.net/gh/code-anan/image/20210618163355.png)

这里我们选择git部署，仓库选择跟我一样的就可以，然后点击部署
![](https://fastly.jsdelivr.net/gh/code-anan/image/20210618163512.png)
> `https://github.com/lete114/Valine-Admin-Server.git`

最后在`安全中心`->`设置`中添加上自己的域名就可以正常接收邮件提醒了
![](https://fastly.jsdelivr.net/gh/code-anan/image/20210618164039.png)
大功告成(￣▽￣)~*