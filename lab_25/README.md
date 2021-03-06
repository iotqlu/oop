---
sort: 25
---

# 持久化实体

## 一、实验介绍

#### 1.1 实验内容

本节课程主要根据第三节的数据库设计完成持久化实体映射设计，并完成持久化实体类的创建。

#### 1.2 实验知识点

- 持久化实体
- Domain Object 层

#### 1.3 实验环境

- JDK1.8
- web IDE

## 二、实验步骤

打开WEB IDE，找到我们之前建立的项目 `hrms`，开始我们的实验。

在项目 `hrms` 的 `src/main/java` 目录下新建包 `com.shiyanlou.domain`，作为 Domain Object 层的包。

接下来，根据第三节数据库的设计创建实体类。

### 2.1 Admin 类（管理员）

在 `src/main/java` 目录下的 `com.shiyanlou.domain` 包中新建实体类 `Admin`，作为`管理员表` 的映射。

```java
package com.shiyanlou.domain;

import java.io.Serializable;

public class Admin implements Serializable {

    private static final long serialVersionUID = 1L;

    private Integer id;  // 管理员编号
    private String username;  // 用户名
    private String password;  // 密码
    private String role_name;   // 管理员角色

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getRole_name() {
        return role_name;
    }

    public void setRole_name(String role_name) {
        this.role_name = role_name;
    }

    @Override
    public String toString() {
        return "Admin:[id=" + id + ",username=" + username + ",password="
                + password + ",role_name=" + role_name + "]";
    }
}
```

```checker
- name: check Admin exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/domain/Admin.java
  error: 我们发现您还没有创建Admin.java! /home/project/hrms/src/main/java/com/shiyanlou/domain/Admin.java
  timeout: 1
```

### 2.2 Post 类（公告）

在 `src/main/java` 目录下的 `com.shiyanlou.domain` 包中新建实体类 `Post`，作为`公告表` 的映射。

```java
package com.shiyanlou.domain;

import java.io.Serializable;
import java.util.Date;

public class Post implements Serializable {

    private static final long serialVersionUID = 1L;

    private Integer id;  // 公告编号
    private String title;  // 标题
    private String content;  // 内容
    private Admin admin;  // 发布人
    private Date date;  // 发布日期

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }

    public Admin getAdmin() {
        return admin;
    }

    public void setAdmin(Admin admin) {
        this.admin = admin;
    }

    public Date getDate() {
        return date;
    }

    public void setDate(Date date) {
        this.date = date;
    }

    @Override
    public String toString() {
        return "Post:[id=" + id + ",title=" + title + ",content=" + content
                + ",admin=" + admin + ",date=" + date + "]";
    }
}
```

```checker
- name: check Post exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/domain/Post.java
  error: 我们发现您还没有创建Post.java! /home/project/hrms/src/main/java/com/shiyanlou/domain/Post.java
  timeout: 1
```

### 2.3 Department 类（部门）

在 `src/main/java` 目录下的 `com.shiyanlou.domain` 包中新建实体类 `Department`，作为`部门表` 的映射。

```java
package com.shiyanlou.domain;

import java.io.Serializable;

public class Department implements Serializable {

    private static final long serialVersionUID = 1L;

    private Integer id;  // 部门编号
    private String name;  // 名称
    private String description;  // 描述 

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    @Override
    public String toString() {
        return "Department:[id=" + id + ",name=" + name + ",description="
                + description + "]";
    }
}
```

```checker
- name: check Department exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/domain/Department.java
  error: 我们发现您还没有创建Department.java! /home/project/hrms/src/main/java/com/shiyanlou/domain/Department.java
  timeout: 1
```

### 2.4 Position 类（职位）

在 `src/main/java` 目录下的 `com.shiyanlou.domain` 包中新建实体类 `Position`，作为`职位表` 的映射。

```java
package com.shiyanlou.domain;

import java.io.Serializable;

public class Position implements Serializable {

    private static final long serialVersionUID = 1L;

    private Integer id;  // 职位编号
    private String name;  // 名称
    private String description;  // 描述

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDescription() {
        return description;
    }

    public void setDescription(String description) {
        this.description = description;
    }

    @Override
    public String toString() {
        return "Position:[id=" + id + ",name=" + name + ",description="
                + description + "]";
    }
}
```

```checker
- name: check Position exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/domain/Position.java
  error: 我们发现您还没有创建Position.java! /home/project/hrms/src/main/java/com/shiyanlou/domain/Position.java
  timeout: 1
```

### 2.5 Employee 类（员工）

在 `src/main/java` 目录下的 `com.shiyanlou.domain` 包中新建实体类 `Employee`，作为`员工表` 的映射。

```java
package com.shiyanlou.domain;

import java.io.Serializable;
import java.util.Date;

public class Employee implements Serializable {

    private static final long serialVersionUID = 1L;

    private String id;  // 员工编号
    private String name;  // 姓名
    private String sex;  // 性别
    private String phone;  // 电话
    private String email;  // 邮箱
    private String address;  // 地址
    private String education;  // 学历
    private Date birthday;  // 生日
    private Department department;  // 部门
    private Position position;  // 职位

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public String getPhone() {
        return phone;
    }

    public void setPhone(String phone) {
        this.phone = phone;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public String getEducation() {
        return education;
    }

    public void setEducation(String education) {
        this.education = education;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public Department getDepartment() {
        return department;
    }

    public void setDepartment(Department department) {
        this.department = department;
    }

    public Position getPosition() {
        return position;
    }

    public void setPosition(Position position) {
        this.position = position;
    }

    @Override
    public String toString() {
        return "Employee:[id=" + id + ",name=" + name + ",sex=" + sex
                + ",phone=" + phone + ",email=" + email + ",address=" + address
                + ",education=" + education + ",birthday=" + birthday
                + ",department=" + department + ",position=" + position + "]";
    }

}
```

```checker
- name: check Employee exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/domain/Employee.java
  error: 我们发现您还没有创建Employee.java! /home/project/hrms/src/main/java/com/shiyanlou/domain/Employee.java
  timeout: 1
```

## 三、实验总结

到这里我们就完成了 Domain Object 层实体类的设计实现，下一节我们将进入 DAO 层的实现。