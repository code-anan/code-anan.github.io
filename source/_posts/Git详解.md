title: Git详解
author: lwl
tags:
 - Git
categories:
 - Git
cover: https://gcore.jsdelivr.net/gh/code-anan/image/20220302202339.png
my: post/git
---

# 版本控制

对Git有一点了解的都知道Git是一款非常流行的版本控制工具，对于他的概念可以去查阅相关文档，说白了就是为了解决多人开发，方便项目更新

# Git和Svn的区别

SVN是集中式的版本控制系统，版本库集中放在中央服务器，工作的时候用的都是用的自己的电脑，所以首先需要从中央服务器得到最新的版本然后工作，完成之后把代码再同步到中央服务器，缺点是工作的时候必须保持联网状态，对网络带宽要求较高

Git的分布式控制系统，没有中央服务器，每个人的电脑就是一个完整的版本库，工作的时候可以不需要联网；协同方式是

自己在电脑上更改了文件A，其他人也在电脑上更新了文件A，这时可以把各自更新的内容推送给对方，就可以看到对方的修改，Git可以直接看到更新了哪些代码和文件

Git是目前世界上最先进的分布式版本控制系统

# Git安装

直接在[Git官网]([Git (git-scm.com)](https://git-scm.com/))无脑下一步即可，如果觉得慢的话可以在淘宝的[镜像地址]([CNPM Binaries Mirror (npmmirror.com)](https://registry.npmmirror.com/binary.html?path=git-for-windows/))下载，下载安装完成之后鼠标右键可以看到`Git Gui Here`和`Git  Bash Here`的选项，这样就表示安装完成了

Git Bash：Unix与linux风格的命令行 使用最多

Git CMD: Windows风格的命令行

Git GUI:图形页面的Git，不建议初学者使用

# 基本的Linux命令

1. cd:改变目录 例`cd test/test1`

2. cd.. :回退到上一个目录，直接cd进入默认目录

3. pwd:显示当前所在的目录路径

4. ls：表示列出当前目录下的所有文件

5. touch :新建一个文件 例`touch index.html`

6. rm: 删除一个文件 例`rm index.html`删除刚才新建的文件

7. mkdir：新建一个文件夹 例·`mkdir test2`

8. rm -r:删除一个文件夹  例`rm -r test2`

9. rm -rf /:删除电脑中所有文件（不可以尝试！）

10. reset :重新初始化终端、清屏

11. clear:清屏

12. history:查看历史命令

13. help：帮助

14. exit: 退出

这些指令全部可以在Git bash中使用

# Git的必要配置

查看配置`git config -l`

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220228111023.png)

查看系统config `git config --system --l` 文件是`C:\Program Files\Git\etc`(安装位置)下的config文件

查看当前用户global配置 `git config --global --l` 可以看到用户的账号密码ssl信息等 文件是`C:\Users\Admin`下的`.config` 

设置git的账号密码指令本质就是配置上面的文件信息，修改账号邮箱指令

`git config --global user.name "code-anan"`

`git config --global user.email "719603766@qq.com"`

# Git工作原理

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220228113101.png)

Working Directory:工作区，平时我们写代码存放的地方

Stage/index:暂存区，用于临时存放代码变动，本质上他是一个文件，保存即将存放到文件列表信息

Repository(History):仓库区，存放数据的位置，这里面有提交的所有版本的数据，其中Head指向最新存放仓库的版本

Remote：远程仓库，托管代码的服务器即远程保存我们代码的地方

# Git的项目搭建

## 本地搭建

在当前目录创建一个git代码库

```git
git init
```

然后会发现多出来一个git隐藏目录，关于版本等信息都在这个目录里面

## 克隆远程仓库

```git
git clone [url]
```

clone后面跟具体的url地址，他就会下载到当前目录，github和gitee的项目地址都可以

# Git基本指令

```git
git init 初始化一个git项目
git status 查看所有文件状态
git add .添加所有问题
git commit -m" " 提交到仓库-m后面添加提交的信息
```

> 忽略文件

有的文件不需要纳入到版本控制中，比如一些数据库文件，临时文件，设计文件等，一般目录下都会有`.gitignore`文件，此文件有以下规则：

