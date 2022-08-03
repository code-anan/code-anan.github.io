title: SpringBoot学习
author: lwl
date: 2022-01-23 14:33:18
tags:
  - SpringBoot
  - 框架
categories:
  - SpringBoot
cover: https://gcore.jsdelivr.net/gh/code-anan/image/20220617204702.png
my: post/springbootkeys
---
# Springboot启动图标

这里分享SpringBoot启动图标修改方式![](https://gcore.jsdelivr.net/gh/code-anan/image/20220123180255.png)

其实也非常简单，首先创建一个SpringBoot项目(Spring Initializer)然后在`resources`目录下创建一个`banner.txt`文件即可，文件中的内容就可以自定义我们的展示内容，上图展示的效果可以在[这里](https://www.bootschool.net/ascii-art)进行搜索

# 原理初探

## 启动器

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
</dependency>
```

* 启动器可以理解为SpringBoot的启动场景
* spring-boot-starter-web启动器会帮我们自动导入web环境需要的所有依赖
* springboot可以把特有的功能场景都变成一个个的启动器
* 更多启动器可以到[官网](https://docs.spring.io/spring-boot/docs/current/reference/html/using.html#using.build-systems.starters)找到

## 主程序

* 注解 

```java
@SpringBootConfiguration:springboot的配置
    @Configuration ：spring的配置类
    @Component： 说明他也是spring的一个组件

@EnableAutoConfiguration：自动配置
    @AutoConfigurationPackage：自动装配的包
    @Import({AutoConfigurationImportSelector.class})：自动配置选择导入
```
根据这些注解查看源码可以看到资源是如何加载到配置类种，具体可以看[狂神的视频](https://www.bilibili.com/video/BV1PE411i7CV?p=6&spm_id_from=pageDriver)

> 结论：springboot所有自动配置都是在启动的时候扫描并加载`spring.factories`,所有的自动配置类都在里面，但是不一定生效，要判断条件是否成立，只要导入了对应的start，就有对应的启动器，有了启动器装配就会生效，然后就会配置成功

## 主启动类SpringApplication

我们知道项目启动是因为main方法中执行了SpringApplication.run方法，这个类主要做了下面四个事情：

1. 推断应用的类型是普通的项目还是web项目
2. 查找并加载所有可用初始化器，设置到initializers属性中
3. 找出所有的应用程序监听器，设置到listeners属性中
4. 推断并设置main方法的定义类，找到运行的主类

# yaml语法

我们知道springboot默认的配置文件叫做`application.properties`,但其实还可以给他改后缀为`yaml`或者`yml`，同时还增添了一些新的语法

例如我们`application.yaml`中定义一些属性给Person类赋值

```yaml
person:
  name: lwl
  age: 3
  list:
    - java
    - css
    - html
  map: {k1: v1 ,k2: v2}
  dog:
    name: 旺财
    age: 12
```

Person类种使用@ConfigurationProperties()注解即可

```java
@Data
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private int age;
    private List<Object> list;
    private Map map;
    private Dog dog;
}
```

在测试类中输出得到结果为

```java
Person(name=lwl, age=3, list=[java, css, html], map={k1=v1, k2=v2}, dog=Dog(name=旺财, age=12))
```

除了使用@ConfigurationProperties注解，我们还可以使用@PropertySource注解进行赋值

```java
@Data
@Component
//@ConfigurationProperties(prefix = "person")
@PropertySource(value = "classpath:my.properties")
public class Person {
    @Value("${name}")
    private String name;
    private int age;
    private List<Object> list;
    private Map map;
    private Dog dog;
}
```

这种方式是读取我们自己定义的my.properties中的属性，除此之外我们还可以直接使用`application.yaml`中的属性，例如我们定义一个name,在实体类中不使用注解直接引入

```java
@Data
@Component
//@ConfigurationProperties(prefix = "person")
//@PropertySource(value = "classpath:my.properties")
public class Person {
    @Value("${name}")
    private String name;
    private int age;
    private List<Object> list;
    private Map map;
    private Dog dog;
}
```

同样可以赋值，这种方式使用的最多常常是项目的全局配置我们会这样使用；通过上面的例子我们可以很清楚的知道yaml语法的使用以及需要注意的点

#多环境配置及配置文件位置

springboot项目配置文件默认是放在`resources`目录下的，其实这个配置文件还可以下图所示的位置并且他们的优先级也跟数字标注的一样![](https://gcore.jsdelivr.net/gh/code-anan/image/20220124190130.png)

企业开发我们经常会使用多种环境，这时候我们就可以创建多个`application.properties`文件，都必须以application开头中间用-  例如下面并且通过`spring.profiles.active=`指定使用哪一个配置文件![](https://gcore.jsdelivr.net/gh/code-anan/image/20220124190854.png)或者是使用`application.yaml`的这种方式

```yaml
server:
  port: 8080
spring:
  profiles:
    active: dev
---
server:
  port: 8081
spring:
  profiles: dev
---
server:
  port: 8082
spring:
  profiles: test
```

中间使用---分开 然后`spring-profiles-active`选择使用哪个环境 不过这种方式现在不推荐了

# Web开发

## 静态资源导入探究

在springboot中，默认使用以下方式处理静态资源

* webjars   访问路径为localhost:8080/webjars
* public,static,/**,resources    访问路径为localhost:8080

优先级resouces>static(默认)>public，源码讲解[地址](https://www.bilibili.com/video/BV1PE411i7CV?p=14)

## 首页和图标

若想把首页以及其他页面放到templates下，需要手动添加`Thymeleaf`的依赖

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
            <version>2.6.2</version>
</dependency>
```

网站图标的话springboot2.2版本以后就不知道添加默认小图标`favicon.cio`的方式了，所以我们只能手动添加，以`Thymeleaf`模板引擎为例

```html
 <link rel="shortcut icon" href="/images/favicon.ico">
```

我们还可以在[这里](https://tool.lu/favicon/)在线制作小图标

## Thymeleaf

依赖：

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
            <version>2.6.2</version>
        </dependency>
```

基本模板：

```html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">

  <head>
    <title>Good Thymes Virtual Grocery</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <link rel="stylesheet" type="text/css" media="all" 
          href="../../css/gtvg.css" th:href="@{/css/gtvg.css}" />
  </head>

  <body>
  
    <p th:text="#{home.welcome}">Welcome to our grocery store!</p>
  
  </body>

</html>
```

更多详细使用教程可以在[官网]([Tutorial: Using Thymeleaf](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#using-texts))进行查看，使用最多也就`th:each`,`th:text`等

# 简易员工管理系统

## 国际化

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220223194220.png)

找到`MessageSourceAutoConfiguration`类下的![](https://gcore.jsdelivr.net/gh/code-anan/image/20220223194410.png)

我们就可以在配置文件(application.properties)中设置路径

```properties
## 说明配置文件的位置
spring.messages.basename=i18n/login
```

这样就可以在前段页面进行绑定，使用#![](https://gcore.jsdelivr.net/gh/code-anan/image/20220223195110.png)

刷新页面可以看到发生了变化![](https://gcore.jsdelivr.net/gh/code-anan/image/20220223195315.png)

下面需要动态的改变他里面的值，首先需要定义一个类实现`LocaleResolver`类并且重写他的两个方法

```java
public class MyLocaleResolver implements LocaleResolver {

    //解析请求
    @Override
    public Locale resolveLocale(HttpServletRequest request) {
        String language = request.getParameter("language");
        Locale locale = Locale.getDefault();
        if(!StringUtils.isEmpty(language)){
            String[] split = language.split("_");
            locale = new Locale(split[0], split[1]);
        }
        return locale;
    }

    @Override
    public void setLocale(HttpServletRequest request, HttpServletResponse response, Locale locale) {

    }
}
```

然后在容器中注入该组件，使用Bean注解

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
       registry.addViewController("/").setViewName("index");
       registry.addViewController("/index.html").setViewName("index");
       registry.addViewController("/login").setViewName("index");
    }

    @Bean
    public LocaleResolver localeResolver(){
        return new MyLocaleResolver();
    }

}
```

前段动态的给他传值

```html
<a class="btn btn-sm" th:href="@{/index.html(language='zh_CN')}">中文</a>
<a class="btn btn-sm" th:href="@{/index.html(language='en_US')}">English</a>
```

效果![](https://gcore.jsdelivr.net/gh/code-anan/image/20220321202806.png)

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220321202827.png)

## 登陆拦截器

还是先创建一个类实现`HandlerInterceptor`

```java
public class LoginHandlerInteceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        Object loginName = request.getSession().getAttribute("loginName");
        if(loginName==null){
            request.setAttribute("msg","请先进行登录");
            request.getRequestDispatcher("/index.html").forward(request,response);
            return false;
        }else{
            return true;
        }
    }
}
```

添加拦截器组件到容器

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
       registry.addViewController("/").setViewName("index");
       registry.addViewController("/index.html").setViewName("index");
       registry.addViewController("/main.html").setViewName("dashboard");
    }

    @Bean
    public LocaleResolver localeResolver(){
        return new MyLocaleResolver();
    }

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginHandlerInteceptor()).
                addPathPatterns("/**")
                .excludePathPatterns("/","/index.html","/login"
                  ,"/css/**","/js/**","/img/**");
    }
}
```

# 整合JDBC使用

首先还是依赖的导入

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
```

