---
sort: 29
---

# Controller 层实现

## 一、实验介绍

#### 1.1 实验内容

本节课程主要利用 Spring MVC 框架实现 Controller 层以及一些辅助类的实现。

#### 1.2 实验知识点

- Spring MVC 框架

#### 1.3 实验环境

- JDK1.8
- WEB IDE

## 二、实验步骤

在项目 `hrms` 的目录 `src/main/java` 下新建包 `com.shiyanlou.controller`，作为 Controller 层的包，新建包 `com.shiyanlou.util`，作为辅助类的包，这些辅助类是为了使 Controller 层的代码更好维护，以及实现一些其他功能。

### 2.1 辅助类的实现

#### 2.1.1 DateUtil 类

在包 `com.shiyanlou.util` 下建一个辅助类 `DateUtil`，其中的 `getDate()` 方法的作用是返回格式化的当前日期，代码如下：

```java
package com.shiyanlou.util;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class DateUtil {
	public static Date getDate() throws ParseException{
		Date date = new Date();   
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");  
		return sdf.parse(sdf.format(date));		
	}
}

```

#### 2.1.2 JsonDateValueProcessor 类

在包 `com.shiyanlou.util` 下建一个辅助类 `JsonDateValueProcessor`，其作用是将日期转化使之能在 easyUI 的 datagrid 中正常显示，代码如下：

```java
package com.shiyanlou.util;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Locale;

import net.sf.json.JsonConfig;
import net.sf.json.processors.JsonValueProcessor;

public class JsonDateValueProcessor implements JsonValueProcessor {

    private String format ="yyyy-MM-dd";  
    
    public JsonDateValueProcessor() {  
        super();  
    }  
      
    public JsonDateValueProcessor(String format) {  
        super();  
        this.format = format;  
    }  
  
    public Object processArrayValue(Object paramObject,  
            JsonConfig paramJsonConfig) {  
        return process(paramObject);  
    }  
  
    public Object processObjectValue(String paramString, Object paramObject,  
            JsonConfig paramJsonConfig) {  
        return process(paramObject);  
    }  
      
      
    private Object process(Object value){  
        if(value instanceof Date){    
            SimpleDateFormat sdf = new SimpleDateFormat(format,Locale.CHINA);    
            return sdf.format(value);  
        }    
        return value == null ? "" : value.toString();    
    }
}

```

#### 2.1.3 ResponseUtil 类

在包 `com.shiyanlou.util` 下建一个辅助类 `ResponseUtil`，其 `write()` 方法的作用是将用 HttpServletResponse 返回前台 JSON 格式数据，同时减少 Controller 层代码的冗余，代码如下：


```java
package com.shiyanlou.util;

import java.io.PrintWriter;

import javax.servlet.http.HttpServletResponse;

public class ResponseUtil {
	public static void write(HttpServletResponse response, Object o)
			throws Exception {
		response.setContentType("text/html;charset=utf-8");
		response.addHeader("Access-Control-Allow-Origin", "*");
		PrintWriter out = response.getWriter();
		out.println(o.toString());
		out.flush();
		out.close();
	}
}

```

#### 2.1.4 IntegrateObject 类

在包 `com.shiyanlou.util` 下建一个辅助类 `IntegrateObject`，其 `genericAssociation()` 方法的作用是完成 Employee 与 Department, Position 对象的关联映射，代码如下：

```java
package com.shiyanlou.util;

import com.shiyanlou.domain.Department;
import com.shiyanlou.domain.Employee;
import com.shiyanlou.domain.Position;

public class IntegrateObject {
	/**
	 * 由于部门和职位在 Employee 中是对象关联映射，
	 * 所以不能直接接收参数，需要创建 Department 对象和 Position 对象
	 * */
	public static void genericAssociation(Integer dept_id,Integer pos_id,Employee employee){
		Department department = new Department();
		department.setId(dept_id);
		Position position = new Position();
		position.setId(pos_id);
		employee.setDepartment(department);
		employee.setPosition(position);
	}
}

```