1. 忽略文件中的空格或#开始的行都会被忽略
2. 可以使用linux通配符；*代表任意多个字符，？代表一个字符，[abc]代表可选字符范围，大括号({string1,string2,..})代表可选的字符串
3. 如果名称的最前面有一个感叹号(!)，表示例外规则，将不会被忽略
4. 如果名称的最前面是一个路径分隔符(/),表示要忽略的文件在此目录下，而子目录中的文件不忽略
5. 如果名称的最后面是一个路径分隔符(/)，表示要忽略的是此目录下该名称的子目录，而非文件（默认文件和目录都忽略）

```
#为注释
*.txt  #忽略所有.txt结尾的文件
!lib.txt #但除了lib.txt文件
/temp  #仅忽略项目根目录下的TODO文件，不包括其他，目录temp
build/  #忽略build/目录下的所有文件
doc/*.txt #忽略doc/notes.txt 单不包括doc/server/arch.txt
```

# 码云使用

## 添加SSH公钥

码云也就是我们知道的[gitee]([我的工作台 - Gitee.com](https://gitee.com/))，注册完基本信息之后，我们还可以让本地电脑绑定ssh公钥，实现免密码登录，首先来到我们路径为`C:\Users\admin\.ssh`,在此路径下输入以下指令并一直回车

```java
ssh-keygen -t rsa
```

可以看到生成了以下文件![](https://gcore.jsdelivr.net/gh/code-anan/image/20220302192259.png)

后缀pub表示公钥 我们复制到我们的giteee上（设置-SSh公钥）即可![](https://gcore.jsdelivr.net/gh/code-anan/image/20220302192520.png)

这里是默认的 也可以在生成密钥的时候输上咱们的账号密码 

## 创建仓库并拷贝到本地

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220302193239.png)

随意创建一个仓库，然后拷贝到本地`git clone`命令

## IDEA集成git

上面我们已经把新建的仓库拷贝到了本地，接下来我们要创建一个新的项目并把代码推送到仓库![](https://gcore.jsdelivr.net/gh/code-anan/image/20220302194714.png)

这里我们直接新创建一个springboot项目，新创建之后可以看到他右上角没有任何图标，然后到我们刚才克隆到的项目中![](https://gcore.jsdelivr.net/gh/code-anan/image/20220302194900.png)

直接复制这些文件拷到我们创建的项目中，有重复的替换掉即可，这样就可以看到我们的项目右上角出现了git图标![](https://gcore.jsdelivr.net/gh/code-anan/image/20220302195044.png)

这表示我们的项目和仓库绑定到了一起，当然也可以在这里直接填上git路径![](https://gcore.jsdelivr.net/gh/code-anan/image/20220302195128.png)

位置在上方工具栏`VCS`->`get from version Controll`，这两种方式都可以跟仓库绑定，代码的提交可以选择右上角的git小符号，也可以在下方的`Terminal`中输入git指令（下面还有个git push指令）

![](https://gcore.jsdelivr.net/gh/code-anan/image/20220302195537.png)

提交之后就可以在仓库中看到我们的代码了![](https://gcore.jsdelivr.net/gh/code-anan/image/20220302195615.png)

## Git分支

熟悉github的朋友都知道，每个项目都可以有多个分支 各个分支之间互不影响 一般用来放不同版本的代码，下面学习一些git分支的常用指令

```java
// 列出所有本地分支
git branch

// 列出所有远程分支
git branch -r

// 新建一个分支，但仍停留在当前分支
git branch [branch-name] //例如git branch dev

//新建一个分支，并切换到该分支
git checkout -b [branch] //例如git checkout -b dev

//合并指定分支到当前分支
git merge [branch]  //例如当前在master分支 但是dev分支修改了master分支的代码 git merge dev

//删除分支
git branch -d [branch] //例如git branch -d dev

//删除远程分支
git push origin --delete [branch]
git branch -dr [remote/branch]
```

以上就是基本上会用到的git知识 更多内容还可以到gitee的([Git 大全 - Gitee.com](https://gitee.com/all-about-git))进行更详细的学习(*^▽^*)