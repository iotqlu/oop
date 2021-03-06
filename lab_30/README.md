---
sort: 30
---

# 表现层 JSP 页面实现

## 一、实验介绍

#### 1.1 实验内容

本节课程主要利用 easyUI 实现系统的前端页面。

#### 1.2 实验知识点

- easyUI
- JavaScript
- html

#### 1.3 实验环境

- JDK1.8
- WEB IDE

## 二、实验步骤

在本章节中将实现表现层即jsp页面，做了这么多工作终于看到界面了啊！

### 2.1 表现层相关文件获取

打开终端，输入以下命令获取相关文件，包括 easyui、jQuery 等相关文件，保存到目录 `/home/shiyanlou/` 下。

```
wget -P /home/shiyanlou/ http://labfile.oss.aliyuncs.com/courses/824/fontendFiles.tar
```

解压文件。

```
tar zxvf /home/shiyanlou/fontendFiles.tar
```

然后将 `fontendFiles` 文件夹下的全部文件夹拷贝到项目 `hrms` 的 `src->main->webapp` 下。

```
mv fontendFiles/* /home/project/hrms/src/main/webapp/
rm -r fontendFiles
```

### 2.2 login.jsp

在项目 `hrms` 的 `src->main->webapp` 下新建 JSP 页面 login.jsp 作为系统的登录页面，并添加如下代码：

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//Dtd HTML 4.01 Transitional//EN">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>hrms-login</title>
<script type="text/javascript"
	src="${pageContext.request.contextPath}/js/jquery.min.js"></script>
<style type=text/css>
body {
	text-align: center;
	padding-bottom: 0px;
	background-color: #ddeef2;
	margin: 0px;
	padding-left: 0px;
	padding-right: 0px;
	padding-top: 0px
}

A:link {
	COLOR: #000000;
	text-decoration: none
}

A:visited {
	COLOR: #000000;
	text-decoration: none
}

A:hover {
	COLOR: #ff0000;
	text-decoration: underline
}

A:active {
	text-decoration: none
}

.input {
	border-bottom: #ccc 1px solid;
	border-left: #ccc 1px solid;
	line-height: 20px;
	width: 182px;
	height: 20px;
	border-top: #ccc 1px solid;
	border-right: #ccc 1px solid
}

.input1 {
	border-bottom: #ccc 1px solid;
	border-left: #ccc 1px solid;
	line-height: 20px;
	width: 120px;
	height: 20px;
	border-top: #ccc 1px solid;
	border-right: #ccc 1px solid;
}
</style>
<script type="text/javascript">
	/* 登录 */
	function login() {
		var username = $("#username").val();
		var password = $("#password").val();
		if (username == null || username == "") {
			alert("username can't be empty\uff01");
			return;
		}
		if (password == null || password == "") {
			alert("password can't be empty\uff01");
			return;
		}
		$("#adminlogin").submit();

	}
	/* 用户名或密码错误时显示 */
	if ('${errorMsg}' != '') {
		alert('${errorMsg}');
	}
	/* 拦截器显示信息 */
	if ('${message}' != '') {
		alert('${message}');
	}
</script>
</head>
<body>
	<form id=adminlogin method=post name=adminlogin
		action="${pageContext.request.contextPath}/admin/login">
		<div></div>
		<table style="margin: auto; width: 100%; height: 100%" border=0
			cellSpacing=0 cellPadding=0>
			<tbody>
				<tr>
					<td height=150>&nbsp;</td>
				</tr>
				<tr style="height: 254px">
					<td>
						<div style="margin: 0px auto; width: 868px">
							<img style="display: block"
								src="${pageContext.request.contextPath}/images/body_03.jpg">
						</div>
						<div style="background-color: #278296">
							<div style="margin: 0px auto; width: 936px">
								<div
									style="BACKGROUND: url(${pageContext.request.contextPath}/images/body_05.jpg) no-repeat; height: 155px">
									<div
										style="text-align: left; width: 265px; float: right; height: 125px; _height: 95px">
										<table border=0 cellSpacing=0 cellPadding=0 width="100%">
											<tbody>
												<tr>
													<td style="height: 45px"><input type="text"
														class=input value="${admin.username}" name="username"
														id="username"></td>
												</tr>
												<tr>
													<td><input type="password" class=input
														value="${admin.password}" name="password" id="password" /></td>
												</tr>
											</tbody>
										</table>
									</div>
									<div style="height: 1px; clear: both"></div>
									<div style="width: 380px; float: right; clear: both">
										<table border=0 cellSpacing=0 cellPadding=0 width=300>
											<tbody>

												<tr>
													<td width=100 align=right><input
														style="border-right-width: 0px; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px"
														id=btnLogin
														src="${pageContext.request.contextPath}/images/btn1.jpg"
														type=image name=btnLogin onclick="javascript:login();"></td>
													<td width=100 align=middle><input
														style="border-right-width: 0px; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px"
														id=btnReset
														src="${pageContext.request.contextPath}/images/btn2.jpg"
														type=image name=btnReset
														onclick="javascript:adminlogin.reset();"></td>
												</tr>
											</tbody>
										</table>
									</div>
								</div>
							</div>
						</div>
						<div style="margin: 0px auto; width: 936px">
							<img src="${pageContext.request.contextPath}/images/body_06.jpg">
						</div>
					</td>
				</tr>
				<tr style="height: 30%">
					<td>&nbsp;</td>
				</tr>
			</tbody>
		</table>
	</form>
</body>
</html>
```

```checker
- name: check login exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/webapp/login.jsp
  error: 我们发现您还没有创建login.jsp! /home/project/hrms/src/main/webapp/login.jsp
  timeout: 1
```

### 2.3 home_page.jsp

在项目 `hrms` 的 `src->main->webapp` 下新建 JSP 页面 home_page.jsp 作为系统的主页面，并添加如下代码：

```jsp
<%@ page language="java" contentType="text/html; charset=utf-8"
	pageEncoding="utf-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<title>hrms_main</title>
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/jquery-easyui-1.3.3/themes/default/easyui.css">
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/jquery-easyui-1.3.3/themes/icon.css">
<script type="text/javascript"
	src="${pageContext.request.contextPath}/jquery-easyui-1.3.3/jquery.min.js"></script>
