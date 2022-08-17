cover: https://gcore.jsdelivr.net/gh/code-anan/image/src=http---www.downkr.com-uploadfile-2021-0720-227530089.jpg&refer=http---www.downkr.jpg
my: post/SpringMVCkeys
title: SpringMVC知识点回顾
categories:
  - 框架
  - SpringMVC
tags:
  - SpringMVC
------

#  概述

基于spring的一个框架，实际上是spring的一个模块专门用来做web开发，底层是servlet

SpringMVC能够创建对象放入到容器中，springMVC中存放的就是控制器对象，springMVC中有一个对象是DispatcherServlet（中央调度器）：他负责接收用户的所有请求，然后把请求转发给我们的Controller对象，最后Controller对象处理请求

# 初步了解

通过示例可以最直观的理解它的运行步骤和模式

## 新建一个项目

![](https://gcore.jsdelivr.net/gh/code-anan/image/20211230184240.png)

并创建需要的java、resources文件夹

## 添加依赖

```xml
    <!--jsp servlet依赖-->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
    </dependency>
    <!--springmvc的依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.10</version>
    </dependency>
```

## web.xml配置

创建中央调度器对象DispatcherServlet，它是一个servlet 父类继承HttpServlet它也叫做前端控制器，负责接收用户发起的请求调用其他的控制器对象并把请求的处理结果显示给用户

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

  <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>

</web-app>
```

在DispatcherServlet对象创建过程中，会同时创建好springmvc容器对象读取springmvc的配置文件，把这个配置文件中的对象都创建好，当用户发起请求时就可以直接使用对象了

servlet的初始化会执行init（）方法，DispatcherServlet在init中创建容器读取配置文件并把容器对象放入到servletContext中

<load-on-startup>表示tomcat启动后创建对象的顺序，数值越小tomcat创建对象的时间越早，但是要大于等于0

init-param标签的值为指定springmvc配置文件的属性和文件位置，否则springmvc创建容器对象会默认读取/WEB-INF/<servlet-name>-servlet.xml文件

url-pattern表示的是路由的地址，他的值可以有两种方式，第一种是像上面一样使用扩展名的方式*.do表示访问路由为这个值时才会把请求都交给该中央调度器处理；第二种方式是使用'/'；下面会详细介绍

## 创建发起请求的页面

这里简单创建一个jsp页面即可

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<p>这是我发起请求的测试</p>
<p><a href="some.do">点击发起请求</a></p>
</body>
</html>
```

## 创建控制器类

控制器类使用@Controller注解用来创建对象，把对象放入到springmvc容器中

```java
@Controller
public class MyController {

    @RequestMapping("/some.do")
    public ModelAndView doSome(){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("name","lwl");
        modelAndView.addObject("age","23");
        modelAndView.setViewName("/show.jsp");
        return modelAndView;
    }
}
```

doSome方法用来处理some.do的请求， @RequestMapping:请求映射，作用是把一个请求地址和一个方法绑定在一起，一个请求指定一个方法处理；该注解的位置可以放到方法上面也可以放到类;@RequestMapping的属性值value可以省略并且可以有多个值,例如

```java
@RequestMapping(value = {"/some.do","/other.do"})
```

属性method可以指定请求方式，值为get或者post不写默认get请求

modelAndView.setViewName表示指定识图的路径，执行forward操作，等同于servlet的request.getRequestDispatcher().forward()

## 展示页面

简单写个展示页面即可

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h4>${name}的年龄为${age}</h4>
</body>
</html>
```

## 创建springmvc的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
     <!--声明组件扫描器-->   
    <context:component-scan base-package="com.weilong.controller"/>
</beans>
```

## 测试结果

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220104182825.png)

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220104182851.png)

这样一个最基本的springmvc的使用就完成了

# 加深理解

## 视图解析器

