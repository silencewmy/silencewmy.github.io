---
title: springbootweb开发
date: 2019-03-10 10:42:42
tags:  springboot
categories: 技术
---


jsp和Thymeleafweb开发


<!--more-->

####springboot web开发 Thymeleaf和jsp####

springboot官方并不推荐JSP技术，推荐使用的是Thymeleaf。  
原因如下：

1.  tomcat只支持war的打包方式，不支持可执行的jar。
2.  Jetty嵌套的容器不支持jsp。
3.  创建自定义的error.jsp不会覆盖错误处理的默认视图。

Thymeleaf是一款用以渲染XML/XHML/HTML5内容的模板引擎。它既可以看动态界面，又可以在没有数据的数据的时候正常查看静态的页面。

Thymeleaf可以轻易的与SpringMVC等web框架进行集成，并作为web应用的模板引擎。有如下特点  

- SpringMVC中的@Controller中的方法可以直接返回模板名称，Thymeleaf模板会自动进行渲染。
- 模板中的表达式支持spring表达式（springEL）
- 支持表单，兼容SpringMVC的数据绑定与验证机制
- 国际化支持

####使用Thymeleaf的步骤方法####

**首先引入Thymeleaf依赖**
~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
~~~
如果添加后没有添加进来就启动一下maven的clean操作，如果是idea的话在界面的右边栏MavenProjects->Plugins->clean:下面的clean:clean，选中后点击上面的绿色三角启动。  
**编写controller**  
值得复习的就是SpringMVC中的@Controller和@RequestMapping的使用  
**编写HTML模板**  
默认的模板路径为src/main/resource/templates。




**如果想要将HTML的静态转换成动态视图，需要在HTML开头添加如下代码**
~~~html
<html xmlns:th="http://www.thymeleaf.org">
~~~
此外，还需要在使用动态处理的元素前使用“th:前缀”。



####springboot的jsp开发####
**添加存放jsp文件的目录**  
在一个webapp目录用来存放jsp文件，静态资源扔放置在resources的static下。创建文件夹不能右击然后徐新建文件夹（这种方式不能创建.jsp文件）。  
创建方法过程如下：

1. idea中右上角project structure（放大镜的左边按钮）
2. 选择左边栏Modules，然后选择中间栏中的Web选项
3. 在右下部分即Web Resource Directories中点击右边的绿色加号，然后增加webapp目录（直接写名字，如果不存在会进行创建）。

**引入依赖包注意 【不能和Thymeleaf同时使用，要将Thymeleaf的依赖注释掉，不然会报错（新手问题）】**  
jsp开发需要tomcat容器来运行，并且随使用到jstl标签。进行jsp开发要引入如下依赖包：
~~~xml
<!--WEB支持-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<!--jsp页面使用jstl标签-->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>jstl</artifactId>
</dependency>

<!--用于编译jsp-->
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
    <scope>provided</scope>
</dependency>
~~~



**告诉springboot视图在哪里**  
在application.properties中配置返回文件的路径以及类型，就是要渲染的视图。
~~~properties
spring.mvc.view.prefix: /WEB-INF/jsp/
spring.mvc.view.suffix: .jsp
~~~

如果对jsp开发任然想深入了解，移步结尾处到嘟嘟MD的博客中。

学习整理自（排名不分先后）  
[嘟嘟MD的SpringBoot系列](http://tengj.top/tags/Spring-Boot/)