---
title: 使用Gitea和Jenkins实现代码管理和自动化部署
date: 2022-11-18 09:54:39
tags: [CentOS,教程,git,jenkins]
categories: 服务器系列
top_img: https://oss.fycoder.top/Blog/giteaTopPic.jpg
cover: https://oss.fycoder.top/Blog/giteaTopPic.jpg
---



## 使用Gitea和Jenkins实现代码管理和自动化部署

### 一、前言

许多个人开发者或者小型公司会有小型的代码管理和自动化部署的需求（大型公司有自己的成熟体系，咱也不敢随意揣摩），今天给大家带来一套代码管理和自动化部署体系的搭建教程，希望能对大家有所帮助。此教程操作系统使用的是Centos7.9，使用yum包管理工具。

### 二、Git安装

1.gitea需要安装git2.0以上，查看git版本

```bash
git --version
```

2.如果版本低于2.0，先卸载git

```bash
yum -y remove git
```

3.安装高版本git

```bash
wget http://opensource.wandisco.com/centos/7/git/x86_64/wandisco-git-release-7-1.noarch.rpm  
rpm -ivh wandisco-git-release-7-1.noarch.rpm
yum install git -y
```

4.查看git版本

```bash
git --version
```

### 三、MySQL安装

安装Gitea前需要拥有一个数据库进行数据的存储，此处选择使用免费且使用广泛的MySQL。

1.首先下载MySQL的yum源配置

```bash
wget http://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
```

2.安装MySQL的yum源

```bash
yum -y install mysql57-community-release-el7-11.noarch.rpm
```

3.yum方式安装MySQL

```bash
yum -y install mysql-server --nogpgcheck
```

4.启动MySQL

```bash
systemctl start mysqld.service
systemctl status mysqld.service
```

5.命令行进入mysql

```bash
cat /var/log/mysqld.log | grep password
```

控制台将会打印出临时密码

