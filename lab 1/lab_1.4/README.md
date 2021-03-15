---
sort: 1.4
---

## 方法

Java 中的方法，可以将其看成一个功能的集合，它们是为了解决特定问题的代码组合。

方法的定义语法：

```java
访问修饰符 返回值类型 方法名(参数列表) {
    方法体
}
```

比如：

```java
public void functionName(Object arg) {
		System.out.println("Hello World.");
}
```

在上面的语法说明中：

1. **访问修饰符**：代表方法允许被访问的权限范围， 可以是 `public`、`protected`、`private` 或者省略（`default`） ，其中 `public` 表示该方法可以被其他任何代码调用。

2. **返回值类型**：方法返回值的类型，如果方法不返回任何值，则返回值类型指定为 `void` （代表无类型）；如果方法具有返回值，则需要指定返回值的类型，并且在方法体中使用 `return` 语句返回值。

3. **方法名**：是方法的名字，必须使用合法的标识符。

4. **参数列表**：是传递给方法的参数列表，参数可以有多个，多个参数间以逗号隔开，每个参数由参数类型和参数名组成，以空格隔开。当方法被调用时，传递值给参数。这个值被称为实参或变量。参数列表是指方法的参数类型、顺序和参数的个数。参数是可选的，方法可以不包含任何参数。

5. **方法体**：方法体包含具体的语句，定义该方法的功能。

根据方法是否带参、是否带返回值，可将方法分为四类：

- 无参无返回值方法
- 无参带返回值方法
- 带参无返回值方法
- 带参带返回值方法

当方法定义好之后，需要调用才可以生效，我们可以通过 `main` 方法（`main` 方法是 Java 程序的入口，所以需要用它来调用）来调用它，比如：

在 `/home/project` 下建立 `MethodDemo.java`：

```java
public class MethodDemo {
    public static void main(String[] args){
        method();
    }
    //这里要加上static关键字 应为静态方法只能调用静态方法
    public static void method(){
        System.out.println("方法被调用");
    }
}
```

编译运行：

```bash
javac MethodDemo.java
java MethodDemo
方法被调用
```

```checker
- name: 检查是否存在 MethodDemo
  script: |
    #!/bin/bash
    ls -l /home/project/MethodDemo.java
  error: |
    没有找到 /home/project/MethodDemo.java 文件
```

### 练习题：方法使用

在 `/home/project/` 目录下新建文件 `MethodTest.java`，在其中新建一个方法 `methodDemo`，运行该方法，在控制台输出 `Hello Shiyanlou`。

```checker
- name: 检查是否存在 MethodTest
  script: |
    #!/bin/bash
    ls -l /home/project/MethodTest.java
  error: |
    没有找到 /home/project/MethodTest.java 文件
- name: 输出结果检测
  script: |
    #!/bin/bash
    javac /home/project/MethodTest.java -d . ;java MethodTest | grep 'Hello Shiyanlou'
  error: |
    输出结果错误
```

### 参考答案

**注意：请务必先独立思考获得 PASS 之后再查看参考代码，直接拷贝代码收获不大**。

```java
public class MethodTest {
    private static void methodDemo() {
        System.out.println("Hello Shiyanlou");
    }

    public static void main(String[] args) {
        methodDemo();
    }
}
```


