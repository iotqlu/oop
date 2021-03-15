---
sort: 1.3
---
# 数据类型与运算符
## 数据类型

#### 数据类型

Java 中一共八种基本数据类型，下表列出了基本数据类型的数据范围、存储格式、默认值和包装类型等。

| 数据类型 | 默认值         | 存储格式 | 数据范围                                                         | 包装类型  |
| -------- | -------------- | -------- | ---------------------------------------------------------------- | --------- |
| short    | 0              | 2 个字节 | -32,768 到 32,767                                                | Short     |
| int      | 0              | 4 个字节 | -2,147,483,648 到 2,147,483,647                                  | Integer   |
| byte     | 0              | 1 个字节 | -128 到 127                                                      | Byte      |
| char     | 空             | 2 个字节 | Unicode 的字符范围：`\u0000`（即为 0）到 `\uffff`（即为 65,535） | Character |
| long     | 0L 或 0l       | 8 个字节 | -9,223,372,036,854,775,808 到 9,223,372,036,854,775,807          | Long      |
| float    | 0.0F 或 0.0f   | 4 个字节 | 32 位 IEEEE-754 单精度范围                                       | Float     |
| double   | 0.0 或 0.0D(d) | 8 个字节 | 64 位 IEEE-754 双精度范围                                        | Double    |
| boolean  | false          | 1 位     | true 或 false                                                    | Boolean   |

#### 整数

`byte`、`short`、`int`、`long` 四种基本数据类型表示整数，需要注意的是 `long` 类型，使用 `long` 修饰的变量需要在数值后面加上 L 或者 l，比如 `long num = 1L;`，一般使用大写 `L`，为了避免小写 `l` 与数值 `1` 混淆。

#### 浮点数

`float` 和 `double` 类型表示浮点数，即可以表示小数部分。需要注意的是 `float` 类型的数值后面需要加上 `F` 或者 `f`，否则会被当成 `double` 类型处理。`double` 类型的数值可以加上 `D` 或 `d`，也可以不加。

#### char 类型

char 类型用于表示单个字符。需要将字符用单引号括起来`char a = 'a'`，char 可以和整数互相转换，如果字符 `a` 也可以写成`char a = 97`。也可以用十六进制表示`char a = '\u0061'`。

#### boolean 类型

`boolean` 类型（布尔类型）用于表示真值 `true`或者假值 `false`，Java 中布尔值不能和整数类型或者其它类型互相转换。

## 运算符

运算符顾名思义是一种符号，它是连接一个以上的操作符，实现某种功能的运算。

### 算术运算符

算术运算符用在数学表达式中，主要实现的是算术运算，如常见的*加减乘除*等。

表格中的例子中，变量 `a` 的值为 5，变量 `b` 的值为 3，变量 `i` 的值为 1：

| 算术运算符 | 名称 | 描述                     | 类型       | 举例                 |
| ---------- | ---- | ------------------------ | ---------- | -------------------- |
| +          | 加法 | 相加运算符两侧的值       | 双目运算符 | a + b 等于 8         |
| -          | 减法 | 左操作数减去右操作数     | 双目运算符 | a - b 等于 2         |
| \*         | 乘法 | 相乘操作符两侧的值       | 双目运算符 | a \* b 等于 15       |
| /          | 除法 | 左操作数除以右操作数     | 双目运算符 | a / b 等于 1         |
| %          | 取余 | 左操作数除右操作数的余数 | 双目运算符 | a % b 等于 2         |
| ++         | 自增 | 操作数的值增加 1         | 单目运算符 | ++i（或 i++） 等于 2 |
| --         | 自减 | 操作数的值减少 1         | 单目运算符 | --i（或 i--） 等于 0 |

其中，自增 (++) 和自减 (--) 运算符有两种写法：**前缀（++i,--i）**和**后缀（i++,i--）**。

- 前缀自增自减法 (++i,--i): 先进行自增或者自减运算，再进行表达式运算。
- 后缀自增自减法 (i++,i--): 先进行表达式运算，再进行自增或者自减运算

新建一个源代码文件 `ArithmeticOperation.java`：

```java
public class ArithmeticOperation {
	public static void main(String args[]) {
		int a = 5;
		int b = 3;
		int c = 3;
		int d = 3;
		System.out.println("a + b = " + (a + b));
		System.out.println("a - b = " + (a - b));
		System.out.println("a * b = " + (a * b));
		System.out.println("a / b = " + (a / b));
		System.out.println("a % b = " + (a % b));
		System.out.println("a++ = " + (a++));
		System.out.println("++a = " + (++a));
		System.out.println("b-- = " + (b--));
		System.out.println("--b = " + (--b));
		System.out.println("c++ = " + (c++));
		System.out.println("++d = " + (++d));
	}
}

```

编译运行：

```bash
$ javac ArithmeticOperation.java
$ java ArithmeticOperation
a + b = 8
a - b = 2
a * b = 15
a / b = 1
a % b = 2
a++ = 5
++a = 7
b-- = 3
--b = 1
c++ = 3
++d = 4
```