```checker
- name: check DateUtil exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/util/DateUtil.java
  error: 我们发现您还没有创建DateUtil.java! /home/project/hrms/src/main/java/com/shiyanlou/util/DateUtil.java
  timeout: 1
- name: check JsonDateValueProcessor exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/util/JsonDateValueProcessor.java
  error: 我们发现您还没有创建JsonDateValueProcessor.java! /home/project/hrms/src/main/java/com/shiyanlou/util/JsonDateValueProcessor.java
  timeout: 1
- name: check ResponseUtil exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/util/ResponseUtil.java
  error: 我们发现您还没有创建ResponseUtil.java! /home/project/hrms/src/main/java/com/shiyanlou/util/ResponseUtil.java
  timeout: 1
- name: check IntegrateObject exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/util/IntegrateObject.java
  error: 我们发现您还没有创建IntegrateObject.java! /home/project/hrms/src/main/java/com/shiyanlou/util/IntegrateObject.java
  timeout: 1
```

### 2.2 Controller 层代码实现

#### 2.2.1 AdminController

在包 `com.shiyanlou.controller` 下新建一个类 `AdminController`，代码如下：

```java
package com.shiyanlou.controller;

import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import net.sf.json.JSONArray;
import net.sf.json.JSONObject;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.shiyanlou.domain.Admin;
import com.shiyanlou.service.AdminService;
import com.shiyanlou.util.ResponseUtil;

/**类中的所有响应方法都被映射到 /admin 路径下
 * 
 * @author shiyanlou
 *
 */
@Controller
@RequestMapping("/admin")
public class AdminController {
	
	// 自动注入 adminService
    @Autowired
	private AdminService adminService;
	
	/** 处理登录请求
	 * 
	 * @param admin
	 * @param request
	 * @param session
	 * @return
	 */
	@RequestMapping("/login")
	public String login(Admin admin, HttpServletRequest request,
			HttpSession session) {
		Admin resultAdmin = adminService.login(admin);
		// 如果该登录的管理员用户名或密码错误返回错误信息
		if (resultAdmin == null) {
			request.setAttribute("admin", admin);
			request.setAttribute("errorMsg",
					"Please check your username and password!");
			return "login";
		} else {
			// 登录成功， Session 保存该管理员的信息
			session.setAttribute("currentAdmin", resultAdmin);
			session.setAttribute("username", resultAdmin.getUsername());
			return "redirect:main";
		}
	}
	
	/**处理跳转至主页请求
	 * 
	 * @param model
	 * @return
	 * @throws Exception
	 */
    @RequestMapping(value="/main")
    public String test(Model model) throws Exception{ 
        return "home_page";
    }
    
	/**处理查询管理员请求
	 * 
	 * @param admin
	 * @param response
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/list")
	public String list(Admin admin, HttpServletResponse response)
			throws Exception {
		Map<String, Object> map = new HashMap<String, Object>();
		// 判断查询条件是否为空，如果是，对条件做数据库模糊查询的处理
		if (admin.getUsername() != null
				&& !"".equals(admin.getUsername().trim())) {
			map.put("username", "%" + admin.getUsername() + "%");
		}
		List<Admin> adminList = adminService.findAdmins(map);
		Integer total = adminService.getCount(map);
		// 将数据以 JSON 格式返回前端
		JSONObject result = new JSONObject();
		JSONArray jsonArray = JSONArray.fromObject(adminList);
		result.put("rows", jsonArray);
		result.put("total", total);
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**处理保存管理员请求
	 * 
	 * @param admin
	 * @param request
	 * @param response
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/save")
	public String save(Admin admin, HttpServletRequest request,
			HttpServletResponse response) throws Exception {
		int resultTotal = 0;
		// 如果 id 不为空，则添加管理员，否则修改管理员
		if (admin.getId() == null)
			resultTotal = adminService.addAdmin(admin);
		else
			resultTotal = adminService.updateAdmin(admin);
		JSONObject result = new JSONObject();
		if (resultTotal > 0) {
			result.put("success", true);
		} else {
			result.put("success", false);
		}
		ResponseUtil.write(response, result);
		return null;
	}
	
	/** 处理删除管理员请求
	 * 
	 * @param ids
	 * @param response
	 * @param session
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/delete")
	public String delete(@RequestParam(value = "ids") String ids,
			HttpServletResponse response, HttpSession session) throws Exception {
		JSONObject result = new JSONObject();
		// 将要删除的管理员的 id 进行处理
		String[] idsStr = ids.split(",");
		for (int i = 0; i < idsStr.length; i++) {
			// 不能删除超级管理员（superadmin） 和当前登录的管理员
			if (idsStr[i].equals("1")||idsStr[i].equals(((Admin)session.getAttribute("currentAdmin")).getId().toString())){
				result.put("success", false);
				continue;
			}else{
				adminService.deleteAdmin(Integer.parseInt(idsStr[i]));
				result.put("success", true);
			}
		}
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**处理退出请求
	 * 
	 * @param session
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/logout")
	public String logout(HttpSession session) throws Exception {
		session.invalidate();
		return "redirect:/login.jsp";
	}
}
```

