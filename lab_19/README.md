---
sort: 19
---

# 建立日记类和最终完善

## 一、实验介绍

#### 1.1 实验内容

这是我们项目课的最后一节。这本节中，我们将完成日记类，实现将用户记录的日记写入到本地磁盘中以及日记的一些基本操作，如查、改、删。同时，我们将完成我们遗留下来的一些问题，使得整个日记软件能够顺利的运行。

#### 1.2 实验知识点

- 文件操作
- IO流操作
- Java Swing 编程

#### 1.3 实验环境

本实验环境采用带桌面的 Ubuntu Linux 环境，实验中会用到的环境或软件：

- JDK1.8
- Eclipse

#### 1.4 适合人群

本节课程难度一般，属于初级课程，适合掌握Java入门基础的用户熟练文件和IO操作以及Swing界面编程。


#### 1.5 代码获取

你可以在Xfce终端下通过下面命令将实验的完整工程项目下载到实验楼环境中，作为参照对比进行学习。

> 注：如果导入项目后遇到 build path 方面的问题，处理方法参考本课程第三节关于 JDOM 用法的介绍。

```
$ wget http://labfile.oss.aliyuncs.com/courses/260/KnowYou.tar.gz
```

## 二、项目文件结构

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475135260091.png)

## 三、实验步骤

接下来我们实现日记类并且完善整个项目。

### 3.1 日记类的实现

对于日记类的实现，需要我们编辑之前建立的 Diary.java 类文件，实现写入日记文件和读取日记文件功能。

相关的讲解将会在注释中给出，请注意阅读。

```
package com.shiyanlou.util;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

import javax.swing.text.Document;

public class Diary {

	public static void addDiary(String pathname, String title, String txt) {
		// pathname是以用户ID命名的文件夹
		File dirfile = new File(pathname);
		BufferedWriter bufw = null;
		// 建立文件夹
		dirfile.mkdirs();

		// 建立日记文件，后缀为.kz
		File file = new File(dirfile, title + ".ky");
		try {
			//写入文件
			bufw = new BufferedWriter(new FileWriter(file, true));
			bufw.write(txt);
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} finally {

			if (bufw != null) {
				try {
					bufw.close();
				} catch (IOException e) {
					// TODO Auto-generated catch block
					e.printStackTrace();
				}
			}
		}
	}

	public static void read(File file, Document doc) {

		// 创建读取流，读取文件内容，并将读到的内容添加到日记显示区
		try (BufferedReader bufr = new BufferedReader(new FileReader(file));) {
			String txt = null;
			// 获取换行符,因为Linux和Windows下的换行符是不一样的。这样可以增强跨平台性
			String line = System.getProperty("line.separator");
			while ((txt = bufr.readLine()) != null) {

				doc.insertString(doc.getLength(), txt + line, null);

			}

		} catch (Exception e1) {
			// TODO Auto-generated catch block
			e1.printStackTrace();
		}

	}

}
```

```checker
- name: check Diary java exist
  script: |
    #!/bin/bash
    ls /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/util/Diary.java
  error: 我们发现您还没有创建Diary.java! /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/util/Diary.java
  timeout: 1
```

使用 GUI 使得 IO 流的操作非常简单，大量的减少了代码的复杂度。

### 3.2 遗留问题回顾

我们在先前的实验中，在一些地方没有实现详细的逻辑代码。此处我们先来回顾一下。

#### 3.2.1 “设计 GUI 界面”实验中的遗留问题
 
RegisterGUI 注册按钮鼠标点击事件缺少实现的逻辑。

```
register.addMouseListener(new MouseAdapter() {
	public void mouseClicked(MouseEvent e) {
	//这里的事件暂且不处理，日后我们将会完善方法。
	}
});
```
LoginGUI 中的登录事件缺少实现的逻辑。

```
private void event_login() {
	//这里的登录事件方法暂不处理，日后补充。
}
```

