---
sort: 17
---

# 设计 GUI 界面

## 一、实验介绍

#### 1.1 实验内容

本节将在上一节的基础上，继续完成下列目标：

- 实现首页 GUI 界面
- 实现登录 GUI 界面
- 实现注册 GUI 界面
- 实现用户 GUI 界面

特别是对于用户 GUI 界面部分，我们采用的是内部窗体设计。该方法能使程序看起来更加漂亮，增强我们这款日记软件的可用性。

#### 1.2 知识点

- WIndowBuilder的使用
- Java Swing 界面设计

#### 1.3 实验环境

本实验环境采用带桌面的 Ubuntu Linux 环境，实验中会用到的环境或软件：

- JDK1.8
- Eclipse

#### 1.4 适合人群

本节课程难度较低，属于初级课程，涉及了 Java GUI 界面编程，包括WindowBuilder插件的使用，适合学习 Java 可视化编程的同学。

#### 1.5 代码获取

你可以在Xfce终端下通过下面命令将实验的完整工程项目下载到实验楼环境中，作为参照对比进行学习。

> 注：如果导入项目后遇到 build path 方面的问题，处理方法参考本课程第三节关于 JDOM 用法的介绍。

```
$ wget http://labfile.oss.aliyuncs.com/courses/260/KnowYou.tar.gz
```

## 二、项目文件结构

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475135260091.png)

## 三、实验步骤

这一节中我们将设计gui界面。

### 3.1 Java GUI 简介

Java GUI 界面的设计是很多 Java 学习者的痛苦，因为 GUI 界面代码较为复杂。但是 Eclipse 中的 **WindowBuilder** 插件可以很好地解决这个问题，能够实现可视化编程。

关于 WindowBuilder的相关参考资料已经列出，建议在学习本节内容之前先作简单了解。

- [WindowBuilder教程](http://www.fengfly.com/plus/view-191057-1.html)
- [WindowBuilder介绍](http://projects.eclipse.org/projects/tools.windowbuilder)

在本节中，我们除了使用常用的 Swing 组件之外，还会有一些其他的组件来完善增加软件的功能，主要有：

1. JFileChooser：用于弹出文件选择对话框，进行文件或目录的选择。
2. FileNameExtensionFilter：文件扩展名过滤器。
3. JInternalFrame：Swing 内部窗体组件。

### 3.2 首页 IndexGUI.java

完成效果如下图所示。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid872timestamp1475142745950.png)

相关的代码如下，代码的讲解将会以注释的形式进行，请在编写代码的同时留意相关的注释。

```
package com.shiyanlou.view;

import java.awt.EventQueue;
import java.awt.Font;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JPanel;
import javax.swing.border.EmptyBorder;

public class IndexGUI extends JFrame {
    //自定义IndexGUI继承JFrame类
	private JPanel contentPane;  //声明面板
	//创建JFrame的类对象声明
    private static IndexGUI frame;
    
	public static void main(String[] args) {
		init();
	}
	public static void init()  //初始化方法
	{
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					frame = new IndexGUI(); //实例化frame
					frame.setVisible(true); //设置窗体可见性
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
	}
	public IndexGUI() {
		setTitle("KnowYou");  //设置窗体标题
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); //设置默认关闭方式，点击窗体关闭按钮可关闭窗体
		setBounds(100, 100, 650, 400);
		contentPane = new JPanel(); //实例化面板
		contentPane.setBorder(new EmptyBorder(5, 5, 5, 5)); //设置面板大小，位置
		setContentPane(contentPane); //frame添加面板
		contentPane.setLayout(null);  //面板设置布局为null,不可省略。否则页面布局将会杂乱。
		
		JLabel lblNewLabel = new JLabel("Welcome to use KnowYou"); //标题
		lblNewLabel.setBounds(132, 74, 386, 35);
		lblNewLabel.setFont(new Font("Ubuntu", Font.BOLD | Font.ITALIC, 30));
		contentPane.add(lblNewLabel);
		
		JButton login = new JButton("Login"); //登录按钮
		//登录按钮鼠标事件，当鼠标被点击时调用
		login.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseClicked(MouseEvent e) {
				event_Login(); //登录事件方法
			}
		});
		
		
		//增加键盘事件
		login.addKeyListener(new KeyAdapter() {
			@Override
			public void keyPressed(KeyEvent e) {
				if(e.getKeyCode()==KeyEvent.VK_ENTER)
				{
					event_Login();//登录事件方法
				}
			}
		});
		login.setBounds(65, 263, 124, 45);
		contentPane.add(login);
		
		JButton register = new JButton("Sign Up"); //注册按钮
		
		//注册鼠标事件
		register.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseClicked(MouseEvent e) {
				event_register(); //注册事件方法
			}
		});
		
		//注册按钮键盘事件
		register.addKeyListener(new KeyAdapter() {
			@Override
			public void keyPressed(KeyEvent e) {
				if(e.getKeyCode()==KeyEvent.VK_ENTER)
				{
					event_register();//注册事件方法
				}
			}
		});
		register.setBounds(489, 263, 109, 45);
		contentPane.add(register);
		
	}
	
	//对登录和注册事件进行私有方法封装
	private void event_Login()
	{
		setVisible(false);
		new LoginGUI().loginGUI();
	}
	
	private void event_register()
	{
    	setVisible(false);
		new RegisterGUI().registerGUI();
	}
}
```

```checker
- name: check IndexGUI java exist
  script: |
    #!/bin/bash
    ls /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/view/IndexGUI.java
  error: 我们发现您还没有创建IndexGUI.java! /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/view/IndexGUI.java
  timeout: 1
```

### 3.3 注册 RegisterGUI.java

完成效果如下图所示。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid872timestamp1475142904621.png)

