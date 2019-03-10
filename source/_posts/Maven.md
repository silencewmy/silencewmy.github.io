---
title: maven学习
date: 2018-11-25 22:42:42
tags:  maven
categories: 技术
---



maven入门

<!--more-->
下载Maven，添加环境变量。

maven：合理叙述项目间的依赖关系。maven基于项目对象模型，通过一段描述信息来管理项目的构建。最直观的就是项目中依赖多个jar包，需要开发者一个个来下载导入，这些繁琐的步骤可以交由maven来管理。
在pom.xml中根据坐标建立依赖，也就是说在pom文件中配置项目中需要的jar包，或者说需要依赖的资源

Maven中的“坐标”：用来唯一标识一个项目  

- groupId 所需jar包的项目名
- artifactId  所需jar包的模块名
- version   所需jar包的版本号


maven库：分为
		本地库：mave从其他地方下载所依赖的jar包的存储地或者说mave工程中依赖的全部jar包的存储地。
		第三方仓库：也称为内部中心仓库，也成为私库。一般为公司每部共享使用作为公司内部公用类库镜像的缓存，保证开发公司开发人员得到的版本都一致。
		中央库：远程的有大量的常用类库，是一个资源地。
maven获取jar包的过程：优先从本地仓库中查找，有私库就在私库中寻找，没有私库就从中央库中下载。


maven Java项目结构：
项目
	---pom.xml   核心配置，项目根下
	---src
		--main
			--Java      Java源码目录
			--resources Java配置文件目录
		--text
			--java      测试源码目录
			--resources 测试配置目录




pom.xml中的标签
<dependencies>
	<dependencie>
		在这其中配置所需的依赖，通过坐标来定位查找所要的依赖
	<dependencie>
<dependencies>


~~~
	<dependencies>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>5.1.34</version>
		</dependency>
	<dependencies>

~~~