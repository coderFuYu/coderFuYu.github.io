---
title: git新手学习笔记
date: 2020-07-24 10:48:07
tags: [git,教程]
categories: 教程
keywords: git 
top_img: https://oss.fycoder.top/Blog/git%20top%20pic.jpeg
cover: https://oss.fycoder.top/Blog/git%20top%20pic.jpeg
---

# git介绍

分布式：Git版本控制系统是一个分布式的系统，是用来保存工程源代码历史状态的命令行工具。

保存点：Git的保存点可以追踪源码中的文件, 并能得到某一个时间点上的整个工程项目的状态；可以在该保存点将多人提交的源码合并, 也可以回退到某一个保存点上。

Git离线操作性：Git可以离线进行代码提交，因此它称得上是完全的分布式处理，Git所有的操作不需要在线进行；这意味着Git的速度要比SVN等工具快得多，因为SVN等工具需要在线时才能操作，如果网络环境不好， 提交代码会变得非常缓慢。

Git基于快照：SVN等老式版本控制工具是将提交点保存成补丁文件，Git提交是将提交点指向提交时的项目快照，提交的东西包含一些元数据(作者，日期，GPG等)。

Git的分支和合并：分支模型是Git最显著的特点，因为这改变了开发者的开发模式，SVN等版本控制工具将每个分支都要放在不同的目录中，Git可以在同一个目录中切换不同的分支。

分支即时性：创建和切换分支几乎是同时进行的，用户可以上传一部分分支，另外一部分分支可以隐藏在本地，不必将所有的分支都上传到GitHub中去。

分支灵活性：用户可以随时创建、合并、删除分支，多人实现不同的功能，可以创建多个分支进行开发，之后进行分支合并，这种方式使开发变得快速、简单、安全。

# 1.安装git
（转载自[Git的安装与使用教程（超详细！！！）](https://blog.csdn.net/weixin_44950987/article/details/102619708)）


[官网下载地址](https://git-scm.com/download)

Git客户端安装过程
1、双击安装程序“Git-2.23.0-64-bit.exe”，显示截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724102516430.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

2、点击“Next”，显示截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724102615552.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

根据自己的情况，选择程序的安装目录。

3、继续点击“Next”，显示截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724102635248.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

说明：

（1）图标组件(Addition icons) : 选择是否创建桌面快捷方式。

（2）桌面浏览(Windows Explorer integration) : 浏览源码的方法，使用bash 或者 使用Git GUI工具。

（3）关联配置文件 : 是否关联 git 配置文件, 该配置文件主要显示文本编辑器的样式。

（4）关联shell脚本文件 : 是否关联Bash命令行执行的脚本文件。

（5）使用TrueType编码 : 在命令行中是否使用TruthType编码, 该编码是微软和苹果公司制定的通用编码。

4、选择完之后，点击“Next”，显示截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724102644291.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

开始菜单快捷方式目录：设置开始菜单中快捷方式的目录名称, 也可以选择不在开始菜单中创建快捷方式。

5、点击“Next”，显示截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724102653177.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

选择编辑器，可以选vim，练练指令

6、点击“Next”，显示截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724102701352.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

设置环境变量

选择使用什么样的命令行工具，一般情况下我们默认使用Git Bash即可：

（1）Git自带：使用Git自带的Git Bash命令行工具。

（2）系统自带CMD：使用Windows系统的命令行工具。

（3）二者都有：上面二者同时配置，但是注意，这样会将windows中的find.exe 和 sort.exe工具覆盖，如果不懂这些尽量不要选择。

7、选择之后，继续点击“Next”，显示如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020072410271085.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724102719636.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)



选择提交的时候换行格式

（1）检查出windows格式转换为unix格式：将windows格式的换行转为unix格式的换行再进行提交。

（2）检查出原来格式转为unix格式：不管什么格式的，一律转为unix格式的换行再进行提交。

（3）不进行格式转换 : 不进行转换，检查出什么，就提交什么。

8、选择之后，点击“Next”，显示截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724102730167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

9、选择之后，点击“Next”，显示截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724102735962.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

10、选择之后，点击“Install”，开始安装，截图显示如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724102741287.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

11、安装完成之后，显示截图如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020072410274779.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

12、安装完成后，还需要最后一步设置，在命令行输入如下：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724102752281.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

因为Git是分布式版本控制系统，所以需要填写用户名和邮箱作为一个标识。

注意：git config --global 参数，有了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然你也可以对某个仓库指定的不同的用户名和邮箱。

这样，我们的Git客户端就下载并安装完成了。

#  2.配置SSH Key
（转载至[GitHub如何配置SSH Key](https://blog.csdn.net/u013778905/java/article/details/83501204)）
一、设置git的user name和email
如果你是第一次使用，或者还没有配置过的话需要操作一下命令，自行替换相应字段。
git config --global user.name "Luke.Deng"
git config --global user.email  "xiangshuo1992@gmail.com"
说明：git config --list 查看当前Git环境所有配置，还可以配置一些命令别名之类的。
二、检查是否存在SSH Key
~~~
cd ~/.ssh
ls
或者
ll
~~~
//看是否存在 id_rsa 和 id_rsa.pub文件，如果存在，说明已经有SSH Key
如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724102949857.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

如果没有SSH Key，则需要先生成一下
ssh-keygen -t rsa -C "xiangshuo1992@gmail.com"
执行之后继续执行以下命令来获取SSH Key
~~~
cd ~/.ssh
ls
或者
ll
~~~
//看是否存在 id_rsa 和 id_rsa.pub文件，如果存在，说明已经有SSH Key
三、获取SSH Key
cat id_rsa.pub
//拷贝秘钥 ssh-rsa开头
如下图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724103734359.png)

四、GitHub添加SSH Key
GitHub点击用户头像，选择setting

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724103745527.png)

新建一个SSH Key

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724103753211.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

取个名字，把之前拷贝的秘钥复制进去，添加就好啦。

五、验证和修改
测试是否成功配置SSH Key
ssh -T git@github.com
//运行结果出现类似如下
Hi xiangshuo1992! You've successfully authenticated, but GitHub does not provide shell access.
之前已经是https的链接，现在想要用SSH提交怎么办？
直接修改项目目录下 .git文件夹下的config文件，将地址修改一下就好了。
git地址获取可以看如下图切换。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724103808933.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)
# 3.新建本地仓库
在要管理的代码文件夹里打开git bash
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020072410384245.png)
运行下述代码

~~~
git init
~~~

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724103955177.png)

有类似输出表示创建成功

在本地写自己要写的代码

运行以下代码将已修改全部代码保存到暂存区
~~~
git add .
~~~


运行git commit将代码保存到本地仓库，在后面可以给这次保存加上说明
第一次commit时要写-m，且必须写说明
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724104210363.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)
# 4.建立线上仓库（以github为例）
一、登录github
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020072410425352.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

注册并登录github
二、新建仓库
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724104259860.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724104308502.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)


创建好以后是这样的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724104314167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)
# 5.建立本地仓库与线上仓库的联系
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724104357736.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)

在创建好的本地文件夹打开git bash
运行上述两行代码
~~~
git remote add origin  *******
git push -u origin master
~~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724104600545.png)

等它上传完你就可以在github上面看到你上传的代码了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200724104607821.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhaXR0aW5nX2luX2xvdmU=,size_16,color_FFFFFF,t_70)
