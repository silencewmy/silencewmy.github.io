---
title: 小白的SSM框架使用
date: 2019-03-10 10:42:42
tags:  SSM框架
categories: 技术
---


新手使用SSM的配置和个人对SSM框架的简单理解


<!--more-->

本文仅仅适用于被整合SSM框架搞糊涂的小白。本文单纯的告诉你怎么配置使的这三个框架结合起来。其中并没有说明每个框架怎么用。文中尽量不掺杂在开发过程中是选择注解还是XML方式的不同。因为笔者也是小白。。。
<!--more-->
##我们按照两个不同的顺序来介绍使用，这样可能不太会让你混乱。
###我们先是从整体到局部
首先我们要明确的是：

- **springMVC的工作**：根据不同请求调用不同的功能是springMVC完成的，是springMVC将我们的功能代码进行分离。接收前端送来的请求，调用相应的处理逻辑来处理请求，最后将结果封装（保存）到模型中传递给前端视图来展示数据。如果是你来根据不同的人来做不同的事情你肯定会：看到来了一个人->奥，是他啊->那我就怎么怎么样。springMVC也是这样来完成。
- **spring的工作**：这里就是让你能接受spring基本功能之一的springIOC，说白了就是我们把自己有的所有的类都告诉它，然后通过spring来获取这些类的对象（这时候你有一点困惑，如果一个类需要参数也就是他需要依赖一些辅助才能完成本类的正常使用要怎么办，后面再说）。我们有了springMVC就可以完成功能逻辑的分离，想想我们的代码组成中，可以说每个类都有它的功能，我们提前编写好它，当我们用到这个功能的时候，我们就创建该类的一个对象来使用它。这种就是：我知道你有这个功能，你这个功能我要用，那我就来创建一个来使用。当通一个功能被多处使用的时候，要修改另一个类的相差无几的功能的时候，想想要怎么做，你要一个个的去修改。而spring就是帮助我们管理这些功能模块，spring中统一管理的资源（这些功能类）叫做bean。想想我们经常写的登录和注册，通过一个id来登录，发现注册表中没有这个id，就没有结果需要去注册。而找到这个id，后就返回它所需的东西，但肯定这个id是提前注册的。spring就类似这个功能。你需要往这个容器中添加东西（bean），别人通过id向容器索要该资源。你告诉容器：我要用这个类提供的功能。那么容器就会返回给你该类的一个对象，不再是你通过new来创建一个对象（当然对象肯定是要创建的，只不过不在是使用者来创建而是IOC容器来创建），这就叫做控制反转。
- **mybatis**:这是用来操作数据库，提供sql语句，获得返回的结果。它就是连接数据库，因为对数据库的连接、操作都简单，因此通过mybatis来连接数据库。

最简单的SSM使用就是：springMVC根据前端请求来调用相对应的功能类，spring管理整个工程中的各种功能类，提供给使用者。mybatis就是执行sql语句来获得数据（将数据封装起来）供逻辑代码使用。


**项目目录，后面的各种扫描的包的路径就是根据下面的目录----本人用的IDEA**
~~~
项目名
  
  |     -.idea

  |     -out

  |     -resources
                -DB.properties
                -mybatis-config.xml
                -spring-mybatis.xml
                -springmvc-config.xml

  |     -src
                -main
                        -aspect
                        -contorller
                        -dao
                        -POJO
                        -service
                        
  |     -web
                -WEB-INF
                        -css
                        -images
                        -jsp

~~~


以下就是将三个框架整合起来的关键性配置语句：

**web.xml:**
~~~ xml
<!-- 第一部分是整合spring相关的 -->

<!-- 这是spring配置文件的路径 -->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring-mybatis.xml</param-value>
</context-parm>



<!-- 初始化SpringIOC是一个比较耗时的操作，而其中又是工程中要用到的所有资源的容器，
因此要在使用前准备好，可以在ContextLoaderListener中实现在DispatcherServlet初始
化前初始化。
ContextLoaderListener是ServletContextListener的一个实现类，
后者与Javaweb生命周期有关。 -->
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>




<!-- 第二部分是整合SpringMVC的，配置要拦截那些东西 -->

