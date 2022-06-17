title: 关于我的butterfly主题配置文件的一些修改
author: lwl
tags:
  - Hexo
  - Butterfly
categories:
  - Hexo
  - 修改记录
cover: 'https://cdn.jsdelivr.net/gh/code-anan/image/butterfly.jpg'
my: post/butterfly
date: 2021-05-28 15:39:32
---
<meta name="referrer" content="no-referrer" />

{% blockquote  %}
本文适用于没有Hexo基础的童鞋，这里我会介绍我主题配置文件所有的修改
{% endblockquote %}
# 准备工作
首先是下载butterfly主题，在博客根目录下
```powershell
git clone -b master https://github.com/jerryc127/hexo-theme-butterfly.git themes/butterfly
```
如果出现克隆失败，把上面的`https`改成`git`即可，克隆完成之后根目录下的/themes中就会出现butterfly的主题文件夹，然后要把博客根目录下的`_config.yml`中的`theme`属性改成butterfly,`注意要有一个空格缩进`，改完之后：
```
hexo clean
hexo g
hexo s
```
在本地打开是否主题，这里很容易出现一大串数字没有界面这种情况需要安装pug以及 stylus 的渲染器，回到博客根目录下输入；
```powershell
npm install hexo-renderer-pug hexo-renderer-stylus --save
```
安装完成之后再次重复上面的那三个操作，然后出现下面这个图就表示主题配置成功了<img src="https://cdn.jsdelivr.net/gh/jerryc127/butterfly_cdn@2.1.0/top_img/index.jpg">
{% note info simple %}
这里强烈建议把主题下面的配置文件放到根目录下面，即把/themes/butterfly下的`_config.yml`文件改名为`_config.butterfly.yml`.这样主要是为了防止升级以后主题配置文件会被更换，当然如果不升级的话可以忽略,并且当他和博客的主题配置文件都存在时，主题配置文件优先级会更高
{% endnote %}
# 修改记录
当我们的主题更换好之后，除了写文章之外我们还会关心网站的样式，因为现在连导航栏都还没有，因此需要先了解hexo博客的一些必要概念：首先是首页菜单栏的添加，找到`主题配置文件`中最上方的`menu`属性，#在markdown文件中属于注释，把前面#删除进行修改：
```yaml
Home: / || fas fa-home
Archives: /archives/ || fas fa-archive
Tags: /tags/ || fas fa-tags
Categories: /categories/ || fas fa-folder-open
List||fas fa-list:
  Music: /music/ || fas fa-music
  Movie: /movies/ || fas fa-video
Link: /link/ || fas fa-link
About: /about/ || fas fa-heart
```
其中`/xxx/`表示该页面的路径，后面的为[Font Awesome](https://fontawesome.com/)的小图标，可以自己DIY，分享我修改后的文件为：
```
menu:
   首页: / || fas fa-home
   文章||fas fa-book:
    时间轴: /archives/ || fas fa-archive
    标签: /tags/ || fas fa-tags
    分类: /categories/ || fas fa-folder-open
   生活||fas fa-frog:
    音乐: /music/ || fas fa-music
    自言自语: /shuoshuo/ || fas fa-bug
    留言板: /message/ || fas fa-sms
   友链: /link/ || fas fa-link
   关于我: /about/ || fas fa-heart
```
{% note info simple %}
修改完之后记得要创建对应的界面：根目录下`hexo new page xxx`,`xxx`为你这里要添加的页面名称，修改menu最好复制，自己手写容易出错`.md`文件缩进很重要,
{% endnote %}

## 代码样式
依然是`主题配置文件`
```yaml
highlight_theme: mac
```
### 代码复制
依然是`主题配置文件`
```yaml
highlight_copy: true
```
### 代码块默认展开/关闭
依然是`主题配置文件`
```yaml
highlight_shrink: false
```
### 代码换行
依然是`主题配置文件`
```yaml
code_word_wrap: true
```
### 代码高度限制
依然是`主题配置文件`
```yaml
highlight_height_limit: 200
```
## 社交图标
这里我没有选择使用[Font Awesome](https://fontawesome.com/)的小图标，如果想要使用它里面的小图标，直接在`主题配置文件`找到social进行修改即可，但是它里面的小图标数量有限，所以我选择了阿里巴巴矢量库来引用小图标，效果展示：
![](https://cdn.jsdelivr.net/gh/code-anan/image/20220522130558.png)
修改方法为:->{% btn '/post/iconfont/',引用阿里巴巴矢量库iconfont修改社交图标,far fa-hand-point-right,outline %}
## 顶部图
我的顶部图设置情况，依然是`主题配置文件`：
```yaml
# The banner image of home page
index_img: /img/background.jpg

# If the banner of page not setting, it will show the top_img
default_top_img: /img/default.jpg

# The banner image of archive page
archive_img: /img/about.jpg

# If the banner of tag page not setting, it will show the top_img
# note: tag page, not tags page (子標籤頁面的 top_img)
tag_img:  'linear-gradient(20deg, #0062be, #925696, #cc426e, #fb0347)'

# The banner image of tag page
# format:
#  - tag name: xxxxx
tag_per_img:  'linear-gradient(20deg, #0062be, #925696, #cc426e, #fb0347)'

# If the banner of category page not setting, it will show the top_img
# note: category page, not categories page (子分類頁面的 top_img)
category_img:  'linear-gradient(20deg, #0062be, #925696, #cc426e, #fb0347)'

# The banner image of category page
# format:
#  - category name: xxxxx
category_per_img:  'linear-gradient(20deg, #0062be, #925696, #cc426e, #fb0347)'
```
> emm这里就不展示具体的图了 总之修改顶部图可以在这里修改 图片路径可以为本地路径也可以是网络路径或者是这种渐变色的写法
## Footer
可以看到我的页脚是可以自由飞舞的小鱼，这个设置主要是参考了[花猪](https://cnhuazhu.gitee.io/2021/02/19/Hexo%E9%AD%94%E6%94%B9/Hexo%E9%A1%B5%E8%84%9A%E5%85%BB%E9%B1%BC%E6%95%88%E6%9E%9C/)老哥的博客，但是他有一些小细节没有处理好，所以我又单独写了一份，应该是比较完善的。
指路:->{% btn '/post/footer/',页脚设置游动的小鱼,far fa-hand-point-right,outline %}
## 评论
评论区域的添加耽误了我不少的时间，一开始设置的时候出现了各种问题，后来多方百度搜索最终决定使用`valine`,效果如图：
![](https://cdn.jsdelivr.net/gh/code-anan/image/20220522125925.png)
修改方法：->{% btn '/post/valine/',添加valine评论,far fa-hand-point-right,outline %}
## 在线聊天
我使用的是`daovoice`工具，也踩了不少坑，效果如图：
![](https://cdn.jsdelivr.net/gh/code-anan/image/20220522130150.png)
修改方法：->{% btn '/post/daovoice/',添加在线聊天工具Daovoice,far fa-hand-point-right,outline %}
## 本地搜索
1. 根目录下输入`npm install hexo-generator-search`下载需要的插件
2. 修改主题配置文件：
```yaml
local_search:
  enable: true
```
这样就可以看到菜单栏多了一个搜索可以进行本地搜索：![](https://cdn.jsdelivr.net/gh/code-anan/image/20220522131033.png)
## 鼠标点击效果
直接修改`主题配置文件`即可
```yaml
fireworks:
  enable: true
  zIndex: 9999 # -1 or 9999
  mobile: false
```
效果：![](https://cdn.jsdelivr.net/gh/code-anan/image/20220522130420.gif)
## 标题前的小图标
修改`主题配置文件`
```yaml
beautify:
  enable: true
  field: site # site/post
  title-prefix-icon: '\f717'
  title-prefix-icon-color: "#F47466"
```
`field`表示在全站（site）还是文章页（post）生效
`title-prefix-icon`为你想要修改的图标样式，可以在[Font Awesome](https://fontawesome.com/)中找到喜欢的小图标,例如![](https://cdn.jsdelivr.net/gh/code-anan/image/20220522125154.png)进行修改即可
`title-prefix-icon-color`为小图标的样式
效果：![](https://cdn.jsdelivr.net/gh/code-anan/image/20220522131234.png)
## 网站副标题
修改`主题配置文件`
```yaml
subtitle:
  enable: true
  # Typewriter Effect (打字效果)
  effect: true
  # loop (循環打字)
  loop: true
  # source調用第三方服務
  # source: false 關閉調用
  # source: 1  調用搏天api的隨機語錄（簡體）
  # source: 2  調用一言網的一句話（簡體）
  # source: 3  調用一句網（簡體）
  # source: 4  調用今日詩詞（簡體）
  # subtitle 會先顯示 source , 再顯示 sub 的內容
  source: 1
  # 如果有英文逗號' , ',請使用轉義字元 &#44;
  # 如果有英文雙引號' " ',請使用轉義字元 &quot;
  # 開頭不允許轉義字元，如需要，請把整個句子用雙引號包住
  # 如果關閉打字效果，subtitle只會顯示sub的第一行文字
  sub:
   - 脑子是个好东西，希望你也有一个，但如果你胸大没有也行。
   - 早睡早起身体好，不是一句口号，而是三个愿望
   - 如果连我的情绪，都要我亲口告诉你，那和考试抄答案有什么区别？
   - 我尝试着做一个有趣的人，后来却跑偏了，成了一个逗逼。
   - 感情和头发一样，时间长了，都会分叉。
```
{% note info simple %}
会先显示`source`里的内容，然后再展示`sub`中自己编辑的内容
{% endnote %}
效果：![](https://cdn.jsdelivr.net/gh/code-anan/image/20220522131259.png)
## snackbar弹窗
修改`主题配置文件`
```yaml
snackbar:
  enable: true
  position: bottom-left
  bg_light: '#49b1f5' #light mode時彈窗背景
  bg_dark: '#2d3035' #dark mode時彈窗背景
```
{% note info simple %}
其实只是在边上显示了 开不开都无所谓
{% endnote %}
## Pjax
据说是可以复加载相同的资源（css/js），  从而提升网页的加载速度。我修改的页面为：
```yaml
pjax:
  enable: true
  exclude:
    - /shuoshuo/
```
## 网站图标
`主题配置文件`的修改：
```yaml
# Favicon（網站圖標）
favicon: https://cdn.jsdelivr.net/gh/code-anan/image/蜘蛛网万圣节.png
```
## 友链魔改
这里我参考的花猪的文章:->[传送门](https://cnhuazhu.gitee.io/2021/02/25/Hexo%E9%AD%94%E6%94%B9/Hexo%E4%BF%AE%E6%94%B9%E5%8F%8B%E9%93%BE%E7%95%8C%E9%9D%A2/)
## 添加说说
这里我参考了`小嘉的部落格`->[传送门](https://blog.imzjw.cn/posts/b74f504f/)
## 留言板
同样参考了`小嘉的部落格`->[传送门](https://blog.imzjw.cn/posts/b74f504f/)
# 写在最后
{% note info simple %}
该文章只是展示了我的博客修改了哪些内容，当然还有一些很琐碎的东西像头像修改啦之类的没有详细说明，如果你想都详细的改的话可以参考[官方文档](https://butterfly.js.org/posts/21cfbf15/),我的很多也都是参考他的文档，最后有任何问题欢迎在评论区告诉我(*^▽^*)
{% endnote %}