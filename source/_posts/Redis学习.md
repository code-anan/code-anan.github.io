tags:
  - Redis
  - 数据库
categories:
  - 教程
cover: https://cdn.jsdelivr.net/gh/code-anan/image/20220627220835.png
my: post/rediskeys
title: Redis学习
date: 2022-06-27 22:07
------

# Nosql概述

## 产生原因

用户的个人信息、社交网络、地理位置、用户自己产生的数据用户胡日志产生爆发式增长、这时候就需要使用nosql数据库，他可以很好的处理上面的情况

## 概念

NoSQL=Not Only Sql(不仅仅是sql)  泛指非关系型数据库

## 特点

1. 方便扩展（数据之前没有关系、好扩展）

2. 大数据量高性能（Redis一秒写入把万次 读十一万次）

3. 数据类型是多样型的（不需要事先设计数据库，随取随用）
4. 传统的RDBMS和Nosql

# Redis概述

## 介绍

Redis（Remote Dictionary Server )，即远程字典服务，是一个开源的使用ANSI [C语言](https://baike.baidu.com/item/C语言)编写、支持网络、可基于内存亦可持久化的日志型、Key-Value[数据库](https://baike.baidu.com/item/数据库/103728)，并提供多种语言的API。是当下最热门的nosql数据 称之为结构化数据库

> Redis可以做什么

1. 内存存储、持久化，内存中是断电即失、所以说持久化很重要
2. 效率高，可以用于高速缓存
3. 发布订阅系统
4. 地图信息分析
5. 计时器 计数器

> 特性

1. 多样的数据类型
2. 持久化
3. 集群
4. 事务

Redis推荐在linux服务器上搭建、windows版本的已经停止更新

## windows安装

首先先进行[下载](https://github.com/MicrosoftArchive/redis/releases)

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220618160926.png)

然后解压到本地即可

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220618161242.png)

双击`redis-server.exe`即可本地启动，这表示启动了redis服务

然后双击`redis-cli.exe`可以登录客户端（上面的服务不要关闭）