然后是数据的基本配置

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=233
spring.datasource.url=jdbc:mysql://localhost:3306/test?serverTimeZone=UTC
```

通过`JdbcTemplate`(Springboot帮我们默认生成的一个类我们就可以完成基本的增删改查操作，在autoconfiguration下的jdbc目录下)

```java
@RestController
public class JdbcController {

    @Resource
    JdbcTemplate jdbcTemplate;

    @GetMapping("/userlist")
    @ResponseBody
    public List getuserlist(){
        String sql="select * from student";
        List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql);
        return maps;
    }


    @GetMapping("/addstudent")
    @ResponseBody
    public Map adduserlist(){
        String sql="insert into student(name) values ('艾伦耶格尔')";
        int i = jdbcTemplate.update(sql);
        HashMap<Object, Object> map=null;
        if (i>0){
             map = new HashMap<>();
            map.put("result","插入成功");
        }else {
            map = new HashMap<>();
            map.put("result","插入失败");
        }
        return map;
    }
}

```

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220606200633.png)

可以看到确实有效

# 整合Druid数据源

依赖导入

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.2.8</version>
</dependency>
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

在配置文件中添加使用druid连接池

```properties
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: 233
    url: jdbc:mysql://localhost:3306/test?serverTimeZone=UTC
    type: com.alibaba.druid.pool.DruidDataSource
    #SpringBoot默认是不注入这些的，需要自己绑定
    #druid数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
    #配置监控统计拦截的filters，stat：监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许报错，java.lang.ClassNotFoundException: org.apache.Log4j.Properity
    #则导入log4j 依赖就行
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionoProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500
```

声明一个配置类与配置文件相绑定：

```java
@Configuration
public class DruidConfig {