#### 3.2.2 本节实验中实现日记类中的遗留问题

1. 添加 Diary.read() 方法读取日记
2. 添加 Diary.addDiary() 方法建立日记
 

### 3.3 完善遗留问题

#### 3.3.1 完善 RegisterGUI

首先，完成 RegisterGUI 的注册按钮鼠标点击事件。

```
    register.addMouseListener(new MouseAdapter() {
    	
    public void mouseClicked(MouseEvent e) {
    	String name = nametext.getText();//得到name
    	String ID = IDtext.getText();//得到ID
    	String passwd = passwdtext.getText();//得到密码
    	//如果检测ID返回为null
       if (Register.checkID(ID) == null) { 
       //如果检测密码返回为null
    	if (Register.checkPasswd(passwd) == null) {
    	//注册信息，并且得到返回信息
    	String srt = Register.register(name, passwd, ID);
  

      //提示框，注册成功
        	JOptionPane.showMessageDialog(contentPane,srt,"information", JOptionPane.PLAIN_MESSAGE);
    	//隐藏当前窗体
    	setVisible(false);
    	//返回首页
    	new IndexGUI().init();
    	} else {
    	//提示框，输出错误信息
    	JOptionPane.showMessageDialog(contentPane,Register.checkPasswd(passwd), "ERROR", JOptionPane.ERROR_MESSAGE);
    	}
    	} else {
    	//提示框，输出错误信息
    	JOptionPane.showMessageDialog(contentPane,Register.checkID(ID), "ERROR",
 JOptionPane.ERROR_MESSAGE);
    			}
    	}  
    	});
```

#### 3.3.2 完善 LoginGUI

接下来，完成 LoginGUI 的登录事件 `event_login()`：

```
    private void event_login()
    {
    	String id=IDtxt.getText(); 
    	String passwd=new String(passwordField.getPassword());
    	String flag=JDOM.read(id, passwd);
    	if(flag.contains("Successful landing"))
    	{
    		//拆分信息
    		String[] bufs=flag.split("/");
    		String name=bufs[1];
		    //提示框，打印登录成功
		    JOptionPane.showMessageDialog(contentPane, "Welcome: "+name,"Welcome",JOptionPane.PLAIN_MESSAGE);
        	UsersGUI.init(name);
        	setVisible(false);
        }
       else
       {
     //提示框，错误信息   			
	 JOptionPane.showMessageDialog(contentPane,flag,"ERROR",JOptionPane.ERROR_MESSAGE);
       }
 	}
```

#### 3.3.3 读取和建立日记

在 UserGUI 的 readButton 的点击事件中添加 `Diary.read()` 方法。

```
//Diary.read()方法读取日记;
Diary.read(file, doc);
```

为 UserGUI的 addButton 的点击事件中添加 `Diary.addDiary()` 方法。
					
```
//调用Diary.addDiary()方法建立日记

Diary.addDiary(pathname, title, txt);
```

至此，我们的日记软件就算完成了，具体效果如下所示。

- 登录页面

    ![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475154772384.png)

- 登录成功提示
    
    ![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475154793616.png)

- 功能选择页面

    ![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475154802339.png)

- 日记编辑页面

    ![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475154809550.png)

## 四、实验总结

至此，一个小型的日记软件就算完成了。本次课程涉及的知识点比较多，包括Java Swing 编程、文件和IO 流操作以及JDOM解析XML的应用，这些都需要同学们能够熟练的使用。当然，它还有很多不完善的地方，比如日记的加密，用户的增加、删除和修改。或者有的同学想在写日记中增加一点音乐等。这都需要同学们自己去完善日记软件了。

## 五、课后习题

同学们可以尝试去完善日记软件的功能，也可以学习一下数据库，作为日记软件的数据存储。


希望大家可以从这个项目中学的自己的东西，或则有什么改进的地方，不满意的地方都可以在 [问答](https://www.shiyanlou.com/questions/) 中提出。