为了防止用户可以直接访问资源文件，我们可以把资源文件放到/WEB-INF下![](https://gcore.jsdelivr.net/gh/code-anan/image/20220105153417.png)

但是这样的问题是,我们在controller中指定资源文件的路径会变得麻烦，所以这里引入`视图解析器`的使用

```xml
<!--添加视图解析器，方便开发人员设置视图文件的路径-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
```

所以这里控制器类的路径就可以改写成

```java
modelAndView.setViewName("show");
```

## 处理器映射器（用注解的方式不要）

```xml
 <!--处理器映射器-->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
```

这个映射器会根据bean的名字进行映射，所以需要同时添加uri的bean标签用来匹配对应的controller，这种方式只适用于Controller中是实现Controller接口的方式

## 处理器适配器（用注解的方式不要）

```xml
 <!--处理器适配器-->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
```

跟处理器映射器同时存在，根据uri的class值找到对应的controller接口实现类，使用注解的方式可以直接不用写

## RestController

@RestController放在类的上面，表示这个类不会被试图解析器解析，这样方法中返回值类型是String时，那么就只会返回一个字符串

## RestFul风格

> Restful是一种软件架构风格、设计风格，而不是标准，只是提供了一组设计原则和约束条件。它主要用于客户端和服务器交互类的软件。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。 ——百度百科

第一种效果是如下图

```java
    @RequestMapping("/get/{name}/{age}")
    public String getName(@PathVariable String name, @PathVariable Integer age, Model model){
        model.addAttribute("mg",name+"的年龄为"+age);
        return "showNameAge";
    }
```

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220109120839.png)

如果使用以前的方式那么url的写法是http://localhost:8088/get/?name=zhangsan&&age=18
这种风格看起来更简洁有层次，同时也更加安全

第二种是根据不同的请求方式，即便路由地址一样但是可以执行不同的操作

```java
    @RequestMapping(value = "/operate",method = RequestMethod.GET)
    public String useGetMethod(Model model){
        model.addAttribute("msg","发起了get请求");
        return "operate";
    }
    @RequestMapping(value = "/operate",method = RequestMethod.POST)
    public String usePostMethod(Model model){
        model.addAttribute("msg","发起了post请求");
        return "operate";
    }
```

## 转发和重定向

forward：表示转发，实现request.getRequestDispatcher("xx.jsp").forward();使用forward不能和视图解析器一起使用

```java
 return "forward:/WEB-INF/pages/operate.jsp";
```

如果返回的是ModelAndView类型，则是下面的方式

```java
 modelAndView.setViewName("forward:/WEB-INF/pages/operate.jsp");
```

redirect：重定向，实现respose.sendRedirect("xx.jsp")

同样不和识图解析器一起使用，并且重定向不能访问WEB-INF下的资源文件

```java
modelAndView.setViewName("redirect:/operate.jsp");
```

并且重定向时，两次的request作用域不同，数据无法共享

## 接收参数

### 提交的域名称和处理方法的参数名一致

提交数据：http://localhost:8080/hello?name=lwl

处理方法：

```java
    @RequestMapping("/hello")
    public String hello(String name,Model model){
        model.addAttribute("msg",name);
        return "operate";
    }
```

### 提交的域名称和处理方法的参数名不一致

提交数据：http://localhost:8080/hello?username=lwl

处理方法：

```java
    @RequestMapping("/hello")
    public String hello(@RequestParam("username") String name, Model model){
        model.addAttribute("msg",name);
        return "operate";
    }
```

@RequestParam:逐个接受请求参数中，解决请求中参数名和形参名称不一样的问题

属性：1.value 请求中的参数名称

2.required 默认为true：表示请求中必须包含此参数 位置一般放到形参前面

### 提交的是一个对象

提交数据：http://localhost:8080/hello?name=zhangsan&id=3&age=15

需要先创建一个实体类包含上面的参数

```java
public class User {
    private Integer id;
    private String name;
    private Integer age;
```

处理方法：

```java
    @RequestMapping("/user")
    public String hello(User user){
        System.out.println(user);
        return "operate";
    }
```

当然提交的数据也可以少部分字段，但是不能有封装实体类没有的属性,否则该字段为null，用对象接收时，不能用@RequestParam注解

## 乱码问题

