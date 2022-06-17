tags:
  - PageHelper
  - SpringBoot
categories:
  - PageHelper
  - 教程
cover: https://cdn.jsdelivr.net/gh/code-anan/image/src=http---img.yijingtu.com-oss-d-file-2021-08-13-3ec801a4b1ed902e88a4d348cb96b0d0.jpg&refer=http---img.yijingtu.jpg
my: post/PagehelperUse
title: SpringBoot如何使用PageHelper
date: 2021-12-24 13:00
------

# 前言

PageHelper是一款非常好用的分页插件，之前介绍了mybatis中它的使用，现在来看一下如何在SpringBoot中使用吧

#  使用步骤

## 依赖导入

```xml
<dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.4.1</version>
</dependency>
```

与mybatis中的不同，需要引入这个`pagehelper-spring-boot-starter`依赖

## 配置文件添加

与mybatis不同的是，SpringBoot中需要添加在`application.properties`中 

```properties
pagehelper.helperDialect=mysql
pagehelper.reasonable=true
pagehelper.supportMethodsArguments=true
pagehelper.params=count=countSql
```

说明：

`pagehelper.helperDialect`:指定数据库，可以不配置，pagehelper插件会自动检测数据库的类型

`pagehelper.reasonable`:分页合理化参数默认false，当该参数设置为true 时，pageNum <= 0 时，默认显示第一页，pageNum 超过 pageSize 时，显示最后一页

`pagehelper.supportMethodsArguments`:用于从对象中根据属性名取值，可以配置pageNum，pageSize，count 不用配置映射的默认值

`pagehelper.params`:分页插件会根据查询方法的参数中，自动根据params 配置的字段中取值，找到合适的值会自动分页

## 代码实现

```java
public List<NotificationDto> getNotificationList(User user, Integer page, Integer size) {
        PageHelper.startPage(page,size);
        NotificationExample example = new NotificationExample();
        example.createCriteria().andReceiverEqualTo(user.getId());
        example.setOrderByClause("GMTCREATE desc");
        example.setOrderByClause("STATUS");
        List<Notification> notifications = notificationMapper.selectByExample(example);
        ArrayList<NotificationDto> resultDto = new ArrayList<>();
        for(Notification notification:notifications){
            NotificationDto notificationDto = new NotificationDto();
            User notifier = userMapper.selectByPrimaryKey(notification.getNotifier());
            //发起人
            notificationDto.setNotifier(notifier);
            //回复了问题还是回复了评论
            notificationDto.setType(NotificationEnum.getNameByTypeId(notification.getType()));
            //问题的标题（用来点击跳转到哪个问题上）
            if(notification.getType()==NotificationEnum.NOTIFICATION_QUESTION.getType()){
                Question question = questionMapper.selectByPrimaryKey(notification.getOuterid());
                notificationDto.setOuterTitle(question.getTitle());
                notificationDto.setOuterId(question.getId());
            }else{
                Comment comment = commentMapper.selectByPrimaryKey(notification.getOuterid());
                notificationDto.setOuterTitle(comment.getContent());
                if(comment.getType()==1){
                    notificationDto.setOuterId(comment.getParentid());
                }else{
                    Comment comment1 = commentMapper.selectByPrimaryKey(comment.getParentid());
                    Question question = questionMapper.selectByPrimaryKey(comment1.getParentid());
                    notificationDto.setOuterId(question.getId());
                }

            }
            //每个Dto的创建时间
            notificationDto.setGmtCreate(notification.getGmtcreate());
            //Dto的状态 未读和已读
            notificationDto.setStatus(notification.getStatus());
            ///notification的ID方便改对应的阅读状态
            notificationDto.setNotificationId(notification.getId());

            resultDto.add(notificationDto);
        }
        return resultDto;
}
```

使用非常简单，只有一条关键语句`PageHelper.startPage(page,size);`

page：表示要查询的页数（最小为1）

size：每一页的数量

## 效果展示

![](https://cdn.jsdelivr.net/gh/code-anan/image/20211226192056.png)

# 总结

pagehelper的使用非常简单方便、但是要实现分页不是仅仅用这个就能实现的了，想知道完整实现分页可以看我的[github项目](https://github.com/code-anan/community)(✪ω✪)