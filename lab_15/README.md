---
sort: 15
---

# Java 版图形界面计算器

## 一、实验介绍

#### 1.1 实验内容

本次实验利用 Java 开发一个可以进行简单的四则运算的图形化计算器，会使用到 Java Swing 图形组件进行开发。

#### 1.2 实验知识点

- Java Swing 界面编程
- 计算器逻辑运算实现

#### 1.3 实验环境

本实验环境采用带桌面的 Ubuntu Linux 环境，实验中会用到环境或软件：

- JDK 1.8
- Xfce 终端
- Eclipse：一个开放源代码的、基于 Java 的可扩展开发平台，用于 Java 程序开发。

#### 1.4 适合人群

本课程难度为一般难度，属于初级课程，适合具有 Java 基础和 Swing 组件编程知识的用户学习。

如果你之前没有了解过 Swing 开发，可以先学习[《JDK 核心 API》](https://www.shiyanlou.com/courses/109)。

#### 1.5 代码获取

你可以通过下面命令将实验的完整代码下载到实验楼环境中，作为参照对比进行学习。

```bash
$ wget http://labfile.oss.aliyuncs.com/courses/185/Calculator.java
```

## 二、实验原理

要制作一个计算器，首先需要知道它由哪些部分组成。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid550timestamp1474599531216.png)

从结构上来说，一个简单的图形界面，需要由界面组件、组件的事件监听器（响应各类事件的逻辑）和具体的事件处理逻辑组成。

整个代码结构如下图所示：

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid550timestamp1474616323112.png)

界面实现的主要工作是创建各个界面组件对象，对其进行初始化，以及控制各组件之间的层次关系和布局。

## 三、实验步骤

接下来我们使用 Eclipse 来开发我们的计算器程序。

点击底部的![按钮](https://doc.shiyanlou.com/document-uid1labid6171timestamp1524190021794.png) 开始进行实验。

### 3.1 项目创建

方式 1：请双击打开桌面上的 eclipse ，等待启动完成后，按照下面的步骤来创建项目。

> 如果你对该步骤已经非常熟悉，可以直接跳转到下一小节学习。

（1）在文件菜单 `File` 中选择 `New -> Project` 来创建项目。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190926-1569484261539)

（2）在弹出的新建项目对话框中选择 `Java Project`，并点击 `Next` 按钮进入下一步。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190926-1569484535316)

（3）在 `Project name` 一栏填写项目名称 `Calculator`，并点击 `Finish` 按钮完成创建。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190928-1569655771622)


（4）如果遇到下图所示的对话框，点击 `Open Perspective` 按钮确认即可。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190928-1569655825574)

（5）在创建好后的项目目录 `src` 上右键点击，在右键菜单中选择 `New -> Class` 来创建一个类。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190928-1569655957580)

（6）在新建类对话框中填写包名 `com.shiyanlou.calculator` 和类名 `Calculator` （首字母大写）。点击 `Finish` 按钮完成创建。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190928-1569656026274)

（7）按照本课程后面的内容编辑 `Calculator.java` 文件。

方式 2：如同方式 1 一样用 Eclipse 创建好包，通过 Xfce 终端下载完整代码，再加入到包中。

```checker
- name: check Calculator exist
  script: |
    #!/bin/bash
    ls /home/shiyanlou/workspace/Calculator
  error: 我们发现您还没有创建项目Calculator! /home/shiyanlou/workspace/Calculator
  timeout: 1
- name: check Calculator java exist
  script: |
    #!/bin/bash
    ls /home/shiyanlou/workspace/Calculator/src/com/shiyanlou/calculator/Calculator.java
  error: 我们发现您还没有创建Calculator.java! /home/shiyanlou/workspace/Calculator/src/com/shiyanlou/calculator/Calculator.java
  timeout: 1
```

### 3.2 UI 组件创建和初始化

