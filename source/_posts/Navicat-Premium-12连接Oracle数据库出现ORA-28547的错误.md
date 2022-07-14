title: Navicat Premium 12连接Oracle数据库出现ORA-28547的错误
author: lwl
date: 2021-06-24 14:23:45
tags:
  - Navicat Premium 12
  - ORA-28547错误
categories:
 - 数据库
 - 工具
my: post/ora28547
cover: https://gcore.jsdelivr.net/gh/code-anan/image/ora28547.jpg
---
# 错误描述
navicat premium 12连接oracle数据库时出现以下错误：
![](https://gcore.jsdelivr.net/gh/code-anan/image/373693846451dae1ea050da787ddcac.png)

# 解决方法
1. 首先必须要下载必要的客户端->[传送门](https://www.oracle.com/database/technologies/instant-client/winx64-64-downloads.html),选择如下版本下载，如果没有oracle账户需要先注册一下，步骤也很简单![](https://gcore.jsdelivr.net/gh/code-anan/image/20210624142559.png)

2. 下载完成并解压到一个目录不包含中文字符的目录，这里我放到了`D:\Oracle`下，图为解压后的文件![](https://gcore.jsdelivr.net/gh/code-anan/image/20210624142647.png)

3. 打开`Nacicat Premium 12`中的`工具`->`选项`->`环境`，修改`OCI环境`为下图，确定之后需要`重启`navicat，记住一定要重启！
![](https://gcore.jsdelivr.net/gh/code-anan/image/20210624142951.png)

# 完成
重启之后，发现可以正常连接了！![](https://gcore.jsdelivr.net/gh/code-anan/image/20210624143305.png)

完结撒花~