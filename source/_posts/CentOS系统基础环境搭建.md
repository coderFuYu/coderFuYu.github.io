---
title: CentOS系统基础环境搭建
date: 2022-09-25 21:44:39
tags: [CentOS,教程]
categories: 教程
keywords: CentOS
top_img: https://oss.fycoder.top/Blog/centos%20top%20pic.png
cover: https://oss.fycoder.top/Blog/centos%20top%20pic.png
---
## CentOS系统基础环境搭建

### 前言

使用了好久的CentOS系统，一直没有很好地整理CentOS的使用笔记，乘着这次兴起搭建博客的机会，一起写了吧，说不定以后还能查呢嘿嘿嘿。

### 一、CentOS镜像下载

CentOS的官网是[The CentOS Project](https://www.centos.org/)，可以在这里寻找到最新最全的镜像。当然，因为这是国外服务器，下载会很慢，建议使用阿里云镜像库下载[centos安装包下载_开源镜像站-阿里云 (aliyun.com)](https://mirrors.aliyun.com/centos/)。

之前大家使用最广的是CentOS7.9 2009版本，我们这次就以这个为例进行讲解。温馨提醒，CentOS7即将停止社区维护，如果想要获取最新维护版本请在官网选择。

进入阿里云镜像站，选择centos7.9.2009，选择isos，选择x86_64,选择下图镜像进行下载。

![image-20220816201205148](https://oss.fycoder.top/markdown/202208162012222.png)

如果是物理机安装系统，需要使用U盘和镜像写入工具，通过物理机的BIOS系统打开并安装，这里不详细展开。

如果是虚拟机安装，只需要选择下载好的镜像新建虚拟机即可。

### 二、CentOS系统安装

无论是虚拟机安装还是物理机安装，成功进入镜像安装后都会进入下面步骤：

1.直接回车，安装。

![image-20220816215903540](https://oss.fycoder.top/markdown/202208162159610.png)

2.等待系统检查资源。

![image-20220816215955892](https://oss.fycoder.top/markdown/202208162159939.png)

3.等待一会后进入图形化安装界面，选择简体中文，点击继续。

![image-20220816220135962](https://oss.fycoder.top/markdown/202208162201005.png)

点击软件选择，勾选开发环境和安全性工具，点击完成。

![image-20220816220259241](https://oss.fycoder.top/markdown/202208162202285.png)

点击安装位置，确认无误，点击完成，没有问题就可以点击开始安装了。

![image-20220816220406110](https://oss.fycoder.top/markdown/202208162204151.png)

![image-20220816220419986](https://oss.fycoder.top/markdown/202208162204032.png)

6.在安装过程中设置ROOT密码并可以选择创建用户。

![image-20220816220541111](https://oss.fycoder.top/markdown/202208162205159.png)

7.等待安装完成。

![image-20220816220620603](https://oss.fycoder.top/markdown/202208162206655.png)

8.安装成功后，点击重启。

![image-20220816220929402](https://oss.fycoder.top/markdown/202208162209447.png)

9.进入这个页面不用选择，等待自己进入或者直接回车。

![image-20220816221015364](https://oss.fycoder.top/markdown/202208162210414.png)

10.输入root和你设置的密码进行登录，密码输入是不显示的。

![image-20220816221046543](https://oss.fycoder.top/markdown/202208162210593.png)

当正确登录，如下显示时就说明你安装成功且成功登录CentOS系统了。

![image-20220816221154581](https://oss.fycoder.top/markdown/202208162211623.png)

### 三、使用SSH连接服务器

1.首先，我们需要先打开CentOS系统的网卡。

```bash
vi /etc/sysconfig/network-scripts/ifcfg-enc33
```

输入a进入编辑模式，将ONBOOT=no改为ONBOOT=yes，按esc退出编辑模式，输入ZZ保存退出。

重启网络服务

```bash
service network restart
```

2.查询服务器ip地址，并ping百度

```bash
ip addr
ping www.baidu.com
```

其中192.168.32.169就是这台服务器的ip

![image-20220816223654581](https://oss.fycoder.top/markdown/202208162236635.png)

3.使用finalshell、xshell等工具连接服务器的SSH服务。

选择新建一个SSH连接。

![image-20220816224222775](https://oss.fycoder.top/markdown/202208162242827.png)

填写服务器信息，点击确定。

![image-20220816224353592](https://oss.fycoder.top/markdown/202208162243646.png)

在连接列表点击新建的服务器，第一次连接会询问是否接受秘钥，点击保存。当进入如下页面时说明SSH连接成功。这时就不再需要面对黑漆漆的CentOS本机了，可以通过更智能的SSH工具控制你的CentOS系统了。

![image-20220816235640869](https://oss.fycoder.top/markdown/202208162356935.png)

![image-20220816224512037](https://oss.fycoder.top/markdown/202208162245090.png)

### 四、更换yum镜像库

安装wget

```bash
yum install wget
```

进入镜像管理文件夹

```bash
cd /etc/yum.repos.d
```

备份原镜像

```bash
mv CentOS-Base.repo CentOS-Base.repo.bak
```

下载阿里云镜像源

```bash
wget -O CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
```

清空缓存

```bash
yum clean all
```

重新建立缓存

```bash
yum makecache
```

### 五、安装Java环境

查看yum库中的java安装包 ：

```bash
yum -y list java*
```


安装需要的jdk版本的所有java程序：

```bash
yum -y install java-1.8.0-openjdk
```

(安装完之后，默认的安装目录是在: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.345.b01-1.el7_9.x86_64)

查看java版本

```bash
java -version
```



### 六、安装NGINX

将nginx添加至yum源

```bash
sudo rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```

查看yum库中的nginx列表

```bash
yum -y list nginx
```

安装nginx

```bash
yum -y install nginx
```

查看nginx 版本

```bash
nginx -v
```

启动nginx

```bash
nginx
```



### 七、安装nodejs环境

将nodejs添加至yum库

```bash
curl -sL https://rpm.nodesource.com/setup_16.x | bash
```

查看yum库中的nodejs列表

```bash
yum list nodejs*
```

安装nodejs

```bash
yum install -y nodejs
```

查看nodejs版本

```bash
node -v
```

