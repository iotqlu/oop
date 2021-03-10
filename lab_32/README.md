---
sort: 32
---

# 运行测试与总结

## 一、实验介绍

#### 1.1 实验内容

本节课程将对实现的系统进行运行测试，并对整个系统的开发实现过程做一个总结。

#### 1.2 实验知识点

- Maven Jetty插件的使用

#### 1.3 实验环境

- jetty 9
- JDK1.8
- WEB IDE

## 二、实验步骤

本节课程我们将实现对我们的系统运行一次，看看我们的功能能不能完成。

### 2.1 运行测试

首先我们要确保我们在pom.xml添加了jetty插件（Jetty和tomcat一样，都是Servlet容器，由于tomcat的maven插件只支持到tomcat7，所以我们选用jetty）  
![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540274479846.png)  
接着打开终端，输入：  
```
mvn jetty:run
```

看到以下的结果就说明我们启动成功，默认端口为8080  

![](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540274932908.png)  

`注意`:目前实验楼只支持8080端口启动服务，如果使用其他端口，将无法访问到服务，这里jetty默认就是8080端口，所以不用更改。

> 注：本次测试的数据是在数据库实现时插入的测试数据的基础上新增了一些，同学们可以不必理会。

#### 2.1.1 拦截器测试

启动服务后，点击`工具--web服务`按钮，直接访问主页，无法进入主页，弹出窗口提示 `Please login in first`，并回到登录页面，说明拦截器起作用。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540275089525.png)

#### 2.1.2 登录测试

在登录页面不输入或者输入错误的用户名或密码，如这里我只输入了用户名 `superadmin`，密码未输入，点击登录，弹出窗口提示 `Please check your username or password!`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540275140257.png)

输入正确的用户名和密码，如用户名：superadmin，密码：123456，点击登录，登录成功进入系统主页。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540275237833.png)

#### 2.1.3 退出测试

点击主页左侧 `System Manage` 下的 `Exit` 链接，即可退出到登录页面。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540275292711.png)


#### 2.1.4 系统管理（管理员管理）测试

**（1）查询管理员**

点击主页左侧 `System Manage` 下的 `Admin List` 链接，显示所有管理员的信息。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540275357770.png)

在 `Username` 旁的文本框中输入用户名（可以是不完整的，如 `super`），点击 `Search`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495606339322.png)

**（2）添加管理员**

点击 `Add` 链接，弹出 `Add new admin` 窗口。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495606632163.png)

输入合格的用户名和密码（均不能为空）后点击 `Save`，保存成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495606839472.png)
![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495606845707.png)

**（3）修改管理员**

不勾选任何管理员，直接点击 `Modify` 链接，弹出 `Please choose a data to edit!` 对话框。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495606947876.png)

选择 `superadmin`，点击 `Modify` 链接，弹出 `Can't modify superadmin' information` 对话框。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495607545758.png)

选择一个管理员，如刚添加的 `admin4`，点击 `Modify` 链接，弹出 `Edit admin information` 窗口。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495607075395.png)

将用户名 `admin4` 修改为 `admin5`，点击 `Save`，保存成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495607235390.png)

**（4）删除管理员**

不勾选任何管理员，直接点击 `Delete` 链接，弹出 `Please choose the data to delete!` 对话框。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495607319157.png)

选中一条或多条数据点击 `Delete` 链接，提示是否删除，点击 `OK`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495607380958.png)

如果这条数据是 `superadmin`，或当前登录的管理员，弹出 `Can't delete superAdmin or current admin！` 对话框。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495607738929.png)


如果是其他数据则删除成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495607801587.png)

#### 2.1.5 公告管理测试

**（1）查询公告**

点击主页左侧 `Post Manage` 下的 `Post Info` 链接，显示所有公告的信息。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495607940093.png)

在 `Title` 旁的文本框中输入公告标题（可以是不完整的，如 `notice`），点击 `Search`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495608556677.png)

选择一个公告，点击 `Show Content` 链接，跳转至新页面，显示公告的内容详情，如在这里选择 `Leave notice`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid441493labid8432timestamp1540275574558.png)

**（2）添加公告**

点击 `Add` 链接，弹出 `Add post` 窗口。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495608873943.png)

`Title` 和 `Content` 均不能为空，输入内容后，点击 `Save`，保存成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495609004622.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495609012333.png)

**（3）修改公告**

选择一条公告，如刚添加的 `shiyanloupost`，点击 `Modify` 链接，弹出 `Edit post` 窗口。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495609196538.png)

将公告标题 `shiyanloupost` 修改为 `newshiyanloupost`，点击 `Save`，保存成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495609283668.png)

**（4）删除公告**

