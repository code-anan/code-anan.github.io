title: Vue学习
author: lwl
cover: https://cdn.jsdelivr.net/gh/code-anan/image/src=http---i.loli.net-2020-01-13-TPKA1wp6s4ufSm2.png&refer=http---i.loli.jpg
my: post/vue
categories:
  - Vue
tags:
  - Vue

---

# vue.js介绍

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220114142235.png)
PS: 本文参考[官网视频](https://learning.dcloud.io/#/?vid=0)

# 安装与部署

## 使用script标签引入

引入之前需要下载，下载地址我们直接在[官网](https://cn.vuejs.org/v2/guide/installation.html#%E7%9B%B4%E6%8E%A5%E7%94%A8-lt-script-gt-%E5%BC%95%E5%85%A5)下载即可

## CDN方式引入

如果不想下载js文件，我们也可以使用CDN的方式引入,html页面中head标签内引入即可

```js
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
```

# 创建第一个VUE应用

第一个应用 只是显示出创建vue实例里面的值

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vue学习</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        {{message}}
    </div>
    <script type="text/javascript">
        var app=new Vue({
            el:"#app",
            data :{
                message:"Hello VUE!"
            }
        })
    </script>
</body>
</html>
```

new Vue表示创建vue的实例，`{{}}`表示取data里面的值，上面div的id的值与el属性保持一致，并且该script标签可以不放在body里面

# 数据与方法

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vue学习</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        {{message}}{{name}}
    </div>

</body>
<script type="text/javascript">
    var mydata={message:"死狗子",name:""}
    var vue=new Vue({
        el:"#app",
        data : mydata
    })
    vue.message="阿西吧"
    mydata.name="lalala"
    vue.$watch("message",function (oldvalue,newvalue) {
            console.log(oldvalue,newvalue)
    })
    vue.$data.message="再次阿西吧"
</script>
</html>
```

data中除了自定义的值，还可以引用外面定义的值，比如上面的mydata，同时可以使用`vue实例.属性值名或者`自定义变量.属性名`的方式改变属性值还有`vue实例.$data.属性名`的方式修改属性值，`vue.$watch`表示给属性增加一个方法，这个回调方法将在 `vm.message` 改变后调用

# 生命周期

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vue学习</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        {{message}}{{name}}
    </div>

</body>
<script type="text/javascript">
    var mydata={message:"死狗子",name:""}
    var vue=new Vue({
        el:"#app",
        data : mydata,
        //在实例初始化之后，数据观测和event/watcher时间配置之前被调用
        beforeCreate: function () {
            console.log("beforeCreate")
        },
        //在实例创建完成后立刻被调用
        //在这一步，实例已完成以下配置：数据观测、属性和方法的运算、event/watcher事件回调
        //挂载阶段还没开始，#el属性目前不可见
        created:function () {
            console.log("Created")
        },
        //在挂载开始之前被调用，相关的渲染函数首次被调用
        beforeMount:function () {
            console.log("beforeMount")
        },
        //el被新创建的wm.$el替换，挂载成功
        mounted:function () {
            console.log("mounted")
        },
        //数据更新时调用
        beforeUpdate:function () {
            console.log("beforeUpdate")
        },
        //组件DOM已经更新，组件更新完毕
        updated:function () {
            console.log("updated")
        },
        
    })
    vue.message="阿西吧"
    mydata.name="lalala"
    vue.$watch("message",function (oldvalue,newvalue) {
            console.log(oldvalue,newvalue)
    })
    vue.$data.message="再次阿西吧"
</script>
</html>
```

常用的生命周期钩子函数如上图所示，还有一些其他的生命周期函数可以在官网查阅但是前期只需要了解即可

# 模板语法

## 插值

### 文本

数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值,这种方式可以动态的改变他的值，但是还有一个`v-once`指令，通过使用它插值处的内容不会进行更新

```html
  <span v-once>{{name}}</span>
```

### 原始html

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 [`v-html` 指令](https://cn.vuejs.org/v2/api/#v-html)：

```html
<span v-html="rawHtml"></span>
```

```javascript
var vue=new Vue({
        el:"#app",
        data :{
            rawHtml: '<span style="color: red">i am cool</span>'
        }

    })
```

### Attribute

```
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>vue学习</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
</head>
<body>
    <div id="app">
        <p v-html="rawHtml"></p>
        <button v-bind:class="color">Button</button>
    </div>


</body>
<script type="text/javascript">
    var vue=new Vue({
        el:"#app",
        data :{
            rawHtml: '<span style="color: red">i am cool</span>',
            color: 'red'
        }
    })
</script>
<style type="text/css">
    .red{
        color: red;
        font-size: 100px;
    }
</style>
</html>
```

使用v-bind的作用就是把这个标签的任何属性跟data中的值绑定到一起，比如上面例子的css跟data中的color相绑定

### javascript表达式

```js
<div>{{ number + 1 }}</div>

        <div>{{ ok ? 'YES' : 'NO' }}</div>

        <div>{{ message.split(' ').reverse().join(',') }}</div>


ar vue=new Vue({
        el:"#app",
        data :{
            number: 1,
            ok: true,
            message: "java css html"
        }
    })
```

得到结果如下：

2

YES

html,css,java

## 指令

指令 (Directives) 是带有 `v-` 前缀的特殊 attribute。指令 attribute 的值预期是**单个 JavaScript 表达式** (`v-for` 是例外情况，稍后我们再讨论)。指令的职责是，当表达式的值改变时，将其产生的连带影响，响应式地作用于 DOM。

例如 v-if后面跟上data里面的值为true时显示，为false时不显示

```html
<div id="app" v-if="seen">
    {{message}}
</div>
```

### 参数

一些指令能够接收一个“参数”，在指令名称之后以冒号表示。例如，`v-bind` 指令可以用于响应式地更新 HTML attribute：

```html
<a v-bind:href="url">...</a>
```

在这里 `href` 是参数，告知 `v-bind` 指令将该元素的 `href` attribute 与表达式 `url` 的值绑定。

另一个例子是 `v-on` 指令，它用于监听 DOM 事件：

```html
<a v-on:click="doSomething">...</a>
```

### 动态参数

```html
<a v-bind:[attributeName]="url"> ... </a>
```

如果你的 Vue 实例有一个 `data` property `attributeName`，其值为 `"href"`，那么这个绑定将等价于 `v-bind:href`不过这种方式不常用

## 缩写

```html
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a :[key]="url"> ... </a>


<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>

<!-- 动态参数的缩写 (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```

# class与style绑定

## 绑定html calss

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>vue学习</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <span v-bind:class="{active : isactive,'green' : isgreen}">我是span</span>
    <span v-bind:class="['active','green']">我是span2</span>
    <span v-bind:class="[isactive?'active':'',isgreen?'green':'']">我是span3</span>
</div>
<script type="text/javascript">
    var app=new Vue({
        el:"#app",
        data :{
            message:"Hello VUE!",
            seen: true,
            isactive: false,
            isgreen: true
        }
    })
</script>
<style type="text/css">
    .active{
        background-color: red;
    }
    .green{
        color: green;
    }
</style>
</body>
</html>
```

观察上面的三个span通过第一个我们能看到，可以使用{}的方式动态的给元素添加class样式其值跟data中绑定，通过第二个可以发现也可以使用[]的方式静态的添加，第三个可以发现可以使用数组同时也能动态的使用data中的值并且加上三目运算符

## 绑定style

```html
<div id="app">
    <div v-bind:style="{color:color,fontSize:size}">
        我是div啊
    </div>
</div>
<script type="text/javascript">
    var app=new Vue({
        el:"#app",
        data :{
            color: "#F00000",
            size: "50px"
        }
    })
```

与class基本一致，也可以使用三目运算符的形式

# 条件渲染

```html
<div id="app">
    <div v-if="type === 'A'" >
            A
    </div>
    <div v-else-if="type === 'B'" >
        B
    </div>
    <div v-else-if="type === 'C'" >
        C
    </div>
    <div v-else>
       不是ABC之中的值
    </div>
    <div v-show="show">
        看不看得到我
    </div>
</div>
<script type="text/javascript">
    var app=new Vue({
        el:"#app",
        data :{
            type: "D",
            show: false
        }
    })
</script>
```

使用上面的例子动态改变type的值即可看到`v-if`,`v-else-if`,`v-else`,`v-show`的用法，需要注意的是`v-show`总是会被渲染到页面上，只是简单地基于 CSS 进行切换；一般来说，`v-if` 有更高的切换开销，而 `v-show` 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 `v-show` 较好；如果在运行时条件很少改变，则使用 `v-if` 较好

# 列表渲染

```html
<div id="app">
    <ul>
        <li v-for="item,index in items" :key="index">
            {{index +1}}{{item.message}}
        </li>
    </ul>

    <ul>
        <li v-for="value,key in object" :key="key">
            {{key}}:{{value}}
        </li>
    </ul>
</div>
<script type="text/javascript">
    var app = new Vue({
        el: "#app",
        data: {
            items: [
                {message: "css"},
                {message: "java"},
                {message: "html"},
            ],
            object:{
                name:"张三",
                age: 18,
                email: "719603766@qq.com"
            }
        }
    })
</script>
```

通过上面的例子不难发现，循环一个数组我们可以获取到每一个对象的值和他的索引（从0开始）除了数组还可以循环一个对象的属性，key和value值都能获取，上面的items和object分别表示要循环的数组或对象的名字，:key是为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素

# 事件绑定

## 基本使用

```html
<div id="app">
    <button v-on:dblclick="myclick()">{{count}}</button>
    <button v-on:click="greet('啦啦啦',$event)">点我</button>
</div>
<script type="text/javascript">
    var app = new Vue({
        el: "#app",
        data: {
            count: 0,
            name :"vue"
        },
        methods:{
            myclick:function () {
                this.count+=1;
            },
            greet:function (str,e) {
                console.log(str);
                console.log(e)
            }
        }
    })
</script>
```

通过这个例子，我们可以快速了解事件绑定的使用，通过`v-on`给元素绑定事件，例如`dblclick`双击和`click`单击事件，并且还有数据传递的方式和特殊参数`$enevnt`,以及在方法中我们同样可以使用data中的数据

## 事件修饰符

```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
```

这些是常用的事件修饰符，更多可以查看[官网](https://cn.vuejs.org/v2/guide/events.html)

# 表单输入绑定

```html
<div id="app">
   <div id="example1">
       <input type="text" v-model="message1" placeholder="edit">
       <p>Message is:{{message1}}</p>
       <textarea v-model="message2" placeholder="输入多行文本框"></textarea>
       <p>Text area:{{message2}}</p>
       <div>
           <input type="checkbox" id="java" value="Java" v-model="checkdValues">
           <label for="java">java</label>
           <input type="checkbox" id="vue" value="vue" v-model="checkdValues">
           <label for="vue">vue</label>
           <input type="checkbox" id="xml" value="xml" v-model="checkdValues">
           <label for="xml"></label>
           <br>
           <span>checkBox value is:{{checkdValues}}</span>
       </div>

       <div>
           <input type="radio" id="123" value="123" v-model="passwords">
           <label for="123">123</label>
           <input type="radio" id="321" value="321" v-model="passwords">
           <label for="321">321</label>
           <br>
           <span>password is:{{passwords}}</span>
       </div>
       <button @click="submit" >提交</button>
   </div>
</div>
<script type="text/javascript">
    var app = new Vue({
        el: "#app",
        data: {
            message1: "",
            message2:" ",
            checkdValues:[],
            passwords:"321"

        },methods:{
            submit:function () {
                    var obj={
                        message:this.message1,
                        textarea:this.message2,
                        checked:this.checkdValues,
                        password:this.passwords
                    }
                    console.log(this.message1)
                    console.log(obj)
            }
        }
    })
</script>
```

使用此例子我们可以看到表单双向绑定使用`v-model`指令进行绑定

# 组件基础

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml" xmlns:v-on="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>vue学习</title>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
</head>
<body>
<div id="app">
    <mybutton title="mytitle:"></mybutton>
    <mybutton></mybutton>
    <anothercomponent @clicknow="clicknow">
        <h2>我是h2</h2>
    </anothercomponent>
</div>
<script type="text/javascript">
    // 定义一个名为mybutton 的新组件
    Vue.component('mybutton', {
        props:['title'],
        data: function () {
            return {
                count: 0
            }
        },
        template: '<div><h1>啦啦啦</h1><button v-on:click="count++">{{title}}You clicked me {{ count }} times.</button></div>',
    })
    Vue.component('anothercomponent', {
        data: function () {
            return {
                count: 0
            }
        },
        template: '<div><button v-on:click="myclick()">我的conunt值为{{count}}</button><slot></slot></div>',
        methods: {
            myclick:function () {
                this.count++;
                this.$emit('clicknow',this.count)
            }
        }
    })
    var app = new Vue({
        el: "#app",
        data: {

        },methods:{
            clicknow:function (e) {
                console.log("app接收的值为："+e)
            }
        }
    })

</script>
<style type="text/css">

</style>
</body>
</html>
```

通过此示例可以观察到，声明组件的格式如上并且声明的组件可以复用且有着各自的作用域，声明组件第一个属性为组件的名字，使用的时候标签名就是组件的名字，然后`props`是给组件添加新的属性，例如上面添加了一个title属性，`template`表示组件包含的内容，在`template`内可以声明事件绑定如果想在组件内再添加html标签则需要在`template`内添加`slot`标签，最后一个`$emit`定义的函数可以把组件的参数传到我们创建的vue实例中进行接收，例如上面的clicknow函数，这个名称可以自定义

# 组件注册

第一种全局注册 就像上面的组建基础的例子一样 在我们创建的vue实例上面直接声明组件，那样就是全局注册

第二种是可以局部注册，方式如下

```html
var app = new Vue({
        el: "#app",
        data: {

        },methods:{
            clicknow:function (e) {
                console.log("app接收的值为："+e)
            }
        },
        components:{
            mybutton:{
                props:['title'],
                data: function () {
                    return {
                        count: 0
                    }
                },
                template: '<div><h1>啦啦啦</h1><button v-on:click="count++">{{title}}You clicked me {{ count }} times.</button></div>'
            }
        }
    })
```

以对象的形式注册局部组件，components中的mybutton为局部组件的名称其他的属性和全局组件一样了

# 单文件组件

使用单文件组件需要满足下面环境

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220116130924.png)

npm的安装只需要安装[Node.js](http://nodejs.cn/download/)即可,其他指令

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm install -g @vue/cli
cnpm install -g webpack
```

启动vue ui的图形化管理页面，使用指令`vue ui`即可开启，这里我们新创建一个项目，包管理器选择npm其他默认就行，创建完成之后我们刚才选择的目录下就可以看到项目了，然后把项目拖到我们的[HBuilderX](https://www.dcloud.io/hbuilderx.html)中打开看到项目结构如下

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220116133411.png)

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220116133707.png)这里引入的就是单文件组件的方式了，当然可以自己定义一个组件进行使用

# Axios异步通信

Axios是一个开源的可以用在浏览器端和`NodeJS`的异步通信框架，主要作用是实现ajax异步通信，其功能特点如下

*  从浏览器创建`XMLHttpRequests`
*  从Nodejs创建http请求
*  支持Promise API
*  拦截请求和响应
*  转换请求数据和响应数据、取消请求
*  自动转换json数据
*  客户端支持防御XSRF（跨站请求伪造）

cdn地址：`<script src="https://unpkg.com/axios/dist/axios.min.js"></script>`

官网地址：[axios中文网|axios API 中文文档 | axios (axios-js.com)](http://www.axios-js.com/)

实例如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="app">
<span>{{info.name}}</span>
<span>{{info.url}}</span>
<li v-for="item in info.links">
    <span >{{item.name}}</span>
    <span >{{item.url}}</span>
</li>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>


    var vue = new Vue({
        el:"#app",
        data(){
            return{
                info:{
                }
            }
        },
        mounted(){
            axios.get('../data.json').then(res=> {
                    console.log(res)
                    this.info=res.data
            })
        },
    });
</script>
</body>
</html>
```

说明：Vue的开发都是基于NodeJs，实际开发采用vue-cli脚手架开发、vue-router路由。vuex做状态管理；Vue UI界面我们一般使用ElementUI或者ICE来快速搭建前端项目

# Vue-cli

vue-cli是官方提供的一个脚手架用于快速生成一个vue的项目模板，类似于创建maven项目创建一个骨架，需要提前下载nodejs即可

## 安装

`cnpm install vue-cli -g`

`vue list`查看可以基于哪些模板创建vue应用程序，通畅选择webpack

## 第一个vue-cli应用程序

首先创建一个vue项目，只需要创建一个文件夹即可

使用`vue init webpack myvue`创建一个基于webpack模板的vue应用程序

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220417155409.png)

来到我们的项目下执行`npm install`安装所有的依赖，可能会失败按照提示修复一下即可

`npm run dev`运行dev环境![](https://cdn.jsdelivr.net/gh/code-anan/image/20220417155839.png)![](https://cdn.jsdelivr.net/gh/code-anan/image/20220417155854.png)

# Vue Router

vue router是vue官方的路由管理器，他和vue到的核心深度集成，让构建单页面应用变得易如反掌

## 安装使用

`npm install vue-router  --save-dev `

然后在组件化的项目中使用的话需要显示的进行声明

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter);
```

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220417165005.png)

但是实际项目开发，我们会把路由配置专门创建一个`router/index.js`之后在main.js中引用即可

```js
import Vue from 'vue'
import App from './App'
import router from './router'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})

