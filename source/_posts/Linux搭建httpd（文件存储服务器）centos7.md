---
title: Linux搭建httpd（文件存储服务器）centos7
date: 2024-02-02 11:16:00
tags: Linux
author: 皇帝
---
注：（服务器必须把安全组相应的端口开放）
先来看效果，如下图
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020091411802.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NzcwNTMx,size_16,color_FFFFFF,t_70#pic_center)
首先安装httpd：`yum -y install httpd`
然后修改配置文件`vi /etc/httpd/conf/httpd.conf`
修改两项内容一个是监听的端口，一个是设置字符
![在这里插入图片描述](https://img-blog.csdnimg.cn/2020102009281077.png#pic_center)
这项是为了在网页上面显示**中文**正常
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020092924979.png#pic_center)
这个设置端口，默认是80，看自己情况（我这里是因为80端口已经被我的网站占用了，但是我又想在这个服务器上面布置一个文件服务器，所以改成了1314，这里改变了安全组的端口也一定要开放）
然后启动服务：`systemctl start httpd`
他的默认目录在`/var/www/html/`底下，可以在这里创建一个文件夹先，然后将文件存放在这里，
打开浏览器访问
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201020093345918.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ0NzcwNTMx,size_16,color_FFFFFF,t_70#pic_center)