    @ConfigurationProperties(prefix = "spring.datasource")
    @Bean
    public DataSource druidDataSource(){
        return new DruidDataSource();
    }

    //后台监控
    //springboot内置了servlet 所以没有web.xml 替代方法ServletRegistrationBean
    @Bean
    public ServletRegistrationBean statViewServlet(){
        ServletRegistrationBean<StatViewServlet> bean = new ServletRegistrationBean<>(new StatViewServlet(), "/druid/*");
        //后台登录的账号密码
        HashMap<String, String> initparams = new HashMap<>();
        initparams.put("loginUsername","admin");//key是固定的
        initparams.put("loginPassword","123");

        //设置谁可以访问
        initparams.put("allow","");//为空表示谁都可以访问

        bean.setInitParameters(initparams);
        return bean;
    }

    //过滤器
    public FilterRegistrationBean webStatFilter(){
        FilterRegistrationBean<Filter> bean = new FilterRegistrationBean<>();
        bean.setFilter(new WebStatFilter());
        //设置过滤哪些请求
        HashMap<String, String> initparams = new HashMap<>();
        //这些东西不进行统计
        initparams.put("exclusions","*.js,*.css,/druid/*");//
        bean.setInitParameters(initparams);
        return bean;
    }
}
```

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220606211200.png)

这样就可以看到druid自带的监控后台了

# 整合mybatis框架

依赖引入

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.2.0</version>
</dependency>
```

基本配置

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=233
spring.datasource.url=jdbc:mysql://localhost:3306/test?serverTimeZone=UTC
```

声明接口

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220607200300.png)

使用@Mapper或者加上@Repository注解  或者在启动类中使用@MapperScan都可以

然后创建对应的`StudentMapper.xml`文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.weilong.mybatis.dao.StudentMapper">

    <select id="queryStudentList" resultType="Student">
    select * from student ;
    </select>


    <select id="queryStudentByid" resultType="Student">
      select * from student where id=${id};
    </select>

    <update id="updateStudent" >
      update stduent set id=#{id},name=#{name}
    </update>

    <delete id="deleteStudent">
        delete from student where id=#{id}
    </delete>

</mapper>
```

