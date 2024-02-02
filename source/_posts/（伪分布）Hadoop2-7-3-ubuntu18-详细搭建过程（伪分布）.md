---
title: （伪分布）Hadoop2.7.3+ubuntu18+详细搭建过程（伪分布）
date: 2024-02-02 11:26:44
tags: 大数据
author: 皇帝
---
```bash
准备工作：windows 10安装ssh工具（开启服务），将jdk以及Hadoop上传至虚拟机（scp -r root@master:/root）
(提示services.msc服务管理系统)
一、
ssh无密登录:
1，生成密钥对
ssh-keygen -t rsa
2，公钥复制
ssh-copy-id 主节点
二、
安装jdk:
编辑profile文件，设置环境变量
export JAVA_HOME=/opt/jdk
export PATH=$PATH:${JAVA_HOME}/bin
三、
安装、配置hadoop:
编辑profile文件，设置环境变量
export HADOOP_HOME=/opt/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
建立Hadoop工作目录
tmp
dfs/{name,data}
配置如下文件
-------------------------------------------------------------------------
vi hadoop-env.sh
export JAVA_HOME=/opt/jdk
-------------------------------------------------------------------------
vi core-site.xml
<property>
<name>fs.defaultFS</name>
<value>hdfs://master:9000</value>
</property>
<property>
<name>hadoop.tmp.dir</name>
<value>/opt/hadoop/tmp</value>
</property>
-------------------------------------------------------------------------
vi hdfs-site.xml(此配置中dfs.permissions在正式服务器上可不能写成false,原因自行百度)
<property>
<name>dfs.namenode.secondary.http-address</name>
<value>master:50090</value>
</property>
<property>
<name>dfs.permissions</name> 
<value>false</value> 
</property>
<property>
<name>dfs.replication</name>
<value>1</value>
</property>
<property>
<name>dfs.namenode.name.dir</name>
<value>/opt/hadoop/dfs/name</value>
</property>
<property>
<name>dfs.datanode.data.dir</name>
<value>/opt/hadoop/dfs/data</value>
</property>
-------------------------------------------------------------------------
vi yarn-site.xml
<property>
<name>yarn.resourcemanager.hostname</name>
<value>master</value>
</property>
<property>
<name>yarn.nodemanager.aux-services</name>
<value>mapreduce_shuffle</value>
</property>
-------------------------------------------------------------------------
vi mapred-site.xml
<property>
<name>mapreduce.framework.name</name>
<value>yarn</value>
</property>
<property>
<name>mapreduce.jobhistory.address</name>
<value>master:10020</value>
</property>
<property>
<name>mapreduce.jobhistory.webapp.address</name>
<value>master:19888</value>
</property>
-------------------------------------------------------------------------
vi slaves
完成之后浏览器打开master:8088/50070/50090/19888查看即可(在win系统下需要指明master的IP地址)

```


