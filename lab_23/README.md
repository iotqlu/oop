---
sort: 23
---

# 项目环境搭建

## 一、实验介绍

#### 1.1 实验内容

本节课程主要用 Maven 搭建项目环境，并添加所需的 jar 包。

#### 1.2 实验知识点

- Maven 环境搭建
- Maven 工程创建
- pom.xml 文件配置

#### 1.3 实验环境

- WEB IDE
- JDK1.8

## 二、项目文件结构

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540201522663.png)  
![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540201524986.png)


> 关于在线环境的使用遇到任何问题请查阅使用指南：[https://www.shiyanlou.com/questions/89126](https://www.shiyanlou.com/questions/89126)

## 三、实验步骤

这一章节中我们将搭建项目环境。

>注：当前环境无法保存，需要保存的同学请自行通过Git保存-[Github 快速上手实战教程](https://www.shiyanlou.com/courses/868)


### 3.1 创建 Maven 工程

首先点击顶栏的 Terminal -> open new terminal

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190929-1569735109069)

在控制台中，我们使用maven骨架创建一个web项目，输入命令：  
```
mvn archetype:generate \
  -DgroupId=com.shiyanlou \
  -DartifactId=hrms \
  -DarchetypeArtifactId=maven-archetype-webapp
```

参数说明：

- Group Id：项目的组织机构，也是包的目录结构，一般都是域名的倒序，比如  `com.shiyanlou`；
- Atifact Id ：项目实际的名字，比如 `hrms`；
- archetype Artifact Id ：使用的maven骨架名称

输入命令之后，maven会提示我们输入版本号，这里可以直接定义版本号也可以直接回车，接着maven会提示当前要创建项目的基本信息，输入y然后回车确认。
结果如下:  
![](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540203093445.png)  
目录结构如下：  
![](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540259327004.png)  
我们可以看到目录结构中没有存放java源代码的目录，所以在main目录下，在新建一个java目录，最后目录结构应该如下：  
![](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540260645956.png)  
创建好了项目之后，**需要将工作空间切换到创建的项目中(即hrms目录)**(必须切换到该目录，否则不会有自动提示和导包等功能)，选择File->Open WorkSpace，找到刚才创建的hrms项目，选择它，点击open，就可以成功切换了。  
切换成功之后，web ide会自动生成一些配置文件（可能需要一点时间）。最后的文件目录如下：  
![](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540261739928.png)  

```checker
- name: check hrms exist
  script: |
    #!/bin/bash
    ls /home/project/hrms
  error: 我们发现您还没有创建项目hrms! /home/project/hrms
  timeout: 1
```

### 3.2 修改web.xml 文件

>注意：使用maven骨架生成的web项目默认为web2.3，我们需要将其升级到更高的版本  

打开`web.xml`,修改内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	id="WebApp_ID" version="3.1">
	<display-name>hrms</display-name>

</web-app>
```

这样我们就升级为了web3.1的版本。

### 3.3 配置 pom.xml

由于我们项目中使用到的 ueditor 编辑器的 jar 包不存在于远程和本地仓库，项目的 pom.xml 中无法添加依赖，导致无法使用 mvn 打包发布，我们需要手动添加到本地仓库。

添加方法：

打开终端输入下面的命令获得 ueditor 的 jar ；
```
wget http://labfile.oss.aliyuncs.com/courses/824/ueditor-1.1.2.jar
```

在终端中输入命令进行 `install`；

```
mvn install:install-file -Dfile=./ueditor-1.1.2.jar -DgroupId=com.baidu -DartifactId=ueditor -Dversion=1.1.2 -Dpackaging=jar
```

这样就将 ueditor 的 jar 包手动添加到了本地仓库。


打开 `pom.xml` 文件，添加项目所需 jar 的依赖，代码如下：

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.shiyanlou</groupId>
	<artifactId>hrms</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>war</packaging>

	<name>hrms</name>
	<url>http://maven.apache.org</url>

	<properties>

		<jdbc.driver.version>5.1.25</jdbc.driver.version>
		<mybatis.version>3.3.0</mybatis.version>
		<mybatis-spring.version>1.2.2</mybatis-spring.version>
		<spring.version>5.1.1.RELEASE</spring.version>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<autoconfig-plugin-version>1.2</autoconfig-plugin-version>
		<maven.test.skip>true</maven.test.skip>
	</properties>

	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>${spring.version}</version>
			<scope>test</scope>
		</dependency>
        
		<!-- commons 包依赖 -->
		<dependency>
			<groupId>commons-logging</groupId>
			<artifactId>commons-logging</artifactId>
			<version>1.1.3</version>
		</dependency>
		<dependency>
			<groupId>commons-collections</groupId>
			<artifactId>commons-collections</artifactId>
			<version>3.2.1</version>
		</dependency>
		<dependency>
			<groupId>commons-io</groupId>
			<artifactId>commons-io</artifactId>
			<version>2.4</version>
		</dependency>
		<dependency>
			<groupId>commons-lang</groupId>
			<artifactId>commons-lang</artifactId>
			<version>2.6</version>
		</dependency>
		<!-- 数据库包依赖 -->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>${mybatis.version}</version>
		</dependency>
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis-spring</artifactId>
			<version>${mybatis-spring.version}</version>
		</dependency>
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>${jdbc.driver.version}</version>
			<scope>runtime</scope>
		</dependency>
		<!-- 日志包依赖 -->
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-api</artifactId>
			<version>1.7.7</version>
		</dependency>
		<dependency>
			<groupId>org.slf4j</groupId>
			<artifactId>slf4j-log4j12</artifactId>
			<version>1.7.7</version>
		</dependency>
		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.16</version>
		</dependency>

		<!-- aspectj 包依赖 -->
        <dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjrt</artifactId>
			<version>1.7.4</version>
		</dependency>
		<dependency>
			<groupId>org.aspectj</groupId>
			<artifactId>aspectjweaver</artifactId>
			<version>1.7.4</version>
		</dependency>

				<!-- Spring 依赖 -->
        <dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-core</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-beans</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context-support</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-jdbc</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>${spring.version}</version>
		</dependency>
		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-webmvc</artifactId>
			<version>${spring.version}</version>
		</dependency>

		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>javax.servlet-api</artifactId>
			<version>3.1.0</version>
			<scope>provided</scope>
		</dependency>
		<dependency>
			<groupId>javax.servlet.jsp</groupId>
			<artifactId>jsp-api</artifactId>
			<version>2.2</version>
		</dependency>
		<dependency>
			<groupId>javax.servlet</groupId>
			<artifactId>jstl</artifactId>
			<version>1.2</version>
		</dependency>

		<dependency>
			<groupId>net.sf.json-lib</groupId>
			<artifactId>json-lib</artifactId>
			<version>2.2.3</version>
			<classifier>jdk15</classifier>
		</dependency>
		<dependency>
			<groupId>com.fasterxml.jackson.core</groupId>
			<artifactId>jackson-databind</artifactId>
			<version>2.9.7</version>
		</dependency>

		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>fastjson</artifactId>
			<version>1.2.4</version>
		</dependency>

		<dependency>
			<groupId>commons-fileupload</groupId>
			<artifactId>commons-fileupload</artifactId>
			<version>1.3.1</version>
		</dependency>
        <dependency>
			<groupId>org.apache.geronimo.specs</groupId>
			<artifactId>geronimo-servlet_2.4_spec</artifactId>
			<version>1.1.1</version>
			<scope>provided</scope>
		</dependency>

		<dependency>
			<groupId>commons-codec</groupId>
			<artifactId>commons-codec</artifactId>
			<version>1.9</version>
		</dependency>
		<dependency>
			<groupId>org.json</groupId>
			<artifactId>json</artifactId>
			<version>20160810</version>
		</dependency>
		<!-- ueditor 依赖 -->
		<dependency>
			<groupId>com.baidu</groupId>
			<artifactId>ueditor</artifactId>
			<version>1.1.2</version>
		</dependency>
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>1.0.24</version>
		</dependency>
	</dependencies>
  <build>
    <plugins>
      <plugin>
	  <!--jetty maven插件，为maven提供运行web程序的能力-->
        <groupId>org.eclipse.jetty</groupId>
        <artifactId>jetty-maven-plugin</artifactId>
        <version>9.4.12.v20180830</version>
        <configuration>
            <scanIntervalSeconds>10</scanIntervalSeconds>
            <webApp>
                <contextPath>/</contextPath>
            </webApp>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

## 四、实验总结

到这里我们就完成了整个项目的环境搭建，包括用 Maven 引入 jar 包，下一节我们将完成数据库的设计实现。

