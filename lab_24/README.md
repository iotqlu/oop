---
sort: 24
---

# 数据库设计实现

## 一、实验介绍

#### 1.1 实验内容

本节课程对人事管理系统的数据库进行设计和建表实现。

#### 1.2 实验知识点

- E-R 图
- MySQL 建表
- MySQL 插入数据

#### 1.3 实验环境

- MySQL 5.7
- 终端

## 二、实验步骤

这一章节我们将设计数据库结构。

### 2.1 系统 E-R 图

根据系统的功能模块， E-R 图如下：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2916timestamp1494380447164.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2916timestamp1494380448196.png)

分析 E-R 图可知：

- 管理员和公告两个实体之间存在`一对多`的关系，即一个管理员可以发布多个公告；
- 部门和员工两个实体之间存在`一对多`的关系，即一个部门可以有多个员工，一个员工只属于一个部门；
- 员工和职位两个实体之间存在`多对一`的关系，即一个员工只担任一个职位。

转换为表：

1. 管理员表（管理员 id，用户名，密码，用户角色），`管理员 id` 为`主键`
2. 公告表（公告 id，标题，内容，管理员 id，发布日期），`公告 id`为主键，`管理员 id` 为`外键`
3. 部门表（部门 id，名称，描述），`部门 id` 为`主键`
4. 职位表（职位 id，名称，描述），`职位 id` 为`主键`
5. 员工表（员工 id，姓名，性别，手机号，邮箱，住址，学历，生日，部门 id，职位 id），`员工 id` 为主键，`部门 id` 为`外键`，`职位 id` 为`外键`

### 2.2 数据库准备

本次课程使用 MySQL 数据库。首先启动 mysql,在File -> Open New Terminal打开终端，输入 ：

```
$ sudo service mysql start
```

然后在终端下输入以下命令，进入到 MySQL 数据库（-u 表示用户名，比如这里的 root，-p 表示密码，这里没有密码就省略了）：

```
$ mysql -u root
```

创建 `hrms_db` 作为人事管理系统的数据库。

```
create database hrms_db;
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2916timestamp1494317795302.png)

### 2.3 建表

#### 2.3.1 管理员表 admin_tb

建表语句：

```
create table admin_tb(
	admin_id int(11) not null auto_increment,
	username varchar(20) not null,
	password varchar(20) not null,
	role_name varchar(20) not null default 'normaladmin',
	primary key(admin_id)
);
```
插入数据：

```
insert into admin_tb(username,password,role_name) values('superadmin','123456','superadmin'),('admin1','123456','normaladmin');
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2916timestamp1494321754826.png)

#### 2.3.2 公告表 post_tb

建表语句：

```
create table post_tb(
	post_id int(11) not null auto_increment,
	title varchar(50) not null,
	content text not null,
	admin_id int(11) not null,
	create_date date not null,
	primary key(post_id),
	foreign key(admin_id) references admin_tb(admin_id)
);
```

插入数据：

```
insert into post_tb(title,content,admin_id,create_date) values('Leave notice','Please pay attention to holiday safety!',1,'2017-4-30');
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2916timestamp1494323072815.png)

#### 2.3.3 部门表 dept_tb

建表语句：

```
create table dept_tb(
	dept_id int(11) not null auto_increment,
	dept_name varchar(50) not null,
	dept_description varchar(200) not null,
	primary key(dept_id)
);
```

插入数据：

```
insert into dept_tb(dept_name,dept_description) values('course','make courses'),('development','Software development');
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2916timestamp1494379700645.png)

#### 2.3.4 职位表 position_tb

建表语句：

```
create table position_tb(
	pos_id int(11) not null auto_increment,
	pos_name varchar(50) not null,
	pos_description varchar(200) not null,
	primary key(pos_id)
);
```

插入数据：

```
insert into position_tb(pos_name,pos_description) values('Java course staff','make Java courses'),('front-end engineers','responsible for front-end development');
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2916timestamp1494380059203.png)

#### 2.3.5 员工表 employee_tb

建表语句：

```
create table employee_tb(
	emp_id varchar(20) not null,
	emp_name varchar(50) not null,
	sex varchar(10) not null,
	phone varchar(20) not null,
	email varchar(50) not null,
	address varchar(100) not null,
	education varchar(50) not null,
	birthday date not null,
	dept_id int(11) not null,
	pos_id int(11) not null,
	primary key(emp_id),
	foreign key(dept_id) references dept_tb(dept_id),
	foreign key(pos_id) references position_tb(pos_id)
);
```

插入数据：

```
insert into employee_tb values('10001','Tom','male','18211234567','524123456@qq.com','chengdu','Bachelor','1990-4-30',1,1),('10002','Jack','male','18211234568','524123457@qq.com','chongqing','Master','1986-9-2',1,1),('10011','Rose','female','18211234580','524198757@qq.com','chengdu','Bachelor','1991-11-21',2,2),('10012','Anny','female','18211237580','594168757@qq.com','chengdu','Master','1987-5-11',2,2);
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2916timestamp1494381518939.png)

## 三、实验总结

到这里我们就完成了整个数据库的设计与实现，下一节我们将进行持久化实体的设计。