```

router中的index.js写具体的路由配置

```js
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import newpage from '@/components/newpage'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/hello',
      name: 'HelloWorld',
      component: HelloWorld
    },
    {
      path: '/newpage',
      name: 'newpage',
      component: newpage
    }
  ]
})
```

最后在入口app.vue中template标签内使用即可

```html
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-link to="/hello">Hello</router-link>
    <router-link to="/newpage">newpage</router-link>
    <router-view/>
  </div>
</template>
```

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220418110809.png)



可以看到他并不是跳转页面 而是组件之间的路由 这就是vue的特点

# Vue+ElementUI实例

官网：[Element - 网站快速成型工具](https://element.eleme.io/#/zh-CN)

## 创建工程

`vue init webpack hello-vue`还是先初始化一个项目（这里可以选择安装vue-router方便其他项看需求选择）

`cd hello-vue`进入项目

`npm i element-ui -S`安装elementUI

`npm insall`安装依赖

`cnpm install sass-loader node-sass --save-dev`安装sass加载器

`npm run dev`启动项目

npm命令说明

> npm install moduleName：安装模块到项目目录下
>
> npm install -g moduleName：-g的意思是将模块安装到全局，具体安装到磁盘哪个位置要看npm
>
> config prefix的位置
>
> npm install -save moduleName：–save的意思是将模块安装到项目目录下， 并在package文件的dependencies节点写入依赖，-S为该命令的缩写
>
> npm install -save-dev moduleName:–save-dev的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖，-D为该命令的缩写

创建好工程之后，我们可以把自动生成的hello组件删掉，生成我们自己的组件

main.vue

```vue
<template>
  <div>首页</div>
