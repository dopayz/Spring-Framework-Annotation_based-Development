# Spring注解驱动开发

> JDK1.8
>
> Spring 4.3.12（maven中央仓库下载spring-context,版本4.3.12.RELEASE）
>
> IntelliJ IDEA ULTIMATE 2017.3

  - [IOC](#IOC)
  - [AOP](#AOP)
  - [声明式事务](#声明式事务)
  - [扩展原理](#扩展原理)
  - [容器创建](#容器创建)
  - [附录](#附录)
  
## IOC
  * [组件注册](组件注册.md)
  * [生命周期](生命周期.md)
  * [属性赋值](属性赋值.md)
  * [自动装配](自动装配.md)

## AOP
  * [AOP](AOP.md)
  * [AspectJ]
## 声明式事务
  * [声明式事务](声明式事务.md)

## 扩展原理
  * [BeanDefinitionRegistryPostProcessor](BeanDefinitionRegistryPostProcessor.md)
  * [BeanFactoryPostProcessor](BeanFactoryPostProcessor.md)
  * [ApplicationListener](ApplicationListener.md)

## 容器创建
  * [容器创建过程](容器创建过程.md)

## 附录
  * id 和 name
  
> * id用来标识bean，是唯一的，且只有一个；name定义的是bean的alias，可以有多个，并可能与其他的bean重名
> * 如果id和name都没有指定，则用类全名作为name
> * 配置文件中不允许出现两个id相同的，否则在初始化时即会报错;但配置文件中允许出现两个name相同的，在用getBean()返回实例时，后面一个Bean被返回,应该是前面那个被后面同名的 覆盖了
  