<script type="text/javascript"
	src="${pageContext.request.contextPath}/jquery-easyui-1.3.3/jquery.easyui.min.js"></script>
<script type="text/javascript">
	var url;
	function addTab(url, text, iconCls) {
		var content = "<iframe frameborder=0 scrolling='auto' style='width:100%;height:100%' src='${pageContext.request.contextPath}/views/"
				+ url + "'></iframe>";
		$("#tabs").tabs("add", {
			title : text,
			iconCls : iconCls,
			closable : true,
			content : content
		});
	}
	function openTab(text, url, iconCls) {
		if ($("#tabs").tabs("exists", text)) {
			$("#tabs").tabs("close", text);
			addTab(url, text, iconCls);
			$("#tabs").tabs("select", text);
		} else {
			addTab(url, text, iconCls);
		}
	}
	/* 退出 */
	function logout() {
		$.messager
				.confirm(
						"system prompt",
						"Do you want to exit?",
						function(r) {
							if (r) {
								window.location.href = "${pageContext.request.contextPath}/admin/logout";
							}
						});
	}
</script>
<body class="easyui-layout">
	<div region="north" style="height: 78px; background-color: #ffff">
		<table width="100%">
			<tr>
				<td width="50%"></td>
				<td valign="bottom"
					style="font-size: 20px; color: #8B8B8B; font-family: '\u6977\u4f53';"
					align="right" width="50%"><font size="3">&nbsp;&nbsp;<strong>Current
							Admin\uff1a</strong>
				</font> <font color="red">${sessionScope.currentAdmin.username}</font></td>
			</tr>
		</table>
	</div>
	<div region="center">
		<div class="easyui-tabs" fit="true" border="false" id="tabs">
			<div title="Home" data-options="iconCls:'icon-home'">
				<div align="center" style="padding-top: 50px">
					<font color="grey" size="10">Human Affairs Management System</font>
				</div>
				<div align="center" style="padding-top: 20px;">
					<font style="font-size: 20px;">www.shiyanlou.com</font>
				</div>
			</div>
		</div>
	</div>
	<div region="west" style="width: 200px; height: 500px;"
		title="Navigation Menu" split="true">
		<div class="easyui-accordion">
			<div title="Department Manage"
				data-options="selected:true,iconCls:'icon-shujias'"
				style="padding: 10px; height: 10px;">
				<a href="javascript:openTab(' Department Info','deptManage.jsp')"
					class="easyui-linkbutton" data-options="plain:true"
					style="width: 150px;"> Department Info</a>
			</div>

			<div title="Position Manage"
				data-options="selected:true,iconCls:'icon-schoolceo'"
				style="padding: 10px; height: 10px;">
				<a href="javascript:openTab(' Position Info','positionManage.jsp')"
					class="easyui-linkbutton" data-options="plain:true"
					style="width: 150px;"> Position Info</a>
			</div>

			<div title="Employee Manage" data-options="iconCls:'icon-students' "
				style="padding: 10px">
				<a href="javascript:openTab(' Employee Info','employeeManage.jsp')"
					class="easyui-linkbutton" data-options="plain:true"
					style="width: 150px;">Employee Info</a>
			</div>

			<div title="Post Manage"
				data-options="selected:true,iconCls:'icon-wenzhang'"
				style="padding: 10px; height: 10px;">
				<a href="javascript:openTab(' Post Info','postManage.jsp')"
					class="easyui-linkbutton" data-options="plain:true"
					style="width: 150px;"> Post Info</a>
			</div>

			<div title="System Manage" data-options="iconCls:'icon-item'"
				style="padding: 10px; border: none;">
				<a
					href="javascript:openTab(' Admin List','adminManage.jsp','icon-lxr')"
					class="easyui-linkbutton"
					data-options="plain:true,iconCls:'icon-lxr'" style="width: 150px;">
					Admin List</a><a href="javascript:logout()" class="easyui-linkbutton"
					data-options="plain:true,iconCls:'icon-exit'" style="width: 150px;">
					Exit</a>
			</div>
		</div>
	</div>
</body>
</html>
```
```checker
- name: check home page exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/webapp/home_page.jsp
  error: 我们发现您还没有创建home_page.jsp! /home/project/hrms/src/main/webapp/home_page.jsp
  timeout: 1
```

### 2.4 postContent.jsp

在项目 `hrms` 的 `src->main->webapp` 下新建 JSP 页面 postContent.jsp 作为公告内容详情的展示页面，并添加如下代码：

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Post Content</title>
</head>
<body>${requestScope.postContent}
</body>
</html>
```

```checker
- name: check postContent exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/webapp/postContent.jsp
  error: 我们发现您还没有创建postContent.jsp! /home/project/hrms/src/main/webapp/postContent.jsp
  timeout: 1
```

### 2.5 五大功能模块 JSP 页面

在项目 `hrms` 的 `src->main->webapp` 下新建 Folder `views`。

#### 2.5.1 adminManage.jsp

在项目 `hrms` 的 `src->main->webapp->views` 下新建 JSP 页面 adminManage.jsp 作为系统管理页面，并添加如下代码：

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/jquery-easyui-1.3.3/themes/default/easyui.css">
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/jquery-easyui-1.3.3/themes/icon.css">
<script type="text/javascript"
	src="${pageContext.request.contextPath}/jquery-easyui-1.3.3/jquery.min.js"></script>
<script type="text/javascript"
	src="${pageContext.request.contextPath}/jquery-easyui-1.3.3/jquery.easyui.min.js"></script>