#### 2.2.2 PostController

在包 `com.shiyanlou.controller` 下新建一个类 `PostController`，代码如下：

```java
package com.shiyanlou.controller;

import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import net.sf.json.JSONArray;
import net.sf.json.JSONObject;
import net.sf.json.JsonConfig;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.shiyanlou.domain.Admin;
import com.shiyanlou.domain.Post;
import com.shiyanlou.service.PostService;
import com.shiyanlou.util.DateUtil;
import com.shiyanlou.util.JsonDateValueProcessor;
import com.shiyanlou.util.ResponseUtil;

/**类中的所有响应方法都被映射到 /post 路径下
 * 
 * @author shiyanlou
 *
 */
@Controller
@RequestMapping("/post")
public class PostController {
	
	// 自动注入 postService
    @Autowired
	private PostService postService;
	
	/**处理查询公告请求
	 * 
	 * @param post
	 * @param response
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/list")
	public String list(Post post, HttpServletResponse response)
			throws Exception {
		Map<String, Object> map = new HashMap<String, Object>();
		// 判断查询条件是否为空，如果是，对条件做数据库模糊查询的处理
		if (post.getTitle() != null && !"".equals(post.getTitle().trim())) {
			map.put("title", "%" + post.getTitle() + "%");
		}
		List<Post> postList = postService.findPosts(map);
		Integer total = postService.getCount(map);
		
		// 处理日期使之能在 easyUI 的 datagrid 中正常显示
		JsonConfig jsonConfig = new JsonConfig();
		jsonConfig.registerJsonValueProcessor(Date.class,
				new JsonDateValueProcessor());
		// 将数据以 JSON 格式返回前端
		JSONObject result = new JSONObject();
		JSONArray jsonArray = JSONArray.fromObject(postList, jsonConfig);
		result.put("rows", jsonArray);
		result.put("total", total);
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**处理保存公告请求
	 * 
	 * @param post
	 * @param request
	 * @param response
	 * @param session
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/save")
	public String save(Post post, HttpServletRequest request,
			HttpServletResponse response, HttpSession session) throws Exception {
		Admin admin = (Admin)session.getAttribute("currentAdmin");
		post.setAdmin(admin);
		post.setDate(DateUtil.getDate());
		int resultTotal = 0;
		// 如果 id 不为空，则添加公告，否则修改公告
		if (post.getId() == null)
			resultTotal = postService.addPost(post);
		else
			resultTotal = postService.updatePost(post);
		JSONObject result = new JSONObject();
		if (resultTotal > 0) {
			result.put("success", true);
		} else {
			result.put("success", false);
		}
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**处理删除公告请求
	 * 
	 * @param ids
	 * @param response
	 * @param session
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/delete")
	public String delete(@RequestParam(value = "ids") String ids,
			HttpServletResponse response, HttpSession session) throws Exception {
		JSONObject result = new JSONObject();
		// 将要删除的公告的 id 进行处理
		String[] idsStr = ids.split(",");
		for (int i = 0; i < idsStr.length; i++) {
			postService.deletePost(Integer.parseInt(idsStr[i]));

		}
		result.put("success", true);
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**处理根据 id 查询公告请求
	 * 
	 * @param id
	 * @param request
	 * @param response
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/getById")
	public String getById(@RequestParam(value = "id") Integer id,
			HttpServletRequest request, HttpServletResponse response)
			throws Exception {
		Post post = postService.getPostById(id);
		request.setAttribute("postContent", post.getContent());
		return "postContent";
	}
}

```

#### 2.2.3 DeptController

在包 `com.shiyanlou.controller` 下新建一个类 `DeptController`，代码如下：

