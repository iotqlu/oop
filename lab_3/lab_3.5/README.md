---
sort: 3.5
---


## 多态

多态是指允许不同类的对象对同一消息做出响应。即同一消息可以根据发送对象的不同而采用多种不同的行为方式。多态也称作动态绑定（dynamic binding），是指在执行期间判断所引用对象的实际类型，根据其实际的类型调用其相应的方法。

通俗地讲，只通过父类就能够引用不同的子类，这就是多态，我们只有在运行的时候才会知道引用变量所指向的具体实例对象。

#### 多态的实现条件

**Java 实现多态有三个必要条件：继承、重写和向上转型（即父类引用指向子类对象）。**

只有满足上述三个条件，才能够在同一个继承结构中使用统一的逻辑实现代码处理不同的对象，从而达到执行不同的行为。

#### 多态的实现方式

Java 中多态的实现方式：继承父类进行方法重写，抽象类和抽象方法，接口实现。

### 向上转型

要理解多态必须要明白什么是"向上转型"，比如，一段代码如下，Dog 类是 Animal 类的子类：

```java
Animal a = new Animal();  //a是父类的引用指向的是本类的对象

Animal b = new Dog(); //b是父类的引用指向的是子类的对象
```

在这里，可以认为由于 Dog 继承于 Animal，所以 Dog 可以自动向上转型为 Animal，所以 `b` 是可以指向 Dog 实例对象的。

> 注：不能使用一个子类的引用去指向父类的对象，因为子类对象中可能会含有父类对象中所没有的属性和方法。

如果定义了一个指向子类对象的父类引用类型，那么它除了能够引用父类中定义的所有属性和方法外，还可以使用子类强大的功能。但是对于只存在于子类的方法和属性就不能获取。

新建一个 `Test.java`，例如：

```java
class Animal {
	//父类方法
	public void bark() {
		System.out.println("动物叫！");
	}
}

class Dog extends Animal {

    //子类重写父类的bark方法
	public void bark() {
		System.out.println("汪、汪、汪！");
	}
	//子类自己的方法
	public void dogType() {
		System.out.println("这是什么品种的狗？");
	}
}


public class Test {

	public static void main(String[] args) {
		Animal a = new Animal();
		Animal b = new Dog();
		Dog d = new Dog();

		a.bark();
		b.bark();
		//b.dogType();
		//b.dogType()编译不通过
		d.bark();
		d.dogType();
	}

}
```

编译运行：

```bash
$ javac Test.java
$ java Test
动物叫！
汪、汪、汪！
汪、汪、汪！
这是什么品种的狗？
```

在这里，由于 `b` 是父类的引用，指向子类的对象，因此不能获取子类的方法（`dogType()` 方法）, 同时当调用 `bark()` 方法时，由于子类重写了父类的 `bark()` 方法，所以调用子类中的 `bark()` 方法。

**因此，向上转型，在运行时，会遗忘子类对象中与父类对象中不同的方法，也会覆盖与父类中相同的方法——重写（方法名，参数都相同）。**

```checker
- name: check Test java exist
  script: |
    #!/bin/bash
    ls /home/project/Test.java
  error: 我们发现您还没有创建Test.java! /home/project/Test.java
  timeout: 1
```

## 抽象类

在定义类时，前面加上 `abstract` 关键字修饰的类叫抽象类。

抽象类中有抽象方法，这种方法是不完整的，仅有声明而没有方法体。抽象方法声明语法如下：

```java
abstract void f();  //f()方法是抽象方法
```

那我们什么时候会用到抽象类呢？

1. 在某些情况下，某个父类只是知道其子类应该包含怎样的方法，但无法准确知道这些子类如何实现这些方法。也就是说抽象类是约束子类必须要实现哪些方法，而并不关注方法如何去实现。
2. 从多个具有相同特征的类中抽象出一个抽象类，以这个抽象类作为子类的模板，从而避免了子类设计的随意性。

所以由上可知，抽象类是限制规定子类必须实现某些方法，但不关注实现细节。

那抽象类如何用代码实现呢，它的规则如下：

1. 用 `abstract` 修饰符定义抽象类。
2. 用 `abstract` 修饰符定义抽象方法，只用声明，不需要实现。
3. 包含抽象方法的类就是抽象类。
4. 抽象类中可以包含普通的方法，也可以没有抽象方法。
5. 抽象类的对象不能直接创建，通常是定义引用变量指向子类对象。

我们来写一写代码吧！

1、在 `home/project/` 目录下创建一个抽象类 `TelePhone.java`。

2、填写需要子类实现的抽象方法。

```java
//抽象方法
public abstract class TelePhone {
	public abstract void call();  //抽象方法,打电话
	public abstract void message(); //抽象方法，发短信
}
```

3、构建子类，并实现抽象方法。新建一个 `CellPhone.java`。

```java
public class CellPhone extends TelePhone {

	@Override
	public void call() {
		System.out.println("我可以打电话！");
	}

	@Override
	public void message() {
        System.out.println("我可以发短信！");
	}

	public static void main(String[] args) {
		CellPhone cp = new CellPhone();
		cp.call();
		cp.message();
	}

}
```

在 CellPhone 类添加 `main` 方法测试运行结果。

编译运行：

```bash
$ javac CellPhone.java TelePhone.java
$ java CellPhone

我可以打电话！
我可以发短信！
```

```checker
- name: check TelePhone java exist
  script: |
    #!/bin/bash
    ls /home/project/TelePhone.java
  error: 我们发现您还没有创建TelePhone.java! /home/project/TelePhone.java
  timeout: 1
- name: check CellPhone java exist
  script: |
    #!/bin/bash
    ls /home/project/CellPhone.java
  error: 我们发现您还没有创建CellPhone.java! /home/project/CellPhone.java
  timeout: 1
```

## 接口

接口用于描述类所具有的功能，而不提供功能的实现，功能的实现需要写在实现接口的类中，并且该类必须实现接口中所有的未实现方法。

接口的声明语法格式如下：

```java
修饰符 interface 接口名称 [extends 其他的接口名] {
        // 声明变量
        // 抽象方法
}
```

如声明一个 Animal 接口：

```java
// Animal.java
interface Animal {
		//int x;
		//编译错误,x需要初始化，因为是 static final 类型
		int y = 5;
		public void eat();
		public void travel();
}
```

**注意点**

在 Java8 中：

- 接口不能用于实例化对象。
- 接口中方法只能是抽象方法、`default` 方法、静态方法。
- 接口成员是 `static final` 类型。
- 接口支持多继承。

在 Java9 中，接口可以拥有私有方法和私有静态方法，但是只能被该接口中的 `default` 方法和静态方法使用。

多继承实现方式：

```java
修饰符 interface A extends 接口1，接口2{

}

修饰符 class A implements 接口1，接口2{

}
```

实现上面的接口：

```java
// Cat.java
public class Cat implements Animal{

	 public void eat(){
		 System.out.println("Cat eats");
	 }

	 public void travel(){
	     System.out.println("Cat travels");
	 }
	 public static void main(String[] args) {
		Cat cat = new Cat();
		cat.eat();
		cat.travel();
	}
}
```

编译运行：

```bash
$ javac Cat.java Animal.java
$ java Cat
Cat eats
Cat travels
```

```checker
- name: check Cat java exist
  script: |
    #!/bin/bash
    ls /home/project/Cat.java
  error: 我们发现您还没有创建Cat.java! /home/project/Cat.java
  timeout: 1
```