<script type="text/javascript">
	var url;
	/* 根据条件查询管理员 */
	function searchAdmin() {
		$("#dg").datagrid('load', {
			"username" : $("#s_username").val()
		});
	}
	/* 删除管理员，可以是多个 */
	function deleteAdmin() {
		var selectedRows = $("#dg").datagrid('getSelections');
		if (selectedRows.length == 0) {
			$.messager.alert("system prompt",
					"Please choose the data to delete!");
			return;
		}
		var strIds = [];
		for ( var i = 0; i < selectedRows.length; i++) {
			strIds.push(selectedRows[i].id);
		}
		var ids = strIds.join(",");
		$.messager
				.confirm(
						"system prompt",
						"Do you want to delete the <font color=red>"
								+ selectedRows.length + "</font> data?",
						function(r) {
							if (r) {
								$
										.post(
												"${pageContext.request.contextPath}/admin/delete",
												{
													ids : ids
												},
												function(result) {
													if (result.success) {
														$.messager
																.alert(
																		"system prompt",
																		"Delete successful!");
														$("#dg").datagrid(
																"reload");
													} else {
														$.messager
																.alert(
																		"system prompt",
																		"Can't delete superAdmin or current admin!");
													}
												}, "json");
							}
						});
	}

	function openAdminAddDialog() {
		$("#dlg").dialog("open").dialog("setTitle", "Add new admin");
		url = "${pageContext.request.contextPath}/admin/save";
	}
	/* 保存管理员，根据不同的 url 选择是添加还是修改 */
	function saveAdmin() {
		$("#fm").form("submit", {
			url : url,
			onSubmit : function() {
				return $(this).form("validate");
			},
			success : function(result) {
				$.messager.alert("system prompt", "Save successful!");
				resetValue();
				$("#dlg").dialog("close");
				$("#dg").datagrid("reload");
			}
		});
	}

	function openAdminModifyDialog() {
		var selectedRows = $("#dg").datagrid('getSelections');
		if (selectedRows.length != 1) {
			$.messager.alert("system prompt", "Please choose a data to edit!");
			return;
		}
		var row = selectedRows[0];
		if (row.id == 1) {
			$.messager.alert("system prompt",
					"Can't modity superadmin' information!");
			return;
		}
		$("#dlg").dialog("open").dialog("setTitle", "Edit admin information");
		$('#fm').form('load', row);
		$("#password").val("******");
		url = "${pageContext.request.contextPath}/admin/save?id=" + row.id;
	}

	function resetValue() {
		$("#username").val("");
		$("#password").val("");
	}

	function closeAdminDialog() {
		$("#dlg").dialog("close");
		resetValue();
	}
</script>
</head>
<body style="margin: 1px;">
	<table id="dg" title="Admin Manage" class="easyui-datagrid"
		fitColumns="true" pagination="true" rownumbers="true"
		url="${pageContext.request.contextPath}/admin/list" fit="true"
		toolbar="#tb">
		<thead>
			<tr>
				<th field="cb" checkbox="true" align="center"></th>
				<th field="id" width="50" align="center">id</th>
				<th field="username" width="80" align="center">username</th>
				<th field="role_name" width="80" align="center">role_name</th>
			</tr>
		</thead>
	</table>
	<div id="tb">
		<div>
			<a href="javascript:openAdminAddDialog()" class="easyui-linkbutton"
				iconCls="icon-add" plain="true">Add</a> <a
				href="javascript:openAdminModifyDialog()" class="easyui-linkbutton"
				iconCls="icon-edit" plain="true">Modify</a> <a
				href="javascript:deleteAdmin()" class="easyui-linkbutton"
				iconCls="icon-remove" plain="true">Delete</a>
		</div>
		<div>
			&nbsp;Username:&nbsp;<input type="text" id="s_username" size="20"
				onkeydown="if(event.keyCode==13) searchAdmin()" /> <a
				href="javascript:searchAdmin()" class="easyui-linkbutton"
				iconCls="icon-search" plain="true">Search</a>
		</div>
	</div>

	<div id="dlg" class="easyui-dialog"
		style="width: 620px; height: 250px; padding: 10px 20px" closed="true"
		buttons="#dlg-buttons">
		<form id="fm" method="post">
			<table cellspacing="8px">
				<tr>
					<td>Username:</td>
					<td><input type="text" id="username" name="username"
						class="easyui-validatebox" required="true" />&nbsp;<font
						color="red">*</font></td>
				</tr>
				<tr>
					<td>Password:</td>
					<td><input type="text" id="password" name="password"
						class="easyui-validatebox" required="true" />&nbsp;<font
						color="red">*</font></td>
				</tr>
			</table>
		</form>
	</div>

	<div id="dlg-buttons">
		<a href="javascript:saveAdmin()" class="easyui-linkbutton"
			iconCls="icon-ok">Save</a> <a href="javascript:closeAdminDialog()"
			class="easyui-linkbutton" iconCls="icon-cancel">Close</a>
	</div>