当我们在jsp上发起post请求时，经常能看到乱码问题![](https://gcore.jsdelivr.net/gh/code-anan/image/20220109183239.png)

这里我们可以选择在web.xml中注册声明过滤器

```xml
<!--声明过滤器-->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!--设置项目中使用的编码-->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <!--强制请求对象（HttpServletRequest）使用编码的值-->
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <!--强制请求对象（HttpServletResponse）使用编码的值-->
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <!--表示所有的请求都要经过过滤器过滤-->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220109183908.png)

可以看到确实起作用了

## 处理器方法的返回值

* ModelAndView：有数据和视图，对视图执行forward操作
* Stirng：一般表示视图名称，有@RestController表示一个字符串
* void ：不能表示数据也不能表示视图，在处理ajax的时候可以使用void返回值，通过HttpServletResponse输出数据
* Object：String、Integer、Map、List、User等都是对象，一般把他们作为数据用来相应ajax的请求

## Jackson的使用

### 依赖导入

```xml
 <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.13.1</version>
    </dependency>
```

### 代码使用

```java
    @RequestMapping(value = "/user",produces = "application/json;charset=utf-8")
    @ResponseBody
    public String hello() throws JsonProcessingException {
        ObjectMapper mapper = new ObjectMapper();
        User user = new User();
        user.setAge(12);
        user.setId(1);
        user.setName("张三");
        System.out.println(user);
        String value = mapper.writeValueAsString(user);
        return value;
    }
```

produces属性是为了防止json串中乱码， @ResponseBody一般和@controller配合使用，加上ResponseBody表示返回的是一个字符串不再走视图解析器

得到结果如下

```json
{"id":1,"name":"张三","age":12}
```

上面的produces注解虽然也可以解决，但是如果有多个就会变得麻烦，所以这里我们也可以在springmvc配置文件里面进行配置

```xml
<mvc:annotation-driven>
    <mvc:message-converters>
        <bean class="org.springframework.http.converter.StringHttpMessageConverter">
            <constructor-arg value="UTF-8"/>
        </bean>
        <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter">
            <property name="objectMapper">
                <bean class="org.springframework.http.converter.json.Jackson2ObjectMapperFactoryBean">
                    <property name="failOnEmptyBeans" value="false"/>
                </bean>
            </property>
        </bean>
    </mvc:message-converters>
</mvc:annotation-driven>
```

## Fastjson使用

### 依赖导入

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.79</version>
</dependency>
```

### 代码使用

把上面的代码稍作修改

```java
    @RequestMapping(value = "/user",produces = "application/json;charset=utf-8")
    @ResponseBody
    public String hello(){
        User user = new User();
        user.setAge(12);
        user.setId(1);
        user.setName("张三");
        System.out.println(user);
        return JSON.toJSONString(user);
    }
```

得到结果如下

```json
{"age":12,"id":1,"name":"张三"}
```

可以看到阿里开发的fastjson还是更符合中国人的习惯

## mvc:annotation-driven驱动

如果我们controller的返回值类型想转换成是json、xml等数据类型，必须要加上此驱动，它能够完成java对象到json、xml等数据格式的转换

```java
    @RequestMapping(value = "/user",produces = "application/json;charset=utf-8")
    @ResponseBody
    public User hello(){
        User user = new User();
        user.setAge(12);
        user.setId(1);
        user.setName("张三");
        System.out.println(user);
        return user;
    }
```

如果springmvc配置文件不添加此注解驱动

```xml
 <mvc:annotation-driven/>
```

那么会报500的错误![](https://gcore.jsdelivr.net/gh/code-anan/image/20220109201839.png)

加上之后，结果变成json形式

```json
{"id":1,"name":"张三","age":12}
```

## url-pattern的第二种值

url-pattern的值为`/`时，需要想办法处理静态资源文件

第一种方式： 配合<mvc:annotation-driven>和<mvc:default-servlet-handler>使用，这样既可以处理静态资源文件又能处理动态资源文件

处理静态资源访问还有第二种方式：<mvc:resources mapping=" " location="" >

mvc:resources加入后框架会创建ResourcesHttpRequestHandler这个处理器对象处理静态资源的访问，不依赖tomcat服务器；

mapping：访问静态资源的uri地址，可以使用通配符**

location:静态资源在项目中的目录位置

例如：<mvc:resources mapping="/images/**" location="/images/">

<mvc:resources mapping="/js/**" location="/js/">

# 整合SSM框架

SSM整合也叫作SSI（Ibatis是mybatis的前身），整合中有容器。第一个容器叫做SpringMVC容器，管理Controller控制器对象，第二个容器Spring容器，管理Service、dao工具类对象；

service、dao对象定义在spring的配置文件中，让spring管理这些对象；springmvc容器是spring容器的子容器类似于java中的继承，子可以访问父中的内容，在子容器中的Controller可以访问父容器中的service对象，就可以实现controller使用service对象

## 实现步骤

### 新建项目

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220110142643.png)

这里我们就用自带的webapp项目即可

### 添加依赖

```xml
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.1</version>
    </dependency>

    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.9</version>
    </dependency>

    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.12</version>
    </dependency>

    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.13.1</version>
    </dependency>
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>servlet-api</artifactId>
      <version>2.5</version>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.1</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
```

主要添加三个框架依赖、druid连接池，fastjson、mysql驱动还有jsp servlet依赖,spring整合mybatis依赖、jdbc依赖等，然后还有pom.xml文件中build标签下需要添加以下代码

```xml
       <resources>
        <resource>
          <directory>src/main/java</directory>
          <includes>
            <include>**/*.xml</include>
          </includes>
          <filtering>false</filtering>
        </resource>
      </resources>
```

因为maven工程在默认情况下src/main/java目录下的所有资源文件是不发布到target目录下的，不添加无法编译xml文件

### web.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--注册DispatcherServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    <!--spring的监视器-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:application.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!--注册字符集过滤器-->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

创建DispatcherServlet对象用来创建Controller类对象才能接收用户的请求；注册Spring的监听器ContextLoaderListener才能创建service、dao对象，字符集过滤器为了解决post请求可能出现的乱码问题

### 创建需要的包

创建好需要的Controller、model、dao和service的包

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220110145840.png)

