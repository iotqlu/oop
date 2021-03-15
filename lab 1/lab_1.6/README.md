---
sort: 1.6
---


# Java关键字
在所有程序中有特殊意义的文字标记，称为关键字。

## 定义类、接口、抽象类

- class	声明一个类
- interface	接口
- abstract	表明类或者成员方法具有抽象属性

## 用于建立类与类之间关系

- implements	表明一个类实现了给定的接口类
- extends	表明一个类型是另一个类型的子类型，常见的类型有类和接口

## 用于定义访问权限修饰符

- private	私有权限，修饰的属性和方法仅供本类引用
- protected	保护权限，保护子类，当前包内和继承的子类中可以引用
- default	默认模式，不写任何访问修饰权限，本包内可以使用
- public	公用模式，可跨包使用，凡是环境下的类和方法都可以使用，需导入包

## 用于定义建立实例及引用实例、判断实例

- new	用来创建新的实例对象
- this	指向当前实例对象的引用
- super	表明当前对象的父类型的引用或者父类型的构造方法
- instanceof	用来测试一个对象是否是指定类型的实例对象

## 用于定义类、函数、变量修饰符

- final	终结器，表明一个类不能派生出子类，或者成员方法不能被覆盖，或者成员域的值不能被改变，用来定义常量
- static	表示具有静态属性
- synchronized	线程同步，修饰一段代码表示多个线程都能同步执行
- volatile	意识，表明两个或者多个变量必须同步地发生变化
- native	本地用来声明一个方法是由计算机相关语言实现的(如C/C++语言等)

## 用于异常处理

- try	尝试一个可能抛出异常的程序块
- catch	用在异常处理中，用来捕捉异常
- finally	用于异常处理情况，用来声明一个基本肯定会被执行到的语句块（有没有异常都执行）
- throw	通常用在方法体中，并且抛出一个异常对象，程序在执行到throw语句时立即停止，它后面的语句都不执行。
- throws	如果一个方法可以引发异常，本身不对异常进行处理，将异常抛给调用者使程序可以继续执行下去

## 用于包的关键字

- import	导入这个类所存在的包
- package	定义包的关键字，将有关类放在一个包中

## 其他修饰符关键字

- assert	断言，用来进行程序调试

## 说明

- Java中有两个未使用的保留字：goto、const。
- Java中有三个特殊含义的单词：null、true、false。
- JDK1.4后追加了assert关键字；JDK1.5以后追加了enum关键字。
————————————————
版权声明：本文为CSDN博主「meng_lemon」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/meng_lemon/article/details/86534017