相关的代码如下，代码的讲解将会以注释的形式进行，请在编写代码的同时留意相关的注释。

```java
package com.shiyanlou.view;

import java.awt.EventQueue;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JTextField;
import javax.swing.border.EmptyBorder;

import com.shiyanlou.util.Register;

public class RegisterGUI extends JFrame {

	private static final long serialVersionUID = 3250371445038102261L;
	private JPanel contentPane;
	private JTextField nametext;  //name输入框
	private JTextField IDtext;  //ID输入框
	private JTextField passwdtext;  //密码输入框

	/**
	 * Launch the application.
	 */
	public void registerGUI() {
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					RegisterGUI frame = new RegisterGUI();
					frame.setVisible(true);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
	}

	/**
	 * Create the frame.
	 */
	public RegisterGUI() {
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setBounds(100, 100, 650, 400);
		contentPane = new JPanel();
		contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));
		setContentPane(contentPane);
		contentPane.setLayout(null);

		JLabel namelabel = new JLabel("Please input user name"); //设置提示姓名输入标签
		namelabel.setBounds(102, 91, 151, 23);
		contentPane.add(namelabel);

		JLabel IDlabel = new JLabel("Please input user ID");//设置提示ID输入标签
		IDlabel.setBounds(102, 160, 151, 23);
		contentPane.add(IDlabel);

		
		JLabel passwdlaber = new JLabel("Please input user password");//设置提示密码输入标签
		passwdlaber.setBounds(102, 224, 163, 23);
		contentPane.add(passwdlaber);

		nametext = new JTextField();  //普通文本输入框
		nametext.setBounds(271, 92, 92, 21); //设置位置及大小
		contentPane.add(nametext);
		nametext.setColumns(10);  //设置长度
		 
		//ID
		IDtext = new JTextField();
		IDtext.setBounds(271, 161, 92, 21);
		contentPane.add(IDtext);
		IDtext.setColumns(8);

		//密码
		passwdtext = new JTextField();
		passwdtext.setBounds(271, 225, 92, 21);
		contentPane.add(passwdtext);
		passwdtext.setColumns(10);

		//注册按钮
		JButton register = new JButton("Sign Up"); 
		
		//注册按钮鼠标点击事件
		register.addMouseListener(new MouseAdapter() {
			public void mouseClicked(MouseEvent e) {
				//这里的事件暂且不处理，日后我们将会完善方法。
			}
		});
		
		register.setBounds(321, 305, 93, 23);
		contentPane.add(register);

		JButton back = new JButton("BACK");  //返回按钮
		back.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseClicked(MouseEvent e) {
				IndexGUI.init(); //创建首页
				setVisible(false); //当前页面不可见
			}
		});
		back.setBounds(531, 305, 93, 23);
		contentPane.add(back);

		JLabel label = new JLabel("Welcome to use KnowYou"); //欢迎标题
		label.setFont(new Font("Ubuntu", Font.BOLD | Font.ITALIC, 30));
		label.setBounds(143, 26, 374, 35);
		contentPane.add(label);

		JLabel lblNewLabel = new JLabel("(There are 1 to 8 numbers)");
		lblNewLabel.setBounds(373, 164, 163, 15);
		contentPane.add(lblNewLabel);

		JLabel lblNewLabel_1 = new JLabel("(There are 6 to 15 numbers)");
		lblNewLabel_1.setBounds(373, 228, 163, 15);
		contentPane.add(lblNewLabel_1);
	}
}
```

