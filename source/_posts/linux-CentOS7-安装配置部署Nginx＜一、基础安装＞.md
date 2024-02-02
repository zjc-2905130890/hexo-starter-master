---
title: linux(CentOS7)安装配置部署Nginx＜一、基础安装＞
date: 2024-02-02 11:20:16
tags: Linux
author: 皇帝
---

关于Nginx的安装部署等步骤，[官网](http://nginx.org/en/linux_packages.html#dynmodules)其实也给了相关说明，这里我结合官方文档以及自己的理解给大家解释一下有关步骤，顺便为大家踩踩坑
**安装采用yum方式**
**注意：我这里用的都是root权限**
首先第一步：安装yum工具包**这东西是针对yum的，其实装不装都行**

```bash
yum install yum-utils -y
```
然后配置nginx的yum源，有了这东西，可以直接使用yum安装，

```bash
vi /etc/yum.repos.d/nginx.repo
```
写入如下内容：
**解释一下这里的文件：
nginx-stable这栏，是比较稳定的安装包（有可能不是最新）
nginx-mainline这个是主线包(就是最新的，但是实际使用中，我们通常不会使用最新的)
参数gpgcheck，意思是下载文件是否进⾏gpg校验，0代表不检查，1代表检查
咱们设置为0就行，官网是1，但是1的话安装指定某些版本的时候可能会校验失败**
```bash
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=0
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```
yum源配置好之后，加载一下

```bash
yum clean
yum makecache
```
安装nginx(默认选择当前最新的稳定版本)

```bash
yum install nginx -y
```
如果想指定版本，则

```bash
yum install nginx-1.12.2 -y
```
安装完成之后，
启动

```bash
systemctl start nginx
```
设置开机自启

```bash
systemctl enable nginx
```
如果想关闭开机自启，则

```bash
systemctl disable nginx
```
启动之后，查看服务状态

```bash
systemctl status nginx
```
如下图，即为正在运行![运行成功](https://img-blog.csdnimg.cn/2fa43ebfc8914cf5b1988731e90630fa.png)
此时安装步骤已然完成，打开浏览器，输入主机名开始访问即可**默认端口是80**![访问成功](https://img-blog.csdnimg.cn/e8cd2b26558c4b4391d44cc7e7c0cf9c.png)

