---
title: springboot 配置文件
date: 2019-03-10 10:42:42
tags:  springboot
categories: 技术
---


springboot配置文件的使用


<!--more-->

####如何使用application.properties配置文件####

一般存在src/main/resource

将配置文件中的值绑定到需要的***属性上***，使用@Value("${属性名}")  
属性太多使用这种方式太繁琐，按照官方推荐，绑定到对象bean上。方式为
@ConfigurationProperties(prefix = "属性的前缀")

**有时候想要将不同的配置放置在不同的配置文件中时，如何引用这些自定义的配置文件中的值**
在将其绑定的类前加上@PropertySource(value="自定义配置文件的地址") 
@ConfigurationProperties(prefix = "所需属性的前缀")
~~~java
//用到test.properties中的属性值，属性以com.wmy开头
@PropertySource(value="classpath:test.properties")
@ConfigurationProperties(prefix = "com.wmy")
@Component
~~~

**配置文件的优先级**：  
在config目录下的配置文件会覆盖在classpath文件下的配置文件。  
相同优先级位置放置的.yml和.properties的情况下.yml会覆盖.properties中的属性。
classpath:中的配置文件属于优先级最低

**针对不同环境的但是配置内容（属性）相同的配置文件的区分和使用。**  
***命名格式为：application-{profile}.properties***例如：application-dev.properties application-test.properties  
想要选择上面两个中的某一个需要在application.properties（全局配置文件）中
添加***spring.profiles.active=profile（你写的名字中的一个）***



springboot启动类上都有一个注解@SpringBootApplication，此注解的定义包含多个注解，其中较主要的是

- @Configuration（实际为@SpringBootConfiguration，但其中主要的是@Configuration）
    + 任何标记了@Configuration的Java类定义都是一个javaconfig配置类。标记为一个SpringIOC的配置类，类中用到@Bean作用到方法上，将返回值作为一个bean注册到SpringIOC容器中，方法名字即为bean的id
- @CompoentScan
    +  自动扫描并加载符合条件的组件到SpringIOC容器中，当不指定basepackages属性时，从该注解的所在类的包中扫描，springboot中就是启动类所在的包。
- @EnableAutoConfiguration
    + 借助@Import的支持，收集和注册特定场景相关的bean定义。（所有符合自动配置条件的bean）
    + 该注解是一个复合型注解，使用了EnableAutoConfigurationImportSelector来选择所有符合条件的@Configuration类的配置。
    + 借助spring框架原有的一个工具类SpringFactoriesLoader，可以智能的自动配置。
    + 可以说自动配置的幕后就是SpringFactoriesLoader，它是一种扩展方案。主要功能是从META/spring.factories中来加载配置。配合@EnableAutoConfiguration使用，更多的是体现一种配置查找的功能支持。以org.springframework.boot.autoconfigure.EnableAutoConfiguration的类的全路径名作为键查找相对应的配置类。
    + @EnableAutoConfiguration所完成的工作就是从classpath中所有的META-INF/spring.factories配置文件搜寻org.springframework.boot.autoconfigure.EnableutoConfiguration对应的配置类（@Configuration）反射为实例类，汇总并加载到SpringIOC中。



###spring boot的启动流程


在启动类的main方法中，运行的是SpringApplication的run（）方法，该方法首要创建一个SpringApplication实例对象，然后调用该实例对象的实例方法。

1. 在初始化SpringApplication之前，要做一些准备工作（SpringIOC容器的设计主要是基于BeanFactory和ApplicationContext两个接口，ApplicationContext是BeanFactory的子接口，ApplicationContext对BeanFactory做了扩展，一般使用ApplicationContext作为SpringIOC容器）
    + 根据classpath中是否存在某个特征类来决定是否创建一个为web程序使用的ApplicationContext类型 ps：特征类为（org.springframework.web.context.ConfigurableWebApplicationContext）
    + 使用SpringFactoriesLoader在classpath中搜索加载可用的ApplicationContextInitializer（ApplicationContext的初始化器）
    + 同样使用SpringFactoriesLoader在classpath查找加载所有可用的ApplicationListener（监听器）
    + 推断并设置main方法的定义类
2. 准备工作做完后就执行run方法的逻辑了。
    - 调用所有的SpringApplicationRunListener的started()方法（方法作用类似名字，告知开始启动springboot应用），**这些运行监听器都是通过SpringFactoriesLoader查找并加载进来的**
3. 创建并配置当前SpringBoot应用将要使用的Environment。
4. **调用所有的运行监听器的environmentPrepared()方法**，告知所需环境准备完毕。
5. 如果SpringApplication的属性showBanner设置为true，打印banner
6. 在这一步中创建ApplicationContext，并将设置好的environment给ApplicationContext使用
    - 根据是否设置了ApplicationContextClass类型，以及初始化准备阶段的main的推断来创建什么类型的ApplicationContext。
7. 对ApplicationContext进一步处理。
    - SpringFactoriesLoader在classpath中查找的所有的ApplicationContextInitializer的initialize()方法来进一步处理
8. 调用所有运行监听器(SpringApplicationRunListener)的ContextPrepared方法
9. 将@EnableAutoConfiguration获取到的所有IOC配置加载到准备好了的ApplicationContext中。
10. 调用所有的运行监听器的contextLoaded()方法,告知加载完成
11. 调用ApplicationContext的refresh()方法，完成IOC的最后一道工序
12. 查找当前ApplicationContext是否注册有CommandLineRunner，如有，遍历执行
13. 调用所有的运行监听器的finished()方法，表示启动完成。


springboot中已经默认开启jpa、jdbc、mybatis的事务，当引入所对应的依赖后，事务就默认开启。
springboot开启事务非常简单，只需要加一行注解。