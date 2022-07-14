title: 写博客文章必须掌握的Markdown基本语法
author: lwl
tags:
  - Hexo
  - Markdown
category:
  - Hexo
  - 文章语法
cover: 'https://gcore.jsdelivr.net/gh/code-anan/image/markdown.jpg'
date: 2021-05-27 21:42:16
my: post/markdown
---
<meta name="referrer" content="no-referrer" />

# 前言
上一篇文章说了如何使用Hexo+github搭建属于自己的个人博客网站，网站有了下一步肯定要开始写文章了，网站建立好以后可以看到它为我们自动创建了一个hello world的文章而且是以`.md`（即Markdown）格式的文章，所以要写好文章需要了解它的基本语法,不过在那之前我觉得还是先说说一下写博客使用的工具，我在一开始的时候是在{% label editplus green %}上进行编辑，但是后来发现容易出现分段的问题，后来在网上找了找写博客用哪些工具，有很多推荐的不过我使用的是hexo自带的工具（Hexo Admin管理插件工具），不过之后更推荐大家用typora了
# Hexo Admin插件的安装
虽然说它是hexo自带的工具，但是这个插件还是需要自己安装的，找到自己的博客根目录下输入：
> npm install --save hexo-admin
{% note info simple %}
 看过有的帖子说最好加上密码保护，但是我觉得其实没有必要
{% endnote %}
安装好之后需要先启动`hexo s`然后在浏览器上输入http://localhost:4000/admin
就可以进入到这个管理界面，这里不仅可以编辑文章，管理界面，还可以在这里发布到远端，设置图片路径等，当然主要是可以编辑博客文章方便而且右端会动态的生成，缺点就是这里编辑内容的一切外挂标签不能动态展示，但是他会自动保存，所以再打开一个链接随时进行刷新就可以![](https://gcore.jsdelivr.net/gh/code-anan/image/20220522124957.png)
出现这个页面表示安装成功`1`可以新建文章 `2`可以开始编辑 感觉还是很方便的
# 基础语法介绍
> 知道了怎么新建文章之后 接下来就可以学习写文章了（当然不是教怎么写内容 内容要自己瞎编啦(*/ω＼*)）
## 标题
hexo支持最多六级标题
```markdown
# 这是一级标题
## 这是二级标题
### 这是三级标题
#### 这是四级标题
##### 这是五级标题
###### 这是六级标题
```
{% note danger simple %}
 注意#后面一定要与内容保持缩进，否则不能生效，
{% endnote %}
效果：
![](https://gcore.jsdelivr.net/gh/code-anan/image/20220522125639.png)

## 字体
字体这一块我感觉常用的就是斜体和加粗 其他的了解一下即可
```markdown
**这是加粗的文字**
*这是倾斜的文字*`
***这是斜体加粗的文字***
~~这是加删除线的文字~~
```
效果：
**这是加粗的文字**
*这是倾斜的文字*`
***这是斜体加粗的文字***
~~这是加删除线的文字~~

## 引用
引用也挺常用的，下面有两种引用方式 ，语法如下：
```markdown
> 这是一个引用
{% blockquote David Levithan, Wide Awake %}
Do not just seek happiness for yourself. Seek happiness for all. Through kindness. Through mercy.
{% endblockquote %}
>> 引用还可以套引用
>>> 还可以再套
```
效果：
> 这是一个引用
> {% blockquote David Levithan, Wide Awake %}
> Do not just seek happiness for yourself. Seek happiness for all. Through kindness. Through mercy.
> {% endblockquote %}
> > 引用还可以套引用
> >
> > > 还可以再套
## 分割线
三个或者三个以上的 - 或者 * 都可以。
```markdown
---
***
-----
**************
```
效果：![](https://gcore.jsdelivr.net/gh/code-anan/image/20220522130324.png)
这里是[butterfly](https://butterfly.js.org/)主题默认美化了 其他主题可能效果会不一样
## 图片
图片在文章中的引用方式应该是最普遍的 然后这里介绍三种引入图片的方式
1. 
```markdown
![图片alt](图片地址 ''图片title'')

图片alt就是显示在图片下面的文字，相当于对图片内容的解释。
图片title是图片的标题，当鼠标移到图片上时显示的内容。title可加可不加
```
图片地址可以为本地也可以为链接 ，例如：
```markdown
![头像](/img/avator.png "头像")
```
效果：
![](https://gcore.jsdelivr.net/gh/code-anan/image/20220522131323.png)
2. 这种方式也是我喜欢用的方法<img>标签 回归html语法
```markdown
<img src="/img/avator.png" width="200px" height="200px">
```
效果：还可以随意调整图片的大小 很适用
![](https://gcore.jsdelivr.net/gh/code-anan/image/20220522124448.jpg)
3. hexo官方API提供的语法 感觉有点麻烦
```markdown
{% img [class names] /path/to/image [width] [height] '"title text" "alt text"' %}
```
感兴趣的可以看看 不演示了╮(╯▽╰)╭
> 另外对于图片存放位置，一般放在source下新创一个img文件夹专门用来存放图片就行，对于图片图床的使用这里不介绍 有兴趣的可以去百度查查
## 超链接
除了markdown的语法 也可以用a标签组
```markdown
[地址名称](地址)
<a href="超链接地址" target="_blank">超链接名</a> 
```
实例：
```markdown
[我的博客](https://code-anan.github.io)
<a href="https://code-anan.github.io" target="_blank">我的博客2</a> 
```
效果：[我的博客](https://code-anan.github.io)
<a href="https://code-anan.github.io" target="_blank">我的博客2</a> 

## 列表
### 有序列表
语法也很简单
```markdown
1. 列表内容
2. 列表内容
3. 列表内容
ps： .后面必须要有空格缩进 不然无效
```
效果：
1. 列表内容
2. 列表内容
3. 列表内容
### 无序列表
```markdown
- 无序
+ 无序
* 无序
ps：同样要加空格缩进 这三个符号效果一样
```
效果：
- 无序
+ 无序
* 无序
### 嵌套使用
```markdown
1. 有序列表
   + 无序1
   - 无序2
   * 无序3
```
效果：
1. 有序列表
   + 无序1
   - 无序2
   * 无序3
## 表格
```markdown
表头|表头|表头
--|:--:|--:
内容|内容|内容
内容|内容|内容

第二行分割表头和内容。
- 有一个就行，为了对齐，多加了几个
文字默认居左
-两边加：表示文字居中
-右边加：表示文字居右
注：原生的语法两边都要用 | 包起来。此处省略
```
示例：
```markdown
姓名|年级|学号
:---:|:---:|:---:
lwl|17|1710241891
xk|17|1710241892
bxl|17|1710241893
lxy|17|1710241381
```
效果：

姓名|年级|学号
:---:|:---:|:---:
lwl|17|1710241891
xk|17|1710241892
bxl|17|1710241893
lxy|17|1710241381
## 代码
### 单行代码
两个反引号就可以
```markdown
`我是单行代码`
```
效果：
`我是单行代码`
### 多行代码
```markdown
(```)
   System.out.pribtln("I love you!")
(```)
ps:小括号是为了防止编译  实际用代码块的时候去掉即可
```
效果：
```java
   System.out.pribtln("I love you!")
```
# Butterfly主题常用的外挂标签
{% blockquote  %}
如果你的主题不是butterfly 建议不要使用 可能会出错，主题更换的方式可以参考别的文章（づ￣3￣）づ╭❤～以下内容来自[jerry](https://butterfly.js.org/posts/4aa8abbe/#Note-Bootstrap-Callout)
{% endblockquote %}
首先要修改`主题配置文件`即（/themes/butterfly目录下的_config.yml）这里建议把这个配置文件改名为`_config.butterfly.yml`并且放到博客的根目录下，如下图：![](https://gcore.jsdelivr.net/gh/code-anan/image/20220522130009.png)
并且进行以下修改：
```yaml
note:
  # Note tag style values:
  #  - simple    bs-callout old alert style. Default.
  #  - modern    bs-callout new (v2-v3) alert style.
  #  - flat      flat callout style with background, like on Mozilla or StackOverflow.
  #  - disabled  disable all CSS styles import of note tag.
  style: simple
  icons: false
  border_radius: 3
  # Offset lighter of background in % for modern and flat styles (modern: -12 | 12; flat: -18 | 6).
  # Offset also applied to label tag variables. This option can work with disabled note tag.
  light_bg_offset: 0
```
## Note
```markdown
{% note simple %}
默認 提示塊標籤
{% endnote %}

{% note default simple %}
default 提示塊標籤
{% endnote %}

{% note primary simple %}
primary 提示塊標籤
{% endnote %}

{% note success simple %}
success 提示塊標籤
{% endnote %}

{% note info simple %}
info 提示塊標籤
{% endnote %}

{% note warning simple %}
warning 提示塊標籤
{% endnote %}

{% note danger simple %}
danger 提示塊標籤
{% endnote %}

```
效果：
{% note simple %}
默認 提示塊標籤
{% endnote %}

{% note default simple %}
default 提示塊標籤
{% endnote %}

{% note primary simple %}
primary 提示塊標籤
{% endnote %}

{% note success simple %}
success 提示塊標籤
{% endnote %}

{% note info simple %}
info 提示塊標籤
{% endnote %}

{% note warning simple %}
warning 提示塊標籤
{% endnote %}

{% note danger simple %}
danger 提示塊標籤
{% endnote %}
## Tabs
```markdown
{% tabs test1 %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}
```
效果：
{% tabs test1 %}
<!-- tab -->
**This is Tab 1.**
<!-- endtab -->

<!-- tab -->
**This is Tab 2.**
<!-- endtab -->

<!-- tab -->
**This is Tab 3.**
<!-- endtab -->
{% endtabs %}
## label
```markdown
{% label text color %}
```
示例：
```markdown
我是{% label 蓝色的 blue %},你是{% label 绿色的 green %}
```
效果：
我是{% label 蓝色的 blue %},你是{% label 绿色的 green %}
## Button
```markdown
{% btn [url],[text],[icon],[color] [style] [layout] [position] [size] %}

[url]         : 鏈接
[text]        : 按鈕文字
[icon]        : [可選] 圖標
[color]       : [可選] 按鈕背景顔色(默認style時）
                      按鈕字體和邊框顔色(outline時)
                      default/blue/pink/red/purple/orange/green
[style]       : [可選] 按鈕樣式 默認實心
                      outline/留空
[layout]      : [可選] 按鈕佈局 默認為line
                      block/留空
[position]    : [可選] 按鈕位置 前提是設置了layout為block 默認為左邊
                      center/right/留空
[size]        : [可選] 按鈕大小
                      larger/留空

```
示例：
```markdown
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}
```
效果：
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,outline %}
This is my website, click the button {% btn 'https://butterfly.js.org/',Butterfly,far fa-hand-point-right,larger %}
> 以上几个是我感觉常用的 如果还想了解其他的 欢迎访问[jetty的官方文档](https://butterfly.js.org/posts/4aa8abbe/#Note-Bootstrap-Callout)
# 温馨提示
{% note simple %}
以上的所有内容，仅仅是针对博客文章内容的介绍，对于博客的主题美化，还有文章的封面分类标签等会在其他文章中提到，有任何问题欢迎在评论区告诉我哦！
{% endnote %}