首先我们需要将界面中要用到的 UI 组件作为 Calculator 类的成员变量在一开始声明。在阅读代码之前，可以思考一下都要用到哪些 UI 组件，以及这些代码应当写在哪个位置等等。

一个计算器界面至少包括窗口、按钮和显示文本框。如下图，这是我们希望达到的效果。

<center><img src="https://doc.shiyanlou.com/courses/uid977658-20190916-1568600866660" /></center>

#### （1）窗口的创建

在《JDK 核心 API》中我们提到，创建一个窗口需要使用 JFrame 类。在本实验中，我们创建一个 JFrame 实例，并调用实例的方法进行组件的添加（与之前编写一个 JFrmae 子类的效果是相同的）。

```java
// 创建一个 JFrame 对象并初始化。JFrame 可以理解为程序的主窗体。
JFrame frame = new JFrame("Calculator");

// 设置主窗口出现在屏幕上的位置
frame.setLocation(300, 200);

// 设置窗体不能调大小
frame.setResizable(false); 
```

这里，我们先不设置窗口的大小，待我们将所有组件添加到窗体上之后，调用 `pack()` 方法，让窗体自己调整大小（在 3.3 （4）窗体添加面板 1 和面板 2 部分会介绍）。

#### （2）所需的组件

- 显示计算结果

```java
// 创建一个 JTextField 对象并初始化。 JTextField 是用于显示操作和计算结果的文本框。
// 参数 20 表明可以显示 20 列的文本内容
JTextField result_TextField = new JTextField(result, 20);
```

> 这里的 result 是等会儿会创建的一个 String 对象，它记录了计算的结果，我们赋予其初始值 `""`（空字符串）。

- 清除按钮

```java
// 清除按钮
JButton clear_Button = new JButton("Clear");
```

- 数字按钮

```java
// 数字键0到9
JButton button0 = new JButton("0");
JButton button1 = new JButton("1");
JButton button2 = new JButton("2");
JButton button3 = new JButton("3");
JButton button4 = new JButton("4");
JButton button5 = new JButton("5");
JButton button6 = new JButton("6");
JButton button7 = new JButton("7");
JButton button8 = new JButton("8");
JButton button9 = new JButton("9");
```

- 操作符按钮

```java
// 计算命令按钮，加减乘除以及小数点等
JButton button_Dian = new JButton(".");
JButton button_jia = new JButton("+");
JButton button_jian = new JButton("-");
JButton button_cheng = new JButton("*");
JButton button_chu = new JButton("/");
```

- 等于按钮（按下后进行计算）

```java
// 计算按钮
JButton button_dy = new JButton("=");
```

### 3.3 在窗体中添加 UI 组件

#### （1）面板

这个计算器有两个 JPanel。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid550timestamp1474600123518.png)

什么是 JPanel：JPanel 是一般轻量级容器。如上图所示，你可以将其理解为一个盛放其他 UI 组件的“篮子”。 JPanel 位于 `javax.swing` 包中，为面板容器，可以加入到 JFrame 中 ， 它自身是个容器，也可以把其他 component （组件） 加入到 JPanel 中，例如 JButton、JTextArea、JTextField 等。

在这个项目中，两个 JPanel 分别对应这个计算器按键除 “Clear” 键外其他的键，另外一个面板则是输出栏跟 “Clear” 键，参考如下图。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190916-1568602922337)

同样，在书写本段代码时，你应当思考它应该放在哪个部分。如果不清楚，可以回到上面的代码结构中查看。

#### （2）放置数字键等的面板

对于面板 1，可供参考的代码如下所示：

首先初始化一个面板对象 pan。

```java
// 创建一个 Jpanel 对象并初始化
JPanel pan = new JPanel();
```