</template>
<script>
  export default {
    name:"Main"
  }
</script>
<style scoped>
</style>

```

login.vue

```vue
<template>
  <div>
    <el-form ref="loginForm" :model="form" :rules="rules" label-width="80px" class="login-box">
      <h3 class="login-title">欢迎登录</h3>
      <el-form-item label="账号" prop="username">
        <el-input type="text" placeholder="请输入账号" v-model="form.username"/>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" placeholder="请输入密码" v-model="form.password"/>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" v-on:click="onsubmit('loginForm')">登录</el-button>
      </el-form-item>
    </el-form>

    <el-dialog title="温馨提示" :visible.sync="dialogVisiable" width="30%" :before-close="handleClose">
      <span>请输入账号和密码</span>
      <span slot="footer" class="dialog-footer">
          <el-button type="primary" @click="dialogVisible = false">确定</el-button>
        </span>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    name: "Login",
    data(){
      return{
        form:{
          username:'',
          password:''
        },
        //表单验证，需要在 el-form-item 元素中增加prop属性
        rules:{
          username:[
            {required:true,message:"账号不可为空",trigger:"blur"}
          ],
          password:[
            {required:true,message:"密码不可为空",tigger:"blur"}
          ]
        },

        //对话框显示和隐藏
        dialogVisible:false
      }
    },
    methods:{
      onsubmit(formName){
        //为表单绑定验证功能
        this.$refs[formName].validate((valid)=>{
          if(valid){
            //使用vue-router路由到指定界面，该方式称为编程式导航
            alert("123")
            this.$router.push('/main');
          }else{
            this.dialogVisible=true;
            return false;
          }
        });
      }
    }
  }
