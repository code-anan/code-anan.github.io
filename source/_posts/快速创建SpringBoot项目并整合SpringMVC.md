title: 快速创建SpringBoot项目并整合SpringMVC
author: lwl
date: 2021-06-18 10:55:43
tags:
  - SpringBoot
  - SpringMVC
categories:
  - SpringBoot
my: post/springboot start
cover: https://gcore.jsdelivr.net/gh/code-anan/image/springboot.jpg
---
# 快速创建SpringBoot项目
> 需要的工具，这里使用的是`IntelliJ IDEA`
## 进入到`idea`,创建一个空项目`Empty Project`
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210618105839.png)
路径自己设置：
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210618105918.png)
## 进去之后，就可以新建一个`module`
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210618110022.png)
JDK不要说了勾选上，这里选择`Default`
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210618110113.png)
## 进入到初始化页面，进行必要的配置
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210618110321.png)
Type选择`Maven Project`，然后Package最好与Group保持一致，就可以下一步了
## 选择要添加的依赖
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210618110526.png)


> 选择`Web`->`Spring Web`->`选择一个稳定版本，例如2.5.1（SNAPSHOT表示正在开发中的版本）`然后下一步即可
路径默认就行，然后`Finish`（第一次初始化项目可能会下载必要的依赖文件，可能会花几分钟，静静等待）

![](https://gcore.jsdelivr.net/gh/code-anan/image/20210618110711.png)

## 完成创建
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210618111139.png)
这个类也是SpringBoot的入口，启动SpringBoot的地方，到这里一个SpringBoot项目就创建完成了

# 整合SpringMVC
{% note danger simple %}
注意：SpringBoot项目代码必须放到Application类所在的同级目录或下级目录,所以这里我们在example包下新建一个类用来测试
{% endnote %}
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210618111848.png)
然后`Application`类中启动即可
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210618112111.png)
启动完成就可以在浏览器上根据我们在`RequestMapping`中设置的路径进行测试，效果如下：
第一个方法的结果：
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210618112335.png)

第二个方法的结果：
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210618112404.png)
# 总结
> 相比于Spring和SpringMVC的繁琐配置，SpringBoot极大的简化了配置，让我们可以把更多的精力放到业务上，SpringBoot牛逼！φ(>ω<*) 