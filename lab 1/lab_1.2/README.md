---
sort: 1.1
---

# 变量与常量
## 变量

变量可以指在计算机存储器里存在值的被命名的存储空间。

变量通常是可被修改的，即可以用来表示可变的状态。这是 Java 的基本概念之一。

程序通过改变变量的值来改变整个程序的状态。为了方便使用变量，所以变量都需要命名，叫做**变量名**。

在 Java 中，变量需要先声明 (declare) 才能使用。在声明中，说明变量的类型，赋予变量以特别名字，以便在后面的程序中调用它。你可以在程序中的任意位置声明变量，语法格式如下：

```
数据类型 变量名称;
```

例如：

```java
int a = 1;
```

在该语法格式中，数据类型可以是 Java 语言中任意的类型，如 `int`。变量名称是该变量的标识符，需要符合标识符的命名规则，数据类型和变量名称之间使用空格进行间隔，使用 `;` 作为结束。

## Java标识符
### 什么是标识符？
  凡是可以由自己命名的地方都称为标识符。
  例如，对于常量、变量、函数、语句块、类、项目等都需要一个名字，这些我们都统统称为标识符。

### 命名规则

对于Java标识符，有以下三点要求：
- 标识符有字母、数字、_(下划线)、$所组成，其中不能以数字开头，不能用Java中的保留字（关键字）
- 标识符采用有意义的简单命名
- "$"不要在代码中出现。（是由于在后面内部类中，编译后会出现$符号）

### 命名规范（驼峰命名法）

类名和接口名：每个单词的首字母，其余为小写。（大驼峰）
方法名：第二个单词起的首字母为大写，其余全为小写。（小驼峰）
常量名：基本数据类型的常量名使用全部大写字母，字与字之间用下划线分隔。

## 关键字和语句

#### 关键字

Java 的关键字对 Java 的编译器有特殊的意义，他们用来表示一种数据类型，或者表示程序的结构等，关键字不能用作变量名、方法名、类名、包名。

Java 关键字有如下表所列，目前共有 50 个 Java 关键字，其中，"const" 和 "goto" 这两个关键字在 Java 语言中并没有具体含义。

![Java关键字](https://doc.shiyanlou.com/document-uid79144labid1048timestamp1434006344973.png)

## 变量练习
在 `/home/project/` 新建一个 `VarTest.java` 文件：

```java
public class VarTest
{
  public static void main(String[] args)
  {
    System.out.println("Define a variable a is ");
    int a; //声明变量a
    a = 5;
    System.out.println(a);  // 打印一个整数a
  }
}
```

编译运行：

```bash
$ javac VarTest.java
$ java VarTest
Define a variable a is
5
```

```checker
- name: check VarTest exist
  script: |
    #!/bin/bash
    ls -l /home/project/VarTest.java
  error: 没有找到 /home/project/VarTest.java 文件
```

## 常量

常量代表程序运行过程中不能改变的值。我们也可以把它们理解为特殊的变量，只是它们在程序的运行过程中是不允许改变的。**常量的值是不能被修改的**。

Java 中的 `final` 关键字可以用于声明属性（常量），方法和类。当 `final` 修饰属性时，代表该属性一旦被分配内存空间就必须初始化，它的含义是“这是无法改变的”或者“终态的”。在变量前面添加关键字 `final` 即可声明一个常量。在 Java 编码规范中，要求常量名必须大写。

语法格式：

```
final 数据类型 常量名 = 值;
```

例如：

```java
final double PI = 3.14;
```

常量也可以先声明，再进行赋值，但只能赋值一次，比如：

```java
final int FINAL_VARIABLE;
FINAL_VARIABLE = 100;
```

在 `/home/project/` 下新建一个 `FinalVar.java`：

```java
public class FinalVar{
    public static void main(String[] args){
        final String FINAL_STRING="shiyanlou";
        System.out.println(FINAL_STRING);
    }
}
```

编译运行：

```bash
$ javac FinalVar.java
$ java FinalVar
shiyanlou
```

```checker
- name: check FinalVar exist
  script: |
    #!/bin/bash
    ls -l /home/project/FinalVar.java
  error: 没有找到 /home/project/FinalVar.java 文件
```