</script>

<style lang="scss" scoped>
  .login-box{
    border:1px solid #DCDFE6;
    width: 350px;
    margin:180px auto;
    padding: 35px 35px 15px 35px;
    border-radius: 5px;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    box-shadow: 0 0 25px #909399;
  }
  .login-title{
    text-align:center;
    margin: 0 auto 40px auto;
    color: #303133;
  }
</style>

```

main.js

```js
import Vue from 'vue'
import App from './App'
import router from './router'

Vue.config.productionTip = false

import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);

new Vue({
  el: '#app',
  router,
  render: h => h(App)
})
```

index.js

```js
import Vue from 'vue'
import Router from 'vue-router'
import main from '@/components/main'
import login from '@/components/login'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'main',
      component: main
    },{
      path: '/login',
      name: 'login',
      component: login
    }
  ]
})
```

App.vue

```vue
<template>
  <div id="app">
    <router-link to="/login">登录</router-link>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>

</style>

```

效果：![](https://cdn.jsdelivr.net/gh/code-anan/image/20220418150519.png)

## 错误解决

运行可能出现的错误：`Module build failed: TypeError: this.getOptions is not a function`

解决方案

```tex
##先卸掉原来的高版本
npm uninstall sass-loader
##安装合适的版本的sass
npm install sass-loader@7.3.1 --save-dev
##这时候可能还会报错Module build failed: Error: Node Sass version 7.0.1 is incompatible with ^4.0.0.
npm uninstall node-sass
npm i -D sass
##执行之后重新运行
npm run dev
```

## 路由嵌套

components下新建一个user文件夹

list.vue

```vue
<template>
  <h1>用户列表</h1>