```java
package com.shiyanlou.controller;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import net.sf.json.JSONArray;
import net.sf.json.JSONObject;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

import com.shiyanlou.domain.Department;
import com.shiyanlou.service.DepartmentService;
import com.shiyanlou.util.ResponseUtil;

/**类中的所有响应方法都被映射到 /dept 路径下
 * 
 * @author shiyanlou
 *
 */
@Controller
@RequestMapping("/dept")
public class DeptController {
	
	// 自动注入 departmentService
    @Autowired
	private DepartmentService departmentService;
	
	/**处理查询部门请求
	 * 
	 * @param department
	 * @param response
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/list")
	public String list(Department department, HttpServletResponse response)
			throws Exception {
		Map<String, Object> map = new HashMap<String, Object>();
		// 判断查询条件是否为空，如果是，对条件做数据库模糊查询的处理
		if (department.getName() != null
				&& !"".equals(department.getName().trim())) {
			map.put("name", "%" + department.getName() + "%");
		}
		List<Department> deptList = departmentService.findDepartments(map);
		Integer total = departmentService.getCount(map);
		JSONObject result = new JSONObject();
		JSONArray jsonArray = JSONArray.fromObject(deptList);
		result.put("rows", jsonArray);
		result.put("total", total);
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**处理保存部门请求
	 * 
	 * @param department
	 * @param request
	 * @param response
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/save")
	public String save(Department department, HttpServletRequest request,
			HttpServletResponse response) throws Exception {
		int resultTotal = 0;
		// 如果 id 不为空，则添加部门，否则修改部门
		if (department.getId() == null)
			resultTotal = departmentService.addDepartment(department);
		else
			resultTotal = departmentService.updateDepartment(department);
		JSONObject result = new JSONObject();
		if (resultTotal > 0) {
			result.put("success", true);
		} else {
			result.put("success", false);
		}
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**处理删除部门请求
	 * 
	 * @param ids
	 * @param response
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/delete")
	public String delete(@RequestParam(value = "ids") String ids,
			HttpServletResponse response) throws Exception {
		JSONObject result = new JSONObject();
		// 将要删除的部门的 id 进行处理
		String[] idsStr = ids.split(",");
		for (int i = 0; i < idsStr.length; i++) {
			// 捕获 service 层抛出的异常，如果捕获到则置 success 值为 false，返回给前端
			try {
				departmentService.deleteDepartment(Integer.parseInt(idsStr[i]));
				result.put("success", true);
			} catch (Exception e) {
				result.put("success", false);
			}					
		}
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**处理获得部门 id 与 name 请求，用于前端 easyUI combobox 的显示
	 * 
	 * @param request
	 * @return
	 */
	@RequestMapping("/getcombobox")
	@ResponseBody
	public JSONArray getDept(HttpServletRequest request) {
		Map<String, Object> map = new HashMap<String, Object>();
		List<Department> deptList = departmentService.findDepartments(map);
		List<Map<String, Object>> list = new ArrayList<Map<String, Object>>();
		for (Department dept : deptList) {
			Map<String, Object> result = new HashMap<String, Object>();
			result.put("id", dept.getId());
			result.put("name", dept.getName());
			list.add(result);
		}
		// 返回 JSON 
		JSONArray jsonArray = JSONArray.fromObject(list);
		return jsonArray;
	}
}

```

#### 2.2.4 PositionController

在包 `com.shiyanlou.controller` 下新建一个类 `PositionController`，代码如下：

