---
sort: 18
---

# 实现用户的注册和登录

## 一、实验介绍

#### 1.1 实验内容

在前面的实验中，我们完成了整个软件的 GUI 界面。接下的代码就是填充程序的功能，使它不再是一个空架子。而且接下来才是我们学习的重点，但也不难。最后，我们将通过 JDOM 工具包来解析之前定义的 UserInfo.xml 文档，以实现用户的注册和登录功能模块。同时，还需要在 util 包建立一个工具类，限制和检查用户输入的有效性。

#### 1.2 实验知识点

- 用户类的设计
- 使用 JDOM 工具包解析 XML 文件
- 正则表达式的应用

#### 1.3 实验环境

本实验环境采用带桌面的 Ubuntu Linux 环境，实验中会用到的环境或软件：

- JDK1.8
- Eclipse

#### 1.4 适合人群

本节课程难度一般，涉及了类的设计、JDOM解析XML文件以及正则表达的应用，适合具有Java Swing编程基础但未学习过数据库知识却想要存储简单数据的同学学习。


#### 1.5 代码获取

你可以在Xfce终端下通过下面命令将实验的完整工程项目下载到实验楼环境中，作为参照对比进行学习。

> 注：如果导入项目后遇到 build path 方面的问题，处理方法参考本课程第三节关于 JDOM 用法的介绍。

```
$ wget http://labfile.oss.aliyuncs.com/courses/260/KnowYou.tar.gz
```

## 二、项目文件结构

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475135260091.png)

## 三、实验步骤

这一节中我们将实现用户的注册和登录。

### 3.1 完善用户类

首先要先分析用户所有的属性，因为这是一个简单的日记软件，只需要用户的姓名，登录 ID 以及密码即可。

所需属性：

1. name
2. ID
3. passwd

需要编辑的文件为 `com.shiyanlou.entity` 包里面的 `User.java` 文件，通过类的方法设置和获取用户的属性。

具体的代码如下：

```java
package com.shiyanlou.entity;

// 用户名、ID、密码
public class User {

	private String ID;  //登录ID
	private String name; //姓名
	private String passwd;//密码
	
	
	public String getName() {
		return name;
	}
	public void setID(String iD) {
		ID = iD;
	}
	public String getID() {
		return ID;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getPasswd() {
		return passwd;
	}
	public void setPasswd(String passwd) {
		this.passwd = passwd;
	}
	
}
```  
  
是不是非常简单呢？没错，这样用户类就完成了。

```checker
- name: check User java exist
  script: |
    #!/bin/bash
    ls /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/entity/User.java
  error: 我们发现您还没有创建User.java! /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/entity/User.java
  timeout: 1
```

### 3.2 XML 文档

接下来，需要对应着 User 类来设计一个 XML 文档，用于持久化存储用户信息。

#### 3.2.1 什么是 XML

 - XML 指可扩展标记语言（EXtensible Markup Language）
 - XML 是一种标记语言，很类似 HTML
 - XML 的设计宗旨是传输数据，而非显示数据
 - XML 标签没有被预定义。您需要自行定义标签。
 - XML 被设计为具有自我描述性。
 - XML 是 W3C 的推荐标准

#### 3.2.2 创建 XML 文件

在桌面空白处右键点击，选择`从模版创建`一个`空文件`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid953timestamp1475138333843.png)

文件名可以填写为 `UserInfo.xml`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid953timestamp1475138458084.png)

创建完成后可以在桌面上找到这个 UserInfo.xml 文件。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid953timestamp1475138495493.png)

右键点击该文件，用 gedit 编辑器打开它。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid953timestamp1475138541283.png)

#### 3.2.3 编辑 XML 文件

在编辑器中输入 XML 文件的信息和项目中会用到的标签 `Users`。

```
<?xml version="1.0" encoding="UTF-8"?>
<Users></Users>
```

编辑完成后，点击保存按钮保存该文件，然后关闭编辑器。
`UserInfo.xml` 如图：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid953timestamp1475138672451.png)

以上就完成了我们保存用户信息的 xml 文档。


