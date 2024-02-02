title: JavaWeb框架学习（SSM）spring入门
tags:
  - SSM
categories: []
author: 皇帝
date: 2024-02-01 14:35:28
---
Spring是什么？
spring英译为春天。
**本教程只讲操作。不会涉及原理**
## spring初体验

 1. 创建一个maven项目
 2. 在pom.xml中导入spring的坐标（导入坐标可以去[maven坐标官网](https://mvnrepository.com/)直接搜索spring，选择版本号点击，复制，导入即可）
 3. 创建测试所需要的类
 目录结构![在这里插入图片描述](https://img-blog.csdnimg.cn/98a18792f3624231949587795f7fc4d3.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/0105b448805d4631ae585e03a71edd1a.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/3238b3fbf55c4672adcb2d3af59b1c53.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/75bc7d1578544a5ab6dbe3c8b10c030b.png)

 4. 创建并配置spring配置文件（**applicationContext.xml**这里文件名字和上方的UserDaoDemo类中加载的xml文件的名字需一致）
 下面是头文件，复制粘贴即可
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">
 <bean id="userDao" class="com.zjc.dao.Impl.UserDaoImpl"></bean>
</beans>
```
5. 然后运行UserDaoDemo中的main方法即可看见控制台打印![在这里插入图片描述](https://img-blog.csdnimg.cn/02811631d72948fea0fd2a3007820b3f.png)

## spring配置文件详解
spring加载配置并不是只有xml这一种方式，还有其他方式，我后面说

```xml
 <bean id="userDao" class="com.zjc.dao.Impl.UserDaoImpl"></bean>
```
首先看这一句，id代表唯一标识符，class代表要让spring容器创建的接口的实现类
这句话意思就是要把实现类交给spring容器，让他来帮我们创建，我们只需要在使用的时候通过唯一标识符id去找spring容器要即可
scope作用范围：
常见属性有singleton和prototype。
singleton代表单例（默认值）
prototype代表多例
#### 接下来我们测试一下
**当scope值为singleton（默认值）时，编写测试类代码如下**![在这里插入图片描述](https://img-blog.csdnimg.cn/c192f7b61efe4e46b91540fb4f6fa848.png)
两次获取的bean地址是一样的，也就是单例  ，只有一个bean对象
**当scope值为prototype时，测试类不变 ，结果如下**![在这里插入图片描述](https://img-blog.csdnimg.cn/6b212db5debf4ddba1feb74ab2a7d0f3.png)
地址不一样，说明创建了两个
知识点：**spring到底是怎么帮我们创建的呢？这里其实用到了反射，也就是通过类的无参构造来帮我们创建，如果一个类没有无参构造，那该咋办？凉拌。**
#### 接下来如何给普通属性通过配置文件去注入值呢？
**有两种方法，一种是通过构造方法，另一种是通过set方法，我这里只介绍set方法**
![在这里插入图片描述](https://img-blog.csdnimg.cn/c405534189574cba8d9e5eac4b271634.png)
这里的name指的是set方法的后半段名字，比如方法名叫setName，那么这里就填的是name
value代表要赋的值
如果要注入的类型是列表或者是map，又或者是类呢？![在这里插入图片描述](https://img-blog.csdnimg.cn/46ec79ca8ca64d9b8b71eeb11e842d81.png)
看图就明白了，**唯一要注意下面给类注入要使用ref属性链接上面的id为user的bean设置值即可。**