因为返回结果是实体类  所以需要在application.properties文件中声明

```properties
##声明mybatis实体类的别名以及mapper的位置
mybatis.type-aliases-package=com.weilong.mybatis.model
mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
```

声明完成可以发现上面没有爆红了，然后简单写个controller测试一下

```java
@RestController
public class MyController {

    @Resource
    private StudentMapper studentMapper;

    @GetMapping("/gerStudentlist")
    public List gerStudentlist(){
        List<Student> list = studentMapper.queryStudentList();
        return list;
    }
}
```

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220607203417.png)

发现正常运行

# SpringSecurity和shiro

## SpringSecurity

[Spring Security](https://docs.spring.io/spring-security/site/docs/3.1.x/reference/springsecurity.html)是针对Spring项目的安全框架，也是SpringBoot底层安全模块默认的技术选型，他可以实现强大的web安全机制，对于安全控制，我们仅需要引入spring-boot-starter-security模块，进行少量的配置，即可实现强大的安全管理！

重要的几个类：

* WebSecurityConfigurerAdapter:自定义Security策略
* AuthenticationManagerBuilder:自定义认证策略
* @EnableWebSecurity:开启WebSecurity模式

Spring Security的两个主要目标是认证(Authentication)和授权(Authorization)

### 认证和授权

方便演示需要使用到一些素材，下载地址[点这里](https://gitee.com/ENNRIAAA/spring-security-material?_from=gitee_search)把static和template下的内容放到我们新建的springboot对应的目录下即可，然后是导入依赖

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
            <version>2.6.7</version>
        </dependency>
```

然后是路由跳转的controller

```java
@Controller
public class RouteController {

    @GetMapping("/toLogin")
    public String tologin(){
        return "views/login";
    }

    @GetMapping("/level1/{id}")
    public String tolevel1(@PathVariable("id")int id){
        return "views/level1/"+id;
    }
    @GetMapping("/level2/{id}")
    public String tolevel2(@PathVariable("id")int id){
        return "views/level2/"+id;
    }
    @GetMapping("/level3/{id}")
    public String tolevel3(@PathVariable("id")int id){
        return "views/level3/"+id;
    }
}
```

创建我们的配置类即可简单的实现认证和授权的操作

```java
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //设置首页所有人可以访问，功能页只有对应权限的人才可以访问
        //请求授权的规则
        http.authorizeRequests().antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");


        //没有权限默认会到登录页面
        http.formLogin();
    }

    //认证
    @Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
       auth.inMemoryAuthentication().passwordEncoder(new BCryptPasswordEncoder())
               .withUser("lwl").password(new BCryptPasswordEncoder().encode("123")).roles("vip2","vip3")
               .and()
               .withUser("root").password(new BCryptPasswordEncoder().encode("123")).roles("vip1","vip2","vip3")
               .and()
               .withUser("guest").password(new BCryptPasswordEncoder().encode("123")).roles("vip1");
    }
}
```

这里我们创建三个可以登录的用户保存在内存中并给他们赋予对应的权限以及每个角色可以访问的页面![](https://gcore.jsdelivr.net/gh/code-anan/image/20220608205833.png)这样一个简单的认证授权操作就完成了

### 注销和权限控制

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220608211115.png)

在这里开启并且在页面上添加注销路由即可

```html
<!--未登录-->
                <a class="item" th:href="@{/toLogin}">
                    <i class="address card icon"></i> 登录
                </a>
                <a class="item" th:href="@{/logout}">
                    <i class="sign out icon"></i> 退出
                </a>
```

然后我们想要在前端页面展示一下登录的角色

pom中添加一下依赖

```xml
<dependency>
    <groupId>org.thymeleaf.extras</groupId>
    <artifactId>thymeleaf-extras-springsecurity5</artifactId>
    <version>3.0.4.RELEASE</version>
