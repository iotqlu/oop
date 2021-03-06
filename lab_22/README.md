---
sort: 22
---

# 系统分析

## 一、实验介绍

本次课程将采用 SSM（Spring + Spring MVC + MyBatis）框架 + easyUI 来开发一个比较简易的人事管理系统，让同学们能够通过实际项目掌握 SSM 项目的开发。在开始实验前，请同学们务必已经掌握 Spring, Spring MVC, MyBatis, easyUI, javaScript 以及 html 等方面的基础知识。

#### 1.1 实验内容

本节课程主要对要开发的人事管理系统进行系统分析，主要包括系统结构、功能分析和开发技术。

#### 1.2 实验知识点

- 需求分析
- java EE 分层结构

#### 1.3 课程来源

本课程主要来自两个地方：

- 项目背景的设计参考自`疯狂软件`编著的`《Spring+MyBatis 企业应用实战》`
- 前端代码大多来自 `ZHENFENG13` 在 GitHub 上的开源项目 ssm-demo，其地址为 `https://github.com/ZHENFENG13/ssm-demo`，在该项目页面设计的基础上根据人事管理系统项目的功能需求等方面稍作修改并加以完善

在这里对上述的作者，特别是 `ZHENFENG13` 表示由衷的感谢。

#### 1.4 代码获取

你可以在Xfce终端下通过下面命令将实验的完整工程项目下载到实验楼环境中，作为参照对比进行学习。

```
wget http://labfile.oss.aliyuncs.com/courses/824/hrms.zip
```

> 注: Maven 的配置，请参考第二节`环境搭建`。
 
## 二、实验步骤

我们首先来分析一下系统需要哪些功能模块。

### 2.1 系统功能结构

本人事管理系统主要用于实验楼内部，主要包括如下几个模块：

- 系统管理
- 部门管理
- 职位管理
- 员工管理
- 公告管理

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2905timestamp1493961297917.png)

整个系统包含上述的几个模块，每个模块又包括多个功能：

1. 系统管理：添加管理员、删除管理员、修改管理员和查询管理员（全部查询和按用户名模糊查询）
2. 部门管理：添加部门、删除部门、修改部门和查询部门（全部查询和按部门名称模糊查询）
3. 职位管理：添加职位、删除职位、修改职位和查询职位（全部查询和按职位名称模糊查询）
4. 员工管理：添加员工、删除员工、修改员工和查询员工（全部查询和按员工编号、员工姓名、部门、职位和性别模糊查询）
5. 公告管理：添加公告、删除公告、修改公告和查询公告（全部查询和按公告标题模糊查询）

### 2.2 开发技术

- Java EE 架构：SSM（Spring + Spring MVC + MyBatis）框架
- 表现层技术：JSP
- 前端框架：easyUI
- 项目管理工具：Maven
- 数据库：MySQL

### 2.3 系统结构

本系统采用的是 Java EE 分层结构：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2905timestamp1493963877870.png)

- 表现层：JSP 页面
- MVC 控制器层：Spring MVC 技术，由一系列控制器组成。
- 业务逻辑层：Spring 技术，由一系列的业务逻辑对象组成。
- DAO 层：MyBatis 框架，由一系列的 DAO 组件组成，这些 DAO 实现了对数据库的创建、查询、更新和删除（CRUD）等原子操作。
- Domain Object 层：由一系列的 POJO（Plain Old Java Object，即普通的、传统的Java对象）组成，是一些简单的 Java Bean 类。
- 数据库：MySQL 数据库，存储持久化数据。

### 2.4 预期效果

登录页面：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2905timestamp1495616229559.png)

系统主页：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2905timestamp1495616231078.png)

功能模块页面：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2905timestamp1495616231366.png)

## 三、实验总结

本节课程主要对人事管理系统的系统结构和功能进行了分析，下一节我们将完成项目环境的搭建。

## 四、参考链接

- [Java EE应用的分层模型](http://blog.csdn.net/lianjiangwei/article/details/50812387)
- 《Spring+MyBatis 企业应用实战》

