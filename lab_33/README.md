---
sort: 33
---

# 项目工程导入及问题解决

## 一、实验介绍

#### 1.1 实验内容

本节课程主要介绍导入 `hrms` 工程的方法和可能遇到的问题及解决方法。

#### 1.2 实验知识点

- Maven 工程导入

## 二、实验步骤

我们将这里介绍一下开发过程中经常遇到的一些的问题以及解决方案。

### 2.1 项目工程导入

打开 eclipse，选择  `File->Import`，再弹出窗口中继续选择 `Maven->Existing Maven Projects`，然后选择已经下载解压好的工程 `hrms`，点击 `Next`，最后 `Finish` 即可。可以看到导入的工程自动 `Validate` 后还有错误，需要稍加解决。

### 2.2 Maven Problems

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2990timestamp1495692846989.png)

**解决办法**：右键选中该问题， 选择 `Quick Fix`，点击 `Finish`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2990timestamp1495692891796.png)

### 2.3 大量 Java Problems

-  `Access restriction:The type Resources is not ....`
-  `Java Version Mismatch`
-  `The method of type must override a superclass method`

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2990timestamp1495693036060.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2990timestamp1495692913938.png)

这些问题都是 Java 版本不匹配的造成的。

**解决方法：**

**第一步**：

右键项目 `hrms`，选择 `Build  Path -> Configure Build Path`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2906timestamp1493974386775.png)

在弹出的窗口中选择 `Java Build Path -> Libraries`， `remove` 掉 J2SE-1.5 版本的 Library。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2906timestamp1493974507201.png)

然后点击 `Add Library..`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2906timestamp1493974559062.png)

在弹出的窗口中选择 `JRE System Library`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2906timestamp1493974700370.png)

接着点击 `Installed JREs...`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2906timestamp1493974753324.png)

在新窗口选择 java 7 的版本，点击 OK。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2906timestamp1493974903714.png)

最后点击 Finish。

**第二步**：

右击 `hrms`，选择 `Properties`，在弹出窗口选择 `Java Compiler`，取消 `Use compliance from ...` 的勾选，更改 `Compiler compiance level` 为 `1.7`，`Use default compliance settings` 也改为 `1.7`，然后依次点击 `Apply` 和 `OK`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2990timestamp1495692977193.png)

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2990timestamp1495696361065.png)

### 2.4 Maven 工程发布到 Tomcat 

虽然项目 `hrms` 看似没有报错，但还需将 Maven 工程发布 Tomcat 才可以运行。

**解决方法**：

右击 `hrms`，选择 `Properties`，在弹出窗口选择 `Deployment Assembly`，在右侧点击 `Add`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2990timestamp1495693376054.png)

在 `New Assembly Directive` 窗口选择 `Java Build Path Entries`，点击 Next。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2990timestamp1495693416731.png)

选择 `Maven Dependencies`，点击 Finish ，最后依次点击 `Apply` 和 `OK` 。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2990timestamp1495693434712.png)

### 2.5 其他问题

在实验楼的环境中服务器 `Servers` 中的 `Tomcat v7.0 Server at localhost` 有可能无法使用，需要删除后新建一个 Server，再修改 Tomcat 的 启动时间（延长启动的 Timeouts）。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid370051labid2990timestamp1495696398099.png)

## 三、实验总结

本节课程主要是作为附录介绍一些 Maven 项目导入后可能出现的一些问题及解决方法。


