# spring原理学习-tuling5-周瑜

- [spring原理学习-tuling5-周瑜](#spring原理学习-tuling5-周瑜)
	- [1、Spring底层核心原理解析](#1spring底层核心原理解析)
		- [Spring中是如何创建一个对象？](#spring中是如何创建一个对象)
		- [Bean的创建过程](#bean的创建过程)
		- [推断构造方法](#推断构造方法)
		- [AOP大致流程](#aop大致流程)
		- [Spring事务](#spring事务)
	- [2、手写模拟Spring底层原理](#2手写模拟spring底层原理)
	- [3、Spring之底层架构核心概念解析](#3spring之底层架构核心概念解析)
		- [BeanDefinition](#beandefinition)
		- [BeanDefinitionReader](#beandefinitionreader)
		- [AnnotatedBeanDefinitionReader](#annotatedbeandefinitionreader)
		- [XmlBeanDefinitionReader](#xmlbeandefinitionreader)
		- [ClassPathBeanDefinitionScanner](#classpathbeandefinitionscanner)
		- [BeanFactory](#beanfactory)
		- [ApplicationContext](#applicationcontext)
		- [AnnotationConfigApplicationContext](#annotationconfigapplicationcontext)
		- [ClassPathXmlApplicationContext](#classpathxmlapplicationcontext)
		- [国际化](#国际化)
		- [资源加载](#资源加载)
		- [获取运行时环境](#获取运行时环境)
		- [事件发布](#事件发布)
		- [类型转化](#类型转化)
		- [OrderComparator](#ordercomparator)
		- [BeanPostProcessor](#beanpostprocessor)
		- [BeanFactoryPostProcessor](#beanfactorypostprocessor)
		- [FactoryBean](#factorybean)
		- [ExcludeFilter和IncludeFilter](#excludefilter和includefilter)
		- [MetadataReader、ClassMetadata、AnnotationMetadata](#metadatareaderclassmetadataannotationmetadata)
	- [4、Spring之Bean生命周期源码解析（上）](#4spring之bean生命周期源码解析上)
		- [Bean的生成过程](#bean的生成过程)
		- [1. 生成BeanDefinition](#1-生成beandefinition)
		- [2. 合并BeanDefinition](#2-合并beandefinition)
		- [3. 加载类](#3-加载类)
		- [4. 实例化前](#4-实例化前)
		- [5.实例化](#5实例化)
			- [5.1 Supplier创建对象](#51-supplier创建对象)
			- [5.2 工厂方法创建对象](#52-工厂方法创建对象)
			- [5.3 推断构造方法](#53-推断构造方法)
		- [6. BeanDefinition的后置处理](#6-beandefinition的后置处理)
		- [7. 实例化后](#7-实例化后)
		- [8. 自动注入](#8-自动注入)
		- [9. 处理属性](#9-处理属性)
		- [10. 执行Aware](#10-执行aware)
		- [11. 初始化前](#11-初始化前)
		- [12. 初始化](#12-初始化)
		- [13. 初始化后](#13-初始化后)
		- [总结BeanPostProcessor](#总结beanpostprocessor)
	- [5、Spring之Bean生命周期源码解析（下）](#5spring之bean生命周期源码解析下)
		- [Bean的销毁过程](#bean的销毁过程)
	- [6、Spring之依赖注入源码解析（上）](#6spring之依赖注入源码解析上)
		- [Spring中到底有几种依赖注入的方式？](#spring中到底有几种依赖注入的方式)
			- [手动注入](#手动注入)
			- [自动注入](#自动注入)
				- [XML的autowire自动注入](#xml的autowire自动注入)
				- [@Autowired注解的自动注入](#autowired注解的自动注入)
		- [寻找注入点](#寻找注入点)
			- [static的字段或方法为什么不支持](#static的字段或方法为什么不支持)
			- [桥接方法](#桥接方法)
		- [注入点进行注入](#注入点进行注入)
			- [字段注入](#字段注入)
			- [Set方法注入](#set方法注入)
	- [7、Spring之依赖注入源码解析（下）](#7spring之依赖注入源码解析下)
		- [findAutowireCandidates()实现](#findautowirecandidates实现)
		- [关于依赖注入中泛型注入的实现](#关于依赖注入中泛型注入的实现)
		- [@Qualifier的使用](#qualifier的使用)
		- [@Resource](#resource)
	- [8、Spring之循环依赖底层源码解析](#8spring之循环依赖底层源码解析)
		- [什么是循环依赖？](#什么是循环依赖)
		- [Bean的生命周期](#bean的生命周期)
		- [三级缓存](#三级缓存)
		- [解决循环依赖思路分析](#解决循环依赖思路分析)
		- [总结](#总结)
			- [反向分析一下singletonFactories](#反向分析一下singletonfactories)
		- [课上github上issue地址：](#课上github上issue地址)
	- [9、Spring之推断构造方法源码解析](#9spring之推断构造方法源码解析)
		- [spring选择构造方法的4种方式](#spring选择构造方法的4种方式)
		- [推断构造方法大致流程](#推断构造方法大致流程)
		- [源码思路](#源码思路)
			- [autowireConstructor()](#autowireconstructor)
		- [为什么分越少优先级越高？](#为什么分越少优先级越高)
		- [@Bean的情况](#bean的情况)
	- [10、Spring之启动过程源码解析](#10spring之启动过程源码解析)
		- [前言分析](#前言分析)
		- [BeanFactoryPostProcessor](#beanfactorypostprocessor-1)
		- [BeanDefinitionRegistryPostProcessor](#beandefinitionregistrypostprocessor)
		- [如何理解refresh()？](#如何理解refresh)
		- [refresh()底层原理流程](#refresh底层原理流程)
		- [执行BeanFactoryPostProcessor](#执行beanfactorypostprocessor)
		- [Lifecycle的使用](#lifecycle的使用)
	- [11、Spring之配置类解析与扫描过程源码解析](#11spring之配置类解析与扫描过程源码解析)
		- [解析配置类](#解析配置类)
			- [解析配置类--总结一下](#解析配置类--总结一下)
	- [12、Spring之整合Mybatis底层源码解析](#12spring之整合mybatis底层源码解析)
		- [整合核心思路](#整合核心思路)
		- [Mybatis-Spring 1.3.2版本底层源码执行流程](#mybatis-spring-132版本底层源码执行流程)
		- [Mybatis-Spring  2.0.6版本(最新版)底层源码执行流程](#mybatis-spring--206版本最新版底层源码执行流程)
		- [Spring整合Mybatis后一级缓存失效问题](#spring整合mybatis后一级缓存失效问题)
	- [13、Spring之AOP底层源码解析（上）](#13spring之aop底层源码解析上)
		- [动态代理](#动态代理)
		- [ProxyFactory](#proxyfactory)
		- [Advice的分类](#advice的分类)
		- [Advisor的理解](#advisor的理解)
		- [创建代理对象的方式](#创建代理对象的方式)
			- [ProxyFactoryBean](#proxyfactorybean)
			- [BeanNameAutoProxyCreator](#beannameautoproxycreator)
			- [DefaultAdvisorAutoProxyCreator](#defaultadvisorautoproxycreator)
		- [对Spring AOP的理解](#对spring-aop的理解)
		- [AOP中的概念](#aop中的概念)
		- [Advice在Spring AOP中对应API](#advice在spring-aop中对应api)
		- [TargetSource的使用](#targetsource的使用)
		- [Introduction](#introduction)
		- [LoadTimeWeaver](#loadtimeweaver)
		- [对Spring AOP的理解(老版笔记)](#对spring-aop的理解老版笔记)
		- [AOP中的概念](#aop中的概念-1)
		- [Advice的分类](#advice的分类-1)
		- [Advice在Spring AOP中对应API](#advice在spring-aop中对应api-1)
		- [Spring AOP中的Advisor](#spring-aop中的advisor)
		- [创建代理对象的方式](#创建代理对象的方式-1)
			- [ProxyFactoryBean](#proxyfactorybean-1)
			- [BeanNameAutoProxyCreator](#beannameautoproxycreator-1)
			- [DefaultAdvisorAutoProxyCreatorDemo](#defaultadvisorautoproxycreatordemo)
		- [TargetSource的使用](#targetsource的使用-1)
	- [14、Spring之AOP底层源码解析（下）](#14spring之aop底层源码解析下)
		- [ProxyFactory选择cglib或jdk动态代理原理](#proxyfactory选择cglib或jdk动态代理原理)
		- [代理对象创建过程](#代理对象创建过程)
			- [JdkDynamicAopProxy](#jdkdynamicaopproxy)
			- [ObjenesisCglibAopProxy](#objenesiscglibaopproxy)
		- [代理对象执行过程](#代理对象执行过程)
				- [各注解对应的MethodInterceptor](#各注解对应的methodinterceptor)
		- [AbstractAdvisorAutoProxyCreator](#abstractadvisorautoproxycreator)
		- [@EnableAspectJAutoProxy](#enableaspectjautoproxy)
		- [Spring中AOP原理流程图（和下面老版图片一致）](#spring中aop原理流程图和下面老版图片一致)
		- [使用ProxyFactory通过编程创建AOP代理（老版笔记）](#使用proxyfactory通过编程创建aop代理老版笔记)
			- [ProxyFactory的工作原理](#proxyfactory的工作原理)
			- [JdkDynamicAopProxy创建代理对象过程](#jdkdynamicaopproxy创建代理对象过程)
			- [JdkDynamicAopProxy创建的代理对象执行过程](#jdkdynamicaopproxy创建的代理对象执行过程)
			- [ObjenesisCglibAopProxy创建代理对象过程](#objenesiscglibaopproxy创建代理对象过程)
			- [ObjenesisCglibAopProxy创建的代理对象执行过程](#objenesiscglibaopproxy创建的代理对象执行过程)
		- [使用“自动代理（autoproxy）”功能](#使用自动代理autoproxy功能)
			- [BeanNameAutoProxyCreator](#beannameautoproxycreator-2)
			- [DefaultAdvisorAutoProxyCreator](#defaultadvisorautoproxycreator-1)
		- [@EnableAspectJAutoProxy](#enableaspectjautoproxy-1)
		- [注解和源码对应关系](#注解和源码对应关系)
		- [Spring中AOP原理流程图](#spring中aop原理流程图)
		- [Introduction](#introduction-1)
	- [15、Spring之事务底层源码解析](#15spring之事务底层源码解析)
		- [@EnableTransactionManagement工作原理](#enabletransactionmanagement工作原理)
		- [Spring事务基本执行原理](#spring事务基本执行原理)
		- [Spring事务详细执行流程（和下面的老版一致）](#spring事务详细执行流程和下面的老版一致)
		- [Spring事务传播机制](#spring事务传播机制)
		- [Spring事务传播机制分类](#spring事务传播机制分类)
		- [案例分析](#案例分析)
			- [情况1](#情况1)
			- [情况2](#情况2)
			- [情况3](#情况3)
			- [情况4](#情况4)
		- [Spring事务强制回滚](#spring事务强制回滚)
		- [TransactionSynchronization](#transactionsynchronization)
		- [spring事物基本原理（老版笔记）](#spring事物基本原理老版笔记)
		- [开始事务](#开始事务)
		- [AutoProxyRegistrar](#autoproxyregistrar)
		- [ProxyTransactionManagementConfiguration](#proxytransactionmanagementconfiguration)
		- [TransactionInterceptor执行流程:](#transactioninterceptor执行流程)
		- [简单版流程](#简单版流程)
		- [传播机制](#传播机制)
		- [举例子](#举例子)
			- [情况1](#情况1-1)
			- [情况2](#情况2-1)
			- [情况3](#情况3-1)
			- [情况4](#情况4-1)



## 1、Spring底层核心原理解析

**本节课会把<font color="red">Spring中核心知识点都给大家进行串讲</font>，让大家对Spring的底层有一个整体的大致了解，** 比如：

- 1，Bean的生命周期底层原理
- 2，依赖注入底层原理
- 3，初始化底层原理
- 4，推断构造方法底层原理
- 5，AOP底层原理
- 6，Spring事务底层原理

但都只是大致流程，<font color ="red">**后续会针对每个流程详细深入的讲解并分析源码实现。**</font>

先来看看入门使用Spring的代码：

```java
ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
UserService userService = (UserService) context.getBean("userService");
userService.test();

```

对于这三行代码应该，大部分同学应该都是比较熟悉，这是学习Spring的hello world。可是，这三行代码底层都做了什么，比如：

+ 第一行代码，会构造一个ClassPathXmlApplicationContext对象，ClassPathXmlApplicationContext该如何理解，调用该构造方法除开会实例化得到一个对象，还会做哪些事情？
+ 第二行代码，会调用ClassPathXmlApplicationContext的getBean方法，会得到一个UserService对象，getBean()是如何实现的？返回的UserService对象和我们自己直接new的UserService对象有区别吗？
+ 第三行代码，就是简单的调用UserService的test()方法，不难理解。

光看这三行代码，其实**并不能体现出来Spring的强大之处**，也不能理解为什么需要ClassPathXmlApplicationContext和getBean()方法，随着课程的深入将会改变你此时的观念，而对于上面的这些疑问，也会随着课程深入逐步得到解决。对于这三行代码，你现在可以认为：如果你要用Spring，你就得这么写。就像你要用Mybatis，你就得写各种Mapper接口。

但是用ClassPathXmlApplicationContext其实已经过时了，在新版的Spring MVC和Spring Boot的底层主要用的都是AnnotationConfigApplicationContext，比如：

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
//ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("spring.xml");
UserService userService = (UserService) context.getBean("userService");
userService.test();
```

可以看到AnnotationConfigApplicationContext的用法和ClassPathXmlApplicationContext是非常类似的，只不过需要传入的是一个class，而不是一个xml文件。

而AppConfig.class和spring.xml一样，表示Spring的配置，比如可以指定扫描路径，可以直接定义Bean，比如：

spring.xml中的内容为：

```java
<context:component-scan base-package="com.zhouyu"/>
<bean id="userService" class="com.zhouyu.service.UserService"/>
```

AppConfig中的内容为：

```java
@ComponentScan("com.zhouyu")
public class AppConfig {
	
    @Bean
	public UserService userService(){
		return new UserService();
	}

}
```

所以spring.xml和AppConfig.class本质上是一样的。

目前，我们基本很少直接使用上面这种方式来用Spring，而是使用Spring MVC，或者Spring Boot，但是它们都是基于上面这种方式的，都需要在内部去创建一个ApplicationContext的，只不过：

+ 1, Spring MVC创建的是**XmlWebApplicationContext**，和  **ClassPathXmlApplicationContext**类似，都是基于XML配置的

+ 2, Spring Boot创建的是**AnnotationConfigApplicationContext**


因为AnnotationConfigApplicationContext是比较重要的，并且AnnotationConfigApplicationContext和ClassPathXmlApplicationContext大部分底层都是共同的，后续课程我们会着重将AnnotationConfigApplicationContext的底层实现，对于ClassPathXmlApplicationContext，同学们可以在课程结束后作为作业，业余时间看看相关源码即可。

### Spring中是如何创建一个对象？

其实不管是AnnotationConfigApplicationContext还是ClassPathXmlApplicationContext，目前，我们都可以简单的将它们理解为就是用来创建Java对象的，比如调用getBean()就会去创建对象（此处不严谨，getBean可能也不会去创建对象，后续课程详解）。

在Java语言中，肯定是根据某个类来创建一个对象的。我们在看一下实例代码：

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
UserService userService = (UserService) context.getBean("userService");
userService.test();

```

当我们调用context.getBean("userService")时，就会去创建一个对象，但是getBean方法内部怎么知道"userService"对应的是UserService类呢？

所以，我们就可以分析出来，在调用AnnotationConfigApplicationContext的构造方法时，也就是第一行代码，会去做一些事情：

+ 1, 解析AppConfig.class，得到扫描路径
+ 2, 遍历扫描路径下的所有Java类，如果发现某个类上存在@Component、@Service等注解，那么Spring就把这个类记录下来，存在一个Map中，比如Map<String, Class>。**（实际上，Spring源码中确实存在类似的这么一个Map，叫做BeanDefinitionMap，后续课程会讲到）**
+ 3,Spring会根据某个规则生成当前类对应的beanName，作为key存入Map，当前类作为value

这样，但调用context.getBean("userService")时，就可以根据"userService"找到UserService类，从而就可以去创建对象了。

### Bean的创建过程

那么Spring到底是如何来创建一个Bean的呢，这个就是Bean创建的生命周期，大致过程如下

- 1, 利用该类的构造方法来实例化得到一个对象（但是如何一个类中有多个构造方法，Spring则会进行选择，这个叫做**推断构造方法**）
- 2, 得到一个对象后，Spring会判断该对象中是否存在被@Autowired注解了的属性，把这些属性找出来并由Spring进行赋值（**依赖注入**）
- 3, 依赖注入后，Spring会判断该对象是否实现了BeanNameAware接口、BeanClassLoaderAware接口、BeanFactoryAware接口，如果实现了，就表示当前对象必须实现该接口中所定义的setBeanName()、setBeanClassLoader()、setBeanFactory()方法，那Spring就会调用这些方法并传入相应的参数（**Aware回调**）
- 4, Aware回调后，Spring会判断该对象中是否存在某个方法被@PostConstruct注解了，如果存在，Spring会调用当前对象的此方法（**初始化前**）
- 5, 紧接着，Spring会判断该对象是否实现了InitializingBean接口，如果实现了，就表示当前对象必须实现该接口中的afterPropertiesSet()方法，那Spring就会调用当前对象中的afterPropertiesSet()方法（**初始化**）
- 6, 最后，Spring会判断当前对象需不需要进行AOP，如果不需要那么Bean就创建完了，如果需要进行AOP，则会进行动态代理并生成一个代理对象做为Bean（**初始化后**）

通过最后一步，我们可以发现，当Spring根据UserService类来创建一个Bean时：

- 1, 如果不用进行AOP，那么Bean就是UserService类的构造方法所得到的对象。
- 2, 如果需要进行AOP，那么Bean就是UserService的代理类所实例化得到的对象，而不是UserService本身所得到的对象。

Bean对象创建出来后：

- 1, 如果当前Bean是单例Bean，那么会把该Bean对象存入一个Map<String, Object>，Map的key为beanName，value为Bean对象。这样下次getBean时就可以直接从Map中拿到对应的Bean对象了。（实际上，在Spring源码中，这个Map就是**单例池**）
- 2, 如果当前Bean是原型Bean，那么后续没有其他动作，不会存入一个Map，下次getBean时会再次执行上述创建过程，得到一个新的Bean对象。

### 推断构造方法

Spring在基于某个类生成Bean的过程中，需要利用该类的构造方法来实例化得到一个对象，但是<font color="red">**如果一个类存在多个构造方法，Spring会使用哪个呢？**</font>

Spring的判断逻辑如下：

- 1, 如果一个类只存在一个构造方法，不管该构造方法是无参构造方法，还是有参构造方法，Spring都会用这个构造方法
- 2, 如果一个类存在多个构造方法

        a,这些构造方法中，存在一个无参的构造方法，那么Spring就会用这个无参的构造方法
        b,这些构造方法中，不存在一个无参的构造方法，那么Spring就会**报错**


Spring的设计思想是这样的：

- 1, 如果一个类只有一个构造方法，那么没得选择，只能用这个构造方法
- 2, 如果一个类存在多个构造方法，Spring不知道如何选择，就会看是否有无参的构造方法，因为无参构造方法本身表示了一种默认的意义
- 3,不过如果某个构造方法上加了@Autowired注解，那就表示程序员告诉Spring就用这个加了注解的方法，那Spring就会用这个加了@Autowired注解构造方法了

需要重视的是，如果Spring选择了一个有参的构造方法，Spring在调用这个有参构造方法时，需要传入参数，那这个参数是怎么来的呢？

Spring会根据入参的类型和入参的名字去Spring中找Bean对象（以单例Bean为例，Spring会从单例池那个Map中去找）：

- 1, 先根据入参类型找，如果只找到一个，那就直接用来作为入参
- 2, 如果根据类型找到多个，则再根据入参名字来确定唯一一个
- 3, 最终如果没有找到，则会报错，无法创建当前Bean对象

确定用哪个构造方法，确定入参的Bean对象，这个过程就叫做**推断构造方法**。

### AOP大致流程

AOP就是进行动态代理，在创建一个Bean的过程中，Spring在最后一步会去判断当前正在创建的这个Bean是不是需要进行AOP，如果需要则会进行动态代理。

如何判断当前Bean对象需不需要进行AOP:

- 1,找出所有的切面Bean
- 2,遍历切面中的每个方法，看是否写了@Before、@After等注解
- 3,如果写了，则判断所对应的Pointcut是否和当前Bean对象的类是否匹配
- 4,如果匹配则表示当前Bean对象有匹配的的Pointcut，表示需要进行AOP

利用cglib进行AOP的大致流程：

- 1,生成代理类UserServiceProxy，代理类继承UserService
- 2,代理类中重写了父类的方法，比如UserService中的test()方法
- 3,代理类中还会有一个target属性，该属性的值为被代理对象（也就是通过UserService类推断构造方法实例化出来的对象，进行了依赖注入、初始化等步骤的对象）
- 4,代理类中的test()方法被执行时的逻辑如下：

        a, 执行切面逻辑（@Before）
        b, 调用target.test()

当我们从Spring容器得到UserService的Bean对象时，拿到的就是UserServiceProxy所生成的对象，也就是代理对象。

UserService代理对象.test()--->执行切面逻辑--->target.test()，注意target对象不是代理对象，而是被代理对象。

### Spring事务

当我们在某个方法上加了@Transactional注解后，就表示该方法在调用时会开启Spring事务，而这个方法所在的类所对应的Bean对象会是该类的代理对象。

Spring事务的代理对象执行某个方法时的步骤：

- 1,判断当前执行的方法是否存在@Transactional注解
- 2,如果存在，则利用事务管理器（TransactionMananger）新建一个数据库连接
- 3,修改数据库连接的autocommit为false
- 4,执行target.test()，执行程序员所写的业务逻辑代码，也就是执行sql
- 5,执行完了之后如果没有出现异常，则提交，否则回滚

Spring事务是否会失效的判断标准：**某个加了@Transactional注解的方法被调用时，要判断到底是不是直接<font color="red">被代理对象调用的</font>，如果是则事务会生效，如果不是则失效**。

## 2、手写模拟Spring底层原理

因为是手写模拟Spring代码，所以没有特殊的笔记，请直接看视频，建议同学们抽空自己也写一个Spring出来，加深理解。

git clone地址：https://gitee.com/archguide/zhouyu-spring.git

课程内容：
通过手写模拟，了解Spring的底层源码启动过程
通过手写模拟，了解BeanDefinition、BeanPostProcessor的概念
通过手写模拟，了解Spring解析配置类等底层源码工作流程
通过手写模拟，了解依赖注入，Aware回调等底层源码工作流程
通过手写模拟，了解Spring AOP的底层源码工作流程

## 3、Spring之底层架构核心概念解析

前面两节课，我们大概了解了Spring中的一些概念和底层工作流程，本节课开始将真正讲一些Spring中的概念和工作流程。

本节课的内容，是后续看Spring源码所必备的，防止后续看源码的过程中，遇到不会的概念得单独跳出来学习。

### BeanDefinition

BeanDefinition表示Bean定义，BeanDefinition中存在很多属性用来描述一个Bean的特点。比如：

- class，表示Bean类型
- scope，表示Bean作用域，单例或原型等
- lazyInit：表示Bean是否是懒加载
- initMethodName：表示Bean初始化时要执行的方法
- destroyMethodName：表示Bean销毁时要执行的方法
还有很多...

在Spring中，我们经常会通过以下几种方式来定义Bean：

以下这些，我们可以称之申明式定义Bean。

```java
1, <bean/>
2, @Bean
3, @Component(@Service,@Controller)

```

我们还可以编程式定义Bean，那就是直接通过BeanDefinition，比如：

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

// 生成一个BeanDefinition对象，并设置beanClass为User.class，并注册到ApplicationContext中
AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.genericBeanDefinition().getBeanDefinition();
beanDefinition.setBeanClass(User.class);
context.registerBeanDefinition("user", beanDefinition);

System.out.println(context.getBean("user"));
```

我们还可以通过BeanDefinition设置一个Bean的其他属性

```java
beanDefinition.setScope("prototype"); // 设置作用域
beanDefinition.setInitMethodName("init"); // 设置初始化方法
beanDefinition.setLazyInit(true); // 设置懒加载
```

和申明式事务、编程式事务类似，通过<bean/>，@Bean，@Component等申明式方式所定义的Bean，最终都会被Spring解析为对应的BeanDefinition对象，并放入Spring容器中。

### BeanDefinitionReader

接下来，我们来介绍几种在Spring源码中所提供的BeanDefinition读取器（BeanDefinitionReader），这些BeanDefinitionReader在我们使用Spring时用得少，但在Spring源码中用得多，相当于Spring源码的基础设施。

### AnnotatedBeanDefinitionReader

可以直接把某个类转换为BeanDefinition，并且会解析该类上的注解，比如

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

AnnotatedBeanDefinitionReader annotatedBeanDefinitionReader = new AnnotatedBeanDefinitionReader(context);

// 将User.class解析为BeanDefinition
annotatedBeanDefinitionReader.register(User.class);

System.out.println(context.getBean("user"));
```

注意：它能解析的注解是：@Conditional，**@Scope**、@Lazy、@Primary、@DependsOn、@Role、@Description

### XmlBeanDefinitionReader

可以解析<bean/>标签

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

XmlBeanDefinitionReader xmlBeanDefinitionReader = new XmlBeanDefinitionReader(context);
int i = xmlBeanDefinitionReader.loadBeanDefinitions("spring.xml");

System.out.println(context.getBean("user"));
```

### ClassPathBeanDefinitionScanner

ClassPathBeanDefinitionScanner是扫描器，但是它的作用和BeanDefinitionReader类似，它可以进行扫描，扫描某个包路径，对扫描到的类进行解析，比如，扫描到的类上如果存在@Component注解，那么就会把这个类解析为一个BeanDefinition，比如：

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext();
context.refresh();

ClassPathBeanDefinitionScanner scanner = new ClassPathBeanDefinitionScanner(context);
scanner.scan("com.zhouyu");

System.out.println(context.getBean("userService"));
```

### BeanFactory

BeanFactory表示Bean工厂，所以很明显，BeanFactory会负责创建Bean，并且提供获取Bean的API。

而ApplicationContext是BeanFactory的一种，在Spring源码中，是这么定义的：

```java
public interface ApplicationContext extends EnvironmentCapable, ListableBeanFactory, HierarchicalBeanFactory,
		MessageSource, ApplicationEventPublisher, ResourcePatternResolver {

            ...
}
```

首先，在Java中，接口是可以**多继承**的，我们发现ApplicationContext继承了ListableBeanFactory和HierarchicalBeanFactory，而ListableBeanFactory和HierarchicalBeanFactory都继承至BeanFactory，所以我们可以认为ApplicationContext继承了BeanFactory，相当于苹果继承水果，宝马继承汽车一样，ApplicationContext也是BeanFactory的一种，拥有BeanFactory支持的所有功能，不过ApplicationContext比BeanFactory更加强大，ApplicationContext还基础了其他接口，也就表示ApplicationContext还拥有其他功能，比如MessageSource表示国际化，ApplicationEventPublisher表示事件发布，EnvironmentCapable表示获取环境变量，等等，关于ApplicationContext后面再详细讨论。

在Spring的源码实现中，当我们new一个ApplicationContext时，其底层会new一个BeanFactory出来，当使用ApplicationContext的某些方法时，比如getBean()，底层调用的是BeanFactory的getBean()方法。

在Spring源码中，BeanFactory接口存在一个非常重要的实现类是：**DefaultListableBeanFactory，也是非常核心的**。具体重要性，随着后续课程会感受更深。

所以，我们可以直接来使用**DefaultListableBeanFactory**，而不用使用ApplicationContext的某个实现类，比如：

```java
DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();

AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.genericBeanDefinition().getBeanDefinition();
beanDefinition.setBeanClass(User.class);

beanFactory.registerBeanDefinition("user", beanDefinition);

System.out.println(beanFactory.getBean("user"));
```

**DefaultListableBeanFactory是非常强大的，支持很多功能，可以通过查看DefaultListableBeanFactory的类继承实现结构来看**

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-18-18-47-59.png width='100%'/></div><br/>


**这部分现在看不懂没关系，源码熟悉一点后回来再来看都可以。**

它实现了很多接口，表示，它拥有很多功能：

```java
1,AliasRegistry：支持别名功能，一个名字可以对应多个别名
2,BeanDefinitionRegistry：可以注册、保存、移除、获取某个BeanDefinition
3,BeanFactory：Bean工厂，可以根据某个bean的名字、或类型、或别名获取某个Bean对象
4,SingletonBeanRegistry：可以直接注册、获取某个单例Bean
5,SimpleAliasRegistry：它是一个类，实现了AliasRegistry接口中所定义的功能，支持别名功能
6,ListableBeanFactory：在BeanFactory的基础上，增加了其他功能，可以获取所有BeanDefinition的beanNames，可以根据某个类型获取对应的beanNames，可以根据某个类型获取{类型：对应的Bean}的映射关系
7,HierarchicalBeanFactory：在BeanFactory的基础上，添加了获取父BeanFactory的功能
8,DefaultSingletonBeanRegistry：它是一个类，实现了SingletonBeanRegistry接口，拥有了直接注册、获取某个单例Bean的功能
9,ConfigurableBeanFactory：在HierarchicalBeanFactory和SingletonBeanRegistry的基础上，添加了设置父BeanFactory、类加载器（表示可以指定某个类加载器进行类的加载）、设置Spring EL表达式解析器（表示该BeanFactory可以解析EL表达式）、设置类型转化服务（表示该BeanFactory可以进行类型转化）、可以添加BeanPostProcessor（表示该BeanFactory支持Bean的后置处理器），可以合并BeanDefinition，可以销毁某个Bean等等功能
10,FactoryBeanRegistrySupport：支持了FactoryBean的功能
11,AutowireCapableBeanFactory：是直接继承了BeanFactory，在BeanFactory的基础上，支持在创建Bean的过程中能对Bean进行自动装配
12,AbstractBeanFactory：实现了ConfigurableBeanFactory接口，继承了FactoryBeanRegistrySupport，这个BeanFactory的功能已经很全面了，但是不能自动装配和获取beanNames
13,ConfigurableListableBeanFactory：继承了ListableBeanFactory、AutowireCapableBeanFactory、ConfigurableBeanFactory
14,AbstractAutowireCapableBeanFactory：继承了AbstractBeanFactory，实现了AutowireCapableBeanFactory，拥有了自动装配的功能
15,DefaultListableBeanFactory：继承了AbstractAutowireCapableBeanFactory，实现了ConfigurableListableBeanFactory接口和BeanDefinitionRegistry接口，所以DefaultListableBeanFactory的功能很强大

```

### ApplicationContext

上面有分析到，ApplicationContext是个接口，实际上也是一个BeanFactory，不过比BeanFactory更加强大，比如：

```java
1,HierarchicalBeanFactory：拥有获取父BeanFactory的功能
2,ListableBeanFactory：拥有获取beanNames的功能
3,ResourcePatternResolver：资源加载器，可以一次性获取多个资源（文件资源等等）
4,EnvironmentCapable：可以获取运行时环境（没有设置运行时环境功能）
5,ApplicationEventPublisher：拥有广播事件的功能（没有添加事件监听器的功能）
6,MessageSource：拥有国际化功能
```

具体的功能演示，后面会有。

我们先来看ApplicationContext两个比较重要的实现类：

+ AnnotationConfigApplicationContext
+ ClassPathXmlApplicationContext


### AnnotationConfigApplicationContext

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-18-18-54-34.png width='100%'/></div><br/>

**这部分现在看不懂没关系，源码熟悉一点后回来再来看都可以。**

```java
ConfigurableApplicationContext：继承了ApplicationContext接口，增加了，添加事件监听器、添加BeanFactoryPostProcessor、设置Environment，获取ConfigurableListableBeanFactory等功能
AbstractApplicationContext：实现了ConfigurableApplicationContext接口
GenericApplicationContext：继承了AbstractApplicationContext，实现了BeanDefinitionRegistry接口，拥有了所有ApplicationContext的功能，并且可以注册BeanDefinition，注意这个类中有一个属性(DefaultListableBeanFactory beanFactory)
AnnotationConfigRegistry：可以单独注册某个为类为BeanDefinition（可以处理该类上的@Configuration注解，已经可以处理@Bean注解），同时可以扫描
AnnotationConfigApplicationContext：继承了GenericApplicationContext，实现了AnnotationConfigRegistry接口，拥有了以上所有的功能

```

### ClassPathXmlApplicationContext

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-18-18-57-02.png width='100%'/></div><br/>

它也是继承了AbstractApplicationContext，但是相对于AnnotationConfigApplicationContext而言，功能没有AnnotationConfigApplicationContext强大，比如不能注册BeanDefinition


### 国际化

先定义一个MessageSource:

```java
@Bean
public MessageSource messageSource() {
	ResourceBundleMessageSource messageSource = new ResourceBundleMessageSource();
	messageSource.setBasename("messages");
	return messageSource;
}
```

有了这个Bean，你可以在你任意想要进行国际化的地方使用该MessageSource。
同时，因为ApplicationContext也拥有国家化的功能，所以可以直接这么用：

```java
context.getMessage("test", null, new Locale("en_CN"))
```

### 资源加载

ApplicationContext还拥有资源加载的功能，比如，可以直接利用ApplicationContext获取某个文件的内容：

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

Resource resource = context.getResource("file://D:\\IdeaProjects\\spring-framework-5.3.10\\tuling\\src\\main\\java\\com\\zhouyu\\service\\UserService.java");
System.out.println(resource.contentLength());
```

你可以想想，如果你不使用ApplicationContext，而是自己来实现这个功能，就比较费时间了。

还比如你可以：

```jajva
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

Resource resource = context.getResource("file://D:\\IdeaProjects\\spring-framework-5.3.10\\tuling\\src\\main\\java\\com\\zhouyu\\service\\UserService.java");
System.out.println(resource.contentLength());
System.out.println(resource.getFilename());

Resource resource1 = context.getResource("https://www.baidu.com");
System.out.println(resource1.contentLength());
System.out.println(resource1.getURL());

Resource resource2 = context.getResource("classpath:spring.xml");
System.out.println(resource2.contentLength());
System.out.println(resource2.getURL());
```

还可以一次性获取多个：

```java
Resource[] resources = context.getResources("classpath:com/zhouyu/*.class");
for (Resource resource : resources) {
	System.out.println(resource.contentLength());
	System.out.println(resource.getFilename());
}
```

### 获取运行时环境

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

Map<String, Object> systemEnvironment = context.getEnvironment().getSystemEnvironment();
System.out.println(systemEnvironment);

System.out.println("=======");

Map<String, Object> systemProperties = context.getEnvironment().getSystemProperties();
System.out.println(systemProperties);

System.out.println("=======");

MutablePropertySources propertySources = context.getEnvironment().getPropertySources();
System.out.println(propertySources);

System.out.println("=======");

System.out.println(context.getEnvironment().getProperty("NO_PROXY"));
System.out.println(context.getEnvironment().getProperty("sun.jnu.encoding"));
System.out.println(context.getEnvironment().getProperty("zhouyu"));
```

注意，可以利用

`@PropertySource("classpath:spring.properties")`

来使得某个properties文件中的参数添加到运行时环境中

### 事件发布

先定义一个事件监听器

```java
@Bean
public ApplicationListener applicationListener() {
	return new ApplicationListener() {
		@Override
		public void onApplicationEvent(ApplicationEvent event) {
			System.out.println("接收到了一个事件");
		}
	};
}
```

然后发布一个事件：

`context.publishEvent("kkk");`

### 类型转化

在Spring源码中，有可能需要把String转成其他类型，所以在Spring源码中提供了一些技术来更方便的做对象的类型转化，关于类型转化的应用场景， 后续看源码的过程中会遇到很多。

**PropertyEditor**

这其实是JDK中提供的类型转化工具类

```java
public class StringToUserPropertyEditor extends PropertyEditorSupport implements PropertyEditor {

	@Override
	public void setAsText(String text) throws IllegalArgumentException {
		User user = new User();
		user.setName(text);
		this.setValue(user);
	}
}
```

```java
StringToUserPropertyEditor propertyEditor = new StringToUserPropertyEditor();
propertyEditor.setAsText("1");
User value = (User) propertyEditor.getValue();
System.out.println(value);
```

如何向Spring中注册PropertyEditor：

```java
@Bean
public CustomEditorConfigurer customEditorConfigurer() {
	CustomEditorConfigurer customEditorConfigurer = new CustomEditorConfigurer();
	Map<Class<?>, Class<? extends PropertyEditor>> propertyEditorMap = new HashMap<>();
    
    // 表示StringToUserPropertyEditor可以将String转化成User类型，在Spring源码中，如果发现当前对象是String，而需要的类型是User，就会使用该PropertyEditor来做类型转化
	propertyEditorMap.put(User.class, StringToUserPropertyEditor.class);
	customEditorConfigurer.setCustomEditors(propertyEditorMap);
	return customEditorConfigurer;
}
```

假设现在有如下Bean:

```java
@Component
public class UserService {

	@Value("xxx")
	private User user;

	public void test() {
		System.out.println(user);
	}

}
```

那么test属性就能正常的完成属性赋值

**ConversionService**

Spring中提供的类型转化服务，它比PropertyEditor更强大

```java
public class StringToUserConverter implements ConditionalGenericConverter {

	@Override
	public boolean matches(TypeDescriptor sourceType, TypeDescriptor targetType) {
		return sourceType.getType().equals(String.class) && targetType.getType().equals(User.class);
	}

	@Override
	public Set<ConvertiblePair> getConvertibleTypes() {
		return Collections.singleton(new ConvertiblePair(String.class, User.class));
	}

	@Override
	public Object convert(Object source, TypeDescriptor sourceType, TypeDescriptor targetType) {
		User user = new User();
		user.setName((String)source);
		return user;
	}
}
```
```java
DefaultConversionService conversionService = new DefaultConversionService();
conversionService.addConverter(new StringToUserConverter());
User value = conversionService.convert("1", User.class);
System.out.println(value);
```

如何向Spring中注册ConversionService：

```java
@Bean
public ConversionServiceFactoryBean conversionService() {
	ConversionServiceFactoryBean conversionServiceFactoryBean = new ConversionServiceFactoryBean();
	conversionServiceFactoryBean.setConverters(Collections.singleton(new StringToUserConverter()));

	return conversionServiceFactoryBean;
}
```

**TypeConverter**
整合了PropertyEditor和ConversionService的功能，是Spring内部用的

```java
SimpleTypeConverter typeConverter = new SimpleTypeConverter();
typeConverter.registerCustomEditor(User.class, new StringToUserPropertyEditor());
//typeConverter.setConversionService(conversionService);
User value = typeConverter.convertIfNecessary("1", User.class);
System.out.println(value);
```

### OrderComparator

OrderComparator是Spring所提供的一种比较器，可以用来根据@Order注解或实现Ordered接口来执行值进行笔记，从而可以进行排序。

比如：

```java
public class A implements Ordered {

	@Override
	public int getOrder() {
		return 3;
	}

	@Override
	public String toString() {
		return this.getClass().getSimpleName();
	}
}
```

```java
public class B implements Ordered {

	@Override
	public int getOrder() {
		return 2;
	}

	@Override
	public String toString() {
		return this.getClass().getSimpleName();
	}
}
```

```java
public class Main {

	public static void main(String[] args) {
		A a = new A(); // order=3
		B b = new B(); // order=2

		OrderComparator comparator = new OrderComparator();
		System.out.println(comparator.compare(a, b));  // 1

		List list = new ArrayList<>();
		list.add(a);
		list.add(b);

		// 按order值升序排序
		list.sort(comparator);

		System.out.println(list);  // B，A
	}
}
```

另外，Spring中还提供了一个OrderComparator的子类：**AnnotationAwareOrderComparator**，它支持用@Order来指定order值。比如：

```java
@Order(3)
public class A {

	@Override
	public String toString() {
		return this.getClass().getSimpleName();
	}

}
```

```java
@Order(2)
public class B {

	@Override
	public String toString() {
		return this.getClass().getSimpleName();
	}

}
```

```java
public class Main {

	public static void main(String[] args) {
		A a = new A(); // order=3
		B b = new B(); // order=2

		AnnotationAwareOrderComparator comparator = new AnnotationAwareOrderComparator();
		System.out.println(comparator.compare(a, b)); // 1

		List list = new ArrayList<>();
		list.add(a);
		list.add(b);

		// 按order值升序排序
		list.sort(comparator);

		System.out.println(list); // B，A
	}
}
```

### BeanPostProcessor

BeanPostProcess表示Bena的后置处理器，我们可以定义一个或多个BeanPostProcessor，比如通过以下代码定义一个BeanPostProcessor：

```java
@Component
public class ZhouyuBeanPostProcessor implements BeanPostProcessor {

	@Override
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		if ("userService".equals(beanName)) {
			System.out.println("初始化前");
		}

		return bean;
	}

	@Override
	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		if ("userService".equals(beanName)) {
			System.out.println("初始化后");
		}

		return bean;
	}
}
```

<font color ="red">一个BeanPostProcessor可以在**任意一个Bean的初始化之前以及初始化之后**去额外的做一些用户自定义的逻辑，当然，我们可以通过判断beanName来进行针对性处理（针对某个Bean，或某部分Bean）。</font>

我们可以通过定义BeanPostProcessor来干涉Spring创建Bean的过程。

### BeanFactoryPostProcessor

BeanFactoryPostProcessor表示Bean工厂的后置处理器，其实和BeanPostProcessor类似，BeanPostProcessor是干涉Bean的创建过程，BeanFactoryPostProcessor是干涉BeanFactory的创建过程。比如，我们可以这样定义一个BeanFactoryPostProcessor：

```java
@Component
public class ZhouyuBeanFactoryPostProcessor implements BeanFactoryPostProcessor {

	@Override
	public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
		System.out.println("加工beanFactory");
	}
}
```

我们可以在postProcessBeanFactory()方法中对BeanFactory进行加工。

### FactoryBean

上面提到，我们可以通过BeanPostPorcessor来干涉Spring创建Bean的过程，但是如果我们想一个Bean完完全全由我们来创造，也是可以的，比如通过FactoryBean：

```java
@Component
public class ZhouyuFactoryBean implements FactoryBean {

	@Override
	public Object getObject() throws Exception {
		UserService userService = new UserService();

		return userService;
	}

	@Override
	public Class<?> getObjectType() {
		return UserService.class;
	}
}
```

通过上面这段代码，我们自己创造了一个UserService对象，并且它将成为Bean。但是通过这种方式创造出来的UserService的Bean，**只会经过初始化后**，其他Spring的生命周期步骤是不会经过的，比如依赖注入。

有同学可能会想到，通过@Bean也可以自己生成一个对象作为Bean，那么和FactoryBean的区别是什么呢？其实在很多场景下他俩是可以替换的，但是站在原理层面来说的，区别很明显，@Bean定义的Bean是会经过完整的Bean生命周期的。

### ExcludeFilter和IncludeFilter

这两个Filter是Spring扫描过程中用来过滤的。ExcludeFilter表示**排除过滤器**，IncludeFilter表示**包含过滤器**。

比如以下配置，表示扫描com.zhouyu这个包下面的所有类，但是排除UserService类，也就是就算它上面有@Component注解也不会成为Bean。

```java
@ComponentScan(value = "com.zhouyu",
		excludeFilters = {@ComponentScan.Filter(
            	type = FilterType.ASSIGNABLE_TYPE, 
            	classes = UserService.class)}.)
public class AppConfig {
}
```

再比如以下配置，就算UserService类上没有@Component注解，它也会被扫描成为一个Bean。

```java
@ComponentScan(value = "com.zhouyu",
		includeFilters = {@ComponentScan.Filter(
            	type = FilterType.ASSIGNABLE_TYPE, 
            	classes = UserService.class)})
public class AppConfig {
}
```

FilterType分为：

    ANNOTATION：表示是否包含某个注解
    ASSIGNABLE_TYPE：表示是否是某个类
    ASPECTJ：表示否是符合某个Aspectj表达式
    REGEX：表示是否符合某个正则表达式
    CUSTOM：自定义

在Spring的扫描逻辑中，默认会添加一个AnnotationTypeFilter给includeFilters，表示默认情况下Spring扫描过程中会认为类上有@Component注解的就是Bean。

### MetadataReader、ClassMetadata、AnnotationMetadata

在Spring中需要去解析类的信息，比如类名、类中的方法、类上的注解，这些都可以称之为类的元数据，所以Spring中对类的元数据做了抽象，并提供了一些工具类。

MetadataReader表示类的元数据读取器，默认实现类为**SimpleMetadataReader**。比如：

```java
public class Test {

	public static void main(String[] args) throws IOException {
		SimpleMetadataReaderFactory simpleMetadataReaderFactory = new SimpleMetadataReaderFactory();
		
        // 构造一个MetadataReader
        MetadataReader metadataReader = simpleMetadataReaderFactory.getMetadataReader("com.zhouyu.service.UserService");
		
        // 得到一个ClassMetadata，并获取了类名
        ClassMetadata classMetadata = metadataReader.getClassMetadata();
	
        System.out.println(classMetadata.getClassName());
        
        // 获取一个AnnotationMetadata，并获取类上的注解信息
        AnnotationMetadata annotationMetadata = metadataReader.getAnnotationMetadata();
		for (String annotationType : annotationMetadata.getAnnotationTypes()) {
			System.out.println(annotationType);
		}

	}
}
```

需要注意的是，SimpleMetadataReader去解析类时，使用的**ASM技术**。

为什么要使用ASM技术，Spring启动的时候需要去扫描，如果指定的包路径比较宽泛，那么扫描的类是非常多的，那如果在Spring启动时就把这些类全部加载进JVM了，这样不太好，所以使用了ASM技术。

## 4、Spring之Bean生命周期源码解析（上）

Spring最重要的功能就是帮助程序员创建对象（也就是IOC），而启动Spring就是为创建Bean对象做准备，所以我们先明白Spring到底是怎么去创建Bean的，也就是先弄明白Bean的生命周期。

Bean的生命周期就是指：**在Spring中，一个Bean是如何生成的，如何销毁的**

Bean生命周期流程图：https://www.processon.com/view/link/5f8588c87d9c0806f27358c1

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-22-11-03-03.png width='100%'/></div><br/>

附带资料JFR介绍：https://zhuanlan.zhihu.com/p/122247741

### Bean的生成过程

### 1. 生成BeanDefinition

Spring启动的时候会进行扫描，会先调用`org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider#scanCandidateComponents(String basePackage)`
扫描某个包路径，并得到BeanDefinition的Set集合。

**关于Spring启动流程，后续会单独的课详细讲，这里先讲一下Spring扫描的底层实现：**

Spring扫描底层流程：https://www.processon.com/view/link/61370ee60e3e7412ecd95d43

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-22-11-42-23.png width='100%'/></div><br/>

+ 首先，通过ResourcePatternResolver获得指定包路径下的所有.class文件（Spring源码中将此文件包装成了Resource对象）
+ 遍历每个Resource对象
+ 利用MetadataReaderFactory解析Resource对象得到MetadataReader（在Spring源码中MetadataReaderFactory具体的实现类为CachingMetadataReaderFactory，MetadataReader的具体实现类为SimpleMetadataReader）
+ 利用MetadataReader进行excludeFilters和includeFilters，以及条件注解@Conditional的筛选（条件注解并不能理解：某个类上是否存在@Conditional注解，如果存在则调用注解中所指定的类的match方法进行匹配，匹配成功则通过筛选，匹配失败则pass掉。）
+ 筛选通过后，基于metadataReader生成ScannedGenericBeanDefinition
+ 再基于metadataReader判断是不是对应的类是不是接口或抽象类
+ 如果筛选通过，那么就表示扫描到了一个Bean，将ScannedGenericBeanDefinition加入结果集

MetadataReader表示类的元数据读取器，主要包含了一个AnnotationMetadata，功能有

+ 获取类的名字
+ 获取父类的名字
+ 获取所实现的所有接口名
+ 获取所有内部类的名字
+ 判断是不是抽象类
+ 判断是不是接口
+ 判断是不是一个注解
+ 获取拥有某个注解的方法集合
+ 获取类上添加的所有注解信息
+ 获取类上添加的所有注解类型集合


值得注意的是，CachingMetadataReaderFactory解析某个.class文件得到MetadataReader对象是利用的**ASM技术**，并没有加载这个类到JVM。并且，最终得到的ScannedGenericBeanDefinition对象，**beanClass属性存储的是当前类的名字，而不是class对象**。（beanClass属性的类型是Object，它即可以存储类的名字，也可以存储class对象）

最后，上面是说的通过扫描得到BeanDefinition对象，我们还可以通过直接定义BeanDefinition，或解析spring.xml文件的\<bean/>，或者@Bean注解得到BeanDefinition对象。（后续课程会分析@Bean注解是怎么生成BeanDefinition的）。

### 2. 合并BeanDefinition

通过扫描得到所有BeanDefinition之后，就可以根据BeanDefinition创建Bean对象了，但是在Spring中支持父子BeanDefinition，和Java父子类类似，但是完全不是一回事。

父子BeanDefinition实际用的比较少，使用是这样的，比如：

```java
<bean id="parent" class="com.zhouyu.service.Parent" scope="prototype"/>
<bean id="child" class="com.zhouyu.service.Child"/>
```

这么定义的情况下，child是单例Bean。

```java
<bean id="parent" class="com.zhouyu.service.Parent" scope="prototype"/>
<bean id="child" class="com.zhouyu.service.Child" parent="parent"/>
```

但是这么定义的情况下，child就是原型Bean了。

因为child的父BeanDefinition是parent，所以会继承parent上所定义的scope属性。

而在根据child来生成Bean对象之前，需要进行BeanDefinition的合并，得到完整的child的BeanDefinition。

### 3. 加载类

BeanDefinition合并之后，就可以去创建Bean对象了，而创建Bean就必须实例化对象，而实例化就必须先加载当前BeanDefinition所对应的class，在AbstractAutowireCapableBeanFactory类的createBean()方法中，一开始就会调用：

`Class<?> resolvedClass = resolveBeanClass(mbd, beanName);`

这行代码就是去加载类，该方法是这么实现的：

```java
if (mbd.hasBeanClass()) {
	return mbd.getBeanClass();
}
if (System.getSecurityManager() != null) {
	return AccessController.doPrivileged((PrivilegedExceptionAction<Class<?>>) () ->
		doResolveBeanClass(mbd, typesToMatch), getAccessControlContext());
	}
else {
	return doResolveBeanClass(mbd, typesToMatch);
}
```
```java
public boolean hasBeanClass() {
	return (this.beanClass instanceof Class);
}
```

如果beanClass属性的类型是Class，那么就直接返回，如果不是，则会根据类名进行加载（doResolveBeanClass方法所做的事情）

会利用BeanFactory所设置的类加载器来加载类，如果没有设置，则默认使用ClassUtils.getDefaultClassLoader()所返回的类加载器来加载。

ClassUtils.getDefaultClassLoader()

+ 优先返回当前线程中的ClassLoader
+ 线程中类加载器为null的情况下，返回ClassUtils类的类加载器
+ 如果ClassUtils类的类加载器为空，那么则表示是Bootstrap类加载器加载的ClassUtils类，那么则返回系统类加载器

### 4. 实例化前

当前BeanDefinition对应的类成功加载后，就可以实例化对象了，但是...

在Spring中，实例化对象之前，Spring提供了一个扩展点，允许用户来控制是否在某个或某些Bean实例化之前做一些启动动作。这个扩展点叫**InstantiationAwareBeanPostProcessor.postProcessBeforeInstantiation()**。比如：

```java
@Component
public class ZhouyuBeanPostProcessor implements InstantiationAwareBeanPostProcessor {

	@Override
	public Object postProcessBeforeInstantiation(Class<?> beanClass, String beanName) throws BeansException {
		if ("userService".equals(beanName)) {
			System.out.println("实例化前");
		}
		return null;
	}
}
```

如上代码会导致，在userService这个Bean实例化前，会进行打印。

值得注意的是，postProcessBeforeInstantiation()是有返回值的，如果这么实现：

```java
@Component
public class ZhouyuBeanPostProcessor implements InstantiationAwareBeanPostProcessor {

	@Override
	public Object postProcessBeforeInstantiation(Class<?> beanClass, String beanName) throws BeansException {
		if ("userService".equals(beanName)) {
			System.out.println("实例化前");
			return new UserService();
		}
		return null;
	}
}
```

userService这个Bean，在实例化前会直接返回一个由我们所定义的UserService对象。如果是这样，表示不需要Spring来实例化了，并且后续的Spring依赖注入也不会进行了，会跳过一些步骤，直接执行初始化后这一步。

### 5.实例化

在这个步骤中就会根据BeanDefinition去创建一个对象了。

#### 5.1 Supplier创建对象

首先判断BeanDefinition中是否设置了Supplier，如果设置了则调用Supplier的get()得到对象。

得直接使用BeanDefinition对象来设置Supplier，比如：

```java
AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.genericBeanDefinition().getBeanDefinition();
beanDefinition.setInstanceSupplier(new Supplier<Object>() {
	@Override
	public Object get() {
		return new UserService();
	}
});
context.registerBeanDefinition("userService", beanDefinition);
```

#### 5.2 工厂方法创建对象

如果没有设置Supplier，则检查BeanDefinition中是否设置了factoryMethod，也就是工厂方法，有两种方式可以设置factoryMethod，比如：

方式一：

```java
<bean id="userService" class="com.zhouyu.service.UserService" factory-method="createUserService" />
```

对应的UserService类为：

```java
public class UserService {

	public static UserService createUserService() {
		System.out.println("执行createUserService()");
		UserService userService = new UserService();
		return userService;
	}

	public void test() {
		System.out.println("test");
	}

}

```

方式二：

```java
<bean id="commonService" class="com.zhouyu.service.CommonService"/>
<bean id="userService1" factory-bean="commonService" factory-method="createUserService" />

```

对应的CommonService的类为：

```java
public class CommonService {

	public UserService createUserService() {
		return new UserService();
	}
}
```

Spring发现当前BeanDefinition方法设置了工厂方法后，就会区分这两种方式，然后调用工厂方法得到对象。

值得注意的是，我们通过@Bean所定义的BeanDefinition，是存在factoryMethod和factoryBean的，也就是和上面的方式二非常类似，@Bean所注解的方法就是factoryMethod，AppConfig对象就是factoryBean。如果@Bean所所注解的方法是static的，那么对应的就是方式一。

#### 5.3 推断构造方法

第一节已经讲过一遍大概原理了，后面有一节课单独分析源码实现。推断完构造方法后，就会使用构造方法来进行实例化了。

额外的，在推断构造方法逻辑中除开会去选择构造方法以及查找入参对象意外，会还判断是否在对应的类中是否存在**使用@Lookup注解**了方法。如果存在则把该方法封装为LookupOverride对象并添加到BeanDefinition中。

在实例化时，如果判断出来当前BeanDefinition中没有LookupOverride，那就直接用构造方法反射得到一个实例对象。如果存在LookupOverride对象，也就是类中存在@Lookup注解了的方法，那就会生成一个代理对象。

@Lookup注解就是**方法注入**，使用demo如下：

```java
@Component
public class UserService {

	private OrderService orderService;

	public void test() {
		OrderService orderService = createOrderService();
		System.out.println(orderService);
	}

	@Lookup("orderService")
	public OrderService createOrderService() {
		return null;
	}

}

```

### 6. BeanDefinition的后置处理

Bean对象实例化出来之后，接下来就应该给对象的属性赋值了。在真正给属性赋值之前，Spring又提供了一个扩展点**MergedBeanDefinitionPostProcessor.postProcessMergedBeanDefinition()**，可以对此时的BeanDefinition进行加工，比如：

```java
@Component
public class ZhouyuMergedBeanDefinitionPostProcessor implements MergedBeanDefinitionPostProcessor {

	@Override
	public void postProcessMergedBeanDefinition(RootBeanDefinition beanDefinition, Class<?> beanType, String beanName) {
		if ("userService".equals(beanName)) {
			beanDefinition.getPropertyValues().add("orderService", new OrderService());
		}
	}
}
```

在Spring源码中，AutowiredAnnotationBeanPostProcessor就是一个MergedBeanDefinitionPostProcessor，它的postProcessMergedBeanDefinition()中会去查找注入点，并缓存在AutowiredAnnotationBeanPostProcessor对象的一个Map中（injectionMetadataCache）。

### 7. 实例化后

在处理完BeanDefinition后，Spring又设计了一个扩展点：**InstantiationAwareBeanPostProcessor.postProcessAfterInstantiation()**
比如：

```java
@Component
public class ZhouyuInstantiationAwareBeanPostProcessor implements InstantiationAwareBeanPostProcessor {

	@Override
	public boolean postProcessAfterInstantiation(Object bean, String beanName) throws BeansException {

		if ("userService".equals(beanName)) {
			UserService userService = (UserService) bean;
			userService.test();
		}

		return true;
	}
}
```

上述代码就是对userService所实例化出来的对象进行处理。

这个扩展点，在Spring源码中基本没有怎么使用。

### 8. 自动注入

这里的自动注入指的是Spring的自动注入，后续依赖注入课程中单独讲

### 9. 处理属性
这个步骤中，就会处理@Autowired、@Resource、@Value等注解，也是通过**InstantiationAwareBeanPostProcessor.postProcessProperties()**扩展点来实现的，比如我们甚至可以实现一个自己的自动注入功能，比如：

```java
@Component
public class ZhouyuInstantiationAwareBeanPostProcessor implements InstantiationAwareBeanPostProcessor {

	@Override
	public PropertyValues postProcessProperties(PropertyValues pvs, Object bean, String beanName) throws BeansException {
		if ("userService".equals(beanName)) {
			for (Field field : bean.getClass().getFields()) {
				if (field.isAnnotationPresent(ZhouyuInject.class)) {
					field.setAccessible(true);
					try {
						field.set(bean, "123");
					} catch (IllegalAccessException e) {
						e.printStackTrace();
					}
				}
			}
		}

		return pvs;
	}
}

```

关于@Autowired、@Resource、@Value的底层源码，会在后续的依赖注入课程中详解。

### 10. 执行Aware

完成了属性赋值之后，Spring会执行一些回调，包括：

    BeanNameAware：回传beanName给bean对象。
    BeanClassLoaderAware：回传classLoader给bean对象。
    BeanFactoryAware：回传beanFactory给对象。

### 11. 初始化前

初始化前，也是Spring提供的一个扩展点：**BeanPostProcessor.postProcessBeforeInitialization()**，比如:

```java
@Component
public class ZhouyuBeanPostProcessor implements BeanPostProcessor {

	@Override
	public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
		if ("userService".equals(beanName)) {
			System.out.println("初始化前");
		}

		return bean;
	}
}
```

利用初始化前，可以对进行了依赖注入的Bean进行处理。

在Spring源码中：
1. InitDestroyAnnotationBeanPostProcessor会在初始化前这个步骤中执行@PostConstruct的方法，
2. ApplicationContextAwareProcessor会在初始化前这个步骤中进行其他Aware的回调：

```java
    EnvironmentAware：回传环境变量
    EmbeddedValueResolverAware：回传占位符解析器
    ResourceLoaderAware：回传资源加载器
    ApplicationEventPublisherAware：回传事件发布器
    MessageSourceAware：回传国际化资源
    ApplicationStartupAware：回传应用其他监听对象，可忽略
    ApplicationContextAware：回传Spring容器ApplicationContext
```

### 12. 初始化

1. 查看当前Bean对象是否实现了InitializingBean接口，如果实现了就调用其afterPropertiesSet()方法
2. 执行BeanDefinition中指定的初始化方法

### 13. 初始化后

这是Bean创建生命周期中的最后一个步骤，也是Spring提供的一个扩展点：**BeanPostProcessor.postProcessAfterInitialization()**，比如：

```java
@Component
public class ZhouyuBeanPostProcessor implements BeanPostProcessor {

	@Override
	public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
		if ("userService".equals(beanName)) {
			System.out.println("初始化后");
		}

		return bean;
	}
}
```

可以在这个步骤中，对Bean最终进行处理，Spring中的**AOP就是基于初始化后实现的，初始化后返回的对象才是最终的Bean对象**。

### 总结BeanPostProcessor

1. InstantiationAwareBeanPostProcessor.postProcessBeforeInstantiation()
2. 实例化
3. MergedBeanDefinitionPostProcessor.postProcessMergedBeanDefinition()
4. InstantiationAwareBeanPostProcessor.postProcessAfterInstantiation()
5. 自动注入
6. InstantiationAwareBeanPostProcessor.postProcessProperties()
7. Aware对象
8. BeanPostProcessor.postProcessBeforeInitialization()
9. 初始化
10. BeanPostProcessor.postProcessAfterInitialization()

## 5、Spring之Bean生命周期源码解析（下）

### Bean的销毁过程

Bean销毁是发生在Spring容器关闭过程中的。

在Spring容器关闭时，比如：

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
UserService userService = (UserService) context.getBean("userService");
userService.test();

// 容器关闭
context.close();
```

在Bean创建过程中，在最后（初始化之后），有一个步骤会去判断当前创建的Bean是不是DisposableBean：
1. 当前Bean是否实现了DisposableBean接口
2. 或者，当前Bean是否实现了AutoCloseable接口
3. BeanDefinition中是否指定了destroyMethod
4. 调用DestructionAwareBeanPostProcessor.requiresDestruction(bean)进行判断
    a. ApplicationListenerDetector中直接使得ApplicationListener是DisposableBean
    b. InitDestroyAnnotationBeanPostProcessor中使得拥有@PreDestroy注解了的方法就是DisposableBean
5. 把符合上述任意一个条件的Bean适配成DisposableBeanAdapter对象，并存入disposableBeans中（一个LinkedHashMap）

在Spring容器关闭过程时：

1. 首先发布ContextClosedEvent事件
2. 调用lifecycleProcessor的onCloese()方法
3. 销毁单例Bean
    a. 遍历disposableBeans

        i. 把每个disposableBean从单例池中移除
        ii. 调用disposableBean的destroy()
        iii. 如果这个disposableBean还被其他Bean依赖了，那么也得销毁其他Bean
        iv. 如果这个disposableBean还包含了inner beans，将这些Bean从单例池中移除掉 (inner bean参考 https://docs.spring.io/spring-framework/docs/current/spring-framework-reference/core.html#beans-inner-beans)

    b. 清空manualSingletonNames，是一个Set，存的是用户手动注册的单例Bean的beanName
    c. 清空allBeanNamesByType，是一个Map，key是bean类型，value是该类型所有的beanName数组
    d. 清空singletonBeanNamesByType，和allBeanNamesByType类似，只不过只存了单例Bean

这里涉及到一个设计模式：**适配器模式**

在销毁时，Spring会找出实现了DisposableBean接口的Bean。

但是我们在定义一个Bean时，如果这个Bean实现了DisposableBean接口，或者实现了AutoCloseable接口，或者在BeanDefinition中指定了destroyMethodName，那么这个Bean都属于“DisposableBean”，这些Bean在容器关闭时都要调用相应的销毁方法。

所以，这里就需要进行适配，将实现了DisposableBean接口、或者AutoCloseable接口等适配成实现了DisposableBean接口，所以就用到了DisposableBeanAdapter。

会把实现了AutoCloseable接口的类封装成DisposableBeanAdapter，而DisposableBeanAdapter实现了DisposableBean接口。

## 6、Spring之依赖注入源码解析（上）

依赖注入底层原理流程图：
https://www.processon.com/view/link/5f899fa5f346fb06e1d8f570

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-22-15-06-10.png width='100%'/></div><br/>

### Spring中到底有几种依赖注入的方式？

首先分两种：

1. 手动注入
2. 自动注入

#### 手动注入

在XML中定义Bean时，就是手动注入，因为是**程序员手动给某个属性指定了值**。

```java
<bean name="userService" class="com.luban.service.UserService">
	<property name="orderService" ref="orderService"/>
</bean>
```

上面这种底层是通过set方法进行注入。

```java
<bean name="userService" class="com.luban.service.UserService">
	<constructor-arg index="0" ref="orderService"/>
</bean>
```

上面这种底层是通过构造方法进行注入。

所以手动注入的底层也就是分为两种：
1. set方法注入
2. 构造方法注入

#### 自动注入

自动注入又分为两种：
1. XML的autowire自动注入
2. @Autowired注解的自动注入

##### XML的autowire自动注入

在XML中，我们可以在定义一个Bean时去指定这个Bean的自动注入模式：
1. byType
2. byName
3. constructor
4. default
5. no

比如：

```java
<bean id="userService" class="com.luban.service.UserService" autowire="byType"/>
```

这么写，表示Spring会自动的给userService中所有的属性自动赋值（不需要这个属性上有@Autowired注解，但需要这个**属性有对应的set方法**）。

在创建Bean的过程中，在填充属性时，Spring会去解析当前类，把**当前类的所有方法**都解析出来，Spring会去解析每个方法得到对应的PropertyDescriptor对象，PropertyDescriptor中有几个属性：

**1. name：这个name并不是方法的名字，而是拿方法名字进过处理后的名字**

    a. 如果方法名字以“get”开头，比如“getXXX”,那么name=XXX
    b. 如果方法名字以“is”开头，比如“isXXX”,那么name=XXX
    c. 如果方法名字以“set”开头，比如“setXXX”,那么name=XXX

**2. readMethodRef：表示get方法的Method对象的引用
3. readMethodName：表示get方法的名字
4. writeMethodRef：表示set方法的Method对象的引用
5. writeMethodName：表示set方法的名字
6. propertyTypeRef：如果有get方法那么对应的就是返回值的类型，如果是set方法那么对应的就是set方法中唯一参数的类型**

**get方法的定义是**： 方法参数个数为0个，并且 （方法名字以"get"开头 或者 方法名字以"is"开头并且方法的返回类型为boolean）

**set方法的定义是**：方法参数个数为1个，并且 （方法名字以"set"开头并且方法返回类型为void）

所以，Spring在通过byName的自动填充属性时流程是：
1. 找到所有set方法所对应的XXX部分的名字
2. 根据XXX部分的名字去获取bean

Spring在通过byType的自动填充属性时流程是：
1. 获取到set方法中的唯一参数的参数类型，并且根据该类型去容器中获取bean
2. <font color="red"> 如果找到多个，会报错。</font>

以上，分析了autowire的byType和byName情况，那么接下来分析constructor，constructor表示通过构造方法注入，其实这种情况就比较简单了，没有byType和byName那么复杂。

如果是constructor，那么就可以不写set方法了，当某个bean是通过构造方法来注入时，spring利用构造方法的参数信息从Spring容器中去找bean，找到bean之后作为参数传给构造方法，从而实例化得到一个bean对象，并完成属性赋值（属性赋值的代码得程序员来写）。

我们这里先不考虑一个类有多个构造方法的情况，**后面单独讲推断构造方法**。我们这里只考虑只有一个有参构造方法。

其实构造方法注入相当于byType+byName，普通的byType是根据set方法中的参数类型去找bean，找到多个会报错，而constructor就是通过构造方法中的参数类型去找bean，如果找到多个会根据参数名确定。

另外两个：
1. no，表示关闭autowire
2. default，表示默认值，我们一直演示的某个bean的autowire，而也可以直接在\<beans>标签中设置autowire，如果设置了，那么\<bean>标签中设置的autowire如果为default，那么则会用\<beans>标签中设置的autowire。

可以发现XML中的自动注入是挺强大的，那么问题来了，**为什么我们平时都是用的@Autowired注解呢？而没有用上文说的这种自动注入方式呢？**

@Autowired注解相当于XML中的autowire属性的**注解方式的替代**。这是在官网上有提到的。

```java
Essentially, the @Autowired annotation provides the same capabilities as described in Autowiring Collaborators but with more fine-grained control and wider applicability
```

翻译一下：
从本质上讲，@Autowired注解提供了与autowire相同的功能，但是拥有更细粒度的控制和更广泛的适用性。

注意：**更细粒度的控制**。

XML中的autowire控制的是整个bean的所有属性，而@Autowired注解是直接写在某个属性、某个set方法、某个构造方法上的。

再举个例子，如果一个类有多个构造方法，那么如果用XML的autowire=constructor，你无法控制到底用哪个构造方法，而你可以用@Autowired注解来直接指定你想用哪个构造方法。

同时，用@Autowired注解，还可以控制，哪些属性想被自动注入，哪些属性不想，这也是细粒度的控制。

但是@Autowired无法区分byType和byName，@Autowired是先byType，如果找到多个则byName。

那么XML的自动注入底层其实也就是:
1. set方法注入
2. 构造方法注入

##### @Autowired注解的自动注入

上文说了@Autowired注解，是byType和byName的结合。

@Autowired注解可以写在：
1. 属性上：先根据**属性类型**去找Bean，如果找到多个再根据**属性名**确定一个
2. 构造方法上：先根据方法**参数类型**去找Bean，如果找到多个再根据**参数名**确定一个
3. set方法上：先根据方法**参数类型**去找Bean，如果找到多个再根据**参数名**确定一个

而这种底层到了：
1. 属性注入
2. et方法注入
3. 构造方法注入

### 寻找注入点

在创建一个Bean的过程中，Spring会利用AutowiredAnnotationBeanPostProcessor的**postProcessMergedBeanDefinition()**找出注入点并缓存，找注入点的流程为：

    1. 遍历当前类的所有的属性字段Field
    2. 查看字段上是否存在@Autowired、@Value、@Inject中的其中任意一个，存在则认为该字段是一个注入点
    3. 如果字段是static的，则不进行注入
    4. 获取@Autowired中的required属性的值
    5. 将字段信息构造成一个AutowiredFieldElement对象，作为一个注入点对象添加到currElements集合中。
    6. 遍历当前类的所有方法Method
    7. 判断当前Method是否是桥接方法，如果是找到原方法
    8. 查看方法上是否存在@Autowired、@Value、@Inject中的其中任意一个，存在则认为该方法是一个注入点
    9. 如果方法是static的，则不进行注入
    10. 获取@Autowired中的required属性的值
    11. 将方法信息构造成一个AutowiredMethodElement对象，作为一个注入点对象添加到currElements集合中。
    12. 遍历完当前类的字段和方法后，将遍历父类的，直到没有父类。
    13. 最后将currElements集合封装成一个InjectionMetadata对象，作为当前Bean对于的注入点集合对象，并缓存。

#### static的字段或方法为什么不支持

```java
@Component
@Scope("prototype")
public class OrderService {


}

```

```java
@Component
@Scope("prototype")
public class UserService  {

	@Autowired
	private static OrderService orderService;

	public void test() {
		System.out.println("test123");
	}

}
```

看上面代码，UserService和OrderService都是原型Bean，假设Spring支持static字段进行自动注入，那么现在调用两次

    UserService userService1 = context.getBean("userService")
    UserService userService2 = context.getBean("userService")

问此时，userService1的orderService值是什么？还是它自己注入的值吗？

答案是:不是，一旦userService2 创建好了之后，static orderService字段的值就发生了修改了，从而出现bug。


#### 桥接方法

```java
public interface UserInterface<T> {
	void setOrderService(T t);
}
```

```java
@Component
public class UserService implements UserInterface<OrderService> {

	private OrderService orderService;

	@Override
	@Autowired
	public void setOrderService(OrderService orderService) {
		this.orderService = orderService;
	}

	public void test() {
		System.out.println("test123");
	}

}
```

UserService对应的字节码为：

```java
// class version 52.0 (52)
// access flags 0x21
// signature Ljava/lang/Object;Lcom/zhouyu/service/UserInterface<Lcom/zhouyu/service/OrderService;>;
// declaration: com/zhouyu/service/UserService implements com.zhouyu.service.UserInterface<com.zhouyu.service.OrderService>
public class com/zhouyu/service/UserService implements com/zhouyu/service/UserInterface {

  // compiled from: UserService.java

  @Lorg/springframework/stereotype/Component;()

  // access flags 0x2
  private Lcom/zhouyu/service/OrderService; orderService

  // access flags 0x1
  public <init>()V
   L0
    LINENUMBER 12 L0
    ALOAD 0
    INVOKESPECIAL java/lang/Object.<init> ()V
    RETURN
   L1
    LOCALVARIABLE this Lcom/zhouyu/service/UserService; L0 L1 0
    MAXSTACK = 1
    MAXLOCALS = 1

  // access flags 0x1
  public setOrderService(Lcom/zhouyu/service/OrderService;)V
  @Lorg/springframework/beans/factory/annotation/Autowired;()
   L0
    LINENUMBER 19 L0
    ALOAD 0
    ALOAD 1
    PUTFIELD com/zhouyu/service/UserService.orderService : Lcom/zhouyu/service/OrderService;
   L1
    LINENUMBER 20 L1
    RETURN
   L2
    LOCALVARIABLE this Lcom/zhouyu/service/UserService; L0 L2 0
    LOCALVARIABLE orderService Lcom/zhouyu/service/OrderService; L0 L2 1
    MAXSTACK = 2
    MAXLOCALS = 2

  // access flags 0x1
  public test()V
   L0
    LINENUMBER 23 L0
    GETSTATIC java/lang/System.out : Ljava/io/PrintStream;
    LDC "test123"
    INVOKEVIRTUAL java/io/PrintStream.println (Ljava/lang/String;)V
   L1
    LINENUMBER 24 L1
    RETURN
   L2
    LOCALVARIABLE this Lcom/zhouyu/service/UserService; L0 L2 0
    MAXSTACK = 2
    MAXLOCALS = 1

  // access flags 0x1041
  public synthetic bridge setOrderService(Ljava/lang/Object;)V
  @Lorg/springframework/beans/factory/annotation/Autowired;()
   L0
    LINENUMBER 11 L0
    ALOAD 0
    ALOAD 1
    CHECKCAST com/zhouyu/service/OrderService
    INVOKEVIRTUAL com/zhouyu/service/UserService.setOrderService (Lcom/zhouyu/service/OrderService;)V
    RETURN
   L1
    LOCALVARIABLE this Lcom/zhouyu/service/UserService; L0 L1 0
    MAXSTACK = 2
    MAXLOCALS = 2
}
```

可以看到在UserSerivce的字节码中有两个setOrderService方法：

    public setOrderService(Lcom/zhouyu/service/OrderService;)V
    public synthetic bridge setOrderService(Ljava/lang/Object;)V

并且都是存在@Autowired注解的。

所以在Spring中需要处理这种情况，当遍历到桥接方法时，得找到原方法。

### 注入点进行注入

Spring在AutowiredAnnotationBeanPostProcessor的**postProcessProperties()**方法中，会遍历所找到的注入点依次进行注入。

#### 字段注入

1. 遍历所有的**AutowiredFieldElement对象。**
2. 将对应的字段封装为**DependencyDescriptor对象。**
3. 调用BeanFactory的resolveDependency()方法，传入**DependencyDescriptor对象**，进行依赖查找，找到当前字段所匹配的Bean对象。
4. 将**DependencyDescriptor对象**和所找到的**结果对象beanName**封装成一个**ShortcutDependencyDescriptor对象**作为缓存，比如如果当前Bean是原型Bean，那么下次再来创建该Bean时，就可以直接拿缓存的结果对象beanName去BeanFactory中去那bean对象了，不用再次进行查找了
5. 利用反射将结果对象赋值给字段。

#### Set方法注入

1. 遍历所有的**AutowiredMethodElement对象**
2. 遍历将对应的方法的参数，将每个参数封装成**MethodParameter对象**
3. 将**MethodParameter对象**封装为**DependencyDescriptor对象**
4. 调用BeanFactory的resolveDependency()方法，传入**DependencyDescriptor对象**，进行依赖查找，找到当前方法参数所匹配的Bean对象。
5. 将**DependencyDescriptor对象**和所找到的**结果对象beanName**封装成一个**ShortcutDependencyDescriptor对象**作为缓存，比如如果当前Bean是原型Bean，那么下次再来创建该Bean时，就可以直接拿缓存的结果对象beanName去BeanFactory中去那bean对象了，不用再次进行查找了
6. 利用反射将找到的所有结果对象传给当前方法，并执行。

## 7、Spring之依赖注入源码解析（下）

上节课我们讲了Spring中的自动注入(byName,byType)和@Autowired注解的工作原理以及源码分析，那么今天这节课，我们来分析还没讲完的，剩下的核心的方法：

```java
@Nullable
Object resolveDependency(DependencyDescriptor descriptor, @Nullable String requestingBeanName,
		@Nullable Set<String> autowiredBeanNames, @Nullable TypeConverter typeConverter) throws BeansException;
```

该方法表示，传入一个依赖描述（DependencyDescriptor），该方法会根据该依赖描述从BeanFactory中找出对应的唯一的一个Bean对象。

下面来分析一下**DefaultListableBeanFactory中resolveDependency()方法的具体实现，具体流程图:**
https://www.processon.com/view/link/5f8d3c895653bb06ef076688

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-22-15-50-01.png width='100%'/></div><br/>

### findAutowireCandidates()实现

**根据类型找beanName的底层流程:** https://www.processon.com/view/link/6135bb430e3e7412ecd5d1f2

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-22-15-52-44.png width='100%'/></div><br/>

**对应执行流程图为**：https://www.processon.com/view/link/5f8fdfa8e401fd06fd984f20

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-22-15-54-00.png width='100%'/></div><br/>

1. 找出BeanFactory中类型为type的所有的Bean的名字，注意是名字，而不是Bean对象，因为我们可以根据BeanDefinition就能判断和当前type是不是匹配，不用生成Bean对象
2. 把resolvableDependencies中key为type的对象找出来并添加到result中
3. 遍历根据type找出的beanName，判断当前beanName对应的Bean是不是能够被自动注入
4. 先判断beanName对应的BeanDefinition中的autowireCandidate属性，如果为false，表示不能用来进行自动注入，如果为true则继续进行判断
5. 判断当前type是不是泛型，如果是泛型是会把容器中所有的beanName找出来的，如果是这种情况，那么在这一步中就要获取到泛型的真正类型，然后进行匹配，如果当前beanName和当前泛型对应的真实类型匹配，那么则继续判断
6. 如果当前DependencyDescriptor上存在@Qualifier注解，那么则要判断当前beanName上是否定义了Qualifier，并且是否和当前DependencyDescriptor上的Qualifier相等，相等则匹配
7. 经过上述验证之后，当前beanName才能成为一个可注入的，添加到result中

### 关于依赖注入中泛型注入的实现

首先在Java反射中，有一个Type接口，表示类型，具体分类为：

1. raw types：也就是普通Class
2. parameterized types：对应ParameterizedType接口，泛型类型
3. array types：对应GenericArrayType，泛型数组
4. type variables：对应TypeVariable接口，表示类型变量，也就是所定义的泛型，比如T、K
5. primitive types：基本类型，int、boolean

大家可以好好看看下面代码所打印的结果：

```java
public class TypeTest<T> {

	private int i;
	private Integer it;
	private int[] iarray;
	private List list;
	private List<String> slist;
	private List<T> tlist;
	private T t;
	private T[] tarray;

	public static void main(String[] args) throws NoSuchFieldException {

		test(TypeTest.class.getDeclaredField("i"));
		System.out.println("=======");
		test(TypeTest.class.getDeclaredField("it"));
		System.out.println("=======");
		test(TypeTest.class.getDeclaredField("iarray"));
		System.out.println("=======");
		test(TypeTest.class.getDeclaredField("list"));
		System.out.println("=======");
		test(TypeTest.class.getDeclaredField("slist"));
		System.out.println("=======");
		test(TypeTest.class.getDeclaredField("tlist"));
		System.out.println("=======");
		test(TypeTest.class.getDeclaredField("t"));
		System.out.println("=======");
		test(TypeTest.class.getDeclaredField("tarray"));

	}

	public static void test(Field field) {

		if (field.getType().isPrimitive()) {
			System.out.println(field.getName() + "是基本数据类型");
		} else {
			System.out.println(field.getName() + "不是基本数据类型");
		}

		if (field.getGenericType() instanceof ParameterizedType) {
			System.out.println(field.getName() + "是泛型类型");
		} else {
			System.out.println(field.getName() + "不是泛型类型");
		}

		if (field.getType().isArray()) {
			System.out.println(field.getName() + "是普通数组");
		} else {
			System.out.println(field.getName() + "不是普通数组");
		}

		if (field.getGenericType() instanceof GenericArrayType) {
			System.out.println(field.getName() + "是泛型数组");
		} else {
			System.out.println(field.getName() + "不是泛型数组");
		}

		if (field.getGenericType() instanceof TypeVariable) {
			System.out.println(field.getName() + "是泛型变量");
		} else {
			System.out.println(field.getName() + "不是泛型变量");
		}

	}

}
```

Spring中，但注入点是一个泛型时，也是会进行处理的，比如：

```java
@Component
public class UserService extends BaseService<OrderService, StockService> {

	public void test() {
		System.out.println(o);
	}

}

public class BaseService<O, S> {

	@Autowired
	protected O o;

	@Autowired
	protected S s;
}
```

1. Spring扫描时发现UserService是一个Bean
2. 那就取出注入点，也就是BaseService中的两个属性o、s
3. 接下来需要按注入点类型进行注入，但是o和s都是泛型，所以Spring需要确定o和s的具体类型。
4. 因为当前正在创建的是UserService的Bean，所以可以通过userService.getClass().getGenericSuperclass().getTypeName()获取到具体的泛型信息，比如com.zhouyu.service.BaseService<com.zhouyu.service.OrderService, com.zhouyu.service.StockService>
5. 然后再拿到UserService的父类BaseService的泛型变量： for (TypeVariable<? extends Class<?>> typeParameter : userService.getClass().getSuperclass().getTypeParameters()) {
   System.out.println(typeParameter.getName());
}
6. 通过上面两段代码，就能知道，o对应的具体就是OrderService，s对应的具体类型就是StockService
7. 然后再调用oField.getGenericType()就知道当前field使用的是哪个泛型，就能知道具体类型了

### @Qualifier的使用

定义两个注解：

```java
@Target({ElementType.TYPE, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier("random")
public @interface Random {
}
```

```java
@Target({ElementType.TYPE, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Qualifier("roundRobin")
public @interface RoundRobin {
}
```

定义一个接口和两个实现类，表示负载均衡：

```java
public interface LoadBalance {
	String select();
}
```

```java
@Component
@Random
public class RandomStrategy implements LoadBalance {

	@Override
	public String select() {
		return null;
	}
}
```

```java
@Component
@RoundRobin
public class RoundRobinStrategy implements LoadBalance {

	@Override
	public String select() {
		return null;
	}
}
```

使用：

```java
@Component
public class UserService  {

	@Autowired
	@RoundRobin
	private LoadBalance loadBalance;

	public void test() {
		System.out.println(loadBalance);
	}

}
```

### @Resource

@Resource注解底层工作流程图：
https://www.processon.com/view/link/5f91275f07912906db381f6e

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-22-16-02-50.png width='100%'/></div><br/>

## 8、Spring之循环依赖底层源码解析

### 什么是循环依赖？

很简单，就是A对象依赖了B对象，B对象依赖了A对象。

比如：

```java
// A依赖了B
class A{
	public B b;
}

// B依赖了A
class B{
	public A a;
}
```

那么循环依赖是个问题吗？

如果不考虑Spring，循环依赖并不是问题，因为对象之间相互依赖是很正常的事情。

比如

```java
A a = new A();
B b = new B();

a.b = b;
b.a = a;
```

这样，A,B就依赖上了。

但是，在Spring中循环依赖就是一个问题了，为什么？
因为，在Spring中，一个对象并不是简单new出来了，而是会经过一系列的Bean的生命周期，就是因为Bean的生命周期所以才会出现**循环依赖问题**。当然，在Spring中，出现循环依赖的场景很多，有的场景Spring自动帮我们解决了，而有的场景则需要程序员来解决，下文详细来说。

要明白Spring中的循环依赖，得先明白Spring中Bean的生命周期。

### Bean的生命周期

这里不会对Bean的生命周期进行详细的描述，只描述一下大概的过程。

Bean的生命周期指的就是：在Spring中，Bean是如何生成的？

被Spring管理的对象叫做Bean。Bean的生成步骤如下：
1. Spring扫描class得到BeanDefinition
2. 根据得到的BeanDefinition去生成bean
3. 首先根据class推断构造方法
4. 根据推断出来的构造方法，反射，得到一个对象（暂时叫做原始对象）
5. 填充原始对象中的属性（依赖注入）
6. 如果原始对象中的某个方法被AOP了，那么则需要根据原始对象生成一个代理对象
7. 把最终生成的代理对象放入单例池（源码中叫做singletonObjects）中，下次getBean时就直接从单例池拿即可

可以看到，对于Spring中的Bean的生成过程，步骤还是很多的，并且不仅仅只有上面的7步，还有很多很多，比如Aware回调、初始化等等，这里不详细讨论。

可以发现，在Spring中，构造一个Bean，包括了new这个步骤（第4步构造方法反射）。

得到一个原始对象后，Spring需要给对象中的属性进行依赖注入，那么这个注入过程是怎样的？

比如上文说的A类，A类中存在一个B类的b属性，所以，当A类生成了一个原始对象之后，就会去给b属性去赋值，此时就会根据b属性的类型和属性名去BeanFactory中去获取B类所对应的单例bean。如果此时BeanFactory中存在B对应的Bean，那么直接拿来赋值给b属性；如果此时BeanFactory中不存在B对应的Bean，则需要生成一个B对应的Bean，然后赋值给b属性。

问题就出现在第二种情况，如果此时B类在BeanFactory中还没有生成对应的Bean，那么就需要去生成，就会经过B的Bean的生命周期。

那么在创建B类的Bean的过程中，如果B类中存在一个A类的a属性，那么在创建B的Bean的过程中就需要A类对应的Bean，但是，触发B类Bean的创建的条件是A类Bean在创建过程中的依赖注入，所以这里就出现了循环依赖：

ABean创建-->依赖了B属性-->触发BBean创建--->B依赖了A属性--->需要ABean（但ABean还在创建过程中）

从而导致ABean创建不出来，BBean也创建不出来。

这是循环依赖的场景，但是上文说了，在Spring中，通过某些机制帮开发者解决了部分循环依赖的问题，这个机制就是**三级缓存**。

### 三级缓存

三级缓存是通用的叫法。
一级缓存为：**singletonObjects**
二级缓存为：**earlySingletonObjects**
三级缓存为：**singletonFactories**

**先稍微解释一下这三个缓存的作用，后面详细分析**：

    singletonObjects中缓存的是已经经历了完整生命周期的bean对象。
    earlySingletonObjects比singletonObjects多了一个early，表示缓存的是早期的bean对象。早期是什么意思？表示Bean的生命周期还没走完就把这个Bean放入了earlySingletonObjects。
    singletonFactories中缓存的是ObjectFactory，表示对象工厂，表示用来创建早期bean对象的工厂。


### 解决循环依赖思路分析

先来分析为什么缓存能解决循环依赖。

上文分析得到，之所以产生循环依赖的问题，主要是：

A创建时--->需要B---->B去创建--->需要A，从而产生了循环

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-22-16-29-01.png width='100%'/></div><br/>

那么如何打破这个循环，加个中间人（缓存）

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-22-16-34-27.png width='100%'/></div><br/>

A的Bean在创建过程中，在进行依赖注入之前，先把A的原始Bean放入缓存（提早暴露，只要放到缓存了，其他Bean需要时就可以从缓存中拿了），放入缓存后，再进行依赖注入，此时A的Bean依赖了B的Bean，如果B的Bean不存在，则需要创建B的Bean，而创建B的Bean的过程和A一样，也是先创建一个B的原始对象，然后把B的原始对象提早暴露出来放入缓存中，然后在对B的原始对象进行依赖注入A，此时能从缓存中拿到A的原始对象（虽然是A的原始对象，还不是最终的Bean），B的原始对象依赖注入完了之后，B的生命周期结束，那么A的生命周期也能结束。

因为整个过程中，都只有一个A原始对象，所以对于B而言，就算在属性注入时，注入的是A原始对象，也没有关系，因为A原始对象在后续的生命周期中在堆中没有发生变化。

从上面这个分析过程中可以得出，只需要一个缓存就能解决循环依赖了，那么为什么Spring中还需要**singletonFactories**呢？

这是难点，基于上面的场景想一个问题：如果A的原始对象注入给B的属性之后，A的原始对象进行了AOP产生了一个代理对象，此时就会出现，对于A而言，它的Bean对象其实应该是AOP之后的代理对象，而B的a属性对应的并不是AOP之后的代理对象，这就产生了冲突。

**B依赖的A和最终的A不是同一个对象。**

AOP就是通过一个BeanPostProcessor来实现的，这个BeanPostProcessor就是AnnotationAwareAspectJAutoProxyCreator，它的父类是AbstractAutoProxyCreator，而在Spring中AOP利用的要么是JDK动态代理，要么CGLib的动态代理，所以如果给一个类中的某个方法设置了切面，那么这个类最终就需要生成一个代理对象。

一般过程就是：A类--->生成一个普通对象-->属性注入-->基于切面生成一个代理对象-->把代理对象放入singletonObjects单例池中。

而AOP可以说是Spring中除开IOC的另外一大功能，而循环依赖又是属于IOC范畴的，所以这两大功能想要并存，Spring需要特殊处理。

如何处理的，就是利用了**第三级缓存singletonFactories**。

首先，singletonFactories中存的是某个beanName对应的ObjectFactory，在bean的生命周期中，生成完原始对象之后，就会构造一个ObjectFactory存入singletonFactories中。这个ObjectFactory是一个函数式接口，所以**支持Lambda表达式：() -> getEarlyBeanReference(beanName, mbd, bean)**

上面的Lambda表达式就是一个ObjectFactory，执行该Lambda表达式就会去执行getEarlyBeanReference方法，而该方法如下：

```java
protected Object getEarlyBeanReference(String beanName, RootBeanDefinition mbd, Object bean) {
	Object exposedObject = bean;
	if (!mbd.isSynthetic() && hasInstantiationAwareBeanPostProcessors()) {
		for (BeanPostProcessor bp : getBeanPostProcessors()) {
			if (bp instanceof SmartInstantiationAwareBeanPostProcessor) {
				SmartInstantiationAwareBeanPostProcessor ibp = (SmartInstantiationAwareBeanPostProcessor) bp;
				exposedObject = ibp.getEarlyBeanReference(exposedObject, beanName);
			}
		}
	}
	return exposedObject;
}
```

该方法会去执行SmartInstantiationAwareBeanPostProcessor中的getEarlyBeanReference方法，而这个接口下的实现类中只有两个类实现了这个方法，一个是AbstractAutoProxyCreator，一个是InstantiationAwareBeanPostProcessorAdapter，它的实现如下：

```java
// InstantiationAwareBeanPostProcessorAdapter
@Override
public Object getEarlyBeanReference(Object bean, String beanName) throws BeansException {
	return bean;
}
```

```java
// AbstractAutoProxyCreator
@Override
public Object getEarlyBeanReference(Object bean, String beanName) {
	Object cacheKey = getCacheKey(bean.getClass(), beanName);
	this.earlyProxyReferences.put(cacheKey, bean);
	return wrapIfNecessary(bean, beanName, cacheKey);
}
```

在整个Spring中，默认就只有AbstractAutoProxyCreator真正意义上实现了getEarlyBeanReference方法，而该类就是用来进行AOP的。上文提到的AnnotationAwareAspectJAutoProxyCreator的父类就是AbstractAutoProxyCreator。

那么getEarlyBeanReference方法到底在干什么？
首先得到一个cachekey，cachekey就是beanName。
然后把beanName和bean（这是原始对象）存入earlyProxyReferences中
调用wrapIfNecessary进行AOP，得到一个代理对象。

那么，什么时候会调用getEarlyBeanReference方法呢？回到循环依赖的场景中

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-22-16-38-02.png width='100%'/></div><br/>

**左边文字：**
这个ObjectFactory就是上文说的labmda表达式，中间有getEarlyBeanReference方法，注意存入singletonFactories时并不会执行lambda表达式，也就是不会执行getEarlyBeanReference方法

**右边文字：**
从singletonFactories根据beanName得到一个ObjectFactory，然后执行ObjectFactory，也就是执行getEarlyBeanReference方法，此时会得到一个A原始对象经过AOP之后的代理对象，然后把该代理对象放入earlySingletonObjects中，注意此时并没有把代理对象放入singletonObjects中，那什么时候放入到singletonObjects中呢？

我们这个时候得来理解一下earlySingletonObjects的作用，此时，我们只得到了A原始对象的代理对象，这个对象还不完整，因为A原始对象还没有进行属性填充，所以此时不能直接把A的代理对象放入singletonObjects中，所以只能把代理对象放入earlySingletonObjects，假设现在有其他对象依赖了A，那么则可以从earlySingletonObjects中得到A原始对象的代理对象了，并且是A的同一个代理对象。

当B创建完了之后，A继续进行生命周期，而A在完成属性注入后，会按照它本身的逻辑去进行AOP，而此时我们知道A原始对象已经经历过了AOP，所以对于A本身而言，不会再去进行AOP了，那么怎么判断一个对象是否经历过了AOP呢？会利用上文提到的earlyProxyReferences，在AbstractAutoProxyCreator的postProcessAfterInitialization方法中，会去判断当前beanName是否在earlyProxyReferences，如果在则表示已经提前进行过AOP了，无需再次进行AOP。

对于A而言，进行了AOP的判断后，以及BeanPostProcessor的执行之后，就需要把A对应的对象放入singletonObjects中了，但是我们知道，应该是要把A的代理对象放入singletonObjects中，所以此时需要从earlySingletonObjects中得到代理对象，然后入singletonObjects中。

**整个循环依赖解决完毕。**

### 总结

至此，总结一下三级缓存：

1. **singletonObjects**：缓存经过了**完整生命周期**的bean
2. **earlySingletonObjects**：缓存**未经过完整生命周期的bean**，如果某个bean出现了循环依赖，就会**提前**把这个暂时未经过完整生命周期的bean放入earlySingletonObjects中，这个bean如果要经过AOP，那么就会把代理对象放入earlySingletonObjects中，否则就是把原始对象放入earlySingletonObjects，但是不管怎么样，就是是代理对象，代理对象所代理的原始对象也是没有经过完整生命周期的，所以放入earlySingletonObjects我们就可以统一认为是**未经过完整生命周期的bean**。
3. **singletonFactories**：缓存的是一个ObjectFactory，也就是一个Lambda表达式。在每个Bean的生成过程中，经过**实例化**得到一个原始对象后，都会提前基于原始对象暴露一个Lambda表达式，并保存到三级缓存中，**这个Lambda表达式可能用到，也可能用不到**，如果当前Bean没有出现循环依赖，那么这个Lambda表达式没用，当前bean按照自己的生命周期正常执行，执行完后直接把当前bean放入singletonObjects中，如果当前bean在依赖注入时发现出现了循环依赖（当前正在创建的bean被其他bean依赖了），则从三级缓存中拿到Lambda表达式，并执行Lambda表达式得到一个对象，并把得到的对象放入二级缓存（(如果当前Bean需要AOP，那么执行lambda表达式，得到就是对应的代理对象，如果无需AOP，则直接得到一个原始对象)）。
4. 其实还要一个缓存，就是**earlyProxyReferences**，它用来记录某个原始对象是否进行过AOP了。

#### 反向分析一下singletonFactories

为什么需要singletonFactories？假设没有singletonFactories，只有earlySingletonObjects，earlySingletonObjects是二级缓存，它内部存储的是为经过完整生命周期的bean对象，Spring原有的流程是出现了循环依赖的情况下：

1. 先从singletonFactories中拿到lambda表达式，这里肯定是能拿到的，因为每个bean**实例化之后，依赖注入之前**，就会生成一个lambda表示放入singletonFactories中
2. 执行lambda表达式，得到结果，将结果放入earlySingletonObjects中

那如果没有singletonFactories，该如何把原始对象或AOP之后的代理对象放入earlySingletonObjects中呢？何时放入呢？

首先，将原始对象或AOP之后的代理对象放入earlySingletonObjects中的有两种：

1. 实例化之后，依赖注入之前：如果是这样，那么对于每个bean而言，都是在依赖注入之前会去进行AOP，这是不符合bean生命周期步骤的设计的。
2. 真正发现某个bean出现了循环依赖时：按现在Spring源码的流程来说，就是getSingleton(String beanName, boolean allowEarlyReference)中，是在这个方法中判断出来了当前获取的这个bean在创建中，就表示获取的这个bean出现了循环依赖，那在这个方法中该如何拿到原始对象呢？更加重要的是，该如何拿到AOP之后的代理对象呢？难道在这个方法中去循环调用BeanPostProcessor的初始化后的方法吗？不是做不到，不太合适，代码太丑。**最关键的是在这个方法中该如何拿到原始对象呢？**还是得需要一个Map，预习把这个Bean实例化后的对象存在这个Map中，那这样的话还不如直接用第一种方案，但是第一种又直接打破了Bean生命周期的设计。

所以，我们可以发现，现在Spring所用的singletonFactories，为了调和不同的情况，在singletonFactories中存的是lambda表达式，这样的话，只有在出现了循环依赖的情况，才会执行lambda表达式，才会进行AOP，也就说只有在出现了循环依赖的情况下才会打破Bean生命周期的设计，如果一个Bean没有出现循环依赖，那么它还是遵守了Bean的生命周期的设计的。

### 课上github上issue地址：

https://github.com/spring-projects/spring-framework/pull/26376
https://github.com/spring-projects/spring-framework/issues/25667
https://github.com/spring-projects/spring-framework/issues/13117


## 9、Spring之推断构造方法源码解析

### spring选择构造方法的4种方式

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-23-09-54-57.png width='100%'/></div><br/>

### 推断构造方法大致流程

**推断构造方法流程图**:

 https://www.processon.com/view/link/5f97bc717d9c0806f291d7eb

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-22-17-01-05.png width='100%'/></div><br/>

AutowiredAnnotationBeanPostProcessor中推断构造方法不同情况思维脑图：
https://www.processon.com/view/link/6146def57d9c08198c58bb26

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-24-10-06-28.png width='100%'/></div><br/>

Spring中的一个bean，需要实例化得到一个对象，而实例化就需要用到构造方法。

一般情况下，一个类只有一个构造方法：

1. 要么是无参的构造方法
2. 要么是有参的构造方法

如果只有**一个无参的构造方法**，那么实例化就只能使用这个构造方法了。
如果只有**一个有参的构造方法**，那么实例化时能使用这个构造方法吗？要分情况讨论：

1. 使用AnnotationConfigApplicationContext，会使用这个构造方法进行实例化，那么Spring会根据构造方法的参数信息去寻找bean，然后传给构造方法
2. 使用ClassPathXmlApplicationContext，表示使用XML的方式来使用bean，要么在XML中指定构造方法的参数值(手动指定)，要么配置autowire=constructor让Spring自动去寻找bean做为构造方法参数值。

上面是只有一个构造方法的情况，那么如果有多个构造方法呢？

又分为两种情况，多个构造方法中存不存在无参的构造方法。

分析：一个类存在多个构造方法，那么Spring进行实例化之前，该如何去确定到底用哪个构造方法呢？

1. 如果开发者指定了想要使用的构造方法，那么就用这个构造方法
2. 如果开发者没有指定想要使用的构造方法，则看开发者有没有让Spring自动去选择构造方法
3. 如果开发者也没有让Spring自动去选择构造方法，则Spring利用无参构造方法，如果没有无参构造方法，则报错


针对第一点，开发者可以通过什么方式来指定使用哪个构造方法呢？

1. xml中的\<constructor-arg>标签，这个标签表示构造方法参数，所以可以根据这个确定想要使用的构造方法的参数个数，从而确定想要使用的构造方法
通过@Autowired注解，@Autowired注解可以写在构造方法上，所以哪个构造方法上写了
2. 用@Autowired注解，表示开发者想使用哪个构造方法，当然，它和第一个方式的不同点是，通过xml的方式，我们直接指定了构造方法的参数值，而通过@Autowired注解的方式，需要Spring通过byType+byName的方式去找到符合条件的bean作为构造方法的参数值

再来看第二点，如果开发者没有指定想要使用的构造方法，则看开发者有没有让Spring自动去选择构造方法，对于这一点，只能用在ClassPathXmlApplicationContext，因为通过AnnotationConfigApplicationContext没有办法去指定某个bean可以自动去选择构造方法，而通过ClassPathXmlApplicationContext可以在xml中指定某个bean的autowire为constructor，虽然这个属性表示通过构造方法自动注入，所以需要自动的去选择一个构造方法进行自动注入，因为是构造方法，所以顺便是进行实例化。

当然，还有一种情况，就是多个构造方法上写了@Autowired注解，那么此时Spring会报错。
但是，因为@Autowired还有一个属性required，默认为ture，所以一个类中，只有能一个构造方法标注了@Autowired或@Autowired（required=true），有多个会报错。但是可以有多个@Autowired（required=false）,这种情况下，需要Spring从这些构造方法中去自动选择一个构造方法。

### 源码思路

1. AbstractAutowireCapableBeanFactory类中的createBeanInstance()方法会去创建一个Bean实例
2. 根据BeanDefinition加载类得到Class对象
3. 如果BeanDefinition绑定了一个Supplier，那就调用Supplier的get方法得到一个对象并直接返回
4. 如果BeanDefinition中存在factoryMethodName，那么就调用该工厂方法得到一个bean对象并返回
5. 如果BeanDefinition已经自动构造过了，那就调用autowireConstructor()自动构造一个对象
6. 调用SmartInstantiationAwareBeanPostProcessor的determineCandidateConstructors()方法得到哪些构造方法是可以用的
7. 如果存在可用得构造方法，或者当前BeanDefinition的autowired是AUTOWIRE_CONSTRUCTOR，或者BeanDefinition中指定了构造方法参数值，或者创建Bean的时候指定了构造方法参数值，那么就调用**autowireConstructor()**方法自动构造一个对象
8. 最后，如果不是上述情况，就根据无参的构造方法实例化一个对象

#### autowireConstructor()

1. 先检查是否指定了具体的构造方法和构造方法参数值，或者在BeanDefinition中缓存了具体的构造方法或构造方法参数值，如果存在那么则直接使用该构造方法进行实例化
2. 如果没有确定的构造方法或构造方法参数值，那么

        a. 如果没有确定的构造方法，那么则找出类中所有的构造方法
        b. 如果只有一个无参的构造方法，那么直接使用无参的构造方法进行实例化
        c. 如果有多个可用的构造方法或者当前Bean需要自动通过构造方法注入
        d. 根据所指定的构造方法参数值，确定所需要的最少的构造方法参数值的个数
        e. 对所有的构造方法进行排序，参数个数多的在前面
        f. 遍历每个构造方法
        g. 如果不是调用getBean方法时所指定的构造方法参数值，那么则根据构造方法参数类型找值
        h. 如果时调用getBean方法时所指定的构造方法参数值，就直接利用这些值
        i. 如果根据当前构造方法找到了对应的构造方法参数值，那么这个构造方法就是可用的，但是不一定这个构造方法就是最佳的，所以这里会涉及到是否有多个构造方法匹配了同样的值，这个时候就会用值和构造方法类型进行匹配程度的打分，找到一个最匹配的

### 为什么分越少优先级越高？

主要是计算找到的bean和构造方法参数类型匹配程度有多高。

假设bean的类型为A，A的父类是B，B的父类是C，同时A实现了接口D
如果构造方法的参数类型为A，那么完全匹配，得分为0
如果构造方法的参数类型为B，那么得分为2
如果构造方法的参数类型为C，那么得分为4
如果构造方法的参数类型为D，那么得分为1

可以直接使用如下代码进行测试：

```java
Object[] objects = new Object[]{new A()};

// 0
System.out.println(MethodInvoker.getTypeDifferenceWeight(new Class[]{A.class}, objects));

// 2
System.out.println(MethodInvoker.getTypeDifferenceWeight(new Class[]{B.class}, objects));

// 4
System.out.println(MethodInvoker.getTypeDifferenceWeight(new Class[]{C.class}, objects));

// 1
System.out.println(MethodInvoker.getTypeDifferenceWeight(new Class[]{D.class}, objects));
```

所以，我们可以发现，越匹配分数越低。

### @Bean的情况

首先，Spring会把@Bean修饰的方法解析成BeanDefinition：

1. 如果方法不是static的，那么解析出来的BeanDefinition中：
   
        factoryBeanName为AppConfig所对应的beanName，比如"appConfig"
        factoryMethodName为对应的方法名，比如"aService"
        factoryClass为AppConfig.class

2. 如果方法是static的，那么解析出来的BeanDefinition中：

        factoryBeanName为null
        factoryMethodName为对应的方法名，比如"aService"
        factoryClass也为AppConfig.class

在由@Bean生成的BeanDefinition中，有一个重要的属性isFactoryMethodUnique，表示factoryMethod是不是唯一的，在普通情况下@Bean生成的BeanDefinition的isFactoryMethodUnique为true，但是如果出现了方法重载，那么就是特殊的情况，比如：

```java
	@Bean
	public static AService aService(){
		return new AService();
	}

	@Bean
	public AService aService(BService bService){
		return new AService();
	}
```

虽然有两个@Bean，但是肯定只会生成一个aService的Bean，那么Spring在处理@Bean时，也只会生成一个aService的BeanDefinition，比如Spring先解析到第一个@Bean，会生成一个BeanDefinition，此时isFactoryMethodUnique为true，但是解析到第二个@Bean时，会判断出来beanDefinitionMap中已经存在一个aService的BeanDefinition了，那么会把之前的这个BeanDefinition的isFactoryMethodUnique修改为false，并且不会生成新的BeanDefinition了。

并且后续在根据BeanDefinition创建Bean时，会根据isFactoryMethodUnique来操作，如果为true，那就表示当前BeanDefinition只对应了一个方法，那也就是只能用这个方法来创建Bean了，但是如果isFactoryMethodUnique为false，那就表示当前BeanDefition对应了多个方法，需要和推断构造方法的逻辑一样，去选择用哪个方法来创建Bean。

## 10、Spring之启动过程源码解析

### 前言分析

通常，我们说的Spring启动，就是构造ApplicationContext对象以及调用refresh()方法的过程。

首先，Spring启动过程主要做了这么几件事情：

1. 构造一个BeanFactory对象
2. 解析配置类，得到BeanDefinition，并注册到BeanFactory中

        解析@ComponentScan，此时就会完成扫描
        解析@Import
        解析@Bean
        ...

3. 因为ApplicationContext还支持国际化，所以还需要初始化MessageSource对象
4. 因为ApplicationContext还支持事件机制，所以还需要初始化ApplicationEventMulticaster对象
5. 把用户定义的ApplicationListener对象添加到ApplicationContext中，等Spring启动完了就要发布事件了
6. 创建非懒加载的单例Bean对象，并存在BeanFactory的单例池中。
7. 调用Lifecycle Bean的start()方法
8. 发布ContextRefreshedEvent事件

由于Spring启动过程中要创建非懒加载的单例Bean对象，那么就需要用到BeanPostProcessor，所以Spring在启动过程中就需要做两件事：

1. 生成默认的BeanPostProcessor对象，并添加到BeanFactory中

        AutowiredAnnotationBeanPostProcessor：处理@Autowired、@Value
        CommonAnnotationBeanPostProcessor：处理@Resource、@PostConstruct、@PreDestroy
        ApplicationContextAwareProcessor：处理ApplicationContextAware等回调

1. 找到外部用户所定义的BeanPostProcessor对象（类型为BeanPostProcessor的Bean对象），并添加到BeanFactory中

### BeanFactoryPostProcessor

BeanPostProcessor表示Bean的后置处理器，是用来对Bean进行加工的，类似的，BeanFactoryPostProcessor理解为BeanFactory的后置处理器，用来用对BeanFactory进行加工的。

Spring支持用户定义BeanFactoryPostProcessor的实现类Bean，来对BeanFactory进行加工，比如:

```java
@Component
public class ZhouyuBeanFactoryPostProcessor implements BeanFactoryPostProcessor {

	@Override
	public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
		BeanDefinition beanDefinition = beanFactory.getBeanDefinition("userService");
		beanDefinition.setAutowireCandidate(false);
	}
}
```

以上代码，就利用了BeanFactoryPostProcessor来拿到BeanFactory，然后获取BeanFactory内的某个BeanDefinition对象并进行修改，注意这一步是发生在Spring启动时，创建单例Bean之前的，所以此时对BeanDefinition进行修改是会生效的。

注意：在ApplicationContext内部有一个核心的DefaultListableBeanFactory，它实现了ConfigurableListableBeanFactory和BeanDefinitionRegistry接口，所以ApplicationContext和DefaultListableBeanFactory是可以注册BeanDefinition的，但是ConfigurableListableBeanFactory是不能注册BeanDefinition的，只能获取BeanDefinition，然后做修改。

所以Spring还提供了一个BeanFactoryPostProcessor的子接口：**BeanDefinitionRegistryPostProcessor**

### BeanDefinitionRegistryPostProcessor

```java
public interface BeanDefinitionRegistryPostProcessor extends BeanFactoryPostProcessor {

	void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) throws BeansException;

}
```

我们可以看到BeanDefinitionRegistryPostProcessor继承了BeanFactoryPostProcessor接口，并新增了一个方法，注意方法的参数为BeanDefinitionRegistry，所以如果我们提供一个类来实现BeanDefinitionRegistryPostProcessor，那么在postProcessBeanDefinitionRegistry()方法中就可以注册BeanDefinition了。比如：

```java
@Component
public class ZhouyuBeanDefinitionRegistryPostProcessor implements BeanDefinitionRegistryPostProcessor {

	@Override
	public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) throws BeansException {
		AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.genericBeanDefinition().getBeanDefinition();
		beanDefinition.setBeanClass(User.class);
		registry.registerBeanDefinition("user", beanDefinition);
	}

	@Override
	public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
		BeanDefinition beanDefinition = beanFactory.getBeanDefinition("userService");
		beanDefinition.setAutowireCandidate(false);
	}
}
```

### 如何理解refresh()？

```java
/**
	 * Load or refresh the persistent representation of the configuration,
	 * which might an XML file, properties file, or relational database schema.
	 * <p>As this is a startup method, it should destroy already created singletons
	 * if it fails, to avoid dangling resources. In other words, after invocation
	 * of that method, either all or no singletons at all should be instantiated.
	 * @throws BeansException if the bean factory could not be initialized
	 * @throws IllegalStateException if already initialized and multiple refresh
	 * attempts are not supported
	 */
	void refresh() throws BeansException, IllegalStateException;
```

这是ConfigurableApplicationContext接口上refresh()方法的注释，意思是：加载或刷新持久化的配置，可能是XML文件、属性文件或关系数据库中存储的。由于这是一个启动方法，如果失败，它应该销毁已经创建的单例，以避免暂用资源。换句话说，在调用该方法之后，应该实例化所有的单例，或者根本不实例化单例 。

有个理念需要注意：**ApplicationContext关闭之后不代表JVM也关闭了，ApplicationContext是属于JVM的，说白了ApplicationContext也是JVM中的一个对象。**

在Spring的设计中，也提供可以刷新的ApplicationContext和不可以刷新的ApplicationContext。比如：

```java
AbstractRefreshableApplicationContext extends AbstractApplicationContext
```

就是可以刷新的

```java
GenericApplicationContext extends AbstractApplicationContext
```

就是不可以刷新的。

```java
AnnotationConfigApplicationContext继承的是GenericApplicationContext，所以它是不能刷新的。
AnnotationConfigWebApplicationContext继承的是AbstractRefreshableWebApplicationContext，所以它是可以刷的。
```

上面说的**不能刷新是指不能重复刷新，只能调用一次refresh方法，第二次时会报错。**

### refresh()底层原理流程

底层原理流程图：https://www.processon.com/view/link/5f60a7d71e08531edf26a919

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-23-15-54-09.png width='100%'/></div><br/>

下面以AnnotationConfigApplicationContext为例子，来介绍refresh的底层原理。

1. 在调用AnnotationConfigApplicationContext的构造方法之前，会调用父类GenericApplicationContext的无参构造方法，会构造一个BeanFactory，为**DefaultListableBeanFactory。**
2. 构造AnnotatedBeanDefinitionReader（**主要作用添加一些基础的PostProcessor，同时可以通过reader进行BeanDefinition的注册**），同时对BeanFactory进行设置和添加**PostProcessor**（后置处理器）

        设置dependencyComparator：AnnotationAwareOrderComparator，它是一个Comparator，是用来进行排序的，会获取某个对象上的Order注解或者通过实现Ordered接口所定义的值进行排序，在日常开发中可以利用这个类来进行排序。
        设置autowireCandidateResolver：ContextAnnotationAutowireCandidateResolver，用来解析某个Bean能不能进行自动注入，比如某个Bean的autowireCandidate属性是否等于true
        向BeanFactory中添加ConfigurationClassPostProcessor对应的BeanDefinition，它是一个BeanDefinitionRegistryPostProcessor，并且实现了PriorityOrdered接口
        向BeanFactory中添加AutowiredAnnotationBeanPostProcessor对应的BeanDefinition，它是一个InstantiationAwareBeanPostProcessorAdapter，MergedBeanDefinitionPostProcessor
        向BeanFactory中添加CommonAnnotationBeanPostProcessor对应的BeanDefinition，它是一个InstantiationAwareBeanPostProcessor，InitDestroyAnnotationBeanPostProcessor
        向BeanFactory中添加EventListenerMethodProcessor对应的BeanDefinition，它是一个BeanFactoryPostProcessor，SmartInitializingSingleton
        向BeanFactory中添加DefaultEventListenerFactory对应的BeanDefinition，它是一个EventListenerFactory

3. 构造ClassPathBeanDefinitionScanner（**主要作用可以用来扫描得到并注册BeanDefinition**），同时进行设置：

        设置this.includeFilters = AnnotationTypeFilter(Component.class)
        设置environment
        设置resourceLoader

4. 利用reader注册AppConfig为BeanDefinition，类型为AnnotatedGenericBeanDefinition
5. **接下来就是调用refresh方法**
6. prepareRefresh()：

        记录启动时间
        可以允许子容器设置一些内容到Environment中
        验证Environment中是否包括了必须要有的属性

7. obtainFreshBeanFactory()：进行BeanFactory的refresh，在这里会去调用子类的refreshBeanFactory方法，具体子类是怎么刷新的得看子类，然后再调用子类的getBeanFactory方法，重新得到一个BeanFactory
8. prepareBeanFactory(beanFactory)：

        a. 设置beanFactory的类加载器
        b. 设置表达式解析器：StandardBeanExpressionResolver，用来解析Spring中的表达式
        c. 添加PropertyEditorRegistrar：ResourceEditorRegistrar，PropertyEditor类型转化器注册器，用来注册一些默认的PropertyEditor
        d. 添加一个Bean的后置处理器：ApplicationContextAwareProcessor，是一个BeanPostProcessor，用来执行EnvironmentAware、ApplicationEventPublisherAware等回调方法
        e. 添加ignoredDependencyInterface：可以向这个属性中添加一些接口，如果某个类实现了这个接口，并且这个类中的某些set方法在接口中也存在，那么这个set方法在自动注入的时候是不会执行的，比如EnvironmentAware这个接口，如果某个类实现了这个接口，那么就必须实现它的setEnvironment方法，而这是一个set方法，和Spring中的autowire是冲突的，那么Spring在自动注入时是不会调用setEnvironment方法的，而是等到回调Aware接口时再来调用（注意，这个功能仅限于xml的autowire，@Autowired注解是忽略这个属性的）
            EnvironmentAware
            EmbeddedValueResolverAware
            ResourceLoaderAware
            ApplicationEventPublisherAware
            MessageSourceAware
            ApplicationContextAware
            另外其实在构造BeanFactory的时候就已经提前添加了另外三个：
            BeanNameAware
            BeanClassLoaderAware
            BeanFactoryAware

        f.添加resolvableDependencies：在byType进行依赖注入时，会先从这个属性中根据类型找bean
            BeanFactory.class：当前BeanFactory对象
            ResourceLoader.class：当前ApplicationContext对象
            ApplicationEventPublisher.class：当前ApplicationContext对象
            ApplicationContext.class：当前ApplicationContext对象 

        g. 添加一个Bean的后置处理器：ApplicationListenerDetector，是一个BeanPostProcessor，用来判断某个Bean是不是ApplicationListener，如果是则把这个Bean添加到ApplicationContext中去，注意一个ApplicationListener只能是单例的
        h. 添加一个Bean的后置处理器：LoadTimeWeaverAwareProcessor，是一个BeanPostProcessor，用来判断某个Bean是不是实现了LoadTimeWeaverAware接口，如果实现了则把ApplicationContext中的loadTimeWeaver回调setLoadTimeWeaver方法设置给该Bean。
        i. 添加一些单例bean到单例池： 
            "environment"：Environment对象
            "systemProperties"：System.getProperties()返回的Map对象
            "systemEnvironment"：System.getenv()返回的Map对象     

9. postProcessBeanFactory(beanFactory) ： 提供给AbstractApplicationContext的子类进行扩展，具体的子类，可以继续向BeanFactory中再添加一些东西
10. invokeBeanFactoryPostProcessors(beanFactory)：执行BeanFactoryPostProcessor 

        a. 此时在BeanFactory中会存在一个BeanFactoryPostProcessor：ConfigurationClassPostProcessor，它也是一个BeanDefinitionRegistryPostProcessor
        b. 第一阶段
        c. 从BeanFactory中找到类型为BeanDefinitionRegistryPostProcessor的beanName，也就是ConfigurationClassPostProcessor， 然后调用BeanFactory的getBean方法得到实例对象
        d. 执行ConfigurationClassPostProcessor的postProcessBeanDefinitionRegistry()方法:
            i. 解析AppConfig类
            ii. 扫描得到BeanDefinition并注册
            iii. 解析@Import，@Bean等注解得到BeanDefinition并注册
            iv. 详细的看另外的笔记，专门分析了ConfigurationClassPostProcessor是如何工作的
            v. 在这里，我们只需要知道在这一步会去得到BeanDefinition，而这些BeanDefinition中可能存在BeanFactoryPostProcessor和BeanDefinitionRegistryPostProcessor，所以执行完ConfigurationClassPostProcessor的postProcessBeanDefinitionRegistry()方法后，还需要继续执行其他BeanDefinitionRegistryPostProcessor的postProcessBeanDefinitionRegistry()方法

        e. 执行其他BeanDefinitionRegistryPostProcessor的postProcessBeanDefinitionRegistry()方法
        f. 执行所有BeanDefinitionRegistryPostProcessor的postProcessBeanFactory()方法
        g. 第二阶段
        h. 从BeanFactory中找到类型为BeanFactoryPostProcessor的beanName，而这些BeanFactoryPostProcessor包括了上面的BeanDefinitionRegistryPostProcessor
        i. 执行还没有执行过的BeanFactoryPostProcessor的postProcessBeanFactory()方法               

11. 到此，所有的BeanFactoryPostProcessor的逻辑都执行完了，主要做的事情就是得到BeanDefinition并注册到BeanFactory中
12. registerBeanPostProcessors(beanFactory)：因为上面的步骤完成了扫描，这个过程中程序员可能自己定义了一些BeanPostProcessor，在这一步就会把BeanFactory中所有的BeanPostProcessor找出来并实例化得到一个对象，并添加到BeanFactory中去（属性**beanPostProcessors**），最后再重新添加一个ApplicationListenerDetector对象（之前其实就添加了过，这里是为了把ApplicationListenerDetector移动到最后）
13. initMessageSource()：如果BeanFactory中存在一个叫做"messageSource"的BeanDefinition，那么就会把这个Bean对象创建出来并赋值给ApplicationContext的messageSource属性，让ApplicationContext拥有**国际化**的功能
14. initApplicationEventMulticaster()：如果BeanFactory中存在一个叫做"**applicationEventMulticaster"**的BeanDefinition，那么就会把这个Bean对象创建出来并赋值给ApplicationContext的applicationEventMulticaster属性，让ApplicationContext拥有**事件发布**的功能
15. onRefresh()：提供给AbstractApplicationContext的子类进行扩展，没用
16. registerListeners()：从BeanFactory中获取ApplicationListener类型的beanName，然后添加到ApplicationContext中的事件广播器**applicationEventMulticaster中**去，到这一步因为FactoryBean还没有调用getObject()方法生成Bean对象，所以这里要在根据类型找一下ApplicationListener，记录一下对应的beanName
17. finishBeanFactoryInitialization(beanFactory)：完成BeanFactory的初始化，主要就是**实例化非懒加载的单例Bean**，单独的笔记去讲。
18. finishRefresh()：BeanFactory的初始化完后，就到了Spring启动的最后一步了

        a. 设置ApplicationContext的lifecycleProcessor，默认情况下设置的是DefaultLifecycleProcessor
        b. 调用lifecycleProcessor的onRefresh()方法，如果是DefaultLifecycleProcessor，那么会获取所有类型为Lifecycle的Bean对象，然后调用它的start()方法，这就是ApplicationContext的生命周期扩展机制
        c. 发布ContextRefreshedEvent事件


### 执行BeanFactoryPostProcessor

1. 执行通过ApplicationContext添加进来的BeanDefinitionRegistryPostProcessor的postProcessBeanDefinitionRegistry()方法
2. 执行BeanFactory中实现了PriorityOrdered接口的BeanDefinitionRegistryPostProcessor的postProcessBeanDefinitionRegistry()方法
3. 执行BeanFactory中实现了Ordered接口的BeanDefinitionRegistryPostProcessor的postProcessBeanDefinitionRegistry()方法
4. 执行BeanFactory中其他的BeanDefinitionRegistryPostProcessor的postProcessBeanDefinitionRegistry()方法
5. 执行上面所有的BeanDefinitionRegistryPostProcessor的postProcessBeanFactory()方法
6. 执行通过ApplicationContext添加进来的BeanFactoryPostProcessor的postProcessBeanFactory()方法
7. 执行BeanFactory中实现了PriorityOrdered接口的BeanFactoryPostProcessor的postProcessBeanFactory()方法
8. 执行BeanFactory中实现了Ordered接口的BeanFactoryPostProcessor的postProcessBeanFactory()方法
9. 执行BeanFactory中其他的BeanFactoryPostProcessor的postProcessBeanFactory()方法


### Lifecycle的使用

Lifecycle表示的是ApplicationContext的生命周期，可以定义一个SmartLifecycle来监听ApplicationContext的启动和关闭：

```java
@Component
public class ZhouyuLifecycle implements SmartLifecycle {

	private boolean isRunning = false;

	@Override
	public void start() {
		System.out.println("启动");
		isRunning = true;
	}

	@Override
	public void stop() {
        // 要触发stop()，要调用context.close()，或者注册关闭钩子（context.registerShutdownHook();）
		System.out.println("停止");
		isRunning = false;
	}

	@Override
	public boolean isRunning() {
		return isRunning;
	}
}
```
## 11、Spring之配置类解析与扫描过程源码解析

### 解析配置类

解析配置类流程图：

https://www.processon.com/view/link/5f9512d5e401fd06fda0b2dd

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-23-10-20-36.png width='100%'/></div><br/>

解析配置类思维脑图：

https://www.processon.com/view/link/614c83cae0b34d7b342f6d14

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-27-10-01-53.png width='100%'/></div><br/>

1. 在启动Spring时，需要传入一个AppConfig.class给ApplicationContext，ApplicationContext会根据AppConfig类封装为一个BeanDefinition，这种BeanDefinition我们把它称为配置类BeanDefinition。
2. ConfigurationClassPostProcessor中会把配置类BeanDefinition取出来
3. 构造一个ConfigurationClassParser用来解析配置类BeanDefinition，并且会生成一个配置类对象ConfigurationClass
4. 如果配置类上存在@Component注解，那么**解析配置类中的内部类（这里有递归，如果内部类也是配置类的话）**
5. 如果配置类上存在@PropertySource注解，那么则解析该注解，并得到PropertySource对象，并添加到environment中去
6. 如果配置类上存在@ComponentScan注解，那么则解析该注解，进行扫描，扫描得到一系列的BeanDefinition对象，然后判断这些BeanDefinition是不是也是配置类BeanDefinition（只要存在@Component注解就是配置类，所以基本上扫描出来的都是配置类），如果是则继续解析该配置类，（**也有递归**），并且会生成对应的ConfigurationClass
7. 如果配置类上存在@Import注解，那么则判断Import的类的类型：

		a.如果是ImportSelector，那么调用执行selectImports方法得到类名，然后在把这个类当做配置类进行解析（也是递归）
		b.如果是ImportBeanDefinitionRegistrar，那么则生成一个ImportBeanDefinitionRegistrar实例对象，并添加到配置类对象中（ConfigurationClass）的importBeanDefinitionRegistrars属性中。

8.  如果配置类上存在@ImportResource注解，那么则把导入进来的资源路径存在配置类对象中的**importedResources**属性中。
9. 如果配置类中存在@Bean的方法，那么则把这些方法封装为BeanMethod对象，并添加到配置类对象中的**beanMethods**属性中。
10. 如果配置类实现了某些接口，则看这些接口内是否定义了@Bean的默认方法
11. 如果配置类有父类，则把父类当做配置类进行解析
12. AppConfig这个配置类会对应一个ConfigurationClass，同时在解析的过程中也会生成另外的一些ConfigurationClass，接下来就利用reader来进一步解析ConfigurationClass

		a. 如果ConfigurationClass是通过@Import注解导入进来的，则把这个类生成一个BeanDefinition，同时解析这个类上@Scope,@Lazy等注解信息，并注册BeanDefinition
		b. 如果ConfigurationClass中存在一些BeanMethod，也就是定义了一些@Bean，那么则解析这些@Bean，并生成对应的BeanDefinition，并注册
		c. 如果ConfigurationClass中导入了一些资源文件，比如xx.xml，那么则解析这些xx.xml文件，得到并注册BeanDefinition
		d. 如果ConfigurationClass中导入了一些ImportBeanDefinitionRegistrar，那么则执行对应的registerBeanDefinitions进行BeanDefinition的注册

#### 解析配置类--总结一下 

1. 解析AppConfig类，生成对应的ConfigurationClass
2. 再扫描，扫描到的类都会生成对应的BeanDefinition，并且同时这些类也是ConfigurationClass
3. 再解析ConfigurationClass的其他信息，比如@ImportResource注解的处理，@Import注解的处理，@Bean注解的处理
## 12、Spring之整合Mybatis底层源码解析


### 整合核心思路


由很多框架都需要和Spring进行整合，而整合的核心思想就是把其他框架所产生的对象放到Spring容器中，让其成为Bean。
​

比如Mybatis，Mybatis框架可以单独使用，而单独使用Mybatis框架就需要用到Mybatis所提供的一些类构造出对应的对象，然后使用该对象，就能使用到Mybatis框架给我们提供的功能，和Mybatis整合Spring就是为了将这些对象放入Spring容器中成为Bean，只要成为了Bean，在我们的Spring项目中就能很方便的使用这些对象了，也就能很方便的使用Mybatis框架所提供的功能了。


### Mybatis-Spring 1.3.2版本底层源码执行流程

1. 通过@MapperScan导入了MapperScannerRegistrar类
2. MapperScannerRegistrar类实现了ImportBeanDefinitionRegistrar接口，所以Spring在启动时会调用MapperScannerRegistrar类中的registerBeanDefinitions方法
3. 在registerBeanDefinitions方法中定义了一个ClassPathMapperScanner对象，用来扫描mapper
4. 设置ClassPathMapperScanner对象可以扫描到接口，因为在Spring中是不会扫描接口的
5. 同时因为ClassPathMapperScanner中重写了isCandidateComponent方法，导致isCandidateComponent只会认为接口是备选者Component
6. 通过利用Spring的扫描后，会把接口扫描出来并且得到对应的BeanDefinition
7. 接下来把扫描得到的BeanDefinition进行修改，把BeanClass修改为MapperFactoryBean，把AutowireMode修改为byType
8. 扫描完成后，Spring就会基于BeanDefinition去创建Bean了，相当于每个Mapper对应一个FactoryBean（单例的）
9. 在MapperFactoryBean中的getObject方法中，调用了getSqlSession()去得到一个sqlSession对象，然后根据对应的Mapper接口生成一个代理对象
10. sqlSession对象是Mybatis中的，一个sqlSession对象需要SqlSessionFactory来产生
11. MapperFactoryBean的AutowireMode为byType，所以Spring会自动调用set方法，有两个set方法，一个setSqlSessionFactory，一个setSqlSessionTemplate，而这两个方法执行的前提是根据方法参数类型能找到对应的bean，所以Spring容器中要存在SqlSessionFactory类型的bean或者SqlSessionTemplate类型的bean。
12. 如果你定义的是一个SqlSessionFactory类型的bean，那么最终也会被包装为一个SqlSessionTemplate对象，并且赋值给sqlSession属性
13. 而在SqlSessionTemplate类中就存在一个getMapper方法，这个方法中就会利用SqlSessionFactory来生成一个代理对象
14. 到时候，当执行该代理对象的某个方法时，就会进入到Mybatis框架的底层执行流程，详细的请看下图

Spring整合Mybatis之后SQL执行流程：

[https://www.processon.com/view/link/6152cc385653bb6791db436c](https://www.processon.com/view/link/6152cc385653bb6791db436c)

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-10-18-23-50-07.png width='100%'/></div><br/>

### Mybatis-Spring  2.0.6版本(最新版)底层源码执行流程

1. 通过@MapperScan导入了MapperScannerRegistrar类
1. MapperScannerRegistrar类实现了ImportBeanDefinitionRegistrar接口，所以Spring在启动时会调用MapperScannerRegistrar类中的registerBeanDefinitions方法
1. **在registerBeanDefinitions方法中注册一个MapperScannerConfigurer类型的BeanDefinition**
1. 而MapperScannerConfigurer实现了BeanDefinitionRegistryPostProcessor接口，所以Spring在启动过程中时会调用它的postProcessBeanDefinitionRegistry()方法
1. 在postProcessBeanDefinitionRegistry方法中会生成一个ClassPathMapperScanner对象，然后进行扫描
1. 后续的逻辑和1.3.2版本一样。

带来的好处是，可以不使用@MapperScan注解，而可以直接定义一个Bean，比如：

```java
@Bean
public MapperScannerConfigurer mapperScannerConfigurer() {
	MapperScannerConfigurer mapperScannerConfigurer = new MapperScannerConfigurer();
	mapperScannerConfigurer.setBasePackage("com.luban");
	return mapperScannerConfigurer;
}
```
### Spring整合Mybatis后一级缓存失效问题


先看下图：
Spring整合Mybatis之后SQL执行流程：

[https://www.processon.com/view/link/6152cc385653bb6791db436c](https://www.processon.com/view/link/6152cc385653bb6791db436c)
​

Mybatis中的一级缓存是基于SqlSession来实现的，所以在执行同一个sql时，如果使用的是同一个SqlSession对象，那么就能利用到一级缓存，提高sql的执行效率。

但是在Spring整合Mybatis后，如果没有执行某个方法时，该方法上没有加@Transactional注解，也就是没有开启Spring事务，那么后面在执行具体sql时，没执行一个sql时都会新生成一个SqlSession对象来执行该sql，这就是我们说的一级缓存失效（也就是没有使用同一个SqlSession对象），而如果开启了Spring事务，那么该Spring事务中的多个sql，在执行时会使用同一个SqlSession对象，从而一级缓存生效，具体的底层执行流程在上图。
​
个人理解：实际上Spring整合Mybatis后一级缓存失效并**不是问题**，是正常的实现，因为，一个方法如果没有开启Spring事务，那么在执行sql时候，那就是每个sql单独一个事务来执行，也就是单独一个SqlSession对象来执行该sql，如果开启了Spring事务，那就是多个sql属于同一个事务，那自然就应该用一个SqlSession来执行这多个sql。所以，在没有开启Spring事务的时候，SqlSession的一级缓存并不是**失效**了，而是存在的生命周期太短了（执行完一个sql后就被销毁了，下一个sql执行时又是一个新的SqlSession了）。
​

## 13、Spring之AOP底层源码解析（上）

有道云链接：http://note.youdao.com/noteshare?id=f30e818e00f2c3eb6d4f26e6c0b70ade&sub=854D9B3F17A64B1DA9F965B2448E9EA6

### 动态代理
代理模式的解释：为**其他对象**提供一种**代理**以控制对这个对象的访问，增强一个类中的某个方法，对程序进行扩展。


比如，现在存在一个UserService类：
```java
public class UserService  {

	public void test() {
		System.out.println("test...");
	}

}
```
此时，我们new一个UserService对象，然后执行test()方法，结果是显而易见的。


如果我们现在想在**不修改UserService类的源码**前提下，给test()增加额外逻辑，那么就可以使用动态代理机制来创建UserService对象了，比如：
```java
UserService target = new UserService();

// 通过cglib技术
Enhancer enhancer = new Enhancer();
enhancer.setSuperclass(UserService.class);

// 定义额外逻辑，也就是代理逻辑
enhancer.setCallbacks(new Callback[]{new MethodInterceptor() {
	@Override
	public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
		System.out.println("before...");
		Object result = methodProxy.invoke(target, objects);
		System.out.println("after...");
		return result;
	}
}});

// 动态代理所创建出来的UserService对象
UserService userService = (UserService) enhancer.create();

// 执行这个userService的test方法时，就会额外会执行一些其他逻辑
userService.test();
```
得到的都是UserService对象，但是执行test()方法时的效果却不一样了，这就是代理所带来的效果。

上面是通过cglib来实现的代理对象的创建，是基于**父子类**的，被代理类（UserService）是父类，代理类是子类，代理对象就是代理类的实例对象，代理类是由cglib创建的，对于程序员来说不用关心。
​

除开cglib技术，jdk本身也提供了一种创建代理对象的动态代理机制，但是它只能代理接口，也就是UserService得先有一个接口才能利用jdk动态代理机制来生成一个代理对象，比如：
```java
public interface UserInterface {
	public void test();
}

public class UserService implements UserInterface {

	public void test() {
		System.out.println("test...");
	}

}
```

利用JDK动态代理来生成一个代理对象：

```java
UserService target = new UserService();

// UserInterface接口的代理对象
Object proxy = Proxy.newProxyInstance(UserService.class.getClassLoader(), new Class[]{UserInterface.class}, new InvocationHandler() {
	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
		System.out.println("before...");
		Object result = method.invoke(target, args);
		System.out.println("after...");
		return result;
	}
});

UserInterface userService = (UserInterface) proxy;
userService.test();
```
如果你把new Class[]{UserInterface.class}，替换成new Class[]{UserService.class}，允许代码会直接报错：
```java
Exception in thread "main" java.lang.IllegalArgumentException: com.zhouyu.service.UserService is not an interface
```
表示一定要是个接口。
​

由于这个限制，所以产生的代理对象的类型是UserInterface，而不是UserService，这是需要注意的。


### ProxyFactory
上面我们介绍了两种动态代理技术，那么在Spring中进行了封装，封装出来的类叫做ProxyFactory，表示是创建代理对象的一个工厂，使用起来会比上面的更加方便，比如：
```java
UserService target = new UserService();

ProxyFactory proxyFactory = new ProxyFactory();
proxyFactory.setTarget(target);
proxyFactory.addAdvice(new MethodInterceptor() {
	@Override
	public Object invoke(MethodInvocation invocation) throws Throwable {
		System.out.println("before...");
		Object result = invocation.proceed();
		System.out.println("after...");
		return result;
	}
});

UserInterface userService = (UserInterface) proxyFactory.getProxy();
userService.test();
```
通过ProxyFactory，我们可以不再关系到底是用cglib还是jdk动态代理了，ProxyFactory会帮我们去判断，如果UserService实现了接口，那么ProxyFactory底层就会用jdk动态代理，如果没有实现接口，就会用cglib技术，上面的代码，就是由于UserService实现了UserInterface接口，所以最后产生的代理对象是UserInterface类型。


### Advice的分类

1. Before Advice：方法之前执行
1. After returning advice：方法return后执行
1. After throwing advice：方法抛异常后执行
1. After (finally) advice：方法执行完finally之后执行，这是最后的，比return更后
1. Around advice：这是功能最强大的Advice，可以自定义执行顺序

​

看课上给的代码例子将一目了然
​

### Advisor的理解
跟Advice类似的还有一个Advisor的概念，一个Advisor是有一个Pointcut和一个Advice组成的，通过Pointcut可以指定要需要被代理的逻辑，比如一个UserService类中有两个方法，按上面的例子，这两个方法都会被代理，被增强，那么我们现在可以通过Advisor，来控制到具体代理哪一个方法，比如：

```java
		UserService target = new UserService();

		ProxyFactory proxyFactory = new ProxyFactory();
		proxyFactory.setTarget(target);
		proxyFactory.addAdvisor(new PointcutAdvisor() {
			@Override
			public Pointcut getPointcut() {
				return new StaticMethodMatcherPointcut() {
					@Override
					public boolean matches(Method method, Class<?> targetClass) {
						return method.getName().equals("testAbc");
					}
				};
			}

			@Override
			public Advice getAdvice() {
				return new MethodInterceptor() {
					@Override
					public Object invoke(MethodInvocation invocation) throws Throwable {
						System.out.println("before...");
						Object result = invocation.proceed();
						System.out.println("after...");
						return result;
					}
				};
			}

			@Override
			public boolean isPerInstance() {
				return false;
			}
		});

		UserInterface userService = (UserInterface) proxyFactory.getProxy();
		userService.test();
```
上面代码表示，产生的代理对象，只有在执行testAbc这个方法时才会被增强，会执行额外的逻辑，而在执行其他方法时是不会增强的。
​

### 创建代理对象的方式


上面介绍了Spring中所提供了ProxyFactory、Advisor、Advice、PointCut等技术来实现代理对象的创建，但是我们在使用Spring时，我们并不会直接这么去使用ProxyFactory，比如说，我们希望ProxyFactory所产生的代理对象能直接就是Bean，能直接从Spring容器中得到UserSerivce的代理对象，而这些，Spring都是支持的，只不过，作为开发者的我们肯定得告诉Spring，那些类需要被代理，代理逻辑是什么。


#### ProxyFactoryBean
```java
@Bean
public ProxyFactoryBean userServiceProxy(){
	UserService userService = new UserService();

	ProxyFactoryBean proxyFactoryBean = new ProxyFactoryBean();
	proxyFactoryBean.setTarget(userService);
	proxyFactoryBean.addAdvice(new MethodInterceptor() {
		@Override
		public Object invoke(MethodInvocation invocation) throws Throwable {
			System.out.println("before...");
			Object result = invocation.proceed();
			System.out.println("after...");
			return result;
		}
	});
	return proxyFactoryBean;
}

```
通过这种方法来定义一个UserService的Bean，并且是经过了AOP的。但是这种方式**只能针对某一个Bean**。它是一个FactoryBean，所以利用的就是FactoryBean技术，间接的将UserService的代理对象作为了Bean。
​

ProxyFactoryBean还有额外的功能，比如可以把某个Advise或Advisor定义成为Bean，然后在ProxyFactoryBean中进行设置
```java
@Bean
public MethodInterceptor zhouyuAroundAdvise(){
	return new MethodInterceptor() {
		@Override
		public Object invoke(MethodInvocation invocation) throws Throwable {
			System.out.println("before...");
			Object result = invocation.proceed();
			System.out.println("after...");
			return result;
		}
	};
}

@Bean
public ProxyFactoryBean userService(){
	UserService userService = new UserService();
	
    ProxyFactoryBean proxyFactoryBean = new ProxyFactoryBean();
	proxyFactoryBean.setTarget(userService);
	proxyFactoryBean.setInterceptorNames("zhouyuAroundAdvise");
	return proxyFactoryBean;
}
```
​

#### BeanNameAutoProxyCreator


ProxyFactoryBean得自己指定被代理的对象，那么我们可以通过BeanNameAutoProxyCreator来通过指定某个bean的名字，来对该bean进行代理
```java
@Bean
public BeanNameAutoProxyCreator beanNameAutoProxyCreator() {
	BeanNameAutoProxyCreator beanNameAutoProxyCreator = new BeanNameAutoProxyCreator();
	beanNameAutoProxyCreator.setBeanNames("userSe*");
	beanNameAutoProxyCreator.setInterceptorNames("zhouyuAroundAdvise");
	beanNameAutoProxyCreator.setProxyTargetClass(true);

    return beanNameAutoProxyCreator;
}
```
通过BeanNameAutoProxyCreator可以对批量的Bean进行AOP，并且指定了代理逻辑，指定了一个InterceptorName，也就是一个Advise，前提条件是这个Advise也得是一个Bean，这样Spring才能找到的，但是BeanNameAutoProxyCreator的缺点很明显，它只能根据beanName来指定想要代理的Bean。
​

#### DefaultAdvisorAutoProxyCreator
```java
@Bean
public DefaultPointcutAdvisor defaultPointcutAdvisor(){
	NameMatchMethodPointcut pointcut = new NameMatchMethodPointcut();
	pointcut.addMethodName("test");

    DefaultPointcutAdvisor defaultPointcutAdvisor = new DefaultPointcutAdvisor();
	defaultPointcutAdvisor.setPointcut(pointcut);
	defaultPointcutAdvisor.setAdvice(new ZhouyuAfterReturningAdvise());

    return defaultPointcutAdvisor;
}

@Bean
public DefaultAdvisorAutoProxyCreator defaultAdvisorAutoProxyCreator() {
	
    DefaultAdvisorAutoProxyCreator defaultAdvisorAutoProxyCreator = new DefaultAdvisorAutoProxyCreator();

	return defaultAdvisorAutoProxyCreator;
}
```
通过DefaultAdvisorAutoProxyCreator会直接去找所有Advisor类型的Bean，根据Advisor中的PointCut和Advice信息，确定要代理的Bean以及代理逻辑。


但是，我们发现，通过这种方式，我们得依靠某一个类来实现定义我们的Advisor，或者Advise，或者Pointcut，那么这个步骤能不能更加简化一点呢？
​

对的，通过**注解**！


比如我们能不能只定义一个类，然后通过在类中的方法上通过某些注解，来定义PointCut以及Advice，可以的，比如：
```java
@Aspect
@Component
public class ZhouyuAspect {

	@Before("execution(public void com.zhouyu.service.UserService.test())")
	public void zhouyuBefore(JoinPoint joinPoint) {
		System.out.println("zhouyuBefore");
	}

}
```


通过上面这个类，我们就直接定义好了所要代理的方法(通过一个表达式)，以及代理逻辑（被@Before修饰的方法），简单明了，这样对于Spring来说，它要做的就是来解析这些注解了，解析之后得到对应的Pointcut对象、Advice对象，生成Advisor对象，扔进ProxyFactory中，进而产生对应的代理对象，具体怎么解析这些注解就是**@EnableAspectJAutoProxy注解**所要做的事情了，后面详细分析。


### 对Spring AOP的理解
OOP表示面向对象编程，是一种编程思想，AOP表示面向切面编程，也是一种编程思想，而我们上面所描述的就是Spring为了让程序员更加方便的做到面向切面编程所提供的技术支持，换句话说，就是Spring提供了一套机制，可以让我们更加容易的来进行AOP，所以这套机制我们也可以称之为Spring AOP。
​

但是值得注意的是，上面所提供的注解的方式来定义Pointcut和Advice，Spring并不是首创，首创是AspectJ，而且也不仅仅只有Spring提供了一套机制来支持AOP，还有比如 JBoss 4.0、aspectwerkz等技术都提供了对于AOP的支持。而刚刚说的注解的方式，Spring是依赖了AspectJ的，或者说，Spring是直接把AspectJ中所定义的那些注解直接拿过来用，自己没有再重复定义了，不过也仅仅只是把注解的定义赋值过来了，每个注解具体底层是怎么解析的，还是Spring自己做的，所以我们在用Spring时，如果你想用@Before、@Around等注解，是需要单独引入aspecj相关jar包的，比如：
```java
compile group: 'org.aspectj', name: 'aspectjrt', version: '1.9.5'
compile group: 'org.aspectj', name: 'aspectjweaver', version: '1.9.5'
```


值得注意的是：AspectJ是在编译时对字节码进行了修改，是直接在UserService类对应的字节码中进行增强的，也就是可以理解为是在编译时就会去解析@Before这些注解，然后得到代理逻辑，加入到被代理的类中的字节码中去的，所以如果想用AspectJ技术来生成代理对象 ，是需要用单独的AspectJ编译器的。我们在项目中很少这么用，我们仅仅只是用了@Before这些注解，而我们在启动Spring的过程中，Spring会去解析这些注解，然后利用动态代理机制生成代理对象的。
​

IDEA中使用Aspectj：[https://blog.csdn.net/gavin_john/article/details/80156963](https://blog.csdn.net/gavin_john/article/details/80156963)


### AOP中的概念
上面我们已经提到Advisor、Advice、PointCut等概念了，还有一些其他的概念，首先关于AOP中的概念本身是比较难理解的，Spring官网上是这么说的：
> Let us begin by defining some central AOP concepts and terminology. These terms are not Spring-specific. Unfortunately, AOP terminology is not particularly intuitive. However, it would be even more confusing if Spring used its own terminology

意思是，AOP中的这些概念不是Spring特有的，不幸的是，AOP中的概念不是特别直观的，但是，如果Spring重新定义自己的那可能会导致更加混乱

1. Aspect：表示切面，比如被@Aspect注解的类就是切面，可以在切面中去定义Pointcut、Advice等等
1. Join point：表示连接点，表示一个程序在执行过程中的一个点，比如一个方法的执行，比如一个异常的处理，在Spring AOP中，一个连接点通常表示一个方法的执行。
1. Advice：表示通知，表示在一个特定连接点上所采取的动作。Advice分为不同的类型，后面详细讨论，在很多AOP框架中，包括Spring，会用Interceptor拦截器来实现Advice，并且在连接点周围维护一个Interceptor链
1. Pointcut：表示切点，用来匹配一个或多个连接点，Advice与切点表达式是关联在一起的，Advice将会执行在和切点表达式所匹配的连接点上
1. Introduction：可以使用@DeclareParents来给所匹配的类添加一个接口，并指定一个默认实现
1. Target object：目标对象，被代理对象
1. AOP proxy：表示代理工厂，用来创建代理对象的，在Spring Framework中，要么是JDK动态代理，要么是CGLIB代理
1. Weaving：表示织入，表示创建代理对象的动作，这个动作可以发生在编译时期（比如Aspejctj），或者运行时，比如Spring AOP


### Advice在Spring AOP中对应API
上面说到的Aspject中的注解，其中有五个是用来定义Advice的，表示代理逻辑，以及执行时机：

1. @Before
1. @AfterReturning
1. @AfterThrowing
1. @After
1. @Around

我们前面也提到过，Spring自己也提供了类似的执行实际的实现类：

1. 接口MethodBeforeAdvice，继承了接口BeforeAdvice
1. 接口AfterReturningAdvice
1. 接口ThrowsAdvice
1. 接口AfterAdvice
1. 接口MethodInterceptor

Spring会把五个注解解析为对应的Advice类：

1. @Before：AspectJMethodBeforeAdvice，实际上就是一个MethodBeforeAdvice
1. @AfterReturning：AspectJAfterReturningAdvice，实际上就是一个AfterReturningAdvice
1. @AfterThrowing：AspectJAfterThrowingAdvice，实际上就是一个MethodInterceptor
1. @After：AspectJAfterAdvice，实际上就是一个MethodInterceptor
1. @Around：AspectJAroundAdvice，实际上就是一个MethodInterceptor

### TargetSource的使用

在我们日常的AOP中，被代理对象就是Bean对象，是由BeanFactory给我们创建出来的，但是Spring AOP中提供了TargetSource机制，可以让我们用来自定义逻辑来创建**被代理对象**。
​
比如之前所提到的@Lazy注解，当加在属性上时，会产生一个代理对象赋值给这个属性，产生代理对象的代码为：
```java
protected Object buildLazyResolutionProxy(final DependencyDescriptor descriptor, final @Nullable String beanName) {
		BeanFactory beanFactory = getBeanFactory();
		Assert.state(beanFactory instanceof DefaultListableBeanFactory,
				"BeanFactory needs to be a DefaultListableBeanFactory");
		final DefaultListableBeanFactory dlbf = (DefaultListableBeanFactory) beanFactory;

		TargetSource ts = new TargetSource() {
			@Override
			public Class<?> getTargetClass() {
				return descriptor.getDependencyType();
			}
			@Override
			public boolean isStatic() {
				return false;
			}
			@Override
			public Object getTarget() {
				Set<String> autowiredBeanNames = (beanName != null ? new LinkedHashSet<>(1) : null);
				Object target = dlbf.doResolveDependency(descriptor, beanName, autowiredBeanNames, null);
				if (target == null) {
					Class<?> type = getTargetClass();
					if (Map.class == type) {
						return Collections.emptyMap();
					}
					else if (List.class == type) {
						return Collections.emptyList();
					}
					else if (Set.class == type || Collection.class == type) {
						return Collections.emptySet();
					}
					throw new NoSuchBeanDefinitionException(descriptor.getResolvableType(),
							"Optional dependency not present for lazy injection point");
				}
				if (autowiredBeanNames != null) {
					for (String autowiredBeanName : autowiredBeanNames) {
						if (dlbf.containsBean(autowiredBeanName)) {
							dlbf.registerDependentBean(autowiredBeanName, beanName);
						}
					}
				}
				return target;
			}
			@Override
			public void releaseTarget(Object target) {
			}
		};

		ProxyFactory pf = new ProxyFactory();
		pf.setTargetSource(ts);
		Class<?> dependencyType = descriptor.getDependencyType();
		if (dependencyType.isInterface()) {
			pf.addInterface(dependencyType);
		}
		return pf.getProxy(dlbf.getBeanClassLoader());
	}
```
这段代码就利用了ProxyFactory来生成代理对象，以及使用了TargetSource，以达到代理对象在执行某个方法时，调用TargetSource的getTarget()方法实时得到一个**被代理对象**。

### Introduction
[https://www.cnblogs.com/powerwu/articles/5170861.html](https://www.cnblogs.com/powerwu/articles/5170861.html)
​
### LoadTimeWeaver
[https://www.cnblogs.com/davidwang456/p/5633609.html](https://www.cnblogs.com/davidwang456/p/5633609.html)


----------------------------------- 以下为老版笔记 ----------------------------------------
### 对Spring AOP的理解(老版笔记)

OOP表示面向对象编程，是一种思想，没有说明到底该如何实现。
AOP表示面向切面编程，也是一种思想，也没有说明到底该如何实现，也没有说明使用什么技术来实现。

不过随着业界的不断发展，对于AOP的实现抽象出来了几个概念，形成了规范，这几个概念分别是：
1. Aspect：表示切面
2. Joint point：表示连接点
3. Pointcut：表示切点
4. Advice：表示通知
5. 等等...，后面详细讲

而目前也有一些项目或框架真正实现了AOP，比如：
1. AspectJ 
2. JBoss 4.0
3. aspectwerkz
4. spring

这些都实现了AOP，不过实现的最完整的当属AspectJ。并且这些项目和框架实现AOP的底层技术是不一样的：
1. AspectJ 是在编译时修改字节码，所以如果用AspectJ ，需要单独的编译器
2. JBoss 4.0是利用的JDK动态代理
3. aspectwerkz和spring用的是运行是修改字节码，比如Spring中用的cglib技术（当然Spring中也使用了JDK动态代理）

对于Spring而言，它并没有实现全部的AOP功能，但是已经实现了大部分了（对于企业而言已经足够用了），同时Spring也利用了AspectJ中关于Advice、Pointcut等概念的相关源码实现，比如Advice接口，@Pointcut注解等，这些都是AspectJ中所提供的，Spring直接拿来用了，但是对于这些接口和注解的底层实现还是有Spring自己实现的，负责实现的模块就叫做Spring AOP。

所以Spring AOP和AspectJ并不是竞争关系，而且Spring AOP除开利用动态代理实现了AOP之外，还有一个目的就是将AOP和Spring IOC结合起来，这是Spring AOP另外的一个目的。

### AOP中的概念

首先关于AOP中的概念本身是比较难理解的，Spring官网上是这么说的：

```java
Let us begin by defining some central AOP concepts and terminology. These terms are not Spring-specific. Unfortunately, AOP terminology is not particularly intuitive. However, it would be even more confusing if Spring used its own terminology
```
意思是，AOP中的这些概念不是Spring特有的，不幸的是，AOP中的概念不是特别直观的，但是，如果Spring重新定义自己的那可能会导致更加混乱

1. Aspect：表示切面，比如被@Aspect注解的类就是切面，可以在切面中去定义Pointcut、Advice等等
2. Join point：表示连接点，表示一个程序在执行过程中的一个点，比如一个方法的执行，比如一个异常的处理，在Spring AOP中，一个连接点通常表示一个方法的执行。
3. Advice：表示通知，表示在一个特定连接点上所采取的动作。Advice分为不同的类型，后面详细讨论，在很多AOP框架中，包括Spring，会用Interceptor拦截器来实现Advice，并且在连接点周围维护一个Interceptor链
4. Pointcut：表示切点，用来匹配一个或多个连接点，Advice与切点表达式是关联在一起的，Advice将会执行在和切点表达式所匹配的连接点上
5. Introduction：可以使用@DeclareParents来给所匹配的类添加一个接口，并指定一个默认实现
6. Target object：表示被嵌入了一个或多个切面的对象，也叫做“advised object”
7. AOP proxy：表示代理工厂，用来创建代理对象的，在Spring Framework中，要么是JDK动态代理，要么是CGLIB代理
8. Weaving：表示织入，表示链接切面和应用程序的类或对象来创建一个代理对象，这个动作可以发生在编译时期（比如Aspejctj），或者运行时，比如Spring AOP
### Advice的分类
1. Before Advice：方法之前执行
2. After returning advice：方法return后执行
3. After throwing advice：方法抛异常后执行
4. After (finally) advice：方法执行完finally之后执行，这是最后的，比return更后
5. Around advice：这是功能最强大的Advice，可以自定义执行顺序

看课上给的代码例子将一目了然
### Advice在Spring AOP中对应API

上面介绍的5种Advice类型，在Aspectj和Spring AOP都有对应的使用API

比如，在AspectJ中就可以通过 以下五个注解来进行对应的使用：
1. @Before
2. @AfterReturning
3. @AfterThrowing
4. @After
5. @Around

而Spring AOP也支持这5个注解，不过Spring AOP仅仅只是支持这5个注解，底层实现完全是有自己实现的（动态代理），没有用到Aspectj的底层实现（编译期），
同时Spring AOP也提供对于这5种Advice的API实现，分别时：
1. 接口MethodBeforeAdvice，继承了接口BeforeAdvice
2. 接口AfterReturningAdvice
3. 接口ThrowsAdvice
4. 接口AfterAdvice
5. 接口MethodInterceptor

这六个接口的关系如下:

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-23-10-54-14.png width='100%'/></div><br/>

有两这5个接口，我们在使用Spring AOP时，可以直接通过实现这个几个接口来提供对应的不同类型的Advice，但是其中AfterAdvice是不能用的，它仅仅只是一个标记接口，如果要实现在finally之后进行切面逻辑，那么就得用MethodInterceptor来实现。

需要注意的是，我们现在看到的都是接口，而上面我们也说了Spring AOP中对Aspectj中的@Before等注解也是实现了，各个注解所对应的实现类为：
1. @Before：AspectJMethodBeforeAdvice，利用的是MethodBeforeAdvice，跟MethodInterceptor没关系
2. @AfterReturning：AspectJAfterReturningAdvice，利用的是AfterReturningAdvice，跟MethodInterceptor没关系
3. @AfterThrowing：AspectJAfterThrowingAdvice，利用的是MethodInterceptor，不过也实现了AfterAdvice这个标记接口
4. @After：AspectJAfterAdvice，利用的是MethodInterceptor，不过也实现了AfterAdvice这个标记接口
5. @Around：AspectJAroundAdvice，利用的是MethodInterceptor

其中@After和@AfterThrowing对应的实现也是采用了MethodInterceptor来实现的。
### Spring AOP中的Advisor

Advisor这个概念是Spring AOP中所特有的，是在Aspectj中不存在的，一个Advisor相当于一个小型的切面，特点在于它只有一个单一的Advice和所关联的一个切点，DefaultPointcutAdvisor是最为通用的Advisor实现类。

### 创建代理对象的方式
#### ProxyFactoryBean

```java
@Bean
public ProxyFactoryBean userService(){
	UserService userService = new UserService();
	
    ProxyFactoryBean proxyFactoryBean = new ProxyFactoryBean();
	proxyFactoryBean.setProxyTargetClass(true);
	proxyFactoryBean.setTarget(userService);
	proxyFactoryBean.setInterceptorNames("zhouyuAroundAdvise");
	return proxyFactoryBean;
}
```
通过这种方法来定义一个UserService的Bean，并且是经过了AOP的。但是这种方式只能针对某一个Bean。
#### BeanNameAutoProxyCreator

```java
@Bean
public BeanNameAutoProxyCreator beanNameAutoProxyCreator() {
	BeanNameAutoProxyCreator beanNameAutoProxyCreator = new BeanNameAutoProxyCreator();
	beanNameAutoProxyCreator.setBeanNames("userSe*");
	beanNameAutoProxyCreator.setInterceptorNames("zhouyuAroundAdvise");
	beanNameAutoProxyCreator.setProxyTargetClass(true);

    return beanNameAutoProxyCreator;
}
```
通过BeanNameAutoProxyCreator可以对批量的Bean进行AOP
#### DefaultAdvisorAutoProxyCreatorDemo

```java
@Bean
public DefaultPointcutAdvisor defaultPointcutAdvisor(){
	NameMatchMethodPointcut pointcut = new NameMatchMethodPointcut();
	pointcut.addMethodName("test");

    DefaultPointcutAdvisor defaultPointcutAdvisor = new DefaultPointcutAdvisor();
	defaultPointcutAdvisor.setPointcut(pointcut);
	defaultPointcutAdvisor.setAdvice(new ZhouyuAfterReturningAdvise());

    return defaultPointcutAdvisor;
}

@Bean
public DefaultAdvisorAutoProxyCreator defaultAdvisorAutoProxyCreator() {
	
    DefaultAdvisorAutoProxyCreator defaultAdvisorAutoProxyCreator = new DefaultAdvisorAutoProxyCreator();
	defaultAdvisorAutoProxyCreator.setAdvisorBeanNamePrefix("defaultPoin");
	defaultAdvisorAutoProxyCreator.setProxyTargetClass(true);

	return defaultAdvisorAutoProxyCreator;
}
```
通过DefaultAdvisorAutoProxyCreator可以直接匹配多个Advisor，而每个Advisor中可以定义切点以及Advice，所以更灵活。

### TargetSource的使用

在我们日常的AOP中，被代理对象就是Bean对象，是有BeanFactory给我们创建出来的，但是Spring AOP中提供了TargetSource机制，可以让我们用来自定义逻辑来创建被代理对象。

```java
@Bean
public CommonsPool2TargetSource targetSource(){
	CommonsPool2TargetSource targetSource = new CommonsPool2TargetSource();
	targetSource.setTargetBeanName("userServiceTarget");
	targetSource.setMaxSize(2);
	return targetSource;
}
```
比如上面用到的CommonsPool2TargetSource就是一个targetSource，它表示被代理对象是两个userServiceTarget，具体可以看课上对于这块的演示。

IDEA中使用Aspectj：https://blog.csdn.net/gavin_john/article/details/80156963

## 14、Spring之AOP底层源码解析（下）

有道云链接：http://note.youdao.com/noteshare?id=8ae9d210cbb3a9bea75d8a142152f011&sub=F6098FC783F649CE85530DDC2456085D

### ProxyFactory选择cglib或jdk动态代理原理
ProxyFactory在生成代理对象之前需要决定到底是使用JDK动态代理还是CGLIB技术：
```java
// config就是ProxyFactory对象

// optimize为true,或proxyTargetClass为true,或用户没有给ProxyFactory对象添加interface
if (config.isOptimize() || config.isProxyTargetClass() || hasNoUserSuppliedProxyInterfaces(config)) {
	Class<?> targetClass = config.getTargetClass();
	if (targetClass == null) {
		throw new AopConfigException("TargetSource cannot determine target class: " +
				"Either an interface or a target is required for proxy creation.");
	}
    // targetClass是接口，直接使用Jdk动态代理
	if (targetClass.isInterface() || Proxy.isProxyClass(targetClass)) {
		return new JdkDynamicAopProxy(config);
	}
    // 使用Cglib
	return new ObjenesisCglibAopProxy(config);
}
else {
    // 使用Jdk动态代理
	return new JdkDynamicAopProxy(config);
}
```

### 代理对象创建过程
#### JdkDynamicAopProxy

1. 在构造JdkDynamicAopProxy对象时，会先拿到被代理对象自己所实现的接口，并且额外的增加SpringProxy、Advised、DecoratingProxy三个接口，组合成一个Class[]，并赋值给proxiedInterfaces属性
1. 并且检查这些接口中是否定义了equals()、hashcode()方法
1. 执行`Proxy.newProxyInstance(classLoader, this.proxiedInterfaces, this)`，得到代理对象，**JdkDynamicAopProxy**作为InvocationHandler，代理对象在执行某个方法时，会进入到JdkDynamicAopProxy的**invoke()**方法中
#### ObjenesisCglibAopProxy

1. 创建Enhancer对象
1. 设置Enhancer的superClass为通过ProxyFactory.setTarget()所设置的对象的类
1. 设置Enhancer的interfaces为通过ProxyFactory.addInterface()所添加的接口，以及SpringProxy、Advised、DecoratingProxy接口
1. 设置Enhancer的Callbacks为DynamicAdvisedInterceptor
1. 最后创建一个代理对象，代理对象在执行某个方法时，会进入到DynamicAdvisedInterceptor的intercept()方法中

### 代理对象执行过程

1. 在使用ProxyFactory创建代理对象之前，需要往ProxyFactory先添加Advisor
1. 代理对象在执行某个方法时，会把ProxyFactory中的Advisor拿出来和当前正在执行的方法进行匹配筛选
1. 把和方法所匹配的Advisor适配成MethodInterceptor
1. 把和当前方法匹配的MethodInterceptor链，以及被代理对象、代理对象、代理类、当前Method对象、方法参数封装为MethodInvocation对象
1. 调用MethodInvocation的proceed()方法，开始执行各个MethodInterceptor以及被代理对象的对应方法
1. 按顺序调用每个MethodInterceptor的invoke()方法，并且会把MethodInvocation对象传入invoke()方法
1. 直到执行完最后一个MethodInterceptor了，就会调用invokeJoinpoint()方法，从而执行被代理对象的当前方法

##### 各注解对应的MethodInterceptor

- **@Before**对应的是AspectJMethodBeforeAdvice，在进行动态代理时会把AspectJMethodBeforeAdvice转成**MethodBeforeAdviceInterceptor**
   - 先执行advice对应的方法
   - 再执行MethodInvocation的proceed()，会执行下一个Interceptor，如果没有下一个Interceptor了，会执行target对应的方法
- **@After**对应的是AspectJAfterAdvice，直接实现了**MethodInterceptor**
   - 先执行MethodInvocation的proceed()，会执行下一个Interceptor，如果没有下一个Interceptor了，会执行target对应的方法
   - 再执行advice对应的方法
- **@Around**对应的是AspectJAroundAdvice，直接实现了**MethodInterceptor**
   - 直接执行advice对应的方法，由@Around自己决定要不要继续往后面调用
- **@AfterThrowing**对应的是AspectJAfterThrowingAdvice，直接实现了**MethodInterceptor**
   - 先执行MethodInvocation的proceed()，会执行下一个Interceptor，如果没有下一个Interceptor了，会执行target对应的方法
   - 如果上面抛了Throwable，那么则会执行advice对应的方法
- **@AfterReturning**对应的是AspectJAfterReturningAdvice，在进行动态代理时会把AspectJAfterReturningAdvice转成**AfterReturningAdviceInterceptor**
   - 先执行MethodInvocation的proceed()，会执行下一个Interceptor，如果没有下一个Interceptor了，会执行target对应的方法
   - 执行上面的方法后得到最终的方法的返回值
   - 再执行Advice对应的方法

### AbstractAdvisorAutoProxyCreator

DefaultAdvisorAutoProxyCreator的父类是AbstractAdvisorAutoProxyCreator。

**AbstractAdvisorAutoProxyCreator**非常强大以及重要，只要Spring容器中存在这个类型的Bean，就相当于开启了AOP，AbstractAdvisorAutoProxyCreator实际上就是一个BeanPostProcessor，所以在创建某个Bean时，就会进入到它对应的生命周期方法中，比如：在某个Bean**初始化之后**，会调用wrapIfNecessary()方法进行AOP，底层逻辑是，AbstractAdvisorAutoProxyCreator会找到所有的Advisor，然后判断当前这个Bean是否存在某个Advisor与之匹配（根据Pointcut），如果匹配就表示当前这个Bean有对应的切面逻辑，需要进行AOP，需要产生一个代理对象。

### @EnableAspectJAutoProxy

这个注解主要就是往Spring容器中添加了一个AnnotationAwareAspectJAutoProxyCreator类型的Bean。

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-10-19-01-03-08.png width='100%'/></div><br/>

**AspectJAwareAdvisorAutoProxyCreator**继承了**AbstractAdvisorAutoProxyCreator**，重写了findCandidateAdvisors()方法，**AbstractAdvisorAutoProxyCreator**只能找到所有Advisor类型的Bean对象，但是**AspectJAwareAdvisorAutoProxyCreator**除开可以找到所有Advisor类型的Bean对象，还能把@Aspect注解所标注的Bean中的@Before等注解及方法进行解析，并生成对应的Advisor对象。
​
所以，我们可以理解@EnableAspectJAutoProxy，其实就是像Spring容器中添加了一个AbstractAdvisorAutoProxyCreator类型的Bean，从而开启了AOP，并且还会解析@Before等注解生成Advisor。
### Spring中AOP原理流程图（和下面老版图片一致）

[https://www.processon.com/view/link/5faa4ccce0b34d7a1aa2a9a5](https://www.processon.com/view/link/5faa4ccce0b34d7a1aa2a9a5)

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-23-11-31-28.png width='100%'/></div><br/>

-----------------------------------以下是老版笔记--------------------------------------
### 使用ProxyFactory通过编程创建AOP代理（老版笔记）

定义一个MyAdvisor

```java
public class MyAdvisor implements PointcutAdvisor {

	@Override
	public Pointcut getPointcut() {
		NameMatchMethodPointcut methodPointcut = new NameMatchMethodPointcut();
		methodPointcut.addMethodName("test");
		return methodPointcut;
	}

	@Override
	public Advice getAdvice() {
		MethodBeforeAdvice methodBeforeAdvice = new MethodBeforeAdvice() {
			@Override
			public void before(Method method, Object[] args, Object target) throws Throwable {
				System.out.println("执行方法前"+method.getName());
			}
		};

		return methodBeforeAdvice;
	}

	@Override
	public boolean isPerInstance() {
		return false;
	}
}
```
定义一个UserService

```java
public class UserService {

	public void test() {
		System.out.println("111");
	}
}
```

```java
ProxyFactory factory = new ProxyFactory();
factory.setTarget(new UserService());
factory.addAdvisor(new MyAdvisor());
UserService userService = (UserService) factory.getProxy();
userService.test();
```

#### ProxyFactory的工作原理

ProxyFactory就是一个代理对象生产工厂，在生成代理对象之前需要对代理工厂进行配置。

ProxyFactory在生成代理对象之前需要决定到底是使用JDK动态代理还是CGLIB技术：

```java
// config就是ProxyFactory对象

// optimize为true,或proxyTargetClass为true,或用户没有给ProxyFactory对象添加interface
if (config.isOptimize() || config.isProxyTargetClass() || hasNoUserSuppliedProxyInterfaces(config)) {
	Class<?> targetClass = config.getTargetClass();
	if (targetClass == null) {
		throw new AopConfigException("TargetSource cannot determine target class: " +
				"Either an interface or a target is required for proxy creation.");
	}
    // targetClass是接口，直接使用Jdk动态代理
	if (targetClass.isInterface() || Proxy.isProxyClass(targetClass)) {
		return new JdkDynamicAopProxy(config);
	}
    // 使用Cglib
	return new ObjenesisCglibAopProxy(config);
}
else {
    // 使用Jdk动态代理
	return new JdkDynamicAopProxy(config);
}
```
#### JdkDynamicAopProxy创建代理对象过程

1. 获取生成代理对象所需要实现的接口集合

		a. 获取通过ProxyFactory.addInterface()所添加的接口，如果没有通过ProxyFactory.addInterface()添加接口，那么则看ProxyFactory.setTargetClass()所设置的targetClass是不是一个接口，把接口添加到结果集合中
		b. 同时把SpringProxy、Advised、DecoratingProxy这几个接口也添加到结果集合中去

2. 确定好要代理的集合之后，就利用Proxy.newProxyInstance()生成一个代理对象

#### JdkDynamicAopProxy创建的代理对象执行过程

1. 如果通过ProxyFactory.setExposeProxy()把exposeProxy设置为了true，那么则把代理对象设置到一个ThreadLocal（currentProxy）中去。
2. 获取通过ProxyFactory所设置的target，如果设置的是targetClass，那么target将为null
3. 根据当前所调用的方法对象寻找ProxyFactory中所添加的并匹配的Advisor，并且把Advisor封装为MethodInterceptor返回，得到MethodInterceptor链叫做chain
4. 如果chain为空，则直接执行target对应的当前方法，如果target为null会报错
5. 如果chain不为空，则会依次执行chain中的MethodInterceptor
		
		a. 如果当前MethodInterceptor是MethodBeforeAdviceInterceptor，那么则先执行Advisor中所advice的before()方法，然后执行下一个MethodInterceptor
		b. 如果当前MethodInterceptor是AfterReturningAdviceInterceptor，那么则先执行下一个MethodInterceptor，拿到返回值之后，再执行Advisor中所advice的afterReturning()方法

#### ObjenesisCglibAopProxy创建代理对象过程

1. 创建Enhancer
2. 设置Enhancer的superClass为通过ProxyFactory.setTarget()所设置的对象的类
3. 设置Enhancer的interfaces为通过ProxyFactory.addInterface()所添加的接口，以及SpringProxy、Advised接口
4. 设置Enhancer的Callbacks为DynamicAdvisedInterceptor
5. 最后通过Enhancer创建一个代理对象

#### ObjenesisCglibAopProxy创建的代理对象执行过程

执行过程主要就看DynamicAdvisedInterceptor中的实现，执行逻辑和JdkDynamicAopProxy中是一样的。

### 使用“自动代理（autoproxy）”功能

"自动代理"表示，只需要在Spring中添加某个Bean，这个Bean是一个BeanPostProcessor，那么Spring在每创建一个Bean时，都会经过这个BeanPostProcessor的判断，去判断当前正在创建的这个Bean是不是需要进行AOP。

我们可以在项目中定义很多个Advisor，定义方式有两种：
1. 通过实现PointcutAdvisor接口
2. 通过@Aspect、@Pointcut、@Before等注解

在创建某个Bean时，会根据当前这个Bean的信息，比如对应的类，以及当前Bean中的方法信息，去和定义的所有Advisor进行匹配，如果匹配到了其中某些Advisor，那么就会把这些Advisor给找出来，并且添加到ProxyFactory中去，在利用ProxyFactory去生成代理对象

#### BeanNameAutoProxyCreator

```java
@Bean
public BeanNameAutoProxyCreator creator(){
	BeanNameAutoProxyCreator beanNameAutoProxyCreator = new BeanNameAutoProxyCreator();
	beanNameAutoProxyCreator.setBeanNames("userService");  
	beanNameAutoProxyCreator.setInterceptorNames("myAdvisor");
	return beanNameAutoProxyCreator;
}
```
定义的这个bean，相当于一个“自动代理”器，有了这个Bean之后，可以自动的对setBeanNames中所对应的bean进行代理，代理逻辑为所设置的interceptorNames

#### DefaultAdvisorAutoProxyCreator

**DefaultAdvisorAutoProxyCreator**这个更加强大，只要添加了这个Bean，它就会自动识别所有的Advisor中的PointCut进行代理

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-23-11-10-18.png width='100%'/></div><br/>

**AbstractAutoProxyCreator**实现了SmartInstantiationAwareBeanPostProcessor接口，是一个BeanPostProcessor
1. 在某个Bean**实例化之前**，查看该AbstractAutoProxyCreator中是不是设置了CustomTargetSource，如果设置了就查看当前Bean是不是需要创建一个TargetSource， 如果需要就会创建一个TargetSource对象，然后进行AOP创建一个代理对象，并返回该代理对象
2. 如果某个Bean出现了循环依赖，那么会利用getEarlyBeanReference()方法提前进行AOP
3. 在某个Bean**初始化之后**，会调用wrapIfNecessary()方法进行AOP
4. 在这个类中提供了一个**抽象方法**：getAdvicesAndAdvisorsForBean()，表示对于某个Bean匹配了哪些Advices和Advisors

**AbstractAdvisorAutoProxyCreator**继承了AbstractAutoProxyCreator，AbstractAdvisorAutoProxyCreator中实现了getAdvicesAndAdvisorsForBean()方法，实现逻辑为：
1. 调用findEligibleAdvisors()

		a. 调用findCandidateAdvisors，得到所有Advisor类型的Bean
		b. 按当前正在进行Bean的生命周期的Bean进行过滤

### @EnableAspectJAutoProxy

这个注解主要是添加了一个AnnotationAwareAspectJAutoProxyCreator类型的BeanDefinition

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-23-11-17-49.png width='100%'/></div><br/>

**AspectJAwareAdvisorAutoProxyCreator**继承了AbstractAdvisorAutoProxyCreator，重写了shouldSkip(Class<?> beanClass, String beanName)方法，表示某个bean需不需要进行AOP，在shouldSkip()方法中：
1. 拿到所有的Advisor
2. 遍历所有的Advisor，如果当前bean是AspectJPointcutAdvisor，那么则跳过

**AnnotationAwareAspectJAutoProxyCreator**继承了AspectJAwareAdvisorAutoProxyCreator，重写了findCandidateAdvisors()方法，它即可以找到Advisor类型的bean，也能把所有@Aspect注解标注的类扫描出来并生成Advisor

### 注解和源码对应关系

1. @Before对应的是AspectJMethodBeforeAdvice，直接实现MethodBeforeAdvice，在进行动态代理时会把AspectJMethodBeforeAdvice转成MethodBeforeAdviceInterceptor，也就转变成了MethodBeforeAdviceInterceptor

		先执行advice对应的方法
		再执行MethodInvocation的proceed()，会执行下一个Interceptor，如果没有下一个Interceptor了，会执行target对应的方法

2. @After对应的是AspectJAfterAdvice，直接实现了MethodInterceptor

		先执行MethodInvocation的proceed()，会执行下一个Interceptor，如果没有下一个Interceptor了，会执行target对应的方法
		再执行advice对应的方法

3. @Around对应的是AspectJAroundAdvice，直接实现了MethodInterceptor

		直接执行advice对应的方法

4. @AfterThrowing对应的是AspectJAfterThrowingAdvice，直接实现了MethodInterceptor

		先执行MethodInvocation的proceed()，会执行下一个Interceptor，如果没有下一个Interceptor了，会执行target对应的方法
		如果上面抛了Throwable，那么则会执行advice对应的方法

5. @AfterReturning对应的是AspectJAfterReturningAdvice，实现了AfterReturningAdvice，在进行动态代理时会把AspectJAfterReturningAdvice转成AfterReturningAdviceInterceptor，也就转变成了MethodInterceptor

		先执行MethodInvocation的proceed()，会执行下一个Interceptor，如果没有下一个Interceptor了，会执行target对应的方法
		执行上面的方法后得到最终的方法的返回值
		再执行Advice对应的方法

### Spring中AOP原理流程图

https://www.processon.com/view/link/5faa4ccce0b34d7a1aa2a9a5

### Introduction

https://www.cnblogs.com/powerwu/articles/5170861.html

## 15、Spring之事务底层源码解析

有道云链接：

http://note.youdao.com/noteshare?id=73adfd2c0d72e9be2f8f613a75008f71&sub=75CD877539EA4470960CCD82C2077264
​
### @EnableTransactionManagement工作原理
开启Spring事务本质上就是增加了一个Advisor，但我们使用@EnableTransactionManagement注解来开启Spring事务是，该注解代理的功能就是向Spring容器中添加了两个Bean：

1. AutoProxyRegistrar
1. ProxyTransactionManagementConfiguration

AutoProxyRegistrar主要的作用是向Spring容器中注册了一个**InfrastructureAdvisorAutoProxyCreator**的Bean。
而InfrastructureAdvisorAutoProxyCreator继承了**AbstractAdvisorAutoProxyCreator**，所以这个类的主要作用就是**开启自动代理**的作用，也就是一个BeanPostProcessor，会在初始化后步骤中去寻找Advisor类型的Bean，并判断当前某个Bean是否有匹配的Advisor，是否需要利用动态代理产生一个代理对象。


ProxyTransactionManagementConfiguration是一个配置类，它又定义了另外三个bean：

1. BeanFactoryTransactionAttributeSourceAdvisor：一个Advisor
1. AnnotationTransactionAttributeSource：相当于BeanFactoryTransactionAttributeSourceAdvisor中的Pointcut
1. TransactionInterceptor：相当于BeanFactoryTransactionAttributeSourceAdvisor中的Advice

**AnnotationTransactionAttributeSource**就是用来判断某个类上是否存在@Transactional注解，或者判断某个方法上是否存在@Transactional注解的。
​
**TransactionInterceptor**就是代理逻辑，当某个类中存在@Transactional注解时，到时就产生一个代理对象作为Bean，代理对象在执行某个方法时，最终就会进入到TransactionInterceptor的invoke()方法。

### Spring事务基本执行原理

一个Bean在执行Bean的创建生命周期时，会经过InfrastructureAdvisorAutoProxyCreator的初始化后的方法，会判断当前当前Bean对象是否和BeanFactoryTransactionAttributeSourceAdvisor匹配，匹配逻辑为判断该Bean的类上是否存在@Transactional注解，或者类中的某个方法上是否存在@Transactional注解，如果存在则表示该Bean需要进行动态代理产生一个代理对象作为Bean对象。
​

该代理对象在执行某个方法时，会再次判断当前执行的方法是否和BeanFactoryTransactionAttributeSourceAdvisor匹配，如果匹配则执行该Advisor中的TransactionInterceptor的invoke()方法，执行基本流程为：

1. 利用所配置的PlatformTransactionManager事务管理器新建一个数据库连接
1. 修改数据库连接的autocommit为false
1. 执行MethodInvocation.proceed()方法，简单理解就是执行业务方法，其中就会执行sql
1. 如果没有抛异常，则提交
1. 如果抛了异常，则回滚

### Spring事务详细执行流程（和下面的老版一致）

Spring事务执行流程图：

[https://www.processon.com/view/link/5fab6edf1e0853569633cc06](https://www.processon.com/view/link/5fab6edf1e0853569633cc06)

左边开始部分

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-23-15-36-54.png width='100%'/></div><br/>

右边结束部分

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-23-15-37-46.png width='100%'/></div><br/>

### Spring事务传播机制
在开发过程中，经常会出现一个方法调用另外一个方法，那么这里就涉及到了多种场景，比如a()调用b()：

1. a()和b()方法中的所有sql需要在同一个事务中吗？
1. a()和b()方法需要单独的事务吗？
1. a()需要在事务中执行，b()还需要在事务中执行吗？
1. 等等情况...

所以，这就要求Spring事务能支持上面各种场景，这就是Spring事务传播机制的由来。那Spring事务传播机制是如何实现的呢?


先来看上述几种场景中的一种情况，a()在一个事务中执行，调用b()方法时需要新开一个事务执行：
​
1. 首先，代理对象执行a()方法前，先利用事务管理器新建一个数据库连接a
1. 将数据库连接a的autocommit改为false
1. 把数据库连接a设置到ThreadLocal中
1. 执行a()方法中的sql
1. 执行a()方法过程中，调用了b()方法（注意用代理对象调用b()方法）
   1. 代理对象执行b()方法前，判断出来了当前线程中已经存在一个数据库连接a了，表示当前线程其实已经拥有一个Spring事务了，则进行**挂起**
   1. 挂起就是把ThreadLocal中的数据库连接a从ThreadLocal中移除，并放入一个**挂起资源对象**中
   1. 挂起完成后，再次利用事务管理器新建一个数据库连接b
   1. 将数据库连接b的autocommit改为false
   1. 把数据库连接b设置到ThreadLocal中
   1. 执行b()方法中的sql
   1. b()方法正常执行完，则从ThreadLocal中拿到数据库连接b进行提交
   1. 提交之后会恢复所挂起的数据库连接a，这里的恢复，其实只是把在**挂起资源对象**中所保存的数据库连接a再次设置到ThreadLocal中
6. a()方法正常执行完，则从ThreadLocal中拿到数据库连接a进行提交

这个过程中最为核心的是：**在执行某个方法时，判断当前是否已经存在一个事务，就是判断当前线程的ThreadLocal中是否存在一个数据库连接对象，如果存在则表示已经存在一个事务了。**

### Spring事务传播机制分类
​
**其中，以非事务方式运行，表示以非Spring事务运行，表示在执行这个方法时，Spring事务管理器不会去建立数据库连接，执行sql时，由Mybatis或JdbcTemplate自己来建立数据库连接来执行sql。**

### 案例分析

#### 情况1
```java
@Component
public class UserService {
	@Autowired
	private UserService userService;

	@Transactional
	public void test() {
		// test方法中的sql
		userService.a();
	}

	@Transactional
	public void a() {
		// a方法中的sql
	}
}
```
默认情况下传播机制为**REQUIRED，表示当前如果没有事务则新建一个事务，如果有事务则在当前事务中执行。**
**​**

所以上面这种情况的执行流程如下：

1. 新建一个数据库连接conn
1. 设置conn的autocommit为false
1. 执行test方法中的sql
1. 执行a方法中的sql
1. 执行conn的commit()方法进行提交

#### 情况2
假如是这种情况
```java
@Component
public class UserService {
	@Autowired
	private UserService userService;

	@Transactional
	public void test() {
		// test方法中的sql
		userService.a();
        int result = 100/0;
	}

	@Transactional
	public void a() {
		// a方法中的sql
	}
}
```
所以上面这种情况的执行流程如下：

1. 新建一个数据库连接conn
1. 设置conn的autocommit为false
1. 执行test方法中的sql
1. 执行a方法中的sql
1. 抛出异常
1. 执行conn的rollback()方法进行回滚，所以两个方法中的sql都会回滚掉

#### 情况3
假如是这种情况：
```java
@Component
public class UserService {
	@Autowired
	private UserService userService;

	@Transactional
	public void test() {
		// test方法中的sql
		userService.a();
	}

	@Transactional
	public void a() {
		// a方法中的sql
        int result = 100/0;
	}
}
```
所以上面这种情况的执行流程如下：

1. 新建一个数据库连接conn
1. 设置conn的autocommit为false
1. 执行test方法中的sql
1. 执行a方法中的sql
1. 抛出异常
1. 执行conn的rollback()方法进行回滚，所以两个方法中的sql都会回滚掉

#### 情况4
如果是这种情况：
```java
@Component
public class UserService {
	@Autowired
	private UserService userService;

	@Transactional
	public void test() {
		// test方法中的sql
		userService.a();
	}

	@Transactional(propagation = Propagation.REQUIRES_NEW)
	public void a() {
		// a方法中的sql
		int result = 100/0;
	}
}
```
所以上面这种情况的执行流程如下：

1. 新建一个数据库连接conn
1. 设置conn的autocommit为false
1. 执行test方法中的sql
1. 又新建一个数据库连接conn2
1. 执行a方法中的sql
1. 抛出异常
1. 执行conn2的rollback()方法进行回滚
1. **继续抛异常，对于test()方法而言，它会接收到一个异常，然后抛出**
1. 执行conn的rollback()方法进行回滚，最终还是两个方法中的sql都回滚了

### Spring事务强制回滚

正常情况下，a()调用b()方法时，如果b()方法抛了异常，但是在a()方法捕获了，那么a()的事务还是会正常提交的，但是有的时候，我们捕获异常可能仅仅只是不把异常信息返回给客户端，而是为了返回一些更友好的错误信息，而这个时候，我们还是希望事务能回滚的，那这个时候就得告诉Spring把当前事务回滚掉，做法就是：

```java
@Transactional
public void test(){
	
    // 执行sql
	try {
		b();
	} catch (Exception e) {
		// 构造友好的错误信息返回
		TransactionAspectSupport.currentTransactionStatus().setRollbackOnly();
	}
    
}

public void b() throws Exception {
	throw new Exception();
}
```
### TransactionSynchronization

Spring事务有可能会提交，回滚、挂起、恢复，所以Spring事务提供了一种机制，可以让程序员来监听当前Spring事务所处于的状态。
​
```java
@Component
public class UserService {

	@Autowired
	private JdbcTemplate jdbcTemplate;

	@Autowired
	private UserService userService;

	@Transactional
	public void test(){
		TransactionSynchronizationManager.registerSynchronization(new TransactionSynchronization() {

			@Override
			public void suspend() {
				System.out.println("test被挂起了");
			}

			@Override
			public void resume() {
				System.out.println("test被恢复了");
			}

			@Override
			public void beforeCommit(boolean readOnly) {
				System.out.println("test准备要提交了");
			}

			@Override
			public void beforeCompletion() {
				System.out.println("test准备要提交或回滚了");
			}

			@Override
			public void afterCommit() {
				System.out.println("test提交成功了");
			}

			@Override
			public void afterCompletion(int status) {
				System.out.println("test提交或回滚成功了");
			}
		});

		jdbcTemplate.execute("insert into t1 values(1,1,1,1,'1')");
		System.out.println("test");
		userService.a();
	}

	@Transactional(propagation = Propagation.REQUIRES_NEW)
	public void a(){
		TransactionSynchronizationManager.registerSynchronization(new TransactionSynchronization() {

			@Override
			public void suspend() {
				System.out.println("a被挂起了");
			}

			@Override
			public void resume() {
				System.out.println("a被恢复了");
			}

			@Override
			public void beforeCommit(boolean readOnly) {
				System.out.println("a准备要提交了");
			}

			@Override
			public void beforeCompletion() {
				System.out.println("a准备要提交或回滚了");
			}

			@Override
			public void afterCommit() {
				System.out.println("a提交成功了");
			}

			@Override
			public void afterCompletion(int status) {
				System.out.println("a提交或回滚成功了");
			}
		});

		jdbcTemplate.execute("insert into t1 values(2,2,2,2,'2')");
		System.out.println("a");
	}


}
```

----------------------------------下面为老版笔记----------------------------------
### spring事物基本原理（老版笔记）

一个类上或某个方法上加了@Transactional注解，那么这个类对应的Bean将会是这个类的一个代理对象，在调用代理对象的某个方法时，**简要流程**为：
1. 先判断当前所调用的方法是否存在@Transactional注解
2. 如果存在则利用事务管理器新建一个数据库连接
3. 修改数据库连接的autocommit为false
4. 执行业务方法，其中就会执行sql
5. 执行方法后，如果没有抛异常，则提交
6. 如果抛了异常，则回滚

### 开始事务

我们是通过添加@EnableTransactionManagement注解来开启事务的，而@EnableTransactionManagement注解的作用是向Spring容器中注入了两个bean：
1. AutoProxyRegistrar
2. ProxyTransactionManagementConfiguration

### AutoProxyRegistrar

AutoProxyRegistrar主要的作用是向Spring容器中注册了一个InfrastructureAdvisorAutoProxyCreator的Bean。
而InfrastructureAdvisorAutoProxyCreator继承了**AbstractAdvisorAutoProxyCreator**，所以这个类的主要作用就是**开启自动代理**的作用。

### ProxyTransactionManagementConfiguration

ProxyTransactionManagementConfiguration中定义了三个bean：
1. BeanFactoryTransactionAttributeSourceAdvisor：一个PointcutAdvisor
2. AnnotationTransactionAttributeSource：就是Pointcut
3. TransactionInterceptor：就是代理逻辑Advice

```java
BeanFactoryTransactionAttributeSourceAdvisor advisor = new BeanFactoryTransactionAttributeSourceAdvisor();
advisor.setTransactionAttributeSource(transactionAttributeSource); 
advisor.setAdvice(transactionInterceptor);
```

BeanFactoryTransactionAttributeSourceAdvisor是一个Advisor，在构造BeanFactoryTransactionAttributeSourceAdvisor这个bean时，需要另外两Bean。

BeanFactoryTransactionAttributeSourceAdvisor中是这么定义PointCut的：

```java
private final TransactionAttributeSourcePointcut pointcut = new TransactionAttributeSourcePointcut() {
	@Override
	@Nullable
	protected TransactionAttributeSource getTransactionAttributeSource() {
		return transactionAttributeSource;
	}
};

```

构造了一个PointCut, TransactionAttributeSource的实现对象为AnnotationTransactionAttributeSource，在PointCut匹配类时，会利用AnnotationTransactionAttributeSource去检查类上是否有@Transactional注解，在PointCut匹配方法时，会利用AnnotationTransactionAttributeSource去检查方法上是否有@Transactional注解。

所以ProxyTransactionManagementConfiguration的作用就是向Spring容器中添加了一个Advisor，有了Advisor，那么Spring在构造bean时就会查看当前bean是不是匹配所设置的PointCut（也就是beanClass上是否有@Transactional注解或beanClass中某个方法上是否有@Transactional注解），如果匹配，则利用所设置的Advise（也就是TransactionInterceptor）进行AOP，生成代理对象。

### TransactionInterceptor执行流程:

Spring事务执行流程图：https://www.processon.com/view/link/5fab6edf1e0853569633cc06

### 简单版流程

1. 生成test事务状态对象
2. test事务doBegin，获取并将数据库连接2825设置到test事务状态对象中
3. 把test事务信息设置到事务同步管理器中
4. 执行test业务逻辑方法（可以获取到test事务的信息）

		a. 生成a事务状态对象，并且可以获取到当前线程中已经存在的数据库连接2825
		b. 判断出来当前线程中已经存在事务
		c. 如果需要新开始事务，就先挂起数据库连接2825，挂起就是把test事务信息从事务同步管理器中转移到挂起资源对象中，并把当前a事务状态对象中的数据库连接设置为null
		d. a事务doBegin，新生成一个数据库连接2826，并设置到a事务状态对象中
		e. 把a事务信息设置到事务同步管理器中
		f. 执行a业务逻辑方法（可以利用事务同步管理器获取到a事务信息）
		g. 利用a事务状态对象，执行提交
		h. 提交之后会恢复所挂起的test事务，这里的恢复，其实只是把挂起资源对象中所保存的信息再转移回事务同步管理器中

5. 继续执行test业务逻辑方法（仍然可以获取到test事务的信息）
6. 利用test事务状态对象，执行提交

### 传播机制

<div align='center'><img src=./images/spring-原理-tuling5zy/spring-原理-tuling5zy_2021-09-23-11-40-27.png width='100%'/></div><br/>

### 举例子

#### 情况1

```java
@Component
public class UserService {
	@Autowired
	private UserService userService;

	@Transactional
	public void test() {
		// test方法中的sql
		userService.a();
	}

	@Transactional
	public void a() {
		// a方法中的sql
	}
}

```

默认情况下传播机制为REQUIRED

所以上面这种情况的执行流程如下：
1. 新建一个数据库连接conn
2. 设置conn的autocommit为false
3. 执行test方法中的sql
4. 执行a方法中的sql
5. 执行conn的commit()方法进行提交

#### 情况2

假如是这种情况

```java
@Component
public class UserService {
	@Autowired
	private UserService userService;

	@Transactional
	public void test() {
		// test方法中的sql
		userService.a();
        int result = 100/0;
	}

	@Transactional
	public void a() {
		// a方法中的sql
	}
}
```
所以上面这种情况的执行流程如下：
1. 新建一个数据库连接conn
2. 设置conn的autocommit为false
3. 执行test方法中的sql
4. 执行a方法中的sql
5. 抛出异常
6. 执行conn的rollback()方法进行回滚

#### 情况3

假如是这种情况：

```java
@Component
public class UserService {
	@Autowired
	private UserService userService;

	@Transactional
	public void test() {
		// test方法中的sql
		userService.a();
	}

	@Transactional
	public void a() {
		// a方法中的sql
        int result = 100/0;
	}
}
```

所以上面这种情况的执行流程如下：
1. 新建一个数据库连接conn
2. 设置conn的autocommit为false
3. 执行test方法中的sql
4. 执行a方法中的sql
5. 抛出异常
6. 执行conn的rollback()方法进行回滚

#### 情况4

如果是这种情况：

```java
@Component
public class UserService {
	@Autowired
	private UserService userService;

	@Transactional
	public void test() {
		// test方法中的sql
		userService.a();
	}

	@Transactional(propagation = Propagation.REQUIRES_NEW)
	public void a() {
		// a方法中的sql
		int result = 100/0;
	}
}
```

所以上面这种情况的执行流程如下：
1. 新建一个数据库连接conn
2. 设置conn的autocommit为false
3. 执行test方法中的sql
4. 又新建一个数据库连接conn2
5. 执行a方法中的sql
6. 抛出异常
7. 执行conn2的rollback()方法进行回滚
8. 继续抛异常
9. 执行conn的rollback()方法进行回滚






