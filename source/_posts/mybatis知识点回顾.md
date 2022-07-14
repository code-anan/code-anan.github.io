title: mybatis知识点回顾
author: lwl
cover: https://gcore.jsdelivr.net/gh/code-anan/image/20211224140144.png
categories:
  - mybatis
  - 记录
my: post/mybatis
date: 2021-12-24 14:03
tags:
  - mybatis
------

# JDBC的缺陷

1. 代码比较多，开发效率低下
2. 需要关注Connection、Statement、ResultSet对象的创建和销毁
3. ResultSet查询出来的结果需要自己封装为List集合
4. 重复的代码多
5. 业务代码和数据库操作代码混在一起 显得凌乱

# mybatis的使用步骤

## 依赖的导入

```xml
<dependency>
  <groupId>org.mybatis</groupId>
  <artifactId>mybatis</artifactId>
  <version>3.5.1</version>
</dependency>
```

如果不使用maven构建项目，需要另外下载对应的jar包[下载地址](https://github.com/mybatis/mybatis-3/releases)另外与数据库连接还需要数据库连接依赖，这里mysql为例：

```xml
<dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.49</version>
</dependency>
```

## 基本使用

###  创建一个maven项目

![](https://gcore.jsdelivr.net/gh/code-anan/image/20211223150019.png)

PS：用这个模板需要手动添加resources资源目录(￣▽￣)~*

### 创建好用来演示的表 

这里就叫Student吧 

![](https://gcore.jsdelivr.net/gh/code-anan/image/20211223150138.png)

### 创建必要文件

创建好需要的实体类(model下的Student)、接口（mapper下的StudentDao）、映射文件(mapper下的StudentDao.xml,也可以放到resources目录下)以及mybatis主配置文件`mybatis.xml`当然名字都随意![](https://gcore.jsdelivr.net/gh/code-anan/image/20211223150257.png)

需要注意的一点：由于编译时候maven不能默认编译src下的xml文件 所以需要在pom.xml中build下添加如下才能编译xml文件

```xml
<resources>
      <resource>
        <directory>src/main/java</directory><!--所在的目录-->
        <includes><!--包含目录下的.xml文件都会被扫描到-->
          <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
      </resource>
</resources>
```

### 创建实体类

实体类属性和数据库保持一致即可

```java
    private int age;
    private String name;
    private int id;
```
### 添加方法

接口中添加一个查找所有学生的方法

![](https://gcore.jsdelivr.net/gh/code-anan/image/20211223152454.png)

### 创建映射文件

由接口文档得到映射文件`StudentDao.xml`的内容应如下,默认格式可以到[官网](https://mybatis.org/mybatis-3/zh/getting-started.html)拷贝

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.weilong.mapper.StudentDao">
    <select id="selectAllStudents" resultType="com.weilong.model.Student">
       select id,name,age from student
    </select>
</mapper>
```

其中mapper标签是当前文件的跟标签，必须存在，而`namespace`为dao接口的`全限定名称`，其实就是用它和接口对应起来，然后`mapper`标签内可以有`<select>`、`<update>`、`<insert>`、`<delete>`四种标签表示执行增删改查的操作
这里以select为例,id表示这个select语句的唯一标识，与接口中的方法名保持一致，resultType表示返回结果的每一项的类型

### 创建mybatis主配置文件

mybatis配置文件内容配置，也可以到[官网](https://mybatis.org/mybatis-3/zh/getting-started.html)拷贝

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
   <!-- <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>-->
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
    </plugins>
    <environments default="development">

        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/lwl"/>
                <property name="username" value="root"/>
                <property name="password" value="233"/>
            </dataSource>
        </environment>

    </environments>
    
    <mappers>
        <mapper resource="com/weilong/mapper/StudentDao.xml"/>
    </mappers>

</configuration>
```

`environments`中的default值为要使用哪一个数据库环境、`environment`表示具体的数据库环境，`transactionManager`的type值为mybatis的事务类型,`dataSource`的type表示数据源类型，pooled表示连接池，四个property分别表示驱动类名、数据库url、数据库账号和密码、mappers中的mapper标签则表示对应的映射文件的路径

### 测试使用

(这个不需要记住 后面引入Spring不需要这些繁琐代码)

```java
public static void main(String[] args) throws IOException {
        String config= "mybatis.xml";
        //读取mybatis配置文件
        InputStream stream = Resources.getResourceAsStream(config);
        //通过SqlSessionFactoryBuilder对象创建SqlSessionFactory对象
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(stream);
        //通过SqlSessionFactory得到SqlSession对象
        SqlSession sqlSession = factory.openSession();
        //找到dao接口定义的方法执行sql
        String sqlId="com.weilong.mapper.StudentDao"+"."+"selectAllStudents";
        //返回执行的结果
        List<Student> list = sqlSession.selectList(sqlId);
        //循环输出
        for (Student s:list){
            System.out.println(s);
        }
        //关闭qlSession对象
        sqlSession.close();
        //如果有更新操作的话 需要提交事务 mybatis默认不提交事务
        sqlSession.commit();
    }
```

得到输出结果（注意Student类的toString方法需要重写）![](https://gcore.jsdelivr.net/gh/code-anan/image/20211223154957.png)

# 深入理解

## parameterType

```xml
 <select id="selectStudentByid" resultType="com.weilong.model.Student" parameterType="java.lang.Integer">
       select id,name,age from student where id=#{id}
 </select>
```

parameterType表示接口中传递过来的参数类型，它的值必须为java类型的全限定名称或者别名，mybatis把java基本数据类型和String类型都叫做简单类型，在映射文件中接收简单类型的方式为#{任意字符}即可,但是由于mybatis的反射机制可以知道入参的类型，所以此属性一般可以不写

## 多参数传递

dao接口：

```java
List<Student>selectMutiStudents(@Param("name")String name,@Param("age")Integer age);
```

映射文件:

```xml
<select id="selectMutiStudents" resultType="com.weilong.model.Student">
       select id,name,age from student where name =#{name} and age=#{age}
</select>
```

多个简单类型参数可以使用@Param 同样使用#{}的方式赋值

## 使用对象传参

dao接口：

```java
List<Student> selectStudents(Student student);
```

映射文件:

```xml
<select id="selectMutiStudents" resultType="com.weilong.model.Student">
       select id,name,age from student where name =#{name} and age=#{age}
</select>
```



对象传参同样用#{属性}的方式赋值

## 按位置传参

接口：

```java
List<Student> selectStudents(String name,Integer id);
```

映射文件：

```xml
<select id="selectStudents" resultType="com.weilong.model.Student">
    select id,name,age from student where name =#{arg0} and id= #{arg1}
</select>
```



mybatis 3.4之前 使用#{0},#{1} 之后都用#{arg0},#{arg1}

## map传参

接口：

```java
List<Student> selectStudents(Map<Object,Object> map);
```

映射文件：

```xml
<select id="selectStudents" resultType="com.weilong.model.Student">
    select id,name,age from student where name=#{myname}
</select>
```



使用map传入多个值、语法为#{map的key}，赋值为对应的value值

## #和$的区分

#是占位符，底层使用的是PrePraeStatement执行sql，这样做更安全防止sql注入

$是字符串替换，底层使用的是Statement进行数据的替换，可能会发生sql注入

## 自定义类型别名

我们知道resultType是指明返回的数据类型，但是也可以使用别名的方式，例如resultType="java.lang.Integer",可以直接写成resultType="int" 除此之外还可以自定义类型别名

第一种方式是mybatis配置文件中引入

```xml
<typeAliases>
        <typeAlias type="com.weilong.model.Student" alias="stu"></typeAlias>
</typeAliases>
```

这样映射文件的resultType可以直接写resultType="stu"了

第二种方式是
```xml
<typeAliases>
        <package name="com.weilong.model"/>
</typeAliases>
```

这种方式类名即是别名

## resultMap

使用方法如下

```xml
<resultMap id="resultMapId" type="com.weilong.model.Student">
        <id column="id" property="id"></id>
        <result column="name" property="name"></result>
        <result column="age" property="age"></result>
</resultMap>

<select id="selectStudents" resultMap="resultMapId">
        select id,name,age from student
</select>
```

select中的resultMap的值为<resultMap>中的id值，然后主键使用id标签、非主键使用result标签，column表示的是`数据库`中的列名、propertity表示的是type对应实体类的java类型中的`属性名`，使用这种方式的好处是如果数据库列名和java类属性名不一致可以一一对应起来，但是也可以通过改名的方式用resultType也一样可以；

## 模糊查询

除了基本使用like的方式 ，mybatis还支持使用下面这种方式

```xml
<select id="selectStudents" resultMap="resultMapId">    
  select id,name,age from student like "%" #{name} "%"
</select>
```

不过"%"要和#{name}中间用空格分开

# 动态sql

正如其名，sql内容需要动态变化，需要用到动态sql的语法，比如对象中的某个字段可能为空，如果不处理可能就会报错

## if标签

语法是`<if test="判断java对象的属性值"> 部分sql语句</if>` 例子：

```xml
<select id="selectStudents" resultMap="resultMapId">
        select id,name,age from student where
        <if test="name!=null">
            name =#{name}
        </if>
</select>
```

这种方式的缺陷就是如果不满足test内的条件可能会使sql不完整，所以可以在where后面添加个恒等式比如1=1

## where标签

相比于单纯的if标签、where标签的使用率更广，他可以包含多个if标签，当有if标签符合条件，他就可以动态的添加where关键字并且可以去掉if中多余的and or关键字容错率高

例子:

```xml
<select id="selectStudents" resultMap="resultMapId">
        select id,name,age from student
        <where>
            <if test="name!=null">
                name =#{name}
            </if>
            <if test="age>18">
               or age>#{age}
            </if>
        </where>
</select>
```

## foreach标签

用来循环数组和list集合的，主要用在sql的in语句中

例子：

```xml
<select id="selectEach" resultType="com.weilong.model.Student">
        select * from student where id in 
        <foreach collection="list" item="myid" open="(" close=")" separator=",">
            #{myid}
        </foreach>
</select>
```

collection:表示接口入参类型 可以是数组array或者list集合

item：自定义数组或者集合每一项的变量名

open：循环开始的字符

close：循环结束的字符

separator:集合或数据成员之间的分隔符

如果上面的item是一个java对象 而我们循环用的是他的一个属性比如id

```xml
<foreach collection="list" item="stu" open="(" close=")" separator=",">   
      #{stu.id}
</foreach>
```

使用对象.的方式即可

## sql代码片段

就是复用一些sql语句 比如查学生的`select id,name,age from student`这一部分重复使用,<sql>和<include>配合使用，例如

```xml
<sql id="selectrepeat">
        select id,name,age from student
</sql>
<select id="selectstuByid" resultType="com.weilong.model.Student">
       <include refid="selectrepeat"></include>
       where id=#{id}
</select>
```

不过这种方式没必要 而且占空可读性差  了解即可

# 主配置文件

## transactionManager

在mybatis.xml中的type表示事务的处理类型；我们用到使其type='JDBC'，这表示mybatis底层调用JDBC的Connection、commit、rollback,type='MANAGED'表示mybatis把事务处理委托给其他的容器例如Spring

## properties

我们配置数据库信息的时候是之间写在mybatis.xml文件中，这样耦合度比较高，所以使用properties标签把数据库信息写在单独的文件`jdbc.properties`中

mybatis.xml中引入jdbc.properties：

```xml
<properties resource="jdbc.properties"></properties>
```

然后jdbc.properties文件放入数据库信息

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/lwl
jdbc.username=root
jdbc.password=233
```

使用${}的方式替换之前的写法

```xml
<environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
</environment>
```

这样修改数据库信息 只需要修改jdbc.properties文件即可

## mappers

指定映射文件的位置，一种方式是一个个引入

```xml
<mappers>
        <mapper resource="com/weilong/mapper/StudentDao.xml"/>
        <mapper resource="com/weilong/mapper/ClassDao.xml"/>
</mappers>
```

或者是直接引入包，把包下的所有映射文件一次性引入，不过

使用package有两个要求

1. mapper文件名称需要和接口名称完全一致，区分大小写

2. mapper文件和dao接口需要在同一目录下

   ```xml
   <mappers>
           <package name="com.weilong.mapper"/>
   </mappers>
   ```

# pageHelper分页插件

这是一款mybatis的通用分页插件，支持多种数据库，使用也很简单可以直接获取每一页的固定数量的数据个数 使用步骤如下

## 添加依赖

```xml
<dependency> 
  <groupId>com.github.pagehelper</groupId>
  <artifactId>pagehelper</artifactId> 
  <version>5.3.0</version>
</dependency>
```

## mybatis配置文件中添加

```xml
<plugins>    
  <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
```

## 加入方法

```java
PageHelper.startPage(pagenum,pagesize);
```

![](https://gcore.jsdelivr.net/gh/code-anan/image/20211224105820.png)

pagenum:第几页的数据 从1开始

pagesize：一页中有多少条数据

这里我取得是第二页六条数据 总共十条数据，所以第二页只有四条数据 所以还是很好用的(*^▽^*) 