![image-20221111140703383](https://oss.fycoder.top/markdown/202211111407519.png)

6.使用临时密码进入命令行

```bash
mysql -uroot -p
```

7.修改密码（密码根据需要修改）

```bash
ALTER USER USER() IDENTIFIED BY '123456Admin@123';
```

8.给其他机器授权能够访问mysql

```bash
grant all privileges on *.* to 'root'@'%' identified by '123456Admin@123' with grant option;

flush privileges;
```

9.Ctrl + D退出MySQL命令行

10.开放防火墙3306端口限制

```bash
firewall-cmd --zone=public --list-ports
firewall-cmd --zone=public --add-port=3306/tcp --permanent
firewall-cmd --reload
```

11.使用数据库连接工具，连接成功

![image-20221111141528108](https://oss.fycoder.top/markdown/202211111415163.png)

12.创建一个名为gitea的数据库为下一步做准备，字符集为utf8mb4。

### 四、Gitea安装

Gitea是一个轻量级代码托管解决方案，相比于Gitlab它会少一些功能，但是所占用的内存会小很多，且Git仓库的基本功能基本都有，对于账号少的代码管理非常实用，有兴趣可以去它们[官网]([Gitea](https://gitea.io/zh-cn/))查看详细文档。

下面是安装过程：

此次使用二进制包进行直接安装，请使用非root用户执行安装操作。

1.下载安装程序并启动

```bash
#下载二进制包
wget -O gitea https://dl.gitea.io/gitea/1.17.3/gitea-1.17.3-linux-amd64
#给安装包赋予权限
chmod +x gitea
#执行安装程序
./gitea web
```

当看见打印 Starting new Web server: tcp:0.0.0.0:3000时说明安装程序已正常启动。

2.开放防火墙3000

```bash
firewall-cmd --zone=public --list-ports
firewall-cmd --zone=public --add-port=3000/tcp --permanent
firewall-cmd --reload
```

3.用浏览器打开服务器的3000端口进行下一步安装操作

![image-20221111142311599](https://oss.fycoder.top/markdown/202211111423684.png)

4.初始配置

使用上面安装的MySQL账密进行配置，建议在MySQL新开一个用户用于Gitea操作不要使用root用户。

![image-20221111144826759](https://oss.fycoder.top/markdown/202211111448806.png)

5.一般配置

配置站点名称、系统用户、端口等数据，特别提醒，所提供的系统用户需要有对仓库根目录的访问权限。在服务器域名栏输入服务器IP地址或者域名。

![image-20221111145153439](https://oss.fycoder.top/markdown/202211111451497.png)

6.可选配置

可在此处配置邮件服务、三方服务、管理员账号等配置，这个根据需要自行选择。不设置管理员账号情况下，第一个注册的用户自动成为管理员。

![image-20221111145446249](https://oss.fycoder.top/markdown/202211111454287.png)

7.点击立即安装，等待安装完成。刷新后出现如下页面表示安装完成。

![image-20221111152544585](https://oss.fycoder.top/markdown/202211111525669.png)

8.创建服务使用systemctl管理gitea

```bash
vi /usr/lib/systemd/system/gitea.service
```

写入

```bash
[Unit]
Description=gitea

[Service]
User=root
ExecStart=/usr/local/gitea/gitea
Restart=on-abort

[Install]
WantedBy=multi-user.target
```

文件路径和启用用户根据需要修改。后面就可以通过以下命令管理gitea了。（第一次使用需要先将之前手动启用的gitea手动结束）

```bash
systemctl start gitea
systemctl stop gitea
systemctl status gitea
```

### 五、Jenkins安装

1.使用yum安装jenkins

```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
yum -y install jenkins
```

2.启动jenkins，如果启动报错大概率是jdk版本问题，可以尝试安装新版本jdk

```bash
systemctl restart jenkins.service
```

3.开放8080端口，jenkins默认运行在8080端口

```bash
firewall-cmd --zone=public --add-port=8080/tcp --permanent
firewall-cmd --reload
```

4.打开浏览器，进入服务器的8080端口，出现如下页面表示安装成功

![image-20221111171004120](https://oss.fycoder.top/markdown/202211111710293.png)

5.解锁Jenkins

根据页面提示，在指定文件中读出初始密码，输入到输入框内，点击继续解锁Jenkins

6.安装插件，新手建议直接安装推荐插件。

7.设置管理员账号

![image-20221111172719266](https://oss.fycoder.top/markdown/202211111727363.png)

8.进入这个页面表示Jenkins启动正常。

![image-20221111174924983](https://oss.fycoder.top/markdown/202211111749095.png)

### 六、Gitea和Jenkins配合实现自动化部署

以下为演示自动化部署的案例，其中类似nginx和node等环境均不是必须环境，需要根据需求更换。

1.在gitea创建一个仓库

![image-20221111175601495](https://oss.fycoder.top/markdown/202211111756582.png)

2.在本地创建一个Vue项目（代码具体内容根据需要修改）

```bash
vue create test-jenkins
```

3.将Vue项目与gitea仓库关联

```bash
cd .\test-jenkins\
git init
git add .
git commit -m "init"
git remote add origin http://192.168.0.192:3000/root/test-jenkins.git
git push -u origin main
```

4.在jenkins新建一个Freestyle项目

![image-20221112090737327](https://oss.fycoder.top/markdown/202211120907493.png)

5.填写git地址和用户名密码

![image-20221112090914635](https://oss.fycoder.top/markdown/202211120909713.png)

6.在构建步骤中选择execute shell，并输入以下内容，点击保存

```bash
npm i --registry https://registry.npm.taobao.org
npm run build
rm -rf /usr/share/nginx/html/test-jenkins/*
cp -r dist/* /usr/share/nginx/html/test-jenkins/
nginx -s reload
```

7.在/usr/share/nginx/html/下新建一个test-jenkins文件夹

```bash
cd /usr/share/nginx/html/
mkdir test-jenkins
```

8.配置nginx

```bash
vim /etc/nginx/conf.d/default.conf
```

在location /段落下增加以下内容

```bash
    location /test-jenkins {
        root  /usr/share/nginx/html;
        index index.html index.htm;
    }
```

9.点击Jenkins上的立即构建，等待构建成功

![image-20221112114533807](https://oss.fycoder.top/markdown/202211121145908.png)

10.打开网页输入对应网站，发现构建成功。至此，一键点击部署项目的功能已经完成，以下为git提交自动部署步骤。

![image-20221112114604189](https://oss.fycoder.top/markdown/202211121146265.png)

11.安装Generic Webhook Trigger插件，等待重启。

![image-20221116141328569](https://oss.fycoder.top/markdown/202211161413658.png)

![image-20221116141450722](https://oss.fycoder.top/markdown/202211161414797.png)

12.在项目配置中，选择构建触发器，选择Generic Webhook Trigger，在token栏输入自定义token,点击保存。

![image-20221116153022332](https://oss.fycoder.top/markdown/202211161530669.png)

![image-20221116153323847](https://oss.fycoder.top/markdown/202211161533137.png)

13.在gitea中添加webhook的白名单，路径为/home/安装用户/gitea/custom/conf/app.ini

在[webhook]栏中增加ALLOWED_HOST_LIST = *，如果没有webhook栏则新增。

添加后保存并重启gitea。

```bash
[webhook]
ALLOWED_HOST_LIST = *
```

14.进入Gitea的仓库设置

![image-20221116153456916](https://oss.fycoder.top/markdown/202211161534179.png)

15.点击web钩子，点击添加web钩子，选择gitea

![image-20221116153545823](https://oss.fycoder.top/markdown/202211161535024.png)

16.在地址栏输入jenkins设置中的地址，例如 **http://192.168.0.192:8080/generic-webhook-trigger/invoke?token=test**其中ip地址和token内容需要自行更换。点击添加钩子。

![image-20221118093045088](https://oss.fycoder.top/markdown/202211180930166.png)

17.回到编辑页，点击测试推送，等待一段时间后看到左侧为绿色后即配置正常，打开jenkins看到已经开始部署。之后git仓库被提交时将会主动发送webhook给jenkins，jenkins将会自动进行项目的部署。

![image-20221118093324920](https://oss.fycoder.top/markdown/202211180933015.png)

![image-20221118093358231](https://oss.fycoder.top/markdown/202211180933320.png)
