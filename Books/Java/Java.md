# 配置

## 环境变量  

Path是系统的管道符变量，它的作用类似于管道，可以想象成系统利用管道打通指定的路径，使这些路径相同，那么所有这些路径中的内容在任何位置就都可以使用了。例如“System32”文件夹是系统预设在Path变量中的，这个文件夹里的所有命令都可以在任意位置执行，如果在Path变量中删除“System32”文件夹的路径，那么ipconfig等系统命令就不能在任意位置运行。

**系统环境变量**  

1. 新建 JAVA_HOME C:\Program Files\Common Files\Oracle\Java\javapath  
2. Path 新建%JAVA_HOME%\bin并移动到最上面  
    -  C:\Program Files\Common Files\Oracle\Java\javapath   --javac  
      -  报错原因：没有这个环境变量  
      - C:\Program Files (x86)\Common Files\Oracle\Java\javapath  
- 完成  

- 或者将%JAVA_HOME%\bin换为E:\Java\jdk-18.0.1.1\bin

![](c:/notebook/pictures/Snipaste_2022-11-19_00-58-11.png =400x)
![](c:/notebook/pictures/Snipaste_2022-11-19_01-00-14.png =400x)

- 用户环境变量也可如上配置，只对一个用户有效

**检查指向的是JDK，而不是JRE**

- cmd运行：`%JAVA_HOME%\bin\javac -version`不报错则是JDK。

***

1. 请区别Java与JavaC命令，Java命令是运行Java程序的命令，被内置在JRE和JDK环境中，JRE安装向导默认的行为会将Java命令复制到系统System32文件夹中，所以不配置环境变量，这个命令也是可以正常执行的。JavaC命令是Java语言的编译命令，用于把“.java”文件编译为“.class”字节码文件，它是JDK独有的命令，所以经常使用它作为配置环境变量后的测试命令。  
这两个命令在视觉上目前最大的区别是Java命令返回英文提示，而JavaC命令返回中文提示。

2. cmd系统控制台窗口在启动时读取环境变量，并配置到自己的环境中。但是如果用户在启动cmd控制台窗口后，修改了环境变量，已经启动的cmd控制台窗口是无法感知的。很多读者就是在这样的情况下，屡次修改；测试，结果都以失败告终。如果修改了环境变量，一定要在新启动的cmd控制台窗口进行测试。

***
Path是系统中重要的环境变量，如果它被破坏会导致系统部分命令不可用，或致使某些软件不能运行。如果把该变量值备份到另一个环境变量中，就可以在Path环境变量遭到破坏时对其进行恢复。实现过程如下：

（1）在“计算机”图标上单击鼠标右键，选择“属性”命令，在弹出的对话框中选择“高级系统设置”选项，然后单击“环境变量”按钮，将弹出“环境变量”对话框。

（2）在“环境变量”对话框下方找到“Path”变量，双击该变量进行编辑，复制完整的变量值。

（3）新创建一个环境变量“PathBackup”，然后把复制的Path值，粘贴到新的环境变量中，这样就给Path环境变量值做了备份。

![](https://pic.imgdb.cn/item/62a9a13f09475431290fe2a5.jpg)

***

## 简介

![](https://pic.imgdb.cn/item/62a99d30094754312908accb.jpg)

## JavaSE

JavaSE就是Java的标准版，主要用于桌面应用程序的开发，同时也是Java的基础，它包含Java语言基础、JDBC数据库操作、I/O输入输出、网络通讯、多线程等技术。

## JavaEE

JavaEE是Java2的企业版，主要用于开发企业级分布式的网络程序，如电子商务网站和ERP系统，其核心为EJB。

## JavaME

J2ME主要应用于嵌入式系统开发，如掌上电脑、手机等移动通讯电子设备，现在大部分手机厂商所生产的手机都支持Java技术，看一下我们身边，有谁的手机是不支持Java的，几乎很少。

## JDK(Java Development Kit Java开发工具包)

JDK是提供给Java开发人员使用的，其中包含了java的开发工具，也包括了JRE。所以安装了JDK，就不用在单独安装JRE了。  
其中的开发工具：编译工具(javac.exe) 打包工具(jar.exe)等  

## JRE(Java Runtime Environment Java运行环境)

包括Java虚拟机(JVM Java Virtual Machine)和Java程序所需的核心类库等，
如果想要运行一个开发好的Java程序，计算机中只需要安装JRE即可。

### JDK,JRE和JVM的关系

JDK包含JRE，JRE包含JVM.

- JDK = JRE + 开发工具集（例如Javac编译工具等）
- JRE = JVM + Java SE标准类库
![](https://pic.imgdb.cn/item/62a9a0e609475431290f52bb.jpg)

### Java API的文档

Java API也可以说是JDK文档，它的全称是Application Programming Interface应用程序编程接口，简称API，是Java程序开发不可缺少的编程词典，它记录了Java语言中海量的API，主要包括类的继承结构、成员变量和成员方法、构造方法、静态成员的详细说明和描述信息。  
Java语言提供了大量的基础类，因此 Oracle 也为这些基础类提供了相应的API文档，用于告诉开发者如何使用这些类，以及这些类里包含的方法。  
下载API：
<http://www.oracle.com/technetwork/java/javase/downloads/index.html>

Additional Resources-Java SE 8 Documentation下载。

***

## java初识

***

### java语言的特点是什么？

- 面向对象性：
  - 两个基本概念：类、对象；
  - 三大特性：封装、继承、多态  
- 健壮性：吸收了C/C++语言的优点，但去掉了其影响程序健壮性的部分（如指针、内存的申请与释放等），提供了一个相对安全的内存管理和访问机制
- 跨平台性：通过Java语言编写的应用程序在不同的系统平台上都可以运行。“Write once , Run Anywhere”

***

### Java语言运行机制及运行过程

- Java两种核心机制
  - Java虚拟机 (Java Virtal Machine)
  - 垃圾收集机制 (Garbage Collection)
- 核心机制—Java虚拟机

#### JVM

JVM是一个虚拟的计算机，具有指令集并使用不同的存储区域。负责执行指令，管理数据、内存、寄存器。
对于不同的平台，有不同的虚拟机。  
只有某平台提供了对应的java虚拟机，java程序才可在此平台运行  
Java虚拟机机制屏蔽了底层运行平台的差别，实现了“一次编译，到处运行”

#### 核心机制—垃圾回收

不再使用的内存空间应回收—— 垃圾回收。 在C/C++等语言中，由程序员负责回收无用内存。  
Java 语言消除了程序员回收无用内存空间的责任：
它提供一种系统级线程跟踪存储空间的分配情况。并在JVM空闲时，检查并释放那些可被释放的存储空间。  
垃圾回收在Java程序运行过程中自动进行，程序员无法精确控制和干预。
***

### System.out.println()和System.out.print()什么区别呢？

以下代码的运行效果是什么？

```
System.out.println();  //打印完后，会换行。  
System.out.print();   //打印完后，不会换行。
```

***

### 一个".java"源文件中是否可以包括多个类（不是内部类）？有什么限制？

答：可以。  
但最多只有一个类名声明为public，与文件名相同。  
Something类的文件名叫OtherThing.java

```
class Something {
    public static void main(String[] something_to_do) {
        System.out.println("Do something ...");
    }
}
```

这个很明显。
答案: 正确。  
从来没有人说过Java的class名字必须和其文件名相同。但public class的名字必须和文件名相同。
***

### 为什么要设置path（或者说，设置path的目的是什么）？

目的是为了在控制台的任何文件路径下，都可以调用jdk指定目录下的所有指令。
***

### 源文件名是否必须与类名相同？如果不是，那么什么情况下，必须相同？

不需要源文件名与类名相同，只有public下才必须相同。  
`public class Test{...}`
***

### 程序中若只有一个public修饰的类，且此类含main方法。那么类名与源文件名可否不一致？

***

### Java的注释方式有哪几种，格式为何？

```
1.单行注释
//
2.多行注释
/*
*/
3.javadoc注释
/**

*/
```

***

### 自己使用java文档注释的方式编写程序，并用javadoc命令解析

***

### 超纲题目：GC是什么? 为什么要有GC

答：GC是垃圾收集的意思（Gabage Collection）,内存处理是编程人员容易出现问题的地方，
忘记或者错误的内存回收会导致程序或系统的不稳定甚至崩溃，Java提供的GC功能可以自动监测对象是否超过作用域从而达到自动回收内存的目的，Java语言没有提供释放已分配内存的显示操作方法。
***

### 超纲题目：垃圾回收器的基本原理是什么？垃圾回收器可以马上回收内存吗？有什么办法主动通知虚拟机进行垃圾回收

答：对于GC来说，当程序员创建对象时，GC就开始监控这个对象的地址、大小以及使用情况。通常，GC采用有向图的方式记录和管理堆(heap)中的所有对象。通过这种方式确定哪些对象是"可达的"，哪些对象是"不可达的"。  
当GC确定一些对象为"不可达"时，GC就有责任回收这些内存空间。可以。程序员可以手动执行System.gc()，通知GC运行，但是Java语言规范并不保证GC一定会执行。
***

# 安装

***

## 安装路径尽量不用中文

***

## Eclipse

IDE 集成开发环境

### 安装

1. JavaEE
2. 设置透视图(perspective)
     - 进入 Eclipse 编辑界面以后,首先选择,设置透视图
     - javaEE
3. 添加透视图需要显示的结构
     - 结构 1：Package Explorer
     - 结构 2：Navigator
     - 结构 3：Outline
     - 结构 4：Console
4. 关闭其它不需要的结构
     - ![](https://pic.imgdb.cn/item/62ab448d0947543129c83e59.jpg)
5. 设置编码集 utf-8
     - window> preforence>General>wokerspace>
     - 设置字体,字形,字体大小
     - window> preforence>General>Appearence>Colords and Fonts>Text Font
6. 设置 package explorer 中右键:new 下显示的结构
    - ![](https://pic.imgdb.cn/item/62ab450a0947543129c9e59f.jpg)
7. 保存当前透视图
    - window>perspective>Save Perspective As...
    - JavaEE
    
### 插件安装

- help --> install New Software

![](C:/NoteBook/pictures/278101611240276.png =248x)

![](C:/NoteBook/pictures/571911411227441.png =581x)

- 右下角进度条

![](C:/NoteBook/pictures/271841511247607.png =250x)

**WindowBulder**
 
`http://download.eclipse.org/windowbuilder/latest/`

### 卸载

eclipse是绿色安装软件，在控制面板的卸载程序中找不到卸载eclipse的地方。只需要在文件资源管理器中删除以下文件就行了：  

1. 删除你安装eclipse的文件夹。  
    - 打开 eclipse.exe 的安装目录后，在它的父目录中查找，直到看到名为“eclipse”的文件夹，点击该文件夹的父文件夹。
2. 删除用户中的".p2”、“eclipse-workspace”、“.eclipse”文件夹即可
    - .p2
    - .eclipse //安装目录
    - eclipse-workspace  //工作空间

### Eclipse启动错误修复教程
**eclipse启动失败，提示“发生了错误，请参阅日志文件.log**

### 编码问题
utf-8    

- 项目的编码
- 输出显示的编码
- 代码文件的编码

### 导入文件

1.新建项目
2.src右键 选择导入Import
3. 
![](C:/NoteBook/pictures/434584411220960.png =410x)
4.
![](C:/NoteBook/pictures/379394511239386.png =494x)

### 自动产生get和set方法
Source -> Generate getters and setters...

或者`Alt + Shift + S` -> Generate getters and setters...

### 创建构造器
`Alt + Shift + S` -> Generate Constructor using Fileds
***
- 选择父类中带参的

![](C:/NoteBook/pictures/314882023221047.png =332x)
### 创建eqals()的重写

`Alt + Shift + S` -> Generate hashCode() And equals()

### 创建toString的重写

`Alt + Shift + S` -> Generate toString()


### 创建类

![](C:/NoteBook/pictures/536772223239473.png =342x)

![](C:/NoteBook/pictures/327712323227340.png =352x)

![](C:/NoteBook/pictures/87102423247506.png =344x)

### 自动包装try

![](C:/NoteBook/pictures/497925823247509.png =441x)

### 快捷键

```
package com.atguigu.java;

import java.io.InputStream;
import java.util.HashMap;
import java.util.Scanner;

/*
 * Eclipse中的快捷键：
 * 1.补全代码的声明：alt + /
 * 2.快速修复: ctrl + 1  
 * 3.批量导包：ctrl + shift + o
 * 4.使用单行注释：ctrl + /
 * 5.使用多行注释： ctrl + shift + /   
 * 6.取消多行注释：ctrl + shift + \
 * 7.复制指定行的代码：ctrl + alt + down 或 ctrl + alt + up
 * 8.删除指定行的代码：ctrl + d
 * 9.上下移动代码：alt + up  或 alt + down
 * 10.切换到下一行代码空位：shift + enter
 * 11.切换到上一行代码空位：ctrl + shift + enter
 * 12.如何查看源码：ctrl + 选中指定的结构   或  ctrl + shift + t
 * 13.退回到前一个编辑的页面：alt + left 
 * 14.进入到下一个编辑的页面(针对于上面那条来说的)：alt + right
 * 15.光标选中指定的类，查看继承树结构：ctrl + t
 * 16.复制代码： ctrl + c
 * 17.撤销： ctrl + z
 * 18.反撤销： ctrl + y
 * 19.剪切：ctrl + x 
 * 20.粘贴：ctrl + v
 * 21.保存： ctrl + s
 * 22.全选：ctrl + a
 * 23.格式化代码： ctrl + shift + f
 * 24.选中数行，整体往后移动：tab
 * 25.选中数行，整体往前移动：shift + tab
 * 26.在当前类中，显示类结构，并支持搜索指定的方法、属性等：ctrl + o
 * 27.批量修改指定的变量名、方法名、类名等：alt + shift + r
 * 28.选中的结构的大小写的切换：变成大写： ctrl + shift + x
 * 29.选中的结构的大小写的切换：变成小写：ctrl + shift + y
 * 30.调出生成getter/setter/构造器等结构： alt + shift + s
 * 31.显示当前选择资源(工程 or 文件)的属性：alt + enter
 * 32.快速查找：参照选中的Word快速定位到下一个 ：ctrl + k
 * 
 * 33.关闭当前窗口：ctrl + w
 * 34.关闭所有的窗口：ctrl + shift + w
 * 35.查看指定的结构使用过的地方：ctrl + alt + g
 * 36.查找与替换：ctrl + f
 * 37.最大化当前的View：ctrl + m
 * 38.直接定位到当前行的首位：home
 * 39.直接定位到当前行的末位：end
 */

public class EclipseKeys{

	public static void main(String[] args) {
		System.out.println();

		Scanner scanner = new Scanner(System.in);

		int[] arr = new int[] { 33, 44, 5, 2, 4, 53, 2 };

		int max = 0;
		int temp = 0;
		String string = new String();
		char c = string.charAt(0);

		for (int i = 0; i < arr.length; i++) {

		}

		EclipseKeys e = new EclipseKeys();
		e.method();
		e.num = 10;

		InputStream is = null;
		
		HashMap map1 = null;
		map1 = new HashMap();
		map1.put(null, null);
		
		System.out.println(map1);
		
	}

	int num = 10;

	public void method() {

	}
}
```

#### 自动提示补完

windows -> preference ->Java -> Editor->Content Assist -> Auti Activation -> Auto activation for Java ->abcdefghijklmnopqrstuvwxyz.

#### 快捷键设置

preference 搜索 keys

### Debug

如何调试
1. 设置断点 --双击行号
    - 注意：可以设置多个断点
2. debug as java application
3. 常用操作
![](C:/NoteBook/pictures/18454012227258.png =659x)
![](C:/NoteBook/pictures/328293012220965.png =430x)
![](C:/NoteBook/pictures/520643012239391.png =432x)

#### Setdep Into 失灵

![](C:/NoteBook/pictures/581875512247424.png =572x)
![](C:/NoteBook/pictures/245315712240093.png =772x)
将JRE改为JDK即可

## Editplus安装

配置用户工具  

- path
- $Filenama
- $FileDir  

***

## vim yyds

***

## IDEA


### Debug调试

### 导入

#### 导入Eclipse的工程

1. 查看Eclipse中存放的物理地址，并复制文件
![](C:/NoteBook/pictures/499751215221070.png =376x)
![](C:/NoteBook/pictures/112101515227363.png =474x)
2. 到IDEA中的文件位置黏贴
![](C:/NoteBook/pictures/448351815247529.png =489x)
![](C:/NoteBook/pictures/47042115240198.png =117x)
3. 将该过程设置为可用
![](C:/NoteBook/pictures/228242215236753.png =272x)
![](C:/NoteBook/pictures/37412315232507.png =498x)
![](C:/NoteBook/pictures/358822315250387.png =249x)
![](C:/NoteBook/pictures/65532415247991.png =394x)
![](C:/NoteBook/pictures/488702415245493.png =476x)
![](C:/NoteBook/pictures/435192515226734.png =565x)

#### 导入第三方jar包

1. 新建一个文件夹，通常命名为lib
![](C:/NoteBook/pictures/121441116247612.png =709x)
2. 把要导入的jar包黏贴到目标文件目录
3. 使其作为API来使用
![](C:/NoteBook/pictures/331611416240281.png =574x)


### 切换JDK版本

**需要修改JDK的路径：%JAVA_HOME%**

- 对于项目
![](c:/notebook/pictures/Snipaste_2022-11-20_17-22-03.png =500x)
- 对于模块
![](c:/notebook/pictures/Snipaste_2022-11-20_17-24-24.png =500x)
- 导入新的JDK
![](c:/notebook/pictures/Snipaste_2022-11-20_17-26-26.png =500x)

### idea jar包导出

1.
![](c:/notebook/pictures/Snipaste_2022-11-21_14-03-57.png =500x)
![](c:/notebook/pictures/Snipaste_2022-11-21_14-05-05.png =500x)
2.
![](c:/notebook/pictures/Snipaste_2022-11-21_14-05-54.png =500x)
![](c:/notebook/pictures/Snipaste_2022-11-21_14-07-25.png =500x)

# Linux安装Java


```shell
# 1.下载tar.gz后缀的
#把下载的文件 jdk-8u151-linux-x64.tar.gz 放在/usr/java/目录下。
# 2.在/usr/目录下创建java目录，
mkdir /usr/java
cd /usr/java
# 3.解压 JDK
tar -zxvf jdk-8u151-linux-x64.tar.gz
# 4.设置环境变量
#修改 /etc/profile
#在 profile 文件中添加如下内容并保存：
set java environment
JAVA_HOME=/usr/java/jdk1.8.0_151        
JRE_HOME=/usr/java/jdk1.8.0_151/jre     
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export JAVA_HOME JRE_HOME CLASS_PATH PATH
#注意：其中 JAVA_HOME， JRE_HOME 请根据自己的实际安装路径及 JDK 版本配置。
#让修改生效：
source /etc/profile
# 5.测试
java -version
#显示 java 版本信息，则说明 JDK 安装成功：
java version "1.8.0_151"
Java(TM) SE Runtime Environment (build 1.8.0_151-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.151-b12, mixed mode)
```

# 基础先导

## DOS命令

常用的DOS命令

- dir : 列出当前目录下的文件以及文件夹
- md : 创建目录
- rd : 删除目录
- cd : 进入指定目录
- cd.. : 退回到上一级目录
- cd\: 退回到根目录
- del : 删除文件
- exit : 退出 dos 命令行
- 补充：echo javase>1.doc
- 常用快捷键
  - ← →：移动光标
  - ↑ ↓：调阅历史操作命令
  - Delete和Backspace：删除字符

***

## 注释

***

### 单行注释 //

***

### 多行注释 /**/

***

### 文档注释

Javadoc注释根据写法和位置的不同，主要分为以下3类：

- 写在类/方法上面的Javadoc注释
- 写在字段上面的Javadoc
- 写在包上面的Javadoc

注意：

- 所有的Javadoc注释都以/**开头，*/结尾。
- 每种Javadoc注释都要和其后对应的类/方法/字段/包保持同样的缩进

在Java源文件编辑器中，选择某个成员方法或成员变量，然后按`<Alt>+<Shift>+<J>`快捷键，Eclipse会自动添加JavaDoc文档注释结构，如果是方法的话，还会自动添加参数名称。

```
class student { 
/** 
* 没有和下面的方法保持同样的缩进，是错误的. 
*/ 
    public int getName(int ID){
            ... 
    } 
}
```

```
/**
@author 指定java程序的作者
@version 指定源文件的版本
*/
```

***

#### 写在类/方法上面的Javadoc注释

此种注释分为三段，顺序如下：

1. 概要描述 (main description)。
     - 用一句话或者一段话简要描述该类的作用，以英文句号结束，这句话会被提取并放到索引目录中。(当识别到第一个英文句号时，系统会自动认为概要描述已经结束，紧接其后的话都不会被放到概要中。要避免这种情况，可以在英文句号后加上`&nbsp;` 即可，这样系统将判定 `&nbsp;`后的下一个英文句号为概要描述结束的标志)
2. 详细描述。
     - 用一段话或者多段话详细描述该类的作用，每段话都以英文句号结束。详细描述中可以使用html标签，如`<p> (定义段落，写在段前), <pre> (放在该标签内的内容可以保留“原始样子”), <a> (定义超链接), <li> (定义列表项目) 等标签。`
3. 文档标记，即类似小标签一样的说明。  

- 描述部分和文档标记之间必须空一行。

```
/**
* Graphics is the abstract base class for all graphics contexts
* which allow an application to draw onto components realized on 
* various devices or onto off-screen images.
* <p>
* A Graphics object encapsulates the state information needed
* for the various rendering operations that Java supports. This
* state information includes:
* <ul>
* <li>The current clip
* <li>The current color
* <li>The current font
* </ul>
* <p>
* Some important points to consider are that drawing a figure that
* covers a given rectangle will occupy one extra row of pixels on
* the right and bottom edges compared to filling a figure that is
* bounded by that same rectangle.
* <p>
* All coordinates which appear as arguments to the methods of this
* Graphics object are considered relative to the translation origin
* of this Graphics object prior to the invocation of the method.
* 
* @author      Sami Shaio
* @since       1.0
*/
public abstract class Graphics {
    /** 
    * Draws as much of the specified image as is currently available
    * with its northwest corner at the specified coordinate (x, y).
    * This method will return immediately in all cases.
    * <p>
    * If the current output representation is not yet complete then
    * the method will return false and the indicated 
    * {@link ImageObserver} object will be notified as the
    * conversion process progresses.
    *
    * @param img       the image to be drawn
    * @param x         the x-coordinate of the northwest corner
    *                  of the destination rectangle in pixels
    * @param y         the y-coordinate of the northwest corner
    *                  of the destination rectangle in pixels
    * @param observer  the image observer to be notified as more
    *                  of the image is converted.  May be 
    *                  <code>null</code>
    * @return          <code>true</code> if the image is completely 
    *                  loaded and was painted successfully; 
    *                  <code>false</code> otherwise.
    * @see             Image
    * @see             ImageObserver
    * @since           1.0
    */
    public abstract boolean drawImage(Image img, int x, int y, 
ImageObserver observer);
```

***

#### 写在包上面的Javadoc

格式和写在类、方法上面的javadoc的格式一样。内容方面主要是介绍这个包是干嘛的，包的结构，在使用方面要注意什么等信息。
包注释是写在package-info.java这个文件里的，该文件在创建包时会顺带生成。

```
/**
 * Provides the classes necessary to create an applet and the classes an applet uses 
 * to communicate with its applet context. 
 * <p>
 * The applet framework involves two entities: 
 * the applet and the applet context. An applet is an embeddable window (see the 
 * {@link java.awt.Panel} class) with a few extra methods that the applet context 
 * can use to initialize, start, and stop the applet.
 *
 * @since 1.0
 * @see java.awt
 */
package java.lang.applet;
```

***

#### 写在字段上面的Javadoc

只有public的字段才需要注释，通常是static的

```
/**
* the static field a
*/
public static int a = 1;
```

#### javadoc

注释内容可以被JDK提供的工具 javadoc 所解析，生成一套以网页文件形式体现的该程序的说明文档。  
只提取文档注释
文档注释只放在类、接口、成员变量、方法之前，因为 Javadoc 只处理这些地方的文档注释，而忽略其它地方的文档注释。  
API 帮助文档相当于产品说明书，而说明书只需要介绍那些供用户使用的部分，所以 Javadoc 默认只提取 public、protected 修饰的部分。如果要提取 private 修饰的部分，需要使用 -private。
Javadoc 工具可以识别文档注释中的一些特殊标签，这些标签一般以@开头，后跟一个指定的名字，有的也以{@开头，以}结束。Javadoc 可以识别的标签如下表所示：  
![](https://pic.imgdb.cn/item/62ab4e500947543129ea40a2.jpg)
对两种标签格式的说明：

- @tag 格式的标签（不被{ }包围的标签）为块标签，只能在主要描述（类注释中对该类的详细说明为主要描述）后面的标签部分（如果块标签放在主要描述的前面，则生成 API 帮助文档时会检测不到主要描述）。
- {@tag} 格式的标签（由{ }包围的标签）为内联标签，可以放在主要描述中的任何位置或块标签的注释中。

Javadoc 标签注意事项：

- Javadoc 标签必须从一行的开头开始，否则将被视为普通文本。
- 一般具有相同名称的标签放在一起。
- Javadoc 标签区分大小写，代码中对于大小写错误的标签不会发生编译错误，但是在生成 API 帮助文档时会检测不到该注释内容。

cmd（命令提示符）中输入`javadoc -help`就可以看到 Javadoc 的用法和选项（前提是安装配置了JDK）  

| 名称                 | 说明                                    |
| :------------------ | :------------------------------------- |
| -public             | 仅显示 public 类和成员                   |
| -protected          | 显示 protected/public 类和成员（默认值） |
| -package            | 显示 package/protected/public 类和成员  |
| -private            | 显示所有类和成员                         |
| -d <directory>      | 输出文件的目标目录                       |
| -version           | 包含 @version 段                        |
| -author             | 包含 @author 段                         |
| -splitindex         | 将索引分为每个字母对应一个文件            |
| -windowtitle <text> | 文档的浏览器窗口标题                     |

```
/**
 @author scott
 @version v1.0123
*/
public class Hello{
        /**
         如下的方式是main()
         @author system
         @version v2.100
         */
        public static void main(String[] args){
                //单行注释
                System.out.println("Hello Java !");
        }
}

cmd>javadoc -author -version Hello.java 
cmd>javadoc [-d 文件夹名] -author -version 文件名.java 

文件夹中 index.html
```

Test.java

```
/**
* @author C语言中文网
* @version jdk1.8.0
*/
public class Test{
    /**
     * 求输入两个参数范围以内整数的和
     * @param n 接收的第一个参数，范围起点
     * @param m 接收的第二个参数，范围终点
     * @return 两个参数范围以内整数的和
     */
    public int add(int n, int m) {
        int sum = 0;
        for (int i = n; i <= m; i++) {
            sum = sum + i;
        }
        return sum;
    }
} 

cmd>javadoc -author -version Test.java
当前文件夹中的Test.html
```

***

##### javadoc 不是内部或外部命令

环境变量的问题

1. 检查jdk/bin目录 是否是有javadocw.exe
    - 在该目录下执行 javadoc 可以
2. 在用户变量里面也需要添加路径
    - 环境变量->_用户变量_（是用户变量不是系统变量）->Path 然后点击编辑，新建%JAVA_HOME%\bin的文件路径并上移到最上，点击确定

***

##### 注释中有汉字可能会乱码

使用

```
javadoc -encoding UTF-8 -charset UTF-8 Test.java
```

***

## 注意

1. Java源文件以“java”为扩展名。源文件的基本组成部分是类（class），如本例中的HelloWorld类。
2. Java应用程序的执行入口是main()方法。它有固定的书写格式：
3. public主类不可以少
   - 错误: 找不到或无法加载主类 Test
   - 原因: java.lang.ClassNotFoundException: Test

```
public static void main(String[] args) {...}
```

3. Java语言严格区分大小写。
4. Java方法由一条条语句构成，每个语句以“;”结束。
5. 大括号都是成对出现的，缺一不可。
6. 一个源文件中最多只能有一个public类。其它类的个数不限，如果源文件包含一个public类，则文件名必须按该类名命名。

## javac编译错误: 编码UTF8/GBK的不可映射字符

在win命令行下编译java文件时，出现了“编码GBK的不可映射字符”错误，
原因是源代码中出现了中文字符。

1. 解决方法是添加encoding 参数
2. 此问题是java文件不是UTF-8格式的，另存为UTF-8格式即可

```
javac -encoding utf-8 main.java
```

***

## 初步使用

Java源代码文件的名称必须和定义的类或接口名称一直，然后添加“.java”后缀
***

### Hello.java

文件名 Hello.java  
//声明为public的类应与文件名一致

```
public class Hello{
        public static void main(String[] args){ 
                System.out.println("Hello Java");  //输出语句
        }
}

cmd> cd 到存放此java文件的路径下
cmd> javac Hello.java //把“.java”文件编译为“.class”字节码文件
cmd> java Hello  //执行编译后的“Hello.class”字节码文件
```

1. 将 Java 代码编写到扩展名为 .java 的文件中。
2. 通过 javac 命令对该 java 文件进行编译。
3. 通过 java 命令对生成的 class 文件进行运行。
![](https://pic.imgdb.cn/item/62a9a19309475431291066a4.jpg)

因为一个程序的执行需要一个起始点或者入口，所以在Test类中的加入

```
public static void main(String[] args){ }
```

***

***

### 输出星号字符组成的三角形.exe

Triangle.java

```
public class Hello{
        public static void main(String[] args){
                System.out.println("   *   ");
                System.out.println("  ***  ");
                System.out.println(" ***** ");
                System.out.println("*******");
        }
}
```

### 计算两个整数的和.exe

SumNum.java

```
public class SumNum{
        public static void main(String[] args){
                int a = 1,b = 2;
                int c = a + b;
                System.out.println(c);
        }
}
```

### 输出当前日期

PrintDate.java

```
import java.util.Date;
public class PrintDate{
        public static void main(String[] args){
                Date date = new Date();
                String str = date.toLocaleString();
                System.out.println("now_date:"+str);
        }
}

注: PrintDate.java使用或覆盖了已过时的 API。
注: 有关详细信息, 请使用 -Xlint:deprecation 重新编译。
```

### 在控制台接收用户输入数字.exe

Receiavinput.java

```
import java.util.Scanner;
public class ReceiveInput{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);
                System.out.println("input your name:");
                String name  = scan.nextLine();
                System.out.println("input your age:");
                int age = scan.nextInt();
                System.out.println("input your height");
                double stature = scan.nextDouble();
                System.out.println("over");
                System.out.println("your age:" + age + "\n" + "your name:" + name + "\n" + "your height:" + stature);
        }
}
```

创建ReceiveInput类，在该类的主方法中创建Scanner扫描器来封装System类的in输入流，然后提示用户输入姓名、年龄和身高信息，并输出由这些信息组成的说明信息。  
程序中用到了System类的输入流也就是类变量in，它可以接收用户的输入信息，并且是标准的输入流实例对象。另外Scanner类是Java的扫描器类，它可以从输入流中读取指定类型的数据或字符串。本实例使用Scanner类封装了输入流对象，并使用nextLine()、nextInt()、nextDouble()方法从输入流中获取用户输入的整行文本字符串、整数数据和double类型数据。

说明：在使用Scanner类时，必须在源代码顶端导入这个类，导入语句为“import java.util.Scanner;”

### 计算用户输入数字的乘积.exe

AverNum.java

```
import java.util.Scanner;
public class AverNum{
 public static void main(String[] args){
  Scanner scan = new Scanner(System.in);
  System.out.println("input n1:");
  double n1 = scan.nextDouble();
  System.out.println("input n2:");
  double n2  =scan.nextDouble();
  System.out.println("-----------------");
  System.out.println(n1 + "*" + n2 + "=" + n1 * n2);
 }
} 
```

### 爱心

PrintHeart1.java

```
class PrintHeart1 {
 public static void main(String[] args) {
  System.out.print("\t" + "*" + "\t\t\t\t\t\t\t\t\t\t\t\t" + "*" + "\t" + "\n");
  System.out.print("*" + "\t\t" + "*" + "\t\t\t\t" + "I love Java" + "\t\t\t\t\t" + "*" + "\t\t" + "*" + "\n");
  System.out.print("\t" + "*" + "\t\t\t\t\t\t\t\t\t\t\t\t" + "*" + "\t" + "\n");
  System.out.print("\t\t" + "*" + "\t\t\t\t\t\t\t\t\t\t" + "*" + "\t\t" + "\n");
  System.out.print("\t\t\t" + "*" + "\t\t\t\t\t\t\t\t" + "*" + "\t" + "\n");
  System.out.print("\t\t\t\t" + "*" + "\t\t\t\t\t\t" + "*" + "" + "\t" + "\n");
  System.out.print("\t\t\t\t\t" + "*" + "\t\t\t\t" + "*" + "" + "\t\t" + "\n");
  System.out.print("\t\t\t\t\t\t" + "*" + "\t\t" + "*" + "" + "\t\t" + "\n");
  System.out.print("\t\t\t\t\t\t\t" + "*" + "\n");
 }
}
```

### 输出基本信息(姓名后空一行,再输出性别/年龄)

```
姓名:   小米
性别:   男
年龄:   18  
```

```
public class PrintInfo{
        public static void main(String[] args){
                System.out.println("姓名:" + "\t" + "小米");
                System.out.println();
                System.out.println("性别:" + "\t" + "男");
                System.out.println("年龄:" + "\t" + 18);
        }
}
```

***

# Java基本语法

***

## Java主类结构

Java语言是面向对象的程序设计语言，Java程序的基本组成单元就是类，类体中又可包括属性与方法两部分。每一个Java本地应用程序都必须包含一个main()方法，main()方法为Java应用程序的入口，包含main()方法的类称之为主类。一个完整的主类结构通常由定义包语句、定义类语句、定义主方法语句、定义变量语句和导入API类库5部分组成，

### 定义包

开发应用程序时，通常将Java类放在指定的包中，每个包内通常放置同类的Java类，这样可以使应用程序的结构更加清晰，便于以后维护。在Java中，可以使用package关键字指定Java类所在包
Java包，在计算机硬盘上的表现形式为文件夹，例如，将First.java类放置在com包中，那么该Java文件将被放置在com文件夹中。
定义类包为structure的代码如下：  

```
package structure;      
```

### 定义类

一个Java应用程序是由若干个类组成的。通过class关键字来定义类，
例如，要定义一个名称为FirstClass的公共类，可以使用以下代码：

```
public class FirstClass {    

}
```

### 定义主方法

main()方法是类体中的主方法。该方法从“{”号开始，至“}”号结束，中间的是方法体。
public、static、void分别是main方法的权限修饰符、静态修饰符和返回值类型声明，
在Java程序中的main()方法，必须声明为public static void。
String arg[]是一个字符串类型的数组，它是main()方法的参数。
main()方法是程序开始执行的位置。
main()方法是程序入口方法。它有固定的格式，方法的参数也固定为一个字符串数组。

### 定义变量

通常将类的属性称之为类的全局变量（或成员变量）。将方法中的属性称之为局部变量。全局变量声明在类体当中，局部变量声明在方法体当中。全局变量和局部变量都有各自的应用范围。

### 导入API类库

Java类必须执行导入才能够被当前类使用。在Java语言中可以通过“import”关键字导入相关的类。
例如，要导入java.util.Date类，可以使用以下代码：

```
import java.util.Date;
```

### 例1

```
package structure;                                                         //定义类包

public class FirstClass {                                                 //类声明
      private static String className = "FirstClass";           //定义成员变量
      /**
       * @param args
       */
      public static void main(String[] args) {                      //主方法
                  String info = "，我的第一个Java类";                   //定义局部变量
            System.out.println(className+info);                    //输出变量className+info
      }
}
```

***

## 关键字和保留字

### 关键字

![001](vx_images/327595014247117.png =345x)

![关键字](vx_images/193245214239786.png =478x)

### 保留字

- goto
- const

***

## 标识符

### 命名规则

- 26个英文字母大小写，0-9，_，$
- 数字不可以开头
- 不能使用关键字和保留字
- 严格区分大小写
- 不能包含空格

### 命名规范

- 包名：多单词时都小写 xxxyyy
- 类名/接口名：多单词时每个单词首字母大写 XxxYyy
- 变量名/方法名：多单词时第一个单词首字母小写，第二个单词开始每个单词首字母大写xxxYyy
- 常量名：所有字母都大写，多单词用下划线连接 XXX_YYY

### java使用unicode字符集可以用中文，尽量别，奇怪的bug

***

## 变量

### 变量概念

- 内存中的一个存储区域
- 该区域的数据可以在同一类型范围内不断变化
- 变量是程序中最基本的存储单元。包含变量类型，变量名和存储的值。  

定义变量？  
简单的说，就是要告诉编译器（compiler）这个变量是属于哪一种数据类型，这样编译器才知道需要配置多少空间给它，以及它能存放什么样的数据。在程序运行过程中，空间内的值是变化的，所以称为变量。为了便于操作，给这个空间取个名字，称为变量名，内存空间内的值就是变量值。

变量的作用

- 用于在内存中保存数据

使用变量注意：

- Java每个变量必须先声明，后使用
- 使用变量名来访问这块区域的数据
- 变量的作用域：其定义所在的一对{}内,即函数体内
- 变量只有在其作用域内才有效
- 同一个作用域内，不能定义重名的变量

#### 系统的内存可大略的分为3个区域

- 系统区（OS）、
- 程序区（Program）、
- 数据区（Data）。

当程序执行时，程序代码会加载到内存中的程序区，数据暂时存储在数据区中。假设上述两个变量定义在方法体中，则程序加载到程序区中。当执行此行程序代码时，会在数据区配置空间给这两个变量

![](C:/NoteBook/pictures/567682923220659.png)

***
格式：

```
数据类型 变量名 = 变量值
```

```
public class VariableTest{
        public static void main(String[] args){
                int id; //变量的声明
                int age = 12;  //变量的定义
                System.out.println(age);   //变量的使用
                id = 001;
                System.out.println(id);
        }
}
```

***

### 基本数据类型

#### 按数据类型分

![](C:/NoteBook/pictures/544083715232095.png =636x)

#### 按声明位置不同

![](C:/NoteBook/pictures/162523815249975.png =478x)
***

#### 整数类型：byte、short、int、long

- Java各整数类型有固定的表数范围和字段长度，不受具体OS的影响，以保
证java程序的可移植性。
- java的整型常量默认为 int 型，声明long型常量须后加‘l’或‘L’
- java程序中变量通常声明为int型，除非不足以表示较大的数，才使用long

500MB 1MB = 1024KB 1KB= 1024B B= byte ? bit?

- bit: 计算机中的最小存储单位。
- byte:计算机中基本存储单元。

![](C:/NoteBook/pictures/64484315247579.png =480x)

```
public class Test{
        public static void main(String[] args){
                byte b1 = -128;
                //byte b2 = 128; 超出范围-128~127 编译错误
                System.out.println(b1);

                short s1 = 128;
                System.out.println(s1);
                
                int i1 = 1234;
                System.out.println(i1);
                
                long l1 = 12334578L; //声明long型常量须后加‘l’或‘L’，否则默认int
                System.out.println(l1);
                //long l2 = 123456789010;  //认为是int自动转long，大于2^31，超过int的范围，编译失败:过大的整数
        }
}
```

***

#### 浮点类型：float、double

与整数类型类似，Java 浮点类型也有固定的表数范围和字段长度，不受具体操作系统的影响。
![](C:/NoteBook/pictures/338745915245081.png =482x)

Java 的浮点型常量默认为double型，声明float型常量，须后加‘f’或‘F’。
double 可以加d/D，也可以不加

浮点型常量有两种表示形式：

- 十进制数形式：如：
  - 5.12
  - 512.0f
  - .512 (必须有小数点）
- 科学计数法形式：如：
  - 5.12e2
  - 512E2
  - 100E-2

##### float:单精度，尾数可以精确到7位有效数字。很多情况下，精度很难满足需求

##### double:双精度，精度是float的两倍。通常采用此类型

```
public class Test{
        public static void main(String[] args){
                double d1 = 123.3;
                System.out.println(d1);
                
                float f1 = 12.3F;   //声明float型常量，须后加‘f’或‘F’。否则默认double
                System.out.println(f1);
                //float f2 = 12.3  //编译失败;被认为是自动类型转换，但不能自动大转小
        }
}
```

***

#### float与long的注意

- 整型常数，默认为int型
- 浮动型常数，默认为double型

因此

```
long l2 = 123456789010;  
//认为是int自动转long，大于2^31，超过int的范围，编译失败:过大的整数

float f2 = 12.3 
 //编译失败;被认为是自动类型转换，但不能自动大转小
```

***

#### 字符类型：char

char 型数据用来表示通常意义上“字符”(2字节)
Java中的所有字符都使用Unicode编码，故一个字符可以存储一个字母，一个汉字，或其他书面语的一个字符。  

字符型变量的三种表现形式：

- 字符常量是用单引号(‘ ’)括起来的单个字符。只能写一个字符,且不能为空(可以是空格):
  - char c1 = 'a';
  - char c2 = '中';
  - char c3 = '9';
- 也可以不使用单引号 ' ' 即ASCII, //非常少见  

```
public class Test{
    public static void main(String[] args){
        char c1 = 'a'; 
        char c2 = 97;  //ASCII //'a'
        System.out.println(c2);  //a
        
        char c3 = 5; //ASCII //口
        char c4 = '5'; // 5
        System.out.println(c3);   //口
    }
}
```

- Java中还允许使用转义字符‘\’来将其后的字符转变为特殊字符型常量。例如：
  - char c3 = ‘\n’; // '\n'表示换行符
  - ![](C:/NoteBook/pictures/94890716226322.png =149x)
- 直接使用 Unicode 值来表示字符型常量：‘\uXXXX’。其中，XXXX代表一个十六进制整数。
  - 如：\u000a 表示 \n。

char类型是可以进行运算的。因为它都对应有Unicode码。

```
public class Test{
        public static void main(String[] args){
                char c1 = 'a';
                System.out.println(c1);
                char c2 = '中';
                System.out.println(c2);
                char c3 = '9'
                System.out.println(c3);

                char c4 = '一'; //可以是日语,韩文等
                System.out.println(c4);

                char c5 = '\n';
                System.out.print("NNN" + c5);
                System.out.print("YYY");

                char c6 = '\u0043';
                System.out.println(c6);
        }
}
```

***

#### 布尔类型：boolean

boolean 类型用来判断逻辑条件，一般用于程序流程控制：

- if条件控制语句；
- while循环控制语句；
- do-while循环控制语句；
- for循环控制语句；

boolean类型数据只允许取值true和false，无null。

- 不可以使用0或非 0 的整数替代false和true，这点和C语言不同。
- Java虚拟机中没有任何供boolean值专用的字节码指令，Java语言表达所操作的boolean值，在编译之后都使用java虚拟机中的int数据类型来代替：true用1表示，false用0表示。———《java虚拟机规范 8版》

```
public class Test{
        public static void main(String[] args){
                boolean bb1 = true;
                System.out.println(bb1);

                boolean isMarried = true;
                if(isMarried){
                        System.out.println("你就有个小可爱了！");
                }
                else{
                        System.out.println("你可真可怜!");
                }
        }
}
```

***

### 基本数据类型变量间转换

#### 自动类型提升

容量(数的范围)小的类型自动转换为容量大的数据类型。
数据类型按容量大小排序为：  
![](C:/NoteBook/pictures/153093916248762.png =701x)

有多种类型的数据混合运算时，系统首先自动将所有数据转换成容量最大的那种数据类型，然后再进行计算。

- byte,short,char之间不会相互转换，他们三者在计算时首先转换为int类型。
  - 不兼容的错误，可能损坏数据，编译不予通过
- boolean类型不能与其它数据类型运算。
- 当把任何基本数据类型的值和字符串(String)进行连接运算时(+)，基本数据类型的值将自动转化为字符串(String)类型。

```
public class Test{
        public static void main(String[] args){
                //byte
                byte b1 = 12;
                int i1 = 123;
                //byte b2 = b1 + i1; 超过范围
                int i2 = b1 + i1;
                System.out.println(i2);

                float f1 = b1 + i1;
                System.out.println(f1);
                
                //char
                char c1 = 'a';  //97 ASCII
                int i3 = 10;
                int i4 = c1 + i3;
                System.out.println(i4);  //107
                
                //三者之间byte/short/char 不兼容的错误，可能损坏数据，编译不予通过
                /*
                byte b01 = 1;
                char c01 = 'a';
                short s01 = 1;
                byte b02 = b01 + c01;
                */
        }
}
```

##### 方式时机

###### 1)变量赋值

当为变量赋值的数据类型与变量类型不一致，并且赋值的数据类型级别低于变量类型的级别时，自动数据类型转换会将赋值数据自动转换为变量的类型。

```
byte byteNum=23;                           // 声明byte类型变量
int intNum=byteNum;                      // 把byte型数据赋值给int型变量
```

###### 2)方法调用

```
public static void say(int num){
   System.out.println("这是int类型数值："+num);
}
public static void main(String[] args) {
   byte byteNum=23;
   say(byteNum);        // 把byte数值传递给方法的int参数
}
```

###### 3) 一元运算

在一元运算中（除了++和—运算符），如果操作数的类型低于int类型（即byte、short和char类型），则操作数会自动转换为int类型。

- i++ 和 ++i的区别
1.赋值

```
i++ 先赋值在运算,例如 a=i++,先赋值a=i,后运算i=i+1,所以结果是a==1
++i 先运算在赋值,例如 a=++i,先运算i=i+1,后赋值a=i,所以结果是a==2
```

2.运算

```
public class Test009 {
	public static void main(String args[]) {
		int i = 1;
		System.out.println("   " + i++);
		i = 1;
		int num = i++ * 3;
		System.out.println(num); // 3
	}
}
```
###### 4)二元运算符

在二元运算中，所有低级的数据类型都会转换为运算中级别最高的数据类型。也就是说，如果有一个操作数是double类型，其他操作数都会自动转换为double类型。如果运算中最高的数据类型是float，其他操作数都会转换为float类型。如果最高的数据类型为long，那么所有操作数都会自动转换为long类型。如果最高的数据类型为int那么所有操作数都会自动转换成int类型。至于byte、short、char类型会自动转换为int或运算中最高的数据类型。

##### 在项目中创建类Conversion，在主方法中创建不同数值型的变量，实现将各变量的自动类型转换

```
public class Conversion{
        public static void main(String[] args){
                byte b1 = 34;
                short s1 = 123;
                char c1 = 'B';
                int i1 = 40;
                long l1 = 900L;
                float f1 = 8.11f;
                double d1 = 3.1415926;
                System.out.println("double/byte+char->double\t"+(d1/b1+c1));
                System.out.println("lobng+short->long\t\t"+(11+s1));
                System.out.println("char+byte+short->int\t\t"+(c1+b1+s1));
                System.out.println("float+b1/s1->float\t\t"+(f1+b1/s1));
                System.out.println("int+double->double\t\t"+(i1+d1));
        }
}
```

#### 强制类型转换

自动类型转换的逆过程

```
(数据类型)变量
强转符()   
```

但可能造成精度降低或溢出
![](C:/NoteBook/pictures/228581200247119.png =843x)

```
public class Test{
        public static void main(String[] args){
                double d1 = 12.3;
                int i1 = (int)d1;
                System.out.println(i1);
                
                //转为byte较特殊 超过精度范围的:
                int i2 = 128;
                byte b1 = (byte)i2;   //-128
                System.out.println(b1);
                int i3 = 200;
                byte b2 = (byte)i3;
                System.out.println(b2);  //-56
                int i4 = 2000;
                byte b3 = (byte)i4;
                System.out.println(b3);  //-48
        }
}
```

***

#### boolean类型不可以转换为其它的数据类型

***

### 基本数据类型与String间转换

#### 字符串类型：String

String不是基本数据类型，属于引用数据类型

- 使用方式与基本数据类型一致。例如：
  - String str = “abcd”;
- 一个字符串可以串接另一个字符串，也可以直接串接其他类型的数据。例如：
  - str = str + “xyz” ;
  - int n = 100;
  - str = str + n;

```
public class StringTest{
        public static void main(String[] args){
                String s1 = "Hellp World !";
                System.out.println(s1);
                String s2 = "a";
                String s3 = "";  //string可以为空

                //char c = '';  //char型不能为空
        }
}
```

- String 可以和其他8种基本数据类型做运算，且运算只能是连接运算 +

```
public class StringTest{
        public static void main(String[] args){
                String idstr = "编号：";
                int id = 19;
                String info = idstr + id;
                System.out.println(info);

                //Stirng test01 = idstr - id;    //编译不通过
                //String test02 = idstr - info;   //编译不通过
        }
}
```

#### + 加号与连接符的判断

- char型与int/float等从左到右更近，则+为加法
- char型与String从左到右更近，则+为连接符
  - 连锁反应：于String相连接后变String

```
public class StringTest{
        public static void main(String[] args){
                char c = 'a';
                int num = 10;
                String str = "Hello";

                System.out.println(c + num + str); //107Hello
                System.out.println(c + str + num); //aHello10
                System.out.println(c + (num + str)); //a10Hello
                System.out.println((c + num) + str); //107Hello
                System.out.println(str + num + c); //Hello10a
        }
}
```

```
public class StringTest{
        public static void main(String[] args){
                System.out.println("*   *"); //*       *
                System.out.println('*' + '\t' + '*');  //93
                System.out.println('*' + '\t' + "*");  //51*
                System.out.println('*' + "\t" + '*');   //*     *
                System.out.println('*' +( '\t' + "*")); //*     *
        }
}
```

```
String str1 = 4; //判断对错：no
String str2 = 3.5f + ""; //判断str2对错：yes 
System.out.println(str2); //输出："3.5"//3.5
System.out .println(3+4+"Hello!"); //输出：7Hello!
System.out.println("Hello!"+3+4); //输出：Hello!34
System.out.println('a'+1+"Hello!"); //输出：98Hello!
System.out.println("Hello"+'a'+1); //输出：Helloa1

判断是否能通过编译
1）
      short s = 5;
      s = s - 2; //判断：no  //2 默认int 自动转换不能大转小
2） 
      byte b = 3;
      b = b + 4; //判断：no  //4 默认int
      b = (byte)(b + 4); //判断：yes
3）
      char c = ‘a’;
      int i = 5;
      float d = .314F;
      double result = c + i + d; //判断：yes
4） 
      byte b = 5;
      short s = 3;
      short t = s + b; //判断：no  // short/byte/char之间不能转换
```

#### String字符串不能直接转换为基本类型，但通过基本类型对应的包装类则可以实现把字符串转换成基本类型

如：

```
String a = "43"; 
int i = Integer.parseInt(a);
```

***

### 进制与进制间转换

#### 进制

所有数字在计算机底层都以二进制形式存在。  
对于整数，有四种表示方式：  

- 二进制(binary)：0,1 ，满2进1.
  - 以0b或0B开头。
- 十进制(decimal)：0-9 ，满10进1。
- 八进制(octal)：0-7 ，满8进1.
  - 以数字0开头表示。
- 十六进制(hex)：0-9及A-F，满16进1.
  - 以0x或0X开头表示。此处的A-F不区分大小写。
  - 如：0x21AF +1= 0X21B0

![](C:/NoteBook/pictures/23553120220658.png =554x)
![](C:/NoteBook/pictures/291703120239084.png =540x)

```
 public class BinaryTest{
        public static void main(String[] args){
                int num1 = 0b110;
                int num2 = 110;
                int num3 = 0127;
                int num4 = 0x110A;
                
                System.out.println(num1); //6  //1 * 2^2 + 1 * 2^1 + 1 * 2^0
                System.out.println(num2); //110
                System.out.println(num3); //87 //1 * 8^2 + 2 * 8^1 + 7 * 8^0
                System.out.println(num3); //4362 //1 * 16^3 + 1 * 16^2 +  0 * 16^1 + 10 * 16^0 
        }
}
```

***

#### 二进制

Java整数常量默认是int类型，

- 当用二进制定义整数时,int，其第32位是符号位；  
- 当是long类型时，二进制默认占64位，第64位是符号位

二进制的整数有如下三种形式：

- 原码：直接将一个数值换成二进制数。最高位是符号位
- 负数的反码：是对原码按位取反，只是最高位（符号位）确定为1。
- 负数的补码：其反码加1。

计算机以二进制补码的形式保存所有的整数。

- 正数的原码、反码、补码都相同
- 负数的补码是其反码+1

为什么要使用原码、反码、补码表示形式呢？  
计算机辨别“符号位”显然会让计算机的基础电路设计变得十分复杂! 于是人们想出了将符号位也参与运算的方法. 我们知道, 根据运算法则减去一个正数等于加上一个负数, 即: 1-1 = 1 + (-1) = 0 , 所以机器可以只有加法而没有减法, 这样计算机运算的设计就更简单了。

![](C:/NoteBook/pictures/520842515220659.png =690x)

#### 二级制转十进制

![](C:/NoteBook/pictures/227482615239085.png =767x)
![](C:/NoteBook/pictures/416642615226952.png =760x)

![](C:/NoteBook/pictures/538902615247118.png =762x)

#### 十进制转二进制

![](C:/NoteBook/pictures/85662715239787.png =767x)

![](C:/NoteBook/pictures/238122715236342.png =768x)

![](C:/NoteBook/pictures/366432715232096.png =764x)

![](C:/NoteBook/pictures/573932715249976.png =769x)

![](C:/NoteBook/pictures/250802815245082.png =766x)

![](C:/NoteBook/pictures/134702815247580.png =764x)

![](C:/NoteBook/pictures/540332815226323.png =766x)

![](C:/NoteBook/pictures/84532915248763.png =767x)

#### 进制间转化

- 十进制 二进制互转
 - 二进制转成十进制 乘以2的幂数
 - 十进制转成二进制 除以2取余数
- 二进制 八进制互转
- 二进制 十六进制互转
- 十进制 八进制互转
- 十进制 十六进制互转

![](C:/NoteBook/pictures/153723815243899.png =532x)

![](C:/NoteBook/pictures/468893815237445.png =766x)

![](C:/NoteBook/pictures/4453915230579.png =573x)

***

### 声明常量 final

在程序运行过程中一直不会改变的量称为常量（constant），通常也被称为“final变量”。常量在整个程序中只能被赋值一次。在为所有对象共享值时，常量是非常有用的。

在Java语言中声明一个常量，除了要指定数据类型外，还需要通过final关键字进行限定。声明常量的标准语法为：

```
final 数据类型 常量名称[=值]
```

常量名通常使用大写字母，这也符合Java编码规范，但这并不是必须的，声明常量时，完全可以和变量名一样使用小写字母。但是，那样容易造成混淆，降低代码可读性，所以才有规范要求常量名全部大写，并使用“_”下划线字符分割多个单词

- 当定义的final变量属于“成员变量”，则必须在定义的时候就设定它的初值。否则将会有编译错误。

```
public class Part{
        static final double PI = 3.14;
        static int age = 23;
        public static void main(String[] args){
                final int number;
                number = 1235;
                age = 22;
                //number = 1111;    //number final变量只能赋值一次
                System.out.println(PI);   //3.14
                System.out.println(number);  //1235
                System.out.println(age);   //22
        }
}
```

***

### 变量的有效范围

由于变量被定义出来后，只是暂存在内存中，等到程序执行到某一个点后，该变量会被释放掉，也就是说变量有它的生命周期。因此变量的有效范围是指程序代码能够访问该变量的区域，若超出该区域访问变量则编译时会出现错误。在程序中，一般会根据变量的有效范围，将变量分为“成员变量”和“局部变量”。

#### 成员变量

在类体中所定义的变量被称为成员变量。成员变量在整个类中都有效，所以成员变量也称作全局变量。

成员变量的有效范围是整个类的代码段，也就是在类体中的任何位置都可以使用该变量。

声明成员变量，并在不同方法中为成员变量赋值

```
public class Temp{
        int num = 0;
        float price = 3.50f;
        public void method01(){
                num = 1;
                price = 12.55f;
        }
        public void method02(){
                num = num * 10;
                price = price * 0.5f;
        }
}
```

- 类的成员变量又可分为静态变量和实例变量两种。

##### 静态变量 static

- 静态变量的有效范围是整个类，并且可以被类的所有实例共享。可通过“类名.静态变量名”的方式来访问静态变量。
- 静态变量的生命周期取决于类的生命周期，当类被加载时，为类中的静态变量分配内存空间，当卸载类时，释放静态变量占用的空间，静态变量被销毁。
- 类加载时，就为静态变量分配内存空间，之后无论创建了多少类的实例，都不会再为静态变量分配内存空间，这些实例会使用同一个静态变量。

```
 static int y = 90;   //定义静态变量
```

##### 实例变量

- 实例变量与类的实例对应，它的有效范围是整个实例。
- 每创建一个类的实例，都会为当前实例分配实例变量的内存空间。
- 所以实例变量的生命周期取决于实例的生命周期，实例被创建时，为实例变量分配内存空间，当销毁实例时，释放实例变量占用的内存空间。

```
int x = 45;          //定义实例变量
```

#### 局部变量

- 在方法体中定义的变量就是一种局部变量，局部变量只在当前代码块（即花括号之内）中有效。
- 局部变量的生命周期取决于声明位置的代码块，超出这个代码块的范围就不能在使用代码块内的局部变量。

就拿方法类说，方法内声明的变量、包括参数变量都属于局部变量，当方法被调用时，Java虚拟机为方法中的局部变量分配内存空间，当该方法的调用结束后，则会释放方法中局部变量占用的内存空间，局部变量也将会销毁，然后Java的垃圾回收机制会在某一时刻清理该内存空间。

![](C:/NoteBook/pictures/596674923239085.png)

##### 局部变量可与全局变量的名字相同，此时全局变量将被隐藏，但是可以使用“this.”做前缀来访问全局变量

```
public class ValClass{
        static String words = "成员变量";
        public static void main(String[] args){
                String words = "局部变量";
                System.out.println("words变量现在是:" + words); //局部变量
        }
}
```

***

## 运算符

### 算术运算符

![](C:/NoteBook/pictures/432854115221109.png =611x)

#### 算术运算符的注意问题

##### 如果对负数取模，可以把模数负号忽略不记

- 如：5%-2=1。
- 但被模数是负数则不可忽略。
- 此外，取模运算的结果不一定总是整数。

##### 对于除号“/”，它的整数除和小数除是有区别的：整数之间做除法时，只保留整数部分而舍弃小数部分

- 例如：
- int x = 3510;
- x=x / 1000 * 1000;  
- x的结果是3

##### “+”除字符串相加功能外，还能把非字符串转换成字符串.例如

- System.out.println(“5+5=”+ 5 + 5); //打印结果是 5+5=55  //字符串

##### 自增1/自减1不会改变本身变量的数据类型

```
short s1

s1 = s1 +1 //编译失败
应该是
s1 = (short)(s1 +1);

整数默认是int
```

##### 超过范围的++

```
byte b1 = 127;
b1++;
System.out.println(b1);  //-128  
```

##### 自增2 (num++)++ //编译失败

```
num += 2;
```

#### 练习1：除 转换

```
public class SignTest{
        public static void main(String[] args){
                int num01 = 12;
                int num02 = 5;

                int result01 = num01 / num02;
                System.out.println(result01);   //2

                int result02 = num01 / num02 * num02;
                System.out.println(result02);   //10

                double result03 = num01 / num02;
                System.out.println(result03);  //2.0

                double result04 = num01 / num02 + 0.0;
                System.out.println(result04);   //2.0

                double result05 = num01 / (num02 + 0.0);
                System.out.println(result05);   //2.4

                double result06 = (double)num01 / num02;
                System.out.println(result06);   //2.4

                double result07 = (double)(num01 / num02);
                System.out.println(result07);   //2.0
        }
}
```

#### 练习2：随意给出一个整数，打印显示它的个位数，十位数，百位数的值

```
格式如下：
数字xxx的情况如下：
个位数：
十位数：
百位数：
例如：
数字153的情况如下：
个位数：3
十位数：5
百位数：1
```

```
public class NumberPrint{
        public static void main(String[] args){
                int num = 123;
                int a = num / 100;
                int b = (num % 100 ) / 10;
                int c = num % 10;
                System.out.println("数字" + num + "的情况如下:");
                System.out.println("百位数:" + a );
                System.out.println("十位数:" + b );
                System.out.println("个位数:" + c );
        }
}
```

#### 练习3：算术运算符：自加、自减

```
public class SignTest{
        public static void main(String[] args){
                int i = 1;
                System.out.println("i\t" + i);
                System.out.println("i++\t" + (i++));
                i = 1;
                System.out.println("++i\t" + (++i));
                i = 1;
                System.out.println("i--\t" + (i--));
                i = 1;
                System.out.println("--i\t" + (--i));
        }
}

i       1
i++     1                                                                                                               ++i     2
i--     1
--i     0  
```

```
public class SignTest{
        public static void main(String[] args){
                int i1 = 10;
                int i2 = 20;
                int i = i1++;
                System.out.println("i=" + i); 
                System.out.println("i1=" + i1);
                i = ++i1;
                System.out.println("i=" + i);
                System.out.println("i1=" + i1);
                i = i2--;
                System.out.println("i=" + i);
                System.out.println("i2=" + i2);
                i = --i2;
                System.out.println("i=" + i);
                System.out.println("i2=" + i2);
        }
}

i=10
i1=11
i=12
i1=12
i=20
i2=19
i=18
i2=18;   
```

#### 为抵抗洪水，战士连续作战89小时，编程计算共多少天零多少小时？

```
public class TimeCount{
        public static void main(String[] args){
                int workTime = 89;
                int day = workTime / 24;
                int hour = workTime % 24;

                System.out.println("共" + day + "天" + hour + "小时");
        }
}
```

#### 今天是周二，100天以后是周几？

```
public class TimeCount{
        public static void main(String[] args){
                int days = 100;
                int weekday = days % 7 + 2;
                System.out.println("周" + weekday);
        }
}
```

#### 总结

```
class SuanShu{
 public static void main(String[] args){
  int a = 23;
  int b = 12;
  System.out.println(a + "+" + b + "=" + (a+b));
  int sum = a + b;
  System.out.println(a + "+" + b + "=" + sum);
  
  int sub = a - b;
  //System.out.println(a + "-" + b + "=" + a-b);
  //错误，原因是a + "-" + b + "=" + a的结果是字符串，字符串不能进行减法
  System.out.println(a + "-" + b + "=" + (a-b));
  System.out.println(a + "-" + b + "=" + sub);
  
  int mul = a * b;
  System.out.println(a + "*" + b + "=" + a*b);
  System.out.println(a + "*" + b + "=" + mul);
  
  //整数相除，只保留整数部分
  int div = a / b;
  System.out.println(a + "/" + b + "=" + a/b);
  System.out.println(a + "/" + b + "=" + div);
  
  double d = (double)a/b;//先把a的类型进行转换，转换成double类型，然后再和b相除
  System.out.println(a + "/" + b + "=" + d);
  
  int yu = a % b;
  System.out.println(a + "%" + b + "=" + yu);
  
  System.out.println("------------------特殊的取模----------------------");
  System.out.println(5%2);//1
  System.out.println(-5%-2);//-1
  System.out.println(-5%2);//-1
  System.out.println(5%-2);//1
  
  System.out.println("------------------负号----------------------");
  int num1 = 12;
  int num2 = -num1;
  System.out.println("num2=" + num2);
  
  System.out.println("------------------自增----------------------");
  int i = 0;
  System.out.println("自增之前i=" + i);
  i++;
  System.out.println("自增第一次之后i=" + i);
  ++i;
  System.out.println("自增第二次之后i=" + i);
  int j = ++i;//把i自增1，然后结果赋值给j，或者说，先算++i，然后再赋值
  System.out.println("自增第三次之后i=" + i);
  System.out.println("j=" + j);
  int k = i++;//先算赋值，把i的值赋值给k，i原来是3，把3赋值给k，然后i在自增1，i变成4
  System.out.println("自增第四次之后i=" + i);
  System.out.println("k=" + k);
  
  //面试题：陷阱题
  i = i++;//先赋值，把i原来的值重新赋值给i，不变，然后i自增，但是这个自增后的值没有在放回变量i的位置
  System.out.println("自增第五次之后i=" + i);
 }
}

```

#### 小明要到美国旅游，可是那里的温度是以华氏度为单位记录的。它需要一个程序将华氏温度（80度）转换为摄氏度，并以华氏度和摄氏度为单位分别显示该温度

![](C:/NoteBook/pictures/575720823220662.png =645x)

```
public class ChangeCF{
        public static void main(String[] args){
                double celsius;
                int fahrenheit = 80;

                celsius = (fahrenheit - 32) / 1.8;
                System.out.println("华氏度：" + fahrenheit);
                System.out.println("摄氏度：" + celsius);
        }
}
```

### 赋值运算符

符号：=
当“=”两侧数据类型不一致时，可以使用自动类型转换或使用强制类型转换原则进行处理。
支持连续赋值。

```
public class SetValueTest{
        public static void main(String[] args){
                int i,j;
                i = j = 1;
                System.out.println("i=" + i + "|" + "j=" + j);
        }
}

i=1|j=1
```

#### 扩展赋值运算符： +=, -=, *=, /=, %=

##### 不会改变本身数据类型

而`s1 = s1 + 10` 改变//整数默认int

```
public class SetValueTest{
        public static void main(String[] args){
                int num;
                num = 10;
                num += 2;
                System.out.println(num);

                num = 10;
                num = num + 2;
                System.out.println(num);

                short s1 = 10;
                //s1 = s1 + 2; 编译失败
                s1 += 2;
                System.out.println(s1);
        }
}
```

#### 思考1

```
short s = 3; 
s = s + 2; ① //编译失败
s += 2; ②
①和②有什么区别？
```

#### 思考2

```
int i = 1;
i *= 0.1;
System.out.println(i); //0
i++;
System.out.println(i); //1
```

#### 思考3

```
int m = 2;
int n = 3;
n *= m++;  //2 * 3 == 6
System.out.println("m=" + m); //3
System.out.println("n=" + n); //6
```

#### 思考4：自增/自减的运算在同一行，运算不是全部按照原来的数一个个运算，而是按顺序在前面运算后的基础上运算

```
int n = 10;
n += (n++) + (++n);  //10 + 12 + 10 == 32
System.out.println(n);  //32
```

### 比较运算符（关系运算符）

- 比较运算符的结果都是boolean型，也就是要么是true，要么是false。
  - 不是0，1
- 比较运算符“==”不能误写成“=” 。
- ==  != 不仅能使用在数值类型之间，还可以使用在其他引用类型变量之间

```
Account acct1 = new Account(1000)

Account acct2 = new Account(1000)

boolean b1 = (acct1 == actt2) //比较两个Account是否是同一个账户 而不是有多少钱
//false
```

- `> < >= <=` 只能使用在数值类型的数据中之间

![](C:/NoteBook/pictures/371082210220660.png =772x)

#### 思考1

```
boolean b1 = false;
//区分好==和=的区别。
if(b1==true){
    System.out.println("结果为真");
}
else{
    System.out.println("结果为假");
}
//结果为假

if(b1=true){    //有误，但编译通过
    System.out.println("结果为真");
}
else{
    System.out.println("结果为假");
}
//结果为假
```

***

### 逻辑运算符

- & —逻辑与
- | —逻辑或
- ！—逻辑非
- && —短路与
- || —短路或
- ^ —逻辑异或

![](C:/NoteBook/pictures/331722510239086.png =608x)

逻辑运算符用于连接布尔型表达式，在Java中不可以写成`3<x<6`，应该写成`x>3 & x<6`。

- "&”和“&&”的区别：
  - 单&时，左边无论真假，右边都进行运算；
  - 双&时，如果左边为真，右边参与运算，如果左边为假，那么右边不参与运算。
- "|”和“||”的区别同理，
  - ||表示：当左边为真，右边不参与运算。
- 异或( ^ )与或( | )的不同之处是：
  - 当左右都为true时，结果为false。
  - 理解：异或，追求的是“异”!

```
public class LogicTest{
        public static void main(String[] args){
                boolean b1 = true;
                int num1 = 10;
                if(b1 & (num1++ > 0)){
                        System.out.println("北京");
                }
                else{
                        System.out.println("南京");
                }
                System.out.println("num1=" + num1);

                boolean b2 = true;
                int num2 = 10;
                if(b2 && (num2++ > 0)){
                        System.out.println("北京");
                }
                else{
                        System.out.println("南京");
                }
                System.out.println("num2=" + num2);

                boolean b3 = false;
                int num3 = 10;
                if(b3 & (num3++ > 0)){
                        System.out.println("北京");
                }
                else{
                        System.out.println("南京");
                }
                System.out.println("num3=" + num3);

                boolean b4 = false;
                int num4 = 10;
                if(b4 && (num4++ > 0)){
                        System.out.println("北京");
                }
                else{
                        System.out.println("南京");
                }
                System.out.println("num4=" + num4);
        }
}

北京
num1=11
北京
num2=11
南京
num3=11
南京
num4=10 
```

#### 练习：请写出每题的输出结果

##### 1

```
int x = 1;
int y = 1;
if(x++ == 2 & ++y == 2){
    x = 7;
}
System.out.println("x=" + x + ",y=" + y);  
//x=2,y=2
```

##### 2

```
int x = 1,y = 1;
if(x++ == 2 && ++y == 2){
    x = 7;
}
System.out.println("x=" + x + ",y=" + y);
//x=2,y=1
```

##### 3

```
int x = 1,y = 1;
if(x++ == 1 | ++y == 1){
    x = 7;
}
System.out.println("x=" + x + ",y=" + y);
//x=7,y=2
```

##### 4

```
int x = 1,y = 1;
if(x++ == 1 || ++y == 1){
    x = 7;
}
System.out.println("x=" + x + ",y=" + y);
//x=7,y=1
```

#### 【面试题】程序输出

```
class Test {
    public static void main (String[] args) {
        boolean x = true;
        boolean y = false;
        short z = 42;
        //if(y == true)
        if((z++ == 42)&&(y = true)) z++; //z == 44;y == true
        if((x = false) || (++z == 45)) z++;  //x == false;z == 46
        
        System. out.println(“z=”+z);  
    }
}

结果为：z=46

```

***

### 位运算符

#### 位运算是直接对整数的二进制进行的运算

- 位运算符操作的都是整型的数据
- 注意数据类型的长度
![](C:/NoteBook/pictures/310843610226953.png =625x)

![](C:/NoteBook/pictures/257723710239788.png =602x)

```
public class BitTest{
        public static void main(String[] args){
                byte num = 2;
                System.out.println("num << 8 = " + (num << 8));
        }
}

num << 8 = 512 ???
```

#### <<  乘2的n次方

- 在范围内，`<< n`即`乘2的n次方`
- 超过范围，负数...

```
public class BitTest{
        public static void main(String[] args){
                int i = 21;

                System.out.println("i << 2 :" + (i << 2));
                System.out.println("i << 27 :" + (i << 27));
        }
}

i << 2 :84
i << 27 :-1476395008
```

##### 最高效计算2*8

- 注：乘数中要有一个是2的倍数

```
public class BitTest{
        public static void main(String[] args){
                int num = 2;
                System.out.println("2 * 8 = "+ (num << 3));
        }
}

public class BitTest{
    public static void main(String[] args){
        System.out.println("2 * 8 = " + (8 << 1));
    }
}
```

#### >> 除以2的n次方

- 在范围内，`>>n`即`除以2的n次方`

#### >>>

#### & 有0则0，都1则1

```
public class BitTest{
        public static void main(String[] args){
                System.out.println(2 & 5);
        }
}
//0

//2  ...010
//5  ...101
//...000
```

#### | 有1则1，都0则0

```
public class BitTest{
        public static void main(String[] args){
                System.out.println(2 | 5);
        }
}
//7

//2  ...010
//5  ...101
//..111
```

#### ^ 不同则1，同则0

```
public class BitTest{
        public static void main(String[] args){
                System.out.println(2 ^ 5);
        }
}
//7
//2  ...010
//5  ...101
//...111
```

```
public class BitTest{
        public static void main(String[] args){
                System.out.println(1 ^ 5);
                System.out.println(1 | 5);
        }
}
// ^ ...100 //4
// | ...101 //5

//1  ...001
//5  ...101

```

#### ~ 取反

```
public class BitTest{
        public static void main(String[] args){
                System.out.println(~6);
        }
}
//-7
```

![](C:/NoteBook/pictures/485374314239086.png =585x)

#### 如何求一个0~255范围内的整数的十六进制值，例如60的十六进制表示形式3C

```
//方式一：自动实现
public class BitTest{
        public static void main(String[] args){
                String str1 = Integer.toBinaryString(60);
                String str2 = Integer.toHexString(60);

                System.out.println(str1);
                System.out.println(str2);
        }
}

//方式二：手动实现
public class BitTest{
        public static void main(String[] args){
            int i1 = 60;      //0111100
            int i2 = i1 & 15; //0001111
            String j = (i2 > 9)? (char)(i2-10 + 'A')+"" : i2+"";
            int temp = i1 >>> 4;
            i2 = temp & 15;
            String k = (i2 > 9)? (char)(i2-10 + 'A')+"" : i2+"";
            System.out.println(k+""+j);
        }
}

```

#### 练习 交换两个变量的值

```
public class ExchangeNum{
        public static void main(String[] args){
                int num01 = 10;
                int num02 = 20;

                //方法1  定义临时变量
                int temp;
                temp = num01;
                num01 = num02;
                num02 = temp;
                System.out.println("num01=" + num01);
                System.out.println("num02=" + num02);
                
                //方法2  不用定义临时变量;但是可能超过数据范围;有局限性,只能是数值型
                num01 = num01 + num02;
                num02 = num01 - num02;
                num01 = num01 - num02;
                System.out.println("num01=" + num01);
                System.out.println("num02=" + num02);
                
                //方法3 ^ 不用定义临时变量;但是可能超过数据范围;有局限性,只能是数值型
                num01 = (num01 ^ num02);
                num02 = (num01 ^ num02);
                num01 = (num01 ^ num02);
                System.out.println("num01=" + num01);
                System.out.println("num02=" + num02);

        }
}
```

#### 错误 实现加法 sum = (num01 ^ num02) | num01 | num02 错误

```
public class AddNum{
        public static void main(String[] args){
                int num01 = 12;
                int num02 = 20;
                int sum;

                sum = (num01 ^ num02) | num01 | num02;
                System.out.println("sum=" + sum);
        }
}

//32
/*
num01 ^ num02
...01100 ^ 
...10100  == ...11000

(num01 ^ num02) | num01 | num02
...11000 |
...01100 == 
...11100 |
...10100 == ...11100
*/
```

#### 实现减法

```

/*
num01 ^ num02
...01100 ^ 
...10100  == ...11000

*/
```

#### 实现乘法

```

```

#### 实现除法

```

```

***

### 三元运算符

```
(条件表达式)?表达式1：表达式2；
```

- 条件表达式的结果为boolean类型
- 条件表达式为true，运算后的结果是表达式1；
- 条件表达式为false，运算后的结果是表达式2；  
- 表达式1和表达式2为要求是同种类型, 或者可以自动转换的
- 可以嵌套使用

```
public class SanYuan{
        public static void main(String[] args){
                int m = 12;
                int n = 5;

                int max = (m > n)? m : n;
                System.out.println(max);
                
                double num = (m > n)? 1 : 1.0;
                System.out.println(num);
                
                //(m > n)? 1:"你好"; //无法自动转换，类型不统一//编译错误
                
                String maxStr = (m > n)? "m大":"n大";
                System.out.println(maxStr);
                
                int max = (((m > n)? m : n) == m)? m : n;
                System.out.println(maxNum);
        }
}
```

#### 获取三个数的最大值

```
public class SanYuan{
        public static void main(String[] args){
                int x = 12;
                int y = 5;
                int z = 9;
                //
                int maxNum = (x > y)? x : y;
                int maxNum = (maxNum > z)? maxNum : z;
                System.out.println(maxNum);
                //
                int max = (((x > y)? x : y) > z)? ((x > y)? x : y) : z;
                System.out.println(max);
        }
}
```

#### 要求用户输入两个数a和b，如果a能被b整除或者a加b大于1000，则输出a；否则输出b

```
import java.util.Scanner;

public class SangYuanTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.print("a: ");
                int a = scan.nextInt();
                System.out.println();
                System.out.print("b: ");
                int b = scan.nextInt();
                System.out.println();

                System.out.println(((a % b == 0) || (a + b > 1000))? a : b);
        }
}
```

#### 使用条件结构实现，如果用户名等于字符‘青’，密码等于数字‘123’，就输出“欢迎你，青”，否则就输出“对不起，你不是青”。 ???

提示：先声明两个变量，一个是char型的，用来存放用户名，一个是int型的，用来存放密码。

```
import java.util.Scanner;

public class SangYuanTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.print("user: ");
                String user = scan.next();
                System.out.println();
                System.out.print("password: ");
                int password = scan.nextInt();
                System.out.println();

                System.out.println(((user == "青") && (password == 123))? "欢迎你！青" : "对不起，你不是青");
        }
}
```

#### 三元运算符与if-else的联系与区别

1. 三元运算符可简化if-else语句,优先三元运算符
2. 三元运算符要求必须返回一个结果。
3. if后的代码块可有多个语句

***

### 运算符的优先级

运算符有不同的优先级，所谓优先级就是表达式运算中的运算顺序。如表，上一行运算符总优先于下一行。

![](C:/NoteBook/pictures/433304110226324.png =378x)

只有单目运算符、三元运算符、赋值运算符是从右向左运算的。

#### 考查运算符的优先级

```
写出输出的结果.
class Demo{
    public static void main(String[] args){
        int x=0,y=1;
        if(++x==y-- & x++==1||--y==0){
            System.out.println("x="+x+",y="+y);   //x = 2,y = 0;
        }
        else{
         System.out.println("y="+y+",x="+x);
        }
     }
}
```

***

## 程序流程控制

### 键盘输入不同类型的变量 Scanner类

1. 导包 `import java.util.Scanner;`
2. Scanner的实例化 `Scanner scan = new Scanner(System.in);`
3. 调用Scanner类的相关方法，来获取指定类型的数据

- 需要根据相应的方法，来输入指定类型的数据，如果输入的数据类型与要求不匹配，报异常：.InputMismatchException，导致程序终止.
- 通过Scanner类的`next()`和`nextline()`方法获取输入的字符串，在读取前一般需要使用`hasNext()`和`hasNextLine()`判断是否还有输入的数据

#### 使用

```
import java.util.Scanner;

public class ScannerTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                int num = scan.nextInt();
                System.out.println(num);
        }
}
```

- scan.next() 字符串
- scan.nextInt() 整型
- scan.nextDouble() 浮点double
- scan.nextBoolean() 布尔  //true/false //而不是0/1
- 没有nextChar()

```
import java.util.Scanner;

public class ScannerTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.println("请输入姓名：");
                String name = scan.next();
                System.out.println(name);

                System.out.println("请输入年龄：");
                int age = scan.nextInt();
                System.out.println(age);

                System.out.println("请输入体重：");
                double weight = scan.nextDouble();
                System.out.println(weight);

                System.out.println("是否单身？");
                boolean isLove = scan.nextBoolean();
                System.out.println(isLove);

                //对于char型的获取。Scanner没有提供方法，只能获取一个字符串
                System.out.println("请输入性别 男/女");
                String gender = scan.next();
                char genderChar = gender.charAt(0); //获取索引位置上的字符
                System.out.println(genderChar);
        }
}
```

```
Exception in thread "main" java.util.InputMismatchException
        at java.base/java.util.Scanner.throwFor(Scanner.java:943)
        at java.base/java.util.Scanner.next(Scanner.java:1598)
        at java.base/java.util.Scanner.nextBoolean(Scanner.java:1898)
        at ScannerTest.main(ScannerTest.java:20)
```

#### next的用法

```
package com.cdoudou.scanner;

import java.util.Scanner;

public class Demo01 {
    public static void main(String[] args) {

        //创建一个扫描器对象，用于接收键盘数据
        Scanner scanner = new Scanner(System.in);
        System.out.println("用next的方式接受");

        //判断用户有没有输入字符串
        if(scanner.hasNext()){
            //使用next接收
            String str = scanner.next();
            System.out.println("输出内容为："+str);
        }

        //注意使用完要注意关掉  凡是从属于IO流的类，如果使用完不关闭会一直占用资源
        scanner.close();
    }
} 
```

#### nextLine()用法

```
package com.cdoudou.scanner;

import java.util.Scanner;

public class Demo02 {
    public static void main(String[] args) {

        Scanner scanner = new Scanner(System.in);
        System.out.println("使用nextLine接收字符串:");

        if(scanner.hasNextLine()){
            String str = scanner.nextLine();
            System.out.println("接受的内容为："+str);
        }
        scanner.close();


//        int sum = 0;
//        for (int i = 0; i < 10; i++) {
//            sum += i;
//            if(i % 3 == 0){
//                break;
//            }
//        }
//        System.out.println(sum);
    }
}
```

#### next() nextLine()

- next()

1. 一定要读取到有效字符后才可以结束输入
2. 对输入有效字符之前遇到的空白，next()方法会自动将其去掉
3. 只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符
4. next()不能得到带有空格的字符串

- nextLine()

1. 以Enter为结束符，也就是说nextLine()方法返回的是输入回车之前的所有字符
2. 可以获得空白

##### 也可以输入中文

```
package com.cdoudou.scanner;

import java.util.Scanner;

public class Demo03 {
    public static void main(String[] args) {

        Scanner scanner = new Scanner(System.in);
        System.out.println("使用nextLine接收字符串:");

        String str = scanner.nextLine();
        System.out.println("接受的内容为："+str);
        scanner.close();
        /*
        *   使用nextLine接收字符串:
            可以输入中文的
            接受的内容为：可以输入中文的
        * */
    }
}
```

#### Java 输入输出流 解决中文乱码问题

***

### 顺序结构

程序从上到下逐行地执行，中间没有任何判断和跳转。
Java中定义成员变量时采用合法的前向引用

```
public class Test{
    int num1 = 12;
    int num2 = num1 + 2;
}
错误形式：
public class Test{
    int num2 = num1 + 2；
    int num1 = 12;
}

```

### 分支结构

根据条件，选择性地执行某段代码。
有if…else和switch-case两种分支语句。

#### if...else

```
if(条件表达式){
    执行代码块;
}
```

```
if(条件表达式){
    执行代码块;
}
else{
    执行代码块;
}
```

```
if(条件表达式1){
    执行代码块;
}
else if(条件表达式2){
    执行代码块;
}
else{   //可以不要
    执行代码块;
}
```

- 条件表达式必须是布尔表达式（关系表达式或逻辑表达式）、布尔变量
- 语句块只有一条执行语句时，一对{}可以省略，但建议保留
- if-else语句结构，根据需要可以嵌套使用
- 当if-else结构是“多选一”时，最后的else是可选的，根据需要可以省略
- 当多个条件是“互斥”关系时，条件判断语句及执行语句间顺序无所谓
- 当多个条件是“包含”("例题1(2)")关系时，“小上大下 / 子上父下”
- 注意变量的赋值 在if外的变量应该要先赋值，再使用，如果不满足条件，则未赋值，编译不通过

```
public class Iftest{
        public static void main(String[] args){
                //例1
                int heartBeats = 79;
                if(heartBeats < 60 || heartBeats > 100){
                        System.out.println("需要进一步检查");
                }
                System.out.println("检查结束");

                //例2
                int age = 23;
                if(age < 18){
                        System.out.println("你只能看动画片");
                }
                else{
                        System.out.println("你还是不可以色色的哦！");
                }

                //例3
                if(age < 0){
                        System.out.println("输入数据不合法");
                }
                else if(age < 18){
                        System.out.println("小孩你好呀！");
                }
                else if(age < 30){
                        System.out.println("青壮年时期");
                }
                else if(age < 60){
                        System.out.println("人到中年");
                }
                else if(age < 120){
                        System.out.println("老年");
                }
                else{
                        System.out.println("你不对劲");
                }
        }
}
```

##### 例0

```
public class Iftest{
        public static void main(String[] args){
                int age = 75;
                if(age < 0){
                        System.out.println("不可能！还没出生！");
                }
                else if(age > 250){
                        System.out.println("老妖怪！！！");
                }
                else{
                        System.out.println("芳龄：" + age);
                }
        }
}
```

##### 例题1 成绩

```
岳小鹏参加Java考试，他和父亲岳不群达成承诺：
如果：
成绩为100分时，奖励一辆BMW；
成绩为(80，99]时，奖励一台iphone xs max；
当成绩为[60,80]时，奖励一个 iPad；
其它时，什么奖励也没有。
请从键盘输入岳小鹏的期末成绩，并加以判断
```

```
1)
import java.util.Scanner;

public class IfTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                int score = scan.nextInt();

                if(score > 80 && score <= 99){
                        System.out.println("iphone xs max");
                }
                else if(score >= 60 && score <=80){
                        System.out.println("iPad");
                }
                else{
                        System.out.println("Null");
                }
        }
}

2)
import java.util.Scanner;

public class IfTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.print("成绩为：");
                int score = scan.nextInt();

                System.out.println("奖励为：");
                if(score > 100){
                        System.out.println("作弊");
                }
                else if(score > 80){
                        System.out.println("iphone");
                }
                else if(score >= 60){
                        System.out.println("ipad");
                }
                else{
                        System.out.println("null");
                }
        }
}
```

##### 例题2 排序

```
编写程序：由键盘输入三个整数分别存入变量num1、num2、num3，
对它们进行排序(使用 if-else if-else),并且从大到小输出。
```

```
import java.util.Scanner;

public class IfTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.println("请输入数字1");
                int num1 = scan.nextInt();
                System.out.println("请输入数字2");
                int num2 = scan.nextInt();
                System.out.println("请输入数字3");
                int num3 = scan.nextInt();

                System.out.println("输出:");

                if(num1 >= num2 && num1 >= num3){
                        System.out.println("first:" + num1);

                        if(num2 >= num3){
                                System.out.println("seconde:" + num2);
                                System.out.println("third:" + num3);
                        }
                        else{
                                System.out.println("seconde:" + num3);
                                System.out.println("third:" + num2);
                        }
                }
                else if (num2 >= num1 && num2 >= num3){
                        System.out.println("first:" + num2);

                        if(num1 >= num3){
                                System.out.println("seconde:" + num1);
                                System.out.println("third:" + num3);
                        }
                        else{
                                System.out.println("seconde:" + num3);
                                System.out.println("third:" + num1);
                        }
                }
                else if(num3 >= num1 && num3 >= num2){
                        System.out.println("first:" + num3);

                        if(num1 >= num2){
                               System.out.println("seconde:" + num1);
                               System.out.println("third:" + num2);
                        }
                        else{
                                System.out.println("seconde:" + num2);
                                System.out.println("third:" + num1);
                        }
                }
                else{
                        System.out.println("有误！！！");
                }
        }
}

2)
import java.util.Scanner;

public class IfTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.print("第一个整数：");
                int num1 = scan.nextInt();
                System.out.print("第二个整数：");
                int num2 = scan.nextInt();
                System.out.print("第三个整数：");
                int num3 = scan.nextInt();

                if(num1 >= num2){
                        if(num3 >= num1){
                                System.out.println(num3 + "\t" + num1 + "\t" + num2);
                        }
                        else if(num3 <= num2){
                                System.out.println(num1 + "\t" + num2 + "\t" + num3);
                        }
                        else{
                                System.out.println(num1 + "\t" + num3 + "\t" + num2);
                        }
                }
                else{
                        if(num3 >= num2){
                                System.out.println(num3 + "\t" + num2 + "\t" + num1 );
                        }
                        else if(num3 <= num1){
                                System.out.println(num2 + "\t" + num1 + "\t" + num3);
                        }
                        else{
                                System.out.println(num2 + "\t" + num3 + "\t" + num1);
                        }
                }
        }
}
```

##### if语句练习1

```
1)对下列代码，若有输出，指出输出结果。
int x = 4;
int y = 1;
if (x > 2) {
    if (y > 2) 
    System.out.println(x + y);
System.out.println("atguigu");
} 
else
    System.out.println("x is " + x);

atguigu
x is 4
//大括号的省略，有缩进要求

2) 
boolean b = true;
//如果写成if(b=false)能编译通过吗？如果能，结果是？
if(b == false) 
    System.out.println("a");
else if(b)
    System.out.println("b");
else if(!b)
    System.out.println("c");
else
    System.out.println("d");
```

##### if语句练习2

```
1) 编写程序，声明2个int型变量并赋值。判断两数之和，如果大于等于50，打印“hello world!”
2) 编写程序，声明2个double型变量并赋值。判断第一个数大于10.0，且第2个数小于20.0，打印两数之和。否则，打印两数的乘积。
3) 我家的狗5岁了，5岁的狗相当于人类多大呢？其实，狗的前两年每一年相当于人类的10.5岁，之后每增加一年就增加四岁。那么5岁的狗相当于人类多少年龄呢？应该是：10.5 + 10.5 + 4 + 4 + 4 = 33岁。
编写一个程序，获取用户输入的狗的年龄，通过程序显示其相当于人类的年龄。如果用户输入负数，请显示一个提示信息。
```

```
1)
import java.util.Scanner;

public class IfTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.println("请输入数字1:");
                int num1 = scan.nextInt();
                System.out.println("请输入数字2:");
                int num2 = scan.nextInt();

                if((num1 + num2) >= 50){
                        System.out.println("Hello wirld!");
                }
                else{
                        System.out.println("not in");
                }
        }
}

2)
import java.util.Scanner;

public class IfTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.println("请输入数字1");
                double num1 = scan.nextDouble();
                System.out.println("请输入数字2");
                double num2 = scan.nextDouble();

                System.out.println("结果是：");
                if(num1 > 10.0 && num2 <20.0){
                        System.out.println("两数相加：" + (num1 + num2));
                }
                else{
                        System.out.println("两数相乘：" + (num1 * num2));
                }
        }
}

3)
import java.util.Scanner;

public class IfTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.println("请输入狗的年龄:");
                double dogAge = scan.nextDouble();
                double humanAge;

                if(dogAge >= 0){
                        if(dogAge <= 2){
                                humanAge = (dogAge * 10.5);
                                System.out.println("人类年龄为：" + humanAge);
                        }
                        else{
                                humanAge = ((dogAge - 2) * 4 +2 * 10.5);
                                System.out.println("人类年龄为：" + humanAge);
                        }
                }
                else{
                        System.out.println("有误!!!");
                }
        }
}
```

##### if语句练习3 彩票 使用(int)(Math.random() * 90 + 10)产生随机数

```
假设你想开发一个玩彩票的游戏，程序随机地产生一个两位数的彩票，提示用户输入一个两位数，然后按照下面的规则判定用户是否能赢。
1)如果用户输入的数匹配彩票的实际顺序，奖金10 000美元。
2)如果用户输入的所有数字匹配彩票的所有数字，但顺序不一致，奖金 3 000美元。
3)如果用户输入的一个数字仅满足顺序情况下匹配彩票的一个数字，奖金1 000美元。
4)如果用户输入的一个数字仅满足非顺序情况下匹配彩票的一个数字，奖金500美元。
5)如果用户输入的数字没有匹配任何一个数字，则彩票作废。
提示：使用(int)(Math.random() * 90 + 10)产生随机数。
Math.random() : [0,1) * 90 [0,90) + 10 [10,100)  [10,99]
```

```
import java.util.Scanner;

public class IfTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.println("请输入你的数字(必须是两位数)");
                int num = scan.nextInt();
                int firstNum = num / 10;
                int secondeNum = num % 10;

                int reword = (int)(Math.random() * 90 + 10);
                //第一位中奖号码
                int firstReword = reword / 10;
                //第二位中奖号码
                int secondeReword = reword % 10;
                //逆序的中奖号码
                int rReword = ((reword / 10) + (reword % 10) * 10);

                if(num >= 10 && num <=99){
                        if(num == reword){
                                System.out.println("大奖10,000美金");
                        }
                        else if(num == rReword){
                                System.out.println("3,000美金");
                        }
                        else if((firstNum == firstReword && secondeNum != secondeReword) || ((firstNum != firstReword) && (secondeNum == secondeReword))){
                                System.out.println("1,000美金");
                        }
                        else{
                                System.out.println("很遗憾！！!");
                        }
                }
                else{
                        System.out.println("有误！！！超过范围");
                }
        }
}
```

##### if语句练习4

```
大家都知道，男大当婚，女大当嫁。那么女方家长要嫁女儿，当然要提出一定的条件：
高：180cm以上；
富：财富1千万以上；
帅：是。
 如果这三个条件同时满足，则：“我一定要嫁给他!!!”
 如果三个条件有为真的情况，则：“嫁吧，比上不足，比下有余。”
 如果三个条件都不满足，则：“不嫁！”
Sysout(“身高: (cm))
scanner.nextInt();
Sysout(“财富: (千万))
scanner.nextDouble();
Sysout(“帅否: (true/false)) (是/否)
scanner.nextBoolean(); scanner.next(); “是”.equals(str) 
```

```
import java.util.Scanner;

public class IfTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.println("请输入你的条件:");
                System.out.println("请输入身高：(cm)");
                int hight = scan.nextInt();
                System.out.println("请输入财富(千万)");
                double wealth = scan.nextDouble();
                System.out.println("请输入帅否？(false/true)");
                boolean handsome = scan.nextBoolean();

                if(hight >= 180 && wealth >= 1 && handsome == true){
                        System.out.println("我一定要嫁给他");
                }
                else if(hight <180 && wealth <1 && handsome == false){
                        System.out.println("不嫁");
                }
                else{
                        System.out.println("嫁吧。");
                }
        }
}
```

##### 求ax2+bx+c=0方程的根

```
求ax2+bx+c=0方程的根。a,b,c分别为函数的参数，
如果a≠0，那么：
（1）当b2-4ac>0，则有两个解； 
（2）当b2-4ac=0，则有一个解； 
（3）当b2-4ac<0，则无解；
如果a=0,b≠0，那么， 

提示1：Math.sqrt(num);  sqrt指平方根
例如：
求x2-4x+1=0方程的根
求x2-2x+1=0方程的根
```

```
import java.util.Scanner;

class Exer5{
 public static void main(String[] args){
  Scanner input = new Scanner(System.in);
  
  System.out.println("一元二次方程：ax^2+bx+c=0");
  System.out.print("请输入参数a：");
  double a = input.nextDouble();
  
  System.out.print("请输入参数b：");
  double b = input.nextDouble();
   
  System.out.print("请输入参数c：");
  double c = input.nextDouble();

  if(a!=0){
   double temp = b*b - 4*a*c;
   if(temp==0){
    double x = -b/(2*a);
    System.out.println("该方程是一元二次方法，有两个相同解：" + x);
   }else if(temp>0){
    double sqrt = Math.sqrt(temp);
    double x1 = (-b+ sqrt)/(2*a);
    double x2 = (-b- sqrt)/(2*a);
    System.out.println("该方程是一元二次方法，两个不同解：" + x1 +"," + x2);
   }else{
    System.out.println("该方程是一元二次方法，在实数范围内无解！");
   }
  }else{
   if(b!=0){
    double x = -c/b;
    System.out.println("该方程是一元一次方程，有一个解：" + x);
   }else{
    System.out.println("不是方程，是一个等式");
                if(c == 0){
                    System.out.println("等式成立");
}else{
    System.out.println("等式不成立");
}
   }
  }
 }
}
```

#### switch-case
##### 冒号case:
1. 根据switch表达式中的值，一次匹配各个case中的常量，一旦匹配成功，则进入相应的case结构中，调用其执行语句。当调用完执行语句以后，则仍然继续向下执行其他case结构中的执行语句，直到遇到break关键字或者此swtich-case结构末尾结束为止。
2. break;可以使用在switch-case结构中，表示一旦执行到此关键字就跳出switch-case结构
3. switch结构的表达式只能是以下6种之一： 不能是boolean(不能是布尔表达式)
      - byte
      - short
      - char
      - int
      - 枚举类型
      - String类型
4. case子句中的值必须是常量，不能是变量名或不确定的表达式值；
5. 同一个switch语句，所有case子句中的常量值互不相同；
6. break语句用来在执行完一个case分支后使程序跳出switch语句块；如果没有break，程序会顺序执行到switch结尾
7. default子句是可任选的。同时，位置也是灵活的(都是最后执行的)。当没有匹配的case时，执行default
8. case:可以合并 case 1: case 2: ....

- 凡是可以使用switch-case结构的都可以使用if-else，分之，不成立
- 两者均可时，且switch中表达式取值情况不多时，优先选择switch-case效率稍高

```
switch(表达式){
case 常量1:
    语句1;
    // break;
case 常量2:
    语句2;
    // break;
… …
case 常量N:
    语句N;
    // break;
default:
    语句;
    // break;
}
```

###### 控制流贯穿
- 无break; 时在第一个满足条件之后的执行语句都执行

```
public class SwitchTest{
        public static void main(String[] args){
                int num = 2;
                switch(num){
                        case 0:
                                System.out.println("zero");
                                break;
                        case 1:
                                System.out.println("one");
                                break;
                        case 2:
                                System.out.println("two");
                                break;
                        default:
                                System.out.println("three");
                                break;

                }
        }
}

//two

public class SwitchTest{
        public static void main(String[] args){
                int num = 2;
                switch(num){
                        case 0:
                                System.out.println("zero");
                        case 1:
                                System.out.println("one");
                        case 2:
                                System.out.println("two");
                        default:
                                System.out.println("three");

                }
        }
}

//two
//three
```

###### 例1 把小写类型的 char型转为大写

```
1.使用 switch 把小写类型的 char型转为大写。只转换 a, b, c, d, e. 其它的输出 “other”。
提示：String word = scan.next(); char c = word.charAt(0); switch(c){}
```

```
import java.util.Scanner;

public class SwitchTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                String word = scan.next();
                char c = word.charAt(0);
                switch(c){
                        case 'a':
                                System.out.println('A');
                                break;
                        case 'b':
                                System.out.println('B');
                                break;
                        case 'c':
                                System.out.println('C');
                                break;
                        default:
                                System.out.println("other");
                                break;
                }
        }
}
```

```
import java.util.Scanner;

public class SwitchTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.print("请输入:");
                String str = scan.next();
                switch(str){
                        case "a":
                                str = "A";
                                break;
                        case "b":
                                str = "B";
                                break;
                        case "c":
                                str = "C";
                                break;
                        case "d":
                                str = "D";
                                break;
                        case "e":
                                str = "E";
                                break;
                        default:
                                str = "other";
                                break;
                }
                System.out.print("结果是: " + str);
        }
}
```

###### 例2 成绩大于60分

```
2.对学生成绩大于60分的，输出“合格”。低于60分的，输出“不合格”。
```

```
import java.util.Scanner;

public class SwitchTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.print("请输入成绩：");
                int score = scan.nextInt();

                switch(score / 60){
                        case 1:
                               System.out.println("合格");
                               break;
                        case 0:
                               System.out.println("不合格");
                               break;
                        default:
                               System.out.println("成绩有误");
                }
        }
}
```

###### 例3 该月份所属的季节

```
3.根据用于指定月份，打印该月份所属的季节。
3,4,5 春季 6,7,8 夏季 9,10,11 秋季 12, 1, 2 冬季
```

```
import java.util.Scanner;

public class SwitchTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.print("请输入月份：");
                int month = scan.nextInt();

                switch(month){
                        case 3: case 4: case 5:
                                System.out.println("春季");
                                break;
                        case 6: case 7: case 8:
                                System.out.println("夏季");
                                break;
                        case 9: case 10: case 11:
                                System.out.println("秋季");
                                break;
                        case 1: case 2: case 12:
                                System.out.println("冬季");
                                break;
                        default:
                                System.out.println("有误");
                }
        }
}
```

###### 例4 2019年的第几天

```
4. 编写程序：从键盘上输入2019年的“month”和“day”，要求通过程序输出输入的日期为2019年的第几天。
```

```
import java.util.Scanner;

public class SwitchTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.print("month = ");
                int month = scan.nextInt();
                System.out.print("day = ");
                int day = scan.nextInt();
                int days;
                int temp = month;

                switch(month){
                        case 12:
                                month += 30;
                        case 11:
                                month += 31;
                        case 10:
                                month += 30;
                        case 9:
                                month += 31;
                        case 8:
                                month += 31;
                        case 7:
                                month += 30;
                        case 6:
                                month += 31;
                        case 5:
                                month += 30;
                        case 4:
                                month += 31;
                        case 3:
                                month += 28;
                        case 2:
                                month += 31;
                        case 1:
                                month += 0;
                                break;
                }
                days = month + day - temp;
                System.out.println("2019年的第" + days + "天");
        }
}
```

###### switch语句练习1 判断这一天是当年的第几天

```
从键盘分别输入年、月、日，判断这一天是当年的第几天
注：判断一年是否是闰年的标准：
1）可以被4整除，但不可被100整除
或
2）可以被400整除
```

```
import java.util.Scanner;

class Test{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.println("请依次输入年月日:");
                int year = scan.nextInt();
                int month = scan.nextInt();
                int day = scan.nextInt();
                int days = 0;
                int temp = 0;
                
                if (((year % 4 == 0) && ((year % 100) != 0)) || (year % 400 == 0)){
                    temp = 0;
                }
                else{
                    temp = 1;
                }
                switch(temp){
                        case 0:
                                switch(month){
                                        case 12:
                                                days += 30;
                                        case 11:
                                                days += 31;
                                        case 10:
                                                days += 30;
                                        case 9:
                                                days += 31;
                                        case 8:
                                                days += 31;
                                        case 7:
                                                days += 30;
                                        case 6:
                                                days += 31;
                                        case 5:
                                                days += 30;
                                        case 4:
                                                days += 31;
                                        case 3:
                                                days += 29;
                                        case 2:
                                                days += 31;
                                        case 1:
                                                days += day;
                                                break;
                                }
                                break;
                        default:
                                switch(month){
                                        case 12:
                                                days += 30;
                                        case 11:
                                                days += 31;
                                        case 10:
                                                days += 30;
                                        case 9:
                                                days += 31;
                                        case 8:
                                                days += 31;
                                        case 7:
                                                days += 30;
                                        case 6:
                                                days += 31;
                                        case 5:
                                                days += 30;
                                        case 4:
                                                days += 31;
                                        case 3:
                                                days += 28;
                                        case 2:
                                                days += 31;
                                        case 1:
                                                days += day;
                                                break;
                                }
                                break;
                }
                System.out.println(year + "年" + month + "月" + day + "日" + "是第" + days + "天");
        }
}
```

###### switch语句练习2 改写下列if

```
使用switch语句改写下列if语句：
int a = 3;
int x = 100;
if(a == 1)
    x += 5;
else if(a == 2)
    x += 10;
else if(a == 3)
    x += 16;
else
    x += 34;
```

```
public class Test{
        public static void main(String[] args){
                int a = 3;
                int x = 100;

                switch(a){
                        case 1:
                                x += 5;
                                break;
                        case 2:
                                x += 3;
                                break;
                        case 3:
                                x += 16;
                                break;
                        default:
                                x += 34;
                                break;
                }
                System.out.println(x);
        }
}
```

###### switch语句练习3

```
编写程序：从键盘上读入一个学生成绩，存放在变量score中，根据score的
值输出其对应的成绩等级：

score>=90 等级: A
70<=score<90 等级: B 
60<=score<70 等级: C
score<60 等级: D

方式一：使用if-else
方式二：使用switch-case: score / 10: 0 - 10
```

```
1.
import java.util.Scanner;

public class Test{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.print("成绩:");
                int score = scan.nextInt();

                if(score >= 90){
                        System.out.println("等级:A");
                }
                else if(score >= 70){
                        System.out.println("等级:B");
                }
                else if(score >= 60){
                        System.out.println("等级:C");
                }
                else{
                        System.out.println("等级:D");
                }
        }
}
2.
import java.util.Scanner;

public class Test{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.print("成绩:");
                int score = scan.nextInt();

                switch(score / 10){
                        case 9:
                                System.out.println("等级:A");
                                break;
                        case 8: case 7:
                                System.out.println("等级:B");
                                break;
                        case 6:
                                System.out.println("等级:C");
                                break;
                        default:
                                System.out.println("等级:D");
                                break;
                }
        }
}
```

###### switch语句练习4

```
编写一个程序，为一个给定的年份找出其对应的中国生肖。中国的生肖基于12年一个周期，每年用一个动物代表：rat、ox/tiger、rabbit、dragon、snake、horse、sheep、monkey、rooster、dog、pig。
```

```
import java.util.Scanner;

public class Test{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.println("year:");
                int year = scan.nextInt();

                switch(year % 12){
                        case 1:
                                System.out.println("鼠");
                                break;
                        case 2:
                                System.out.println("牛");
                                break;
                        case 3:
                                System.out.println("虎");
                                break;
                        case 4:
                                System.out.println("兔");
                                break;
                        case 5:
                                System.out.println("龙");
                                break;
                        case 6:
                                System.out.println("蛇");
                                break;
                        case 7:
                                System.out.println("马");
                                break;
                        case 8:
                                System.out.println("羊");
                                break;
                        case 9:
                                System.out.println("猴");
                                break;
                        case 10:
                                System.out.println("鸡");
                                break;
                        case 11:
                                System.out.println("狗");
                                break;
                        case 0:
                                System.out.println("猪");
                                break;
                }
        }
}
```

##### 箭头case->
```
case label_1,label_2,...,label_n -> expression;|throw - statement;|block
```
- label中任意一个匹配成功，箭头右侧的代码都被执行，且在这些代码结束之后，不会产生控制流贯穿问题
- throw语句
- block 代码块

###### yield
- 大多数时候，在switch表达式内部，我们会返回简单的值。
- 但是，如果需要复杂的语句，我们也可以写很多语句，放到{…}里，然后，用yield返回一个值作为switch语句的返回值：

- yield 和 return 的区别在于：return 会直接跳出当前循环或者方法，而 yield 只会跳出当前 switch 块；
```
public class Test004 {
	public static void main(String args[]) {
		String fruit = "orange";
		
		int opt = switch(fruit){
		case "apple" -> 1;
		case "banana" -> 2;
		default ->{
			int code = fruit.hashCode();
			yield code;
			}
		};
		
		System.out.println(opt);
	}
}
```

###### throw
- 除了可以通过throws关键字抛出异常外,还可以使用throw关键字抛出异常。
- 与throws有所不同的是,
    - throw用于方法体内,并且抛出的是一个异常类对象,
    - 而throws关键字用在方法声明中,用来指明方法可能抛出的多个异常。
- 通过throw关键字抛出异常后，还需要使用throws关键字或try…catch对异常进行处理。
- 要注意的是，如果throw抛出的是Error、RuntimeException或它们的子类异常现象无需使用throws关键字或try…catch对异常进行处理。

```
public class Example34 {
	// 定义printAge()输出年龄
	public static void printAge(int age) throws Exception {
		if (age < 0) {
			// 对业务逻辑进行判断，当输入年龄为负数的时候抛出异常
			throw new Exception("输入的年龄有误，必须是正整数!");
		} else {
			System.out.println("此人年龄为：" + age);
		}
	}

	public static void main(String[] args) {
		// 下面的代码定义了一个try...catch语句用于捕获异常
		int age = -1;
		try {
			printAge(age);
		} catch (Exception e) {
			System.out.println("捕获的异常信息为：" + e.getMessage());
		}
	}
}
```

###### 例1

```
public class Test001 {
	public static void main(String args[]) {
		int numLetters = 0;

		Day day = Day.wednesday;
		
		switch(day) {
		case monday,friday,sunday -> numLetters = 6;
		case tuesday -> numLetters = 7;
		case thursday -> numLetters = 8;
		case wednesday -> numLetters = 9;
//		default -> throw new IllegaStateException("Invalid day: " + day);
		default -> numLetters = 0;
		}
		
		System.out.println(numLetters);
	}
}

enum Day{monday,tuesday,wednesday,thursday,friday,satarday,sunday;}
```

###### 例2

```
public class Test003 {
	public static void main(String args[]) {
		Day1 day = Day1.wednesday;
		
		int j = switch(day) {
		case monday -> 0;
		case tuesday -> 1;
		default -> {
			int k = day.toString().length();
			int result = k;
			yield result;
			}
		};
	}
}

enum Day1{monday,tuesday,wednesday,thursday,friday,satarday,sunday;}
```


***

### 循环结构

根据循环条件，重复性的执行某段代码。
有while、do…while、for三种循环语句。
注：JDK1.5提供了foreach循环，方便的遍历集合、数组元素。

#### for

```
for (①初始化部分; ②循环条件部分; ④迭代部分)｛
③循环体部分;
｝
```

- 执行过程：
①-②-③-④-②-③-④-②-③-④-.....-②
  - 则循环条件在循环失败时也执行了一次

- 循环条件部分为boolean类型表达式，当值为false时，退出循环
- 初始化部分可以声明多个变量，但必须是同一个类型，用逗号分隔
- 可以有多个变量更新，用逗号分隔

```
public class ForTest{
        public static void main(String[] args){
                int result = 0;
                for(int i = 1; i <= 100; i++){
                        result += i;
                        System.out.println("result=" + result);
                }
        }
}
```

```
public class Fortest{
        public static void main(String[] args){
                int num = 1;
                for(System.out.println('a');num <= 5;System.out.println('c')){
                        System.out.println('b');
                        num++;
                }
        }
}
```

##### for语句例题1 每个3的倍数行上打印出“foo”,在每个5的倍数行上打印“biz”,在每个7的倍数行上打印输出“baz”

```
编写程序从1循环到150，并在每行打印一个值，另外在每个3的倍数行上打印出“foo”,在每个5的倍数行上打印“biz”,在每个7的倍数行上打印输出“baz”。
```

```
public class ForTest{
        public static void main(String[] args){
                for(int i = 1;i <= 150;i++){
                        System.out.print(i);
                        if(i % 3 == 0){
                                System.out.print("\t" + "foo");
                        }
                        if(i % 5 == 0){
                                System.out.print("\t" + "biz");
                        }
                        if(i % 7 == 0){
                                System.out.print("\t" + "baz");
                        }
                        System.out.println();
                }
        }
}
```

##### for语句例题2 输入两个正整数m和n，求其最大公约数和最小公倍数

```
题目：输入两个正整数m和n，求其最大公约数和最小公倍数。
比如：12和20的最大公约数是4，最小公倍数是60。
说明：break关键字的使用
```

```
import java.util.Scanner;

public class Fortest{
        public  static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                System.out.print("请输入第一个正整数：");
                int n = scan.nextInt();
                System.out.print("请输入第二个正整数：");
                int m = scan.nextInt();

                int min = (n > m)? m : n;
                for(int i = min; i >= 1;i--){
                        if(m % i == 0 && n % i == 0){
                                System.out.println("最大公约数为: " + i);
                                break;
                        }
                }

                int max = (n > m)? n: m;
                for(int i = max; i <= m * n; i++){
                        if(i % m == 0 && i % n == 0){
                                System.out.println("最下公倍数为：" + i);
                                break;
                        }
                }
        }
}
```

```
public class ForTest{
        public static void main(String[]  args){
                Scanner scan = new Scanner(System.in);

                System.out.print("请输入正整数n:");
                int n = scan.nextInt();
                System.out.println();
                System.out.print("请输入正整数m:");
                int m = scan.nextInt();
                int t1 = 0;
                int t2 = n * m;

                for(int i = 1;i <= (n * m);i++){
                        if((n % i == 0) && (m % i == 0)){
                                if(t1 <= i){
                                        t1 = i;
                                }
                        }
                        if((i % n == 0) && (i % m == 0)){
                                if(t2 >= i){
                                        t2 = i;
                                }
                        }
                }
                System.out.println("最大公约数: " + t1);
                System.out.println("最小公倍数: " + t2);
        }
}
```

##### for语句练习1.打印1~100之间所有奇数的和

```
public class ForTest{
        public static void main(String[] args){
                int sum = 0;

                for(int i = 1;i <= 100;i++){
                        if( i % 2 == 1){
                                sum += i;
                        }
                }
                System.out.println(sum);
        }
}
```

##### for语句练习2.打印1~100之间所有是7的倍数的整数的个数及总和（体会设置计数器的思想）

```
public class ForTest{
        public static void main(String[] args){
                int count = 0;
                int sum = 0;

                for(int i = 1; i <= 100; i++){
                        if(i % 7 == 0){
                                count += 1;
                                sum += i;
                        }
                }
                System.out.println("count: " + count);
                System.out.println("sum " + sum);
        }
}
```

##### for语句练习3.输出所有的水仙花数，所谓水仙花数是指一个3位数，其各个位上数字立方和等于其本身

`例如： 153 = 1*1*1 + 3*3*3 + 5*5*5`

```
public class ForTest{
        public static void main(String[] args){
                int num1,num2,num3;
                for(int i = 100; i <= 999; i++){
                        num1 = i / 100;
                        num2 = (i % 100) / 10;
                        num3 = i % 10;

                        if(i == num1 * num1 * num1 + num2 * num2 * num2 + num3 * num3 * num3){
                                System.out.println(i);
                        }
                }
        }
}
```

##### 找出1000以内的所有完数

```
 一个数如果恰好等于它的因子之和，这个数就称为"完数"。（因子：除去这个数本身的约数）
例如6=1＋2＋3.编程 找出1000以内的所有完数
```

```
class ForTest{
 public static void main(String[] args){
  int factor = 0;
 
  for(int i = 1; i <= 1000; i++){
   
   for(int j = 1; j < i; j++){
   
    if(i % j == 0){
     factor += j;
    }
   }

   if(i == factor){
    System.out.println(i);
   }

   factor = 0;
  }
 }
}
```

优化

```
class ForTest{
 public static void main(String[] args){
  int factor = 0;
 
  for(int i = 1; i <= 1000; i++){
   
   for(int j = 1; j < i/2; j++){
   
    if(i % j == 0){
     factor += j;
    }
   }

   if(i == factor){
    System.out.println(i);
   }

   factor = 0;
  }
 }
}
```

#### 增强的for循环

```
for(类型 标识符: 可迭代类型的表达式) {
    语句;
} 
```
- 可迭代的数据类型按一般是数组或集合
```
public class Test008 {
	public static void main(String args[]) {
		int[] numbers = new int[] {1,2,3,4,5,6,7,8,9,10};
		
		for(int element : numbers) {
			System.out.println(element);
		}
	}
}
```

```
public class Test009 {
	public static void main(String args[]) {
		String[] strs = new String[] {"One","Two","There"};
		
		for(String str : strs) {
			System.out.println(str.toLowerCase());
		}
	}
}
```
- string.toLowCase()
    - 使用默认地区的规则将此String中的所有字符转换为小写字符。
    - 这相当于调用toLowerCase(Locale.getDefault())

#### while

```
①初始化部分
while(②循环条件部分)｛
    ③循环体部分;
    ④迭代部分;
}
```

- 执行过程：  
①-②-③-④-②-③-④-②-③-④-...-②
-
- 注意不要忘记声明④迭代部分。否则，循环将不能结束，变成死循环。
- for循环和while循环可以相互转换

```
public class WhileTest{
        public static void main(String[] args){
                int result = 0;
                int i = 1;
                while (i <= 100){
                        result += i;
                        i++;
                }
                System.out.println("result=" + result);
                //注 结果i为101
        }
}
```

![](C:/NoteBook/pictures/289043723220669.png =696x)
##### 字符串复制

```
public class Test002 {
	public static void main(String args[]) {
		
		String copyFromMe = "Copy this string until you encounter the";
		String copyToMe = "";

		 int i = 0;
		 char c= copyFromMe.charAt(i);
		 
		 while(c != 'g') {
			 copyToMe = copyToMe + c;
			 c = copyFromMe.charAt(++i);
		 }
		 
		 System.out.println(copyToMe);
	}
}
```

###### charAt()方法
- charAt() 方法用于返回指定索引处的字符。索引范围为从 0 到 length() - 1。

```
public class Test {
    public static void main(String args[]) {
        String s = "www.runoob.com";
        
        char result = s.charAt(6);
        
        System.out.println(result); //n
    }
}
```


#### for和while区别

##### for循环和while循环的初始化作用域范围不同

- for 只在循环内部
- while 在外部

##### i++ 的值不一样

```
int i = 1;
while (i <= 100){
       result += i;
       i++;
}
// i = 101; 满足条件 i <= 100 后仍然i++;i++之后才不满足条件则停止，会比范围多1
//其i++本质上是执行语句

for(int i = 1; i <= 100; i++){
    ...    
}
//i = 100; 执行到 i <= 100 就停止;不会超过范围
```

#### do...while

```
①初始化部分;
do{
    ③循环体部分
    ④迭代部分
}while(②循环条件部分);
```

- 执行过程：
①-③-④-②-③-④-②-③-④-...②
- do-while循环至少执行一次循环体。
  - 即在不满足条件的时候也会执行一次
- 但如果满足执行多次，do...while和while没有区别，并不会多出1次

```
public class DoWhileTest{
        public static void main(String[] args){
                int result = 0,i = 1;
                do{
                        result += i;
                        i++;
                }while(i <= 100);
                System.out.println("result=" + result);
        }
}
```

![](C:/NoteBook/pictures/199433823239095.png =689x)

##### 遍历100以内的偶数，并计算所有偶数的和及偶数的个数

```
public class DoWhiletest{
        public static void main(String[] args){
                int i = 1;
                int count = 0;
                int sum = 0;

                do{
                        if(i % 2 == 0){
                                count += 1;
                                sum += i;
                        }
                        i++;
                }while(i <= 100);
                System.out.println("个数：" + count);
                System.out.println("和: " + sum);
                System.out.println(i);
        }
}
```

##### 猜数字游戏

```
随机生成一个100以内的数，猜数字游戏：
从键盘输入数，如果大了提示，大了，如果小了，提示小了，如果对了，就不再猜了，并统计一共猜了多少次？
提示：随机数
import java.util.Random;
Random rand = new Random();
int num= rand.nextInt(100);
```

```
import java.util.Random;
import java.util.Scanner;

public class TestDoWhileLoop{
 public static void main(String[] args){
  //1、随机产生一个100以内的整数
  Random rand = new Random();
  //int num = rand.nextInt();//产生的是任意大小的整数
  int num = rand.nextInt(100);//产生[0,100)的整数
  System.out.println(num);
  
  //2、键盘输入
  Scanner input = new Scanner(System.in);
  
  //声明变量
  int guess;
  int count = 0;
  do{
   //循环体至少执行一次
   System.out.print("请输入一个整数：");
   guess = input.nextInt();//为变量赋值
   
   count++;//输入一次，计数一次
   
   if(guess>num){
    System.out.println("大了");
   }else if(guess < num){
    System.out.println("小了");
   }else{
    System.out.println("猜对了");
   }
  }while(guess != num);
  
  System.out.println("一共猜了：" + count + "次");
 }
 
}

public static void main(String[] args) {
  Random rand = new Random();
  int num= rand.nextInt(100);
  Scanner input = new Scanner(System.in);
  int count =0 ;
  do{
   count++;
   System.out.println("请猜：");
   int temp = input.nextInt();
   if(temp<num){
    System.out.println("小了");
    continue;
   }
   if(temp>num){
    System.out.println("大了");
    continue;
   }
   if(temp == num){
    break;
   }
  }while(true);
  System.out.println("总共猜了"+count+"次");
 }

```

#### 循环语句综合例题 无限循环

最简单“无限” 循环格式：

- `while(true)`
- `for(;;)`
- 无限循环存在的原因是并不知道循环多少次，需要根据循环体内部某些条件，来控制循环的结束。

##### 从键盘读入个数不确定的整数，并判断读入的正数和负数的个数，输入为0时结束程序

```
题目：
从键盘读入个数不确定的整数，并判断读入的正数和负数的个数，输入为0时结束程序。
```

```
1.while(true){}
import java.util.Scanner;

public class WhileTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);
                int countPositive = 0;
                int countNegative = 0;

                int i = 1;
                while(true){
                        System.out.print("请输入数字" + i + ":");
                        int num = scan.nextInt();
                        System.out.println();

                        if(num > 0){
                                countPositive += 1;
                        }
                        if(num < 0){
                                countNegative += 1;
                        }
                        if(num == 0){
                                break;
                        }
                        i++;
                }
                System.out.println("负数的个数为：" + countNegative);
                System.out.println("正数的个数为：" + countPositive);
        }
}

2.for(;;){}
import java.util.Scanner;

public class ForTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);

                int countPositive = 0;
                int countNegative = 0;

                for(;;){
                        System.out.print("请输入数字" + i + ": ");
                        int num = scan.nextInt();

                        if(num > 0 ){
                                countPositive += 1;
                        }
                        if(num < 0 ){
                                countNegative += 1;
                        }
                        if(num == 0){
                                break;
                        }
                }
                System.out.println("正数的个数为：" + countPositive);
                System.out.println("负数的个数为：" + countNegative);
        }
}
```

#### 嵌套循环(多重循环)

- 将一个循环放在另一个循环体内，就形成了嵌套循环。其中，for ,while ,do…while均可以作为外层循环或内层循环。
- 实质上，嵌套循环就是把内层循环当成外层循环的循环体。当只有内层循环的循环条件为false时，才会完全跳出内层循环，才可结束外层的当次循环，开始下一次的循环。
- 设外层循环次数为m次，内层为n次，则内层循环体实际上需要执行m*n次。

##### 0) 练习图形

- 内层控制行数
- 外层控制列数

###### 矩形

```
*****
***** 
***** 
***** 
*****
```

```
public class RForTest{
        public static void main(String[] args){
                for(int i = 1; i <= 5; i++){
                        System.out.println();
                        for (int j = 1; j <= 5; j++){
                                System.out.print("*");
                        }
                }
        }
}
```

###### 直角三角形

```
*
**
***
****
*****
```

```
public class RForTest{
        public static void main(String[] args){
                for(int i = 1; i <= 5; i++){
                        System.out.println();
                        for (int j = 1; j <= i; j++){
                                System.out.print("*");
                        }
                }
        }
}
```

###### 倒直角三角形

```
*****
****
***
**
*
```

```
public class RForTest{
        public static void main(String[] args){
                for(int i = 1; i <= 5; i++){
                        System.out.println();
                        for (int j = 1; j <= (5 - i + 1); j++){
                                System.out.print("*");
                        }
                }
        }
}
```

###### 半菱形

```
*
**
***
****
*****
****
***
**
*
```

```
public class RForTest{
        public static void main(String[] args){
                for(int i = 1; i <= 9; i++){
                        System.out.println();
                        if (i <= 5){
                                for (int j = 1; j <= i; j++){
                                        System.out.print("*");
                                }
                        }
                        else{
                                for (int j = 1; j <= (9 - i + 1); j++){
                                        System.out.print("*");
                                }
                        }
                }
        }
}
```

###### 菱形

```
   *
  ***
 *****
  ***
   *
```

```
public class RForTest{
        public static void main(String[] args){
                for(int i = 1; i <= 5; i++){
                        System.out.println();
                        if(i <= 3){
                               for(int j = 1; j <= (6 - i); j++){
                                       System.out.print(" ");
                               }
                               for(int k = 1; k <= (i * 2 - 1); k++){
                                       System.out.print("*");
                               }
                        }
                        else{
                                for(int j = 1; j <= i; j++){
                                        System.out.print(" ");
                                }
                                for(int k = 1; k <= ((5 - i) * 2 + 1); k++){
                                       System.out.print("*");
                                }
                        }
                }
        }
}
```

###### 菱形2

```
**********        
****  ****        
***    ***        
**      **        
*        *        
**      **
***    ***
****  ****
**********
```

```
public class RForTest{
        public static void main(String[] args){
                for(int i = 1; i <= 9; i++){
                        System.out.println();
                        if(i <= 5){
                               for(int j = 1; j <= (6 - i); j++){
                                       System.out.print("*");
                               }
                               for(int k = 1; k <= (i * 2 - 1); k++){
                                       System.out.print(" ");
                               }
                               for(int h = 1; h <= (6 - i); h++){
                                       System.out.print("*");
                               }
                        }
                        else{
                                for(int j = 1; j <= (i - 4); j++){
                                        System.out.print("*");
                                }
                                for(int k = 1; k <= ((9 - i) * 2 + 1); k++){
                                       System.out.print(" ");
                                }
                                for(int h = 1; h <= (i - 4); h++){
                                       System.out.print("*");
                                }
                        }
                }
        }
}
```

###### 心形

```
      ******       ******          
    **********   **********         
  ************* *************  
 *****************************     
 *****************************     
 *****************************    
  ***************************       
    ***********************         
      *******************          
        ***************             
          ***********             
            *******                
              ***                  
               *    
```

```
public class RForTest{
        public static void main(String[] args){
                for(int i = 1; i <= 18; i++){
                        System.out.println();
                        if(i <= 3){
                                for(int j = 1; j <=(10 - i) ; j++){
                                        System.out.print(" ");
                                }
                                for(int k = 1; k <= (4 + 2 * i); k++){
                                        System.out.print("*");
                                }
                                for(int h = 1; h <= (5 - ( i - 1) * 2); h++){
                                        System.out.print(" ");
                                }
                                for(int m = 1; m <= (4 + 2 * i); m++){
                                        System.out.print("*");
                                }
                        }
                        else if( i <= 6 && i >= 3){
                                //int num = 15 + 2 * i;
                                int num = 23;
                                for(int j = 1; j <= 6; j++){
                                        System.out.print(" ");
                                }
                                for(int k = 1; k <= num; k++){
                                        System.out.print("*");
                                }
                        }
                        else{
                                for(int j = 1;j <= (6 + i - 6); j++){
                                        System.out.print(" ");
                                }
                                for(int k = 1;k <= (23 - 2 * (i - 6)); k++){
                                        System.out.print("*");
                                }
                        }

                }
        }
}
```

##### 1）九九乘法表

```
public class RForTest{
        public static void main(String[] args){
                for (int i = 1; i <= 9; i++){
                        for (int j = 1; j <= i; j++){
                                System.out.print( i + "*" + j + "=" + (i * j) + "\t");
                                if(i == j){
                                        System.out.println();
                                }
                        }
                }
        }
}
```

```
public class RForTest{
        public static void main(String[] args){
                for (int i = 1; i <= 9; i++){
                        System.out.println();
                        for (int j = 1; j <= 9; j++){
                                System.out.print( j + "*" + i + "=" + (i * j) + "\t");
                                if(i == j){
                                        break;
                                }
                        }
                }
        }
}
```

##### 2）100以内的所有质数

```
public class RForTest{
        public static void main(String[] args){
                for(int i = 1; i <= 100; i++){
                        int count = 0;
                        for(int j = 1; j <= i; j++){
                                if(i % j == 0){
                                        count++;
                                }
                        }
                        if(count == 2){
                                System.out.println(i);
                        }
                }
        }
}
```

```
class RForTest{
        public static void main(String[] args){                       
 lable:               for(int i = 2; i <= 100; i++){
      //优化2：只对本身是质数的自然数有效
                        for(int j = 2; j <= Math.sqrt(i); j++){
                                if(i % j == 0){
                                    continue lable;
                                }
                        }
                        System.out.println(i);
                }
        }
}
```

```
public class RForTest{
        public static void main(String[] args){
                for(int i = 2; i <= 100; i++){
                        boolean isFlag = true;
                        for(int j = 2; j < i; j++){
                                if(i % j == 0){
                                        isFlag = false;
                                }
                        }
                        if(isFlag){
                                System.out.println(i);
                        }
                }
        }
}
```

###### 优化 100000 以内

```
class RForTest{
        public static void main(String[] args){
                boolean isFlag = true; //标识i是否被j除尽               
    int count = 0; //记录质数的个数
                //获取当前时间距离1970-01-01 00:00:00 的毫秒数
                long start = System.currentTimeMillis();
                
                for(int i = 2; i <= 100000; i++){
      //优化2：只对本身是质数的自然数有效
                        for(int j = 2; j <= Math.sqrt(i); j++){

                                if(i % j == 0){
                                        isFlag = false;
                                        break;  //优化1：只对本身非质数的自然数有效
                                }
                        }

                        if(isFlag){
                                //System.out.println(i);
        count++;   //优化3：
                        }
      //重置isflag
                        isFlag = true;
                }
                //获取当前时间距离1970-01-01 00:00:00 的毫秒数
                long end = System.currentTimeMillis();
                System.out.println("质数的个数: " + count);
                System.out.println("所花费的时间是：" + (start - end));
        }
}
```

#### 特殊关键字的使用

- break
- continue

##### break 语句

- break语句用于终止某个语句块的执行,跳出最近的一个循环体

```
{   ……
    break;
    ……
}
```

###### 带label的break

```
int students[][] = {{10, 12, 11}, {8, 9, 1}, {9, 15, 7}};
jumpOutHere:
for (int i = 0; i < 3; i++) {
    System.out.println(String.format("start outer for loop index %d", i));
    int j = 0;
    while (j < 3) {
        System.out.println(String.format("current retrieve value %d", students[i][j]));
        if (students[i][j] == 9) {
            break jumpOutHere;
        } else {
            j++;
        }
    }
    System.out.println(String.format("end outer for loop index %d", i));
}
```

- break语句出现在多层嵌套的语句块中时，可以通过标签指明要终止的是哪一层语句块

```
label1: { ……
label2:     { ……
label3:         { ……
                break label2;
                ……
                }
            }
        }
```

###### 输入密码的次数

```
import java.util.Scanner;

class WhileTest{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);
label1:
    while (true)
    {
     System.out.println("请输入账号: 输入0退出");
     int id = scan.nextInt();

     if (id == 1001)
     {
      System.out.println("当前账号为: " + id);
      for(int i = 1; i <= 3; i++){
       System.out.println("请输入密码:");
       int code = scan.nextInt();
       if(code == 1234){
         System.out.println("登录成功");
         break label1;
          }
       else{
         System.out.println("登录失败");
         i++;
         System.out.println("还有" + ( 3 - i) + "次机会");
       }
      }
     }
     else if (id == 1002)
     {
      System.out.println("当前账号为: " + id);
      for(int i = 1; i <= 3; i++){
       System.out.println("请输入密码:");
       int code = scan.nextInt();
       if(code == 1234){
         System.out.println("登录成功");
         break label1;
          }
       else{
         System.out.println("登录失败");
         i++;
         System.out.println("还有" + ( 3 - i) + "次机会");
       }
      }
     }
     else if (id == 0)
     {
      System.out.println("已推出!");
      break label1;
     }
     else{
      System.out.println("无此账号");
     }
    }

        }
}
```

##### continue 语句

- continue只能使用在循环结构中
- continue语句用于跳过其所在循环语句块的一次执行，继续下一次循环
- continue语句出现在多层嵌套的循环语句体中时，可以通过标签指明要跳过的是哪一层循环

##### 带标签的continue

###### 在一个字符串中搜索另一个字符串
```
public class Test005 {
	public static void main(String args[]) {
		StringBuffer searchMe = new StringBuffer(
				"peter piper picked a peck of pickled peppers"
				);
		String subString = "pick";
		boolean foundIt = false;
		int max = searchMe.length() - subString.length();
		
		test:
			for(int i = 0; i <= max; i++) {
				int n = subString.length();
				int j = i;
				int k = 0;
				
				while(n-- != 0) {
					if(searchMe.charAt(j++) != subString.charAt(k++)) {
						continue test;
					}
				}
				foundIt = true;
			}
		System.out.println(foundIt?"found":"no");
	}
}

```
###### 打印1-100之间非13的倍数，使用continue语句
```  
public class ForTest {
	public static void main(String[] args) {
		for(int i = 1; i < 100; i++) {
			if(i % 13 != 0 || i == 13) {
				continue;
			}
			System.out.println(i);
		}
	}
}
```

###### 在字符串缓冲区中寻找指定字符并进行处理
```
对一个字符串缓冲区中的字符逐一检查。
如果当前字符不是字母p，则continue语句跳过循环的其余语句并开始处理下一个字符；
如果是字母p，则程序将计数器加1并把p转换为大写
```

```
public class Test007 {
	public static void main(String args[]) {
		StringBuffer search = new StringBuffer(
				"peter piper picked this"
				);
		
		Test007 test = new Test007();
		test.checkString(search);  //引用类型 地址值传递
		System.out.println(search);
	}
	
	public void checkString(StringBuffer str) {
		int times = 0;
		
		for(int i = 0; i < str.length(); i++) {
			
			if(str.charAt(i) == 'p') {
				times++;
				str.setCharAt(i,'P');
			}
		}
	}
}
```


##### return 特殊流程控制语句3

- return：并非专门用于结束循环的，它的功能是结束一个方法。
- 当一个方法执行到一个return语句时，这个方法将被结束。
- 与break和continue不同的是，return直接结束整个方法，不管这个return处于多少层循环之内

##### 特殊流程控制语句说明

- break只能用于switch语句和循环语句中。
- continue 只能用于循环语句中。
  - 二者功能类似，但continue是终止本次循环，break是终止本层循环。
- break、continue之后不能有其他的语句，因为程序永远不会执行其后的语句。
- 标号语句必须紧接在循环的头部。标号语句不能用在非循环语句的前面。
- 很多语言都有goto语句，goto语句可以随意将控制转移到程序中的任意一条语句上，然后执行它。但使程序容易出错。Java的break和continue是不同于goto的。

***

### 项目一：家庭收支记账软件

#### 需求说明

```
• 模拟实现基于文本界面的《家庭记账软件》。
• 该软件能够记录家庭的收入、支出，并能够打印收支明细表。
• 项目采用分级菜单方式。主菜单如下：
-----------------家庭收支记账软件-----------------
                1 收支明细
                2 登记收入
                3 登记支出
                4 退 出
                请选择(1-4)：_
```

```
• 假设家庭起始的生活基本金为10000元。
• 每次登记收入（菜单2）后，收入的金额应累加到基本金上，并记录本次收入明细，以便后续的查询。
• 每次登记支出（菜单3）后，支出的金额应从基本金中扣除，并记录本次支出明细，以便后续的查询。
• 查询收支明细（ 菜单1）时，将显示所有的收入、支出名细列表
```

```
• “登记收入”的界面及操作过程如下所示：
-----------------家庭收支记账软件-----------------
                1 收支明细
                2 登记收入
                3 登记支出
                4 退 出
                请选择(1-4)：2
                本次收入金额：1000
                本次收入说明：劳务费_
```

```
• “登记支出”的界面及操作过程如下所示：
-----------------家庭收支记账软件-----------------
                1 收支明细
                2 登记收入
                3 登记支出
                4 退 出
                请选择(1-4)：3
                本次支出金额：800
                本次支出说明：物业费_
```

```
• “收支明细”的界面及操作过程如下所示：
-----------------家庭收支记账软件-----------------
                1 收支明细
                2 登记收入
                3 登记支出
                4 退 出
                请选择(1-4)：1
-----------------当前收支明细记录-----------------
收支     账户金额     收支金额     说 明
收入     11000        1000       劳务费
支出     10200        800        物业费
--------------------------------------------------
• 提示：明细表格的对齐，可以简单使用制表符‘\t’来实现
```

```
• “退 出”的界面及操作过程如下所示：
-----------------家庭收支记账软件-----------------
                1 收支明细
                2 登记收入
                3 登记支出
                4 退 出
                请选择(1-4)：4
                确认是否退出(Y/N)：_
```

#### 基本金和收支明细的记录

```
• 基本金的记录可以使用int类型的变量来实现：
  int balance = 10000;
• 收支明细记录可以使用Sting类型的变量来实现，其初始值为明细表的表头。例如：
  String details = "收支\t账户金额\t收支金额\t说 明\n";
• 在登记收支时，将收支金额与balance相加或相减，收支记录直接串接到details后面即可
```

![](C:/NoteBook/pictures/391195310239093.png =764x)

![](C:/NoteBook/pictures/526725310226960.png =747x)

#### 键盘访问的实现

```
• 项目中提供了Utility.java类，可用来方便地实现键盘访问。
• 该类提供了以下静态方法：
 public static char readMenuSelection() ：该方法读取键盘，如果用户键入’1’-’4’中的任意字符，则方法返回。返回值为用户键入字符。
 public static int readNumber() ：该方法从键盘读取一个不超过4位长度的整数，并将其作为方法的返回值。
 public static String readString() ：该方法从键盘读取一个不超过8位长度的字符串，并将其作为方法的返回值。
 public static char readConfirmSelection() ：该方法从键盘读取‘Y’或’N’，并将其作为方法的返回值。
```

#### 第1步 — 实现主程序结构

1. 创建FamilyAccount类及main方法
2. 在main方法中，参照主流程图，实现程序主体结构
3. 测试程序，确认可以正常执行第1和第4菜单选项

#### 第2步 — 实现收入和支出登记处理

1. 在main方法中，参照收入和支出流程，实现“登记收入”功能
2. 测试“登记收入”功能
3. 在main方法中，参照收入和支出流程，实现“登记支出”功能
4. 测试“登记支出”功能

#### 方案0

##### Utility 工具类

```
import java.util.Scanner;
/**
Utility工具类：
将不同的功能封装为方法，就是可以直接通过调用方法使用它的功能，而无需考虑具体的功能实现细节。
*/
public class Utility {
    private static Scanner scanner = new Scanner(System.in);
    /**
 用于界面菜单的选择。该方法读取键盘，如果用户键入’1’-’4’中的任意字符，则方法返回。返回值为用户键入字符。
 */
 public static char readMenuSelection() {
        char c;
        for (; ; ) {
            String str = readKeyBoard(1);
            c = str.charAt(0);
            if (c != '1' && c != '2' && c != '3' && c != '4') {
                System.out.print("选择错误，请重新输入：");
            } else break;
        }
        return c;
    }
 /**
 用于收入和支出金额的输入。该方法从键盘读取一个不超过4位长度的整数，并将其作为方法的返回值。
 */
    public static int readNumber() {
        int n;
        for (; ; ) {
            String str = readKeyBoard(4);
            try {
                n = Integer.parseInt(str);
                break;
            } catch (NumberFormatException e) {
                System.out.print("数字输入错误，请重新输入：");
            }
        }
        return n;
    }
 /**
 用于收入和支出说明的输入。该方法从键盘读取一个不超过8位长度的字符串，并将其作为方法的返回值。
 */
    public static String readString() {
        String str = readKeyBoard(8);
        return str;
    }
 
 /**
 用于确认选择的输入。该方法从键盘读取‘Y’或’N’，并将其作为方法的返回值。
 */
    public static char readConfirmSelection() {
        char c;
        for (; ; ) {
            String str = readKeyBoard(1).toUpperCase();
            c = str.charAt(0);
            if (c == 'Y' || c == 'N') {
                break;
            } else {
                System.out.print("选择错误，请重新输入：");
            }
        }
        return c;
    }
 
 
    private static String readKeyBoard(int limit) {
        String line = "";

        while (scanner.hasNext()) {
            line = scanner.nextLine();
            if (line.length() < 1 || line.length() > limit) {
                System.out.print("输入长度（不大于" + limit + "）错误，请重新输入：");
                continue;
            }
            break;
        }

        return line;
    }
}
```

##### 实现

```
class FamilyAccount{

 public static void main(String[] args){

  boolean isFlag = true;
  //用于记录用户的收入支出情况
  String details = "收支\t账户金额\t收支金额\t说   明\n";
  //初始金额
  int balance = 10000;

  while (isFlag){
   System.out.println("---------------------家庭收支记账软件-----------------\n");
   System.out.println("                       1 收支明细");
   System.out.println("                       2 登记收入");
   System.out.println("                       3 登记支出");
   System.out.println("                       4 退    出");
   System.out.print("                      请选择(1-4): ");
   //获取用户的选择
   char selection = Utility.readMenuSelection();
   switch (selection){
    case '1':
     System.out.println("---------------------1 收支明细-----------------");
     System.out.println(details);
     System.out.println("------------------------------------------------");
     break;
    case '2':
     System.out.print("本次收入金额：");
     int addMoney = Utility.readNumber();
     System.out.print("本次收入说明：");
     String addInfo = Utility.readString();
     System.out.println("----------------------登记完成------------------");
     //处理balance
     balance += addMoney;

     //处理details
     details += ("收入\t" + balance + "\t\t" + addMoney + "\t\t" + addInfo + "\n");
     break;
    case '3':
     System.out.print("本次支出金额：");
     int minusMoney = Utility.readNumber();
     System.out.print("本次支出说明：");
     String minusInfo = Utility.readString();
     System.out.println("----------------------登记完成------------------");
     //处理balance
     if(balance >= minusMoney){
      balance -= minusMoney;
      //处理details
      details += ("支出\t" + balance + "\t\t" + minusMoney + "\t\t" + minusInfo + "\n");
     }
     else{
      System.out.println("超出金额,支付失败");
     }
     break;
    case '4':
     System.out.println("4 退    出");
     System.out.println("确认是否退出(Y/N)");
     char isExit = Utility.readConfirmSelection();
     if(isExit == 'Y'){
      isFlag = false;
     }
     break;
   }
  }
 }
}
```

#### 方案1 我的初步

```
import java.util.Scanner;

class Project1{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);
    
    int income = 0;  //收入  
    int outcome = 0; //支出
    int inCount = 0; //记录收入的次数
    int outCount = 0; //记录支出的次数
    String trucationIn = "";
    String trucationOut = "";
    int basic = 10000;
    
    while(true){

     System.out.println("---------家庭收支记账软件----------");
     System.out.println("\t1 收支明细");
     System.out.println("\t2 登记收入");
     System.out.println("\t3 登记支出");
     System.out.println("\t4 退出");
     System.out.print("\t请选择(1-4)：");

     int choice = scan.nextInt();
     System.out.println();

     if(choice == 1){
       System.out.println("---------当前收支明细记录----------");
       System.out.println("收支\t账户金额\t收支金额\t说明");

        System.out.println("收入\t" + basic + "\t" + income + "\t" + trucationIn);
        System.out.println("支出\t" + basic + "\t" + outcome + "\t" + trucationOut);
     }
     else if(choice == 2){  //登记收入
       System.out.print("\t本次收入金额：");
       int tempIncome = scan.nextInt();
       System.out.println();
       
       System.out.print("\t本次收入说明：");
          trucationIn = scan.next();
       System.out.println();
       
       basic += tempIncome;
       income = tempIncome;
       //inCount++;
     }
     else if(choice == 3){  //登记支出
       System.out.print("\t本次支出金额：");
       int tempOutcome = scan.nextInt();
       System.out.println();
       
       System.out.print("\t本次支出说明：");
       trucationOut = scan.next();
       System.out.println();

       basic -= tempOutcome;
       outcome = tempOutcome;
       //outCount++;
     }
     else if(choice == 4){  //退出
      System.out.println("是否确认退出:(1/0)");
      int outChoice = scan.nextInt();
      if(outChoice == 1){
       break;
      }
      else{
       continue;
      }
     }
     else{  //报错
       System.out.println("输入选项不正确!请重新输入!");
     }
    }
        }
}
```

#### 方案2 添加记录查询功能 for

```
import java.util.Scanner;

class Project1{
        public static void main(String[] args){
                Scanner scan = new Scanner(System.in);
    
    int income = 0;  //收入  
    int outcome = 0; //支出
    int inCount = 0; //记录收入的次数
    int outCount = 0; //记录支出的次数
    String trucationIn = "";
    String trucationOut = "";
    int basic = 10000;
    
    while(true){

     System.out.println("---------家庭收支记账软件----------");
     System.out.println("\t1 收支明细");
     System.out.println("\t2 登记收入");
     System.out.println("\t3 登记支出");
     System.out.println("\t4 退出");
     System.out.print("\t请选择(1-4)：");

     int choice = scan.nextInt();
     System.out.println();

     if(choice == 1){
       System.out.println("---------当前收支明细记录----------");
       System.out.println("收支\t账户金额\t收支金额\t说明");
       /*
       for (int i = 1; i <= inCount; i++)  //遍历每一次收入及其说明
       {
        System.out.println("收入\t" + basic + "\t" + income + "\t" + trucationIn);
       }
       
       for (int i = 1; i <= outCount; i++)  //遍历每一次支出及其说明
       {
        System.out.println("支出\t" + basic + "\t" + outcome + "\t" + trucationOut);
       }
       */
        System.out.println("收入\t" + basic + "\t" + income + "\t" + trucationIn);
        System.out.println("支出\t" + basic + "\t" + outcome + "\t" + trucationOut);
     }
     else if(choice == 2){  //登记收入
       System.out.print("\t本次收入金额：");
       int tempIncome = scan.nextInt();
       System.out.println();
       
       System.out.print("\t本次收入说明：");
          trucationIn = scan.next();
       System.out.println();
       
       basic += tempIncome;
       income = tempIncome;
       //inCount++;
     }
     else if(choice == 3){  //登记支出
       System.out.print("\t本次支出金额：");
       int tempOutcome = scan.nextInt();
       System.out.println();
       
       System.out.print("\t本次支出说明：");
       trucationOut = scan.next();
       System.out.println();

       basic -= tempOutcome;
       outcome = tempOutcome;
       //outCount++;
     }
     else if(choice == 4){  //退出
      System.out.println("是否确认退出:(1/0)");
      int outChoice = scan.nextInt();
      if(outChoice == 1){
       break;
      }
      else{
       continue;
      }
     }
     else{  //报错
       System.out.println("输入选项不正确!请重新输入!");
     }
    }
        }
}
```

***

## 衡量一个功能代码的优劣

1. 正确性
2. 可读性
3. 健壮性
4. 高效率与低存储：时间复杂度/空间复杂度 （衡量算法的好坏）

# 数组

数组(Array)，是多个相同类型数据按一定顺序排列的集合，并使用一个名字命名，并通过编号的方式对这些数据进行统一管理。

数组的常见概念

- 数组名
- 下标(或索引)
- 元素
- 数组的长度
-
- 数组本身是引用数据类型，而数组中的元素可以是任何数据类型，包括基本数据类型和引用数据类型。
- 创建数组对象会在内存中开辟一整块连续的空间，而数组名中引用的是这块连续空间的首地址。
- 数组的长度一旦确定，就不能修改。
- 我们可以直接通过下标(或索引)的方式调用指定位置的元素，速度很快。
-
- 数组的分类：
- 按照维度：
  - 一维数组、
  - 二维数组、
  - 三维数组、
  - …
- 按照元素的数据类型分：
  - 基本数据类型元素的数组、
  - 引用数据类型元素的数组(即对象数组)

## 一维数组

### 一维数组的声明方式

```
type var[] 或 type[] var；
例如：
int a[];
int[] a1;
double b[];
String[] c; //引用类型变量数组
```

Java语言中声明数组时不能指定其长度(数组中元素的数)， 例如： `int a[5];`//非法

### 初始化
```
public class ArrayTest2 {
	
	public static void main(String[] args) {
		
		//一维数组
		//1.1 静态初始化
		int[] ids;
		ids = new int[] {1001,1002,1003,1004};
		//1.2动态初始化
		String[] names = new String[5];
		
		//调用
		//数组索引调用 0开始，-1结尾
		names[0] = "test0";
		names[1] = "test1";
		names[2] = "test2";
		names[3] = "test3";
		names[4] = "test4";
		
		//names[5] = "周扬"; 
		//超过范围
		
		//获取数组长度  .length
		System.out.println(names.length);
		System.out.println(ids.length);
		
		//遍历数组
		for(int i = 0; i < names.length; i++) {
			System.out.println(names[i]);
		}
	}
}

```
#### 动态初始化：数组声明且为数组元素分配空间与赋值的操作分开进行

```
int[] arr = new int[3];
arr[0] = 3;
arr[1] = 9;
arr[2] = 8;
```

```
String names[];
names = new String[3];
names[0] = “钱学森”;
names[1] = “邓稼先”;
names[2] = “袁隆平”;
```

#### 静态初始化：在定义数组的同时就为数组元素分配空间并赋值

```
int arr[] = new int[]{ 3, 9, 8};
或
int[] arr = {3,9,8};
```

```
String names[] = {
“李四光”,“茅以升”,“华罗庚”
}
```

#### 数组元素的引用

- 定义并用运算符new为之分配空间后，才可以引用数组中的每个元素；
- 数组元素的引用方式：数组名[数组元素下标]
- 数组元素下标可以是整型常量或整型表达式。如`a[3] , b[i] , c[6*i];`
- 数组元素下标从0开始；长度为n的数组合法下标取值范围: `0 —>n-1`；
  - 如`int a[]=new int[3];` 可引用的数组元素为a[0]、a[1]、a[2]
- 每个数组都有一个属性length指明它的长度，例如：a.length 指明数组a的长度(元素个数)
- 数组一旦初始化，其长度是不可变的

#### 数组元素的默认初始化值

数组是引用类型，它的元素相当于类的成员变量，因此数组一经分配空间，其中的每个元素也被按照成员变量同样的方式被隐式初始化。例如：

- 对于基本数据类型而言，默认初始化值各有不同
- 对于引用数据类型而言，默认初始化值为null(注意与0不同！)

![](C:/NoteBook/pictures/564975123220667.png =598x)

```
public class Test {
    public static void main(String args[]){
        int a[]= new int[5];
        System.out.println(a[3]); //a[3]的默认值为0
    }
}
```
```
public class ArrayTest2 {
	
	public static void main(String[] args) {
		
		int[] arr = new int[4];
		
		for(int i = 0; i < arr.length; i++) {
				System.out.println(arr[i]);
		}
		System.out.println("**********");
		
		short[] arr1 = new short[4];
		for(int i =0; i < arr1.length; i++) {
			System.out.println(arr1[i]);
		}
		System.out.println("**********");
		
		float[] arr2 = new float[4];
		for(int i = 0; i < arr2.length; i++) {
			System.out.println(arr2[i]);
		}
		System.out.println("**********");
		
		char[] arr3 = new char[4];
		for(int i = 0; i < arr3.length; i++) {
			System.out.println(arr3[i]);
		}
		if(arr3[0] == 0) {
			System.out.println("== 0");
		}
		System.out.println("**********");
		
		boolean[] arr4 = new boolean[4];
		for(int i = 0; i < arr4.length; i++) {
			System.out.println(arr4[i]);
		}
		System.out.println("**********");
		
		String[] arr5 = new String[4];
		for(int i = 0; i < arr5.length; i++) {
			System.out.println(arr[i]);
		}
		if(arr5[0] == null) {
			System.out.println("null");
		}
		System.out.println("**********");
	}
}
```
#### 创建基本数据类型数组
##### 创建基本数据类型数组 (1)

Java中使用关键字new来创建数组
如下是创建基本数据类型元素的一维数组

```
public class Test{
    public static void main(String args[]){
        int[] s;
        s = new int[10];
        for ( int i=0; i<10; i++ ) {
            s[i] =2*i+1;
            System.out.println(s[i]);
        }
    }
}
```

![](C:/NoteBook/pictures/531785323239093.png =296x)

##### 创建基本数据类型数组 (2)

```
public class Test{
    public static void main(String args[]){
        int[] s;
        s = new int[10];
        //int[] s=new int[10];
        //基本数据类型数组在显式赋值之前，
        //Java会自动给他们赋默认值。
        for ( int i=0; i<10; i++ ) {
            s[i] =2*i+1;
            System.out.println(s[i]);
        }
    }
}
```

![](C:/NoteBook/pictures/397325523247126.png =330x)

##### 创建基本数据类型数组 (3)

```
public class Test{
    public static void main(String args[]){
        int[] s;
        s = new int[10];
        for ( int i=0; i<10; i++ ) {
            s[i] =2*i+1;
            System.out.println(s[i]);
        }
    }
}
```

![](C:/NoteBook/pictures/39505523226960.png =294x)


#### 一维数组的内存解析

![](C:/NoteBook/pictures/170022423239096.png =507x)

```
int[] arr = new int[4]
arr[1] = 10;

arr = new int[3]
//新的内存空间
arr[1] == 0
```
#### 练习1用索引读数组

```        
升景坊单间短期出租4个月，550元/月（水电煤公摊，网费35元/月），空调、卫生间、厨房齐全。
屋内均是IT行业人士，喜欢安静。所以要求来租者最好是同行或者刚毕业的年轻人，爱干净、安静。
```
```
package com.atguigu.java;

public class ArrayTest3 {
	public static void main(String[] args) {
		int[] arr = new int[] {8,2,1,0,3};
		int[] index = new int[] {2,0,3,2,4,0,1,3,2,3,3};
		String tel = "";
		for(int i = 0; i < index.length; i++) {
			tel += arr[index[i]];
		}
		System.out.println("connection:" + tel);
	}
}

//18013820100
```

#### 练习2从键盘读入学生成绩，找出最高分，并输出学生成绩等级。
```
2. 从键盘读入学生成绩，找出最高分，并输出学生成绩等级。
成绩>=最高分-10 等级为’A’ 
成绩>=最高分-20 等级为’B’
成绩>=最高分-30 等级为’C’ 
其余 等级为’D’
提示：先读入学生人数，根据人数创建int数组，存放学生成绩。
```
```
import java.util.Scanner;

public class ArrayStudentTest {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		
		System.out.println("student number:")
		int students = scan.nextInt();
		int[] arr = new int[students];
		int max = 0;
		
		for(int i = 0; i < students;i++) {
			System.out.print("student" + i + "\'s " + "score: ");
			int num = scan.nextInt();
			System.out.print(num);
			arr[i] = num;
			if(num < 0) {
				break;
			}
			if(i == 0) {
				max = arr[i];
			}
			else {
				if(arr[i - 1] < arr[i]) {
					max = arr[i];
				}
			}
			System.out.println();
		}
		System.out.println("max score is :" + max);
		for(int i = 0; i < arr.length; i++) {
			if(arr[i] >= (max - 10)) {
				System.out.println("student" + i + "is " + arr[i] + " grade" + ":A");
			}
			else if(arr[i] >= (max - 20)) {
				System.out.println("student" + i + "is " + arr[i] + " grade" + ":B");
			}
			else if(arr[i] >= (max - 30)) {
				System.out.println("student" + i + "is " + arr[i] + " grade" + ":C");
			}
			else {
				System.out.println("student" + i + "is " + arr[i] + " grade" + ":D");
			}
		}
	}
}
```
```
import java.util.Scanner;

public class ArrayStudentTest {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		
		//学生数量
		System.out.print("student number is: ");
		int stuNum = scan.nextInt();
		System.out.println();
		
		//每个学生的成绩录入
		int[] scores = new int[stuNum];
		for(int i = 0; i < stuNum; i++) {
			System.out.print("student" + i + "\'s" + "score is");
			scores[i] = scan.nextInt();
			System.out.println(scores[i]);
		}
		
		//最大值
		int max = 0;
		for(int i = 0; i < scores.length - 1; i++) {
			if(scores[i] > scores[i + 1]) {
				max = scores[i];
			}
		}
		System.out.println("max scores is " + max);
		
		//成绩评价
		for(int i = 0; i < scores.length; i++) {
			System.out.print("student" + i + "\'s grade is ");
			char level = ' ';
			if(scores[i] >= (max - 10)) {
				level = 'A';
			}
			else if(scores[i] >= (max -20)) {
				level = 'B';
			}
			else if(scores[i] >= (max -30)) {
				level = 'C';
			}
			else {
				level = 'D';
			}
			System.out.println(level);
		}	
	}
}
```
## 二维数组[][]：数组中的数组
 二维数组就相当于是一个表格

```
public class ArrayTest02 {
	public static void main(String[] args) {
		int[][] arr1 = new int[][] {{1,2,3},{4,5},{6,7,8}};
		
		String[][] arr2 = new String[3][2];
		
		String[][] arr3 = new String[3][];
	}
}
```
```
public class ArrayTest03 {
	public static void main(String[] args) {
		int[][] arr = new int[4][3];
		
		System.out.println(arr[0]);  //[I@36baf30c  [ 一维数组  地址值
		System.out.println(arr[0][0]);  //0 默认值
		
		System.out.println(arr);  //[[I@7a81197d  [[ 二维数组  地址值
		
		String[][] arr2 = new String[4][3];
		System.out.println(arr2);  //[[Ljava.lang.String;@5ca881b5 二维数组地址 包括String包的地址
		System.out.println(arr[0]);  //[I@36baf30c 一维数组地址
		System.out.println(arr[0][0]); //null 空
		}
}
```
### 格式1（动态初始化）：int[][] arr = new int[3][2];
- 定义了名称为arr的二维数组 
- 二维数组中有3个一维数组
- 每一个一维数组中有2个元素
- 一维数组的名称分别为arr[0], arr[1], arr[2]
- 给第一个一维数组1脚标位赋值为78写法是：arr[0][1] = 78;

其中
- 外层元素的初始化值为：地址值
- 内层元素的初始化值为：与一维数组的初始化情况相同
### 格式2（动态初始化）：int[][] arr = new int[3][];
- 二维数组中有3个一维数组。
- 每个一维数组都是默认初始化值null (注意：区别于格式1）
- 可以对这个三个一维数组分别进行初始化
- arr[0] = new int[3]; arr[1] = new int[1]; arr[2] = new int[2];
- 注：
    - int[][]arr = new int[][3]; //非法

其中
- 外层元素的初始化值为：null
- 内层元素的初始化值为：不能调用，否则出错
### 格式3（静态初始化）：int[][] arr = new int[][]{{3,8,2},{2,7},{9,0,1,6}};
- 定义一个名称为arr的二维数组，二维数组中有三个一维数组
- 每一个一维数组中具体元素也都已初始化
   - 第一个一维数组 arr[0] = {3,8,2};
   - 第二个一维数组 arr[1] = {2,7};
   - 第三个一维数组 arr[2] = {9,0,1,6};
   - 第三个一维数组的长度表示方式：arr[2].length;
- 注意特殊写法情况：int[] x,y[]; x是一维数组，y是二维数组。
- Java中多维数组不必都是规则矩阵形式

### 获取数组的长度 行数
```
int[][] numTable = new int[][] {{3,5,8},{12,9},{7,0,6,4}};
numTable.length == 3
```
### 遍历二维数组
初始化方式为：`int[][] arr = new arr[][]{{},{}}`

若遍历的位置`arr[i][j]`无相应的数值，则报错
```
public class ArrayTest02 {
	public static void main(String[] args) {

		int[][] numTable = new int[][] {{1,2,3},{4,5,9,10},{6,7,8}};
		
		for(int i = 0; i < numTable.length; i++) {
		
			for(int j = 0; j < numTable[i].length; j++) {
				System.out.print(numTable[i][j]);
			}
			
			System.out.println();
		}
	}
}
```
### 练习 2 获取arr数组中所有元素的和。
![](C:/NoteBook/pictures/50595520220671.png =288x)

```
public class ArrayTest02 {
	public static void main(String[] args) {
		int[][] numTable = new int[][] {{3,5,8},{12,9},{7,0,6,4}};

		for(int i = 0; i < numTable.length; i++) {
			
			for(int j = 0; j < numTable[i].length; j++) {
				System.out.print(numTable[i][j]);
			}
			
			System.out.println();
		}
	}
}
```

### 练习3
```
声明：int[] x,y[]; 在给x,y变量赋值以后，以下选项允许通过编译的是：
a ) x[0] = y; no
b) y[0] = x; yes
c) y[0][0] = x; no
d) x[0][0] = y; no
e) y[0][0] = x[0]; yes
f) x = y; no
提示：
一维数组：int[] x 或者int x[] 
二维数组：int[][] y 或者 int[] y[] 或者 int y[][]
```
### 练习4 使用二维数组打印一个 10 行杨辉三角。

```
【提示】
1. 第一行有 1 个元素, 第 n 行有 n 个元素
2. 每一行的第一个元素和最后一个元素都是 1
3. 从第三行开始, 对于非第一个元素和最后一个元素的元素。即：
yanghui[i][j] = yanghui[i-1][j-1] + yanghui[i-1][j]
```
```
public class ArrayTest{
        public static void main(String[] args){

                int[][] yangHui = new int[10][10];

                for(int i = 0; i < yangHui.length; i++){
                        for(int j = 0; j <= i; j++){
                                yangHui[i][0] = 1;
                                yangHui[i][i] = 1;
                                if(i >= 2 && j > 0 && j < i){
                                        yangHui[i][j] = yangHui[i - 1][j - 1] + yangHui[i - 1][j];
                                }
                        }
                }

                for(int i = 0; i < yangHui.length; i++){
                        for(int j = 0; j <= i; j++){
                                System.out.print(yangHui[i][j] + " , ");
                        }
                        System.out.println();
                }
        }
}
```
```
public class test002 {
	public static void main(String[] args) {
		int[][] yanghui = new int[10][];
		
		for(int i = 0; i < yanghui.length; i++) {
			yanghui[i]= new int [i + 1];
			yanghui[i][0] = yanghui[i][i] = 1;
			if(i > 1) {
				for(int j = 1; j < yanghui[i].length - 1; j ++) {
					yanghui[i][j] = yanghui[i-1][j-1] + yanghui[i-1][j];
				}
			}
		}
		
		for(int i = 0; i < yanghui.length; i++) {
			System.out.println();
			for(int j = 0; j < yanghui[i].length; j++) {
				System.out.print(yanghui[i][j] + "\t");
			}
		}
	}
}
```
### 【拓展之笔试题】创建一个长度为6的int型数组，要求数组元素的值都在1-30之间，且是随机赋值。同时，要求元素的值各不相同。
```
```
***

## 数组中涉及到的常见算法
1. 数组元素的赋值(杨辉三角、回形数等)
2. 求数值型数组中元素的最大值、最小值、平均数、总和等
3. 数组的复制、反转、查找(线性查找、二分法查找)
4. 数组元素的排序算法

### 练习5 随机数
```
定义一个int型的一维数组，包含10个元素，分别赋一些随机整数，
然后求出所有元素的最大值，最小值，和值，平均值，并输出出来。
要求：所有随机数都是两位数。
提示；
[0,1) * 90 [0,90) + 10  [10,100) [10,99]
(int)(Math.random() * 90 + 10)
```
```
public class Test002 {
	public static void main(String args[]) {
		int[] arr = new int[10];
		
		for(int i = 0; i < arr.length; i++) {
			arr[i] = (int)(Math.random() * (99 - 10 + 1) + 10);
		}
		
		//最大值
		int maxValue = arr[0];
		for(int i = 0; i < arr.length; i++) {
			if(maxValue < arr[i]) {
				maxValue = arr[i];
			}
		}
		System.out.println("最大值: " + maxValue);
		
		//最小值
		int minValue = arr[0];
		for(int i = 0; i < arr.length; i++) {
			if(minValue > arr[i]) {
				minValue = arr[i];
			}
		}
		System.out.println("最大值: " + minValue);
		
		//和值
		int sumValue = 0;
		for(int i = 0; i < arr.length; i++) {
			sumValue += arr[i];
		}
		System.out.println("和值: " + sumValue);
		
		//均值
		float avgValue = sumValue / (float)(arr.length);
		System.out.println("均值: " + avgValue);
	}
}
```
***
### 练习 6 数组复制 System.arraycopy()
```
使用简单数组
(1)创建一个名为ArrayTest的类，在main()方法中声明array1和array2两个变量，
他们是int[]类型的数组。
(2)使用大括号{}，把array1初始化为8个素数：2,3,5,7,11,13,17,19。
(3)显示array1的内容。
(4)赋值array2变量等于array1，修改array2中的偶索引元素，使其等于索引值
(如array[0]=0,array[2]=2)。打印出array1。
思考：array1和array2是什么关系？
拓展：修改题目，实现array2对array1数组的复制
```
```
public class Test003 {
	public static void main(String args[]) {
		int[] array1 = {2,3,5,7,11,13,17,19};
		
		for(int i = 0; i < array1.length; i++) {
			System.out.println(array1[i]);
		}  
		
		int[] array2 = new int[array1.length];
		System.arraycopy(array1, 0, array2, 0, array1.length);
		
////		不是数组的复制 类似于快捷方式的创建
//		int[] array2;
//		array2 = array1;
		
//		for(int i = 0; i < array1.length; i++) {
//			array2[i] = array1[i];
//		}
		
		for(int i = 0; i < array2.length; i++) {
			if(i % 2 == 0) {
				array2[i] = i;
			}
			
			System.out.println(array2[i]);
		}
	}
}}
}
```
***
### 数组的复制，反转，查找(线性查找，二分法查找)
```
public class Test004 {
	
	public static void main(String args[]) {
		
		String[] arr = new String[] {"JJ", "DD", "MM" , "BB","AA" }; 
		
		System.out.print("原值" + "\t");
		for(int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + "\t");
		}
		System.out.println();
		
//		数组的复制
		String[] arr1 = new String[arr.length];
		for(int i = 0; i < arr.length; i++) {
			arr1[i] = arr[i];
		}
		
//		数组的反转
//		1.
		String[] arr2 = new String[arr.length];
		for(int i = 0; i < arr.length; i++) {
			arr2[arr.length - i - 1] = arr[i];
		}
//		2.
		for(int i = 0; i < arr.length / 2; i++) {
			String temp = arr[i];
			arr[i] = arr[arr.length - i - 1];
			arr[arr.length - i - 1] = temp;
		}
//		3.
		for(int i = 0, j = arr1.length - 1; i < j; i++, j--) {
			String temp = arr1[i];
			arr1[i] = arr1[j];
			arr1[j] = temp;
		}
		
		System.out.print("复制" + "\t");
		for(int i = 0; i < arr.length; i++) {
			System.out.print(arr1[i] + "\t");
		}
		System.out.println();
		
		System.out.print("反转1" + "\t");
		for(int i = 0; i < arr.length; i++) {
			System.out.print(arr2[i] + "\t");
		}
		System.out.println();
		
		System.out.print("反转2" + "\t");
		for(int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + "\t");
		}
		System.out.println();
		
		System.out.print("反转3" + "\t");
		for(int i = 0; i < arr1.length; i++) {
			System.out.print(arr1[i] + "\t");
		}
		System.out.println();
		
//		数组的查找
//		1.1线性查找
		String dest = "BB";
		boolean isFlag = true;
		
		for(int i = 0; i < arr.length; i++) {
			if(dest.equals(arr[i])) {
				System.out.println("找到指定元素的位置为：" + i);
				isFlag = false;
				break;
			}
		}
		
		if(isFlag) {
			System.out.println("查询不到");
		}
		
//      1.2
		
        for(int i = 0; i < arr.length; i++) {
			if(dest == arr[i]) {
				System.out.println("已找到：" + i);
				break;
			}
			if( i == arr.length - 1) {
				System.out.println("未找到");
			}
		}
		

//		2.二分法查找 
//		前提：所要查找的数组必须有序
		int[] arr02 = new int[] {-98,-34,2,34,54,66,79,105,210,333};
		
		int dest1 = 2;
		int head = 0; 
		int end = arr02.length - 1;
		boolean isFlag02 = true;
		
		while(head <= end) {
			
			int middle = (head + end) / 2;
			
			if(dest1 == arr02[middle]) {
				System.out.println("找到指定元素的位置为：" + middle);
				isFlag02 = false;
				break;
			}
			else if(arr02[middle] > dest1) {
				end = middle - 1;
			}
			else {
				head = middle + 1;
			}
		}
		
		if(isFlag02) {
			System.out.println("没有找到");
		}
	}
}
```
***
### 排序(十大内部排序)
#### 选择排序（直接选择排序，堆排序）

#### 交换排序（冒泡排序，快速排序）  
##### 冒泡排序
```
public class Test005 {
	public static void main(String args[]) {
		int[] arr = new int[] {1,2,3,4,6,7,8,90,4,3,2,5,8};
		
		System.out.print("原值：\t");
		for(int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + "\t");
		}
		System.out.println();
		
		System.out.print("排序：\t");
		
		//冒泡排序
		for(int i = 0; i < arr.length - 1; i++) {
			
			for (int j = 0; j < arr.length - 1 - i; j++) {
				
                if(arr[j] > arr[j + 1]) {
					int temp = arr[j];
					arr[j] = arr[j + 1];
					arr[j + 1] = temp;
				}
			}
		}
		
		for(int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + "\t");
		}
	}
}
```
##### 快速排序
#### 插入排序（直接插入排序，拆半插入排序，Shell排序）
#### 归并排序
#### 桶式排序
#### 基数排序

***

## java.util.Arrays Arrays工具类
- Arrays.equals(arr1,arr2)  返回boolean类型
- arr.toString() 返回String类型
- fill(arr,value) 往arr中填充value
- sort(arr) 排序
- binarySearch(arr,value) 二分法查找，在arr中查找value 

***

## 数组中常见异常
### 数组角标越界的异常 ArrayIndexOutofBoundsExcetion --下标超过数组范围
### 空指针异常  NullPointerException 
```
public class Test008 {
	public static void main(String args[]) {
//		情况一：
		int[] arr1 = new int[]{12,3,4};
		arr1 = null;
		System.out.println(arr1[0]);  //报空指针错
		
//		情况二
		int[][] arr2 = new int[4][];
		System.out.println(arr2[0]); //返回null
		System.out.println(arr2[0][0]); //报空指针错

//		情况三
		String[] arr3 = new String[] {"AA","BB","CC"};
		arr3[0] = null;
		System.out.println(arr3[0].toString());
	}
}
```
## 对象数组


***

# 面向对象
## 概要
### Java学习三条主线

- 面向对象的三大特征
  1. 封装性
  2. 继承性
  3. 多态性
  4. （抽象性）

- Java类及类的成员
   - 属性，
   - 方法，
   - 构造器，
   - 代码块
   - 内部类 

- 其他关键字
   - this
   - super
   - static
   - final
   - abstract
   - interface
   - package
   - import
   - ...
   - 
![](C:/NoteBook/pictures/238793222227242.png =593x)

### 面向过程与面向对象
- 面向过程 POP
   - 强调的是功能行为，以函数为最小单位，考虑怎么做
- 面向对象 OOP
   - 强调具备功能的对象，以类/对象为最小单位，考虑谁来做 

### 面向对象的两个要素
- 类
   - 对一类事物的描述，是抽象的，概念上的定义
- 对象（实例）
   - 是实际存在的该类事物的每个个体

1. 面向对象程序设计的重点是类的设计
2. 设计类，就是设计类的成员

### 类和对象的使用（面向对象思想落地的实现）

1. 创建类，设计类的成员
2. 创建类的对象
3. 通过”对象.属性“，”对象.方法“调用对象的结构

- 如果创建了一个类的多个对象，则每个对象都独立的拥有一套类的属性（非static的）
   - 意味着，如果我们修改一个对象的属性a，则不影响另外一个对象属性a的值

```
package StudyTest;

//  测试类
public class Test010 {
	public static void main(String args[]) {
//		创建Person类的对象 = 类的实例化 = 实例化类
		Person p1 = new Person();
		
//		调用对象的结构：属性，方法
//		调用属性：对象.属性
		p1.name = "Tom";
		p1.isMale = true;
		System.out.println(p1.name);
		
//		调用方法：对象.方法
		p1.eat();
		p1.sleep();
		p1.talk("Chinese");
		
		Person p2 = new Person();
		System.out.println(p2.name);
//		输出 null 默认初始化值
		System.out.println(p2.isMale);
//		输出 false 默认初始值
		
//		将p1变量保存的对象地址值赋给p3，导致p1和p3指向了维空间的同一个对象实体
//		因而改变p3的属性的同时也改变了p1的属性
		Person p3 = p1;
		System.out.println(p3.name); //Tom
		
		p3.age = 10;
		System.out.println(p1.age);  //10
	}
}

class Person{
	
//	属性
	String name;
	int age = 1;
	boolean isMale;
	
//	方法
	public void eat() {
		System.out.println("人可以吃饭");
	}
	
	public void sleep() {
		System.out.println("人可以睡觉");
	}
	
	public void talk(String language) {
		System.out.println("人可以说话,语言为: " + language);
	}
}
```

### 对象的内存解析

编译完程序以后，生成一个或多个字节码文件

我们使用JVM中的类的加载器和解释器对生成的字节码文件进行解释运行。意味着，需要将字节码文件对应的类加载到内存中，涉及到内存解析。

#### JVM

- 堆（Heap）
   - 此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例都在这里分配内存。
   - 这一点在Java虚拟机规范中的描述是：所有的对象实例以及数组都要在堆上分配。
- 通常所说的栈（Stack），是指虚拟机栈。
  - 虚拟机栈用于存储局部变量等。
  - 局部变量表存放了编译期可知长度的各种基本数据类型（boolean、byte、char 、 short 、 int 、 float 、 long 、double）、对象引用（reference类型，它不等同于对象本身，是对象在堆内存的首地址）。 方法执行完，自动释放。
- 方法区（Method Area）
  - 用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

《JVM规范》

![](C:/NoteBook/pictures/451485022220954.png =781x)

#### 内存解析的说明

- 引用类型的变量，只可能存储两类值：null 或 地址值（含变量的类型）

![](C:/NoteBook/pictures/242364122239382.png =688x)

### 匿名对象
- 不定义对象的句柄，而直接调用这个对象的方法。这样的对象叫做匿名对象。
   - 如：new Person().shout(); 
- 使用情况
   - 如果对一个对象只需要进行一次方法调用，那么就可以使用匿名对象。
   - 我们经常将匿名对象作为实参传递给一个方法调用。

1. 理解：创建的变量没有显式的赋给一个变量名，即为一个匿名对象
2. 特征：匿名对象只能调用一次
3. 使用：

```
public class Test20 {
	public static void main(String args[]) {
		Phone p = new Phone();
//		p = null  //打印 == null 
//		则p.price 报错
		System.out.println(p);  //返回地址值
		
		p.sendEmail();
		p.playGame();
		
//		匿名对象
		new Phone().sendEmail();
		new Phone().playGame();
//		上两个语句并不是同一个对象
		new Phone().price = 1999;
		new Phone().showPrice();  //不是1999 而是 默认值0.0

//		匿名对象的使用
		PhoneMall mall = new PhoneMall();
//		mall.show(p);
		mall.show(new Phone());
	}
}

class PhoneMall{
	
	public void show(Phone phone) {
		phone.sendEmail();
		phone.playGame();
	}
}

class Phone{
	double price; //价格
	
	public void sendEmail(){
		System.out.println("发送邮件");
	}
	
	public void playGame() {
		System.out.println("玩游戏");
	}
	
	public void showPrice() {
		System.out.println("手机价格为：" + price);
	}
}
```
### 属性
语法格式：
`修饰符 数据类型 属性名 = 初始化值 ; `
- 说明1: 修饰符
   - 常用的权限修饰符(声明属性时，指明其权限; 封装性)有：private、缺省、protected、public
   - 其他修饰符：static、final (暂不考虑)
- 说明2：数据类型
   - 任何基本数据类型(如int、Boolean) 或 任何引用数据类型。
- 说明3：属性名
   - 属于标识符，符合命名规则和规范即可。
   
#### 变量的分类：成员变量与局部变量
- 在方法体外，类体内声明的变量称为成员变量。
- 在方法体内部声明的变量称为局部变量。
- 注意：二者在初始化值方面的异同:
    - 同：都有生命周期
    - 异：局部变量除形参外，均需显式初始化。

![](C:/NoteBook/pictures/328105822227247.png =497x)

![](C:/NoteBook/pictures/2235922247413.png =691x)

**对象属性的默认初始化赋值**
- 属性：类的属性，根据其类型都有默认的初始值
- 局部变量没有默认初始化值
   - 形参在调用时赋值即可
- 当一个对象被创建时，会对其中各种类型的成员变量自动进行初始化赋值。除了基本数据类型之外的变量类型都是引用类型，如上面的Person及前面讲过的数组。

![](C:/NoteBook/pictures/465885922240082.png =357x)

```
public class Test011 {
	public static void main(String args[]) {
		User u1 = new User();
		System.out.println(u1.name);
		System.out.println(u1.age);
		System.out.println(u1.isMale);
		
		u1.talk("汉语");
		
		u1.eat();
	}
}

class User {
//	属性
//	private String name;  //权限修饰符
	String name;
	public int age;
	boolean isMale;
	
	public void talk(String language) {  //language 形参;也是局部变量
		System.out.println("我们使用" + language + "进行交流");
		
	}
	
	public void eat() {
		String food = "烙饼";
//		public String foodTest = "烙饼"; 报错，局部变量不可以使用权限修饰符
//		final String foodTest = "烙饼"; 局部变量可以使用final修饰符
		System.out.println("北方人喜欢吃" + food);
	}
	
}
```
### 方法(method)

- 描述类应该具有的功能
- 比如：
     - Math类：sqrt() \ random() \...
     - Scanner类：nextXxx() ...
     - Arrays类：sort() \ binarySearch() \ toString() \ equals() \ ...
     
1. 举例:
    - public void eat(){}
    - public void sleep(int hour){}
    - public String getName(){}
    - public String getNation(String nation){}
2. 方法的声明: 

```
权限修饰符 返回值类型 方法名(形参列表){
        方法体;    
}
```
```
public class Test012 {
	public static void main(String args[]) {
		
	}
}

class Customer{
	String name;
	int age;
	boolean isMale;
	
	public void eat() {
		System.out.println("客户吃饭");
	}
	
	public void sleep(int hour) {
		System.out.println("休息了" + hour + "小时");
	}
	
	public String getName() {
		return name;
	}
	
	public String getNation(String nation) {
		String info = "我的国籍是：" + nation;
		return info;
	}
	
}
```

#### 权限修饰符

Java规定的4种权限修饰符：private ,  public , 缺省 , protected 

#### 返回值类型 
- 有返回值
   - 如果方法有返回值，则必须在方法声明时，指定返回值的类型。
   - 同时方法中需要使用return关键字来返回指定类型的变量或常量。
 
```
 	public String getName() {
		
		if(age > 18) {
			return name;
		}
	}
	报错，必须要有返回值，if可能没有返回值
```

- 没返回值
   - 方法声明时，使用void来表示
   - 通常没有返回值的方法中不需要使用return，如果使用，只能`return;`表示结束此方法的意思
   
```
	public void eat() {
		System.out.println("客户吃饭");
		return;  //表示结束
//      return后不可以声明表达式
    	System.out.println("你好"); //无法到达 Unreachable code
	}
```

##### return关键字的使用
1. 使用范围：使用在方法体中
2. 使用：
    - 结束方法
    - 针对有返回值的方法，使用”`return 数据`“的方法返回所要的数据
3. return关键字后面不可以声明执行语句 
- 该不该有返回值
    - 题目要求
    - 凭经验
#### 方法名：
- 属于标识符，遵循标识符的规则和规范

#### 形参列表：
- 方法可以声明零个，一个或多个形参
- 格式：`数据类型1 形参1,数据类型2 形参2,...`
- 该不该定义形参
    - 题目要求
    - 凭经验，具体问题具体分析   
#### 方法体
- 方法功能的体现

```
public class Test012 {
	public static void main(String args[]) {
		Customer cust1 = new Customer();
		
		int[] arrTest = new int[] {1,2,3,4,7,5,6};
		cust1.outPrint(arrTest);
		cust1.sort(arrTest);
		cust1.outPrint(arrTest);
	}
}

class Customer{
	String name;
	int age;
	boolean isMale;
	
	public void sort(int[] arr) {
		for(int i = 0; i < arr.length - 1; i++) {
			for(int j = 0; j < arr.length - 1 - i; j++) {
				if(arr[i] > arr[i + 1]) {
					int temp = arr[i];
					arr[i] = arr[i + 1];
					arr[i + 1] = temp;
				}
			}
		}
	}
	
	public void outPrint(int[] arr) {
		System.out.print("数组为: \t");
		for(int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + " , ");
		}
		System.out.println();
	}
}
```

#### 方法的使用中，可以调用当前类的属性或方法

- 方法的内部可以调用方法
     - 递归：方法中再次调用自身

```
public class Test012 {
	public static void main(String args[]) {
		Customer cust1 = new Customer();
		
		cust1.eat();
			
		cust1.sleep(1);
	}
}

class Customer{

	public void eat() {
		System.out.println("客户吃饭");
	}
	
	public void sleep(int hour) {
		System.out.println("休息了" + hour + "小时");
		
		eat();
	}
}
```
- 不能在方法中声明方法

```
public void test(){
    public void testTwice(){} //报错，不可以在方法体内部声明方法
}
```
### 练习
#### 1.创建一个Person类
```
要求：
(1)创建Person类的对象，设置该对象的name、age和sex属性，
   调用study方法，输出字符串“studying”，
   调用showAge()方法显示age值，
   调用addAge()方法给对象的age属性值增加2岁。
(2)创建第二个对象，执行上述操作，体会同一个类的不同对象之间的关系。
```
![](C:/NoteBook/pictures/238341115220955.png =166x)

```
public class Test013 {
	public static void main(String args[]) {
		PersonTest per1 = new PersonTest();
		
		per1.name = "kk";
		per1.age = 10;
		per1.sex = 1;
		
		per1.study();
		per1.showAge();
		System.out.println(per1.addAge(2));
		
	}
}

class PersonTest{
	String name;
	int age;
	int sex;
	
	public void study() {
		System.out.println("studing");
	}
	
	public void showAge() {
		System.out.println("age = " + age);
	}
	
	public int addAge(int i) {
		age += i;
		return age;
	}
}
```
#### 2.利用面向对象的编程方法，设计类Circle计算圆的面积。
```
public class Test014 {
	public static void main(String args[]) {
		Circle c1 = new Circle();
		c1.radius = 10;
		System.out.println("圆的面积为：" + c1.findArea());
	}
}

class Circle{
	//属性
	double radius;
	
	//求圆的面积
	public double findArea() {
		double area = Math.PI * radius *radius;
		return area;
	}
}
```
```
public class Test014 {
	public static void main(String args[]) {
		Circle c1 = new Circle();
		
		c1.r = 10;
		c1.findArea();
	}
}

class Circle{
	double r;
	public void findArea() {
		double area = r * r * Math.PI;
		System.out.println("圆的面积为：" + area);
	}
}
```
```
public class Test014 {
	public static void main(String args[]) {
		Circle c1 = new Circle();
		
		System.out.println("圆的面积为：" + c1.findArea(10));
		
	}
}

class Circle{
	public double findArea(double r) {
		double area = r * r * Math.PI;
		return area;
	}
}
```
#### 3.1 编写程序，声明一个method方法，在方法中打印一个`10*8 的*型矩形`，在main方法中调用该方法。
```
public class Test015 {
	public static void main(String args[]) {
		Test015 test = new Test015();
		test.method(10,8);
	}
	
	public void method(int column,int row) {
		for(int i = 0; i < column; i++) {
			for(int j = 0; j < row; j++) {
				System.out.print("*");
			}
			System.out.println();
		}
	}
}
```
#### 3.2 修改上一个程序，在method方法中，除打印一个`10*8的*型矩形`外，再计算该矩形的面积，并将其作为方法返回值。在main方法中调用该方法，接收返回的面积值并打印。

#### 3.3 修改上一个程序，在method方法提供m和n两个参数，方法中打印一个`m*n的*型矩形`，并计算该矩形的面积， 将其作为方法返回值。在main方法中调用该方法，接收返回的面积值并打印。
```
public class Test015 {
	public static void main(String args[]) {
		Test015 test = new Test015();
		test.method(10,8);
	}
	
	public void method(int column,int row) {
		for(int i = 0; i < column; i++) {
			for(int j = 0; j < row; j++) {
				System.out.print("*");
			}
			System.out.println();
		}
		System.out.println("面积为：" + countArea(column,row));
	}
	
	public int countArea(int column,int row) {
		return column * row;
	}
}
```
```
public class Test016 {
	public static void main(String args[]) {
		Test016 test = new Test016();
		System.out.println("面积为 " + test.method(10,8));
	}
	
	public int method(int m, int n) {
		for(int i = 0; i < m; i++) {
			for(int j = 0; j < n; j++) {
				System.out.print("*");
			}
			System.out.println();
		}
		
		return m * n;
	}
}
```
#### 对象数组题目：
```
定义类Student，包含三个属性：
  学号number(int)，
  年级state(int)，
  成绩score(int)。 
创建20个学生对象，学号为1到20，年级和成绩都由随机数确定。
问题一：打印出3年级(state值为3）的学生信息。
问题二：使用冒泡排序按学生成绩排序，并遍历所有学生信息
提示：
1) 生成随机数：Math.random()，返回值类型double; 
2) 四舍五入取整：Math.round(double d)，返回值类型long。
```
```
public class Test017 {
	public static void main(String args[]) {
//		声明Student类型的数组
		Student[] stus = new Student[20];
		
		for(int i = 0; i < stus.length; i++) {
//			给数组元素赋值
			stus[i] = new Student();
//			给Student对象的属性赋值
			stus[i].number = ( i + 1);
//			年级[1,6]
			stus[i].state = (int)(Math.random() * (6 - 1 + 1) + 1);
//			成绩[0,100]
			stus[i].score = (int)(Math.random() * (100 - 0 + 1));
		}
		
//		遍历学生数组
		for(int i = 0; i < stus.length; i++) {
//			System.out.println(stus[i]);  返回地址值
			System.out.println(stus[i].info());
		}
		
//		问题1：打印出3年级学生的信息
		System.out.println("3年级学生的信息如下：");
		for(int i = 0; i < stus.length; i++) {
			if(stus[i].state == 3) {
				System.out.println(stus[i].info());
			}
		}
		
//		问题2：使用冒泡排序按学生成绩排序，并遍历学生信息
		System.out.println("排序：");
		for(int i = 0; i < stus.length - 1; i++) {
			for(int j = 0; j < stus.length - 1 - i; j++) {
				if(stus[j].score < stus[j + 1].score) {
					Student temp = stus[j];
					stus[j] = stus[j + 1];
					stus[j + 1] = temp;
				}
			}
		}
		
		for(int i = 0; i < stus.length; i++) {
			System.out.println(stus[i].info());
		}
	}
}
```
封装改进
```
public class Test018 {
	public static void main(String args[]) {
//		声明StudentTest类型的数组
		StudentTest[] stus = new StudentTest[20];
		
		for(int i = 0; i < stus.length; i++) {
//			给数组元素赋值
			stus[i] = new StudentTest();
//			给StudentTest对象的属性赋值
			stus[i].number = ( i + 1);
//			年级[1,6]
			stus[i].state = (int)(Math.random() * (6 - 1 + 1) + 1);
//			成绩[0,100]
			stus[i].score = (int)(Math.random() * (100 - 0 + 1));
		}
		
		Test018 test = new Test018();
		
//		遍历学生数组
		test.print(stus);
		test.searchState(stus,3);
		test.sort(stus);
	}
	
//	遍历学生数组  改进------------------------------------
	public void print(StudentTest[] stus) {
		for(int i = 0; i < stus.length; i++) {
			System.out.println(stus[i].info());
		}
	}
	
//	问题1：打印出3年级学生的信息  改进-----------------------
	/**
	 * @Description 查找Student数组中指定年级的学生信息
	 * @param stus 要查找的数组
	 * @param state 要查找的年级
	 */
	public void searchState(StudentTest[] stus, int state) {
		System.out.println(state +"年级学生的信息如下：");
		for(int i = 0; i < stus.length; i++) {
			if(stus[i].state == state) {
				System.out.println(stus[i].info());
			}
		}
	}
	
//	问题2：使用冒泡排序按学生成绩排序，并遍历学生信息 改进----------
	/**
	 * Description 给StudentTest数组冒泡排序
	 * @param stus 要排序的数组
	 */
	public void sort(StudentTest[] stus) {
		System.out.println("排序：");
		for(int i = 0; i < stus.length - 1; i++) {
			for(int j = 0; j < stus.length - 1 - i; j++) {
				if(stus[j].score < stus[j + 1].score) {
					StudentTest temp = stus[j];
					stus[j] = stus[j + 1];
					stus[j + 1] = temp;
				}
			}
		}
		
		for(int i = 0; i < stus.length; i++) {
			System.out.println(stus[i].info());
		}
	}
}

class StudentTest{
	int number;//学号
	int state;//年级
	int score;//成绩
	
	public String info() {
		return "学号：\t" + number + "\t 年级：\t" + state + "\t 成绩：\t" + score;
	}
}
```
#### 5.声明一个日期类型MyDate：有属性：年year,月month，日day。创建2个日期对象，分别赋值为：你的出生日期，你对象的出生日期，并显示信息。
```
public class Test019 {
	public static void main(String args[]) {
		MyDate mine = new MyDate();
		mine.year = 2003;
		mine.month = 3;
		mine.day = 8;
		
		MyDate lover = new MyDate();
		lover.year = 2002;
		lover.month = 2;
		lover.day = 27;	
		
		mine.showDate();
		lover.showDate();
	}
}

class MyDate{
	int year;
	int month;
	int day;
	
	public void showDate() {
		System.out.println("出生日期 : " + year + "年" + month + "月" + day + "日");
	}
}
```

## 自定义数组的工具类
### 工具类
```
package StudyTest;

public class Test021 {
	public static void main(String args[]) {
		
	}
	
//	求数组的最大值
	public int getMax(int[] arr) {
		int maxValue = arr[0];
		
		for(int i = 0; i < arr.length; i++) {
			if(maxValue < arr[i]) {
				maxValue = arr[i];
			}
		}
		
		return maxValue;
	}
	
//	求数组的最小值
	public int getMin(int[] arr) {
		int minValue = arr[0];
		
		for(int i = 0; i < arr.length; i++) {
			if(minValue > arr[i]) {
				minValue = arr[i];
			}
		}
		
	    return minValue;
	}
	
//	求数组的总和
	public int getSum(int[] arr) {
		int sumValue = 0;
		
		for(int i = 0; i < arr.length; i++) {
			sumValue += arr[i];
		}
		
		return sumValue;
	}
	
//	求数组的平均值
	public int getAvg(int[] arr) {
		
		return getSum(arr) / arr.length;
		
//		int sumValue = 0;
//		
//		for(int i = 0; i < arr.length; i++) {
//			sumValue += arr[i];
//		}
//		
//		int avgValue = sumValue / arr.length;
//		
//		return avgValue;
	}
	
//	反转数组
	public void reverse(int[] arr) {
		for(int i = 0; i < arr.length / 2; i++) {
			int temp = arr[i];
			arr[i] = arr[arr.length - i - 1];
			arr[arr.length - i - 1] = temp; 
		}
	}
	
//	复制数组
	public int[] copy(int[] arr) {
		int[] arrCopy = new int[arr.length];
		
		for(int i = 0; i < arr.length; i++) {
			arrCopy[i] = arr[i];
		}
		
		return arrCopy;
	}
	
//	数组排序
	public void sort(int[] arr) {
		for(int i = 0; i < arr.length; i++) {
			
			for(int j = 0; j < arr.length - 1; j++) {
			
				if(arr[j] > arr[j + 1]) {
					int temp = arr[j];
					arr[j] = arr[j + 1];
					arr[j + 1] = temp;
				}
			}
		}
	}
	
//	遍历数组
	public void print(int[] arr) {
		for(int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + "\t");
		}
		
		System.out.println();
	}
	
//	查找指定元素
	public int getIndex(int[] arr,int dest) {
		
		for(int i = 0; i < arr.length; i ++) {
		
			if(arr[i] == dest) {
				return i;
			}
		}
		
		return -1; //-1表示未找到
	}
}
```
### 使用类
```
package StudyTest;

public class Test022 {
	public static void main(String args[]) {
		Test021 util = new Test021();
		int[] arr = new int[] {32,34,32,5,3,54,645};
		int max = util.getMax(arr);
		System.out.println("最大值为：" + max);
		
		System.out.println("排序前");
		util.print(arr);
		
		System.out.println("排序后");
		util.sort(arr);
		util.print(arr);
		
		System.out.println("查找");
		int index = util.getIndex(arr, 5);
		if(index >= 0) {
			System.out.println("找到了：" + index);
		}
		else {
			System.out.println("未找到");
		}
	}
}
```

## 方法
### 方法的重载
- 重载的概念
   - 在同一个类中，允许存在一个以上的同名方法，只要它们的参数个数或者参数类型不同即可。
- 重载的特点：
   - 与返回值类型无关，只看参数列表，且参数列表必须不同。(参数个数或参数类型)。
   - 调用时，根据方法参数列表的不同来区别。
- 重载示例：

```
//返回两个整数的和
int add(int x,int y){
    return x+y;
}

//返回三个整数的和
int add(int x,int y,int z){
    return x+y+z;
}

//返回两个小数的和
double add(double x,double y){
    return x+y;
}
```
```
public class Test023 {
	public static void main(String args[]) {
		Test023 test = new Test023();
		test.getSum(2,3);
	}
	
	public void getSum(int i, int j) {
		System.out.println(i + j + "  1 ");
	}
	
//  重载与返回类型/形参变量名/方法体无关
//	public int getSum(int m, int n) {
//		System.out.println(i + j);
//	}
	
	public void getSum(double i, double j) {
		System.out.println(i + j + "  2 ");
	}
	
	public void getSum(String i, int j) {
		System.out.println(i + j + "  3 ");
	}
	
	public void getSum(int i, String j) {
		System.out.println(i + j + "  4 ");
	}
}
```
#### 练习
##### 1.
```
编写程序，定义三个重载方法并调用。方法名为mOL。
三个方法分别接收一个int参数、两个int参数、一个字符串参数。
分别执行平方运算并输出结果，相乘并输出结果，输出字符串信息。
在主类的main ()方法中分别用参数区别调用三个方法。
```
```
public class Test024 {
	public static void main(String args[]) {
		Test024 test = new Test024();
		
		test.mOL(2);
		test.mOL(2,3);
		test.mOL("test exam");
	}
	
	public void mOL(int i) {
		System.out.println(i * i);
	}
	
	public void mOL(int i, int j) {
		System.out.println( i * j);
	}
	
	public void mOL(String i) {
		System.out.println(i);
	}
}
```
##### 2.
```
3.定义三个重载方法max()，
第一个方法求两个int值中的最大值，
第二个方法求两个double值中的最大值，
第三个方法求三个double值中的最大值，
并分别调用三个方法。
```
```
public class Test025 {
	public static void main(String args[]) {
		Test025 test = new Test025();
		
		test.max(2, 3);
		test.max(2.34, 3.456);
		test.max(3.12, 5.343, 3.123);
	}
	
	public void max(int i, int j) {
		System.out.println((i > j) ? i : j);
	}
	
	public void max(double i, double j) {
		System.out.println((i > j) ? i : j);
	}
	
	public void max(double i, double j, double k) {
		double maxValue = (i > j) ? i : j;
		maxValue = (maxValue > k) ? maxValue : k;
		System.out.println(maxValue);
	}
}
```

#### 方法的重载与重写的区别


### 可变形参的方法

- JavaSE 5.0 中提供了Varargs(variable number of arguments)机制，允许直接定义能和多个实参相匹配的形参。从而，可以用一种更简单的方式，来传递个数可变的实参。
    - //JDK 5.0以前：采用数组形参来定义方法，传入多个同一类型变量`public static void test(int a ,String[] books);`
    - //JDK5.0：采用可变个数形参来定义方法，传入多个同一类型变量`public static void test(int a ,String…books);`
- 说明：
1. 声明格式：方法名(参数的类型名 ...参数名)
2. 可变参数：方法参数部分指定类型的参数个数是可变多个：0个，1个或多个
3. 可变个数形参的方法与本类中同名的方法之间，彼此构成重载
4. 可变参数方法的使用与方法参数部分使用数组是一致的 不能构成重载，即不能共存
5. 方法的参数部分有可变形参，需要放在形参声明的最后
6. 在一个方法的形参位置，最多只能声明一个可变个数形参

```
public class Test026 {
	public static void main(String args[]) {
		Test026 test = new Test026();
		test.show(12);
		test.show();
		test.show("Hello");
		test.show("Hello","world");
	}
	
	public void show(int i) {
		
	}
	
	public void show(String i) {
		System.out.println(i + " show(String i)");
	}
	
//	可变形参
	public void show(String ... strs) {
//		输出的strs为地址值
		System.out.println(strs + " show(String .. strs)");
		
//		可变形参的输出和数组类似
		for(int i = 0; i < strs.length; i++) {
			System.out.println(strs[i]);
		}
	}	
	
//	编译器认为数组和可变形参的定义相同，不能构成重载
//	public void show(String[] strs) {
//		
//	}
	
//  可变形参必须放在形参位置的末尾
//	public void test(int ... i; i) {
//		
//	}
}
```
### 方法参数的值传递机制
- 关于变量的赋值
   - 基本数据类型 （赋值的是数据值）
   - 引用数据类型（赋值的是地址值）String也是
- 方法，必须由其所在类或对象调用才有意义。若方法含有参数：
   - 形参：方法声明时的参数
   - 实参：方法调用时实际传给形参的参数值
- Java的实参值如何传入方法呢？
- Java里方法的参数传递方式只有一种：值传递。 即将实际参数值的副本（复制品）传入方法内，而参数本身不受影响。
   - 形参是基本数据类型：将实参基本数据类型变量的“数据值”传递给形参
   - 形参是引用数据类型：将实参引用数据类型变量的“地址值”传递给形参
       - 对象作为参数时，参数的值是对该对象的引用，这时对象的内容可以在方法中改变，但是对象的引用不会改变。 
   
- String也是引用数据类型
    -  即底层是用char[]存储
    - 特征：字符串常量值；不可变

```
public class Test027 {
	public static void main(String args[]) {
	
		int m = 10;
		int n = 20;
		System.out.println("m=" + m + "," + "n=" + n);
//		交换两个变量的值的操作
//		int temp = m;
//		m = n;
//		n = temp;
//		System.out.println("m=" + m + "," + "n=" + n);

//		实参的值未发生交换
//		仅形参发生交换，未改变实参
//		形参是基本数据类型：将实参基本数据类型变量的“数据值”传递给形参
		Test027 test = new Test027();
		test.swap(m,n);
		System.out.println("m=" + m + "," + "n=" + n);
	}
	
	public void swap(int i, int j) {
		int temp = i;
		i = j;
		j= temp;
	}
}
```

```
public class Test029 {
	public static void main(String args[]) {
		Data data = new Data();
		
		data.m = 10;
		data.n = 20;
		
		System.out.println("m=" + data.m + " , n=" + data.n);
		
//		交换m和n的值
//		int temp = data.m;
//		data.m = data.n;
//		data.n = temp;
		
		Test029 test = new Test029();
		test.swap(data);
		System.out.println("m=" + data.m + " , n=" + data.n);
	}
	
//	形参是引用数据类型：将实参引用数据类型变量的“地址值”传递给形参
	public void swap(Data data) {
		int temp = data.m;
		data.m = data.n;
		data.n = temp;
	}
}

class Data{
	int m;
	int n;
}
```
```
public class Test028 {
	public static void main(String args[]) {
		int[] arr = new int[] {1,2,3,4,2,3};
		
		for(int i = 0; i < arr.length; i++) {
			System.out.print(arr[i]);
		}
		System.out.println();
		
		Test028 test = new Test028();
//		错误.实际上并未改变实参，arr[i]传递的仍然只是数据值
		test.swap(arr[1],arr[0]);
		
		for(int i = 0; i < arr.length; i++) {
			System.out.print(arr[i]);
		}
		System.out.println();
		
//		正确的，交换数组中指定俩个位置元素的值
		test.swap(arr, 1,0);
		
		for(int i = 0; i < arr.length; i++) {
			System.out.print(arr[i]);
		}
		System.out.println();
	}

//	错误.实际上并未改变实参，arr[i]传递的仍然只是数据值	
	public void swap(int i , int j) {
		int temp = i;
		i = j;
		j = temp;
	}
	
//	正确的，交换数组中指定俩个位置元素的值
	public void swap(int[] arr, int i,int j) {
		int temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
	}
}
```

#### 练习1 地址值改变的输出值

##### 1.

```
public class Test30 {
	public static void main(String args[]) {
		Test30 test = new Test30();
		test.first();
	}
	
	public void first() {
		int i = 5;
		
		Value v = new Value(); //设为0x5566
		v.i = 25;
		
		second(v, i);  //15 0
		//second()方法结束
		//0x8899地址值被丢弃
		//v 指向地址值0x5566 v.i ==20
		
		System.out.println(v.i);  //20
	}
	
	public void second(Value v, int i) {
		i = 0;  //实参i==5 形参i==0
		v.i = 20; //地址值0x5566的实参v.i==20
		
		Value val = new Value();
		v = val; 
		//此时v的地址值0x5566变为val的地址值0x8899
		//v.i 指向 val.i == 15
		
		System.out.println(v.i + " " + i);
		// v.i == 15 ; 形参i == 0 ;但是实参i仍然为i == 5 
	}
}
class Value {
	int i = 15;
}

输出
15 0
20
```

##### String也是引用数据类型，但是较特殊,不可变

```
public class Test043 {
	public static void main(String args[]) {
		String s1 = "hello";
		
		Test043 test = new Test043();
		
		test.change(s1);
		
		System.out.println(s1); //hello
	}
	
	public void change(String s) {
		s = "hi";    
		//虽然传入s1地址值，但s新建指向hi，s1仍然指向hello
	}
}
```

#### 练习2 陷阱题

```
public class Test031 {
	public static void main(String args[]) {
		int a = 10;
		int b = 10;
		method(a,b);
//		需要在method方法被调用之后，仅打印出a=100,b=200,请携程method方法的代码
		System.out.println("a=" + a);
		System.out.println("b=" + b);
	}
	
//	代码编写处
}
```

1. System.exit(status)

```
System.exit(status)是用来结束当前正在运行中的java虚拟机。

System.exit(0) 是正常退出程序，
System.exit(1) 或者说非0表示非正常退出程序
System.exit(status)不管status为何值都会退出程序。

和return 相比有以下不同点：return是回到上一层，而System.exit(status)是回到最上层
```
```
public class Test031 {
	public static void main(String args[]) {
		int a = 10;
		int b = 10;
		method(a,b);
//		需要在method方法被调用之后，仅打印出a=100,b=200,请携程method方法的代码
		System.out.println("a=" + a);
		System.out.println("b=" + b);
	}
	
//	代码编写处
	public static void method(int a, int b) {
		a = a * 10;
		b = b * 20;
		System.out.println("a=" + a);
		System.out.println("b=" + b);
		System.exit(0); //终止JVM的执行，程序退出
		//使之后的语句无法执行
	}
}
```
2.PrintStream 打印流 ???
```
public class Test031 {
	public static void main(String args[]) {
		int a = 10;
		int b = 10;
		method(a,b);
//		需要在method方法被调用之后，仅打印出a=100,b=200,请携程method方法的代码
		System.out.println("a=" + a);
		System.out.println("b=" + b);
	}
	

	public static void method(int a,int b) {
		PrintStream ps = new PrintStream(System.out) {
			@Override
			public void println(String x) {
				
				if("a=10".equals(x)) {
					x = "a=100";
				}
				else if("b=10".equals(x)) {
					x = "b=200";
				}
				super.println(x);
			}
		};
		
		System.setOut(ps);
	}
}
```
#### 练习3 陷阱题
```
定义一个int型的数组：int[] arr = new int[]{12,3,3,34,56,77,432};
让数组的每个位置上的值去除以首位置的元素，得到的结果，作为该位置上的新值。
遍历新的数组。
```
```
public class Test032 {
	public static void main(String args[]) {
		int[] arr = new int[]{12,3,3,34,56,77,432};
		
		Test032 test = new Test032();
		
		test.traverse(arr);
		
		test.reAddition(arr);
		test.traverse(arr);
	}
//	让数组的每个位置上的值去除以首位置的元素，得到的结果，作为该位置上的新值。
//	遍历新的数组。
	public void reAddition(int[] arr) {
		int head = arr[0];
		
		for(int i = 0; i <arr.length; i++) {
			arr[i] = arr[i] / head;
		}
	}
	
	public void traverse(int[] arr) {
		for(int i = 0; i < arr.length; i++) {
			System.out.print(arr[i] + "\t");
		}
		
		System.out.println();
	}
}
```

#### 练习4 陷阱题

```
public class Test035 {
	public static void main(String args[]) {
		int[] arr = new int[] {1,2,3};
		System.out.println(arr); //地址值
//		void java.io.PrintStream.println(Object x)
		
		char[] arr1 = new char[] {'a','b','c'};
		System.out.println(arr1); //abc
//		void java.io.PrintStream.println(char[] x)
	}
}
```

#### 练习5：将对象作为参数传递给方法
（1）定义一个Circle类，包含一个double型的radius属性代表圆的半径，一个findArea()方法返回圆的面积。
```
import java.util.Scanner;

public class Test033 {
	public static void main(String args[]) {
		Scanner scan = new Scanner(System.in);
			
		CircleTest c1 = new CircleTest();
		
		c1.radius = scan.nextDouble();
		System.out.println(c1.findArea());
		
	}
}

class CircleTest{
	double radius;
	
	public double findArea() {
		return radius * radius * Math.PI;
	}
}
```
（2）定义一个类PassObject，在类中定义一个方法printAreas()，该方法的定义如下：
public void printAreas(Circle c, int time)
在printAreas方法中打印输出1到time之间的每个整数半径值，以及对应的面积。
例如，times为5，则输出半径1，2，3，4，5，以及对应的圆面积。
```
public class Test034 {
	public static void main(String args[]) {
		PassObject p1 = new PassObject();
		Circle c1 = new Circle();
		p1.printAreas(c1, 5);
	}
}

class PassObject{
	public void printAreas(Circle c, int time) {
		for(int i = 1; i <= time; i++) {
			System.out.println("r=" + i + "  Area=" + c.findArea(i));
		}
	}
}
```
（3）在main方法中调用printAreas()方法，调用完毕后输出当前半径值。程序运行结果如图所示
![](C:/NoteBook/pictures/129730620220958.png =349x)
```
public class Test034 {
	public static void main(String args[]) {
		PassObject p1 = new PassObject();
		Circle c1 = new Circle();
		p1.printAreas(c1, 5);
	}
}

class PassObject{
	public void printAreas(Circle c, int time) {
		System.out.println("radius \t Area");
		
		for(double i = 1; i <= time; i++) {
			System.out.println( i + "\t" + c.findArea(i));
		}
	}
}
```
### 递归方法 
- 递归方法：一个方法体内调用它自身。
- 方法递归包含了一种隐式的循环，它会重复执行某段代码，但这种重复执行无须循环控制。
- 递归一定要向已知方向递归，否则这种递归就变成了无穷递归，类似于死循环。
#### 计算1-100之间所有自然数的和
```
public class Test036 {
	public static void main(String args[]) {
		Test036 test = new Test036();
		
		int sum = test.getSum(100);
		
		System.out.println(sum);
	}

	public int getSum(int num) {
		if(num == 1) {
			return 1;
		}
		else {
			return num + getSum(num - 1);
		}
	}
}
```
```
public class Test037 {
	public static void main(String args[]) {
		Test037 test = new Test037();
		
		System.out.println(test.sum(1));
	}
	
	public static int sum(int num) {
		if(num <= 100 && num >0) {
			return num + sum(num + 1);
		}
		else {
			return 0;
		}
	}
}
```
#### 练习7.1：请用Java写出递归求阶乘(n!)的算法

```
public class Test038 {
	public static void main(String args[]) {
		Test038 test = new Test038();
		
		System.out.println(test.factorial(10));
	}
	
	public int factorial(int num) {
		if(num == 1) {
			return 1;
		}
		else {
			return num * factorial(num - 1);
		}
	}
}
```
#### 练习7.2：
```
已知有一个数列：f(0) = 1,f(1) = 4,f(n+2)=2*f(n+1) + f(n),
其中n是大于0的整数，求f(10)的值。
```

```
//f(n) = 2 * f(n -1) + f(n -2)

public class Test039 {
	public static void main(String args[]) {
		Test039 test = new Test039();
		System.out.println(test.arrCount(3));
	}
	
	public int arrCount(int num) {
		if(num == 0) {
			return 1;
		}
		else if(num == 1) {
			return 2 * arrCount(num - 1) + 2;
		}
		else {
			return 2 * arrCount(num - 1) + arrCount(num - 2);
		}
	}
}
```
#### 练习7.3：
```
已知一个数列：f(20) = 1,f(21) = 4,f(n+2) = 2*f(n+1)+f(n),
其中n是大于0的整数，求f(10)的值。
```
```

```

#### 练习7.4：斐波那契数列
```
输入一个数据n，计算斐波那契数列(Fibonacci)的第n个值
1 1 2 3 5 8 13 21 34 55
规律：一个数等于前两个数之和
要求：计算斐波那契数列(Fibonacci)的第n个值，并将整个数列打印出来
```
```
public class Test041 {
	public static void main(String args[]) {
		Test041 test = new Test041();
	
		int n = 10;
		System.out.println(test.fibonacci(n));
		
		for(int i = 1; i <= n; i++) {
			System.out.print(test.fibonacci(i) + "\t");
		}
		System.out.println();
		
	}
	
	public int fibonacci(int n) {
		if(n == 1) {
			return 1;
		}
		else if(n == 2){
			return fibonacci(n - 1) + 0;
		}
		else {
			return fibonacci(n - 1) + fibonacci(n - 2);
		}
	}
}
```

## 面向对象特征之一：封装和隐藏

- 高内聚 ：类的内部数据操作细节自己完成，不允许外部干涉；
- 低耦合 ：仅对外暴露少量的方法用于使用。
- 
- 隐藏对象内部的复杂性，只对外公开简单的接口。便于外界调用，从而提高系统的可扩展性、可维护性。通俗的说，把该隐藏的隐藏起来，该暴露的暴露出来。这就是封装性的设计思想。

- 封装性的体现:
   - Java中通过将数据声明为私有的(private)，
   - 再提供公共的（public）方法:getXxx()和setXxx()实现对该属性的操作，
   - 以实现下述目的：
   1. 隐藏一个类中不需要对外提供的实现细节；
   2. 使用者只能通过事先定制好的方法来访问数据，可以方便地加入控制逻辑，限制对属性的不合理操作；
   3. 便于修改，增强代码的可维护性；


**封装性的体现：**  
1. 如上
2. 不对外暴露的方法
3. 单例模式(将构造器私有化)
4. 如果不希望类在包外被调用，可以将类设置为缺省的


### 封装性的体现需要访问权限修饰符  ==封装性
- Java权限修饰符public、protected、(缺省)、private置于类的成员定义前，用来限定对象对该类成员的访问权限。
- 可以用来修饰类及类的内部结构：属性，方法，构造器，内部类
- 具体：
    - 4种权限都可以用来修饰类的内部结构；属性，方法，构造器，内部类
    - 修饰类的话，只能使用：缺省，public
 

![](C:/NoteBook/pictures/228253722220960.png =805x)

- 对于class的权限修饰只可以用public和default(缺省)。
   - public类可以在任意地方被访问。
   - default类只可以被同一个包内部的类访问。

![](C:/NoteBook/pictures/56643822239386.png =411x)

### 例1
```
public class Test001 {
	public static void main(String args[]) {
		
		Animal a = new Animal();
//		a.name = "大黄";  //private
//		a.age = 1;  //private
//		a.legs = 1; //private 
		//The field Animal.legs is not visible
		
		a.setName("大黄");
		a.setAge(10);
		a.setLegs(4);
		
		a.show();
	}
}

class Animal{
	
	String name;
	private int age;
	private int legs;
	
//	对legs属性的设置
	public void setLegs(int leg) {
		if(leg >= 0 && leg % 2 == 0) {
			legs = leg;
		}
		else {
			legs = -1;
		}
	}
	
//	对legs属性的获取
	public int getLegs() {
		return legs;
	}
	
//	对age属性的设置
	public void setAge(int a) {
		if(a >= 0 && a <= 100) {
			age = a;
		}
		else {
			age = -1;
		}
	}
	
//	对age属性的获取
	public int getAge() {
		return age;
	}
	
//	对name属性的设置
	public void setName(String str) {
		name = str;
	}
	
//	对name属性的获取
	public String getName() {
		return name;
	}
	
	public void eat() {
		System.out.println("动物进食");
	}
	
	public void show() {
		System.out.println("name = " + name + "\t age = " + age + "\t legs = " + legs);
	}
}
```

### 例2
- 同一类中

```
package StudyTest02;

public class Test002 {
	
	private int orderPrivate;
	int orderDefault;
	public int orderPublic;
	
	private void methodPrivate() {
		orderPrivate = 1;
		orderDefault = 2;
		orderPublic  = 3;
	}
	
	void methodDefault() {
		orderPrivate = 1;
		orderDefault = 2;
		orderPublic  = 3;
	}
	
	public void methodPublic() {
		orderPrivate = 1;
		orderDefault = 2;
		orderPublic  = 3;
	}
}
```

- 同一个包中

```
package StudyTest02;

public class Test003 {
	public static void main(String args[]) {
		
		Test002 order = new Test002();
		
		order.orderDefault = 1;
		order.orderPublic = 2;
//		在Order类之外，私有的结构就不可调用了
//		order.orderPrivate = 3;
//		he field Test002.orderPrivate is not visible
		
		order.methodDefault();
		order.methodPublic();
//		order.methodPrivate();
//		The method methodPrivate() from the type Test002 is not visible

	}
}
```

- 同一个项目中

```
package StudyTest;

import StudyTest02.Test002;

public class Test045 {
	public static void main(String args[]) {
		
		Test002 order = new Test002();
		
//      在order包之外，缺省的结构就不可调用了
//		order.orderDefault = 1;
//		The field Test002.orderDefault is not visible
		order.orderPublic = 2;
//		在Order类之外，私有的结构就不可调用了
//		order.orderPrivate = 3;
//		he field Test002.orderPrivate is not visible
		
//		order.methodDefault();
//		The method methodDefault() from the type Test002 is not visible
		order.methodPublic();
//		order.methodPrivate();
//		The method methodPrivate() from the type Test002 is not visible
	}
}
```

### 1.创建程序,在其中定义两个类：Person和PersonTest类。
```
定义如下：
用setAge()设置人的合法年龄(0~130)，用getAge()返回人的年龄。
在 PersonTest 类 中实例化 Person 类的对象 b ， 
调 用 setAge() 和getAge()方法，体会Java的封装性。
```
![](C:/NoteBook/pictures/172641809220961.png =252x)
```
public class Test004 {
	public static void main(String args[]) {
		Person p1 = new Person();
		PersonTest t1 = new PersonTest();
		
		t1.setAge(p1, 1110);
		System.out.println(t1.getAge(p1));
	}
}

class Person{
	protected int age;
}

class PersonTest{
	public void setAge(Person b, int a) {
		if(a >= 0 && a <= 130) {
			b.age = a;
		}
		else {
			b.age = -1;
			throw new RuntimeException("输入的数据有误");
//			System.out.println("输入年龄有误");
		}
	}
	
	public int getAge(Person b) { 
		return b.age;
	}
}
```

## 构造器(或构造方法)

### 概要

- 构造器不是类的方法

- 构造器的特征
   - 它具有与类相同的名称
   - 它不声明返回值类型。（与声明为void不同）
   - 不能被static、final、synchronized、abstract、native修饰，
   - 不能有return语句返回值
- 构造器的作用：创建对象；给对象进行初始化
   - 如：
   - Order o = new Order(); 
   - Person p = new Person(“Peter”,15);
- 如果没有显式的定义类的构造器的话，则系统默认提供一个空参的构造器
- 语法格式：
```
修饰符 类名 (参数列表) {
    初始化语句；
} 
```

- 创建Animal类的实例：Animal a = new Animal();调用构造器，将legs初始化为4。

![](C:/NoteBook/pictures/211974122247419.png =358x)

- 根据参数不同，构造器可以分为如下两类：
   - 隐式无参构造器（系统默认提供）
   - 显式定义一个或多个构造器（无参、有参）
- 注 意：
   - Java语言中，每个类都至少有一个构造器
   - 默认构造器的修饰符与所属类的修饰符一致
   - 一旦显式定义了构造器，则系统不再提供默认构造器（空参）; 注意此时空参构造器需要自己构造
   - 一个类可以创建多个重载的构造器
   - 父类的构造器不可被子类继承
   
#### 例1

```
public class Test001 {
	public static void main(String args[]) {
//		创建类的对象： new + 构造器Person()
		Person p1 = new Person();
		p1.eat();
		
		Person p2 = new Person("大户",12);
		p2.show();
	}
}

class Person{
	String name;
	int age;
	
	public Person() {
		System.out.println("Person....");
	}
	
	public Person(String n,int a) {
		name = n;
		age = a;
	}
	
	public void eat() {
		System.out.println("人吃饭");
	}
	
	public void study() {
		System.out.println("人学习");
	}
	
	public void show() {
		System.out.println("name=" + name + ", age=" + age);
	}
}
```

#### 练习
- 练习1. 在前面定义的Person类中添加构造器，利用构造器设置所有人的age属性初始值都为18。
- 练习2. 修改上题中类和构造器，增加name属性,使得每次创建Person对象的同时初始化对象的age属性值和name属性值。

```
package javatest001;

public class Test001 {
	public static void main(String args[]) {
//		创建类的对象： new + 构造器Person()
		Person p1 = new Person();
		p1.eat();
		p1.show();
		Person p2 = new Person("大户",12);
		p2.show();
	}
}

class Person{
	String name;
	int age;
	
	public Person() {
		System.out.println("Person....");
		age = 18;
		name = "null";
	}
	
	public Person(String n,int a) {
		name = n;
		age = a;
	}
	
	public void eat() {
		System.out.println("人吃饭");
	}
	
	public void study() {
		System.out.println("人学习");
	}
	
	public void show() {
		System.out.println("name=" + name + ", age=" + age);
	}
}
```

#### 练习3.编写两个类，TriAngle和TriAngleTest，

```
其中TriAngle类中声明私有的底边长base和高height，同时声明公共方法访问私有变量。
此外，提供类必要的构造器。另一个类中使用这些公共方法，计算三角形的面积。
```

```
public class Test002 {
	public static void main(String args[]) {
		TriAngle t = new TriAngle(3,4);
		t.getTriAngle();
		System.out.println(t.getArea(t));
	}
}

class TriAngle{
	private int base;
	private int height;
	
	public TriAngle() {
		System.out.println("null");
	}
	
	public TriAngle(int b, int h) {
		base = b;
		height = h;
	}
	
	public void getTriAngle() {
		System.out.println("base = " + base + ", height = " + height);
	}
	
	public int getArea(TriAngle t) {
		return t.base * t.height / 2;
	}
}
```

### 构造器重载

- 构造器一般用来创建对象的同时初始化对象。如

```
class Person{
    String name;
    int age;
    
    public Person(String n , int a){ 
        name=n; age=a;
    }
}
```
- 构造器重载使得对象的创建更加灵活，方便创建各种不同的对象。
- 构造器重载举例：

```
public class Person{
    public Person(String name, int age, Date d) {
        this(name,age);
        …
    }
    public Person(String name, int age) {…}
    public Person(String name, Date d) {…}
    public Person(){…}
}
```

- 构造器重载，参数列表必须不同

#### 构造器重载举例

```
public class Person { 
    private String name;
    private int age;
    private Date birthDate;

    public Person(String n, int a, Date d) {
        name = n;
        age = a;
        birthDate = d;
    }
    
    public Person(String n, int a) {
        name = n;
        age = a;
    }

    public Person(String n, Date d) {
        name = n;
        birthDate = d;
    }

    public Person(String n) {
        name = n;
        age = 30;
    }
}
```

#### 练习
```
(1)定义Student类,有4个属性：
String name; 
int age; 
String school; 
String major;
(2)定义Student类的3个构造器:
第一个构造器Student(String n, int a)设置类的name和age属性；
第二个构造器Student(String n, int a, String s)设置类的name, age 和school属性；
第三个构造器Student(String n, int a, String s, String m)设置类的name, age ,school和major属性；
(3)在main方法中分别调用不同的构造器创建的对象，并输出其属性值。
```

```
public class Test003 {
	public static void main(String args[]) {
		Student s1 = new Student("1号",12);
		Student s2 = new Student("2号",14,"2号学府");
		Student s3 = new Student("3号",15,"3号学府","3号职务");
		
		s1.show();
		s2.show();
		s3.show();
	}
}

class Student{
	private String name;
	private int age;
	private String school;
	private String major;
	
	public Student(String n, int a) {
		name = n;
		age = a;
	}
	
	public Student(String n, int a, String s) {
		name = n;
		age = a;
		school = s;
	}
	
	public Student(String n, int a, String s, String m) {
		name = n;
		age = a;
		school = s;
		major = m;
	}
	
	public void show() {
		System.out.println("name = " + name + " , age = " + age + " , school = " + school + " , major = " + major);
	}
}
```

## 属性赋值过程

- 赋值的位置：
   - ① 默认初始化
   - ② 显式初始化
   - ③ 构造器中初始化
   - ④ 通过“对象.属性“或“对象.方法”的方式赋值

- 赋值的先后顺序：
   - ① - ② - ③ - ④
   
## JavaBean
- JavaBean是一种Java语言写成的可重用组件。
- 所谓javaBean，是指符合如下标准的Java类：
   - 类是公共的
   - 有一个无参的公共的构造器
   - 有属性，且有对应的get、set方法
- 用户可以使用JavaBean将功能、处理、值、数据库访问和其他任何可以用Java代码创造的对象进行打包，并且其他的开发者可以通过内部的JSP页面、Servlet、其他JavaBean、applet程序或者应用来使用这些对象。用户可以认为JavaBean提供了一种随时随地的复制和粘贴的功能，而不用关心任何改变。

```
public class JavaBean {
	private String name; // 属性一般定义为private
	private int age;
	
	public JavaBean() {
	}
	
	public int getAge() {
		return age;
	}

	public void setAge(int a) {
		age = a;
	}

	public String getName() {
		return name;
	}

	public void setName(String n) {
		name = n;
	}
}
```

```
public class Customer {
	private int id;
	private String name;
	
	public Customer() {
		
	}
	
	public void setId(int i) {
		id = i;
	}
	
	public void setName(String n) {
		name = n;
	}
	
	public String getName() {
		return name;
	}
}
```

## UML类图

![](C:/NoteBook/pictures/332602419227254.png =746x)

## this 关键字

- 它在方法内部使用，即这个方法所属对象的引用；
- 它在构造器内部使用，表示该构造器正在初始化的对象。
- this 可以调用类的属性、方法和构造器
- 什么时候使用this关键字呢？
   - 当在方法内需要用到调用该方法的对象时，就用this。(当需要在方法中使用对象时)
   - 具体的：我们可以用this来区分属性和局部变量。(类的属性与方法的形参同名时)
   - 比如：this.name = name;
   
```
public class PersonTest {
	public static void main(String args[]) {
		Person p1 = new Person();
		
		p1.setAge(1);
		System.out.println(p1.getAge());
		p1.eat();
	}
}

class Person{
	private String name;
	private int age;
	
	public void setName(String name) {
		this.name = name;
	}
	
	public String getName() {
		return name; //自动加上this.
//		return this.name;
	}
	
	public void setAge(int age) {
		this.age = age;
	}
	
	public int getAge() {
		return age;
	}
	
	public void eat() {
		System.out.println("吃饭");
		study(); //自动加上this.
//		this.study();
	}
	
	public void study() {
		System.out.println("study");
	}
}
```

1. 在任意方法或构造器内，如果使用当前类的成员变量或成员方法可以在其前面添加this，增强程序的阅读性。不过，通常我们都习惯省略this。
2. 当形参与成员变量同名时，如果在方法内或构造器内需要使用成员变量，必须添加this来表明该变量是类的成员变量
3. 使用this访问属性和方法时，如果在本类中未找到，会从父类中查找
4. this可以作为一个类中构造器相互调用的特殊格式

- 注意：
   - 可以在类的构造器中使用"this(形参列表)"的方式，调用本类中重载的其他的构造器！
- 明确：
   - 构造器中不能通过"this(形参列表)"的方式调用自身构造器
   - 如果一个类中声明了n个构造器，则最多有 n - 1个构造器中使用了"this(形参列表)"
   - "this(形参列表)"必须声明在类的构造器的首行！
   - 在类的一个构造器中，最多只能声明一个"this(形参列表)"
   
```
public class PersonTest {
	public static void main(String args[]) {
		Person p1 = new Person();
		
		p1.setAge(1);
		System.out.println(p1.getAge());
		p1.eat();
		
		Person p2 = new Person("Jerry",20);
	}
}

class Person{
	private String name;
	private int age;
	
	public Person() {
//      this() //构造器中不能通过"this(形参列表)"的方式调用自身构造器		

//      this(age) //与Person(int age)[有this()]形成一个死循环
//Cannot refer to an instance field age while explicitly invoking a constructor
        eat();
		
		String info = "hello----------";
		System.out.println(info);
	}
	
	public Person(String name) {
		this();  //调用Person()
		this.name = name;
	}
	
	public Person(int age) {
		this();
		this.age = age;
	}
	
	public Person(String name, int age) {
//	    this()  //在类的一个构造器中，最多只能声明一个"this(形参列表)"
		this(age);
		this.name = name;
		this.age = age;
	}
	
	public void setName(String name) {
		this.name = name;
	}
	
	public String getName() {
		return name;
	}
	
	public void setAge(int age) {
		this.age = age;
	}
	
	public int getAge() {
		return age;
	}
}
```
### 练习 添加必要的构造器，综合应用构造器的重载，this关键字

![](C:/NoteBook/pictures/139281721239387.png =558x)

```
package javatest003;

public class Boy {
	private String name;
	private int age;
		
	public Boy() {
		
	}
	
	public Boy(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}
	
	public void marry(Girl girl) {
		System.out.println("娶" + girl.getName());
	}
	public void shout() {
		if(age >= 22) {
			System.out.println("登记结婚");
		}
		else {
			System.out.println("不可");
		}
	}
}
```

```
package javatest003;

public class Girl {
	private String name;
	private int age;
	
	public Girl() {
		
	}
	
	public Girl(String name, int age) {
		this.name = name;
		this.age = age;
	}
	
	public String getName() {
		return name;
	}
	
	public void setName(String name) {
		this.name = name;
	}
	
	public void marry(Boy boy) {
		System.out.println("嫁" + boy.getName());
		boy.marry(this);  //this代表当前对象
	}
	
	/**
	 * @Description 比较两个对象的大小
	 * @param girl
	 * @return 正数：当前对象大；负数：当前对象小; 0：当前对象与形参对象相等
	 */
	public int compare(Boy boy) {
//		if(this.age > girl.age) {
//			return 1;
//		}
//		else if(this.age < girl.age){
//			return -1;
//		}
//		else {
//			return 0;
//		}
		
		return this.age - boy.getAge();
	}
}
```

```
package javatest003;

public class BoyGirlTest {
	public static void main(String args[]) {
		Boy boy = new Boy("郑继凯",22);
		boy.shout();
		
		Girl girl = new Girl("韦章倩",23);
		girl.marry(boy);
		
		int compare = girl.compare(boy);
		if(compare > 0) {
			System.out.println(girl.getName() + "大");
		}
		else if(compare < 0) {
			System.out.println(girl.getName() + "小");
		}
		else {
			System.out.println(girl.getName() + "和" + boy.getName() + "一样大");
		}
	}
	
}
```

## 实验1：Account_Customer.doc

### 要求
```
1、写一个名为 Account 的类模拟账户。该类的属性和方法如下图所示。
该类包括的属性：
账号 id，余额 balance，年利率 annualInterestRate；
包含的方法：
访问器方法（getter 和 setter方法），取款方法 withdraw()，存款方法 deposit()。
```
![](C:/NoteBook/pictures/329575522240089.png =461x)

```
2. 创建 Customer 类。
```
![](C:/NoteBook/pictures/12335622236644.png =465x)

```
a. 声明三个私有对象属性：firstName、lastName 和 account。
b. 声明一个公有构造器，这个构造器带有两个代表对象属性的参数（f 和 l）
c. 声明两个公有存取器来访问该对象属性，方法 getFirstName 和 getLastName 返回相应的属性。
d. 声明 setAccount 方法来对 account 属性赋值。
e. 声明 getAccount 方法以获取 account 属性。
```
```
3.写一个测试程序。
（1） 创建一个 Customer ，名字叫 Jane Smith, 
      他有一个账号为 1000，余额为 2000 元，年利率为 1.23％ 的账户。
（2） 对 Jane Smith 操作。
     存入 100 元，再取出 960 元。再取出 2000 元。
     打印出 Jane Smith 的基本信息
```
![](C:/NoteBook/pictures/287495722232398.png =588x)

### 完成

```
package account.customer;

public class Account {
	private int id;
	private double balance; //余额
	private double annualInterestRate; //年利率
	
	public Account(int id, double balance, double annualInterestRate) {
		this.id = id;
		this.balance = balance;
		this.annualInterestRate = annualInterestRate;
	}

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public double getBalance() {
		return balance;
	}

	public void setBalance(double balance) {
		this.balance = balance;
	}

	public double getAnnualInterestRate() {
		return annualInterestRate;
	}

	public void setAnnualInterestRate(double annuallnterestRate) {
		this.annualInterestRate = annuallnterestRate;
	}
	
//	存钱
	public void withdraw(double amount) {
		balance += amount;
		System.out.println("成功存入: " + amount);
	}

//	取钱
	public void deposit(double amount) {
		if(balance >= amount) {
			balance -= amount;
			System.out.println("成功取出: " + amount);
		}
		else {
			System.out.println("余额不足，取款失败");
		}
	}
}
```

```
package account.customer;

public class Customer {
	private String firstName;
	private String lastName;
	private Account account;
	
	public Customer(String f, String l) {
		firstName = f;
		lastName = l;
	}

	public String getFirstName() {
		return firstName;
	}

	public String getLastName() {
		return lastName;
	}

	public Account getAccount() {
		return account;
	}

	public void setAccount(Account account) {
		this.account = account;
	}
	
	public void show() {
		System.out.println("Customer[" + firstName + "," + lastName + "] has a account : id is" + account.getId() + ",annualInterestRate is" + account.getAnnualInterestRate() + ",balance is" + account.getBalance());
	}
}
```

```
package account.customer;

public class Test {
	public static void main(String args[]) {
		Customer c1 = new Customer("Jane","Smith");
		Account a1 = new Account(1000,2000,1.23);
		
		c1.setAccount(a1);
		
		c1.getAccount().withdraw(100);
		c1.getAccount().deposit(960);
		c1.getAccount().deposit(2000);
		c1.show();
	}
}
```

## 实验2：Account_Customer_Bank.doc
### 要求
```
1.按照如下的 UML 类图，创建相应的类，提供必要的结构
```

![](C:/NoteBook/pictures/551995923247882.png =277x)

```
在提款方法 withdraw()中，需要判断用户余额是否能够满足提款数额的要求，如果不能，应给出提示。
deposit()方法表示存款。
```

```
2. 按照如下的 UML 类图，创建相应的类，提供必要的结构
```

![](C:/NoteBook/pictures/69510200245385.png =231x)

```
3. 按照如下的 UML 类图，创建相应的类，提供必要的结构
 addCustomer 方法必须依照参数（姓，名）构造一个新的 Customer 对象，然后把它放到 customer 数组中。还必须把 numberOfCustomer 属性的值加 1。
 getNumOfCustomers 方法返回 numberofCustomers 属性值。
 getCustomer 方法返回与给出的 index 参数相关的客户。
```

![](C:/NoteBook/pictures/79480300226626.png =300x)

```
4. 创建 BankTest 类，进行测试
```

### 完成

```
package com.zjk;

public class Account {
	
	private double balance;
	
	public Account (double initBalance) {
		balance = initBalance;
	}
	
	public double getBalance() {
		return balance;
	}
	
	public void deposit(double amt) {
		balance += amt;
		System.out.println("成功存入: " + amt);
	}
	
	public void withdraw(double amt) {
		if(balance < amt) {
			System.out.println("余额不足");
		}
		if(balance >= amt) {
			System.out.println("成功取出: " + amt);
			balance -= amt;
		}
	}
}
```

```
package com.zjk;

public class Customer {
	private String firstName;
	private String lastName;
	private Account account;
	
	public Customer(String f, String l) {
		firstName = f;
		lastName = l;
	}

	public String getFirstName() {
		return firstName;
	}

	public String getLastName() {
		return lastName;
	}

	public Account getAccount() {
		return account;
	}
	
	public void setAccount(Account acc) {
		this.account = acc;
	}
}
```

```
package com.zjk;

public class Bank {
	private Customer[] customers;
	private int numberOfCustomers = 0;
	
	public Bank() {
		customers = new Customer[10];
	}
	
	public void addCustomer(String f,String l) {
//		需要先造数组
		customers[numberOfCustomers++] = new Customer(f,l);  
//		java.lang.NullPointerException
//		private Customer[] customers; 数组还未造
	}
	
	public int getNumOfCustomers() {
		return numberOfCustomers;
	}
	
	public Customer getCustomer(int index) {
		if(index >= 0 && index < numberOfCustomers) {
			return customers[index];
		}
		else {
			return null;
		}
	}
}
```

```
package com.zjk;

public class Test {
	public static void main(String args[]) {
		Bank bank = new Bank();
		bank.addCustomer("理事", "国家");
		Account a = new Account(100);
		bank.getCustomer(0).setAccount(a);
		
		System.out.println("银行用户数： " + bank.getNumOfCustomers());
		
		System.out.println(bank.getCustomer(0).getFirstName() + bank.getCustomer(0).getLastName() + "的余额：" +bank.getCustomer(0).getAccount().getBalance());
		bank.getCustomer(0).getAccount().deposit(1000);
		System.out.println(bank.getCustomer(0).getFirstName() + bank.getCustomer(0).getLastName() + "的余额：" +bank.getCustomer(0).getAccount().getBalance());
		bank.getCustomer(0).getAccount().withdraw(1000);
		bank.getCustomer(0).getAccount().withdraw(1000);
	}
}
```

## package关键字

- package语句作为Java源文件的第一条语句，指明该文件中定义的类所在的包。(若缺省该语句，则指定为无名包)。它的格式为：
    - package 顶层包名.子包名 ;
    - 举例：pack1\pack2\PackageTest.java

```
package pack1.pack2; //指定类PackageTest属于包pack1.pack2

public class PackageTest{
    public void display(){
        System.out.println("in method display()");
    }
}
```

- 包对应于文件系统的目录，package语句中，用 “.” 来指明包(目录)的层次；
- 包通常用小写单词标识。通常使用所在公司域名的倒置：com.atguigu.xxx

**源文件布局：**


**包的作用：**
- 包帮助管理大型软件系统：将功能相近的类划分到同一个包中。比如：MVC的设计模式
- 包可以包含类和子包，划分项目层次，便于管理
- 解决类命名冲突的问题
- 控制访问权限
![](C:/NoteBook/pictures/102931408239388.png =656x)

### MVC设计模式

MVC是常用的设计模式之一，将整个程序分为三个层次：视图模型层，控制器层，与数据模型层。这种将程序输入输出、数据处理，以及数据的展示离开来的设计模式使程序结构变的灵活而且清晰，同时也描述了程序各个对象间的通信方式，降低了程序的耦合性。

![](C:/NoteBook/pictures/70001508227255.png =769x)

![](C:/NoteBook/pictures/300021508247421.png =549x)

### JDK中主要的包介绍
1. java.lang----包含一些Java语言的核心类，如String、Math、Integer、 System和Thread，提供常用功能
2. java.net----包含执行与网络相关的操作的类和接口。
3. java.io ----包含能提供多种输入/输出功能的类。
4. java.util----包含一些实用工具类，如定义系统特性、接口的集合框架类、使用与日期日历相关的函数。
5. java.text----包含了一些java格式化相关的类
6. java.sql----包含了java进行JDBC数据库编程的相关类/接口
7. java.awt----包含了构成抽象窗口工具集（abstract window toolkits）的多个类，这些类被用来构建和管理应用程序的图形用户界面(GUI)。 B/S C/S

## import关键字

- 为使用定义在不同包中的Java类，需用import语句来引入指定包层次下所需要的类或全部类`(.*)`。import语句告诉编译器到哪里去寻找类。
- 语法格式：`import 包名. 类名;`
- 应用举例：

```
import pack1.pack2.Test; //import pack1.pack2.*;表示引入pack1.pack2包中的所有结构

public class PackTest{
    public static void main(String args[]){
        Test t = new Test(); //Test类在pack1.pack2包中定义
        t.display();
    }
}
```

```
package com.zjk;

//import java.util.ArrayList;
//import java.util.Arrays;
//import java.util.HashMap;

//等同于

import java.util.*;

public class PackageimportTest {
	public static void main(String args[]) {
		String info = Arrays.toString(new int[] {1,2,3};
		
		Circle c1 = new Circle();
		
		ArrayList list = new ArrayList();
		HashMap map = new HashMap();
	}
}
```

- 注意：
1. 在源文件中使用import显式的导入指定包下的类或接口
2. 声明在包的声明和类的声明之间。
3. 如果需要导入多个类或接口，那么就并列显式多个import语句即可
4. 举例：可以使用java.util.*的方式，一次性导入util包下所有的类或接口。
5. 如果导入的类或接口是java.lang包下的，或者是当前包下的，则可以省略此import语句。
6. 如果在代码中使用不同包下的同名的类。那么就需要使用类的全类名的方式指明调用的是哪个类。
7. 如果已经导入java.a包下的类。那么如果需要使用a包的子包下的类的话，仍然需要导入。
8. import static组合的使用：调用指定类或接口下的静态的属性或方法

```
package com.zjk;

import java.util.Date;

import com.zjk.account001.Account;
//import com.zjk.account002.Account;
//已经存在同名类的引用

public class PackageimportTest {
	public static void main(String args[]) {
		Account acc = new Account(1000,10.2,0.1);
		com.zjk.account002.Account acc2 = new com.zjk.account002.Account(100);
//		引用另一个包中的同名类: 要写出完整的
		
		Date date = new Date();
		java.sql.Date date1 = new java.sql.Date(123);
	}
}
```

```
package com.zjk;

import static java.lang.System.*;
import static java.lang.Math.*;

public class PackageimportTest {
	public static void main(String args[]) {
		out.println("hello");
		
		long num = Math.abs(11);
	}
}
```

## 项目二

### 需求
![](C:/NoteBook/pictures/525044422220964.png =555x)

![](C:/NoteBook/pictures/81074522239390.png =703x)
![](C:/NoteBook/pictures/241254522227257.png =693x)

![](C:/NoteBook/pictures/488944522247423.png =512x)
![](C:/NoteBook/pictures/590444522240092.png =606x)

![](C:/NoteBook/pictures/92914622236647.png =535x)
![](C:/NoteBook/pictures/183244622232401.png =560x)
![](C:/NoteBook/pictures/289644622250281.png =688x)

![](C:/NoteBook/pictures/513334622247885.png =544x)

![](C:/NoteBook/pictures/16044722245387.png =714x)

![](C:/NoteBook/pictures/123824722226628.png =696x)

![](C:/NoteBook/pictures/215394722249068.png =583x)
![](C:/NoteBook/pictures/313484722244204.png =686x)

![](C:/NoteBook/pictures/410194722237750.png =628x)

![](C:/NoteBook/pictures/501814722230884.png =585x)

![](C:/NoteBook/pictures/43074822221414.png =663x)

![](C:/NoteBook/pictures/151314822223918.png =668x)

![](C:/NoteBook/pictures/272374822232865.png =678x)
![](C:/NoteBook/pictures/51085322225750.png =650x)
![](C:/NoteBook/pictures/146845322226359.png =697x)

![](C:/NoteBook/pictures/260115322253314.png =709x)
### 实现

CMUtility 
```
package com.atguigu.p2;


import java.util.*;
/**
CMUtility工具类：
将不同的功能封装为方法，就是可以直接通过调用方法使用它的功能，而无需考虑具体的功能实现细节。
*/
public class CMUtility {
    private static Scanner scanner = new Scanner(System.in);
    /**
	用于界面菜单的选择。该方法读取键盘，如果用户键入’1’-’5’中的任意字符，则方法返回。返回值为用户键入字符。
	*/
	public static char readMenuSelection() {
        char c;
        for (; ; ) {
            String str = readKeyBoard(1, false);
            c = str.charAt(0);
            if (c != '1' && c != '2' && 
                c != '3' && c != '4' && c != '5') {
                System.out.print("选择错误，请重新输入：");
            } else break;
        }
        return c;
    }
	/**
	从键盘读取一个字符，并将其作为方法的返回值。
	*/
    public static char readChar() {
        String str = readKeyBoard(1, false);
        return str.charAt(0);
    }
	/**
	从键盘读取一个字符，并将其作为方法的返回值。
	如果用户不输入字符而直接回车，方法将以defaultValue 作为返回值。
	*/
    public static char readChar(char defaultValue) {
        String str = readKeyBoard(1, true);
        return (str.length() == 0) ? defaultValue : str.charAt(0);
    }
	/**
	从键盘读取一个长度不超过2位的整数，并将其作为方法的返回值。
	*/
    public static int readInt() {
        int n;
        for (; ; ) {
            String str = readKeyBoard(2, false);
            try {
                n = Integer.parseInt(str);
                break;
            } catch (NumberFormatException e) {
                System.out.print("数字输入错误，请重新输入：");
            }
        }
        return n;
    }
	/**
	从键盘读取一个长度不超过2位的整数，并将其作为方法的返回值。
	如果用户不输入字符而直接回车，方法将以defaultValue 作为返回值。
	*/
    public static int readInt(int defaultValue) {
        int n;
        for (; ; ) {
            String str = readKeyBoard(2, true);
            if (str.equals("")) {
                return defaultValue;
            }

            try {
                n = Integer.parseInt(str);
                break;
            } catch (NumberFormatException e) {
                System.out.print("数字输入错误，请重新输入：");
            }
        }
        return n;
    }
	/**
	从键盘读取一个长度不超过limit的字符串，并将其作为方法的返回值。
	*/
    public static String readString(int limit) {
        return readKeyBoard(limit, false);
    }
	/**
	从键盘读取一个长度不超过limit的字符串，并将其作为方法的返回值。
	如果用户不输入字符而直接回车，方法将以defaultValue 作为返回值。
	*/
    public static String readString(int limit, String defaultValue) {
        String str = readKeyBoard(limit, true);
        return str.equals("")? defaultValue : str;
    }
	/**
	用于确认选择的输入。该方法从键盘读取‘Y’或’N’，并将其作为方法的返回值。
	*/
    public static char readConfirmSelection() {
        char c;
        for (; ; ) {
            String str = readKeyBoard(1, false).toUpperCase();
            c = str.charAt(0);
            if (c == 'Y' || c == 'N') {
                break;
            } else {
                System.out.print("选择错误，请重新输入：");
            }
        }
        return c;
    }

    private static String readKeyBoard(int limit, boolean blankReturn) {
        String line = "";

        while (scanner.hasNextLine()) {
            line = scanner.nextLine();
            if (line.length() == 0) {
                if (blankReturn) return line;
                else continue;
            }

            if (line.length() < 1 || line.length() > limit) {
                System.out.print("输入长度（不大于" + limit + "）错误，请重新输入：");
                continue;
            }
            break;
        }

        return line;
    }
}

```
Customer
```
package com.atguigu.p2;

public class Customer {
	private String name;
	private char gender;
	private int age;
	private String phone;
	private String email;

	public Customer(String name, char gender, int age) {
		this(name, gender, age, "", "");
	}

	public Customer(String name, char gender, int age, String phone,
			String email) {
		this.name = name;
		this.gender = gender;
		this.age = age;
		this.phone = phone;
		this.email = email;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public char getGender() {
		return gender;
	}

	public void setGender(char gender) {
		this.gender = gender;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public String getPhone() {
		return phone;
	}

	public void setPhone(String phone) {
		this.phone = phone;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	public String getDetails() {
		return name + "\t" + gender + "\t" + age + "\t\t" + phone + "\t" + email;
	}
}
```
CustomerList
```
package com.atguigu.p2;
public class CustomerList {
    private Customer[] customers;
    private int total = 0;

    public CustomerList(int totalCustomer) {
        customers = new Customer[totalCustomer];
    }

    public boolean addCustomer(Customer customer) {
        if (total >= customers.length) return false;
        
        customers[total++] = customer;
        return true;
    }
     
    public boolean replaceCustomer(int index, Customer cust) {
        if (index < 0 || index >= total) return false;
        
        customers[index] = cust;
        return true;
    }

    public boolean deleteCustomer(int index) {
        if (index < 0 || index >= total) return false;
        
        for (int i = index; i < total - 1; i++) {
            customers[i] = customers[i + 1];
        }
        
        customers[--total] = null;

        return true;
    }

    public Customer[] getAllCustomers() {
        Customer[] custs = new Customer[total];
        for (int i = 0; i < total; i++) {
            custs[i] = customers[i];
        }
        return custs;
    }

    public int getTotal() {
        return total;
    }

    public Customer getCustomer(int index) {
        if (index < 0 || index >= total) return null;
        
        return customers[index];
    }
}
```
CustomerView
```
package com.atguigu.p2;

public class CustomerView {
	private CustomerList customers = new CustomerList(10);

	public CustomerView() {
		Customer cust = new Customer("张三", '男', 30, "010-56253825",
				"abc@email.com");
		customers.addCustomer(cust);
	}

	public void enterMainMenu() {
		boolean loopFlag = true;
		do {
			System.out
					.println("\n-----------------客户信息管理软件-----------------\n");
			System.out.println("                   1 添 加 客 户");
			System.out.println("                   2 修 改 客 户");
			System.out.println("                   3 删 除 客 户");
			System.out.println("                   4 客 户 列 表");
			System.out.println("                   5 退       出\n");
			System.out.print("                   请选择(1-5)：");

			char key = CMUtility.readMenuSelection();
			System.out.println();
			switch (key) {
			case '1':
				addNewCustomer();
				break;
			case '2':
				modifyCustomer();
				break;
			case '3':
				deleteCustomer();
				break;
			case '4':
				listAllCustomers();
				break;
			case '5':
				System.out.print("确认是否退出(Y/N)：");
				char yn = CMUtility.readConfirmSelection();
				if (yn == 'Y')
					loopFlag = false;
				break;
			}
		} while (loopFlag);
	}

	private void addNewCustomer() {
		System.out.println("---------------------添加客户---------------------");
		System.out.print("姓名：");
		String name = CMUtility.readString(4);
		System.out.print("性别：");
		char gender = CMUtility.readChar();
		System.out.print("年龄：");
		int age = CMUtility.readInt();
		System.out.print("电话：");
		String phone = CMUtility.readString(15);
		System.out.print("邮箱：");
		String email = CMUtility.readString(15);

		Customer cust = new Customer(name, gender, age, phone, email);
		boolean flag = customers.addCustomer(cust);
		if (flag) {
			System.out
					.println("---------------------添加完成---------------------");
		} else {
			System.out.println("----------------记录已满,无法添加-----------------");
		}
	}

	private void modifyCustomer() {
		System.out.println("---------------------修改客户---------------------");

		int index = 0;
		Customer cust = null;
		for (;;) {
			System.out.print("请选择待修改客户编号(-1退出)：");
			index = CMUtility.readInt();
			if (index == -1) {
				return;
			}

			cust = customers.getCustomer(index - 1);
			if (cust == null) {
				System.out.println("无法找到指定客户！");
			} else
				break;
		}

		System.out.print("姓名(" + cust.getName() + ")：");
		String name = CMUtility.readString(4, cust.getName());

		System.out.print("性别(" + cust.getGender() + ")：");
		char gender = CMUtility.readChar(cust.getGender());

		System.out.print("年龄(" + cust.getAge() + ")：");
		int age = CMUtility.readInt(cust.getAge());

		System.out.print("电话(" + cust.getPhone() + ")：");
		String phone = CMUtility.readString(15, cust.getPhone());

		System.out.print("邮箱(" + cust.getEmail() + ")：");
		String email = CMUtility.readString(15, cust.getEmail());

		cust = new Customer(name, gender, age, phone, email);

		boolean flag = customers.replaceCustomer(index - 1, cust);
		if (flag) {
			System.out
					.println("---------------------修改完成---------------------");
		} else {
			System.out.println("----------无法找到指定客户,修改失败--------------");
		}
	}

	private void deleteCustomer() {
		System.out.println("---------------------删除客户---------------------");

		int index = 0;
		Customer cust = null;
		for (;;) {
			System.out.print("请选择待删除客户编号(-1退出)：");
			index = CMUtility.readInt();
			if (index == -1) {
				return;
			}

			cust = customers.getCustomer(index - 1);
			if (cust == null) {
				System.out.println("无法找到指定客户！");
			} else
				break;
		}

		System.out.print("确认是否删除(Y/N)：");
		char yn = CMUtility.readConfirmSelection();
		if (yn == 'N')
			return;

		boolean flag = customers.deleteCustomer(index - 1);
		if (flag) {
			System.out
					.println("---------------------删除完成---------------------");
		} else {
			System.out.println("----------无法找到指定客户,删除失败--------------");
		}
	}

	private void listAllCustomers() {
        System.out.println("---------------------------客户列表---------------------------");
        Customer[] custs = customers.getAllCustomers();
        if (custs.length == 0) {
            System.out.println("没有客户记录！");
        } else {
            System.out.println("编号\t姓名\t性别\t年龄\t\t电话\t\t邮箱");
            for (int i = 0; i < custs.length; i++) {
//            System.out.println(i + 1 + "\t" + custs[i].getName() + "\t" + custs[i].getGender() + "\t" + custs[i].getAge() + "\t\t" + custs[i].getPhone() + "\t" + custs[i].getEmail());
            	System.out.println((i+1) + "\t" + custs[i].getDetails());
            }
        }

        System.out.println("-------------------------客户列表完成-------------------------");
    }

	public static void main(String[] args) {
		CustomerView cView = new CustomerView();
		cView.enterMainMenu();
	}
}
```
## 面向对象特征之二：继承性

![](C:/NoteBook/pictures/452381510220962.png =588x)
![](C:/NoteBook/pictures/585981510239388.png =559x)
![](C:/NoteBook/pictures/142261610227255.png =674x)

![](C:/NoteBook/pictures/305161610247421.png =602x)

```
package com.zjk;

public class Person {

	private String name;
	private int age;

	public void eat() {
		System.out.println("吃饭");
	}

	private void sleep() {
		System.out.println("睡觉");
	}
	
	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}

	public void setName(String name) {
		this.name = name;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public Person() {

	}

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}
}

package com.zjk;

public class Student extends Person{
	private String school;
	
	public void study() {
		System.out.println("学习");
	}
	
	public void show() {
//		System.out.println("name=" + name + "school=" + school);
//		The field Person.name is not visible
//		name Person类中私有private 在子类Student中需要使用Person的公有方法调用
		
		System.out.println("name=" + getName() + "school=" + school);
	}
}

package com.zjk;

public class ExtendsTest {
	public static void main(String[] args) {
		Person p1 = new Person();
		p1.eat();
		
		Student s1 = new Student();
		s1.eat();
//		s1.sleep();
//		The method sleep() from the type Person is not visible
//		Person类的sleep()私有private
	}
}

```


- 为什么要有继承？
    - 多个类中存在相同属性和行为时，将这些内容抽取到单独一个类中，那么多个类无需再定义这些属性和行为，只要继承那个类即可。
- 此处的多个类称为子类(派生类)，单独的这个类称为父类(基类或超类)。可以理解为:“子类 is a 父类”
- 类继承语法规则:

```
class Subclass extends SuperClass{ }
```
![](C:/NoteBook/pictures/247221810240090.png =498x)
- 作用：
   1. 继承的出现减少了代码冗余，提高了代码的复用性。
   2. 继承的出现，更有利于功能的扩展。
   3. 继承的出现让类与类之间产生了关系，提供了多态的前提。
- 注意：不要仅为了获取其他类中某个功能而去继承
   - 子类继承了父类，就继承了父类的方法和属性。(获取了直接父类和所有间接父类中声明的属性和方法)
   - 在子类中，可以使用父类中定义的方法和属性，也可以创建新的数据和方法。
   - 在Java 中，继承的关键字用的是“extends”，即子类不是父类的子集，而是对父类的“扩展”。
- 类的继承不改变类成员的访问权限，也就是说，如果父类的成员是公有的、被保护的或默认的，它的子类仍具有相应的这些特性，并且子类不能获得父类的构造方法。
- 注意：如果在父类中存在有参的构造方法而并没有重载无参的构造方法，那么在子类中必须含有有参的构造方法，因为如果在子类中不含有构造方法，默认会调用父类中无参的构造方法，而在父类中并没有无参的构造方法，因此会出错。
- 关于继承的规则：
   - 子类不能直接访问父类中私有的(private)的成员变量和方法(封装性)。(获得了)
   
![](C:/NoteBook/pictures/19012010232399.png =350x)

- Java只支持单继承和多层继承，不允许多重继承
   - 一个子类只能有一个父类
   - 一个父类可以派生出多个子类

``` 
class SubDemo extends Demo{ } //ok
class SubDemo extends Demo1,Demo2...//error
```

![](C:/NoteBook/pictures/173902110250279.png =692x)

![](C:/NoteBook/pictures/308232110247883.png =632x)

### java.lang.Object类(根父类)
- 如果我们没有显示的声明一个类的父类时，则此类继承于java.lang.Object类
- 所有的java类(除java.lang.Object类之外)都直接或间接的继承于java.lang.Object类
- 意味着，所有的java类都具有java.lang.Object类声明的功能

```
package com.zjktr;

public class Creature {

}

package com.zjktr;

public class Test {
	public static void main(String[] args) {
		Creature c = new Creature();

		System.out.println(c.toString());
		//Creature类继承自java.lang.Object类的toString()
	}
}

```

### 练习1
#### 1.定义一个学生类Student，它继承自Person类

![](C:/NoteBook/pictures/54332210226626.png =419x)

```
public class Person {
	String name;
	char sex;
	int age;
	
	public Person(String name, char sex, int age) {
		this.name = name;
		this.sex = sex;
		this.age = age;
	}
	
	public String toString() {
		return "person";
	}
}

class Student extends Person{
	long number;
	int math;
	int english;
	int computer;
	
	public Student(String n, char s, int a, long k, int m, int e, int c) {
		super(n,s,a); 
		number = k;
		math = m;
		english = e;
		computer = c;
	}
	
	public double aver() {
		return (math + english + computer) / 3;
	}
	
	public int max() {
		int maxValue = math > english ? math : english;
		maxValue = maxValue > computer ? maxValue : computer;
		return maxValue;
	}
	
	public int min() {
		int min = math < english ? math : english;
		return min = min < computer ? min : computer;
	}
	
	public String toString() {
		return "student";
	}
}
```

#### 
```
(1)定义一个ManKind类，包括
成员变量int sex和int salary；
方法void manOrWoman()：根据sex的值显示“man”(sex==1)或者“woman”(sex==0)；
方法void employeed()：根据salary的值显示“no job”(salary==0)或者“ job”(salary!=0)。
(2)定义类Kids继承ManKind，并包括
成员变量int yearsOld；
方法printAge()打印yearsOld的值。
(3)定义类KidsTest，在类的main方法中实例化Kids的对象someKid，用该对象访问其父类的成员变量及方法。
```

```
package com.zjk;

public class ManKind {
	int sex;
	int salary;
	
	public void manOrWoman() {
		if(sex == 1) {
			System.out.println("man");
		}
		else if(sex == 0) {
			System.out.println("woman");
		}
		else {
			System.out.println("有误");
		}
	}
	
	public void employeed() {
		if(salary == 0) {
			System.out.println("no job");
		}
		else if(salary > 0){
			System.out.println("job");
		}
	}
}

class Kids extends ManKind{
	int yearsOld;
	
	public void printAge() {
		System.out.println("old: " + yearsOld);
	}
}
```

```
package com.zjk;

public class kidTest {
	public static void main(String args[]) {
		Kids somekid = new Kids();
		somekid.sex = 1;
		somekid.manOrWoman();
	}
}
```

#### 根据下图实现类。在CylinderTest类中创建Cylinder类的对象，设置圆柱的底面半径和高，并输出圆柱的体积。

![](C:/NoteBook/pictures/382592510244202.png =371x)

```
package com.zjk;

public class Circle {
	private double radius;
	
	Circle(){
		radius = 1;
	}
	
	public void setRadius(double radius) {
		this.radius = radius;
	}
	
	public double getRadius() {
		return radius;
	}
	
	public double findArea() {
		return radius * radius * Math.PI;
	}
}

class Cylinder extends Circle{
	private double length;
	
	Cylinder(){
		length = 1;
	}
	
	public void setLength(double length) {
		this.length = length;
	}
	
	public double getLength() {
		return length;
	}
	
	public double findVolume() {
		return super.findArea() * length;
	}
}
```

```
package com.zjk;

public class CircleTest {
	public static void main(String args[]) {
		Cylinder cy = new Cylinder();
		
		cy.setLength(2);
		cy.setRadius(3);
		System.out.println(cy.findVolume());
	}
}
```

### 方法的重写(同名同参)

- 定义：在子类中可以根据需要对从父类中继承来的方法进行改造，也称为方法的重置、覆盖。在程序执行时，子类的方法将覆盖父类的方法。
- 要求：
   1. 子类重写的方法必须和父类被重写的方法具有相同的方法名称、参数列表
   2. 子类重写的方法的返回值类型不能大于父类被重写的方法的返回值类型
   - 父类被重写的方法的返回值是void，则子类重写的方法的返回值类型只能是void
   - 父类被重写的方法的返回值是A类型，则子类重写的方法的返回值类型可以是A类或A类的子类
   - 父类被重写的方法的返回值是基本数据类型，则子类重写的方法的返回值类型必须是相同的基本数据类型(父类double则子类只能是double)
   3. 子类重写的方法使用的访问权限不能小于父类被重写的方法的访问权限
   - 子类不能重写父类中声明为private权限的方法
   4. 子类方法抛出的异常不能大于父类被重写方法抛出的异常
- 注意：
   - 子类与父类中同名同参数的方法必须同时声明为非static的(即为重写)，或者同时声明为static的（不是重写）。
      - 因为static方法是属于类的，子类无法覆盖父类的方法。静态的方法不能被覆盖，是随着类的加载而加载。
 
#### 区分方法的重载和重写



#### 例1
![](C:/NoteBook/pictures/124002610237748.png =733x)

![](C:/NoteBook/pictures/247452610230882.png =704x)


#### 例2

```
package com.zjk;

public class Person {
	String name;
	int age;

	public Person() {

	}

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public void eat() {
		System.out.println("吃饭");
	}

	public void walk(int distance) {
		System.out.println("走路: " + distance + "km");
	}

	private void show() {
		System.out.println("我是人");
	}

	public void test() {
		show();
		eat();
	}
	
	public Object info() {
		return null;
	}
	
	public double info1() {
		return 0.0;
	}
}

package com.zjk;

public class Student extends Person{
	String major;

	public Student() {

	}

	public Student(String major) {
		this.major = major;
	}

	public void study() {
		System.out.println("学习");
	}
	
	//重写
	public void eat() {
		System.out.println("多吃营养的");
	}
	
//	Person类中的show()方法是private权限，Student类不可见
//	此时不认为是重写，
	public void show() {
		System.out.println("我是学生");
	}
	
//	public Object info() {
	public String info() { //String类是Object类的子类
		return null;
	}
	
//	The return type is incompatible with Person.info1()
//	public float info1() {
//		return 1.1f;
//	}
//  返回类型:父类double则子类只能是double
	public double info1() {
		return 0.0;
	}
}

package com.zjk;

public class PersonTest {
	public static void main(String[] args) {
		Student s = new Student("计算机科学与技术");
		s.eat();
		s.walk(10);
		
		s.study();
		s.test();
		s.show(); //没有重写
		//如果重写，test()输出的应该是重写之后的方法
	}
}
```
#### 练习2

1.如果现在父类的一个方法定义成private访问权限，在子类中将此方法声明为default访问权限，那么这样还叫重写吗？(NO)
2. 修改练习1.2中定义的类Kids，在Kids中重新定义employeed()方法，覆盖父类ManKind中定义的employeed()方法，输出“Kids should study and no job.”

```
package com.zjk;

public class ManKind {
	int sex;
	int salary;
	
	public void manOrWoman() {
		if(sex == 1) {
			System.out.println("man");
		}
		else if(sex == 0) {
			System.out.println("woman");
		}
		else {
			System.out.println("有误");
		}
	}
	
	public void employeed() {
		if(salary == 0) {
			System.out.println("no job");
		}
		else if(salary > 0){
			System.out.println("job");
		}
	}
}

class Kids extends ManKind{
	int yearsOld;
	
	public void printAge() {
		System.out.println("old: " + yearsOld);
	}
	
	public void employeed() {
		System.out.println("kids should study and no job");
	}
}
```
 
### 继承的权限修饰符(同之前)
![](C:/NoteBook/pictures/579543022239392.png =391x)
### super关键字：

- 在Java类中使用super来调用父类中的指定操作：
    - super.属性 super可用于访问父类中定义的属性
    - supere.方法 super可用于调用父类中定义的成员方法
    - super(形参列表) super可用于在子类构造器中调用父类的构造器
- 注意：
    - 尤其当子父类出现同名成员时，可以用super表明调用的是父类中的成员
    - super的追溯不仅限于直接父类
    - super和this的用法相像，this代表本类对象的引用，super代表父类的内存空间的标识

#### 举例

##### 例1
```
class Person {
	protected String name = "张三";
	protected int age;

	public String getInfo() {
		return "Name: " + name + "\nage: " + age;
	}
}

class Student extends Person {
	protected String name = "李四";
	private String school = "New Oriental";

	public String getSchool() {
		return school;
	}

	public String getInfo() {
		return super.getInfo() + "\nschool: " + school;
		//调用父类Person的getInfo()方法
	}
}

public class StudentTest {
	public static void main(String[] args) {
		Student st = new Student();
		System.out.println(st.getInfo());
	}
}
```

- 我们可以通过super关键字来实现对父类成员的访问，用来引用当前对象的父类。

```
class Animal {
  void eat() {
    System.out.println("animal : eat");
  }
}
 
class Dog extends Animal {
  void eat() {
    System.out.println("dog : eat");
  }
  void eatTest() {
    this.eat();   // this 调用自己的方法
    super.eat();  // super 调用父类方法
  }
}
 
public class Test {
  public static void main(String[] args) {
    Animal a = new Animal();
    a.eat();
    Dog d = new Dog();
    d.eatTest();
  }
}
```

##### 例2
教师和学生都属于人，他们具有共同的属性：姓名、年龄、性别和身份证号，而学生还具有学号和所学专业两个属性，教师还具有教龄和所教专业两个属性。

下面编写 Java 程序代码，使教师（Teacher）类和学生（Student）类都继承于人（People）类，具体的实现步骤如下。
```
public class People {
    public String name; // 姓名
    public int age; // 年龄
    public String sex; // 性别
    public String sn; // 身份证号

    public People(String name, int age, String sex, String sn) {
        this.name = name;
        this.age = age;
        this.sex = sex;
        this.sn = sn;
    }

    public String toString() {
        return "姓名：" + name + "\n年龄：" + age + "\n性别：" + sex + "\n身份证号：" + sn;
    }
}
```

```
public class Student extends People {
    private String stuNo; // 学号
    private String department; // 所学专业

    public Student(String name, int age, String sex, String sn, String stuno, String department) {
        super(name, age, sex, sn); // 调用父类中的构造方法
        this.stuNo = stuno;
        this.department = department;
    }

    public String toString() {
        return "姓名：" + name + "\n年龄：" + age + "\n性别：" + sex + "\n身份证号：" + sn + "\n学号：" + stuNo + "\n所学专业：" + department;
    }
}
```

```
public class Teacher extends People {
    private int tYear; // 教龄
    private String tDept; // 所教专业

    public Teacher(String name, int age, String sex, String sn, int tYear, String tDept) {
        super(name, age, sex, sn); // 调用父类中的构造方法
        this.tYear = tYear;
        this.tDept = tDept;
    }

    public String toString() {
        return "姓名：" + name + "\n年龄：" + age + "\n性别:" + sex + "\n身份证号：" + sn + "\n教龄：" + tYear + "\n所教专业：" + tDept;
    }
}
```

```
public class PeopleTest {
    public static void main(String[] args) {
        // 创建Student类对象
        People stuPeople = new Student("王丽丽", 23, "女", "410521198902145589", "00001", "计算机应用与技术");
        System.out.println("----------------学生信息---------------------");
        System.out.println(stuPeople);

        // 创建Teacher类对象
        People teaPeople = new Teacher("张文", 30, "男", "410521198203128847", 5, "计算机应用与技术");
        System.out.println("----------------教师信息----------------------");
        System.out.println(teaPeople);
    }
} 
```

#### 调用父类的构造器
- 子类中所有的构造器默认都会访问父类中空参数的构造器
- 当父类中没有空参数的构造器时，子类的构造器必须通过this(参数列表)或者super(参数列表)语句指定调用本类或者父类中相应的构造器。同时，只能”二选一”，且必须放在构造器的首行。
- 在类的多个构造器中，至少有一个类的构造器中使用了`super(形参列表)`调用父类中的构造器。
- 如果子类构造器中既未在首行显式调用父类`super(参数列表)`或本类`this(参数列表)`的构造器，且父类中又没有无参的构造器，则编译出错

#### this和super的区别
![](C:/NoteBook/pictures/596003622227259.png =511x)


#### 例

```
package com.zjk;

public class Person {
	private String name;
	private int age;
	int id = 1001; // 身份证

	public Person() {

	}

	public Person(String name) {
		this.name = name;
	}

	public Person(String name, int age) {
		this(name);
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}

	public void eat() {
		System.out.println("eat");
	}

	public void walk() {
		System.out.println("walk");
	}
}

package com.zjk;

public class Student extends Person {
	String major;
	int id = 1002; // 学号

	public Student() {
		super(); 
//		子类中所有的构造器默认都会访问父类中空参数的构造器,
//		如果父类中没有默认空参构造器而子类构造器首行也没有super(参数列表)时报错
//		不管是否删除，在子类的构造器中都存在super(形参列表)
	}

	public Student(String major) {
		this.major = major;
	}

	public Student(String name, int age, String major) {
//		当父类的属性是public等对子类可见时：this
//		this.name  = name;
//		this.age = age;

//      当父类的属性对子类不可见时：如private: super
		super(name, age);
//      super(形参) 调用父类的构造器

		this.major = major;
	}

	@Override
	public void eat() {
		System.out.println("eat well");
	}

	public void study() {
		System.out.println("study");

	}

	public void show() {
		System.out.println("name=" +getName() 
		+ ",age=" + getAge() 
		+ ",major=" +major);
	}

	public void showId() {
		System.out.println("身份证：" + super.id + ", 学号：" + id);
//		子父类属性同名(不是重写)时：必须使用 super.属性 的方法来显式的调用父类的属性
//		super.属性 调用父类的属性
	}

	public void test() {
		this.eat();
		super.eat();
//		在子类中调用父类被重写的方法，必须使用 super.方法 的方式显示调用父类的方法
		walk();
	}
}

package com.zjk;

public class SuperTest {
	public static void main(String[] args) {
		Student s1 = new Student();
		s1.showId();
		s1.eat();
		s1.study();
		s1.test();
		
		Student s2 = new Student("JK",21,"IT");
		s2.show();
	}
}
```

#### 练习

##### 1.
修改1.2中定义的类Kids中employeed()方法，在该方法中调用父类ManKind的employeed()方法，然后再输出“but Kids should study and no job.”
##### 
修改练习1.3中定义的Cylinder类，在Cylinder类中覆盖findArea()方法，计算圆柱的表面积。考虑：findVolume方法怎样做相应的修改？在CylinderTest类中创建Cylinder类的对象，设置圆柱的底面半径和高，并输出圆柱的表面积和体积。
##### 附加题：
在CylinderTest类中创建一个Circle类的对象，设置圆的半径，计算输出圆的面积。体会父类和子类成员的分别调用。


### 子类对象实例化的全过程

1. 从结果上来看，（继承性）
    - 子类继承父类以后，就获取了父类中声明的属性或方法
    - 创建子类的对象，在堆空间中，就会加载所有父类中声明的属性
2. 从过程上来看，
    - 当我们通过子类的构造器创建子类对象时，我们一定会直接或间接的调用其父类的构造器，继而调用其父类的父类的构造器，直到调用了java.lang.Obeject类中空参的构造器为止。正因为加载过所有父类的结构，所以才可以看到内存中有父类中的结构，子类对象才可以考虑进行调用。
    
- 虽然创建子类对象时，调用了父类的构造器，但就只创建了一个对象，即为new的子类对象。
    
 
![](C:/NoteBook/pictures/170393723247425.png =775x)

- 无论哪个构造器创建子类对象，需要保证先初始化父类
- 目的：当子类继承父类之后，“继承”父类中所有的属性和方法，因此子类有必要知道父类如何为对象进行初始化
![](C:/NoteBook/pictures/82235323236649.png =598x)

#### 例
```
class Creature {
    public Creature() {
        System.out.println("Creature无参数的构造器");
    }
}

class Animal extends Creature {
	public Animal(String name) {
		System.out.println("Animal带一个参数的构造器，该动物的name为" + name);
	}

	public Animal(String name, int age) {
		this(name);
		System.out.println("Animal带两个参数的构造器，其age为" + age);
	}
}

public class Wolf extends Animal {
	public Wolf() {
		super("灰太狼", 3);
		System.out.println("Wolf无参数的构造器");
	}

	public static void main(String[] args) {
		new Wolf();
	}
}
```
#### 练习4
修改练习1.3中定义的Circle类和Cylinder类的构造器，利用构造器参数为对象的所有属性赋初值。

```

```

### 实验-继承&super.doc
#### 1
##### 要求
```
1、写一个名为 Account 的类模拟账户。该类的属性和方法如下图所示。
该类包括的属性：
账号 id，余额 balance，年利率 annualInterestRate；
包含的方法：
访问器方法（getter 和setter 方法），
返回月利率的方getMonthlyInterest()，
取款方法 withdraw()，存款方法deposit()。
```

![](C:/NoteBook/pictures/290665623232403.png =441x)


写一个用户程序测试 Account 类。在用户程序中，创建一个账号为 1122、余额为 20000、年利率 4.5%的 Account 对象。使用 withdraw 方法提款 30000 元，并打印余额。再使用 withdraw 方法提款 2500 元，使用 deposit 方法存款 3000 元，然后打印余额和月利率。

提示：在提款方法 withdraw 中，需要判断用户余额是否能够满足提款数额的要求，如果不能，应给出提示。

运行结果如图所示

![](C:/NoteBook/pictures/87385723250283.png =213x)

##### 实现
```
package com.zjk;

public class Account {
	private int id;
	private double balance;
	private double annualInterestRate;

	public Account(int id, double balance, double annualInterestRate) {
		super();
		this.id = id;
		this.balance = balance;
		this.annualInterestRate = annualInterestRate;
	}

	public int getId() {
		return id;
	}

	public double getBalance() {
		return balance;
	}

	public double getAnnualInterestRate() {
		return annualInterestRate;
	}

	public void setId(int id) {
		this.id = id;
	}

	public void setBalance(double balance) {
		this.balance = balance;
	}
	/**
	 * 
	 * @Desciption 取款
	 * @param amount
	 */
	public void withdraw(double amount) {
		if (balance >= amount) {
			balance -= amount;
			System.out.println("取款成功");
			System.out.println("余额：" + balance);
		} else {
			System.out.println("余额不足");
			System.out.println("余额：" + balance);
		}
	}

	/**
	 * 
	 * @Desciption 存款
	 * @param amount
	 */
	public void deposit(double amount) {
		balance += amount;
		System.out.println("存款成功");
		System.out.println("余额：" + balance);
		System.out.println("月利率：" + annualInterestRate);
	}
}

package com.zjk;

public class Test001 {
	public static void main(String[] args) {
		Account a1 = new Account(1122,20000,0.045);
		a1.withdraw(100);
		a1.deposit(2300);
	}
}
```

#### 2

##### 要求

2、创建 Account 类的一个子类 CheckAccount 代表可透支的账户，该账户中定义一个属性overdraft 代表可透支限额。在 CheckAccount 类中重写 withdraw 方法，其算法如下：


![](C:/NoteBook/pictures/302075923249070.png =439x)

```
要求：写一个用户程序测试 CheckAccount 类。在用户程序中，创建一个账号为 1122、余
额为 20000、年利率 4.5%，可透支限额为 5000 元的 CheckAccount 对象。
使用 withdraw 方法提款 5000 元，并打印账户余额和可透支额。
再使用 withdraw 方法提款 18000 元，并打印账户余额和可透支额。
再使用 withdraw 方法提款 3000 元，并打印账户余额和可透支额。
提示：
（1） 子类 CheckAccount 的构造方法需要将从父类继承的 3 个属性和子类自己的属性全
部初始化。
（2） 父类Account的属性balance被设置为private，但在子类CheckAccount的withdraw
方法中需要修改它的值，因此应修改父类的 balance 属性，定义其为 protected。
运行结果如下图所示
```
![](C:/NoteBook/pictures/589225823245389.png =182x)

##### 实现

```
package com.zjk;

public class Account {
	private int id;
	protected double balance;
	private double annualInterestRate;

	public Account(int id, double balance, double annualInterestRate) {
		super();
		this.id = id;
		this.balance = balance;
		this.annualInterestRate = annualInterestRate;
	}

	public int getId() {
		return id;
	}

	public double getBalance() {
		return balance;
	}

	public double getAnnualInterestRate() {
		return annualInterestRate;
	}

	public void setId(int id) {
		this.id = id;
	}

	public void setBalance(double balance) {
		this.balance = balance;
	}
	/**
	 * 
	 * @Desciption 取款
	 * @param amount
	 */
	public void withdraw(double amount) {
		if (balance >= amount) {
			balance -= amount;
			System.out.println("取款成功");
			System.out.println("余额：" + balance);
		} else {
			System.out.println("余额不足");
			System.out.println("余额：" + balance);
		}
	}

	/**
	 * 
	 * @Desciption 存款
	 * @param amount
	 */
	public void deposit(double amount) {
		balance += amount;
		System.out.println("存款成功");
		System.out.println("余额：" + balance);
		System.out.println("月利率：" + annualInterestRate);
	}
}

package com.zjk;

public class CheckAccount extends Account {
	private double overdraft;

	public CheckAccount(int id, double balance, double annualInterestRate, double overdraft) {
		super(id, balance, annualInterestRate);
		this.overdraft = overdraft;
	}

	@Override
	public void withdraw(double amount) {
		// TODO Auto-generated method stub
		if (amount <= balance) {
			balance -= amount;
			System.out.println("取款成功");
			System.out.println("余额：" + balance);
		} else if (amount > balance) {
			if (amount <= (balance + overdraft)) {
				amount -= balance;
				balance = 0;
				overdraft -= amount;
				System.out.println("取款成功");
				System.out.println("余额：" + balance);
				System.out.println("透支：" + overdraft);
			} else {
				System.out.println("超过透支限额");
			}
		}
	}
}

package com.zjk;

public class Test001 {
	public static void main(String[] args) {
		Account a1 = new Account(1122,20000,0.045);
		a1.withdraw(100);
		a1.deposit(2300);
		
		CheckAccount c1 = new CheckAccount(1122,20000,0.045,5000);
		c1.withdraw(5000);
		c1.withdraw(18000);
		c1.withdraw(3000);
	}
}
```

## 面向对象特征之三：多态性

- 多态性，是面向对象中最重要的概念，在Java中的体现：
   - 对象的多态性：父类的引用指向子类的对象
   - 可以直接应用在抽象类和接口上
- Java引用变量有两个类型：编译时类型和运行时类型。
   - 编译时类型由声明该变量时使用的类型决定，在编译期，只能调用父类中声明的方法
   - 运行时类型由实际赋给该变量的对象决定。 在运行期，实际执行的是子类重写父类的方法
       - 简称：编译时，看左边；运行时，看右边。
- 若编译时类型和运行时类型不一致，就出现了对象的多态性(Polymorphism)
  - 多态情况下，
     - “看左边”看的是父类的引用（父类中不具备子类特有的方法）
     - “看右边”看的是子类的对象（实际运行的是子类重写父类的方法）

- 多态性使用前提：
    1. 类的继承关系
    2. 方法的重写

- 对象的多态性只适用于方法，不适用于属性(属性的编译和运行都看左边)
- 对象的多态 —在Java中,子类的对象可以替代父类的对象使用
- 一个变量只能有一种确定的数据类型
- 一个引用类型变量可能指向(引用)多种不同类型的对象
- 有了对象的多态性以后，内存中实际上是加载了子类特有的属性和方法，
    - 但是由于变量声明为父类类型，导致编译时，只能调用父类中声明的属性和方法。子类特有的属性和方法不能调用。
```
Person p = new Student();
Object o = new Person();//Object类型的变量o，指向Person类型的对象
o = new Student(); //Object类型的变量o，指向Student类型的对象
```

- 子类可看做是特殊的父类，所以父类类型的引用可以指向子类的对象：向上转型(upcasting)。

- 一个引用类型变量如果声明为父类的类型，但实际引用的是子类对象，那么该变量就不能再访问子类中添加的属性和方法

```
Student m = new Student();
m.school = “pku”; //合法,Student类有school成员变量
Person e = new Student(); 
e.school = “pku”; //非法,Person类没有school成员变量
```
- 属性是在编译时确定的，编译时e为Person类型，没有school成员变量，因而编译错误。

### 例1

```
package com.zjk;

public class PersonTest {
	public static void main(String[] args) {

		Person p1 = new Person();
		p1.eat();

		Man man = new Man();
		man.eat();
		man.age = 25;
		man.earnMoney();

		System.out.println("-------------");

//		对象的多态性: 父类的引用指向子类的对象(子类的对象赋给父类引用)
		Person p2 = new Man();

//		多态的使用：当调用子父类同名同参的方法时，实际执行的是子类重写父类的方法
//		--虚拟方法调用
		p2.eat();
		p2.walk();

//		p2.earnMoney(); 
//		编译时，看左边：Person p2  只能是Person类中声明的
//		执行时，看右边：new Man()  实际执行的是Man类里重写的
//      不能调用子类所特有的方法/属性，编译时，p2是Person类型

//      有了对象的多态性以后，内存中实际上是加载了子类特有的属性和方法，
//      但是由于变量声明为父类类型，导致编译时，只能调用父类中声明的属性和方法。子类特有的属性和方法不能调用。

		System.out.println(p2.id);
//		1001 输出的是父类中定义的属性
//		对象的多态性只适用于方法，不适用于属性
	}
}

class Person {
	String name;
	int age;
	int id = 1001;

	public void eat() {
		System.out.println("吃饭");
	}

	public void walk() {
		System.out.println("走路");
	}
}

class Man extends Person {

	boolean isSmoking;
	int id = 1002;

	public void earnMoney() {
		System.out.println("男人负责赚钱养家");
	}

	@Override
	public void eat() {
		// TODO Auto-generated method stub
		System.out.println("多吃长肉");
	}

	@Override
	public void walk() {
		// TODO Auto-generated method stub
		System.out.println("帅气走路");
	}
}

class Woman extends Person {

	boolean isBeauty;

	public void goShopping() {
		System.out.println("女生喜欢购物");
	}

	@Override
	public void eat() {
		// TODO Auto-generated method stub
		System.out.println("女生少吃，减肥");
	}

	@Override
	public void walk() {
		// TODO Auto-generated method stub
		System.out.println("走路窈窕");
	}
}
```

### 例2

```
package com.zjk;

import java.sql.Connection;

public class AnimalTest {
	public static void main(String[] args) {

		AnimalTest test = new AnimalTest();
		test.function(new Dog());

		test.function(new Cat());

	}

	public void function(Animal animal) {
		animal.eat();
		animal.shout();
	}
//  多态性省去了以下的重载代码
//	public void function(Cat cat) {
//		cat.eat();
//		cat.shout();
//	}
//
//	public void function(Dog dog) {
//		dog.eat();
//		dog.shout();
//	}
}

class Animal {

	public void eat() {
		System.out.println("动物,进食");
	}

	public void shout() {
		System.out.println("动物，叫");
	}
}

class Dog extends Animal {
	@Override
	public void eat() {
		// TODO Auto-generated method stub
		System.out.println("狗，吃肉");
	}

	@Override
	public void shout() {
		// TODO Auto-generated method stub
		System.out.println("狗，叫");
	}
}

class Cat extends Animal {
	@Override
	public void eat() {
		// TODO Auto-generated method stub
		System.out.println("猫，吃鱼");
	}

	@Override
	public void shout() {
		// TODO Auto-generated method stub
		System.out.println("猫，叫");
	}
}

//附加例1
class Order {
	public void method(Object obj) {

	}
}

//附加例2
class Driver {

	public void doData(Connection conn) { 
//		conn = new MySQLConnection();
//		conn = new OracleConnection();
//		规范的步骤去操作数据
//		conn.方法1();
//		conn.方法2();
		
	}
}
```

### 多态性应用举例
- 方法声明的形参类型为父类类型，可以使用子类的对象作为实参调用该方法

```
public class Test {
    public void method(Person e) {
        // ……
        e.getInfo();
}
public static void main(Stirng args[]) {
    Test t = new Test();
    Student m = new Student();
    t.method(m); // 子类的对象m传送给父类类型的参数e
    }
}
```

### 多态性的使用：虚拟方法调用(Virtual Method Invocation)

- 正常的方法调用

```
Person e = new Person();
e.getInfo();
Student e = new Student();
e.getInfo();
```

- 虚拟方法调用(多态情况下)
子类中定义了与父类同名同参数的方法，在多态情况下，将此时父类的方法称为虚拟方法，父类根据赋给它的不同子类对象，动态调用属于子类的该方法。这样的方法调用在编译期是无法确定的。

```
Person e = new Student();
e.getInfo(); //调用Student类的getInfo()方法
```

- 编译时类型和运行时类型
编译时e为Person类型，而方法的调用是在运行时确定的，所以调用的是Student类的getInfo()方法。——动态绑定

#### 虚拟方法调用举例：

![](C:/NoteBook/pictures/200711710220967.png =621x)

- 前提：
   - Person类中定义了welcome()方法，各个子类重写了welcome()。
- 执行：
   - 多态的情况下，调用对象的welcome()方法，实际执行的是子类重写的方法。

### 方法的重载与重写
1. 二者的定义细节：略
2. 从编译和运行的角度看：
   - 重载，是指允许存在多个同名方法，而这些方法的参数不同。编译器根据方法不同的参数表，对同名方法的名称做修饰。对于编译器而言，这些同名方法就成了不同的方法。它们的调用地址在编译期就绑定了。Java的重载是可以包括父类和子类的，即子类可以重载父类的同名不同参数的方法。
所以：对于重载而言，在方法调用之前，编译器就已经确定了所要调用的方法，这称为“早绑定”或“静态绑定”；而对于多态，只有等到方法调用的那一刻，解释运行器才会确定所要调用的具体方法，这称为“晚绑定”或“动态绑定”。引用一句Bruce Eckel的话：“不要犯傻，如果它不是晚绑定，它就不是多态。”

### 多态小结
- 多态作用：
   - 提高了代码的通用性，常称作接口重用
- 前提：
   - 需要存在继承或者实现关系
   - 有方法的重写
- 成员方法：
   -  编译时：要查看引用变量所声明的类中是否有所调用的方法。 
   - 运行时：调用实际new的对象所属的类中的重写方法。
- 成员变量：
   - 不具备多态性，只看引用变量所声明的类。
   
### instanceof 操作符
- x instanceof A：检验x是否为类A的对象，返回值为boolean型。
    - 要求x所属的类与类A必须是子类和父类的关系，否则编译错误。
    - 如果x属于类A的子类B，x instanceof A值也为true。
- 为了避免在向下转型时出现ClassCastException的异常，在向下转型之前，先进行instanceof的判断，一旦返回true，就进行向下转型。如果返回false，不进行向下转型。

```
public class Person extends Object {…}
public class Student extends Person {…}
public class Graduate extends Person {…}
-------------------------------------------------------------------
public void method1(Person e) {
    if (e instanceof Person) 
        // 处理Person类及其子类对象
    if (e instanceof Student) 
        //处理Student类及其子类对象
    if (e instanceof Graduate)
        //处理Graduate类及其子类对象
}
```

### 对象类型转换 (Casting )
- 基本数据类型的Casting：
   - 自动类型转换：小的数据类型可以自动转换成大的数据类型
     - 如long g=20; double d=12.0f
   - 强制类型转换：可以把大的数据类型强制转换(casting)成小的数据类型
     - 如 float f=(float)12.0; int a=(int)1200L
- 对Java对象的强制类型转换称为造型
   - 从子类到父类的类型转换可以自动进行
   - 从父类到子类的类型转换必须通过造型(强制类型转换)实现
   - 无继承关系的引用类型间的转换是非法的
   - 在造型前可以使用instanceof操作符测试一个对象的类型

**向下转型**

使用子类中特有的属性和方法

![](C:/NoteBook/pictures/381672710239393.png =608x)

#### 例

```
package com.zjk;

public class PersonTest {
	public static void main(String[] args) {

		Person p1 = new Person();
		p1.eat();

		Man man = new Man();
		man.eat();
		man.age = 25;
		man.earnMoney();

		System.out.println("-------------");

//		对象的多态性: 父类的引用指向子类的对象(子类的对象赋给父类引用)
		Person p2 = new Man();

//		多态的使用：当调用子父类同名同参的方法时，实际执行的是子类重写父类的方法
//		--虚拟方法调用
		p2.eat();
		p2.walk();

//		p2.earnMoney(); 
//		编译时，看左边：Person p2  只能是Person类中声明的
//		执行时，看右边：new Man()  实际执行的是Man类里重写的

		System.out.println(p2.id);
//		1001 输出的是父类中定义的属性
//		对象的多态性只适用于方法，不适用于属性(属性的编译和运行都看左边)
		
		Man m1 = (Man)p2;  //向下转型
		m1.earnMoney();
		m1.isSmoking = true;
		
//		Woman w1 = (Woman) p2; //此时p2为man类型强转为Woman类型，并不是子父类的关系
//		w1.goShopping();
//		使用强制向下转型可能：Exception in thread "main" java.lang.ClassCastException
		
		
//		为了避免在向下转型时出现ClassCastException的异常，
//		在向下转型之前，先进行instanceof的判断，一旦返回true，就进行向下转型。如果返回false，不进行向下转型。
		if(p2 instanceof Woman) {
			Woman w1 = (Woman)p2;
			w1.goShopping();
			System.out.println("****Woman****");
		}
		if(p2 instanceof Man) {
			Man m2 = (Man)p2;
			m1.earnMoney();
			System.out.println("****Man****");
		}
	}
}

class Person {
	String name;
	int age;
	int id = 1001;

	public void eat() {
		System.out.println("吃饭");
	}

	public void walk() {
		System.out.println("走路");
	}
}

class Man extends Person {

	boolean isSmoking;
	int id = 1002;

	public void earnMoney() {
		System.out.println("男人负责赚钱养家");
	}

	@Override
	public void eat() {
		// TODO Auto-generated method stub
		System.out.println("多吃长肉");
	}

	@Override
	public void walk() {
		// TODO Auto-generated method stub
		System.out.println("帅气走路");
	}
}

class Woman extends Person {

	boolean isBeauty;

	public void goShopping() {
		System.out.println("女生喜欢购物");
	}

	@Override
	public void eat() {
		// TODO Auto-generated method stub
		System.out.println("女生少吃，减肥");
	}

	@Override
	public void walk() {
		// TODO Auto-generated method stub
		System.out.println("走路窈窕");
	}
}
```

#### 对象类型转换举例

```
public class ConversionTest {
	public static void main(String[] args) {
		double d = 13.4;
		long l = (long) d;
		System.out.println(l);
		int in = 5;
// boolean b = (boolean)in;
		Object obj = "Hello";
		String objStr = (String) obj;
		System.out.println(objStr);
		Object objPri = new Integer(5);
// 所以下面代码运行时引发ClassCastException异常
		String str = (String) objPri;
	}
}
```

```
public class Test {
	public void method(Person e) { // 设Person类中没有getschool() 方法
        // System.out.pritnln(e.getschool()); //非法,编译时错误
		if (e instanceof Student) {
			Student me = (Student) e; // 将e强制转换为Student类型
			System.out.pritnln(me.getschool());
		}
	}

	public static void main(String[] args) {
		Test t = new Test();
		Student m = new Student();
		t.method(m);
	}
}
```

#### 练习
##### 问题一：编译时通过，运行时不通过

```
Person p3 = new Woman();
Man m3 = (Man)p3; 
```

```
Person p4 = new Person();
Man m4 = (Man)p4;
//Man.eranMoney();
```

##### 问题二：编译时通过，运行时也通过

```
Object obj = new Woman();
Person p = (Person)obj;
```

##### 问题三：编译不通过

```
Man m5 = new Woman();

String str = new Date();
```
#### 练习：继承成员变量和继承方法的区别

```
package com.zjk;

class Base {
	int count = 10;

	public void display() {
		System.out.println(this.count);
	}
}

public class FieldMethodTest {
	public static void main(String[] args) {
		Sub s = new Sub();
		System.out.println(s.count); //20
		s.display(); //20
		
		Base b = s;
//      == 对于引用数据类型，比较的是两个引用数据类型的地址值是否相同		
		System.out.println(b == s); //true
		
		System.out.println(b.count); //10
//      对象的多态性不适用于属性
//      调用Base类的count
		
        b.display(); //20
//		虚拟方法调用，调用Sub中的display()方法，输出Sub中的count
	}
}

class Sub extends Base {
	int count = 20;

	public void display() {
		System.out.println(this.count);
	}
}
```

##### 区别

1. 若子类重写了父类方法，就意味着子类里定义的方法彻底覆盖了父类里的同名方法。系统不可以把父类里的方法转移到子类中。编译看左边，运行看右边。
2. 对于实例变量则不存在这样的现象，即使子类里定义了与父类完全相同的实例变量，这个实例变量依然不可能覆盖父类中定义的实例变量。编译运行都看左边。


### 子类继承父类
- 若子类重写了父类方法，就意味着子类里定义的方法彻底覆盖了父类里的同名方法，系统将不可能把父类里的方法转移到子类中。
- 对于实例变量则不存在这样的现象，即使子类里定义了与父类完全相同的实例变量，这个实例变量依然不可能覆盖父类中定义的实例变量

#### 练习5

![](C:/NoteBook/pictures/586652910227260.png =698x)

```
package com.zjk;

public class Person {
	protected String name = "person";
	protected int age = 50;

	public String getInfo() {
		return "Name:" + name + "\nage:" + age;
	}
}

class Student extends Person {
	protected String school = "pku";

	public String getInfo() {
		return "name:" + name + "\nage:" + age + "\nschool" + school;
	}
}

class Graduate extends Student {
	public String major = "IT";

	@Override
	public String getInfo() {
		// TODO Auto-generated method stub
		return super.getInfo() + "\nmajor" + major;
	}
}

package com.zjk;

public class InstanceTest {
	public static void main(String[] args) {
		Person p1 = new Person();
		Student s1 = new Student();
		Graduate g1 = new Graduate();
		
		InstanceTest test = new InstanceTest();
		test.method(p1);
		test.method(s1);
		test.method(g1);
	}

	public void method(Person e) {
		e.getInfo();

		if (e instanceof Person) {
			System.out.println("a Person");
		}
		if (e instanceof Student) {
			System.out.println("a Student");
			System.out.println("a Person");
		}
		if (e instanceof Graduate) {
			System.out.println("a graduate student");
			System.out.println("a Student");
			System.out.println("a Person");
		}
	}
}
```

#### 练习6

定义三个类，父类GeometricObject代表几何形状，子类Circle代表圆形，MyRectangle代表矩形。

定义一个测试类GeometricTest，编写equalsArea方法测试两个对象的面积是否相等（注意方法的参数类型，利用动态绑定技术），编写displayGeometricObject方法显示对象的面积（注意方法的参数类型，利用动态绑定技术）。

![](C:/NoteBook/pictures/175993110247426.png =650x)


```
package com.zjk;

/*
 * 定义一个测试类GeometricTest，编写equalsArea方法测试两个对象的面积是否相等（注意方法的参数类型，利用动态绑定技术）
 * 编写displayGeometricObject方法显示对象的面积（注意方法的参数类型，利用动态绑定技术）。
 */

public class GeometricTest {
	public static void main(String[] args) {
		Circle c1 = new Circle("red",2,3);
		MyRectangle r1 = new MyRectangle("red",2,3,4);
		
		GeometricTest test = new GeometricTest();
		System.out.println(test.equalsArea(c1,r1));
		
		test.displayGeometricObject(c1);
		test.displayGeometricObject(r1);
	}
	
	public boolean equalsArea(GeometricObject o1,GeometricObject o2) {
		return o1.findArea() == o2.findArea();
	}
	
	public void displayGeometricObject(GeometricObject o) {
		System.out.println("面积：" + o.findArea());
	}
}


class GeometricObject{
	protected String color;
	protected double weight;
	
	protected GeometricObject(String color,double weight) {
		this.color = color;
		this.weight = weight;
	}

	public String getColor() {
		return color;
	}

	public double getWeight() {
		return weight;
	}

	public void setColor(String color) {
		this.color = color;
	}

	public void setWeight(double weight) {
		this.weight = weight;
	}
	
	public double findArea() {
		return 0.0;
	}
}

class Circle extends GeometricObject{
	private double radius;

	public Circle(String color,double weight,double radius) {
		super(color,weight);
		this.radius = radius;
	}
	
	
	public double getRadius() {
		return radius;
	}


	public void setRadius(double radius) {
		this.radius = radius;
	}


	public double findArea() {
		return radius * radius * Math.PI;
	}
}

class MyRectangle extends GeometricObject{
	private double width;
	private double height;
	
	public MyRectangle(String color,double weight,double width, double height) {
		super(color,weight);
		this.width = width;
		this.height = height;
	}
	
	public double findArea() {
		// TODO Auto-generated method stub
		return width * getWeight();
	}
}
```

### 面试题：
多态是编译时行为还是运行时行为？如何证明？

#### 证明见：InterviewTest.java
```
package com.atguigu.test;

import java.util.Random;

//面试题：多态是编译时行为还是运行时行为？
//证明如下：
class Animal  {
 
	protected void eat() {
		System.out.println("animal eat food");
	}
}

class Cat  extends Animal  {
 
	protected void eat() {
		System.out.println("cat eat fish");
	}
}

class Dog  extends Animal  {
 
	public void eat() {
		System.out.println("Dog eat bone");

	}

}

class Sheep  extends Animal  {
 

	public void eat() {
		System.out.println("Sheep eat grass");

	}

 
}

public class InterviewTest {

	public static Animal  getInstance(int key) {
		switch (key) {
		case 0:
			return new Cat ();
		case 1:
			return new Dog ();
		default:
			return new Sheep ();
		}

	}

	public static void main(String[] args) {
		int key = new Random().nextInt(3);

		System.out.println(key);

		Animal  animal = getInstance(key);
		
		animal.eat();
		 
	}

}
```
#### 拓展问题：InterviewTest1.java

```
package com.atguigu.test;
//考查多态的笔试题目：
public class InterviewTest1 {

	public static void main(String[] args) {
		Base base = new Sub();
		base.add(1, 2, 3);

//		Sub s = (Sub)base;
//		s.add(1,2,3);
	}
}

class Base {
	public void add(int a, int... arr) {
		System.out.println("base");
	}
}

class Sub extends Base {

	public void add(int a, int[] arr) { //编译器认为int[] arr 与 int...arr相同; 是重写
		System.out.println("sub_1");
	}

//	public void add(int a, int b, int c) {
//		System.out.println("sub_2");
//	}

}
```

### Object类的使用

- Object类是所有Java类的根父类
     - 如果在类的声明中未使用extends关键字指明其父类，则默认父类为java.lang.Object类

```
package com.zjk;

public class ObjectTest {

	public static void main(String[] args) {
		Order order = new Order();
		System.out.println(order.getClass().getSuperclass());
	}
}

class Order {

}
```

```
public class Person {
    ...
}

等价于：
public class Person extends Object {
    ...
}
```

- 数组也可以作为Object类的子类出现，可以调用Object类中声明的方法

```
@Test
public void test1() {
	int[] arr = new int[] {1,2,3};
	print(arr);
	
	System.out.println(arr.getClass());
	System.out.println(arr.getClass().getSuperclass());
}

public void print(Object obj) {
	System.out.println(obj);
}
```

例：
```
method(Object obj){…} //可以接收任何类作为其参数
Person o=new Person();
method(o);
```
![](C:/NoteBook/pictures/26161023220968.png =516x)

#### ==操作符与equals方法

##### ==
- 基本类型比较值:只要两个变量的值相等，即为true。(不一定类型相等)
- 引用类型比较引用(是否指向同一个对象)：只有指向同一个对象时，==才返回true。
- 用“==”进行比较时，符号两边的数据类型必须兼容(可自动转换的基本数据类型除外)，否则编译出错

```
package com.zjk;

public class EqualsTest {
	public static void main(String[] args) {

//		基本数据类型的== ************************
		int i = 10;
		int j = 10;
		double d = 10.0;
		System.out.println(i == j);// true
		System.out.println(i == d);// true

		boolean b = true;
//		System.out.println(i == b);编译不通过

		char c = 10;
		System.out.println(i == c);// true

		char c1 = 'A';
		char c2 = 65;
		System.out.println(c1 == c2);// true

//		引用数据类型的== ************************
		Customer cust1 = new Customer("Tom", 21);
		Customer cust2 = new Customer("Tom", 21);
		System.out.println(cust1 == cust2); // false

		Customer cust3 = cust1;
		System.out.println(cust3 == cust1); //true
		
		String str1 = new String("atguigu");
		String str2 = new String("atguigu");
		System.out.println(str1 == str2); // false
	}
}
```

##### equals()：
- 所有类都继承了Object，也就获得了equals()方法。还可以重写。
- 只能比较引用类型，Object类中的equals()其作用和定义与“==”相同,比较是否指向同一个对象。
- Object类中equals()的定义

```
public boolean equals(Object obj){
    return (this == obj);
}
```
- 格式:obj1.equals(obj2)
-  特例：当用equals()方法进行比较时，对类File、String、Date及包装类（Wrapper Class）来说，是比较类型及“实体内容”而不考虑引用的是否是同一对象；
    - 原因：在这些类中重写了Object类的equals()方法。
- 当自定义使用equals()时，可以重写。用于比较两个对象的“内容”是否都相等

```
package com.zjk;

import java.util.Date;

public class EqualsTest {
	public static void main(String[] args) {

//		引用数据类型的== ************************
		Customer cust1 = new Customer("Tom", 21);
		Customer cust2 = new Customer("Tom", 21);
		System.out.println(cust1 == cust2); // false

		Customer cust3 = cust1;
		System.out.println(cust3 == cust1); // true

		String str1 = new String("atguigu");
		String str2 = new String("atguigu");
		System.out.println(str1 == str2); // false

//		equals() *******************************
		System.out.println(cust1.equals(cust2)); // false
		// Object类中的equals()未重写
		System.out.println(str1.equals(str2)); // true
		// String类中的equals()已经重写

		Date date1 = new Date(32432525324L);
		Date date2 = new Date(32432525324L);
		System.out.println(date1.equals(date2)); // true
		
	}
}
```

##### 重写equals()方法的原则

通常情况下，我们自定义的类如果使用equals()的话，也通常是比较这两个对象的“实体内容”是否相同，那么，我们就需要对Object类中的equals()进行重写。
- **比较两个对象的实体内容是否相同**
- 对称性：如果x.equals(y)返回是“true”，那么y.equals(x)也应该返回是“true”。
- 自反性：x.equals(x)必须返回是“true”。
- 传递性：如果x.equals(y)返回是“true”，而且y.equals(z)返回是“true”，那么z.equals(x)也应该返回是“true”。
- 一致性：如果x.equals(y)返回是“true”，只要x和y内容一直不变，不管你重复x.equals(y)多少次，返回都是“true”。
- 任何情况下，x.equals(null)，永远返回是“false”；x.equals(和x不同类型的对象)永远返回是“false”。


- String类中的equals()重写

![](C:/NoteBook/pictures/233225614220969.png =616x)

```
package com.zjk;

public class Customer {

	private String name;
	private int age;

	public Customer() {

	}

	public Customer(String name, int age) {
		super();
		this.name = name;
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}

	public void setName(String name) {
		this.name = name;
	}

	public void setAge(int age) {
		this.age = age;
	}

	@Override
	public boolean equals(Object obj) {
		// TODO Auto-generated method stub
		if (this == obj) {
			return true;
		}
		if (obj instanceof Customer) { //只要是子类就true，有一定的漏洞
			Customer cust = (Customer) obj;
//			if (this.age == cust.age && this.name == cust.name) {
//          遇到Customer cust = new Customer(new String("BB"),12) ;== 有误
			// name是String引用数据类型，应该使用equals()

//			if (this.age == cust.age && this.name.equals(cust.name)) {
//				return true;
//			}

			return this.age == cust.age && this.name.equals(cust.name);
		} else {
			return false;
		}
	}
}


package com.zjk;

import java.util.Date;

public class EqualsTest {
	public static void main(String[] args) {

//		引用数据类型的== ************************
		Customer cust1 = new Customer("Tom", 21);
		Customer cust2 = new Customer("Tom", 21);
		System.out.println(cust1 == cust2); // false

//		重写Customer类的equals() *******************************
		System.out.println(cust1.equals(cust2)); // true		
	}
}
```

###### 手写的存在的漏洞

```
	@Override
	public boolean equals(Object obj) {
		// TODO Auto-generated method stub
		if (this == obj) {
			return true;
		}
		if (obj instanceof Customer) {
			Customer cust = (Customer) obj;
			return this.age == cust.age && this.name.equals(cust.name);
		} else {
			return false;
		}
	}
```

- 子类

###### 自动生成的equals() //eclipse快捷键

```
	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Customer other = (Customer) obj;
		return age == other.age && Objects.equals(name, other.name);
	}
```

##### 面试题：==和equals的区别

![](C:/NoteBook/pictures/383161423227261.png =737x)

##### 练 习7

```
int it = 65;
float fl = 65.0f;
System.out.println(“65和65.0f是否相等？” + (it == fl)); //true

char ch1 = 'A'; char ch2 = 12;
System.out.println("65和'A'是否相等？" + (it == ch1));//true
System.out.println(“12和ch2是否相等？" + (12 == ch2));//true

String str1 = new String("hello");
String str2 = new String("hello");
System.out.println("str1和str2是否相等？"+ (str1 == str2));//false

System.out.println("str1是否equals str2？"+(str1.equals(str2)));//true

System.out.println(“hello” == new java.util.Date()); //编译不通过
```

##### 练 习8
###### 1.
编写Order类，有int型的orderId，String型的orderName，相应的getter()和setter()方法，两个参数的构造器，重写父类的equals()方法：public boolean equals(Object obj)，并判断测试类中创建的两个对象是否相等。

```
package com.zjk;

public class Order {

	int orderId;
	String orderName;

	public int getOrderId() {
		return orderId;
	}

	public String getOrderName() {
		return orderName;
	}

	public void setOrderId(int orderId) {
		this.orderId = orderId;
	}

	public void setOrderName(String orderName) {
		this.orderName = orderName;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj) {
			return true;
		}

		if (this instanceof Order) {
			Order order = (Order) obj;
			return this.orderId == order.orderId && this.orderName.equals(order.orderName);
		} else {
			return false;
		}
	}
}
```

###### 

请根据以下代码自行定义能满足需要的MyDate类,在MyDate类中覆盖equals方法，使其判断当两个MyDate类型对象的年月日都相同时，结果为true，否则为false。 public boolean equals(Object o)

```
package com.zjk;

public class MyDate {
	private int day;
	private int month;
	private int year;

	public int getDay() {
		return day;
	}

	public int getMonth() {
		return month;
	}

	public int getYear() {
		return year;
	}

	public void setDay(int day) {
		this.day = day;
	}

	public void setMonth(int month) {
		this.month = month;
	}

	public void setYear(int year) {
		this.year = year;
	}

	@Override
	public boolean equals(Object obj) {
		// TODO Auto-generated method stub
		if (this == obj) {
			return true;
		}
		if (this instanceof MyDate) {
			MyDate m = (MyDate) obj;
			return this.day == m.day && this.month == m.month && this.year == m.year;
		} else {
			return false;
		}
	}
}
```

#### toString() 方法

- 当我们输出一个对象的引用时，实际上就是调用当前对象的toString()

- toString()方法在Object类中定义，其返回值是String类型，返回类名和它的引用地址。
- Object类中的toString()的定义

```
public String toString(){  
    return getClass().getName + "@" + Integer.toHexStrign(hashCode());
}

//Java中的地址值并不是真实的物理地址值，而是hashCode算出来的虚拟地址值
```

- 像String/Date/File/包装类等都重写了Object类中的toString()
    - 使得在调用对象的toString()时，返回“实体内容”信息
  
```
package com.zjk;

import java.util.Date;

public class ToStringTest {
	public static void main(String[] args) {
//      未重写，输出地址值
		Customer cust1 = new Customer("Tom", 21);
		System.out.println(cust1.toString());
		// com.zjk.Customer@1d8d30f7
		System.out.println(cust1);
		// com.zjk.Customer@1d8d30f7

//		重写过
		String str = new String("YY");
		System.out.println(str); // YY

		Date date = new Date(131231L);
		System.out.println(date.toString());// Thu Jan 01 08:02:11 CST 1970
	}
}
```
  
- 自定义类也可以重写toString()方法，当调用此方法时，返回对象的“实体内容”

```
package com.zjk;

import java.util.Objects;

public class Customer {

	private String name;
	private int age;

	public Customer() {

	}

	public Customer(String name, int age) {
		super();
		this.name = name;
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}

	public void setName(String name) {
		this.name = name;
	}

	public void setAge(int age) {
		this.age = age;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Customer other = (Customer) obj;
		return age == other.age && Objects.equals(name, other.name);
	}
	
	@Override
	public String toString() {
		// TODO Auto-generated method stub
		return "Customer[name = " + name + ",age = " + age + "]";
	}
}


package com.zjk;

public class ToStringTest {
	public static void main(String[] args) {
		Customer cust1 = new Customer("Tom", 21);
		System.out.println(cust1.toString());
		//Customer[name = Tom,age = 21]
		System.out.println(cust1);
		//Customer[name = Tom,age = 21]
	}
}

```
- 在进行String与其它类型数据的连接操作时，自动调用toString()方法

```
Date now=new Date();
System.out.println(“now=”+now); 相当于
System.out.println(“now=”+now.toString());
```

- 可以根据需要在用户自定义类型中重写toString()方法

```
如String 类重写了toString()方法，返回字符串的值。
s1=“hello”;
System.out.println(s1);//相当于System.out.println(s1.toString());
```

- 基本类型数据转换为String类型时，调用了对应包装类的toString()方法

```
int a=10; System.out.println(“a=”+a);
```

##### 面试题 只有char[]数组输出字符串，其他数组都是地址值 

```
public class MyDate {
	public static void main(String[] args) {
		MyDate m1 = new MyDate();
		m1.test();

	}

	public void test() {
		char[] arr = new char[] { 'a', 'b', 'c' };
		System.out.println(arr); //abc
		
		int[] arr1 = new int[] { 1, 2, 3 };
		System.out.println(arr1); //[I@37d31475
		
		double[] arr2 = new double[] { 1.1, 2.2, 3.3 };
		System.out.println(arr2); //[D@27808f31
	}
}
```

##### 当null.toString()时，空指针异常NullPointException

```
@Test
public void test2() {
	String s = "abc";
	System.out.println(s); //abc
	
	s = null;
	System.out.println(s); //null
	
	System.out.println(s.toString()); //当null.toString()时，
	//空指针异常NullPointException	
}
```

#### 练习9
定义两个类，父类GeometricObject代表几何形状，子类Circle代表圆形。

![](C:/NoteBook/pictures/284831823236651.png =459x)

![](C:/NoteBook/pictures/419631823232405.png =554x)

```
package com.zjk;

import java.util.Objects;

public class GeometricObject {
	protected String color;
	protected double weight;

	protected GeometricObject() {
		color = "white";
		weight = 1.0;

	}

	protected GeometricObject(String color, double weight) {
		this.color = color;
		this.weight = weight;
	}

	public String getColor() {
		return color;
	}

	public double getWeight() {
		return weight;
	}

	public void setColor(String color) {
		this.color = color;
	}

	public void setWeight(double weight) {
		this.weight = weight;
	}

}

class Circle extends GeometricObject {
	private double radius;

	public Circle() {
		super();
		radius = 1.0;
	}

	public Circle(double radius) {
		this.radius = radius;
	}

	public Circle(double radius, String color, double weight) {
		super(color, weight);
		this.radius = radius;
	}

	public double getRadius() {
		return radius;
	}

	public void setRadius(double radius) {
		this.radius = radius;
	}

	public double findArea() {
		return radius * radius * Math.PI;
	}

	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Circle other = (Circle) obj;
		return Double.doubleToLongBits(radius) == Double.doubleToLongBits(other.radius);
	}

	@Override
	public String toString() {
		return "Circle [radius=" + radius + "]";
	}
}


package com.zjk;

public class GeometricObjectTest {
	public static void main(String[] args) {
		Circle c1 = new Circle();
		Circle c2 = new Circle();
		System.out.println();

		GeometricObjectTest test = new GeometricObjectTest();
		test.colorTest(c1, c2);

		if (c1.equals(c2)) {
			System.out.println("面积相等");
		} else {
			System.out.println("面积不相等");
		}

		System.out.println(c1.toString());
		System.out.println(c2);
	}

	public void colorTest(Circle c1, Circle c2) {
		if (c1.getColor().equals(c2.getColor())) {
			System.out.println("颜色相同: " + c1.getColor());
		} else {
			System.out.println("颜色不相同:" + c1.getColor() + "  " + c2.getColor());
		}
	}
}
```

### JUnit单元测试

1. 右击当前项目 - build path - add libraries - JUnit5 

![](C:/NoteBook/pictures/380741100220970.png =454x)

![](C:/NoteBook/pictures/146601300239396.png =409x)


![](C:/NoteBook/pictures/154351400227263.png =189x)

2. 创建一个java类，进行单元测试
   - 此时的Java类要求：
       - 此类是public的
       - 此类提供公共的无参的构造器 
3. 此类中声明单元测试方法
   - 此时的单元测试方法：方法的权限是public，没有返回值，没有形参
4. 此单元测试方法上需要声明注释：`@Test`，并在单元测试类中导入: `import org.junit.Test`
5. 声明号单元测试方法以后，就可以在方法体内测试相关的代码
6. 写完代码以后：左键双击方法，右键 Run as - JUnit Test

```
@Test
public void testMehod(){
    ...
}
```

![](C:/NoteBook/pictures/47712200247429.png =522x)

- 一次只执行一个选中的单元测试方法

- 如果执行结果没有任何异常：

```
package com.zjk2;

import java.util.Date;

import org.junit.Test;

public class JUnitTest {

	@Test
	public void testEquals() {
		String s1 = "EE";
		String s2 = "EE";
		System.out.println(s1.equals(s2));
	}
}
```

![](C:/NoteBook/pictures/359272300240098.png =190x)

- 如果执行结果出现异常:

```
package com.zjk2;

import java.util.Date;

import org.junit.Test;

public class JUnitTest {

	@Test
	public void testEquals() {
		String s1 = "EE";
		String s2 = "EE";
		System.out.println(s1.equals(s2));
		
		Object obj = new String("GG");
		Date date = (Date)obj; //类型转换异常
	}
}
```

![](C:/NoteBook/pictures/561952400236653.png =185x)

### 包装类的使用

- 针对八种基本数据类型定义相应的引用类型—包装类（封装类）
- 有了类的特点，就可以调用类中的方法，Java才是真正的面向对象

![](C:/NoteBook/pictures/551802023250285.png =367x)

- 基本数据类型包装成包装类的实例 ---装箱
    - 通过包装类的构造器实现：

```
int i = 500; Integer t = new Integer(i);
```

   - 还可以通过字符串参数构造包装类对象：

```
Float f = new Float(“4.56”);
Long l = new Long(“asdf”); //NumberFormatException
```

- 获得包装类对象中包装的基本类型变量 ---拆箱
- 调用包装类的.xxxValue()方法：

```
boolean b = bObj.booleanValue();
```

- JDK5.0之后，支持自动装箱，自动拆箱。但类型必须匹配。

![](C:/NoteBook/pictures/361952223247889.png =102x)

- 字符串转换成基本数据类型
1. 通过包装类的构造器实现：

```
int i = new Integer(“12”);
```

2. 通过包装类的parseXxx(String s)静态方法：

```
Float f = Float.parseFloat(“12.1”);
```

- 基本数据类型转换成字符串
1.  调用字符串重载的valueOf()方法：

```
String fstr = String.valueOf(2.34f);
```

2. 更直接的方式：

```
String intStr = 5 + ""
```

![](C:/NoteBook/pictures/10672423226632.png =132x)

#### 基本类型、包装类与String类间的转换

![](C:/NoteBook/pictures/521452623244208.png =762x)

```
package com.zjk2;

import org.junit.jupiter.api.Test;

public class WrapperTest {
	
//	基本数据类型-->包装类 调用包装类的构造器
	@Test
	public void test1() {
		
		int num1 = 10;
		Integer in1 = new Integer(num1);
		System.out.println(in1.toString());
		
		Integer in2 = new Integer("123");
		System.out.println(in2.toString());
		
//		Integer in3 = new Integer("123abc");
//		System.out.println(in3.toString());
//		报错
		
		Float f1 = new Float(12.3f);
		Float f2 = new Float("12.3");
		System.out.println(f1);
		System.out.println(f2);
		
		Float f3 = new Float("12.3f");  
//		不报错，float数据类型需要在后面加f
		System.out.println(f3);
		
		Boolean b1 = new Boolean(true);
		Boolean b2 = new Boolean("true");
		Boolean b3 = new Boolean("TRUE");
		System.out.println(b1);
		System.out.println(b2);
		System.out.println(b3);
		
//		Boolean类：源码中true自动转大小写，不是true的即false
		Boolean b4 = new Boolean("true123");
		System.out.println(b4); //false
		Boolean b5 = new Boolean("");
		System.out.println(b5); //false
		
//		Boolean b6 = new Boolean(); //报错
		
		Order order = new Order();
		System.out.println(order.isMale); //false
		System.out.println(order.isFemale); //null
//		isMale是boolean基本数据类型，默认false，
//		而isFemale是Boolean包装类，默认null
	}
	
//	包装类-->基本数据类型,调用包装类的xxxValue()
	@Test
	public void test2() {
		Integer in1 = new Integer(12);
//		包装类不能进行简单的加减乘除运算
		int i1 = in1.intValue();
		System.out.println(i1 + 1);
		
		Float f1 = new Float(12.3);
		float f2 = f1.floatValue();
		System.out.println(f2);
	}
	
//	JDK5.0之后，支持自动装箱，自动拆箱。但类型必须匹配。

	@Test
	public void test3() {
			
//		自动装箱 --基本数据类型的变量直接赋值给包装类的对象
		int num2 = 10;
		Integer in1 = num2; //自动装箱
		Integer in = 10;
		
		boolean b1 = true;
		Boolean b2 = b1; //自动装箱
		Boolean b = true;

//		自动拆箱
		System.out.println(in1);
		
		int num3 = in1;
	}
	
//	基本数据类型,包装类-->String
	@Test
	public void test4(){

//      基本数据类型-->String
		int num1 = 10;
//		方式1
		String str1 = num1 + "";
//		方式2 调用String类的valueOf()方法 
//		基本数据类型和包装类均可使用
		float f1 = 12.3f;
		String str2 = String.valueOf(f1); //"12.3"
		System.out.println(str2);
		
		Double d1 = new Double(12.3);
		String str3 = String.valueOf(d1);
		System.out.println(str3);
	}
	
//	String类型-->基本数据类型，包装类
//	调用包装类的parseXxx()
	@Test
	public void test5() {
		String str1 = "123";
//		int num1 = (int)str;
//		Integer in1 = (Integer)str1;
//		无关系的类之间强转报错
		
//		可能会报NumberFormatExcption
		int num2 = Integer.parseInt(str1);
		System.out.println(num2 + 1);
		
		String str2 = "true";
		Boolean b1 = Boolean.parseBoolean(str2);
		System.out.println(b1); //true
	}
}


class Order{
	boolean isMale;
	Boolean isFemale;
}
```
#### 包装类用法举例

```
int i = 500;
Integer t = new Integer(i);
装箱：包装类使得一个基本数据类型的数据变成了类。
有了类的特点，可以调用类中的方法。
String s = t.toString(); // s = “500“,t是类，有toString方法
String s1 = Integer.toString(314); // s1= “314“ 将数字转换成字符串。
String s2=“4.56”;
double ds=Double.parseDouble(s2); //将字符串转换成数字

拆箱：将数字包装类中内容变为基本数据类型。
int j = t.intValue(); // j = 500，intValue取出包装类中的数据

包装类在实际开发中用的最多的在于字符串变为基本数据类型。
String str1 = "30" ;
String str2 = "30.3" ;
int x = Integer.parseInt(str1) ; // 将字符串变为int型
float f = Float.parseFloat(str2) ; // 将字符串变为float型
```

#### 面试题 

如下两个题目的输出结果；各是什么？

##### 三元运算符
```
public class Test {
	public static void main(String[] args) {
		
		Object o1 = true ? new Integer(1) : new Double(2.0);
//		三元运算符 ?: 要求同一类型，Integer类型自动提升为Double类型
		System.out.println(o1); //1.0
		
		Object o2;
		if(true) {
			o2 = new Integer(1);
		}
		else {
			o2 = new Double(2.0);
		}
		System.out.println(o2); //1
	}
	
}
```

##### Integer内部定义了IntegerCache结构，IntegerCache中定义了数组Integer[],保存了`-128~127`范围的整数。如果我们使用自动装箱的方法，给Integer赋值的范围在`-128~127`时，可以直接使用数组中的元素，不用再去new了。目的：提高效率。

```
public class Test {
	public static void main(String[] args) {
		Test test = new Test();
		test.method();
	}
	
	public void method() {
		Integer i = new Integer(1);
		Integer j = new Integer(1);
		System.out.println(i == j); //false
		//比较地址值
		
		Integer m = 1;
		Integer n = 1;
		System.out.println(m == n); //true
		//自动装箱

		Integer x = 128;  //相当于new了一个Integer对象
		Integer y = 128;  //相当于new了一个Integer对象
		System.out.println(x == y); //false
		
        //Integer包装类中的IntegerCache结构，
		//提前造好了-128~127的int值
		//自动装箱时，赋值在-128~127之间的，直接取造好的int值，所以m和n地址值相同
		//超出范围的则new一个int值，新建地址值，所以x和y地址值不相同
	}
}
```

### 练习10
利用Vector代替数组处理：从键盘读入学生成绩（以负数代表输入结束），找出最高分，并输出学生成绩等级。

提示：数组一旦创建，长度就固定不变，所以在创建数组前就需要知道它的长度。而向量类java.util.Vector可以根据需要动态伸缩。

- 创建Vector对象：Vector v=new Vector();
- 给向量添加元素：v.addElement(Object obj); //obj必须是对象
- 取出向量中的元素：Object obj=v.elementAt(0);
    - 注意第一个元素的下标是0，返回值是Object类型的。
- 计算向量的长度：v.size();
- 若与最高分相差10分内：A等；20分内：B等；30分内：C等；其它：D等

```
package com.zjk2;

import java.util.Scanner;
import java.util.Vector;

public class ScoreTest {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);

		Vector v = new Vector();

		int maxScore = 0;

		for (;;) {
//			输入成绩
			System.out.println("请输入学生成绩（负数结束）");
			int score = scan.nextInt();
			if (score < 0) {
				break;
			}
			if (score > 100) {
				System.out.println("数据有误，重新输入");
				continue;
			}
////		jdk5.0之前添加对象
//			Integer inScore = new Integer(score);
//			v.addElement(inScore);
			v.addElement(score); // 自动装箱
//			获取最大值(使用基本数据类型比）
			if (maxScore < score) {
				maxScore = score;
			}
		}
//		评级
		char level;
		for (int i = 0; i < v.size(); i++) {
			Object obj = v.elementAt(i);
//			jdk5.0之前
//			Integer inScore = (Integer) obj;
//			int score = inScore.intValue();
//			jdk5.0之后
			int score = (int)obj;
			
			if (maxScore - score <= 10) {
				level = 'A';
			} else if (maxScore - score <= 20) {
				level = 'B';
			} else if (maxScore - score <= 30) {
				level = 'c';
			} else {
				level = 'D';
			}
		

			System.out.println("student-" + (i + 1) + " score is " + score + ",level is " + level);
		}
	}
}
```

## 关键字：static

当我们编写一个类时，其实就是在描述其对象的属性和行为，而并没有产生实质上的对象，只有通过new关键字才会产生出对象，这时系统才会分配内存空间给对象，其方法才可以供外部调用。我们有时候希望无论是否产生了对象或无论产生了多少对象的情况下，某些特定的数据在内存空间里只有一份，例如所有的中国人都有个国家名称，每一个中国人都共享这个国家名称，不必在每一个中国人的实例对象中都单独分配一个用于代表国家名称的变量。

- **如果想让一个类的所有实例共享数据，就用类变量！**

### static关键字使用

- 使用范围：
   - 在Java类中，可用static修饰属性、方法、代码块、内部类
- 被修饰后的成员具备以下特点：
   - 随着类的加载而加载
   - 优先于对象存在
   - 修饰的成员，被所有对象所共享
   - 访问权限允许时，可不创建对象，直接被类调用

```
class Circle {
    private double radius;

    public static String name = "这是一个圆";
    

    public static String getName() {
        return name;
    }

    public Circle(double radius) {
        this.radius = radius;
    }

    public double findArea() {
        return Math.PI * radius * radius;
    }

    public void display() {
        System.out.println("name:" + name + "radius:" + radius);
    }
}

public class StaticTest {
    public static void main(String[] args) {
        Circle c1 = new Circle(2.0);
        Circle c2 = new Circle(3.0);
        c1.display();
        c2.display();
    }
}
```

### 用static修饰属性 ，类变量（静态变量）

- 实例变量： 
    - 创建了类的多个对象，每个对象都独立的拥有一套类中的非静态属性，归某个对象所有
    - 当修改其中一个对象中的非静态属性时，不会导致其他对象中同样的属性值的修改
- 静态变量（静态属性）（类变量）
    -  创建了类的多个对象，多个对象共享同一个静态变量
    - 当通过某一个对象修改静态变量时，会导致其他对象调用此静态变量时，是修改过的值。

#### 静态变量的特点：

- 静态变量随着类的加载而加载
   - 可以通过`类.静态变量`的方法进行调用 ，而实例变量不可以
- 优先于对象存在，
- 由于类只加载一次，则静态变量在内存中也只会存在一份，存在方法区的静态域中



#### 类变量 vs 实例变量内存解析

![](C:/NoteBook/pictures/576250900226364.png =731x)


**静态变量的内存解析**

![](C:/NoteBook/pictures/224761000253319.png =692x)


### 类方法（静态方法）

#### 使用static修饰方法，静态方法：

1. 随着类的加载而加载，
   - 可以通过`类.静态方法`的方式进行调用，
2. 静态方法中只能调用静态的方法或属性。(生命周期)
   - 非静态方法中既可以调用静态的也可以调用非静态的方法或属性 
- 在静态方法内不能使用this或super关键字
- static修饰的方法不能被重写

### 在开发中，如何确定一个属性或方法是否声明为static的？

- **类属性、类方法的设计思想**

- 类属性作为该类各个对象之间共享的变量。在设计类时,分析哪些属性不因对象的不同而改变，将这些属性设置为类属性。
   - 操作静态变量的相应的方法设置为类方法。
   - 类中的常量也常常声明为static
- 如果方法与调用者无关，则这样的方法通常被声明为类方法，由于不需要创建对象就可以调用类方法，从而简化了方法的调用。
   - 工具类中的方法，习惯上声明为static，如Math，Array，Collections 
   

### 例1

```
package com.zjk;

public class StaticTest {
	public static void main(String[] args) {
		Chinese.nation = "中国";// 静态变量随着类的加载而加载

		Chinese c1 = new Chinese();
		c1.name = "黎明";
		c1.age = 18;
		c1.nation = "CHN";

		Chinese c2 = new Chinese();
		c2.name = "助手";
		c2.age = 30;

		System.out.println(c1.nation); // CHN
		System.out.println(c2.nation); // CHN

		c2.nation = "CHINA";
		System.out.println(c1.nation); // CHIAN
		System.out.println(c2.nation); // CHINA

		c1.eat();

		Chinese.show();// 静态方法随着类的加载而加载
	}
}

class Chinese {
	String name;
	int age;
	static String nation; // 静态变量

	public void eat() {
		System.out.println("吃饭");
		// 调用非静态结构
		this.info();

		// 调用静态结构
		walk(); // 调用的类的Chinese.walk()
		System.out.println(nation); // 调用的是类的Chinese.nation
	}

	public static void show() { // 静态方法
		System.out.println("中国人");
//		eat(); 静态方法中只能调用静态的方法或属性
		walk();

//		System.out.println(name); //name非静态
		System.out.println(nation); // 不是this.nation
		// 而是Chinese.nation 即类.静态属性
	}

	public static void walk() {
		System.out.println("walk");
	}

	public void info() {
		System.out.println("name:" + name + "\tage: " + age);
	}
}
```

### 例2

```
package com.zjk;

public class CircleTest {
	public static void main(String[] args) {

		Circle c1 = new Circle();
		
		Circle c2 = new Circle();
		
		Circle c3 = new Circle(2.0);
		
		System.out.println("c1_id: " + c1.getId());
		System.out.println("c2_id: " + c2.getId());
		
		System.out.println("total: " + Circle.getTotal());
	}
}

class Circle {

	private double radius;
	private int id;

	public Circle() {
		id = init++;
		total++;
	}
	
	public Circle(double radius) {
		this();
		this.radius = radius;
	}

	private static int total; // 圆的总数
	private static int init = 1001; // 给id赋值
	//static声明的属性被所有对象共享

	public double findArea() {
		return Math.PI * radius * radius;
	}

	public int getId() {
		return id;
	}

	public static int getTotal() {
		return total;
	}
}
```
### 练习1
编写一个类实现银行账户的概念，包含的属性有“帐号”、“密码”、“存款余额”、“利率”、“最小余额”，定义封装这些属性的方法。账号要自动生成。编写主类，使用银行账户类，输入、输出3个储户的上述信息。

考虑：哪些属性可以设计成static属性。

```
package com.zjk;

public class AccountTest {
	public static void main(String[] args) {
		Account a1 = new Account();
		a1.show();

		Account a2 = new Account("123",1000);
		a2.show();
		
		System.out.println(a1);
	}
}

class Account {
	private int customer; // 账号
	private String code = "123456";// 密码
	private double balance; // 余额

	private static double rate = 0.15; // 利率
	private static double minBalance = 30000; // 最小余额
	private static int id = 1001; // 用于生成账号

	public Account() {
		customer = id++;
	}

	public Account(String code, double balance) {
		customer = id++;
		this.code = code;
		this.balance = balance;
	}

	public void show() {
		System.out.println("*************************");
		System.out.println("customer:  \t" + customer);
		System.out.println("balance:   \t" + balance);
		System.out.println("rate:      \t" + rate);
		System.out.println("minBalance:\t" + minBalance);
		System.out.println("*************************");
	}

	public void getMinbalance() {
		System.out.println(minBalance);
	}

	@Override
	public String toString() {
		return "Account [customer=" + customer + ", code=" + code + ", balance=" + balance + "]";
	}
}
```

## 单例 (Singleton)设计模式

- 设计模式是在大量的实践中总结和理论化之后优选的代码结构、编程风格、以及解决问题的思考方式。设计模式免去我们自己再思考和摸索。
- 所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，对某个类只能存在一个对象实例，并且该类只提供一个取得其对象实例的方法。
- 如果我们要让类在一个虚拟机中只能产生一个对象，我们首先必须将类的构造器的访问权限设置为private，这样，就不能用new操作符在类的外部产生类的对象了，但在类内部仍可以产生该类的对象。
- 因为在类的外部开始还无法得到类的对象，只能调用该类的某个静态方法以返回类内部创建的对象，静态方法只能访问类中的静态成员变量，所以，指向类内部产生的该类对象的变量也必须定义成静态的。

### 单例(Singleton)设计模式-饿汉式

```
package com.zjk;

public class SingletonTest1 {
	public static void main(String[] args) {
//		Bank bank1 = new Bank(); 
//		构造器私有化，无法在类外创建对象
		
		Bank bank1 = Bank.getInstance(); 
		Bank bank2 = Bank.getInstance();
		System.out.println(bank1 == bank2); //true
		
	}
}

class Bank{
//	1.私有化类的构造器
	private Bank() {
		
	}
	
//	2.内部创建类的对象
//	要求此对象也必须是静态的
	private static Bank instance = new Bank();
	
//	3.提供公共的静态方法，返回类的对象
	public static Bank getInstance() {
		return instance;
	}
}
```
```
package com.zjk;

public class SingletonTest1 {
	public static void main(String[] args) {
//		Bank bank1 = new Bank(); 
//		构造器私有化，无法在类外创建对象
		
		Bank bank1 = Bank.instance; 
//		Bank.instance = null;
		Bank bank2 = Bank.instance;
		System.out.println(bank1 == bank2); //true
		
	}
}

class Bank{
//	1.私有化类的构造器
	private Bank() {
		
	}
	
//	2.内部创建类的对象
//	要求此对象也必须是静态的
	public final static Bank instance = new Bank();
	//进行final的修改，public
	
//	3.提供公共的静态方法，返回类的对象
	public static Bank getInstance() {
		return instance;
	}
}

class BankTest{
	private static BankTest instance = null;
	static {
		instance = new BankTest();
	}
}
```
```
class BankTest{
	private static BankTest instance = null;
	static {
		instance = new BankTest();
	}
}
```

### 单例(Singleton)设计模式-懒汉式

```
package com.zjk;

public class SingketonTest2 {
	public static void main(String[] args) {
		
		Order o1 = Order.getInstance();
		Order o2 = Order.getInstance();
		System.out.println(o1 == o2);
	}
}

class Order{
//	1.私有化类的构造器
	private Order() {
		
	}
	
//	2.声明当前类对象,不进行初始化
//	此对象也必须声明为static的
	private static Order instance = null;
	
//	3.声明public static的返回当前类对象的方法
	public static Order getInstance() {
		if(instance == null){
			instance = new Order();
		}
		return instance;
	}
}
```

```
class Order{
//	1.私有化类的构造器
	private Order() {
		
	}
	
//	2.声明当前类对象,不进行初始化
//	此对象也必须声明为static的
	private static Order instance = null;
	
//	3.声明public static的返回当前类对象的方法	
//方式1 ：效率较低
//public static sychronized Order getInstance() {
//		if(instance == null){
//			instance = new Order();
//		}
//		return instance;
//	}
//方式2 
  public static Order getInstance(){
    if(instance == null){
      sychronized(Order.class){
        if(instance == null){
           instance = new Order();
        }
      }
    } 
    return instance;
  }
}
```

### 区分饿汉式和懒汉式

- 饿汉式：
   - 对象加载时间过长 
   - 线程安全
- 懒汉式：
   - 延迟对象的创建 
   - 线程不安全  --多线程时修改
   
### 单例模式的优点：

由于单例模式只生成一个实例，减少了系统性能开销，当一个对象的产生需要比较多的资源时，如读取配置、产生其他依赖对象时，则可以通过在应用启动时直接产生一个单例对象，然后永久驻留内存的方式来解决。

![](C:/NoteBook/pictures/339561509227262.png =462x)

### 单例(Singleton)设计模式-应用场景

- 网站的计数器，一般也是单例模式实现，否则难以同步。
- 应用程序的日志应用，一般都使用单例模式实现，这一般是由于共享的日志文件一直处于打开状态，因为只能有一个实例去操作，否则内容不好追加。
- 数据库连接池的设计一般也是采用单例模式，因为数据库连接是一种数据库资源。
- 项目中，读取配置文件的类，一般也只有一个对象。没有必要每次使用配置文件数据，都生成一个对象去读取。
- Application 也是单例的典型应用
- Windows的Task Manager (任务管理器)就是很典型的单例模式
- Windows的Recycle Bin (回收站)也是典型的单例应用。在整个系统运行过程中，回收站一直维护着仅有的一个实例。


## 理解main方法的语法

- 由于Java虚拟机需要调用类的main()方法，所以该方法的访问权限必须是public，又因为Java虚拟机在执行main()方法时不必创建对象，所以该方法必须是static的，该方法接收一个String类型的数组参数，该数组中保存执行Java命令时传递给所运行的类的参数。
- 又因为main() 方法是静态的，我们不能直接访问该类中的非静态成员，必须创建该类的一个实例对象后，才能通过这个对象去访问类中的非静态成员，这种情况，我们在之前的例子中多次碰到

**mian()方法的使用说明：**

1. main()方法作为程序的入口
2. main()方法也是一个普通的静态方法
3. main()方法可以作为我们与控制台交互的方式


### 命令行参数用法举例

```
package com.zjk;

public class MainTest {

	public static void main(String[] args) { //静态的方法
//		public 访问权限修饰符 
//		static 静态
//		void   无返回值
//		main   main方法名
//		String[] args 形参
		Main.main(new String[100]);
		
//		show(); 非静态的方法，要通过对象调用
	}
	
	public void show() {
		
	}
}

class Main {
	public static void main(String[] args) {
		args = new String[100];
		for (int i = 0; i < args.length; i++) {
			args[i] = "args_" + i;
			System.out.println(args[i]);
		}
	}
}
```

### main()方法可以作为我们与控制台交互的方式

#### eclipse
```
package com.zjk;

public class MainDemo {
	public static void main(String[] args) {
		for (int i = 0; i < args.length; i++) {
			System.out.println("*****" + args[i]);
		}
		
	}
}
```

![](C:/NoteBook/pictures/3364418239472.png =460x)

![](C:/NoteBook/pictures/25953914221046.png =534x)
![](C:/NoteBook/pictures/125253914239472.png =303x)


```
package com.zjk;

public class MainDemo {
	public static void main(String[] args) {
		for (int i = 0; i < args.length; i++) {
			System.out.println("*****" + args[i]);

			int num = Integer.parseInt(args[i]);
			System.out.println("#######" + num);
		}

	}
}
```

![](C:/NoteBook/pictures/143835418227339.png =531x)

![](C:/NoteBook/pictures/267235418247505.png =297x)


#### cmd

```
public class MainDemo {
	public static void main(String[] args) {
		for (int i = 0; i < args.length; i++) {
			System.out.println("*****" + args[i]);

			int num = Integer.parseInt(args[i]);
			System.out.println("#######" + num);
		}

	}
}
```

```
G:\vim_test>javac MainDemo.java 
G:\vim_test>java MainDemo.java "87" 89 "66"
```

#### 如何将控制台获取的数据传给形参 String[] args

```
运行时：java 类名 "Tom" "123" "true"

System.out.println(args[0]) //"Tom"
System.out.println(args[2]) //"true" --> Boolean.parseBoolean(args[2])
System.out.println(args[3]) //报异常
```

### 面试题

![](C:/NoteBook/pictures/272661709240097.png =633x)


## 类的成员之四：代码块(初始化块)

- 代码块(或初始化块)的作用：
  - 对Java类或对象进行初始化 
  - 代码块(或初始化块)的分类：
- 一个类中代码块若有修饰符，则只能被static修饰，称为静态代码块(static block)，没有使用static修饰的，为非静态代码块。
  - 有且只能使用static修饰 
- static代码块通常用于初始化static的属性

```
class Person {
    public static int total;
    
    static {
        total = 100;//为total赋初值
    }
    …… //其它属性或方法声明
}
```

-  静态代码块：用static 修饰的代码块
1. 可以有输出语句。
2. 可以对类的属性、类的声明进行初始化操作。
3. 不可以对非静态的属性初始化。即：不可以调用非静态的属性和方法。
4. 若有多个静态的代码块，那么按照从上到下的顺序依次执行。
5. 静态代码块的执行要先于非静态代码块。
6. 静态代码块随着类的加载而加载，且只执行一次。

- 非静态代码块：没有static修饰的代码块
1. 可以有输出语句。
2. 可以在创建对象时，对类（对象）的属性、类（对象）的声明进行初始化操作。
3. 除了调用非静态的结构外，还可以调用静态的变量或方法。
4. 若有多个非静态的代码块，那么按照从上到下的顺序依次执行。
5. 每次创建对象的时候，都会执行一次。且先于构造器执行。

```
package com.zjk;

public class BlockTest {
	public static void main(String[] args) {

		String desc = Person.desc;
//		静态代码块随着类的加载而加载且执行
//		只执行一次
		String desc2 = Person.desc;

		System.out.println(desc);

		Person p1 = new Person();
//		非静态代码块随着对象的创建而执行
//		且每创建一次对象，就执行一次
		Person p2 = new Person();

	}
}

class Person {
	String name;
	int age;
	static String desc = "我是一个人";

	public Person() {

	}

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

//	代码块
//	如果在类中定义了多个静态代码块
//	则按照声明的先后顺序执行;
//	静态代码块的执行优先与非静态代码块
	static {
		System.out.println("static block - 1");
		desc = "学习的人";
		info();
//		eat();
//		在静态代码块中只能调用静态的属性和结构
	}

	static {
		System.out.println("static block - 2");
	}
//	如果在类中定义了多个非静态代码块
//	则按照声明的先后顺序执行;
	{
		System.out.println("block - 1");
		age = 1; // 给对象赋初值
		desc = "学习的人";
		info();
		eat();
//		非静态代码块都可以调用
	}

	{
		System.out.println("block - 2");
	}

	public void eat() {
		System.out.println("吃饭");
	}
	
	public static void info() {
		System.out.println("快乐的人");
	}

	@Override
	public String toString() {
		return "Person [name=" + name + ", age=" + age + "]";
	}

}
```


### 程序中成员变量赋值的执行顺序

- 对属性可以赋值的位置

1. 默认初始化
2. 显式初始化/在代码块中赋值(谁在上，谁先)
3. 构造器中初始化
4. 有了构造器后：“对象.属性”或“对象.方法”的方式，进行赋值

![](C:/NoteBook/pictures/485915210221046.png =462x)

```
package com.zjk;

public class OrderTest {
	public static void main(String[] args) {
		Order order = new Order();
		System.out.println(order.orderId);
	}
}

class Order {
	
//	显式初始化/在代码块中赋值(谁在上，谁先)
	int orderId = 3;
	{
		orderId = 4;
	}
	
}
```

```
package com.zjk;

class Root{
	static{
		System.out.println("Root的静态初始化块");
	}
	{
		System.out.println("Root的普通初始化块");
	}
	public Root(){
		System.out.println("Root的无参数的构造器");
	}
}
class Mid extends Root{
	static{
		System.out.println("Mid的静态初始化块");
	}
	{
		System.out.println("Mid的普通初始化块");
	}
	public Mid(){
		System.out.println("Mid的无参数的构造器");
	}
	public Mid(String msg){
		//通过this调用同一类中重载的构造器
		this();
		System.out.println("Mid的带参数构造器，其参数值："
			+ msg);
	}
}
class Leaf extends Mid{
	static{
		System.out.println("Leaf的静态初始化块");
	}
	{
		System.out.println("Leaf的普通初始化块");
	}	
	public Leaf(){
		//通过super调用父类中有一个字符串参数的构造器
		super("尚硅谷");
		System.out.println("Leaf的构造器");
	}
}
public class LeafTest{
	public static void main(String[] args){
		new Leaf(); 
		//new Leaf();
	}
}
```

```
package com.zjk;

class Father {
	static {
		System.out.println("11111111111");
	}
	
	{
		System.out.println("22222222222");
	}

	public Father() {
		System.out.println("33333333333");

	}

}

public class Son extends Father {
	static {
		System.out.println("44444444444");
	}

	{
		System.out.println("55555555555");
	}

	public Son() {
		System.out.println("66666666666");
	}

	public static void main(String[] args) { // 由父及子 静态先行
		System.out.println("77777777777");
		System.out.println("************************");
		new Son();
		System.out.println("************************");

		new Son();
		System.out.println("************************");
		new Father();
	}

}
```

## 关键字：final

- 在Java中声明类、变量和方法时，可使用关键字final来修饰,表示“最终的”。
    - final标记的类不能被继承。提高安全性，提高程序的可读性。
       - String类、System类、StringBuffer类
    - final标记的方法不能被子类重写。
       - 比如：Object类中的getClass()。
    - final标记的变量(成员变量或局部变量)即称为常量。名称大写，且只能被赋值一次。
       - final标记的成员变量必须在声明时或在每个构造器中或代码块中显式赋值，然后才能使用。
       - final double MY_PI = 3.14;

**final修饰变量--常量**

- 修饰属性。可以考虑赋值的位置：显式初始化/代码块中初始化/构造器中初始化
- 修饰局部变量		
   - 使用final修饰形参时，为常量。当我们调用此方法时，给常量形参赋一个实参，一旦赋值以后，就只能在方法中使用此形参，但不能进行重新赋值
- static final  修饰属性 ： 全局常量
   
```
package com.zjk;

public class FinalTest {

//	final int WIDTH;
//	final 不适用于默认初始化,必须显式初始化
	final int WIDTH = 10;

	final int LEFT;
	{
		LEFT = 1; // 可以在代码块中赋值
	}

	final int RIGHT;

	public FinalTest() { // 可以在构造器中初始化
		// 每个构造器中都应该要为RIGHT赋值
		RIGHT = 2;
	}

	public FinalTest(int n) {
		RIGHT = n;
	}

//	public FinalTest(int n, int m) {
//	} //每个构造器中都应该要为RIGHT赋值

//	final int DOWN;
//  不可以在方法中调用final赋值。
//	在创建对象，调用构造器时，就需要得到final变量的值
//	public void setDown(int down) {
//		this.DOWN = down;
//	}

	public void doWith() {
//		WIDTH = 20; WIDTH被final修饰
	}
	
	public void show() {
		final int NUM = 10; //常量（局部变量）
//		NUM ++;
	}
	
	public final void show(final int NUM) {
//		NUM = 10; 
		//使用final修饰形参时，为常量
		//当我们调用此方法时，给常量形参赋一个实参，
		//一旦赋值以后，就只能在方法中使用此形参，但不能进行修改
	}

	public static void main(String[] args) {

		int num = 10;

		num = num + 5;
	}
}
```

### 面试题

#### 1

```
public class Something {
	public int addOne(final int x) {
		return ++x;  //编译出错。X的值不能改变
     // return x + 1; //编译通过
	}
}
```

#### 2

```
public class Something {
	public static void main(String[] args) {
		Other o = new Other();
		new Something().addOne(o);
	}

	public void addOne(final Other o) {
     // o = new Other(); //o的地址值不能改变
		o.i++; //但是o的地址值中的内容可以改变 
	}
}

class Other {
	public int i;
}
```

## 抽象类与抽象方法
 
随着继承层次中一个个新子类的定义，类变得越来越具体，而父类则更一般，更通用。类的设计应该保证父类和子类能够共享特征。有时将一个父类设计得非常抽象，以至于它没有具体的实例，这样的类叫做抽象类。

- 用abstract关键字来修饰一个类，这个类叫做抽象类。
    - 抽象类不能被实例化。抽象类是用来被继承的，（抽象类可以使用多态）
        - new  抽象类()；是非法的
    - 抽象类中一定有构造器，便于子类实例化时调用（涉及子类对象实例化的全过程）
    - 开发中，都会提供抽象类的子类，让子类对象实例化，完成相关操作。
- 用abstract来修饰一个方法，该方法叫做抽象方法。
    - 抽象方法：只有方法的声明，没有方法的实现。以分号结束：（没有代码块{}）
    - 含有抽象方法的类必须被声明为抽象类。
         - 而抽象类中不一定拥有抽象方法（可以没有）
    - 抽象类的子类必须重写父类（包括父类的父类...）的抽象方法，并提供方法体。才可以进行实例化
         - 若没有重写全部的抽象方法，仍为抽象类。则必须使用abstract修饰，不能进行实例化。
- 不能用abstract修饰变量、代码块、构造器；
- 不能用abstract修饰私有方法private、静态方法static、final的方法、final的类。
    - 父类中private的结构对子类不可见，不是重写
    - static修饰的方法不能被重写
    - final修饰的方法不能重写 
    - final修饰的类不能被继承

```
package com.zjk;

public class AbstractTest {
	public static void main(String[] args) {

//		Person p1 = new Person();
//		abstract修饰的类，抽象类不可实例化

//		Person.name;
//	    就算给抽象类的属性static修饰，也不能直接通过类.属性的方式调用
	}
}

abstract class Creature{
	public abstract void breath();
}

abstract class Person extends Creature {
    String name;
    //static String name;
	int age;

	public Person() {

	}

	public Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

//	不是抽象方法
//	public void eat() {
//		
//	}
	
//	抽象方法
	public abstract void eat();
	
//  若类是抽象类则无需重写继承自抽象父类的抽象方法
	
	public void walk() {
		System.out.println("walk");
	}
}

class Student extends Person {

	public Student(String name,int age) {
		super(name,age);
	}
	
	@Override
	public void eat() {
		System.out.println("eat more");
	}
	
//	Person类中还有继承自Creature类的抽象方法breath()，需要重写
	@Override
	public void breath() {
		System.out.println("breath");
	}
}
```

### 练 习2

```
编写一个Employee类，声明为抽象类，
包含如下三个属性：name，id，salary。
提供必要的构造器和抽象方法：work()。
对于Manager类来说，他既是员工，还具有奖金(bonus)的属性。
请使用继承的思想，设计CommonEmployee类和Manager类，
要求类中提供必要的方法进行属性访问。
```

```
package com.zjk;

public class EmployeeTest {
	public static void main(String[] args) {
		
		Manager manager = new Manager("King",1001,7000,80000);
//		Employee manager = new Manager("King",1001,7000,80000);
//		抽象类可以使用多态
		manager.work();
		
		CommonEmployee c = new CommonEmployee();
		c.work();
	}
}

abstract class Employee {

	private String name;
	private int id;
	private double salary;

	public Employee() {

	}

	public Employee(String name, int id, int salary) {
		this.name = name;
		this.id = id;
		this.salary = salary;
	}

	public abstract void work();
}

class CommonEmployee extends Employee {

	@Override
	public void work() {
		System.out.println("Common Work");
	}
}

class Manager extends Employee {

	private double bonus;

	public Manager(double bonus) {
		super();
		this.bonus = bonus;
	}

	public Manager(String name, int id, int salary, double bonus) {
		super(name, id, salary);
		this.bonus = bonus;
	}

	@Override
	public void work() {
		System.out.println("Manager Work");
	}
}
```

### 抽象类的匿名子类

```
package com.zjk;

public class PersonTest {

	public static void main(String[] args) {
		method(new Student());// 匿名对象

		Worker worker = new Worker();
		method1(worker); // 非匿名的类，非匿名的对象

		method1(new Worker()); // 非匿名的类，匿名的对象

		System.out.println();

//		抽象类的匿名子类
//		相当于创建了匿名子类的对象p
		Person p = new Person() {

			@Override
			public void eat() {
				// TODO Auto-generated method stub
				System.out.println("eat food");
			}

			@Override
			public void breath() {
				// TODO Auto-generated method stub
				System.out.println("breath too");
			}

		};

		method1(p); // 此时调用的是匿名子类的对象

		System.out.println();
		
//		创建匿名子类的匿名对象
		method1(new Person() {
			@Override
			public void eat() {
				// TODO Auto-generated method stub
				System.out.println("eat food 2");
			}

			@Override
			public void breath() {
				// TODO Auto-generated method stub
				System.out.println("breath too 2");
			}
		});
	}

	public static void method1(Person p) {
		p.eat();
		p.walk();
	}

	public static void method(Student s) {

	}
}

class Worker extends Person {
	@Override
	public void eat() {
		// TODO Auto-generated method stub

	}

	@Override
	public void breath() {
		// TODO Auto-generated method stub

	}
}
```

### 多态(抽象类)的应用：模板方法设计模式(TemplateMethod)

- 抽象类体现的就是一种模板模式的设计，抽象类作为多个子类的通用模板，子类在抽象类的基础上进行扩展、改造，但子类总体上会保留抽象类的行为方式。

- 解决的问题：
   - 当功能内部一部分实现是确定的，一部分实现是不确定的。这时可以把不确定的部分暴露出去，让子类去实现。
   - 换句话说，在软件开发中实现一个算法时，整体步骤很固定、通用，这些步骤已经在父类中写好了。但是某些部分易变，易变部分可以抽象出来，供不同子类实现。这就是一种模板模式。
   
- 模板方法设计模式是编程中经常用得到的模式。各个框架、类库中都有他的影子，比如常见的有：
   - 数据库访问的封装
   - Junit单元测试
   - JavaWeb的Servlet中关于doGet/doPost方法调用
   - Hibernate中模板程序
   - Spring中JDBCTemlate、HibernateTemplate等
   
```
package com.zjk;

public class TemplateTest {
	public static void main(String[] args) {
		Template t = new SubTemplate();
		t.spendTime();
	}
}

abstract class Template {

//	计算某段代码执行所需要花费的时间
	public void spendTime() {
		long start = System.currentTimeMillis();

		code();//不确定的部分

		long end = System.currentTimeMillis();

		System.out.println("花费的时间：" + (end - start));
	}

//	不确定（易变）的部分
	public abstract void code();
}

class SubTemplate extends Template {

	@Override
	public void code() {
		// TODO Auto-generated method stub
		for (int i = 2; i <= 1000; i++) {
			boolean isFlag = true;
			for (int j = 2; j < Math.sqrt(i); j++) {
				if (i % j == 0) {
					isFlag = false;
					break;
				}
			}
			if (isFlag) {
				System.out.println(i);
			}
		}
	}

}
```

```
package com.atguigu.java;
//抽象类的应用：模板方法的设计模式
public class TemplateMethodTest {

	public static void main(String[] args) {
		BankTemplateMethod btm = new DrawMoney();
		btm.process();

		BankTemplateMethod btm2 = new ManageMoney();
		btm2.process();
	}
}
abstract class BankTemplateMethod {
	// 具体方法
	public void takeNumber() {
		System.out.println("取号排队");
	}

	public abstract void transact(); // 办理具体的业务 //钩子方法

	public void evaluate() {
		System.out.println("反馈评分");
	}

	// 模板方法，把基本操作组合到一起，子类一般不能重写
	public final void process() {
		this.takeNumber();

		this.transact();// 像个钩子，具体执行时，挂哪个子类，就执行哪个子类的实现代码

		this.evaluate();
	}
}

class DrawMoney extends BankTemplateMethod {
	public void transact() {
		System.out.println("我要取款！！！");
	}
}

class ManageMoney extends BankTemplateMethod {
	public void transact() {
		System.out.println("我要理财！我这里有2000万美元!!");
	}
}
```

#### 练习3
编写工资系统，实现不同类型员工(多态)的按月发放工资。如果当月出现某个Employee对象的生日，则将该雇员的工资增加100元。

实验说明：
```
（1）定义一个Employee类，该类包含：
private成员变量name,number,birthday，其中birthday 为MyDate类的对象；
abstract方法earnings()；
toString()方法输出对象的name,number和birthday。
（2）MyDate类包含:
private成员变量year,month,day ；
toDateString()方法返回日期对应的字符串：xxxx年xx月xx日
（3）定义SalariedEmployee类继承Employee类，实现按月计算工资的员工处理。
该类包括：private成员变量monthlySalary；
实现父类的抽象方法earnings() ,该方法返回monthlySalary值；
toString()方法输出员工类型信息及员工的name，number,birthday。
（4）参照SalariedEmployee类定义HourlyEmployee类，实现按小时计算工资的员工处理。该类包括：
private成员变量wage和hour；
实现父类的抽象方法earnings(),该方法返回wage*hour值；
toString()方法输出员工类型信息及员工的name，number,birthday。
（5）定义PayrollSystem类，创建Employee变量数组并初始化，该数组存放各类雇员对象的引用。
利用循环结构遍历数组元素，输出各个对象的类型,name,number,birthday,以及该对象生日。
当键盘输入本月月份值时，如果本月是某个Employee对象的生日，还要输出增加工资信息。
提示：
//定义People类型的数组People c1[]=new People[10];
//数组元素赋值
c1[0]=new People("John","0001",20);
c1[1]=new People("Bob","0002",19);
//若People有两个子类Student和Officer，则数组元素赋值时，可以使父类类型的数组元素指向子类。
c1[0]=new Student("John","0001",20,85.0);
c1[1]=new Officer("Bob","0002",19,90.5);
```

```
package com.zjk;

import java.util.Scanner;

public abstract class Employee {

	private String name;
	private int number;
	private MyDate birthday;

	public Employee(String name, int number, MyDate birthday) {
		super();
		this.name = name;
		this.number = number;
		this.birthday = birthday;
	}

	public abstract double earnings();

	@Override
	public String toString() {
		return "name=" + name + ", number=" + number + ", birthday=" + birthday.toDateString();
	}

	public String getName() {
		return name;
	}

	public int getNumber() {
		return number;
	}

	public MyDate getBirthday() {
		return birthday;
	}

	public void setName(String name) {
		this.name = name;
	}

	public void setNumber(int num) {
		this.number = num;
	}

	public void setBirthday(MyDate birthday) {
		this.birthday = birthday;
	}

}

class MyDate {

	private int year;
	private int month;
	private int day;

	
	public MyDate(int year, int month, int day) {
		super();
		this.year = year;
		this.month = month;
		this.day = day;
	}

	public String toDateString() {
		return year + "年" + month + "月" + day + "日";
	}

	public int getYear() {
		return year;
	}

	public int getMonth() {
		return month;
	}

	public int getDay() {
		return day;
	}

	public void setYear(int year) {
		this.year = year;
	}

	public void setMonth(int month) {
		this.month = month;
	}

	public void setDay(int day) {
		this.day = day;
	}

}

class SalariedEmployee extends Employee {

	private double monthlySalary;

	public SalariedEmployee(String name, int number, MyDate birthday, double monthlySalary) {
		super(name, number, birthday);
		this.monthlySalary = monthlySalary;
	}

	@Override
	public double earnings() {
		// TODO Auto-generated method stub
		return monthlySalary;
	}

	@Override
	public String toString() {
		return "SalariedEmployee [" + super.toString() + ", monthlySalary=" + monthlySalary + "]";
	}
}

class HourlyEmployee extends Employee {

	private double wage;
	private double hour;

	public HourlyEmployee(String name, int number, MyDate birthday, double wage, double hour) {
		super(name, number, birthday);
		// TODO Auto-generated constructor stub
		this.wage = wage;
		this.hour = hour;
	}

	@Override
	public double earnings() {
		// TODO Auto-generated method stub
		return wage * hour;
	}

	@Override
	public String toString() {
		return "HourlyEmployee [" + super.toString() + ", wage*hour=" + wage * hour + "]";
	}

}

package com.zjk;

import java.util.Scanner;

public abstract class Employee {

	private String name;
	private int number;
	private MyDate birthday;

	public Employee(String name, int number, MyDate birthday) {
		super();
		this.name = name;
		this.number = number;
		this.birthday = birthday;
	}

	public abstract double earnings();

	@Override
	public String toString() {
		return "name=" + name + ", number=" + number + ", birthday=" + birthday.toDateString();
	}

	public String getName() {
		return name;
	}

	public int getNumber() {
		return number;
	}

	public MyDate getBirthday() {
		return birthday;
	}

	public void setName(String name) {
		this.name = name;
	}

	public void setNumber(int num) {
		this.number = num;
	}

	public void setBirthday(MyDate birthday) {
		this.birthday = birthday;
	}

}

class MyDate {

	private int year;
	private int month;
	private int day;

	
	public MyDate(int year, int month, int day) {
		super();
		this.year = year;
		this.month = month;
		this.day = day;
	}

	public String toDateString() {
		return year + "年" + month + "月" + day + "日";
	}

	public int getYear() {
		return year;
	}

	public int getMonth() {
		return month;
	}

	public int getDay() {
		return day;
	}

	public void setYear(int year) {
		this.year = year;
	}

	public void setMonth(int month) {
		this.month = month;
	}

	public void setDay(int day) {
		this.day = day;
	}

}

class SalariedEmployee extends Employee {

	private double monthlySalary;

	public SalariedEmployee(String name, int number, MyDate birthday, double monthlySalary) {
		super(name, number, birthday);
		this.monthlySalary = monthlySalary;
	}

	@Override
	public double earnings() {
		// TODO Auto-generated method stub
		return monthlySalary;
	}

	@Override
	public String toString() {
		return "SalariedEmployee [" + super.toString() + ", monthlySalary=" + monthlySalary + "]";
	}
}

class HourlyEmployee extends Employee {

	private double wage;
	private double hour;

	public HourlyEmployee(String name, int number, MyDate birthday, double wage, double hour) {
		super(name, number, birthday);
		// TODO Auto-generated constructor stub
		this.wage = wage;
		this.hour = hour;
	}

	@Override
	public double earnings() {
		// TODO Auto-generated method stub
		return wage * hour;
	}

	@Override
	public String toString() {
		return "HourlyEmployee [" + super.toString() + ", wage*hour=" + wage * hour + "]";
	}

}
```
## 接口(interface)

- 一方面，有时必须从几个类中派生出一个子类，继承它们所有的属性和方法。但是，Java不支持多重继承。有了接口，就可以得到多重继承的效果。
- 另一方面，有时必须从几个类中抽取出一些共同的行为特征，而它们之间又没有is-a的关系，仅仅是具有相同的行为特征而已。
- 接口就是规范，定义的是一组规则，体现了现实世界中“如果你是/要...则必须能...”的思想。
    - 继承是一个"是不是"的关系，而接口实现则是 "能不能"的关系。
- 接口的本质是契约，标准，规范

- **接口的特点：**

- 用interface来定义。
- 接口和类是并列关系，或者可以理解为一种特殊的类。
- 接口(interface)是抽象方法和常量值定义的集合。
    - JDK7以之前：只能定义全局常量和抽象方法
    - 从本质上讲，接口是一种特殊的抽象类，这种抽象类中只包含常量和方法的定义(JDK7.0及之前)，而没有变量和方法的实现。
        - 全局常量：public static final修饰的 编译时可以省略
           - 接口中的所有成员变量都默认是由public static final修饰的。
        - 抽象方法：public abstract修饰的 编译时可以省略
           - 接口中的所有抽象方法都默认是由public abstract修饰的。
    - JDK8：
        - 除了定义JDK7之前的，还可以：
        - 定义静态方法pulic static
        - 默认方法
- 接口中没有构造器。不能实例化
   - 接口的主要用途就是被实现类实现。（面向接口编程）
   - 实现接口的类中必须提供接口中所有方法的具体实现内容(覆盖接口中的所有抽象方法)，方可实例化。否则，仍为抽象类。
- 接口采用多继承机制。Java类可以实现多个接口. **多态性**
   - 与继承关系类似，接口与实现类之间存在多态性
   - 定义Java类的语法格式：先写extends，后写implements

```
class SubClass extends SuperClass implements InterfaceA{ 
   ...
}
```

- 一个类可以实现(implements)多个接口，接口也可以继承(extends)其它接口(可以多继承)。

```
interface 接口1 extends 接口2,接口3{ //不是implements
   ...
}
```

### 例

```
package com.zjk;

public class InterfaceTest {
	public static void main(String[] args) {

		System.out.println(Flyable.MAX_SPEED);
		System.out.println(Flyable.MIN_SPEED);

		Plane p = new Plane();
		p.fly();
		p.stop();
	}
}

interface Flyable {
//	全局常量
	public static final int MAX_SPEED = 7900;
	int MIN_SPEED = 1; // 默认public static final
	// 仍然是全局常量

//	抽象方法
	public abstract void fly();

//	仍然是抽象方法 默认public abstract
	void stop();

//	Interfaces cannot have constructors
//	接口不能有构造器，不能实例化
//	public Flyable() {
//		
//	}
}

interface Attackable {
	void attack();
}

class Plane implements Flyable {

	@Override
	public void fly() {
		// TODO Auto-generated method stub
		System.out.println("引擎起飞");
	}

	@Override
	public void stop() {
		// TODO Auto-generated method stub
		System.out.println("减速停止");
	}
}

abstract class Kite implements Flyable {
// 实现接口的类必须覆盖接口的所有方法，否则仍然为抽象类
	@Override
	public void fly() {
		// TODO Auto-generated method stub

	}
}

class Bullet extends Object implements Flyable, Attackable {

	@Override
	public void attack() {
		// TODO Auto-generated method stub

	}

	@Override
	public void fly() {
		// TODO Auto-generated method stub

	}

	@Override
	public void stop() {
		// TODO Auto-generated method stub

	}

}

interface AA{
	
}

interface BB{
	
}

interface CC extends AA,BB{
	
}
```

```
package com.zjk;

public class USBTest {
	public static void main(String[] args) {
		Computer com = new Computer();
//		1.创建了接口的非匿名实现类的非匿名对象
		Flash falsh = new Flash();
		com.transferDate(falsh);
//		2.创建接口的非匿名实现类的匿名对象
		com.transferDate(new Flash());
//		3.创建接口的匿名实现类的非匿名对象
		USB phone = new USB() {

			@Override
			public void start() {
				// TODO Auto-generated method stub
				System.out.println("手机工作");
			}

			@Override
			public void stop() {
				// TODO Auto-generated method stub
				System.out.println("手机结束工作");
			}

		};
		com.transferDate(phone);
//		4.创建接口的匿名实现类的匿名对象
		com.transferDate(new USB() {

			@Override
			public void start() {
				// TODO Auto-generated method stub
				System.out.println("mp3开始工作");
			}

			@Override
			public void stop() {
				// TODO Auto-generated method stub
				System.out.println("mp3结束工作");
			}

		});
	}
}

class Computer {
//	接口的多态性
	public void transferDate(USB usb) {
		usb.start();

		System.out.println("具体传输数据细节");

		usb.stop();
	}
}

interface USB {

	void start();

	void stop();
}

class Flash implements USB {

	@Override
	public void start() {
		// TODO Auto-generated method stub
		System.out.println("U盘开始工作");
	}

	@Override
	public void stop() {
		// TODO Auto-generated method stub
		System.out.println("U盘结束工作");
	}

}

class Printer implements USB {

	@Override
	public void start() {
		// TODO Auto-generated method stub
		System.out.println("打印机开始工作");
	}

	@Override
	public void stop() {
		// TODO Auto-generated method stub
		System.out.println("打印机开始工作");
	}

}
```

### 接口的应用：代理模式(Proxy)

代理模式是Java开发中使用较多的一种设计模式。代理设计就是为其他对象提供一种代理以控制对这个对象的访问。

- 应用场景：
   - 安全代理：屏蔽对真实角色的直接访问。
   - 远程代理：通过代理类处理远程方法调用（RMI）
   - 延迟加载：先加载轻量级的代理对象，真正需要再加载真实对象

比如你要开发一个大文档查看软件，大文档中有大的图片，有可能一个图片有100MB，在打开文件时，不可能将所有的图片都显示出来，这样就可以使用代理模式，当需要查看图片时，用proxy来进行大图片的打开。

- 分类
   - 静态代理（静态定义代理类） 
   - 动态代理（动态生成代理类）
      - JDK自带的动态代理，需要反射等知识
   
```
package com.zjk;

public class NetWorkTest {

	public static void main(String[] args) {
		Server server = new Server();
		ProxyServer proxyServer = new ProxyServer(server);
		
		proxyServer.browse();
	}
}

interface NetWork {
	void browse();
}

//被代理类
class Server implements NetWork {

	@Override
	public void browse() {
		// TODO Auto-generated method stub
		System.out.println("真实的服务器访问网络");
	}

}

//代理类

class ProxyServer implements NetWork {

	private NetWork work;

	public ProxyServer(NetWork work) {
		this.work = work;
	}

	public void check() {
		System.out.println("检查工作");
	}

	@Override
	public void browse() {
		// TODO Auto-generated method stub
		check();

		work.browse();
	}

}
```

### 接口的应用：工厂模式

- 实现了创建者与调用者的分离，即将创建对象的具体过程屏蔽隔离起来，达到提高灵活性的目的。

**面向对象的设计原则（总共六个）：这里说几个和工厂模式相关的**

- OCP（开闭原则，Open-Closed Principle） 
   - 一个软件的实体应当对扩展开放，对修改关闭。
   - 当我们写完的代码，不能因为需求变化就修改。我们可以通过新增代码的方式来解决变化的需求。如果每次需求变动都去修改原有的代码，那原有的代码就存在被修改错误的风险，当然这其中存在有意和无意的修改，都会导致原有正常运行的功能失效的风险，这样很有可能会展开可怕的蝴蝶效应，使维护工作剧增。
   - 说到底，开闭原则除了表面上的可扩展性强以外，在企业中更看重的是维护成本
   - 所以，开闭原则是设计模式的第一大原则，它的潜台词是：控制需求变动风险，缩小维护成本。
- DIP（依赖倒转原则，Dependence Inversion Principle） 
   - 要针对接口编程，不要针对实现编程。
   - 如果 A 中关联 B，那么尽量使得 B 实现某个接口，然后 A 与接口发生关系，不与 B 实现类发生关联关系。
   - 依赖倒置的潜台词是：面向抽象编程，解耦调用和被调用者。
- LOD（迪米特法则，Law Of Demeter） 
   - 只与你直接的朋友通信，而避免和陌生人通信。
   - 要求尽量的封装，尽量的独立，尽量的使用低级别的访问修饰符。这是封装特性的典型体现。
   - 一个类如果暴露太多私用的方法和字段，会让调用者很茫然。并且会给类造成不必要的判断代码。所以，我们使用尽量低的访问修饰符，让外界不知道我们的内部。这也是面向对象的基本思路。这是迪米特原则的一个特性，无法了解类更多的私有信息。另外，迪米特原则要求类之间的直接联系尽量的少，两个类的访问，通过第三个中介类来实现。
   - 迪米特原则的潜台词是：不和陌生人说话，有事去中介。

**工厂模式的分类：**

- 简单工厂模式：用来生产同一等级结构中的任意产品。（对于增加新的产品，需要修改已有代码）
- 工厂方法模式：用来生产同一等级结构中的固定产品。（支持增加任意产品）
- 抽象工厂模式：用来生产不同产品族的全部产品。（对于增加新的产品，无能为力；支持增加产品族

GOF 在《设计模式》一书中将工厂模式分为两类：工厂方法模式（Factory Method）与抽象工厂模式（Abstract Factory）。将简单工厂模式（Simple Factory）看为工厂方法模式的一种特例，两者归为一类。

**核心本质：**
- 实例化对象，用工厂方法代替 new 操作。
- 将选择实现类、创建对象统一管理和控制。从而将调用者跟我们的实现类解耦。

#### 1、无工厂模式

![](C:/NoteBook/pictures/271765020247508.png =302x)

```
package com.atguigu.pattern.factory.nofactory;

interface Car {
	void run();
}

class Audi implements Car {
	public void run() {
		System.out.println("奥迪在跑");
	}
}

class BYD implements Car {
	public void run() {
		System.out.println("比亚迪在跑");
	}
}

public class Client01 {
	public static void main(String[] args) {
		Car a = new Audi();
		Car b = new BYD();
		a.run();
		b.run();
	}
}
```
#### 2、简单工厂模式(广义的工厂)

- 存在的目的很简单：定义一个用于创建对象的工厂类。
- 调用者只要知道他要什么，从哪里拿，如何创建，不需要知道。分工，多出了一个专门生产 Car 的实现类对象的工厂类。把调用者与创建者分离。
- 简单工厂模式也叫静态工厂模式，就是工厂类一般是使用静态方法，通过接收的参数的不同来返回不同的实例对象。
- 缺点：对于增加新产品，不修改代码的话，是无法扩展的。违反了开闭原则（对扩展开放；对修改封闭）。

![](C:/NoteBook/pictures/127305020227342.png =318x)

```
package com.atguigu.pattern.factory.simple;

interface Car {
	void run();
}

class Audi implements Car {
	public void run() {
		System.out.println("奥迪在跑");
	}
}

class BYD implements Car {
	public void run() {
		System.out.println("比亚迪在跑");
	}
}

//工厂类
class CarFactory {
//方式一
	public static Car getCar(String type) {
		if ("奥迪".equals(type)) {
			return new Audi();
		} else if ("比亚迪".equals(type)) {
			return new BYD();
		} else {
			return null;
		}
	}
//方式二
//public static Car getAudi() {
//    return new Audi();
//}
//
//public static Car getByd() {
//    return new BYD();
//}
}

public class Client02 {
	public static void main(String[] args) {
		Car a = CarFactory.getCar("奥迪");
		a.run();
		Car b = CarFactory.getCar("比亚迪");
		b.run();
	}
}
```
#### 3、工厂方法模式 

- 为了避免简单工厂模式的缺点，不完全满足 OCP（对扩展开放，对修改关闭）。
- 工厂方法模式和简单工厂模式最大的不同在于，简单工厂模式只有一个（对于一个项目或者一个独立的模块而言）工厂类，而工厂方法模式有一组实现了相同接口的工厂类。这样在简单工厂模式里集中在工厂方法上的压力可以由工厂方法模式里不同的工厂子类来分担。



![](C:/NoteBook/pictures/523314820239475.png =549x)
简单工厂模式与工厂方法模式真正的避免了代码的改动了？没有。在简单工厂模式中，新产品的加入要修改工厂角色中的判断语句；而在工厂方法模式中，要么将判断逻辑留在抽象工厂角色中，要么在客户程序中将具体工厂角色写死（就像上面的例子一样）。而且产品对象创建条件的改变必然会引起工厂角色的修改。面对这种情况，Java 的反射机制与配置文件的巧妙结合突破了限制——这在Spring 中完美的体现了出来。

```
package com.atguigu.pattern.factory.method;

interface Car {
	void run();
}

//两个实现类
class Audi implements Car {
	public void run() {
		System.out.println("奥迪在跑");
	}
}

class BYD implements Car {
	public void run() {
		System.out.println("比亚迪在跑");
	}
}

//工厂接口
interface Factory {
	Car getCar();
}

//两个工厂类
class AudiFactory implements Factory {
	public Audi getCar() {
		return new Audi();
	}
}

class BydFactory implements Factory {
	public BYD getCar() {
		return new BYD();
	}
}

public class Client {
	public static void main(String[] args) {
		Car a = new AudiFactory().getCar();
		Car b = new BydFactory().getCar();
		a.run();
		b.run();
	}
}
```
#### 4、抽象工厂模式

- 抽象工厂模式和工厂方法模式的区别就在于需要创建对象的复杂程度上。而且抽象工厂模式是三个里面最为抽象、最具一般性的。
- 抽象工厂模式的用意为：给客户端提供一个接口，可以创建多个产品族中的产品对象。
- 而且使用抽象工厂模式还要满足一下条件：
  1. 系统中有多个产品族，而系统一次只可能消费其中一族产品。
  2. 同属于同一个产品族的产品以其使用。

### 接口和抽象类之间的对比

![](C:/NoteBook/pictures/58533111239472.png =656x)

在开发中，常看到一个类不是去继承一个已经实现好的类，而是要么继承抽象类，要么实现接口。

- 相同：不能实例化，都可以包含抽象方法的
- 不同：
   - 定义。内部结构等
   - 类 ：单继承 ； 接口：多继承
   -  类与接口：多实现

### 【面试题】排错

#### 1

```
interface A {
	int x = 0;
}

class B {
	int x = 1;
}

class C extends B implements A {

	public void px() {
//		System.out.println(x);
//		The field x is ambiguous
		System.out.println(super.x); // 类B的x
//		没有super.super的写法；如果需要调用父类的父类的x
//		则需要在父类中有调用的方法； 
//		尽量避免变量重名
		System.out.println(A.x); // 接口A的x
	}

	public static void main(String[] args) {
		new C().px();
	}
}
```

#### 2

```
interface Playable {
	void play();
}

interface Bounceable {
	void play();
}

interface Rollable extends Playable, Bounceable {
	Ball ball = new Ball("PingPang");//默认 public static final
}

class Ball implements Rollable {
	private String name;

	public String getName() {
		return name;
	}

	public Ball(String name) {
		this.name = name;
	}

	public void play() {
//		即认为是接口Playable的重写也认为是接口Bounceable的重写，是可以的

//		ball = new Ball("Football");
//		在接口Rollable中定义了ball是public static final修饰的
		System.out.println(ball.getName());
	}
}
```

### 练习4

定义一个接口用来实现两个对象的比较。

```
interface CompareObject{
    public int compareTo(Object o); 
    //若返回值是 0 , 代表相等; 若为正数，代表当前对象大；负数代表当前对象小
}
```

定义一个Circle类，声明redius属性，提供getter和setter方法

定义一个ComparableCircle类，继承Circle类并且实现CompareObject接口。

在ComparableCircle类中给出接口中方法compareTo的实现体，用来比较两个圆的半径大小。

定义一个测试类InterfaceTest，创建两个ComparableCircle对象，调用compareTo方法比较两个类的半径大小。

思 考 ： 参 照 上 述 做 法 定 义 矩 形 类 Rectangle 和 ComparableRectangle 类 ， 
  
在ComparableRectangle类中给出compareTo方法的实现，比较两个矩形的面积大小。

```
package com.zjk;

public class Test {
	public static void main(String[] args) {
		ComparableCircle c1 = new ComparableCircle(1.2);
		ComparableCircle c2 = new ComparableCircle(1.3);

		System.out.println(c1.compareTo(c2));

		ComparableRectangle r1 = new ComparableRectangle(1, 2);
		ComparableRectangle r2 = new ComparableRectangle(2, 1);

		System.out.println(r1.compareTo(r2));
	}
}

interface CompareObject {
	// 若返回值是 0 , 代表相等; 若为正数，代表当前对象大；负数代表当前对象小
	public int compareTo(Object o);
}

class Circle {
	private Double radius;

	public Circle(Double radius) {
		super();
		this.radius = radius;
	}

	public Double getRadius() {
		return radius;
	}

	public void setRadius(Double radius) {
		this.radius = radius;
	}
}

class ComparableCircle extends Circle implements CompareObject {

	public ComparableCircle(Double radius) {
		super(radius);
	}

	@Override
	public int compareTo(Object o) {
		// TODO Auto-generated method stub
		if (this == o) {
			return 0;
		}
		if (o instanceof Circle) {
			ComparableCircle c = (ComparableCircle) o;
////			return (int) (this.getRadius() - c.getRadius()); 强转数据损失的问题
//          方式1
//			if (this.getRadius() > c.getRadius()) {
//				return 1;
//			} else if (this.getRadius() < c.getRadius()) {
//				return -1;
//			} else {
//				return 0;
//			}
			
//			方式二
			return this.getRadius().compareTo(c.getRadius());
		} else {
			throw new RuntimeException("输入的类型不同");
		}
	}

}

class Rectangle {
	private double wide;
	private double length;

	public Rectangle(double wide, double length) {
		super();
		this.wide = wide;
		this.length = length;
	}

	public double getWide() {
		return wide;
	}

	public double getLength() {
		return length;
	}

	public void setWide(double wide) {
		this.wide = wide;
	}

	public void setLength(double length) {
		this.length = length;
	}

	public double findArea() {
		return wide * length;
	}

}

class ComparableRectangle extends Rectangle implements CompareObject {

	public ComparableRectangle(double wide, double length) {
		super(wide, length);
		// TODO Auto-generated constructor stub
	}

	@Override
	public int compareTo(Object o) {
		// TODO Auto-generated method stub
		if (this == o) {
			return 0;
		}
		if (o instanceof Rectangle) {
			Rectangle r = (Rectangle) o;
			if (this.findArea() > r.findArea()) {
				return 1;
			} else if (this.findArea() < r.findArea()) {
				return -1;
			} else {
				return 0;
			}
		} else {
			throw new RuntimeException("输入的数据类型不一致");
		}
	}

}
```

### Java 8中关于接口的改进

- Java 8中，除了定义全局常量和抽象方法之外，还可以为接口添加静态方法和默认方法。
   - 从技术角度来说，这是完全合法的，只是它看起来违反了接口作为一个抽象定义的理念。
- 静态方法：使用 static 关键字修饰。
     - 只能通过接口直接调用静态方法，并执行其方法体。
        - 我们经常在相互一起使用的类中使用静态方法。你可以在标准库中找到像Collection/Collections或者Path/Paths这样成对的接口和类。
- 默认方法：默认方法使用 default 关键字修饰。
     - 只能通过实现类对象来调用。
     - 如果实现类重写了接口中的默认方法，调用时，仍然调用重写后的方法
        - 我们在已有的接口中提供新方法的同时，还保持了与旧版本代码的兼容性。
        - 比如：java 8 API中对Collection、List、Comparator等接口提供了丰富的默认方法。

#### 接口中的默认方法

- 若一个接口中定义了一个默认方法，而另外一个接口中也定义了一个同名同参数的方法（不管此方法是否是默认方法），在实现类同时实现了这两个接口时，会出现：接口冲突。
    - 解决办法：实现类必须覆盖(重写)接口中同名同参数的方法，来解决冲突。
- 若一个接口中定义了一个默认方法，而父类中也定义了一个同名同参数的非抽象方法，则不会出现冲突问题。
    - 那么子类在没有重写此方法的情况下，默认调用父类同名同参的方法（类优先原则）
    - 因为此时遵守：类优先原则。接口中具有相同名称和参数的默认方法会被忽略。
- 如何在子类（实现类）的方法中调用父类，接口中被重写的方法
   - 父类：`super.方法();`
   - 接口：`接口.super.方法();`

#### 例

```java
package com.zjk;

public interface CompareA {

//	静态方法
	public static void method1() {
		System.out.println("CompareA:北京");
	}

//	默认方法
	public default void method2() {
		System.out.println("ComparA：上海");
	}

	default void method3() {
		System.out.println("ComparA：上海");
	}
}


package com.zjk;

public class SubClassTest {

	public static void main(String[] args) {
		SubClass s = new SubClass();
		
//		s.method1();
//		SubClass.method1();
//		接口中定义的静态方法，只能通过接口来调用
		CompareA.method1();
		
//		CompareA.method2();
//		SubClass.method2();
//		接口中定义的默认方法，只能通过实现类对象来调用。
//		如果实现类重写了接口中的默认方法，调用时，仍然调用重写后的方法
		s.method2();
		s.method3();
		//父类和接口的method3()重名
//		如果子类（实现类）继承的父类和实现的接口中声明了同名同参的方法。
//		那么子类在没有重写此方法的情况下，默认调用父类同名同参的方法
	}
}

class SubClass extends SuperClass implements CompareA,CompareB{
//class SubClass implements CompareA,CompareB{
//  如果实现类实现了多个接口，而多个接口中定义了同名同参的默认方法
//	那么在实现类没有重写的情况下，报错——>接口冲突
//	此时需要必须在实现类中重写此方法
	
	//重写而不是实现(abstract)
	public void method2() {
		System.out.println("SubClass: 上海"); 
	}
	
	public void method3() {
		System.out.println("SubClass: 深圳");
	}
	
	public void myMethod() {
		method3();//调用自己定义的重写方法
		super.method3();//调用父类中声明的
		CompareA.super.method3();//调用接口中的默认方法
		CompareB.super.method3();//调用接口中的默认方法
		
	}
}

class SuperClass{
	public void method3() {
		System.out.println("SuperClass:上海");
	}
}

interface CompareB{
	default void method3() {
		System.out.println("ComparB：上海");
	}
}
```

#### 接口新特性的应用

```java
package com.zjk;

interface Filial {
	default void help() {
		System.out.println("-Filial");
	}
}

interface Spoony {
	default void help() {
		System.out.println("-Spoony");
	}
}

class Father{
	public void help() {
		System.out.println("Father");
	}
}

public class Man extends Father implements Filial, Spoony {
	@Override
	public void help() {
		// TODO Auto-generated method stub
		System.out.println("who?");
		Filial.super.help();
		Spoony.super.help();
	}
}
```

## 类的内部成员之五：内部类

当一个事物的内部，还有一个部分需要一个完整的结构进行描述，而这个内部的完整的结构又只为外部事物提供服务，那么整个内部的完整结构最好使用内部类。

- 在Java中，允许一个类的定义位于另一个类的内部，前者称为内部类，后者称为外部类。
   - (允许一个类A声明在另一个类B中，则类A是内部类，类B是外部类)
- Inner class一般用在定义它的类或语句块之内，在外部引用它时必须给出完整的名称。
- Inner class的名字不能与包含它的外部类类名相同；
- 分类： 
   - 成员内部类（static成员内部类和非static成员内部类）
   - 局部内部类[方法内，代码块内，构造器内]（不谈修饰符）、匿名内部类

- 成员内部类和局部内部类，在编译以后，都会生成字节码文件
- 格式：
   - 成员内部类：外部类$内部类名.class
   - 局部内部类：外部类$数字 内部类名.class 

### 成员内部类

**成员内部类作为外部类的成员：**

- 和外部类不同，成员内部类还可以声明为private或protected；（包括public和缺省）
- 可以调用外部类的结构
- 可以被static修饰，但此时就不能再使用外层类的非static的成员变量；

**成员内部类作为一个类：**

- 可以在内部定义属性、方法、构造器等结构
- 可以被abstract修饰，
- 可以被final修饰,表示此类不能被继承；
    - 没有被final修饰则可以被继承 
- 编译以后生成OuterClass$InnerClass.class字节码文件（也适用于局部内部类）

**关注**

1. 如何实例化成员内部类的对象
2. 如何在成员内部类中区分调用外部类的结构
3. 开发中局部内部类的使用

**【注意】**

1. 非static的成员内部类中的成员不能声明为static的，只有在外部类或static的成员内部类中才可声明static成员。
2. 外部类访问成员内部类的成员，需要“内部类.成员”或“内部类对象.成员”的方式
3. 成员内部类可以直接使用外部类的所有成员，包括私有的数据
4. 当想要在外部类的静态成员部分使用内部类时，可以考虑内部类声明为静态的


#### 例

```
package com.zjk;

public class InnerClassTest {
	public static void main(String[] args) {

//		创建Dog实例（静态的成员内部类）
		Person.Dog dog = new Person.Dog();
		dog.show();
//		创建Bird实例（非静态的成员内部类）
		Person p = new Person();
		Person.Bird bird = p.new Bird();
		bird.sing();
		bird.display("夜莺");
	}
}

class Person {
	String name = "小米";
	int age;

	public void eat() {
		System.out.println("eat-Person");
	}

//	成员内部类(静态)
//	abstract static class Dog {
	static class Dog {
		String name;
		int age;

		public void show() {
			System.out.println("Dog");
//			eat(); 
//			静态成员内部类不能调用外部类中非静态的结构
		}
	}

//	成员内部类(非静态)
	class Bird {
		String name = "鸟";

		public Bird() {

		}

		public void sing() {
			System.out.println("Bird-sing");
			eat();
//			即Person.this.eat();
//			调用外部类的非静态结构
		}

		public void display(String name) {
			System.out.println(name); // 方法的形参
			System.out.println(this.name); // 内部类的属性
			System.out.println(Person.this.name); // 外部类的属性
		}
	}

	public void method() {
//	局部内部类
		class AA {

		}
	}

	{
//	局部内部类
		class BB {

		}
	}

	public Person() {
//	局部内部类
		class CC {

		}
	}
}
```

### 局部内部类(开发中很少见)

- 声明局部内部类

```
class 外部类{
    方法(){
        class 局部内部类{}
    }
    {
    class 局部内部类{}
    }
}
```

- 使用局部内部类
   - 只能在声明它的方法或代码块中使用，而且是先声明后使用。除此之外的任何地方都不能使用该类
   - 但是它的对象可以通过外部方法的返回值返回使用，返回值类型只能是局部内部类的父类或父接口类型
   
**局部内部类的特点**

- 内部类仍然是一个独立的类，在编译之后内部类会被编译成独立的.class文件，但是前面冠以外部类的类名和$符号，以及数字编号。
- 只能在声明它的方法或代码块中使用，而且是先声明后使用。除此之外的任何地方都不能使用该类。
- 局部内部类可以使用外部类的成员，包括私有的。
- 局部内部类可以使用外部方法的局部变量，但是必须是final的。
    - 由局部内部类和局部变量的声明周期不同所致。
    - JDK7之前，要求此变量显式声明为final
    - JDK8之后，可以省略final的声明(自动补上)
- 局部内部类和局部变量地位类似，不能使用public,protected,缺省,private
- 局部内部类不能使用static修饰，因此也不能包含静态成员

#### 例

```
package com.zjk;

public class InnerClassTest1 {

	public void method() {
//		局部内部类
		class AA {

		}
	}

//	返回一个实现了Comparable接口的类的对象
	public Comparable getComparable() {

//		创建一个实现了Comparable接口的类
////		方式1
//		class MyComparable implements Comparable {
//
//			@Override
//			public int compareTo(Object o) {
//				// TODO Auto-generated method stub
//				return 0;
//			}
//
//		}
//		
//		return new MyComparable();
		
//		方式2 
		return new Comparable() {

			@Override
			public int compareTo(Object o) {
				// TODO Auto-generated method stub
				return 0;
			}
			
		};
	}
}
```
```
package com.zjk;

public class InnerClassTest {

	public void method() {

//		局部变量
		final int num = 10;
//		局部内部类可以使用外部方法的局部变量，但是必须是final的。
		class AA {
			public void show() {
//				num =20;
				System.out.println(num);
			}
		}
	}
}
```

### 匿名内部类

- 匿名内部类不能定义任何静态成员、方法和类，只能创建匿名内部类的一个实例。
- 一个匿名内部类一定是在new的后面，用其隐含实现一个接口或实现一个类。
- 格式：

```
new 父类构造器（实参列表）|实现接口(){
    //匿名内部类的类体部分
}
```
**匿名内部类的特点**

- 匿名内部类必须继承父类或实现接口
- 匿名内部类只能有一个对象
- 匿名内部类对象只能使用多态形式引用

## 包

**Java中包是相关类与接口的一个集合，它提供了类的命名空间的管理和访问保护。**

- 为了避免命名冲突和限定类的访问权限，将一组相关类与接口“包裹”在一起形成包(package)，体现了OOP的封装思想，为Java中管理大量的类和接口提供了方便。
- 另外，由于Java编译器要为每个类生成一个字节码文件，且文件名与类名相同，因此有可能由于同名类的存在而导致命名冲突。包的引入为Java提供了包为单位的独立命名空间，位于不同包中的类即使同名也不会冲突，从而有效地解决了命名冲突的问题。
- 同时，包具有特定的访问控制权限，同一个包中的类之间拥有特定的访问权限（protected）。


**Java的JDK提供的包主要有**

- java.applet
- java.awt 
   - java.awt.datatransfer 
   - java.awt.event,
   - java.awt.image,
- java.beans,
- java.io、
- java.lang、
- java.lang.reflect,
- java.math 
- java.net、
- java.rmi,
- java.security、
- java.sql、
- java.util
- 等。


Java平台中的类与接口都是根据功能以包组织的。每个包中都定义了许多功能相关的类和接口。我们也可以定义自己的包来实现自己的应用程序。

- Java编译器把包对应于文件系统的目录和文件管理，还可以使用ZIP或JAR压缩文件的形式保存。
    - 例如，以Windows平台为例，名为java.applet的包中，所有类文件都存储在目录classPath\java\applet下。其中包根目录一classPath由环境变量CLASSPATH来设定。

**包机制的好处主要体现在如下几点。**

- 程序员容易确定包中的类是相关的，并且容易根据所需的功能找到相应的类。
- 每个包都创建一个新的命名空间，因此不同包中的类名不会冲突。
- 同一个包中的类之间有比较宽松的访问控制。

### 定义与使用包。

#### 包的定义

使用package语句指定一个源文件中的类属于一个特定的包。package语句的格式如下：

```
package pkg1[.pkg2[.pkg3...]];
```

```
package graphics;
//Circle类成为graphics包中的一个public成员，并存放在classPath\graphics目录中。
public class Circle extends Graphic implements Draggable{
    ...
}
```

**说明：**

- package语句在每个Java源程序中只能有一条，一个类只能属于一个包。
- package语句必须在程序的第一行，该行前可以有空格或注释行
- 包名以”.“为分隔符
- 如果源文件中没有package语句，则指定为无名包。无名包没有路径，一般情况下，会把源文件中的类存储在当前目录（即存放Java源文件的目录）下。

#### 包成员的使用

包中的成员是指包中的类和接口。只有public类型的成员才能被包外的类访问，

**从包外访问public类型的成员**

- 写引入包成员或整个包，然后使用短名(short name,类名或接口名)引用包成员。
- 使用长名(long name,由包名与类/接口名组成)引用包成员。

##### import语句 引入包成员
可以先引人包中的指定类或整个包，再使用该类。这时可以直接使用类名或接口名在Java中引人包（如JDK中的包或用户自定义的包）中的类是通过import语句实现的，

**import语句的格式如下**：

```
import pkgl[.pkg2[.pkg3.].(classname|*)；
```

其中pkgl[.pkg2[.pkg3…]表明包的层次，与package语句相同，它对应于文件目录，classname则指明所要引入的类。
- 如果要从一个包中引人所有类，则可以用通配符(`*`)来代替。
- 注意：import语句必须在源程序所有类声明之前，在package语句之后。
- java包程序的一般结构如下：

```
[package语句] //默认是package.;(属于当前目录)
[import语句] //默认是import java.lang.*;
[类声明]
```

##### 包名.类名 使用长名引用包成员

要在程序中使用其他包中的类，而该包并没有引人，则必须使用长名引用该类。
- 一般只有当两个包中含有同名类时，加以区分使用。

**格式是：**

```
包名.类名
```


## 面向对象内容总结

### 生命周期


