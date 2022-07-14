title: 为Hexo博客添加Twikoo评论
author: lwl
tags:
  - Hexo
  - Twikoo
categories:
  - Hexo
  - 修改记录
my: post/Twikoo
cover: >-
  https://gcore.jsdelivr.net/gh/code-anan/image/src=http---ask.qcloudimg.com-draft-6236398-rtwl4nn2uk.jpg&refer=http---ask.qcloudimg.jpg
date: 2021-06-19 12:38:32
---
# 前言
之前我一直使用的是valine评论系统，虽然它确实简洁，但是缺点是不能显示用户的头像，这一点我感觉很不好，所以后来想着换成`Twikoo`评论

# CloudBase配置
首先进入到[云开发CloudBase](https://cloud.tencent.com/act/free)，找到咱们的免费试用版，如下![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619124239.png)
然后就是`创建环境`，选择`空模板`
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619125333.png)
环境信息填写如下
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619125743.png)
地域最好选择`上海`，如果选择`广州`的话，需要在`twikoo.init()`时额外指定环境`region: "ap-guangzhou"`
环境名称无所谓，符合要求即可
套餐版本选择免费版就够用的了，然后下一步，`立即购买`
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619130115.png)
购买完成之后即可进入`控制台`![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619130202.png)
来到`环境`-`登录授权`，开启`匿名登录`
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619130414.png)
然后来到`安全配置`中，将自己的网站域名添加到`web安全域名`
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619130543.png)
下一步是`新建云函数`
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619130657.png)
并且填写以下内容
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619130801.png)
把`函数代码`,替换为`exports.main = require('twikoo-func').main`
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619130930.png)
确定之后，点击刚才创建的云函数`twikoo`中，找到`函数代码`->`文件`->`新建文件`![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619131048.png)
命名为`package.json`，按下回车，并且复制以下代码`{ "dependencies": { "twikoo-func": "1.3.0" } }`
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619131319.png)
配置完之后可以看到其状态变成`正常`
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619131413.png)
最后复制需要的`环境id`
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619131924.png)
# 主题配置文件配置
来到`主题配置文件`，找到以下内容
```yaml
twikoo:
  envId: xxxxxxxx
  region:
  visitor: false
  option:
```
envid为上面复制的`环境id`
 
然后还需要开启评论
```yaml
comments:
  # Up to two comments system, the first will be shown as default
  # Choose: Disqus/Disqusjs/Livere/Gitalk/Valine/Waline/Utterances/Facebook Comments/Twikoo
  use:
    - Twikoo
  # - Disqus

  text: false # Display the comment name next to the button
  # lazyload: The comment system will be load when comment element enters the browser's viewport.
  # If you set it to true, the comment count will be invalid
  lazyload: false
  count: true # Display comment count in post's top_img
  card_post_count: true # Display comment count in Home Page
```
保存，最后hexo三连查看效果
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619134056.png)
如图，输入昵称和邮箱即可获取到用户的头像，这也是我为什么选择Twikoo的原因
# 开启twikoo评论管理面板
+ 找到环境->登录授权，点击`私钥下载`并复制其私钥内容
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619134223.png)
+ 来到评论窗口，点击那个小齿轮，粘贴私钥内容并设置密码注册
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619134546.png)

![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619134606.png)
+ 在配置管理中进行必要的设置，如设置提醒的邮箱，默认的头像等
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210619134843.png)

设置完上述步骤就大功告成啦 (￣▽￣)~*