设置 pan 的布局为网格布局 GridLayout，具体的使用方法可以参考 [Class GridLayout - 官方文档](https://docs.oracle.com/javase/9/docs/api/java/awt/GridLayout.html)。在本程序中，我们使用的 GridLayout 构造函数传入了四个参数，含义分别为创建一个 4 行（第一个参数）、4 列（第二个参数）的网格，每个网格宽度为 5（第三个参数）、高度为 5 （第四个参数）。

```java
// 设置该容器的布局为四行四列，边距为5像素
pan.setLayout(new GridLayout(4, 4, 5, 5));
```

如下图，但我们对 pan 进行 add 操作时，组件会按照 1、2、3... 的顺序进行填充。

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190916-1568603847693)

对比之前的效果图，我们应该按照下面的顺序进行 add 操作。

```java
// 将用于计算的按钮添加到容器内
pan.add(button7);
pan.add(button8);
pan.add(button9);
pan.add(button_chu);
pan.add(button4);
pan.add(button5);
pan.add(button6);
pan.add(button_cheng);
pan.add(button1);
pan.add(button2);
pan.add(button3);
pan.add(button_jian);
pan.add(button0);
pan.add(button_Dian);
pan.add(button_dy);
pan.add(button_jia);
```

为了更加好看，我们可以为 pan 对象设置边距。

```java
// 设置 pan 对象的边距
pan.setBorder(BorderFactory.createEmptyBorder(5, 5, 5, 5));
```

#### （3）放置清除框等的面板

对于面板 2，可供参考的代码如下：

首先初始化一个面板对象 pan2。

```java
// 按照同样的方式设置第二个JPanel
JPanel pan2 = new JPanel();
```

设置它的布局为边界布局。边界布局管理器把容器的的布局分为五个位置：CENTER、EAST、WEST、NORTH、SOUTH。依次对应为：上北（NORTH）、下南（SOUTH）、左西（WEST）、右东（EAST），中（CENTER）。如下图所示：