</dependency>
```

稍微修改index页面

```html
<!--未登录-->
                <div sec:authorize="!isAuthenticated()">
                    <a class="item" th:href="@{/toLogin}">
                        <i class="address card icon"></i> 登录
                    </a>
                </div>

                <div sec:authorize="isAuthenticated()">
                        用户名：<span sec:authentication="name"></span>
                        角色：<span sec:authentication="principal.authorities"></span>
                </div>
                <div sec:authorize="isAuthenticated()">
                    <a class="item" th:href="@{/logout}">
                        <i class="sign out icon"></i> 退出
                    </a>
                </div>
<div class="column" sec:authorize="hasRole('vip1')">
                <div class="ui raised segment">
                    <div class="ui">
                        <div class="content">
                            <h5 class="content">Level 1</h5>
                            <hr>
                            <div><a th:href="@{/level1/1}"><i class="bullhorn icon"></i> Level-1-1</a></div>
                            <div><a th:href="@{/level1/2}"><i class="bullhorn icon"></i> Level-1-2</a></div>
                            <div><a th:href="@{/level1/3}"><i class="bullhorn icon"></i> Level-1-3</a></div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="column" sec:authorize="hasRole('vip2')">
                <div class="ui raised segment">
                    <div class="ui">
                        <div class="content">
                            <h5 class="content">Level 2</h5>
                            <hr>
                            <div><a th:href="@{/level2/1}"><i class="bullhorn icon"></i> Level-2-1</a></div>
                            <div><a th:href="@{/level2/2}"><i class="bullhorn icon"></i> Level-2-2</a></div>
                            <div><a th:href="@{/level2/3}"><i class="bullhorn icon"></i> Level-2-3</a></div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="column" sec:authorize="hasRole('vip3')">
                <div class="ui raised segment">
                    <div class="ui">
                        <div class="content">
                            <h5 class="content">Level 3</h5>
                            <hr>
                            <div><a th:href="@{/level3/1}"><i class="bullhorn icon"></i> Level-3-1</a></div>
                            <div><a th:href="@{/level3/2}"><i class="bullhorn icon"></i> Level-3-2</a></div>
                            <div><a th:href="@{/level3/3}"><i class="bullhorn icon"></i> Level-3-3</a></div>
                        </div>
                    </div>
                </div>
            </div>
```

然后我们就可以看到不同的账户显示不同的内容了

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220608214817.png)

### 记住我和首页定制

上面有一个很大的问题就是我们自己写的登录没有使用到，下面我们要使用他自己带的登录按钮以及记住我选项，首先是登录页的小修改

```html
<div class="ui placeholder segment">
            <div class="ui column very relaxed stackable grid">
                <div class="column">
                    <div class="ui form">
                        <form th:action="@{/login}" method="post">
                            <div class="field">
                                <label>Username</label>
                                <div class="ui left icon input">
                                    <input type="text" placeholder="Username" name="username">
                                    <i class="user icon"></i>
                                </div>
                            </div>
                            <div class="field">
                                <label>Password</label>
                                <div class="ui left icon input">
                                    <input type="password" name="password">
                                    <i class="lock icon"></i>
                                </div>
                            </div>
                            <div class="field">
                                <input type="checkbox" name="remember">记住我
                            </div>
                            <input type="submit" class="ui blue submit button"/>
                        </form>
                    </div>
                </div>
            </div>
        </div>
```

这里一个是表单提交的地址以及增加记住我选项，在security配置类中

```java
 @Override
    protected void configure(HttpSecurity http) throws Exception {
        //设置首页所有人可以访问，功能页只有对应权限的人才可以访问
        //请求授权的规则
        http.authorizeRequests().antMatchers("/").permitAll()
                .antMatchers("/level1/**").hasRole("vip1")
                .antMatchers("/level2/**").hasRole("vip2")
                .antMatchers("/level3/**").hasRole("vip3");


        //没有权限默认会到登录页面
        http.formLogin().loginPage("/toLogin").usernameParameter("username").passwordParameter("password").loginProcessingUrl("/login");

        //在这里开启注销
        http.csrf().disable(); //关闭csrf
        http.logout().logoutSuccessUrl("/");

        http.rememberMe().rememberMeParameter("remember");
    }
