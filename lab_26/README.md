---
sort: 26
---

# DAO 层实现

## 一、实验介绍

#### 1.1 实验内容

本节课程主要利用 MyBatis 框架实现 DAO 层。

#### 1.2 实验知识点

- MyBatis 框架
- MySQL

#### 1.3 实验环境

- JDK1.8
- WEB IDE

## 二、实验步骤

根据第一节，我们可以知道系统的功能包括了哪些，根据第三节和第四节，我们知道了数据库表的结构和持久化实体，因此，在这里我们完成数据库的访问操作。

首先在项目 `hrms` 的目录 `src/main/java` 下新建包 `com.shiyanlou.dao`，作为 DAO 层的包， 并在 `src/main/resources` 下新建一个 Folder `mappers` 用来放置 MyBatis 的 mapper.xml 文件。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540265073668.png)

### 2.1 MyBatis 配置文件

在目录 `src/main/resources` 下新建 MyBatis 配置文件 `mybatis-config.xml` ，在这里主要配置了为 JavaBean 取别名，代码如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" 
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	<!-- 为JavaBean 起类别名 -->
    <typeAliases>
        <package name="com.shiyanlou.domain" />
    </typeAliases> 
</configuration>
```

> 注：在这里，我们没有配置 MyBatis 的运行环境、数据源等，那是因为我们要将这些交给 Spring 进行配置管理。

```checker
- name: check mybatis config xml exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/resources/mybatis-config.xml
  error: 我们发现您还没有创建mybatis-config.xml! /home/project/hrms/src/main/resources/mybatis-config.xml
  timeout: 1
```

### 2.2 AdminDao 接口

在包 `com.shiyanlou.dao` 下建一个 `AdminDao.java` 接口文件，代码如下：

```java
package com.shiyanlou.dao;

import java.util.List;
import java.util.Map;

import org.springframework.stereotype.Repository;

import com.shiyanlou.domain.Admin;

@Repository
public interface AdminDao {

    /** 登录
     * 
     * @param admin
     * @return
     */
    public Admin login(Admin admin);

    /** 根据条件查询管理员
     * 
     * @param map
     * @return
     */
    public List<Admin> findAdmins(Map<String, Object> map);

    /** 根据条件查询管理员人数
     * 
     * @param map
     * @return
     */
    public Integer getCount(Map<String, Object> map);

    /** 添加管理员
     * 
     * @param admin
     * @return
     */
    public Integer addAdmin(Admin admin);

    /** 修改管理员
     * 
     * @param admin
     * @return
     */
    public Integer updateAdmin(Admin admin);

    /** 删除管理员
     * 
     * @param id
     * @return
     */
    public Integer deleteAdmin(Integer id);
}
```

接着在 `src/main/resources/mappers` 路径下新建与 AdminDao 接口对应的映射文件 `AdminMapper.xml`，代码如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.shiyanlou.dao.AdminDao">
	<!-- 自定义结果集 -->
    <resultMap type="Admin" id="AdminResult">
        <id property="id" column="admin_id" />
        <result property="username" column="username" />
        <result property="password" column="password" />
        <result property="role_name" column="role_name" />
    </resultMap>
	<!-- 登录 -->
    <select id="login" parameterType="Admin" resultMap="AdminResult">
        select * from
        admin_tb where username=#{username} and password=#{password} limit 1
    </select>
	<!-- 根据条件查询管理员 -->
    <select id="findAdmins" parameterType="Map" resultMap="AdminResult">
        select * from admin_tb
        <where>
            <if test="username!=null and username!='' ">
                username like #{username}
            </if>
        </where>
    </select>
	<!-- 根据条件查询管理员的人数 -->
    <select id="getCount" parameterType="Map" resultType="Integer">
        select count(*) from admin_tb
        <where>
            <if test="username!=null and username!='' ">
                username like #{username}
            </if>
        </where>
    </select>
	<!-- 添加管理员 -->
    <insert id="addAdmin" useGeneratedKeys="true" keyProperty="admin_id">
        insert into admin_tb(username,password)
        values(#{username},#{password})
    </insert>
	<!-- 修改管理员 -->
    <update id="updateAdmin" parameterType="Admin">
        update admin_tb set
        username=#{username},password=#{password} where admin_id=#{id}
    </update>
	<!-- 删除管理员 -->
    <delete id="deleteAdmin" parameterType="Integer">
        delete from admin_tb where
        admin_id=#{id}
    </delete>
</mapper>
```

