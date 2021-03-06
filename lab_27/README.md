---
sort: 27
---

# Service 层实现

## 一、实验介绍

#### 1.1 实验内容

本节课程主要利用 Spring 框架实现 Service 层。

#### 1.2 实验知识点

- Spring 框架

#### 1.3 实验环境

- JDK1.8
- WEB IDE

## 二、实验步骤

在项目 `hrms` 的目录 `src/main/java` 下新建包 `com.shiyanlou.service`，作为 Servcie 层接口的包，新建包 `com.shiyanlou.service.impl` 作为 Servcie 层实现的包。

### 2.1 AdminService 接口与实现

在包 `com.shiyanlou.service` 下建一个 `AdminService.java` 接口文件，代码如下：

```java
package com.shiyanlou.service;

import java.util.List;
import java.util.Map;

import com.shiyanlou.domain.Admin;

public interface AdminService {

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

在包 `com.shiyanlou.service.impl` 下建一个类 `AdminServiceImpl` 实现上述的接口，代码如下：

```java
package com.shiyanlou.service.impl;

import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;

import org.springframework.stereotype.Service;

import com.shiyanlou.dao.AdminDao;
import com.shiyanlou.domain.Admin;
import com.shiyanlou.service.AdminService;

@Service("adminService")
public class AdminServiceImpl implements AdminService {

    @Autowired
    private AdminDao adminDao;

    public Admin login(Admin admin) {
        return adminDao.login(admin);
    }

    public List<Admin> findAdmins(Map<String, Object> map) {

        return adminDao.findAdmins(map);
    }

    public Integer getCount(Map<String, Object> map) {

        return adminDao.getCount(map);
    }

    public Integer addAdmin(Admin admin) {

        return adminDao.addAdmin(admin);
    }

    public Integer updateAdmin(Admin admin) {

        return adminDao.updateAdmin(admin);
    }

    public Integer deleteAdmin(Integer id) {

        return adminDao.deleteAdmin(id);
    }

}

```

```checker
- name: check AdminService exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/service/AdminService.java
  error: 我们发现您还没有创建AdminService.java! /home/project/hrms/src/main/java/com/shiyanlou/service/AdminService.java
  timeout: 1
- name: check AdminServiceImpl exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/service/impl/AdminServiceImpl.java
  error: 我们发现您还没有创建AdminServiceImpl.java! /home/project/hrms/src/main/java/com/shiyanlou/service/impl/AdminServiceImpl.java
  timeout: 1
```

### 2.2 PostService 接口与实现

在包 `com.shiyanlou.service` 下建一个 `PostService.java` 接口文件，代码如下：

```java
package com.shiyanlou.service;

import java.util.List;
import java.util.Map;

import com.shiyanlou.domain.Post;

public interface PostService {
	