```

需要注意的是loginPage指的是没有权限的时候我们访问有权限的页面会跳转的页面，后面的loginProcessingUrl表示使用security的登录功能（注意是功能不是页面）是用到的url，我们上面自己写的登录页的提交也是此地址，然后添加两个参数username和password以及下面 http.rememberMe().rememberMeParameter("remember");是security的登录功能添加记住我参数的设置![](https://gcore.jsdelivr.net/gh/code-anan/image/20220608222259.png)

到此 这个小demo的功能就算真正写完了；

## Shiro

### 简单介绍

[Shiro](https://shiro.apache.org/) Java 的一个`安全框架`， 可以帮助我们完成：认证、授权、加密、会话管理、与 Web 集成、缓存等；它相当简单，对比 Spring Security，可能没有 Spring Security 做的功能强大，但是在实际工作时可能并不需要那么复杂的东西，所以使用小而简单的 Shiro 就足够了；基本架构![](https://gcore.jsdelivr.net/gh/code-anan/image/20220609205044.png)

**Subject**：主体，**代表了当前 “用户”**，这个用户不一定是一个具体的人，与当前应用交互的任何东西都是 Subject，如网络爬虫，机器人等；即一个抽象概念；所有 Subject 都绑定到 SecurityManager，与 Subject 的所有交互都会委托给 SecurityManager；可以把 Subject 认为是一个门面；SecurityManager 才是实际的执行者；

**SecurityManager**：**安全管理器**；即所有与安全有关的操作都会与 SecurityManager 交互；且它管理着所有 Subject；可以看出它是 Shiro 的核心，它负责与后边介绍的其他组件进行交互，如果学习过 SpringMVC，你可以把它看成 DispatcherServlet 前端控制器；

**Realm**：**域，Shiro 从 Realm 获取安全数据（如用户、角色、权限）**，就是说 SecurityManager 要验证用户身份，那么它需要从 Realm 获取相应的用户进行比较以确定用户身份是否合法；也需要从 Realm 得到用户相应的角色 / 权限进行验证用户是否能进行操作；可以把 Realm 看成 DataSource，即安全数据源。

### SpringBoot整合Shiro

首先依然是依赖导入

```xml
        <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
			<version>2.6.7</version>
		</dependency>
		<dependency>
			<groupId>org.apache.shiro</groupId>
			<artifactId>shiro-spring-boot-starter</artifactId>
			<version>1.8.0</version>
		</dependency>
```

然后是配置类的声明

```java
@Configuration
public class ShiroConfig {

    //创建shirofilterFactorybean
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("webSecurityManager") DefaultWebSecurityManager defaultWebSecurityManager){
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
        bean.setSecurityManager(defaultWebSecurityManager);
        return bean;
    }

    //创建DefaultWebSecurityManager

    @Bean(name="webSecurityManager")
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("relam") UserRelam userRelam){
        DefaultWebSecurityManager webSecurityManager = new DefaultWebSecurityManager();
        webSecurityManager.setRealm(userRelam);
        return webSecurityManager;
    }
    //创建relam对象
    @Bean(name = "relam")
    public UserRelam getUserRelam(){
        return new UserRelam();
    }
}

```

和UserRelam类

```java
public class UserRelam extends AuthorizingRealm {

    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        return null;
    }
}

```



这里只是一个基本的架子

### 实现登录拦截

```java
//创建shirofilterFactorybean
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("webSecurityManager") DefaultWebSecurityManager defaultWebSecurityManager) {
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
        bean.setSecurityManager(defaultWebSecurityManager);

        //shiro的内置过滤器
        /*
         * anon:无需认证就可以访问
         * authc：必须认证才可以访问
         * user：必须拥有 记住我 功能才可以用
         * perms：拥有对某个资源的权限才能访问
         * role：拥有某个角色权限才可以fangwen
         * */
        Map<String, String> filterMap = new LinkedHashMap<>();
        //filterMap.put("/user/add","authc");
        // filterMap.put("/user/update","authc");
        filterMap.put("/user/*", "authc");
        bean.setFilterChainDefinitionMap(filterMap);

        //设置登录拦截
        bean.setLoginUrl("/tologin");
        return bean;
    }
```

当用户想要访问user下的内容必须要进行认证才可以 否则跳转到登录页（这里省略）

### 实现用户认证

首先是一个简陋的登录页

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>登录页</title>
</head>
<body>
<p th:text="${msg}" style="color: darkred"></p>
<form th:action="@{/login}">
    <p>用户名:<input type="text" name="username"></p>
    <p>密码:<input type="password" name="password"></p>
    <p><input type="submit" value="登录"></p>
</form>
</body>
</html>
```

登录需要用到的Controller

