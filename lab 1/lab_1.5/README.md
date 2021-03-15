---
sort: 1.5
---


## String

Java 中使用 `String` 类来定义一个字符串，字符串是**常量**，它们的值在创建之后不能更改。字符串缓冲区支持可变的字符串。

`String` 对象的初始化格式有如下两种：

```java
String s0 = "abc";

String s1 = new String("abd");
```

String 类具有丰富的方法，比如计算字符串的长度、连接字符串、比较字符串、提取字符串等等。

#### 计算字符串长度

`length()` 方法：

```java
//方法原型
public int length(){
}
```

调用方法：`字符串标识符.length();`
返回一个 `int` 类型的整数（字符串中字符数，中文字符也是一个字符）。例如：

```java
String s1 = "abc";
String s2 = "Java语言";
int len1 = s1.length();
int len2 = s2.length();
```

则变量 `len1` 的值是 3，变量 `len2` 的值是 6。

#### 字符串比较

`equals()` 方法，该方法的作用是判断两个字符串对象的内容是否相同。如果相同则返回 `true`，否则返回 `false`。

`equals()` 方法比较是从第一字符开始，一个字符一个字符依次比较。

![equals比较原理](https://doc.shiyanlou.com/document-uid79144labid1085timestamp1435503766697.png)

如果想忽略掉大小写关系，比如：java 和 Java 是一样的，那怎么办呢？可以调用 `equalsIgnoreCase()` 方法，其用法与 `equals()` 一致，不过它会忽视大小写。

比如：

```java
public class StringTest {
	public static void main(String[] args){
		String s = new String("Java");
		String m = "java";
		System.out.println("用equals()比较，java和Java结果为"+s.equals(m));
		System.out.println("用equalsIgnoreCase()比较，java和Java结果为"+s.equalsIgnoreCase(m));
	}
}
```

编译运行：

```bash
$ javac StringTest.java
$ java StringTest
用equals()比较，java和Java结果为false
用equalsIgnoreCase()比较，java和Java结果为true
```

而使用 `"=="` 比较的是两个对象在内存中存储的地址是否一样。例如：

```java
         String s1 = "abc";
         String s2 = new String("abc");
         boolean b = (s1 == s2);
```

则变量 `b` 的值是 `false`，因为 `s1` 对象对应的地址是 `"abc"` 的地址，而 `s2` 使用 `new` 关键字申请新的内存，所以内存地址和 `s1` 的 `"abc"` 的地址不一样，所以获得的值是 `false`。

#### 字符串连接

字符串连接有两种方法：

1. 使用 `+`，比如 `String s = "Hello " + "World!"`。
2. 使用 `String` 类的 `concat()` 方法。

代码示例：

```java
String s0 = new String("Hello ");
String s1 = "World" + "!";   //+号连接
String s2 = s0.concat(s1); //concat()方法连接
System.out.println(s2);
```

而且使用 `+` 进行连接，不仅可以连接字符串，也可以连接其他类型。但是要求进行连接时至少有一个参与连接的内容是字符串类型。

#### charAt() 方法

`charAt()` 方法的作用是按照索引值（规定字符串中第一个字符的索引值是 0，第二个字符的索引值是 1，依次类推），获得字符串中的指定字符。例如：

```java
String s = "abc";
char c = s.charAt(1);
```

则变量 `c` 的值是 `'b'`。

#### 字符串常用提取方法

| 方法                                    | 返回值 | 功能描述                                     |
| --------------------------------------- | ------ | -------------------------------------------- |
| indexOf(char ch)                        | int    | 搜索字符 ch 第一次出现的索引                 |
| indexOf(String value)                   | int    | 搜索字符串 value 第一次出现的索引            |
| lastIndexOf(char ch)                    | int    | 搜索字符 ch 最后一次出现的索引               |
| lastIndexOf(String value)               | int    | 搜索字符串 value 最后一次出现的索引          |
| substring(int index)                    | String | 提取从位置索引开始到结束的字符串             |
| substring(int beginindex, int endindex) | String | 提取 beginindex 和 endindex 之间的字符串部分 |
| trim()                                  | String | 返回一个前后不含任何空格的调用字符串的副本   |

> 说明：在字符串中，第一个字符的索引为 0，子字符串包含 `beginindex` 的字符，但不包含 `endindex` 的字符。

来写一些代码，验证一下上面的方法吧。

```java
public class StringTest {
	public static void main(String[] args) {
		 String s = "abcdefabc";
		 System.out.println("字符a第一次出现的位置为"+s.indexOf('a'));
		 System.out.println("字符串bc第一次出现的位置为"+s.indexOf("bc"));
		 System.out.println("字符a最后一次出现的位置为"+s.lastIndexOf('a'));
		 System.out.println("从位置3开始到结束的字符串"+s.substring(3));
		 System.out.println("从位置3开始到6之间的字符串"+s.substring(3,6));
	}
}
```

编译运行：

```bash
$ javac StringTest.java
$ java StringTest
字符a第一次出现的位置为0
字符串bc第一次出现的位置为1
字符a最后一次出现的位置为6
从位置3开始到结束的字符串defabc
从位置3开始到6之间的字符串def
```

```checker
- name: 检查是否存在 StringTest
  script: |
    #!/bin/bash
    ls -l /home/project/StringTest.java
  error: |
    没有找到 /home/project/StringTest.java 文件
```
