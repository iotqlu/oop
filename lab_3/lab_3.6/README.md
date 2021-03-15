---
sort: 3.6
---


## 内部类

将一个类的定义放在另一个类的定义内部，这就是内部类。而包含内部类的类被称为外部类。

内部类的主要作用如下：

1. 内部类提供了更好的封装，可以把内部类隐藏在外部类之内，不允许同一个包中的其他类访问该类
2. 内部类的方法可以直接访问外部类的所有数据，包括私有的数据
3. 内部类所实现的功能使用外部类同样可以实现，只是有时使用内部类更方便
4. 内部类允许继承多个非接口类型（具体将在以后的内容进行讲解）

> 注：内部类是一个编译时的概念，一旦编译成功，就会成为完全不同的两类。对于一个名为 outer 的外部类和其内部定义的名为 inner 的内部类。编译完成后出现 `outer.class` 和 `outer$inner.class` 两类。所以内部类的成员变量 / 方法名可以和外部类的相同。

我们通过代码来详细学习一下内部类吧！

### 成员内部类

```java
// People.java
//外部类People
public class People {
	private String name = "LiLei";         //外部类的私有属性
	//内部类Student
	public class Student {
		String ID = "20151234";               //内部类的成员属性
		//内部类的方法
		public void stuInfo(){
			System.out.println("访问外部类中的name：" + name);
			System.out.println("访问内部类中的ID：" + ID);
		}
	}

	//测试成员内部类
	public static void main(String[] args) {
		People a = new People();     //创建外部类对象，对象名为a
		Student b = a.new Student(); //使用外部类对象创建内部类对象，对象名为b
		// 或者为 People.Student b = a.new Student();
		b.stuInfo();   //调用内部对象的suInfo方法
	}
}
```

编译运行：

```bash
$ javac People.java
$ java People
访问外部类中的name：LiLei
访问内部类中的ID：20151234
```

成员内部类的使用方法：

1. Student 类相当于 People 类的一个成员变量，所以 Student 类可以使用任意访问修饰符。
2. Student 类在 People 类里，所以访问范围在类里的所有方法均可以访问 People 的属性（即内部类里可以直接访问外部类的方法和属性，反之不行）。
3. 定义成员内部类后，必须使用外部类对象来创建内部类对象，即 `内部类 对象名 = 外部类对象.new 内部类();`。
4. 如果外部类和内部类具有相同的成员变量或方法，内部类默认访问自己的成员变量或方法，如果要访问外部类的成员变量，可以使用 `this` 关键字。如上述代码中：`a.this`。

> 注：成员内部类不能含有 `static` 的变量和方法，因为成员内部类需要先创建了外部类，才能创建它自己的。

### 静态内部类

静态内部类通常被称为嵌套类。

```java
// People.java
//外部类People
public class People {
	private String name = "LiLei";         //外部类的私有属性

/*外部类的静态变量。
Java 中被 static 修饰的成员称为静态成员或类成员。它属于整个类所有，而不是某个对象所有，即被类的所有对象所共享。静态成员可以使用类名直接访问，也可以使用对象名进行访问。
*/
	static String ID = "510xxx199X0724XXXX";

	//静态内部类Student
	public static class Student {
		String ID = "20151234";               //内部类的成员属性
		//内部类的方法
		public void stuInfo(){
			System.out.println("访问外部类中的name：" + (new People().name));
			System.out.println("访问外部类中的ID：" + People.ID);
			System.out.println("访问内部类中的ID：" + ID);
		}
	}

	//测试成员内部类
	public static void main(String[] args) {
		Student b = new Student();   //直接创建内部类对象，对象名为b
		b.stuInfo();                 //调用内部对象的suInfo方法
	}
}
```

编译运行：

```bash
$ javac People.java
$ java People
访问外部类中的name：LiLei
访问外部类中的ID：510xxx199X0724XXXX
访问内部类中的ID：20151234
```

静态内部类是 `static` 修饰的内部类，这种内部类的特点是：

1. 静态内部类不能直接访问外部类的非静态成员，但可以通过 `new 外部类().成员` 的方式访问。
2. 如果外部类的静态成员与内部类的成员名称相同，可通过 `类名.静态成员` 访问外部类的静态成员；如果外部类的静态成员与内部类的成员名称不相同，则可通过 `成员名` 直接调用外部类的静态成员。
3. 创建静态内部类的对象时，不需要外部类的对象，可以直接创建 `内部类 对象名 = new 内部类();`。

### 局部内部类

局部内部类，是指内部类定义在方法和作用域内。

例如：

