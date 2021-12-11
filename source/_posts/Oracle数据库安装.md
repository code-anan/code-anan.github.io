title: Oracle数据库安装
author: lwl
date: 2021-07-30 17:44:01
tags:
  - Oracle
categories:
  - 数据库
  - 安装记录
cover: https://cdn.jsdelivr.net/gh/code-anan/image/src=http---www.51wendang.com-pic-edf49c27470e000e62312406-1-807-jpg_6-1077-0-0-1077.jpg&refer=http---www.51wendang.jpg
my: post/oracleinstall
---
# 前言
今天想把几个查询结果导成dmp文件，结果那个指令需要本地安装oracle数据库，无奈只得安装一下`Oracle`~ 
PS:本文主要参考[逆流君](https://zhuanlan.zhihu.com/p/152206091)的文章
# 安装路径
可以选择官网下载，但是官网有时候比较难进而且慢，这里可以选择在我的网盘下载,版本为R11
网盘地址：`https://pan.baidu.com/s/1JFnNXPAOT2mYamKI1h8qBg`
提取码：`xh27`

只要下载两个文件即可![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731092210.png)

# 安装步骤
1. 上面两个压缩吧下载好之后，选中这两个压缩文件同时解压（可能稍微会有点慢），解压完成之后会发现生成一个文件夹名为`database`![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731092532.png)

2. 进入该文件夹，双击`setup.exe`![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731092837.png)

3. 之后很可能会看到下面的提示，直接忽略掉点击是即可![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731093114.png)
4. 去掉勾选接收安全更新![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731093312.png)
5. 提醒未填写邮件，也是直接忽略点是![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731093433.png)
6. 然后选择`创建和配置数据库`，下一步![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731093531.png)
7. 系统类中选择`桌面类`![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731093705.png)
8. 典型安装配置中，除了口令其他最后按照默认，每个人的可能会不同，需要注意的是口令需要自己输入而且必须有大小写字母和数字且不少于八位字符，这里我设置的是Aa123456，然后继续下一步![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731094034.png)
9. 概要基本不用看，直接点击`完成`即可![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731094149.png)
10. 然后慢慢等待安装完成。。。。（注意有防火墙拦截某些程序点击允许运行）
![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731094247.png)
11. 安装完成会看到下面界面![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731094746.png)
12. 上面走完会跳出一下界面，注意千万不要点确定，要选择呢`口令管理`！![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731095349.png)
13. 进入到口令管理中，需要对一下账户做对应操作
    + 找到SYS，将SYS的口令设置为`change_on_install`
    + 找到system，将system口令设置为`manager`
    + 找到SH，设置不锁定账户，口令为`sh`
    + 找到SCOTT，设置不设定账户，口令自己设置，这里我设置为`scott`![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731095631.png)
提示密码复杂性 直接忽略确定即可![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731100250.png)
14. 然后继续确定可以看到安装完成了，关闭即可![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731100756.png)
15. 测试安装是否成功，运行页面输入sqlplus,登录scott的账号密码查oracle自带的emp表看是否有结果，出现如下表示成功![](https://cdn.jsdelivr.net/gh/code-anan/image/20210731102741.png) 

# 错过口令管理的解决方案
1. win+R键 打开运行页面 并且输入`sqlplus`
2. 输入之后让输入用户名，这里用户名输入`sys`
3. 输入口令+ as sysdba，这是设置口令密码时输入的，比如我的是Aa123456（注意他这里不会显示所以要细心输入，而且后面要接as sysdba）
4. 输入`alter user scott account unlock`;可以看到提示用户已更改
5. 输入`commit`；提交完成
6. 输入 `conn scott/tiger`(这里是更改scott的口令)
7. 新口令输入，然后再次输入，已连接。
