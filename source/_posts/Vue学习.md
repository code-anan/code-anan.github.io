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
        mouted:function () {
            console.log("Mount")
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

# 免终端开发vue应用

商业级别项目不会在html页面中内嵌使用vue这种方式，这种方式无论是搭建运行还是发布以及安装各种流行的组件库都比较繁琐，为了解决这个问题常常采用`uni-app`加`HBuilderX`的方式，前者是框架后者是IDE，它们互相搭配，使得基于vue.js开发项目变得更简单高效

## 使用步骤

### 创建uni-app项目

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220116135003.png)

选择默认模板即可

### 项目结构

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220116180743.png)

###  HbuilderX使用

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220116181209.png)

上方可以选择在浏览器运行，并一键发布生成需要的html页面，并且可以前往插件市场安装我们需要的插件，例如使用less语言我们找到less编译插件即可

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220116181642.png)

### 插件使用

在[插件市场](https://ext.dcloud.net.cn)找到一款插件导入到项目中，选择`使用HBuilderX导入插件`

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220116182533.png)

并且文档中有着详细的使用说明，例如这一个插件我们在index.vue中加入

```html
<template>
	<view class="content">
		<!-- 在 template 中使用组件 -->
		<yp-number-box></yp-number-box>
		<yp-number-box :min="0" :max="9"></yp-number-box>
		<yp-number-box @change="bindChange"></yp-number-box>
		<yp-number-box @change="change" :index="index" />  
	</view>
</template>

<script>
	import ypNumberBox from "@/components/yp-number-box/yp-number-box.vue" //在 script 中引用组件
	export default {
		data() {
			return {
				title: 'Hello baby'
			}
		},
		onLoad() {

		},
		methods: {

		},
		components: {ypNumberBox} //在 script 中引用组件
	}
	
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
```

### 自定义组件

在`commponents`目录下右键选择新建组件![](https://cdn.jsdelivr.net/gh/code-anan/image/20220116183454.png)

编写内容

```html
<template>
	<view>
		<h1>{{tilte}}</h1>
		<div>{{content}}</div>
	</view>
</template>

<script>
	export default {
		props:{
			tilte:{
				type:String,
				default:"默认标题"
			},
			content:{
				type:String,
				default:"默认内容"
			}
		},
		name:"mycomponent",
		data() {
			return {
				
			};
		}
	}
</script>

<style>

</style>
```

然后在index,vue中引入即可，在`template`标签中使用![](https://cdn.jsdelivr.net/gh/code-anan/image/20220116184454.png)

vue内部还有很多，对于java程序员来说能熟练掌握这些常用的知识以及足够啦(*^▽^*)