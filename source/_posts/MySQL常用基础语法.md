title: MySQL常用基础语法
tags: 数据库
author: 皇帝
categories: []
date: '2024-02-06T06:10:08.000Z'
---
# 环境准备

 - 3台CentOS7（本例使用VMware替代）
 - jdk8
 - Hadoop3.3
# 虚拟机基础准备
 - 网络适配器选用NAT模式（针对VMware）
 - 设置静态IP，对应关系如下
 
|名称|地址|
|--|--|
| hadoop01 |192.168.138.201|
| hadoop02 |192.168.138.202|
| hadoop03 |192.168.138.203|
- 设置主机名，在各自机器上执行`hostnamectl set-hostname 主机名称`即可
- 将jdk、Hadoop安装包分别上传至**hadoop01**:/hadoop目录下

> 接下来的操作，我会标明在那个机器里面操作，执行的时候**切记**不要执行错了

# hadoop01中执行
#### 解压安装包
```bash
cd /hadoop
// 解压jdk
tar -xzvf jdk*
// 解压hadoop
tar -xzvf hadoop*
// 删除安装包
rm -rf ./*.gz
// 重命名jdk
mv jdk* jdk8
// 重命名hadoop
mv hadoop* hadoop3
```
#### 安装jdk

> 使用命令`vi /etc/profile`将如下代码添加到末尾

```bash
export JAVA_HOME=/hadoop/jdk8
export CLASSPATH=.$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$JAVA_HOME/bin:$PATH
```

> 配置完成后执行`source /etc/profile`重新加载配置文件
> 然后使用`java -version`测试jdk是否安装成功
#### 安装Hadoop
1、配置环境变量
> 使用命令`vi /etc/profile`将如下代码添加到末尾

```bash
export HADOOP_HOME=/hadoop/hadoop3
export PATH=$HADOOP_HOME/bin:$HADOOP_HOME/sbin:$PATH
```

> 配置完成后执行`source /etc/profile`重新加载配置文件

2、修改Hadoop配置文件

> 涉及修改的文件有**core-site.xml、hadoop-env.sh、hdfs-site.xml、mapred-site.xml、yarn-site.xml、workers**

 - core-site.xml
```xml
 <property>
     <name>fs.defaultFS</name>
     <value>hdfs://hadoop01:9000</value>
 </property>
```
 - hadoop-env.sh
```bash
export JAVA_HOME=/hadoop/jdk8
export HDFS_NAMENODE_USER="root"
export HDFS_DATANODE_USER="root"
export HDFS_SECONDARYNAMENODE_USER="root"
export YARN_RESOURCEMANAGER_USER="root"
export YARN_NODEMANAGER_USER="root"
```
 - hdfs-site.xml
```xml
  <property>
    <name>dfs.replication</name>
    <value>2</value>
  </property>
  <property>
    <name>dfs.namenode.name.dir</name>
    <value>/hadoop/hadoop3/hdfs/name</value>
  </property>
  <property>
    <name>dfs.datanode.data.dir</name>
    <value>/hadoop/hadoop3/hdfs/data</value>
  </property>
<!-- nn web 端访问地址--> 
  <property> 
    <name>dfs.namenode.http-address</name> 
    <value>hadoop01:9870</value> 
  </property> 
  <property>
    <name>dfs.permissions</name>
    <value>false</value>
  </property>
```
 - mapred-site.xml
 

```xml
  <property>
     <name>mapreduce.framework.name</name>
     <value>yarn</value>
  </property>
```

 - yarn-site.xml
 

```xml
  <property>
    <name>yarn.resourcemanager.hostsname</name>
    <value>hadoop01</value>
  </property>
  <property>
    <name>yarn.resourcemanager.webapp.address</name>
    <value>hadoop01:8088</value>
  </property>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>
```

 - workers
 
```bash
hadoop01
hadoop02
hadoop03
```

#### 修改hosts文件

> 使用`vi /etc/hosts`编辑文件，在末尾加入

```bash
192.168.138.201 hadoop01
192.168.138.202 hadoop02
192.168.138.203 hadoop03
```
#### 将jdk、hadoop等文件分发给其他机器

```bash
// hadoop02
// 配置文件
scp -r /etc/profile hadoop02:/etc/
// 映射关系
scp -r /etc/hosts hadoop02:/etc/
// 安装文件
scp -r /hadoop hadoop02:/

// hadoop03
scp -r /etc/profile hadoop03:/etc/
scp -r /hadoop hadoop03:/
scp -r /etc/hosts hadoop03:/etc/
```
# 以下命令三个机器全部执行
1、刷新配置文件

```bash
source /etc/profile
```
2、配置ssh免密登录

 - 生成密钥（一路回车直到结束）
 

```bash
ssh-keygen -t rsa
```

 - 分发公钥
 

```bash
ssh-copy-id hadoop01
ssh-copy-id hadoop02
ssh-copy-id hadoop03
```
3、关闭防火墙

```bash
systemctl stop firewalld.service
systemctl disable firewalld.service
```
# hadoop01中执行

 - 格式化namenode
 

```bash
hdfs namenode -format
```
- 启动hadoop

```bash
start-all.sh
```
到此搭建已经完成，访问web页面`http://192.168.138.201:8088`即可，