</body>
</html>
```

#### 2.5.2 postManage.jsp

在项目 `hrms` 的 `src->main->webapp->views` 下新建 JSP 页面 postManage.jsp 作为公告管理页面，并添加如下代码：

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>postManage</title>
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/jquery-easyui-1.3.3/themes/default/easyui.css">
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/jquery-easyui-1.3.3/themes/icon.css">
<script type="text/javascript"
	src="${pageContext.request.contextPath}/jquery-easyui-1.3.3/jquery.min.js"></script>
<script type="text/javascript"
	src="${pageContext.request.contextPath}/jquery-easyui-1.3.3/jquery.easyui.min.js"></script>
<script type="text/javascript"
	src="${pageContext.request.contextPath}/ueditor/ueditor.config.js">
	
</script>
<script type="text/javascript"
	src="${pageContext.request.contextPath}/ueditor/ueditor.all.min.js">
	
</script>
<script type="text/javascript"
	src="${pageContext.request.contextPath}/js/common.js"></script>
<script type="text/javascript">
	var url;
	function ResetEditor() {
		UE.getEditor('myEditor', {
			initialFrameHeight : 480,
			initialFrameWidth : 660,
			enableAutoSave : false,
			elementPathEnabled : false,
			wordCount : false,

		});
	}
	/* 根据条件查询公告 */
	function searchPost() {
		$("#dg").datagrid('load', {
			"title" : $("#postTitle").val(),
		});
	}
	/* 删除公告，可以是多个 */
	function deletePost() {
		var selectedRows = $("#dg").datagrid('getSelections');
		if (selectedRows.length == 0) {
			$.messager.alert("system prompt", "Please choose a data to edit!");
			return;
		}
		var strIds = [];
		for ( var i = 0; i < selectedRows.length; i++) {
			strIds.push(selectedRows[i].id);
		}
		var ids = strIds.join(",");
		$.messager
				.confirm(
						"Please choose a data to edit!",
						"Do you want to delete the <font color=red>"
								+ selectedRows.length + "</font> data?",
						function(r) {
							if (r) {
								$
										.post(
												"${pageContext.request.contextPath}/post/delete",
												{
													ids : ids
												},
												function(result) {
													if (result.success) {
														$.messager
																.alert(
																		"system prompt",
																		"Delete successful!");
														$("#dg").datagrid(
																"reload");
													} else {
														$.messager
																.alert(
																		"system prompt",
																		"Delete error!");
													}
												}, "json");
							}
						});

	}

	function openPostAddDialog() {
		var html = '<div id="myEditor" name="content"></div>';
		$('#editor').append(html);
		ResetEditor(editor);
		var ue = UE.getEditor('myEditor');
		ue.ready(function() {
			ue.setContent("");
		});

		$("#dlg").dialog("open").dialog("setTitle", "Add post");
		url = "${pageContext.request.contextPath}/post/save";

	}
	/* 保存公告，根据不同的 url 选择是添加还是修改 */
	function savePost() {
		$("#fm").form("submit", {
			url : url,
			onSubmit : function() {
				return $(this).form("validate");
			},
			success : function(result) {
				$.messager.alert("system prompt", "Save successful!");
				$("#dlg").dialog("close");
				$("#dg").datagrid("reload");
				resetValue();
			}
		});
	}

	function openPostModifyDialog() {
		var selectedRows = $("#dg").datagrid('getSelections');
		if (selectedRows.length != 1) {
			$.messager.alert("system prompt", "Please choose a data to edit!");
			return;
		}
		var row = selectedRows[0];
		$("#dlg").dialog("open").dialog("setTitle", "Edit post");
		$('#fm').form('load', row);
		var html = '<div id="myEditor" name="content"></div>';
		$('#editor').append(html);
		ResetEditor(editor);
		var ue = UE.getEditor('myEditor');
		ue.ready(function() {
			ue.setContent(row.content);
		});

		url = "${pageContext.request.contextPath}/post/save?id=" + row.id;
	}

	function formatHref(val, row) {
		return "<a href='${pageContext.request.contextPath}/post/getById?id="
				+ row.id + "' target='_blank'>Show Content</a>";
	}

	function resetValue() {
		$("#title").val("");
		$("#container").val("");
		ResetEditor();
	}

	function closePostDialog() {
		$("#dlg").dialog("close");
		resetValue();
	}
</script>
</head>
<body style="margin: 1px;" id="ff">
	<table id="dg" title="Post Manage" class="easyui-datagrid"
		pagination="true" rownumbers="true" fit="true"
		data-options="pageSize:10"
		url="${pageContext.request.contextPath}/post/list" toolbar="#tb">
		<thead data-options="frozen:true">
			<tr>
				<th field="cb" checkbox="true" align="center"></th>
				<th field="id" width="10%" align="center" hidden="true">id</th>
				<th field="title" width="300" align="center">title</th>
				<th field="date" width="150" align="center">create_date</th>
				<th field="admin.username" width="150" align="center">announcer</th>
				<th field="content" width="150" align="center"
					formatter="formatHref">operation</th>
			</tr>
		</thead>
	</table>
	<div id="tb">
		<div>
			<a href="javascript:openPostAddDialog()" class="easyui-linkbutton"
				iconCls="icon-add" plain="true">Add</a> <a
				href="javascript:openPostModifyDialog()" class="easyui-linkbutton"
				iconCls="icon-edit" plain="true">Modify</a> <a
				href="javascript:deletePost()" class="easyui-linkbutton"
				iconCls="icon-remove" plain="true">Delete</a>
		</div>
		<div>
			&nbsp;Title:&nbsp;<input type="text" id="postTitle" size="20"
				onkeydown="if(event.keyCode==13) searchPost()" />&nbsp; <a
				href="javascript:searchPost()" class="easyui-linkbutton"
				iconCls="icon-search" plain="true">Search</a>
		</div>
	</div>

	<div id="dlg" class="easyui-dialog"
		style="width: 850px; height: 555px; padding: 10px 20px; position: relative; z-index: 1000;"
		closed="true" buttons="#dlg-buttons">
		<form id="fm" method="post">
			<table cellspacing="8px">
				<tr>
					<td>TiTle:</td>
					<td><input type="text" id="title" name="title"
						class="easyui-validatebox" required="true" />&nbsp;<font
						color="red">*</font></td>
				</tr>
				<tr>
					<td>Content:</td>
					<td id="editor"></td>
				</tr>
			</table>
		</form>
	</div>

	<div id="dlg-buttons">
		<a href="javascript:savePost()" class="easyui-linkbutton"
			iconCls="icon-ok">Save</a> <a href="javascript:closePostDialog()"
			class="easyui-linkbutton" iconCls="icon-cancel">Close</a>
	</div>
</body>
</html>
```

#### 2.5.3 deptManage.jsp

在项目 `hrms` 的 `src->main->webapp->views` 下新建 JSP 页面 deptManage.jsp 作为部门管理页面，并添加如下代码：

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/jquery-easyui-1.3.3/themes/default/easyui.css">
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/jquery-easyui-1.3.3/themes/icon.css">
<script type="text/javascript"
	src="${pageContext.request.contextPath}/jquery-easyui-1.3.3/jquery.min.js"></script>
<script type="text/javascript"
	src="${pageContext.request.contextPath}/jquery-easyui-1.3.3/jquery.easyui.min.js"></script>