选中一条或多条数据点击 `Delete` 链接，提示是否删除，点击 `OK`，删除成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495609434804.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495609435075.png)

#### 2.1.6 部门管理测试

**（1）查询部门**

点击主页左侧 `Department Manage` 下的 `Department Info` 链接，显示所有部门的信息。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495609614207.png)

在 `Name` 旁的文本框中输入部门名称（可以是不完整的，如 `deve`），点击 `Search`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495609732417.png)

**（2）添加部门**

点击 `Add` 链接，弹出 `Add new department` 窗口。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495609941416.png)


部门名称 `Name` 和描述 `Description` 均不能为空，输入内容后，点击 `Save`，保存成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495610072105.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495610072327.png)

**（3）修改部门**

选择一个部门，如刚添加的 `human resources`，点击 `Modify` 链接，弹出 `Edit department information` 窗口。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495610244834.png)

将部门描述修改为 `human resources,shiyanlou!`，点击 `Save`，保存成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495610245137.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495610245339.png)

**（4）删除部门**

选中一条或多条数据点击 `Delete` 链接，提示是否删除，点击 `OK`。

如果该部门有员工，删除失败。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495614834591.png)

否则，删除成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495610698927.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495610700402.png)

#### 2.1.7 职位管理测试

**（1）查询职位**

点击主页左侧 `Position Manage` 下的 `Position Info` 链接，显示所有职位的信息。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495610889708.png)

在 `Name` 旁的文本框中输入职位名称（可以是不完整的，如 `Java`），点击 `Search`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495610992700.png)

**（2）添加职位**

点击 `Add` 链接，弹出 `Add new position` 窗口。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495611040783.png)


职位名称 `Name` 和描述 `Description` 均不能为空，输入内容后，点击 `Save`，保存成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495611138606.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495611138859.png)

**（3）修改职位**

选择一个职位，如刚添加的 `python engineers`，点击 `Modify` 链接，弹出 `Edit position information` 窗口。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495611298496.png)

将职位描述修改为 ``python engineers for shiyanlou!`，点击 `Save`，保存成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495611269891.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495611270161.png)

**（4）删除职位**

选中一条或多条数据点击 `Delete` 链接，提示是否删除，点击 `OK`。

如果该职位有员工，删除失败。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495615008529.png)

否则，删除成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495611382273.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495611382592.png)


#### 2.1.8 员工管理测试

**（1）查询员工**

点击主页左侧 `Employee Manage` 下的 `Employee Info` 链接，显示所有员工的信息。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495612510208.png)

根据 id 查询员工，如输入 `001`，点击 `Search`

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495612510487.png)

根据 id 和 部门查询员工，如分别输入 `001` 和 `course`，点击 `Search`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495611646628.png)

其他条件查询类似，这里不再演示。

**（2）添加员工**

点击 `Add` 链接，弹出 `Add new employee` 窗口。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495611940289.png)

填写信息提交，会进行包括如下校验：

空校验：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495613513447.png)

id 必须为数字：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495613529673.png)

正确的手机号格式：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495613529944.png)

重复 id 校验：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495613530200.png)

输入合格的信息后，点击 `Save`，保存成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495613530516.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495614119724.png)

**（3）修改员工**

选择一个员工，如刚添加的 id 为 `10003` 的员工，点击 `Modify` 链接，弹出 `Edit employee information` 窗口。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495614499767.png)

将该员工的地址修改为 `chongqing`，点击 `Save`，保存成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495614571408.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495614571810.png)

（**4）删除员工**

选中一条或多条数据点击 `Delete` 链接，提示是否删除，点击 `OK`，删除成功。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495614757793.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2989timestamp1495614759035.png)

### 2.2 系统开发总结

本项目中的人事管理系统，采用了 SSM 框架+ easyUI 技术实现。在项目的开发过程中，首先是对系统进行了分析，在这里只做了功能性分析，然后是进行设计，如系统设计、数据库设计、界面设计等，其次才是编码实现，最后是测试。系统的整个开发工程中，用到了多种技术，包括轻量级 Java EE 开发框架 SSM，前端框架 easyUI 等。本项目的系统只是比较简易的人事管理系统，功能也比较简单，还有一些细节并未完善，如文件的上传下载、分页、页面的优化等方面。

本次课程的目的主要在于让同学们亲自动手采用 SSM 框架实现简单的开发，相信通过本课程，同学们的编码能力能得到提高。


在这里再次感谢本课程参考资料的作者们如 `ZHENFENG13`，感谢他们提供的优质学习资源。

## 三、实验总结

到这里我们的人事管理系统就开发完成，希望同学们能在下来的时间巩固整个开发过程，并尝试完善该系统的功能。 
