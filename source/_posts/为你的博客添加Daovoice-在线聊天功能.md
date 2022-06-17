title: 为你的博客添加Daovoice--在线聊天功能
author: lwl
tags:
  - Daovoice
  - 在线聊天
categories:
  - Hexo
  - 修改记录
cover: https://cdn.jsdelivr.net/gh/code-anan/image/daovoice.jpg
date: 2021-05-30 21:32:49
my: post/daovoice
---
<meta name="referrer" content="no-referrer" />

# 前言
{% note info simple %}
[官方](https://butterfly.js.org/posts/ceeb73f/#%E5%9C%A8%E7%B6%AB%E8%81%8A%E5%A4%A9)给我们推荐了几款在线聊天工具，本文旨在记录自己添加`Daovoice`的经验分享和遇到的问题，如果你想要使用别的聊天工具， 建议查看别人的文章
{% endnote %}

# 添加步骤
## 注册Daovoice账号
首先需要到[DaoCloud](https://account.daocloud.io/signin)登录注册（很简单），登录之后需要进入到[控制台](http://dashboard.daovoice.io/get-started)
{% note danger simple %}
注意：从DaoCloud创建应用不可行，一定要从上面的控制台进入，创建你的应用，应用名称是后来点击小图标发起会话时会显示
{% endnote %}
![](https://cdn.jsdelivr.net/gh/code-anan/image/20220522130116.png)

## 控制台配置
进入到应用之后，点击左边的`应用设置`->`安装到网站`->`仅匿名用户`，下方会看到我们的`app_id`后面需要用到
![](https://cdn.jsdelivr.net/gh/code-anan/image/20220522125509.png)

## 聊天设置
找到`应用设置`->`聊天设置`，可以进行聊天小图标的设置，我的设置如下你也可以设置自己喜欢的颜色和位置
![](https://cdn.jsdelivr.net/gh/code-anan/image/20220522130738.png)

## 主题配置文件修改
找到`主题配置文件`中的`daovoice`进行如下修改
```yaml
# daovoice
# http://daovoice.io/
daovoice:
  enable: true
  app_id: xxxxx
```
`xxxxx`填上之前获取到的`appid`最后需要清理重启一下就可以看到效果了

## 绑定微信
如果你想要在微信也能够及时的接收消息（如果不想在微信接收可以忽略此步），在控制台右上角`微信绑定`->`微信绑定`，关注个公众号，然后找到`通话通知`勾选上`微信`就可以了,这样就可以在微信公众号就可以及时的收到信息了(￣▽￣)~*
![](https://cdn.jsdelivr.net/gh/code-anan/image/20220522125535.png)

# 常见问题
如果上述方法不行的话（这一步其实我没有用可以正常使用）还需要在`themes\butterfly\source\js`新建一个js文件，命名随意例如`daovoice.js`，内容填入
```js
(function (i, s, o, g, r, a, m) {
    i["DaoVoiceObject"] = r;
    i[r] = i[r] || function () {
        (i[r].q = i[r].q || []).push(arguments)
    }, i[r].l = 1 * new Date();
    a = s.createElement(o), m = s.getElementsByTagName(o)[0];
    a.async = 1;
    a.src = g;
    a.charset = "utf-8";
    m.parentNode.insertBefore(a, m)
})(window, document, "script", ('https:' == document.location.protocol ? 'https:' : 'http:') + "//widget.daovoice.io/widget/XXXXXXXX.js", "daovoice");
daovoice('init', {
    app_id: "XXXXXXXX"
});
daovoice('update');
```
xxxxxxx为你的`appid`，然后在`主题配置文件`->`inject`->`bottom`引入刚才的`daovoice.js`
```yaml
 bottom:
    - <script src="/js/daovoice.js"></script>
```
最后清理重新启动就可以看到效果了
```
hexo clean
hexo g
hexo s
```