	/** 根据条件查询公告
	 * 
	 * @param map
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

在包 `com.shiyanlou.service.impl` 下建一个类 `PostServiceImpl` 实现上述的接口，代码如下：

```java
package com.shiyanlou.service.impl;

import java.util.List;
import java.util.Map;


import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.shiyanlou.dao.PostDao;
import com.shiyanlou.domain.Post;
import com.shiyanlou.service.PostService;
@Service("postService")
public class PostServiceImpl implements PostService {
	
	@Autowired
	private PostDao postDao;

	public List<Post> findPosts(Map<String, Object> map) {

		return postDao.findPosts(map);
	}

	public Integer getCount(Map<String, Object> map) {
		
		return postDao.getCount(map);
	}

	public Integer addPost(Post post) {
		
		return postDao.addPost(post);
	}

	public Integer updatePost(Post post) {

		return postDao.updatePost(post);
	}

	public Integer deletePost(Integer id) {

		return postDao.deletePost(id);
	}

	public Post getPostById(Integer id) {

		return postDao.getPostById(id);
	}

}

```

```checker
- name: check PostService exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/service/PostService.java
  error: 我们发现您还没有创建PostService.java! /home/project/hrms/src/main/java/com/shiyanlou/service/PostService.java
  timeout: 1
- name: check PostServiceImpl exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/service/impl/PostServiceImpl.java
  error: 我们发现您还没有创建PostServiceImpl.java! /home/project/hrms/src/main/java/com/shiyanlou/service/impl/PostServiceImpl.java
  timeout: 1
```

### 2.3 DepartmentService 接口与实现

在包 `com.shiyanlou.service` 下建一个 `DepartmentService.java` 接口文件，代码如下：

```java
package com.shiyanlou.service;

import java.util.List;
import java.util.Map;

import com.shiyanlou.domain.Department;

public interface DepartmentService {

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

在包 `com.shiyanlou.service.impl` 下建一个类 `DepartmentServiceImpl` 实现上述的接口，代码如下：

```java
package com.shiyanlou.service.impl;

import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.shiyanlou.dao.DepartmentDao;
import com.shiyanlou.domain.Department;
import com.shiyanlou.service.DepartmentService;

@Service("departmentService")
public class DepartmentServiceImpl implements DepartmentService {

    @Autowired
    private DepartmentDao departmentDao;

    public List<Department> findDepartments(Map<String, Object> map) {

        return departmentDao.findDepartments(map);
    }

    public Integer getCount(Map<String, Object> map) {

        return departmentDao.getCount(map);
    }

    public Integer addDepartment(Department department) {

        return departmentDao.addDepartment(department);
    }

    public Integer updateDepartment(Department department) {

        return departmentDao.updateDepartment(department);
    }

    public Integer deleteDepartment(Integer id) {
        Integer flag = null;
        try {
            flag = departmentDao.deleteDepartment(id);
        } catch (Exception e) {
            throw new RuntimeException();
        }

        return flag;
    }
}

```

```checker
- name: check DepartmentService exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/service/DepartmentService.java
  error: 我们发现您还没有创建DepartmentService.java! /home/project/hrms/src/main/java/com/shiyanlou/service/DepartmentService.java
  timeout: 1
- name: check DepartmentServiceImpl exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/service/impl/DepartmentServiceImpl.java
  error: 我们发现您还没有创建DepartmentServiceImpl.java! /home/project/hrms/src/main/java/com/shiyanlou/service/impl/DepartmentServiceImpl.java
  timeout: 1
```

### 2.4 PositionService 接口与实现

在包 `com.shiyanlou.service` 下建一个 `PositionService.java` 接口文件，代码如下：

```java
package com.shiyanlou.service;

import java.util.List;
import java.util.Map;

import com.shiyanlou.domain.Position;

public interface PositionService {
	
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
	
	/* 修改职位
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

在包 `com.shiyanlou.service.impl` 下建一个类 `PositionServiceImpl` 实现上述的接口，代码如下：

```java
package com.shiyanlou.service.impl;

import java.util.List;
import java.util.Map;


import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.shiyanlou.dao.PositionDao;
import com.shiyanlou.domain.Position;
import com.shiyanlou.service.PositionService;

@Service("positionService")
public class PositionServiceImpl implements PositionService {

    @Autowired
	private PositionDao positionDao;

	public List<Position> findPositions(Map<String, Object> map) {

		return positionDao.findPositions(map);
	}

	public Integer getCount(Map<String, Object> map) {

		return positionDao.getCount(map);
	}

	public Integer addPosition(Position position) {

		return positionDao.addPosition(position);
	}

	public Integer updatePosition(Position position) {

		return positionDao.updatePosition(position);
	}

	public Integer deletePosition(Integer id) {
		Integer flag = null;
		try {
			flag = positionDao.deletePosition(id);
		} catch (Exception e) {
			throw new RuntimeException();
		}

		return flag;
	}

}

```

```checker
- name: check PositionService exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/service/PositionService.java
  error: 我们发现您还没有创建PositionService.java! /home/project/hrms/src/main/java/com/shiyanlou/service/PositionService.java
  timeout: 1
- name: check PositionServiceImpl exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/service/impl/PositionServiceImpl.java
  error: 我们发现您还没有创建PositionServiceImpl.java! /home/project/hrms/src/main/java/com/shiyanlou/service/impl/PositionServiceImpl.java
  timeout: 1
```

### 2.5 EmployeeService 接口与实现

在包 `com.shiyanlou.service` 下建一个 `EmployeeService.java` 接口文件，代码如下：

```java
package com.shiyanlou.service;

import java.util.List;
import java.util.Map;

import com.shiyanlou.domain.Employee;
import com.shiyanlou.domain.Post;

public interface EmployeeService {

	/** 根据条件查询员工
	 * 
	 * @param map
	 * @return
	 */
	public List<Post>findEmployees(Map<String, Object> map);
	
	/** 根据条件查询员工人数
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

在包 `com.shiyanlou.service.impl` 下建一个类 `EmployeeServiceImpl` 实现上述的接口，代码如下：

```java
package com.shiyanlou.service.impl;

import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.shiyanlou.dao.EmployeeDao;
import com.shiyanlou.domain.Employee;
import com.shiyanlou.domain.Post;
import com.shiyanlou.service.EmployeeService;

@Service("employeeService")
public class EmployeeServiceImpl implements EmployeeService {

	@Autowired
	private EmployeeDao employeeDao;
	
	public List<Post> findEmployees(Map<String, Object> map) {
		
		return employeeDao.findEmployees(map);
	}

	public Integer getCount(Map<String, Object> map) {
		
		return employeeDao.getCount(map);
	}

	public Integer addEmployee(Employee employee) {
		Integer flag = null;
		try {
			//如果插入主键重复，抛出异常
			flag =  employeeDao.addEmployee(employee);
		} catch (Exception e) {
			throw new RuntimeException();
		}
		
		return flag;
	}

	public Integer updateEmployee(Employee employee) {
		
		return employeeDao.updateEmployee(employee);
	}

	public Integer deleteEmployee(String id) {
		
		return employeeDao.deleteEmployee(id);
	}

}

```

```checker
- name: check EmployeeService exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/service/EmployeeService.java
  error: 我们发现您还没有创建EmployeeService.java! /home/project/hrms/src/main/java/com/shiyanlou/service/EmployeeService.java
  timeout: 1
- name: check EmployeeServiceImpl exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/service/impl/EmployeeServiceImpl.java
  error: 我们发现您还没有创建EmployeeServiceImpl.java! /home/project/hrms/src/main/java/com/shiyanlou/service/impl/EmployeeServiceImpl.java
  timeout: 1
```

## 三、实验总结

到这里我们就完成了 Service 层的代码实现，下一节我们将完成 MyBatis 和 Spring 的整合及测试。