controller包下的StudentController:

```java
@Controller
public class StudentController {

    @Autowired
    private StudentService studentService;


    @RequestMapping("/get")
    @ResponseBody
    public List<Student> getStudent(){
        List<Student> list = studentService.selectAllStudent();
        for(Student student:list){
            System.out.println(student);
        }
        return list;
    }
}
```

这里我们就以一个查询所有学生信息为例

dao包下的StudentDao和StudentDao.xml

```java
@Repository
public interface StudentDao {
    List<Student> queryAllStudent();
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.weilong.dao.StudentDao">
    <select id="queryAllStudent" resultType="com.weilong.model.Student">
       select * from student;
    </select>
</mapper>
```

model下的实体类对象，还是经典student类（需要跟数据库字段保持一致）

```java
public class Student {
    private int id;
    private String name;
    private int age;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

service包下的StudentService和他的实现类

```java
public interface StudentService {
    List<Student> selectAllStudent();
}

```

```java
@Service
public class studentServiceImpl implements StudentService {

    @Autowired
    private StudentDao studentDao;
    @Override
    public List<Student> selectAllStudent() {
        List<Student> list = studentDao.queryAllStudent();
        return list;
    }
}
```

### 配置文件填写

- springmvc配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

        <context:component-scan base-package="com.weilong.controller"/>
        <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
            <property name="prefix" value="/WEB-INF/pages"/>
            <property name="suffix" value=".jsp"/>
        </bean>

        <mvc:annotation-driven/>
</beans>
```

组件扫描器跟识图解析器不需要解释，创建controller和返回视图需要的，mvc:annotation-driven的作用一个是相应ajax请求返回json还有就是可以解决静态资源访问不到的问题

