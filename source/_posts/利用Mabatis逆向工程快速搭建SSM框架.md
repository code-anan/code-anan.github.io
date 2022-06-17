title: 利用Mabatis逆向工程快速搭建SSM框架
author: lwl
date: 2021-06-22 09:38:54
tags:
  - SSM
  - Springboot
categories:
  - SpringBoot
my: post/SSM build
cover: https://cdn.jsdelivr.net/gh/code-anan/image/src=http---p7.zbjimg.com-task-2018-10-09-pub-5bbc781cb7f16.jpg&refer=http---p7.zbjimg.jpg
---
# 准备工作
为方便测试，在本地数据库准备好一张表![](https://cdn.jsdelivr.net/gh/code-anan/image/20210622094047.png)
以及一个`JDBC驱动jar包`![](https://cdn.jsdelivr.net/gh/code-anan/image/20210622094222.png)

# 正式开始
上面两项工作做好之后，就可以正式开始了
## 逆向工程创建
1. 创建一个简单的`springboot项目`![](https://cdn.jsdelivr.net/gh/code-anan/image/20210622094417.png)
相关配置注意jdk设为8就行
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210622094544.png)
注意勾选为`Spring Web`项目
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210622094746.png)

2. 创建完之后，就是相关的配置
   + 添加`MySQL驱动依赖`和`Mybatis整合SpringBoot框架的起步依赖`
   ```
        <!--MYSQL驱动-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
        <!--Mybatis整合springboot框架的起步依赖-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.0.0</version>
        </dependency>
   ```
   + 然后是在项目根目录下添加`GeneratorMapper.xml`文件
   ```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!-- 指定连接数据库的 JDBC 驱动包所在位置，指定到你本机的完整路径 -->
    <classPathEntry location="F:\MySql Connector Java 5.1.23\mysql-connector-java-5.1.23-bin.jar"/>
    <!-- 配置 table 表信息内容体，targetRuntime 指定采用 MyBatis3 的版本 -->
    <context id="tables" targetRuntime="MyBatis3">
        <!-- 抑制生成注释，由于生成的注释都是英文的，可以不让它生成 -->
        <commentGenerator>
            <property name="suppressAllComments" value="true" />
        </commentGenerator>
        <!-- 配置数据库连接信息 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/springboot"
                        userId="root"
                        password="233">
        </jdbcConnection>
        <!-- 生成 model 类，targetPackage 指定 model 类的包名， targetProject 指定
        生成的 model 放在 eclipse 的哪个工程下面-->
        <javaModelGenerator targetPackage="com.example.model"
                            targetProject="src/main/java">
            <property name="enableSubPackages" value="false" />
            <property name="trimStrings" value="false" />
        </javaModelGenerator>
        <!-- 生成 MyBatis 的 Mapper.xml 文件，targetPackage 指定 mapper.xml 文件的
        包名， targetProject 指定生成的 mapper.xml 放在 eclipse 的哪个工程下面 -->
        <sqlMapGenerator targetPackage="com.example.mapper"
                         targetProject="src/main/java">
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>
        <!-- 生成 MyBatis 的 Mapper 接口类文件,targetPackage 指定 Mapper 接口类的包
        名， targetProject 指定生成的 Mapper 接口放在 eclipse 的哪个工程下面 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.example.mapper" targetProject="src/main/java">
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>
        <!-- 数据库表名及对应的 Java 模型类名 -->
        <table tableName="t_student" domainObjectName="Student"
               enableCountByExample="false"
               enableUpdateByExample="false"
               enableDeleteByExample="false"
               enableSelectByExample="false"
               selectByExampleQueryId="false"/>
    </context>
</generatorConfiguration>
   ```
   {% note danger simple %}
注意：把上面`JDBC`驱动包的位置设置为你本地的路径，数据库`连接信息`也改为你的，`targetPackage`的包结构也要对应起来,`table`标签一张表对应一个实体类和其对应的dao接口和映射文件，这里只用到一张表所以只写了一个，如果有多张表那么则需要写多个table标签
   {% endnote %}
   + 然后还需要在pom文件中添加逆向工程需要的插件
   ```
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.6</version>
                <configuration>
                    <configurationFile>GeneratorMapper.xml</configurationFile>
                    <verbose>true</verbose>
                    <overwrite>true</overwrite>
                </configuration>
            </plugin>
   ```
   添加完之后记得`reimport`不然读取不到pom中添加的依赖
   ![](https://cdn.jsdelivr.net/gh/code-anan/image/20210622101301.png)
   刷新之后可以看到项目插件中多了`mybatis-generator`，双击就可以看到自动生成了实体类、Dao接口和映射文件![](https://cdn.jsdelivr.net/gh/code-anan/image/20210622103548.png)
   至此，逆向工程创建完成了~

## 控制层、业务层的创建
  逆向工程创建好，就等于持久层创建完了，现在只需要创建控制层和业务层即可
  1. 控制层，这里创建一个`StudentController`![](https://cdn.jsdelivr.net/gh/code-anan/image/20210622104530.png)
  2. 业务层，需要创建控制层调用的接口和对应的实现类，并加上service注解![](https://cdn.jsdelivr.net/gh/code-anan/image/20210622105047.png)
  3. Dao层的接口虽然逆向工程的时候自动为我们添加了方法，但在业务层调用的时候需要使用autowired注解从spring容器中获取，所以需要添加注解`Mapper`
  ![](https://cdn.jsdelivr.net/gh/code-anan/image/20210622105200.png)
## 核心配置文件的配置
上面配置好之后，还需要在核心配置文件中(application.properties)添加必要的数据库配置
```
#设置连接数据的配置(mysql8之后需要加上cj)
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/springboot
spring.datasource.username=root
spring.datasource.password=233
```
## 手动添加指定文件夹
上面的步骤以及差不多了，但是我们测试的时候出现报错，原来是dao层中的映射文件没被编译，这里有两种解决方案
 1. 在pom中手动指定资源文件夹，使映射文件可以被编译（同样需要reimport）
 ```
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </resource>
        </resources>
 ```
 2. 将映射文件移动到resources资源文件夹,并在核心配置文件中声明位置
 ![](https://cdn.jsdelivr.net/gh/code-anan/image/20210622110435.png)
 
 ## 成功测试
 启动项目，在浏览器上测试![](https://cdn.jsdelivr.net/gh/code-anan/image/20210622111136.png)
 可以看到返回到了数据，大功告成~
# 补充
1. 上面在业务层使用Autowired注解调用Dao层，除了添加`mapper`注解之外，还可以在核心配置文件中添加`MapperSacn`注解，其值为dao接口所在包![](https://cdn.jsdelivr.net/gh/code-anan/image/20210622111624.png)
2. Mybatis逆向生成只针对单表，并且数据库中的字段由多个单词构成时必须用_分隔