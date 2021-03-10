---
sort: 21
---

# Java实现个人博客网站

## 一、实验介绍

#### 1.1 实验内容

本次实验的内容是利用 SSM 框架和 Mysql 以及一些简单的前端知识搭建一个自己的个人博客网站，网站功能包括写博客和日记，浏览博客与日记，以及作为网站拥有者的我们对博客和日记的管理。

#### 1.2 实验知识点

1. Spring MVC
2. Spring
3. Mybatis
4. CSS/JS
5. Jquery

#### 1.3 实验环境

1. WEB IDE
2. Mysql
3. SSM 框架所需 Jar 包
4. MarkDown 编辑器 EditorMd
5. Jetty9

#### 1.4 源码获取

可以通过以下命令获取本次课程实验的源码：

```shell
wget http://labfile.oss.aliyuncs.com/courses/930/PersonalBlog.zip
```

## 二、项目结构

![项目结构](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540370288443.png)  
![](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540370286225.png)


> 关于在线环境的使用遇到任何问题请查阅使用指南：[https://www.shiyanlou.com/questions/89126](https://www.shiyanlou.com/questions/89126)

## 三、实验步骤

接下来我们将开始对个人博客项目进行开发。

>注：当前环境无法保存，需要保存的同学请自行通过Git保存-[Github 快速上手实战教程](https://www.shiyanlou.com/courses/868)

### 3.1 开发准备

开发准备包括项目创建、JAR 包的导入、第三方插件的下载、页面所需图片下载以及和创建数据库。

#### 3.1.1 项目创建

首先打开WEB IDE，选择 Terminal->Open New Terminal，在终端中输入：  

```
mvn archetype:generate \
  -DgroupId=com.shiyanlou \
  -DartifactId=PersonalBlog \
  -DarchetypeArtifactId=maven-archetype-webapp
```
参数说明：

- Group Id：项目的组织机构，也是包的目录结构，一般都是域名的倒序，比如  `com.shiyanlou`；
- Atifact Id ：项目实际的名字，比如 `PersonalBlog`；
- archetype Artifact Id ：使用的maven骨架名称

输入命令之后，maven会提示我们输入版本号，这里可以直接定义版本号也可以直接回车，接着maven会提示当前要创建项目的基本信息，输入y然后回车确认。
接着在`src/main/`目录中创建`java`目录。

![](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540360973720.png)

#### 3.1.2 Jar 包导入

打开pom.xml文件，修改为下面的内容：  

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.shiyanlou</groupId>
    <artifactId>PersonalBlog</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

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
    </dependencies>
    <build>
        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </resource>
                <resource>
                <directory>src/main/resources</directory>
                <includes>
                    <include>**/**</include>
                </includes>
            </resource>
        </resources>
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


#### 3.1.3 MarkDown 编辑器

本次实验项目我们需要使用 markdown 文档编辑来实现写博客和日记的功能。我们选择的是开源的 markdown 编辑器 Editor.md
。打开终端（File -> Open New Terminal），使用命令从内网中下载，然后解压，解压完成后，将其复制至项目的 `webapp` 目录下，方便之后的使用。

```shell
wget http://labfile.oss.aliyuncs.com/courses/930/EditorMd.zip
unzip EditorMd.zip -d ./src/main/webapp/
rm EditorMd.zip -f
```

`注意：`上面的命令必须在/home/project/hrms目录下执行，否则解压的文件不能到webapp目录下

#### 3.1.4 图片下载

本次页面上使用的图片有两张，包括 logo 和在主页上使用的另一张图片。图片可以通过以下命令获得,然后解压，解压完成后将其同样移动到 `webapp` 目录下。

```shell
wget http://labfile.oss.aliyuncs.com/courses/930/img.zip
unzip img.zip -d ./src/main/webapp/
rm img.zip -f
```

`注意：`上面的命令必须在/home/project/hrms目录下执行，否则解压的文件不能到webapp目录下

#### 3.1.5 创建数据库

首先终端，然后输入命令：

```shell
sudo service mysql start
```

打开 mysql 服务，然后需要使用 mysql 账号和密码进入数据库进行操作由于在线花环境中的 mysql 默认没有密码，那么可以使用命令

```shell
mysql -u root
```

进入数据库，然后新建一个数据库供本次实验使用，数据库名为 personalblog ，命令如下：

```
create database personalblog;
```

新建成功后，在该数据库下建表。首先要选中数据库：

```
use personalblog;
```

然后建表：
博客表 blog

```
CREATE TABLE `blog` (
  `blogid` int(20) NOT NULL AUTO_INCREMENT,
  `blogtitle` varchar(50) DEFAULT NULL,
  `article` varchar(10000) DEFAULT NULL,
  `time` varchar(50) DEFAULT NULL,
  PRIMARY KEY (`blogid`)
) ENGINE=InnoDB AUTO_INCREMENT=19 DEFAULT CHARSET=utf8;
```

插入一条博客数据

```
insert into blog(blogtitle,article,time) values("The first article","This is the first article to be used for testing",2017-06-01);
```

日记表 diary

```
CREATE TABLE `diary` (
  `diaryid` int(20) NOT NULL AUTO_INCREMENT,
  `diary` varchar(10000) DEFAULT NULL,
  `time` varchar(20) DEFAULT NULL,
  PRIMARY KEY (`diaryid`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;
```

#### 3.1.6 实体类

实体类对应数据库的表，所有实体类在 com.personalblog.model 包下。


blog 实体类：

```java
package com.personalblog.model;

public class Blog {
	private int blogid;
	private String blogtitle;
	private String article;
	private String time;
	public int getBlogid() {
		return blogid;
	}
	public void setBlogid(int blogid) {
		this.blogid = blogid;
	}
	public String getBlogtitle() {
		return blogtitle;
	}
	public void setBlogtitle(String blogtitle) {
		this.blogtitle = blogtitle;
	}

	public String getArticle() {
		return article;
	}
	public void setArticle(String article) {
		this.article = article;
	}
	public String getTime() {
		return time;
	}
	public void setTime(String time) {
		this.time = time;
	}
	
	
}
```

diary 实体类：

```java
package com.personalblog.model;

public class Diary {
	private int diaryid;
	private String diary;
	private String time;
	public int getDiaryid() {
		return diaryid;
	}
	public void setDiaryid(int diaryid) {
		this.diaryid = diaryid;
	}
	public String getDiary() {
		return diary;
	}
	public void setDiary(String diary) {
		this.diary = diary;
	}
	public String getTime() {
		return time;
	}
	public void setTime(String time) {
		this.time = time;
	}
}
```

以上，我们的开发准备工作就完成了。

```checker
- name: check PersonalBlog exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog
  error: 我们发现您还没有创建项目PersonalBlog! /home/project/PersonalBlog
  timeout: 1
- name: check Blog java exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/java/com/personalblog/model/Blog.java
  error: 我们发现您还没有创建Blog.java! /home/project/PersonalBlog/src/main/java/com/personalblog/model/Blog.java
  timeout: 1
- name: check Diary java exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/java/com/personalblog/model/Diary.java
  error: 我们发现您还没有创建Diary.java! /home/project/PersonalBlog/src/main/java/com/personalblog/model/Diary.java
  timeout: 1
```

### 3.2 前端页面

本次课程的前端页面是使用 jsp 编写的。另外，还有一些简单的 css 样式和 js。

#### 3.2.1 css/js

本次课程实验项目的 css 样式文件位于 `webapp` 的 `css` 文件夹下，包括有 index.css、about.css、saylist.css 三个样式表。其中 index.css 样式表具体信息如下：

```css
* { margin: 0; padding: 0 }
body { font: 12px "宋体", Arial, Helvetica, sans-serif; color: #756F71 }
img { border: 0; display: block }
ul { list-style: none; }
a:link, a:visited {text-decoration: none; color: #333;}
.left { float: left; }
.right { float: right; }
.blank { height: 5px; overflow: hidden; width: 100%; margin: auto; clear: both }
.box{ width:1000px; margin:auto; overflow:hidden}
header { width: 1000px; margin: auto; height: 80px; position: relative; overflow: hidden }
#logo a { width:310px; height: 60px; margin: 10px 0 0 0; position: absolute; background: url(../img/logo.jpg) no-repeat; display: block }
nav { float: right; width: 100%; margin: 40px 0 0 0; text-align: right }
nav a { position: relative; display: inline-block; font-size: 18px; font-family: "", Arial, Helvetica, sans-serif; }
nav a:hover { text-decoration: none }
.topnav a { margin: 0 5px; padding: 0 8px; }
.topnav a span:first-child { z-index: 2; display: block; }
.topnav a span:last-child { z-index: 1; display: block; color: #999; font: 12px Georgia, serif; opacity: 0; -webkit-transition: -webkit-transform 0.3s, opacity 0.3s; -moz-transition: -moz-transform 0.3s, opacity 0.3s; transition: transform 0.3s, opacity 0.3s; -webkit-transform: translateY(-100%); -moz-transform: translateY(-100%); transform: translateY(-100%); text-align: center }
.topnav a:hover span:last-child, .topnav a:focus span:last-child { opacity: 1; -webkit-transform: translateY(0%); -moz-transform: translateY(0%); transform: translateY(0%); }
#topnav_current { color: #e15782; }
.en { color: #999; font-size: 12px; z-index: 1; display: block; }/* ie */
article { width: 1000px; margin: 20px auto; overflow: hidden }
aside { width: 250px; }
footer { text-align: center; line-height: 40px; border-top: #E8E8E8 1px solid ; width:1000px; margin:0 auto;}
footer p span{padding-left:10px;color:#292627;}
footer p span a{color:#292627;}
.clear{clear:both;}
.banner { background: url(../img/b2.jpg) no-repeat;width:1000px;height:345px;margin:0 auto;overflow: hidden;border-radius:5px;position:relative;}
h2.title{color: crimson; }
/*wz*/
.bloglist { width: 1000px; overflow: hidden; background: url(../images/r_line.jpg) repeat-y right; }
.bloglist .wz{width:1000px;bodrder:1px solid red;}
.wz h1 { margin: 8px 0px 4px 2px; color:#D6533A;font-size: 20px}
.wz ul { float: left; width: 1000px; margin: 10px 0px 0 2px; line-height: 20px; font-size:20px}
.dateview { width: 695px; overflow: hidden; clear: both; margin: 10px 0 0 0; display: inline-block; background: #f6f6f6 url(../images/time.jpg) 1px center no-repeat; line-height: 26px; height: 26px; color: #838383; padding-left:8px }
.dateview span { margin: 0 10px; }
.dateview span a { color: #099B43; }
a.readmore { background: #fd8a61; color: #fff; padding: 5px 10px; float: right; margin: 20px 0 0 0;border-radius:3px; }
.gut(width:1000px)
.admin{dispaly: inline-block;text-align: center;}

.blogtitle{font-family: "微软雅黑";font-size:18px;float: left;}
/*end*/
/*markdown*/
#text {letter-spacing:2px;line-height:50px}
```
其功能包括设置页面的头部导航栏、显示logo、控制文章显示格式等。

about.css 的功能是设置博主个人相关信息页面的样式，包括字体等样式。其具体信息如下：

```css
@charset "utf-8";
/*about*/
.aboutcon{ color:#696969; }
h1.t_nav {  font-size: 15px; font-weight: normal; line-height: 40px; height: 40px; }
h1.t_nav span{float:right; color:#DC143C}
.about{width: 740px;overflow: hidden; line-height:22px;float: left;}
.about h2{font-family:"comic sans ms";font-size: 30px;color:blue;}
.about #me_text{width: 1000px; font-family: "微软雅黑";font-size: 17px;overflow: hidden;}
#me_text p{width:700px;padding:6px 20px;text-indent:2em;font-size:16px;}
.about_c,.about{margin:4px 0}
.about ul{ width:710px}
.about_c{ padding:10px 0 0 10px; color:#242222; line-height:26px; }


```

saylist.css 的作用是设置日记显示格式，包括日记显示框和日期显示格式等。其具体代码信息如下：

```css
/*个人日记*/
.moodlist { margin: auto; width: 100%; overflow: hidden }
h1.t_nav span{ float:right; color:red}
h1.t_nav { border-bottom: #F1F1F1 1px solid; font-size: 20px; font-weight: normal; line-height: 40px; height: 40px;margin:20px auto;width:1000px }
h1.t_nav a { width: 100px; display: block; text-align: center; color: #fff; float: left }
.bloglist { width: 1000px; margin: 0 auto;repeat-y 764px 0;overflow: hidden; }
.arrow_box { background: #f8f8f8; box-shadow: 0px 1px 0px, inset 0px 1px 1px rgba(214, 214, 214, 0.7); width: 730px;  position: relative; padding: 20px 0; margin: 20px 0; }
.arrow_box img { width: 150px; float: left; margin: 0 20px 0 20px; }
.arrow_box p { line-height: 24px; padding: 0 20px 20px }
.arrow_box:hover { background: #f4f2f2; color: #333; text-shadow: #f7f7f7 1px 1px 1px }
.dateview { position: absolute; left: 788px; top: 20px; width: 125px; line-height: 30px; background: #5EA51B; border-radius: 0px 40px; text-align: center; color: #fff }
```

```checker
- name: check index css exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/css/index.css
  error: 我们发现您还没有创建index.css! /home/project/PersonalBlog/src/main/webapp/css/index.css
  timeout: 1
- name: check about css exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/css/about.css
  error: 我们发现您还没有创建about.css! /home/project/PersonalBlog/src/main/webapp/css/about.css
  timeout: 1
- name: check saylist css exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/css/saylist.css
  error: 我们发现您还没有创建saylist.css! /home/project/PersonalBlog/src/main/webapp/css/saylist.css
  timeout: 1
```

js 文件目录位于 `webapp` 下新建的 js 文件夹下，包括控制显示头部标签 nav 的显示格式的 nav.js、验证管理员身份的 alert.js、以及 jquery 的函数库 jquery-3.2.1.main.js ,以及渲染markdown的showdown.js。其中 nav.js 的代码是：

```js
var obj = null;
var As = document.getElementById('topnav').getElementsByTagName('a');
obj = As[0];
for (i = 1; i < As.length; i++) {
    if (window.location.href.indexOf(As[i].href) >= 0)
        obj = As[i];
}
obj.id = 'topnav_current'
```

alert.js 的具体信息如下：

```js
function fun1() {
    var result = confirm("需要验证您的管理员身份");
    if (result) {//true
        var password = prompt("请输入你的管理员密码：");
        if (password == "000000") {
            window.location.href = "selectAllBlog2"
        }
    } else { //false
        window.history.back(-1)
    }
}
```

jquery 的函数库可以通过 jquery 官网下载获得，这里我们已将最新版的函数库上传到了服务器上，可以通过命令：

```shell
wget http://labfile.oss.aliyuncs.com/courses/930/jquery-3.2.1.min.js
```

下载获得。下载完成后将其复制到 js 文件夹下即可。

```shell
mv jquery-3.2.1.min.js ./src/main/webapp/js
```

接下来再下载showdown.js放到js文件夹下  

```shell
wget http://labfile.oss.aliyuncs.com/courses/930/showdown.min.js
mv showdown.min.js ./src/main/webapp/js
```

```checker
- name: check nav js exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/js/nav.js
  error: 我们发现您还没有创建nav.js! /home/project/PersonalBlog/src/main/webapp/js/nav.js
  timeout: 1
- name: check alert js exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/js/alert.js
  error: 我们发现您还没有创建alert.js! /home/project/PersonalBlog/src/main/webapp/js/alert.js
  timeout: 1
- name: check jq js exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/js/jquery-3.2.1.min.js
  error: 我们发现您还没有引入jquery-3.2.1.min.js! /home/project/PersonalBlog/src/main/webapp/js/jquery-3.2.1.min.js
  timeout: 1
```

#### 3.2.2 blogindex.jsp

blogindex.jsp 是项目的欢迎页面，它的作用是访问这个页面时，自动加载 js，执行查询博客文章的操作，然后跳转到真正的主页 index.jsp ，起着一个中介的作用。设计这个页面是为了避免之后需要通过按钮或链接等跳转到主页时可能会出现循环跳转的情况。其具体代码信息如下：

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
    <script type="text/javascript" src="js/jquery-3.2.1.min.js"></script>
    <title>Insert title here</title>
    <script>
        $(function () {
            window.location.href = 'selectAllBlog';
        })
    </script>
</head>
<body>

</body>

</html>
```

#### 3.2.3 index.jsp

index.jsp 是网站主页，显示的是全部的博客文章，并且提供显示选择文章的具体信息的链接。其具体代码如下：

```html
<%@ page language="java" contentType="text/html; charset=utf-8"
         pageEncoding="utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>index</title>
    <link rel="stylesheet" type="text/css" href="css/index.css"/>
    <script type="text/javascript" src="js/jquery-3.2.1.min.js"></script>
</head>
<body>
<header>
    <div id="logo"><a href="/"></a></div>
    <nav class="topnav" id="topnav">
        <a href="selectAllBlog"><span>Home</span><span class="en">主页</span></a>
        <a href="about.jsp"><span>About</span><span class="en">关于我</span></a>
        <a href="selectAllDiary"><span>Diary</span><span class="en">日记心得</span></a>
        <a href="#" onclick="fun1()"><span>Admin</span><span class="en">管理</span></a>
    </nav>
</header>
<div class="box">
    <div class="banner"></div>
    <br>
    <h2 class="title">文章列表</h2>
    <div class="bloglist">
        <div class="wz">
            <c:forEach items="${blogs}" var="blogs">
                <h3><c:out value="${blogs.blogtitle}"/></h3>

                <ul>

                    <a title="阅读全文" href="selectBlogById?blogid=<c:out value="${blogs.blogid}"/>" class="readmore">阅读全文>></a>
                </ul>
            </c:forEach>
            <div class="clear"></div>
        </div>
    </div>
</div>
</body>
<script type="text/javascript" src="js/alert.js"></script>
</html>

```

#### 3.2.4 blog.jsp

blog.jsp 是用于显示博客文章具体信息的页面。主要作用是将 markdown 的文档转化为html，然后通过简单的 css 将其排版显示在页面上。其具体代码信息如下：

```html
<%@ page language="java" contentType="text/html; charset=utf-8"
         pageEncoding="utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
    <title>blog</title>
    <link rel="stylesheet" type="text/css" href="css/index.css"/>
    <script type="text/javascript" src="js/jquery-3.2.1.min.js"></script>
    <script src="js/showdown.min.js"></script>
</head>
<body>
<header>
    <div id="logo"><a href="/"></a></div>
    <nav class="topnav" id="topnav">
        <a href="selectAllBlog"><span>Home</span><span class="en">主页</span></a>
        <a href="about.jsp"><span>About</span><span class="en">关于我</span></a>
        <a href="selectAllDiary"><span>Diary</span><span class="en">日记心得</span></a>
        <a href="#" onclick="fun1()"><span>Admin</span><span class="en">管理</span></a>
    </nav>
</header>
<div class="box">
    <div class="banner"></div>
    <br>
    <h2 class="title"><c:out value="${blog.blogtitle}"></c:out></h2>
    <input type="hidden" id="blogarticle" value="${blog.article}">
    <div class="bloglist">
        <div class="wz">
            <script type="text/javascript">
                $(function () {
                    var converter = new showdown.Converter();
                    var x = document.getElementById('blogarticle').value;
                    var html = converter.makeHtml(x);
                    document.getElementById('text').innerHTML = html;
                });
            </script>
            <div id="text"></div>
            <div class="clear"></div>
        </div>
    </div>
</div>
</body>
<script type="text/javascript" src="js/alert.js"></script>
</html>
```

这里使用的是 showdown.js 插件来实现的 markdown 向 html 转化。

#### 3.2.5 about.jsp

about.jsp 是一个没有和后台有交互的页面，功能是显示关于博主的信息。其具体代码信息如下：

```html
<%@ page language="java" contentType="text/html; charset=utf-8"
         pageEncoding="utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>Insert title here</title>
    <link href="css/index.css" rel="stylesheet"/>
    <link href="css/about.css" rel="stylesheet"/>
    <script type="text/javascript" src="js/alert.js"></script>
</head>
<body>
<header>
    <div id="logo"><a href="/"></a></div>
    <nav class="topnav" id="topnav">
        <a href="selectAllBlog"><span>Home</span><span class="en">主页</span></a>
        <a href="about.jsp"><span>About</span><span class="en">关于我</span></a>
        <a href="selectAllDiary"><span>Diary</span><span class="en">日记心得</span></a>
        <a href="#" onclick="fun1()"><span>Admin</span><span class="en">管理</span></a>
    </nav>
</header>
<article class="aboutcon">
    <h1 class="t_nav"><span>信念和目标促使我努力前进。</span></h1><br/>
    <div class="about">
        <h2>About me</h2><br/>
        <div id="me_text">
            <p> 一个不断学习和研究，web前端和Java的普通人。</p>
            <p>很多时候觉得自己对待任何事都很执着，一旦定了目标，就会不达目标不罢休，永不退缩！
                面对爱情也是一样，当真正遇到自己喜欢的一个人，会不顾一切去追。</p>
            <p>
                在学习这条路上，最大的收获就是：自己对待人生观和价值观有了自己独特的看法。我是一个对理想有着执着追求的人，坚信是金子总会发光。大学毕业后的工作，让我在文案策划方面有了很大的提高，文笔流畅，熟悉传媒工作、广告学制作与设计等工作方面。为人热情，活泼，大方，
                本人好学上进，诚信、敬业、
                责任心强，有强烈的团体精神，对工作认真积极，严谨负责。</p>
            <p>感谢那些曾经帮助过我的人，因为有你们我才会变得如此的优秀。----By:xxx</p>
        </div>

    </div>
    <aside class="right">
        <div class="about_c">
            <br/>
            <p>网名：非一般的感觉</p>
            <p>姓名：<a href="wwww.duanliang920.com" target="_blank">xxx</a></p>
            <p>星座：xxx座</p>
            <p>现居：xx省-xx市</p>
            <p>博客：<a href="xx" target="_blank">xx.blog</a></p>
            <p>喜欢的书：《我的互联网方法论》..</p>
            <p>喜欢的音乐：《一生中最爱》..</p>
        </div>
    </aside>
</article>
<footer>
    <p><span>Design By:<a href="www.duanliang920.com" target="_blank">xxx</a></span><span>网站地图</span><span><a href="/">网站统计</a></span>
    </p>
</footer>
<script src="js/nav.js"></script>
</body>
<script type="text/javascript" src="js/alert.js"></script>
</html>

```


#### 3.2.6 admin.jsp

admin.jsp 是管理端的主页，验证管理员身份通过后会跳转到此页面（一般个人博客的管理都是隐藏起来的，但是在本次课程中将管理功能显示出来，只需要验证通过即可进行管理），其布局结构与主页相似，但是头部导航栏显示的是管理功能，其具体代码信息如下：

```html
<%@ page language="java" contentType="text/html; charset=utf-8"
         pageEncoding="utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>Admin</title>
    <link href="css/index.css" rel="stylesheet">
</head>
<body>
<header>
    <div id="logo"><a href="/"></a></div>
    <nav class="topnav" id="topnav">
        <a href="selectAllBlog"><span>Home</span><span class="en">主页</span></a>
        <a href="adminblog"><span>Admin Blog</span><span class="en">管理博客</span></a>
        <a href="admindiary"><span>Admin Diary</span><span class="en">管理日记</span></a>
        <a href="writeblog.jsp"><span>Write Blog</span><span class="en">写博客</span></a>
        <a href="writediary.jsp"><span>Write Diary</span><span class="en">写日记</span></a>
    </nav>
</header>
<div class="box">
    <div class="banner"></div>
    <br>
    <h2 class="title">文章列表</h2>
    <div class="bloglist">
        <div class="wz">
            <c:forEach items="${blogs}" var="blogs">
                <h3><c:out value="${blogs.blogtitle}"/></h3>

                <ul>

                    <a title="阅读全文" href="selectBlogById?blogid=<c:out value="${blogs.blogid}"/>" class="readmore">阅读全文>></a>
                </ul>
            </c:forEach>
            <div class="clear"></div>
        </div>
    </div>
</div>
</body>
<script type="text/javascript" src="js/alert.js"></script>
</html>

```

#### 3.2.7 writeblog.jsp

writeblog.jsp 是用于写博客的页面，在本页面中使用 EditorMd 插件来提供写博客的工具。其具体代码信息如下：

```html
<%@ page language="java" contentType="text/html; charset=utf-8"
         pageEncoding="utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>Write Blog</title>
    <link rel="stylesheet" href="EditorMd/examples/css/style.css"/>
    <link rel="stylesheet" href="EditorMd/css/editormd.css"/>
    <link href="css/index.css" rel="stylesheet">
    <link href="css/about.css" rel="stylesheet"/>
</head>
<body>
<header>
    <div id="logo"><a href="/"></a></div>
    <nav class="topnav" id="topnav">
        <a href="selectAllBlog"><span>Home</span><span class="en">主页</span></a>
        <a href="adminblog"><span>Admin Blog</span><span class="en">管理博客</span></a>
        <a href="admindiary"><span>Admin Diary</span><span class="en">管理日记</span></a>
        <a href="writeblog.jsp"><span>Write Blog</span><span class="en">写博客</span></a>
        <a href="writediary.jsp"><span>Write Diary</span><span class="en">写日记</span></a>
    </nav>
</header>
<div class="box">
    <form action="writeBlog" method="post">
        <div class="admin">
            <h1 class="t_nav"><span>分享使人感到愉悦。</span></h1><br/>
            <div id="layout">
                <header>
                    <h1>写博客</h1>
                </header>
                <div class="blogtitle">
                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
                    标题 ：<input type="text" name="blogtitle" style="width: 400px;height:25px;"/>
                </div>
                <br/>
                <br/>
                <br/>
                <input type="submit" value="uplode" style="background-color:#0055AA;color: white;font-size: 15px;
            	height:30px;width:80px;display:inline-block;float: left;margin-left: 55px;"/>
                <br/>
                <br/>
                <br/>
                <div id="test-editormd">
                <textarea style="display:none;" name="article"></textarea>
                </div>
            </div>
            <script src="EditorMd/examples/js/jquery.min.js"></script>
            <script src="EditorMd/editormd.min.js"></script>
            <script type="text/javascript">
                var testEditor;

                $(function () {
                    testEditor = editormd("test-editormd", {
                        width: "90%",
                        height: 800,
                        syncScrolling: "single",
                        path: "EditorMd/lib/"
                    });

                    /*
                    // or
                    testEditor = editormd({
                        id      : "test-editormd",
                        width   : "90%",
                        height  : 640,
                        path    : "../lib/"
                    });
                    */
                });
            </script>
        </div>
    </form>
</div>
</body>
</html>

</html>
```

#### 3.2.8 writediary.jsp

writediary.jsp 是用于写日记的页面，和 writeblog.jsp 类似，其具体代码信息如下：

```html
<%@ page language="java" contentType="text/html; charset=utf-8"
         pageEncoding="utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>Wright Diary</title>
    <link rel="stylesheet" href="EditorMd/examples/css/style.css"/>
    <link rel="stylesheet" href="EditorMd/css/editormd.css"/>
    <link href="css/index.css" rel="stylesheet">
    <link href="css/about.css" rel="stylesheet"/>
</head>
<body>
<div class="admin">
    <header>
        <div id="logo"><a href="/"></a></div>
        <nav class="topnav" id="topnav">
            <a href="selectAllBlog"><span>Home</span><span class="en">主页</span></a>
            <a href="adminblog"><span>Admin Blog</span><span class="en">管理博客</span></a>
            <a href="admindiary"><span>Admin Diary</span><span class="en">管理日记</span></a>
            <a href="writeblog.jsp"><span>Write Blog</span><span class="en">写博客</span></a>
            <a href="writediary.jsp"><span>Write Diary</span><span class="en">写日记</span></a>
        </nav>
    </header>
</div>
<div class="box">
    <div class="admin">
        <form action="writediary">
            <h1 class="t_nav"><span>诉说你的喜怒哀乐。</span></h1><br/>
            <div id="layout">
                <header>

                    <h1>写日记</h1>
                </header>
                <input type="submit" value="uplode" style="background-color:#0055AA;color: white;font-size: 15px;
            	height:30px;width:80px;display:inline-block;float: left;margin-left: 55px;"/>
                <br/>
                <br/>
                <br/>
                <div id="test-editormd">
                <textarea style="display:none;" name="diary"></textarea>
                </div>
            </div>
            <script src="EditorMd/examples/js/jquery.min.js"></script>
            <script src="EditorMd/editormd.min.js"></script>
            <script type="text/javascript">
                var testEditor;

                $(function () {
                    testEditor = editormd("test-editormd", {
                        width: "90%",
                        height: 800,
                        syncScrolling: "single",
                        path: "EditorMd/lib/"
                    });

                    /*
                    // or
                    testEditor = editormd({
                        id      : "test-editormd",
                        width   : "90%",
                        height  : 640,
                        path    : "../lib/"
                    });
                    */
                });
            </script>
        </form>
    </div>
</div>
</body>
</html>

```

#### 3.2.9 adminblog.jsp

adminblog.jsp 是用于管理博客的页面，管理员能管理博客文章，即删除选择的博客。页面具体代码如下所示：

```html
<%@ page language="java" contentType="text/html; charset=utf-8"
         pageEncoding="utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>AdminBlog</title>
    <link href="css/index.css" rel="stylesheet">
</head>
<body>
<header>
    <div id="logo"><a href="/"></a></div>
    <nav class="topnav" id="topnav">
        <a href="selectAllBlog"><span>Home</span><span class="en">主页</span></a>
        <a href="adminblog"><span>Admin Blog</span><span class="en">管理博客</span></a>
        <a href="admindiary"><span>Admin Diary</span><span class="en">管理日记</span></a>
        <a href="writeblog.jsp"><span>Write Blog</span><span class="en">写博客</span></a>
        <a href="writediary.jsp"><span>Write Diary</span><span class="en">写日记</span></a>
    </nav>
</header>

<article>
    <h2 class="title">文章列表</h2>
    <br>
    <br><br>

    <div class="bloglist">
        <div class="wz">
            <c:forEach items="${blogs}" var="blogs">
                <h3><c:out value="${blogs.blogtitle}"/></h3>

                <ul>

                    <a title="Delete" href="deleteBlogById?blogid=<c:out value="${blogs.blogid}"/>" class="readmore">Delete>></a>
                </ul>
            </c:forEach>
            <div class="clear"></div>
        </div>
    </div>
    </div>
</article>
</body>
<script type="text/javascript" src="js/alert.js"></script>
</html>

```

#### 3.2.10 admindiary.jsp

admindiary.jsp 是管理日记的页面，功能和布局同 adminblog.jsp 类似，其具体代码信息如下：

```html
<%@ page language="java" contentType="text/html; charset=utf-8"
         pageEncoding="utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html >
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>AdminDiary</title>
    <link href="css/index.css" rel="stylesheet">
</head>
<body>
<header>
    <div id="logo"><a href="/"></a></div>
    <nav class="topnav" id="topnav">
        <a href="selectAllBlog"><span>Home</span><span class="en">主页</span></a>
        <a href="adminblog"><span>Admin Blog</span><span class="en">管理博客</span></a>
        <a href="admindiary"><span>Admin Diary</span><span class="en">管理日记</span></a>
        <a href="writeblog.jsp"><span>Write Blog</span><span class="en">写博客</span></a>
        <a href="writediary.jsp"><span>Write Diary</span><span class="en">写日记</span></a>
    </nav>
</header>
<article>
    <h2 class="title">日记列表</h2>
    <br>
    <br>
    <br>
    <div class="bloglist">
        <div class="wz">
            <c:forEach items="${diarys}" var="diarys">
                <h3><c:out value="${diarys.diary}"/></h3>

                <ul>

                    <a title="Delete" href="deleteDiaryById?diaryid=<c:out value="${diarys.diaryid}"/>"
                       class="readmore">Delete>></a>
                </ul>
            </c:forEach>
            <div class="clear"></div>
        </div>
    </div>
</article>
</body>
<script type="text/javascript" src="js/alert.js"></script>
</html>
```

#### 3.2.11 success.jsp

success.jsp 的功能是弹出上传博客成功的提示框，并回到写博客页面。其具体信息如下：

```html
<%@ page language="java" contentType="text/html; charset=utf-8"
         pageEncoding="utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>Insert title here</title>
</head>
<body>
<script>
    window.onload = function () { //设置当页面加载时执行
        var result = confirm("上传成功");
        if (result) //判断是否点击确定
            window.location = "writeblog.jsp" //确定的话游览器自身跳转
    }
</script>
</body>

</html>

```

#### 3.2.12 Dsuccess.jsp

 Dsuccess.jsp 与  success.jsp 功能类似， Dsuccess.jsp 是提示日记上传成功的页面：

```html
<%@ page language="java" contentType="text/html; charset=utf-8"
         pageEncoding="utf-8" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html>
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <title>Dsuccess</title>
</head>
<body>
<script>
    window.onload = function () { //设置当页面加载时执行
        var result = confirm("上传成功");
        if (result) //判断是否点击确定
            window.location = "writediary.jsp" //确定的话游览器自身跳转
    }
</script>
</body>

</html>

```

至此，我们的页面编写就完成了。

```checker
- name: check blogindex jsp exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/blogindex.jsp
  error: 我们发现您还没有创建blogindex.jsp! /home/project/PersonalBlog/src/main/webapp/blogindex.jsp
  timeout: 1
- name: check index jsp exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/index.jsp
  error: 我们发现您还没有创建index.jsp! /home/project/PersonalBlog/src/main/webapp/index.jsp
  timeout: 1
- name: check blog jsp exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/blog.jsp
  error: 我们发现您还没有创建blog.jsp! /home/project/PersonalBlog/src/main/webapp/blog.jsp
  timeout: 1
- name: check about jsp exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/about.jsp
  error: 我们发现您还没有创建about.jsp! /home/project/PersonalBlog/src/main/webapp/about.jsp
  timeout: 1
- name: check admin jsp exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/admin.jsp
  error: 我们发现您还没有创建admin.jsp! /home/project/PersonalBlog/src/main/webapp/admin.jsp
  timeout: 1
- name: check writeblog jsp exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/writeblog.jsp
  error: 我们发现您还没有创建writeblog.jsp! /home/project/PersonalBlog/src/main/webapp/writeblog.jsp
  timeout: 1
- name: check writediary jsp exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/writediary.jsp
  error: 我们发现您还没有创建writediary.jsp! /home/project/PersonalBlog/src/main/webapp/writediary.jsp
  timeout: 1
- name: check adminblog jsp exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/adminblog.jsp
  error: 我们发现您还没有创建adminblog.jsp! /home/project/PersonalBlog/src/main/webapp/adminblog.jsp
  timeout: 1
- name: check admindiary jsp exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/admindiary.jsp
  error: 我们发现您还没有创建admindiary.jsp! /home/project/PersonalBlog/src/main/webapp/admindiary.jsp
  timeout: 1
- name: check success jsp exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/success.jsp
  error: 我们发现您还没有创建success.jsp! /home/project/PersonalBlog/src/main/webapp/success.jsp
  timeout: 1
- name: check Dsuccess jsp exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/Dsuccess.jsp
  error: 我们发现您还没有创建Dsuccess.jsp! /home/project/PersonalBlog/src/main/webapp/Dsuccess.jsp
  timeout: 1
```

### 3.3 博客相关功能的实现

博客相关功能包括：存储博客的内容信息、查找博客、根据id查找博客、删除博客。

#### 3.3.1 BlogController层

BlogController 是 blog 相关功能的 Controller 类，位于 com.personalblog.controller 包下，其功能和实现请看下面的代码和详细注释：

```java

package com.personalblog.controller;

import com.personalblog.model.Blog;
import com.personalblog.service.BlogService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpServletRequest;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.List;

/**
 * 标注controller层
 * @author shiyanlou
 */
@Controller
public class BlogController {

    /**
     * 自动注入
     */
    @Autowired
    private BlogService blogservice;

    /**
     * 存储博客信息
     *
     * @param blog the blog
     * @return the string
     */
    @RequestMapping("writeBlog")
    public String writeBlog(Blog blog) {
        //获取当前日期
        Date currentTime = new Date();
        //将日期转化为指定格式
        SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd");
        String dateString = formatter.format(currentTime);
        blog.setTime(dateString);
        this.blogservice.writeBlog(blog);
        return "success";
    }

    /**
     * 查找全博客，用于主页显示
     *
     * @param request the request
     * @return the string
     */
    @RequestMapping("selectAllBlog")
    public String selectAllBlog(HttpServletRequest request) {
        try {
            List<Blog> blogs;
            blogs = this.blogservice.selectAllBlog();
            //将查询结果的list放入request返回给页面，页面用JSTL表达式获取显示
            request.setAttribute("blogs", blogs);
            return "index";
        } catch (Exception e) {
            System.out.println(e);
            return "index";
        }
    }

    /**
     * 与selectAllBlog的操作一样，但是返回跳转的页面不同
     *
     * @param request the request
     * @return the string
     */
    @RequestMapping("selectAllBlog2")
    public String selectAllBlog2(HttpServletRequest request) {

        try {
            List<Blog> blogs;
            blogs = this.blogservice.selectAllBlog();
            System.out.println("title:" + blogs.get(0).getBlogtitle());
            request.setAttribute("blogs", blogs);
            return "admin";
        } catch (Exception e) {
            System.out.println(e);
            return "admin";
        }
    }

    /**
     * 通过id查找博客，用于显示博客的正文
     *
     * @param request the request
     * @return the string
     */
    @RequestMapping("selectBlogById")
    public String selectBlogById(HttpServletRequest request) {
        try {
            //获取id并转化类型
            String id = request.getParameter("blogid");
            int blogid = Integer.parseInt(id);
            List<Blog> blogs = this.blogservice.selectBlogById(blogid);
            //将查询结果返回
            request.setAttribute("blog", blogs.get(0));
            return "blog";
        } catch (Exception e) {
            System.out.println(e);
            return null;
        }
    }

    /**
     同样是查询所有博客信息，用于管理博客
     *
     * @param request the request
     * @return the string
     */
    @RequestMapping("adminblog")
    public String adminblog(HttpServletRequest request) {
        try {
            List<Blog> blogs;
            blogs = this.blogservice.selectAllBlog();
            request.setAttribute("blogs", blogs);
            return "adminblog";
        } catch (Exception e) {
            System.out.println(e);
            return null;
        }
    }

    /**
     * 删除博客信息
     *
     * @param request the request
     * @return the string
     */
    @RequestMapping("deleteBlogById")
    public String deleteBlogById(HttpServletRequest request) {
        try {
            String id = request.getParameter("blogid");
            int blogid = Integer.parseInt(id);
            //调用删除
            this.blogservice.deleteBlogById(blogid);
            return "redirect:adminblog";
        } catch (Exception e) {
            return null;
        }
    }
}


```

#### 3.3.2 博客 Service 层

博客功能相关的 Service 层（位于 com.personalblog.service 包下）包括接口 BlogService 和 是实现类 BlogServiceImpl
其中 BlogService 接口的具体信息如下：

```java
package com.personalblog.service;

import java.util.List;

import com.personalblog.model.Blog;

public interface BlogService {

	void writeBlog(Blog blog);

	List<Blog> selectAllBlog();

	List<Blog> selectBlogById(int blogid);

	void deleteBlogById(int blogid);

}
```

实现类 BlogServiceImpl 的代码如下：

```java
package com.personalblog.service;

import com.personalblog.mapper.BlogMapper;
import com.personalblog.model.Blog;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class BlogServiceImpl implements BlogService {

    @Autowired
    private BlogMapper blogMapper;

    public void writeBlog(Blog blog) {
        System.out.println(blog.getArticle());
        blogMapper.writeBlog(blog);
    }

    public List<Blog> selectAllBlog() {
        List<Blog> blogs = this.blogMapper.selectAllBlog();
        return blogs;
    }

    public List<Blog> selectBlogById(int blogid) {
        List<Blog> blogs = this.blogMapper.selectBlogById(blogid);
        return blogs;
    }

    public void deleteBlogById(int blogid) {
        blogMapper.deleteBlogById(blogid);

    }

}



```

#### 3.3.3 博客功能的 Mapper 层

博客功能相关的 Mapper 层中包含了 BlogMapper 接口和 BlogMapper.xml 配置文件，Mapper 层位于 com.personalblog.mapepr
 包下。
 接口 BlogMapper 的代码如下：

```java
package com.personalblog.mapper;

import com.personalblog.model.Blog;

import java.util.List;

public interface BlogMapper {

    void writeBlog(Blog blog);

    List<Blog> selectAllBlog();

    List<Blog> selectBlogById(int blogid);

    void deleteBlogById(int blogid);

}

```

BlogMapper.xml 的具体代码如下：

```html
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.personalblog.mapper.BlogMapper">
    <!-- 自定义结果集 -->
    <resultMap type="Blog" id="blogResultMap">
        <id property="blogid" column="blogid"/>
        <result property="blogtitle" column="blogtitle"/>
        <result property="article" column="article"/>
        <result property="time" column="time"/>
    </resultMap>
    <insert id="writeBlog" parameterType="Blog" useGeneratedKeys="true" keyProperty="blogid">
        insert into blog (article, blogtitle, time)
        values (#{article}, #{blogtitle}, #{time})
    </insert>
    <select id="selectAllBlog" parameterType="Blog" resultMap="blogResultMap">
        select *
        from blog
    </select>
    <select id="selectBlogById" parameterType="Blog" resultMap="blogResultMap">
        select *
        from blog
        where blogid = #{blogid}
    </select>
    <delete id="deleteBlogById">
        delete
        from blog
        where blogid = #{blogid}
    </delete>
</mapper>
```

以上便是博客相关功能的后台 Java 代码。

### 3.4 日记相关功能的实现

日记 diary 的相关功能包括写日记、显示日记和管理日记。

#### 3.4.1 日记功能的 Controller

DiaryController 是日记相关功能的 Controller 层，其具体代码和注释如下：

```java
package com.personalblog.controller;

import com.personalblog.model.Diary;
import com.personalblog.service.DiaryService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import javax.servlet.http.HttpServletRequest;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.List;

/**
 * 日记控制器
 * @author shiyanlou
 */
@Controller
public class DiaryController {
    @Autowired
    private DiaryService diaryService;

    /**
     * 写日记
     *
     * @param diary the diary
     * @return the string
     */
    @RequestMapping("writediary")
    public String writediary(Diary diary) {
        //获取当前日期
        Date currentTime = new Date();
        //将日期转化为指定格式
        SimpleDateFormat formatter = new SimpleDateFormat("yyyy-MM-dd");
        String dateString = formatter.format(currentTime);
        diary.setTime(dateString);
        this.diaryService.writediary(diary);
        return "Dsuccess";
    }

    /**
     * 查找全部的日记
     *
     * @param request the request
     * @return the string
     */
    @RequestMapping("selectAllDiary")
    public String selectAllDiary(HttpServletRequest request) {
        try {
            List<Diary> diarys;
            diarys = this.diaryService.selectAllDiary();
            request.setAttribute("diarys", diarys);
            return "saylist";
        } catch (Exception e) {
            System.out.println(e);
            e.printStackTrace();
            return null;
        }

    }

    /**
     * 查找全部日记，但是跳转到日记管理页面
     *
     * @param request the request
     * @return the string
     */
    @RequestMapping("admindiary")
    public String selectAllDiary2(HttpServletRequest request) {
        try {
            List<Diary> diarys;
            diarys = this.diaryService.selectAllDiary();
            request.setAttribute("diarys", diarys);
            return "admindiary";
        } catch (Exception e) {
            System.out.println(e);
            return null;
        }

    }

    /**
     * 删除日记
     *
     * @param request the request
     * @return the string
     */
    @RequestMapping("deleteDiaryById")
    public String deleteDiaryById(HttpServletRequest request) {
        try {
            String id = request.getParameter("diaryid");
            int diaryid = Integer.parseInt(id);
            this.diaryService.deleteDiaryById(diaryid);
            return "redirect:admindiary";
        } catch (Exception e) {
            return null;
        }
    }
}


```

#### 3.4.2 日记相关的 Service 层

包括 service 接口和 serviceImpl 实现类。其中 DiaryService 接口如下:

```java
package com.personalblog.service;

import com.personalblog.model.Diary;

import java.util.List;

public interface DiaryService {

    void writediary(Diary diary);

    List<Diary> selectAllDiary();

    void deleteDiaryById(int diaryid);

}


```

DiaryServiceImpl 实现类如下:

```java
package com.personalblog.service;

import com.personalblog.mapper.DiaryMapper;
import com.personalblog.model.Diary;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class DiaryServiceImpl implements DiaryService {

    @Autowired
    private DiaryMapper diarymapper;

    public void writediary(Diary diary) {
        diarymapper.writediary(diary);

    }

    public List<Diary> selectAllDiary() {
        List<Diary> diarys = this.diarymapper.selectAllDiary();
        return diarys;
    }

    public void deleteDiaryById(int diaryid) {
        diarymapper.deleteDiaryById(diaryid);

    }

}


```

#### 3.4.3 Mapper 层

Mapper 层包括 DiayrMapper 接口和 DiaryMapper.xml 文件。其中 DiaryMapper 接口 具体的代码如下：

```java
package com.personalblog.mapper;

import com.personalblog.model.Diary;

import java.util.List;

public interface DiaryMapper {

    void writediary(Diary diary);

    List<Diary> selectAllDiary();

    void deleteDiaryById(int diaryid);

}

```

DiaryMapper.xml ：

```html
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.personalblog.mapper.DiaryMapper">
    <!-- 自定义结果集 -->
    <resultMap type="Diary" id="diaryResultMap">
        <id property="diaryid" column="diaryid"/>
        <result property="diary" column="diary"/>
        <result property="time" column="time"/>
    </resultMap>
    <insert id="writediary" parameterType="Diary" useGeneratedKeys="true" keyProperty="diartid">
        insert into diary (diary, time)
        values (#{diary}, #{time})
    </insert>
    <select id="selectAllDiary" parameterType="Diary" resultMap="diaryResultMap">
        select *
        from diary
    </select>
    <delete id="deleteDiaryById">
        delete
        from diary
        where diaryid = #{diaryid}
    </delete>
</mapper>



```

以上就是我们的逻辑处理过程的代码。接下来我们再来配置我们的 xml 文件。

### 3.5 xml 文件的配置

#### 3.5.1 web.xml

web.xml 文件中我们需要配置的东西是 SSM 框架的相关配置，包括监听器、核心加载类等等，其具体信息如下：

```html
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <display-name>PersonalBlog</display-name>
    <!-- 配置 Spring 核心监听器 -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!-- 指定 Spring 的配置文件 -->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
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
    <!-- 解析静态资源 -->
    <servlet-mapping>
        <servlet-name>default</servlet-name>
        <url-pattern>*.html</url-pattern>
        <url-pattern>*.css</url-pattern>
        <url-pattern>*.js</url-pattern>
        <url-pattern>*.jpg</url-pattern>
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
    <welcome-file-list>
        <welcome-file>blogindex.jsp</welcome-file>
    </welcome-file-list>
</web-app>
```

#### 3.5.2 applicationContext.xml

applicationContext.xml 是 spring 的核心配置文件，它整合了 Spring mvc 和 mybatis ，并配置了数据库的持久化。其具体信息如下：

```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="
          http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
          http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context-4.2.xsd
          http://www.springframework.org/schema/tx
          http://www.springframework.org/schema/tx/spring-tx-4.1.xsd">

    <!-- 自动扫描有 Spring 相关注解的类，并把这些类注册为 bean -->
    <context:component-scan base-package="com.personalblog"/>
    <!-- 配置数据源 -->
    <bean id="dataSource"
          class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url"
                  value="jdbc:mysql://localhost:3306/personalblog?useUnicode=true&amp;characterEncoding=UTF-8"/>
        <property name="username" value="root"/>
        <property name="password" value=""/>
    </bean>
    <!-- MyBatis 的 SqlSession 的工厂，并引用数据源，扫描 MyBatis 的配置文件 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>

    <!-- MyBatis 自动扫描加载 Sql 映射文件/接口 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.personalblog.mapper"/>
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
    </bean>
    <!-- JDBC 事务管理器 -->
    <bean id="txManager"
          class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>
    <!-- 启用支持 annotation 注解方式事务管理 -->
    <tx:annotation-driven transaction-manager="txManager"/>

</beans>
```

#### 3.5.3 mybatis-config.xml

mybatis-config.xml 是加载 mapper 层的接口和 xml 文件，具体的代码信息如下：

```html
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 为JavaBean起类别名 -->
    <typeAliases>
        <package name="com.personalblog.model"/>
    </typeAliases>
    <!-- 通过 mapper 接口包加载整个包的映射文件 -->
    <mappers>
        <package name="com.personalblog.mapper"/>
    </mappers>
</configuration>
```

#### 3.5.4 spring-mvc.xml

spring-mvc.xml 配置的是配置方案和视图解析器、自动扫描并加载成 bean 等，其具体代码如下:

```html
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context-4.2.xsd
      http://www.springframework.org/schema/mvc
     http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd">

    <!-- 自动扫描该包，Spring MVC 会将包下用 @Controller 注解的类注册为 Spring 的 controller -->
    <context:component-scan base-package="com.personalblog.controller"/>
    <!-- 设置默认配置方案 -->
    <mvc:annotation-driven/>
    <!-- 视图解析器 -->
    <bean id="viewResolver"
          class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 执行完action后会返回xxx，xxx会和下面的property组合，形成跳转页面的路径 -->
        <property name="prefix" value=""/>
        <property name="suffix" value=".jsp"/>
    </bean>
    <mvc:default-servlet-handler/>
</beans>
```

#### 3.5.4 log4j.properties

log4j.properties用于设置输出日志  

```
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```


```checker
- name: check BlogController java exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/java/com/personalblog/controller/BlogController.java
  error: 我们发现您还没有创建BlogController.java! /home/project/PersonalBlog/src/main/java/com/personalblog/controller/BlogController.java
  timeout: 1
- name: check BlogService java exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/java/com/personalblog/service/BlogService.java
  error: 我们发现您还没有创建BlogService.java! /home/project/PersonalBlog/src/main/java/com/personalblog/service/BlogService.java
  timeout: 1
- name: check BlogServiceImpl java exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/java/com/personalblog/service/BlogServiceImpl.java
  error: 我们发现您还没有创建BlogServiceImpl.java! /home/project/PersonalBlog/src/main/java/com/personalblog/service/BlogServiceImpl.java
  timeout: 1
- name: check BlogMapper java exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/java/com/personalblog/mapper/BlogMapper.java
  error: 我们发现您还没有创建BlogMapper.java! /home/project/PersonalBlog/src/main/java/com/personalblog/mapper/BlogMapper.java
  timeout: 1
- name: check BlogMapper xml exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/java/com/personalblog/mapper/BlogMapper.xml
  error: 我们发现您还没有创建BlogMapper.xml! /home/project/PersonalBlog/src/main/java/com/personalblog/mapper/BlogMapper.xml
  timeout: 1
- name: check DiaryController java exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/java/com/personalblog/controller/DiaryController.java
  error: 我们发现您还没有创建DiaryController.java! /home/project/PersonalBlog/src/main/java/com/personalblog/controller/DiaryController.java
  timeout: 1
- name: check DiaryService java exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/java/com/personalblog/service/DiaryService.java
  error: 我们发现您还没有创建DiaryService.java! /home/project/PersonalBlog/src/main/java/com/personalblog/service/DiaryService.java
  timeout: 1
- name: check DiaryServiceImpl java exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/java/com/personalblog/service/DiaryServiceImpl.java
  error: 我们发现您还没有创建DiaryServiceImpl.java! /home/project/PersonalBlog/src/main/java/com/personalblog/service/DiaryServiceImpl.java
  timeout: 1
- name: check DiaryMapper java exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/java/com/personalblog/mapper/DiaryMapper.java
  error: 我们发现您还没有创建DiaryMapper.java! /home/project/PersonalBlog/src/main/java/com/personalblog/mapper/DiaryMapper.java
  timeout: 1
- name: check DiaryMapper xml exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/java/com/personalblog/mapper/DiaryMapper.xml
  error: 我们发现您还没有创建DiaryMapper.xml! /home/project/PersonalBlog/src/main/java/com/personalblog/mapper/DiaryMapper.xml
  timeout: 1
- name: check web xml exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/main/webapp/WEB-INF/web.xml
  error: 我们发现您还没有创建web.xml! /home/project/PersonalBlog/src/main/webapp/WEB-INF/web.xml
  timeout: 1
- name: check applicationContext xml exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/applicationContext.xml
  error: 我们发现您还没有创建applicationContext.xml! /home/project/PersonalBlog/src/applicationContext.xml
  timeout: 1
- name: check mybatis config xml exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/mybatis-config.xml
  error: 我们发现您还没有创建mybatis-config.xml! /home/project/PersonalBlog/src/mybatis-config.xml
  timeout: 1
- name: check spring mvc xml exist
  script: |
    #!/bin/bash
    ls /home/project/PersonalBlog/src/spring-mvc.xml
  error: 我们发现您还没有创建spring-mvc.xml! /home/project/PersonalBlog/src/spring-mvc.xml
  timeout: 1
```

## 四、测试结果

在测试项目之前，我们需要做一些准备：
1. 将 Msyql 服务启动，启动命令见开始部分。（如果已经启动则省略）
2. 打开控制台终端，输入
   ```shell
   mvn jetty:run
   ```
3. 点击`工具--web服务`按钮，就可以访问服务了。

`注意`:目前实验楼只支持8080端口启动服务，如果使用其他端口，将无法访问到服务，这里jetty默认就是8080端口，所以不用更改。

### 4.1 页面展示

#### 4.1.1 主页

![主页](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540370843406.png)

#### 4.1.2 关于博主

![关于](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540371002322.png)

#### 4.1.3 日记管理

![日记管理](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540371125464.png)

#### 4.1.4 博客管理

![博客管理](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540371163570.png)

#### 4.1.5 验证管理员

点击 Admin ，会弹出提示框，要求输入密码，这里密码为 000000.
![验证管理员](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540371278722.png)

`注意`：这里需要数据库有第一篇博文，否则会报错。感兴趣的同学请根据代码优化此处功能。

#### 4.1.6 写博客

![写博客](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540371364470.png)

#### 4.1.7 写日记

![写日记](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540371369607.png)

## 五、实验总结

在本次课程实验中，我们简单的利用了前端的一些基本知识以及第三方插件、Jquery 框架编写了页面，而后台处理则交给了 SSM 框架。作为一个 Java 后台程序员，是不能够不懂前端知识的。能够熟练使用基础前端知识，将会使你在开发网站类型的 Java 程序的时候如虎添翼。