```checker
- name: check RegisterGUI java exist
  script: |
    #!/bin/bash
    ls /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/view/RegisterGUI.java
  error: 我们发现您还没有创建RegisterGUI.java! /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/view/RegisterGUI.java
  timeout: 1
```

### 3.4 登录 Login.java

完成效果如下图所示。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid872timestamp1475152920425.png)

相关的代码如下，代码的讲解将会以注释的形式进行，请在编写代码的同时留意相关的注释。
```
package com.shiyanlou.view;

import java.awt.EventQueue;
import java.awt.Font;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JPasswordField;
import javax.swing.JTextField;
import javax.swing.border.EmptyBorder;

import com.shiyanlou.util.JDOM;


public class LoginGUI extends JFrame {
	private static final long serialVersionUID = 4994949944841194839L;
	private JPanel contentPane;  //面板
	private JTextField IDtxt; //ID输入框
	private JLabel Passwdlabel;//密码标签
	private JPasswordField passwordField;//密码输入框
	private JButton login;//登录按钮
	private JButton back;//返回按钮

	/**
	 * Launch the application.
	 * @return 
	 */
	public void loginGUI() {
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					LoginGUI frame = new LoginGUI();
					frame.setVisible(true);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
	}

	/**
	 * Create the frame.
	 */
	public LoginGUI() {
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setBounds(100, 100, 650, 400);
		contentPane = new JPanel();
		contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));
		setContentPane(contentPane);
		contentPane.setLayout(null);
		
		JLabel IDlabel = new JLabel("Please input ID");//ID标签
		IDlabel.setBounds(68, 170, 100, 39);
		contentPane.add(IDlabel);
		
		IDtxt = new JTextField();
		IDtxt.setBounds(220, 179, 126, 21);
		contentPane.add(IDtxt);
		IDtxt.setColumns(10);
		
		Passwdlabel = new JLabel("Please input password");
		Passwdlabel.setBounds(68, 219, 150, 50);
		contentPane.add(Passwdlabel);
		
		passwordField = new JPasswordField();
		passwordField.setBounds(220, 234, 126, 21);
		contentPane.add(passwordField);
		
		login = new JButton("login");
        
		//鼠标事件
		login.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseClicked(MouseEvent e) {
				
				event_login();//登录事件方法
			}
		});
		
		//键盘事件
		login.addKeyListener(new KeyAdapter() {
			public void keyPressed(KeyEvent e)
			{
				if(e.getKeyCode()==KeyEvent.VK_ENTER)//当键盘按下enter时调用
				{
					event_login();//登录事件方法
				}
			}
		});
		login.setBounds(239, 310, 93, 23);
		contentPane.add(login);
		
		//返回按钮
		back = new JButton("BACK");
		back.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseClicked(MouseEvent e) {
				 IndexGUI.init();
				 setVisible(false);
			}
		});
		back.setBounds(507, 310, 93, 23);
		contentPane.add(back);
		
		//标题
		JLabel label = new JLabel("Welcome to use KnowYou");
		label.setFont(new Font("Ubuntu", Font.BOLD | Font.ITALIC, 30));
		label.setBounds(142, 54, 386, 35);
		contentPane.add(label);
	}
	
	//封装登录事件
	private void event_login()
	{
		//这里的登录事件方法暂不处理，日后补充。
	}
}
```