![](https://cdn.jsdelivr.net/gh/code-anan/image/20220618161819.png)

ping是测试有没有连接成功 下面就是对他的赋值取值简单操作

## Linux安装

首先下载的话直接到[Redis中文网](https://www.redis.net.cn/)即可![](https://cdn.jsdelivr.net/gh/code-anan/image/20220618162710.png)

安装的话 直接看pdf文档即可https://www.aliyundrive.com/s/tX5SmNKbAwP 有非常详细的步骤

常用的命令有` tar zxvf /root/redis-5.0.8.tar.gz -C ./`解压到当前路径

`make && make install`编译并安装、默认安装路径`usr/local/bin`

redis安装目录utils目录下`sh install_server.sh`进行启动或者`redsi-server redis配置文件`(需要修改daemonize 为yes)启动Redis

`redis -cli`使用客户端进行连接  `systemctl status redis_6379.service`查看启动的结果

`systemctl restart  redis_6379.service`重新启动

## redis-benchmark

redis自带的测试工具，目录在`usr/local/bin`  ，可以自定义参数 了解一下即可![image-20220619112228814](https://cdn.jsdelivr.net/gh/code-anan/image/20220619112235.png)

## 基础知识

首先redis默认的是有16个数据库（配置文件中可以查看），使用select进行切换，例如我想切换到第四个数据库

```sql
127.0.0.1:6379[6]> select 3
OK
127.0.0.1:6379[3]> dbsize
(integer) 0
127.0.0.1:6379[3]> set name lwl
OK
127.0.0.1:6379[3]> get name
"lwl"
127.0.0.1:6379[3]> select 6
OK
127.0.0.1:6379[6]> dbsize
(integer) 0
127.0.0.1:6379[6]> 
```

可以看到不同的数据库之间不能共享数据

```sql
127.0.0.1:6379[3]> set password 233
OK
127.0.0.1:6379[3]> keys *
1) "password"
127.0.0.1:6379[3]> flushdb
OK
127.0.0.1:6379[3]> keys *
(empty list or set)
127.0.0.1:6379[3]> 
```

`flushdb`清空当前数据库

`flushall`清空全部数据库

> Redis是单线程的！

redis基于内存操作，cpu不是redis性能瓶颈而是机器的内存和网络贷款并且是使用单线程的；

核心：redis是将所有的数据全部放在内存中的  所以使用单线程去操作效率就是最高的 多线程cpu上下文会进行切换这是一种耗时的操作，对于内存系统来说 没有上下文切换效率就是最高的 多次读写都在一个cpu上

# 五大数据类型

Redis可以用作数据库、缓存和消息中间件；

## Redis-key

```sql
127.0.0.1:6379[3]> set name lwl
OK
127.0.0.1:6379[3]> keys *
1) "name"
127.0.0.1:6379[3]> exists name
(integer) 1
127.0.0.1:6379[3]> move name 2
(integer) 1
127.0.0.1:6379[3]> exists name
(integer) 0
127.0.0.1:6379[3]> select 2
OK
127.0.0.1:6379[2]> keys *
1) "name"
127.0.0.1:6379[2]> get name
"lwl"
```

`exists key`是否存在某个key

`move key db`将某个key移动到别的数据库里

```sql
127.0.0.1:6379[2]> EXPIRE name 5
(integer) 1
127.0.0.1:6379[2]> get name
"lwl"
127.0.0.1:6379[2]> get name
(nil)
127.0.0.1:6379[2]> ttl name
(integer) -2
127.0.0.1:6379[2]> del name
(integer) 1
127.0.0.1:6379[2]> set name lwl
OK
127.0.0.1:6379[2]> type name
string

```

`EXPIRE key seconds`给某个key设置过期时间 单位为秒

`ttl key`可以查看剩余有效的时间

`del key`删除某个key

`type key`查看key的数据类型

其他更多命令可以点击这里[redis命令手册](https://www.redis.net.cn/order/)查看

## String类型

```sql
127.0.0.1:6379> set name lwl
OK
127.0.0.1:6379> APPEND name dog
(integer) 6
127.0.0.1:6379> get name
"lwldog"
127.0.0.1:6379> strlen name
(integer) 6
127.0.0.1:6379> APPEND name1 zhangsan
(integer) 8
127.0.0.1:6379> get name1
"zhangsan"
127.0.0.1:6379> keys *
1) "name1"
2) "name"
127.0.0.1:6379> 
```

`append key value`key存在时往后追加、key不存在时相当于set一个key

`strlen key`获取某个key的长度

```sql
127.0.0.1:6379> set view 0
OK
127.0.0.1:6379> incr view
(integer) 1
127.0.0.1:6379> incrby view 2
(integer) 3
127.0.0.1:6379> get view
"3"
127.0.0.1:6379> decr view
(integer) 2
127.0.0.1:6379> DECRBY view 2
(integer) 0
```

`incr key`某个key每次增加1	

`incrby key nums`某个key每次增加nums

`decr key`某个key每次减少1	

`decrby key nums`某个key每次减少nums

```sql
127.0.0.1:6379> set key1 hello
OK
127.0.0.1:6379> getrange key1 0 2
"hel"
127.0.0.1:6379> getrange key1 0 -1
"hello"
127.0.0.1:6379> SETRANGE key1 1 xx
(integer) 5
127.0.0.1:6379> get key1
"hxxlo"
```

`getrange key`类似于java的substrig

-1表示倒数第一个 所以可以截取全部

`setrange key offset value`替换操作，表示从第几位开始替换为某个值

```sql
127.0.0.1:6379> setex key2 10 hello
OK
127.0.0.1:6379> ttl key2
(integer) 3
127.0.0.1:6379> get key2
(nil)
127.0.0.1:6379> setnx key2 hello
(integer) 1
127.0.0.1:6379> get key2
"hello"
127.0.0.1:6379> 
```

`setex key seconds value`表示设置key的值以及过期时间(set with expire)

`setnx key value`表示key不存在时才会设置key值（分布式锁中常使用）(set if not exist)

```sql
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3
OK
127.0.0.1:6379> mget k1 k2 k3
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 v1 k4 v4
(integer) 0
127.0.0.1:6379> get v4
(nil)
127.0.0.1:6379> getset redis monogocb
(nil)
127.0.0.1:6379> get redis
"monogocb"
```

批量赋值取值`mset`和`mget`

`msetnx`跟上面一样表示不存在时才创建，但同时他是个原子性操作k1以及存在了 所以直接不执行

`getset`先get再执行set， get的值为空也一样会set操作

String的使用场景：value除了是我们的字符串还可以是我们的数字

计数器、统计多单位的数量、粉丝数、对象缓存存储！

## List类型

```sql
127.0.0.1:6379> lpush list 1  #从左边插入第一个值
(integer) 1
127.0.0.1:6379> lpush list 2
(integer) 2
127.0.0.1:6379> lpush list 3
(integer) 3
127.0.0.1:6379> lrange list 0 -1
1) "3"
2) "2"
3) "1"
127.0.0.1:6379> rpush list 4  #从右边插入第一个值
(integer) 4
127.0.0.1:6379> lrange list 0 -1
1) "3"
2) "2"
3) "1"
4) "4"
127.0.0.1:6379> lpop list  #移除list第一个元素
"3"
127.0.0.1:6379> rpop list  #移除list最后一个元素
"4"
127.0.0.1:6379> lrange list 0 -1
1) "2"
2) "1"
127.0.0.1:6379> lindex list 0  #通过下标获取list某一个值
"2"
127.0.0.1:6379> llen list   #获取list的长度
(integer) 2

```

```sql
127.0.0.1:6379> lrange list 0 -1
1) "three"
2) "three"
3) "two"
4) "one"
127.0.0.1:6379> lrem list 2 three
(integer) 2
127.0.0.1:6379> lrange list 0 -1
1) "two"
2) "one"
```

`lrem key count value`从某个list移除count个值为value的值，按照从上往下的顺序移除（可以理解成堆栈）

```sql
127.0.0.1:6379> rpush mylist hello
(integer) 1
127.0.0.1:6379> rpush mylist hello1
(integer) 2
127.0.0.1:6379> rpush mylist hello2
(integer) 3
127.0.0.1:6379> rpush mylist hello3
(integer) 4
127.0.0.1:6379> ltrim mylist 1 2
OK
127.0.0.1:6379> lrange mylist 0 -1
1) "hello1"
2) "hello2"
```

`ltrim list start stop`从起始结束位置删除list中的元素

```sql
127.0.0.1:6379> rpush mylist hello
(integer) 1
127.0.0.1:6379> rpush mylist hello1
(integer) 2
127.0.0.1:6379> rpush mylist hello2
(integer) 3
127.0.0.1:6379> rpoplpush mylist otherlist
"hello2"
127.0.0.1:6379> lrange mylist 0 -1
1) "hello"
2) "hello1"
127.0.0.1:6379> lrange otherlist 0 -1
1) "hello2"
```

`rpoplpush list1 list2`移除list1中的最后一个元素并添加到list2中

```sql
127.0.0.1:6379> lpush list item1 item2
(integer) 2
127.0.0.1:6379> lrange list 0 -1
1) "item2"
2) "item1"
127.0.0.1:6379> lset list 0 item3
OK
127.0.0.1:6379> lrange list 0 -1
1) "item3"
2) "item1"
```

`lset list start value`更新list中下表为start的值为value

```sql
127.0.0.1:6379> lrange list 0 -1
1) "item3"
2) "item1"
127.0.0.1:6379> LINSERT list before item3 item4
(integer) 3
127.0.0.1:6379> lrange list 0 -1
1) "item4"
2) "item3"
3) "item1"
127.0.0.1:6379> LINSERT list after item3 item2
(integer) 4
127.0.0.1:6379> lrange list 0 -1
1) "item4"
2) "item3"
3) "item2"
4) "item1"
```

`linsert list before|after pivot value`元素pivot前面before或者后面after插入一个元素值为value

> 小结：
>
> ​		list本质是一个链表，左右都可以插入
>
> ​		在两边插入或者改动值 效率最高 中间元素 相对来说效率低一点

## Set（无序集合）

set中的值不能重复

```sql
127.0.0.1:6379> sadd myset "member1"  #往集合中添加元素
(integer) 1
127.0.0.1:6379> sadd myset "member2"
(integer) 1
127.0.0.1:6379> sadd myset "member3"
(integer) 1
127.0.0.1:6379> smembers myset  # 查看指定set的所有元素
1) "member3"
2) "member1"
3) "member2"
127.0.0.1:6379> sadd myset "member3"
(integer) 0
127.0.0.1:6379> smembers myset
1) "member3"
2) "member1"
3) "member2"
127.0.0.1:6379> SISMEMBER myset member2  # 判断元素是否在set中
(integer) 1
127.0.0.1:6379> scard myset  # 查看set集合中的元素个数
(integer) 3
127.0.0.1:6379> srem myset member2  # 移除某个元素
(integer) 1
127.0.0.1:6379> smembers myset
1) "member3"
2) "member1"
127.0.0.1:6379> SRANDMEMBER myset  # 随机选出一个元素
"member3"
127.0.0.1:6379> SRANDMEMBER myset 2 # 随机选出二个元素
1) "member3"
2) "member1"
```

```sql
127.0.0.1:6379> SMEMBERS myset
1) "member3"
2) "member1"
3) "lala"
4) "hello"
127.0.0.1:6379> spop myset 2
1) "lala"
2) "hello"
127.0.0.1:6379> SMEMBERS myset
1) "member3"
2) "member1"
```

`spop key count`随机弹出key集合中的count个元素

```sql
127.0.0.1:6379> SMEMBERS myset
1) "member3"
2) "member1"
127.0.0.1:6379> SMOVE myset myset2 member3
(integer) 1
127.0.0.1:6379> smembers myset2
1) "member3"
```

`smove source target member`将source集合中的member元素移动到target集合中

```sql
127.0.0.1:6379> sadd set1 a
(integer) 1
127.0.0.1:6379> sadd set1 b
(integer) 1
127.0.0.1:6379> sadd set1 c
(integer) 1
127.0.0.1:6379> sadd set2 c
(integer) 1
127.0.0.1:6379> sadd set2 d
(integer) 1
127.0.0.1:6379> sadd set2 e
(integer) 1
127.0.0.1:6379> sdiff set1 set2
1) "b"
2) "a"
127.0.0.1:6379> sinter set1 set2
1) "c"
127.0.0.1:6379> sunion set1 set2
1) "b"
2) "a"
3) "c"
4) "d"
5) "e"
```

分别表示差集 交集和并集  一眼就能看出来了

## Hash（集合）

Map集合，他的值是一个键值对的集合，key-map

```sql
127.0.0.1:6379> hset myhash name zhangsan
(integer) 1
127.0.0.1:6379> hget myhash name
"zhangsan"
127.0.0.1:6379> hmset myhash name zhangsan age 18
OK
127.0.0.1:6379> hmget myhash name age
1) "zhangsan"
2) "18"
127.0.0.1:6379> hgetall myhash
1) "name"
2) "zhangsan"
3) "age"
4) "18"
```

跟String相似 只是value是一个key-value类型 这里多了一个hgetall 也是很清晰

```sql
127.0.0.1:6379> hdel myhash name
(integer) 1
127.0.0.1:6379> hgetall myhash
1) "age"
2) "18"
127.0.0.1:6379> hlen myhash
(integer) 1
```

删除某个值和查看元素个数

```sql
127.0.0.1:6379> HEXISTS myhash name
(integer) 0
127.0.0.1:6379> HEXISTS myhash age
(integer) 1
```

判断hash某个元素的key是否存在

```sql
127.0.0.1:6379> hkeys myhash
1) "age"
2) "name"
127.0.0.1:6379> HVALS myhash
1) "18"
2) "lwl"
```

获取所有的key和value值

```sql
127.0.0.1:6379> hincrby myhash age 2
(integer) 20
127.0.0.1:6379> hsetnx myhash name lwl2
(integer) 0
```

同样和String类似的还有指定增量 如果有值就不更新的操作 类别String就可以 

Hash更适合对象的存储

## Zset(有序集合)

在set的基础上，增加一个排序的值

```sql
127.0.0.1:6379> zadd salary 5000 zhangsan
(integer) 1
127.0.0.1:6379> zadd salary 6500 lisi
(integer) 1
127.0.0.1:6379> zadd salary 5500 wangwu
(integer) 1
127.0.0.1:6379> zrangebyscore salary -inf +inf
1) "zhangsan"
2) "wangwu"
3) "lisi"
```

`zrangebyscore set start stop `按照score（添加元素的第三项）在start到stop范围内的排序

```sql
127.0.0.1:6379> zrange myset 0 -1
1) "xiaohong"
2) "xiaoming"
3) "xiaolan"
127.0.0.1:6379> ZRANGEBYSCORE myset 60 90
1) "xiaoming"
2) "xiaolan"
127.0.0.1:6379> ZRANGEBYSCORE myset -inf +inf withscores
1) "xiaohong"
2) "50"
3) "xiaoming"
4) "60"
5) "xiaolan"
6) "70"
```

`withscores`排序并且携带成绩值

```sql
127.0.0.1:6379> zrevrange myset 0 -1
1) "xiaolan"
2) "xiaoming"
3) "xiaohong"
```

降序排列`zrevrange`

```sql
127.0.0.1:6379> zrange myset 0 -1
1) "xiaohong"
2) "xiaoming"
3) "xiaolan"
127.0.0.1:6379> zrem myset xiaohong
(integer) 1
127.0.0.1:6379> zcard myset
(integer) 2
```

`zrem`移除某个元素，以及查看集合中的元素的个数

```sql
127.0.0.1:6379> zcount myset 20 90
(integer) 2
```

`zcount myset min max`统计myset集合中score的值再min和max之间的个数，其余的API在有需要的时候进行查看使用

# 三种特殊类型

## geospatial(地理位置)

这个功能可以推算地理位置信息、两地距离；

在这里可以进行[城市经纬度查询-国内城市经度纬度在线查询工具 (jsons.cn)](http://www.jsons.cn/lngcode/)

```sql
127.0.0.1:6379> geoadd china:city 116.40 39.90 beijing
(integer) 1
127.0.0.1:6379> geoadd china:city 121.47 31.23 shanghai
(integer) 1
127.0.0.1:6379> geoadd china:city 106.50 29.53 chongqing
(integer) 1
```

key为china：city ，value分别为经度 纬度 名称

有效的经度值为-180到180度 纬度为-85到85度 超过这两个值的话就会报错

```sql
127.0.0.1:6379> geopos china:city beijing
1) 1) "116.39999896287918091"
   2) "39.90000009167092543"
127.0.0.1:6379> geopos china:city shanghai
1) 1) "121.47000163793563843"
   2) "31.22999903975783553"
127.0.0.1:6379> 
```

geopos获取某个member的经纬度

```sql
127.0.0.1:6379> geodist china:city beijing shanghai m
"1067378.7564"
127.0.0.1:6379> geodist china:city beijing shanghai km
"1067.3788"
```

`geodist key member1 member2 unit`获取member

1和member2之间的距离，单位为unit可以写m（米）、km（千米）

```sql
127.0.0.1:6379> georadius china:city 130 30 1000 km
1) "shanghai"
127.0.0.1:6379> georadius china:city 130 30 10000 km
1) "chongqing"
2) "shanghai"
3) "beijing"
127.0.0.1:6379> georadius china:city 130 30 10000 km withdist withcoord count 1
1) 1) "shanghai"
   2) "827.6683"
   3) 1) "121.47000163793563843"
      2) "31.22999903975783553"
```

`georadius key longitude latitude value unit`获取在以经纬度为中心半径value+unit内的元素名称,withdist表示距离，withcoord显示经纬度count显示出来的数量

```sql
127.0.0.1:6379> GEORADIUSBYMEMBER china:city shanghai 10000 km
1) "chongqing"
2) "shanghai"
3) "beijing"
```

`groradiusbymember key member radius unit `以member为中心寻找半径为radius单位为unit的元素

```sql
127.0.0.1:6379> geohash china:city beijing shanghai
1) "wx4fbxxfke0"
2) "wtw3sj5zbj0"
```

`geohash key member`返回一个或多个元素的geohash表示，即将二维的经纬度转换成一维的字符串 如果这两个字符串越接近 表示距离越近

geo底层的实现原理就是zset 可以使用zset操作geo，例如

```sql
127.0.0.1:6379> zrange china:city 0 -1
1) "chongqing"
2) "shanghai"
3) "beijing"
127.0.0.1:6379> zrem china:city beijing
(integer) 1
127.0.0.1:6379> zrange china:city 0 -1
1) "chongqing"
2) "shanghai"
```

## Hyperloglog(基数统计)

网站的uv（一个人访问一个网站多次、但还是算作一个人）传统的方式使用set但会占用内存

优点：占用内存是固定的，2^64不同的元素的技术，只需要占用12kb内存。

```sql
127.0.0.1:6379> pfadd key1 a b c d e f g h j k l
(integer) 1
127.0.0.1:6379> pfcount key1
(integer) 11
127.0.0.1:6379> pfadd key2 h j k l w r o x
(integer) 1
127.0.0.1:6379> pfcount key2
(integer) 8
127.0.0.1:6379> pfmerge key3 key1 key2
OK
127.0.0.1:6379> pfcount key3
(integer) 15
```

`pfadd key member`向key中添加一个或多个元素（不会添加重复元素）

`pfcount key`查看key中拥有的个数

`pfmerge key3 key1 key2`合并key1和key2为key3（重复的元素只会统计一次）

## Bitmap（位存储）

Bitmap位图，是一种数据结构 都是操作二进制位来进行记录，就只有0和1两个状态

```sql
127.0.0.1:6379> setbit sign 0 1
(integer) 0
127.0.0.1:6379> setbit sign 1 0
(integer) 0
127.0.0.1:6379> setbit sign 2 1
(integer) 0
127.0.0.1:6379> setbit sign 3 1
(integer) 0
127.0.0.1:6379> setbit sign 4 0
(integer) 0
127.0.0.1:6379> setbit sign 5 1
(integer) 0
127.0.0.1:6379> setbit sign 6 0
(integer) 0
127.0.0.1:6379> getbit sign 0
(integer) 1
127.0.0.1:6379> getbit sign 1
(integer) 0
127.0.0.1:6379> bitcount sign
(integer) 4
```

`setbit key offset value`value的值只能为0或者1 上面表示周一到周日打卡情况 1表示打卡0表示未打开 `getbit key offset`查看某一天的打卡情况`bitcount key[start stop]`查看打卡情况(可以设置开始结束时间)返回4表示打卡了4天

# Redis的事务

Redis单条命令是保存原子性的 但是事务不保证原子性！

Redis事务没有隔离级别的概念，所有的命令在事务中，并没有直接被执行，只有发起执行命令的时候才会执行；

Redis 的事务：

1. 开启事务(multi)

2. 命令入队

3. 执行事务(exec)

   > 正常执行事务

```sql
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set name zhangsan
QUEUED
127.0.0.1:6379> set password 123
QUEUED
127.0.0.1:6379> get name
QUEUED
127.0.0.1:6379> get password
QUEUED
127.0.0.1:6379> exec
1) OK
2) OK
3) "zhangsan"
4) "123"
127.0.0.1:6379> 
```

> 放弃事务(discard)

```sql
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set key4 key4
QUEUED
127.0.0.1:6379> discard
OK
127.0.0.1:6379> get key4
(nil)
127.0.0.1:6379> 
```

> 编译时异常

```sql
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set name zhangsan
QUEUED
127.0.0.1:6379> getset password
(error) ERR wrong number of arguments for 'getset' command
127.0.0.1:6379> set password 123
QUEUED
127.0.0.1:6379> exec
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379> get password
(nil)
```

当发生编译时异常 比如语法错误 整个事务都不会执行

> 运行时异常

```sql
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set name zhangsan
QUEUED
127.0.0.1:6379> incr name
QUEUED
127.0.0.1:6379> exec
1) OK
2) (error) ERR value is not an integer or out of range
127.0.0.1:6379> get name
"zhangsan"
```

当发生运行时异常时 执行成功的事务会执行

### 悲观锁

认为什么时候都会出现问题，无论做什么都会加锁

### 乐观锁

很乐观，认为什么时候都不会出问题，所以不会上锁，更新数据的时候判断，在此期间是否有人修改过数据，获取version， 更新的时候比较version

> Redis监视测试

```sql
127.0.0.1:6379> keys *
1) "money"
2) "out"
127.0.0.1:6379> watch money #使用watch实现乐观锁
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> decrby money 20
QUEUED
127.0.0.1:6379> incrby out 20
QUEUED
127.0.0.1:6379> exec
1) (integer) 80
2) (integer) 20
127.0.0.1:6379> unwatch  #解除监视
OK
127.0.0.1:6379> watch money #重新监视  获取最新的值
OK
```

如果执行过程 另外一个线程修改了我们的值 这个时候 事务就会执行失败

执行失败 我们先`unwatch`解锁，重新监视（获取最新的值）

# Jedis

使用java来操作redis

> Jedis是Redis官方推荐的java连接开发工具，使用java操作redis中间件

添加依赖

```xml
<dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>2.0.7</version>
        </dependency>

        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>4.2.2</version>
        </dependency>
```

然后把本地的redis打开![image-20220705204443601](https://cdn.jsdelivr.net/gh/code-anan/image/20220705204450.png)

基本的使用

```java
public static void main(String[] args) throws Exception {
        Jedis jedis = new Jedis("127.0.0.1", 6379);
        System.out.println(jedis.ping());
        jedis.flushDB();
        jedis.exists("name");
        jedis.set("name","zhangsan");
        jedis.set("password","123");
        String name = jedis.get("name");
        System.out.println(name);
        Set<String> keys = jedis.keys("*");
        System.out.println(keys);
    }
```

输入结果

```java
PONG
zhangsan
[password, name]
```

可以看到这些语法和我们的linux上操作的是一模一样的  对照着上面的语法使用即可

# SpringBoot集成Redis

SpringBoot2.x之后 jedis被替换成了letture

jedis：采用的直连方式 多个线程操作不安全 如果想要避免不安全 需要使用jedis pool连接池！BIO

lettuce：采用netty，实例可以在多个线程中进行共享，不存在线程不安全的情况，可以减少线程数据 NIO模式

首先还是导入依赖

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
            <version>2.6.7</version>
        </dependency>
```

然后是配置连接在`RedisAutoConfiguration`类中的`RedisProperties.class`可以看到有哪些配置项

```properties
spring.redis.host=127.0.0.1
spring.redis.database=0
spring.redis.port=6379
```

最后简单的测试一下

```java
@Test
    void contextLoads() {
        redisTemplate.opsForValue().set("name","lwl");
        redisTemplate.opsForValue().set("password","233");
        Object name = redisTemplate.opsForValue().get("name");
        Object password = redisTemplate.opsForValue().get("password");
        System.out.println(name);
        System.out.println(password);
        redisTemplate.getConnectionFactory().getConnection().flushDb();
    }
```

![image-20220705214040298](https://cdn.jsdelivr.net/gh/code-anan/image/20220705214040.png)

opsfor可以获取不同的类型进行操作，然而在实际开发中我们不会使用自带的RedisTemplate,所以我们需要自己定义一个Template来使用

## 自定义RedisTemplate

首先，redis存放的对象必须要是经过序列化的 不序列话的话会直接报错![image-20220706205728339](https://cdn.jsdelivr.net/gh/code-anan/image/20220706205735.png)

实现`Serializable`接口之后就会发现不报错了 但是他默认的jdk序列化 我们想要自定义的序列化方式 就需要自定义我们的template，下面是一个最常见的模板可以涵盖大部分应用场景

```java
@Configuration
public class RedisConfig {
    //固定模板
    @Bean
    @SuppressWarnings("all")
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        //我们为方便开发，一般直接使用<String, Object>
        RedisTemplate<String, Object> template = new RedisTemplate();
        template.setConnectionFactory(factory);

        //Json序列化配置
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        //string的序列化
        StringRedisSerializer stringRedisSerializer = new StringRedisSerializer();

        //Key采用String的序列化方式
        template.setKeySerializer(stringRedisSerializer);
        //hash的key也采用String的序列化方式
        template.setHashKeySerializer(stringRedisSerializer);
        //value序列化方式采用jackson
        template.setValueSerializer(jackson2JsonRedisSerializer);
        //hash的value序列化方式采用jackson
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        template.afterPropertiesSet();

        return template;
    }


}
```

以及常用的自定义工具类，操作redis更加的方便快捷

## Redis的工具类

```java
@Component
public class RedisUtils {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    private static double size = Math.pow(2, 32);


    /**
     * 写入缓存
     * @param key
     * @param offset 位 8Bit=1Byte
     * @return
     */
    public boolean setBit(String key, long offset, boolean isShow) {
        boolean result = false;
        try {
            ValueOperations<String, Object> operations = redisTemplate.opsForValue();
            operations.setBit(key, offset, isShow);
            result = true;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return result;
    }

    /**
     * 写入缓存
     *
     * @param key
     * @param offset
     * @return
     */
    public boolean getBit(String key, long offset) {
        boolean result = false;
        try {
            ValueOperations<String, Object> operations = redisTemplate.opsForValue();
            result = operations.getBit(key, offset);
        } catch (Exception e) {
            e.printStackTrace();
        }
        return result;
    }


    /**
     * 写入缓存
     *
     * @param key
     * @param value
     * @return
     */
    public boolean set(final String key, Object value) {
        boolean result = false;
        try {
            ValueOperations<String, Object> operations = redisTemplate.opsForValue();
            operations.set(key, value);
            result = true;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return result;
    }


    /**
     * 写入缓存设置失效时间
     * @param key
     * @param value
     * @return
     */
    public boolean set(final String key, Object value, Long expireTime) {
        boolean result = false;
        try {
            ValueOperations<String, Object> operations = redisTemplate.opsForValue();
            operations.set(key, value);
            redisTemplate.expire(key, expireTime, TimeUnit.SECONDS);
            result = true;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return result;
    }

    /**
     * 批量删除对应的value
     *
     * @param keys
     */
    public void remove(final String... keys) {
        for (String key : keys) {
            remove(key);
        }
    }


    /**
     * 删除对应的value
     *
     * @param key
     */
    public void remove(final String key) {
        if (exists(key)) {
            redisTemplate.delete(key);
        }
    }

    /**
     * 判断缓存中是否有对应的value
     *
     * @param key
     * @return
     */
    public boolean exists(final String key) {
        return redisTemplate.hasKey(key);
    }

    /**
     * 读取缓存
     *
     * @param key
     * @return
     */
    public Object get(final String key) {
        Object result = null;
        ValueOperations<String, Object> operations = redisTemplate.opsForValue();
        result = operations.get(key);
        return result;
    }

    /**
     * 哈希 添加
     *
     * @param key
     * @param hashKey
     * @param value
     */
    public void hmSet(String key, Object hashKey, Object value) {
        HashOperations<String, Object, Object> hash = redisTemplate.opsForHash();
        hash.put(key, hashKey, value);
    }

    /**
     * 哈希获取数据
     *
     * @param key
     * @param hashKey
     * @return
     */
    public Object hmGet(String key, Object hashKey) {
        HashOperations<String, Object, Object> hash = redisTemplate.opsForHash();
        return hash.get(key, hashKey);
    }

    /**
     * 列表添加
     *
     * @param k
     * @param v
     */
    public void lPush(String k, Object v) {
        ListOperations<String, Object> list = redisTemplate.opsForList();
        list.rightPush(k, v);
    }

    /**
     * 列表获取
     *
     * @param k
     * @param l
     * @param l1
     * @return
     */
    public List<Object> lRange(String k, long l, long l1) {
        ListOperations<String, Object> list = redisTemplate.opsForList();
        return list.range(k, l, l1);
    }

    /**
     * 集合添加
     *
     * @param key
     * @param value
     */
    public void add(String key, Object value) {
        SetOperations<String, Object> set = redisTemplate.opsForSet();
        set.add(key, value);
    }

    /**
     * 集合获取
     * @param key
     * @return
     */
    public Set<Object> setMembers(String key) {
        SetOperations<String, Object> set = redisTemplate.opsForSet();
        return set.members(key);
    }

    /**
     * 有序集合添加
     * @param key
     * @param value
     * @param scoure
     */
    public void zAdd(String key, Object value, double scoure) {
        ZSetOperations<String, Object> zset = redisTemplate.opsForZSet();
        zset.add(key, value, scoure);
    }

    /**
     * 有序集合获取
     * @param key
     * @param scoure
     * @param scoure1
     * @return
     */
    public Set<Object> rangeByScore(String key, double scoure, double scoure1) {
        ZSetOperations<String, Object> zset = redisTemplate.opsForZSet();
        redisTemplate.opsForValue();
        return zset.rangeByScore(key, scoure, scoure1);
    }


    //第一次加载的时候将数据加载到redis中
    public void saveDataToRedis(String name) {
        double index = Math.abs(name.hashCode() % size);
        long indexLong = new Double(index).longValue();
        boolean availableUsers = setBit("availableUsers", indexLong, true);
    }

    //第一次加载的时候将数据加载到redis中
    public boolean getDataToRedis(String name) {

        double index = Math.abs(name.hashCode() % size);
        long indexLong = new Double(index).longValue();
        return getBit("availableUsers", indexLong);
    }

    /**
     * 有序集合获取排名
     * @param key 集合名称
     * @param value 值
     */
    public Long zRank(String key, Object value) {
        ZSetOperations<String, Object> zset = redisTemplate.opsForZSet();
        return zset.rank(key,value);
    }


    /**
     * 有序集合获取排名
     * @param key
     */
    public Set<ZSetOperations.TypedTuple<Object>> zRankWithScore(String key, long start,long end) {
        ZSetOperations<String, Object> zset = redisTemplate.opsForZSet();
        Set<ZSetOperations.TypedTuple<Object>> ret = zset.rangeWithScores(key,start,end);
        return ret;
    }

    /**
     * 有序集合添加
     * @param key
     * @param value
     */
    public Double zSetScore(String key, Object value) {
        ZSetOperations<String, Object> zset = redisTemplate.opsForZSet();
        return zset.score(key,value);
    }


    /**
     * 有序集合添加分数
     * @param key
     * @param value
     * @param scoure
     */
    public void incrementScore(String key, Object value, double scoure) {
        ZSetOperations<String, Object> zset = redisTemplate.opsForZSet();
        zset.incrementScore(key, value, scoure);
    }


    /**
     * 有序集合获取排名
     * @param key
     */
    public Set<ZSetOperations.TypedTuple<Object>> reverseZRankWithScore(String key, long start,long end) {
        ZSetOperations<String, Object> zset = redisTemplate.opsForZSet();
        Set<ZSetOperations.TypedTuple<Object>> ret = zset.reverseRangeByScoreWithScores(key,start,end);
        return ret;
    }

    /**
     * 有序集合获取排名
     * @param key
     */
    public Set<ZSetOperations.TypedTuple<Object>> reverseZRankWithRank(String key, long start, long end) {
        ZSetOperations<String, Object> zset = redisTemplate.opsForZSet();
        Set<ZSetOperations.TypedTuple<Object>> ret = zset.reverseRangeWithScores(key, start, end);
        return ret;
    }


}
```

使用变得非常的便捷

```java
 @Autowired
    private RedisUtils redisUtils;

    @Test
    void contextLoads(){
            redisUtils.set("name","lwl");
            System.out.println(redisUtils.get("name"));
    }
```

# Redis配置文件

> 网络配置



![image-20220708204201970](https://cdn.jsdelivr.net/gh/code-anan/image/20220708204209.png)

默认的127.0.0.1  如果想让任何机器访问设置`0.0.0.0`即可

```bash
bind 127.0.0.0  #绑定端口
protected-mode yes #保护模式
port 6379 #默认的服务端口
```

> 通用Genaral

```bash
daemonize no #以守护进程的方式运行、默认为no

pidfile /var/run/redis_6379.pid  #如果以后台的方式运行，我们就需要指定pdi文件

# Specify the server verbosity level.日志等级
# This can be one of:
# debug (a lot of information, useful for development/testing)
# verbose (many rarely useful info, but not a mess like the debug level)
# notice (moderately verbose, what you want in production probably)
# warning (only very important / critical messages are logged)
loglevel notice

logfile ""  #日志文件位置 默认为空没有日志

database 16 #数据库数量
always-show-logo yes  #是否显示logo
```

> SNAPSHOTTING快照

```bash
# redis是内存数据库，如果没有持久化，那么数据就会断电丢失
save 900 1 ##如果900s内 如果至少有一个key进行修改 那么我们就进行持久化操作
save 300 10 ##如果300s内 如果至少有10个key进行了修改 我们就会进行持久化操作
save 60 10000
stop-writes-on-bgsave-error yes #持久化如果出错 是否还要继续工作 默认为yes
rdbcompression yes #是否压缩rdb文件、需要消耗cpu资源
rdbchecksum yes #保存rdb文件时 进行错误的交叉校验
dir ./  #rdb文件保存的目录 默认当前文件夹下
```

>  REPLICATION 复制 ，后面讲解主从配置再进行讲解

> SECURITY安全

```bash
#这里我们主要用来设置密码（默认没有密码）
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) ""
127.0.0.1:6379> config set requirepass 233
OK
127.0.0.1:6379> ping
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 233
OK
127.0.0.1:6379> config get requirepass
1) "requirepass"
2) "233"
127.0.0.1:6379> 
# 或者直接在配置文件中设置 `requirepass 233`
```

> CLIENTS 

```bash
maxclients 10000  #设置能连接上redis的最大客户端的数量
maxmemory <bytes>  #redis配置最大的内存容量
maxmemory-policy noeviction #内存达到上限之后的处理策略
1、volatile-lru：只对设置了过期时间的key进行LRU（默认值） 
2、allkeys-lru ： 删除lru算法的key   
3、volatile-random：随机删除即将过期key   
4、allkeys-random：随机删除   
5、volatile-ttl ： 删除即将过期的   
6、noeviction ： 永不过期，返回错误
```

> APPEND ONLY MODE aof配置

```bash
appendonly no  #默认不开启aof模式 默认使用rdb方式持久化  大部分情况下 rdb够用
appendfilename "appendonly.aof" #持久化文件的名字
# appendfsync always  #每次修改都会写入 消耗性能
appendfsync everysec  #每秒执行一次 可能会丢失这一秒的数据
# appendfsync no   #不同步 不执行 操作系统自己同步 速度最快
```

# Redis持久化

无论是面试还是工作 持久化都是一个重点，Redis是内存数据库 如果不将内存中的数据库状态保存到硬盘 那么服务器一旦出现问题 数据库信息状态就会丢失 所以redis提供了持久化功能

## RDB（Redis DataBase）

含义：在指定的时间间隔间隔内将内存中的数据集快照写入到磁盘 也就是行话说的snapshot快照，他恢复时将快照文件直接读到内存中；

Redis会单独创建一个fork（一个子进程来进行持久化，会先将数据写到一个临时文件，等到持久化过程结束，再用这个临时文件替换上次持久化好的文件。整个过程中，主进程不进行任何io操作，这样就保证了极高的性能、如果需要进行大规模数据的回复，且对于数据恢复的完整性不是非常敏感、那么RDB方式比AOF更加的高效，他的缺点是最后一次持久化的数据可能丢失，比如服务器宕机 我们默认使用的就是RDB 一般情况不需要修改这个配置）

文件保存的文件是dump.rdb文件，在配置文件中可以设置！

> 触发机制

1. save的规则满足的情况下 会自动触发rdb规则
2. 执行flushall命令 也会触发
3. 退出redis 也会产生rdb文件

备份自动生成dump.rdb

> 恢复rdb文件里面的数据

1. 只要将rdb文件放到我们redis的启动目录即可 redis启动会自动检查dump.rdb恢复其中数据
2. 查看需要存在的位置

```bash
127.0.0.1:6379> config get dir
1) "dir"
2) "/var/lib/redis/6379" ##如果这个目录下存在dump.rdb文件 启动就会自动恢复其中的数据
```

优点：适合大规模的数据恢复，对数据的完整性要求不高

缺点：需要一定的时间间隔进程操作，如果redis意外宕机 最后一次修改的数据就会丢失 fork进程的时候，会占用一定的空间

## AOF(Append Only File)

将所有命令记录下来，恢复（history）的时候把文件执行一遍，不包含读操作

> 开启

```conf
appendonly no
```

默认是不开启的 改为yes则进行开启，生成的文件为`appendonly.aof`

如果aof文件有错误，那么redis客户端连接不了，我们需要修复这个aof文件`redis-check-aof --fix appendonly.aof`

优点：每一次修改都同步，文件完整性更好，每秒同步一次，可能会丢失一秒的数据，从不同步 效率最高

缺点：相对于数据文件来说，aof远远大于rdb 修改速度也比rdb慢

Aof运行效率也比rdb慢 所以redis默认持久化选的是rdb

# Redis发布订阅

Redis发布订阅(pub/sub)是一种消息通信模式：发送者（pub）发送消息、订阅者（sub）接收消息；Redis客户端可以订阅任意数量的频道

![image-20220710190405205](https://cdn.jsdelivr.net/gh/code-anan/image/20220710190412.png)

> 测试

订阅者：

```bash
127.0.0.1:6379> subscribe lwl
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "lwl"
3) (integer) 1
1) "message"
2) "lwl"
3) "hello,dog"
```

发布者：

```bash
127.0.0.1:6379> publish lwl hello,dog
(integer) 1
```

# Redis主从复制

## 概念

主从复制，是指将一台redis服务器的数据复制到其他的redis服务器。前者成为主节点（master），后者称为从节点（slave），数据的复制是单向的，只能从主节点到从节点，mater以写为主，slave以读为主

默认情况下，每台redis服务器都是单节点；且一个主节点可以有多个从节点或者没有从节点，但一个从节点只能有一个主节点

主从复制，读写分离！80%的操作都是读操作，这样可以减轻服务器的压力，架构中经常使用

## 作用

1. 数据冗余：主从复制实现了数据的热备份，是持久化之外的一种数据冗余方式
2. 故障恢复：当主节点出现问题时，可以由从节点提供服务，实现快速的故障恢复；实际上是一种服务的冗余
3. 负载均衡：在主从复制的基础上，配合读写分离，可以由主节点提供写服务，从节点提供读服务（即写redis数据时应用连接主节点、读Redis数据时应用连接从节点），分担服务器负载；尤其是在写少读多的场景下，通过多个从节点分担读负载，可以大大提高Redis服务器的并发量
4. 高可用基石：除了上述作用之外，主从复制还是哨兵和集群能够实施的基础，因此说主从复制是Redis高可用的基础

 ## 环境配置

只配置从库，不需要配置主库

```bash
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:0
master_replid:51733d45fc7a2bce5acc99d6447b83cb128edc99
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
```

下面 模拟Redis的集群 一主二从 我们通过修改端口启动的方式 复制三个配置文件 然后修改对应的信息

```bash
root@localhost lwlredisconfig]# cp redis.conf redis79.conf
[root@localhost lwlredisconfig]# cp redis.conf redis80.conf
[root@localhost lwlredisconfig]# cp redis.conf redis81.conf
```

1.端口 分别为6379 6380 6381

2.pid名字

3.log文件名字

4.dump.rdb名字 启动之后效果如下

![image-20220710203101046](https://cdn.jsdelivr.net/gh/code-anan/image/20220710203101.png)

这样就成功的模拟了一个小集群

## 一主二从

上面我们说了 只需要配置从节点即可(在6380和6381端口上执行)

```bash
[root@localhost lwlredisconfig]# redis-cli -p 6380
127.0.0.1:6380> info replication
# Replication
role:master
connected_slaves:0
master_replid:8261d164635c5c5e3ea04adf8ea26ebd037c95a3
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
127.0.0.1:6380> slaveof 127.0.0.1 6379
OK
127.0.0.1:6381> info replication
# Replication
role:slave
master_host:127.0.0.1
master_port:6379
master_link_status:up
master_last_io_seconds_ago:5
master_sync_in_progress:0
slave_repl_offset:70
slave_priority:100
slave_read_only:1
connected_slaves:0
master_replid:dcc904eb6026b4ea356b12e82b396f63be8ddefa
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:70
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:71
repl_backlog_histlen:0
```

使用`slaveof host port`命令即可，可以看到role角色也变成了slave，在查看主机,可以看到从机的信息

```bash
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:0
master_replid:a50f3c2a618c08b7b1c6d9392629d2647220d792
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
127.0.0.1:6379> info replication
# Replication
role:master
connected_slaves:2
slave0:ip=127.0.0.1,port=6380,state=online,offset=308,lag=0
slave1:ip=127.0.0.1,port=6381,state=online,offset=308,lag=1
master_replid:dcc904eb6026b4ea356b12e82b396f63be8ddefa
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:308
second_repl_offset:-1
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:1
repl_backlog_histlen:308
127.0.0.1:6379> 
```

真实的主配置应该在配置文件中修改，那样的话是永久的 使用上面的命令 只是暂时的；主机只能写 从机只能读

![image-20220711201849638](https://cdn.jsdelivr.net/gh/code-anan/image/20220711201856.png)

```bash
127.0.0.1:6380> get k1
"v1"
127.0.0.1:6380> set k2 v2
(error) READONLY You can't write against a read only replica.
```

主机写 从机只能读 主机宕机 从机依然可以读数据并且连接到主机，如果主机回来 从机依然可以直接获取主机的数据；从机宕机 主机重新set值 从机重连查不到值 发现它又变成了主机 但是再次设置为从机 又可以获取到值；

> 复制原理

slave启动成功连接到master后会发送一个sync同步命令

Master接到命令，启动后台的存盘进程，同时收集所有接收到的用于修改数据集命令，在后台进程执行完毕之后，master将传送整个数据文件到slave，并完成一次完全同步

全量复制：而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中

增量复制：Master继续将新的所有收集到的修改命令依次传给slave完成同步，但是只要重新连接master，一次完全同步（全量复制）会被自动执行，在从机中一定可以看到主机的数据

`slaveof no one`可以自己变成主节点

# 哨兵模式

一种自动选举老大的模式，能够后台监控主机是否故障，如果故障了根据投票数自动将从库转换为主库；

哨兵模式是一种特殊的模式，首先Redis提供了哨兵的命令，哨兵是一个独立的进程，作为进程，它会独立运行，其原理是哨兵通过发送命令，等待Redis服务器响应，从而监控运行的多个Redis实例

> 测试

1.配置哨兵配置文件sentinel.conf（不止这一个配置）

```bash
sentinel monitor myredis 127.0.0.1 6379 1
后面的数字1 代表主机挂了 slave投票看让谁接替成为主机 票最多的就会成为主机
```

2.启动哨兵

```bash
[root@localhost lwlredisconfig]# redis-sentinel sentinel.conf
3907:X 11 Jul 2022 06:01:56.968 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
3907:X 11 Jul 2022 06:01:56.968 # Redis version=5.0.8, bits=64, commit=00000000, modified=0, pid=3907, just started
3907:X 11 Jul 2022 06:01:56.968 # Configuration loaded
3907:X 11 Jul 2022 06:01:56.969 * Increased maximum number of open files to 10032 (it was originally set to 1024).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 5.0.8 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in sentinel mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 26379
 |    `-._   `._    /     _.-'    |     PID: 3907
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

