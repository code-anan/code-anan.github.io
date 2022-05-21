title: Swagger技术学习
author: lwl
cover: https://fastly.jsdelivr.net/gh/code-anan/image/20220425182545.png
my: post/swagger	
tags:
  - Swagger
categories:
  - Swagger

------

# 简介

- 号称世界上最流行的API框架
- Restful api文档在线自动生成工具=》Api文档与API定义同步更新
- 直接运行，可以在线测试API接口

官网：[API Documentation & Design Tools for Teams | Swagger](https://swagger.io/)

# SpringBoot集成swagger

1. 新建springboot项目
2. 导入相关依赖

```xml
<dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>

        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
```

3. 编写一个hellocontroller

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello(){
        return "hello world!";
    }
}
```

4. 配置swagger（一般都在config目录下）

```java

@Configuration
@EnableSwagger2
public class SwaggerConfig {
}
```

5. 测试运行：http://localhost:8999/swagger-ui.html

![](https://fastly.jsdelivr.net/gh/code-anan/image/20220421143722.png)

需要注意的是springboot版本不能太高，我这里换成了2.4.0版本的springboot

swagger的ui页面我们也可以换用bottstrap的 更好看一点

将上面的`springfox-swagger-ui`依赖换成下面的

```xml
<dependency>
			<groupId>com.github.xiaoymin</groupId>
			<artifactId>swagger-bootstrap-ui</artifactId>
			<version>1.9.2</version>
		</dependency>
```

访问路径变成`/doc.html`![](https://fastly.jsdelivr.net/gh/code-anan/image/d08b5c578071398bb42d31ac31ab36d.png)

# 配置Swagger

Swagger的bean实例Docket

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    //创建了一个docket的bean实例
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo());
    }

    private ApiInfo apiInfo(){
        Contact contact = new Contact("lwl", "https://weilong98.com/", "719603766@qq.com");
        return  new ApiInfo("我的swagger文档",
               "gogogo",
               "1.0",
               "https://weilong98.com/",
                contact,
               "Apache 2.0",
               "http://www.apache.org/licenses/LICENSE-2.0",
                new ArrayList());
    }
}
```

在这里我们可以配置自己的一些信息![](https://fastly.jsdelivr.net/gh/code-anan/image/20220421151359.png)

#  Swagger配置扫描接口

```java
@Configuration
@EnableSwagger2
public class SwaggerConfig {

    //创建了一个docket的bean实例
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                //RequestHandlerSelectors.basePackage 设置扫描接口的方式（基于包的方式扫描）
                //any  扫描全部
                //none  全都不扫描
                //withMethodAnnotation 在有某个注解的方法上才扫描
                //withClassAnnotation 在有某个注解的类上才扫描
                .apis(RequestHandlerSelectors.basePackage("com.weilong.swagger.controller"))
                //过滤的路径
                //ant过滤设置的某些路径 还有any none的可以忽略掉
                .paths(PathSelectors.ant("com.weilong.swagger.controller"))
                .build();
    }

    private ApiInfo apiInfo(){
        Contact contact = new Contact("lwl", "https://weilong98.com/", "719603766@qq.com");
        return  new ApiInfo("我的swagger文档",
               "gogogo",
               "1.0",
               "https://weilong98.com/",
                contact,
               "Apache 2.0",
               "http://www.apache.org/licenses/LICENSE-2.0",
                new ArrayList());
    }
}
```

设置哪些扫描成接口

## 设置swagger是否自己启动

```
return new Docket(DocumentationType.SWAGGER_2)
        .apiInfo(apiInfo())
        .enable(false)
        .select()
```

默认为true，如果为false那么无法在浏览器中访问

## 小题目

如何在生产环境中可以访问，正式环境无法访问swagger？

```
public Docket docket(Environment environment){
    Profiles profiles = Profiles.of("dev","test");
    boolean flag = environment.acceptsProfiles(profiles);
    return new Docket(DocumentationType.SWAGGER_2)
            .apiInfo(apiInfo())
            .enable(flag)
            .select()
            //RequestHandlerSelectors.basePackage 设置扫描接口的方式（基于包的方式扫描）
            //any  扫描全部
            //none  全都不扫描
            //withMethodAnnotation 在有某个注解的方法上才扫描
            //withClassAnnotation 在有某个注解的类上才扫描
            .apis(RequestHandlerSelectors.basePackage("com.weilong.swagger.controller"))
            .build();
}
```

思路就是设置可以使用的环境，再获取到当前的环境进行动态的改变

# 分组和注解

## 分组

分组的话也非常简单，只需要创建多个Docket实例即可,给groupname赋值即可

```java
 @Bean
    public Docket docket1(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("A");
    }

    @Bean
    public Docket docket2(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("B");
    }
```

![](https://fastly.jsdelivr.net/gh/code-anan/image/20220421164953.png)

## 一些常用的注解

实体类：

```java
//@Api(注释一般放在模块中)
@ApiModel("用户实体类")
public class User {
    @ApiModelProperty("姓名")
    private String name;
    @ApiModelProperty("用户id")
    private Integer id;
}
```

接口中：

```java
@Api(value = "HelloController",tags = {"第一个controller"})
@RestController
public class HelloController {

    @GetMapping("/hello")
    @ApiOperation("输入hello")
    public String hello(){
        return "hello world!";
    }



    @PostMapping("/user")
    @ApiOperation("获取用户信息")
    public User getuser(@ApiParam("用户id") Integer id){
        return new User();
    }
}

```

效果：

![](https://fastly.jsdelivr.net/gh/code-anan/image/20220421180514.png)

# 总结

可以给一些属性和接口增加注释，接口文档可以实时更新，企业中使用较多