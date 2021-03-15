---
sort: 2.1
---

# 流程控制

## 简介

本节内容接上节内容，继续讲解 Java 基础语法的剩余内容。主要包括流程控制中的条件语句、循环语句以及跳转语句、数组的相关操作以及用户输入的操作。

#### 知识点

- 流程控制
- 数组
- 用户输入操作

## 流程控制

流程控制对任何一门编程语言都是至关重要的，它为我们提供了控制程序步骤的基本手段。常见对主要分为，条件语句、循环语句、跳转语句。

### if 语句

`if` 语句是一种判断语句。

语法：

```java
if(条件){
    条件成立时执行的代码
}
```

![if语句执行过程](https://doc.shiyanlou.com/document-uid79144labid1051timestamp1434347907568.png)

`if...else` 语句当条件成立时，则执行 `if` 部分的代码块； 条件不成立时，则进入 `else` 部分。例如，如果一个月天数大于 30 天，则为大月，否则为小月。

语法：

```java
if(条件){
    代码块1
}
else{
    代码块2
}
```

![if...else语句执行过程](https://doc.shiyanlou.com/document-uid79144labid1051timestamp1434347936247.png)

多重 `if` 语句，在条件 1 不满足的情况下，才会进行条件 2 的判断，以此向下；当前面的条件均不成立时，最终执行 `else` 块内的代码。

语法：

```java
if(条件1){
    代码块1
}
else if(条件2){
    代码块2
}
...
else {
    代码块n
}
```

![多重if语句](https://doc.shiyanlou.com/document-uid79144labid97timestamp1437039991806.png)

> 注意：如果 `if`（或 `else if`，或 `else`) 条件成立时的执行语句只有一条，是可以省略大括号的！但如果执行语句有多条，那么大括号就是不可或缺的。

比如：

```java
int days = 31;
if(days > 30)
    System.out.println("本月是大月");
else
    System.out.println("本月是小月");
```

if 语句是可以在内层进行嵌套的。嵌套 if 语句，只有当外层 if 的条件成立时，才会判断内层 if 的条件。

语法：

```java
if(条件1){
    if(条件2){
        代码块1
    }
    else{
        代码块2
    }
}
else{
    代码块3
}
```

![if的嵌套](https://doc.shiyanlou.com/document-uid79144labid1051timestamp1434348012040.png)

`if` 语句练习：小明考了 78 分，60 分以上及格，80 分以上为良好，90 分以上为优秀，60 分以下要重考，编写源代码 `ScoreJudge.java`，输出小明的情况。

参考代码如下：

```java
public class ScoreJudge {
	public static void main(String[] args){
		int score = 78;
		if(score >= 60){
			if(score >= 80){
				if(score >= 90){
					System.out.println("成绩优秀");
				}
				else{
					System.out.println("成绩良好");
				}
			}
			else{
				System.out.println("成绩及格");
			}
		}
		else{
			System.out.println("需要补考");
		}
	}
}
```

> **注**：所有的条件语句都是利用条件表达式的真或假来决定执行路径，Java 里不允许将一个数字作为布尔值使用，虽然这在 C 和 C++ 是允许的，如果要在布尔测试里使用一个非布尔值，需要先用一个条件表达式将其转换成布尔值，其他控制语句同理。

编译执行：

```shell
$ javac ScoreJudge.java
$ Java ScoreJude
成绩及格
```

```checker
- name: check ScoreJudge java exist
  script: |
    #!/bin/bash
    ls /home/project/ScoreJudge.java
  error:没有找到 /home/project/ScoreJudge.java 文件
```

### switch 语句

当需要对选项进行等值判断时，使用 `switch` 语句更加简洁明了。比如：摇号摇到 1 的得一等奖，摇到 2 的得二等奖，摇到 3 的等三等奖，摇到其他的没有奖。

语法：

```java
switch(表达式){
    case 值1:
        代码块1
        break;
    case 值2:
        代码块2
        break;
    ...
    default:
        默认执行的代码块
}
```

当 `switch` 后表达式的值和 `case` 语句后的值相同时，从该位置开始向下执行，直到遇到 `break` 语句或者 `switch` 语句块结束；如果没有匹配的 `case` 语句则执行 `default` 块的代码。

> `defualt` 块不是必须的，默认为空。

新建一个源代码文件`Draw.java`。

```java
public class Draw {
	public static void main(String[] args){
		int num = 2;
		switch(num){
		case 1:
			System.out.println("恭喜你，获得了一等奖");
			break;
		case 2:
			System.out.println("恭喜你，获得了二等奖");
			break;
		case 3:
			System.out.println("恭喜你，获得了三等奖");
			break;
		default:
			System.out.println("很遗憾，下次再来");
		}
	}
}
```

编译运行：

```bash
$ javac Draw.java
$ java Draw
恭喜你，获得了二等奖
```

```checker
- name: check Draw java exist
  script: |
    #!/bin/bash
    ls /home/project/Draw.java
  error: 没有找到 /home/project/Draw.java 文件
```

### while 和 do-while 语句

`while`语法：

```java
while(条件){
    代码块
}
```

`while` 的执行过程是先判断，再执行。

1. 判断 `while` 后面的条件是否成立 ( `true` or `false` )
2. 当条件成立时，执行循环内的代码。

然后重复执行 `1`、`2`， 直到循环条件不成立为止。

![while的语句流程](https://doc.shiyanlou.com/document-uid79144labid1051timestamp1434348643037.png)

`do-while` 语法：

```java
do{
    代码块
}while(条件);
```

`do-while` 的执行过程是先执行一次，再循环判断（所以循环内的代码至少会执行一次）。

1. 先执行一遍循环操作，然后判断循环条件是否成立。
2. 如果条件成立，继续执行`1`、`2`，直到循环条件不成立为止。

![do...while的流程](https://doc.shiyanlou.com/document-uid79144labid1051timestamp1434348718160.png)

如：

```java
int i = 0;
while(i < 100){
    System.out.println("I love ShiYanlou!");
    i++;
}
```

```java
int i = 0;
do {
    System.out.println("I love ShiYanlou!");
    i++;
} while(i < 100);
```

练习：分别用 `while` 和 `do-while` 两种方法，编写代码，文件名为： `SumOfEven.java`。实现计算 1-1000 中所有偶数的和，并输出。验证一下两种方法你输出的结果是一致吗？

参考代码如下：

```java
public class SumOfEven {
	public static void main(String[] args){
		int i1 = 1, i2 = 1;
		int sum1 = 0, sum2 = 0;

		while (i1 <= 1000){     //循环1000次
			if(0 == i1 % 2){   //判断是否为偶数
				sum1 += i1;    //将偶数加入到总数里
			}
			i1++;              //i自增1
		}
		System.out.println("用while，1到1000中，所有偶数的和为："+sum1);

		do {
			if (0 == i2 % 2){   //在条件语句中，将数值写在前面是为了防止将==写成了=
				sum2 += i2;
			}
		    i2++;
		} while(i2 <= 1000);
		System.out.println("用do-while，1到1000中，所有偶数的和为："+sum2);
	}
}
```

编译运行：

```bash
$ javac SumOfEven.java
$ java SumOfEven
用while，1到1000中，所有偶数的和为：250500
用do-while，1到1000中，所有偶数的和为：250500
```

```checker
- name: check SumOfEven java exist
  script: |
    #!/bin/bash
    ls /home/project/SumOfEven.java
  error: 没有找到 /home/project/SumOfEven.java 文件
```

### for 语句

`for` 语法：

```java
for(循环变量初始化①; 循环条件②; 循环变量值操作③){
    循环操作④
}
```

`for` 相比 `while` 和 `do-while` 语句结构更加简洁易读，它的执行顺序：

1. 执行循环变量初始化部分（1），设置循环的初始状态，此部分在**整个循环中只执行一次**。
2. 进行循环条件的判断（2），如果条件为 `true`，则执行循环体内代码（4）；如果为 `false` ，则直接退出循环。
3. 执行循环变量值操作部分（3），对循环变量的值进行修改，然后进行下一次循环条件判断（4）。

整个循环的流程可以简化为：

```
(1) -> [(2)->(4)->(3)] -> [(2)->(4)->(3)] -> ... => (3) 结果为 false, 退出循环。
```

![for的流程](https://doc.shiyanlou.com/document-uid79144labid1051timestamp1434348757545.png)

例如，计算 100 以内不能被 3 整除的数之和：

```java
	int sum = 0; // 保存不能被3整除的数之和
	// 循环变量 i 初始值为 1 ,每执行一次对变量加 1，只要小于等于 100 就重复执行循环
	for (int i = 1;i<=100;i++) {
    // 变量 i 与 3 进行求模（取余），如果不等于 0 ，则表示不能被 3 整除
		if (i % 3 != 0) {
			sum = sum + i; // 累加求和
		}
	}
	System.out.println("1到100之间不能被3整除的数之和为：" + sum);
```

练习：编写代码，文件名为： `SumOfEven.java`，实现计算 1-1000 中所有偶数的和，并输出。

参考代码如下：

```java
public class SumOfEven {
	public static void main(String[] args){
		int sum = 0;
		for(int i = 1; i <= 1000; i++){
			if(0 == i % 2){
				sum += i;
			}
		}
		System.out.println("用for，1到1000中，所有偶数和为："+sum);
	}
}
```

编译运行：

```shell
$ javac SumOfEven.java
$ java SumOfEven
用for，1到1000中，所有偶数和为：250500
```

```checker
- name: check SumOfEven java exist
  script: |
    #!/bin/bash
    ls /home/project/SumOfEven.java
  error: 没有找到 /home/project/SumOfEven.java 文件
```

### 练习题：字符串处理

在 `/home/project/` 目录下新建文件 `StringUtil.java`，你需要实现以下需求：

- 从控制台输入一行字符串
- 去除字符串中的所有空格
- 打印去除空格后的字符串

示例：

```java
输入：
    shi ya n  lou
输出：
    shiyanlou
```

提示：`java.util.Scanner` 可以获取控制台输入。

```java
Scanner in = new Scanner(System.in);
//获取String值
String a = in.nextLine();
```

```checker
- name: 检查是否存在 StringUtil
  script: |
    #!/bin/bash
    ls -l /home/project/StringUtil.java
  error: |
    没有找到 /home/project/StringUtil.java 文件
- name: 输出结果检测
  script: |
    #!/bin/bash
    javac /home/project/StringUtil.java -d . ;echo -e 'shi   yan   lo u1' |java StringUtil |grep -w shiyanlou1
  error: |
    输出结果错误
  timeout: 20
```

### 参考答案

**注意：请务必先独立思考获得 PASS 之后再查看参考代码，直接拷贝代码收获不大**

```java
import java.util.Scanner;
public class StringUtil {
    public static void main(String[] args) {
        Scanner in =new Scanner(System.in);
		//获取String值
		String a=in.nextLine();
        StringBuilder stringBuilder = new StringBuilder(a);
        for (int i = 0; i < stringBuilder.length(); i++) {
            if (stringBuilder.charAt(i)==' ') {
                System.out.println(i);
                stringBuilder.deleteCharAt(i);
                i--;
            }else {
                stringBuilder.charAt(i);
            }
        }
        System.out.println(stringBuilder.toString());
    }
}
```

### 练习题：对比字符串

在`/home/project/`目录下新建`ContrastString.java`，你需要实现以下需求：

- 从控制台输入字符串 a 和字符串 b
- 比较字符串 a 和字符 b 是否完全一致，长度，内容等完全一致。
- 如果完全一致，输出`相同`，如果不一致，输出`不同`。
- 禁止使用`equals`方法

示例：

```java
输入：
    abc3
    abc3
输出：
    相同
```

提示：`java.util.Scanner` 可以获取控制台输入。

```java
import java.util.Scanner;

public class App{
  	public static void main(String[] args){
      	Scanner in = new Scanner(System.in);
        //获取String值
        String a = in.nextLine();
        String b = in.nextLine();
    }
}
```

```checker
- name: 检查是否存在 ContrastString
  script: |
    #!/bin/bash
    ls -l /home/project/ContrastString.java
  error: |
    没有找到 /home/project/ContrastString.java 文件
- name: 输出结果检测
  script: |
    #!/bin/bash
    javac /home/project/ContrastString.java -d . ;echo -e 'adbdfa\n323s' |java ContrastString |grep '不同'
  error: |
    输出结果错误
  timeout: 20
```

### 参考答案

**注意：请务必先独立思考获得 PASS 之后再查看参考代码，直接拷贝代码收获不大**

```java
import java.util.Scanner;

public class ContrastString {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        //获取String值
        String a = in.nextLine();
        String b = in.nextLine();
        if (a.length() != b.length()) {
            System.out.println("不同");
            return;
        }
        for (int i = 0; i < a.length(); i++) {
            if (a.charAt(i) != b.charAt(i)) {
                System.out.println("不同");
                return;
            }
        }
        System.out.println("相同");
    }
}
```

### 跳转语句

`break` 关键字经常用在条件和循环语句中，用来跳出**循环语句**。

`continue`关键字的作用是跳过循环体中剩余的语句执行下一次循环。
新建一个源代码文件`Jump.java`。

```java
public class Jump{
    public static void main(String[] args){
		//break 练习
        for(int i = 1; i <= 10; i++){
            System.out.println("循环第"+i+"次");
            if(0 == i % 3){
                break;
            }
            if(0 == i % 5){
                System.out.println("我进来了！");
            }
        }
		//continue练习 打印10以内的所有奇数
        for(int i = 1; i <= 10; i++){
            if(0 == i % 2) //判断i是否为偶数
                continue;  //通过continue结束本次循环
            System.out.println(i);
        }
    }
}
```

编译运行：

```bash
$ javac Jump.java
$ java Jump
循环第1次
循环第2次
循环第3次
1
3
5
7
9
```

```checker
- name: check Jump java exist
  script: |
    #!/bin/bash
    ls /home/project/Jump.java
  error: 没有找到 /home/project/Jump.java 文件
```

### 练习题：打印星期

在`/home/project/`目录下新建一个源代码文件`PrintWeek.java`。

你需要在实现以下需求：

- 从控制台获取一个整型参数
- 当输入数字 1 时输出`今天是星期一`
- 当输入数字 2 时输出`今天是星期二`
- ......
- 当输入数字 7 时输出`今天是星期天`

示例：

```java
输入：
    1
输出：
    今天是星期一

```

提示：`java.util.Scanner`可以获取控制台输入。

```java
Scanner in =new Scanner(System.in);
//获取int值
int x=in.nextInt();
```

```checker
- name: 检查是否存在 PrintWeek
  script: |
    #!/bin/bash
    ls /home/project/PrintWeek.java
  error: |
    没有找到 /home/project/PrintWeek.java 文件
- name: 输出结果检测
  script: |
    #!/bin/bash
    javac /home/project/PrintWeek.java -d . ;echo 1|java PrintWeek |grep '今天是星期一'
  error: |
    输出结果错误
  timeout: 20
```

### 参考答案

**注意：请务必先独立思考获得 PASS 之后再查看参考代码，直接拷贝代码收获不大**

```java
import java.util.Scanner;

public class PrintWeek {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        //获取int值
        int x = in.nextInt();
        switch (x) {
            case 1:
                System.out.println("今天是星期一");
                break;
            case 2:
                System.out.println("今天是星期二");
                break;
            case 3:
                System.out.println("今天是星期三");
                break;
            case 4:
                System.out.println("今天是星期四");
                break;
            case 5:
                System.out.println("今天是星期五");
                break;
            case 6:
                System.out.println("今天是星期六");
                break;
            case 7:
                System.out.println("今天是星期天");
                break;
        }
    }
}
```
