---
sort: 28
---

# Spring 与 MyBatis 整合

## 一、实验介绍

#### 1.1 实验内容

本节课程将整合 Spring 和 MyBatis，并采用 Junit 进行单元测试。

#### 1.2 实验知识点

- Spring - MyBatis 整合
- Junit 单元测试

#### 1.3 实验环境

- JDK1.8
- WEB IDE

## 二、实验步骤

在这一章节中我们将学习如何将mybatis和spring整合。

### 2.1 log4j.properties 日志文件

在目录 `src/main/resources` 下新建日志文件 `log4j.properties` ，在里面添加如下内容：

```
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```

```checker
- name: check log4j properties exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/resources/log4j.properties
  error: 我们发现您还没有创建log4j.properties! /home/project/hrms/src/main/resources/log4j.properties
  timeout: 1
```

### 2.2 jdbc.properties 属性文件

在目录 `src/main/resources` 下新建 JDBC 属性文件 `jdbc.properties` ，在里面添加如下内容：

```
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/hrms_db
jdbc.username=root
jdbc.password=
```

```checker
- name: check jdbc properties exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/resources/jdbc.properties
  error: 我们发现您还没有创建jdbc.properties! /home/project/hrms/src/main/resources/jdbc.properties
  timeout: 1
```

### 2.3 spring-mybatis.xml 配置文件

在目录 `src/main/resources` 下新建 `spring-mybatis.xml` 配置文件，该文件用来完成 Spring 和 MyBatis 的整合，主要包括了自动扫描，自动注入，配置数据库等，内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="
          http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
          http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context-4.2.xsd
          http://www.springframework.org/schema/tx
          http://www.springframework.org/schema/tx/spring-tx-4.2.xsd">
	
	<!-- 自动扫描有 Spring 相关注解的类，并把这些类注册为 bean --> 
	<context:component-scan base-package="com.shiyanlou" />

	<context:property-placeholder location="classpath:jdbc.properties" />
	<!-- 配置数据源 -->
	<bean id="dataSource"
		class="org.springframework.jdbc.datasource.DriverManagerDataSource">
		<property name="driverClassName" value="${jdbc.driver}" />
		<property name="url" value="${jdbc.url}" />
		<property name="username" value="${jdbc.username}" />
		<property name="password" value="${jdbc.password}" />
	</bean>
	<!-- MyBatis 的 SqlSession 的工厂，并引用数据源，扫描 MyBatis 的配置文件 -->
	<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
		<property name="dataSource" ref="dataSource"></property>
		<property name="mapperLocations" value="classpath:/mappers/*.xml"></property>
		<property name="configLocation" value="classpath:mybatis-config.xml" />
	</bean>
	<!-- MyBatis 自动扫描加载 Sql 映射文件/接口 -->
	<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer"
		p:basePackage="com.shiyanlou.dao" p:sqlSessionFactoryBeanName="sqlSessionFactory">

	</bean>
	<!-- JDBC 事务管理器 -->
	<bean id="txManager"
		class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"></property>
	</bean>
	<!-- 启用支持 annotation 注解方式事务管理 -->
	<tx:annotation-driven transaction-manager="txManager" />

</beans>
```

到这里，Spring 和 MyBatis 就整合完成，下面我们通过一个测试类测试整合是否成功。

```checker
- name: check spring mybatis xml exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/resources/spring-mybatis.xml
  error: 我们发现您还没有创建spring-mybatis.xml! /home/project/hrms/src/main/resources/spring-mybatis.xml
  timeout: 1
```

### 2.4 Junit 单元测试

在项目 `hrms` 的目录 `src/test/java` 下新建包 `com.shiyanlou.test`，并在该包下新建测试类 `SpringMybatisTest`，添加如下代码：

```java
package com.shiyanlou.test;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import com.shiyanlou.domain.Admin;
import com.shiyanlou.service.AdminService;

/**
 * 配置spring和junit整合，junit启动时加载springIOC容器 spring-test,junit
 **/
@RunWith(SpringJUnit4ClassRunner.class)
// 告诉junit spring 配置文件
@ContextConfiguration({ "classpath:spring-mybatis.xml" })
public class SpringMybatisTest {
	
	@Autowired
	private AdminService adminService;
	
	@Test
	public void testLogin() {
		Admin admin = new Admin();
		admin.setUsername("superadmin");
		admin.setPassword("123456");
		System.out.println(adminService.login(admin).toString());

	}
}
```
在这里只测试了登录功能，目的是测试 Spring 与 MyBatis 是否整合成功，其他可以自行测试。


以 `mvn test` 方式运行 `SpringMybatisTest` 测试类。打开终端，输入`mvn test`(注意：需要将pom.xml中的<maven.test.skip>true</maven.test.skip>修改为<maven.test.skip>false</maven.test.skip>，否则会跳过测试)  

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540272281925.png)

测试结果：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540272294448.png)

从测试结果可以看出 Spring 和 MyBatis 已经成功整合。

```checker
- name: check spring mybatis test exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/test/java/com/shiyanlou/test/SpringMybatisTest.java
  error: 我们发现您还没有创建SpringMybatisTest.java! /home/project/hrms/src/test/java/com/shiyanlou/test/SpringMybatisTest.java
  timeout: 1
```

## 三、实验总结

到这里我们就完成了 Spring 与 MyBatis 的整合，下一节我们将进入 Controller 层的实现。

