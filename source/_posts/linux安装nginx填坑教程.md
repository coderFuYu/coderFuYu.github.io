---
title: linux安装nginx填坑教程
date: 2020-11-06 14:17:57
tags: [linux,nginx,教程]
categories: 教程
keywords: nginx
top_img: https://oss.fycoder.top/Blog/nginxTopPic.png
cover: https://oss.fycoder.top/Blog/nginxTopPic.png
---
# 前言
阿里云之前白送域名，脑子一热就买了一年的学生服务器，尝试了一下linux，在装nginx这里碰到了几次坑，重装了好几次，记录一下
#  详细步骤
## 1.安装依赖

```bash
yum -y install gcc gcc-c++ automake pcre pcre-devel zlib zlib-devel open openssl-devel
```
##  2.进入下载路径

```bash
cd /etc
```
##  3.下载安装包（此处下载了1.16.1版本）

```bash
 wget http://nginx.org/download/nginx-1.16.1.tar.gz 
```
##  4.解压

```bash
tar -zxvf nginx-1.16.1.tar.gz
```
##  5.修改文件夹名称,进入

```bash
mv nginx-1.16.1 nginx
cd nginx
```
##  6.配置

```bash
./configure
```
##  7.编译

```bash
make
make install
```
##  8.进入生成的文件夹

```bash
cd /usr/local/nginx/sbin
```
##  9.验证安装成功并启动

```bash
./nginx -t
./nginx
```
#  设置linux环境变量
如果之前的步骤没有报错的话，你已经可以正常使用nginx的功能了，可是，每次要进入文件夹打开、停止、重启nginx未免有些麻烦，要直接使用nginx指令还需要配置环境变量
##  1.修改linux系统变量

```bash
vim /etc/profile
```
在配置文件最下面加上

```bash
export PATH=$PATH:/usr/local/nginx/sbin
```
保存
##  2.刷新配置

```bash
source /etc/profile
```
##  3.测试

```bash
nginx -t
```
如果出现下面提示说明安装已大功告成！恭喜![在这里插入图片描述](https://img-blog.csdnimg.cn/2020110614132698.png#pic_center)