```java
package com.shiyanlou.controller;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import net.sf.json.JSONArray;
import net.sf.json.JSONObject;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.ResponseBody;

import com.shiyanlou.domain.Position;
import com.shiyanlou.service.PositionService;
import com.shiyanlou.util.ResponseUtil;

/**类中的所有响应方法都被映射到 /position 路径下
 * 
 * @author shiyanlou
 *
 */
@Controller
@RequestMapping("/position")
public class PositionController {
	
	// 自动注入 positionService
    @Autowired
	private PositionService positionService;
	
	/**处理查询职位请求
	 * 
	 * @param position
	 * @param response
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/list")
	public String list(Position position, HttpServletResponse response)
			throws Exception {
		Map<String, Object> map = new HashMap<String, Object>();
		// 判断查询条件是否为空，如果是，对条件做数据库模糊查询的处理
		if (position.getName() != null
				&& !"".equals(position.getName().trim())) {
			map.put("name", "%" + position.getName() + "%");
		}
		List<Position> dpositionList = positionService.findPositions(map);
		Integer total = positionService.getCount(map);
		JSONObject result = new JSONObject();
		JSONArray jsonArray = JSONArray.fromObject(dpositionList);
		result.put("rows", jsonArray);
		result.put("total", total);
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**处理保存职位请求
	 * 
	 * @param position
	 * @param request
	 * @param response
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/save")
	public String save(Position position, HttpServletRequest request,
			HttpServletResponse response) throws Exception {
		int resultTotal = 0;
		// 如果 id 不为空，则添加职位，否则修改职位
		if (position.getId() == null)
			resultTotal = positionService.addPosition(position);
		else
			resultTotal = positionService.updatePosition(position);
		JSONObject result = new JSONObject();
		if (resultTotal > 0) {
			result.put("success", true);
		} else {
			result.put("success", false);
		}
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**处理删除职位请求
	 * 
	 * @param ids
	 * @param response
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/delete")
	public String delete(@RequestParam(value = "ids") String ids,
			HttpServletResponse response) throws Exception {
		JSONObject result = new JSONObject();
		// 将要删除的部门的 id 进行处理
		String[] idsStr = ids.split(",");
		for (int i = 0; i < idsStr.length; i++) {
			// 捕获 service 层抛出的异常，如果捕获到则置 success 值为 false，返回给前端
			try {
				positionService.deletePosition(Integer.parseInt(idsStr[i]));
				result.put("success", true);
			} catch (Exception e) {
				result.put("success", false);
			}				
		}		
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**处理获得职位 id 与 name 请求，用于前端 easyUI combobox 的显示
	 * 
	 * @param request
	 * @return
	 */
	@RequestMapping("/getcombobox")
	@ResponseBody
	public JSONArray getPos(HttpServletRequest request) {
		Map<String, Object> map = new HashMap<String, Object>();
		List<Position> posList = positionService.findPositions(map);
		List<Map<String, Object>> list = new ArrayList<Map<String, Object>>();
		for (Position pos : posList) {
			Map<String, Object> result = new HashMap<String, Object>();
			result.put("id", pos.getId());
			result.put("name", pos.getName());
			list.add(result);
		}
		// 返回 JSON 
		JSONArray jsonArray = JSONArray.fromObject(list);
		return jsonArray;
	}
}

```

#### 2.2.5 EmployeeController

在包 `com.shiyanlou.controller` 下新建一个类 `EmployeeController`，代码如下：