<script type="text/javascript">
	var url;
	/* 根据条件查询部门 */
	function searchDept() {
		$("#dg").datagrid('load', {
			"name" : $("#s_name").val()
		});
	}
	/* 删除部门，可以是多个 */
	function deleteDept() {
		var selectedRows = $("#dg").datagrid('getSelections');
		if (selectedRows.length == 0) {
			$.messager.alert("system prompt",
					"Please choose the data to delete!");
			return;
		}
		var strIds = [];
		for ( var i = 0; i < selectedRows.length; i++) {
			strIds.push(selectedRows[i].id);
		}
		var ids = strIds.join(",");
		$.messager
				.confirm(
						"system prompt",
						"Do you want to delete the <font color=red>"
								+ selectedRows.length + "</font> data?",
						function(r) {
							if (r) {
								$
										.post(
												"${pageContext.request.contextPath}/dept/delete",
												{
													ids : ids
												},
												function(result) {
													if (result.success) {
														$.messager
																.alert(
																		"system prompt",
																		"Delete successful!");
														$("#dg").datagrid(
																"reload");
													} else {
														$.messager
																.alert(
																		"system prompt",
																		"Delete error! The department has employees!");
													}
												}, "json");
							}
						});
	}

	function openDeptAddDialog() {
		$("#dlg").dialog("open").dialog("setTitle", "Add new department");
		url = "${pageContext.request.contextPath}/dept/save";
	}
	/* 保存部门，根据不同的 url 选择是添加还是修改 */
	function saveDept() {
		$("#fm").form("submit", {
			url : url,
			onSubmit : function() {
				return $(this).form("validate");
			},
			success : function(result) {
				$.messager.alert("system prompt", "Save successful!");
				resetValue();
				$("#dlg").dialog("close");
				$("#dg").datagrid("reload");
			}
		});
	}

	function openDeptModifyDialog() {
		var selectedRows = $("#dg").datagrid('getSelections');
		if (selectedRows.length != 1) {
			$.messager.alert("system prompt", "Please choose a data to edit!");
			return;
		}
		var row = selectedRows[0];
		$("#dlg").dialog("open").dialog("setTitle",
				"Edit department information");
		$('#fm').form('load', row);
		url = "${pageContext.request.contextPath}/dept/save?id=" + row.id;
	}

	function resetValue() {
		$("#name").val("");
		$("#description").val("");
	}

	function closeDeptDialog() {
		$("#dlg").dialog("close");
		resetValue();
	}
</script>
</head>
<body style="margin: 1px;">
	<table id="dg" title="Department Manage" class="easyui-datagrid"
		fitColumns="true" pagination="true" rownumbers="true"
		url="${pageContext.request.contextPath}/dept/list" fit="true"
		toolbar="#tb">
		<thead>
			<tr>
				<th field="cb" checkbox="true" align="center"></th>
				<th field="id" width="50" align="center" hidden="true">id</th>
				<th field="name" width="80" align="center">name</th>
				<th field="description" width="200" align="center">description</th>
			</tr>
		</thead>
	</table>
	<div id="tb">
		<div>
			<a href="javascript:openDeptAddDialog()" class="easyui-linkbutton"
				iconCls="icon-add" plain="true">Add</a> <a
				href="javascript:openDeptModifyDialog()" class="easyui-linkbutton"
				iconCls="icon-edit" plain="true">Modify</a> <a
				href="javascript:deleteDept()" class="easyui-linkbutton"
				iconCls="icon-remove" plain="true">Delete</a>
		</div>
		<div>
			&nbsp;Name:&nbsp;<input type="text" id="s_name" size="20"
				onkeydown="if(event.keyCode==13) searchDept()" /> <a
				href="javascript:searchDept()" class="easyui-linkbutton"
				iconCls="icon-search" plain="true">Search</a>
		</div>
	</div>

	<div id="dlg" class="easyui-dialog"
		style="width: 450px; height: 250px; padding: 10px 20px" closed="true"
		buttons="#dlg-buttons">
		<form id="fm" method="post">
			<table cellspacing="8px">
				<tr>
					<td>Name:</td>
					<td><input type="text" id="name" name="name"
						style="width: 180px" class="easyui-validatebox" required="true" />&nbsp;<font
						color="red">*</font></td>
				</tr>
				<tr>
					<td>Description:</td>
					<td><textarea id="description" name="description"
							style="width: 180px; height: 80px;" class="easyui-validatebox"
							required="true"></textarea>&nbsp;<font color="red">*</font></td>
				</tr>
			</table>
		</form>
	</div>

	<div id="dlg-buttons">
		<a href="javascript:saveDept()" class="easyui-linkbutton"
			iconCls="icon-ok">Save</a> <a href="javascript:closeDeptDialog()"
			class="easyui-linkbutton" iconCls="icon-cancel">Close</a>
	</div>