3907:X 11 Jul 2022 06:01:56.970 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
3907:X 11 Jul 2022 06:01:56.971 # Sentinel ID is 117052e5211488f43bd6cf6d84f616661444f0b3
3907:X 11 Jul 2022 06:01:56.971 # +monitor master myredis 127.0.0.1 6379 quorum 1
3907:X 11 Jul 2022 06:01:56.973 * +slave slave 127.0.0.1:6380 127.0.0.1 6380 @ myredis 127.0.0.1 6379
3907:X 11 Jul 2022 06:01:56.975 * +slave slave 127.0.0.1:6381 127.0.0.1 6381 @ myredis 127.0.0.1 6379
```

`redis-sentinel sentinel.conf`即可启动

如果Master主节点断开，这个时候就会从从机中随机选择一个服务器（里面有一个算法）

这时候主机重连 发现变成了从机 这就是哨兵模式的规则

优点：哨兵集群，基于主从复制，所有的主从配置优点 他都有，主从可以切换 故障可以转移 系统的可用性会更好 ，哨兵模式就是主从模式的升级 手动到自动 更加健壮

缺点：redis不好在线扩容 集群容量一旦到达上限 在线扩容十分麻烦 实现哨兵模式的配置比较复杂 里面有很多选择 可以搜索一下哨兵的所有配置

# Redis缓存穿透和雪崩

## 缓存穿透

> 概念

用户想要查询一个数据，发现redis内存数据库没有，也就是缓存没有命中 于是向持久层数据库查询 但是发现持久层数据库也没有，于是本次查询失败。当用户很多的时候，缓存都没有命中 于是都去请求了持久层数据库 这会给持久层数据库造成很大的压力 这时候就相当于出现了缓存穿透

> 解决方案

1.布隆过滤器

布隆过滤器是一种数据结构，对所有可能查询的参数以hash形式存储，在控制层先进行校验，不符合则丢弃，从而避免了对底层存储系统的查询压力

2.缓存空对象

当存储层不命中后，即使返回的空对象也将其缓存起来，同时设置一个过期时间，之后再访问这个数据将会从缓存中获取，保护了后端数据源

但是这种方法会有两个问题：

1.如果空值能够被缓存起来，这就意味着缓存需要更多的空间存储更多的键，因为这当中可能会有很多的空值的键

2.即使对空值设置了过期时间，还是会存在缓存层和存储层的数据会有一段时间窗口的不一致，这对于需要保持一致性的业务有影响

> 缓存击穿（量太大，缓存过期）

缓存击穿，是指一个key非常热点，在不停的扛着大并发，大并发集中对这一个点进行访问，当这个key在失效的瞬间，持续的大并发就穿破缓存，直接请求数据库，就像在屏障上凿开了一个洞；

当某个key在过期的瞬间，有大量的请求并发访问，这类数据一般是热点数据，由于缓存过期，会同时访问数据库来查询最新数据并且回写缓存，会导致数据库瞬间压力过大

> 解决方案

**设置热点数据永不过期**

**加互斥锁**

分布式锁：使用分布式锁，保证对于某个key同时只有一个线程去查询后端服务，其他线程没有获得分布式锁的权限，因此只需要等待即可。这种方式将高并发的压力转移到了分布式锁，因此对分布式锁的考验很大

## 缓存雪崩

缓存雪崩，是指在某一个时间段，缓存集中过期失效，Redis宕机

> 结局方案

**redis高可用**

这个思想的含义是，既然reids有可能会挂掉，那我多增设几台redis，这样一台挂掉之后其他的还可以正常工作，其实这就是搭建的集群

**限流降级**

在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量，比如对某个key只允许一个线程查询和写缓存，其他线程等待

**数据加热**

数据加热的含义是在正式部署之前，先把可能的数据先预先访问一边，这样部分可能大量访问的数据就会加载到缓存中，在即将发生大并发访问前手动触发加载缓存不同的key，设置不同的过期时间，让缓存失效的时间点尽量均匀