```java
package com.shiyanlou.controller;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.springframework.beans.factory.annotation.Autowired;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

import net.sf.json.JSONArray;
import net.sf.json.JSONObject;
import net.sf.json.JsonConfig;

import org.springframework.beans.propertyeditors.CustomDateEditor;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.WebDataBinder;
import org.springframework.web.bind.annotation.InitBinder;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;

import com.shiyanlou.domain.Employee;
import com.shiyanlou.domain.Post;
import com.shiyanlou.service.EmployeeService;
import com.shiyanlou.util.IntegrateObject;
import com.shiyanlou.util.JsonDateValueProcessor;
import com.shiyanlou.util.ResponseUtil;

/**类中的所有响应方法都被映射到 /empl 路径下
 * 
 * @author shiyanlou
 *
 */
@Controller
@RequestMapping("/empl")
public class EmployeeController {
	
	// 自动注入 employeeService
    @Autowired
	private EmployeeService employeeService;

	/**处理查询员工请求
	 * 
	 * @param employee
	 * @param request
	 * @param response
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/list")
	public String list(Employee employee, HttpServletRequest request,
			HttpServletResponse response) throws Exception {
		Map<String, Object> map = new HashMap<String, Object>();
		// 判断查询条件是否为空，如果是，对条件做数据库模糊查询的处理
		if (employee.getId() != null && !"".equals(employee.getId().trim())) {
			map.put("id", "%" + employee.getId() + "%");
		}
		if (employee.getName() != null && !"".equals(employee.getName().trim())) {
			map.put("name", "%" + employee.getName() + "%");
		}
		if (employee.getSex() != null && !"".equals(employee.getSex().trim())) {
			map.put("sex", "%" + employee.getSex() + "%");
		}
		if (employee.getDepartment() != null) {
			if (employee.getDepartment().getName() != null
					&& !"".equals(employee.getDepartment().getName().trim())) {
				map.put("department_name", "%"
						+ employee.getDepartment().getName() + "%");
			}
		}
		if (employee.getPosition() != null) {
			if (employee.getPosition().getName() != null
					&& !"".equals(employee.getPosition().getName().trim())) {
				map.put("position_name", "%" + employee.getPosition().getName()
						+ "%");
			}
		}
		List<Post> postList = employeeService.findEmployees(map);
		Integer total = employeeService.getCount(map);
		// 处理日期使之能在 easyUI 的 datagrid 中正常显示
		JsonConfig jsonConfig = new JsonConfig();
		jsonConfig.registerJsonValueProcessor(Date.class,
				new JsonDateValueProcessor());

		JSONObject result = new JSONObject();
		JSONArray jsonArray = JSONArray.fromObject(postList, jsonConfig);
		result.put("rows", jsonArray);
		result.put("total", total);
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**处理保存员工请求
	 * 
	 * @param dept_id
	 * @param pos_id
	 * @param updateFlag
	 * @param employee
	 * @param request
	 * @param response
	 * @param session
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/save")
	public String save(@RequestParam("dept_id") Integer dept_id,
			@RequestParam("pos_id") Integer pos_id, @RequestParam("updateFlag") String updateFlag, Employee employee,
			HttpServletRequest request, HttpServletResponse response,
			HttpSession session) throws Exception {
		int resultTotal = 0;
		// 完成 Department 和 Position 在 Employee 中的关联映射
		IntegrateObject.genericAssociation(dept_id, pos_id, employee);
		
		JSONObject result = new JSONObject();
		// 根据 updateFlag 的值，判断保存方式，如果值为 no，则添加员工，如果值为 yes，则修改员工
		if (updateFlag.equals("no")){
			// 捕获 service 层插入时主键重复抛出的异常，如果捕获到则置 success 值为 false，返回给前端
			try {
				resultTotal = employeeService.addEmployee(employee);
				if (resultTotal > 0) {
					result.put("success", true);
				} else {
					result.put("success", false);
				}
			} catch (Exception e) {
				result.put("success", false);
			}
			
		}else if(updateFlag.equals("yes")){
			resultTotal = employeeService.updateEmployee(employee);
			if (resultTotal > 0) {
				result.put("success", true);
			} else {
				result.put("success", false);
			}
		}
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**处理删除员工请求
	 * 
	 * @param ids
	 * @param response
	 * @param session
	 * @return
	 * @throws Exception
	 */
	@RequestMapping("/delete")
	public String delete(@RequestParam(value = "ids") String ids,
			HttpServletResponse response, HttpSession session) throws Exception {
		JSONObject result = new JSONObject();
		// 将要删除的部门的 id 进行处理
		String[] idsStr = ids.split(",");
		for (int i = 0; i < idsStr.length; i++) {
			employeeService.deleteEmployee(idsStr[i]);

		}
		result.put("success", true);
		ResponseUtil.write(response, result);
		return null;
	}
	
	/**springmvc 日期绑定
	 * 
	 * @param binder
	 */
	@InitBinder
	public void initBinder(WebDataBinder binder) {
		SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd");
		CustomDateEditor editor = new CustomDateEditor(dateFormat, true);
		binder.registerCustomEditor(Date.class, editor);
	}

}

```

```checker
- name: check AdminController exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/controller/AdminController.java
  error: 我们发现您还没有创建AdminController.java! /home/project/hrms/src/main/java/com/shiyanlou/controller/AdminController.java
  timeout: 1
- name: check PostController exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/controller/PostController.java
  error: 我们发现您还没有创建PostController.java! /home/project/hrms/src/main/java/com/shiyanlou/controller/PostController.java
  timeout: 1
- name: check DeptController exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/controller/DeptController.java
  error: 我们发现您还没有创建DeptController.java! /home/project/hrms/src/main/java/com/shiyanlou/controller/DeptController.java
  timeout: 1
- name: check PositionController exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/controller/PositionController.java
  error: 我们发现您还没有创建PositionController.java! /home/project/hrms/src/main/java/com/shiyanlou/controller/PositionController.java
  timeout: 1
- name: check EmployeeController exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/java/com/shiyanlou/controller/EmployeeController.java
  error: 我们发现您还没有创建EmployeeController.java! /home/project/hrms/src/main/java/com/shiyanlou/controller/EmployeeController.java
  timeout: 1
```

## 三、实验总结

到这里我们就完成了 Controller 层的代码实现，下一节我们将完成表现层 JSP 页面的实现。