</body>
</html>
```

#### 2.5.4 positionManage.jsp

在项目 `hrms` 的 `src->main->webapp->views` 下新建 JSP 页面 positionManage.jsp 作为职位管理页面，并添加如下代码：

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/jquery-easyui-1.3.3/themes/default/easyui.css">
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/jquery-easyui-1.3.3/themes/icon.css">
<script type="text/javascript"
	src="${pageContext.request.contextPath}/jquery-easyui-1.3.3/jquery.min.js"></script>
<script type="text/javascript"
	src="${pageContext.request.contextPath}/jquery-easyui-1.3.3/jquery.easyui.min.js"></script>
<script type="text/javascript">
	var url;
	/* 根据条件查询职位 */
	function searchPosition() {
		$("#dg").datagrid('load', {
			"name" : $("#s_name").val()
		});
	}
	/* 删除职位，可以是多个 */
	function deletePosition() {
		var selectedRows = $("#dg").datagrid('getSelections');
		if (selectedRows.length == 0) {
			$.messager.alert("system prompt",
					"Please choose the data to delete!");
			return;
		}
		var strIds = [];
		for ( var i = 0; i < selectedRows.length; i++) {
			strIds.push(selectedRows[i].id);
		}
		var ids = strIds.join(",");
		$.messager
				.confirm(
						"system prompt",
						"Do you want to delete the <font color=red>"
								+ selectedRows.length + "</font> data?",
						function(r) {
							if (r) {
								$
										.post(
												"${pageContext.request.contextPath}/position/delete",
												{
													ids : ids
												},
												function(result) {
													if (result.success) {
														$.messager
																.alert(
																		"system prompt",
																		"Delete successful!");
														$("#dg").datagrid(
																"reload");
													} else {
														$.messager
																.alert(
																		"system prompt",
																		"Delete error! The position has employees!");
													}
												}, "json");
							}
						});
	}

	function openPositionAddDialog() {
		$("#dlg").dialog("open").dialog("setTitle", "Add new position");
		url = "${pageContext.request.contextPath}/position/save";
	}
	/* 保存职位，根据不同的 url 选择是添加还是修改 */
	function savePosition() {
		$("#fm").form("submit", {
			url : url,
			onSubmit : function() {
				return $(this).form("validate");
			},
			success : function(result) {
				$.messager.alert("system prompt", "Save successful!");
				resetValue();
				$("#dlg").dialog("close");
				$("#dg").datagrid("reload");
			}
		});
	}

	function openPositionModifyDialog() {
		var selectedRows = $("#dg").datagrid('getSelections');
		if (selectedRows.length != 1) {
			$.messager.alert("system prompt", "Please choose a data to edit!");
			return;
		}
		var row = selectedRows[0];
		$("#dlg").dialog("open")
				.dialog("setTitle", "Edit position information");
		$('#fm').form('load', row);
		url = "${pageContext.request.contextPath}/position/save?id=" + row.id;
	}

	function resetValue() {
		$("#name").val("");
		$("#description").val("");
	}

	function closePositionDialog() {
		$("#dlg").dialog("close");
		resetValue();
	}
</script>
</head>
<body style="margin: 1px;">
	<table id="dg" title="Position Manage" class="easyui-datagrid"
		fitColumns="true" pagination="true" rownumbers="true"
		url="${pageContext.request.contextPath}/position/list" fit="true"
		toolbar="#tb">
		<thead>
			<tr>
				<th field="cb" checkbox="true" align="center"></th>
				<th field="id" width="50" align="center" hidden="true">id</th>
				<th field="name" width="80" align="center">name</th>
				<th field="description" width="200" align="center">description</th>
			</tr>
		</thead>
	</table>
	<div id="tb">
		<div>
			<a href="javascript:openPositionAddDialog()"
				class="easyui-linkbutton" iconCls="icon-add" plain="true">Add</a> <a
				href="javascript:openPositionModifyDialog()"
				class="easyui-linkbutton" iconCls="icon-edit" plain="true">Modify</a>
			<a href="javascript:deletePosition()" class="easyui-linkbutton"
				iconCls="icon-remove" plain="true">Delete</a>
		</div>
		<div>
			&nbsp;Name:&nbsp;<input type="text" id="s_name" size="20"
				onkeydown="if(event.keyCode==13) searchPosition()" /> <a
				href="javascript:searchPosition()" class="easyui-linkbutton"
				iconCls="icon-search" plain="true">Search</a>
		</div>
	</div>

	<div id="dlg" class="easyui-dialog"
		style="width: 450px; height: 250px; padding: 10px 20px" closed="true"
		buttons="#dlg-buttons">
		<form id="fm" method="post">
			<table cellspacing="8px">
				<tr>
					<td>Name:</td>
					<td><input type="text" id="name" name="name"
						style="width: 180px" class="easyui-validatebox" required="true" />&nbsp;<font
						color="red">*</font></td>
				</tr>
				<tr>
					<td>Description:</td>
					<td><textarea id="description" name="description"
							style="width: 180px; height: 80px;" class="easyui-validatebox"
							required="true"></textarea>&nbsp;<font color="red">*</font></td>
				</tr>
			</table>
		</form>
	</div>

	<div id="dlg-buttons">
		<a href="javascript:savePosition()" class="easyui-linkbutton"
			iconCls="icon-ok">Save</a> <a href="javascript:closePositionDialog()"
			class="easyui-linkbutton" iconCls="icon-cancel">Close</a>
	</div>
</body>
</html>
```

#### 2.5.5 employeeManage.jsp

在项目 `hrms` 的 `src->main->webapp->views` 下新建 JSP 页面 employeeManage.jsp 作为员工管理页面，并添加如下代码：

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/jquery-easyui-1.3.3/themes/default/easyui.css">
<link rel="stylesheet" type="text/css"
	href="${pageContext.request.contextPath}/jquery-easyui-1.3.3/themes/icon.css">
<script type="text/javascript"
	src="${pageContext.request.contextPath}/jquery-easyui-1.3.3/jquery.min.js"></script>
<script type="text/javascript"
	src="${pageContext.request.contextPath}/jquery-easyui-1.3.3/jquery.easyui.min.js"></script>
<script type="text/javascript"
	src="${pageContext.request.contextPath}/jquery-easyui-1.3.3/locale/easyui-lang-zh_CN.js"></script>
