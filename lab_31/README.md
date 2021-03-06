---
sort: 31
---

# 拦截器及 Spring MVC 整合

## 一、实验介绍

#### 1.1 实验内容

本节课程主要利用 Spring MVC 框架实现拦截器以及 Spring MVC 框架的整合。

#### 1.2 实验知识点

- Spring MVC 框架
- 拦截器

#### 1.3 实验环境

- JDK1.8
- WEB IDE

## 二、实验步骤

这一章节中我们将学习拦截器以及springmvc的整合。

### 2.1 拦截器实现

在项目 `hrms` 的目录 `src/main/java` 下新建包 `com.shiyanlou.interceptor`，并在该包下新建类 `LoginInterceptor`，来验证用户是否登录，代码如下：

```java
package com.shiyanlou.interceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

public class LoginInterceptor implements HandlerInterceptor {
	
	// 不拦截 "/login" 请求
	private static final String[] IGNORE_URI = { "/login" };

	@Override
	public void afterCompletion(HttpServletRequest arg0,
			HttpServletResponse arg1, Object arg2, Exception arg3)
			throws Exception {

	}

	@Override
	public void postHandle(HttpServletRequest arg0, HttpServletResponse arg1,
			Object arg2, ModelAndView arg3) throws Exception {

	}
	
	// 该方法将在 Controller 处理前进行调用
	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response,
			Object handler) throws Exception {
		// flag 表示是否登录
		boolean flag = false;
		// 获取请求的 URL
		String url = request.getServletPath();
		for (String s : IGNORE_URI) {
			if (url.contains(s)) {
				flag = true;
				break;
			}
		}
		if (!flag) {
			// 获取 Session 并判断是否登录
			String username = (String) request.getSession().getAttribute(
					"username");
			if (username == null) {
				request.setAttribute("message", "Please log in first!");
				// 如果未登录，进行拦截，跳转到登录页面
				request.getRequestDispatcher("/login.jsp")
						.forward(request, response);
			} else {
				flag = true;
			}
		}
		return flag;
	}

}

```

```checker
- name: check LoginInterceptor exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/interceptor/LoginInterceptor.java
  error: 我们发现您还没有创建LoginInterceptor.java! /home/project/hrms/src/main/java/com/shiyanlou/interceptor/LoginInterceptor.java
  timeout: 1
```

### 2.2 spring-mvc.xml 配置文件

在目录 `src/main/resources` 下新建 Spring MVC 配置文件 `spring-mvc.xml`，添加如下代码：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
	xmlns:mvc="http://www.springframework.org/schema/mvc"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context-4.2.xsd
      http://www.springframework.org/schema/mvc
     http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd">

	<!-- 自动扫描该包，Spring MVC 会将包下用 @Controller 注解的类注册为 Spring 的 controller -->
	<context:component-scan base-package="com.shiyanlou.controller" />
	<!-- 设置默认配置方案 -->
	<mvc:annotation-driven />
	<!-- 静态资源访问 -->
	<mvc:resources location="/" mapping="/**" />
	<!-- 视图解析器 -->
	<bean id="viewResolver"
		class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/" />
		<property name="suffix" value=".jsp" />
	</bean>

	<bean id="multipartResolver"
		class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
		<property name="maxUploadSize" value="3500000" />
		<property name="defaultEncoding" value="UTF-8" />
	</bean>
	<!-- 配置拦截器 -->
	<mvc:interceptors>
        <mvc:interceptor>
            <mvc:mapping path="/*" />
            <bean class="com.shiyanlou.interceptor.LoginInterceptor" />
        </mvc:interceptor>
    </mvc:interceptors>
</beans>
```

```checker
- name: check spring mvc exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/resources/spring-mvc.xml
  error: 我们发现您还没有创建spring-mvc.xml! /home/project/hrms/src/main/resources/spring-mvc.xml
  timeout: 1
```

### 2.3 配置 web.xml

修改项目 `hrms` 的 `src->main->webapp->WEB-INF` 目录下的 `web.xml` 内容如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns="http://xmlns.jcp.org/xml/ns/javaee"
	xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
	id="WebApp_ID" version="3.1">
	<display-name>hrms</display-name>
	<!-- 配置 Spring 核心监听器 -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

	<!-- 指定 Spring 的配置文件 -->
	<context-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:spring-mybatis.xml</param-value>
	</context-param>

	<!-- 定义 Spring MVC 前端控制器 -->
	<servlet>
		<servlet-name>springMVC</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:spring-mvc.xml</param-value>
		</init-param>
		<load-on-startup>1</load-on-startup>
	</servlet>
	
	<!-- 为 DispatcherServlet 建立映射 -->
	<servlet-mapping>
		<servlet-name>springMVC</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
	<listener>
		<listener-class>org.springframework.web.util.IntrospectorCleanupListener</listener-class>
	</listener>
	
	<!-- 编码过滤器 -->
	<filter>
		<filter-name>encodingFilter</filter-name>
		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
		<init-param>
			<param-name>encoding</param-name>
			<param-value>UTF-8</param-value>
		</init-param>
		<init-param>
			<param-name>forceEncoding</param-name>
			<param-value>true</param-value>
		</init-param>
	</filter>

	<filter-mapping>
		<filter-name>encodingFilter</filter-name>
		<url-pattern>/*</url-pattern>
	</filter-mapping>
	
	<!-- 设置首页 -->
	<welcome-file-list>
		<welcome-file>login.jsp</welcome-file>
	</welcome-file-list>
</web-app>
```

```checker
- name: check web xml exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/webapp/WEB-INF/web.xml
  error: 我们发现您还没有创建web.xml! /home/project/hrms/src/main/webapp/WEB-INF/web.xml
  timeout: 1
```

## 三、实验总结

到这里我们就实现了拦截器和 Spring MVC 的整合，项目的代码全部完成，下一节我们将对实现的系统进行运行测试。