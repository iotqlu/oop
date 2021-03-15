---
sort: 3.3
---


## static

#### 静态成员

Java 中被 `static` 修饰的成员称为静态成员或类成员。它属于整个类所有，而不是某个对象所有，即被类的所有对象所共享。静态成员可以使用类名直接访问，也可以使用对象名进行访问。

如：

```java
public class StaticTest{
    public static String string="shiyanlou";
    public static void main(String[] args){
        //静态成员不需要实例化 直接就可以访问
        System.out.println(StaticTest.string);
        //如果不加static关键字 需要这样访问
        StaticTest staticTest=new StaticTest();
        System.out.println(staticTest.string);
        //如果加上static关键字，上面的两种方法都可以使用
    }
}

```

#### 静态方法

被 `static` 修饰的方法是静态方法，静态方法不依赖于对象，不需要将类实例化便可以调用，由于不实例化也可以调用，所以不能有 `this`，也不能访问非静态成员变量和非静态方法。但是非静态成员变量和非静态方法可以访问静态方法。

## final

`final` 关键字可以修饰类、方法、属性和变量

1. `final` 修饰类，则该类不允许被继承，为最终类
2. `final` 修饰方法，则该方法不允许被覆盖（重写）
3. `final` 修饰属性：则该类的属性不会进行隐式的初始化（类的初始化属性必须有值）或在构造方法中赋值（但只能选其一）
4. final 修饰变量，则该变量的值只能赋一次值，即常量

如：

```java
//静态常量
public final static String SHI_YAN_LOU="shiyanlou";
```

## 权限修饰符

代码中经常用到 `private` 和 `public` 修饰符，权限修饰符可以用来修饰属性和方法的访问范围。

![访问范围](https://doc.shiyanlou.com/document-uid79144labid1072timestamp1434941168916.png)

如图所示，代表了不同的访问修饰符的访问范围，比如 `private` 修饰的属性或者方法，只能在当前类中访问或者使用。`默认` 是什么修饰符都不加，默认在当前类中和同一包下都可以访问和使用。`protected` 修饰的属性或者方法，对同一包内的类和所有子类可见。`public` 修饰的属性或者方法，对所有类可见。

我们可以举一个例子，比如 `money`，如果我们用 `private` 修饰代表着这是私有的，只能我自己可以使用。如果是 `protected` 代表着我可以使用，和我有关系的人，比如儿子也可以用。如果是 `public` 就代表了所有人都可以使用。

## 封装

> 封装，即隐藏对象的属性和实现细节，仅对外公开接口，控制在程序中属性的读和修改的访问级别

这样做有什么好处？

1. 只能通过规定的方法访问数据。
2. 隐藏类的实例细节，方便修改和实现。

我们在开汽车的时候，只用去关注如何开车，我们并不在意车子是如何实现的，这就是封装。

如何去实现类的封装呢？

1. 修改属性的可见性，在属性的前面添加修饰符 (`private`)
2. 对每个值属性提供对外的公共方法访问，如创建 getter/setter（取值和赋值）方法，用于对私有属性的访问
3. 在 getter/setter 方法里加入属性的控制语句，例如我们可以加一个判断语句，对于非法输入给予否定。

![封装的步骤](https://doc.shiyanlou.com/document-uid79144labid1072timestamp1434933673483.png)

如果我们没有在属性前面添加任何修饰符，我们通过对象就可以直接对属性值进行修改，没有体现封装的特性。这在许多程序设计中都是不安全的，所以我们需要利用封装，来改进我们的代码。

首先在类里要将属性前添加 `private` 修饰符。然后定义 `getter` 和 `setter` 方法。修改 `People.java` 和 `NewObject.java` 的内容如下。

```java
public class People {
    //属性（成员变量）有什么，前面添加了访问修饰符private
    //变成了私有属性，必须通过方法调用
    private double height;     //身高

    //属性已经封装好了，如果用户需要调用属性
    //必须用getter和setter方法进行调用
    //getter和setter方法需要程序员自己定义
    public double getHeight(){
    //getter 方法命名是get关键字加属性名（属性名首字母大写）
    //getter 方法一般是为了得到属性值
      return height;
    }

    //同理设置我们的setter方法
    //setter 方法命名是set关键字加属性名（首字母大写）
    //setter 方法一般是给属性值赋值，所以有一个参数
    public void setHeight(double newHeight){
      height = newHeight;
    }
}

```

现在 `main` 函数里的对象，不能再直接调用属性了，只能通过 `getter` 和 `setter` 方法进行调用。

```java
public class NewObject {

	public static void main(String[] args) {
		People LiLei = new People();    //创建了一个People对象LiLei

		//利用setter方法为属性赋值
		LiLei.setHeight(170.0);

		//利用getter方法取属性值
		System.out.println("LiLei的身高是"+LiLei.getHeight());
	}
}

```

编译运行：

```shell
javac NewObject.java People.java
java NewObject.java
```

编译运行：

```bash
$ javac NewObject.java People.java
$ java NewObject
LiLei的身高是170.0
```

```checker
- name: check People java exist
  script: |
    #!/bin/bash
    ls /home/project/People.java
  error: 我们发现您还没有创建People.java! /home/project/People.java
  timeout: 1
- name: check NewObject java exist
  script: |
    #!/bin/bash
    ls /home/project/NewObject.java
  error: 我们发现您还没有创建NewObject.java! /home/project/NewObject.java
  timeout: 1

```

## this

`this` 关键字代表当前对象。使用 `this.属性` 操作当前对象的属性，`this.方法` 调用当前对象的方法。

用 `private` 修饰的属性，必须定义 getter 和 setter 方法才可以访问到 (Eclipse 和 IDEA 等 IDE 都有自动生成 getter 和 setter 方法的功能）。

如下：

```java
public void setAge(int age) {
  this.age = age;
}
public int getAge() {
  return age;
}
```

创建好了 getter 和 setter 方法后，我们发现方法中参数名和属性名一样。

当成员变量和局部变量之间发生冲突时，在属性名前面添加了 `this` 关键字。
此时就代表将一个参数的值赋给当前对象的属性。同理 `this` 关键字可以调用当前对象的方法，同学们自行验证一下吧。