<script type="text/javascript">
	var url;
	var firstDepartmentId;
	var firstDepartmentName;
	var firstPositionId;
	var firstPositionName;
	var submitDeptId;
	var submitPosId;
	var updateFlag;
	/* 根据条件查询员工 */
	function searchEmpl() {
		$("#dg").datagrid('load', {
			"id" : $("#e_id").val(),
			"name" : $("#e_name").val(),
			"department.name" : $("#e_dept").val(),
			"position.name" : $("#e_position").val(),
			"sex" : $("#e_sex").val()
		});
	}
	/* 删除员工，可以是多个 */
	function deleteEmpl() {
		var selectedRows = $("#dg").datagrid('getSelections');
		if (selectedRows.length == 0) {
			$.messager.alert("system prompt",
					"Please choose the data to delete!");
			return;
		}
		var strIds = [];
		for ( var i = 0; i < selectedRows.length; i++) {
			strIds.push(selectedRows[i].id);
		}
		var ids = strIds.join(",");
		$.messager
				.confirm(
						"system prompt",
						"Do you want to delete the <font color=red>"
								+ selectedRows.length + "</font> data?",
						function(r) {
							if (r) {
								$
										.post(
												"${pageContext.request.contextPath}/empl/delete",
												{
													ids : ids
												},
												function(result) {
													if (result.success) {
														$.messager
																.alert(
																		"system prompt",
																		"Delete successful!");
														$("#dg").datagrid(
																"reload");
													} else {
														$.messager
																.alert(
																		"system prompt",
																		"Delete error!");
													}
												}, "json");
							}
						});
	}

	function openEmplAddDialog() {
		$("#dlg").dialog("open").dialog("setTitle", "Add new employee");
		$("#id").attr("readOnly", false);
		url = "${pageContext.request.contextPath}/empl/save";
		updateFlag = "no";
	}
	/* 保存员工，根据不同的 url 选择是添加还是修改 */
	function saveEmpl() {
		submitDeptId = $("#dept").combobox('getValue');
		submitPosId = $("#pos").combobox('getValue');
		$("#fm")
				.form(
						"submit",
						{
							url : url + "?dept_id= " + submitDeptId
									+ "&pos_id= " + submitPosId
									+ "&updateFlag=" + updateFlag,
							onSubmit : function() {
								var msg = "";
								// 进行输入信息的校验，非空，id 和 phone 的格式
								if (!$(this).form("validate")) {
									msg = "all information can't be empty!";
								} else if (!/^[0-9]+$/.test($("#id").val())) {
									msg = "Please input number type for id!";
								} else if (!/^1[3|5|7|8]\d{9}$/
										.test($("#phone").val())) {
									msg = "Please input right phone!";
								}
								if (msg != "") {
									$.messager.alert("system prompt", msg);
									return false;
								} else {
									return true;
								}
							},
							success : function(data) {
								var result = eval('(' + data + ')');
								if (result.success) {
									$.messager.alert("system prompt",
											"Save successful!");
									resetValue();
									$("#dlg").dialog("close");
									$("#dg").datagrid("reload");
								} else {
									$.messager
											.alert("system prompt",
													"The id already exists,please input a new id!");
								}
							}
						});
	}

	function openEmplModifyDialog() {
		var selectedRows = $("#dg").datagrid('getSelections');
		if (selectedRows.length != 1) {
			$.messager.alert("system prompt", "Please choose a data to edit!");
			return;
		}
		var row = selectedRows[0];
		$("#dlg").dialog("open")
				.dialog("setTitle", "Edit employee information");
		$("#id").attr("readOnly", true);
		$('#fm').form('load', row);
		$('#birthday').datebox('setValue', row.birthday);
		$('#dept').combobox('setValue', row.department.id);
		$('#dept').combobox('setText', row.department.name);
		$('#pos').combobox('setValue', row.position.id);
		$('#pos').combobox('setText', row.position.name);
		url = "${pageContext.request.contextPath}/empl/save";
		updateFlag = "yes";
	}

	function resetValue() {
		$("#id").val("");
		$("#id").attr("readOnly", false);
		$("#name").val("");
		$('#sex').combobox('setValue', "male");
		$("#phone").val("");
		$("#email").val("");
		$("#address").val("");
		$('#education').combobox('setValue', "Bachelor");
		$('#birthday').datebox('setValue', formatterDate(new Date()));
		$('#dept').combobox('setValue', firstDepartmentId);
		$('#dept').combobox('setText', firstDepartmentName);
		$('#pos').combobox('setValue', firstPositionId);
		$('#pos').combobox('setText', firstPositionName);
	}

	function closeEmplDialog() {
		$("#dlg").dialog("close");
		resetValue();
	}
	/* 为部门的 combobox 设默认值 */
	$(document).ready(function() {
		$.ajax({
			url : '${pageContext.request.contextPath}/dept/getcombobox',
			type : 'post',
			success : function(data) {
				if (data) {
					$('#dept').combobox('setValue', data[0].id);
					$('#dept').combobox('setText', data[0].name);
					firstDepartmentId = data[0].id;
					firstDepartmentName = data[0].name;
				}
			}
		});
	});
	/* 为职位的 combobox 设默认值 */
	$(document).ready(function() {
		$.ajax({
			url : '${pageContext.request.contextPath}/position/getcombobox',
			type : 'post',
			success : function(data) {
				if (data) {
					$('#pos').combobox('setValue', data[0].id);
					$('#pos').combobox('setText', data[0].name);
					firstPositionId = data[0].id;
					firstPositionName = data[0].name;
				}
			}
		});
	});
	
	/* 设置 datebox 默认值为指定格式的当前日期 */
	formatterDate = function(date) {
		var day = date.getDate() > 9 ? date.getDate() : "0" + date.getDate();
		var month = (date.getMonth() + 1) > 9 ? (date.getMonth() + 1) : "0"
				+ (date.getMonth() + 1);
		return date.getFullYear() + '-' + month + '-' + day;
	};

	window.onload = function() {
		$('#birthday').datebox('setValue', formatterDate(new Date()));
	};