```checker
- name: check ArithmeticOperation java exist
  script: |
    #!/bin/bash
    ls -l /home/project/ArithmeticOperation.java
  error: 我们发现您还没有创建ArithmeticOperation.java! /home/project/ArithmeticOperation.java
  timeout: 1
```

### 位运算符

Java 定义了位运算符，应用于整数类型 (`int`)，长整型 (`long`)，短整型 (`short`)，字符型 (`char`)，和字节型 (`byte`) 等类型。位运算时先转换为二进制，再按位运算。

表格中的例子中，变量 `a` 的值为 60（二进制：`00111100`），变量 `b` 的值为 13（二进制：`00001101`）：

| 位运算符 | 名称         | 描述                                                         | 举例                              |
| -------- | ------------ | ------------------------------------------------------------ | --------------------------------- |
| &        | 按位与       | 如果相对应位都是 1，则结果为 1，否则为 0                     | （a＆b），得到 12，即 0000 1100   |
| 丨       | 按位或       | 如果相对应位都是 0，则结果为 0，否则为 1                     | （ a 丨 b ）得到 61，即 0011 1101 |
| ^        | 按位异或     | 如果相对应位值相同，则结果为 0，否则为 1                     | （a^b）得到 49，即 0011 0001      |
| ~        | 按位补       | 翻转操作数的每一位，即 0 变成 1，1 变成 0                    | （〜a）得到 -61，即 1100 0011     |
| <<       | 按位左移     | 左操作数按位左移右操作数指定的位数                           | a<<2 得到 240，即 1111 0000       |
| >>       | 按位右移     | 左操作数按位右移右操作数指定的位数                           | a>>2 得到 15 即 1111              |
| >>>      | 按位右移补零 | 左操作数的值按右操作数指定的位数右移，移动得到的空位以零填充 | a>>>2 得到 15 即 0000 1111        |

在 `/home/project` 目录下新建一个源代码文件 `BitOperation.java`：

```java
public class BitOperation {
	public static void main(String args[]) {
		int a = 60;
		int b = 13;
		System.out.println("a & b = " + (a & b));
		System.out.println("a | b = " + (a | b));
		System.out.println("a ^ b = " + (a ^ b));
		System.out.println("~a = " + (~a));
		System.out.println("a << 2 = " + (a << 2));
		System.out.println("a >> 2 = " + (a >> 2));
		System.out.println("a >>> 2 = " + (a >>> 2));
	}
}
```

编译运行：

```bash
$ javac BitOperation.java
$ java BitOperation
a & b = 12
a | b = 61
a ^ b = 49
~a = -61
a << 2 = 240
a >> 2 = 15
a >>> 2 = 15
```

```checker
- name: check BitOperation java exist
  script: |
    #!/bin/bash
    ls -l /home/project/BitOperation.java
  error: 我们发现您还没有创建BitOperation.java! /home/project/BitOperation.java
  timeout: 1
```

### 逻辑运算符

逻辑运算符是通过运算符将操作数或等式进行逻辑判断的语句。

表格中的例子中，假设布尔变量 `a` 为真（`true`），变量 `b` 为假（`true`）：

| 逻辑运算符 | 名称 | 描述                                                           | 类型       | 举例                         |
| ---------- | ---- | -------------------------------------------------------------- | ---------- | ---------------------------- |
| && 或 &    | 与   | 当且仅当两个操作数都为真，条件才为真                           | 双目运算符 | (a && b) 或 (a & b) 为假     |
| \|\| 或 \| | 或   | 两个操作数任何一个为真，条件为真                               | 双目运算符 | （a \|\| b) 或 (a \| b) 为真 |
| !          | 非   | 用来反转操作数的逻辑状态。如果条件为真，则逻辑非运算符将得到假 | 单目运算符 | （!a）为假                   |
| ^          | 异或 | 如果两个操作数逻辑相同，则结果为假，否则为真                   | 双目运算符 | (a ^ b) 为真                 |

`&&` 与 `||` 是具有短路性质，当按优先级顺序计算到当前表达式时，表达式的结果可以确定整个表达式的结果时，便不会继续向后进行判断和计算，而直接返回结果。

例如：当使用 `&&` 逻辑运算符时，在两个操作数都为 `true` 时，结果才为 `true`，但是当得到第一个操作为 `false` 时，其结果就必定是 `false`，这时候就不会再判断第二个操作了。在计算表达式 `(a | b) && (a & a)` 时，首先计算 `a | b` 得到了 `false`，因为之后是 `&&`，任何值与 `false` 进行与操作都是 `false`，所以可以不用再计算下去，而直接返回 `a | b` 的结果 `false`。

在`/home/project`目录下新建一个`LogicOperation.java`。

```java
public class LogicOperation {
	public static void main(String args[]) {
		boolean a = true;
		boolean b = false;
		System.out.println("a && b = " + (a && b));
		System.out.println("a || b = " + (a || b));
		System.out.println("!a = " + (!a));
		System.out.println("a ^ b = " + (a ^ b));
	}
}
```

编译运行：