```java
// People.java
//外部类People
public class People {
	//定义在外部类中的方法内：
	public void peopleInfo() {
		final String sex = "man";  //外部类方法中的常量
		class Student {
			String ID = "20151234"; //内部类中的常量
			public void print() {
				System.out.println("访问外部类的方法中的常量sex：" + sex);
				System.out.println("访问内部类中的变量ID:" + ID);
			}
		}
		Student a = new Student();  //创建方法内部类的对象
		a.print();//调用内部类的方法
	}
	//定义在外部类中的作用域内
	public void peopleInfo2(boolean b) {
		if(b){
			final String sex = "man";  //外部类方法中的常量
			class Student {
				String ID = "20151234"; //内部类中的常量
				public void print() {
					System.out.println("访问外部类的方法中的常量sex：" + sex);
					System.out.println("访问内部类中的变量ID:" + ID);
				}
			}
			Student a = new Student();  //创建方法内部类的对象
			a.print();//调用内部类的方法
		}
	}
	//测试方法内部类
	public static void main(String[] args) {
		People b = new People(); //创建外部类的对象
		System.out.println("定义在方法内：===========");
		b.peopleInfo();  //调用外部类的方法
		System.out.println("定义在作用域内：===========");
		b.peopleInfo2(true);
	}
}
```

编译运行：

```bash
$ javac People.java
$ java People
定义在方法内：===========
访问外部类的方法中的常量sex：man
访问内部类中的变量ID:20151234
定义在作用域内：===========
访问外部类的方法中的常量sex：man
访问内部类中的变量ID:20151234
```

局部内部类也像别的类一样进行编译，但只是作用域不同而已，只在该方法或条件的作用域内才能使用，退出这些作用域后无法引用的。

### 匿名内部类

匿名内部类，顾名思义，就是没有名字的内部类。正因为没有名字，所以匿名内部类只能使用一次，它通常用来简化代码编写。但使用匿名内部类还有个前提条件：必须继承一个父类或实现一个接口。

例如：

```java
// Outer.java
public class Outer {

    public Inner getInner(final String name, String city) {
        return new Inner() {
            private String nameStr = name;
            public String getName() {
                return nameStr;
            }
        };
    }

    public static void main(String[] args) {
    	Outer outer = new Outer();
        Inner inner = outer.getInner("Inner", "NewYork");
        System.out.println(inner.getName());
    }
}
interface Inner {
    String getName();
}
```

运行结果：Inner。

匿名内部类是**不能加访问修饰符**的。要注意的是，new 匿名类，这个类是要先定义的, 如果不先定义，编译时会报错该类找不到。

同时，在上面的例子中，当所在的方法的形参需要在内部类里面使用时，该形参必须为 `final`。这里可以看到形参 `name` 已经定义为 `final` 了，而形参 `city` 没有被使用则不用定义为 `final`。

然而，因为匿名内部类没名字，是用默认的构造函数的，无参数的，如果需要该类有带参数的构造函数，示例如下：

```java
public Inner getInner(final String name, String city) {
  return new Inner(name, city) {
    private String nameStr = name;

    public String getName() {
      return nameStr;
    }
  };
}
```

注意这里的形参 `city`，由于它没有被匿名内部类直接使用，而是被抽象类 Inner 的构造函数所使用，所以不必定义为 `final`。

```checker
- name: check Outer java exist
  script: |
    #!/bin/bash
    ls /home/project/Outer.java
  error: 我们发现您还没有创建Outer.java! /home/project/Outer.java
  timeout: 1
```

## package

为了更好地组织类，Java 提供了包机制，用于区别类名的命名空间。

**包的作用**

- 把功能相似或相关的类或接口组织在同一个包中，方便类的查找和使用。
- 包采用了树形目录的存储方式。同一个包中的类名字是不同的，不同的包中的类的名字是可以相同的，当同时调用两个不同包中相同类名的类时，应该加上包名加以区别。
- 包也限定了访问权限，拥有包访问权限的类才能访问某个包中的类。

定义包语法：

```java
package 包名
//注意：必须放在源程序的第一行，包名可用"."号隔开
```

例如：

```java
//在定义文件夹的时候利用"/"来区分层次
//包中用"."来分层
package com.shiyanlou.java
```

不仅是我们这样利用包名来区分类，系统也是这样做的。

![Java系统中的包](https://doc.shiyanlou.com/document-uid79144labid1072timestamp1434937042272.png)

**如何在不同包中使用另一个包中的类？**

使用 `import` 关键字。比如要导入包 `com.shiyanlou` 下 `People` 这个类，`import com.shiyanlou.People;`。同时如果 `import com.shiyanlou.*;` 这是将包下的所有文件都导入进来，`*` 是通配符。

**包的命名规范是全小写字母拼写**。