```java
@Controller
public class MyController {

    @GetMapping({"/" ,"/index"})
    public String toindex(Model model){
        model.addAttribute("msg","我是首页");
        return "index";
    }

    @GetMapping("/user/add")
    public String toadd(){
        return "user/add";
    }

    @GetMapping("/user/update")
    public String update(){
        return "user/update";
    }

    @GetMapping("/tologin")
    public String tologin(){
        return "login";
    }

    @RequestMapping("/login")
    public String login(@RequestParam("username") String name,@RequestParam("password")String password,Model model){
        //获取当前的用户
        Subject subject = SecurityUtils.getSubject();
        //封装用户的登陆数据
        UsernamePasswordToken token = new UsernamePasswordToken(name, password);
        try {
            subject.login(token);
            return "index";
        } catch (UnknownAccountException e) {
            model.addAttribute("msg","用户名不存在");
            return "login";
        }catch (IncorrectCredentialsException e){
            model.addAttribute("msg","密码错误");
            return "login";
        }
    }
}
```

subject.login登录成功之后我们UserRelam中的authenticationToken也就会赋值上去了

```java
//认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {

        String name="lwl";
        String password="233";
        UsernamePasswordToken usernamePasswordToken= (UsernamePasswordToken) authenticationToken;
        if(!usernamePasswordToken.getUsername().equals(name)){
            return null;
        }
        return new SimpleAuthenticationInfo("",password,"");
    }
```

### Shiro集成mybatis

首先是再导入依赖 这里也包含了druid连接池的依赖

```xml
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
</dependency>
<dependency>
	<groupId>log4j</groupId>
	<artifactId>log4j</artifactId>
	<version>1.2.17</version>
</dependency>
<dependency>
	<groupId>com.alibaba</groupId>
	<artifactId>druid</artifactId>
	<version>1.2.8</version>
</dependency>
<dependency>
	<groupId>org.mybatis.spring.boot</groupId>
	<artifactId>mybatis-spring-boot-starter</artifactId>
	<version>2.2.2</version>
</dependency>
```

首先resources下新建一个`application.yml`配置druid连接池的配置

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: 233
    url: jdbc:mysql://localhost:3306/test?serverTimeZone=UTC
    type: com.alibaba.druid.pool.DruidDataSource
    #SpringBoot默认是不注入这些的，需要自己绑定
    #druid数据源专有配置
    initialSize: 5
    minIdle: 5
    maxActive: 20
    maxWait: 60000
    timeBetweenEvictionRunsMillis: 60000
    minEvictableIdleTimeMillis: 300000
    validationQuery: SELECT 1 FROM DUAL
    testWhileIdle: true
    testOnBorrow: false
    testOnReturn: false
    poolPreparedStatements: true
    #配置监控统计拦截的filters，stat：监控统计、log4j：日志记录、wall：防御sql注入
    #如果允许报错，java.lang.ClassNotFoundException: org.apache.Log4j.Properity
    #则导入log4j 依赖就行
    filters: stat,wall,log4j
    maxPoolPreparedStatementPerConnectionSize: 20
    useGlobalDataSourceStat: true
    connectionoProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=500