</template>
<script>
  export default {
    name: "UserList"
  }
</script>
<style scoped>
</style>

```

profile.vue

```vue
<template>
  <h1>个人信息</h1>
</template>
<script>
  export default {
    name: "UserProfile"
  }
</script>
<style scoped>
</style>

```

路由设置index.js

```vue
import Vue from 'vue'
import Router from 'vue-router'
import main from '@/components/main'
import login from '@/components/login'
import list from '@/components/user/list'
import profile from '@/components/user/Profile'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/main',
      name: 'main',
      component: main,
      children: [{
        path: '/user/profile', component: profile,

      }, {
        path: '/user/list', component: list,

      }]
    }, {
      path: '/login',
      name: 'login',
      component: login
    }
  ]
})

```

主页面main.js

```js
<template>
  <div>
    <el-container>
      <el-aside width="200px">
        <el-menu :default-openeds="['1']">
          <el-submenu index="1">
            <template slot="title"><i class="el-icon-caret-right"></i>用户管理</template>
            <el-menu-item-group>
              <el-menu-item index="1-1">
                <!--插入的地方-->
                <router-link to="/user/profile">个人信息</router-link>
              </el-menu-item>
              <el-menu-item index="1-2">
                <!--插入的地方-->
                <router-link to="/user/list">用户列表</router-link>
              </el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <el-submenu index="2">
            <template slot="title"><i class="el-icon-caret-right"></i>内容管理</template>
            <el-menu-item-group>
              <el-menu-item index="2-1">分类管理</el-menu-item>
              <el-menu-item index="2-2">内容列表</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
        </el-menu>
      </el-aside>

      <el-container>
        <el-header style="text-align: right; font-size: 12px">
          <el-dropdown>
            <i class="el-icon-setting" style="margin-right: 15px"></i>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item>个人信息</el-dropdown-item>
              <el-dropdown-item>退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </el-header>
        <el-main>
          <!--在这里展示视图-->
          <router-view />
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>
<script>
  export default {
    name: "Main"
  }
