title: 页脚Footer设置游动的小鱼
author: lwl
date: 2021-05-29 14:33:18
tags:
  - Hexo
  - Footer
categories:
  - Hexo
  - 修改记录
cover: https://cdn.jsdelivr.net/gh/code-anan/image/fish.png
my: post/footer
---
<meta name="referrer" content="no-referrer" />

# 前言
{% note info simple %}
此文主要参考[花猪](https://cnhuazhu.gitee.io/)老哥的文章，但是老哥有一些细节没有说好，所以遇到了一些困难，好在最后解决了,效果在最下方的页脚
{% endnote %}
# 操作步骤
1. 修改`footer.pug`：根目录下`\themes\butterfly\layout\includes\foot.pug`文件最后一行中添加以下代码：
```yaml
#jsi-flying-fish-container.container
   script(src='/js/fish.js')
style.
   
       @media only screen and (max-width: 767px){
       #sidebar_search_box input[type=text]{width:calc(100% - 24px)}
    }
```
{% note warning simple %}
注意这里引入`fish.js`的路径是放在`/js`下，上面的代码最好直接复制不然容易出现有时候游动鱼儿效果失效的情况
{% endnote %}
2. 首先必须引入需要的`jquery`文件，网址为`https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js`，以及必要的鱼儿游动的js文件，网址为`https://cdn.jsdelivr.net/gh/xiabo2/CDN@latest/fish.js`
{% note warning simple %}
虽然说可以在`bottom`下直接引入就可以，但是这里建议在浏览器打开这个地址把这两个文件下载到本地`\themes\butterfly\source\js`下，这里我为了方便把他们改名为`jq.js`，`fish.js`放到这个目录下面
{% endnote %}
3. 修改`主题配置文件`，找到`inject`下的`bottom`,依次引入上一步保存的两个js文件
```yaml
  bottom:
    # - <script src="xxxx"></script>
    - <script src="/js/jq.js"></script>
    - <script src="/js/fish.js"></script>
```
{% note danger simple %}
特别注意：上面引入`jq.js`时，必须把他放在第一行，否则都会出现错误，像效果不出现等
{% endnote %}
4. 为了页脚看起来更美观点，可以选择修改`foot.styl`:`\themes\butterfly\source\css\_layout`,直接全部替换为：
```yaml
#footer
  position: relative
  background: $light-blue
  background-attachment: local
  background-position: bottom
  background-size: cover

  if hexo-config('footer_bg') != false
    &:before
      position: absolute
      width: 100%
      height: 100%
      background-color: alpha($dark-black, .1) 
      content: ''

#footer-wrap
  position: absolute
  padding: 1.2rem 1rem 1.4rem
  color: var(--light-grey)
  text-align: center
  left: 0
  right: 0
  top: 0
  bottom: 0

  a
    color: var(--light-grey)

    &:hover
      text-decoration: underline

  .footer-separator
    margin: 0 .2rem

  .icp-icon
    padding: 0 4px
    vertical-align: text-bottom
    max-height: 1.4em
    width auto

```
5. 根据需要修改透明度（这里我没设置 根据自己的需求来），`\themes\butterfly\source\css`新建一个css文件，放入以下代码：
   + 页脚半透明
```css
/* 页脚半透明 */
#footer {
    background: rgba(255, 255, 255, 0);
    color: #000;
    border-top-right-radius: 20px;
    border-top-left-radius: 20px;
    backdrop-filter: saturate(100%) blur(5px)
}

#footer::before {
    background: rgba(255,255,255,0)
}

#footer #footer-wrap {
    color: var(--font-color);
}

#footer #footer-wrap a {
    color: var(--font-color);
}
```
   + 全透明
```css
/* 页脚透明 */
#footer {
    background: transparent !important;
}
```
最后在依然是`主题配置文件`，inject下的head中引入上面创建的css文件：
```yaml
- <link rel="stylesheet" href="/css/xxx.css">
```

# 可能遇到的问题
  问：为什么我第一次刷新的时候有效果，跳转页面就没有了？
  答：第一步修改`footer.png`中，引入`fish.js`时的路径为`'/js/fish.js'`,可能是少了/
  问：按照上面的修改了之后页脚背景图都消失了怎么回事？
  答：在inject下引入js文件时，必须把`jq.js`放到第一行，修改完再刷新重启试试

