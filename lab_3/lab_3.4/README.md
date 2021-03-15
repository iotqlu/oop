---
sort: 3.4
---



## 继承

继承可以看成是类与类之间的衍生关系。比如狗类是动物类，牧羊犬类又是狗类。于是我们可以说狗类继承了动物类，而牧羊犬类就继承了狗类。于是狗类就是动物类的子类（或派生类），动物类就是狗类的父类（或基类）。

所以继承需要符合的关系是：is-a，父类更通用，子类更具体。

语法：

```java
class 子类 extends 父类
```

例如我们定义了一个 Animal 类，再创建一个 Dog 类，我们需要它继承 Animal 类。

```java
class Dog extends Animal {
    ...
}
```

接下来我们就来练习一下吧！

我们先创建一个父类 `Animal.java`：

```java
public class Animal {
	public int legNum;     //动物四肢的数量

	//类方法
	public void bark() {
		System.out.println("动物叫！");
	}
}
```

接下来创建一个子类`Dog.java`

```java
public class Dog extends Animal {
}

```

Dog 类继承了父类 Animal，我们 Dog 类里什么都没有写，其实它继承了父类 Animal，所以 Dog 类拥有 Animal 类的全部方法和属性（除开 private 方法和属性）。我们创建一个测试类测试一下。

```java
public class Test{
    public static void main(String[] args) {
        Dog a = new Dog();
        a.legNum = 4;
        a.bark();
    }
}
```

编译运行：

```bash
$ javac Test.java Animal.java Dog.java
$ java Test
动物叫！
```

**为什么需要继承？**

如果有两个类相似，那么它们会有许多重复的代码，导致后果就是代码量大且臃肿，后期的维护性不高。通过继承就可以解决这个问题，将两段代码中相同的部分提取出来组成一个父类，实现代码的复用。

**继承的特点**：

- 子类拥有父类除 `private` 以外的所有属性和方法。
- 子类可以拥有自己的属性和方法。
- 子类可以重写实现父类的方法。
- Java 中的继承是单继承，一个类只有一个父类。

> 注：Java 实现多继承的一个办法是 `implements`（实现）接口，但接口不能有非静态的属性，这一点请注意。

```checker
- name: check Animal java exist
  script: |
    #!/bin/bash
    ls /home/project/Animal.java
  error: 我们发现您还没有创建Animal.java! /home/project/Animal.java
- name: check Dog java exist
  script: |
    #!/bin/bash
    ls /home/project/Dog.java
  error: 我们发现您还没有创建Dog.java! /home/project/Dog.java
```

## super

`super` 关键字在子类内部使用，代表父类对象。

1. 访问父类的属性 `super.属性名`。
2. 访问父类的方法 `super.bark()`。
3. 子类构造方法需要调用父类的构造方法时，在子类的构造方法体里**最前面**的位置：`super()`。

## 方法重载与重写

#### 方法重载

方法重载是指在一个类中定义多个同名的方法，但要求每个方法具有不同的参数的类型或参数的个数。方法重载一般用于创建一组任务相似但是参数不同的方法。

```java
public class Test {
    void f(int i) {
		System.out.println("i=" + i);
	}

	void f(float f) {
		System.out.println("f=" + f);
	}

	void f(String s) {
		System.out.println("s=" + s);
	}

	void f(String s1, String s2){
		System.out.println("s1+s2="+(s1+s2));
	}

	void f(String s, int i){
		System.out.println("s="+s+",i="+i);
	}

	public static void main(String[] args) {
		Test test = new Test();
		test.f(3456);
		test.f(34.56f);
		test.f("abc");
		test.f("abc","def");
		test.f("abc",3456);
	}
}
```

编译运行：

```bash
$ javac Test.java
$ java Test
i=3456
f=34.56
s=abc
s1+s2=abcdef
s=abc,i=3456
```

方法重载有以下几种规则：

- 方法中的参数列表必须不同。比如：参数个数不同或者参数类型不同。
- 重载的方法中允许抛出不同的异常
- 可以有不同的返回值类型，但是参数列表必须不同。
- 可以有不同的访问修饰符。

```checker
- name: check Test java exist
  script: |
    #!/bin/bash
    ls /home/project/Test.java
  error: 我们发现您还没有创建Test.java! /home/project/Test.java
  timeout: 1
```

#### 方法重写

子类可以继承父类的方法，但如果子类对父类的方法不满意，想在里面加入适合自己的一些操作时，就需要将方法进行重写。并且子类在调用方法中，优先调用子类的方法。

比如 `Animal` 类中有 `bark()` 这个方法代表了动物叫，但是不同的动物有不同的叫法，比如狗是汪汪汪，猫是喵喵喵。

当然在方法重写时要注意，重写的方法一定要与原父类的方法语法保持一致，比如返回值类型，参数类型及个数，和方法名都必须一致。

例如：

```java
public class Animal {
	//类方法
	public void bark() {
		System.out.println("动物叫！");
	}
}
```

```java
public class Dog extends Animal {
       //重写父类的bark方法
    	public void bark() {
		System.out.println("汪！汪！汪！");
	}
}
```

写个测试类来看看输出结果：

```java
public class Test{
	public static void main(String args[]){
   		Animal a = new Animal(); // Animal 对象
		Dog d = new Dog();   // Dog 对象

      	Animal b = new Dog(); // Dog 对象,向上转型为Animal类型，具体会在后面的内容进行详解

      	a.bark();// 执行 Animal 类的方法
 		d.bark();//执行 Dog 类的方法
      	b.bark();//执行 Dog 类的方法
   	}
}
```

编译运行：

```bash
$ javac Test.java Dog.java Animal.java
$ java Test
动物叫！
汪！汪！汪！
汪！汪！汪！
```
