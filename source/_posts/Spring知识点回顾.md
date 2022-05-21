title: Spring知识点回顾
author: lwl
cover: https://fastly.jsdelivr.net/gh/code-anan/image/src=http---www.8090.com-uploads-allimg-210410-293530-210410140505B9.png&refer=http---www.8090.jpg

my: post/SpringKeyPoints

date: 2021-12-30 14:03

categories: 

  - spring

tags:

  - spring
------

# Spring优点

1. 轻量化
2. 针对接口编程、解耦合
3. AOP编程的支持
4. 方便集成各种优秀框架

# IOC控制反转

IOC（Inversion of Control）控制反转：一种思想，把对象的创建、赋值、管理都交给代码之外的`容器`实现，也就是对象的创建由外部资源完成。

简单理解就是不用new Student()的方式创建对象、由Spring容器来做这件事情

## IOC的技术实现

DI（Dependency Injection）:依赖注入是IOC的技术实现，只需要在程序中提供要使用的对象名称、对象创建赋值都由容器内部实现、其底层是使用的` 反射 `机制。可以把Spring理解成一个容器，用来管理对象给对象赋值。

## 基于bean标签的DI

### 创建一个maven项目

![](https://fastly.jsdelivr.net/gh/code-anan/image/20211223150019.png)

### 引入依赖

```xml
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.5.RELEASE</version>
</dependency>
```

### 创建实体类

这里我们创建一个接口和实现类以及一个普通类

接口

```java
public interface MyService {
    void someService(Integer i);
}

```

接口实现类

```java
public class MyServiceImpl implements MyService {
    @Override
    public void someService(Integer i) {
        System.out.println(i+"的方法实现了！");
    }
}
```

普通类：

```java
public class Student {
    private int id;
    private String name;
    private String interest;

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

    public String getInterest() {
        return interest;
    }

    public void setInterest(String interest) {
        this.interest = interest;
    }

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", interest='" + interest + '\'' +
                '}';
    }
}

```

### 创建spring的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">


    <bean id="myService" class="com.weilong.example.mapper.impl.MyServiceImpl">
    </bean>
    <bean id="student" class="com.weilong.example.model.Student">
    </bean>

</beans>
```

bean标签的作用就是告诉Spring要创建某个类的对象

id：对象的自定义名称，唯一值，Spring通过id的值找到这个对象

class：类的全限定名称，不能是接口否则不能创建对象

这里定义的bean相当于了执行了MyService  myService=new MyServiceImpl();并且spring把创建好的对象放入到map中（Spring有一个map用来存放对象）SpringMap.put("myService",new MyServiceImpl());

一个bean标签仅代表创建一个对象；

### 使用bean创建的对象

```java
public static void main(String[] args) {
        //指定Spring配置文件的名称(默认放在resources目录下)
        String config="SpringConfig.xml";
        //创建spring容器对象（ClassPathXmlApplicationContext通过从类路径中加载spring配置文件）
        ApplicationContext application = new ClassPathXmlApplicationContext(config);
        //通过容器获取对象 getBean("配置文件中bean的值")
        MyService myService = (MyService)application.getBean("myService");
        Student student = (Student)application.getBean("student");
        //执行对象的方法
        myService.someService(1);
        student.setId(1);
        student.setName("张三");
        student.setInterest("玩游戏");
        System.out.println(student);
    }
```

运行之：

```java
1的方法实现了！
Student{id=1, name='张三', interest='玩游戏'}
```

emm 觉得这种方式傻逼很正常  我也这样觉得（后面全部用注解了）

### 创建非自定义类的对象

Spring还可以创建非自定义类的对象，例如java工具包下的Date类

```java
<bean id="mydate" class="java.util.Date">
```

### 给对象赋值

DI的实现方式有两种：

* 在spring的配置文件中，使用标签和属性完成，叫做基于xml的di实现

* 使用spring中的注解（常用）完成属性赋值，叫做基于注解的di实现


DI的语法分类

* set注入（设置注入）：spring调用类的set方法，在set方法中完成对属性的赋值
* 构造注入，spring调用类的有参构造方法创建对象

#### set注入

spring的配置文件中

* 简单类型的set注入(Spring中java基本数据类型和String类型都被视为简单类型)

```xml
<bean id="student" class="com.weilong.example.model.Student">
        <property name="id" value="2"></property>
        <property name="interest" value="玩小车"></property>
        <property name="name" value="艾伦"></property>
</bean>
```

* 引用类型的set注入

这里演示给Student类添加一个school属性即可

```xml
<bean id="student" class="com.weilong.example.model.Student">
        <property name="id" value="2"></property>
        <property name="interest" value="玩小车"></property>
        <property name="name" value="艾伦"></property>
        <property name="school" ref="myschool"></property>
</bean>
    
<bean id="myschool" class="com.weilong.example.model.School">
        <property name="name" value="清华大学"></property>
        <property name="address" value="北京海淀区"></property>
</bean>
```

引用类型的value换为ref 值为bean的id即可

#### 构造注入

使用的前提是确定我们的类中定义好了有参构造

```xml
<bean id="student" class="com.weilong.example.model.Student">
       <constructor-arg name="id" value="1"></constructor-arg>
       <constructor-arg name="name" value="艾伦"></constructor-arg>
       <constructor-arg name="interest" value="变身巨人"></constructor-arg>
       <constructor-arg name="school" ref="myschool"></constructor-arg>
</bean>
```

有参构造赋值的方式使用constructor-arg标签并且可以按照idnex的方式赋值，这里不再复述

引用类型的自动注入也分为两种：

* 按照名称注入

```xml
<bean id="student" class="com.weilong.example.model.Student" autowire="byName">
       <constructor-arg name="id" value="1"></constructor-arg>
       <constructor-arg name="name" value="艾伦"></constructor-arg>
       <constructor-arg name="interest" value="变身巨人"></constructor-arg>
       <!--<constructor-arg name="school" ref="myschool"></constructor-arg>-->
</bean>

<bean id="school" class="com.weilong.example.model.School">
        <property name="name" value="清华大学"></property>
        <property name="address" value="北京海淀区"></property>
</bean>
```

autowire="byName":需要保证的引用类型的属性名和配置文件中的bean标签中的id保持一致且数据类型一致，这样就能够赋值给引用类型

* 按照类型注入

```xml
<bean id="student" class="com.weilong.example.model.Student" autowire="byType">
       <constructor-arg name="id" value="1"></constructor-arg>
       <constructor-arg name="name" value="艾伦"></constructor-arg>
       <constructor-arg name="interest" value="变身巨人"></constructor-arg>
       <!--<constructor-arg name="school" ref="myschool"></constructor-arg>-->
</bean>

<bean id="myschool" class="com.weilong.example.model.School">
        <property name="name" value="清华大学"></property>
        <property name="address" value="北京海淀区"></property>
</bean>
```

autowire="byType":java类中引用类型的数据类型(上面那个bean中)和配置文件bean的class属性（下面的bean）是有同源关系的，这样的bean能够赋值给引用类型

同源关系：一样、父子类关系、接口和实现类关系

注意：使用这种方式必须只有一个bean是符合条件的 负责会报错

### 第一个小例总结

什么样的对象放入容器中：

dao类，service类，controller类和工具类且在容器中同一个名称的对象只有一个

不放入到容器中：

实体类对象因为他们一般来自数据库

servlet、listener和filter等

### 多个配置文件

可以使用具有包含关系的配置文件，可以把配置文件按照模块或者功能进行分类，在总配置文件中可以使用下面的方式加载其他配置文件，例如

```xml
<import resource="classpath:spring/spring-school.xml">
<import resource="classpath:spring/spring-student.xml">
```

或者使用通配符*

```xml
<import resource="classpath:spring/spring-*.xml">
```

注意主配置文件名称不能包含在通配符的范围之内

## 基于注解的DI

顾名思义，可以使用注解的方式完成java对象的创建并赋值

### 使用步骤

####  加入spring-context依赖

加入Spring-context(上面加过了)的同时，也间接的加入了Spring-aop的依赖，使用注解就需要这个依赖

#### 类中加入spring的注解

![](https://fastly.jsdelivr.net/gh/code-anan/image/20211224171126.png)

@component:创建对象 等同于bean标签的作用

value就是对象的名称 也就是bean的id值 并且唯一不重复 value可忽略不写 如果不指定对象名称 默认为类名的首字符小写

与@Component功能一致的其他注解：

* @Repository(用在持久层类的上面)：放在dao的实现类上面表示创建dao对象 使其与数据库交互
* @Service（用在业务层类的上面）：放在service的实现类上创建service对象，serevice对象作用是做业务处理可以有事务的功能
* @Controller（用在控制层上）：创建控制层对象，能够接收用户提交传递的参数并能处理请求结果

#### 声明组件扫描器

在spring配置文件中加入下面标签

```xml
<context:component-scan base-package="org.example.model"></context:component-scan>
```

base-package：指定添加注解的包名

工作方式：spring会扫描遍历base-package指定的包把包中和子类的包中的所有类，找到类中的注解创建对象或给属性赋值

如果有多个包 可以选择扫描多个包或者使用`;` 或者`,`分隔还可以直接指定父类包的方式

### 属性赋值

#### 简单类型的属性赋值

直接在实体类中的属性上面使用@value注解赋值

```java
    @Value(value = "2")
    private int id;

    @Value("张三")
    private String name;

    @Value("打王者")
    private String hobby;
```

其实value=可以省略，也可以在set方法上面使用

#### 引用类型的赋值

使用注解@Autowired：使用的是自动注入原理，支持byName、byType默认使用的是byType自动注入

位置可以在属性定义的上面也可以在set方法的上面

如果要使用byName的方式，则需要添加@Qualifier注解

```java
    @Value(value = "2")
    private int id;

    @Value("张三")
    private String name;

    @Value("打王者")
    private String hobby;
    
    @Autowired
    @Qualifier(value = "aSchool")
    private School school;
```

School类中应为(名称需要跟@Qualifier的value值一致)：

```java
@Component("aSchool")
public class School {
    @Value("深圳中学")
    private String name;
    @Value("深圳市")
    private String address;
```

输入结果为：

```java
Student{id=2, name='张三', hobby='打王者', school=School{name='深圳中学', address='深圳市'}}
```

默认使用byType的方式：

```java
    @Value(value = "2")
    private int id;

    @Value("张三")
    private String name;

    @Value("打王者")
    private String hobby;

    @Autowired
    private School school;
```

School类中应为：

```java
@Component("anotherName")
public class School {
    @Value("深圳小学")
    private String name;
    @Value("深圳市")
    private String address;
```

输出结果为：

```java
Student{id=2, name='张三', hobby='打王者', school=School{name='深圳小学', address='深圳市'}}
```

可以看出来默认使用byType的方式 与它的名称无关了

### Required属性

required：@Autowired的属性，布尔类型，默认为true

required=true:表示如果引用类型赋值失败程序报错并且终止执行

required=false:表示引用类型如果赋值失败，程序可以正常执行，引用类型是null

### Resource注解

jdk中的一个注解，可以用它给引用类型赋值使用的也是自动注入原理，支持byType、byName两种方式并且默认的是byName

位置可以放在属性定义上也可以放到set方法上

```java
   @Resource(name = "anotherName")
    private School school;
```

如果不添加name属性，则默认使用byName的方式赋值失败之后选择byType，如果添加name属性，则表示只使用byName的方式，name就是bean的id（名称）

# AOP面向切面编程

AOP（Aspect Orient Programming）:面向切面编程基于动态代理可以使用jdk、cglib两种代理方式，AOP就是动态代理的规范化，把动态代理的实现步骤和方式都定义好，让开发人员用一种统一的方式使用动态代理

## 面向切面编程的理解

1. 分析项目功能找出切面
2. 合理安排切面的执行时间（在目标方法之前还是之后）
3. 合理安排切面的执行位置，具体在哪个类哪个方法增强功能

## 术语

1. Aspect:切面，表示增强的功能，本质还是一堆完成一个功能的代码，但不是业务功能，常见的切面功能有日志、事务、统计信息、参数检查、权限验证

2. JoinPoint:连接点，连接业务方法和切面的位置

3. Pointcut:切入点，指多个连接点方法的集合

4. 目标对象：给哪个类的方法增加功能，这个类就是目标对象

5. Advice：通知，表示切面功能执行的时间 


## AOP的实现

aspectJ:一个开源专门做aop的框架，spring框架中继承了aspectJ框架，通过spring就能使用他的功能

aspectJ实现AOP有两种方式：

1. 使用xml的配置文件：配置全局事务
2. 使用注解，我们在项目中要做AOP功能一般都使用注解，aspectJ中有五个注解

## aspectJ的使用

### 通知类型

这个执行时间在规范中叫做通知(Advice)，在aspectJ框架中使用注解表示，也可以使用xml配置文件中的标签

* @Before
* @AfterReturning
* @Around
* @AfterThrowing
* @After

### 切入点表达式

我们使用`切入点表达式`进行表示切面的执行位置，语法如下

execution(访问权限 `方法返回值` `方法参数` 异常类型)

表达式中的访问权限和异常类型可以省略，各部分之间使用空格分开，在其中可以使用下列符号

`*`:0至多个任意字符

`..`:用在方法参数中，表示任意多个参数；用在包名后，表示当前包及其子包路径

`+`:用在类名后，表示当前类及其子类；用在接口后，表示当前接口及其实现类

举例：

execution(public * *(..))

指定切入点为：任意的public修饰的方法

execution(* set*(..))

指定切入点为：任意一个以"set"开始的方法

```java
execution(* com.xyz.service.*.*(..))
```

指定切入点为：service包下任何类的任何方法

```java
execution(* com.xyz.service..*.*(..))
```

指定切入点为：定义在service包或者子包里的任意类的任意方法。".."出现在类名中时，后面必须跟"*"，表示包、子包下的所有类

```java
execution(* *..service.*.*(..))
```

指定切入点为：所有包下的service子包下所有类（接口）所有方法为切入点

### 案例使用

使用aspectJ框架实现aop：目的是给已经存在的一些类和方法增加额外的功能，并且不会改变原来的类的代码

#### 新建maven项目

![](https://fastly.jsdelivr.net/gh/code-anan/image/20211223150019.png)

需要额外手动加入resources目录

#### 加入依赖

```xml
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.5.RELEASE</version>
</dependency>
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>5.3.14</version>
</dependency>
```

需要spring的依赖和spring整合的aspectJ的依赖

#### 创建目标类

目的是创建目标方法，这里我们创建一个接口（省略）和他的实现类即可

```java
public class MyInterfaceImpl implements MyInterface {
    @Override
    public String say() {
        return "我是大傻逼";
    }
}

```

#### 创建切面类

首先在类上面加入@Aspect注解 ；在类中定义方法  方法就是切面要执行的功能代码，代码如下

```java
@Aspect
public class MyAspect {

    @Before(value = "execution(public String com.weilong.mapper.impl.MyInterfaceImpl.say())")
    public void myBefore(){
        SimpleDateFormat format = new SimpleDateFormat("YYYY-MM-dd HH:mm:ss");
        System.out.println("目标方法的执行时间为:"+format.format(new Date()));
    }
}

```

@Aspect:aspectJ框架中的注解，用来表示当前类是切面类，也就是为业务方法增强功能的类，在这个类中有切面的功能代码，位置定义在类的上面

方法的定义要求：

1. 公共方法public
2. 方法没有返回值（因为不是给别人调用的）

@Before注解：前置通知注解(通知的一种)

属性：value值为切入点表达式，表示切面的功能执行的位置，位置定义在方法上面

特点：在目标方法执行之前执行，不会对目标方法造成任何影响，例如我们这里只是在执行目标方法之前输出一下当前时间

#### 配置文件

spring配置文件中加入目标对象和切面类对象

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean id="myservice" class="com.weilong.mapper.impl.MyInterfaceImpl"></bean>
    <bean id="myaspect" class="com.weilong.aop.MyAspect"></bean>

    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```

声明目标对象和切面类对象由spring统一管理对象；

aop:aspectj-autoproxy标签的作用是声明自动代理生成器：使用aspectJ框架内部的功能，创建目标对象的代理对象，创建代理对象是在内存中实现的，修改目标对象在内存中的结构所以目标对象就是被修改后的代理对象

aspectj-autoproxy：会把spring容器中所有的目标对象一次性都生成代理对象

#### 创建测试类

```java
public class Test {
    public static void main(String[] args) {
        String config="SpringConfig.xml";
        ApplicationContext context = new ClassPathXmlApplicationContext(config);
        MyInterface myservice = (MyInterface) context.getBean("myservice");
        String say = myservice.say();
        System.out.println(say);
    }
}

```

执行之，得到结果![](https://fastly.jsdelivr.net/gh/code-anan/image/20211227140418.png)

可以看到 目标方法执行之前执行了切面方法

### joinPoint的使用

我们在上述接口和实现类新增一个有参数的方法

```java
    @Override
    public void dosome(String str, Integer i) {
        System.out.println(str+"的年龄是"+i);
    }
```

切面类改为：

```java
@Aspect
public class MyAspect {

    @Before(value = "execution(public void com.weilong.mapper.impl.MyInterfaceImpl.dosome(String,Integer))")
    public void myBefore(JoinPoint jp){
        System.out.println("方法的定义是："+jp.getSignature().toString());
        System.out.println("方法的名称是："+jp.getSignature().getName());
        Object[] args = jp.getArgs();
        for(Object o:args){
            System.out.println(o);
        }
        SimpleDateFormat format = new SimpleDateFormat("YYYY-MM-dd HH:mm:ss");
        System.out.println("目标方法的执行时间为:"+format.format(new Date()));
    }
}
```

测试类为：

```java
public class Test {
    public static void main(String[] args) {
        String config="SpringConfig.xml";
        ApplicationContext context = new ClassPathXmlApplicationContext(config);
        MyInterface myservice = (MyInterface) context.getBean("myservice");
        //System.out.println(myservice.say());
        myservice.dosome("皮卡",20);

    }
}
```

得到结果为：![](https://fastly.jsdelivr.net/gh/code-anan/image/20211227142259.png)

通过此例 可以看出来JoinPoint在通知方法中可以获取方法执行时的一些信息，比如方法名称、方法的实参如果需要在通知方法中需要用到这些信息就可以选择加入JoinPoint，并且他必须是通知方法中的第一个参数

### 其他类型的通知

我们知道@Before是前置通知，在我们调用目标方法之前执行的，当然还有一些其他类型的通知

#### 后置通知

切面类中新增一个后置通知方法，如下

```java
@AfterReturning(value = "execution(public String com.weilong.mapper.impl.MyInterfaceImpl.say())",returning = "res")
    public void afterReturning(Object res){
        System.out.println("方法的返回结果是："+res);
        SimpleDateFormat format = new SimpleDateFormat("YYYY-MM-dd HH:mm:ss");
        System.out.println("目标方法的执行时间为:"+format.format(new Date()));
}
```

@AfterReturning：

属性：value 值仍为切入点表达式

returning 自定义变量，表示目标方法的返回值

位置为通知方法的上面

特点：在目标方法执行之后执行，并且能够获取到目标方法的返回值，可以根据这个返回制作不同的处理功能并且可以`修改这个返回值`但是不会影响目标方法的执行结果

后置通知方法定义要求：

1. 公共方法（public）
2. 方法没有返回值
3. 方法有参数

执行say方法可以看到返回的结果如下：

![](https://fastly.jsdelivr.net/gh/code-anan/image/20211227143432.png)

#### 环绕通知

实现类中新增一个方法

```java
 @Override
    public Object returnStudent(int id, String name) {
        Student student = new Student();
        student.setId(id);
        student.setName(name);
        return student;
}
```

切面类中新增一个环绕通知方法：

```java
@Around(value = "execution(public Object com.weilong.mapper.impl.MyInterfaceImpl.returnStudent(int,String))")
    public Object myAround(ProceedingJoinPoint point) throws Throwable {
        SimpleDateFormat format = new SimpleDateFormat("YYYY-MM-dd HH:mm:ss");
        System.out.println("环绕通知，在执行目标方法之前的时间为:"+format.format(new Date()));
        Student student = (Student) point.proceed();
        student.setId(2);
        student.setName("张飞");
        return student;
}
```

环绕通知方法定义格式：

1. 公共方法（public）
2. 需要定义返回值类型（最好Object）
3. 方法有参数固定（ProceedingJoinPoint）

@Around:环绕通知 

属性：value 值为切入点表达式

位置：在方法的上面

特点:功能最强的通知，可以在目标方法的执行前后都增强功能，控制目标方法是否被调用执行，并且可以修改目标方法的执行结果并影响目标方法的执行结果

环绕通知：等同于jdk动态代理的InvocationHandler接口

参数：ProceedingJoinPoint，等同于Method，作用是执行目标方法的返回值并且可以对其修改

环绕通知经常用来做事务，在目标方法之前开启事务，执行目标方法，在目标方法执行之后提交事务

测试类中：

```java
public class Test {
    public static void main(String[] args) {
        String config="SpringConfig.xml";
        ApplicationContext context = new ClassPathXmlApplicationContext(config);
        MyInterface myservice = (MyInterface) context.getBean("myservice");
        //System.out.println(myservice.say());
       /* myservice.dosome("皮卡",20);*/
        Student student =(Student) myservice.returnStudent(1, "lucas");
        System.out.println(student);

    }
}
```

结果：![](https://fastly.jsdelivr.net/gh/code-anan/image/20211227151012.png)

可以看到在环绕通知中其结果已经被修改了

#### 异常通知（了解）

异常通知方法定义格式

1. 公共的（public）
2. 没有返回值
3. 方法参数有一个Exception，还可以添加一个JoinPoint

@AfterThrowing:异常通知

属性：value 值为切入点表达式

throwing自定义的变量，表示目标方法抛出的异常对象，变量名和方法的参数名一致

特点：1.在目标方法抛出异常时执行2.可以用来做异常的监控程序，监控目标方法是不是有异常

定义格式如下：

```java
 @AfterThrowing(value = "execution(public Object com.weilong.mapper.impl.MyInterfaceImpl.returnStudent(int,String))",throwing = "ex")
    public void myThowing(Exception ex) {
        System.out.println("异常通知，方法发生异常时执行了:"+ex.getMessage());
}
```

执行的效果等价于

```java
try {
           MyInterfaceImpl.returnStudent(int,String)
}catch (Exception ex){
                myThowing(ex);
}
```

#### 最终通知（了解）

最终通知方法的定义格式：

1. public

2. 没有返回值

3. 方法没有参数，如果有是JoinPoint


@After:最终通知

属性：value 值为切入点表达式

位置：方法的上面

特点：总是会执行，在目标方法执行之后执行，及时目标方法出现异常，也会执行

```java
@After(value = "execution(public Object com.weilong.mapper.impl.MyInterfaceImpl.returnStudent(int,String))")
    public void myAfter() {
        System.out.println("最后都会执行的代码");
        //一般都是关闭 清理的代码
}
```

### Pointcut注解

作用：定义和管理切入点，项目中有多个切入点表达式重复，复用使用pointcut

属性：value 值为切入点表达式

位置：自定义方法上面

特点：当使用@Pointcut定义在一个方法的上面，此时这个方法的名称就是切入点表达式的别名，其他通知中，value属性就可以使用这个方法名称，代替这个切入点表达式

```java
    @Pointcut(value = "*..   *com.weilong.mapper.impl")
    public void mypointcut(){

    }
    @After(value = "mypointcut()")
    public void myfianlafter(){

    }
    @Before(value = "mypointcut()")
    public void mybefore(){
        
    }
```

其实就是起到一个复用的作用

如果目标类没有接口，spring框架会自动启动cglib代理，如果目标类有接口，也可以使用cglib代理

spring配置文件设置如下

```xml
 <aop:aspectj-autoproxy proxy-target-class="true"/>
```

proxy-target-class="true"表示告诉spring框架使用cglib动态代理

# 集成mybatis框架

IOC技术可以把对象都交给spring容器统一管理，所以spring可以很好的把mybatis集成在一起，看起来像是一个框架，下面通过实例看如何集成

## 新建maven项目

![](https://fastly.jsdelivr.net/gh/code-anan/image/20211223150019.png)

同样需要手动创建resources文件夹

## 添加依赖

```xml
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
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
      <version>5.1.49</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>

    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.1</version>
    </dependency>
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.12</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>3.0.7.RELEASE</version>
    </dependency>
```

分别添加spring依赖、mybatis依赖、mysql驱动依赖、spring的事务依赖、spring集成mybatis的依赖（用来在spring项目中创建mybatis的SqlSessFactory和dao对象的）和druid连接池依赖，最后的`spring-jdbc`依赖不添加可能会报错

## 创建实体类

这里我们依然经典student即可注意属性需要跟数据库字段一致

```java
public class Student {
    private int id;
    private String name;
    private int age;

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

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

## 创建接口和对应的mapper文件

dao接口：

```java
public interface StudentDao {
    List<Student> queryAllStudent();
}
```

StudentDao.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.example.dao.StudentDao">
    <select id="queryAllStudent" resultType="org.example.model.Student">
       select id,name,age from student
    </select>
</mapper>
```

定义一个查询所有学生的接口

## 创建mybatis主配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

<mappers>
    <package name="org.example.dao"/>
</mappers>
</configuration>
```

## 创建service接口和实现类

service接口:

```java
public interface StudentService {
    List<Student> queryAllStudent();
}
```

我们这里同样写一个查询所有学生的接口

StudentServiceImpl:

```java
public class StudentServiceImpl implements StudentService {

    private StudentDao studentDao;

    public void setStudentDao(StudentDao studentDao) {
        this.studentDao = studentDao;
    }

    @Override
    public List<Student> queryAllStudent() {
        List<Student> studentList = studentDao.queryAllStudent();
        return studentList;
    }
}
```

这里定义一个studentDao的属性，目的是使用set注入

## 创建spring的配置文件

名字随意，同样放到resources目录下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">
        <!--声明数据源，作用是连接数据库-->
        <bean id="myDataSource" class="com.alibaba.druid.pool.DruidDataSource"
        init-method="init" destroy-method="close">
                <property name="url" value="jdbc:mysql://localhost:3306/lwl"/>
                <property name="username" value="root"/>
                <property name="password" value="233"/>
                <property name="maxActive" value="20"/>
        </bean>
        <!--创建sqlsessionFactory对象-->
        <!--mybatis中的SqlSessionFactoryBean类为我们创建sqlSessionFactory-->
        <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
                <!--数据库连接池交给dataSource属性-->
                <property name="dataSource" ref="myDataSource"/>
                <!--mybatis主配置文件的位置-->
                <property name="configLocation" value="classpath:mybatis.xml"/>
        </bean>

        <!--创建dao对象-->
        <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
                <!--指定sqlSessionFactory的id-->
                <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
                <!--指定dao接口的包名，为每个dao接口都创建一个dao对象放入到spring容器中 dao对象的默认名是接口名首字母小写-->
                <property name="basePackage" value="org.example.dao"/>
        </bean>
        
        <!--创建service对象-->
        <bean id="myservice" class="org.example.service.impl.StudentServiceImpl">
                <property name="studentDao" ref="studentDao"/>
        </bean>
</beans>
```

## 创建测试类

```java
public static void main(String[] args) {
        String config="SpringConfig.xml";
        //创建spring容器对象（ClassPathXmlApplicationContext通过从类路径中加载spring配置文件）
        ApplicationContext application = new ClassPathXmlApplicationContext(config);
        StudentService service= (StudentService) application.getBean("myservice");
        List<Student> studentList = service.queryAllStudent();
        for(Student student:studentList){
            System.out.println(student);
        }
        /*StudentDao dao= (StudentDao) application.getBean("studentDao");
        List<Student> studentList = dao.queryAllStudent();
        for(Student student:studentList){
            System.out.println(student);
        }*/
    }
```

测试类中 可以拿service对象也可以直接拿dao对象直接查询所得结果![](https://fastly.jsdelivr.net/gh/code-anan/image/20211228112256.png)

## 使用属性配置文件

在spring配置文件中添加

```xml
<context:property-placeholder location="classpath:jdbc.properties"/>
```

然后在resources目录下添加`jdbc.properties`文件

```properties
url=jdbc:mysql://localhost:3306/lwl
username=root
password=233
max=20
```

最后spring中数据库信息写法改为下面的方式即可

```xml
<property name="url" value="${url}"/>
<property name="username" value="${username}"/>
<property name="password" value="${password}"/>
<property name="maxActive" value="${max}"/>
```

# Spring的事务

## JDBC、mybatis、Hibernate如何处理事务

jdbc访问数据库处理事务：Connection conn;conn.commit(),conn.rollback();

mybatis访问数据库处理事务：SqlSession.commit(),SqlSession.rollback()

Hibernate访问数据库处理事务：

Session.commit(),Session.rollback()

可以看到多种数据库的访问技术，有不同的事务处理机制、不同的对象和方法，为了解决这种不足，所以推荐使用spring的事务

Spring提供一种处理事务的统一模型，能使用统一步骤和方式完成多种不同数据库访问技术的事务处理

## 事务的隔离级别

事务的隔离级别有4个值，mysql默认的隔离级别是REPEATBLE_READ;oracle的默认级别是READ_COMMITTED;

* READ_UNCOMMITTED：读未提交，未解决任何并发问题

* READ_COMMITTED:读已提交。解决脏读，存在不可重复读与幻读

* REPEATABLE_READ:可重复读。解决脏读，不可重复读、存在幻读

* SERIALIZABLE:串行化，不存在并发问题


## 事务的超过时间

表示一个方法最长的施行时间，如果方法执行时超过了此时间，事务就回滚，单位是秒，整数值默认为-1；

## 事务的传播行为

表示控制业务方法是不是有事务，是什么样的事务；

总共7个传播行为，表示你的业务方法调用时，事务在方法之间是如何使用的

* PROPAGATION_REQUIRED:指定的方法必须在事务内执行。若当前存在事务，就加入到当前事务中；若当前没有事务，则创建一个新事务；这种传播行为是最常见的选择，也是Spring默认的事务传播行为
* PROPAGATION_SUPPORTS：指定的方法支持当前事务，但若当前没有事务，也可以以非事务的方式执行，比如查询操作，一般不需要事务
* PROPAGATION_REQUIRES_NEW：总是新建一个事务，若当前存在事务，就将当前事务挂起，直到新事务执行完毕
* PROPAGATION_MANDATORY
* PROPAGATION_NESTED
* PROPAGATION_NEVER
* PROPAGATION_NOT_SUPPORT

## 回滚事务的时机

1. 业务方法执行成功，没有异常抛出方法执行完毕后，spring在方法执行后提交事务，事务管理器commit
2. 当业务方法抛出运行时异常或者error，spring执行回滚，调用事务管理器的rollback
3. 当业务方法抛出非运行时异常，主要是编译时异常时，提交事务

## 总结Spring的事务

1. 管理事务的是 事务管理和他的实现类

2. Spring的事务是一个统一模型

   2.1 指定要使用的事务管理器实现类，使用<bean>

   2.2 指定哪些类，哪些方法需要加入事务的功能

   2.3 指定方法需要的隔离级别、传播行为，超时

## Spring提供的事务处理方案

### 注解方案(中小型项目使用)

spring框架自己用aop实现给业务方法增加事务的功能，使用`@Transactional`注解增加事务

### 使用步骤

#### 声明事务管理器对象

spring配置文件中添加

```xml
<bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" id="dataSourceTransactionManager">
                <!--指定数据库-->
                <property name="dataSource" ref="myDataSource"/>
</bean>
```

#### 开启事务注解驱动

spring配置文件中添加

```xml
 <tx:annotation-driven transaction-manager="dataSourceTransactionManager"/>
```

作用是告诉spring框架，要使用注解的方式管理事务

PS：driver是以tx为结尾的

#### 添加@Transactional注解

```java
@Transactional(
            propagation = Propagation.REQUIRED,
            isolation = Isolation.DEFAULT,
            readOnly = false,
            rollbackFor = {
                    NullPointerException.class
            }
    )
    @Override
    public int insertStudent(Student student) {
        Student insertStudent = new Student();
        insertStudent.setId(student.getId());
        insertStudent.setName(student.getName());
        insertStudent.setAge(student.getAge());
        int i = studentDao.insertStudent(insertStudent);
        return i;
}
```

propagation:表示事务传播方式

isolation：隔离级别

rollbackFor:表示发生指定的异常一定进行回滚，首先会检查方法抛出的异常是不是在rollbackFor的属性中如果在之中不论是什么类型的异常一定会进行回滚如果不在之中，spring进行判断是不是运行时异常如果是一定回滚

这些Transactional的属性全部可以默认不写，默认传播方式为Propagation.REQUIRED，隔离级别为Default、默认抛出运行时异常事务回滚

### 适合大型项目的方式

#### 添加aspectJ依赖

```xml
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>5.3.14</version>
</dependency>
```

#### 声明事务管理器

spring配置文件中添加

```xml
<bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager" id="dataSourceTransactionManager">
                <!--指定数据库-->
                <property name="dataSource" ref="myDataSource"/>
        </bean>
```

#### 声明方法需要的事务类型

配置方法的事务属性（隔离级别、传播行为、超时等）

```xml
<tx:advice transaction-manager="dataSourceTransactionManager" id="interceptor">
                <tx:attributes>
                        <tx:method name="queryAllStudent" propagation="REQUIRED" isolation="DEFAULT"
                        rollback-for="NullPointerException.class"/>
                        <tx:method name="insertStudent" propagation="REQUIRES_NEW"/>
                </tx:attributes>
</tx:advice>
```

tx:advice:

 id：自定义名称唯一标识

transaction-manager：事务管理器的id

tx:method：给具体的方法配置事务属性，可以有多个

name：方法名称，不带有包和类并且可以使用通配符，*表示任意字符

propagation：传播行为

isolation：隔离级别

rollback-for：表示发生指定的异常一定进行回滚，首先会检查方法抛出的异常是不是在rollbackFor的属性中如果在之中不论是什么类型的异常一定会进行回滚如果不在之中，spring进行判断是不是运行时异常如果是一定回滚

#### 配置AOP

指定哪些类要创建代理  同样是Spring配置文件中指定

```xml
<aop:config>
                <aop:pointcut id="servicept" expression="execution(* *..service..*.*(..))"/>
                <aop:advisor advice-ref="interceptor" pointcut-ref="servicept"></aop:advisor>
</aop:config>
```

aop:pointcut:配置切入点表达式，指定哪些包类要使用事务

aop:advisor：配置增强器：关联advice和pointcut

advice-ref为上面tx:advice的id值

pointcut-ref为切入点表达式的id

这种方式可以为具体的哪些包哪些类和方法设置怎样的事务属性

# Spring与web

web项目在Tomcat服务器上运行，tomcat一旦启动、项目一直运行不可能还像javase那样创建classPathXmlApplicationContext对象，下面介绍如何在web项目中使用容器对象

##  创建web项目

![](https://fastly.jsdelivr.net/gh/code-anan/image/20211230184240.png)

创建一个webapp项目，需要手动添加java跟resources目录

##  添加依赖

```xml
<!--spring核心ioc-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
    <!--做spring事务用到的-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
    <!--mybatis依赖-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.1</version>
    </dependency>
    <!--mybatis和spring集成的依赖-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>1.3.1</version>
    </dependency>
    <!--mysql驱动-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.9</version>
    </dependency>
    <!--阿里公司的数据库连接池-->
    <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.12</version>
    </dependency>
    <!-- servlet依赖 -->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
    <!-- jsp依赖 -->
    <dependency>
      <groupId>javax.servlet.jsp</groupId>
      <artifactId>jsp-api</artifactId>
      <version>2.2.1-b03</version>
      <scope>provided</scope>
    </dependency>

    <!--为了使用监听器对象，加入依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
```

##  创建dao和映射文件 

dao接口：

```java
public interface StudentDao {
    int insertStudent(Student student);
    List<Student> selectStudents();
}

```

映射文件:

```xml
<mapper namespace="com.bjpowernode.dao.StudentDao">

   <insert id="insertStudent" parameterType="Student">
        insert into student (id,name,email,age) values(#{id},#{name},#{email},#{age})
    </insert>

    <select id="selectStudents" resultType="Student">
        select id,name,email,age from student order by id desc
    </select>
</mapper>

```

## 创建实体类对象

依然经典student即可

```java
public class Student {
    //属性名和列名一样。
    private Integer id;
    private String name;
    private String email;
    private Integer age;

    public Student() {
    }

    public Student(Integer id, String name, String email, Integer age) {
        this.id = id;
        this.name = name;
        this.email = email;
        this.age = age;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", email='" + email + '\'' +
                ", age=" + age +
                '}';
    }
}

```

## 创建service接口和实现类

```java
public interface StudentService {

    int addStudent(Student student);
    List<Student> queryStudents();
}
```

```java
public class StudentServiceImpl implements StudentService {

    //引用类型
    private StudentDao studentDao;

    //使用set注入，赋值
    public void setStudentDao(StudentDao studentDao) {
        this.studentDao = studentDao;
    }

    @Override
    public int addStudent(Student student) {
        int nums = studentDao.insertStudent(student);
        return nums;
    }

    @Override
    public List<Student> queryStudents() {
        List<Student> students = studentDao.selectStudents();
        return students;
    }
}
```

## mybatis配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--settings：控制mybatis全局行为-->
    <settings>
        <!--设置mybatis输出日志-->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <!--设置别名-->
    <typeAliases>
        <!--name:实体类所在的包名
            表示com.bjpowernode.domain包中的列名就是别名
            你可以使用Student表示com.bjpowenrode.domain.Student
        -->
        <package name="com.bjpowernode.domain"/>
    </typeAliases>


    <!-- sql mapper(sql映射文件)的位置-->
    <mappers>
        <!--
          name：是包名， 这个包中的所有mapper.xml一次都能加载
        -->
        <package name="com.bjpowernode.dao"/>
    </mappers>
</configuration>

```

## 创建spring主配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--
       把数据库的配置信息，写在一个独立的文件，编译修改数据库的配置内容
       spring知道jdbc.properties文件的位置
    -->
    <context:property-placeholder location="classpath:jdbc.properties" />

    <!--声明数据源DataSource, 作用是连接数据库的-->
    <bean id="myDataSource" class="com.alibaba.druid.pool.DruidDataSource"
          init-method="init" destroy-method="close">
        <!--set注入给DruidDataSource提供连接数据库信息 -->
        <!--    使用属性配置文件中的数据，语法 ${key} -->
        <property name="url" value="${jdbc.url}" /><!--setUrl()-->
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.passwd}" />
        <property name="maxActive" value="${jdbc.max}" />
    </bean>

    <!--声明的是mybatis中提供的SqlSessionFactoryBean类，这个类内部创建SqlSessionFactory的
        SqlSessionFactory  sqlSessionFactory = new ..
    -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--set注入，把数据库连接池付给了dataSource属性-->
        <property name="dataSource" ref="myDataSource" />
        <!--mybatis主配置文件的位置
           configLocation属性是Resource类型，读取配置文件
           它的赋值，使用value，指定文件的路径，使用classpath:表示文件的位置
        -->
        <property name="configLocation" value="classpath:mybatis.xml" />
    </bean>

    <!--创建dao对象，使用SqlSession的getMapper（StudentDao.class）
        MapperScannerConfigurer:在内部调用getMapper()生成每个dao接口的代理对象。

    -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--指定SqlSessionFactory对象的id-->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
        <!--指定包名， 包名是dao接口所在的包名。
            MapperScannerConfigurer会扫描这个包中的所有接口，把每个接口都执行
            一次getMapper()方法，得到每个接口的dao对象。
            创建好的dao对象放入到spring的容器中的。 dao对象的默认名称是 接口名首字母小写
        -->
        <property name="basePackage" value="com.bjpowernode.dao"/>
    </bean>

    <!--声明service-->
    <bean id="studentService" class="com.bjpowernode.service.impl.StudentServiceImpl">
        <property name="studentDao" ref="studentDao" />
    </bean>

</beans>
```

## 对应的属性文件

```properties
jdbc.url=jdbc:mysql://localhost:3306/lwl
jdbc.username=root
jdbc.passwd=233
jdbc.max=30
```

## 创建前端页面

为了方便直接就jsp页面了

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<p>注册学生</p>
<form action="register" method="post">
    <table>
        <tr>
            <td>id</td>
            <td><input type="text" name="id"></td>
        </tr>
        <tr>
            <td>姓名：</td>
            <td><input type="text" name="name"></td>
        </tr>
        <tr>
            <td>email:</td>
            <td><input type="text" name="email"></td>
        </tr>
        <tr>
            <td>年龄：</td>
            <td><input type="text" name="age"></td>
        </tr>
        <tr>
            <td></td>
            <td><input type="submit" value="注册学生"></td>
        </tr>
    </table>
</form>
</body>
</html>
```

##  创建servlet

这里还没到springmvc 所以先用servlet 小技巧 ![](https://fastly.jsdelivr.net/gh/code-anan/image/20211230192756.png)

这样新建可以直接在web.xml中 直接生成一个servlet并且还会自动继承`HttpServlet`,编写业务代码

```java
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        Student student = new Student();
        student.setId(Integer.valueOf(request.getParameter("id")));
        student.setName(request.getParameter("name"));
        student.setAge(Integer.valueOf(request.getParameter("age")));
        student.setEmail(request.getParameter("email"));
        String config="applicationContext.xml";
        ApplicationContext context = new ClassPathXmlApplicationContext(config);
        StudentService studentService = (StudentService) context.getBean("studentService");
        studentService.addStudent(student);
        request.getRequestDispatcher("/result.jsp").forward(request,response);
```

这里我们创建spring容器对象创建出了service对象，然后执行添加学生的操作执行之后页面跳转到result.jsp中

## 设置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <servlet>
        <servlet-name>RegisterServlet</servlet-name>
        <servlet-class>com.bjpowernode.servlet.RegisterServlet</servlet-class>
    </servlet>
    
    <servlet-mapping>
        <servlet-name>RegisterServlet</servlet-name>
        <url-pattern>/register</url-pattern>
    </servlet-mapping>
</web-app>
```

在这里声明刚刚创建的servlet

## 测试

![](https://fastly.jsdelivr.net/gh/code-anan/image/20211230194608.png)

![](https://fastly.jsdelivr.net/gh/code-anan/image/20211231190218.png)![](https://fastly.jsdelivr.net/gh/code-anan/image/20211231190253.png)

## 配置监听器

web项目中容器对象只需要创建一次，把容器对象放到全局作用域ServletContext中，这里就要用到监听器

监听器作用：创建容器对象，把对象放入到ServletContext中

监听器可以自己创建，也可以使用框架中提供好的ContextLoaderListener



### 添加依赖

上面我们已经添加过了

```xml
<!--为了使用监听器对象，加入依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
```

### 注册监听器

在我们的web.xml文件中添加

```xml
<!--注册监听器ContextLoaderListener-->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
```

监听器被创建对象后，默认会读取/WEB-INF/applicationContext.xml文件，因为我们spring的配置文件是放在resources目录下所以我们要修改默认的文件位置，使用context-param重新指定文件的位置

```xml
 <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
</context-param>
```

### servlet代码修改

使用上述方式，我们就不用每次都创建容器对象了

```java
        request.setCharacterEncoding("utf-8");
        response.setCharacterEncoding("utf-8");
        Student student = new Student();
        student.setId(Integer.valueOf(request.getParameter("id")));
        student.setName(request.getParameter("name"));
        student.setAge(Integer.valueOf(request.getParameter("age")));
        student.setEmail(request.getParameter("email"));
        String key= WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE;
        WebApplicationContext webApplicationContext= (WebApplicationContext) getServletContext().getAttribute(key);
        StudentService studentService = (StudentService) webApplicationContext.getBean("studentService");
        studentService.addStudent(student);
        request.getRequestDispatcher("/result.jsp").forward(request,response);
```

配置监听器的目的是创建容器对象，创建了容器对象，就能把spring配置文件中的所有对象都创建好，用户发起请求就可以直接使用对象了