### 3.3 JDOM 简介

JDOM 是一种使用 XML（标准通用标记语言下的一个子集） 的独特 Java 工具包。它的设计包含 Java 语言的语法乃至语义。 

#### 3.3.1 JDOM 的用法

要使用 JDOM 解析 XML文件，需要下载 JDOM 的包，实验中使用的是 jdom-1.1。解压之后，将 lib 文件夹下的 `*.jar` 文件以及 build 文件夹下的 jdom.jar 拷贝到工程文件夹下，然后就可以使用 JDOM 操作 XML 文件了。

- [jdom下载地址](http://labfile.oss.aliyuncs.com/courses/480/jdom-2.0.6.zip)
- [jdom解析xml文档](http://wuhongyu.iteye.com/blog/361842)

在实验环境中下载 jdom 可以使用下面的方式，打开Xfce终端，输入命令：

```
$ wget http://labfile.oss.aliyuncs.com/courses/480/jdom-2.0.6.zip
$ unzip jdom-2.0.6.zip
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid955timestamp1475138859534.png)

#### 3.3.2 JDOM 的具体实现

首先需要将 JDOM 的 jar 包导入到我们的工程中。

右键点击项目目录，选择 `Properties` 进入项目属性设置。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid955timestamp1475139047551.png)

在 `Java Build Path` 设置项里切换到 `Libraries` 选项卡，然后点击右侧的 `Add External JARs...` 按钮。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid955timestamp1475139125970.png)

在弹出的 `JAR Selection` 对话框中选择 Shiyanlou 目录下刚解压的 JDOM 相关包 `jdom-2.0.6.jar`，然后点击 `确定` 按钮完成添加。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid955timestamp1475142261806.png)

最后在属性页点击 OK 完成设置。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid955timestamp1475142287108.png)

接下来就需要对 JDOM 类进行编辑。

在类 `JDOM.java`中，主要包含了两个方法，`write()` 和 `read()` 方法，分别用于将用户信息写入到 xml 文档中和读出用户信息。

```java
package com.shiyanlou.util;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;
import java.util.Map;
import java.util.TreeMap;
import org.jdom2.Attribute;
import org.jdom2.Document;
import org.jdom2.Element;
import org.jdom2.JDOMException;
import org.jdom2.input.SAXBuilder;
import org.jdom2.output.XMLOutputter;

public class JDOM {
	
    //注册用户信息
	public static String write(String n, String p, String id) {
		// TODO Auto-generated method stub
		
		// UserInfo.xml文档的路径
		// 注意此处的路径要跟你的项目的实际情况来修改！
		String path = "/home/shiyanlou/Desktop/UserInfo.xml";
		//将xml文档封装成file
		File file = new File(path);
		//使用默认的sax解析器
		SAXBuilder saxBuilder = new SAXBuilder();
		Document doc; //声明document文档
		try {
			doc = saxBuilder.build(file);
			
			//元素对应到xml文档中就是标签
			Element root = doc.getRootElement(); //得到根元素
			Element user = new Element("User"); //建立User元素
			Element name = new Element("name");//建立name元素
			Element passwd = new Element("passwd");//建立passwd元素
			
			/*首先检测xml文档中是否已经存在了ID号相同的用户，如果不存在才可以继续注册*/
			if (checkID(id, root)) {
				//将ID设置为user的属性
				user.setAttribute(new Attribute("id", id)); 
				//设置姓名和密码
				name.setText(n);
				passwd.setText(p);
				
				//将name，passwd元素添加到user元素下
				user.addContent(name);
				user.addContent(passwd);
				
				//将user元素添加到根元素下
				root.addContent(user);
				
				//输出xml文档
				XMLOutputter out = new XMLOutputter();
				out.output(doc, new FileOutputStream(file));
				return "Successful registration";//返回注册成功
			} else
				//返回ID存在信息，重新输入ID
				return "ID already exists, please input again";

		} catch (JDOMException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return "ERROR";//未知错误

	}

	public static boolean checkID(String id, Element root) {
		// 检测ID是否存在
		boolean flag = true;
		@SuppressWarnings("unchecked")
		//得到User标签的所有子元素，并加入到map集合中
		List<Element> list = root.getChildren("User");
		//迭代检测是否存在ID
		Iterator<Element> it = list.iterator();
		while (it.hasNext()) {
			Element e = (Element) it.next();
			if (e.getAttributeValue("id").equals(id)) {
				flag = false;
			}
		}
		return flag;

	}

	//读取xml文档用于登录
	public static String read(String id, String passwd) {
	
		// UserInfo.xml文档的路径
		// 注意此处的路径要跟你的项目的实际情况来修改！
		String path = "/home/shiyanlou/Desktop/UserInfo.xml";
		File file = new File(path);
		SAXBuilder saxBuilder = new SAXBuilder();

		try {
			Document doc = saxBuilder.build(file);
			Element root = doc.getRootElement();
			
			//取出用户密码和姓名
			String info = getPasswd(root).get(id);
			if (info == null) {
				return "User does not exist!!";
			}
			String[] buf = info.split("/");

			if (buf[0].equals(passwd)) {
				return "Successful landing/" + buf[1];
			}

		} catch (JDOMException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return "Wrong password!!";
	}

	@SuppressWarnings("unchecked")
	/*将用户的密码和姓名添加到map集合中*/
private static Map<String, String> getPasswd(Element root) {
		Map<String, String> map = new TreeMap<String, String>();//存贮用户信息
		List<Element> list = new ArrayList<Element>();
		//得到User标签的所有子元素信息
		list = root.getChildren("User");
		Iterator<Element> it = list.iterator();
		while (it.hasNext()) {
			Element e = it.next();
			String id = e.getAttributeValue("id");
			String passwd = e.getChildText("passwd");
			String name = e.getChildText("name");
			map.put(id, getInfo(passwd, id));
		}

		return map;

	}

	//处理用户密码和信息
	private static String getInfo(String passwd, String name) {

		return passwd + "/" + name;

	}
}
```

以上，我们就完成了 JDOM 写入和读取用户信息。这样注册用户就可以持久化保存了。

```checker
- name: check JDOM java exist
  script: |
    #!/bin/bash
    ls /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/util/JDOM.java
  error: 我们发现您还没有创建JDOM.java! /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/util/JDOM.java
  timeout: 1
```

### 3.4 实现用户注册输入信息的检查 

正则表达式验证的规则：

 - 用户ID：1~8 位数字组成——`\\d{1,8}`
 - 密码：6~15 位数字组成——`\\d{6,15}`

为了实现注册功能，我们建立了 Register.java ，此处需要对它进行编辑。

相关的代码如下，具体的讲解将以注释的形式给出。

```java
package com.shiyanlou.util;

import com.shiyanlou.entity.User;

public class Register {

	static User user = new User();

	public static String checkName(String name) {
		user.setName(name);
		return null;

	}

	public static String checkID(String ID) {
		if (ID.matches("\\d{1,8}")) {
			user.setID(ID);
			return null;
		} else
			return "ID not conform to the rules";
	}

	public static String checkPasswd(String passwd) {
		if (passwd.matches("\\d{6,15}")) {
			user.setPasswd(passwd);
			return null;
		} else
			return "Password not conform to the rules";
	}

	//如果以上验证都没有错误信息返回，则执行写入用户信息
	public static String register(String name,String passwd,String ID) {
		user.setName(name);
		user.setPasswd(passwd);
		user.setID(ID);
		return (JDOM.write(user.getName(), user.getPasswd(),
				user.getID()));

	}
}
```

这样，我们就完成了用户信息的验证。

```checker
- name: check Register java exist
  script: |
    #!/bin/bash
    ls /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/util/Register.java
  error: 我们发现您还没有创建Register.java! /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/util/Register.java
  timeout: 1
```

## 四、实验总结

在本节实验中，我们实现了用户的注册和登录功能，并应对了 “坏用户” 来挑战程序的容错性，比如在注册时输入一些空字符等等。下一节我们将完成日记类的新建和阅读。