```bash
$ javac LogicOperation.java
$ java LogicOperation
a && b = false
a || b = true
!a = false
a ^ b = true
```

```checker
- name: check LogicOperation java exist
  script: |
    #!/bin/bash
    ls -l /home/project/LogicOperation.java
  error: 我们发现您还没有创建LogicOperation.java! /home/project/LogicOperation.java
  timeout: 1
```

### 关系运算符

关系运算符生成的是一个 `boolean`（布尔）结果，它们计算的是操作数的值之间的关系。如果关系是真实的，结果为 `true`（真），否则，结果为 `false`（假）。

表格中的例子中，假设变量 `a` 为 3，变量 `b` 为 5：

| 比较运算符 | 名称     | 描述                                                           | 举例               |
| ---------- | -------- | -------------------------------------------------------------- | ------------------ |
| ==         | 等于     | 判断两个操作数的值是否相等，如果相等则条件为真                 | (a == b） 为 false |
| !=         | 不等于   | 判断两个操作数的值是否相等，如果值不相等则条件为真             | (a != b) 为 true   |
| >          | 大于     | 判断左操作数的值是否大于右操作数的值，如果是那么条件为真       | (a > b) 为 false   |
| <          | 小于     | 判断左操作数的值是否小于右操作数的值，如果是那么条件为真       | (a < b) 为 true    |
| >=         | 大于等于 | 判断左操作数的值是否大于或等于右操作数的值，如果是那么条件为真 | (a >= b) 为 false  |
| <=         | 小于等于 | 判断左操作数的值是否小于或等于右操作数的值，如果是那么条件为真 | (a <= b) 为 true   |

除了上表列出的二元运算符，Java 还有唯一的一个三目运算符 `?:` 。

语法格式：

```
布尔表达式 ？表达式 1 : 表达式 2;
```

运算过程：如果布尔表达式的值为 `true`，则返回**表达式 1**的值，否则返回**表达式 2**的值。

在 `/home/project` 目录下新建一个源代码文件 `RelationalOperation.java`：

```java
public class RelationalOperation {
    public static void main(String args[]) {
        int a = 3;
        int b = 5;
        System.out.println("a == b = " + (a == b));
        System.out.println("a != b = " + (a != b));
        System.out.println("a > b = " + (a > b));
        System.out.println("a < b = " + (a < b));
        System.out.println("a >= b = " + (a >= b));
        System.out.println("a <= b = " + (a <= b));
        System.out.println("a > b ? a : b = " + (a > b ? a : b));
    }
}
```

编译运行：

```bash
$ javac RelationalOperation.java
$ java RelationalOperation
a == b = false
a != b = true
a > b = false
a < b = true
a >= b = false
a <= b = true
a > b ? a : b = 5
```

**强调**：

- `==` 和 `!=` 适用于所有的基本数据类型，其他关系运算符不适用于 `boolean`，因为 `boolean` 值只有 `true` 和 `false`，比较没有任何意义。
- `==` 和 `!=` 也适用于所有对象，可以比较对象的**引用**是否相同。

**引用：Java 中一切都是对象，但操作的标识符实际是对象的一个引用。**

```checker
- name: check RelationalOperation java exist
  script: |
    #!/bin/bash
    ls -l /home/project/RelationalOperation.java
  error: 没有找到 /home/project/RelationalOperation.java 文件！
```

### 运算符优先级

运算符的优先级是帮助我们在一个表达式中如何对于不同的运算符和相同的运算符，进行正确的运算顺序。

运算符的优先级不需要特别地去记忆它，比较复杂的表达式一般使用圆括号 `()` 分开，提高可读性。

!![运算符的优先级](https://doc.shiyanlou.com/document-uid79144labid1050timestamp1434082078141.png)

![运算符的优先级2](https://doc.shiyanlou.com/document-uid79144labid1050timestamp1434082102195.png)

### 练习：计算数字和

在 `/home/project/` 目录下新建文件 `Sum.java`，你需要实现以下需求：

- 获取控制台输入的两个整型参数。
- 输出两个整型参数和。

比如输入 3 和 4 对应输出 7。

示例：

```
输入：
    3
    4
输出：
    7
```

提示：`java.util.Scanner` 可以获取控制台输入。

```java
Scanner in =new Scanner(System.in);
//获取int值
int x1=in.nextInt();
int x2=in.nextInt();
```

```checker
- name: 检查是否存在 Sum
  script: |
    #!/bin/bash
    ls -l /home/project/Sum.java
  error: |
    没有找到 /home/project/Sum.java 文件
- name: 输出结果检测
  script: |
    #!/bin/bash
    javac /home/project/Sum.java -d . ;echo 7  8 |java Sum |grep 15
  error: |
    输出结果错误
  timeout: 20
```

### 参考答案

**注意：请务必先独立思考获得 PASS 之后再查看参考代码，直接拷贝代码收获不大**。

```java
import java.util.Scanner;

public class Sum {
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        int a = in.nextInt();
        int b = in.nextInt();
        System.out.println(a + b);
    }
}
```