<!-- DispatcherServlet的定义处，DispatcherServlet本身就是一个servlet,
DispatcherServlet是来初始化请求上下文的，就是根据请求转到处理器中 -->
<servlet>
    <servlet-name>springmvc-config</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

    <!-- 这里是告知SpringMVC配置文件的地址 -->

    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-valer>classpath:springmvc-config.xml</param-valer>
    </init-param>

    <load-on-startup>1</load-on-startup>

</servlet> 

<!-- 这里是定义servlet拦截哪些请求，通过servlet-name来对应，这里是拦截所有的
特别说一下，如果url-pattern配置的是 *.do  。 你可以理解为是一个正则，就是它只会拦截哪些请求中是以.do结尾的请求 -->
<servlet-mapping>
    <servlet-name>springmvc-config</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>




~~~


**springMVC配置:springmvc-config.xml**
~~~xml
<!-- 这里表明使用注解驱动 -->
<mvc:annotation-drivern />

<!-- 配置要扫描成controller的包 -->
<context:component-scan base-package="main.controller">



<!-- 配置静态资源的访问映射，下面配置中的文件将不再被前端拦截 -->
<mvc:resources location="/WEB-INF/js/" mapping="/js/**" />
<mvc:resources location="/WEB-INF/css/" mapping="/css/**" />
<mvc:resources location="/WEB-INF/images/" mapping="/images/**" />


<!-- 这里是配置视图解析器，根据返回的模型来渲染到的视图的对应关系 -->
<bean class="org.springframe.web.servelet.view.InternalResourceViewResolver">
    <property name="prefix" value="/WEB-INF/jsp/" />
    <property name="suffix" value=".jsp" />
    <!-- 上面两个属性，你配置的 prefix+你模型中给的名字+suffix 就是选择的将要渲染的视图。 -->

</bean>
~~~

**spring的配置：spring-mybatis.xml 不过一般是applicationContext.xml这个名字。**

~~~xml
<!-- 告知配置文件在哪里 -->
<context:property-placeholder location="classpath:DB.properties" />

<!-- 数据库连接池，数据源配置 -->
<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
    <property name="driverClassName" value="${jdbc.driver}" />
    <property name="url" value="${jdbc.url}"/>
    <property name="username" value="${jdbc.username}"/>
    <property name="password" value="${jdbc.password}"/>
</bean>

<!-- 集成mybatis，这里是配置mybatis关键的SQLSessionFactory -->
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <property name="dataSource" ref="datasource" />

    <!-- 下面是告知mybatis的配置文件在哪里 -->
    <property name="configLocation" value="classpath:mybatis-config.xml">
</bean>

<!-- spring针对mybatis的提供事务功能的模板，也就是DataSourceTransactionManager
配饰事务管理器，配置的属性只有数据源 -->
<bean id="transactionManager" class="org.springframe.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource">
</bean>
<!-- 使用注解定义事务 也就是@Transcctional-->
<tx:annotation-driven transaction-manager="transactionManager" />

<!-- 配置成自动扫描dao，也就是mybatis中的mapper。 -->
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <!-- 告知springIOC在哪个包中扫描dao层类 -->
    <property name="basePackage" value="main.dao" />

    <!-- 注入用于生成session的mybatis中关键的sqlSessionFactory -->
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />

    <!-- 下面这条的属性叫做注解类，表示某个类只有被value中指定的注解标识后才会被扫描成
    Mapper，Spring中使用的是@Repository，也就是说只要被@Repository标识后它就是一个dao层。 -->
    <property name="annotationClass" value="org.springframework.stereotype.Repository"> 


    <!-- spring自动扫描的包，这个包是扫描service层 -->
    <context:component-scan base-package="main.service"/>

    <!-- XML方式启用AspectJ自动代理 -->
    <aop:aspectj-autoproxy/>
</bean>


~~~

**mybatis配置**
~~~xml
<configuration>
    <!-- 设置别名，使用别名来代替复杂的完整类名，下面的是配置将这个包下的类名首
    字母小写来作为springIOC中的bean的id -->
    <typeAliases>
        <package name="main.POJO">
    </typeAliases>

</configuration>
~~~

经过上述的配置就可以将三个框架结合起来。