![图片描述](https://doc.shiyanlou.com/courses/uid977658-20190916-1568604086334)

```java
pan2.setLayout(new BorderLayout());
pan2.add(result_TextField, BorderLayout.WEST);
pan2.add(clear_Button, BorderLayout.EAST);
```

这里我们只设置了 WEST 和 EAST，其他部分没有添加任何东西（没有添加的部分相当于空白）。

#### （4）窗体添加面板 1 和面板 2

窗体中可以放置 JPanel，这里是指我们刚刚创建的面板 1 和面板 2，添加的代码如下：

```java
frame.getContentPane().setLayout(new BorderLayout());
frame.getContentPane().add(pan2, BorderLayout.NORTH);
frame.getContentPane().add(pan, BorderLayout.CENTER);
```

这里，对于 `frame.getContentPane()`（它返回 JFrame 中默认的 JPanel），我们设置布局为 BorderLayout。

当我们添加窗体之后

```java
frame.pack();
frame.setVisible(true);
```

布局结束后，就是计算器的难点：事件处理程序。

### 3.4 响应事件需要使用的变量

对于计算器而言，涉及到的事件响应逻辑主要有：数字键、加减乘除运算、小数点处理、等于以及清除。

这里，我们定义了一些成员变量，方便响应的逻辑实现。

首先，需要定义存储当前被按下的操作数和操作符，result 存储运算的结果。

```java
// 操作数1，为了程序的安全，初值一定设置，这里我们设置为0。
String str1 = "0"; 

// 操作数2
String str2 = "0"; 

// 运算符
String signal = "+"; 

// 运算结果
String result = "";
```

接下来，我们还定义了五个状态开关（五个 int 变量），其含义在注释中有说明。

```java
// 以下k1至k5为状态开关

// 开关1用于选择输入方向，将要写入str1或str2
// 为 1 时写入 str1，为 2 时写入 str2
int k1 = 1;

// 开关 2 用于记录符号键的次数
// 如果 k2>1 说明进行的是 2+3-9+8 这样的多符号运算
int k2 = 1;

// 开关3用于标识 str1 是否可以被清 0 
// 等于 1 时可以，不等于1时不能被清0
int k3 = 1;

// 开关4用于标识 str2 是否可以被清 0
// 等于 1 时可以，不等于1时不能被清0
int k4 = 1;

// 开关5用于控制小数点可否被录入
// 等于1时可以，不为1时，输入的小数点被丢掉
int k5 = 1;
```

这里我们额外定义了一个 JButton 变量，用于存储被按下的符号键。

```java
// store的作用类似于寄存器，用于记录是否连续按下符号键
JButton store; 
```

vt 存储之前输入的运算符。

```java
@SuppressWarnings("rawtypes")
Vector vt = new Vector(20, 10);
```

### 3.5 数字键的响应

注意，我们后面所有定义的 ActionListener 都写在构造函数中，即定义为局部内部类。

关于局部内部类的知识，可以学习 [《Java 编程语言基础》 课程的第三个实验 《面向对象》](https://www.shiyanlou.com/courses/1230/learning/?id=9467)。

数字键响应的主要是处理数字存入到对应的变量中（第一个操作数存入 str1，第二个操作数存入 str2）。

这里我们定义的局部内部类名为 Listener，继承 ActionListener 接口。继承之后，我们需要重写接口定义的 `actionPerformed` 方法。

```java
class Listener implements ActionListener {
	@Override
	public void actionPerformed(ActionEvent e) {

	}
}
```

通过上面的 `actionPerformed` 方法 的入参 `ActionEvent e`，我们可以获取到事件源，如下：

```java
// 获取事件源，并从事件源中获取输入的数据
String ss = ((JButton) e.getSource()).getText();
```

接下来读入存储的符号键，并添加到 vt 中去。

```java
// 读入存储的符号键
store = (JButton) e.getSource();
vt.add(store);
```

还记得我们之前定义的 k1 开关吗？当 k1 为 1 时，我们输入的数字是操作数 1 的一部分；当 k1 为 2 时，我们输入的数字是操作数 2 的一部分。因此会有以下逻辑：

```java
if( k1 == 1) {
	// 输入是操作数 1 的一部分
} else if( k1 == 2) {
	// 输入是操作数 2 的一部分
}
```

- 输入为操作数 1 的一部分时

我们需要判断操作数 1 是否可以被清零（通过 k3 的值即可判断），如果可以（存储的内容是上一次运算的），则先清空再写入；如果不可以清零（先前已经输入了操作数 1 的一部分，比如输入数字 34，上一次按了 3，这一次读到的是 4），这种情况下需要将输入追加到上一次的输入中

```java
if (k3 == 1) {
	str1 = "";
	
	// 还原开关k5状态
	k5 = 1;
}
str1 = str1 + ss;
```

这里，我们输入的是数字，因此后面随时可用输入小数点，为了防止出错，给 k5 进行赋值。

当输入完成后，我们需要给 k3 的值加 1，保证 操作数 1 不会被清空。并且还需要将操作数 1 打印到结果栏。

```java
k3 = k3 + 1;

// 显示结果
result_TextField.setText(str1);
```

- 输入为操作数 2 的一部分时

这部分的逻辑与操作数 1 是完全相同的。唯一不同的是，操作数变为了 str2（即操作数 2）。

```java
if (k4 == 1) {
	str2 = "";
	
	// 还原开关k5状态
	k5 = 1; 
}
str2 = str2 + ss;
k4 = k4 + 1;
result_TextField.setText(str2);
```

完整的代码如下：

```java
// 数字键
class Listener implements ActionListener {
	@SuppressWarnings("unchecked")
	public void actionPerformed(ActionEvent e) {
		// 获取事件源，并从事件源中获取输入的数据
		String ss = ((JButton) e.getSource()).getText();

		store = (JButton) e.getSource();
		vt.add(store);

		if (k1 == 1) {
			if (k3 == 1) {
				str1 = "";
				
				// 还原开关k5状态
				k5 = 1;
			}
			str1 = str1 + ss;

			k3 = k3 + 1;
			
			// 显示结果
			result_TextField.setText(str1);

		} else if (k1 == 2) {
			if (k4 == 1) {
				str2 = "";
				
				// 还原开关k5状态
				k5 = 1; 
			}
			str2 = str2 + ss;
			k4 = k4 + 1;
			result_TextField.setText(str2);
		}

	}
}
```

### 3.6 小数点的响应

注意，小数点的响应也是定义为局部内部类，与数字键的响应类是相同的。这个局部内部类命令为 `Listener_xiaos`，继承 ActionListener 接口。

首先是获取响应源，并添加到 vt 中。

```java
store = (JButton) e.getSource();
vt.add(store);
```

输入小数点需要在 k5 为 1 的情况下才可以输入，否则输入的小数点被丢掉。

```java
if( k5 == 1) {
	// 添加对小数点的处理
}
```

接下来，我们写上面的 if 语句中的语句块。

首先还是获取输入的内容：

```java
String ss2 = ((JButton) e.getSource()).getText();
```

对于输入的小数点，可能是 str1 的，也有可能是 str2 的，这部分的逻辑与数字的逻辑是相似的。

```java
if (k1 == 1) {
	if (k3 == 1) {
		str1 = "";
		// 还原开关k5状态
		k5 = 1; 
	}
	str1 = str1 + ss2;

	k3 = k3 + 1;

	// 显示结果
	result_TextField.setText(str1);

} else if (k1 == 2) {
	if (k4 == 1) {
		str2 = "";
		// 还原开关k5的状态
		k5 = 1;
	}
	str2 = str2 + ss2;

	k4 = k4 + 1;

	result_TextField.setText(str2);
}
```

最后，为了防止输入小数点之后再次输入小数点，需要进行 `k5 = k5 + 1;` 的操作。

完整的代码如下：

```java
// 小数点的处理
class Listener_xiaos implements ActionListener {
	@SuppressWarnings("unchecked")
	public void actionPerformed(ActionEvent e) {
		store = (JButton) e.getSource();
		vt.add(store);
		if (k5 == 1) {
			String ss2 = ((JButton) e.getSource()).getText();
			if (k1 == 1) {
				if (k3 == 1) {
					str1 = "";
					// 还原开关k5状态
					k5 = 1; 
				}
				str1 = str1 + ss2;

				k3 = k3 + 1;

				// 显示结果
				result_TextField.setText(str1);

			} else if (k1 == 2) {
				if (k4 == 1) {
					str2 = "";
					// 还原开关k5的状态
					k5 = 1;
				}
				str2 = str2 + ss2;

				k4 = k4 + 1;

				result_TextField.setText(str2);
			}
		}

		k5 = k5 + 1;
	}
}
```

### 3.7 运算符号的响应

注意，运算符的响应定义为局部内部类，与数字键的响应类是相同的。这个局部内部类命令为 `Listener_signal`，继承 ActionListener 接口。

获取响应事件的源，读取内容，并且将响应源存入 vt 中。

```java
String ss2 = ((JButton) e.getSource()).getText();
store = (JButton) e.getSource();
vt.add(store);
```

运算符的处理，需要分情况讨论。k2 变量为 1 时，说明这是进行的普通运算操作（比如 `2+3`，先输入 `2`，再输入 `+`，然后输入 `3`）；如果 k2 > 1 说明进行的是 2+3-9+8 这样的多符号运算（已经输入 `2+3`，然后输入 `-` 和 `9`），即上一次的运算结果存储在 str1 中，符号输入之后要输入的数字是 str2。

- 普通运算操作

当 k2 为 1 时，我们只需要将 k1 开关设置为 2，即接下来输入的数字是 str2。第二个操作数不能以 `.` 开头，因此将 k5 置为 1。k2 自增 1，如果等会儿还有符号输入，则对应到第二种情况中。

```java
if (k2 == 1) {
	// 开关 k1 为 1 时向数 1 写输入值，为 2 时向数2写输入值。
	k1 = 2;
	k5 = 1;
	signal = ss2;
	k2 = k2 + 1;// 按符号键的次数
} else {
	// ...
}
```

- 连续运算

else 部分对应这种情况。首先读入上一次的输入（vt 中的第 `vt.size()-2` 个元素），如果这个输入不是 `+`、`-`、`*`、`/` 中的一个，说明是要进行连续运算。

> 从逻辑上还可以防止连续输入运算符的情况。

此时调用 `calc()` 进行运算（这个方法是我们自己定义的运算，在 3.9 中实现），将结果存入到 `str1` 中。

在这个符号之后就是输入操作数 2，因此 k1 置为 2；在输入数字之前不能输入小数点，因此 k5 置为 1；对于连续运算，str2 应该先被清空再输入，因此 k4 置为 1。

singal 存储此次输入的符号。

最后 k2 加 1，增加已经输入的符号的次数。

```java
if (k2 == 1) {
	// ...
} else {
	int a = vt.size();
	JButton c = (JButton) vt.get(a - 2);

	if (!(c.getText().equals("+"))
			&& !(c.getText().equals("-"))
			&& !(c.getText().equals("*"))
			&& !(c.getText().equals("/")))

	{
		cal();
		str1 = result;
		// 开关 k1 为 1 时，向数 1 写值，为2时向数2写
		k1 = 2;
		k5 = 1;
		k4 = 1;
		signal = ss2;
	}
	k2 = k2 + 1;
}
```

完整的代码如下：

```java
// 输入的运算符号的处理
class Listener_signal implements ActionListener {
	@SuppressWarnings("unchecked")
	public void actionPerformed(ActionEvent e) {
		String ss2 = ((JButton) e.getSource()).getText();
		store = (JButton) e.getSource();
		vt.add(store);

		if (k2 == 1) {
			// 开关 k1 为 1 时向数 1 写输入值，为 2 时向数2写输入值。
			k1 = 2;
			k5 = 1;
			signal = ss2;
			k2 = k2 + 1;// 按符号键的次数
		} else {
			int a = vt.size();
			JButton c = (JButton) vt.get(a - 2);

			if (!(c.getText().equals("+"))
					&& !(c.getText().equals("-"))
					&& !(c.getText().equals("*"))
					&& !(c.getText().equals("/")))

			{
				cal();
				str1 = result;
				// 开关 k1 为 1 时，向数 1 写值，为2时向数2写
				k1 = 2;
				k5 = 1;
				k4 = 1;
				signal = ss2;
			}
			k2 = k2 + 1;

		}

	}
}
```

### 3.8 等于的响应

注意，等于的响应也是定义为局部内部类，与数字键的响应类是相同的。这个局部内部类命令为 `Listener_dy`，继承 ActionListener 接口。

当等于键按下之后，调用 `calc()` 进行运算，还原开关的值即可。

最后做了一个操作 `str1 = result;`，是为了应对 `7+5=12 +5=17` 这种情况。上一次运算的结果在下一个运算中默认作为第一个操作数。

```java
// 等于按键的逻辑，即在输入完成后开始计算
class Listener_dy implements ActionListener {
	@SuppressWarnings("unchecked")
	public void actionPerformed(ActionEvent e) {

		store = (JButton) e.getSource();
		vt.add(store);
		cal();
		
		// 还原开关k1状态 
		k1 = 1; 
		
		// 还原开关k2状态
		k2 = 1;
		
		// 还原开关k3状态
		k3 = 1;
		
		// 还原开关k4状态
		k4 = 1; 

		// 为 7+5=12 +5=17 这种计算做准备
		str1 = result; 
	}
}
```

### 3.9 计算逻辑的实现

计算的逻辑要针对输入的不同运算符来对操作数进行运算，同时还要考虑到除以 0 这种不合理的算法容错。

对于计算逻辑，我们写在一个名为 `calc()` 的成员函数中。

首先要将操作数转为 double 类型，代码中定义了 a2 和 b2 用来存储操作数 1 和 操作数 2。

```java
// 操作数1
double a2;
// 操作数2
double b2;

//...

// 手动只输入一个小数点的问题
if (str1.equals("."))
	str1 = "0.0";
if (str2.equals("."))
	str2 = "0.0";

// 转换字符串为 double
a2 = Double.valueOf(str1).doubleValue();
b2 = Double.valueOf(str2).doubleValue();
```

还需要定义一个存储中间运算结果的值

```java
// 运算结果
double result2 = 0;
```

对于运算符号，我们使用一个 `String c` 来存储。

```java
// 运算符
String c = signal;

if (c.equals("")) {
	// 还没有输入符号，不能计算
	result_TextField.setText("Please input operator");
} else {
	// 可以进行计算

	// 手动只输入一个小数点的问题
	if (str1.equals("."))
		str1 = "0.0";
	if (str2.equals("."))
		str2 = "0.0";

	// 转换字符串为 double
	a2 = Double.valueOf(str1).doubleValue();
	b2 = Double.valueOf(str2).doubleValue();

	//...
}
```

当上面的运算符判断和操作数转换都完成后，就可以进行加减乘除运算了。要注意，进行乘法时，为了保证精度，可以将 double 存入大的浮点数类 `BigDecimal` 中。

```java
if (c.equals("")) {
	// 还没有输入符号，不能计算
	result_TextField.setText("Please input operator");
} else {

	//...

	if (c.equals("+")) {
		result2 = a2 + b2;
	}
	if (c.equals("-")) {
		result2 = a2 - b2;
	}
	if (c.equals("*")) {
		BigDecimal m1 = new BigDecimal(Double.toString(a2));
		BigDecimal m2 = new BigDecimal(Double.toString(b2));
		result2 = m1.multiply(m2).doubleValue();
	}
	if (c.equals("/")) {
		if (b2 == 0) {
			result2 = 0;
		} else {
			result2 = a2 / b2;
		}
	}
}
```

最后，输出结果

```java
if (c.equals("")) {
	// 还没有输入符号，不能计算
	result_TextField.setText("Please input operator");
} else {

	//...

	result = ((new Double(result2)).toString());
	result_TextField.setText(result);
}

``

完整代码如下：

```java
// 计算逻辑
public void cal() {
	// 操作数1
	double a2;
	// 操作数2
	double b2;
	// 运算符
	String c = signal;
	// 运算结果
	double result2 = 0;

	if (c.equals("")) {
		result_TextField.setText("Please input operator");
	} else {
		// 手动处理小数点的问题
		if (str1.equals("."))
			str1 = "0.0";
		if (str2.equals("."))
			str2 = "0.0";
		a2 = Double.valueOf(str1).doubleValue();
		b2 = Double.valueOf(str2).doubleValue();

		if (c.equals("+")) {
			result2 = a2 + b2;
		}
		if (c.equals("-")) {
			result2 = a2 - b2;
		}
		if (c.equals("*")) {
			BigDecimal m1 = new BigDecimal(Double.toString(a2));
			BigDecimal m2 = new BigDecimal(Double.toString(b2));
			result2 = m1.multiply(m2).doubleValue();
		}
		if (c.equals("/")) {
			if (b2 == 0) {
				result2 = 0;
			} else {
				result2 = a2 / b2;
			}

		}

		result = ((new Double(result2)).toString());
		result_TextField.setText(result);
	}
}
```

### 3.10 清除的响应

清除的逻辑非常简单，将所有变量的值清空或者置为初始值。

其代码如下：

```java
// 清除键的逻辑（Clear）
class Listener_clear implements ActionListener {
	@SuppressWarnings("unchecked")
	public void actionPerformed(ActionEvent e) {
		store = (JButton) e.getSource();
		vt.add(store);
		k5 = 1;
		k2 = 1;
		k1 = 1;
		k3 = 1;
		k4 = 1;
		str1 = "0";
		str2 = "0";
		signal = "";
		result = "";
		result_TextField.setText(result);
		vt.clear();
	}
}
```

### 3.11 注册监听器

注册各个监听器，即绑定事件响应逻辑到各个 UI 组件上：

```java
// 监听等于键
Listener_dy jt_dy = new Listener_dy();
button_dy.addActionListener(jt_dy);
```

```java
// 监听数字键
Listener jt = new Listener();
button0.addActionListener(jt);
button1.addActionListener(jt);
button2.addActionListener(jt);
button3.addActionListener(jt);
button4.addActionListener(jt);
button5.addActionListener(jt);
button6.addActionListener(jt);
button7.addActionListener(jt);
button8.addActionListener(jt);
button9.addActionListener(jt);

```java
// 监听符号键
Listener_signal jt_signal = new Listener_signal();
button_jia.addActionListener(jt_signal);
button_jian.addActionListener(jt_signal);
button_cheng.addActionListener(jt_signal);
button_chu.addActionListener(jt_signal);
```

```java
// 监听清除键
Listener_clear jt_c = new Listener_clear(); 
clear_Button.addActionListener(jt_c);
```

```java
// 监听小数点键
Listener_xiaos jt_xs = new Listener_xiaos();
button_Dian.addActionListener(jt_xs);
```

除了绑定 UI 的响应时间之外，我们还给窗口绑定了一个事件。

```java
// 窗体关闭事件的响应程序
frame.addWindowListener(new WindowAdapter() {
	public void windowClosing(WindowEvent e) {
		System.exit(0);
	}
});
```

至此，整个计算器的主要逻辑就已经讲解完毕，请自行补充其他的细节。

完成后，请点击菜单中的 `Run -> Run` 选项或者点击工具栏上方的运行按钮来编译运行这个项目。

如果没有遇到错误，则会弹出计算器的窗口。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid550timestamp1474619786086.png)

你可以试用一下。

## 四、实验总结

至此，相信你已经完成了所有的工作。我们在本实验内学习了如何制作一个简易的计算器，具体来说，学习了如何制作 Swing 图形化界面以及为 UI 组件设置事件响应逻辑。

你可能注意到项目的最后部分有如下的代码：

```java
try {
    UIManager.setLookAndFeel("com.sun.java.swing.plaf.windows.WindowsLookAndFeel");
} catch(Exception e) {
    e.printStackTrace();
}
```

我们是试图通过 UIManager 来设置窗体的 UI 风格，如果需要更改，只要做相应的替换就可以了：

- Windows 风格：`com.sun.java.swing.plaf.windows.WindowsLookAndFeel`
- Metal 风格（默认）：`javax.swing.plaf.metal.MetalLookAndFeel`
- 更换为 Motif 风格：`com.sun.java.swing.plaf.motif.MotifLookAndFeel`
- 更换为 Mac 风格：`com.sun.java.swing.plaf.mac.MacLookAndFeel`
- 更换为 GTK 风格：`com.sun.java.swing.plaf.gtk.GTKLookAndFeel`

上述的各种风格需要到相关的操作系统上方可实现。如果你是在 Windows 环境下编程，不妨试一下 Windows 风格。

本次实验目的在于让同学们练习如何使用 Java Swing 进行可视化编程，实验开发的计算器也只具备基本的逻辑，并没有考虑运算的优先级等问题。

## 五、课后习题

同学们下来尝试实现一个较为复杂的计算器，考虑运算的优先级，并增加括号，这些需要数据结构方面的一些知识。

如果你在学习的过程中有任何的问题或者疑问，都欢迎到实验楼的[问答版块](https://www.shiyanlou.com/questions/)与我们交流。