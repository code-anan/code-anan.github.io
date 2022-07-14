title: 记录一下JDBC使用及遇到的问题
author: lwl
date: 2021-12-07 13:31:20
tags:
  - JDBC
  - 数据库
categories:
  - 数据库
my: post/jdbc
cover: https://gcore.jsdelivr.net/gh/code-anan/image/20211207142826.png
---
# 前言
`JDBC`其实已经很久没用到了，但是其原理还是有必要了解的，作为我们初期用java语言与数据库交互的工具，第一次见到它还是会觉得有些“神奇的”，但是今天闲来无事复习的时候发现了一个小问题，真是温故而知新。所以在这里记录一下以及如何使用它

# jar包的引入
* 首先是jar包的下载，使用它我们才能和数据库连接交互（跟我们做maven项目导入依赖原理一样的）
百度网盘链接：`https://pan.baidu.com/s/1GGuWeJDyyGUXSAZ_9jPuuA `
提取码：`l9jb`
* 下载之后就可以在java项目中引入刚才下载的jar包（`Project Structure`->`+`->`mysql-connector-java-5.1.46.jar`->`ok`）
![](https://gcore.jsdelivr.net/gh/code-anan/image/20211207134155.png)

# 使用JDBC
jar包引入成功之后我们就可以使用了，它的使用总共可分为六步
##  注册驱动
使用不同的数据库有着不同的驱动，这里以我本地的mysql为例
```java
DriverManager.registerDriver(new com.mysql.jdbc.Driver());
```
或者是使用ResourceBundle类读取属性文件 看起来更加优雅同时符合oop降低耦合的特性（下面的案例都按照这种方式写）
```java
ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
String driver = bundle.getString("driver");
Class.forName(driver);
```
注意这里的`jdbc.properties`用这种方式引入的话需要放在src下，否则需要调整路径，这里我的`jdbc.properties`文件内容为：
```
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/lwl?useUnicode=true&characterEncoding=UTF8
user=root
password=233
```
## 获取连接 
即打开JVM和数据库进程间的通道，后面需要关闭
```
String url = bundle.getString("url");
String user = bundle.getString("user");
String password = bundle.getString("password");
Connection connection=null;
connection=DriverManager.getConnection(url,user,password);
```
##  获取数据库操作对象
这里操作对象有两种 第一种是Statement类 第二种是PreparedStatement
但是Statement类会有`sql`注入的危险一般情况不会用，这里用PreparedStatement演示
```
PreparedStatement statement=null;
String sql="select ename,job,sal from emp where sal >?";
statement= connection.prepareStatement(sql);
statement.setInt(1,1000);
```
需要注意的是这里的给占位符？赋值时需要从1开始

## 执行sql语句
```
ResultSet resultSet = statement.executeQuery();
```
这里是查询 如果是DML语句应该用executeUpdate()方法
## 处理查询结果集
如果是DML语句则没有这一步(这里仅遍历输出一下)
```
while (resultSet.next()){
    String ename = resultSet.getString("ename");
    String job = resultSet.getString("job");
    System.out.println("职工名称为:"+ename+","+"职位为:"+job);
}
```
## 释放资源
```
if(connection!=null){
                try {
                    connection.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }else if(statement!=null){
                try {
                    statement.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
```
连在一起的代码就是
```
public class JDBCTest {
    public static void main(String[] args) {
        ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        Connection connection = null;
        PreparedStatement statement = null;

        try {
            //1.注册驱动
            //DriverManager.registerDriver(new com.mysql.jdbc.Driver());
            Class.forName(driver);

            //2.获取连接
            connection = DriverManager.getConnection(url, user, password);

            //3.获取数据库操作对象
            String sql = "select ename,job,sal from emp where sal >?";
            statement = connection.prepareStatement(sql);
            statement.setInt(1, 1000);

            //4.执行sql
            ResultSet resultSet = statement.executeQuery();

            //5.处理查询结果集
            while (resultSet.next()) {
                String ename = resultSet.getString("ename");
                String job = resultSet.getString("job");
                System.out.println("职工名称为:" + ename + "," + "职位为:" + job);
            }
        } catch (SQLException throwables) {
            throwables.getErrorCode();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            //6.释放资源
            if (connection != null) {
                try {
                    connection.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            } else if (statement != null) {
                try {
                    statement.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }


    }
}
```
以及`jdbc.properties`文件的内容
```
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/lwl?useUnicode=true&characterEncoding=UTF8
user=root
password=233
```
上述查询的最终结果(表为安装mysql或者oracle自带的emp表)
```
职工名称为:SMITH,工作为:CLERK
职工名称为:ALLEN,工作为:SALESMAN
职工名称为:WARD,工作为:SALESMAN
职工名称为:JONES,工作为:MANAGER
职工名称为:MARTIN,工作为:SALESMAN
职工名称为:BLAKE,工作为:MANAGER
职工名称为:CLARK,工作为:MANAGER
职工名称为:SCOTT,工作为:ANALYST
职工名称为:KING,工作为:PRESIDENT
职工名称为:TURNER,工作为:SALESMAN
职工名称为:ADAMS,工作为:CLERK
职工名称为:JAMES,工作为:CLERK
职工名称为:FORD,工作为:ANALYST
职工名称为:MILLER,工作为:CLERK
```
# 今天遇到的问题
问题其实也很简单，就是如果把上述的sql改成`String sql = "select ?,?   from emp ";`使用setString(1,"ename") 和setString(2,"job") 的时候会发现返回的结果集全部为列名`ename`和`job`，最后查到原因是使用这种方法给占位符赋值时，它的sql语句会变成`select 'ename','job'from emp`,这样返回的结果肯定是列名了,所以我们用它给占位符赋值的时候只能在后面的条件赋值，这样才不会出现这种问题，这也算是jdbc的一个弊端了吧，所以后来才会出现mybatis等优秀的框架吧
