---
layout: post
title:  "java spring"
date:   2023-05-11 08:54:15 +0800
categories: jekyll javaSpring
---



# ioc控制反转

在java web中程序耦合度高，改一点就要全改，所以要解耦。

ioc控制反转inversion of control 

由主动new一个对象转变为,由外部提供对象，此过程中对象控制权由程序转移到外部（ioc容器）。 

ioc容器负责创建对象和初始化对象，被创建或者被管理的对象在ioc容器中统称为bean（如service对象和dao）

DI（dependency injection）依赖注入

在容器中建立bean和bean之间的依赖关系的过程称为依赖注入（service->dao）

# ioc入门案例思路分析

1.管理什么？（service与dao)

2.如何将被管理的对象告知ioc容器？（配置）

3.被管理的对象交给ioc容器，如何获取ioc容器？（接口）

4.ioc容器被得到后，如何从容器中获取bean？（接口方法）

5.使用spring导入哪些坐标（pom.xml）



ioc入门操作：

1.导包

```
<dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.10.RELEASE</version>
 </dependency>
```

2.创建配置文件

resources文件创建applicationContext.xml文件

​    2.1配置bean    id为给bean起名字，class属性表示给bean定义类型

```
<bean id="bookservice" class="com.实现类"> 要实现类不要接口
```

3.获取ioc容器  

```
applicationContext ctx= new classpathxmlpathapplicationcontext("xml配置文件")
```

4.获取bean

```
bookservice bookservice=(bookservice) ctx.getbean("bookbean")  要进行强转类型
```



DI入门操作：

5.删除serviceimpl层使用new的方式创建dao对象的方式

```
private bookdao bookdao  这是一个dao层对象
```

6.提供对应的set方法用于设置service层dao对象

7.写配置service与dao的关系

property标签表示配置当前bean的属性

name属性表示配置哪一个具体的属性

ref属性表示参照哪一个bean

```
<property name="bookdao" ref="bookdao"> name指的是步骤5下面的dao  ref的dao指的是xml里的dao bean
```



疑问：为什么要在service中创建private bookdao 它是怎么通过set方法实现实现获取dao对象的

应该是spring在内部调用了set方法，从ioc容器中获取出dao对象，再通过set方法给serviceimpl。

# bean配置

### bean基础配置   

起别名：name属性 

```
<bean name="service service2 bookEbi"/>别名之间用空格 ， ；分隔开 
```

### bean的作用范围

spring默认给我们创建的对象为一个，默认为单例对象，如果要创建双例对象，则需要

```
<bean scope="pretotype"/>  singleton单例
```

### 为什么bean默认为单例

比如dao层的对象，用一次造一个对象，对ioc容器来说压力很大，所以不如都用同一个对象



# bean的实例化

## bean是如何创建的

bean本质是对象，创建bean使用构造方法完成，使用的是无参构造

报错从最后一行开始看

## 实例化bean的三种方式

1.使用构造方法实例化bean

2.使用静态工厂造对象  

```
<bean id="orderDao" class="com.rencai.factory" factory-method="getorderdao" 工厂哪个方法造的对象/>
```

工厂内getorderdao方法创建dao对象

3.使用实例工厂实例化bean

配置两个bean，一个bean用于实例工厂，另外一个bean用于实例dao 要多配置一个factory-bean=""

```
<bean id="factory" class="com.rencai.factorybean " /> 这个用于实例化工厂
<bean id="orderDao" factory-bean="orderdao" factory-method="factory" />factory-bean用于实例化factory，再用method实例化dao
```

4.使用factorybean实例化bean

```
<bean id="orderDao" class="com.rencai.factorybean " />
```

操作：

1.写一个userdaofactorybean的java类implements factorybean<userdao>造什么对象里面填什么

2.实现两个方法

两个方法的作用

```
getObject(){//代替原始实例工厂中创建对象的方法
return new 一个对象
}
getObjectType(){
return 一个对象.class
}
isSingleton() 单例非单例
```

3.配置步骤4的配置

# bean的生命周期

在xml配置的class里面写两个方法，init和destory方法，然后在xml配置里面写

```
<bean id="bookservice" class="" init-method="init" init方法是初始化的方法 destory-method="destory"
```

要想看到destory方法被执行，则需要classpathxmlapplicationcontext  这个类    p12  6.29

然后再调用.close方法  是个暴力方法

还有ctx.registershutdownhoook()方法 告诉它关闭java虚拟机之前要记得销毁 注册关闭钩子



使用接口的方式去初始化和销毁  （了解）

这样就不需要在xml文件里面配置init和destory了

操作：

implement后面加上 initializingbean 和disposablebean

然后覆盖

destory和afterpropertiesset方法

# 依赖注入方式

setter注入和构造器注入

setter：简单类型，引用类型

构造器注入：简单类型  引用类型