</script>
</head>
<body style="margin: 1px;">
	<table id="dg" title="Employee Manage" class="easyui-datagrid"
		fitColumns="true" pagination="true" rownumbers="true"
		url="${pageContext.request.contextPath}/empl/list" fit="true"
		toolbar="#tb">
		<thead>
			<tr>
				<th field="cb" checkbox="true" align="center"></th>
				<th field="id" width="50" align="center">id</th>
				<th field="name" width="80" align="center">name</th>
				<th field="sex" width="50" align="center">sex</th>
				<th field="phone" width="80" align="center">phone</th>
				<th field="email" width="100" align="center">email</th>
				<th field="address" width="80" align="center">address</th>
				<th field="education" width="60" align="center">education</th>
				<th field="birthday" width="80" align="center">birthday</th>
				<th field="department.id" width="50" align="center" hidden="true">dept_id</th>
				<th field="department.name" width="80" align="center">department</th>
				<th field="position.id" width="50" align="center" hidden="true">pos_id</th>
				<th field="position.name" width="80" align="center">position</th>
			</tr>
		</thead>
	</table>
	<div id="tb">
		<div>
			<a href="javascript:openEmplAddDialog()" class="easyui-linkbutton"
				iconCls="icon-add" plain="true">Add</a> <a
				href="javascript:openEmplModifyDialog()" class="easyui-linkbutton"
				iconCls="icon-edit" plain="true">Modify</a> <a
				href="javascript:deleteEmpl()" class="easyui-linkbutton"
				iconCls="icon-remove" plain="true">Delete</a>
		</div>
		<div>
			<table>
				<tr>
					<td>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ID:&nbsp;
						<input type="text" id="e_id" size="20"
						onkeydown="if(event.keyCode==13) searchEmpl()" />
					</td>
					<td>&nbsp;Name:&nbsp; <input type="text" id="e_name" size="20"
						onkeydown="if(event.keyCode==13) searchEmpl()" />
					</td>
					<td>&nbsp;Department:&nbsp; <input type="text" id="e_dept"
						size="20" onkeydown="if(event.keyCode==13) searchEmpl()" />
					</td>
				</tr>
				<tr>
					<td>&nbsp;Position:&nbsp;<input type="text" id="e_position"
						size="20" onkeydown="if(event.keyCode==13) searchEmpl()" />
					</td>
					<td>&nbsp;&nbsp;&nbsp;&nbsp;Sex:&nbsp; <input type="text"
						id="e_sex" size="20"
						onkeydown="if(event.keyCode==13) searchEmpl()" />
					</td>
					<td>&nbsp;&nbsp;&nbsp;<a href="javascript:searchEmpl()"
						class="easyui-linkbutton" iconCls="icon-search" plain="true">Search</a>
					</td>
				</tr>
			</table>
		</div>
	</div>

	<div id="dlg" class="easyui-dialog"
		style="width: 500px; height: 550px; padding: 10px 20px" closed="true"
		buttons="#dlg-buttons">
		<form id="fm" method="post">
			<table cellspacing="8px">
				<tr>
					<td>ID:</td>
					<td><input type="text" id="id" name="id" style="width: 210px"
						class="easyui-validatebox"
						data-options="required:true,validType:'digits'" />&nbsp;<font
						color="red">*</font></td>
				</tr>
				<tr>
					<td>Name:</td>
					<td><input type="text" id="name" name="name"
						style="width: 210px" class="easyui-validatebox" required="true" />&nbsp;<font
						color="red">*</font></td>
				</tr>
				<tr>
					<td>Sex:</td>
					<td><select id="sex" class="easyui-combobox" name="sex"
						style="width: 220px; heigth: 60px"
						data-options="editable:false,panelHeight:'auto'">
							<option>male</option>
							<option>female</option>
					</select></td>
				</tr>
				<tr>
					<td>Phone:</td>
					<td><input type="text" id="phone" name="phone"
						style="width: 210px" class="easyui-validatebox" required="true" />&nbsp;<font
						color="red">*</font></td>
				</tr>
				<tr>
					<td>Email:</td>
					<td><input type="text" id="email" name="email"
						style="width: 210px" class="easyui-validatebox"
						data-options="required:true,validType:'email'" />&nbsp;<font
						color="red">*</font></td>
				</tr>
				<tr>
					<td>Address:</td>
					<td><textarea id="address" name="address"
							style="width: 210px; height: 50px" class="easyui-validatebox"
							required="true"></textarea>&nbsp;<font color="red">*</font></td>
				</tr>
				<tr>
					<td>Education:</td>
					<td><select id="education" class="easyui-combobox"
						name="education" style="width: 220px; heigth: 60px"
						data-options="editable:false,panelHeight:'auto'">
							<option>Bachelor</option>
							<option>Master</option>
							<option>Doctor</option>
					</select></td>
				</tr>
				<tr>
					<td>Birthday:</td>
					<td><input id="birthday" name="birthday" type="text"
						class="easyui-datebox" data-options="editable:false"
						required="required" style="width: 220px"></td>
				</tr>
				<tr>
					<td>Department:</td>
					<td><select class="easyui-combobox" id="dept" name="dept"
						data-options="url:'${pageContext.request.contextPath}/dept/getcombobox',  
                                           method:'post',  
                                           valueField:'id',  
                                           textField:'name', 
                                           editable:false, 
                                           panelHeight:'auto'"
						style="width: 220px"></select></td>
				</tr>
				<tr>
					<td>Position:</td>
					<td><select class="easyui-combobox" id="pos" name="pos"
						data-options="url:'${pageContext.request.contextPath}/position/getcombobox',  
                                           method:'post',  
                                           valueField:'id',  
                                           textField:'name', 
                                           editable:false, 
                                           panelHeight:'auto'"
						style="width: 220px"></select></td>
				</tr>

			</table>
		</form>
	</div>


	<div id="dlg-buttons">
		<a href="javascript:saveEmpl()" class="easyui-linkbutton"
			iconCls="icon-ok">Save</a> <a href="javascript:closeEmplDialog()"
			class="easyui-linkbutton" iconCls="icon-cancel">Close</a>
	</div>
</body>
</html>
```

```checker
- name: check adminManage exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/webapp/views/adminManage.jsp
  error: 我们发现您还没有创建adminManage.jsp! /home/project/hrms/src/main/webapp/views/adminManage.jsp
  timeout: 1
- name: check postManage exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/webapp/views/postManage.jsp
  error: 我们发现您还没有创建postManage.jsp! /home/project/hrms/src/main/webapp/views/postManage.jsp
  timeout: 1
- name: check deptManage exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/webapp/views/deptManage.jsp
  error: 我们发现您还没有创建deptManage.jsp! /home/project/hrms/src/main/webapp/views/deptManage.jsp
  timeout: 1
- name: check positionManage exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/webapp/views/positionManage.jsp
  error: 我们发现您还没有创建positionManage.jsp! /home/project/hrms/src/main/webapp/views/positionManage.jsp
  timeout: 1
- name: check employeeManage exist
  script: |
    #!/bin/bash
    ls /home/project/hrms/src/main/webapp/views/employeeManage.jsp
  error: 我们发现您还没有创建employeeManage.jsp! /home/project/hrms/src/main/webapp/views/employeeManage.jsp
  timeout: 1
```

## 三、实验总结

到这里我们就完成了表现层 JSP 代码的实现，下一节我们将完成拦截器和 Spring MVC 框架的整合。