```checker
- name: check Login java exist
  script: |
    #!/bin/bash
    ls /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/view/Login.java
  error: 我们发现您还没有创建Login.java! /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/view/Login.java
  timeout: 1
```

### 3.5 用户界面 UsersGUI.java

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid872timestamp1475152962134.png)

点击阅读按钮：

![图片描述信息](https://doc.shiyanlou.com/userid55977labid951time1430306044372)

点击删除按钮：

![图片描述信息](https://doc.shiyanlou.com/userid55977labid951time1430306113097)

点击新建按钮：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid872timestamp1475153038826.png)

```
package com.shiyanlou.view;

import java.awt.Color;
import java.awt.EventQueue;
import java.awt.Font;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.io.File;
import javax.swing.JButton;
import javax.swing.JEditorPane;
import javax.swing.JFileChooser;
import javax.swing.JFrame;
import javax.swing.JInternalFrame;
import javax.swing.JLabel;
import javax.swing.JOptionPane;
import javax.swing.JPanel;
import javax.swing.JTabbedPane;
import javax.swing.JTextField;
import javax.swing.JTextPane;
import javax.swing.border.EmptyBorder;
import javax.swing.filechooser.FileNameExtensionFilter;

import com.shiyanlou.util.Diary;



public class UsersGUI extends JFrame {
	private JPanel contentPane;
	private JTextField textField;
	
	//文件选择组建，用于用户阅读日记和删除日记时选择文件。
	private JFileChooser chooser;
	
	/*每个注册用户所记录的日记都位于自己的文件夹下，pathname用于保存用户的文件夹路径*/
	private static String pathname; 

	public static void init(String path) { //初始化方法
		pathname = path;
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					UsersGUI frame = new UsersGUI();
					frame.setVisible(true);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
	}

	/**
	 * Create the frame.
	 */
	public UsersGUI() {
		setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		setBounds(100, 100, 600, 400);
		contentPane = new JPanel();
		contentPane.setBorder(new EmptyBorder(5, 5, 5, 5));
		setContentPane(contentPane);
		contentPane.setLayout(null);

		JTabbedPane tabbedPane = new JTabbedPane(JTabbedPane.TOP);
		tabbedPane.setToolTipText("KonwYou");
		tabbedPane.setBounds(0, 0, 574, 67);
		contentPane.add(tabbedPane);

		final JPanel panel = new JPanel();
		tabbedPane.addTab("Management Journal", null, panel, null);

        chooser = new JFileChooser(".\\"+pathname);//初始化JFileChooser，并设置默认目录为用户目录
        FileNameExtensionFilter filter=new FileNameExtensionFilter("Allowed","ky");//文件选择器，只允许选择.ky文件
        chooser.setFileFilter(filter);//为文件设置选择器

        JButton readButton = new JButton("Read the diary");
        readButton.addMouseListener(new MouseAdapter() {
			@Override
            //阅读按钮鼠标事件，当用户点击时，将会创建一个新的内部窗体
        	public void mouseClicked(MouseEvent e) {
				
	        	int value = chooser.showOpenDialog(panel);//判断用户是否选择了文件
				
            	//内部窗体
                JInternalFrame internalFrame_Read = new JInternalFrame("Read the diary", false, true, false, false);
                internalFrame_Read.setBounds(0, 77, 584, 275);
                contentPane.add(internalFrame_Read);
                internalFrame_Read.getContentPane().setLayout(null);
                JTextPane txtDiary = new JTextPane();
                txtDiary.setBounds(0, 0, 568, 246);
                internalFrame_Read.getContentPane().add(txtDiary);
				
                //JTextPane没有append方法，所以使用Document来不断插入文本
                javax.swing.text.Document doc=txtDiary.getDocument();
                txtDiary.setBackground(Color.GREEN);//背景颜色为绿色
                txtDiary.setEditable(false);//设置为不可编辑

                //当value的值和JFileChooser.APPROVE_OPTION相等时，证明用户选择了文件
                if (value == JFileChooser.APPROVE_OPTION) {
    					
	                //得到用户选择的文件
	                File file = chooser.getSelectedFile();
		               
		            //如果文件存在
		            if(file.exists()) {	
			        
    				    //Diary.read()方法读取日记;
    				    //该方法将会在以后的课程中完成
    					
    			        internalFrame_Read.setVisible(true);
				    }
			    }
		    }
	    });
	    
		panel.add(readButton);

    	JButton addButton = new JButton("Create a diary");//新建按钮
        addButton.addMouseListener(new MouseAdapter() {
			@Override
            public void mouseClicked(MouseEvent e) {
				
				//创建新建日记内部窗体
                final JInternalFrame internalFrame_Write = new JInternalFrame("Create a diary",false, true, false, false);


                internalFrame_Write.setBounds(0, 77, 584, 275);   
            	contentPane.add(internalFrame_Write);
               	internalFrame_Write.getContentPane().setLayout(null);

                textField = new JTextField();
                textField.setBounds(76, 0, 492, 21);
                internalFrame_Write.getContentPane().add(textField);
                textField.setColumns(10);

                JLabel label = new JLabel("Title");

                label.setFont(new Font("楷体", Font.PLAIN, 12));
   	            label.setBounds(46, 3, 52, 15);
   	            internalFrame_Write.getContentPane().add(label);

				//日记编辑区
                final JEditorPane editorPane = new JEditorPane();
                editorPane.setBounds(0, 31, 568, 179);
                internalFrame_Write.getContentPane().add(editorPane);

                JButton save = new JButton("SAVE");//保存按钮
                save.setBounds(465, 213, 93, 23);
                save.addMouseListener(new MouseAdapter() {
                    public void mouseClicked(MouseEvent e) {
            	        //获取标题
            	        String title = textField.getText();	
            	        //获取内容
                    	String txt = editorPane.getText();
                    	//调用Diary.addDiary()方法建立日记
                    	//该方法将会在以后的课程中完成

	                    //日记建立完成后隐藏当前窗口
	                    internalFrame_Write.setVisible(false);
		            }
                });
    	        internalFrame_Write.getContentPane().add(save);
    	        internalFrame_Write.setVisible(true);
	        }  
        });

		panel.add(addButton);

		//删除按钮
		JButton delButton = new JButton("Delete");
		delButton.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				File file=null;
				int value=chooser.showOpenDialog(panel);
				if(value==JFileChooser.APPROVE_OPTION)
				{
					file=chooser.getSelectedFile();
					
				    //删除确认框，用于确定用户是否确定删除      
		            int x=                                     JOptionPane.showConfirmDialog(panel,"Confirm delete?","Please confirm",JOptionPane.OK_CANCEL_OPTION,JOptionPane.QUESTION_MESSAGE);
					
                	if(file.exists())
                	{
            			//当用户选择了OK时，调用删除方法
            			if(x==JOptionPane.OK_OPTION) {
						    file.delete();
					
					        //打印删除成功提示信息
					        JOptionPane.showMessageDialog(panel, "Delete Success!","information", JOptionPane.PLAIN_MESSAGE);
				        }
		            }
					
	            }
				
		    }
        });
        
		panel.add(delButton);

        //返回按钮
		JButton back = new JButton("BACK");
		back.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				IndexGUI.init();
				setVisible(false);
			}
		});
		
		panel.add(back);
	}
}
```

以上就是用户界面的代码。

```checker
- name: check UsersGUI java exist
  script: |
    #!/bin/bash
    ls /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/view/UsersGUI.java
  error: 我们发现您还没有创建UsersGUI.java! /home/shiyanlou/eclipse-workspace/KnowYou/src/com/shiyanlou/view/UsersGUI.java
  timeout: 1
```

## 四、实验总结

在本节实验中，我们完成了系统的 GUI 设计实现，在以后的章节中，我们将逐渐完成我们的功能类的设计，也是我们日记软件的重点部分。

- 建立用户类
- 用户 XML 文件设计