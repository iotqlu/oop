---
sort: 2.3
---

## 用户输入操作

Java 可以使用 `java.util` 包下的`Scanner` 类来获取用户的输入。使用 `import java.util.Scanner;` 即可导入 Scanner，使用方法示例：

在 `/home/project` 目录下新建 `ScannerDemo.java` 类。

```java
import java.util.Scanner;

public class ScannerDemo {
    public static void main(String[] args) {
        Scanner in=new Scanner(System.in);
        //获取用户输入的一行数据  返回为字符串
        String s = in.nextLine();
        System.out.println(s);
        //返回用户输入的int值
        int i = in.nextInt();
        System.out.println(i);
        //循环读取String数据，当输入exit时退出循环
        while (!in.hasNext("exit")) {
            System.out.println(in.nextLine());
        }
        //关闭输入
        in.close();
    }
}

```

编译运行：

```java
javac ScannerDemo.java
java ScannerDemo
```

运行结果示例：

```shell
shiyanlou
shiyanlou
aa
aa
bbb
bbb
cc
cc
exit
```

除去以上列举的方法，其他方法可以在 API 文档中查询[https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html)。

```checker
- name: 检查是否存在 ScannerDemo
  script: |
    #!/bin/bash
    ls /home/project/ScannerDemo.java
  error: |
    没有找到 /home/project/ScannerDemo.java 文件
```

### 练习题：用户输入

在 `/home/project/` 目录下新建文件 `InputTest.java`，你需要完成以下需求：

- 获取用户的输入信息（字符串）。
- 当用户输入 end 时，结束输入并打印用户之前输入的所有信息（输入的信息数量不超过 100 个）。

示例：

```shell
输入：
    shi
    yan
    lou
    end
输出：
    shi
    yan
    lou
```

提示：

- 使用数组保存元素。

```checker
- name: 检查是否存在 InputTest
  script: |
    #!/bin/bash
    ls /home/project/InputTest.java
  error: |
    没有找到 /home/project/InputTest.java 文件
- name: 输出结果检测
  script: |
    #!/bin/bash
    javac /home/project/InputTest.java -d . ;echo -e 'shi\nyan\nlou\nend' |java InputTest |tr -d '\n'|grep -w shiyanlou
  error: |
    输出结果错误
  timeout: 20
```

### 参考答案

**注意：请务必先独立思考获得 PASS 之后再查看参考代码，直接拷贝代码收获不大**。

```java
import java.util.Scanner;

public class InputTest {
    public static void main(String[] args) {
        String[] data = new String[100];
        Scanner in = new Scanner(System.in);
        for (int i = 0; i < 100; i++) {
            if ((data[i] = in.nextLine()).equals("end")) {
                break;
            }
        }
        for (String a : data) {
            if (a.equals("end")) {
                break;
            }
            System.out.println(a);
        }
    }
}
```

## 练习题：最大最小值

现给出一串数据（313, 89, 123, 323, 313, 15, 90, 56, 39）求出最大值和最小值并输出。

在 `/home/project/` 目录下新建文件 `MaxAndMin.java`，在其中编写正确的代码。

```checker
- name: 检查是否存在 MaxAndMin
  script: |
    #!/bin/bash
    ls /home/project/MaxAndMin.java
  error: |
    没有找到 /home/project/MaxAndMin.java 文件
- name: 输出结果检测
  script: |
    #!/bin/bash
    javac /home/project/MaxAndMin.java -d . ;java MaxAndMin |tr -d '\n' | grep -E '15.*323|323.*15'
  error: |
    输出结果错误
  timeout: 20
```

### 参考答案

**注意：请务必先独立思考获得 PASS 之后再查看参考代码，直接拷贝代码收获不大**。

```java
import java.util.Arrays;

public class MaxAndMin {
    public static void main(String[] args) {
        int[] data = {313, 89, 123, 323, 313, 15, 90, 56, 39};
        //    方法1
        int max = data[0];
        int min = data[0];
        for (int i = 0; i < data.length; i++) {
            if (data[i] > max) {
                max = data[i];
            }
            if (data[i] < min) {
                min = data[i];
            }
        }
        System.out.println(min);
        System.out.println(max);
        //方法二
        //Arrays.sort(data);
        //System.out.println(data[0]);
        //System.out.println(data[data.length - 1]);
        //方法三
        //System.out.println(Arrays.stream(data).min().getAsInt());
        //System.out.println(Arrays.stream(data).max().getAsInt());
    }
}
```