```checker
- name: check AdminDao exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/dao/AdminDao.java
  error: 我们发现您还没有创建AdminDao.java! /home/project/hrms/src/main/java/com/shiyanlou/dao/AdminDao.java
  timeout: 1
- name: check AdminMapper exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/resources/mappers/AdminMapper.xml
  error: 我们发现您还没有创建AdminMapper.xml! /home/project/hrms/src/main/resources/mappers/AdminMapper.xml
  timeout: 1
```

### 2.3 PostDao 接口

在包 `com.shiyanlou.dao` 下建一个 `PostDao.java` 接口文件，代码如下：

```java
package com.shiyanlou.dao;

import java.util.List;
import java.util.Map;

import org.springframework.stereotype.Repository;

import com.shiyanlou.domain.Post;

@Repository
public interface PostDao {
	
	/** 根据条件查询公告
	 * 
	 * @return
	 */
	public List<Post>findPosts(Map<String, Object> map);
	
	/** 根据条件查询公告数量
	 * 
	 * @param map
	 * @return
	 */
	public Integer getCount(Map<String, Object> map);
	
	/** 添加公告
	 * 
	 * @param post
	 * @return
	 */	
	public Integer addPost(Post post);
	
	/** 修改公告
	 * 
	 * @param post
	 * @return
	 */
	public Integer updatePost(Post post);
	
	/** 删除公告
	 * 
	 * @param id
	 * @return
	 */
	public Integer deletePost(Integer id);
	
	/** 根据 ID 查询公告信息
	 * 
	 * @param id
	 * @return
	 */
	public Post getPostById(Integer id);
}

```