- application.xml(spring的配置文件)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
        <!--声明jdbc.properties文件的位置-->
        <context:property-placeholder location="classpath:jdbc.properties"/>
        <!--声明数据源，连接数据库-->
        <bean class="com.alibaba.druid.pool.DruidDataSource" id="dataSource"
        init-method="init" destroy-method="close">
            <property name="url" value="${jdbc.url}"/>
            <property name="username" value="${jdbc.username}"/>
            <property name="password" value="${jdbc.password}"/>
        </bean>
        <!--创建sqlsessionFactory-->
        <bean id="sqlSessionFactory"  class="org.mybatis.spring.SqlSessionFactoryBean">
            <property name="dataSource" ref="dataSource"/>
            <property name="configLocation" value="classpath:mybatis.xml"/>
        </bean>
        <!--创建dao对象-->
        <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
            <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
            <property name="basePackage" value="com.weilong.dao"/>
        </bean>
        <!--声明service包的位置-->
        <context:component-scan base-package="com.weilong.service"/>
</beans>
```

- mybatis.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <mappers>
        <package name="com.weilong.dao"/>
    </mappers>

</configuration>
```

这里我们需要指定映射文件的位置，直接使用包的方式更方便

- jdbc.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/lwl
jdbc.username=root
jdbc.password=233
```

数据库的信息文件，方便修改降低耦合度

### 测试结果

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220110164117.png)

# 拦截器

拦截器是AOP思想的具体应用，并且SpringMVC拦截器是SpringMVC框架自己的只有使用SpringMVC框架的工程才能使用；

拦截器只会拦截访问的控制器方法，静态资源js html等不会拦截;

拦截器是全局的，可以对多个controller进行拦截，一个项目中可以有多个拦截器，拦截器常用在：用户登录处理、权限检查、记录日志

## 使用步骤

创建一个普通类实现`HandlerInterceptor`接口，并重写他的方法

```java
@Override
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    System.out.println("=====拦截前");
    return true;
}

@Override
public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
    System.out.println("=====清理");
}

@Override
public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    System.out.println("=====拦截后");
}
```

然后在springmvc配置文件中配置拦截器

```xml
<!--配置拦截器-->
    <mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="com.weilong.interceptor.MyInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

path属性表示拦截所有目录 通过MyInterceptor这个controller类进行拦截

然后我们调用某个controller得到测试结果为：

```java
=====拦截前
Student{id=1, name='碳治郎', age=13}
Student{id=2, name='妳豆子', age=12}
Student{id=3, name='猪猪', age=14}
Student{id=4, name='善逸', age=15}
Student{id=5, name='恋柱', age=13}
=====清理
=====拦截后
```

可以看到后面两个方法是在目标方法之后执行的，所以我们要验证登陆之类的只需要在preHandle里面进行判断即可,例如：

```java
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Cookie[] cookies = request.getCookies();
        for(Cookie cookie:cookies){
            if(cookie.getName().equals("user")){
                return true;
            }
        }
        if(request.getRequestURI().contains("login")){
            return true;
        }else{
            //request.getRequestDispatcher("login").forward(request,response);
            response.sendRedirect("login");
        }
        return false;
    }
```

## 拦截器和过滤器的区别

* 过滤器是servlet中的对象，拦截器是框架中的对象
* 过滤器实现Filter接口，拦截器实现HandlerInterceptor接口
* 过滤器用来设置request、response的参数属性，侧重对数据的过滤；拦截器是用来验证请求，能截断请求
* 过滤器在拦截器之前执行
* 过滤器可以处理jsp html等；拦截器拦截Controller的对象

# SpringMVC执行原理
1. 用户发起一个请求
2. DispatcherServlet接收请求并把请求转交给处理器映射器
   处理器映射器：Springmvc框架中的一种对象，框架把实现了HandlerMapping的类都叫做映射器（可以有多个）
   处理器映射器作用：根据请求， 从Springmvc容器对象中获取处理器对象，框架把找到的处理器对象放到一个叫做处理器执行链的类（HandlerExecutionChain）保存
   HandlerExecutionChain：类中保存着处理器对象和项目中所有的拦截器
3. DispatcherServlet把HandlerExecutionChain中的处理器对象都交给处理器适配器（可以有多个）
   处理器适配器：Springmnvc框架中的对象，需要实现HandlerAdapter接口
   处理器适配作用：执行处理器方法获取返回值
