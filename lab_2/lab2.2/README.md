---
sort: 2.2
---

# 数组


## 数组

所谓数组，是有序的元素序列。若将有限个类型相同的变量的集合命名，那么这个名称为数组名。组成数组的各个变量称为数组的分量，也称为数组的元素，有时也称为下标变量。用于区分数组的各个元素的数字编号称为下标。数组是在程序设计中，为了处理方便，把具有相同类型的若干元素按无序的形式组织起来的一种形式。这些无序排列的同类数据元素的集合称为数组。数组是用于储存多个相同类型数据的集合。-- 来自百度百科

### 数组

数组就是相同数据类型的元素按一定顺序排列的集合。可以把它看成一个大的盒子，里面按顺序存放了多个数据类型相同的数据。

![数组的定义](https://doc.shiyanlou.com/document-uid79144labid1052timestamp1434356533170.png)

数组中的元素都可以通过下标来访问，**下标从 0 开始，到数组长度 -1 结束**。例如，可以通过 `ages[0]` 获取数组中的第一个元素 18 ，`ages[3]` 就可以取到第四个元素 10。

**注意**：

使用数组前要声明数组。

语法：

```java
数据类型[ ] 数组名;   //或者: 数据类型 数组名[ ];
```

数组名为任意合法的变量名，如：

```java
int ages[];      //存放年龄的数组，类型为整型
char symbol[];   //存放符号的数组，类型为字符型
String [] name;  //存放名称的数组，类型为字符串型
```

声明数组后，需要为数组分配空间，也就是定义多大的数组。

语法：

```java
数组名 = new  数据类型 [ 数组长度 ];
```

数组长度就是数组最多可存放元素的个数。可以在数组声明的时候初始化数组，或者在声明时就为它分配好空间，这样就不用再为数组分配空间。

语法：

```java
int [] ages = {12,18,9,33,45,60}; //声明并初始化了一个整型数组，它有6个元素
char [] symbol = new char[10] //声明并分配了一个长度为10的char型数组
```

分配空间后就可以向数组中放数据了，数组中元素都是通过下标来访问的。
如：

```java
ages[0]=12;
```

Java 中可以将一个数组赋值给另一个数组，如：

```java
int [] a1 = {1,2,3};
int [] a2;
a2 = a1;
```

这里只是复制了一个引用，即 a2 和 a1 是相同数组的不同名称。

在`/home/project/`下新建一个`Test.java`测试一下。

```java
public class Test {
	public static void main(String[] args) {
		int [] a1 = {1,2,3};
		int [] a2;
		a2 = a1;
		for(int i = 0; i < a2.length; i++){
			a2[i]++;
		}
		for(int i = 0; i < a1.length; i++){
			System.out.println(a1[i]);
		}
	}
}
```

编译输出：

```bash
$ javac Test.java
$ java Test
2
3
4
```

可以看到，修改 a2 的值，a1 的值也跟着变化。

```checker
- name: check Test java exist
  script: |
    #!/bin/bash
    ls /home/project/Test.java
  error: 我们发现您还没有创建Test.java! /home/project/Test.java
```

数组遍历：

```java
int [] ages = {12, 18, 9, 33, 45, 60};
for(int i = 0; i < ages.length; i++){ //ages.length是获取数组的长度
    System.out.println("数组中第"+(i+1)+"个元素是 "+ages[i]); //数组下标是从零开始，一定要注意
}
```

**注意**：

1. 数组下标从 0 开始。所以数组的下标范围是 0 至 数组长度 -1。
2. 数组不能越界访问，否则会报错。

for 语句在数组内可以使用特殊简化版本，在遍历数组、集合时，foreach 更简单便捷。从英文字面意思理解 foreach 也就是“ for 每一个”的意思。

语法：

```java
for(元素类型 元素变量:遍历对象){
    执行的代码
}
```

在`/home/project/`下新建`JudgePrime.java`

```java
public class JudgePrime {
	public static void main(String[] args){
		int [] ages = {12, 18, 9, 33, 45, 60};
		int i = 1;
		for(int age:ages){
		    System.out.println("数组中第"+i+"个元素是"+age);
		    i++;
		}
	}
}
```

编译运行：

```bash
$ javac JudgePrime.java
$ java JudgePrime
数组中第1个元素是12
数组中第2个元素是18
数组中第3个元素是9
数组中第4个元素是33
数组中第5个元素是45
数组中第6个元素是60
```

```checker
- name: check JudgePrime java exist
  script: |
    #!/bin/bash
    ls /home/project/JudgePrime.java
  error: 我们发现您还没有创建JudgePrime.java! /home/project/JudgePrime.java
  timeout: 20
```

### 二维数组

二维数组可以看成是一间有座位的教室，座位一般用第几排的第几个进行定位，每一个座位都有一个行和一个列的属性，一排的座位相当于一个一维数组，所以可以将二维数组简单的理解为是一种“特殊”的一维数组，它的每个数组空间中保存的是一个一维数组。

二维数组也需要声明和分配空间。

语法：

```java
数据类型 [][] 数组名 = new 数据类型[行的个数][列的个数];

//或者
数据类型 [][] 数组名;
数组名 = new 数据类型[行的个数][列的个数];

//也可以
数据类型 [][] 数组名 = {
{第一行值1,第一行值2,...}
{第二行值1,第二行值2,...}
...
}

//二维数组的赋值和访问，跟一维数组类似，可以通过下标来逐个赋值和访问，注意索引从 0 开始
数组名[行的索引][列的索引] = 值;
```

在`/home/project/`下新建`ArrayTest.java`

```java
public class ArrayTest {
    public static void main(String[] args) {
        String[][] name = { {"ZhaoYi", "QianEr", "SunSan"},
                {"LiSi", "ZhouWu", "WuLiu"} };
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < 3; j++) {
                System.out.println(name[i][j]);
            }
        }
    }
}
```

编译运行：

```bash
$ javac ArrayTest.java
$ java ArrayTest
ZhaoYi
QianEr
SunSan
LiSi
ZhouWu
WuLiu
```

```checker
- name: check ArrayTest exist
  script: |
    #!/bin/bash
    ls /home/project/ArrayTest.java
  error: 没有找到 /home/project/ArrayTest.java 文件
```

### 练习题：数组应用

有一份成绩单，上面有 10 位学生的成绩（61，57，95，85，75，65，44，66，90，32），请求出平均成绩并输出。

在`/home/project/`目录下新建文件`AverageScore.java`，并在其中编写正确的代码。

提示：

- 将 10 位同学的成绩保存在数组中

```checker
- name: 检查是否存在 AverageScore
  script: |
    #!/bin/bash
    ls /home/project/AverageScore.java
  error: |
    没有找到 /home/project/AverageScore.java 文件
- name: 输出结果检测
  script: |
    #!/bin/bash
    javac /home/project/AverageScore.java -d . ; java AverageScore |grep 67
  error: |
    输出结果错误！请检查算式或者数组值是否正确！
```

### 参考答案

**注意：请务必先独立思考获得 PASS 之后再查看参考代码，直接拷贝代码收获不大**

```java
public class AverageScore {
    public static void main(String[] args) {
        int[] data = {61, 57, 95, 85, 75, 65, 44, 66, 90, 32};
        int sum = 0;
        for (int i = 0; i < data.length; i++) {
            sum += data[i];
        }
        System.out.println("平均成绩：" + sum / data.length);
    }
}
```