```

然后在`application.properties`文件中配置mybatis相关

```properties
##声明mybatis实体类的别名以及mapper的位置
mybatis.type-aliases-package=com.example.demo.model
mybatis.mapper-locations=classpath:mybatis/mapper/*.xml
```

然后是对应咱们数据库的实体类的声明

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Student {
    private Integer id;
    private String name;
    private String password;
}
```

以及mapper接口的声明以及对应xml的声明

```java
@Mapper
public interface StudentMapper {

    Student queryStudentByName(String name);
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.demo.mapper.StudentMapper">

    <select id="queryStudentByName" resultType="Student">
                select * from student where name=#{name};
    </select>

</mapper>
```

最后修改我们UserRelam的逻辑代码即可

```java
public class UserRelam extends AuthorizingRealm {

    @Resource
    StudentMapper studentMapper;
    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {

        UsernamePasswordToken usernamePasswordToken= (UsernamePasswordToken) authenticationToken;
        Student student = studentMapper.queryStudentByName(usernamePasswordToken.getUsername());
        if(student==null){
            return null;  //这里表示没有此用户
        }
        return new SimpleAuthenticationInfo("",student.getPassword(),"");
    }
}
```

测试之后发现成功了！

### Shiro请求授权

首先给我们的用户添加一个权限的字段（实体类也要添加），如下![](https://gcore.jsdelivr.net/gh/code-anan/image/20220614214855.png)

shiro的配置类中设置权限

```java
//创建shirofilterFactorybean
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("webSecurityManager") DefaultWebSecurityManager defaultWebSecurityManager) {
        ShiroFilterFactoryBean bean = new ShiroFilterFactoryBean();
        bean.setSecurityManager(defaultWebSecurityManager);

        //shiro的内置过滤器
        /*
         * anon:无需认证就可以访问
         * authc：必须认证才可以访问
         * user：必须拥有 记住我 功能才可以用
         * perms：拥有对某个资源的权限才能访问
         * role：拥有某个角色权限才可以fangwen
         * */
        Map<String, String> filterMap = new LinkedHashMap<>();
        //filterMap.put("/user/add","authc");
        // filterMap.put("/user/update","authc");
        filterMap.put("/user/add","perms[add]");
        filterMap.put("/user/update", "perms[update]");
        bean.setFilterChainDefinitionMap(filterMap);

        //设置登录拦截
        bean.setLoginUrl("/tologin");
        //设置未授权跳转页面
        bean.setUnauthorizedUrl("/unauthorize");
        return bean;
    }
```

到add目录需要有add的权限、到upadte目录需要有update的权限，然后在relam类中修改代码

```java
public class UserRelam extends AuthorizingRealm {

    @Resource
    StudentMapper studentMapper;
    //授权
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {

        SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();

        Student principal = (Student) SecurityUtils.getSubject().getPrincipal();
        info.addStringPermission(principal.getPemers());
        return info;
    }

    //认证
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {

        UsernamePasswordToken usernamePasswordToken= (UsernamePasswordToken) authenticationToken;
        Student student = studentMapper.queryStudentByName(usernamePasswordToken.getUsername());
        if(student==null){
            return null;  //这里表示没有此用户
        }
        return new SimpleAuthenticationInfo(student,student.getPassword(),"");
    }
}
```

需要注意的认证的代码 我们返回的SimpleAuthenticationInfo中给第一个属性Principal赋值了 这样我们才可以在上面拿到用户信息，然后给用户添加他们在数据库中有的权限、例如我们的root用户只有add权限

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220614215257.png)

那么他就无法访问更新目录下的资源

### Shiro整合thymeleaf

上面我们做完了给用户授权的操作，但是前端页面还是会两个功能按钮都展示 我们想要给用户只展示他有的权限进行展示，所以这里需要整合一下thymeleaf

首先是依赖

```xml
<dependency>
    <groupId>com.github.theborakompanioni</groupId>
    <artifactId>thymeleaf-extras-shiro</artifactId>
    <version>2.1.0</version>
</dependency>
```

然后在我们的shiro的配置类中添加如下bean

```java
@Bean
    public ShiroDialect getShiroDialect(){
        return new ShiroDialect();
    }
```

最后稍微修改我们的前端页面接口

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.w3.org/1999/xhtml" xmlns:shiro="http://www.pollix.at/thymeleaf/shiro">>
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
<div th:text="${msg}"></div>
<div shiro:notAuthenticated="true">
<a th:href="@{/tologin}">登录</a>
</div>
<div shiro:hasPermission="add">
<a th:href="@{/user/add}">添加</a>
</div>
<div shiro:hasPermission="update">
<a th:href="@{/user/update}">更新</a>
</div>
</body>
</html>
```

大功告成了

# 任务

## 异步任务

在SpringBoot启动类上添加@EnableAsync  业务方法上添加@Async注解  完了

## 定时任务

也很简单，创建一个类添以下注解即可

```java
@Configuration
@EnableScheduling
public class SchedulerTask {

    @Scheduled(cron = "* * * * * ?")
    public void taskmethod(){
        System.out.println(new Date());
    }
}
```

唯一需要注意的是这里需要用到cron表达式，在[这里](https://qqe2.com/cron#p_hour)可以查到每个表达式表达的含义 上面默认的表示每秒执行一次，还有其他的比如我现在的项目用的是3 0 0 * * ?  表示每天凌晨三秒的时候执行一次方法中的代码

# 总结

上面仅仅是一些基础的使用、SpringBoot的内容还有很多需要学习，继续加油吧(￣▽￣)／