4. DispatcherServlet把适配器返回来的值比如ModelAndview交给视图解析器对象
   视图解析器：Springmvc框架中的对象，需要实现viewResolver接口
   视图解析器作用：组成视图完整路径，使用前缀后缀并创建view对象，view是一个接口表示视图，在框架中jsp、html不是String表示，而是使用view和他的实现类表示视图
   InternalResourceView：视图类，表示jsp文件，视图解析器会创建InternalResourceView类对象，这个对象里面有一个属性url
5. DispatcherServlet把创建的view对象获取到调用view类自己的方法，把model数据放入到request作用域，执行视图对象的forward，请求结束
  流程图如下：![](https://gcore.jsdelivr.net/gh/code-anan/image/20220115124658.png)

# 文件上传与下载

## 添加依赖

```xml
<dependency>
  <groupId>commons-fileupload</groupId>
  <artifactId>commons-fileupload</artifactId>
  <version>1.4</version>
</dependency>
<dependency>
  <groupId>javax.servlet</groupId>
  <artifactId>javax.servlet-api</artifactId>
  <version>4.0.1</version>
</dependency>
```

注意这里需要引入高版本的servlet依赖把低版本的删掉，否则缺少方法

## 前段页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>文件上传</title>
</head>
<body>
<form action="/upload" enctype="multipart/form-data" method="post">
    <input type="file" name="file">
    <input type="submit" value="上传">
</form>
</body>
</html>
```

只要有个文件上传功能即可

## Springmvc配置文件

```xml
<!--文件上传配置-->
    <bean class="org.springframework.web.multipart.commons.CommonsMultipartResolver" id="commonsMultipartResolver">
        <!--请求的编码格式，需要和前段页面保持一致，默认为ISO-8859-1-->
        <property name="defaultEncoding" value="utf-8"/>
        <!--上传文件大小上限，单位是字节，（1M=10485760）-->
        <property name="maxUploadSize" value="10485760"/>
        <property name="maxInMemorySize" value="40960"/>
     </bean>
```

## 接收处理

上传文件：

```java
@RestController
public class FileController {
    //@RequestParam("file")把name=file的控件封装成CommonsMultipartFile对象
    @RequestMapping("/upload")
    public String uploadFile(@RequestParam("file") CommonsMultipartFile file, HttpServletRequest request) throws IOException {
        //获取文件名
        String filename = file.getOriginalFilename();

        //判断文件名是否为空
        if("".equals(filename)){
            return "redirect:/index.jsp";
        }
        System.out.println("上传文件名"+filename);

        //上传路径保存设置
        String path = request.getServletContext().getRealPath("/upload");
        //如果路径不存在，创建一个
        File realFile = new File(path);
        if(!realFile.exists()){
            realFile.mkdir();
        }
        System.out.println("上传文件路径："+realFile);
        //获取输入输出流
        InputStream inputStream = file.getInputStream();
        OutputStream outputStream = new FileOutputStream(new File(realFile, filename));

        //读文件
        int length=0;
        byte[] bytes = new byte[1024];
        while ((length=inputStream.read(bytes))!=-1){
            outputStream.write(bytes,0,length);
            outputStream.flush();
        }
        inputStream.close();
        outputStream.close();
        return "redirect:/index.jsp";
    }
}
```

下载文件：

```java
    @RequestMapping("/download")
    public String download(HttpServletRequest request, HttpServletResponse response,@RequestParam("filename") String filename) throws IOException {
        //要下载的图片地址
        String path = request.getServletContext().getRealPath("/upload");


        //设置response响应头
        response.reset(); //设置页面不缓存
        response.setCharacterEncoding("utf-8");
        response.setContentType("multipart/form-data");

        response.setHeader("Content-Disposition","attatchment;fileName="+ URLEncoder.encode(filename,"UTF-8"));
        File file = new File(path, filename);
        //读取文件
        InputStream input = new FileInputStream(file);
        //写出文件
        OutputStream output = response.getOutputStream();

        //读文件
        int length=0;
        byte[] bytes = new byte[1024];
        while ((length=input.read(bytes))!=-1){
            output.write(bytes,0,length);
            output.flush();
        }
        input.close();
        output.close();
        return "ok";
    }
```

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220116201701.png)

好了，Springmvc差不多就这些结束(￣▽￣)~*