</script>
<style scoped lang="scss">
  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }
  .el-aside {
    color: #333;
  }
</style>
```

路由嵌套说白了就是在一个路由下有n个子路由 使用`children`即可

## 参数传递及重定向

依然使用上面的示例

main.js

```js
<template>
  <div>
    <el-container>
      <el-aside width="200px">
        <el-menu :default-openeds="['1']">
          <el-submenu index="1">
            <template slot="title"><i class="el-icon-caret-right"></i>用户管理</template>
            <el-menu-item-group>
              <el-menu-item index="1-1">
                <!--插入的地方-->
                <router-link :to="{name:'profile',params:{name:'皮卡丘'}}">个人信息</router-link>
              </el-menu-item>
              <el-menu-item index="1-2">
                <!--插入的地方-->
                <router-link to="/user/list">用户列表</router-link>
              </el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <el-submenu index="2">
            <template slot="title"><i class="el-icon-caret-right"></i>内容管理</template>
            <el-menu-item-group>
              <el-menu-item index="2-1">分类管理</el-menu-item>
              <el-menu-item index="2-2">内容列表</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
        </el-menu>
      </el-aside>

      <el-container>
        <el-header style="text-align: right; font-size: 12px">
          <el-dropdown>
            <i class="el-icon-setting" style="margin-right: 15px"></i>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item>个人信息</el-dropdown-item>
              <el-dropdown-item>退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </el-header>
        <el-main>
          <!--在这里展示视图-->
          <router-view />
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>
<script>
  export default {
    name: "Main"
  }
