---
sort: 16
---

# 项目分析及 Swing 插件安装

## 一、实验介绍

#### 1.1 实验内容

本节课程的主要内容是准备开发环境，对项目结构进行分析并完成Eclipse Swing 插件的安装。

#### 1.2 实验知识点

- 类的建立
- Swing 插件的安装

#### 1.3 实验环境

本实验环境采用带桌面的 Ubuntu Linux 环境，实验中会用到的环境或软件：

- JDK1.8
- Eclipse

#### 1.4 适合人群

本节课程难度较低，只涉及了用Eclipse建立项目并新建类以及安装插件，适合Java开发的新手学习。

#### 1.5 代码获取

你可以在Xfce终端下通过下面命令将实验的完整工程项目下载到实验楼环境中，作为参照对比进行学习。

> 注：如果导入项目后遇到 build path 方面的问题，处理方法参考本课程第三节关于 JDOM 用法的介绍。

```
$ wget http://labfile.oss.aliyuncs.com/courses/260/KnowYou.tar.gz
```

## 二、项目文件结构

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475135260091.png)

## 三、实验步骤

接下来我们将为Eclipse安装swing插件并且做项目分析。

### 3.1 安装 Swing 插件

在项目开始制作之前，需要在 Eclipse 中添加 Swing 插件。

请打开桌面上 Xfce 终端，输入以下命令来下来需要的安装包：

```
wget http://labfile.oss.aliyuncs.com/courses/160/WindowBuilder_v1.1.0_UpdateSite_for_Eclipse3.7.zip
```

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid464timestamp1474549566164.png)

这个安装包默认是放置在 `/home/shiyanlou/` 目录下的。

然后双击打开 Eclipse ，等待 Eclipse 启动完成后，首先需要在菜单的 `help` 中找到 `install new software` 选项。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid464timestamp1474531616940.png)

然后添加源，点击 `Add`按钮。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid464timestamp1474533641528.png)

在 `Add Repositry` 对话框中点击 `Archive` 按钮。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid464timestamp1474533667459.png)

在文件选择对话框中点击刚刚下载的压缩文件。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid464timestamp1474533751362.png)

导入后点击 `OK` 按钮。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid464timestamp1474533805253.png)

选中下方三个选项。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid464timestamp1474534798667.png)

> 此处的 WindowBuilder 版本是针对 Eclipse Indigo （3.7）版本的。如果你需要在最新的几个 Eclipse 版本中安装 WindowBuilder ，请将上述 URL 修改为当前 Eclipse 对应的版本。

点击 `Next` 按钮进入下一步。

在 `Install Details` 步骤直接进入下一步即可。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid464timestamp1474533879676.png)

在软件协议一页，需要选中 `I accept the terms of the license agreement` 。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid464timestamp1474533919358.png)

最后点击 `Finish` 按钮进行安装，安装过程中会弹出如下图所示的进度条。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid464timestamp1474534856126.png)

安装完成之后需要重启 Eclipse 。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid464timestamp1474534883982.png)

### 3.2 项目结构分析及创建

这是一个完整的 Java 小项目，所以代码较多。但是不用担心，本项目代码相对比较简单。

如果你已具备一定的 Java 项目开发能力，可以直接按照下面的项目结构来创建对应的包和类。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475135260091.png)

如果对这个过程不太熟悉也没有关系，可以按照下面的步骤逐个完成项目的创建。

#### 3.2.1 创建项目

首先请双击打开桌面上的 Eclipse ，等待它启动完成后，在菜单 `File` 中点击 `New -> Project`选项。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475132899595.png)

在弹出的新建项目对话框中，选择项目类型为 Java Project，然后点击 `Next` 按钮进入下一步。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475132950757.png)

在这一步需要填写项目的名称 `KnowYou`，然后点击 `Next` 按钮进入下一步。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475133809915.png)

接下的一部是设置一些编译选项，此处可以直接点击 `Finish` 按钮完成项目的创建。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475133892680.png)

如果遇到下图所示的对话框，直接点击 `Yes` 按钮即可。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475133986477.png)

#### 3.2.2 创建包和类

项目创建完成后，我们需要按照之前的项目结构来创建各个类。

这些类大致分为三类：

- GUI界面类：用于完成各个用户交互界面的交互逻辑和事件监听和响应逻辑。
- UTIL类：用于实现日记的读写及辅助功能的实现。
- User类：用于定义日记本用户信息

因此我们首先需要创建一个名为 `com.shiyanlou.view` 的包。

请在创建好的项目目录 `src` 文件夹上右键点击，然后选择 `New -> Package`。 

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475134745911.png)

在弹出的 `New Java Package` 对话框中填写包名，并点击 `Finish` 按钮。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475134837536.png)

接下来是在这个包内创建相应的类。同样需要右键点击刚刚创建的包，然后选择 `New -> Class`。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475134965837.png)

在弹出的 `New Java Class` 对话框中填写类名，并点击 `Finish` 按钮。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475135040159.png)

接下来，请仿照上面的步骤，完成下面的各个包和类的创建。

![此处输入图片的描述](https://doc.shiyanlou.com/document-uid162034labid844timestamp1475135260091.png)

```checker
- name: check KnowYou exist
  script: |
    #!/bin/bash
    ls /home/shiyanlou/eclipse-workspace/KnowYou
  error: 我们发现您还没有创建项目KnowYou! /home/shiyanlou/eclipse-workspace/KnowYou
  timeout: 1
```

## 四、实验总结

在本节实验中，我们完成了开发环境的准备，安装了 Swing 插件。并且了解了日记本项目的主要结构，完成了项目和相应的包、类的创建。

在下节中，我们将正式开始我们的 Java 日记软件征途。首先完成我们的 GUI 界面。

- 完成首页界面
- 完成注册页面
- 完成登录页面