接着在 `src/main/resources/mappers` 路径下新建与 PostDao 接口对应的映射文件 `PostMapper.xml`，代码如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.shiyanlou.dao.PostDao">
	<!-- 自定义结果集 -->
	<resultMap type="Post" id="PostResult">
		<id property="id" column="post_id" />
		<result property="title" column="title" />
		<result property="content" column="content" />
		<result property="date" column="create_date" />
		<association property="admin" javaType="Admin">
			<id property="id" column="admin_id" />
			<result property="username" column="username" />
		</association>
	</resultMap>
	<!-- 根据条件查询公告 -->
	<select id="findPosts" parameterType="Map" resultMap="PostResult">
		select a.admin_id,a.username,p.post_id,p.title,p.content,p.create_date
		from admin_tb a,post_tb p where p.admin_id = a.admin_id
		<if test="title!=null and title!='' ">
			and p.title like #{title}
		</if>
	</select>
	<!-- 根据条件查询公告数量 -->
	<select id="getCount" parameterType="Map" resultType="Integer">
		select count(*) from post_tb
		<where>
			<if test="title!=null and title!='' ">
				title like #{title}
			</if>
		</where>
	</select>
	<!-- 添加公告 -->
	<insert id="addPost" useGeneratedKeys="true" keyProperty="post_id">
		insert
		into post_tb(title,content,admin_id,create_date)
		values(#{title},#{content},#{admin.id},#{date})
	</insert>
	<!-- 修改公告 -->
	<update id="updatePost" parameterType="Post">
		update post_tb set
		title=#{title},content=#{content},admin_id=#{admin.id},create_date=#{date}
		where post_id=#{id}
	</update>
	<!-- 删除公告 -->
	<delete id="deletePost" parameterType="Integer">
		delete from post_tb
		where
		post_id=#{id}
	</delete>
	<!-- 根据 ID 查询公告信息 -->
	<select id="getPostById" parameterType="Integer" resultMap="PostResult">
		select a.admin_id,a.username,p.post_id,p.title,p.content,p.create_date
		from admin_tb a,post_tb p where p.admin_id = a.admin_id and
		p.post_id=#{id}
	</select>
</mapper>
```

```checker
- name: check PostDao exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/dao/PostDao.java
  error: 我们发现您还没有创建PostDao.java! /home/project/hrms/src/main/java/com/shiyanlou/dao/PostDao.java
  timeout: 1
- name: check PostMapper exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/resources/mappers/PostMapper.xml
  error: 我们发现您还没有创建PostMapper.xml! /home/project/hrms/src/main/resources/mappers/PostMapper.xml
  timeout: 1
```

### 2.4 DepartmentDao 接口

在包 `com.shiyanlou.dao` 下建一个 `DepartmentDao.java` 接口文件，代码如下：

```xml
package com.shiyanlou.dao;

import java.util.List;
import java.util.Map;

import org.springframework.stereotype.Repository;

import com.shiyanlou.domain.Department;

@Repository
public interface DepartmentDao {
	
	/** 根据条件查询部门
	 * 
	 * @param map
	 * @return
	 */
	public List<Department> findDepartments(Map<String, Object> map);
	
	/** 根据条件查询部门数量
	 * 
	 * @param map
	 * @return
	 */
	public Integer getCount(Map<String, Object> map);
		
	/** 添加部门
	 * 
	 * @param department
	 * @return
	 */
	public Integer addDepartment(Department department);
	
	/** 修改部门
	 * 
	 * @param department
	 * @return
	 */
	public Integer updateDepartment(Department department);
	
	/** 删除部门
	 * 
	 * @param id
	 * @return
	 */
	public Integer deleteDepartment(Integer id);	
	
}

```

接着在 `src/main/resources/mappers` 路径下新建与 DepartmentDao 接口对应的映射文件 `DepartmentMapper.xml`，代码如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.shiyanlou.dao.DepartmentDao">
	<!-- 自定义结果集 -->
	<resultMap type="Department" id="DepartmentResult">
		<id property="id" column="dept_id" />
		<result property="name" column="dept_name" />
		<result property="description" column="dept_description" />
	</resultMap>
	<!-- 根据条件查询部门 -->
	<select id="findDepartments" parameterType="Map" resultMap="DepartmentResult">
		select * from dept_tb
		<where>
			<if test="name!=null and name!='' ">
				dept_name like #{name}
			</if>
		</where>
	</select>
	<!-- 根据条件查询部门数量 -->
	<select id="getCount" parameterType="Map" resultType="Integer">
		select count(*) from dept_tb
		<where>
			<if test="name!=null and name!='' ">
				dept_name like #{name}
			</if>
		</where>
	</select>
	<!-- 添加部门 -->
	<insert id="addDepartment" useGeneratedKeys="true" keyProperty="dept_id">
		insert into dept_tb(dept_name,dept_description)
		values(#{name},#{description})
	</insert>
	<!-- 修改部门 -->
	<update id="updateDepartment" parameterType="Department">
		update dept_tb set
		dept_name=#{name},dept_description=#{description} where dept_id=#{id}
	</update>
	<!-- 删除部门 -->
	<delete id="deleteDepartment" parameterType="Integer">
		delete from dept_tb where
		dept_id=#{id}
	</delete>
</mapper>
```

```checker
- name: check DepartmentDao exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/dao/DepartmentDao.java
  error: 我们发现您还没有创建DepartmentDao.java! /home/project/hrms/src/main/java/com/shiyanlou/dao/DepartmentDao.java
  timeout: 1
- name: check DepartmentMapper exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/resources/mappers/DepartmentMapper.xml
  error: 我们发现您还没有创建DepartmentMapper.xml! /home/project/hrms/src/main/resources/mappers/DepartmentMapper.xml
  timeout: 1
```

### 2.5 PositionDao 接口

在包 `com.shiyanlou.dao` 下建一个 `PositionDao.java` 接口文件，代码如下：

```java
package com.shiyanlou.dao;

import java.util.List;
import java.util.Map;

import org.springframework.stereotype.Repository;

import com.shiyanlou.domain.Position;

@Repository
public interface PositionDao {
	
	/** 根据条件查询职位
	 * 
	 * @param map
	 * @return
	 */
	public List<Position> findPositions(Map<String, Object> map);
	
	/** 根据条件查询职位数量
	 * 
	 * @param map
	 * @return
	 */
	public Integer getCount(Map<String, Object> map);
		
	/** 添加职位
	 * 
	 * @param position
	 * @return
	 */
	public Integer addPosition(Position position);
	
	/** 修改职位
	 * 
	 * @param position
	 * @return
	 */
	public Integer updatePosition(Position position);
	
	/** 删除职位
	 * 
	 * @param id
	 * @return
	 */
	public Integer deletePosition(Integer id);
}

```

接着在 `src/main/resources/mappers` 路径下新建与 PositionDao 接口对应的映射文件 `PositionMapper.xml`，代码如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.shiyanlou.dao.PositionDao">
	<!-- 自定义结果集 -->
    <resultMap type="Position" id="PositionResult">
        <id property="id" column="pos_id" />
        <result property="name" column="pos_name" />
        <result property="description" column="pos_description" />
    </resultMap>
	<!-- 根据条件查询职位 -->
    <select id="findPositions" parameterType="Map" resultMap="PositionResult">
        select * from position_tb
        <where>
            <if test="name!=null and name!='' ">
                pos_name like #{name}
            </if>
        </where>
    </select>
	<!-- 根据条件查询职位数量 -->
    <select id="getCount" parameterType="Map" resultType="Integer">
        select count(*) from position_tb
        <where>
            <if test="name!=null and name!='' ">
                pos_name like #{name}
            </if>
        </where>
    </select>
	<!-- 添加职位 -->
    <insert id="addPosition" useGeneratedKeys="true" keyProperty="pos_id">
        insert into position_tb(pos_name,pos_description)
        values(#{name},#{description})
    </insert>
	<!-- 修改职位 -->
    <update id="updatePosition" parameterType="Position">
        update position_tb set
        pos_name=#{name},pos_description=#{description} where pos_id=#{id}
    </update>
	<!-- 删除职位 -->
    <delete id="deletePosition" parameterType="Integer">
        delete from position_tb where
        pos_id=#{id}
    </delete>
</mapper>
```

```checker
- name: check PositionDao exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/dao/PositionDao.java
  error: 我们发现您还没有创建PositionDao.java! /home/project/hrms/src/main/java/com/shiyanlou/dao/PositionDao.java
  timeout: 1
- name: check PositionMapper exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/resources/mappers/PositionMapper.xml
  error: 我们发现您还没有创建PositionMapper.xml! /home/project/hrms/src/main/resources/mappers/PositionMapper.xml
  timeout: 1
```

### 2.6 EmployeeDao 接口

在包 `com.shiyanlou.dao` 下建一个 `EmployeeDao.java` 接口文件，代码如下：

```java
package com.shiyanlou.dao;

import java.util.List;
import java.util.Map;

import org.springframework.stereotype.Repository;

import com.shiyanlou.domain.Employee;
import com.shiyanlou.domain.Post;

@Repository
public interface EmployeeDao {
	
	/** 根据条件查询员工
	 * 
	 * @param map
	 * @return
	 */
	public List<Post>findEmployees(Map<String, Object> map);
	
	/** 根据条件查询员工数量
	 * 
	 * @param map
	 * @return
	 */
	public Integer getCount(Map<String, Object> map);
	
	/** 添加员工
	 * 
	 * @param employee
	 * @return
	 */
	public Integer addEmployee(Employee employee);
	
	/** 修改员工
	 * 
	 * @param employee
	 * @return
	 */
	public Integer updateEmployee(Employee employee);
	
	/** 删除员工
	 * 
	 * @param id
	 * @return
	 */
	public Integer deleteEmployee(String id);
}

```

接着在 `src/main/resources/mappers` 路径下新建与 EmployeeDao 接口对应的映射文件 `EmployeeMapper.xml`，代码如下：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.shiyanlou.dao.EmployeeDao">
	<!-- 自定义结果集 -->
	<resultMap type="Employee" id="EmployeeResult">
		<id property="id" column="emp_id" />
		<result property="name" column="emp_name" />
		<result property="sex" column="sex" />
		<result property="phone" column="phone" />
		<result property="email" column="email" />
		<result property="address" column="address" />
		<result property="education" column="education" />
		<result property="birthday" column="birthday" />
		<!-- 关联映射：association -->
		<association property="department" javaType="Department">
			<id property="id" column="dept_id" />
			<result property="name" column="dept_name" />
		</association>
		<association property="position" javaType="Position">
			<id property="id" column="pos_id" />
			<result property="name" column="pos_name" />
		</association>
	</resultMap>
	<!-- 根据条件查询员工 -->
	<select id="findEmployees" parameterType="Map" resultMap="EmployeeResult">
		select e.emp_id,e.emp_name,e.sex,e.phone,e.email,e.address,e.education,e.birthday,d.dept_id,d.dept_name,p.pos_id,p.pos_name
		from employee_tb e,dept_tb d,position_tb p where e.dept_id = d.dept_id
		and e.pos_id = p.pos_id
		<if test="id!=null and id!='' ">
			and e.emp_id like #{id}
		</if>
		<if test="name!=null and name!='' ">
			and e.emp_name like #{name}
		</if>
		<if test="department_name!=null and department_name!='' ">
			and d.dept_name like #{department_name}
		</if>
		<if test="position_name!=null and position_name!='' ">
			and p.pos_name like #{position_name}
		</if>
		<if test="sex!=null and sex!='' ">
			and e.sex like #{sex}
		</if>
	</select>
	<!-- 根据条件查询员工人数 -->
	<select id="getCount" parameterType="Map" resultType="Integer">
		select count(*) from employee_tb e,dept_tb d,position_tb p where
		e.dept_id = d.dept_id
		and e.pos_id = p.pos_id
		<if test="id!=null and id!='' ">
			and e.emp_id like #{id}
		</if>
		<if test="name!=null and name!='' ">
			and e.emp_name like #{name}
		</if>
		<if test="sex!=null and sex!='' ">
			and e.sex like #{sex}
		</if>
	</select>
	<!-- 添加员工 -->
	<insert id="addEmployee" parameterType="Employee">
		insert
		into employee_tb(emp_id,emp_name,sex,phone,email,address,education,birthday,dept_id,pos_id)
		values(#{id},#{name},#{sex},#{phone},#{email},#{address},#{education},#{birthday},#{department.id},#{position.id})
	</insert>
	<!-- 修改员工 -->
	<update id="updateEmployee" parameterType="Employee">
		update employee_tb set
		emp_name=#{name},sex=#{sex},phone=#{phone},email=#{email},address=#{address},education=#{education},birthday=#{birthday},dept_id=#{department.id},pos_id=#{position.id}
		where emp_id=#{id}
	</update>
	<!-- 删除员工 -->
	<delete id="deleteEmployee" parameterType="String">
		delete from employee_tb
		where
		emp_id=#{id}
	</delete>

</mapper>
```

```checker
- name: check EmployeeDao exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/dao/EmployeeDao.java
  error: 我们发现您还没有创建EmployeeDao.java! /home/project/hrms/src/main/java/com/shiyanlou/dao/EmployeeDao.java
  timeout: 1
- name: check EmployeeMapper exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/resources/mappers/EmployeeMapper.xml
  error: 我们发现您还没有创建EmployeeMapper.xml! /home/project/hrms/src/main/resources/mappers/EmployeeMapper.xml
  timeout: 1
```

## 三、实验总结

到这里我们就完成了 DAO 层的代码实现，下一节我们将进入 Service 层的实现。