</script>
<style scoped lang="scss">
  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }
  .el-aside {
    color: #333;
  }
</style>
```

router下的index.js

```js
import Vue from 'vue'
import Router from 'vue-router'
import main from '@/components/main'
import login from '@/components/login'
import list from '@/components/user/list'
import profile from '@/components/user/Profile'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/main',
      name: 'main',
      component: main,
      children: [{
        path: '/user/profile/:name', name:'profile',component: profile,

      }, {
        path: '/user/list', component: list,

      }]
    }, {
      path: '/login',
      name: 'login',
      component: login
    }
  ]
})
```

注意这里也可以加上props设为true这样就可以在组件中获取参数`path: '/user/profile/:name', name:'profile',component: profile,props:true`

profile.vue

```vue
<template>
  <div>
    <h1>个人信息</h1>
    {{$route.params.name}}
    {{name}}
  </div>

</template>
<script>
  export default {
    props:['name'],
    name: "UserProfile"
  }
</script>
<style scoped>
</style>
```

重定向也很简单，在router中的index中新添加个路由即可

```js
{
      path: '/gohome',
      redirect: '/main'
}
```

使用我们登录的用户

login.vue

```vue
 if(valid){
            //使用vue-router路由到指定界面，该方式称为编程式导航
            this.$router.push('/main/'+this.form.username);
```

然后通过路由index.js

```vue
path: '/main/:name',
      name: 'main',
      component: main,
      props: true,
      children: [{
        path: '/user/profile/:name', name:'profile',component: profile,props:true
```

最后在main.vue中展示

```vue
<template>
  <div>
    <el-container>
      <el-aside width="200px">
        <el-menu :default-openeds="['1']">
          <el-submenu index="1">
            <template slot="title"><i class="el-icon-caret-right"></i>用户管理</template>
            <el-menu-item-group>
              <el-menu-item index="1-1">
                <!--插入的地方-->
                <router-link :to="{name:'profile',params:{name:'皮卡丘'}}">个人信息</router-link>
              </el-menu-item>
              <el-menu-item index="1-2">
                <!--插入的地方-->
                <router-link to="/user/list">用户列表</router-link>
              </el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <el-submenu index="2">
            <template slot="title"><i class="el-icon-caret-right"></i>内容管理</template>
            <el-menu-item-group>
              <el-menu-item index="2-1">分类管理</el-menu-item>
              <el-menu-item index="2-2">内容列表</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
        </el-menu>
      </el-aside>

      <el-container>
        <el-header style="text-align: right; font-size: 12px">
          <el-dropdown>
            <i class="el-icon-setting" style="margin-right: 15px"></i>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item>个人信息</el-dropdown-item>
              <el-dropdown-item>退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
          <span>{{name}}</span>
        </el-header>
        <el-main>
          <!--在这里展示视图-->
          <router-view />
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>
<script>
  export default {
    props:['name'],
    name: "Main"
  }
</script>
<style scoped lang="scss">
  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }
  .el-aside {
    color: #333;
  }
</style>
```

## 路由模式与404

hash：路径带#符号 如http://localhost/#/login

history: 路径不带#符号，如http://localhost/login

修改路由模式

```vue
export default new Router({
  mode: 'history',
  routes: [
    {
      path: '/main/:name',
      name: 'main',
      component: main,
      props: true,
      children: [{
        path: '/user/profile/:name', name:'profile',component: profile,props:true

      }, {
        path: '/user/list', component: list,

      }]
    }, {
      path: '/login',
      name: 'login',
      component: login
    },
    {
      path: '/gohome',
      redirect: '/main'
    }
  ]
})
```

404页面也是添加个路由新建个页面就行（页面忽略）

```vue
{
      path: '*',
      component: NotFound
    }
```

## 路由钩子

beforeRouteEnter:在进入路由前执行

beforeRouteLeave:在离开路由前执行

```js
<script>
  export default {
    props:['name'],
    name: "UserProfile",
    beforeRouteEnter:(to,from,next)=>{
      console.log("在进入路由前执行")
      next();
    },
    beforeRouteLeave:(to,from,next)=>{
      console.log("在离开路由前执行")
      next();
    }
  }
</script>
```

参数说明:

> to:路由将要跳转的路径信息
>
> from：路径跳转前的路径信息
>
> next：路由的控制参数
>
> ​        next()跳入下一个页面
>
> ​	next('/path')改变路由的跳转方向，使其跳到另一个路由
>
> ​	next（false）返回原来的页面
>
> ​	next((vm)=>{})仅在beforeRouteEnter中可用 vm是组件实例

使用axios

安装其组件`npm install --save axios vue-axios`

main.js中加入

```js
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios, axios)
```

在profile.vue中使用axios获取数据

```vue
<template>
  <div>
    <h1>个人信息</h1>
    {{$route.params.name}}
    {{name}}
  </div>

</template>
<script>
  export default {
    props:['name'],
    name: "UserProfile",
    beforeRouteEnter:(to,from,next)=>{
      console.log("在进入路由前执行")
      next(vm => {
            vm.getdata();
      });
    },
    beforeRouteLeave:(to,from,next)=>{
      console.log("在离开路由前执行")
      next();
    },
    methods:{
      getdata:function () {
        this.axios.get("http://localhost:8082/static/data.json").then((response) => {
          console.log(response.data)
        })
      }
    }
  }
</script>
<style scoped>
</style>
```

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220419104241.png)

可以看到确实获取到数据了