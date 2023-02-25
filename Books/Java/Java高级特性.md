# 异常

## 异常概述

Java 中的异常又称为例外，是一个在程序执行期间发生的事件，它中断正在执行程序的正常指令流。为了能够及时有效地处理程序中的运行错误，必须使用异常类，这可以让程序具有极好的容错性且更加健壮。 

**在 Java 中一个异常的产生，主要有如下三种原因：**
 
1. Java 内部错误发生异常，Java 虚拟机产生的异常。
2. 编写的程序代码中的错误所产生的异常，例如空指针异常、数组越界异常等。
3. 通过 throw 语句手动生成的异常，一般用来告知该方法的调用者一些必要信息。

Java 通过面向对象的方法来处理异常。在一个方法的运行过程中，如果发生了异常，则这个方法会**产生代表该异常的一个对象**，并把它交给运行时的系统，运行时系统寻找相应的代码来处理这一异常。

我们把生成异常对象，并把它提交给运行时系统的过程称为拋出（throw）异常。运行时系统在方法的调用栈中查找，直到找到能够处理该类型异常的对象，这一个过程称为捕获（catch）异常。


**Java程序在执行过程中所发生的异常事件可分为两类：**

- Error：定义了在通常环境下不希望被程序捕获的异常。Java虚拟机无法解决的严重问题。
   - Error 错误是任何处理技术都无法恢复的情况，肯定会导致程序非正常终止。并且 Error 错误属于未检查类型，大多数发生在运行时。
    - 如：JVM系统内部错误、资源耗尽等严重情况。比如：StackOverflowError和OOM。
    - 一般不编写针对性的代码进行处理。
       
```java
package com.zjk;

public class ErrorTest {
	public static void main(String[] args) {
//      1.栈溢出: java.lang.StackOverflowError
//		main(args);
		
//		2.堆溢出: java.lang.OutOfMemoryError
//		Integer[] arr = new Integer[1024*1024*1024];
		
		
	}
}
```

- Exception: 其它因编程错误或偶然的外在因素导致的一般性问题，可以使用针对性的代码进行处理。
   - 例如：
   - 空指针访问
   - 试图读取不存在的文件
   - 网络连接中断
   - 数组角标越界
- Exception 又分为可检查（checked）异常和不检查（unchecked）异常，
   - 可检查异常在源码里必须显示的进行捕获处理，这里是编译期检查的一部分
   - 不检查异常就是所谓的运行时异常，通常是可以编码避免的逻辑错误，具体根据需要来判断是否需要捕获，并不会在编译器强制要求。
- 对于这些错误，一般有两种解决方法：
   - 一是遇到错误就终止程序的运行。
   - 另一种方法是由程序员在编写程序时，就考虑到错误的检测、错误消息的提示，以及错误的处理。

- 捕获错误最理想的是在编译期间，但有的错误只有在运行时才会发生。
   - 比如：除数为0，数组下标越界等
   - 分类：编译时异常和运行时异常
 
**Error（错误）和 Exception（异常）都是 java.lang.Throwable 类的子类，在 Java 代码中只有继承了 Throwable 类的实例才能被 throw 或者catch。**

## 异常体系结构

![](C:/NoteBook/pictures/291550121221050.png =690x)


- 在 Java 中所有异常类型都是内置类 java.lang.Throwable 类的子类，即 Throwable 位于异常类层次结构的顶层。
    - Throwable 类下有两个异常分支 Exception 和 Error，

![](C:/NoteBook/pictures/139441011247510.png =285x)


![](C:/NoteBook/pictures/270542822227343.png =525x)

- 运行时异常 (不检查异常（Unchecked Exception）)。
   -  是指编译器不要求强制处置的异常。一般是指编程时的逻辑错误，是程序员应该积极避免其出现的异常。
   - java.lang.RuntimeException类及它的子类都是运行时异常。
   - 对于这类异常，可以不作处理，因为这类异常很普遍，若全处理可能会对程序的可读性和运行效率产生影响。
- 编译时异常 (检查异常（Checked Exception）)
   - 是指编译器要求必须处置的异常。即程序在运行时由于外界因素造成的一般性异常。编译器要求Java程序必须捕获或声明所有编译时异常。
   - 对于这类异常，如果程序不处理，可能会带来意想不到的结果。

## 常见异常

- 运行时异常
  - java.lang.RuntimeException
 
![](C:/NoteBook/pictures/218961211240179.png =483x)

- 编译时异常
  - java.io.IOExeption
  - java.lang.ClassNotFoundException
  - java.lang.InterruptedException
  - java.io.FileNotFoundException
  - java.sql.SQLException

![](C:/NoteBook/pictures/222711311236734.png =324x)

1. 运行时异常（RuntimeException）：
- NullPropagation：空指针异常；
- ClassCastException：类型强制转换异常
- IllegalArgumentException：传递非法参数异常
- IndexOutOfBoundsException：下标越界异常
- NumberFormatException：数字格式异常

2. 非运行时异常：
- ClassNotFoundException：找不到指定 class 的异常
- IOException：IO 操作异常

3. 错误（Error）：
- NoClassDefFoundError：找不到 class 定义异常
- StackOverflowError：深递归导致栈被耗尽而抛出的异常
- OutOfMemoryError：内存溢出异常

```java
package com.zjk;

import java.io.File;
import java.io.FileInputStream;
import java.util.Scanner;

import org.junit.Test;

public class ExceptionTest {

//	**运行时异常 java.lang.RuntimeException**********
//	空指针异常：NullPointerException
	@Test
	public void test1() {
//		int[] arr = null;
//		System.out.println(arr[3]);

//		String str = "abc";
//		str = null;
//		System.out.println(str.charAt(0));
	}

//	角标越界
	@Test
	public void test2() {
//	数组角标越界：ArrayIndexOutOfBoundsException
//		int[] arr = new int[10];
//		System.out.println(arr[10]);

//  字符串角标越界：StringIndexOutOfBoundsException
//		String str = "abc";
//		System.out.println(str[3]);
	}

//	类型转换异常：ClassCastException
	@Test
	public void test3() {
//		Object obj = new Date();
//		String str = (String)obj;
	}

//	NumberFormatException
	@Test
	public void test4() {
//		String str = "abc";
//		int num = Integer.parseInt(str);
	}

//	InputMismatchException
	@Test
	public void test5() {
//		Scanner scan = new Scanner(System.in);
//		int score = scan.nextInt(); //输入的不是int数据类型
//		
//		scan.close();
	}

//	ArithmeticException
	@Test
	public void test6() {
//		int a = 10;
//		int b = 0;
//		System.out.println(a / b);
	}
	
//	**编译时异常*****************************
	@Test
	public void test7() {
//		File file = new File("hello.txt");
//		FileInputStream fis = new FileInputStream(file);
//		
//		int data = fis.read();
//		while(data != -1) {
//			System.out.println((char)data);
//			data = fis.read();
//		}
//		
//		fis.close();
	}
}
```

## 异常处理机制

- Java采用的异常处理机制，是将异常处理的程序代码集中在一起，与正常的程序代码分开，使得程序简洁、优雅，并易于维护。

**Java提供的是异常处理的抓抛模型。**

- 过程一：抛出
   - 程序在正常执行的过程中，一旦出现异常，就会在异常代码处**生成一个对应异常类的对象**，并将此对象抛出。
   - **一旦抛出对象之后，其他的代码就不执行**。
- 过程二：捕获 （异常的处理方式）
   1. try-catch-finally
   2. throws

**异常对象的生成**

由虚拟机自动生成：程序运行过程中，虚拟机检测到程序发生了问题，如果在当前代码中没有找到相应的处理程序，就会在后台自动创建一个对应常类的实例对象并抛出——自动抛出

由开发人员手动创建：Exception exception = new ClassCastException();——创建好的异常对象不抛出对程序没有任何影响，和创建一个普通对象一样

如果一个方法内抛出异常，该异常对象会被抛给调用者方法中处理。如果异常没有在调用者方法中处理，它继续被抛给这个调用方法的上层方法。这个过程将一直继续下去，直到异常被处理。这一过程称为捕获(catch)异常。

如果一个异常回到main()方法，并且main()也不处理，则程序运行终止。

程序员通常只能处理Exception，而**对Error无能为力**。

### 异常处理机制一：try-catch-finally  


- 语法:

```java
try{
    //可能出现异常的代码
}catch(异常类型1 变量名1){
    //处理异常的方式1
}catch(异常类型2 变量名2){
    //处理异常的方式2
}
...
finally{
    //一定会执行的代码
}
```

- catch 块和 finally 块都是可选的，但 catch 块和 finally 块至少出现其中之一，也可以同时出现；
   - 多个 catch 块必须位于 try 块之后，finally 块必须位于所有的 catch 块之后。 
- 使用try将可能出现异常的代码块包装起来，一旦出现异常，就会生成一个对应异常类的对象，根据此对象的类型，去catch中进行匹配
- 一旦try中的异常对象匹配到某一个catch时，就进入catch中进行异常处理。一旦处理完成，就跳出当前的try-catch结构（在没有finally的情况下）。进行执行其后的代码。**如果 try 代码块中拋出的异常没有被任何 catch 子句捕捉到，那么将直接执行 finally 代码块中的语句，并把该异常传递给该方法的调用者。finally不管有没有捕获异常都会执行**。
- catch中的异常类型如果满足子父类关系，则要求子类一定声明在父类的上面，否则报错
   - catch中的异常类型如果没有子父类关系，则谁声明在上无所谓

- 在try结构中声明定义的对象，在出了try结构以后，不能被调用
   - 可以在try结构外面先对改变量进行声明初始化 
- try-catch-finally结构可以相互嵌套

**总结**

- 体会1： 使用try-catch-finally处理编译时异常，使得程序在编译时就不再报错，但是运行时仍可能报错，相当于我们使用try-catch-finally将第一个编译时可能出现的异常，延迟到运行时出现(相当于把一个编译时异常变为运行时异常)
- 体会2：开发中，由于运行时异常比较常见，所以我们不针对运行时异常编写try-catch-finally。
   - 针对编译时异常，我们一定要考虑异常的处理。

**常用的异常对象处理方式：**
 
1. `getMessage()`获取异常信息，返回字符串
2. `printStackTrace()` 获取异常类名和异常信息，以及异常出现在程序中的位置。返回值void。

```java
package com.zjk;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

import org.junit.Test;

public class ExceptionTest1 {
	@Test
	public void test1() {

		String str = "abc";

		int num = 0;
		try {
			num = Integer.parseInt(str);
//			int num = Integer.parseInt(str);

//			抛出异常,一旦处理完成，就跳出当前的try-catch结构
//			没有执行下面的语句
			System.out.println("1");
		} catch (NumberFormatException e) {
			System.out.println("出现数值转换异常");
//			String getMessage()
			System.out.println(e.getMessage());
//			printStackTrace
			e.printStackTrace();
		} catch (NullPointerException e) {
			System.out.println("出现空指针异常");
		} catch (Exception e) {
			System.out.println("出现异常");
		}
//	    } catch (Exception e) {
//			System.out.println("出现异常");
//		//Unreachable catch block
//		catch中的异常类型如果满足子父类关系，则要求子类一定声明在父类的上面，否则报错
//		} catch (NumberFormatException e) {
//			System.out.println("出现数值转换异常");
//		}
//		执行
		System.out.println("2");

//		System.out.println(num);
	}

	@Test
	public void test2() {
		try {
			File file = new File("hello.txt");
			FileInputStream fis = new FileInputStream(file);

			int data = fis.read();
			while (data != -1) {
				System.out.println((char) data);
				data = fis.read();
			}

			fis.close();
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```

#### finally的使用

finally语句为异常处理提供一个统一的出口，使得在控制流转到程序的其它部分以前，能够对程序的状态作统一的管理。

- finally是可选的
- **finally中声明的是一定会被执行的代码**。
   - 即使catch中又出现异常了，try或catch中有return语句等情况。
   - 除非在 try 块、catch 块中调用了退出虚拟机的方法`System.exit(int status)`，否则不管在 try 块或者 catch 块中执行怎样的代码，出现怎样的情况，异常处理的 finally 块总会执行。
- 像数据库连接，输入输出流（IO），网络编程Socket等资源，JVM是不能自动回收的，我们需要自己手动的进行资源的释放。此时的**资源释放**，就需要声明在finally中。(注：Java 9增强的自动资源管理)

![](C:/NoteBook/pictures/176842210221051.png =421x)

```java
package com.zjk;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

import org.junit.Test;

public class ExceptionTest1 {
	@Test
	public void test1() {

		String str = "abc";

		int num = 0;
		try {
			num = Integer.parseInt(str);
//			int num = Integer.parseInt(str);

//			抛出异常,一旦处理完成，就跳出当前的try-catch结构
//			没有执行下面的语句
			System.out.println("1");
		} catch (NumberFormatException e) {
			System.out.println("出现数值转换异常");
//			String getMessage()
			System.out.println(e.getMessage());
//			printStackTrace
			e.printStackTrace();
		} catch (NullPointerException e) {
			System.out.println("出现空指针异常");
		} catch (Exception e) {
			System.out.println("出现异常");
		}
//	    } catch (Exception e) {
//			System.out.println("出现异常");
//		//Unreachable catch block
//		catch中的异常类型如果满足子父类关系，则要求子类一定声明在父类的上面，否则报错
//		} catch (NumberFormatException e) {
//			System.out.println("出现数值转换异常");
//		}
//		执行
		System.out.println("2");

//		System.out.println(num);
	}

	@Test
	public void test2() {
		try {
			File file = new File("hello.txt");
			FileInputStream fis = new FileInputStream(file);

			int data = fis.read();
			while (data != -1) {
				System.out.println((char) data);
				data = fis.read();
			}

			fis.close();
		} catch (FileNotFoundException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
}
```

##### 增强的自动资源管理

###### Java7
**当 try 代码块结束时，自动释放资源。不再需要显式的调用 close() 方法，该形式也称为“带资源的 try 语句”。**

- 当程序使用 finally 块关闭资源时，程序会显得异常臃肿
   - 为了解决这种问题，Java 7 增加了一个新特性，该特性提供了另外一种管理资源的方式，这种方式能自动关闭文件，被称为自动资源管理（Automatic Resource Management）。该特性是在 try 语句上的扩展，主要释放不再需要的文件或其他资源。

- 自动资源管理替代了 finally 代码块，并优化了代码结构和提高程序可读性。
- 语法

```java
try (声明或初始化资源语句) {
    // 可能会生成异常语句
} catch(Throwable e1){
    // 处理异常e1
} catch(Throwable e2){
    // 处理异常e1
} catch(Throwable eN){
    // 处理异常eN
}
```

**注意：**

- try 语句中声明的资源被隐式声明为 final，资源的作用局限于带资源的 try 语句。
- 可以在一条 try 语句中声明或初始化多个资源，每个资源以;隔开即可。
- 需要关闭的资源必须实现了 AutoCloseable 或 Closeable 接口。
   - Closeable 是 AutoCloseable 的子接口，Closeable 接口里的 close() 方法声明抛出了 IOException，因此它的实现类在实现 close() 方法时只能声明抛出 IOException 或其子类；AutoCloseable 接口里的 close() 方法声明抛出了 Exception，因此它的实现类在实现 close() 方法时可以声明抛出任何异常。
   

Java 7 几乎把所有的“资源类”（包括文件 IO 的各种类、JDBC 编程的 Connection 和 Statement 等接口）进行了改写，改写后的资源类都实现了 AutoCloseable 或 Closeable 接口。

```java
public class AutoCloseTest {
    public static void main(String[] args) throws IOException {
        try (
                // 声明、初始化两个可关闭的资源
                // try语句会自动关闭这两个资源
                BufferedReader br = new BufferedReader(new FileReader("AutoCloseTest.java"));
                PrintStream ps = new PrintStream(new FileOutputStream("a.txt"))) {
            // 使用两个资源
            System.out.println(br.readLine());
            ps.println("C语言中文网");
        }
    }
}
```

###### Java9

Java 9 再次增强了这种 try 语句。Java 9 不要求在 try 后的圆括号内声明并创建资源，只需要自动关闭的资源有 final 修饰或者是有效的 final (effectively final)，Java 9 允许将资源变量放在 try 后的圆括号内。上面程序在 Java 9 中可改写为如下形式。

```java
public class AutoCloseTest {
    public static void main(String[] args) throws IOException {
        // 有final修饰的资源
        final BufferedReader br = new BufferedReader(new FileReader("AutoCloseTest.java"));
        // 没有显式使用final修饰，但只要不对该变量重新赋值，该变量就是有效的
        final PrintStream ps = new PrintStream(new FileOutputStream("a. txt"));
        // 只要将两个资源放在try后的圆括号内即可
        try (br; ps) {
            // 使用两个资源
            System.out.println(br.readLine());
            ps.println("C语言中文网");
        }
    }
}
```


#### 练习1

```java
编写一个类ExceptionTest，在main方法中使用try、catch、finally，要求：
在try块中，编写被零除的代码。
在catch块中，捕获被零除所产生的异常，并且打印异常信息
在finally块中，打印一条语句。
```

```java
public class ExceptionTest01 {

	public static void main(String[] args) {
		int a = 1;
		int b = 0;
		try {
			System.out.println(a / b);
		} catch (ArithmeticException e) {
			e.printStackTrace();
		}finally {
			System.out.println("finally");
		}
	}
}
```

#### 练习2 捕获和处理IOException异常

编译、运行应用程序IOExp.java，体会Java语言中异常的捕获和处理机制。

相关知识：FileInputStream类的成员方法read()的功能是每次从相应的(本地为ASCII码编码格式)文件中读取一个字节，并转换成0~255之间的int型整数返回，到达文件末尾时则返回-1

```java

```

### 异常处理机制二：throws + 异常类型

- `throws + 异常类型`(声明抛出异常)写在方法的声明处，指明此方法执行时，可能会抛出的异常类型
   - 一旦当方法体执行时，出现异常仍然会在异常代码处生成一个异常类的对象，此对象满足throws后异常类型时，就会被抛出。异常代码后续的代码就不再执行。
   - 使用 throws 声明抛出异常的思路是，当前方法不知道如何处理这种类型的异常，该异常应该**由向上一级的调用者处理**；如果 main 方法也不知道如何处理这种类型的异常，也可以使用 throws 声明抛出异常，该异常将交给 JVM 处理。JVM 对异常的处理方法是，打印异常的跟踪栈信息，并中止程序运行，这就是前面程序在遇到异常后自动结束的原因。
-  体会：
   - try-catch-finally：真正地将异常给处理了
   - throws地方式只是将异常抛给了方法的调用者，并没有真正将异常处理掉 。

![](C:/NoteBook/pictures/162353610239477.png =436x)

#### 例

```java
package com.zjk;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

public class ExceptionTest2 {

	public static void main(String[] args) {
		try {
			method2();
//		method2()的异常抛给main()方法
		} catch (IOException e) {
			e.printStackTrace();
		}
		
		method3();
//		在method3()中解决异常，不再往上抛

	}

	public static void method3() {
		try {
			method2();
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	
	public static void method2() throws IOException {
		method1();
//		method1()的异常抛给method2()
	}

	public static void method1() throws FileNotFoundException, IOException {
		File file = new File("hello1.txt");
		FileInputStream fis = new FileInputStream(file);

		int data = fis.read();
		while (data != -1) {
			System.out.println((char) data);
			data = fis.read();
		}

		fis.close();
		
		System.out.println("method1");//不执行
//		满足throws后异常类型时，就会被抛出。异常代码后续的代码就不再执行。
	}
}
```

#### 重写方法异常抛出的规则

- 子类重写的方法抛出的异常类型不大于父类被重写的方法抛出的异常类型

```java
package com.zjk;

import java.io.FileNotFoundException;
import java.io.IOException;

public class OverrideTest {
	
	public static void main(String[] args) {
		OverrideTest test = new OverrideTest();
		test.display(new SubClass());
	}

	public void display(SuperClass s) {
		try {
			s.method();
		} catch (IOException e) {
//			只处理了父类抛出的异常，如果子类重写的方法抛出的异常大于父类的异常
//			则子类在调用时出现异常的无法被处理，
			e.printStackTrace();
		}
	}
}

class SuperClass{
	
	public void method() throws IOException{
		
	}
}


class SubClass extends SuperClass{
	@Override
	public void method() throws FileNotFoundException {
//	public void method() throws Exception {
//		子类重写父类的方法，抛出的异常不能大于父类抛出的异常
//		如果父类没有抛出异常，子类也不能抛出
		
	}
}
```

### 开发中选择使用哪一个处理

- 如果父类中被重写的方法没有被throws方式处理异常，则子类重写的方法也不能使用throws，意味着如果子类重写的方法中有异常，必须使用try-catch-finally方式处理。
- 执行的方法中，先后又调用了另外的几个方法，这几个方法是递进关系执行的（A调用B，B调用C...），我们建议这几个方法使用throws的方式进行处理，而执行的方法A可以考虑使用try-catch-finally的方式进行处理。
   - 如果在C方法中使用了try-catch-finally，则C方法中有的代码可能没有执行，传递给B方法的数据可能有误（或没有数据），但B方法仍然执行，但无意义。所以在B,C方法中使用throws，而在A中处理。如果B,C方法中出现异常，则立刻终止执行，将异常抛给A方法解决。 

### 手动抛出异常 throw

- Java异常类对象除在程序执行过程中出现异常时由系统自动生成并抛出，也可根据需要使用人工创建并抛出。
- 首先要生成异常类对象，然后通过throw语句实现抛出操作(提交给Java运行环境)。

- 当 throw 语句执行时，它后面的语句将不执行，此时程序转向调用者程序，寻找与之相匹配的 catch 语句，执行相应的异常处理程序。如果没有找到相匹配的 catch 语句，则再转向上一层的调用程序。这样逐层向上，直到最外层的异常处理程序终止程序并打印出调用栈情况。

- throw 关键字不会单独使用，它的使用完全符合异常的处理机制，但是，一般来讲用户都在避免异常的产生，所以不会手工抛出一个新的异常类的实例，而往往会抛出程序中已经产生的异常类的实例。

```java
IOException e = new IOException();
throw e;
```

- 可以抛出的异常必须是Throwable或其子类的实例.

```java
package com.zjk;

public class StudentTest {

	public static void main(String[] args) {
		Student s = new Student();

		try {
			s.regist(-1);
			System.out.println(s);
		} catch (Exception e) {
			// TODO Auto-generated catch block
			System.out.println(e.getMessage());
		}
	}
}

class Student {

	private int id;

	public void regist(int id) throws Exception {
		if (id > 0) {
			this.id = id;
		} else {
//			手动抛出异常对象
//			throw new RuntimeException("数据输入非法");
			throw new Exception("输入数据非法");
		}
	}

	@Override
	public String toString() {
		return "Student [id=" + id + "]";
	}

}
```

#### 例 

在某仓库管理系统中，要求管理员的用户名需要由 8 位以上的字母或者数字组成，不能含有其他的字符。当长度在 8 位以下时拋出异常，并显示异常信息；当字符含有非字母或者数字时，同样拋出异常，显示异常信息。代码如下：

```java
import java.util.Scanner;

public class Test05 {
    public boolean validateUserName(String username) {
        boolean con = false;
        if (username.length() > 8) {
            // 判断用户名长度是否大于8位
            for (int i = 0; i < username.length(); i++) {
                char ch = username.charAt(i); // 获取每一位字符
                if ((ch >= '0' && ch <= '9') || (ch >= 'a' && ch <= 'z') || (ch >= 'A' && ch <= 'Z')) {
                    con = true;
                } else {
                    con = false;
                    throw new IllegalArgumentException("用户名只能由字母和数字组成！");
                }
            }
        } else {
            throw new IllegalArgumentException("用户名长度必须大于 8 位！");
        }
        return con;
    }

    public static void main(String[] args) {
        Test05 te = new Test05();
        Scanner input = new Scanner(System.in);
        System.out.println("请输入用户名：");
        String username = input.next();
        try {
            boolean con = te.validateUserName(username);
            if (con) {
                System.out.println("用户名输入正确！");
            }
        } catch (IllegalArgumentException e) {
            System.out.println(e);
        }
    }
}
```

#### throws 关键字和 throw 关键字在使用上的几点区别如下：

**throws**

- throws 处理异常的方式。使用在方法声明处的末尾。
   - 用来声明一个方法可能抛出的所有异常信息，表示出现异常的一种可能性，但并不一定会发生这些异常；
- 通常在一个方法（类）的声明处通过 throws 声明方法（类）可能拋出的异常信息，
- throws 通常不用显示地捕获异常，可由系统自动将所有捕获的异常信息抛给上级方法； <-> try-catch-finally

**throw**

- throw 抛出一个异常对象，并抛出。使用在方法内部<->自动抛出
- throw 则是指拋出的一个具体的异常类型，执行 throw 一定抛出了某种异常对象。 
- 在方法（类）内部通过 throw 声明一个具体的异常信息。
- throw 则需要用户自己捕获相关的异常，而后再对其进行相关包装，最后将包装后的异常信息抛出。

### Java 7新特性：多异常捕获

使用一个 catch 块捕获多种类型的异常时需要注意如下两个地方。

- 捕获多种类型的异常时，多种异常类型之间用竖线|隔开。
- 捕获多种类型的异常时，异常变量有隐式的 final 修饰，因此程序不能对异常变量重新赋值。

```java
public class ExceptionTest {
    public static void main(String[] args) {
        try {
            int a = Integer.parseInt(args[0]);
            int b = Integer.parseInt(args[1]);
            int c = a / b;
            System.out.println("您输入的两个数相除的结果是：" + c);
        } catch (IndexOutOfBoundsException | NumberFormatException | ArithmeticException e) {
            System.out.println("程序发生了数组越界、数字格式异常、算术异常之一");
            // 捕获多异常时，异常变量默认有final修饰
            // 所以下面代码有错
            e = new ArithmeticException("test");
        } catch (Exception e) {
            System.out.println("未知异常");
            // 捕获一种类型的异常时，异常变量没有final修饰
            // 所以下面代码完全正确
            e = new RuntimeException("test");
        }
    }
}
```

### 用户自定义异常类

- 一般地，用户自定义异常类都是RuntimeException的子类。
- 自定义异常类通常需要**编写几个重载的构造器**。
- 自定义异常需要提供**serialVersionUID**
- 自定义的异常通过throw抛出。
- 自定义异常最重要的是异常类的名字，当异常出现时，可以根据名字判断异常类型

如果 Java 提供的内置异常类型不能满足程序设计的需求，这时我们可以自己设计 Java 类库或框架，其中包括异常类型。实现**自定义异常类需要继承 Exception 类或其子类，如果自定义运行时异常类需继承 RuntimeException 类或其子类**。

**自定义异常的语法**形式为：

`<class><自定义异常名><extends><Exception>`

- 在编码规范上，一般将自定义异常类的类名命名为**XXXException**，其中 XXX 用来代表该异常的作用。
- 自定义异常类一般包含两个构造方法：
   - 一个是无参的默认构造方法，
   - 另一个构造方法以字符串的形式接收一个定制的异常消息，并将该消息传递给超类的构造方法。
   
```java
class IntegerRangeException extends Exception {
    public IntegerRangeException() {
        super();
    }
    public IntegerRangeException(String s) {
        super(s);
    }
}
```

```java
package com.zjk;

public class MyException extends RuntimeException{
	
	static final long serialVersionUID = 1L; 
//	对该类的唯一标识
	
	public MyException(){
		
	}
	
	public MyException(String msg) {
		super(msg);
	}
}


package com.zjk;

public class StudentTest {

	public static void main(String[] args) {
		Student s = new Student();

		try {
			s.regist(-1);
			System.out.println(s);
		} catch (Exception e) {
			// TODO Auto-generated catch block
			System.out.println(e.getMessage());
		}
	}
}

class Student {

	private int id;

//	public void regist(int id) throws Exception {
//	非运行时异常需要throws语句声明
    public void regist(int id) {
		if (id > 0) {
			this.id = id;
		} else {
//			手动抛出异常对象
//			throw new RuntimeException("数据输入非法");
			throw new MyException("输入数据非法");
//			throw的只能是Exception或其（间接或直接）子类的异常
		}
	}

	@Override
	public String toString() {
		return "Student [id=" + id + "]";
	}

}
```

####  例

```java
例 1
编写一个程序，对会员注册时的年龄进行验证，即检测是否在 0~100 岁。

1）这里创建了一个异常类 MyException，并提供两个构造方法。MyException 类的实现代码如下：
public class MyException extends Exception {
    public MyException() {
        super();
    }
    public MyException(String str) {
        super(str);
    }
}

2）接着创建测试类，调用自定义异常类。代码实现如下：
import java.util.InputMismatchException;
import java.util.Scanner;
public class Test07 {
    public static void main(String[] args) {
        int age;
        Scanner input = new Scanner(System.in);
        System.out.println("请输入您的年龄：");
        try {
            age = input.nextInt();    // 获取年龄
            if(age < 0) {
                throw new MyException("您输入的年龄为负数！输入有误！");
            } else if(age > 100) {
                throw new MyException("您输入的年龄大于100！输入有误！");
            } else {
                System.out.println("您的年龄为："+age);
            }
        } catch(InputMismatchException e1) {
            System.out.println("输入的年龄不是数字！");
        } catch(MyException e2) {
            System.out.println(e2.getMessage());
        }
    }
}
3）运行该程序，当用户输入的年龄为负数时，则拋出 MyException 自定义异常，执行第二个 catch 语句块中的代码，打印出异常信息。程序的运行结果如下所示。
请输入您的年龄：
-2
您输入的年龄为负数！输入有误！

当用户输入的年龄大于 100 时，也会拋出 MyException 自定义异常，同样会执行第二个 catch 语句块中的代码，打印出异常信息，如下所示。
请输入您的年龄：
110
您输入的年龄大于100！输入有误！

在该程序的主方法中，使用了 if…else if…else 语句结构判断用户输入的年龄是否为负数和大于 100 的数，如果是，则拋出自定义异常 MyException，调用自定义异常类 MyException 中的含有一个 String 类型的构造方法。在 catch 语句块中捕获该异常，并调用 getMessage() 方法输出异常信息。

提示：因为自定义异常继承自 Exception 类，因此自定义异常类中包含父类所有的属性和方法。
```

#### 练习4

```java
编写应用程序EcmDef.java，接收命令行的两个参数，要求不能输入负数，计算两数相除。
对 数 据 类 型 不 一 致 (NumberFormatException) 、 
缺 少 命 令 行 参 数(ArrayIndexOutOfBoundsException、
除0(ArithmeticException)
及输入负数(EcDef 自定义的异常)
进行异常处理。

提示：
(1)在主类(EcmDef)中定义异常方法(ecm)完成两数相除功能。
(2)在main()方法中使用异常处理语句进行异常处理。
(3)在程序中，自定义对应输入负数的异常类(EcDef)。
(4)运行时接受参数 java EcmDef 20 10 //args[0]=“20” args[1]=“10”
(5)Interger类的static方法parseInt(String s)将s转换成对应的int值。
如：int a=Interger.parseInt(“314”); //a=314;
```

```java
package com.zjk1;

public class EcmDef {

	public static void main(String[] args) {

		try {
			int a = Integer.parseInt(args[0]);
			int b = Integer.parseInt(args[1]);

			int result = ecm(a, b);
			System.out.println(result);
		} catch (NumberFormatException e) {
			System.out.println("数据类型不一致");
		} catch (ArrayIndexOutOfBoundsException e) {
			System.out.println("缺少命令行参数");
		} catch (ArithmeticException e) {
			System.out.println("除0");
		}catch(EcDef e) {
			System.out.println(e.getMessage());
		}

	}

	public static int ecm(int a, int b) throws EcDef {

		if (a < 0 || b < 0) {
			throw new EcDef("分子或分母为负数");
		}
		return a / b;
	}
}

class EcDef extends Exception {
	public final long serverVesionUID = 1L;

	public EcDef() {
		super();
	}

	public EcDef(String msg) {
		super(msg);
	}
}
```

## assert 断言

- java中提供了专门的assert语句，为java程序提供了一种错误检查机制。
- **每个断言都包含了一个boolean表达式**。
  - 如果程序没有错误，则运行assert语句时，该表达式的值应该为true
  - 如果该表达式的值为false，则系统将抛出一个错误。
- **断言是发现程序错误最快最有效的方法之一**。
   - 相当程序内部处理的文档，增强了程序的可维护性 

### 语法定义

1.

```java
assert boolean表达式;
```

- 当系统运行时，将求出该boolean表达式的值，
    - 如果是false，说明系统处于不正确的状态，将抛出一个没有任何详细信息的AssertionError类型的错误，并且退出
    - 如果是true，进行执行。
    
2.

```java
assert boolean表达式1 : 表达式2;
```

- 表达式2可以是：boolean/char/double/float/int/long基本类型的值或者一个Object类型的对象。
   - 比较常见的是一个描述错误的字符串。
- 当系统运行时，且boolean表达式1的值为false时，
  - 则系统计算出表达式2的值，然后以该boolean表达式2的值为参数调用AssertionError类的构造方法，创建一个包含详细描述信息的**AssetError对象抛出并退出**。
 
### 执行

- 断言语句运行时，默认不执行

**打开断言检查：**

```java
java -ea 类/包
或
java -enableassertions 类/包
```

**关闭断言检查: **

```java
java -da 类/包
或
java -disableassertions 类/包
```

- 如果在`-ea或-da`选项后面无参数，则将对程序中除了系统类之外的所有其他类都打开/关闭断言检查。

### 使用

**1.保证控制流的正确性**

- 在if else语句和switch语句中，可以在不应该执行的控制流下，使用assert false语句，使得如果控制流异常，则报AssertionError异常。

```java
public class AssertDemo {
    public static void main(String[] args) {
        int number = 3;
        switch (number) {
            case 1:
                System.out.println(1);
                break;
            case 2:
                System.out.println(2);
                break;
            default:
                assert false : "该数字为其他数字，不是1 or 2";
                break;
        }
    }
}

****************************************
G:\vim_test>javac AssertionDemo.java

G:\vim_test>java -ea AssertionDemo.java
Exception in thread "main" java.lang.AssertionError: 该数字为其他数字，不是1 or 2
        at AssertionDemo.main(AssertionDemo.java:10)
```

**2.检查私有方法输入参数的有效性**

- 在私有方法调用时，会直接使用传入的参数。如果私有方法对参数有特定要求，可在方法开始处使用断言进行参数检查。

```java
public class AssertDemo {
    public static void main(String[] args) {
        AssertDemo assertDemo = new AssertDemo();
        assertDemo.printX(null);
    }

    public int printX(String num) {
        assert num != null;
        return 1;
    }
}
```

**3.检查方法的返回结果是否有效**

- 对于一些计算型方法，可以通过断言语句在方法返回前，检查返回值是否满足必要的性质。

```java
public class AssertDemo {
    public static void main(String[] args) {
        AssertDemo assertDemo = new AssertDemo();
        assert (assertDemo.printX() >= 0) : "不能小于0";
    }

    public int printX() {
        Scanner scan = new Scanner(System.in);
        int x = scan.nextInt();

        return x;
    }
}
```

**4.检查程序不变量**

- 程序不变量(invariant))是在程序某个特定点或某些特定点都保持为真的一种特性。
- 不变量反映程序的特性，通过分析程序关键点上的不变量，可以监测到程序运行中的异常。

```java
public class AssertDemo {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
        int x = scan.nextInt();

        if(x > 0){
            System.out.println("ok");
        }
        else{
            assert  (x <= 0) : "x<=0";
        }
    }
}
```
 
## Java验证用户名和密码

```java
假设在某仓库管理系统的登录界面中需要输入用户名和密码，其中用户名只能由 6~10 位数字组成，密码只能有 6 位，任何不符合用户名或者密码要求的情况都视为异常，并且需要捕获并处理该异常。

下面使用自定义异常类来完成对用户登录信息的验证功能，实现步骤如下。

1）编写自定义异常类 LoginException，该类继承自 Exception。在 LoginException 类中包含两个构造方法，分别为无参的构造方法和含有一个参数的构造方法，代码如下：
public class LoginException extends Exception {
    public LoginException() {
        super();
    }
    public LoginException(String msg) {
        super(msg);
    }
}

2）创建测试类 Test08，在该类中定义 validateLogin() 方法，用于对用户名和密码进行验证。当用户名或者密码不符合要求时，使用自定义异常类 LoginException 输出相应的异常信息。validateLogin() 方法的定义如下：
public boolean validateLogin(String username,String pwd) {
    boolean con = false;    // 用户名和密码是否正确
    boolean conUname = false;    // 用户名格式是否正确
    try {
        if (username.length() >= 6 && username.length() <= 10) {
            for (int i = 0;i < username.length();i++) {
                char ch = username.charAt(i);    // 获取每一个字符
                if (ch >= '0' && ch <= '9') {    // 判断字符是否为0~9的数字
                    conUname = true;    // 设置 conUname 变量值为 true
                } else {    
                    // 如果字符不是0~9的数字，则拋出LoginException异常
                    conUname = false;
                    throw new LoginException("用户名中包含有非数字的字符！");
                }
            }
        } else {    
            // 如果用户名长度不在6~10位之间，拋出异常
            throw new LoginException("用户名长度必须在6〜10位之间！");
        }
        if (conUname) {    // 如果用户名格式正确，判断密码长度
            if (pwd.length() == 6) {    
                // 如果密码长度等于6
                con=true;    // 设置con变量的值为true，表示登录信息符合要求
            } else {    // 如果密码长度不等于6，拋出异常
                con = false;
                throw new LoginException("密码长度必须为 6 位！");
            }
        }
    } catch(LoginException e) {    
        // 捕获 LoginException 异常
        System.out.println(e.getMessage());
    }
    return con;
}
3）在 Test08 类中添加 main() 方法，调用 validateLogin() 方法，如果该方法返回 true，则输出登录成功的信息。main() 方法的定义如下：
public static void main(String[] args) {
    Scanner input = new Scanner(System.in);
    System.out.println("用户名：");
    String username = input.next();
    System.out.println("密码：");
    String password = input.next();
    Test08 lt = new Test08 ();
    boolean con = lt.validateLogin(username,password);    // 调用 validateLogin() 方法
    if (con) {
        System.out.println("登录成功！");
    }
}

在本程序的 validateLogin() 方法中使用条件控制语句和  for 循环语句分别对用户名和密码进行了验证。任何不符合用户名或者密码要求的情况都拋出自定义异常 LoginException，并在 catch 语句中捕获该异常，输出异常信息。

运行程序，当用户输入的用户名含有非数字字符时将拋出 LoginException 异常，执行 catch 语句块中的代码打印异常信息，如下所示。
用户名：
xiake8!
密码：
123456
用户名中包含有非数字的字符！

当用户输入的用户名长度不在 6~10 位时同样会拋出 LoginException 异常并打印异常信息，如下所示。
用户名：
administrator
密码：
123456
用户名长度必须在6~10位之间！

当用户输入的登录密码不等于 6 位时也会拋出 LogWException 异常，并打印出异常信息，如下所示。
用户名：
20181024
密码：
12345
密码长度必须为 6 位！

当用户输入的用户名和密码都符合要求时，则打印登录成功的信息，如下所示。
用户名：
20181024
密码：
123456
登录成功！
```

## 总结：异常处理5个关键字

![](C:/NoteBook/pictures/150934310227344.png =456x)

## 项目三 《开发团队调度软件》

# 多线程

## 概念

**程序，进程，线程**

- 程序(program)是为完成特定任务、用某种语言编写的一组指令的集合。
    - 即指一段静态的代码，静态对象。
    - 作为执行蓝本的同一段程序，可以被多次加载到系统的不同内存区域执行，形成不同进程。
- **进程(process)是程序的一次执行过程，或是正在运行的一个程序。是由代码，数据，内核状态和一组寄存器组成**
    - 进程是一个内核级的实体，进程结构的所有成分都在内核空间中。
        - 一个用户程序不能直接访问这些数据   
    - 是一个程序的一次动态执行过程：有它自身的产生、存在和消亡的过程。——生命周期
        - **一个进程在其执行的过程中，可以产生多个线程，形成多个执行流。每个线程也有其生命周期。**
    - **进程作为资源分配的单位，系统在运行时会为每个进程分配不同的内存区域**
- 线程(thread)：进程可进一步细化为线程（轻型进程），是一个程序内部的一条执行路径（执行流）。是由表示程序运行状态的寄存器（程序计数器，栈指针等）以及堆栈组成。线程是计算过程在某一时刻的状态。
    - 线程是一个用户级的实体，线程结构驻留在用户空间中，
        - 能被普通的用户级函数直接访问。 
    - 若一个进程同一时间并行执行多个线程，就是支持多线程的。
    - 线程作为调度和执行的单位，每个线程拥有独立的运行栈和程序计数器(pc)，线程切换的开销小
    - 一个进程中的多个线程共享相同的内存单元/内存地址空间，线程包含进程地址空间中的代码和数据。（多个线程共享同一个进程中的方法区和堆）
        - 它们从同一堆中分配对象，可以访问相同的变量和对象。这就使得线程间通信更简便、高效。但多个线程操作共享的系统资源可能就会带来安全的隐患
        - 当两个线程对同一个对象进行访问时，他们将共享数据。
    - **一个执行流是由CPU运行程序代码并操作程序的数据所形成的**，
       - 线程被认为是以CPU为主体的行为，  

**单核CPU和多核CPU的理解**

- 单核CPU，其实是一种假的多线程，因为在一个时间单元内，也只能执行一个线程的任务（可以将一个线程挂起，先执行其他的线程），但是因为CPU时间单元特别短，因此感觉不出来。
- 如果是多核的话，才能更好的发挥多线程的效率。（现在的服务器(Linux,Windos Server)都是多核的）
- 一个Java应用程序java.exe，其实**至少有三个线程：main()主线程，gc()垃圾回收线程，异常处理线程**。
   - 当然如果发生异常，会影响主线程。

**并行与并发**

- 并行：多个CPU同时执行多个任务。
- 并发：一个CPU(采用时间片)同时执行多个任务。

**使用多线程的优点**

1.  提高应用程序的响应。对图形化界面更有意义，可增强用户体验。
2.  提高计算机系统CPU的利用率
3.  改善程序结构。将既长又复杂的进程分为多个线程，独立运行，利于理解和修改

**何时需要多线程**

- 程序需要同时执行两个或多个任务。
- 程序需要实现一些需要等待的任务时，如用户输入、文件读写操作、网络操作、搜索等。
- 需要一些后台运行的程序时。

## 线程的创建和使用

- Java语言的JVM允许程序运行多个线程，它通过`java.lang.Thread类`来体现。

**Thread类的特性**

- 每个线程都是通过某个特定Thread对象的run()方法来完成操作的，经常把run()方法的主体称为线程体
- 通过该Thread对象的start()方法来启动这个线程，而非直接调用run()

**Thread类构造器**

- Thread()：创建新的Thread对象
- Thread(String threadname)：创建线程并指定线程实例名
- Thread(Runnable target)：指定创建线程的目标对象，它实现了Runnable接口中的run方法
- Thread(Runnable target, String name)：创建新的Thread对象
- Thread(TreadGroup group,Runnable target,String name)  
     - 指明该线程所属的线程组 
     - 提供线程体的对象。
     - 线程名称

**API中创建线程的两种方式**

JDK1.5之前创建新执行线程有两种方法：

- 继承Thread类的方式
- 实现Runnable接口的方式

### 方式一：继承Thread类

1. 定义子类继承Thread类。
2. 子类中重写Thread类中的run方法。
   - 将此线程执行的操作声明在run()中
3. 创建Thread子类对象，即创建了线程对象。
4. 调用线程对象start方法：启动线程，调用run方法。

**注意点：**

1. 如果自己手动调用run()方法，那么就只是普通方法，没有启动多线程模式。
2. run()方法由JVM调用，什么时候调用，执行的过程控制都有操作系统的CPU调度决定。
3. 想要启动多线程，必须调用start方法。
4. **一个线程对象只能调用一次start()方法启动，如果重复调用了，则将抛出以上的异常“IllegalThreadStateException”**

#### 例1

```java
public class ThreadTest {
    public static void main(String[] args) { //主线程
//     3.创建Thread类的子类的对象 //主线程
        MyThread t1 = new MyThread();

//        4.通过此对象调用start() //主线程
        t1.start();
//        在主线程中创建t1对象，在当前线程中执行(与main线程一起执行)
//        (1.)启动当前线程
//        (2.)调用当前线程的run()

//        不能通过直接调用run()的方式启动线程
//        t1.run();
//        若直接调用run()则视为直接调用方法，而不是开启线程

//        再启动一个线程
//        t1.start();
//        不能令已经调用start()的对象来再启动一个线程，否则报错
//        需要重新创建一个线程对象来调用start()
        MyThread t2 = new MyThread();
        t2.start();

        //以下仍然是在main线程中执行的。
        for (int i = 0; i < 100; i++) {
            if (i % 2 != 0) {
                System.out.println(i + "*");
                System.out.println(Thread.currentThread().getName());
            }
        }
    }
}

//1.创建Thread类的子类
class MyThread extends Thread {
    @Override
//    2.重写run()方法
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) {
                System.out.println(i + "-");
                System.out.println(Thread.currentThread().getName());
            }
        }
    }
}
```

#### 练习

创建两个分线程，让其中一个线程输出1-100之间的偶数，另一个线程输出1-100之间的奇数。

```java
public class ThreadDemo {
    public static void main(String[] args) {
        MyThread1 t1 = new MyThread1();
        MyThread2 t2 = new MyThread2();

        t1.start();
        t2.start();
    }
}

class MyThread1 extends Thread {
    @Override
    public void run() {
        super.run();
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ":" + i);
            }
        }
    }
}

class MyThread2 extends Thread {
    @Override
    public void run() {

        for (int i = 0; i < 100; i++) {
            if (i % 2 != 0) {
                System.out.println(Thread.currentThread().getName() + ":" + i);
            }
        }
    }
}
```

#### 例：以Thread类的匿名子类的方式

```java
public class ThreadDemo {
    public static void main(String[] args) {
//        以Thread类的匿名子类的方式
        new Thread() {
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    System.out.println(Thread.currentThread().getName() + ":" + i);
                }
            }
        }.start();
        new Thread() {
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    System.out.println(Thread.currentThread().getName() + ":" + i);
                }
            }
        }.start();
    }
}
```

#### 例

模拟一个人生产50个玩具，每200毫秒生产一个，当生产到第20个时加入每秒吃1个馒头，共吃完3个后在接着生产的多线程

```java
public class ThreadTest1 {
    public static void main(String[] args) {
        Person person = new Person();
        person.start();
    }
}

class Person extends Thread {
    private int toy = 0;
    private int food = 0;

    public void eatfood(){
        System.out.println("吃了" + ++food +"个馒头");
    }

    @Override
    public void run() {
        while (toy < 50) {
            System.out.println("生产了:" + ++toy);
            if (toy % 20 == 0) {
                for(;food < 3;){
                    try {
                        sleep(1000);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                    eatfood();
                }
                food = 0;
            }
        }
    }
}
```

### Thread类的有关方法

- **start()**: 启动当前线程，并调用当前线程的run()方法
- **run()**: 线程在被调度时执行的操作
    - 通常Thread子类需要重写此方法，将创建的线程要执行的操作声明在此方法
- String **getName():** 返回线程的名称
- void **setName(String name)** : 设置该线程名称
- static Thread **currentThread()**: 返回当前线程。
     - 在Thread子类中就是this，通常用于主线程和Runnable实现类
- static void **yield()**：线程让步(释放当前CPU的执行权)（有可能立刻又被赋予执行权）
  - 暂停当前正在执行的线程，把执行机会让给优先级相同或更高的线程
  - 若队列中没有同优先级的线程，忽略此方法
- **join()** ：当某个程序执行流中调用其他线程的 join() 方法时，该线程将被阻塞，直到 join() 方法加入的 join 线程执行完为止，该线程才结束阻塞状态。
  - 低优先级的线程也可以获得执行
- static void **sleep(long millis)**：让当前线程睡眠(指定时间:毫秒)，在睡眠时间内，当前线程是阻塞状态。
  - 令当前活动线程在指定时间段内放弃对CPU控制,使其他线程有机会被执行,时间到后重排队。
  - 抛出InterruptedException异常
- **stop()**: 强制线程生命期结束，不推荐使用（已过时）
- boolean **isAlive()**：返回boolean，判断线程是否还活着
- String **getState()** : 获得线程的状态，该方法的返回值是Thread.State，它是线程状态的枚举
- **suspend()** :  使另一个线程(`线程对象.suspend()`)暂停执行，要想恢复，需要其他线程使用resume()方法。
   - 容易造成死锁  
      - 如果存在a.b两线程使用同一个锁，在a线程中使用了 `b.suspend()`使得b线程阻塞，但是b线程并未释放锁，那么a线程也无法打开锁。于是死锁。
- **resume()** : 恢复线程
- **interrupt()** ：中断线程的阻塞，并且该线程收到InterruptException异常.

#### 例1

```java
public class ThreadMethodTest {
    public static void main(String[] args) {
        MyThreadTest t = new MyThreadTest("分线程");
//        方式1：给分线程命名
//        t.setName("线程1：");
        t.start();

//        给主线程命名
        Thread.currentThread().setName("主线程");

        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ": " + i);
            }

            System.out.println(Thread.currentThread().isAlive()); //判断线程是否存活(boolean)

            if (i == 20) {
                try {
                    t.join();
//                    中断当前线程的执行，先执行插入的线程，直到插入的线程执行结束，才执行原来的线程
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }
        }
        System.out.println(t.isAlive()); //判断线程是否存活(boolean)

    }
}

class MyThreadTest extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ":" + i);
                System.out.println(this.getState()); //获取t线程的状态
                try {
                    sleep(10); //睡眠
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }
            }

            if (i % 20 == 0) {
                this.yield(); //释放当前CPU的执行权
//                等同于
//                Thread.currentThread().yield();
            }
        }
    }

    //    方式二：构造器命名
    public MyThreadTest(String name) {
        super(name);
    }
}
```

#### 练习

编写一个继承Thread类的方式实现多线程的程序。该类MyThread有两个属性，一个字符串WhoAmI代表线程名，一个整数delay代表该线程随机要休眠的时间。构造有参的构造器，线程执行时，显示线程名和要休眠时间。

另外，定义一个测试类TestThread，创建三个线程对象以展示执行情况。

```java
public class MyThreadTest {
    public static void main(String[] args) {
        MyThread myThread= new MyThread("线程1",100);
        myThread.start();
    }
}

class MyThread extends Thread{
    private String whoAmI;
    private int delay;

    public MyThread(String whoAmI, int delay) {
        this.whoAmI = whoAmI;
        this.delay = delay;
    }

    @Override
    public void run() {
        Thread.currentThread().setName(whoAmI);
        try {
            sleep(delay);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        System.out.println(Thread.currentThread().getName());
    }
}
```

### ThreadGroup类 线程组

Java应用程序中，所有的线程都属于一个线程组，线程组中的线程一般是相关的。java.lang包中的ThreadGroup类实现了线程组，并提供了对线程组或组中的每一个线程进行操作的方法。

- Java中每个线程都属于某个线程组。线程组使一组线程可以作为一个对象进行统一处理或维护。
    - 例如可以用一个方法统一调用、启动或挂起线程组内的所有线程。
- 一个线程只能在创建时设置其所属的线程组，在线程创建后就不允许将线程从一个线程组移到另一个线程组。

**构造方法**

- 线程组是由java.lang包中的ThreadGroup类实现的。在创建线程时可以显式地指定线程组，此时需要从如下3种线程构造方法中选择一种：
   - public Thread(ThreadGroup group,Runnable target);
   - public Thread(ThreadGroup group,String name);
   - public Thread(ThreadGroup group,Runnable target,String name);

**若在线程创建时并没有显式指定线程组，则新创建的线程自动属于父线程所在的线程组。**

- 在Java应用程序启动时，Java运行系统为该应用程序创建了一个称为main的线程组。
- 如果以后创建的线程没有指定线程组，则这些线程都将属于main线程组。
- 程序中可以利用ThreadGroup类显式创建线程组，并将新创建的线程放入该线程组，

**ThreadGroup类对Java应用程序中的线程组进行管理。**

- 一个线程组可以包含任意数目的线程。
- 一个线程组内不仅可以包含线程，还可以包含其他线程组。
- 在Java应用程序中，最顶层线程组是main。
   - 在main中可以创建线程或线程组，并且可以在main的线程组中进一步创建线程组。
   - 因此在Java应用程序中，形成了以main为根的线程与线程组的树状结构。

### 方式二：实现Runnable接口

1. 定义子类，实现Runnable接口。
2. 子类中重写Runnable接口中的run方法。
3. 通过Thread类含参构造器创建线程对象。
4. 将Runnable接口的子类对象作为实际参数传递给Thread类的构造器中。
5. 调用Thread类的start方法：开启线程，调用Runnable子类接口的run方法。

#### 例

```java
public class RunnableTest {
    public static void main(String[] args) {
    //3. 通过Thread类含参构造器创建线程对象。
    //4. 将Runnable接口的子类对象作为实际参数传递给Thread类的构造器中。
        Thread t = new Thread(new RunTest());
    //5. 调用Thread类的start方法：开启线程，调用Runnable子类接口的run方法。
        t.start();
    }

}

//1. 定义子类，实现Runnable接口
class RunTest implements Runnable {
    int i;

//2. 子类中重写Runnable接口中的run方法
    @Override
    public void run() {
        while (true) {
            System.out.println("Hello" + i++);
            if (i == 5) {
                break;
            }
        }
    }
}
```

### 继承方式和实现方式的联系与区别

**联系：**

```java
public class Thread extends Object implements Runnable
```

1. Tread类实现了Runnable接口
2. 都需要重写run()

**区别**

- 继承Thread：线程代码存放Thread子类run方法中。
- 实现Runnable：线程代码存在接口的子类的run方法。

**优点比较：**

- 继承Tread类：
   - 代码简单 
- 实现Runnable接口
  1. 避免了单继承的局限性
  2. 多个线程可以共享同一个接口实现类的对象，非常适合多个相同线程来处理同一份资源。
  3. 符合OOP的设计思想
     - 从OO设计的角度，Tread类时虚拟CPU的封装，其子类也应该与CPU有关，但在继承Tread类的子类构造线程的方法中，其子类大多也与CPU无关。
     
### 线程的调度

虽然概念上多个线程可以并发执行，但由于目前计算机多数是单个CPU,所以一个时刻只能运行一个线程。在单个CPU上以某种顺序运行多个线程，称为线程的调度。

**调度策略**

- 时间片

![](C:/NoteBook/pictures/9075515221057.png =212x)

- 抢占式：高优先级的线程抢占CPU

**Java的调度方法**
- 同优先级线程组成先进先出队列（先到先服务），使用时间片策略
- 对高优先级，使用优先调度的抢占式策略

### 线程的优先级

**线程的优先级等级（静态常量）**

- MAX_PRIORITY：10 
- MIN _PRIORITY：1 
- NORM_PRIORITY：5
 
**涉及的方法**

- getPriority() ：返回线程优先值
- setPriority(int newPriority) ：改变线程的优先级

**说明**

- 一般，主线程具有普通优先级NORM_PRIOPITY
- 线程创建时继承父线程的优先级
   - 父线程：执行创建新线程语句所在线程。 

**基于优先级的抢先式调度**

- Java基于线程的优先级选择高优先级的线程进行运行。该线程（当前线程）将持续运行，直到它中止运行，或其他高优先级线程成为可运行的。
   - 在后一种情况，低优先级线程被高优先级线程抢占运行。线程中止运行的原因可能有多种，如执行Thread.sleep()调用，或等待访问共享资源。
   - 低优先级只是获得调度的概率低，并非一定是在高优先级线程之后才被调用
- 在Java运行系统中可以按优先级设置多个线程等待池，JVM先运行高优先级池中的线程，高优先级等待池空后，才考虑低优先级。如果线程运行中有更高优先级的线程成为可运行的，则CPU将被高优先级线程抢占。
- 抢先式调度可能是分时的，即每个同等优先级池中的线程轮流运行，也可能不是，即线程逐个运行，由具体JVM而定。
   - 线程一般通过使用sleep()等方法保证给其他线程运行时间。
   

#### 例 创建三个窗口买票，总票数100

##### 继承Tread 类

```java
public class WindowTest {
    public static void main(String[] args) {
        Window w1 = new Window();
        Window w2 = new Window();
        Window w3 = new Window();

        w1.setName("窗口1");
        w2.setName("窗口2");
        w3.setName("窗口3");

        w1.start();
        w2.start();
        w3.start();
    }

}

class Window extends Thread {
    private static int tickets = 100;

    @Override
    public void run() {
        while (true) {
            if (tickets > 0) {
                System.out.println(getName() + "卖票，票号为：" + tickets);
                tickets--;
            } else {
                break;
            }
        }
    }
}

//存在线程安全
//窗口3卖票，票号为：100
//窗口2卖票，票号为：100
//窗口1卖票，票号为：100
```

##### 实现Runable接口

```java
public class WindowTest2 {
    public static void main(String[] args) {
        Windows w1 = new Windows();
        Windows w2 = new Windows();
        Windows w3 = new Windows();

        Thread t1 = new Thread(w1);
        Thread t2 = new Thread(w2);
        Thread t3 = new Thread(w3);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }


}

class Windows implements Runnable {
    private static int ticket = 100;

    @Override
    public void run() {
        for (int i = 0; i < 100; i--) {
            if (ticket > 0) {
                System.out.println(Thread.currentThread().getName() + ": " + ticket);
                ticket--;
            } else {
                break;
            }
        }
    }
}
```

### 线程的分类

Java中的线程分为两类：一种是守护线程，一种是用户线程。

- 它们在几乎每个方面都是相同的，唯一的区别是判断JVM何时离开。
- 守护线程是用来服务用户线程的，通过在start()方法前调用
- `Thread.setDaemon(true)`可以把一个用户线程变成一个守护线程。
- Java垃圾回收就是一个典型的守护线程。
   - 若JVM中都是守护线程，当前JVM将退出。

## 线程的生命周期

**JDK中用Thread.State类定义了线程的几种状态**

要想实现多线程，必须在主线程中创建新的线程对象。Java语言使用Thread类及其子类的对象来表示线程，在它的一个完整的生命周期中通常要经历如下的五种状态：

- 新建状态 new： 当一个Thread类或其子类的对象被声明并创建时，新生的线程对象处于新建状态
   - 线程还未被分配有关的系统资源。 
- 就绪（可运行）状态 runnable：处于新建状态的线程被start()后，将进入线程队列等待CPU时间片，此时它已具备了运行的条件，只是没分配到CPU资源
   - 表示系统处于运行就绪状态，此时线程仅仅是可以运行。
   - start()方法使系统为线程分配必要的资源，将线程中虚拟的CPU置为Runnable状态，并将线程交给系统调度。
   - 在多线程程序设计中，系统中往往会有多个线程同时处于Runnable状态，他们将 竞争有限的CPU资源，有运行系统根据线程调度策略进行调度
- 运行状态：当就绪的线程被调度并获得CPU资源时,便进入运行状态（线程占有CPU并实际运行的状态）， run()方法定义了线程的操作和功能 。此时线程状态的变迁有以下三种：
   1. 如果线程正常执行结束或应用程序停止运行，线程将进人终止状态。
   2. 如果当前线程执行了yild()方法，或者当前线程因调度策略（执行过程中，有一个更高优先级的线程进入可运行状态，这个线程立即被调度执行，当前线程占有的CPU被抢占；或在分时方式时，当前执行线程执行完当前时间片)由系统控制进入可运行状态。
   3. 如果发生下面几种情况时，线程就进入阻塞状态。
       - 线程调用了sleep()方法或join()方法，进入阻塞状态。
       - 线程调用wait()方法时，由运行状态进入阻塞状态。
       - 如果线程中使用synchronized来请求对象的锁未获得时，进入阻塞状态。
       - 如线程中有输入/输出操作，也将进入阻塞状态，待输入/输出操作结束后，线程进入可运行状态。
- 阻塞状态：在某种特殊情况下，被人为挂起或执行输入输出操作时，让出 CPU 并临时中止自己的执行，进入阻塞状态。阻塞状态根据产生的原因又可分为对象锁阻塞(blocked in loek pool)、等待阻塞(blocked in wait pool)和其他阻塞(otherwise blocked)。
   - 对象锁阻塞： 如果线程中使用synchronized来请求对象的锁但未获得时，进人对象锁阻塞状态。
      - 该状态下的线程当获得对象锁后，将进人可运行状态。
   - 等待阻塞：线程调用wait()方法时，线程由运行状态进人等待阻塞状态。
      - 在等待阻塞状态下的线程若被notify()和notifyAll()唤醒，被interrupt()中断或者等待时间到，线程将进入对象锁阻塞状态。
   - 其他阻塞：线程调用了sleep()方法或join()方法时，线程进人其他阻塞状态。
      - 由于调用sleep()方法而进人其他阻塞状态的线程，睡眠时间到时将进人可运行状态，
      - 由于调用join()方法而进人其他阻塞状态的线程，当t线程结束或等待时间到时，进入可运行状态。
- 死亡(终止)状态：线程完成了它的全部工作或线程被提前强制性地中止或出现异常导致结束
   - 终止状态是线程执行结束的状春，没有任何方法可改变它的状态。
![](C:/NoteBook/pictures/204051510227351.png =701x)


![](C:/NoteBook/pictures/331541510247517.png =529x)

## 线程的同步(线程的安全问题)

**提出问题**

- 多个线程执行的不确定性引起执行结果的不稳定
- 多个线程对账本的共享，会造成操作的不完整性，会破坏数据。

1. 多线程出现了安全问题
2. 问题的原因：
   - 当多条语句在操作同一个线程共享数据时，一个线程对多条语句只执行了一部分，还没有执行完，另一个线程参与进来执行。导致共享数据的错误。
3. 解决办法：
  - 对多条操作共享数据(多个线程共同操作的变量，如static)的语句，只能让一个线程都执行完，在执行过程中，其他线程不可以参与执行。

```java
public class WindowTest {
    public static void main(String[] args) {
        Window w1 = new Window();
        Window w2 = new Window();
        Window w3 = new Window();

        w1.setName("窗口1");
        w2.setName("窗口2");
        w3.setName("窗口3");

        w1.start();
        w2.start();
        w3.start();
    }

}

class Window extends Thread {
    private static int tickets = 100;

    @Override
    public void run() {
        while (true) {
            if (tickets > 0) {
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }

                System.out.println(getName() + "卖票，票号为：" + tickets);
                tickets--;
            } else {
                break;
            }
        }
    }
}

//存在线程安全
//重票
//窗口3卖票，票号为：100
//窗口2卖票，票号为：100
//窗口1卖票，票号为：100

//错票 在线程切换时，可能出现的问题。
//通过sleep()使线程睡眠，来延长for循环满足条件时执行的过程和ticket的值开始变化的时间，只是让这个更直观。
//在这个时间段里（某个过程在操作的过程中，对ticket值的操作尚未完成的时候）：
//启动的进程在ticket的值满足条件的时候都进入执行语句，而此时因为ticket的值没来得及改变，
//因此，启动的进程中的ticket如果刚刚好满足条件，但是已经有多个线程在这段时间经过条件判断进入执行，
//此时，将对ticket的值t产生多次改变。
//窗口3卖票，票号为：0
//窗口1卖票，票号为：-1
```

### 同步锁机制

对于并发工作，你需要某种方式来防止两个任务访问相同的资源（其实就是共享资源竞争）。 防止这种冲突的方法就是当资源被一个任务使用时，在其上加锁。第一个访问某项资源的任务必须锁定这项资源，使其他任务在其被解锁之前，就无法访问它了，而在其被解锁之时，另一个任务就可以锁定并使用它了。

**synchronized的锁(对象锁)**

Java中对共享数据操作的并发控制是采用传统的封锁技术。

一个程序的各个并发线程中对同一个对象进行访问的代码段，称为临界区(critical sections).。在Java语言中，临界区可以是一个语句块或是一个方法，并且用synchronized关键字标识。

临界区的控制是通过对象锁进行的。Java平台将每个由`synchronized(someObject){}`语句指定的对象someObject设置一个锁，称为对象锁(monitor)。对象锁是一种独占的排他锁(exclusive locks)。这种锁的含义是，当一个线程获得了对象的锁后，便拥有该对象的操作权，其他任何线程不能对该对象进行任何操作。线程在进入临界区时，首先通过`synchronized(someObject)`语句测试并获得对象的锁，只有获得对象锁才能继续执行临界区中的代码，否则将进入等待状态。

- 任意对象都可以作为同步锁。所有对象都自动含有单一的锁（监视器）。
- 同步方法的锁：静态方法（类名.class）、非静态方法（this）
- 同步代码块：自己指定，很多时候也是指定为this或类名.class
- 注意：
   - 必须确保使用同一个资源的多个线程共用一把锁，这个非常重要，否则就无法保证共享资源的安全
  - 一个线程类中的所有静态方法共用同一把锁（类名.class），所有非静态方法共用同一把锁（this），同步代码块（指定需谨慎）


**对于对象锁的使用有如下几点说明**

1. 关于对象锁的返还。对象的锁在如下几种情况下由持有线程返还。
   - 当synchronized()语句块执行完后
   - 当在synchronized()语句块中出现异常(Exception)。
   - 当持有锁的线程调用该对象的wait()方法。此时该线程将释放对象的锁，而被放人对象的wait pool中，等待某种事件的发生。
2. 共享数据的所有访问都必须作为临界区，使用synchronized进行加锁控制。
   - 对共享数据所有访问的代码，都应该作为临界区使用synchronized进行标识。这样保证所有的操作都能够通过对象锁的机制进行控制。
   - 如果有一种访问操作未标记为synchronized,则这种操作将绕过对象锁，很可能破坏共享数据的一致性。
3. 用synchronized保护的共享数据必须是私有的。
   - 将共享数据定义为私有的，使线程不能直接访问这些数据，必须通过对象的方法。
   - 而对象的方法中带有由synchronized标记的临界区，实现对并发操作多个线程的控制。
4. 同步方法：如果一个方法的整个方法体都包含在synchronized语句块中，则可以把该关键字放在方法的声明中。
   - 这种方式程序的可读性好，便于理解，因此比较常用。
   - 但控制对象锁的时间稍长，因此并发执行的效率会受到一定的影响，但影响不是很大。
5. Java中对象锁具有可重人性。
   - Java运行系统中，一个线程在持有某个对象的锁的情况下，可以再次请求并获得该对象的锁，这就是对象锁具有可重入性的含义。
   - 锁的可重入性是很重要的因为这可以避免单个线程因为自已已经持有的锁而产生死锁。例如下面的程序。

**同步的范围**

1. 如何找问题，即代码是否存在线程安全？（非常重要）
   1. 明确哪些代码是多线程运行的代码
   2. 明确多个线程是否有共享数据
   3. 明确多线程运行代码中是否有多条语句操作共享数据
2. 如何解决呢？（非常重要）
   - 对多条操作共享数据的语句，只能让一个线程都执行完，在执行过程中，其他线程不可以参与执行。
      - 即所有操作共享数据的这些语句都要放在同步范围中
3. 切记：
   - 范围太小：没锁住所有有安全问题的代码
   - 范围太大：没发挥多线程的功能。**

**释放锁的操作**

-  当前线程的同步方法、同步代码块执行结束。
- 当前线程在同步代码块、同步方法中遇到break、return终止了该代码块、该方法的继续执行。
- 当前线程在同步代码块、同步方法中出现了未处理的Error或Exception，导致异常结束。
- 当前线程在同步代码块、同步方法中执行了线程对象的wait()方法，当前线程暂停，并释放锁。

**不会释放锁的操作**

- 线程执行同步代码块或同步方法时，程序调用`Thread.sleep()、Thread.yield()方法`暂停当前线程的执行
- 线程执行同步代码块时，其他线程调用了该线程的`suspend()方法`将该线程挂起，该线程不会释放锁（同步监视器）。
   - 应尽量避免使用`suspend()和resume()`来控制线程

#### 线程的死锁问题

- 死锁
    - **不同的线程分别占用对方需要的同步资源不放弃**，都在等待对方放弃自己需要的同步资源，就形成了线程的死锁
    - 出现死锁后，**不会出现异常，不会出现提示**，只是所有的线程都处于阻塞状态，无法继续
- 解决方法
   - 专门的算法、原则
   - 尽量减少同步资源的定义
   - 尽量避免嵌套同步

##### 例：死锁：

```java
public class TreadTest {
    public static void main(String[] args) {
        StringBuffer s1 = new StringBuffer();
        StringBuffer s2 = new StringBuffer();
        new Thread(){
            @Override
            public void run() {
                synchronized (s1){
                    s1.append("a");
                    s2.append("1");

                    try {
                        sleep(100);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }

                    synchronized (s2){
                        s1.append("b");
                        s2.append("2");

                        System.out.println(s1);
                        System.out.println(s2);
                    }
                }
            }
        }.start();

        new Thread(new Runnable() {
            @Override
            public void run() {
                synchronized (s2){
                    s1.append("c");
                    s2.append("3");
                    try {
                        Thread.currentThread().sleep(100);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                    synchronized (s2){
                        s1.append("d");
                        s2.append("4");

                        System.out.println(s1);
                        System.out.println(s2);
                    }
                }
            }
        }).start();
    }
}
```

### synchronized的使用方法 同步机制

#### 1. 同步代码块：

```java
synchronized (同步监视器){  
    // 需要被同步的代码；(即操作共享数据的代码)
}
```  

1. 操作共享数据的代码，即需要被同步的代码
2. 共享数据：多个线程共同操作的变量，如static
3. 同步监视器：即锁。如何一个类的对像都可以充当锁
    - 要求：**多个线程必须要共用同一把锁**
4. 解决了线程的安全问题。
    - 但，操作同步代码时，只能有一个线程参与，其他线程等待。相当于是一个单线程的过程，效率低。
        - 包含同步代码的范围(共享数据)要注意。

**充当锁的方式**

(共用)

- 在实现Runnable接口创建多线程的方式中，可以使用以下充当锁：
   - this  
   - 类名.class 
   - run()方法外的对象
- 在继承Thread类创建多线程的方式中，可以使用以下充当锁：
   - 慎用this 
   - 类名.class 
   - run()方法外的静态对象（static）

#####  解决通过实现Runable接口的方式的线程安全问题

```java
public class WindowTest2 {
    public static void main(String[] args) {
        Windows w1 = new Windows();

        Thread t1 = new Thread(w1);
        Thread t2 = new Thread(w1);
        Thread t3 = new Thread(w1);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}

class Windows implements Runnable {
    private int ticket = 100;
    private static int tickets = 100;
    Object obj = new Object();

    @Override
    public void run() {
        while (true) {
//            Object obj = new Object(); 错误
            //synchronized (obj) { //要求线程必须共用同一把锁
            //synchronized(this){ //使用当前对象作锁。此时的this：唯一的Windows的对象。
            synchroniaed(Window.class){  //通过类作为锁，类也是对象。
            //如果都是使用同一个Runnable接口的实现类的对象来创建线程时。
                if (tickets > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }

                    System.out.println(Thread.currentThread().getName() + "卖票，票号为：" + tickets);
                    tickets--;
                } else {
                    break;
                }
            }
        }
    }
}
```

#####  解决通过继承Tread类的方式的线程安全问题

```java
public class WindowTest {
    public static void main(String[] args) {
        Window w1 = new Window();
        Window w2 = new Window();
        Window w3 = new Window();

        w1.setName("窗口1");
        w2.setName("窗口2");
        w3.setName("窗口3");

        w1.start();
        w2.start();
        w3.start();
    }

}

class Window extends Thread {
    private static int tickets = 100;
    static Object obj = new Object(); //static 确保线程使用同一把锁

    @Override
    public void run() {
        while (true) {
//            Object obj = new Object(); 错误
            //synchronized (obj) { //要求线程必须共用同一把锁
            synchroniaed(Window.class){  //通过类作为锁，类也是对象。
                if (tickets > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }

                    System.out.println(getName() + "卖票，票号为：" + tickets);
                    tickets--;
                } else {
                    break;
                }
            }
        }
    }
}
```

#### 2. 同步方法

**synchronized还可以放在方法声明中，表示整个方法为同步方法。**

- 通过实现**Runnable接口的线程，此时同步监视器为this**
- 通过**继承Tread类的线程，此时的同步方法要是静态的，同步监视器为类**

**实现**

```java
public class WindowTest2 {
    public static void main(String[] args) {
        Windows w1 = new Windows();

        Thread t1 = new Thread(w1);
        Thread t2 = new Thread(w1);
        Thread t3 = new Thread(w1);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }


}

class Windows implements Runnable {
    private int ticket = 100;
    Object obj = new Object();

    @Override
    public void run() {
        while (true) {
//            Object obj = new Object(); 错误
            changeTicket();
        }

    }

    public synchronized void changeTicket() {//同步监视器：this
        if (ticket > 0) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }

            System.out.println(Thread.currentThread().getName() + "卖票，票号为：" + ticket);
            ticket--;
        }
    }
}
```

**继承**

```java
public class WindowTest {
    public static void main(String[] args) {
        Window w1 = new Window();
        Window w2 = new Window();
        Window w3 = new Window();

        w1.setName("窗口1");
        w2.setName("窗口2");
        w3.setName("窗口3");

        w1.start();
        w2.start();
        w3.start();
    }

}

class Window extends Thread {
    private static int ticket = 100;

    @Override
    public void run() {
        while (true) {
            changeTicket();
        }
    }

    public static synchronized void changeTicket() {//同步监视器： 类（静态方法）
        if (ticket > 0) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }

            System.out.println(Thread.currentThread().getName() + "卖票，票号为：" + ticket);
            ticket--;
        }
    }
}
```


### Lock(锁)

从JDK 5.0开始，Java提供了更强大的线程同步机制——通过显式定义同步锁对象来实现同步。同步锁使用Lock对象充当。

- `java.util.concurrent.locks.Lock接口`是控制多个线程对共享资源进行访问的工具。锁提供了对共享资源的独占访问，每次只能有一个线程对Lock对象
加锁，线程开始访问共享资源之前应先获得Lock对象。
- ReentrantLock 类实现了 Lock ，它拥有与 synchronized 相同的并发性和内存语义，在实现线程安全的控制中，比较常用的是ReentrantLock，可以显式加锁、释放锁。
- 对于继承Tread类的方式，对ReentrantLock对象加static

```java
public class LockTest {
    public static void main(String[] args) {
        Window w = new Window();

        Thread t1 = new Thread(w);
        Thread t2 = new Thread(w);
        Thread t3 = new Thread(w);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }

}

class Window implements Runnable {
    private int ticket = 100;
    //1.实例化
    private ReentrantLock lock = new ReentrantLock();
//    private ReentrantLock lock = new ReentrantLock(true);
//    true：公平 即不会一直都是一个线程抢占执行
//    默认false

    @Override
    public void run() {
        while (true) {
            try{
//                2.调用锁定lock()方法
                lock.lock();
                if (ticket > 0) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }

                    System.out.println(Thread.currentThread().getName() + ", 票号:" + ticket);
                    ticket--;
                } else {
                    break;
                }
            }finally {
//                3. 调用解锁unlock()的方法
                lock.unlock();
            }

        }
    }
}
```

### synchronized 与 Lock 的对比

1. Lock是显式锁（手动开启(lock())和关闭锁(unlock())），synchronized是隐式锁，出了作用域自动释放同步监视器。
2. Lock只有代码块锁，synchronized有代码块锁和方法锁
3. 使用Lock锁，JVM将花费较少的时间来调度线程，性能更好。并且具有更好的扩展性（提供更多的子类）

**优先使用顺序：**

Lock --> 同步代码块（已经进入了方法体，分配了相应资源）--> 同步方法（在方法体之外）

### 练习

#### 练 习1

```java
银行有一个账户。
有两个储户分别向同一个账户存3000元，每次存1000，存3次。每次存完打印账户余额。
问题：该程序是否有安全问题，如果有，如何解决？
【提示】
1，明确哪些代码是多线程运行代码，须写入run()方法
2，明确什么是共享数据。
3，明确多线程运行代码中哪些语句是操作共享数据的。
拓展问题：可否实现两个储户交替存钱的操作
```

```java
public class AccountTest {
    public static void main(String[] args) {
        Account acct = new Account(0);
        Customer c1 = new Customer(acct);
        Customer c2 = new Customer(acct);

        c1.setName("甲");
        c2.setName("乙");

        c1.start();
        c2.start();
    }
}

class Account{
    private int balance;

    public Account(int balance) {
        this.balance = balance;
    }

//    存钱
    public void deposit(int amt){
        if(amt > 0){
            synchronized(Account.class){
                balance += amt;

                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }

                System.out.println(Thread.currentThread().getName() + "存钱成功，余额：" + balance);
            }
        }
    }

}

class Customer extends Thread{
    private Account acct;

    public Customer(Account acct) {
        this.acct = acct;
    }

    @Override
    public void run() {
        for(int i = 0; i< 3;i++){
            acct.deposit(1000);
        }
    }
}
```

#### 练习2

利用多线程设计一个程序，同时输出 50 以内的奇数和偶数，以及当前运行的线程名。

```java
//利用多线程设计一个程序，同时输出 50 以内的奇数和偶数，以及当前运行的线程名。
public class ThreadTest2 {
    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {
            for (int i = 0; i <= 50; i++) {
                if (i % 2 == 0) {
                    System.out.println(Thread.currentThread().getName() + i);
                } else {
                    try {
                        Thread.currentThread().sleep(1000);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                }
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i <= 50; i++) {
                if (i % 2 == 1) {
                    System.out.println(Thread.currentThread().getName() + i);
                } else {
                    try {
                        Thread.currentThread().sleep(1000);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                }
            }
        });

        t1.start();
        t2.start();
    }
}
```

## 线程的通信

**wait() 与 notify() 和 notifyAll()**

- wait()：令当前线程挂起并放弃CPU、同步资源并等待，使别的线程可访问并修改共享资源，而当前线程排队等候其他线程调用`notify()或notifyAll()`方法唤醒，唤醒后等待重新获得对监视器的所有权后才能继续执行。
- notify()：唤醒正在排队等待同步资源的线程中优先级最高者结束等待
- notifyAll ()：唤醒正在排队等待资源的所有线程结束等待.

这三个方法**只有在`synchronized方法或synchronized代码块`中才能使用**，否则会报`java.lang.IllegalMonitorStateException`异常。

- 因为这三个方法**必须由锁对象(即必须是同步代码/同步方法的同步监视器)调用**，
   - 而任意对象都可以作为synchronized的同步锁，因此这三个方法只能在**Object类中声明**

### wait() 方法

- 在当前线程中调用方法： 对象名.wait()
- 使当前线程进入等待（某对象）状态 ，直到另一线程对该对象发出 notify (或notifyAll) 为止。
- 调用方法的必要条件：当前线程必须具有对该对象的监控权（加锁）
- 调用此方法后，当前线程将释放对象监控权(释放锁，即打开了锁，其他线程得以进入) ，然后进入等待
- 在当前线程被notify后，要重新获得监控权，然后从断点处继续代码的执行。

#### sleep()和wait()的异同点

**相同：**

- sleep()和wait()两个执行方法，都可以使得当前的线程进入阻塞状态

**不同：**

1. 声明位置不同：
   - sleep()在Tread类中声明
   - wait()在Object类中声明
2. 调用的要求不同：
   - sleep()可以在任何需要的场景下调用
   - wait()必须使用在同步代码块和同步方法中
3. 是否释放同步监视器：（ 如果两个方法都使用在同步代码块/同步方法中 ）
   - sleep()不释放
   - wait()释放

### notify()/notifyAll()

- 在当前线程中调用方法： 对象名.notify()
- 功能：唤醒等待该对象监控权的一个/所有线程(被wait()的线程)。
- 调用方法的必要条件：当前线程必须具有对该对象的监控权（加锁）

### 例题

#### 例

```java
public class CommunicationTest {
    public static void main(String[] args) {
        Print p = new Print();
        Thread t1 = new Thread(p);
        Thread t2 = new Thread(p);

        t1.setName("线程1");
        t2.setName("线程2");

        t1.start();
        t2.start();
    }
}

class Print implements Runnable {
    private int number = 1;

    @Override
    public void run() {
        while (true) {
            synchronized (this){

                notify();

                if (number <= 100) {

                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }

                    System.out.println(Thread.currentThread().getName() + ": " + number++);

                    try {
//                         阻塞：使得调用如下wait()方法的线程进入阻塞状态。
                        wait();
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                } else {
                    break;
                }
            }
        }
    }
}
```

#### 典例：生产者消费者问题

生产者(Productor)将产品交给店员(Clerk)，而消费者(Customer)从店员处取走产品，店员一次只能持有固定数量的产品(比如:20），如果生产者试图生产更多的产品，店员会叫生产者停一下，如果店中有空位放产品了再通知生产者继续生产；如果店中没有产品了，店员会告诉消费者等一下，如果店中有产品了再通知消费者来取走产品。

- 这里可能出现两个问题：

1. 生产者比消费者快时，消费者会漏掉一些数据没有取到。
2. 消费者比生产者快时，消费者会取相同的数据。

```java
public class ProductTest {
    public static void main(String[] args) {
        Clerk clerk = new Clerk();

        Producer p = new Producer(clerk);
        Thread p1 = new Thread(p);
        p1.setName("生产者1");

        Consumer c = new Consumer(clerk);
        Thread c1 = new Thread(c);
        c1.setName("消费者1");

        p1.start();
        c1.start();
    }
}

class Clerk {
    private int productCount = 0;

    //    生产产品
    public synchronized void produceProduct() {
        if (productCount < 20) {
            productCount++;
            System.out.println(Thread.currentThread().getName() + ": 开始生产第" + productCount + "个产品");

            notify();
        } else {
            try {
                wait();
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }

    //    消费产品
    public synchronized void consumeProduct() {

        if (productCount > 0) {
            System.out.println(Thread.currentThread().getName() + ": 开始消费第" + productCount + "个产品");
            productCount--;

            notify();
        } else {
            try {
                wait();
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}

class Producer implements Runnable { //消费者

    private Clerk clerk;

    public Producer(Clerk clerk) {
        this.clerk = clerk;
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + ":生产产品...");

        while (true) {

            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }

            clerk.produceProduct();
        }
    }
}

class Consumer implements Runnable { //消费者

    private Clerk clerk;

    public Consumer(Clerk clerk) {
        this.clerk = clerk;
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + ":消费产品...");

        while (true) {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }

            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }

            clerk.consumeProduct();
        }
    }
}
```

#### 生产者-消费者问题

编写生产者-消费者模式的程序。生产者每隔100ms产生0~9的一个数，保存在一个MyNumber类型的对象中，并显示出来。只要这个MyNumber对象中保存了新的数字，消费者就将其取出并显示。试定义MyNumber类，编写消费者和生产者程序，并编写主程序创建一个MyNumber对象，以及一个生产者线程、一个消费者线程，并将这两个线程启动运行。

```java
public class MyNumberTest {
    public static void main(String[] args) {
        MyNumber myNumber = new MyNumber();
        Productor p1 = new Productor(myNumber);
        Customer c1 = new Customer(myNumber);

        p1.setName("生产者1");
        c1.setName("消费者1");

        p1.start();
        c1.start();
    }
}

class MyNumber {
    private int num;

    public synchronized void productNewNumber() {
        num = (int) (Math.random() * 100 / 10);
        notify();
        try {
            wait();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }

    public synchronized void consumNewNumber() {
        if(num == 0){
            notify();
            try {
                wait();
            } catch (InterruptedException e) {
                x
            }
        }
        System.out.println("取出数字" + num);
        num = 0;
        notify();

        try {
            wait();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }
}

class Customer extends Thread {
    public Customer(MyNumber myNumber) {
        this.myNumber = myNumber;
    }

    MyNumber myNumber = new MyNumber();

    @Override
    public void run() {
        while (true) {
            myNumber.consumNewNumber();
            System.out.println(Thread.currentThread().getName() + "消费产品完成");
        }
    }
}

class Productor extends Thread {

    public Productor(MyNumber myNumber) {
        this.myNumber = myNumber;
    }

    private MyNumber myNumber = new MyNumber();

    @Override
    public void run() {
        while (true) {
            try {
                sleep(100);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            myNumber.productNewNumber();
            System.out.println(Thread.currentThread().getName() + "生产产品完成");
        }
    }
}
```

#### 练 习 2 模拟银行取钱的问题

```
1.定义一个Account类
1）该Account类封装了账户编号（String）和余额（double）两个属性
2）设置相应属性的getter和setter方法
3）提供无参和有两个参数的构造器
4）系统根据账号判断与用户是否匹配，需提供hashCode()和equals()方法的重写
2.提供两个取钱的线程类：小明、小明’s wife
1）提供了Account类的account属性和double类的取款额的属性
2）提供带线程名的构造器
3）run()方法中提供取钱的操作
3.在主类中创建线程进行测试。考虑线程安全问题。
```

## JDK5.0 新增线程创建方式

### 新增方式一：实现Callable接口

与使用Runnable相比， Callable功能更强大些

- call()方法相比run()方法，可以有返回值
- call()方法可以抛出异常,被外面的操作捕获，获取异常的信息
- Callable支持泛型的返回值
- 需要借助FutureTask类，比如获取返回结果

**Future接口**
-  可以对具体Runnable、Callable任务的执行结果进行取消、查询是否完成、获取结果等。
- FutrueTask是Futrue接口的唯一的实现类
- FutureTask 同时实现了Runnable, Future接口。它既可以作为Runnable被线程执行，又可以作为Future得到Callable的返回值

**使用：**

1. 创建一个Callable接口的实现类
2. 实现call()方法，将此线程需要执行的操作声明在call()方法中
3. 创建一个Callable接口实现类的对象
4. 将Callable接口实现类的对象传递到FutureTask构造器中，创建FutureTask的对象
5. 将FutureTask的对象作为参数传递到Thread类的构造器中，创建Thread对象，并调用start()方法启动线程
6. （可选）获取callable中的call方法的返回值 

**例**

```java
public class TreadNew {
    public static void main(String[] args) {
//      3. 创建一个Callable接口实现类的对象
        NumTread numTread = new NumTread();
//      4. 将Callable接口实现类的对象传递到FutureTask构造器中，创建FutureTask的对象
        FutureTask futureTask = new FutureTask(numTread);
//        FutureTask<Integer> futureTask = new FutureTask<Integer>(numTread); //支持泛型
//      5. 将FutureTask的对象作为参数传递到Thread类的构造器中，创建Thread对象，并调用start()方法启动线程
        new Thread(futureTask).start();

        try {
//            6. 获取callable中的call方法的返回值
            //get()的返回值即为FutureTask构造器参数Callable实现类重写的call()方法的返回值
            Object sum = futureTask.get();
//            Integer sum = futureTask.get();//支持泛型
            System.out.println(sum);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        } catch (ExecutionException e) {
            throw new RuntimeException(e);
        }
    }
}

//1. 创建一个Callable接口的实现类
class NumTread implements Callable {
    //class NumTread implements Callable<Integer> { //支持泛型
    //    2. 实现call()方法，将此线程需要执行的操作声明在call()方法中
    @Override
//    public Integer call() throws Exception {//支持泛型
    public Object call() throws Exception {
        int sum = 0; //自动装箱
        for (int i = 0; i <= 100; i++) {
            if (i % 2 == 0) {
                System.out.println(i);
                sum += i;
            }
        }
        return sum;
    }
}
```

### 新增方式二：使用线程池

背景：经常创建和销毁、使用量特别大的资源，比如并发情况下的线程，对性能影响很大。

思路：提前创建好多个线程，放入线程池中，使用时直接获取，使用完放回池中。可以避免频繁创建销毁、实现重复利用。

好处：

- 提高响应速度（减少了创建新线程的时间）
- 降低资源消耗（重复利用线程池中线程，不需要每次都创建）
- 便于线程管理
   - corePoolSize：核心池的大小
   - maximumPoolSize：最大线程数
   - keepAliveTime：线程没有任务时最多保持多长时间后会终止
   -  ...
   
**例**

```java
public class ThreadPool {
    public static void main(String[] args) {
//        1.提供指定线程数量的线程池
        ExecutorService service = Executors.newFixedThreadPool(10);

//        设置线程池的属性
        ThreadPoolExecutor service1 = (ThreadPoolExecutor) service;
//        System.out.println(service.getClass());
        service1.setCorePoolSize(15);
//        service1.setKeepAliveTime();

//        2.执行指定的线程的操作，需要提供实现Runnable接口或Callable接口实现类的对象
        service.execute(new NumberThread()); //适合使用于Runnable
        service.execute(new NumberThread1()); //适合使用于Runnable
//        service.submit(Callable接口的实现类对象); //适合使用于Callable
        service.shutdown();
    }
}

class NumberThread implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i <= 100; i++) {
            System.out.println(Thread.currentThread().getName() + ":" + i);
        }
    }
}

class NumberThread1 implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i <= 100; i++) {
            System.out.println(Thread.currentThread().getName() + ":" + i);
        }
    }
}
```

#### 线程池相关API

- JDK 5.0起提供了线程池相关API：ExecutorService 和 Executors

**ExecutorService：真正的线程池接口。常见子类ThreadPoolExecutor**

-  void execute(Runnable command) ：执行任务/命令，没有返回值，一般用来执行Runnable
   - `<T> Future<T> submit(Callable<T> task)`：执行任务，有返回值，一般又来执行Callable
- void shutdown() ：关闭连接池

**Executors：工具类、线程池的工厂类，用于创建并返回不同类型的线程池**

- Executors.newCachedThreadPool()：创建一个可根据需要创建新线程的线程池
- Executors.newFixedThreadPool(n); 创建一个可重用固定线程数的线程池
- Executors.newSingleThreadExecutor() ：创建一个只有一个线程的线程池
- Executors.newScheduledThreadPool(n)：创建一个线程池，它可安排在给定延迟后运行命令或者定期地执行。

# Java常用类

##  字符串相关的类：String

- String类：代表字符串。Java 程序中的所有字符串字面值（如 "abc" ）都作为此类的实例实现。
- String是一个final类，代表不可变的字符序列。（不可变性）
   - 字符串是常量，用双引号引起来表示。它们的值在创建之后不能更改。
- String实现了Serializable接口：表示字符串是支持序列化的。
- String实现了Comparable接口：表示String可以比较大小。
- String对象的字符内容是存储在一个字符数组value[]中的。
   - String内部定义了final char[] value用于存储字符串数据 
- 字符串常量池中是不会存储相同内容(即使用String的equals方法返回true)的字符串的。

**String不可变性的体现**

1. 当字符串重新赋值时，需要重新指定内存区域赋值，不能使用原有的value进行赋值
![](C:/NoteBook/pictures/245100421227353.png =562x)
![](C:/NoteBook/pictures/11970921247519.png =564x)
2. 当对现有的字符串进行连接操作时，也需要重新指定内存区域赋值，不能使用原有的value进行赋值。
3. 当调用String的replace()方法修改指定的字符或字符串时，也需要重新指定内存区域赋值，不能使用原有的value进行赋值。
![](C:/NoteBook/pictures/185481721240188.png =551x)

```java
@Test
public void test(){
    String s1 = "abc";//字面量的定义方式
    String s2 = "abc";

    System.out.println(s1 == s2); //s1和s2指向同一个对象
    //true

    s1 = "hello";

    System.out.println(s1 == s2); //false

    System.out.println(s1);
    System.out.println(s2);

    String s3 = "abc";
    s3 += "def";
    System.out.println(s3);
    System.out.println(s3 == s2);//true

    String s4 = "abc";
    String s5 = s4.replace('a','m');
    System.out.println(s4);
    System.out.println(s5);
}
```

### String对象的创建

- String str = "hello";
- String s1 = new String();    
   - //本质上this.value = new char[0];
- String s2 = new String(String original); 
   - //this.value = original.value;
- String s3 = new String(char[] a); 
   - //this.value = Arrays.copyOf(value, value.length);
- String s4 = new String(char[] a,int startIndex,int count);

### String的实例方式

1. 方式1：通过字面量定义的方式
2. 方式2：通过new + 构造器的方式

**String s = new String("abc")；方式创建对象，在内存中创建了几个对象？**
- 两个：一个堆空间中的new结构，一个char[]对应的常量池中的数据"abc".

![](C:/NoteBook/pictures/377974621232497.png =560x)
![](C:/NoteBook/pictures/350640322245483.png =549x)
![](C:/NoteBook/pictures/207800822226724.png =556x)

```java
@Test
public void test2() {
//      通过字面量定义的方式：此时的s1和s2的数据javaEE声明在方法区中的字符串常量池中。
    String s1 = "javaEE";
    String s2 = "javaEE";
//      通过new + 构造器的方式：此时的s3和s4保存的地址值，是数据在堆空间开辟空间以后对应的地址值。
    String s3 = new String("javaEE");
    String s4 = new String("javaEE");

    System.out.println(s1 == s2);//true
    System.out.println(s1 == s3);//false
    System.out.println(s1 == s4);//false
    System.out.println(s3 == s4);//false

    Person p1 = new Person("Tom",12);
    Person p2 = new Person("Tom",12);
    System.out.println(p1.name == p2.name);//true
    Person p3 = new Person(new String("Tom"),12);
    Person p4 = new Person(new String("Tom"),12);
    System.out.println(p1.name == p3.name);//false
    System.out.println(p3.name == p4.name);//false
}
```

### 字符串的特性

- 常量与常量的拼接结果在常量池。且常量池中不会存在相同内容的常量。
- 只要其中有一个是变量，结果就在堆中
- 如果拼接的结果调用`intern()方法`，返回值就在常量池中

```java
@Test
public void test3(){
    String s1 = "javaEE";
    String s2 = "hadoop";

    //字面量：s3,s4都在常量池内
    String s3 = "javaEEhadoop";
    String s4 = "javaEE" + "hadoop";
    //变量：堆空间中开辟
    String s5 = s1 + "hadoop";
    String s6 = "javaEE" + s2;
    String s7 = s1 + s2;

    System.out.println(s3 == s4); //true
    System.out.println(s3 == s5); //false
    System.out.println(s3 == s6); //false
    System.out.println(s3 == s7); //false

    System.out.println(s4 == s5); //false
    System.out.println(s4 == s6); //false
    System.out.println(s4 == s7); //false

    System.out.println(s5 == s6); //false
    System.out.println(s5 == s7); //false

    System.out.println(s6 == s7); //false

    String s8 = s5.intern();
    //返回值得到的s8是使用常量池中已经存在的"javaEEhadoop"
    System.out.println(s3 == s8); //true
}
```

```java
@Test
public void test1() {
    String s1 = "javaEEhadoop";
    final String s2 = "javaEE";
    String s3 = s2 + "hadoop";
    System.out.println(s1 == s3); //true
// 常量与常量的拼接结果在常量池。
}
```

#### String陷阱

- String s1 = "a"; 
  - 说明：在字符串常量池中创建了一个字面量为"a"的字符串。
- s1 = s1 + "b"; 
  - 说明：实际上原来的“a”字符串对象已经丢弃了，现在在堆空间中产生了一个字符串s1+"b"（也就是"ab")。如果多次执行这些改变串内容的操作，会导致大量副本字符串对象存留在内存中，降低效率。如果这样的操作放到循环中，会极大影响程序的性能。
- String s2 = "ab";
  - 说明：直接在字符串常量池中创建一个字面量为"ab"的字符串。
- String s3 = "a" + "b";
  - 说明：s3指向字符串常量池中已经创建的"ab"的字符串。
- String s4 = s1.intern();
  - 说明：堆空间的s1对象在调用intern()之后，会将常量池中已经存在的"ab"字符串赋值给s4。

#### 面试题

```java
public class StringTest {
    String str = new String("good");
    char[] ch = {'t', 'e', 's', 't'};

    public void change(String str, char ch[]) {
        str = "test ok";
        ch[0] = 'b';
    }

    public static void main(String[] args) {
        StringTest ex = new StringTest();
        ex.change(ex.str, ex.ch);
        System.out.println(ex.str);//good
        //String的不可变性
        System.out.println(ex.ch);//best
    }
}
```

### String的方法

**1.常用**

- int length()：返回字符串的长度： return value.length
- char charAt(int index)： 返回某索引处的字符return value[index]
- boolean isEmpty()：判断是否是空字符串：return value.length == 0
- String toLowerCase()：使用默认语言环境，将 String 中的所有字符转换为小写
- String toUpperCase()：使用默认语言环境，将 String 中的所有字符转换为大写
- String trim()：返回字符串的副本，忽略前导空白和尾部空白
- boolean equals(Object obj)：比较字符串的内容是否相同
- boolean equalsIgnoreCase(String anotherString)：与equals方法类似，忽略大小写
- String concat(String str)：将指定字符串连接到此字符串的结尾。 等价于用“+”
- int compareTo(String anotherString)：比较两个字符串的大小
- String substring(int beginIndex)：返回一个新的字符串，它是此字符串的从beginIndex开始截取到最后的一个子字符串。
- String substring(int beginIndex, int endIndex) ：返回一个新字符串，它是此字符串从beginIndex开始截取到endIndex(不包含)的一个子字符串。

**2.前缀后缀**

- boolean endsWith(String suffix)：测试此字符串是否以指定的后缀结束
- boolean startsWith(String prefix)：测试此字符串是否以指定的前缀开始
- boolean startsWith(String prefix, int toffset)：测试此字符串从指定索引开始的子字符串是否以指定前缀开始

**3.索引（下标）位置**

- boolean contains(CharSequence s)：当且仅当此字符串包含指定的 char 值序列时，返回 true
- int indexOf(String str)：返回指定子字符串在此字符串中第一次出现处的索引
- int indexOf(String str, int fromIndex)：返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始
- int lastIndexOf(String str)：返回指定子字符串在此字符串中最右边出现处的索引
- int lastIndexOf(String str, int fromIndex)：返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索
     - 注：indexOf和lastIndexOf方法如果未找到都是返回-1
     - indexOf(str)和lastIndexOf(str)返回值相同的情况:
        - 存在唯一的一个str
        - 不存在str

**4.正则**

- String replace(char oldChar, char newChar)：返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。
- String replace(CharSequence target, CharSequence replacement)：使用指定的字面值替换序列替换此字符串所有匹配字面值目标序列的子字符串。
- String replaceAll(String regex, String replacement) ： 使用给定的replacement 替换此字符串所有匹配给定的正则表达式的子字符串。
- String replaceFirst(String regex, String replacement) ： 使用给定的replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。
- boolean matches(String regex)：告知此字符串是否匹配给定的正则表达式。
- String[] split(String regex)：根据给定正则表达式的匹配拆分此字符串。
- String[] split(String regex, int limit)：根据匹配给定的正则表达式来拆分此字符串，最多不超过limit个，如果超过了，剩下的全部都放到最后一个元素中。

```java
public class StringMethodTest {
    @Test
    public void test1() {
        String s1 = "hellpworld";
        System.out.println(s1.length()); //10
        System.out.println(s1.charAt(0)); // h
        System.out.println(s1.charAt(9)); //d
//        s1 = "";
//        System.out.println(s1.isEmpty()); //true

//        s1.toLowerCase(); s1.toUpperCase() s1不可变，仍然为原来的字符串。
        System.out.println(s1.toLowerCase());
        String s2 = s1.toUpperCase();
        System.out.println(s1);  // s1不可变，仍然为原来的字符串
        System.out.println(s2);

        System.out.println(s2.equals(s1)); //false
        //忽略大小写的equals()
        System.out.println(s2.equalsIgnoreCase(s1)); //true

        String s3 = "  he llo   w o rld   ";
        String s4 = s3.trim();//去除首尾的空格
        System.out.println("-" + s3 + "-"); //-  he llo   w o rld   -
        System.out.println("-" + s4 + "-"); //-he llo   w o rld-

        String s5 = "abc";
        //连接字符串
        String s6 = s5.concat("def");
        System.out.println(s6); //abcdef

        String s7 = "abc";
        String s8 = new String("bda");
//        compareTo()字符串做加减法 //涉及到字符串的排序
//        依照ASCII码的加减：a=97,b=98,c=99,d=100;
//        "abc".compareTo("bda"); (97+98+99) - (98+100+97) == -1;
        System.out.println(s7.compareTo(s8)); //-1

        String s9 = "天下第一，举世无二";
        //substring(n) 截取从下标n(包括n)开始一直到所有的字符
        System.out.println(s9.substring(2));
        //substring(n,m) 截取从下标n(包括n)开始一直到下标m(不包括m)的字符
        System.out.println(s9.substring(2,3));
    }

    @Test
    public void test2(){
        String str1 = "helloworld";

        boolean b1 = str1.endsWith("ld");
        System.out.println(b1); //true

        boolean b2 = str1.startsWith("He");
        System.out.println(b2); //false

        boolean b3 = str1.startsWith("ll",2);
        //从下标2开始的对应字符串是否匹配
        System.out.println(b3); //true

        String str2 = "wo";
//        s1.contains(s2) s1是否包含s2
        System.out.println(str1.contains(str2)); //true

//        s1.indexOf(s2) s2在s1中第一次出现的下标位置;没有则返回-1
        System.out.println(str1.indexOf("lo")); //3
        System.out.println(str1.indexOf("HHH")); //-1
//        s1.indexOf(s2,n) 从指定的下标n（包含该下标n)开始查找,返回的仍然是整个字符串的下标
        System.out.println(str1.indexOf("lo", 3)); //3
//        s1.lastIndexOf(s1) 从字符串后面开始查找(最后一次)出现的下标
        System.out.println(str1.lastIndexOf("l")); //8
//        s1.lastIndexOf(s1,n) 从指定的下标n（包含该下标n)(仍然是正向的下标)开始反向查找，最后一次出现的下标位置
//        ,返回的仍然是整个字符串的下标
        System.out.println(str1.lastIndexOf("l",8)); //8
    }

    @Test
    public void test3(){
        String str1 = "天下第一，下，举世无二";
//        替换字符串中所有匹配的 一个字符''或字符串""
        String str2 = str1.replace('下','世');

        System.out.println(str1); //天下第一，下，举世无二
        System.out.println(str2); //天世第一，世，举世无二
        System.out.println(str1.replace("天下", "世上")); //世上第一，下，举世无二

        String str01 = "12bhbad2sada3ifb73nn2j9";
//        把字符串中的数字替换成'，'如果结果中开头和结尾有'，'的话去掉，
//        d-数字; +-数量; ^-开头; $-结尾
        String string = str01.replaceAll("\\d+",",").replaceAll("^,|,$","");
        System.out.println(string); //bhbad,sada,ifb,nn,j

        String str02 = "12345";
//        判断石头人字符串中是否全部由数字组成，即由1-n个数字组成
        boolean matches = str02.matches("\\d+");
        System.out.println(matches); //true

        String tel = "1885-0035771";
        // \\d{7,8} 是否是7-8位的数字
        boolean result = tel.matches("1885-\\d{7,8}");
        System.out.println(result);//true

        String str03 = "hello|world|java";
//        切片
        String[] strs = str03.split("\\|");
        for(int i =0;i<strs.length;i++){
            System.out.println(strs[i]);
        }

        System.out.println();

        String str04 = "hellp.world.java";
//        切片
        String[] strs2 = str04.split("\\.");
        for(int i = 0;i<strs2.length;i++){
            System.out.println(strs2[i]);
        }
    }
}
```

### String类与其他结构之间的转换

#### String与基本数据类型/包装类转换

**字符串-->基本数据类型/包装类**

- Integer包装类的`public static int parseInt(String s)`：可以将由“数字”字符组成的字符串转换为整型。  
- 类似地,使用java.lang包中的Byte、Short、Long、Float、Double类调相应的类方法可以将由“数字”字符组成的字符串，转化为相应的基本数据类型。

```
基本数据类型/包装类 变量 = 包装类.parseXxx(字符串);
```

**基本数据类型/包装类-->字符串**

- 调用String类重载的`public String valueOf(int n)`可将int型转换为字符串
- 相应的valueOf(byte b)、valueOf(long l)、valueOf(float f)、valueOf(double d)、valueOf(boolean b)可由参数的相应类型到字符串的转换

```
String 变量 = String.valueOf(基本数据类型/包装类)

或

String 变量 = 基本数据类型/包装类 + "";
```

#### String与char[]之间的转换

**String-->char[]**

1. 调用String的toCharArray()方法;
2. `public void getChars(int srcBegin, int srcEnd, char[] dst, int dstBegin)`：提供了将指定索引范围内的字符串存放到数组中的方法。

```java
@Test
public void test1(){
    String str1 = "abc123";

    char[] chars = str1.toCharArray();
    for(int i =0;i<chars.length;i++){
        System.out.println(chars[i]);
    }
}
```

**char[]-->String**

- 调用String的构造器`new String(chars)`即可

```java
public void test2(){
    char chars[] = new char[]{'h','e','l','l','o'};

    String str = new String(chars);
    System.out.println(str); //hello
}
```

#### String与byte[]之间的转换(字节数组)

- 编码: 字符串-->字节(二级制数据)
- 解码: 字节-->字符串 编码的逆过程

**Sting-->byte[] 编码**

- bytes[] 字节数组 = 字符串.getBytes("指定编码集");
- 需要该方法引入throws UnsupportedEncodingException 

**byte[]-->String 解码**

- String 字符串 = new String(字节数组,"指定编码集");

- 解码时使用的字符集必须与编码时使用的字符集相一致
  - 否则会出现乱码

```java
   @Test
    public void test4() throws UnsupportedEncodingException {
        String str1 = "abc123";

//      ----编码
        byte[] bytes = str1.getBytes(); //使用默认的字符集UTF-8，进行转换
//        ASCII码
        System.out.println(Arrays.toString(bytes));
//        [97, 98, 99, 49, 50, 51]

        String str2 = "abc123中国";
//        String str1 = "abc123中国"; 需要指定字符集
        byte[] bytes2 = str2.getBytes();
//        [97, 98, 99, 49, 50, 51, -28, -72, -83, -27, -101, -67] UTF-8中一个中文三个字节
        //使用指定的字符集，进行转换
//        需要throws UnsupportedEncodingException;
        byte[] gbks = str2.getBytes("gbk");
        //使用gbk字符集进行编码
//          编码: 字符串-->字节(二级制数据)
//          解码: 字节-->字符串 编码的逆过程
        System.out.println(Arrays.toString(gbks));
//        [97, 98, 99, 49, 50, 51, -42, -48, -71, -6]

//        ----解码
        String string = new String(bytes2); //使用默认的字符集解码
        System.out.println(string); //abc123中国

        String str3 = new String(gbks);
//        编码解码使用的字符集不一致 乱码
        System.out.println(str3); //abc123�й�
//        指定解码的字符集
        String str4 = new String(gbks, "gbk");
        System.out.println(str4); //abc123中国
    }
```

### 常见算法题目

#### 1. 模拟一个trim方法，去除字符串两端的空格。

```java
public class StringTrimTest {
    public static void main(String[] args) {
        String str1 = "   sad   test   ";
        String str2 = new String(myTrim(str1));

        System.out.println("-" + str1 + "-");
        System.out.println("-" + str2 + "-");
    }

    public static char[] myTrim(String str) {
        //如果空字符串则：
        if(str.isEmpty() == true){
            throw new RuntimeException("空字符串");
        }

        //获取str字符串，并转换为char[]
        char[] copyChars = str.toCharArray();
        //暂存copyChars中的元素（去除空格后，但是存在未赋值的元素）
        char[] intoChars = new char[str.length()];
        int counter = 0; //计数，用来充当returnChar的长度

        //去除空格
        for (int i = 0; i < str.length(); i++) {
            //空格的ASCII码值是32
            if (str.charAt(i) == 32) {
                continue;
            } else {
                intoChars[counter++] = copyChars[i];
            }
        }

        //最终为返回的字符串赋值的char[]
        char[] returnChars = new char[counter];

        //将intoChars的元素放入returnChars
        for (int i = 0; i < returnChars.length; i++) {
            returnChars[i] = intoChars[i];
        }

        return returnChars;
    }
}
```

- 正则切片的方式


#### 2. 将一个字符串进行反转。将字符串中指定部分进行反转。

- 比如“abcdefg”反转为”abfedcg”

```java
public String reverse(String str, int startIndex, int endIndex) {
    if (str != null && str.length() != 0) {
        char[] arr = str.toCharArray();
        for (int x = startIndex, y = endIndex; x < y; x++, y--) {
            char temp = arr[x];
            arr[x] = arr[y];
            arr[y] = temp;
        }
        return new String(arr);
    }
    return null;
}
```

```java
public class StringFlipTest {
    public static void main(String[] args) {
        String str1 = "abcdefg";
        String str2 = new String(flip(str1, 2, 5));
        System.out.println(str2);
    }

    public static char[] flip(String str, int from, int to) {
        //如果空字符串则：
        if (str.isEmpty() == true) {
            throw new RuntimeException("空字符串");
        }

        //如果from>to
        if (from > to) {
            throw new RuntimeException("from的值不能大于to");
        }

        char[] chars = str.toCharArray();
        while (from < to) {
            char temp = chars[to];
            chars[to] = chars[from];
            chars[from] = temp;

            from++;
            to--;
        }

        return chars;
    }
}
```

```java
public String reverse2(String str, int startIndex, int endIndex) {
    if (str != null && str.length() != 0) {
//        第一部分
        String reverseStr = str.substring(0, startIndex);
//        第二部分
        for (int i = endIndex; i >= startIndex; i--) {
            reverseStr += str.charAt(i);
        }
//        第三部分
        reverseStr += str.substring(endIndex + 1);
        return reverseStr;
    }
    return null;
}
```

```java
public String reverse3(String str, int startIndex, int endIndex) {
    if (str != null && str.length() != 0) {
        StringBuffer sbf = new StringBuffer(str.length());

        //第一部分
        sbf.append(str.substring(0,startIndex));

        //第二部分
        for (int i = endIndex; i >= startIndex; i--) {
            sbf.append(str.charAt(i));
        }

        //第三部分
        sbf.append(str.substring(endIndex + 1));

        return sbf.toString();
    }
    return null;
}
```

#### 3. 获取一个字符串在另一个字符串中出现的次数。

- 比如：获取“ ab”在 “abkkcadkabkebfkabkskab” 中出现的次数

```sql
public int getCount(String mainStr, String subStr) {
    int mainLength = mainStr.length();
    int subLength = subStr.length();
    int count = 0;
    int index;

    if (subLength < mainLength) {
        while ((index = mainStr.indexOf(subStr)) != -1) {
            count++;
            mainStr = mainStr.substring((index + subStr.length()));
        }
        return count;
    }
    return 0;
}
```

```sql
public int getCount2(String mainStr, String subStr) {
    int mainLength = mainStr.length();
    int subLength = subStr.length();
    int count = 0;
    int index;

    if (subLength < mainLength) {
        while ((index = mainStr.indexOf(subStr, 0)) != -1) {
            count++;
            index += subLength;
        }
        return count;
    }
    return 0;
}
```

```java
public class StringCountTest {
    public static void main(String[] args) {
        String str1 = "abkkcadkabkebfkabkskab";
        String str2 = "ab";

        System.out.println(countRepeat02(str2, str1));
    }

    public static int countRepeat02(String inStr, String totalStr) {
        //空字符串错误：
        if (inStr.isEmpty() || totalStr.isEmpty()) {
            throw new RuntimeException("空字符串错误");
        }

        //计算重复次数
        int counter = 0;

        //切片
        String[] strs = totalStr.split(inStr);
        //计算切片剩下的总长度
        int length = 0;
        for (int i = 0;i<strs.length;i++){
            length += strs[i].length();
        }

        //计算重复次数
        counter = (totalStr.length() - length) / inStr.length();

        return counter;
    }
}
```

#### 4. 获取两个字符串中最大相同子串。比如：str1 = "abcwerthelloyuiodef“;str2 = "cvhellobnm"

- 提示：将短的那个串进行长度依次递减的子串与较长的串比较。

```java
//只能找到第一个
public String getMaxString(String str1, String str2) {

    if(str1 != null && str1 != null){
        String maxString = (str1.length() >= str2.length()) ? str1 : str2;
        String minString = (str1.length() < str2.length()) ? str1 : str2;

        int length = minString.length();

        for (int i = 0; i < length; i++) {
            for (int x = 0, y = length - i; y <= length; x++, y++) {
                String subStr = minString.substring(x, y);
                if (maxString.contains(subStr)) {
                    return subStr;
                }
            }
        }
    }

    return null;
}
```

```java
public String[] getMaxString(String str1, String str2) {

    if (str1 != null && str1 != null) {
        StringBuffer stringBuffer = new StringBuffer();
        String maxString = (str1.length() >= str2.length()) ? str1 : str2;
        String minString = (str1.length() < str2.length()) ? str1 : str2;

        int length = minString.length();

        for (int i = 0; i < length; i++) {
            for (int x = 0, y = length - i; y <= length; x++, y++) {
                String subStr = minString.substring(x, y);
                if (maxString.contains(subStr)) {
                    stringBuffer.append(subStr + ",");
                }
            }

            if(stringBuffer.length() != 0){
                break;
            }
        }

        String[] strings = stringBuffer.toString().replaceAll(",$","").split("\\,");
        return  strings;
    }

    return null;
}
```

#### 5.对字符串中字符进行自然顺序排序。
- 提示：
1. 字符串变成字符数组。
2. 对数组排序，选择，冒泡，Arrays.sort();
3. 将排序后的数组变成字符串。

```java

```

### StringBuffer,StringBuilder

**转换**

- String --> StringBuffer/StringBuilder
   - 调用StringBuffer/StringBuilder构造器
- StringBuffer/StringBuilder --> String
   - 调用String构造器
   - 对应的toString()方法   

**String/StringBuffer/StringBulilder的异同**

- String: 
   - 不可变的字符序列
   - 底层结构使用char[]存储
   - 效率最低
- StringBuffer: 
   - 可变的字符序列
   - 线程安全（sychronized)  效率较低
   - 底层结构使用char[]存储
- StringBuilder:  jdk5.0新增
   - 可变的字符序列
   - 线程不安全  效率较高
   - 底层使用char[]存储
   
```java
public class StringBufferStringBuilderTest {
    @Test
    public void test1(){
        StringBuffer sb1 = new StringBuffer("abc");
        sb1.setCharAt(0,'m');
        System.out.println(sb1); //mbc
    }
}
```
   
**源码分析**

1. StringBuffer的长度问题
2. 扩容问题: 如果要添加的数据，底层数组存不下，那么需要扩容底层的数组。
  - 开发中，建议使用StringBuffer(int capacity) 或StringBuilder(int capacity);指定额外的数组长度(默认16)   
     - 默认情况下扩容为原来的2倍+2，同时将原有数组中的元素复制到新的数组中
     - 如果还是不满足，则直接将该数组作为新的数组。

```java
String str = new String(); //new char[0];
String str1 = new String("abc"); //new char[3]{'a','b','c'};

StringBuffer sb1 = new StringBuffer(); //char[] value = new char[16]; 相当于底层创建了一个长度是16的数组
System.out.println(sb1.length()); //0 而不是16
sb1.append('a'); //value[0] = 'a';
sb1.append('b'); //value[1] = 'b';

StringBuffer sb2 = new StringBuffer("abc"); // char[] value = new char["abc".length() + 16];
System.out.println(sb2.length()); //3 而不是19
```

#### StringBuffer类的常用方法

- StringBuffer append(xxx)：提供了很多的append()方法，用于进行字符串拼接
- StringBuffer delete(int start,int end)：删除指定位置的内容,包括end位置的内容
- StringBuffer replace(int start, int end, String str)：把[start,end)位置替换为str
- StringBuffer insert(int offset, xxx)：在指定位置插入xxx
- StringBuffer reverse() ：把当前字符序列逆转
- public int indexOf(String str)
- public String substring(int start,int end) : 返回[start,end)的字符串
- public int length()
- public char charAt(int n )
- public void setCharAt(int n ,char ch)

```java
@Test
public void test3() {
    StringBuffer s1 = new StringBuffer("abc");

    s1.append(1); //追加
    s1.append('d'); //追加
    System.out.println(s1); //abc1d

    s1.delete(2, 4); //只删除包括起始下标到（不包括）结束下标
    System.out.println(s1); //abd

    s1.replace(1, 2, "hello"); //替换[1,2)的内容
    System.out.println(s1); //ahellod

    s1.insert(2, false); //在指定位置插入内容
    System.out.println(s1); //ahfalseellod
    System.out.println(s1.length()); //12 false也视为字符串

    s1.reverse();
    System.out.println(s1); //dolleeslafha

    String s2 = s1.substring(1, 3);
    System.out.println(s1); //dolleeslafha
    System.out.println(s2); //ol
}
```

#### 方法链的原理

#### 题

```java
@Test
public void test(){
    String str = null;
    StringBuffer sb = new StringBuffer();
    sb.append(str);
    System.out.println(sb.length());//
    System.out.println(sb);//
    StringBuffer sb1 = new StringBuffer(str);
    System.out.println(sb1);//   
}
```

## 日期时间API

### JDK8之前日期时间API

**日期API的迭代**

1. jdk 1.0 Date类
2. jdk 1.1 Calendar类，一定程度上替换Date类
3. jdk 1.8 提出一套新的API

**前两代的问题**

- 可变性：像日期和时间这样的类应该是不可变的。
- 偏移性：Date中的年份是从1900开始的，而月份都从0开始。
- 格式化：格式化只对Date有用，Calendar则不行。
- 此外，它们也不是线程安全的；不能处理闰秒等。

#### java.lang.System类

- System类提供的`public static long currentTimeMillis()`用来返回当前时间与1970年1月1日0时0分0秒之间以毫秒为单位的时间差。
  - 此方法适于计算时间差。
  - 时间戳
  
**计算世界时间的主要标准有：**

- UTC(Coordinated Universal Time)
- GMT(Greenwich Mean Time)
- CST(Central Standard Time

#### java.util.Date类

**表示特定的瞬间，精确到毫秒**

- 构造器：
  - Date()：使用无参构造器创建的对象可以获取本地当前时间。
  - Date(long date)  ： 通过毫秒数（时间戳）获取时间
- 常用方法
  - `getTime()`: 返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此Date 对象表示的毫秒数。
  - `toString()`: 把此 Date 对象转换为以下形式的 String：
      - 格式：
      ![](C:/NoteBook/pictures/166405516247526.png =216x)
      
```java
public void test2(){
//    空参构造器：创建当前时间的Date对象
    Date date1 = new Date();
    System.out.println(date1);
//     Wed Oct 26 16:51:02 CST 2022
//     星期 月 日  时间   时间标准 年
    System.out.println(date1.getTime());
//      1666774415833
//       时间戳

//     指定时间的构造器
    Date date2 = new Date(1666774415833L);
    System.out.println(date2);
//     Wed Oct 26 16:53:35 CST 2022
}
```

#### java.sql.Date类 （继承自java.util.Date类)

- 对应数据库中的日期类型的变量

**java.util.Date和java.sql.Date的转换**

```java
public void test3() {
    //创建java.sql.Date的对象
    java.sql.Date date = new java.sql.Date(174242344342L);
    System.out.println(date); //1975-07-11
    //java.sql.Date --> java.util.Date
//     1.
    Date date1 = new java.sql.Date(13819381293819L);
    java.sql.Date date2 = (java.sql.Date)date1;
//     2.
    Date date3 = new Date();
//     java.sql.Date date4 = (java.sql.Date)date3; 报错
    java.sql.Date date4 = new java.sql.Date(date.getTime());
}
```

#### java.text.SimpleDateFormat类

Date类的API不易于国际化，大部分被废弃了，java.text.SimpleDateFormat类是一个不与语言环境有关的方式来格式化和解析日期的具体类。

**对日期Date类的格式化和解析：**

- 实例化SimpleDateFormat
  - 使用默认构造器 
     - `new SimpleDateFormat();`
  - 指定方式格式化和解析 调用带参的构造器:
     - `new SimpleDateFormat("yyyyy.MMMMM.dd GGG hh:mm aaa");`
     - 通过API查找SimpleDateFormat类的具体格式设置
     
**格式化：日期-->字符串**

- format()方法
  - `SimpleDateFormat对象.format(Date对象);`

**解析：字符串-->日期  (格式化的逆过程)**

- parse()方法
  - `SimpleDateFormat对象.parse(字符串);` 
  - 要求字符串必须符合SimpleDateFormat识别的格式
  - 需要方法加上`throws ParseException`
  
```java
@Test
public void testSimpleDateFormat() throws ParseException {
    //实例化SimpleDateFormat 使用默认的构造器
    SimpleDateFormat sdf = new SimpleDateFormat();

    Date date = new Date();
    System.out.println(date);
    //Thu Oct 27 22:11:08 CST 2022

    //格式化：日期-->字符串
    String format = sdf.format(date);
    System.out.println(format);
    //2022/10/27 下午10:11

    //解析
    String str = "2022/10/23 上午10:10";
    //格式要求默认："yyyy/MM/dd aaa hh:mm:ss " 不同版本不一样
    Date date1 = sdf.parse(str);
    System.out.println(date1);
    //Sun Oct 23 10:10:00 CST 2022

    //指定方式格式化和解析 调用带参的构造器
    //SimpleDateFormat sdf1 = new SimpleDateFormat("yyyyy.MMMMM.dd GGG hh:mm aaa");
    SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
    //格式化
    String format1 = sdf1.format(date);
    System.out.println(format1);
    //02022.十月.27 公元 10:20 下午
    //2022-23-27 10:23:55
    //解析: 要求字符串必须符合SimpleDateFormat识别的格式（通过构造器参数实现)
    //否则报错
    Date date2 = sdf1.parse("1999-12-31 23:59:59");
    System.out.println(date2);
    //Sun Jan 31 23:59:59 CST 1999
}
```

##### 练习

###### 练习1

字符串"2020-09-08"转换为java.sql.Date

```java
@Test
public void test2() throws ParseException {
    String birth = "2020-09-08";
    
    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
    Date date = simpleDateFormat.parse(birth);
    
    java.sql.Date date1 = new java.sql.Date(date.getTime());
    System.out.println(date1); //2020-09-08
}
```

###### 练习2

-“三天打鱼两天晒网”从1990-01-01开始，问xxxx-xx-xx打鱼还是晒网?
  - 即：计算 总天数%5
  - 1,2,3 打鱼
  - 4,0 晒网
  
**1.利用时间戳**

```java
public void fishing(String dateStr) throws ParseException {

    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd");
    //开始时间
    Date startDate = simpleDateFormat.parse("1990-01-01");
    //提问的时间
    Date endDate = simpleDateFormat.parse(dateStr);

    //相差时间
    long totalTime = endDate.getTime() - startDate.getTime();
    //当前的天数
    int day = (int) (totalTime / (24 * 60 * 60 * 1000) + 1) % 5;
    //注意抹零的影响
    switch (day) {
        case 1:
        case 2:
        case 3:
            System.out.println("打鱼");
            break;
        case 4:
        case 0:
            System.out.println("晒网");
            break;
    }
}
```

**2.计算天数**

```java

```

#### java.util.Calendar(日历)类

- Calendar是一个抽象基类，主用用于完成日期字段之间相互操作的功能。

**获取Calendar实例的方法**

1. 使用`Calendar.getInstance()`方法
2. 调用它的子类GregorianCalendar的构造器。
   - 一个Calendar的实例是系统时间的抽象表示，通过`get(int field)`方法来取得想要的时间信息。
   - 比如YEAR、MONTH、DAY_OF_WEEK、HOUR_OF_DAY 、MINUTE、SECOND

具体field查看API

- public int get(int field)
  - 获取日历 
- public void set(int field,int value)
  - 设置时间  
- public void add(int field,int amount) 
  - 增加或减少时间 , 负数减 
- public final Date getTime()
  - 日历类-->Date类 
- public final void setTime(Date date)
  - Date类-->日历类 

**注意:**

- 获取月份时：一月是0，二月是1，... ，12月是11
- 获取星期时：周日是1，周二是2 ... 周六是7

```java
@Test
public void testCalendar(){
//1.实例化
    //方式一：创建子类GregorianCalendar的对象
    //方式二：调用其静态方法getInstance()
    Calendar calendar = Calendar.getInstance();
    //System.out.println(calendar.getClass());
    //class java.util.GregorianCalendar 仍然来自于其子类

//2.常用方法
//get()
    int days = calendar.get(Calendar.DAY_OF_MONTH);
    //get(Calendar.该类内部的静态属性)  具体查看API
    System.out.println(days); //返回本个月的第几天
    System.out.println(calendar.get(Calendar.DATE)); //返回-日
//set()
    calendar.set(Calendar.DAY_OF_MONTH,22);
    //修改calendar对象内的属性 //Calendar类的属性不变：不可变性
    days = calendar.get(Calendar.DAY_OF_MONTH);
    System.out.println(days); //22 返回修改后的日期
//add()
    calendar.add(Calendar.DAY_OF_MONTH,3); //负数则减
    //增加3天
    days = calendar.get(Calendar.DAY_OF_MONTH);
    System.out.println(days); //25 返回增加后的日期
    calendar.add(Calendar.DAY_OF_MONTH,-1); //负数则减
    //减1天
    days = calendar.get(Calendar.DAY_OF_MONTH);
    System.out.println(days); //24 返回减后的日期
//getTime() 日历类-->Date类
    Date time = calendar.getTime();
    System.out.println(time);
    //Mon Oct 24 23:29:51 CST 2022
//setTime() Date类-->日历类
    Date date = new Date();
    calendar.setTime(date);
    System.out.println(calendar.get(Calendar.DAY_OF_MONTH));
    //27
}
```

### JDK8中新日期时间API

- java.time – 包含值对象的基础包
- java.time.chrono – 提供对不同的日历系统的访问
- java.time.format – 格式化和解析时间和日期
- java.time.temporal – 包括底层框架和扩展特性
- java.time.zone – 包含时区支持的类

#### LocalDate、LocalTime、LocalDateTime

LocalDate、LocalTime、LocalDateTime 类是其中较重要的几个类，它们的实例是不可变的对象，分别表示使用 ISO-8601日历系统的日期、时间、日期和时间。它们提供了简单的本地日期或时间，并不包含当前的时间信息，也不包含与时区相关的信息

**不可变的对象**

- LocalDate代表IOS格式（yyyy-MM-dd）的日期,可以存储 生日、纪念日等日期。

- LocalTime表示一个时间，而不是日期。
- LocalDateTime是用来表示日期和时间的，这是一个最常用的类之一
   - LocalDateTime使用频率更高 
   
**方法**

- now() / * now(ZoneId zone) 
    - 静态方法，根据当前时间创建对象/指定时区的对象
- of() 静态方法，根据指定日期/时间创建对象
- getDayOfMonth()/getDayOfYear()
   -  获得月份天数(1-31) /获得年份天数(1-366)
- getDayOfWeek() 获得星期几(返回一个 DayOfWeek 枚举值)
- getMonth() 
   - 获得月份, 返回一个 Month 枚举值
- getMonthValue() / getYear() 
   - 获得月份(1-12) /获得年份
- getHour()/getMinute()/getSecond() 
   - 获得当前对象对应的小时、分钟、秒
- withDayOfMonth()/withDayOfYear()/withMonth()/withYear() 
   - 将月份天数、年份天数、月份、年份修改为指定的值并返回新的对象
- plusDays(), plusWeeks(), plusMonths(), plusYears(),plusHours() 
   - 向当前对象添加几天、几周、几个月、几年、几小时
- minusMonths() / minusWeeks()/minusDays()/minusYears()/minusHours() 
   - 从当前对象减去几月、几周、几天、几年、几小时

```java
@Test
public void test1(){
    //now() 获取当前时间
    LocalDate localDate = LocalDate.now();
    LocalTime localTime = LocalTime.now();
    LocalDateTime localDateTime = LocalDateTime.now();

    System.out.println(localDate);
    System.out.println(localTime);
    System.out.println(localDateTime);
    //2022-10-28
    //15:54:10.480256600
    //2022-10-28T15:54:10.480256600

    //of() 设置指定的年月日时分秒时，没有偏移量
    LocalDateTime localDateTime1 = LocalDateTime.of(2020, 10, 6, 13, 23, 13);
    System.out.println(localDateTime1);
    //2020-10-06T13:23:13

    //getXxx() 获取日期属性
    System.out.println(localDateTime.getDayOfMonth());
    System.out.println(localDateTime.getDayOfWeek());
    System.out.println(localDateTime.getDayOfYear());
    System.out.println(localDateTime.getMinute());
    //28
    //FRIDAY
    //301
    //28

//体现不可变性
    //withXxx() 修改
    LocalDate localDate1 = localDate.withDayOfMonth(22);
    System.out.println(localDate);
    System.out.println(localDate1);
    //2022-10-28
    //2022-10-22

    LocalDateTime localDateTime2 = localDateTime.withHour(4);
    System.out.println(localDateTime);
    System.out.println(localDateTime2);
    //2022-10-28T16:34:05.769278400
    //2022-10-28T04:34:05.769278400

    //plusXxx() 添加
    LocalDateTime localDateTime3 = localDateTime.plusYears(3);
    System.out.println(localDateTime);
    System.out.println(localDateTime3);
    //2022-10-28T16:35:34.425305800
    //2025-10-28T16:35:34.425305800
}
```

#### Instant类

- 类似于Java.util.Date类

Instant：时间线上的一个瞬时点。 这可能被用来记录应用程序中的事件时间戳。

- java.time包通过值类型Instant提供机器视图。Instant表示时间线上的一点，而不需要任何上下文信息，
    - 例如，时区。概念上讲，它只是简单的表示自1970年1月1日0时0分0秒（UTC）开始的秒数。
    - 因为java.time包是基于纳秒计算的，所以Instant的精度可以达到纳秒级。 
       -  (1 ns = 10-9 s) 1秒 = 1000毫秒 =10^6微秒=10^9纳秒

**方法**

- now()
   - 静态方法，返回默认UTC时区的Instant类的对象
- ofEpochMilli(long epochMilli) 
   - 静态方法，返回在1970-01-01 00:00:00基础上加上指定毫秒数之后的Instant类的对象
- atOffset(ZoneOffset offset) 
   - 结合即时的偏移来创建一个 OffsetDateTime
- toEpochMilli() 
   - 返回1970-01-01 00:00:00到当前时间的毫秒数，即为时间戳

```java
@Test
public void test(){
    Instant instant = Instant.now();
    System.out.println(instant);
    //2022-10-28T08:40:37.774837200Z

    //添加时间的偏移
    OffsetDateTime offsetDateTime = instant.atOffset(ZoneOffset.ofHours(8));
    System.out.println(offsetDateTime);
    //2022-10-28T16:42:42.421531800+08:00

    //toEpochMilli() 获取自1970年1月1日0时0分0秒（UTC）开始的毫秒数
    long milli = instant.toEpochMilli();
    System.out.println(milli);
    //1666946757201

    //ofEpochmilli() 通过给定的毫秒数
    Instant instant1 = instant.ofEpochMilli(12312312312L);
    System.out.println(instant1);
    //1970-05-23T12:05:12.312Z
}
```

#### 格式化与解析日期或时间

**java.time.format.DateTimeFormatter 类：**

类似于SimpleDateFormat类

**该类提供了三种格式化方法：**

1. 预定义的标准格式。
   - ISO_LOCAL_DATE_TIME
   - ISO_LOCAL_DATE
   - ISO_LOCAL_TIME
2. 本地化相关的格式。
   - 如：ofLocalizedDateTime(FormatStyle.LONG)
3. 自定义的格式。
   - 如：ofPattern(“yyyy-MM-dd hh:mm:ss”)

**方法**

- ofPattern(String pattern) 
   - 静态方法 ， 返 回 一 个 指 定 字 符 串 格 式 的
- DateTimeFormatterformat(TemporalAccessor t)
   - 格式化一个日期、时间，返回字符串
- parse(CharSequence text) 
   - 将指定格式的字符序列解析为一个日期、时间

```java
@Test
public void test3(){
//方式1预定义的标准格式。
    DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ISO_LOCAL_DATE_TIME;
      //格式化 日期-->字符串
    LocalDateTime localDateTime = LocalDateTime.now();
    String str1 = dateTimeFormatter.format(localDateTime);
    dateTimeFormatter.format(localDateTime);
    System.out.println(str1);
    System.out.println(localDateTime);
      //2022-10-28T17:00:23.5674663
      //2022-10-28T17:00:23.567466300
      //解析 字符串-->日期
    TemporalAccessor parse = dateTimeFormatter.parse("2019-02-18T17:00:23.22222");
    System.out.println(parse);
       //{},ISO resolved to 2019-02-18T17:00:23.222220
//方式2本地化相关的格式。
    DateTimeFormatter dateTimeFormatter1 = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.SHORT);
    //格式化
    String format = dateTimeFormatter1.format(localDateTime);
    System.out.println(format);
    //2022/10/28 下午5:04

    DateTimeFormatter formatter = DateTimeFormatter.ofLocalizedDate(FormatStyle.FULL);
    //格式化
    String format1 = formatter.format(LocalDate.now());
    System.out.println(format1);
    //2022年10月28日星期五

//方式3自定义的格式。
    DateTimeFormatter dateTimeFormatter2 = DateTimeFormatter.ofPattern("yyyy-MM-dd hh:mm:ss");
    //格式化
    String str = dateTimeFormatter2.format(LocalDateTime.now());
    System.out.println(str);
    //2022-10-28 05:10:10

    //解析
    TemporalAccessor parse1 = dateTimeFormatter2.parse("2019-10-23 12:12:23");
    System.out.println(parse1);
    //{MilliOfSecond=0, HourOfAmPm=0, NanoOfSecond=0, SecondOfMinute=23, MinuteOfHour=12, MicroOfSecond=0},ISO resolved to 2019-10-23
}
```

#### 其它API

**ZoneId：**

- 该类中包含了所有的时区信息，一个时区的ID，如 Europe/Paris

**ZonedDateTime：**

- 一个在ISO-8601日历系统时区的日期时间，
   - 如 2007-12-03T10:15:30+01:00 Europe/Paris。
-  其中每个时区都对应着ID，地区ID都为“{区域}/{城市}”的格式，
    - 例如：Asia/Shanghai等

**Clock：**

- 使用时区提供对当前即时、日期和时间的访问的时钟。
-  持续时间：Duration，用于计算两个“时间”间隔
- 日期间隔：Period，用于计算两个“日期”间隔

**TemporalAdjuster**
 
- 时间校正器。有时我们可能需要获取例如：将日期调整到“下一个工作日”等操作。

**TemporalAdjusters :** 

- 该类通过静态方法(`firstDayOfXxx()/lastDayOfXxx()/nextXxx()`)提供了大量的常用TemporalAdjuster 的实现。

## Java比较器

Java中的对象，正常情况下，只能进行比较 == 或  ！= 不能使用>或<，但是在开发场景中，需要对多个对象进行排序时。

### 自然排序 java.lang.Comparable接口

- String和包装类等实现了Comparable接口，重写了compareTo()方法，给出了比较两个对象大小的方法
- String/包装类等重写了compareTo()之后，进行了从大到小的排序

**自然排序**

- 对于自定义类，如果需要排序，可以让自定义类实现Comparable接口，重写comparaTo()方法。在compareTo()中指明如何排序

**重写compareTo()的规则:**

- 如果当前对象this大于形参对象obj，则返回正整数，
- 如果当前对象this小于形参对象obj，则返回负整数，
- 如果当前对象this等于形参对象obj，则返回零。

```sql
public class CompareTest {

    @Test
    public void test1(){
        String[] arr = new String[]{"AA","CC","MM","GG","JJ","DD","KK"};

        Arrays.sort(arr);
        //实现了Comparable接口
        System.out.println(Arrays.toString(arr));
        //[AA, CC, DD, GG, JJ, KK, MM]
    }

    @Test
    public void test2(){
        Goods[] arr = new Goods[4];
        arr[0] = new Goods("联想",199);
        arr[1] = new Goods("华硕",100);
        arr[2] = new Goods("星际",100);
        arr[3] = new Goods("盛大",2321);

        Arrays.sort(arr);
        System.out.println(Arrays.toString(arr));
        //[Goods{name='星际', price=99.0}, Goods{name='华硕', price=100.0}, Goods{name='联想', price=199.0}, Goods{name='盛大', price=2321.0}]
    }
}
```

```java
public class Goods implements Comparable {
    private String name;
    private double price;

    public Goods() {
    }

    public Goods(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Goods{" +
                "name='" + name + '\'' +
                ", price=" + price +
                '}';
    }

    //指明Goods的排序，按价格从低到高排序，再按照产品名称从高到低排序
    @Override
    public int compareTo(Object o) {
        if (o instanceof Goods) {
            Goods goods = (Goods) o;
            //方式一
            if (this.price > goods.price) {
                return 1;
            } else if (this.price < goods.price) {
                return -1;

            } else {
                //return 0;
                //使用String类重写的compareTo()方法排序
                //注意负号 从高到低
                return -this.name.compareTo(goods.name);
            }
            //方式二
            //return  Double.compare(this.price,goods.price);
        }

        throw new RuntimeException("数据类型不一致");
    }
}
```

### 定制排序 java.util.Comparator接口

当元素的类型没有实现java.lang.Comparable接口而又不方便修改代码，或者实现了java.lang.Comparable接口的排序规则不适合当前的操作，那么可以考虑使用 Comparator 的对象来排序，强行对多个对象进行整体排序的比较。

**重写写compare(Object o1,Object o2)方法规则**

- 返回正整数，则表示o1大于o2；
- 返回0，表示相等；
- 返回负整数，表示o1小于o2。

```java
@Test
public void test3() {
    Goods[] arr = new Goods[5];
    arr[0] = new Goods("联想", 199);
    arr[1] = new Goods("华硕", 100);
    arr[2] = new Goods("星际", 100);
    arr[3] = new Goods("盛大", 2321);
    arr[4] = new Goods("盛大", 221);

    Arrays.sort(arr, new Comparator() {
        //指明Goods的排序，按产品名称从低到高排序，再按照价格从高到低排序
        @Override
        public int compare(Object o1, Object o2) {
            if (o1 instanceof Goods && o2 instanceof Goods) {
                Goods g1 = (Goods)o1;
                Goods g2 = (Goods)o2;

                if(g1.getName().equals(g2.getName())){
                    return -Double.compare(g1.getPrice(),g2.getPrice());
                }else{
                    return g1.getName().compareTo(g2.getName());
                }
            }
            throw new RuntimeException("数据类型不一致");
        }
    });
    System.out.println(Arrays.toString(arr));
    //[Goods{name='星际', price=99.0}, Goods{name='华硕', price=100.0}, Goods{name='联想', price=199.0}, Goods{name='盛大', price=2321.0}]

}

@Test
public void test4() {
    String[] arr = new String[]{"AA", "CC", "MM", "GG", "JJ", "DD", "KK"};
    Arrays.sort(arr, new Comparator() {
        @Override
        public int compare(Object o1, Object o2) {
            if(o1 instanceof String && o2 instanceof String){
                String s1 = (String) o1;
                String s2 = (String) o2;
                
                return -s1.compareTo(s2);
            }
            
            throw new RuntimeException("数据类型不一致");
        }
    });
    System.out.println(Arrays.toString(arr));
    //[MM, KK, JJ, GG, DD, CC, AA]
}
```

### 对比

- Comparable接口的方式一旦确定`compareTo()`，保证Comparable接口实现类的对象在任何位置都可以比较大小
- Comparator接口属于临时性的比较

## System类

System类代表系统，系统级的很多属性和控制方法都放置在该类的内部。该类位于java.lang包。

- 由于该类的构造器是private的，所以无法创建该类的对象，也就是无法实例化该类。
- 其内部的成员变量和成员方法都是static的，所以也可以很方便的进行调用。

**成员变量**

- System类内部包含in、out和err三个成员变量，分别代表标准输入流(键盘输入)，标准输出流(显示器)和标准错误输出流(显示器)。

**成员方法**

- `native long currentTimeMillis()`：
   - 该方法的作用是返回当前的计算机时间，时间的表达格式为当前计算机时间和GMT时间(格林威治时间)1970年1月1号0时0分0秒所差的毫秒数。
- `void exit(int status)`：
   - 该方法的作用是退出程序。
   - 其中status的值为0代表正常退出，非零代表异常退出。
   - 使用该方法可以在图形界面编程中实现程序的退出功能等。
- `void gc()`:
   - 该方法的作用是请求系统进行垃圾回收。
   - 至于系统是否立刻回收，则取决于系统中垃圾回收算法的实现以及系统执行时的情况。
- `String getProperty(String key)`：
   - 该方法的作用是获得系统中属性名为key的属性对应的值。
   - 系统中常见的属性名以及属性的作用如下表所示：
![](C:/NoteBook/pictures/89133321239495.png =422x)


```java
@Test
public void test1() {
    String javaVersion = System.getProperty("java.version");
    System.out.println("java的version:" + javaVersion);

    String javaHome = System.getProperty("java.home");
    System.out.println("java的home:" + javaHome);

    String osName = System.getProperty("os.name");
    System.out.println("os的name:" + osName);

    String osVersion = System.getProperty("os.version");
    System.out.println("os的version:" + osVersion);

    String userName = System.getProperty("user.name");
    System.out.println("user的name:" + userName);

    String userHome = System.getProperty("user.home");
    System.out.println("user的home:" + userHome);

    String userDir = System.getProperty("user.dir");
    System.out.println("user的dir:" + userDir);

}
```

## Math类

java.lang.Math提供了一系列静态方法用于科学计算。其方法的参数和返回值类型一般为double型。

- abs 
   - 绝对值
- acos,asin,atan,cos,sin,tan 
   - 三角函数
- sqrt   
   - 平方根
- pow(double a,doble b) 
   - a的b次幂
- log 
   - 自然对数
- exp
   -  e为底指数
- max(double a,double b)
- min(double a,double b)
- random() 
   - 返回0.0到1.0的随机数
- long round(double a) 
   - double型数据a转换为long型（四舍五入）
- toDegrees(double angrad) 
   - 弧度—>角度
- toRadians(double angdeg) 
  - 角度—>弧度

## BigInteger与BigDecimal

### BigInteger类

- Integer类作为int的包装类，能存储的最大整型值为2^31-1，Long类也是有限的，最大为2^63-1。
     - 如果要表示再大的整数，不管是基本数据类型还是他们的包装类都无能为力，更不用说进行运算了。
- java.math包的BigInteger可以表示不可变的任意精度的整数。
- BigInteger 提供所有 Java 的基本整数操作符的对应物，并提供 java.lang.Math 的所有相关方法。
- 另外，BigInteger 还提供以下运算：模算术、GCD 计算、质数测试、素数生成、位操作以及一些其他操作。

**构造器**

- `BigInteger(String val)`：
   - 根据字符串构建BigInteger对象

**方法**

- public BigInteger abs()：
   - 返回此 BigInteger 的绝对值的 BigInteger。
- BigInteger add(BigInteger val) ：
   - 返回其值为 (this + val) 的 BigInteger
- BigInteger subtract(BigInteger val) ：
   - 返回其值为 (this - val) 的 BigInteger
- BigInteger multiply(BigInteger val) ：
   - 返回其值为 (this * val) 的 BigInteger
- BigInteger divide(BigInteger val) ：  
   - 返回其值为 (this / val) 的 BigInteger。整数相除只保留整数部分。
- BigInteger remainder(BigInteger val) ：
   - 返回其值为 (this % val) 的 BigInteger。
- BigInteger[] divideAndRemainder(BigInteger val)：
   - 返回包含 (this / val) 后跟(this % val) 的两个 BigInteger 的数组。
- BigInteger pow(int exponent) ：
   - 返回其值为 (thisexponent) 的 BigInteger。

### BigDecimal类

- 一般的Float类和Double类可以用来做科学计算或工程计算，但在商业计算中，要求数字精度比较高，故用到java.math.BigDecimal类。
- BigDecimal类支持不可变的、任意精度的有符号十进制定点数。

**构造器**

- `public BigDecimal(double val)`
- `public BigDecimal(String val)`

**方法**

- public BigDecimal add(BigDecimal augend)
- public BigDecimal subtract(BigDecimal subtrahend)
- public BigDecimal multiply(BigDecimal multiplicand)
- public BigDecimal divide(BigDecimal divisor, int scale, int roundingMode)

```java
@Test
public void test2() {
    BigInteger bi = new BigInteger("2313123232323312223123122222222");
    BigDecimal bd = new BigDecimal("1213123.12312312312");
    BigDecimal bd2 = new BigDecimal("23");
    System.out.println(bd.divide(bd2, BigDecimal.ROUND_HALF_DOWN));
    //52744.48361404883
    System.out.println(bd.divide(bd2, 15, BigDecimal.ROUND_HALF_UP));
    //52744.483614048831304
}
```

# 枚举类

- 类的对象只有有限个，是确定的.
- 当需要定义一组常量时，强烈建议使用枚举类

**枚举类的实现**

- JDK1.5之前需要自定义枚举类
- JDK 1.5 新增的 enum 关键字用于定义枚举类
- 若枚举只有一个对象, 则可以作为一种单例模式的实现方式。

**枚举类的属性**

- 枚举类对象的属性不应允许被改动, 所以应该使用 private final 修饰
- 枚举类的使用 private final 修饰的属性应该在构造器中为其赋值
- 若枚举类显式的定义了带参数的构造器, 则在列出枚举值时也必须对应的传入参数

## 自定义枚举类

1. 私有化类的构造器，保证不能在类的外部创建其对象
2. 在类的内部创建枚举类的实例。声明为：public static final 
3. 对象如果有实例变量，应该声明为private final，并在构造器中初始化

```java
public class SeasonTest {
    public static void main(String[] args) {
        Season season = Season.SPRING;
        System.out.println(season);
    }
}

class Season {
    //    1.声明Season对象的属性
    private final String seasonName;
    private final String seasonDesc;

    //    2.私有化类的构造器,并为对象属性赋值
    private Season(String seasonName, String seasonDesc) {
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

//    3.提供当前枚举类的多个对象 public static final
    public static final Season SPRING = new Season("春天","万物复苏");
    public static final Season SUMMER = new Season("夏天","赤日炎炎");
    public static final Season AUTUMN = new Season("秋天","硕果累累");
    public static final Season WINTER = new Season("冬天","天寒地冻");

//    其他诉求：getter获取枚举类对象的属性
    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }

    //其他诉求：toString()

    @Override
    public String toString() {
        return "Season{" +
                "seasonName='" + seasonName + '\'' +
                ", seasonDesc='" + seasonDesc + '\'' +
                '}';
    }
}
```

## enum定义枚举类

**使用说明**

- 使用 `enum关键字`定义的枚举类<mark>默认继承了 java.lang.Enum类 ，因此不能再继承其他类</mark>
- 枚举类的构造器只能使用 private 权限修饰符
- 枚举类的所有实例必须在枚举类中显式列出
   - 多个对象之间用 逗号, 隔开。而不是分号 ; 
   - 最后一个对象以分号 ; 结束
   - 列出的实例系统会自动添加 public static final 修饰
- 必须在枚举类的第一行声明枚举类对象(枚举类的对象要移到最前面。)

JDK 1.5 中可以在 switch 表达式中使用Enum定义的枚举类的对象作为表达式, case 子句可以直接使用枚举值的名字, 无需添加枚举类作为限定。

```java
public class SeasonTest {
    public static void main(String[] args) {
        Season season = Season.SPRING;
        System.out.println(season);

        System.out.println(Season.class.getSuperclass());
        //class java.lang.Enum 定义的枚举类默认继承Enum类
    }
}

enum Season{

//    1.提供当前枚举类的对象,
//    多个对象之间用 逗号, 隔开。而不是分号 ; 最后一个对象以分号 ; 结束
//    枚举类的对象要移到最前面
    SPRING("春天","万物复苏"),
    SUMMER("夏天","赤日炎炎"),
    AUTUMN("秋天","硕果累累"),
    WINTER("冬天","天寒地冻");

//    声明Season对象的属性
    private final String seasonName;
    private final String seasonDesc;

//    私有化类的构造器,并为对象属性赋值
    private Season(String seasonName, String seasonDesc) {
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    //    其他诉求：getter获取枚举类对象的属性
    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }

    //其他诉求：toString() Enum类中已经重写
    //默认打印当前对象的名字
    @Override
    public String toString() {
        return "Season{" +
                "seasonName='" + seasonName + '\'' +
                ", seasonDesc='" + seasonDesc + '\'' +
                '}';
    }
}
```

## Enum类的主要方法

-  `values()`方法：
   - 返回枚举类型的对象数组。该方法可以很方便地遍历所有的枚举值。
-  `valueOf(String str)`：
   - 可以把一个字符串转为对应的枚举类对象。
   - 要求字符串必须是枚举类对象的“名字”。如不是，会有运行时异常：IllegalArgumentException。
- `toString()`：
   - 默认返回当前枚举类对象常量的名称

```java
public class SeasonTest {
    public static void main(String[] args) {
        Season season = Season.SPRING;
//        toString()
        System.out.println(season);

        System.out.println(Season.class.getSuperclass());
        //class java.lang.Enum 定义的枚举类默认继承Enum类

//        values()  返回对象数组
        System.out.println(Season.values());
        //[Lcom.zjk.java2.Season;@52cc8049
//        Season[] values = Season.values();
        for(int i = 0 ; i<Season.values().length;i++){
            System.out.println(Season.values()[i]);
        }

        Thread.State[] values = Thread.State.values();
        for(int i = 0;i<values.length;i++){
            System.out.println(values[i]);
        }

//        valueOf(String objName) 根据提供的objName，返回枚举类中对象名是objName的对象。
        Season winter = Season.valueOf("WINTER");
        System.out.println(winter);
//        如果没有符合objName的对象，则异常
//        Season winter1 = Season.valueOf("WINTER1");
        //java.lang.IllegalArgumentException
    }
}
```

## 实现接口的枚举类

和普通 Java 类一样，枚举类可以实现一个或多个接口

- 情况一：实现接口，在枚举类中实现抽象方法
   - 若每个枚举值在调用实现的接口方法呈现相同的行为方式，则只要统一实现该方法即可。
-  情况二：枚举类的对象分别实现接口中的方法
   - 若需要每个枚举值在调用实现的接口方法呈现出不同的行为方式, 则可以让每个枚举值分别来实现该方法
   - 哪怕只有一个枚举类对象实现也可实现该接口，不会报异常
   

```java
public class SeasonTest {
    public static void main(String[] args) {
        Season season = Season.SPRING;
        season.show();
    }
}

interface Info{
    void show();
}

enum Season implements Info{

//    情况二：枚举类的对象分别实现接口中的方法
    SPRING("春天","万物复苏"){
//        @Override
//        public void show() {
//            System.out.println("春天");
//        }
    },
    SUMMER("夏天","赤日炎炎"){
        @Override
        public void show() {
            System.out.println("夏天");
        }
    },
    AUTUMN("秋天","硕果累累"){
        @Override
        public void show() {
            System.out.println("秋天");
        }
    },
    WINTER("冬天","天寒地冻"){
        @Override
        public void show() {
            System.out.println("冬天");
        }
    };
    private final String seasonName;
    private final String seasonDesc;

    private Season(String seasonName, String seasonDesc) {
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }

    @Override
    public String toString() {
        return "Season{" +
                "seasonName='" + seasonName + '\'' +
                ", seasonDesc='" + seasonDesc + '\'' +
                '}';
    }

//    情况一：实现接口，在枚举类中实现抽象方法
    @Override
    public void show() {
        System.out.println("季节");
    }
}
```

### 练习：枚举类实现内部抽象方法

- 枚举类可以在内部定义抽象方法，但是他的所有实例都要实现该抽象方法。

```java
/**
 * 1)有 RED,BLUE,BLACK,YELLOW,GREEN这个五个枚举值；
 * 2)Color有三个属性redValue，greenValue，blueValue，
 * 3)创建构造方法，参数包括这三个属性，
 * 4)每个枚举值都要给这三个属性赋值，三个属性对应的值分别是red：255,0,0  	blue:0,0,255  black:0,0,0  yellow:255,255,0  green:0,255,0
 * 5)重写toString方法显示三属性的值
 * 6)在Color中添加抽象方法meaning，不同的枚举类的meaning代表的意思各不相同
 */

enum Color {
    RED(255, 0, 0){
        String meaning(){
          return "Red";
        };
    },
    BLUE(0, 0, 255){
        String meaning(){
            return "blue";
        }
    },
    BLACK(0, 0, 0){
        @Override
        String meaning() {
            return "black";
        }
    },
    YELLOW(255, 255, 0){
        @Override
        String meaning() {
            return "yellow";
        }
    },
    GREEN(0, 255, 0){
        @Override
        String meaning() {
            return "green";
        }
    };
    private final int redValue;
    private final int greenValue;
    private final int blueValue;

    Color(int redValue, int greenValue, int blueValue) {
        this.redValue = redValue;
        this.greenValue = greenValue;
        this.blueValue = blueValue;
    }

    @MyAnnotation2(value = "hi")
    @Override
    public String toString() {
        return "Color{" +
                "redValue=" + redValue +
                ", greenValue=" + greenValue +
                ", blueValue=" + blueValue +
                '}';
    }
    
    abstract String meaning();
}
```

# 注解(Annotation)

## 注解 (Annotation) 概述

- 从 JDK 5.0 开始, Java 增加了对元数据(MetaData) 的支持, 也就是Annotation(注解)
- Annotation 其实就是代码里的特殊标记, 这些标记可以在编译, 类加载, 运行时被读取, 并执行相应的处理。通过使用 Annotation, 程序员可以在不改变原有逻辑的情况下, 在源文件中嵌入一些补充信息。代码分析工具、开发工具和部署工具可以通过这些补充信息进行验证或者进行部署。
- Annotation 可以像修饰符一样被使用, 可用于修饰包,类, 构造器, 方法, 成员变量, 参数, 局部变量的声明, 这些信息被保存在 Annotation 的 “name=value” 对中。
- 在JavaSE中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。在JavaEE/Android中注解占据了更重要的角色，例如用来配置应用程序的任何切面，代替JavaEE旧版中所遗留的繁冗代码和XML配置等。
- 未来的开发模式都是基于注解的，JPA是基于注解的，Spring2.5以上都是基于注解的，Hibernate3.x以后也是基于注解的，现在的Struts2有一部分也是基于注解的了，
   - 注解是一种趋势，一定程度上可以说：**框架 = 注解 + 反射 + 设计模式**

## 常见的Annotation示例

- 使用 Annotation 时要在其前面增加 @ 符号, 并把该 Annotation 当成一个修饰符使用。用于修饰它支持的程序元素

### 示例一：生成文档相关的注解

- `@author` 标明开发该类模块的作者，多个作者之间使用,分割
- `@version` 标明该类模块的版本
- `@see` 参考转向，也就是相关主题
- `@since` 从哪个版本开始增加的
- `@param` 对方法中某参数的说明，如果没有参数就不能写
- `@return` 对方法返回值的说明，如果方法的返回值类型是void就不能写
- `@exception` 对方法可能抛出的异常进行说明 ，如果方法没有用throws显式抛出的异常就不能写
- 其中
     - `@param @return 和 @exception` 这三个标记都是只用于方法的。
     - `@param`的格式要求：`@param 形参名 形参类型 形参说明`
     - `@return` 的格式要求：`@return 返回值类型 返回值说明`
     - `@exception`的格式要求：`@exception 异常类型 异常说明`
     - `@param和@exception`可以并列多个

### 示例二：在编译时进行格式检查(JDK内置的三个基本注解)

- `@Override`: 限定重写父类方法, 该注解只能用于方法
- `@Deprecated`: 用于表示所修饰的元素(类, 方法等)已过时。
   - 通常是因为所修饰的结构危险或存在更好的选择
- `@SuppressWarnings`: 抑制编译器警告

```java
@Override  //强制校验是否重写 ；不加也可以也是重写，但是没有校验。
public void walk() {
    System.out.println("学生走路");
}

//---------

@SuppressWarnings("unused")
int num = 10;

@SuppressWarnings({"unused","rawtypes"})
ArrayList list = new ArrayList();
```

### 示例三：跟踪代码依赖性，实现替代配置文件功能

- Servlet3.0提供了注解(annotation),使得不再需要在web.xml文件中进行Servlet的部署。
- spring框架中关于“事务”的管理


## 自定义 Annotation

- 注解声明为 `@interface` 
- 自定义注解自动继承了`java.lang.annotation.Annotation接口`
- Annotation 的成员变量在 Annotation 定义中以<mark>无参数方法</mark>的形式来声明。
    - 其方法名和返回值定义了该成员的名字和类型。我们称为**配置参数**。
    - 类型只能是八种基本数据类型、String类型、Class类型、enum类型、Annotation类型、以上所有类型的数组。
- 指定成员的默认值 ，使用`default 关键字`
- 如果只有一个参数成员，建议使用参数名为value
- 如果定义的注解含有配置参数，那么使用时**必须指定参数值，除非它有默认值**。格式是“参数名 = 参数值”，
    - 如果只有一个参数成员，且名称为value，可以省略“value=”
- 没有成员定义的 Annotation 称为**标记**; 
- 包含成员变量的 Annotation 称为**元数据 Annotation**
- 注意：自定义注解必须配上注解的信息处理流程才有意义。

```java
//1.注解声明为@interface
public @interface MyAnnocation {
    String value() default "hi";
}

@MyAnnocation(value = "hello")
class Person {}
```

## JDK 中的元注解

- 对现有的注解进行解释说明的注解
   - JDK 的元 Annotation 用于修饰其他 Annotation 定义
- JDK5.0提供了4个标准的meta-annotation类型，分别是：
  - Retention
  - Target
  - Documented
  - Inherited
- 自定义注解通常都会指明两个元注解：Retention，Target


**@Retention**: 

- 只能用于修饰一个 Annotation 定义, 用于指定该 Annotation 的生命周期, @Rentention 包含一个 RetentionPolicy 类型的成员变量, 使用@Rentention 时必须为该 value 成员变量指定值:
   - RetentionPolicy.SOURCE:在源文件中有效（即源文件保留），编译器直接丢弃这种策略的注释
   - RetentionPolicy.CLASS:在class文件中有效（即class保留） ， 当运行 Java 程序时, JVM 不会保留注解。 
        - 这是默认值
   - RetentionPolicy.RUNTIME:在运行时有效（即运行时保留），当运行 Java 程序时, JVM 会保留注释。
      - 只有说明为RUNTIME生命周期的注解，才可以通过反射获取该注释。

**@Target**: 

- 用于修饰 Annotation 定义, 用于指定被修饰的 Annotation 能用于修饰哪些程序元素。 @Target 也包含一个名为 value 的成员变量。

**@Documented**: 

- 用于指定被该元 Annotation 修饰的 Annotation 类将被javadoc 工具提取成文档。
   - 默认情况下，javadoc是不包括注解的。
- 定义为@Documented的注解必须设置@Retention值为RUNTIME。
   
**@Inherited**: 

- 被它修饰的 Annotation 将具有继承性。如果某个类使用了被@Inherited 修饰的 Annotation, 则其子类将自动具有该注解。
    - 比如：如果把标有@Inherited注解的自定义的注解标注在类级别上，子类则可以继承父类类级别的注解
    - 实际应用中，使用较少

```java
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})
public @interface MyAnnocation {
    String value() default "hi";
}

@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, MODULE, PARAMETER, TYPE})
public @interface Deprecated {...}
```

### 练 习

1.编写一个Person类，使用Override注解它的toString方法
2.自定义一个名为“MyTiger”的注解类型，它只可以使用在方法上，带一
个String类型的value属性，然后在第1题中的Person类上正确使用。


## 利用反射获取注解信息

- JDK 5.0 在 java.lang.reflect 包下新增了 AnnotatedElement 接口, 该接口代表程序中可以接受注解的程序元素
- 当一个 Annotation 类型被定义为运行时 Annotation 后, 该注解才是运行时可见, 当 class 文件被载入时保存在 class 文件中的 Annotation 才会被虚拟机读取 
  - 要求此注解的元注解@Retention中声明的生命周期状态为: RUNTIME
- 程序可以调用 AnnotatedElement对象的如下方法来访问 Annotation 信息

```java
@Test
public void testGetAnnotation() {
    Class studentClass = Student.class;
    Annotation[] anotations = studentClass.getAnnotations();
    for (int i = 0; i < anotations.length; i++) {
        System.out.println(anotations[i]);
    }
    //@com.zjk.java2.MyAnnocation("hello")
}
```

## JDK8中注解的新特性

Java 8对注解处理提供了两点改进：可重复的注解及可用于类型的注解。此外，反射也得到了加强，在Java8中能够得到方法参数的名称。这会简化标注在方法参数上的注解。

**可重复注解**

1. 注解说明`@Repeatable(存放该重复注解的注解)`
2. 创建新的Annotation注解存放要重复的注解的数组（成员为`注解.class`），(如果存在)同时需要相同的`@Inherited，@Tartget和@Rentention`

```java
@Inherited
@Repeatable(MyAnnotations.class)
@Retention(RetentionPolicy.RUNTIME)
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})
public @interface MyAnnotation {
    String value() default "hi";
}

@Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})
public @interface MyAnnotations {
    MyAnnotation[] value();
}

//重复注解
@MyAnnotation(value = "hello")
@MyAnnotation(value = "abc")
class Person {
  ...
}
```

```java
//重复注解 jdk8.0之前

@Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE})
public @interface MyAnnotation {
    String value() default "hi";
}

public @interface MyAnnotations {
    MyAnnotation[] value();
}

@MyAnnotations({@MyAnnotation(value = "hello"),@MyAnnotation(value = "hi")})
class Person(){
    ...
}
```

**类型注解：**

- JDK1.8之后，关于元注解@Target的参数类型ElementType枚举值多了两个：
    - TYPE_PARAMETER
    - TYPE_USE。
-  在Java 8之前，注解只能是在声明的地方所使用，Java8开始，注解可以应用在任何地方。
    - ElementType.TYPE_PARAMETER 
        - 表示该注解能写在类型变量的声明语句中（如：泛型声明）。 
    - ElementType.TYPE_USE 
        - 表示该注解能写在使用类型的任何语句中。

```java
@Inherited
@Repeatable(MyAnnotations.class)
@Retention(RetentionPolicy.RUNTIME)
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE, MODULE,TYPE_PARAMETER,TYPE_USE})
public @interface MyAnnotation {
    String value() default "hi";
}

class Generic<@MyAnnotation T>{  //TYPE_PARAMETER
    //TYPE_USE
    public void show()throws @MyAnnotation RuntimeException{
        ArrayList<@MyAnnotation String> list = new ArrayList<>();

        int num = (@MyAnnotation int)10L;
    }
}
```

# 集合

**集合框架的概述**

1. 集合和数组都是对多个数据进行存储操作的，简称：Java容器。
   - 此时的存储主要指的是内存层面的存储，不涉及到持久化的存储（硬盘中：.txt/.jpg/.avi/数据库中等)

**与数组的比较**

- 数组在存储多个数据方面的特点
  - 一旦初始化以后，其长度就确定了。
  - 数组一旦定义好，其元素的数据类型也就确定了，就只能操作指定类型的数据，
      - 如 String[] arr ; int[] arr 
      - Object[] arr 
- 数组在存储多个数据方面的缺点
  - 一旦初始化以后，其长度就不可修改了 
  - 数组提供的方法非常有限，对于添加，删除，插入数据等操作，非常不便，同时效率不高。
  - 获取数组中实际元素的个数的需求，数组没有现成的属性或方法可用
  - 数组存储数据的特点：有序，可重复
     - 对于无序，不可重复的需求，数组不可满足。 

## 集合框架

**Java集合的两种体系**

- Collection接口 ： 单列数据，定义了存取一组对象(一个一个的对象)的方法的集合
   - List接口: 元素有序，可重复的集合  -->“动态数组”
      -  ArrayList / LinkedList / Vector
   - Set接口: 元素无序，不可重复的集合 --> 类型于数学中的”集合“ 
      - HashSet / LinkedSet / TreeSet
- Map接口：双列数据，保存具有映射关系"key-value"对(一对一对的数据)的集合 --> 类似于数字中的：”函数  y=f(x)“ ；一个key只能对应一个value，而一个value可以对应多个key
   - HashMap / LinkedHashMap / TreeMap / Hashtable / Properties 

![](C:/NoteBook/pictures/51232021239497.png =696x)
![](C:/NoteBook/pictures/236772121227364.png =662x)

## Collection接口中的方法的使用

**添加**

- add(Object obj)
  - 将元素e添加到集合中
- addAll(Collection coll)
  - 将coll集合中的元素添加到当前的集合中

**获取有效元素的个数**

- int size()

**清空集合**

-  void clear()
   
**是否是空集合**

- boolean isEmpty()
   
**是否包含某个元素**

- boolean contains(Object obj)：
    - 是通过元素的equals方法来判断是否是同一个对象
    - 向Collection接口的实现类中添加数据obj时，要求obj所在类要重写equals()方法，用来判断
- boolean containsAll(Collection c)：
    - 也是调用元素的equals方法来比较的。拿两个集合的元素挨个比较。

**删除**

- boolean remove(Object obj) ：
    - 通过元素的equals方法判断是否是要删除的那个元素。
    - 只会删除找到的第一个元素
- boolean removeAll(Collection coll)：
    - 取当前集合的差集

**取两个集合的交集**

- boolean retainAll(Collection c)：
   - 把交集的结果存在当前集合中，不影响c
   - （交集）从当前集合中返回coll中匹配的元素,并将当前集合改为交集。

**集合是否相等**

-  boolean equals(Object obj)
   - 调用元素内的equals()方法一一进行比较
   - 且如果是ArrayList() 则还要求有序 

**转成对象数组**

- Object[] toArray()
   - 集合-->数组
- 数组-->集合 调用Arrays类静态方法 aslist()

**获取集合对象的哈希值**

- hashCode()
  - 返回当前对象的哈希值
  
**遍历**

- iterator()：
   - 返回迭代器对象，用于集合遍历
   - 返回Iterator接口的实例 用于遍历元素，放在Iterator.Test.java中
   
```java
public class CollectionTest {

    @Test
    public void test1() {
        Collection coll = new ArrayList();
        // add() 添加
        coll.add(123);
        coll.add("Jack");
        coll.add(false);
        coll.add(new String("Tom"));
        coll.add(new Person("Mac", 20));

        Collection coll1 = new ArrayList();
        // addAll(Collection coll) 添加coll集合中的所有元素
        coll1.addAll(coll);
        System.out.println(coll); // [123, Jack, false, Tom, com.zjk.Person@76ed1b7c]
        // 调用了String中的toString()方法
        // Person中为重写toString()方法

        coll1.clear();

        // isEmpty() 判断当前集合是否为空
        coll1.isEmpty();

        // contains(Object obj) 判断当前集合中是否包含obj
        // 在判断时，会调用obj对象所在类的equals()方法
        boolean contains = coll.contains(123);
        System.out.println(contains); // true
        System.out.println(coll.contains(new String("Tom"))); // true
        // 调用String重写的equals()方法
        System.out.println(coll.contains(new Person("Mac", 20))); // false
        // Person类中未重写equals()方法

        // containsAll(Collection coll) 判断形参coll中的所有元素是否都存在于当前集合中
        Collection coll2 = new ArrayList();
        coll2.add(123);

        System.out.println(coll.containsAll(coll2)); //true

        //remove(Object obj) 移除
        coll.remove(123);
        System.out.println(coll.remove(new String("Tom"))); //true
        //调用了equals()方法
        System.out.println(coll.remove(new Person("Mac", 20))); //false
        //Person类中未重写equals()方法，所以无法匹配来删除
        System.out.println(coll); //[Jack, false, com.zjk.java2.Person@75bd9247]

        //removeAll(Collection coll) 从当前集合中移除coll中的所有元素
        coll.removeAll(coll2);
        System.out.println(coll); //[Jack, false, com.zjk.java2.Person@75bd9247]

        coll1.add(new Person("Mac", 20));
        //retainAll(Collection coll) （交集）从当前集合中返回coll中匹配的元素,并将当前集合改为交集。
        //调用equals()方法来匹配
        System.out.println(coll); //[Jack, false, com.zjk.java2.Person@75bd9247]
        System.out.println(coll1); //[com.zjk.java2.Person@7d417077]
        coll.retainAll(coll1);
        System.out.println(coll); //[]

        Collection coll3 = new ArrayList();
        coll3.add(new String("Tom"));
        coll3.add(new Person("Mac", 20));
        Collection coll4 = new ArrayList();
        coll4.add(new String("Tom"));
        coll4.add(new Person("Mac", 20));

        //equals(Object obj)
        System.out.println(coll3.equals(coll4)); //false
        //调用元素内的equals()方法一一进行比较
        //且如果是ArrayList() 则还要求有序

        //hashCode()
        System.out.println(coll.hashCode()); //1

        //toArray() 集合-->数组
        Object[] arr = coll3.toArray();
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }

        //数组-->集合 调用Arrays类静态方法 aslist()
        List<Object> list = Arrays.asList(arr);
        System.out.println(list);//[Tom, com.zjk.java2.Person@7dc36524]

        List<int[]> ints = Arrays.asList(new int[]{123, 456});
        System.out.println(ints);//[[I@35bbe5e8] 只有一个元素

        List ints1 = Arrays.asList(new Integer[]{123, 456});
        System.out.println(ints1);//[123, 456]  包装类

        List ints2 = Arrays.asList(123, 456);
        System.out.println(ints2); //[123, 456]

        //iterator() 返回Iterator接口的实例 用于遍历元素，放在Iterator.Test.java中
    }
}
```

### Iterator迭代器接口

- Iterator对象称为迭代器(设计模式的一种)，主要用于遍历 Collection 集合中的元素。
- GOF给迭代器模式的定义为：提供一种方法访问一个容器(container)对象中各个元素，而又不需暴露该对象的内部细节。
   - 迭代器模式，就是为容器而生。类似于“公交车上的售票员”、“火车上的乘务员”、“空姐”。
- Collection接口继承了`java.lang.Iterable`接口，该接口有一个iterator()方法，
   - 所有实现了Collection接口的集合类都有一个iterator()方法，用以返回一个实现了Iterator接口的对象。
- Iterator 仅用于遍历集合，Iterator 本身并不提供承装对象的能力。
   - 如果需要创建Iterator 对象，则必须有一个被迭代的集合。
- 集合对象每次调用iterator()方法都得到一个全新的迭代器对象，
   - 默认游标都在集合的第一个元素之前。
   
**Iterator方法**

![](C:/NoteBook/pictures/337951223221072.png =676x)

![](C:/NoteBook/pictures/265952123239498.png =493x)

#### 迭代器遍历集合的操作

- hasNext()和next()搭配使用
- 在调用it.next()方法之前必须要调用it.hasNext()进行检测。
   - 若不调用，且下一条记录无效，直接调用it.next()会抛出NoSuchElementException异常。

```java
public class IteratorTest {
    @Test
    public void test1() {
        Collection coll = new ArrayList();
        coll.add(123);
        coll.add("Jack");
        coll.add(false);
        coll.add(new String("Tom"));
        coll.add(new Person("Mac", 20));

        Iterator iterator = coll.iterator();

        //方式1
//        System.out.println(iterator.next()); //123
//        System.out.println(iterator.next()); //Jack
//        System.out.println(iterator.next()); //false
//        System.out.println(iterator.next()); //Tom
//        System.out.println(iterator.next()); //com.zjk.java2.Person@75bd9247
//        System.out.println(iterator.next()); //报异常 NoSuchElementException
        //方式2 不推荐
        for (int i = 0; i < coll.size(); i++) {
            System.out.println(iterator.next());
        }
        //方式3
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }
        //方式4
        coll.forEach(System.out::println);
    }
}
```

**迭代器的执行原理**

- 类似于Oracle的SEQUENCE的 .NEXTVAL 

```java
//提通过Collection接口的iterator()方法创建Iterator对象
Iterator iterator = coll.iterator();
//hasNext():判断是否还有下一个元素
while(iterator.hasNext()){
    //next():①指针下移 ②将下移以后集合位置上的元素返回
    System.out.println(iterator.next());
}
```

```java
//错误1 it.next() 报错NoSuchElementException
Iterator iterator = coll.iterator();
Object obj;
while((obj = iterator.next()) != null){
    System.out.println(obj);
}

//错误2：每次通过coll.iterator()方法创建的Iterator对象都是新的对象
//跳不出循环
while((coll.iterator().next()).hasNext){
    System.out.println(obj);
}
```

#### Interator接口的remove()

- Iterator可以删除集合的元素，但是是遍历过程中通过迭代器对象的remove方法，不是集合对象的remove方法。
- 如果还未调用next()或在上一次调用 next 方法之后已经调用了 remove 方法，再调用remove都会报IllegalStateException。
   - 在没有使用next()切换时，不能连续remove()同一个位置的 

```java
@Test
public void test2(){
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add("Yo");

    Iterator iterator = coll.iterator();
    while(iterator.hasNext()){
        Object obj = iterator.next();
        if("Yo".equals(obj)){
            iterator.remove();
            //iterator.remove(); //报错 在没有使用next()切换时，不能连续remove()同一个位置的 
        }
    }

    //错误；使用同一个Iterator对象，此时游标还在上次调用.next()的位置
    //while(iterator.hasNext()){
    //    System.out.println(iterator.next());
    //}

    Iterator iterator1 = coll.iterator();
    while(iterator1.hasNext()){
        System.out.println(iterator1.next());
    }

}
```

#### 使用 foreach 循环遍历集合元素

- Java 5.0 提供了 foreach 循环迭代访问 Collection和数组。
- 遍历操作不需获取Collection或数组的长度，无需使用索引访问元素。
- 遍历集合的底层调用Iterator完成操作。
- foreach还可以用来遍历数组。

```java
for(要遍历的集合/数组的元素的类型 在循环中使用的元素名称 : 要遍历的集合/数组){
    ...
}
```

```java
@Test
public void test3() {
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add("Yo");

    for (Object obj : coll) {
        System.out.println(obj);
    }
}
```

```java
@Test
public void test4() {
    String[] arr = new String[]{"GG", "GG", "GG"};

    //普通for循环 
    //成功改变
    for (int i = 0; i < arr.length; i++) {
        arr[i] = "MM";
    }

    for (int i = 0; i < arr.length; i++) {
        System.out.println(arr[i]);
    }

    //增强for循环
    //s类似于形参，不会改变原来数组中的String（不可变性)
    for(String s : arr){
        s = "GG";
    }

    for(String s : arr){
        System.out.println(s);
    }
}
```

### List接口

- 鉴于Java中数组用来存储数据的局限性，我们通常使用List替代数组
- List集合类中元素有序、且可重复，集合中的每个元素都有其对应的顺序索引。
- List容器中的元素都对应一个整数型的序号记载其在容器中的位置，可以根据序号存取容器中的元素。
- JDK API中List接口的实现类常用的有：ArrayList / LinkedList / Vector

**ArrayList / LinkedList / Vector三者的异同**

- 同：
  - 三个类都实现了List接口。存储数据的特点相同：存储有序的，可重复的数据。
- 不同：
   - ArrayList 
      - 作为List接口的主要实现类
      - 线程不安全，效率高
      - 底层使用Object[] elementData存储
   - LinkedList 
      - 对于频繁的插入和删除操作，使用此类比ArrayList效率高 
      - 底层使用双向链表存储 
   - Vector 
      - 作为List接口的古老实现类
      - 线程安全，效率低
      - 底层使用Object[] elementData存储

#### 源码分析

##### ArrayList类

- ArrayList 是 List 接口的典型实现类、主要实现类
   - 本质上，ArrayList是对象引用的一个”变长”数组
- JDK1.7：ArrayList像饿汉式，直接创建一个初始容量为10的数组
   - Arrays.asList(…) 方法返回的 List 集合，既不是 ArrayList 实例，也不是Vector 实例。 
   - Arrays.asList(…) 返回值是一个固定长度的 List 集合

**源码分析 jdk 1.7**

- ArrayList像饿汉式，直接创建一个初始容量为10的数
   - `ArrayList list = new ArrayList();` //底层创建了长度是10的Object[] 数组elementData
   - `list.add(123); //elementData[0] = new Integer(123);`
   - 如果此次的添加导致底层elementData数组容量不够，则扩容。
   - 默认情况下，扩容为原来的容量的1.5倍，同时需要将原来数组中的数据复制到新的数组中
- 建议开发中使用带参的构造器: `ArrayList list = new ArrayList(int capacity);` 指定底层创建Object[] elementData数组的长度
   - 直接指定较大的容量，可以避免扩容 

**源码分析 jdk 1.8**

- ArrayList像懒汉式，一开始创建一个长度为0的数组，当添加第一个元素时再创建一个始容量为10的数组
   - `ArrayList list = new ArrayList(); ` //底层Object[] elementData初始化为{},并没有创建长度
   - `list.add(123)` //从第一调用add()时，底层才创建了长度为10的数组，并将数据123 添加到elementData数组中
- 其他的添加和扩容和jdk1.7相同
   
##### LinkedList类

**源码分析**

- 对于频繁的插入或删除元素的操作，建议使用LinkedList类，效率较高
- LinkedList：双向链表，内部没有声明数组，
   - 而是定义了`Node类型的first和last`，用于记录首末元素。
      - `LinkedList list = new LinkedList(); `//内部声明了Node类型的first和last属性，默认值为null
   - 同时，定义`内部类Node`，作为LinkedList中保存数据的基本结构。
      - `list.add(123);` //将123封装带Node中，创建了Node对象。
   - Node除了保存数据，还定义了两个变量：
      - `prev变量`记录前一个元素的位置
      - `next变量`记录下一个元素的位置
      
```java
//Node定义为：体现了LinkedList的双向链表
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

##### Vector类

**源码分析**

- jdk1.7和jdk1.8中通过Vector()构造器创建对象时，底层都创建了长度为10的数组.
- 扩容默认2倍

#### List接口方法

- `void add(int index, Object ele)`:在index位置插入ele元素
- `boolean addAll(int index, Collection eles)`:从index位置开始将eles中的所有元素添加进来
- `Object get(int index)`:获取指定index位置的元素
- `int indexOf(Object obj)`:返回obj在集合中首次出现的位置，没有则返回-1
- `int lastIndexOf(Object obj)`:返回obj在当前集合中末次出现的位置
- `Object remove(int index)`:移除指定index位置的元素，并返回此元素
- `Object set(int index, Object ele)`:设置指定index位置的元素为ele，并返回此(原先的)元素
- `List subList(int fromIndex, int toIndex)`:返回从fromIndex到toIndex(不包括toIndex)位置的子集合

**常用方法**

- 增 add(int index, Object obj)
- 插 add(int index Object obj)
- 删 remove(int index) /remove(Object obj) Collection接口内的
- 改 set(int index, Object obj)
- 查 get(int index)
- 长度 size()
- 遍历 
   1. Iterator迭代器
   2. foreach
   3. for
 

```java
package com.zjk.java2;

import org.junit.Test;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.Iterator;
import java.util.List;

public class ListTest {
    @Test
    public void test1() {
        ArrayList list = new ArrayList();
        list.add(123);
        list.add(456);
        list.add("Tom");
        list.add(new Person("Mac", 20));

        System.out.println(list); //[123, 456, Tom, com.zjk.java2.Person@75bd9247]

        //void add(int index Object ele) 在index位置插入ele元素
        list.add(1, "BB");
        System.out.println(list); //[123, BB, 456, Tom, com.zjk.java2.Person@75bd9247]

        //boolean addAll(int index,Collection eles) 从index位置开始将eles中的所有元素依序添加到当前的list
        System.out.println(list.size()); //5
        List list1 = Arrays.asList(1, 2, 3);
        list.addAll(list1);
        System.out.println(list.size()); //8

        //Object get(int index) 从index位置获取元素
        System.out.println(list.get(2)); //456

        //int indexOf(Object obj) 返回obj在集合中首次出现的位置, 没有则返回-1
        int index = list.indexOf(456);
        System.out.println(index); //2

        //int lastIndexOf(Object obj) 返回obj在集合中最后出现的位置，没有则返回-1
        int lastIndex = list.lastIndexOf(456);
        System.out.println(lastIndex); //2

        //Object remove(int index) 移除指定index位置的元素，并返回此元素
        Object obj = list.remove(2);
        System.out.println(obj); //456

        //Object set(int index,Object obj) 设置指定index位置的元素为ele,并返回此(原先的)元素
        System.out.println(list.set(2, "Test"));

        //List subList(int fromIndex, int toIndex) 返回从fromIndex到toIndex(不包括toIndex)位置的子集合
        List list2 = list.subList(1, 3);
        System.out.println(list2); //[BB, Test]
    }

    @Test
    public void test2() {
        ArrayList list = new ArrayList();
        list.add(123);
        list.add(456);
        list.add("Tom");

        //方式1 Iterator迭代器
        Iterator iterator = list.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }

        //方式2 foreach
        for (Object obj : list) {
            System.out.println(obj);
        }

        //方式3 for
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
       
        //方式4 forEach()
        list.forEach(System.out::println);
    }
}
``` 

##### Arrays.asList() 的注意

- Arrays.asList() 返回的不是java.util下的ArrayList，而是Arrays的内部类，继承AbstractList类（List），但没有重写add()方法
- 因此，通过Arrays.asList()方法构造的List集合，尽量不要用add()等方法修改。

```java
public class ListTest {
    @Test
    public void test1(){
        List<Integer> list = Arrays.asList(1,2,3);
    //     list.add(new Integer(4));
        //抛出异常：Arrays.asList() 
        // 返回的不是java.util下的ArrayList，
        // 而是Arrays的内部类，继承AbstractList类（List），但没有重写add()方法
    //     System.out.println(list.contains(new Integer(4)));
    }
}
```

##### ArrayList的sublist() 的注意

1. 修改原集合元素的值，会影响子集合
2. 修改原集合的结构，会引起异常：ConcurrentModificationException
3. 修改子集合元素的值，会影响原集合
4. 修子集合的结构，会影响原集合

- subList()返回的是ArrayList的内部类SubList（是ArraysList的一个视图），而不是ArrayList。对于SubList子列表的所有操作最终会反映到原列表上。
- 在subList()中，对父集合元素的增加或删除，均会导致子列表的遍历、增加、删除产生ConcurrentModificationException异常。
    - 对子集合调用的remove()，此时删除的是对应于该子集合的下标位置的元素。且只能是子集合中包含的元素。

```java
@Test
public void test2() {
    List<String> list = new ArrayList<String>();
    list.add("一号");
    list.add("二号");
    list.add("三号");

    //subList() 获取子列表
    List<String> subList = list.subList(1, 3); //["二号","三号"]

    //对原集合元素的修改
    list.set(2, "三号-修改");
    //对原集合的修改影响子集合
    System.out.println(subList); //[二号, 三号-修改]

    //修改原集合的结构
    //list.add("四号");
    //在原集合结构修改后，子集合的遍历、增加、删除出现异常.
    //遍历子集合时，出现异常java.util.ConcurrentModificationException
    //System.out.println(subList);
    //subList.add("五号");
    //subList.remove(3);

    //子集合元素的修改、结构的改变（增加、删除），影响父集合。
    subList.set(1, "二号-sub修改");
    System.out.println(list); //[一号, 二号, 二号-sub修改]
    subList.add("五号");
    System.out.println(list); //[一号, 二号, 二号-sub修改, 五号]
    //此时删除的是对应于该子集合的下标位置的元素。
    subList.remove(1); 
    System.out.println(list); //[一号, 二号, 五号]
}
```

##### remove() ， Collection接口和ArrayList接口

```java
@Test
public void testListRemove() {
    List list = new ArrayList();
    list.add(1);
    list.add(2);
    list.add(3);
    updateList(list);
    System.out.println(list);//[1,2]
}

private static void updateList(List list) {
    list.remove(2);  //视为索引2 而不是数据2
    //list.remove(new Integer(2)); //删除数据2
}
```

```java
//关于添加的数据被修改
public class SetTest {
    @Test
    public void test1() {
        HashSet hashSet = new HashSet();

        Person p1 = new Person("Mac", 21);
        Person p2 = new Person("Tom", 21);
        hashSet.add(p1);
        hashSet.add(p2);

        p1.setName("AA");
        //对应集合中的元素也改变,但是此时存放的位置还是按照原先p1("Mac",21)计算的哈希值存放的。
        hashSet.forEach(System.out::println);
        //Person{name='AA', age=21}
        //Person{name='Tom', age=21}

        hashSet.remove(p1);
        //此时remove()寻找的哈希值，是修改后的p1("AA",21)的哈希值，无法和集合中p1的哈希值匹配，
        // 故找不到，无法成功remove()
        hashSet.forEach(System.out::println);
        //Person{name='AA', age=21}
        //Person{name='Tom', age=21}

        Person p3 = new Person("AA",21);
        hashSet.add(p3);
        //同理，此时在集合中，存放("AA",21)计算的哈希值的位置实际上是空的，所以可以添加
        hashSet.forEach(System.out::println);
        //Person{name='AA', age=21}
        //Person{name='Tom', age=21}
        //Person{name='AA', age=21}
    }
}
```

![](C:/NoteBook/pictures/227085513239570.png =696x)

### Set接口

- 存储无序，不可重复的数据
- Set接口是Collection的子接口，Set接口没有提供额外的方法，使用的都是Collect接口中的方法
- Set 集合不允许包含相同的元素，如果试把两个相同的元素加入同一个Set 集合中，则添加操作失败。
- Set 判断两个对象是否相同不是使用 == 运算符，而是根据 equals() 方法

**实现类**

- `HashSet `
   - 作为Set接口的主要实现类，线程不安全的，可以存储null值
   - `LinkedHashSet` 作为HashSet的子类；遍历其内部数据时，可以安装添加的顺序遍历
- `TreeSet`
   - 可以按照添加对象的指定属性，进行排序。

**Set存储无序，不可重复的数据**

以HashSet为例：

- 无序性
  - 不等于随机性  遍历的顺序仍然是添加的顺序
  - 存储的数组在底层数组并非按照数组索引的顺序添加。
  - 而是根据数据的Hash值决定的。
- 不可重复性
  - 保证添加的元素按照equals()方法判断时，返回的不是true。相同的元素只能添加一次。

**添加元素的过程**

以HashSet为例

- 像hashSet中添加元素a，首先调用元素a所在类的hashCode()方法，计算元素a的哈希值，此哈希值接着通过某种算法计算出在hashSet底层数组中的存放位置（即为：索引位置），判断数组此位置上是否已经有元素。
- 如果此位置上没有其他元素，则元素a添加成功 --情况1
- 如果此位置上有其他元素b（或以链表形式存在的多个元素），则比较元素a于元素b的hash值
   - 如果hash值不相同，则元素a添加成功 --情况2
   - 如果hash值相同，进而需要调用元素a所在类的equals()方法
      - equals()返回true，元素a添加失败
      - equals()返回false，则元素a添加成功  --情况3
- 对于添加成功的情况2和情况3而言，元素a，与已经存在指定索引位置上数据以链表的方式存储。
- HashSet底层：数组+链表
  - jdk 1.7 ：元素a放到数组中，指向原来的元素
  - jdk 1.8 ：原来的元素在数组中，指向元素a
  ![](C:/NoteBook/pictures/539705218221142.png =661x)
  ![](C:/NoteBook/pictures/141841619239568.png =669x)
  


```java
public class SetTest {
    @Test
    public void test1(){
        Set set = new HashSet();
        set.add(456);
        set.add(123);
        set.add("AA");
        set.add("BB");
        set.add(new Person("Mac",21));
        set.add(new String("CC"));
        System.out.println(set); //[AA, BB, CC, 456, com.zjk.java2.Person@75bd9247, 123]

        //不可重复性
        set.add(123);
        System.out.println(set); //[AA, BB, CC, 456, com.zjk.java2.Person@75bd9247, 123]
        //调用equals()方法比较,需要用到hashCode()方法，Integer 123发现相同，所以不添加
        set.add(new Person("Mac",21));
        System.out.println(set);//[AA, BB, CC, com.zjk.java2.Person@7d417077, 456, com.zjk.java2.Person@75bd9247, 123]
        //Person类中未重写hashCode()方法和equals()方法,比较的还是地址，所以认为不一样

        Iterator iterator = set.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        } //遍历的顺序仍然是添加的顺序
    }
}
```

#### 重写hashCode()

以Eclipse/IDEA为例，在自定义类中可以调用工具自动重写equals()和hashCode()。

**要求:**

- 向Set中添加的数据，其所在类一定要重写hashCode()和equals()
- 重写的hashCode()方法和equals()方法尽可能保持一致性。
  - 相等的对象必须具有相等的散列码 

**基本原则**

- 在程序运行时，同一个对象多次调用 hashCode() 方法应该返回相同的值。
- 当两个对象的 equals() 方法比较返回 true 时，这两个对象的 hashCode() 方法的返回值也应相等。
- 对象中用作 equals() 方法比较的 Field，都应该用来计算 hashCode 值。

**问题：为什么用Eclipse/IDEA复写hashCode方法，有31这个数字？**

- 选择系数的时候要选择尽量大的系数。因为如果计算出来的hash地址越大，所谓的“冲突”就越少，查找起来效率也会提高。（减少冲突）
-  并且31只占用5bits,相乘造成数据溢出的概率较小。
- 31可以 由`i*31== (i<<5)-1`来表示,现在很多虚拟机里面都有做相关优化。（提高算法效率）
-  31是一个素数，素数作用就是如果我用一个数字来乘以这个素数，那么最终出来的结果只能被素数本身和被乘数还有1来整除！(减少冲突)

#### HashSet类

- HashSet 是 Set 接口的典型实现，大多数时候使用 Set 集合时都使用这个实现类。
- HashSet 按 Hash 算法来存储集合中的元素，因此具有很好的存取、查找、删除性能。

**HashSet 具有以下特点：**

- 不能保证元素的排列顺序
- HashSet 不是线程安全的
- 集合元素可以是 null
- HashSet 集合判断两个元素相等的标准：两个对象通过 hashCode() 方法比较相等，并且两个对象的 equals() 方法返回值也相等。
- 对于存放在Set容器中的对象，对应的类一定要重写equals()和hashCode(Object obj)方法，以实现对象相等规则。
   - 即：“相等的对象必须具有相等的散列码”。

#### LinkedHashSet类

- LinkedHashSet作为HashSet的子类，在添加数据的同时，每个数据还维护了两个引用，记录此数据前一个数据和后一个数据。
- 对于频繁的遍历操作，LinkedHashSet效率高于HashSet

![](C:/NoteBook/pictures/208262222221142.png =609x)

#### TreeSet类

- 向TreeSet中添加的数据，要求是相同类的对象

![](C:/NoteBook/pictures/35032923239568.png =563x)

**两种排序方式：自然排序 和 定制排序**

- 自然排序(实现Comparable接口)中，比较两个对象是否相同的标准为：compareTo()返回0，不再是equals()
  - 默认情况下，TreeSet 采用自然排序。
- 定制排序(实现Comparator接口)中，比较两个对象是否相同的标准为：compare()返回0，不再是equals()

```java
public class Test{
    @Test
    public void test2() {
        TreeSet set = new TreeSet();
        //TreeSet不能添加不同类的对象
        set.add(456);
        set.add(123);
        set.add(-12);
        set.add(789);

        //按照集合中对象的大小遍历。 使用Comparable接口的compareTo()
        Iterator iterator = set.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }

        TreeSet set1 = new TreeSet();
        set1.add(new Person("Tom", 22));
        set1.add(new Person("Jck", 23));
        set1.add(new Person("Mac", 21));
//        set1.add(new Person("Mac",22)); //Person中只写了name的比较
//        TreeSet中判断是否相同使用的是compareTo()，而不是equals()

        Iterator iterator1 = set1.iterator();
        while (iterator1.hasNext()) {
            System.out.println(iterator1.next());
        }
    }

    @Test
    public void test3() {
        Comparator comparator = new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                if (o1 instanceof Person && o2 instanceof Person) {
                    Person p1 = (Person) o1;
                    Person p2 = (Person) o2;

                    if (p1.getName().compareTo(p2.getName()) == 0) {
                        return Integer.compare(p1.getAge(), p2.getAge());
                    }

                    return p1.getName().compareTo(p2.getName());
                } else {
                    throw new RuntimeException("数据类型不一致");
                }
            }
        }; 

        //使用定制排序
        TreeSet set = new TreeSet(comparator); 
        //此时使用的是comparator对象中的compare()方法来比较是否相同，而不是Person类中的实现Comparable接口的compareTo();
        set.add(new Person("Tom", 22));
        set.add(new Person("Jck", 23));
        set.add(new Person("Mac", 21));
        set.add(new Person("Mac", 24));

        Iterator iterator = set.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }
    }
}
```

## Map接口

![](C:/NoteBook/pictures/453525713247603.png =502x)

-  Map接口的常用实现类：HashMap、TreeMap、LinkedHashMap和Properties。
   - 其中，HashMap是 Map 接口使用频率最高的实现类

**Map ： 双列数据，存储key-value对的数据**

- `HashMap` 作为Map的主要实现类 线程不安全，效率高 可以存储null的key和value
   - HashMap的底层实现：
      - jdk1.7之前 : 数组+链表 
      - jdk1.8 : 数组+链表+红黑树   
   - `LinkedMap` 保证在遍历map元素时，可以按照添加的顺序实现遍历
      - 原因：在原有的HashMap底层结构基础上，添加了一对指针，指向前一个和后一个元素。
      - 对于频繁的遍历操作 ，此类执行效率高于HashMap。
- `TreeMap` 保证按照添加的key-value进行排序，实现排序遍历。此时考虑key的自然排序或定制排序。
   - 底层使用红黑树。 
- `Hashtable` 作为古老的实现类 线程安全，效率低 不能存储null的key和value
   - `Properties` 常用来处理配置文件，key和value都是String类型。

**Map结构**

- Map与Collection并列存在。用于保存具有映射关系的数据:key-value
   - Map 中的 key 和 value 都可以是任何引用类型的数据
   - Map 中的 key 用Set来存放，不允许重复，即同一个 Map 对象所对应的类，须重写hashCode()和equals()方法
   - 常用String类作为Map的“键”
- key 和 value 之间存在单向一对一关系，即通过指定的 key 总能找到唯一的、确定的 value

![](C:/NoteBook/pictures/235545813240272.png =568x)

- Map中的key：无序的，不可重复的，使用Set存储所有的key 
   - key所在的类要重写equals()和hashCode()
- Map中的value：无序的，可重复的，使用Collection存储所有的value
   - value所在的类要重写equals()
- 一个键值对：key-value构成了一个Entry对象。
- Map中的entry：无序的，不可重复的，使用Set存储所有的entry

### Map接口中定义的方法

**添加、删除、修改操作：**

- `Object put(Object key,Object value)`：将指定key-value添加到(或修改)当前map对象中
- `void putAll(Map m)`:将m中的所有key-value对存放到当前map中
- `Object remove(Object key)`：移除指定key的key-value对，并返回value
- `void clear()`：清空当前map中的所有数据

**元素查询的操作：**

- `Object get(Object key)`：获取指定key对应的value
- `boolean containsKey(Object key)`：是否包含指定的key
- `boolean containsValue(Object value)`：是否包含指定的value
- `int size()`：返回map中key-value对的个数
- `boolean isEmpty()`：判断当前map是否为空
- `boolean equals(Object obj)`：判断当前map和参数对象obj是否相等

**元视图操作的方法：**

- `Set keySet()`：返回所有key构成的Set集合
- `Collection values()`：返回所有value构成的Collection集合
- `Set entrySet()`：返回所有key-value对构成的Set集合

```java
public class LinkedHashMapTest {
    @Test
    public void test1() {
        Map map = new HashMap();

        //put() 添加
        map.put("AA", 123);
        map.put("BB", 456);
        map.put("CC", 789);
        //修改 使用key替换原有的value
        map.put("AA", 999);

        System.out.println(map);//{123=AA, 456=BB, 789=CC}

        Map map1 = new HashMap();
        //putAll()
        map1.putAll(map);
        System.out.println(map);

        //remove() 按照指定的key来移除 返回移除的value值
        System.out.println(map1.remove("AA"));//999
        System.out.println(map1.remove("EE"));//null
        System.out.println(map1);//{BB=456, CC=789}

        //clear() 清空
        map1.clear(); //不等于：map = null
        System.out.println(map1);//{}
        System.out.println(map1.size());//0
    }

    @Test
    public void test2(){
        Map map = new HashMap();
        map.put("AA", 123);
        map.put("BB", 456);
        map.put("CC", 789);
        map.put(12,711);

        //get() 根据key来获取value
        System.out.println(map.get(12)); //711

        //containsKey() 是否包含指定的key
        boolean isExit = map.containsKey("AA");
        System.out.println(isExit);//true

        //containsValue() 是否包含指定的value
        boolean isExit1 = map.containsValue(456);
        System.out.println(isExit1);//true

        //size() 获取Map中键值对的个数
        System.out.println(map.size()); //4

        //isEmpty() 是否为空
        System.out.println(map.isEmpty()); //false

        //equals() 判断map和某个对象是否相等
    }

    @Test
    public void test3(){
        Map map = new HashMap();
        map.put("AA", 123);
        map.put("BB", 456);
        map.put("CC", 789);

        //keySet() 获取所有的key类
        Set set = map.keySet();
        Iterator iterator = set.iterator();
        while(iterator.hasNext()){
            System.out.println(iterator.next());
        }

        //values() 获取所有的value类
        Collection collection = map.values();
        Iterator iterator1 = collection.iterator();
        while(iterator1.hasNext()){
            System.out.println(iterator1.next());
        }

        //entrySet() 遍历所有的key-value
        //方式1
        Set entrySet = map.entrySet();
        Iterator iterator2 = entrySet.iterator();
        while(iterator2.hasNext()){
            Object obj;
            System.out.println(obj = iterator2.next()); //key=value

            //entrySet集合中的元素都是Entry
            Map.Entry entry = (Map.Entry) obj;
            System.out.println(entry.getKey() + "=" + entry.getValue());
        }

        //方式2
        Set keySet = map.keySet();
        Iterator iterator3 = keySet.iterator();
        while(iterator3.hasNext()){
            Object key = iterator3.next();
            Object value = map.get(key);

            System.out.println(key + "=" + value);
        }
    }
}
```

### HashMap类 底层实现原理

**常量**

- `DEFAULT_INITIAL_CAPACITY` : HashMap的默认容量，16
- `MAXIMUM_CAPACITY` ： HashMap的最大支持容量，2^30
- `DEFAULT_LOAD_FACTOR`：HashMap的默认加载因子(0.75)
- `TREEIFY_THRESHOLD`：Bucket中链表长度大于该默认值，转化为红黑树(8)
- `UNTREEIFY_THRESHOLD`：Bucket中红黑树存储的Node小于该默认值，转化为链表
- `MIN_TREEIFY_CAPACITY`：桶中的Node被树化时最小的hash表容量。
   - 当桶中Node的数量大到需要变红黑树时，若hash表容量小于MIN_TREEIFY_CAPACITY时，此时应执行resize扩容操作这MIN_TREEIFY_CAPACITY的值至少是TREEIFY_THRESHOLD的4倍。
- `table`：存储元素的数组，总是2的n次幂
- `entrySet`：存储具体元素的集
- `size`：HashMap中存储的键值对的数量
- `modCount`：HashMap扩容和结构改变的次数。
- `threshold`：扩容的临界值，= 容量*填充因子 (`16*0.75`)
  - 提前扩容，为了减少链表的长度。
- `loadFactor`：填充因子

**面试题：负载因子值的大小，对HashMap有什么影响**

- 负载因子的大小决定了HashMap的数据密度。
- 负载因子越大密度越大，发生碰撞的几率越高，数组中的链表越容易长,造成查询或插入时的比较次数增多，性能会下降。
- 负载因子越小，就越容易触发扩容，数据密度也越小，意味着发生碰撞的几率越小，数组中的链表也就越短，查询和插入时比较的次数也越小，性能会更高。但是会浪费一定的内容空间。而且经常扩容会影响性能，建议初始化预设大一点的空间。
- 按照其他语言的参考及研究经验，会考虑将负载因子设置为0.7~0.75，此时平均检索长度接近于常数

**HashMap的底层实现原理 以jdk1.7为例**

- `HashMap map = new HashMap()`;
  - 在实例化以后，底层创建了长度是16的一维数组Entry[] table。
- ...假设之前可能已经执行过多次put...
- `map.put(key1,value1)`;
  - 首先，调用key1所在类的hashCode()计算key1的哈希值，此哈希值经过某种算法计算之后，得到Entry数组中的存放位置。
  - 如果此位置上的数据为空，此时的key1-value1添加成功   **情况1**
  - 如果此位置上的数据不为空，（意味着此位置上存在一个或多个数据（以链表形式存在）），比较key1和已经存在的一个或多个数据的哈希值：
       - 如果key1的哈希值与已经存在的数据的哈希值都不相同，此时key1-value1添加成功  **情况2**
       - 如果key1的哈希值和已经存在的某一个数据(key2-value2)的哈希值相同，继续比较：调用key1所在类的equals()，比较：
         - 如果equals()返回false：此时key1-value1添加成功  **情况3**
         - 如果equals()返回true：使用value1替换相同key的value值(value2)。 
- 关于情况2和情况3：此时key1-value1和原来的数据以链表的方式存储
- 在不断的添加过程中会涉及到扩容问题：
  - 当超过临界值threadshould（12）时，且要存放的位置非空时，扩容。
  - 默认的扩容方式：扩容为原来容量的2倍，并将原来的数据复制过来。

![](C:/NoteBook/pictures/331595916239572.png =851x)

**HashMap的底层实现原理 jdk1.8相较与jdk1.7的不同**

- `new HashMap()`底层没有创建一个长度为16 的数组
- jdk1.8 底层的数组是 Node[]  而非Entry[];
- 首次调用put()方法时，底层创建长度为16的数组 
- jdk1.7的底层结构只有数组+链表；jdk1.8的底层结构：数组+链表+红黑树
  - jdk1.7新的元素指向旧的元素，jdk1.8旧的元素指向新的元素. 
  - 当数组的某一个索引位置上的元素以链表形式存在的数据个数 > 8 且当前数组的长度 > 64，此时，此索引位置上的所有数据改为使用红黑树存储。 

![](C:/NoteBook/pictures/506635916227439.png =920x)

### LinkedHashMap类 底层实现原理

- 继承自HashMap，底层基本相同

- LinkedMap中定义内部类 Entry替换了HashMap中的Node

```java
static class Entry<K,V> extends HashMap.Node<K,V> {
    Entry<K,V> before, after;
    Entry(int hash, K key, V value, Node<K,V> next) {
        super(hash, key, value, next);
    }
}
```

### TreeSet类

- TreeMap存储 Key-Value 对时，需要根据 key-value 对进行排序。
   - TreeMap 可以保证所有的 Key-Value 对处于有序状态。
- TreeSet底层使用红黑树结构存储数据
- TreeMap要求所有的 Key 应该是同一个类的对象，否则将会抛出 ClasssCastException

**TreeMap 的 Key 的排序：**


- 自然排序：TreeMap 的所有的 Key 必须实现 Comparable 接口
- 定制排序：创建 TreeMap 时，传入一个 Comparator 对象，该对象负责对TreeMap 中的所有 key 进行排序。
  - 此时不需要 Map 的 Key 实现Comparable 接口
- TreeMap判断两个key相等的标准：两个key通过compareTo()方法或者compare()方法返回0。

```java
public class TreeMapTest {
    @Test
    public void test1() {
        Person p1 = new Person("Tom", 21);
        Person p2 = new Person("Jac", 23);
        Person p3 = new Person("Mac", 19);

        TreeMap map = new TreeMap();

        map.put(p1, 99);
        map.put(p2, 98);
        map.put(p3, 100);

        //自然排序
        //Person类中重写了Comparable接口的compareTo()方法
        Set entrySet = map.entrySet();
        Iterator iterator = entrySet.iterator();
        while (iterator.hasNext())
            System.out.println(iterator.next());

        TreeMap treeMap = new TreeMap(new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                if (o1 instanceof Person && o2 instanceof Person) {
                    Person p1 = (Person) o1;
                    Person p2 = (Person) o2;

                    return p1.getAge() - p2.getAge();
                }
                throw new RuntimeException("数据类型不一致");
            }
        });
        treeMap.putAll(map);


        Set entrySet1 = treeMap.entrySet();
        Iterator iterator1 = entrySet1.iterator();
        while (iterator1.hasNext())
            System.out.println(iterator1.next());
    }
}
```

### Properties类

- Properties 类是 Hashtable 的子类，该对象用于处理属性文件
   - 由于属性文件里的 key、value 都是字符串类型，所以 Properties 里的 key 和 value 都是字符串类型
- 存取数据时，建议使用`setProperty(String key,String value)`方法和`getProperty(String key)`方法
  
![](C:/NoteBook/pictures/558883523239572.png =506x)

```java
//建立jdbc.properties

name=Tom\u73A9\u5BB6
password=abc123
```

```java
public class PropertiesTest {
    public static void main(String[] args) throws IOException {
        Properties properties = new Properties();

        FileInputStream fileIn = new FileInputStream("jdbc.properties");

        properties.load(fileIn); //加载流对应的文件

        String name = properties.getProperty("name");
        String password = properties.getProperty("password");

        System.out.println("name=" + name + "\n" + "passsword=" + password);
        fileIn.close();
    }
}
```

## Collections工具类

- Collections 是一个操作 Set、List 和 Map 等集合的工具类
- Collections 中提供了一系列静态的方法对集合元素进行排序、查询和修改等操作，还提供了对集合对象设置不可变、对集合对象实现同步控制等方法

**排序操作：（均为static方法）**

- `reverse(List)`：反转 List 中元素的顺序
- `shuffle(List)`：对 List 集合元素进行随机排序
- `sort(List)`：根据元素的自然顺序对指定 List 集合元素按升序排序
- `sort(List, Comparator)`：根据指定的 Comparator 产生的顺序对 List 集合元素进行排序
- `swap(List, int, int)`：将指定 list 集合中的 i 处元素和 j 处元素进行交换

**查找、替换**

- `Object max(Collection)`：根据元素的自然顺序，返回给定集合中的最大元素
- `Object max(Collection，Comparator)`：根据 Comparator 指定的顺序，返回给定集合中的最大元素
- `Object min(Collection)`
- `Object min(Collection，Comparator)`
- `int frequency(Collection，Object)`：返回指定集合中指定元素的出现次数
- `void copy(List dest,List src)`：将src中的内容复制到dest中
  - 注意dest的长度问题，报错 
- `boolean replaceAll(List list，Object oldVal，Object newVal)`：使用新值替换List 对象的所有旧值

```java
public class CollectionsTest {
    @Test
    public void test1() {
        List list = new ArrayList();
        list.add(123);
        list.add(456);
        list.add(11);
        list.add(789);

        System.out.println(list); //[123, 456, 11, 789]

        //Collections.reverse(list); 反转
        Collections.reverse(list);
        System.out.println(list); //[789, 11, 456, 123]

        //Collections.shuffle(list); 随机排序
        Collections.shuffle(list);

        //Collections.sort(list) 自然排序
        Collections.sort(list);

        //Collections.sort(list,Comparator) 定制排序
        Comparator comparator = new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                if (o1 instanceof Integer && o2 instanceof Integer) {
                    Integer i1 = (Integer) o1;
                    Integer i2 = (Integer) o2;

                    return i1 - i2;
                }
                throw new RuntimeException("数据类型不一致");
            }
        };
        Collections.sort(list,comparator);
        System.out.println(list); //[11, 123, 456, 789]

        //Collections.swap(list, int x, int y) 交换list集合中的x和y位置的元素
        Collections.swap(list,1,2);
        System.out.println(list);

        //Collections.frequency(Collection, Object)  出现的频率
        System.out.println(Collections.frequency(list, 11)); //1
    }

    @Test
    public void test2(){
        List list = new ArrayList();
        list.add(123);
        list.add(456);
        list.add(11);
        list.add(789);

        //Collections.copy(toList,fromList) 复制

        //报异常: IndexOutOfBoundsException: Source does not fit in dest
        //List copyToList = new ArrayList();
        //Collections.copy(list,copyToList);

        List toList = Arrays.asList(new Object[list.size()]);
        //System.out.println(toList); //[null, null, null, null]
        Collections.copy(toList,list);
        System.out.println(toList);
    }
}
```

### Collections常用方法 : 同步控制

Collections 类中提供了多个 synchronizedXxx() 方法，该方法可使将指定集合包装成线程同步的集合，从而可以解决多线程并发访问集合时的线程安全问题

```java
@Test
public void test3() {
    List list = new ArrayList();
    list.add(123);
    list.add(456);
    list.add(11);
    list.add(789);

    //Collections.synchronizedXxx() 返回线程安全的
    //返回线程安全的list
    List list1 = Collections.synchronizedList(list);
}
```

## 练习

### 1.

请从键盘随机输入10个整数保存到List中，并按倒序、从大到小的顺序显示出来

```java
public class Test01 {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        List list = new ArrayList();

        synchronized (scan.getClass()) {
            for (int i = 0; i < 10; i++) {
                list.add(scan.nextInt());
            }
        }

        Comparator comparator = new Comparator() {
            @Override
            public int compare(Object o1, Object o2) {
                if (o1 instanceof Integer && o2 instanceof Integer) {
                    Integer i1 = (Integer) o1;
                    Integer i2 = (Integer) o2;

                    return -i1.compareTo(i2);
                }
                throw new RuntimeException("数据类型不一致");
            }
        };

        Collections.sort(list, comparator);
        System.out.println(list);
        
        Collections.reverse(list);
        System.out.println(list);
    }
}
```


### 2.

请把学生名与考试分数录入到集合中，并按分数显示前三名成绩学员的名字。

TreeSet(Student(name,score,id));

```java
package com.zjk.java1;

import java.util.Iterator;
import java.util.Objects;
import java.util.TreeSet;

public class Test02 {
    public static void main(String[] args) {
        TreeSet treeSet = new TreeSet();
        treeSet.add(new Student("Jac", 78, 01));
        treeSet.add(new Student("Mac", 99, 02));
        treeSet.add(new Student("Tom", 50, 03));
        treeSet.add(new Student("Joc", 60, 04));
        treeSet.add(new Student("King", 33, 05));

        Iterator iterator = treeSet.iterator();

        int i = 0;
        while(iterator.hasNext()){
            if(i++ == 3)
                break;
            System.out.println(iterator.next());
        }

    }
}

class Student implements Comparable {
    private String name;
    private int score;
    private int id;

    public Student(String name, int score, int id) {
        this.name = name;
        this.score = score;
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getScore() {
        return score;
    }

    public void setScore(int score) {
        this.score = score;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Student student)) return false;
        return getId() == student.getId() && Objects.equals(getName(), student.getName()) && Objects.equals(getScore(), student.getScore());
    }

    @Override
    public int hashCode() {
        return Objects.hash(getName(), getScore(), getId());
    }

    @Override
    public int compareTo(Object o) {
        if (o instanceof Student) {
            Student s = (Student) o;

            return -(this.score - s.score);
        }
        throw new RuntimeException("数据类型不一致");
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", score=" + score +
                ", id=" + id +
                '}';
    }
}
```

### 3.

![](C:/NoteBook/pictures/381045210221147.png =493x)

```java

```

### 4.

 对一个Java源文件中的关键字进行计数。

提示：Java源文件中的每一个单词，需要确定该单词是否是一个关键字。为了高效处理这个问题，将所有的关键字保存在一个HashSet中。用contains()来测试。

```java
File file = new File("Test.java");
Scanner scanner = new Scanner(file);
while(scanner.hasNext()){
    String word = scanner.next();
    System.out.println(word);
}
```

```java

```

# 泛型 （钻石操作符）

## 概念

集合容器类在设计阶段/声明阶段不能确定这个容器到底实际存的是什么类型的对象，所以在JDK1.5之前只能把元素类型设计为Object，JDK1.5之后使用泛型来解决。因为这个时候除了元素的类型不确定，其他的部分是确定的，例如关于这个元素如何保存，如何管理等是确定的，因此此时把元素的类型设计成一个参数，这个类型参数叫做泛型。`Collection<E>，List<E>，ArrayList<E>` 这个`<E>`就是类型参数，即泛型。

- 所谓泛型，就是允许在定义类、接口时通过一个标识表示类中某个属性的类型或者是某个方法的返回值及参数类型。这个类型参数将在使用时（例如，继承或实现这个接口，用这个类型声明变量、创建对象时）确定（即传入实际的类型参数，也称为类型实参）。


## 集合中使用泛型

- 集合接口或集合类在jdk5.0时，都修改为带泛型的结构。JDK1.5改写了集合框架中的全部接口和类，为这些接口、类增加了泛型支持，从而可以在声明集合变量、创建集合对象时传入类型实参。
- 在实例化集合类时，可以指明具体的泛型类型。Java引入了“参数化类型（Parameterized type）”的概念，允许我们在创建集合时再指定集合元素的类型，正如：`List<String>`，这表明该List只能保存字符串类型的对象。
- 指明完之后，在集合类或接口中凡是定义类或接口时，内部结构使用到泛型的位置，都指定为指明的泛型类型
  - 如：`add(E e)` 实例化之后 `add(Integer e)`
   
- 注：泛型中的类型必须是类，不能是基本数据类型，需要用到基本数据类型的位置，要使用其包装类。
- 如果实例化时，没有指明泛型的类型。默认类型为java.lang.Object类型

**JDK 7.0新特性：类型推断：可以只写一部分的<>泛型指明**

- 如：`Map<String,Integer> map = new HashMap<>();`

1. 解决元素存储的安全性问题
2. 解决获取数据元素时，需要类型强制转换的问题

- Java泛型可以保证如果程序在编译时没有发出警告，运行时就不会产生`ClassCastException`异常。同时，代码更加简洁、健壮。

![](C:/NoteBook/pictures/224281213221147.png =582x)

![](C:/NoteBook/pictures/371651213239573.png =564x)

```java
package com.zjk.java1;

import org.junit.Test;

import java.util.*;

public class GenericTest {
    @Test
    public void test1() {
        //在集合中使用泛型的情况
        ArrayList list = new ArrayList();
        //存放学生的成绩
        list.add(78);
        list.add(22);
        list.add(34);

        //问题1：类型不安全
        //list.add("Tom");

        for (Object obj : list) {
            //问题2：强转时，可能出现ClassCastException，类型转换异常
            int score = (Integer) obj;
            System.out.println(score);
        }
    }

    //以ArrayList为例
    @Test
    public void test2() {
        ArrayList<Integer> list = new ArrayList<Integer>();
        list.add(78);
        list.add(37);
        //在编译时，就会进行类型检查，保证数据的安全
        //list.add("we");

        //方式1
        for (Integer i : list) {
            //避免了强转操作
            int score = i;
            System.out.println(i);
        }
        //方式2
        Iterator<Integer> iterator = list.iterator();
        while (iterator.hasNext()) {
            int num;
            System.out.println(num = iterator.next());
        }
    }

    //以HashMap为例
    @Test
    public void test3() {
        HashMap<String, Integer> map = new HashMap<String, Integer>();

        map.put("Jac", 12);
        map.put("Max", 23);
        map.put("Tom", 34);
        //map.put(123,"sad");

        Set<Map.Entry<String, Integer>> entry = map.entrySet();
        Iterator<Map.Entry<String, Integer>> iterator = entry.iterator();
        while (iterator.hasNext()) {
            Map.Entry<String, Integer> e = iterator.next();
            String key = e.getKey();
            Integer value = e.getValue();
            System.out.println(key + "=" + value);
        }
    }
}
```

```java
public class Employee implements Comparable<Employee>{
    
     ...
    
    //指明泛型时
    @Override
    public int compareTo(Employee e) {
        if (e instanceof Employee) {

            return this.name.compareTo(e.getName());
        } else {
            throw new RuntimeException("数据类型不一致");
        }
    }
}
```

```java
Collection<Employee> treeSet = new TreeSet<Employee>(new Comparator<Employee>() {
    @Override
    public int compare(Employee e1, Employee e2) {
        return e1.getBirthday().getYear() - e2.getBirthday().getYear();
    }
});
```

## 自定义泛型结构

- 泛型类
- 泛型接口
- 泛型方法

1. 泛型的声明
   - `interface List<T> 和 class GenTest<K,V> ` 
      - 其中，T,K,V不代表值，而是表示类型。这里使用任意字母都可以。常用T表示，是Type的缩写。
2. 泛型的实例化：
   - 一定要在类名后面指定类型参数的值（类型）。如：
   - `List<String> strList = new ArrayList<String>();`
   - `Iterator<Customer> iterator = customers.iterator();`
- T只能是类，不能用基本数据类型填充。但可以使用包装类填充
- 把一个集合中的内容限制为一个特定的数据类型，这就是generics背后的核心思想

### 自定义泛型类/泛型接口

```java
package com.zjk.java2;

public class Order<T> {
    private String orderName;
    private int orderId;

    //类的内部结构就可以使用类的泛型
    T orderT;

    public Order() {

    }

    public Order(String orderName, int orderId, T orderT) {
        this.orderName = orderName;
        this.orderId = orderId;
        this.orderT = orderT;
    }

    public T getOrderT() {
        return orderT;
    }

    public void setOrderT(T orderT) {
        this.orderT = orderT;
    }
}
```

```java
package com.zjk.java2;


//在继承时，指明泛型
public class SubOrder extends Order<String> {
}

//在继承时，不指明泛型
//SubOreder1<T>:仍然是泛型类
class SubOrder1<T> extends Order<T> {
}



package com.zjk.java2;

import org.junit.Test;

public class GenericTest1 {
    @Test
    public void test1() {
        Order o1 = new Order();
        //如果定义了泛型类，实例化没有指明类的泛型，则认为此泛型类型为Object类型
        //要求：如果定义了的类是带泛型的，建议在实例化时指明类的泛型
        o1.setOrderT(123);
        o1.setOrderT("Tom");

        //建议：实例化时指明类的泛型
        Order<String> o2 = new Order<String>("order02", 1002, "Test");

        o2.setOrderT("reTest");
    }

    @Test
    public void test2() {
        //由于子类在继承带泛型的父类时，指明了泛型类型，则实例化子类对象时，不再需要指明泛型。
        SubOrder subOrder = new SubOrder();
        subOrder.setOrderT("Test");


        SubOrder1<String> subOrder1 = new SubOrder1();
        subOrder1.setOrderT("Test");
    }
}
```

**注：**

1. 泛型类可能有多个参数，此时应将多个参数一起放在尖括号内。比如：`<E1,E2,E3>`
2. 泛型类的构造器如下：`public GenericClass(){}`。而下面是错误的：`public GenericClass<E>(){}`
3. 实例化后，操作原来泛型位置的结构必须与指定的泛型类型一致。
4. 泛型不同的引用不能相互赋值。
    - 尽管在编译时`ArrayList<String>和ArrayList<Integer>`是两种类型，但是，在运行时只有一个ArrayList被加载到JVM中。
5. 泛型如果不指定，将被擦除，泛型对应的类型均按照Object处理，但不等价于Object。
    - 经验：泛型要使用一路都用。要不用，一路都不要用。
6. 如果泛型结构是一个接口或抽象类，则不可创建泛型类的对象。
7. jdk1.7，泛型的简化操作：`ArrayList<Fruit> flist = new ArrayList<>();`
8. 泛型的指定中不能使用基本数据类型，可以使用包装类替换。
9. 在类/接口上声明的泛型，在本类或本接口中即代表某种类型，可以作为非静态属性的类型、非静态方法的参数类型、非静态方法的返回值类型。  
    - 但在静态方法中不能使用类的泛型。
10. 异常类不能是泛型的
11. 不能使用`E[] arr = new E[]`。
    - 但是可以：`E[] arr = (E[])new Object[10];`
    - 参考：ArrayList源码中声明：`Object[] elementData`，而非泛型参数类型数组。
12. 父类有泛型，子类可以选择保留泛型也可以选择指定泛型类型：
    - 子类不保留父类的泛型：
       - 按需实现
       - 没有类型 擦除具体类型
    - 子类保留父类的泛型：泛型子类
       - 全部保留
       - 部分保留
    - 结论：子类必须是“富二代”，子类除了指定或保留父类的泛型，还可以增加自己的泛型


```java
@Test
public void test3(){
    ArrayList<String> list1 = null;
    ArrayList<Integer> list2 = null;

    //泛型不同的引用不能相赋值
    //list1 = list2;
}
```

```java
//静态方法中不能使用类的泛型
//类的泛型是在实例化的时候指明的，而静态方法在类中实现
//public static void show(T orderT){
//    System.out.println(orderT);
//}
```

```java
class GenericTest {
    public static void main(String[] args) {
    // 1、使用时：类似于Object，不等同于Object
        ArrayList list = new ArrayList();
    // list.add(new Date());//有风险
        list.add("hello");
        test(list);// 泛型擦除，编译不会类型检查
    // ArrayList<Object> list2 = new ArrayList<Object>();
    // test(list2);//一旦指定Object，编译会类型检查，必须按照Object处理
    }
    public static void test(ArrayList<String> list) {
        String str = "";
        for (String s : list) {
            str += s + ",";
        }
        System.out.println("元素:" + str);
    }
}
```

```java
class Father<T1, T2> {
}
// 子类不保留父类的泛型
// 1)没有类型 擦除
class Son1 extends Father {// 等价于class Son extends Father<Object,Object>{
}
// 2)具体类型
class Son2 extends Father<Integer, String> {
}
// 子类保留父类的泛型
// 1)全部保留
class Son3<T1, T2> extends Father<T1, T2> {
}
// 2)部分保留
class Son4<T2> extends Father<Integer, T2> {
}
```

### 自定义泛型方法

- 方法，也可以被泛型化，不管此时定义在其中的类是不是泛型类。在泛型方法中可以定义泛型参数，此时，参数的类型就是传入数据的类型。

**泛型方法的格式：**

```java
[访问权限] <泛型> 返回类型 方法名([泛型标识 参数名称]) 抛出的异常
```

```java
public class Order<T>{
    //泛型方法：在方法中出现泛型结构，泛型的参数与类的泛型参数没有任何关系
    //即：泛型方法所属的类是否是泛型类均可
    //泛型方法可以是static的,在调用时指明泛型,并非实例化时。
    public static <E> List<E> copyFormArrayToList(E[] arr) {
        ArrayList<E> list = new ArrayList<>();

        for (E e : arr) {
            list.add(e);
        }
        return list;
    }
}


@Test
public void test4() {
    Order<String> order = new Order<>();
    Integer[] arr = new Integer[]{1,2,3,4};
    //泛型方法在调用时指明泛型参数的类型
    List<Integer> list = order.copyFormArrayToList(arr);
    System.out.println(list);
}
```

## 泛型在继承方面的体现

- 类A是类B的父类，
   - 但是`G<A>和G<B>`不具备子父类关系，编译不通过。
      - 二者公共的父类是`G<?>` 
   - 而：`A<G>和B<G>`具备子父类关系

```java
@Test
public void test5() {
    Object obj = null;
    String str = null;
    obj = str;

    Object[] objects = null;
    String[] strings = null;;
    objects = strings;

    //此时的list1和list2不具备子父类关系，是并列的。
    List<Object> list1 = null;
    List<String> list2 = null;
    //list1 = list2;

    GenericTest1 test = new GenericTest1();
    test.show(list1);
    //show(List<Object> list) 参数也不能是是list2
    //test.show(list2);
    test.show1(list2);

    //此时的list01和list02具备子父类关系
    List<String> list01 = null;
    ArrayList<String> list02 = null;
    list01 = list02;
}

public void show(List<Object> list){
}

public void show1(List list){
}
```

## 通配符的使用

### `<?>`

**? 通配符**

- 设类A是类B的父类，`G<?>`作为`G<A>`和`G<B>`的公共父类

**添加**

- 使用通配符?的结构，对于List<?>就不能向其内部添加数据。
- 除了添加null之外；
   
**读取**

- 允许读取数据，读取的数据类型为Object

### 有限制条件的通配符

**<?>**

- 允许所有泛型的引用调用

**通配符指定上限**

- `? extends 类`
- 上限extends：使用时指定的类型必须是(该类的子类)继承某个类，或者实现某个接口，即<= 

**通配符指定下限**

- `? super 类`
- 下限super：使用时指定的类型（该类的父类）不能小于操作的类，即>=

```java
@Test
public void test7() {
    List<? extends Person> list1 = null;
    List<? super Person> list2 = null;

    List<Student> list3 = new ArrayList<Student>();
    List<Person> list4 = new ArrayList<Person>();
    List<Object> list5 = new ArrayList<Object>();

    list1 = list3;
    list1= list4;
    //list1 = list5; ? extends Person 需要小于等于Person

    //list2 = list3; ? super Person 需要大于等于Person
    list2 = list4;
    list2 = list5;

    //读取数据
    Person p1 = list1.get(0);
    //Student s1 = list1.get(0); //编译不通过
    Object o2 = list2.get(0);
    //Person p2 = list2.get(0); //编译不通过

    //写入数据
    //list1.add(new Person()); 编译不通过
    //list1.add(new Student()); 编译不通过
    list2.add(new Person());
    list2.add(new Student());
}
```

## 泛型嵌套

## 练习

- 定义个泛型类 DAO<T>，在其中定义一个 Map 成员变量，Map 的键为 String 类型，值为 T 类型。
- 分别创建以下方法：
  - public void save(String id,T entity)： 保存 T 类型的对象到 Map 成员变量中
  - public T get(String id)：从 map 中获取 id 对应的对象
  - public void update(String id,T entity)：替换 map 中 key 为 id 的内容,改为 entity 对象
  - public List<T> list()：返回 map 中存放的所有 T 对象
  - public void delete(String id)：删除指定 id 对象
  - 定义一个 User 类：
     - 该类包含：private 成员变量（int 类型） id，age；（String 类型）name。
- 定义一个测试类：
- 创建 DAO 类的对象， 分别调用其 save、get、update、list、delete 方法来操作 User 对象，使用 Junit 单元测试类进行测试

```java
package com.zjk.java2;

import org.junit.Test;

import java.util.*;

public class DAO<T> {
    @Test
    public void test(){
        DAO<User> dao = new DAO<>();

        User user1 = new User(1001,19,"Tom");
        User user2 = new User(1002,21,"Mac");
        User user3 = new User(1003,20,"Jac");

        dao.save("AA",user1);
        dao.save("BB",user2);
        dao.save("CC",user3);

        System.out.println(dao.get("BB"));

        User user4 = new User(1004,33,"King");
        dao.update("BB",user4);

        dao.list().forEach(System.out::println);

        dao.delete("AA");
    }
    
    private Map<String, T> map = new HashMap<String, T>();

    public void save(String id,T entity){
        map.put(id,entity);
    }

    public T get(String id){
        return map.get(id);
    }

    public void update(String id,T entity){
        Set set = map.keySet();
        if(set.contains(id))
            map.put(id,entity);
        else
            System.out.println("未找到数据");
    }


    public List list(){
        Collection<T> collection = map.values();
        List<T> list = new ArrayList<T>();

        Iterator<T> iterator = collection.iterator();
        while(iterator.hasNext()){
            list.add(iterator.next());
        }
        return list;
    }

    //public List list(){
    //    Collection collection = map.values();
    //    List list = Arrays.asList(collection.toArray());
    //    return list;
    //}
    
    //编译通过，但强转错误。Collection -> List错误
    //public List list() {
    //    Collection<T> values = map.values();
    //    return (List<T>) values;
    //}

    public void delete(String id){
        map.remove(id);
    }
}

class User implements Comparable<User>{
    private int id;
    private int age;
    private String name;

    public User(int id, int age, String name) {
        this.id = id;
        this.age = age;
        this.name = name;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof User user)) return false;
        return id == user.id && age == user.age && Objects.equals(name, user.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, age, name);
    }

    @Override
    public int compareTo(User u) {
        return this.age - u.age;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", age=" + age +
                ", name='" + name + '\'' +
                '}';
    }
}
```


# I/O

**I/O是从计算机主机系统的角度上看的**

- 主机系统：运算器，控制器，存储器
   - CPU ： 运算器，控制器，合为CPU
      - 控制器：执行代码
      - 运算器：运算
- 输入：将外部的数据写入主机设备。
   - 读取外部数据（磁盘、光盘等存储设备）到程序（内存）中。 
- 输出：将主机设备的数据写入外部设备。
   - 将程序（内存）数据输出到磁盘、光盘等存储设备中。 

- 键盘和显示器合称标准控制台
  - 键盘：标准输入设备
  - 显示器：标准输出设备
   
## 流式输入/输出

在java中，流(stream)是从源到目的地的字节的有序序列。流中的字节依据先进先出，具有严格顺序，因此流式I/O是一种顺序存取方式。



### 流的概念

| 抽象基类 |    字节流     | 字符流 |
| :------: | :----------: | :----: |
|  输入流  | InputStream  | Reader |
|  输出流  | OutputStream | Writer |

![](C:/NoteBook/pictures/207913315221148.png =587x)

![](C:/NoteBook/pictures/113763815239574.png =663x)

- 输入流（读）
   - java程序可以打开一个从某种数据源（如文件、内存等）到程序的一个流，从这个流中读取数据，这就是输入流。因为流是有方向的，所以只能从输入流读入，而不能向它写数据，
   ![](C:/NoteBook/pictures/590751810239498.png =340x)
- 输出流（写）
   - 同样，程序可以打开到外界某种目的地的流，把数据顺序写到该流中，以把程序中的数据保存在外界，这就是输出流。与输入流相类似，只能向该流写，而不能从该流中读取数据，
   ![](C:/NoteBook/pictures/312871910247531.png =350x)
因此，Java中有两种基本的流——输入流(InputStream)与输出流(OutpurStream)，对这两种流都采取相同的顺序读写方式，

**其读写操作过程如下。**

- 流的读操作过程：打开流→当流中还有数据时执行读操作→关闭流。
- 流的写操作过程，打开流→当有数据需要输出时执行写操作→关闭流。

**Java中实现上述流式I/O的类都在java.io包中。**

**这些类根据流相对于程序的另一个端点的不同:**

分为节点流(Node Stream)和过滤流(Filter Stream)。

- 节点流
   - 以特定源如磁盘文件、内存某区域或线程之间的管道为端点构造的输入/输出流，它是一种最基本的流。
   - 是真正完成读写的
- 过滤流
   - 以其他已经存在的流为端点构造的输入/输出流，称为过滤流或处理流，要对与其相连的另一个流进行某种转换。
   - 可以提高读写操作的某一方面的性能
   - 必须依附于某个节点流

**流类根据流中的数据单位不同也分为两个类的层次体系**

- 字节流：
   - 流中的数据以8位字节为单位进行读写，以InputStream与OutputStream为基础类。
   - 主要用于访问非文本
- 字符流：
   - 流中的数据以16位字符为单位进行读写，以Reader与Writer为基础类。
   - 访问文本
      - ASCII码或Unicode码字符组成的文件
      - 文本文件，简单的判定方法：
         - 即用一个通用的文本编辑器能够打开并读懂的文件 
    
java中流常指的是字节流。

**可以发现实际上字节流与字符流主要的区别在于处理的数据类型。**

Reader/Writer与InputStream/OutputStream具有相类似的API,并且每个核心的输入/输出字节流，都有相应的Reader和Writer版本，

例如FilelnputStream/FileOutputStream与FileReader/FileWriter,PipedInputStream/PipedOutputStream PipedReader/PipedWriter等

### 字节流

- InputStream和OutputStream是字节流的两个顶层父类。它们提供了输入流类与输出流类的通用API。
- 字节流一般用于读写二进制数据，如图像和声音数据。
- 有两个字节流ObjectInputStream和ObjectOutputStream是用来实现对象的串行化，即对象的输入/输出，

#### 输入字节流

InputStream是输入字节流类的抽象顶层父类，它包含了所有输入流类都继承并实现的基本数据读取方法

**read() 基本的读方法。**

InputStream类中最基本的方法应该是read()。该类中定义了如下3个read()方法：

- int read(): 
   - 读一个字节作为方法的返回值。如果返回值是一1，则表示文件结束。
- int read(byte[] b):
   - 将读入的数据放在一个字节数组中，并返回所读的字节数。
- int(byte[] b, int off, int len):
   - 将读入的数据放在一个字节数组中，并返回所读的字节数。
   - 两个整型参数表示所读入数据在数组b中的存放位置。

**void close()**

- 当输入流中的数据读取完毕后，使用该方法关闭流。
- 对于过滤流，则把最顶层的流关闭，会自动自顶向下关闭所有流。

**int available()**

- 返同输人流中还有多少可读的字节。
- 在读取大块数据前，常使用该方法测试。

**long skip(long n)**

- 跳过（扔掉）流中指定字节数量的数据

**流的回读方法。**

可以通过下列3个方法提供"书签"功能，在支持回读的流上实现已读取数据的重复读。

- boolean markSupported():
   - 测试打开的流是否支持同读。
- void mark(int readlimt):
   - 标记当前流，并创建大小由readlimt指示的缓冲区。
   - 方法的参数指定了将来通过reset()方法能够重复读取的字节数。
- void reset()
   - 如果用mark()方法对流做了标记，则在继续从流中读取一定数量的字节后调用reset()方法，将使后续的读操作从标记处开始读数据。
   - 如果在做标记后所读取的字节数大于mark()方法所创建的缓冲区的大小，则reset()方法将不起任何作用
   

#### 输出字节流


OutputStream是输出字节流类的抽象顶层父类。它所包含的成员方法如下。

**write() 基本的写方法**

InputStream类中最基本的方法应该是write()。该类中定义了如下3个write()方法。

- void write(int c):
   - 向输出流中写一个字节。
- void write(byte[] b):
   - 向输出流中写一个字节数组。
- void write(byte[] b, int off, int len):
   - 将字节数组中由off和len指示的数据块写入输出流。
   
**void close()**

- 当完成输出流的写操作后关闭流。如使用过滤流，则把最顶层的流关闭，会自动自顶向下关闭所有流。

**void flush()**

- 该方法将强制将缓存的输出数据写出去。
- 有的输出流会把几次写操作的数据缓存，然后一起提交，而该方法将把这些数据立即写到目的地。
- 一般在调用close()方法关闭流前，可以先调用flush()方法。


### 字符流

- Reader和Writer是java.io包中两个字符流类的顶层抽象父类。它们定义了在输入/输出流中读写16位字符的通用API。
- 字符流能够处理Unicode字符集中的所有字符，而字节流仅限于处理ISO-Latin-1的8位字节。
  - 所以使用字符流来读写文本类数据 
  
#### 输入字符流

**read() 基本的读方法**

- int read() 
  - 读一个字符作为返回值
  - 如果返回值是-1，则表示文件结束
- int read(char[] cbuf)
  - 读字符放入数组中，返回所读的字符数  
- int read(char[] cbuf, int offset, int length)
  - 读字符放入数组的指定位置，返回所读的字符数 
  
**void close()**

- 关闭流

**long skip(long n)**

- 跳过n个字符

**boolean markSupported()**

- 测试打开的流是否支持书签

**void mark(int buf)**

- 标记当前的流，并建立由参数buf指示大小的缓冲区

**void reset()**

- 返回标签处

**boolean ready()**

- 测试当前流是否准备好进行读

#### 输出字符流

Writer类是输出字符流抽象顶层父类。它所包含的成员方法如下。

**int write() 基本的写方法**

- int write(intc):
   - 写单个字符。
- int write(char cbuf[]):
   - 写字符数组。
- int write(char cbuf[], int offset, int length):
   - 写字符数组中的部分数据。
- int write(String str)
   - 写一个字符串。
- int write(String str, int offset, int length):
   - 写字符串的一部分。
   
**void close()**

- 关闭流。

**void flush()**

- 强行写。



### Java流式I/O类概述

#### Java节点流分类与描述

##### Memory(内存I/O)

- 从/向内存数组读写数据
   - ByteArrayInputStream
   - ByteArrayOutputStream
   - CharArrayReader
   - CharArrayWriter
- 从/向内存字符串读写数据
   - StringBufferInputStream
   - StringReader
   - StringWriter 
##### Pipe(管道I/O)

- 实现管道的输入和输出
   - PipedInputStream
   - PipedOutoutStream
   - PipedReader
   - PipedWriter

##### File(文件I/O)
- 统称为文件流，对文件进行读写操作
   - FileInputStream
   - FileOutputStream
   - FileReader
   - FileWriter    

#### Java过滤流(处理流)分类与描述

##### Object Serialization(对象I/O)

- 实现对象的输入/输出
    - ObjectInputStream
    - ObjectOutputStream

##### Data Conversion(数据转换)

- 按基本数据类型读写数据
    - DataInputStream
    - DataOutputStream

##### Printing（打印流）

- 包含方便的打印方法，是最简单的输出流
    - PrintStream
    - PrinterWriter

##### Buffering（缓存I/O）

- 在读入或写出时，对数据进行缓存，以减少I/O的次数。缓存流一般比相类似的非缓存流效率高，并且常与其他流一起使用。
- Buffer，缓存区（内存），专用于解决高速设备和低速设备的匹配。
    - BufferedInputStream
    - BufferedOutputStream
    - BufferedReader
    - BufferedWriter      

##### Filtering（流过滤）

- 是抽象类，定义了过滤流的通用方法。这些方法将在数据读写时进行过滤
   - FilterInputStream
   - FilterOutputStream
   - FilterReader
   - FilterWriter 

##### Concatenation（流连接）

- 将多个输入流连接成一个输入流
   - SequenceInputStream

##### Counting（流数据计数）

- 在读入数据时对行计数
   - LineNumberReader
   - LineNumberInputStream
##### Peeking Ahead（流预读）

- 提供缓存机制，进行预读
   - PushbackInputStream
   - PushbackReader
   
##### Convering between Bytes and Characters（字节与字符转换）

- InputStreamReader按照一定的编码/解码标准将InputStream中的字节转换为字符；
- OutputStreamWriter进行反向转换，即把字符转换为字节

### 输入输出流的套接

- 节点流在程序中不是很常用。一般常通过过滤流将多个流套接在一起，利用各种流的特性共同处理数据。套接的多个流构成了一个流链。
- 流链的中间流与程序所读写的最终流是过滤流，而直接对数据源读写的是节点流。
- 程序中可以根据对外界输入/输出数据的需要构造I/O的流链，以方便数据的处理并提高处理的效率。
   - 一个文件流为了提高效率套接了缓存流，最后套接了数据流以实现按基本数据类型的读取。
![](C:/NoteBook/pictures/232134814221067.png =591x)
   - 一个输出流，程序中的数据按数据类型写到数据输出流，再经过缓存最后由文件流写到外存的文件中。
![](C:/NoteBook/pictures/289645014239493.png =633x)

### 常用输入/输出流

|     抽象基类      | 节点流（文件流）                                    | 缓存流(处理流的一种)                                              |
| :--------------: | :------------------------------------------------ | :-------------------------------------------------------------- |
| **InputStream**  | **FileInputStream** read(byte[] buffer)           | **BufferedInputStream** read(byte[] buffer)                     |
| **OutputStream** | **FileOutputStream** write(byte[] buffer, 0, len) | **BufferedOutputStream** write(byte[] buffer, 0, len) / flush() |
|    **Reader**    | **FileReader** read(char[] cbuf)                  | **BufferedReader** read(char[] cbuf) / readLine()               |
|    **Writer**    | **FileWriter** write(char[] cbuf, 0, len)         | **BufferedWriter** writer(char[] cbuf, 0, len) / flush()        |

#### 文件流
   
**文件流是节点流，包括以下类:** 

- FileInputStream
- FileOutputStream
- FileReader
- FileWriter 

**FileInputStream类的构造方法**
   
- public FileInputStream(String name)
- public FileInputStream(File file)


##### 字符流

###### FileReader 读入的操作

- `read()`： 返回读入的一个字符；如果达到文件末尾，则返回-1
- `read(char[] cbuf)` 返回每次读入cbuf数组中的字符的个数，如果达到文件末尾，返回-1.
- 为了保证流一定可以执行关闭操作，需要使用`try-catch-finally`处理
- 读入的文件一定要存在，否则报：`java.io.FileNotFoundException`


```java
package com.zjk.java;

import org.junit.Test;

import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class FileReaderWriterTest {
    @Test
    public void test1() {
        //public void test1() throws IOException {
        //使用throws容易出现资源泄露的问题
        //即：throws在抛出异常对象后，并没有关闭流
        //应该使用try-catch-finally
        FileReader fr = null;

        try {
            //将Hello.txt的文件内容读入程序中，并输出到控制台
            //1.实例化File类的对象，指明要操作的文件
            File file = new File("Hello.txt"); //相对路径，相较于当前Module
            //System.out.println(file.getAbsolutePath());//G:\ideaProjects\JavaSerrior\JavaStudy009\Hello.txt

            //2.提供具体的流
            fr = new FileReader(file);

            //3.数据的读入
            //read() 返回读入的一个字符；如果达到文件末尾，则返回-1
            int data;
            while ((data = fr.read()) != -1) {
                System.out.print((char) data);
            }
//        int data = fr.read();
//        while(data != -1){
//            System.out.print((char) data);
//            data = fr.read();
//        }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                //4.流的关闭操作
                if (fr != null)
                    fr.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }

    @Test
    public void test2() {

        FileReader fileReader = null;

        try {
            //1.File类的实例化
            File file = new File("Hello.txt");
            //2.FileReader流的实例化
            fileReader = new FileReader(file);
            //3.读入的操作
            //read(char[] cbuf) 返回每次读入cbuf数组中的字符的个数，如果达到文件末尾，返回-1.
            char[] cbuffer = new char[6];
            int len;
            while ((len = fileReader.read(cbuffer)) != -1) {
                //方式2
                String str = new String(cbuffer,0,len);
                System.out.print(str);
                //错误
//                System.out.print(str);
                //方式1
//                for (int i = 0; i < len; i++) {
//                    System.out.print(cbuffer[i]);
//                }
                //错误：
//                for (int i = 0; i < cbuffer.length; i++) {
//                    System.out.print(cbuffer[i]);
//                }
                //test
                //Hello World !orld
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.资源的关闭
            try {
                if (fileReader != null) {
                    fileReader.close();
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```

###### FileWriter 写出的操作

- 从内存中写出数据到硬盘的文件里。
- File对应的硬盘中的文件如果不存在，则在输出的过程中，会自动创建此文件。
- File对应的硬盘中的文件如果存在
  - 如果流使用的构造器是：`FileWriter(file, false);`  或 `FileWriter(file)`，对原有文件执行覆盖。
  - 如果流使用的构造器是：`FileWriter(file, true)` ，则对原有文件执行追加操作。

```java
package com.zjk.java;

import org.junit.Test;

import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class FileWriterTest {
    @Test
    public void test1() {
        FileWriter fileWriter = null;

        try {
            //1.提供File类的对象，指明写出到的文件，对应的File可以不存在
            //FileWriter fileWriter = new FileWriter(file,false);
            //如果该文件不存在，则在输出的过程中，自动创建文件
            //如果该文件存在，则覆盖
            File file = new File("HelloWrite.txt");

            //2.操作FileWriter的对象，用于数据的写出
            fileWriter = new FileWriter(file);
            //FileWriter fileWriter = new FileWriter(file,false);
            //false : 表示不在文件后面追加，而是直接覆盖
            //true ：表示在文件后面追加

            //3.写出的操作
            fileWriter.write("I have a dream.");
            fileWriter.write("\nyou need to have a dream, too.");
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                //4.流资源的关闭
                fileWriter.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    @Test
    public void test2() {
        FileReader fileReader = null;
        FileWriter fileWriter = null;

        try {
            //1.创建File类的对象，指明读入和写入的文件
            File readFile = new File("Hello.txt");
            File writeFile = new File("HelloWriteFile.txt");
            //2.创建输入流和输出流
            fileReader = new FileReader(readFile);
            fileWriter = new FileWriter(writeFile);
            //3.数据的读入和写出操作
            char[] cbuf = new char[5];
            int len;
            while ((len = fileReader.read(cbuf)) != -1) {
                fileWriter.write(cbuf, 0, len);
                //错误
//                fileWriter.write(cbuf);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            //4.流资源的关闭
            try {
                if (fileReader != null)
                    fileReader.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }

            try {
                fileWriter.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }

    }
}
```


**通过文件字符流实现文件的复制**

1. 创建文件字符输入/输出流
  - `new FileReader("读文件路径")`;
  - `new FileWriter("写文件路径")`; 
2. 读写数据
  - `FileReader对象.read()`
  - `FileWriter对象.write()`; 
3. 关闭流
  - `FileReader对象.close()`
  - `FileWriter对象.close()`

```java
import java.io.*;

/**
* 通过文件字符流实现文件的复制
*/

public class Copy {
    public static void main(String[] args) throws IOException {

        //1.创建文件字符输入/输出流。
//        输入文件
        FileReader in = new FileReader("G:\\javaIOtest\\f.txt");
//        输出文件
        FileWriter out = new FileWriter("G:\\javaIOtest\\o.txt");

        int c;

        //2.读、写数据。
        while ((c = in.read()) != -1)
            out.write(c);

        //3.关闭流。
        in.close();
        out.close();
    }
}
```

##### 字节流

- 对于文本文件，使用字符流处理
   - .txt、.java、.c、.cpp
- 对于非文本文件，使用字节流处理
   - .jpg、.mp3、.mp4、.avi、.doc、.ppt…… 
- 对于文本文件的复制，字节流也是可以的
  - 但是非文本文件的复制，字符流仍然不可以 

使用字节流处理文本文件可能出现乱码。

```java
package com.zjk.java;

import org.junit.Test;

import java.io.*;

public class FileInputStreamTest {
    @Test
    public void test1() throws FileNotFoundException {
        FileInputStream fileInputStream = null;

        try {
            //1.文件
            File file = new File("th.jpg");
            //2.流
            fileInputStream = new FileInputStream(file);
            //3.读数据
            byte[] buffer = new byte[5];
            int len; //记录每次读取的字节个数
            while ((len = fileInputStream.read(buffer)) != -1) {
                String str = new String(buffer, 0, len);
                System.out.print(str);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                //4.流资源的关闭
                if (fileInputStream != null)
                    fileInputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    /*实现对文件的复制操作*/
    @Test
    public void test2() {
        FileInputStream fileInputStream = null;
        FileOutputStream fileOutputStream = null;

        try {
            File inputFile = new File("th.jpg");
            File outputFile = new File("copyTh.jpg");

            fileInputStream = new FileInputStream(inputFile);
            fileOutputStream = new FileOutputStream(outputFile);

            byte[] buff = new byte[5];
            int len;

            while ((len = fileInputStream.read(buff)) != -1) {
                fileOutputStream.write(buff, 0, len);
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (fileInputStream != null) {
                    fileInputStream.close();
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }

            try {
                if (fileOutputStream != null) {
                    fileOutputStream.close();
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    /*指定路径下文件的复制*/
    public void copyFile(String srcPath, String destPath) {
        FileInputStream fileInputStream = null;
        FileOutputStream fileOutputStream = null;

        try {
            File inputFile = new File(srcPath);
            File outputFile = new File(destPath);

            fileInputStream = new FileInputStream(inputFile);
            fileOutputStream = new FileOutputStream(outputFile);

            byte[] buff = new byte[1024];
            int len;

            while ((len = fileInputStream.read(buff)) != -1) {
                fileOutputStream.write(buff, 0, len);
            }
        } catch (Exception e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (fileInputStream != null) {
                    fileInputStream.close();
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }

            try {
                if (fileOutputStream != null) {
                    fileOutputStream.close();
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    @Test
    public void test4() {
        long start = System.currentTimeMillis();

        String src = "th.jpg";
        String dest = "copyTh2.jpg";
        copyFile(src, dest);

        long end = System.currentTimeMillis();

        System.out.println("花费的时间：" + (end - start));
    }
}
```

**通过文件字节流实现文件的复制**

1. 创建File对象
    - `new File("文件路径")`; 
2. 创建文件输入/输出字节流
   - `new FileInputStream(作为输入(读)文件的File对象)`;
   - `new FileOutputStream(作为输出(写)文件的File对象)`;
3. 读写文件流中的数据
   - `FileInputStream对象.read()` 
   - `FileOutputStream对象.write()`
4. 关闭字节流
   - `FileInputStream对象.close()` 
   - `FileOutputStream对象.close()`

```java
import java.io.*;

/**
* 通过文件字节流实现文件的复制
*/
public class CopyBytes {
    public static void main(String[] args) throws IOException {

        //1.创建两个File类对象。
//        输入文件 
        File inputFile = new File("G:\\javaIOtest\\f.txt");
//        输出文件 自动创建
        File outputFile = new File("G:\\javaIOtest\\o.txt"); 

        //2.创建文件输入/输出字节流。
        FileInputStream in = new FileInputStream(inputFile);
        FileOutputStream out = new FileOutputStream(outputFile);

        int c;
        //3.读写文件流中的数据。
        while ((c = in.read()) != -1){
            //c=in.read() 从输入流in读取一个整数赋给c
            //该赋值表达式的值就是c被赋的值
            out.write(c);
        }

        //4.关闭流。
        in.close();
        out.close();
    }
}
```

```java
import java.io.*;

/**
* 通过文件字节流实现文件的复制,
* 并将大写转为小写
*/

public class CopyBytes {
    public static void main(String[] args) throws IOException {

        //1.创建两个File类对象。
//        输入文件
        File inputFile = new File("G:\\javaIOtest\\f.txt");
//        输出文件 自动创建
        File outputFile = new File("G:\\javaIOtest\\o.txt");

        //2.创建文件输入/输出字节流。
        FileInputStream in = new FileInputStream(inputFile);
        FileOutputStream out = new FileOutputStream(outputFile);

        int c;
        //3.读写文件流中的数据。
        while ((c = in.read()) != -1){
            //c=in.read() 从输入流in读取一个整数(ASCII码)赋给c
            //该赋值表达式的值就是c被赋的值

            //将读取的转为小写
            char[] c1 = new char[1];
            c1[0] =(char)c;
            String s = new String(c1);
            char[] c2 = s.toLowerCase().toCharArray();
            c = c2[0];

            out.write(c);
        }

        //4.关闭流。
        in.close();
        out.close();
    }
}
```



#### 缓存流

- 是处理流的一种。
   - 缓存流在创建具体流时，需要给出一个InputStream/OutputStream类型的流作为前端流，
   - 处理流就是“套接”在已有的流的基础上的。
- 内部提供了一个缓冲区，并可以指明缓冲区的大小。
   - 提高流的读取和写入速度。
- 缓存流把数据从原始流成块读入或把数据累积到一个大数据块后再成批写出，通过减少系统资源的读写操作来加快程序的执行。

**缓存流包括以下类：**

- BufferedInputStream
- BufferedOutputStream
- BufferedReader
- BufferedWriter

- 注：BufferedOutputStream类和BufferedWriter类仅仅在缓冲区满或调用flush()方法时，才将数据写到目的地。 

**BufferedInputStream类的构造方法**

- public BufferedInputStream(InputStream in)
- public BufferedInputStream(InputStream in, int size)

**方法**

- flush() 刷新缓冲区

**流资源的关闭：**

- 要求：先关闭外层的流，再关闭内层的流
- 在关闭外层流的同时，内层流也会自动进行关闭。内层流的关闭可以省略
   

##### 字节流

- readLine() 读取一行的数据，不包括换行符 BufferedReader
- newLine() 提供换行的操作。 BufferedWriter

```java
package com.zjk.java;

import org.junit.Test;

import java.io.*;

public class BufferedTest2 {
    @Test
    public void test() {
        BufferedReader bufferedReader = null;
        BufferedWriter bufferedWriter = null;
        try {
            //流
            bufferedReader = new BufferedReader(new FileReader(new File("Hello.txt")));
            bufferedWriter = new BufferedWriter(new FileWriter(new File("HelloCopy.txt")));

            //读写操作
            //方式2
            String data;
            while ((data = bufferedReader.readLine()) != null) {
                //方法2
                bufferedWriter.write(data); //readLine()返回的，不包括换行符
                bufferedWriter.newLine(); //提供换行的操作
                //方法1
                //此时data中不包含换行符 \n
                bufferedWriter.write(data);
            }
            //方式1
//            char[] cbuf = new char[1024];
//            int len;
//            while ((len = bufferedReader.read(cbuf)) != -1) {
//                bufferedWriter.write(cbuf, 0, len);
//            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            //流资源的关闭
            try {
                if (bufferedReader != null)
                    bufferedReader.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }

            try {
                if (bufferedWriter != null)
                    bufferedWriter.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

##### 字符流

```java
package com.zjk.java;

import org.junit.Test;

import java.io.*;

public class BufferedTest2 {
    @Test
    public void test() {
        BufferedReader bufferedReader = null;
        BufferedWriter bufferedWriter = null;
        try {
            //流
            bufferedReader = new BufferedReader(new FileReader(new File("Hello.txt")));
            bufferedWriter = new BufferedWriter(new FileWriter(new File("HelloCopy.txt")));

            //读写操作
            char[] cbuf = new char[1024];
            int len;
            while ((len = bufferedReader.read(cbuf)) != -1) {
                bufferedWriter.write(cbuf, 0, len);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            //流资源的关闭
            try {
                if (bufferedReader != null)
                    bufferedReader.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }

            try {
                if (bufferedWriter != null)
                    bufferedWriter.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }

    }
}
```

#### 转换流

- 属于字符流
- 作用：转换流提供了在字节流和字符流之间的转换

**Java API提供了两个转换流：**

- `InputStreamReader`： 
   - `InputStreamReader(FileInputStream fis)`
   - `InputStreamReader(FileInputStream fis, String charsetName)` 
      - 参数2指明了字符集：具体使用哪个字符集，取决于该文件保存时使用的字符集 
   - 将InputStream转换为Reader
   - 将字节的输入流转换为字符的输入流
   - 解码： 字节/字节数组 --> 字符/字符数组
- `OutputStreamWriter`：
   - `OutputStreamWriter(FileOutputStream fos)` 
   - `OutputStreamWriter(FileOutputStream fos, String charsetName)`
     -  参数2指明了文件输出时使用的编码集
   - 将Writer转换为OutputStream
   - 将字符的输出流转换为字节的输出流
   - 编码：字符/字符数组 --> 字节/字节数组

 字节流中的数据都是字符时，转成字符流操作更高效。

很多时候我们使用转换流来处理文件乱码问题。实现编码和解码的功能。

**字符集**

- ASCII：美国标准信息交换码。
   - 用一个字节的7位可以表示。
   - 字符集基本都兼容ASCII
- ISO8859-1：拉丁码表。欧洲码表
   - 用一个字节的8位表示。
- GB2312：中国的中文编码表。
   - 最多两个字节编码所有字符
- GBK：中国的中文编码表升级，融合了更多的中文文字符号。
   - 最多两个字节编码
- Unicode：国际标准码，融合了目前人类使用的所有字符。
   - 为每个字符分配唯一的字符码。
   - 所有的文字都用两个字节来表示。
- UTF-8：变长的编码方式，可用1-4个字节来表示一个字符。

![](C:/NoteBook/pictures/190814900221150.png =760x)


```java
package com.zjk.java;

import org.junit.Test;

import java.io.*;

public class IOTest {
    @Test
    public void test1() {
        InputStreamReader isr = null;
        try {
            FileInputStream fis = new FileInputStream(new File("Hello.txt"));
            //new InputStreamReader(fis); 使用系统默认的字符集
//        InputStreamReader isr = new InputStreamReader(fis);
            //new InputStreamReader(fis,"UTF-8"); 使用指定的字符集
            //参数2指明了字符集：具体使用哪个字符集，取决于该文件保存时使用的字符集
            isr = new InputStreamReader(fis, "UTF-8");

            char[] cbuf = new char[20];
            int len;
            while ((len = isr.read(cbuf)) != -1) {
                for (int i = 0; i < len; i++) {
                    System.out.print(cbuf[i]);
                }
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (isr != null)
                    isr.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    @Test
    public void test2(){
        InputStreamReader isr = null;
        OutputStreamWriter osw = null;
        try {
            File file1 = new File("Hello.txt");
            File file2 = new File("HelloIOTest.txt");

            FileInputStream fis = new FileInputStream(file1);
            FileOutputStream fos = new FileOutputStream(file2);

            isr = new InputStreamReader(fis, "utf-8");
            osw = new OutputStreamWriter(fos, "gbk");

            char[] cbuf = new char[20];
            int len;
            while ((len = isr.read(cbuf)) != -1) {
                osw.write(cbuf, 0, len);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (isr != null)
                    isr.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (osw != null)
                    osw.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

#### 管道流

- 管道流可以实现线程间数据的直接传输。
  - 线程A可以通过它的输出管道发送数据，
  - 另一个线程B把它的输人管道接到A的输出管道上即可接收A发送的数据。

![](C:/NoteBook/pictures/396373115227360.png =595x)

- 一个管道由管道输出端（管道输出流）与管道输入端（管道输入流）连接而成。
- 管道的连接实际上是使管道的输入流指向管道的输出流，或管道的输出流也指向管道输入流，这样从管道的输入流就可以读取写入管道输出流的数据了。
- PipedReader//PipedInputStream实现管道的输入流，而PipedWriter/PipedOutputStream实现管道的输出流。

**管道流的创建**

- 管道流的创建是将管道输出流和管道输入流进行挂接。
- 基于管道类的构造方法，可以采取下列两种方式创建管道流：
1. 1
   - PipedInputStream pin =  new PipedInputStream();
   - PipedOutputStream pout =new PipedOutputStream(pin)
2. 2
   - PipedInputStream pin = new PipedInputstream();
   - PipedOutputStream pout = new PipedOutputStrea();
   - pin.connect(out)；或pout.connect(in);

- 管道流创建后，需要把它的输出流连接到一个线程的输出流，并且把它的输入流连接到另一个线程的输入流，才能利用该管道流实现这两个线程之间的数据交流。

**例**

```java
/*
一个单词处理程序。该程序从文件中读人一组单词，
先将每个单词逆序(reverse),
再将所有单词排序(sort),
然后将这些词逆序输出。
最后输出的单词列表能够押韵。
由如下3个程序组成。
·RhymingWords.java:包含main(),reverse()与sort()方法。
  在main()方法中调用reverse()与sort()方法对单词进行处理，并将处理结果输出显示。
·ReverseThread.java:包含执行逆序的线程类。
·SortThread.java:包含执行排序的线程类。
*/
```

#### 数据流  基本数据类型和String的串行化

**数据流包括以下类:**

- DataInputStream 
- DataOutputStream
  - 分别“套接”在 InputStream 和 OutputStream 子类的流上 
- 用于读取或写出基本数据类型的变量或字符串
它们允许按Java的基本数据类型读写流中的数据。为了方便地操作Java语言的基本数据类型和String的数据，可以使用数据流。

**方法:**

|  DataInputStream类提供的读取数据   |  DataOutputStream类相对应的写方法   |
| :-- | :-- |
|   byte readByte()  |   void writeByte(int v)  |
| boolean readBoolean()    |  void writeBoolean(Boolean v)   |
|   char readChar()  |   void writeChar(int v)  |
|    double readDouble() |  void writeDouble(double v)   |
|   float readFloat()  |   void writeFloat(float v)  |
|   int readInt()  |  void writeInt (int v)   |
|  long readLong()   |   void writeLong(long v)  |
|  short readshort()   |  void writeshort(int v)   |
|  String readUTF()  读取以UTF格式保存的字符串   |  void writeUTF(String str)  将字符串以UTF格式写出    |
| - |  void writeBytes(String s) |
| - | void writeChars(String s)  |

- 读取不同类型数据的顺序需要与保存不同类型的数据的顺序相同

```java
package com.zjk.java;

import org.junit.Test;

import java.io.*;

public class DataStreamTest {
    //将内存的字符串\基本数据类型的变量写到文件中
    @Test
    public void test() {
        DataOutputStream dataOutputStream = null;
        try {
            dataOutputStream = new DataOutputStream(new FileOutputStream("data.txt"));
            dataOutputStream.writeUTF("苏打水");
            dataOutputStream.flush(); //刷新操作，将内存中的数据写入文件
            dataOutputStream.writeBoolean(true);
            dataOutputStream.flush();
            dataOutputStream.writeInt(23);
            dataOutputStream.flush();
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                dataOutputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    //将文件中存储的基本数据类型变量和字符串读取到内存中，保存在变量中。
    @Test
    public void test2() {
        DataInputStream dataInputStream = null;
        try {
            dataInputStream = new DataInputStream(new FileInputStream("data.txt"));

            //读取不同类型数据的顺序需要与保存不同类型的数据的顺序相同
            //否则可能出错，或数据不对。
            String name = dataInputStream.readUTF();
            boolean sex = dataInputStream.readBoolean();
            int age = dataInputStream.readInt();

            System.out.println(name + ": " + age + ": " + sex);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                dataInputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }

    }
}
```

```java
import java.io.*;

public class DataIOTest {

    public static void main(String[] args) throws IOException {

        // 创建数据输出流，前端套接文件流并以invoice1.txt为输出目的地。
        DataOutputStream out = new DataOutputStream(new FileOutputStream("invoice1.txt"));
//        数据流作为过滤流，可以实现基本数据类型读写数据

        //定义要保存的数据数组。
        double[] prices = {19.99, 9.99, 15.99, 3.99, 4.99};
        int[] units = {12, 8, 13, 29, 50};
        String[] descs = {"Java T-shirt", "Java Mug", "Duke Juggling Dolls", "Java Pin", "Java Key Chain"};

        //将prices,unites以及descs中的数据以Tab键为分割符保存在文件中。
        for (int i = 0; i < prices.length; i++) {
            out.writeDouble(prices[i]);
            out.writeChar('\t');
            out.writeInt(units[i]);
            out.writeChar('\t');
            out.writeUTF(descs[i]);
            out.writeChar('\t');
        }
        out.close();

        // 创建数据输入流，将上面保存的文件再次打开并读取。
        DataInputStream in = new DataInputStream(new FileInputStream("invoice1.txt"));

        double price;
        int unit;
        String desc;
        double total = 0.0;

        for (int i = 0; i < prices.length; i++) {
            price = in.readDouble();
            in.readChar();       // 扔掉tab键
            unit = in.readInt();
            in.readChar();       // 扔掉tab键
            desc = in.readUTF();
            in.readChar();   // 扔掉tab键

            System.out.println("You've ordered " + unit + " units of " + desc + " at $" + price);
            total = total + unit * price;
        }

        System.out.println("For a TOTAL of: $" + total);
        in.close();
    }
}
```

#### 标准输入/输出

- Java中，标准输入是键盘，标准输出是显示器屏幕（更准确地说是加载Java程序的命令窗口)。Java程序使用字符界面与系统标准输入/输出间进行数据通信。
- 因为从键盘读入数据或向屏幕输出数据是十分常见的操作，每次操作都创建输入/输出流将影响系统的运行效率。
- 为此Java在System类中定义了与系统标准输入/输出相联系的3个流
   - System.in 标准输入流 默认从键盘输入
   - System.out 标准输出流 默认从控制台输出
   - System.err

**System类**

- System是Java中一个功能强大的类，利用它可以获得Java运行时的系统信息。
   - System类的所有变量和方法都是static，即通过类名System可以直接调用。
   - System.in，System.out与System.err就是System类的3个静态变量。
- System.in完整定义是`public static final InputStream in`，是标准输入流。
   - 这个流在程序运行时一直打开并准备好提供输入数据。该流一般对应于键盘输入。
- System.out完整定义是`public static final PrintStream out`，是标准输出流。
   - 这个流在程序运行时一直打开并准备好接收输出的数据。
   - 该流一般对应于屏幕。
- System.err完整定义是`public static final PrintStream err`，是标准错误输出流。
   - 这个流在程序运行时一直打开并准备好接收输出的数据。
   - 该流一般对应于屏幕并且用来显示错误消息或其他能够马上引起用户注意的信息。

**重定向输入/输出**

- System类的`setIn(InpuStream in) / setOut(PrintStream out)`重新指定输入和输出的流

##### System.in 标准输入

- Java的标准输人System.in是`InputStream类的对象`。
- 当程序中需要从键盘读入数据的时候， 默认从键盘输入
   - 只需System.in的`read()`方法即可。
   - 也可以在System.in上套接其他过滤流，这样可以使用更方便的方法从标准输人流上读取数据。

**在使用`System.in.read()`方法时需要注意以下几点。**

- 必须使用`try catch`对System.in.read()可能抛出的`IOException`类型的异常进行处理。
- 执行System.in.read()方法将从键盘缓冲区读入一个字节的数据，但返回的是**16位的整型值**，<mark>该整型值只有低位字节是真正输人的数据，高位字节全部是零</mark>
- System.in.read()方法的执行将使整个程序被挂起，直到用户从键盘输人数据才继续运行

##### System.out 标准输出

Java的标准输出System.out是`打印输出流PrintStream类的对象`。

##### 打印流

- 打印流：PrintStream和PrintWriter
- 提供了一系列重载的print()和println()方法，用于多种数据类型的输出
- PrintStream和PrintWriter的输出不会抛出IOException异常
- PrintStream和PrintWriter有自动flush功能
-  PrintStream 打印的所有字符都使用平台的默认字符编码转换为字节。
- 在需要写入字符而不是写入字节的情况下，应该使用 PrintWriter 类。
- System.out返回的是PrintStream的实例

**PrintStream是一种过滤流，其中定义了在屏幕上显示不同类型数据的方法print()和println()。**

**println()方法**

- println()方法向屏幕输出其参数指定的变量或对象，然后再换行，光标停留在屏幕下一行第一个字符的位置。
   - 如果该方法的参数为空，则输出一个空行。
- println()方法可输出多种不同类型的变量或对象，包括boolean、double、float、int、long,char、字符数组以及Object类型的对象。
   - 因为Object类是Java中所有类的父类，所以使用println()方法可以在屏幕上输出所有类的对象。

**print()方法**

- print()方法与println()方法相类似，也可以将不同类型的变量与对象输出到屏幕。
- 不同的是pint()方法输出后不换行，下次输出时将显示在同一行中。

**Juint单元测试不支持标准输入在控制台的输入 read-only**

```java
//从键盘输入字符串，要求将读取到的整行字符串转成大写输出。
//然后继续进行输入操作，直至当输入“e”或者“exit”时，退出程序。
package com.zjk.java;

import org.junit.Test;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class SystemIOtest {
    @Test
    public static void main(String[] args) {
        BufferedReader bfr = null;
        try {
            //方法1 使用Scanner类,调用next()返回一个字符串

            //方法2 System.in实现System.in --> 转换流 --> BufferedReader.readLine()
            InputStreamReader isr = new InputStreamReader(System.in);
            bfr = new BufferedReader(isr);

            while (true) {
                System.out.println("请输入");
                String data = bfr.readLine();
                if ("e".equals(data.toLowerCase()) || "exit".equals(data.toLowerCase())) {
                    System.out.println("程序结束");
                    break;
                }

                String upperCase = data.toUpperCase();
                System.out.println(upperCase);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                bfr.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

```java
import java.io.*;

public class StandardIO {

    public static void main(String[] args) {
        String s;

//        节点流InputStreamReader和过滤流BufferedReader的套接
        BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
//        System.in和System.out类似，是System类的一个对象，表示键盘

        System.out.println("Please input : ");

        //对流操作时，需要try-catch，因为容易出错
        try {
            s = in.readLine();
            while (!s.equals("exit")) {
                System.out.println("  read: " + s);
                s = in.readLine();
            }
            System.out.println("End of Inputting.");
            in.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

```java
package com.zjk.java;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.PrintStream;

public class PrintTest {

    public static void main(String[] args) {
        PrintStream ps = null;
        try {
            FileOutputStream fos = new FileOutputStream(new File("G:\\JavaIOTest\\o.txt"));
            // 创建打印输出流,设置为自动刷新模式(写入换行符或字节 '\n' 时都会刷新输出缓冲区)
            ps = new PrintStream(fos, true);
            if (ps != null) {// 把标准输出流(控制台输出)改成文件
                System.setOut(ps);
            }
            for (int i = 0; i <= 255; i++) { // 输出ASCII字符
                System.out.print((char) i);
                if (i % 50 == 0) { // 每50个数据一行
                    System.out.println(); // 换行
                }
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } finally {
            if (ps != null) {
                ps.close();
            }
        }
    }
}
```

#### 练习

##### 1.

分别使用节点流：FileInputStream、FileOutputStream和缓冲流：BufferedInputStream、BufferedOutputStream实现文本文件/图片/视频文件的复制。并比较二者在数据复制方面的效率

```java
package com.zjk.java;

import org.junit.Test;

import java.io.*;

public class Test1 {
    @Test
    public void test() {
        String src = "th.jpg";
        String dest = "thtest.jpg";

        long startF = System.currentTimeMillis();
        fileCopy(src, dest);
        long endF = System.currentTimeMillis();

        long startB = System.currentTimeMillis();
        fileCopy(src, dest);
        long endB = System.currentTimeMillis();

        System.out.println("节点流：" + (endF - startF));
        System.out.println("缓存流：" + (endB - startB));
    }

    public void bufferedCopy(String src, String dest) {
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;
        try {
            bis = new BufferedInputStream(new FileInputStream(new File(src)));
            bos = new BufferedOutputStream(new FileOutputStream(new File(dest)));

            byte[] buffer = new byte[1024];
            int len;
            while ((len = bis.read(buffer)) != -1) {
                bos.write(buffer, 0, len);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (bis != null)
                    bis.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (bos != null)
                    bos.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }

    }

    public void fileCopy(String src, String dest) {
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream(new File(src));
            fos = new FileOutputStream(new File(dest));

            byte[] buffer = new byte[1024];
            int len;
            while ((len = fis.read(buffer)) != -1) {
                fos.write(buffer, 0, len);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (fis != null)
                    fis.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (fos != null)
                    fos.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

##### 2.实现图片加密操作。

![](C:/NoteBook/pictures/527985022221148.png =341x)

```java
package com.zjk.java;

import org.junit.Test;

import java.io.*;

public class Test2 {
    @Test
    public void test() {
        String src = "th.jpg";
        String secretFile = "secretTh.jpg";
        String decryptionFile = "decryptionTh.jpg";

        secret(src, secretFile);
        decryption(secretFile, decryptionFile);
    }


    public void secret(String src, String dest) {
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;
        try {
            bis = new BufferedInputStream(new FileInputStream(new File(src)));
            bos = new BufferedOutputStream(new FileOutputStream(new File(dest)));

            byte[] buffer = new byte[20];
            int len;
            while ((len = bis.read(buffer)) != -1) {
                for (int i = 0; i < len; i++) {
                    bos.write(buffer[i] ^ 5);
                }
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (bis != null)
                    bis.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (bos != null)
                    bos.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    public void decryption(String src, String dest) {
        secret(src, dest);
    }
}
```

##### 3.获取文本上每个字符出现的次数

提示：遍历文本的每一个字符；字符及出现的次数保存在Map中；将Map中数据写入文件

```java
public class WordCount {
    @Test
    public void testWordCount() {
        FileReader fr = null;
        BufferedWriter bw = null;
        try {
            //1.创建Map集合
            Map<Character, Integer> map = new HashMap<Character, Integer>();

            //2.遍历每一个字符,每一个字符出现的次数放到map中
            fr = new FileReader("dbcp.txt");
            int c = 0;
            while ((c = fr.read()) != -1) {
                //int 还原 char
                char ch = (char) c;
                // 判断char是否在map中第一次出现
                if (map.get(ch) == null) {
                    map.put(ch, 1);
                } else {
                    map.put(ch, map.get(ch) + 1);
                }
            }

            //3.把map中数据存在文件count.txt
            //3.1 创建Writer
            bw = new BufferedWriter(new FileWriter("wordcount.txt"));

            //3.2 遍历map,再写入数据
            Set<Map.Entry<Character, Integer>> entrySet = map.entrySet();
            for (Map.Entry<Character, Integer> entry : entrySet) {
                switch (entry.getKey()) {
                    case ' ':
                        bw.write("空格=" + entry.getValue());
                        break;
                    case '\t'://\t表示tab 键字符
                        bw.write("tab键=" + entry.getValue());
                        break;
                    case '\r'://
                        bw.write("回车=" + entry.getValue());
                        break;
                    case '\n'://
                        bw.write("换行=" + entry.getValue());
                        break;
                    default:
                        bw.write(entry.getKey() + "=" + entry.getValue());
                        break;
                }
                bw.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.关流
            if (fr != null) {
                try {
                    fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
            if (bw != null) {
                try {
                    bw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }

            }
        }

    }
}
```

## 文件 File类

- 声明在java.io包下  

**路径分隔符**

在java中需要换成：`/`或`\\`

- Windows或DOS系统：\  --> `\\`
- Unix或URL：/  --> `/`

**路径**

- 相对路径：相较于某个路径下，指明的路径
- 绝对路径：包含盘符在内的文件或文件的目录的路径

- 在IDEA中
  - 如果使用main，则相对路径在当前工程下
  - 如果使用JUnit，则相对路径在当前Module下(即与src文件同级目录，而不是包内的)
- 在Eclipse中
  - 相对路径都在当前工程下 

**Java中的文件类File是外存文件和目录的抽象表示。**

- File类用来操作文件和获得文件的信息，但不提供对文件数据读取的方法，这些方法由文件流提供。
- 通过File类的方法，可以得到文件或目录的描述信息，包括文件名、路径，可读写性，长度等，还可以生成新的目录，临时文件，改变文件名，删除文件，列出一个目录中所有的文件或满足某种模式的文件等。

### 创建File对象

- 可以使用File类的构造方法来创建File类的对象(在Java中目录也当作文件)，
   - 因此File对象可以表示一个磁盘文件，也可以表示某个目录。

**File类常用的构造方法如下**

- `public File(String pathname)`
   - 参数pathname指定新创建的File对象对应的磁盘文件或目录名及其路径名。
   - pathname可以是绝对路径，如，“d:\works\source\myfile..txt”,
   - 也可以是相对路径，如，“source\myfile.txt”,表示当前目录的source目录下的myfile..txt。
      - 为了保证程序的可移植性，应该尽量使用相对路径。例如：`File myFile =  new File("myfile.txt");`
- `public File(String parent,String child)`
   - 参数parent指定了文件或目录的父目录的绝对或相对路径
   - 参数child指定了文件或目录名。
   - 将路径与名称分开的好处是相同路径的文件或目录可共享同一个路径字符串，便于管理和修改。
- `public File(File parent,String child)`
   - 参数parent是已经存在的代表文件或父目录的File类对象，
   - 参数child表示文件或目录名。
   
```java
public class FileTest {
    public static void main(String[] args) {
        //构造器1：new File(pathname)
        //相对路径：相较于某个路径下，指明的路径
        //即：相对于当前的module
        File file1 = new File("Hello.txt");
        //绝对路径：包含盘符在内的文件或文件的目录的路径
        File file2 = new File("G:\\ideaProjects\\JavaSerrior\\JavaStudy009\\src\\com\\zjk\\java\\Hello.txt");

        //构造器2：new File(parentPath, childPath);
        File file3 = new File("G:\\ideaProjects","JavaSerrior");

        //构造器3：new File(parentPath, childFile)
        File file4 = new File("G:\\javaIOTest","f.txt");        
    }
}
```

### 操作File对象

- 在创建了File对象后，就可以使用下列方法获取文件信息或进行其他操作。
- 并没有涉及到文件内容的操作：需要IO流
- File类的对象常会作为此参数，传递到流的构造器中，指明读取或写入的“终点”

**文件名的操作**

- `public String getName()` 获取文件名
- `public String getParent()` 返回此抽象路径名的父目录的路径名字符串，如果此路径名未命名为父目录，则返回null。
- `public String getPath()` 将此抽象路径名转换为路径名字符串
- `public String getAbsolutePath()` 获取文件的绝对路径 （返回此抽象路径名的绝对形式。）
- `public boolean renameTo(File dest)` 将文件重命名为dest所对应的文件名
  - 要想保证返回true，需要file在硬盘中是存在的，且dest不能在硬盘中存在(即不能存在同名的文件)
   
**文件信息测试**

- `public boolean isAbsolute()` 测试这个抽象路径名是否是绝对的。
- `public boolean canRead()` 文件是否可读
- `public boolean canWrite()` 文件是否可写
- `public boolean exists()` 文件是否存在（文件名）
- `public boolean isDirectory()` 是否是文件夹
- `public boolean isFile()` 是否是文件
- `public boolean isHidden()` 判断文件是否隐藏

**获取文件一般信息**

- `public long length()` 返回由此抽象路径名表示的文件的长度。
- `public long lastModified()` 返回此抽象路径名表示的文件上次修改的时间。
   - 返回的是时间戳 
    
**目录操作**

需要对应的File对象是文件目录；否则空指针。

- `public String[] list()` 将目录中所有文件名保存在字符数组中返回
- `public File[] listFiles()` 获取指定目录下的所有文件或者文件目录的File数组

**创建**

如果在创建文件或文件目录时，没有写盘符路径，那么默认在项目路径下。

- `public boolean mkdir()` 创建由此抽象路径名命名的目录。
   - 如果此文件目录存在，则不创建。
   - 如果此文件目录的上层目录不存在，也不创建。
- `public boolean mkdirs()` 创建由此抽象路径名命名的目录，包括任何必需但不存在的父目录
   - 如果上层文件目录不存在，一并创建 
- `public boolean createNewFile()` 创建文件，若文件存在，则不创建，返回false
  
**删除**

- `public boolean delete()` 删除文件或目录
  - java中的删除不走回收站
  - 要删除一个文件目录，则文件目录内不能包含文件或文件目录。 

```java
@Test
public void test1() {
    File file1 = new File("Hello.txt");
    File file2 = new File("G:\\ideaProjects\\JavaSerrior\\JavaStudy009\\src\\com\\zjk\\java\\Hello.txt");

    System.out.println(file1.getAbsolutePath()); //G:\ideaProjects\JavaSerrior\JavaStudy009\Hello.txt
    System.out.println(file1.getPath()); //Hello.txt
    System.out.println(file1.getName()); //Hello.txt
    //找不到该文件，文件不存在 返回默认值
    System.out.println(file1.getParent()); //null
    System.out.println(file1.length()); //0
    System.out.println(file1.lastModified()); //0

    System.out.println(file2.getAbsolutePath()); //G:\ideaProjects\JavaSerrior\JavaStudy009\Hello.txt
    System.out.println(file2.getPath()); //G:\ideaProjects\JavaSerrior\JavaStudy009\src\com\zjk\java\Hello.txt
    System.out.println(file2.getName()); //Hello.txt
    System.out.println(file2.getParent()); //G:\ideaProjects\JavaSerrior\JavaStudy009\src\com\zjk\java
    System.out.println(file2.length()); //19
    System.out.println(file2.lastModified()); //1667742367361
}

@Test
public void test2() {
    File file = new File("G:\\javaIOTest");
    //list()和listFiles() 需要相应的File对象是文件目录
    //否则:java.lang.NullPointerException

    //list() 返回文件的相对路径名 如：outFile2.txt
    String[] list = file.list();
    for (String str : list) {
        System.out.println(str);
    }

    //listFiles() 返回文件的绝对路径名 如：G:\javaIOTest\date.ser
    File[] files = file.listFiles();
    for (File f : files) {
        System.out.println(f);
    }
}

@Test
public void test3(){
    File file = new File("G:\\javaIOTest\\f.txt");
    File refile = new File("G:\\javaIOTest\\t.txt");
    //renameTo()
    //如果要改名的位置上有同名的文件，则重命名失败
    System.out.println(file.renameTo(refile));
}

@Test
public void test4(){
    File file = new File("G:\\javaIOTest\\f.txt");

    System.out.println(file.isFile()); //true
    System.out.println(file.isDirectory()); //false
    System.out.println(file.exists()); //true
    System.out.println(file.canRead()); //true
    System.out.println(file.canWrite()); //true
    System.out.println(file.isHidden()); //false

    File dir = new File("G:\\javaIOTest");

    System.out.println(dir.isFile()); //false
    System.out.println(dir.isDirectory()); //true
    System.out.println(dir.exists()); //true
    System.out.println(dir.canRead()); //true
    System.out.println(dir.canWrite()); //true
    System.out.println(dir.isHidden()); //false
}

@Test
public void test6() throws IOException {
    //文件的创建和删除
    File file = new File("G:\\javaIOTest\\f.txt");
    if(!file.exists()){
        //创建文件
        file.createNewFile();
        System.out.println("创建成功");
    }else{
        //删除文件
        file.delete();
        System.out.println("删除成功");
    }

    //文件目录的创建和删除
    File dir = new File("G:\\javaIOTest\\javaIODir");
    if(!dir.exists()){
        //创建文件目录
        dir.mkdirs();
        System.out.println(dir.getAbsolutePath());
    }else{
        //删除文件目录
        dir.delete();
        System.out.println("删除成功");
    }
}
```

```java
public class RenameFile {

    //显示文件基本信息。
    //File f = new File("G:/javaIOTest/f.txt");
    private static void fileData(File f) {
        System.out.println(
                "Absolute path: " + f.getAbsolutePath() + //文件绝对路径 G:\javaIOTest\f.txt
                        "\n Can read: " + f.canRead() + //是否可写 true
                        "\n Can write: " + f.canWrite() + //是否可读 true
                        "\n getName: " + f.getName() + //获取文件名（没有路径）f.txt
                        "\n getParent: " + f.getParent() + //获取文件的父目录 G:\javaIOTest
                        "\n getPath: " + f.getPath() + //获取文件完整路径 G:\java\IOTest\f.txt
                        "\n length: " + f.length() + //文件内容的长度
                        "\n lastModified: " + new Date(f.lastModified()));
        if (f.isFile())
            System.out.println("It's a file");
        else if (f.isDirectory())
            System.out.println("It's a directory");
    }

    //将命令行第一个参数是原来的文件名，第二个参数是新文件名。
    public static void main(String[] args) {
        args = new String[3];
        args[0] = "G:/javaIOTest/f.txt";
        args[1] = "G:/javaIOTest/new.txt";

        File old = new File(args[0]);
        File rname = new File(args[1]);

        System.out.println("The original file's information:");
        fileData(old);
        old.renameTo(rname); //文件重命名。
        System.out.println("\n The file information after rename:");
        fileData(rname);
        if (!old.exists()) {  //文件是否存在
            System.out.println("\n The original file never exists.");
        }
    }
}
```

### 练习

#### 1.

利用File构造器，new 一个文件目录file

1)在其中创建多个文件和目录

2)编写方法，实现删除file中指定文件的操作

```java
import java.io.File;
import java.io.IOException;

public class FileTest1 {
    public static void main(String[] args) throws IOException {
        File dir = new File("G:\\javaIOTest");

        if(!dir.exists()){
            dir.mkdirs();
        }

        File file1 = new File(file"file1.txt");
        System.out.println(file1.createNewFile());
        File dir2 = new File("G:\\javaIOTest\\IOdir");
        System.out.println(dir2.mkdir());

        FileTest1 test = new FileTest1();
        test.deleteFile("G:\\javaIOTest\\IOdir");
    }

    public void deleteFile(String path) {
        File deleteFile = new File(path);
        if (deleteFile.exists()) {
            deleteFile.delete();
            System.out.println("删除成功");
        } else {
            System.out.println("该文件不存在");
        }
    }
    public void deleteFile(File file){
        if(file.exists()){
            file.delete();
            System.out.println("删除成功");
        }else{
            System.out.println("该文件不存在");
        }
    }
}
```

#### 2.

判断指定目录下是否有后缀名为.jpg的文件，如果有，就输出该文件名称

```java
import java.io.File;
import java.util.ArrayList;
import java.util.Collection;

public class FileTest2 {
    public static void main(String[] args) {
        File dir = new File("G:\\javaIOTest\\javaIO");
        System.out.println(checkJpg(dir));
    }

    public static Collection<String> checkJpg(File dir) {
        if (dir.exists()) {
            String[] fileNames = dir.list();
            Collection collection = new ArrayList();

            for (String str : fileNames) {
                if (str.endsWith(".jpg")) {
                    collection.add(str);
                }
            }

            if (!collection.isEmpty()) {
                return collection;
            } else {
                System.out.println("不存在后缀为.jpg的文件");
                return null;
            }

        } else {
            throw new RuntimeException("该文件目录不存在");
        }
    }
}
```

#### 3.

遍历指定目录所有文件名称，包括子文件目录中的文件。

拓展1：并计算指定目录占用空间的大小

拓展2：删除指定文件目录及其下的所有文件

```java
import java.io.File;

public class FileTest3 {
    public static void main(String[] args) {
        File dir = new File("G:\\javaIOTest");
        toList(dir);
    }

    public static void toList(File dir) {
        if (!dir.exists())
            throw new RuntimeException("文件目录不存在");

        File[] fileList = dir.listFiles();

        for (int i = 0; i < fileList.length; i++) {
            System.out.println(fileList[i]);
            if(fileList[i].isDirectory()){
                forFiles(fileList[i]);
            }
        }

    }

    public static void forFiles(File dir){
        System.out.println("----进入文件目录：" + dir);
        File[] list = dir.listFiles();
        for(int i = 0;i < list.length;i++){
            System.out.println(list[i]);
            if(list[i].isDirectory()){
                forFiles(list[i]);
            }
        }
        System.out.println("----离开文件目录：" + dir);
    }
}
```

```java
import java.io.File;

public class FileTest3 {
    public static void main(String[] args) {
        File dir = new File("G:\\javaIO");
        toList(dir);
    }

    public static void toList(File dir) {
        if (!dir.exists())
            throw new RuntimeException("文件目录不存在");

        File[] list = dir.listFiles();

        for (int i = 0; i < list.length; i++) {
            if (list[i].isDirectory()) {
                forFiles(list[i]);
                deleteFile(list[i]);
            }else{
                deleteFile(list[i]);
            }
        }
    }

    public static void forFiles(File dir) {
        System.out.println("----进入文件目录：" + dir);
        File[] list = dir.listFiles();
        for (int i = 0; i < list.length; i++) {

            if (list[i].isDirectory()) {
                forFiles(list[i]);
                deleteFile(list[i]);
            } else {
                deleteFile(list[i]);
            }
        }
        System.out.println("----离开文件目录：" + dir);
    }

    public static void deleteFile(File file) {
        if (file.delete()) {
            System.out.println(file + "已删除");
        } else {
            System.out.println(file + "删除失败");
        }
    }
}
```

## RandomAccessFile类 随机存取文件

- 随机存取文件类RandomAccessFile，可以实现对文件的随机读写操作，
- RandomAccessFile类不是从InputStream类或OutputStream类派生的，而是继承Object类
![](C:/NoteBook/pictures/350645010221143.png =201x)

- 既可以作为输入流也可以作为输出流。
- RandomAccessFile类实现了DataInput和DataOutput接口，所以对于支持这两个接口的过滤器将适用于RandomAccessFile。

### 随机存取文件的操作

**RandomAccessFile类提供两个构造方法：**

- `public RandomAccessFile(String name,String mode) throws FileNotFoundException`
- `public RandomAccessFile(File file,String mode) throws FileNotFoundException`

     - 参数mode有4种取值：""
     - r  —— 以只读方式打开文件；
        - 如果模式为只读r。则不会创建文件，而是会去读取一个已经存在的文件，如果读取的文件不存在则会出现异常。 
     - rw —— 以读写方式打开文件，
        - 则用一个RandomAccessFile对象就可以同时进行读写两种操作；
        - 如果模式为rw读写。如果文件不存在则会去创建文件，如果存在则不会创建。
     - rwd —— 以读写方式打开文件，并且要求对文件内容的更新要同步地写到底层存储设备；
       - "rwd"模式可用于减少执行的I / O操作数。 使用"rwd"只需要更新要写入存储的文件内容 
     - rws —— 与rwd基本相同，只是还可以更新文件的元数据(MetaData)。
       - 使用"rws"需要更新要写入的文件的内容及其元数据，这通常需要至少一个低级I / O操作。

**作为输入流 RandomAccessFile类的读方法**

- read()
- readBoolean()
- readChar()
- readInt()
- readLong()
- readFloat()
- readDouble()
- readLine()
  - 从当前位置开始，到第一个\n'为止，读取一行文本，它将返回一个String对象。 
- readUTF()
- readFully(byte[] b)
    - 从此文件读取 b.length字节到字节数组，从当前文件指针开始。
- void readFully(byte[] b, int off, int len)
    - 从此文件中读取 len个字节到字节数组，从当前文件指针开始。

**作为输出流 RandomAccessFile类包含的写方法**

- write()
- writeBoolean()
- writeChar()
- writeUTF()
- writeInt()
- writeLong()
- writeFloat()
- writeDouble()

**RandomAccessFile作为输出流时 (至少是rw模式时)**

- 如果写出到的文件不存在，则在执行过程中自动创建
- 如果写出到的文件存在，则会对原有文件内容进行覆盖（默认情况下，从头覆盖）

**其他**

- `getChannel()` 
   - 返回与此文件关联的唯一的FileChannel对象。
- `getFD()`
   - 返回与此流关联的不透明文件描述符对象。

#### 文件指针的操作

- 文件指针是指以字节为单位的相对于文件开头的偏移量，是下次读写的起点。

**文件指针的运行规律**

- RandomAceessFile对象的文件指针位于文件的开头处；
- 操作后，文件位置指针都相应后移读写的字节数。

**文件指针操作方法**

- `long getFilePointer()`:
   - 返回当前文件指针，即从文件开头算起的绝对位置。
- `void seek(long pos)`
   - 将文件指针定位到指定位置。
   - 参数pos是相对于文件开头的绝对偏移量。
- `long length()`:
   - 返回文件长度。
   - 可以通过将文件长度与文件指针相比较，判断是否读到了文件尾。
- `int skipBytes(int n)`
   - 从当前位置开始跳过n个字节，返回值表示实际跳过的字节数。

**可通过相关的操作，实现RandomAccessFile插入数据的效果**

```java
package com.zjk.java2;

import org.junit.Test;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.RandomAccessFile;

public class RandomAccessFileTest {
    @Test
    public void test1() {
        RandomAccessFile randomAccessFile1 = null;
        RandomAccessFile randomAccessFile2 = null;
        try {
            randomAccessFile1 = new RandomAccessFile(new File("Hello.txt"), "r");
            randomAccessFile2 = new RandomAccessFile(new File("HelloRandomAccessFileTest.txt"), "rw");

            byte[] buf = new byte[20];
            int len;
            while ((len = randomAccessFile1.read(buf)) != -1) {
                randomAccessFile2.write(buf, 0, len);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (randomAccessFile1 != null)
                    randomAccessFile1.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (randomAccessFile2 != null)
                    randomAccessFile2.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    @Test
    public void test2() {
        RandomAccessFile randomAccessFile = null;
        try {
            randomAccessFile = new RandomAccessFile(new File("Hello.txt"), "rw");

            //将指针调到下标为3的位置。
            randomAccessFile.seek(3);

            //RandomAccessFile作为输出流时 rw
            //如果写出到的文件不存在，则在执行过程中自动创建
            //如果写出到的文件存在，则会对原有文件内容进行覆盖（默认情况下，从头覆盖）
            randomAccessFile.write("take a test".getBytes());
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                randomAccessFile.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    /*使用RandomAccessFile实现数据的插入操作*/
    @Test
    public void test3() throws IOException {
        RandomAccessFile randomAccessFile = null;
        try {
            randomAccessFile = new RandomAccessFile("Hello.txt", "rw");

            //将指针调到下标3的位置
            randomAccessFile.seek(3);

            //保存指针下标3以后的所有数据到StringBuilder中
            StringBuilder stringBuilder = new StringBuilder((int) new File("Hello.txt").length());
            byte[] buffer = new byte[20];
            int len;
            while ((len = randomAccessFile.read(buffer)) != -1) {
                stringBuilder.append(new String(buffer, 0, len));
            }

            //调回指针到下标3，并写入数据
            randomAccessFile.seek(3);
            randomAccessFile.write("juts for a test".getBytes());

            //将StringBuilder中的数据从当前指针的位置插入
            randomAccessFile.write(stringBuilder.toString().getBytes());
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                randomAccessFile.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

```java
public class RandomAccessTest {

    public static void main(String args[]) throws Exception {
        long filePoint = 0;
        String s;

        RandomAccessFile file = new RandomAccessFile("G:\\ideaProjects\\JavaSerrior\\JavaStudy007\\src\\com\\zjk\\java2\\RandomAccessTest.java", "r");
        long fileLength = file.length();

        while (filePoint < fileLength) {
            s = file.readLine();
            //作为文本文件，每一行的行末有（看不见的）回车符\n和换行符\a
            System.out.println(s);
            filePoint = file.getFilePointer();
        }

        file.seek(7); //移动文件指针，实现随机存取
        System.out.println(file.readLine());

        file.close();
    }
}
```

## 对象流 对象的串行化

### 串行化概念和目的

- 将java程序中的对象保存在外存中，称为对象永久化。

**对象流 ObjectInputStream和ObjectOutputStream**

- 用于存储和读取基本数据类型数据或对象的处理流。
- 可以把Java中的对象写入到数据源中，也能把对象从数据源中还原回来。
   - 序列化：用ObjectOutputStream类保存基本类型数据或对象的机制
   - 反序列化：用ObjectInputStream类读取基本类型数据或对象的机制
- ObjectOutputStream和ObjectInputStream<mark>不能序列化static和transient修饰</mark>的成员变量
   - `static关键字` 属于类，反序列化时会被变成默认值。
   - `transient关键字` 不允许序列化

**对象序列化机制**

- 对象序列化机制允许把内存中的Java对象转换成平台无关的二进制流，从而允许把这种二进制流持久地保存在磁盘上，或通过网络将这种二进制流传输到另一个网络节点。
    - 当其它程序获取了这种二进制流，就可以恢复成原来的Java对象
- 序列化的好处在于可将任何实现了Serializable接口的对象转化为字节数据，使其在保存和传输时可被还原
- 序列化是 RMI（Remote Method Invoke – 远程方法调用）过程的参数和返回值都必须实现的机制，而 RMI 是 JavaEE 的基础。
    - 因此序列化机制是JavaEE 平台的基础

### 使用对象流序列化对象

**对象序列化需要实现的接口**

- 如果需要让某个对象支持序列化机制，则必须让对象所属的类及其内部的成员属性是可序列化的，
   - 基本数据类型，String都是可序列化的  
   - 如果内部成员是自定义类，则也需要实现Serializable接口。
- 为了让某个类是可序列化的，该类必须实现如下两个接口之一。否则，会抛出`NotSerializableException`异常
   - `Serializable`
   - `Externalizable`

#### Serializable接口 标识接口

**serialVersionUID**

- 凡是实现Serializable接口的类都有一个表示序列化版本标识符的静态变量：
   - `private static final long serialVersionUID`;
   - 自定义为： `public static final long serialVersionUID = 值L;`
- `serialVersionUID`用来表明类的不同版本间的兼容性。
  - 其目的是以序列化对象进行版本控制，有关各版本反序列化时是否兼容。
  - 如果类没有显示定义这个静态常量，它的值是Java运行时环境根据类的内部细节自动生成的。
     - 若类的实例变量做了修改，serialVersionUID 可能发生变化。故建议，显式声明。
- 简单来说，Java的序列化机制是通过在运行时判断类的serialVersionUID来验证版本一致性的。
- 在进行反序列化时，JVM会把传来的字节流中的serialVersionUID与本地相应实体类的serialVersionUID进行比较，如果相同就认为是一致的，可以进行反序列化，否则就会出现序列化版本不一致的异常。(InvalidCastException)

![](C:/NoteBook/pictures/482332600239578.png =540x)

#### 序列化和反序列化

**序列化**

- 若某个类实现了 Serializable 接口，该类的对象就是可序列化的：
1. 创建一个 `ObjectOutputStream`
2. 调用 ObjectOutputStream 对象的 `writeObject(对象)` 方法输出可序列化对象
    - 注意写出一次，操作flush()一次

**反序列化 ObjectOutputStream**

- 将磁盘文件中的对象还原为内存的一个java对象 
1. 创建一个 `ObjectInputStream`
2. 调用 `readObject()` 方法读取流中的对象
- 强调：如果某个类的属性不是基本数据类型或 String 类型，而是另一个引用类型，那么这个引用类型必须是可序列化的，否则拥有该类型的Field 的类也不能序列化

```java
package com.zjk.java2;

import org.junit.Test;

import java.io.*;

public class ObjectInputStreamTest {
    @Test
    public void test() {

        ObjectOutputStream oos = null;
        try {
            oos = new ObjectOutputStream(new FileOutputStream("object.myobj"));

            oos.writeObject(new String("时代"));

            //刷新操作
            oos.flush();
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                oos.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    @Test
    public void test2() throws IOException, ClassNotFoundException {
        ObjectInputStream ois = null;
        try {
            ois = new ObjectInputStream(new FileInputStream("object.myobj"));

            Object obj = ois.readObject();

            String str = (String) obj;

            System.out.println(str);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                ois.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

```java
package com.zjk.java2;

import org.junit.Test;

import java.io.*;

public class ObjectIOTest {
    @Test
    public void test1() {
        ObjectOutputStream oos = null;
        try {
            oos = new ObjectOutputStream(new FileOutputStream("Person01.person"));

            oos.writeObject(new Person("Jac", 20));

            oos.flush();
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                oos.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    @Test
    public void test2() {
        ObjectInputStream ois = null;
        try {
            ois = new ObjectInputStream(new FileInputStream("Person01.person"));

            Person p1 = (Person) ois.readObject();

            System.out.println(p1);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                ois.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }
}


package com.zjk.java2;

import java.io.Serializable;

//实现Serializable接口
public class Person implements Serializable {
    //当前类提供一个全局常量：serialVersionUID
    //建议自定义值
    public static final long serialVersionUID = 1054860443L;
    private String name;
    private int age;

    public Person() {
    }

    public Person(String name, int age) {
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

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

```java
public class UnSerializeDate {

    Date date = null;

    UnSerializeDate() {

        try {
            FileInputStream fileInputStream = new FileInputStream("G:/javaIOTest/date.ser");
            ObjectInputStream objectInputStream = new ObjectInputStream(fileInputStream);
            date = (Date) objectInputStream.readObject();
            fileInputStream.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    public static void main(String args[]) {
        UnSerializeDate unSerializeDate = new UnSerializeDate();
        System.out.println("The date read is :" + unSerializeDate.date.toString());
    }
}
```

## Java NIO

Java NIO (New IO，Non-Blocking IO)是从Java 1.4版本开始引入的一套新的IO API，可以替代标准的Java IO API。

- NIO支持面向缓冲区的(IO是面向流的)、基于通道的IO操作。更加高效

**Java API中提供了两套NIO**

- 标准输入输出NIO
- 网络编程NIO。

![](C:/NoteBook/pictures/73665915239579.png =531x)

**NIO.2**

随着 JDK 7 的发布，Java对NIO进行了极大的扩展，增强了对文件处理和文件系统特性的支持。

**Path、Paths和Files核心API**

### Path接口

- NIO. 2引入了Path接口，代表一个平台无关的平台路径，描述了目录结构中文件的位置。
- Path可以看成是File类的升级版本，实际引用的资源也可以不存在。
  - ![](C:/NoteBook/pictures/355120216227446.png =305x)
  
**构造器**

- static Path get(String first, String … more) : 用于将多个字符串串连成路径
- static Path get(URI uri): 返回指定uri对应的Path路径

**Path 常用方法**

- String toString() ： 返回调用 Path 对象的字符串表示形式
- boolean startsWith(String path) : 判断是否以 path 路径开始
- boolean endsWith(String path) : 判断是否以 path 路径结束
- boolean isAbsolute() : 判断是否是绝对路径
- Path getParent() ：返回Path对象包含整个路径，不包含 Path 对象指定的文件路径
- Path getRoot() ：返回调用 Path 对象的根路径
- Path getFileName() : 返回与调用 Path 对象关联的文件名
- int getNameCount() : 返回Path 根目录后面元素的数量
- Path getName(int idx) : 返回指定索引位置 idx 的路径名称
- `Path toAbsolutePath()` : 作为绝对路径返回调用 Path 对象
- Path resolve(Path p) :合并两个路径，返回合并后的路径对应的Path对象
- `File toFile()`: 将Path转化为File类的对象

### Files 类

- java.nio.file.Files 用于操作文件或目录的工具类。

**常用方法**

- Path copy(Path src, Path dest, CopyOption … how) : 文件的复制
- `Path createDirectory(Path path, FileAttribute<?> … attr)` : 创建一个目录
- `Path createFile(Path path, FileAttribute<?> … arr)` : 创建一个文件
- void delete(Path path) : 删除一个文件/目录，如果不存在，执行报错
- `void deleteIfExists(Path path)` : Path对应的文件/目录如果存在，执行删除
- Path move(Path src, Path dest, CopyOption…how) : 将 src 移动到 dest 位置
- `long size(Path path)` : 返回 path 指定文件的大小

**Files常用方法：用于判断**

- boolean exists(Path path, LinkOption … opts) : 判断文件是否存在
- boolean isDirectory(Path path, LinkOption … opts) : 判断是否是目录
- boolean isRegularFile(Path path, LinkOption … opts) : 判断是否是文件
- boolean isHidden(Path path) : 判断是否是隐藏文件
- boolean isReadable(Path path) : 判断文件是否可读
- boolean isWritable(Path path) : 判断文件是否可写 
- boolean notExists(Path path, LinkOption … opts) : 判断文件是否不存在

**Files常用方法：用于操作内容**

- SeekableByteChannel newByteChannel(Path path, OpenOption…how) : 获取与指定文件的连接，how 指定打开方式。
- DirectoryStream<Path> newDirectoryStream(Path path) : 打开 path 指定的目录
- InputStream newInputStream(Path path, OpenOption…how):获取 InputStream 对象
- OutputStream newOutputStream(Path path, OpenOption…how) : 获取 OutputStream 对象

# 网络编程

## 概要

Java提供的网络类库，可以实现无痛的网络连接，联网的底层细节被隐藏在 Java 的本机安装系统里，由 JVM 进行控制。并且 Java 实现了一个跨平台的网络库，程序员面对的是一个统一的网络编程环境。

**计算机网络：**

把分布在不同地理区域的计算机与专门的外部设备用通信线路互连成一个规模大、功能强的网络系统，从而使众多的计算机可以方便地互相传递信息、共享硬件、软件、数据信息等资源。

**网络编程的目的：**

直接或间接地通过网络协议与其它计算机实现数据交换，进行通讯。

**网络编程中有两个主要的问题：**

- 如何准确地定位网络上一台或多台主机；定位主机上的特定的应用
- 找到主机后如何可靠高效地进行数据传输

**网络通信要素**

- 通信双方地址
   - IP
   - 端口号
- 一定的规则（即：网络通信协议。有两套参考模型）
  - OSI参考模型：模型过于理想化，未能在因特网上进行广泛推广
  - TCP/IP参考模型(或TCP/IP协议)：事实上的国际标准。

![](C:/NoteBook/pictures/430412716221153.png =497x)
![](C:/NoteBook/pictures/20712816239579.png =397x)

## 通信要素1：IP 和 端口号  --> Socket

**IP 地址：InetAddress类**

- 唯一的标识 Internet 上的计算机（通信实体）
- 本地回环地址(hostAddress)：127.0.0.1 主机名(hostName)：localhost
- IP地址分类方式1：IPV4 和 IPV6
   - IPV4：4个字节组成，4个0-255。大概42亿，30亿都在北美，亚洲4亿。2011年初已经用尽。以点分十进制表示，如192.168.0.1
   - IPV6：128位（16个字节），写成8个无符号整数，每个整数用四个十六进制位表示，数之间用冒号（：）分开，如：3ffe:3201:1401:1280:c8ff:fe4d:db39:1984
- IP地址分类方式2：
   - 公网地址(万维网使用)
   - 私有地址(局域网使用)。
       - 192.168.开头的就是私有址址，范围即为192.168.0.0--192.168.255.255，专门为组织机构内部使用
- 特点：不易记忆

**域名**

- 域名解析
  - 域名容易记忆，当在连接网络时输入一个主机的域名后，域名服务器(DNS)负责将域名转化成IP地址，这样才能和主机建立连接。

![](C:/NoteBook/pictures/229354916240281.png =613x)

**本地回路地址 127.0.0.1**

- 对应着：localhost 即本机地址

**端口号标识正在计算机上运行的进程（程序）**

- 不同的进程有不同的端口号
- 被规定为一个 16 位的整数 0~65535。
- 端口分类：
   - 公认端口：0~1023。被预先定义的服务通信占用（如：HTTP占用端口80，FTP占用端口21，Telnet占用端口23）
   - 注册端口：1024~49151。分配给用户进程或应用程序。（如：Tomcat占用端口8080，MySQL占用端口3306，Oracle占用端口1521等）。
   - 动态/私有端口：49152~65535。

**端口号与IP地址的组合得出一个网络套接字：Socket。**

## InetAddress类

**Internet上的主机有两种方式表示地址：**

- 域名(hostName)：www.atguigu.com
- IP 地址(hostAddress)：202.108.35.210

**InetAddress类主要表示IP地址，两个子类**：

- Inet4Address
- Inet6Address。

InetAddress 类 对 象 含 有 一 个 Internet 主 机 地 址 的 域 名 和 IP 地 址 ：www.atguigu.com 和 202.108.35.210。

**实例化InetAddress类**

- getByName(String host)
   - 获取对应IP地址
- getLocalHost()
   - 获取本机地址 (127.0.0.1) 一般是局域网中的地址  
   
**常用方法**

- getHostName()
   - 获取域名 
- getHostAddress()
   - 获取IP地址

## 通信要素2：网络通信协议

**网络通信协议**

计算机网络中实现通信必须有一些约定，即通信协议，对速率、传输代码、代码结构、传输控制步骤、出错控制等制定标准。
  
**TCP/IP协议簇**

- 传输层协议中有两个非常重要的协议：
  - 传输控制协议TCP(Transmission Control Protocol)
  - 用户数据报协议UDP(User Datagram Protocol)。
- TCP/IP 以其两个主要协议：
   - 实际上是一组协议，包括多个具有不同功能且互为关联的协议。
        - TCP/IP协议模型从更实用的角度出发，形成了高效的四层体系结构，即物理链路层、IP层、传输层和应用层 
   - 传输控制协议(TCP)
   - 网络互联协议(IP)  
       - IP(Internet Protocol)协议是网络层的主要协议，支持网间互连的数据通信。

**TCP 和 UDP**

- TCP协议：
    - 使用TCP协议前，须先建立TCP连接，形成传输数据通道
    - 传输前，采用“三次握手”方式，点对点通信，是可靠的
    - TCP协议进行通信的两个应用进程：客户端、服务端。
    - 在连接中可进行大数据量的传输
    - 传输完毕，需释放已建立的连接，效率低
- UDP协议：
    - 将数据、源、目的封装成数据包，不需要建立连接
    - 每个数据报的大小限制在64K内
    - 发送不管对方是否准备好，接收方收到也不确认，故是不可靠的
    - 可以广播发送
    - 发送数据结束时无需释放资源，开销小，速度快

![](C:/NoteBook/pictures/595062117236836.png =638x)
![](C:/NoteBook/pictures/230152217232590.png =639x)

## Socket类

- 利用套接字(Socket)开发网络应用程序早已被广泛的采用，以至于成为事实上的标准。
- 网络上具有唯一标识的IP地址和端口号组合在一起才能构成唯一能识别的标识符套接字。
- 通信的两端都要有Socket，是两台机器间通信的端点。
- 网络通信其实就是Socket间的通信。
- Socket允许程序把网络连接当成一个流，数据在两个Socket间通过IO传输。
- 一般主动发起通信的应用程序属客户端，等待通信请求的为服务端。
- Socket分类
  - 流套接字（stream socket）：使用TCP提供可依赖的字节流服务
  - 数据报套接字（datagram socket）：使用UDP提供“尽力而为”的数据报服务

## TCP网络编程

**1. 客户端发送数据到服务端**

```java
package com.zjk.java1;

import org.junit.Test;

import java.io.*;
import java.net.InetAddress;
import java.net.ServerSocket;
import java.net.Socket;
import java.net.UnknownHostException;

public class TCPTest {

    //客户端
    @Test
    public void client() {

        Socket socket = null;
        OutputStream outputStream = null;
        try {
            //1. 创建Socket对象,指明服务器的IP和端口号
            InetAddress inet = InetAddress.getByName("127.0.0.1");
            socket = new Socket(inet, 22112);
            //2. 获取一个输出流，输出数据
            outputStream = socket.getOutputStream();
            //3. 写出数据
            outputStream.write("客户端".getBytes());
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            //4. 关闭资源
            try {
                if (outputStream != null)
                    outputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (socket != null)
                    socket.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    //服务端
    @Test
    public void server(){
        ServerSocket serverSocket = null;
        Socket accept = null;
        InputStream inputStream = null;
        ByteArrayOutputStream byteArrayOutputStream = null;
        try {
            //1. 创建服务器端的ServerSocket，指明自己的端口号
            serverSocket = new ServerSocket(22112);
            //2. 调用accept()方法 表示接收来自客户端的Socket
            accept = serverSocket.accept();
            //3. 获取输入流
            inputStream = accept.getInputStream();

            //不建议，可能会有乱码
//        byte[] buffer = new byte[20];
//        int len;
//        while ((len = inputStream.read(buffer)) != -1) {
//            String str = new String(buffer, 0, len);
//            System.out.println(str);
//        }

            //4. 读取输入流中的数据
            byteArrayOutputStream = new ByteArrayOutputStream();
            byte[] buffer = new byte[20];
            int len;
            while ((len = inputStream.read(buffer)) != -1) {
                byteArrayOutputStream.write(buffer, 0, len);
            }

            System.out.println(byteArrayOutputStream);

            System.out.println(accept.getInetAddress().getHostAddress());
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            //5. 关闭资源
            try {
                if(byteArrayOutputStream != null)
                    byteArrayOutputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if(inputStream!=null)
                    inputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if(accept!=null) {
                    accept.close();
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if(serverSocket!=null)
                    serverSocket.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }

    }
}
```

**2. 客户端发送文件到服务端，服务端将文件保存到本地**

```java
package com.zjk.java1;

import org.junit.Test;

import java.io.*;
import java.net.InetAddress;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPTest2 {
    @Test
    public void client() {
        Socket socket = null;
        OutputStream outputStream = null;
        BufferedInputStream bufferedInputStream = null;
        try {
            socket = new Socket(InetAddress.getByName("127.0.0.1"), 22112);
            outputStream = socket.getOutputStream();
            bufferedInputStream = new BufferedInputStream(new FileInputStream("th.jpg"));

            byte[] buffer = new byte[1024];
            int len;
            while ((len = bufferedInputStream.read(buffer)) != -1) {
                outputStream.write(buffer, 0, len);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (bufferedInputStream != null)
                    bufferedInputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (outputStream != null)
                    outputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (socket != null)
                    socket.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    @Test
    public void server() {
        ServerSocket serverSocket = null;
        Socket accept = null;
        InputStream inputStream = null;
        FileOutputStream fileOutputStream = null;
        try {
            serverSocket = new ServerSocket(22112);
            accept = serverSocket.accept();
            inputStream = accept.getInputStream();
            fileOutputStream = new FileOutputStream(new File("thClient.jpg"));

            byte[] buffer = new byte[1024];
            int len;
            while ((len = inputStream.read(buffer)) != -1) {
                fileOutputStream.write(buffer, 0, len);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (fileOutputStream != null)
                    fileOutputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (inputStream != null)
                    inputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (accept != null)
                    accept.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (serverSocket != null)
                    serverSocket.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }

    }
}
```

**3.从客户端发送文件给服务端，服务端保存到本地。并返回“发送成功”给客户端。并关闭相应的连接。**

```java
package com.zjk.java1;

import org.junit.Test;

import java.io.*;
import java.net.InetAddress;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPTest3 {
    @Test
    public void client() {
        Socket socket = null;
        OutputStream outputStream = null;
        BufferedInputStream bufferedInputStream = null;
        InputStream inputStream = null;
        ByteArrayOutputStream byteArrayOutputStream = null;
        try {
            socket = new Socket(InetAddress.getByName("127.0.0.1"), 22112);
            outputStream = socket.getOutputStream();
            bufferedInputStream = new BufferedInputStream(new FileInputStream("th.jpg"));

            byte[] buffer = new byte[1024];
            int len;
            while ((len = bufferedInputStream.read(buffer)) != -1) {
                outputStream.write(buffer, 0, len);
            }
            //关闭数据的输出
            socket.shutdownOutput();

            //接收服务端的反馈，并显示在控制台
            inputStream = socket.getInputStream();
            byteArrayOutputStream = new ByteArrayOutputStream();
            byte[] buffer2 = new byte[20];
            int len2;
            while ((len2 = inputStream.read(buffer2)) != -1) {
                byteArrayOutputStream.write(buffer2, 0, len2);
            }

            System.out.println(byteArrayOutputStream.toString());

        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (bufferedInputStream != null)
                    bufferedInputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (outputStream != null)
                    outputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (socket != null)
                    socket.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (inputStream!=null)
                    inputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if(byteArrayOutputStream!=null)
                    byteArrayOutputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }
    }

    @Test
    public void server() {
        ServerSocket serverSocket = null;
        Socket accept = null;
        InputStream inputStream = null;
        FileOutputStream fileOutputStream = null;
        OutputStream outputStream = null;

        try {
            serverSocket = new ServerSocket(22112);
            accept = serverSocket.accept();
            inputStream = accept.getInputStream();
            fileOutputStream = new FileOutputStream(new File("thClient2.jpg"));

            byte[] buffer = new byte[1024];
            int len;
            while ((len = inputStream.read(buffer)) != -1) {
                fileOutputStream.write(buffer, 0, len);
            }

            //服务器端给予客户端反馈
            outputStream = accept.getOutputStream();
            outputStream.write("服务端：已接收客户端文件。".getBytes());

        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (fileOutputStream != null)
                    fileOutputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (inputStream != null)
                    inputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (accept != null)
                    accept.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
            try {
                if (serverSocket != null)
                    serverSocket.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }

            try {
                if(outputStream!=null)
                    outputStream.close();
            } catch (IOException e) {
                throw new RuntimeException(e);
            }
        }

    }
}
```

## UDP 网络编程

```java
package com.zjk.java1;


import org.junit.Test;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

public class UDPTest {

    //发送端
    @Test
    public void sender() {
        DatagramSocket socket = null;
        try {
            socket = new DatagramSocket();

            String str = "UDP:发送端";
            byte[] date = str.getBytes();
            InetAddress inet = InetAddress.getLocalHost();
            DatagramPacket packet = new DatagramPacket(date, 0, date.length, inet, 21122);

            socket.send(packet);
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                socket.close();
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        }

    }

    //接收端
    @Test
    public void receiver() {
        DatagramSocket socket = null;
        try {
            socket = new DatagramSocket(21122);

            byte[] buffer = new byte[100];
            DatagramPacket packet = new DatagramPacket(buffer, 0, buffer.length);

            socket.receive(packet);

            System.out.println(new String(packet.getData(), 0, packet.getLength()));
        } catch (IOException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                socket.close();
            } catch (Exception e) {
                throw new RuntimeException(e);
            }
        }

    }
}
```

## URL 网络编程

- URL(Uniform Resource Locator)：统一资源定位符，它表示 Internet 上某一资源的地址。
- 它是一种具体的URI，即URL可以用来标识一个资源，而且还指明了如何locate这个资源。
- 通过 URL 我们可以访问 Internet 上的各种网络资源，比如最常见的 www，ftp 站点。浏览器通过解析给定的 URL 可以在网络上查找相应的文件或其他资源。

**URL的基本结构由5部分组成：**'

- `<传输协议>://<主机名>:<端口号>/<文件名>#片段名?参数列表`
    - 例如: `http://192.168.1.100:8080/helloworld/index.jsp#a?username=shkstart&password=123`
    - #片段名：即锚点，例如看小说，直接定位到章节
    - 参数列表格式：参数名=参数值&参数名=参数值....
    
**URL类 构造器**

URL对象一旦创建就不可修改。

- public URL (String spec)
     - 通过一个表示URL地址的字符串可以构造一个URL对象。
     - 例如：URL url = new URL ("http://www. atguigu.com/"); 
- public URL(URL context, String spec)
     - 通过基 URL 和相对 URL 构造一个 URL 对象。
     - 例如：URL downloadUrl = new URL(url, “download.html")
- public URL(String protocol, String host, String file)
     - 例如：new URL("http", "www.atguigu.com", “download. html");
- public URL(String protocol, String host, int port, String file);
     - 例如: URL gamelan = new URL("http", "www.atguigu.com", 80, “download.html");

**常用方法**

- public String getProtocol( ) 获取该URL的协议名
- public String getHost( ) 获取该URL的主机名
- public String getPort( ) 获取该URL的端口号,如果没有则返回-1
- public String getPath( ) 获取该URL的文件路径
- public String getRef() 获取URL的相对位置（引用）
- public String getFile( ) 获取该URL的文件名
- public String getQuery( ) 获取该URL的查询名

```java
//URL对象信息的获取

package com.zjk.java1;

import java.net.MalformedURLException;
import java.net.URL;

public class URLtest1 {
    public static void main(String[] args) {
        try {
            URL url = new URL("http:/java.sun.com:80/docs/books/tutorial/index.html#DOWNLOADING");

            //getProtocol() 获取协议名
            System.out.println(url.getProtocol());//http
            //getHost() 获取主机
            System.out.println(url.getHost());
            //getPort() 获取端口
            System.out.println(url.getPort());//-1
            //getRef() 获取相对路径
            System.out.println(url.getRef());//DOWNLOADING
            //getFile() 获取文件名
            System.out.println(url.getFile());// /java.sun.com:80/docs/books/tutorial/index.html
            //getQuery() 获取查询名
            System.out.println(url.getQuery());//null
        } catch (MalformedURLException e) {
            throw new RuntimeException(e);
        }
    }
}
```

# 反射机制

## 概述

- java 准动态语言
- 反射：动态语言的关键。
   - 在运行时借助于Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及方法
   ![](C:/NoteBook/pictures/54754423239580.png =438x)

**Java反射机制提供的功能**

- 在运行时判断任意一个对象所属的类
- 在运行时构造任意一个类的对象
- 在运行时判断任意一个类所具有的成员变量和方法
- 在运行时获取泛型信息
- 在运行时调用任意一个对象的成员变量和方法
- 在运行时处理注解
- 生成动态代理

**反射相关的API**

- java.lang.Class:代表一个类
- java.lang.reflect.Method:代表类的方法
- java.lang.reflect.Field:代表类的成员变量
- java.lang.reflect.Constructor:代表类的构造器

**反射之前**

1. 不能通过类的对象调用其内部的私有结构

**反射**

1. 通过反射可以调用运行时类的私有结构。

**反射与封装**

 - 封装性：建议能不能调用
 - 反射：非要调用的问题。

**new 和 反射 调用公共结构的选择**

- 建议 new 的方式
- 在编译时，无法确定创建哪个类的对象时，使用反射的方式。

```java
package com.zjk.java2;

import org.junit.Test;

import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class ReflectionTest {
    //反射之后，对Person类的操作
    @Test
    public void test1() throws Exception {
        Class<Person> personClass = Person.class;
        //1.通过反射，创建Person类的对象
        Constructor<Person> constructor = personClass.getConstructor(String.class, int.class);
        Person tom = constructor.newInstance("Tom", 21);
        System.out.println(tom);

        //2.通过反射，调用对象指定的属性、方法
        //调用属性
        Field age = personClass.getDeclaredField("age");
        age.set(tom,10);
        System.out.println(tom);
        //调用方法
        Method show = personClass.getDeclaredMethod("show");
        show.invoke(tom);
        //通过反射可以调用Person类的私有结构
        //调用私有构造器
        Constructor<Person> constructor1 = personClass.getDeclaredConstructor(String.class);
        constructor1.setAccessible(true);
        Person jac = constructor1.newInstance("Jac");
        System.out.println(jac);
        //调用私有属性
        Field name = personClass.getDeclaredField("name");
        name.setAccessible(true);
        name.set(jac,"Mac");
        System.out.println(jac);
        //调用私有方法
        Method showNation = personClass.getDeclaredMethod("showNation", String.class);
        showNation.setAccessible(true);
        String china = (String) showNation.invoke(jac, "China");
        //invoke() 相当于：p1.showNation("China")
        System.out.println(china);
    }
}


package com.zjk.java2;

public class Person {
    private String name;
    public int age;

    private Person(String name) {
        this.name = name;
    }

    public Person() {
    }

    public Person(String name, int age) {
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

    public void show() {
        System.out.println("人");
    }

    private String showNation(String nation) {
        System.out.println("国籍：" + nation);
        return nation;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

## Class 类

- Class本身也是一个类
- Class类是Reflection的根源，针对任何你想动态加载、运行的类，唯有先获得相应的Class对象
- Class 对象只能由系统建立对象，一个Class对象对应的是一个加载到JVM中的一个.class文件
- 对于每个类而言，JRE 都为其保留一个不变的 Class 类型的对象。即：每个类的实例都会记得自己是由哪个 Class 实例所生成
   - 一个 Class 对象包含了特定某个结构(`class / interface / enum / annotation / primitive type 基本数据类型 / void / [] 数组 `)的有关信息。
      - class包括 外部类，成员(成员内部类，静态内部类)，局部内部类，匿名内部类 
      - 对于数组，只要数组的数据类型和维度一样，则属于同一个Class，即：地址相同。
      - 一个加载的类在 JVM 中只会有一个Class实例 
   - 某个类的属性、方法和构造器、某个类到底实现了哪些接口。
     - 通过Class可以完整地得到一个类中的所有被加载的结构

**获取Class实例**

- 加载到内存中的运行时类，会缓存一定的时间，在此时间之内，可以通过各种方式来获取此运行时类

1. 前提：若已知具体的类，通过类的class属性获取，该方法最为安全可靠，程序性能最高
   - 实例：`Class clazz = String.class;`
2. 前提：已知某个类的实例，调用该实例的getClass()方法获取Class对象
   - 实例：`Class clazz = "www.atguigu.com".getClass();`
3. 前提：已知一个类的全类名，且该类在类路径下，可通过Class类的静态方法forName()获取，可能抛出ClassNotFoundException
   - 实例：`Class clazz = Class.forName("java.lang.String");`
4. 其他方式(不做要求) 类的加载器
   - `ClassLoader cl = this.getClass().getClassLoader();`
   - `Class clazz4 = cl.loadClass("类的全类名");`

**常用方法**

- static Class forName(String name) 
   - 返回指定类名 name 的 Class 对象
- Object newInstance() 
   - 调用缺省构造函数，返回该Class对象的一个实例
- getName() 
   - 返回此Class对象所表示的实体（类、接口、数组类、基本类型或void）名称
- Class getSuperClass() 
   - 返回当前Class对象的父类的Class对象
- Class[] getInterfaces() 
   - 获取当前Class对象的接口
- ClassLoader getClassLoader() 
   - 返回该类的类加载器
- Constructor[] getConstructors() 
   - 返回一个包含某些Constructor对象的数组
- Field[] getDeclaredFields() 
   - 返回Field对象的一个数组
- Method getMethod(String name,Class … paramTypes)
   - 返回一个Method对象，此对象的形参类型为paramType

```java
@Test
public void test2() throws ClassNotFoundException {
    //获取Class实例
    //1. 调用运行时类的属性，.class
    Class<Person> personClass = Person.class;
    System.out.println(personClass); //class com.zjk.java2.Person
    //2. 通过运行时类的对象，调用getClass()
    Person p1 = new Person();
    Class<? extends Person> personClass2 = p1.getClass();
    System.out.println(personClass2); //class com.zjk.java2.Person
    //3. 调用Class的静态方法，forName(String class)
    Class<?> personClass3 = Class.forName("com.zjk.java2.Person");
    System.out.println(personClass3); //class com.
    // zjk.java2.Person

    //指向的都是同一个对象
    System.out.println(personClass == personClass2); //true
    System.out.println(personClass == personClass3); //true
    System.out.println(personClass2 == personClass3); //true

    //Class实例可以是哪些结构
    Class<Object> objectClass = Object.class;//class 类
    Class<Comparable> comparableClass = Comparable.class; //interface 接口
    Class<String[]> aClass = String[].class; //数组
    Class<int[][]> aClass1 = int[][].class;  //数组
    Class<ElementType> elementTypeClass = ElementType.class; //enum 枚举
    Class<Override> overrideClass = Override.class; //annotion 注解
    Class<Integer> integerClass = int.class; //基本数据类型
    Class<Void> voidClass = void.class; //void
    Class<Class> classClass = Class.class;//Class类

    //对于数组而言 只要数组的元素类型和维度相同，则就是同一个Class
    int[] a = new int[10];
    int[] b = new int[1];
    System.out.println(a.getClass() == b.getClass()); //true


    System.out.println(personClass.getName()); //com.zjk.java2.Person
    System.out.println(personClass.getSuperclass()); //class java.lang.Object
    System.out.println(personClass.getClassLoader()); //jdk.internal.loader.ClassLoaders$AppClassLoader@78308db1
    Constructor<?>[] constructors = personClass.getConstructors();
    for (Constructor<?> con : constructors) {
        System.out.println(con);
        //public com.zjk.java2.Person(java.lang.String,int)
        //public com.zjk.java2.Person()
    }
}
```

## 类的加载过程

1. 程序经过javac.exe命令以后，会生成一个或多个字节码文件（.class文件），
2. 接着，使用java.exe命令对某个字节码文件进行解释运行。
    - 即：将某个字节码文件加载到内存中。此过程就称为类的加载
3. 加载到内存中的类，称为运行时类，作为Class的一个实例。
    - 即：Class的实例就对应着一个运行时类 

![](C:/NoteBook/pictures/408071312221155.png =620x)

1. 加载：将class文件字节码内容加载到内存中，并将这些静态数据转换成方法区的运行时数据结构，然后生成一个代表这个类的java.lang.Class对象，作为方法区中类数据的访问入口（即引用地址）。所有需要访问和使用类数据只能通过这个Class对象。这个加载的过程需要类加载器参与。
2. 链接：将Java类的二进制代码合并到JVM的运行状态之中的过程。
   - 验证：确保加载的类信息符合JVM规范，例如：以cafe开头，没有安全方面的问题
   - 准备：正式为类变量（static）分配内存并设置类变量默认初始值的阶段，这些内存都将在方法区中进行分配。
   - 解析：虚拟机常量池内的符号引用（常量名）替换为直接引用（地址）的过程。
3. 初始化：
    - 执行类构造器`<clinit>()`方法的过程。类构造器`<clinit>()`方法是由编译期自动收集类中所有类变量的赋值动作和静态代码块中的语句合并产生的。（类构造器是构造类信息的，不是构造该类对象的构造器）。
    - 当初始化一个类的时候，如果发现其父类还没有进行初始化，则需要先触发其父类
的初始化。
    - 虚拟机会保证一个类的`<clinit>()`方法在多线程环境中被正确加锁和同步。

![](C:/NoteBook/pictures/273142511221156.png)

### ClassLoader 类加载器

![](C:/NoteBook/pictures/378702711239582.png)

![](C:/NoteBook/pictures/313352911227449.png)

- 对于自定义类，使用系统类加载器进行加载。
- 引导类加载器主要负责加载Java的核心类库，无法加载自定义类。

```java
package com.zjk.java2;

import org.junit.Test;

public class ClassLoaderTest {
    @Test
    public void test1(){
        //系统类加载器
        ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
        System.out.println(classLoader); //jdk.internal.loader.ClassLoaders$AppClassLoader@78308db1

        //扩展类加载器 调用系统类加载器的getParent() 获取上一级加载器（扩展类加载器）
        ClassLoader parent = classLoader.getParent();
        System.out.println(parent); //jdk.internal.loader.ClassLoaders$PlatformClassLoader@35bbe5e8

        //引导类加载器  调用扩展类加载器的getParent() 获取不到引导类加载器 null
        //引导类加载器主要负责加载Java的核心类库，无法加载自定义类。
        ClassLoader parent1 = parent.getParent();
        System.out.println(parent1); //null

        //引导类加载器 无法获取到null
        //String是引导类加载器加载的，核心类库。
        ClassLoader classLoader1 = String.class.getClassLoader();
        System.out.println(classLoader1); //null
    }
}
```

### ClassLoader 用于读取Properties

```java
package com.zjk.java2;

import org.junit.Test;
import org.junit.experimental.theories.suppliers.TestedOn;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.Properties;

public class ClassLoaderTest {
    @Test
    public void test2() throws IOException {
        //Properties 用于读取配置文件
        Properties pros = new Properties();
        //方式1 此时默认在Module下
//            FileInputStream fis = new FileInputStream("jdbcTest.properties");
//            pros.load(fis);

        //方式2 此时默认在Module的src下
        ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
        InputStream resourceAsStream = classLoader.getResourceAsStream("jdbcTest.properties");
        pros.load(resourceAsStream);

        String user = pros.getProperty("user");
        String passwd = pros.getProperty("passwd");
        System.out.println("user = " + user + " , passwd = " + passwd);
    }
}
```

## 创建运行时类的对象

**newInstance()** 创建对应的运行时类的对象，内部调用了运行时类的空参构造器。

- 要求：
1. 运行时类必须提供空参构造器。
2. 空参构造器的访问权限得够。

**在JavaBean中要求提供一个空参构造器**

1. 便于通过反射，创建运行时类得对象
2. 便于子类继承此运行时类时，默认调用super()时，保证父类有此构造器。

```java
@Test
public void test1() throws InstantiationException, IllegalAccessException {
    Class<Person> personClass = Person.class;
    //newInstance() 创建对应的运行时类的对象。内部调用了运行时类的空参构造器
    //如果没有空参构造器，则报错 找不到合适的构造器 java.lang.InstantiationException
    Person person = personClass.newInstance();
    System.out.println(person);
}
```

```java
@Test
public void test2() {
    String classPath = null;
    int num = new Random().nextInt(3);
    switch (num) {
        case 0:
            classPath = "java.util.Date";
            break;
        case 1:
            classPath = "java.sql.Date";
            break;
        case 2:
            classPath = "com.zjk.java1.Person";
            break;
    }
    try {
        Object instance = this.getInstance(classPath);
        System.out.println(instance);
    } catch (ClassNotFoundException e) {
        throw new RuntimeException(e);
    } catch (InstantiationException e) {
        throw new RuntimeException(e);
    } catch (IllegalAccessException e) {
        throw new RuntimeException(e);
    }
}

public Object getInstance(String classPath) throws ClassNotFoundException, InstantiationException, IllegalAccessException {
    return Class.forName(classPath).newInstance();
}
```

## 获取运行时类的完整结构

Field、Method、Constructor、Superclass、Interface、Annotation

```java
package com.zjk.java1;

@MyAnnnotation(value = "Hi")
public class Person extends Creature implements Comparable<String>, MyInterface {
    private String name;
    int age;
    public int id;

    @MyAnnnotation("Person name")
    private Person(String name) {
        this.name = name;
    }

    public Person() {
    }

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public Person(String name, int age, int id) {
        this.name = name;
        this.age = age;
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
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

    @MyAnnnotation
    private void show() {
        System.out.println("人");
    }

    private String showNation(String nation) {
        System.out.println("国籍：" + nation);
        return nation;
    }

    private static String display(String interest) {
        System.out.println("兴趣爱好");
        return "兴趣爱好" + interest;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", id= " + id +
                '}';
    }

    @Override
    public int compareTo(String o) {
        return 0;
    }

    @Override
    public void info() {
        System.out.println("人 info");
    }
}

//MyInterface
package com.zjk.java1;

public interface MyInterface {
    void info();
}

//MyAnnotation
package com.zjk.java1;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target({ElementType.TYPE, ElementType.FIELD,ElementType.PARAMETER,ElementType.CONSTRUCTOR,ElementType.LOCAL_VARIABLE,ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnnotation {
    String value() default "Hello";
}

//Creature
package com.zjk.java1;

import java.io.Serializable;

public class Creature implements Serializable {
    private char gender;
    public double weight;

    private void breath(){
        System.out.println("生物呼吸");
    }

    public void eat(){
        System.out.println("生物吃");
    }
}
```

### 1. 实现的全部接口

- `public Class<?>[] getInterfaces()` 
   - 确定此对象所表示的类或接口实现的接口。
   - 获取当前运行时类（不包括父类）实现的接口

```java
@Test
public void test(){
    Class<Person> personClass = Person.class;
    //getInterfaces() 获取当前运行时类（不包括父类）实现的接口
    Class<?>[] interfaces = personClass.getInterfaces();
    for(Class<?> inter:interfaces){
        System.out.println(inter);
        //interface java.lang.Comparable
        //interface com.zjk.java1.MyInterface
    }
    //可以通过getSuperClass()的方法获取父类实现的接口
    Class<? super Person> superclass = personClass.getSuperclass();
    Class<?>[] interfaces1 = superclass.getInterfaces();
    for(Class<?> inter:interfaces1){
        System.out.println(inter);
        //interface java.io.Serializable
    }
}
```

### 2. 所继承的父类

- `public Class<? Super T> getSuperclass()`
   - 返回表示此 Class 所表示的实体（类、接口、基本类型）的父类的Class。

### 3. 全部的构造器

- `public Constructor<T>[] getConstructors()`
   - 返回此 Class 对象所表示的类的所有public构造方法。
   - 获取当前运行时类声明为public的构造器
- `public Constructor<T>[] getDeclaredConstructors()`
   - 返回此 Class 对象表示的类声明的所有构造方法。
   - 获取当前运行时类中所有的构造器

- Constructor类中：
   - 取得修饰符: `public int getModifiers()`;
   - 取得方法名称: `public String getName()`
   - 取得参数的类型：`public Class<?>[] getParameterTypes()`;

```java
package com.zjk.java1;

import oracle.sql.OPAQUE;
import org.junit.jupiter.api.Test;

import java.lang.reflect.Constructor;

public class ConstuctorTest {
    @Test
    public void test1(){
        Class<Person> personClass = Person.class;
        //gerConstructors() 获取当前运行时类声明为public的构造器
        Constructor<?>[] constructors = personClass.getConstructors();
        for(Constructor<?> constructor : constructors){
            System.out.println(constructor);
            //public com.zjk.java1.Person(java.lang.String,int,int)
            //public com.zjk.java1.Person()
        }

        //getDeclaredConstructors() 获取当前运行时类中所有的构造器
        Constructor<?>[] declaredConstructors = personClass.getDeclaredConstructors();
        for(Constructor<?> constructor : declaredConstructors){
            System.out.println(constructor);
            //private com.zjk.java1.Person(java.lang.String)
            //public com.zjk.java1.Person(java.lang.String,int,int)
            //com.zjk.java1.Person(java.lang.String,int)
            //public com.zjk.java1.Person()
        }
    }
}
```

### 4. 全部的方法

- `public Method[] getDeclaredMethods()`
   - 返回此Class对象所表示的类或接口的全部方法
   - 获取当前运行时类中声明的所有方法，不包含父类
- `public Method[] getMethods() `
   - 返回此Class对象所表示的类或接口的public的方法
   - 获取当前运行时类及其所有父类（包括Object类）中声明为public的所有方法
- Method类中：@注解 权限修饰符 返回值类型 方法名(形参列表) throws XxxException{}
   - `getAnnotations()`
       - 获取方法的注解  
   - `public Class<?> getReturnType()` 
       - 取得全部的返回值
   - `public Class<?>[] getParameterTypes()`
       - 取得全部的参数
   - `getParameterTypes()`
       - 获取形参类型
   - `public int getModifiers()`
       - 取得修饰符
   - `public Class<?>[] getExceptionTypes()`
       - 取得异常信息

```java
package com.zjk.java1;

import org.junit.jupiter.api.Test;

import java.lang.annotation.Annotation;
import java.lang.reflect.Method;
import java.lang.reflect.Modifier;

public class MethodTest {
    @Test
    public void test1() {
        Class<Person> personClass = Person.class;

        //getMethods() 获取当前运行时类及其父类中声明为public的所有方法
        Method[] methods = personClass.getMethods();
        for (Method method : methods) {
            System.out.println(method);
        }

        System.out.println();

        //getDeclaredMethods() 获取当前运行时类中声明的所有方法，不包含父类
        Method[] declaredMethods = personClass.getDeclaredMethods();
        for (Method method : declaredMethods) {
            System.out.println(method);
        }
    }

    /*
     * @注解
     * 权限修饰符 返回值类型 方法名(参数类型 参数名) throws XxxException{}
     */

    @Test
    public void test2() {
        Class<Person> personClass = Person.class;
        Method[] declaredMethods = personClass.getDeclaredMethods();
        for (Method method : declaredMethods) {
            //1. 获取方法的注解
            Annotation[] annotations = method.getAnnotations();
            for (Annotation annotation : annotations) {
                System.out.println(annotation);
                //@com.zjk.java1.MyAnnnotation("Hello")
            }

            //2. 获取方法的修饰符
            System.out.println(Modifier.toString(method.getModifiers()));

            //3. 获取方法的返回值类型
            System.out.println(method.getReturnType());

            //4. 获取方法的方法名
            System.out.println(method.getName());

            //5. 获取方法的形参列表
            //获取形参类型  //获取形参名：。。。
            Class<?>[] parameterTypes = method.getParameterTypes();
            if (!(parameterTypes == null && parameterTypes.length == 0)) {
                for (Class parameterType : parameterTypes) {
                    System.out.print(parameterType.getName());
                }
            }

            //6. 抛出的异常
            Class<?>[] exceptionTypes = method.getExceptionTypes();
            if (!(exceptionTypes == null && exceptionTypes.length == 0)) {
                for (Class exceptionType : exceptionTypes) {
                    System.out.println(exceptionType.getName());
                }
            }
        }
    }
}
```

### 5. 全部的Field

- `public Field[] getFields() `
   - 返回此Class对象所表示的类或接口的public的Field。
   - 获取当前运行时类及其父类中声明为public的所有属性
- `public Field[] getDeclaredFields() `
   - 返回此Class对象所表示的类或接口的全部Field。
   - 获取当前运行时类的所有属性，不包含父类
- Field方法中：权限修饰符 数据类型 属性名 
   - `public int getModifiers()` 
      - 以整数形式返回此Field的修饰符
      - private-2，public-1，缺省-0 
          - `Modifier.toString(field.getModifiers())` 转为修饰符
   - `public Class<?> getType()`
      -  得到Field的属性类型
   - `public String getName()` 
      - 返回Field的名称。
   
```java
package com.zjk.java1;

import org.junit.jupiter.api.Test;

import java.lang.reflect.Field;
import java.lang.reflect.Modifier;

public class FiledTest {
    @Test
    public void test1() {
        Class<Person> personClass = Person.class;

        //获取属性结构
        //getFileds() 获取当前运行时类及其父类中声明为public的所有属性
        Field[] fields = personClass.getFields();
        for (Field field : fields) {
            System.out.println(field);
            //public int com.zjk.java1.Person.id
            //public double com.zjk.java1.Creature.weight
        }
        //getDeclaredFileds() 获取当前运行时类的所有属性,不包含父类
        Field[] declaredFields = personClass.getDeclaredFields();
        for (Field field : declaredFields) {
            System.out.println(field);
            //private java.lang.String com.zjk.java1.Person.name
            //int com.zjk.java1.Person.age
            //public int com.zjk.java1.Person.id
        }
    }

    @Test
    public void test2() {
        Class<Person> personClass = Person.class;
        Field[] declaredFields = personClass.getDeclaredFields();
        for (Field field : declaredFields) {
            //1. 权限修饰符
            int modifiers = field.getModifiers();
              //2 = private ，1 = public ，0 = 缺省
            System.out.println(modifiers);
              //从数字转为对应的权限修饰符 缺省--空
            System.out.println(Modifier.toString(modifiers));

            //2. 数据类型
            Class<?> type = field.getType();
            System.out.println(type);
            //class java.lang.String
            //int

            //3. 变量名
            String name = field.getName();
            System.out.println(name);

            System.out.println("----");
        }
    }
}
```

### 6. Annotation相关

- `get Annotation(Class<T> annotationClass) `
   - 获取运行时类声明的注解，（不包括父类）
- `getDeclaredAnnotations() `

```java
    @Test
    public void test4(){
        Class<Person> personClass = Person.class;
        for (Annotation annotation : personClass.getAnnotations()) {
            System.out.println(annotation);
            //@com.zjk.java1.MyAnnnotation("Hi")
        }
    }
```

### 7. 泛型相关

- `Type getGenericSuperclass()`
    - 获取父类泛型类型
- 泛型类型：`ParameterizedType`
    - `getActualTypeArguments()`
    - 获取实际的泛型类型参数数组

```java
@Test
public void test2() {
    Class<Person> personClass = Person.class;
    System.out.println(personClass.getSuperclass());
    //class com.zjk.java1.Creature

    //获取运行时类带泛型的父类
    Type genericSuperclass = personClass.getGenericSuperclass();
    System.out.println(genericSuperclass);
    //com.zjk.java1.Creature<java.lang.String>

    ParameterizedType parameterizedType = (ParameterizedType) genericSuperclass;
    //获取泛型类型
    Type[] actualTypeArguments = parameterizedType.getActualTypeArguments();
    for (Type type : actualTypeArguments) {
        System.out.println(((Class) type).getTypeName());
        //java.lang.String
    }
}
```

### 8. 类所在的包 

- `Package getPackage()`

```java
@Test
public void test3(){
    Class<Person> personClass = Person.class;
    Package aPackage = personClass.getPackage();
    System.out.println(aPackage);
    //package com.zjk.java1
}
```

## 调用运行时类的指定结构

**获取指定的方法属性**

- `getField(String fieldName) `
    - 获取指定的某个属性,要求运行时类获取的属性是public的
- `getDeclaredFiled(String fieldName)`  
    - 获取指定的属性
- `getDeclaredMethod(Sting methodName, Class ... args)` 
    - 获取指定的某个方法，指定方法名和形参列表
- `getDeclaredConstruct(Class<?> ... parameterType)` 
    - 获取指定的构造器，指定参数列表，空：则空参构造器

**调用指定方法**

- `Object invoke(Object obj, Object ... args)`
1. invoke()方法的返回值对应原方法的返回值，若原方法无返回值，此时返回null
2. 若原方法若为静态方法，此时形参Object obj可为null
   - 调用静态方法 `invoke(Class class, Object ... args)`
3. 若原方法形参列表为空，则Object[] args为null
4. 若原方法声明为private,则需要在调用此invoke()方法前，显式调用方法对象的`setAccessible(true)`方法，将可访问private的方法。

**调用指定属性**

- `public Object get(Object obj)`
    -  取得指定对象obj上此Field的属性内容
- `public void set(Object obj,Object value)` 
    - 设置指定对象obj上此Field的属性内容

**关于setAccessible方法的使用**

- Method和Field、Constructor对象都有setAccessible()方法。
- setAccessible启动和禁用访问安全检查的开关。
- 参数值为true则指示反射的对象在使用时应该取消Java语言访问检查。
- 提高反射的效率。如果代码中必须用反射，而该句代码需要频繁的被调用，那么请设置为true。
- 使得原本无法访问的私有成员也可以访问
- 参数值为false则指示反射的对象应该实施Java语言访问检查。

```java
package com.zjk.java1;

import org.junit.jupiter.api.Test;

import java.io.PipedOutputStream;
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.InvocationTargetException;
import java.lang.reflect.Method;

public class ReflectionTest {
    //调用指定的属性1
    @Test
    public void test() throws NoSuchFieldException, IllegalAccessException, InstantiationException {
        Class<Person> personClass = Person.class;

        //创建运行时类的对象
        Person person = personClass.newInstance();

        //getField(String fieldName) 获取指定的某个属性,要求运行时类获取的属性是public的
        Field id = personClass.getField("id");

        //set(Object obj, Object value) 设置当前属性的值
        id.set(person,1001);

        //get(Object obj)
        System.out.println(id.get(person)); //1001
    }

    //调用指定的属性2
    @Test
    public void test2() throws InstantiationException, IllegalAccessException, NoSuchFieldException {
        Class<Person> personClass = Person.class;
        Person person = personClass.newInstance();

        //getDeclaredFiled(String fieldName) 获取指定的属性
        Field name = personClass.getDeclaredField("name");

        //对于非public权限的属性：需要启动和禁用访问安全检查的开关
        //保证当前属性是可访问的
        name.setAccessible(true);

        name.set(person,"西山");

        System.out.println(name.get(person));
    }

    //调用指定的方法
    @Test
    public void test3() throws Exception{
        Class<Person> personClass = Person.class;

        Person person = personClass.newInstance();

        //getDeclaredMethod(Sting methodName, Class ... args) 获取指定的某个方法，指定方法名和形参列表
        Method showNation = personClass.getDeclaredMethod("showNation", String.class);

        showNation.setAccessible(true);
        //invoke(Object obj, Object ... args) 调用相关的方法
        showNation.invoke(person,"China");

        //invoke(Class class, Object ... args) 调用静态方法
        Method display = personClass.getDeclaredMethod("display", String.class);
        display.setAccessible(true);

        String interest = (String)display.invoke(personClass, "Java");
        System.out.println(interest);
    }

    //调用指定的构造器
    @Test
    public void test4() throws NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {
        Class<Person> personClass = Person.class;
        //getDeclaredConstruct(Class<?> ... parameterType) 获取指定的构造器
        //空：则空参构造器
        Constructor<Person> declaredConstructor = personClass.getDeclaredConstructor(String.class,int.class);
        declaredConstructor.setAccessible(true);

        Person person = declaredConstructor.newInstance("Tom", 21);
        System.out.println(person);
        //Person{name='Tom', age=21, id= 0}
    }
}
```

## 动态代理

使用一个代理将对象包装起来, 然后用该代理对象取代原始对象。任何对原始对象的调用都要通过代理。代理对象决定是否以及何时将方法调用转到原始对象上

- 动态代理是指客户通过代理类来调用其它对象的方法，并且是在程序运行时根据需要动态创建目标类的代理对象。
- 动态代理使用场合:
  - 调试
  - 远程方法调用

**动态代理相比于静态代理的优点：**

抽象角色中（接口）声明的所有方法都被转移到调用处理器一个集中的方法中处理，这样，我们可以更加灵活和统一的处理众多的方法。

**静态代理**

```java
package com.zjk.java1;

interface ClothFactory {
    void produceCloth();
}

//代理类
class ProxyClothFactory implements ClothFactory{

    private ClothFactory factory; //用被代理类对象进行实例化

    public ProxyClothFactory(ClothFactory factory){
        this.factory = factory;
    }

    @Override
    public void produceCloth() {
        System.out.println("代理工厂");

        factory.produceCloth();

        System.out.println("后续");
    }
}

//被代理类
class NikeClothFactory implements ClothFactory{
    @Override
    public void produceCloth() {
        System.out.println("Nike工厂生产");
    }
}

public class ProxyTest {
    public static void main(String[] args) {
        //创建被代理类的对象
        NikeClothFactory nikeClothFactory = new NikeClothFactory();
        //创建代理类的对象
        ProxyClothFactory proxyClothFactory = new ProxyClothFactory(nikeClothFactory);

        proxyClothFactory.produceCloth();
    }
}
```  

### Proxy类

专门完成代理的操作类，是所有动态代理类的父类。通过此类为一个或多个接口动态地生成实现类。

**提供用于创建动态代理类和动态代理对象的静态方法**

![](C:/NoteBook/pictures/139852309221157.png =683x)

**动态代理**

```java
package com.zjk.java1;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

interface Human {

    String getBelief();

    void eat(String food);
}

//被代理类
class SuperMan implements Human {

    @Override
    public String getBelief() {
        return "no belief";
    }

    @Override
    public void eat(String food) {
        System.out.println("i like eat " + food);
    }
}

/*
 * 实现动态代理要解决的问题：
 * 1. 如何根据加载到内存中的被代理类，动态的创建一个代理类及其对象。
 * 2. 当通过代理对象调用方法时，如何动态的调用被代理类的同名方法。
 * */

class ProxyFactory {
    //返回一个代理类的对象，解决问题1
    public static Object getProxyInstance(Object obj) {
        //obj 被代理类的对象
        MyInvocationHandler myInvocationHandler = new MyInvocationHandler(obj);
        return Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj.getClass().getInterfaces(), myInvocationHandler);
    }
}

class MyInvocationHandler implements InvocationHandler {
    private Object obj; //需要使用被代理类的对象赋值

    public MyInvocationHandler(Object obj) {
        this.obj = obj;
    }

    //当我们通过代理类的对象，调用方法a时，就会自动的调用如下的方法：invoke()
    //将被代理类要执行的方法a的功能就声明在invoke()中
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //method 即为代理类对象调用的方法，此方法也就作为了被代理类对象要调用的方法
        return method.invoke(obj, args);
    }
}

public class ProxyTest1 {
    public static void main(String[] args) {
        SuperMan superMan = new SuperMan();
        //proxyInstance 代理类的对象
        Human proxyInstance = (Human) ProxyFactory.getProxyInstance(superMan);
        //当通过代理类对象调用方法时，自动调用被代理类中同名的方法
        System.out.println(proxyInstance.getBelief());
        proxyInstance.eat("❄");

        NikeClothFactory nikeClothFactory = new NikeClothFactory();
        ClothFactory proxyInstance1 = (ClothFactory) ProxyFactory.getProxyInstance(nikeClothFactory);
        proxyInstance1.produceCloth();
    }
}
```

### 动态代理与AOP（Aspect Orient Programming)

![](C:/NoteBook/pictures/439643715239583.png =456x)

```java
class HumanUtil{
    public void method1(){
        System.out.println("通用方法1");
    }

    public void method2(){
        System.out.println("通用方法2");
    }
}

@Override
public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

    HumanUtil humanUtil = new HumanUtil();
    humanUtil.method1();
    humanUtil.method2();

    //method 即为代理类对象调用的方法，此方法也就作为了被代理类对象要调用的方法
    return method.invoke(obj, args);
}
```

# GUI 基于Swing的图形化用户界面

## Java GUI 概述

**JFC**

- AWT
   - AWT是重量级组件，依赖本地操作系统，可移植性差。
- Java 2D
- Accessibility
- Drag&Drop
- Swing
- Internationalization

**配色**

- 三原色 红绿蓝 RGB
- 三基色 红黄蓝

## Swing组件

- Swing中的所有组件全部用Java语言实现。
- 采用分离模型：组件及与组件相关的数据模型（或简称模型） 
- 可设置的组件外观感觉（look and feel ）
- 与AWT相比提供更丰富的GUI组件，引入新的特征，并提供更丰富的功能。
- 一般如果使用Swing组件，则程序中只使用Swing组件和Swing容器。

**Swing包中定义了两类组件：**

- 顶层容器（JFrame,Japplet,Jwindow,Jdialog)
- 轻型组件（‘J’开头的所有组件，如Jbutton,Jpanel,Jmenu等）

**Swing组件分类**

1. 顶层容器组件 JFrame、JApplet、JDialog等
2. 通用容器组件 JPanel、JScrollPane、JToolBar等
3. 特殊容器 JIternalFrame、JLayeredPane、JRootPane等
4. 基本控制组件 JButton、JComboBox、JList、JSlider、JTestField等
5. 不可编辑的信息显示组件 JLabel、JProgressBar等
6. 可编辑的信息显示组件 JTable、JFileChooser、JTree等 

![](C:/NoteBook/pictures/420302910221148.png =490x)

```java
package com.zjk.java;

//引入Swing 包及其它程序包

import javax.swing.*;

public class HelloWorldSwing {

    //所有J开头的类都是Swing组件
    public static void main(String[] args) {
        //1.选择GUI的外观风格L&F
        //2.创建并设置窗口容器

        //JFrame 窗体 最大的容器：其他的组件都包含在窗体内
        //new JFrame("窗体标题")
        JFrame frame = new JFrame("窗体标题"); //窗体的标题栏

        //JLabel 标签 一般用来显示静态的文字，也可以改变
        JLabel label = new JLabel("Hello World!");

        //3.创建与添加Swing组件

        //getContentPane() 窗体的内容面板
        //add(label) 将标签组件添加到窗体内
        frame.getContentPane().add(label);

        //setDefaultCloseOperation() 设置缺省的关闭操作
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        //setSize() 设置窗体的初始长宽
        frame.setSize(2000, 700);

        //4.显示顶层容器，将整个GUI显示出来
        //setVisible() 将窗体从内存中显示
        frame.setVisible(true);
    }
}
```

### JComponent类

- Swing组件中,除了顶层容器组件，以J开头的都是JComponent类的子类
- JComponent类是java.awt.Container的子类，是抽象类。

![](c:/notebook/pictures/Snipaste_2022-11-21_10-34-34.png =600x)

**常用方法**

**组件设置**

- setLocation(int x, int y)
   - 设置组件大小
- setSize(int width, int height)
   - 设置组件位置
- setBounds(int x, int y, int width, int height)
   - 设置组件大小位置

**文本**

- getText()       
   -  获取String,组件内容 
- setText()      
   - 组件内容设置        

**焦点**

- requestFocus()                 
   - 请求焦点

**选择内容**

- setSelectionStart() 和 setSelectionEnd()
   - 设置选择的区域

### 常用容器

- 容器本身也是一个组件，具有组件的所有性质，另外还具有容纳其它功能
- 所有的组件都可以通过`add()`方法向容器中添加组件
- 在Swing中，常用的三种类型的容器是JFrame，JPanel，JApplet 
- Swing GUI形成顶层容器-中间层容器-基本组件的层次包容关系

#### 顶层容器

- 具有Swing GUI的应用必须有至少一个顶层容器 
- 顶层Swing容器是JFrame、JDialog或JApplet的实例

**向顶层容器中加入组件**

- Swing组件不能直接添加到顶层容器中，必须添加到一个与顶层容器相关联的内容面板（Content Pane）上

##### JFrame 框架

- JFrame是最常用的顶层容器，一般作为Java Application的主窗口 

**JFrame的构造器**

- `JFrame()` 创建一个最初不可见的框架
- `JFrame(String title)` 创建一个带有标题的最初不可见的框架

**方法**

- `set size(int length, int weigth)` 
   - 设置初始窗体大小 
- `setTitle(String title)` 
   - 设置窗体标题
- `void setContentPane(Container contentPane)`
   - 设置 contentPane属性。
- `Container	getContentPane()` 
   - 返回此框架的 contentPane对象。
- `void setDefaultCloseOperation(int operation)`
    - 设置用户在此框架上启动“关闭”时默认执行的操作
   - `WindowConstants.DO_NOTHING_ON_CLOSE` 
       - 不要做任何事情; 要求程序处理WindowListener对象的windowClosing方法的操作。
   - `WindowConstants.HIDE_ON_CLOSE` 
       - 默认，在调用任何已注册的WindowListener对象后自动隐藏框架。
   - `WindowConstants.DISPOSE_ON_CLOSE` 
       - 在调用任何已注册的WindowListener对象后自动隐藏和处理该框架。
   - `JFrame.EXIT_ON_CLOSE`
       - 使用`System exit`方法退出exit程序。 仅在应用程序中使用。 
- `int getDefaultCloseOperation()` 
    - 返回当用户在此框架上启动“关闭”时发生的操作。
- `static void setDefaultLookAndFeelDecorated(boolean defaultLookAndFeelDecorated)`
   - 提供关于新创建的 JFrame是否应该具有由当前外观提供的窗口装饰
   - 必须在创建JFrame窗口前设定
- `static boolean isDefaultLookAndFeelDecorated()`
   - 如果新创建的 JFrame应该具有当前外观提供的窗口装饰，则返回true。
- `void setLayout(LayoutManager manager)`
   - 设置 布局管理器 LayoutManager 。
- `public Component add(Component comp)`
   - 将指定的组件附加到此容器的末尾。
- `public Component add(Component comp, int index)`
   - 在给定的位置将指定的组件添加到此容器。
- `pack()` 
   - 使此窗口的大小适合其子组件的首选大小和布局。
- `setVisible(boolean b)`
   - 使窗体可视 
- `public void setPreferredSize(Dimension preferredSize)`
   - 将此组件的首选大小设置为常量值
   - `new Dimension(int x, int y)`

```java
package com.zjk.java;

import javax.swing.*;
import java.awt.*;

public class JFrameDemo {
    public static void main(String[] args) {
        //setDefaultLookAndFeelDecorated() 指定使用当前的Look&Feel装饰窗口
        JFrame.setDefaultLookAndFeelDecorated(true);

        //new JFrame(String title) 创建最初不可视的窗体，并设置标题
        JFrame frame = new JFrame("Hello World ！");
        //setDefaultCloseOperation() 设定关闭窗口操作
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
//        frame.setDefaultCloseOperation(WindowConstants.DISPOSE_ON_CLOSE);
        //new JLabel() 创建标签组件
        JLabel emptyLabel = new JLabel("Hello");
        emptyLabel.setPreferredSize(new Dimension(175,100));
        //add() 将组件添加到窗体
        frame.getContenPane().add(emptyLabel);

        //pack() 整理窗体布局
        frame.pack();
        //setViesible() 使窗口可视
        frame.setVisible(true);
    }
}
```

##### JOptionPane类 对话框类

**方法**

- `showXxxDialog()` 
  - 显示简单的模式对话框。包括确认对话框、输入对话框、消息对话框、选项对话框。
  - `showConfirmDialog()` 确认对话框
  - `showInputDialog()` 输入对话框
  - `showMessageDialog()`消息对话框
  - `showOptionDialog()` 选项对话框  
  - 参数：
     - `Component parentComponent` 
        - 定义对话框所依赖的窗口。该参数可为null，这时使用默认的窗口，并且对话框将在屏幕中央显示。
     - `Object message`
        - 该参数指定了对话框中要显示的内容。一般指定为一个字符串。
     - `String title`
        - 指定对话框的标题。
     - `int optionType`
        - 指定对话框底部要出现的一组按钮。从下列标准设置中选取，静态常量：
        - DEFAULT_OPTION
        - YES_NO_OPTION
        - YES_NO_CANCEL_OPTION
        - OK_CANCEL_OPTION
     - `int messageType`
        - 指定信息显示的风格，可以从下列值中选取，静态常量，它们都带有默认的图标：
        - PLAIN_MESSAGE (无图标)
        - ERROR_MESSAGE
        - INFORMATION_MESSAGE
        - WARNING_MESSAGE
        - QUESTION_MESSAGE
     - `Icon icon`
        - 指定对话框中显示的图标。该参数的默认值是由`massage Type`参数所指定的值。
     - `Object[] options`
        - 是对话框底部按钮更详细的描述。一般是一个字符串数组。
     - `Object initialValue`
        - 指定要选取项的默认输人值。
- `showInternalXxx()` 使用内部窗口的方式显示相应的对话框

```java
package com.zjk.java;

import javax.swing.*;

public class JOptionPaneTest {
    public static void main(String[] args) {

        JFrame frame = new JFrame("JOption测试");

        //信息对话框
        //带信息图标的对话框
        JOptionPane.showMessageDialog(frame, "似乎出现了一些奇怪的错误");
        //带警告图标的对话框
        JOptionPane.showMessageDialog(frame, "出现不可预计的错误", "警告",
                JOptionPane.WARNING_MESSAGE);

        //选项对话框
        Object[] options = {"是的", "不了", "???"};
        String optionText = "你想要一些???吗?";
        String optionTitle = "一个简单的问题";
        int n = JOptionPane.showOptionDialog(frame, optionText, optionTitle,
                JOptionPane.YES_NO_CANCEL_OPTION,
                JOptionPane.QUESTION_MESSAGE,
                null, options, options[2]);

        //输入对话框
        //1
        Object[] possibleValues = {"first", "second", "third"};
        String message = "请选择选项: ";
        String messageTile = "选择";
        Object selectedValue = JOptionPane.showInputDialog(null, message, messageTile,
                JOptionPane.INFORMATION_MESSAGE, null,
                possibleValues, possibleValues[0]);
        String inputValue = JOptionPane.showInputDialog("请输入数据");
        //2
        JOptionPane.showConfirmDialog(null, "请选择：", "选择", JOptionPane.YES_NO_OPTION);
    }
}
```

#### 通用容器

##### JPanel

- JPanel是存放轻型组件的通用容器，缺省情况下是透明的，其对象可作为顶层容器的Content Pane使用。 

**构造器**

- `JPanel()`
   - 创建一个新的 JPanel双缓冲区和流布局。
- `JPanel(boolean isDoubleBuffered)`
   - 创建一个新的 JPanel与 FlowLayout和指定的缓冲策略。
- `JPanel(LayoutManager layout)`
   - 使用指定的布局管理器创建一个新的缓冲JPanel
- `JPanel(LayoutManager layout, boolean isDoubleBuffered)`
   - 使用指定的布局管理器和缓冲策略创建一个新的JPanel。

```java
import java.awt.*;
import javax.swing.*;

public class FrameWithPanel extends JFrame {
    public static void main(String args[]) {
        FrameWithPanel fr = new FrameWithPanel("Hello !");
        fr.setSize(200, 200);
        fr.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        JPanel pan = new JPanel();
        pan.setBackground(Color.yellow);
        pan.setLayout(new GridLayout(2, 1));
        pan.add(new JButton("确定"));
        fr.setContentPane(pan);
        fr.setVisible(true);
    }

    public FrameWithPanel(String str) {
        super(str);
    }
} 
```

##### JScrollPane 滚动面板

**构造器**

- `public  JScrollPane()`
- `public  JScrollPane(Comonent view)`
- `public  JScrollPane(int vsbPolicym, int hsbPolicy)`
- `public  JScrollPane(Component view, int vsbPolicym, int hsbPolicy)`
   - view参数 客户组件
   - int vsbPolicym, int hsbPolicy 参数 指定水平和垂直滚动条的显示策略
      - JScrollPane.VERTICAL_SCROLLBAR_AS_NEEDED
      - JScrollPane.VERTICAL_SCROLLBAR_NEVER
      - JScrollPane.VERTICAL_SCROLLBARA_ALWAYS

```java
package com.zjk.java;

import javax.swing.*;
import java.awt.*;

public class JScrollPaneTest {
    public static void main(String[] args) {
        JFrame.setDefaultLookAndFeelDecorated(true);
        JFrame frame = new JFrame();

        FlowLayout flowLayout = new FlowLayout();

        frame.setLayout(flowLayout);

        JTextArea textArea = new JTextArea(5, 30);
        JScrollPane jScrollPane = new JScrollPane(textArea);

        JLabel jLabel = new JLabel("Test");

        frame.add(jScrollPane);
        frame.add(jLabel);

        frame.setSize(100,400);
        frame.pack();
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
```

##### JSplitPane 分隔面板

**构造器**

- `JSplitPane()`
- `JSplitPane(int newOrientation)`
- `JSplitPane(int newOrientation, boolean newContinuousLayout)`
- `JSplitPane(int newOrientation, Component newLeftComponent, Component newRightComponent)`
- `JSplitPane(int newOrientation, boolean newContinuousLayout, Component newLeftComponent, Component newRightComponent)`
  - newOrientation参数 指定JSplitPane的分隔方向
     - JSplitPane.HORIZONTAL_SPLIT
     - JSplitPane.VERTICAL_SPLIT
  - newLeftComponent参数 指定位于JSplitPane左边或上边的组件
  - newRightComponent参数 指定位于JSplitPane右边或下边的组件

```java
package com.zjk.java;

import javax.swing.*;
import java.awt.*;

public class JSplitPaneTest {
    public static void main(String[] args) {
        JFrame.setDefaultLookAndFeelDecorated(true);

        JFrame frame = new JFrame("test");
        FlowLayout flowLayout = new FlowLayout();
        frame.setLayout(flowLayout);

        JLabel label = new JLabel("内容区域");
        JTextArea text = new JTextArea();
        //指定组件的最小尺寸
        JScrollPane jScrollPane = new JScrollPane(text);
        label.setMinimumSize(new Dimension(200,200));
        jScrollPane.setMinimumSize(new Dimension(200,200));

        //设置分面板
        JSplitPane jSplitPane = new JSplitPane(JSplitPane.VERTICAL_SPLIT,label,jScrollPane);
        //设置每个分面板的快速扩展按钮
        jSplitPane.setOneTouchExpandable(true);
        //设置分割条的位置
        jSplitPane.setDividerLocation(150);

        frame.add(jSplitPane);

        frame.setSize(300,400);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
```

##### JTabbedPane类 标签面板

**构造器**

- JTabbedPane()
- JTabbedPane(int tabPlacement)
   - tabPlacement 指定组件标签的显示位置，默认JTabbedPane.TOP 
   - JTabbedPane.TOP
   - JTabbedPane.BOTTOM
   - JTabbedPane.LEFT
   - JTabbedPane.RIGHT
- JTabbedPane(int tabPlacement, int tabLayoutPolicy) 
   - tabLayoutPolicy 指定布局
   - WRAP_TAB_LAYOUT
   - SCROLL_TAB_LAYOUT 

**添加组件**

- addTab(String title, Icon icon, Component Component, String tip)
   - 指定标签上的文字，指定标签上的图标，指定选择该标签时要显示的组件，指定标签的提示信息。
- addTab(String title, Icon icon, Conponent component)
- addTab(String title, Component component)

### 常用组件

#### JButton类  按钮

#### JLabel类  标签

- JLabel对象可以显示文本，图像或两者。 
- 可以通过设置垂直和水平对齐来指定标签的显示区域中标签内容对齐的位置。 
- 默认情况下，标签在其显示区域中垂直居中，纯文本标签是前缘对齐的，仅图像标签水平居中。文本位于图像的后端，文本和图像垂直对齐。

**构造器**

- `JLabel()`
   - 创建一个没有图像的 JLabel实例，标题为空字符串。
- `JLabel(Icon image)`
   - 使用指定的图像创建一个 JLabel实例。
- `JLabel(Icon image, int horizontalAlignment)`
   - 创建一个具有指定图像和水平对齐的 JLabel实例。
-  `JLabel(String text)`
   - 使用指定的文本创建一个 JLabel实例。
- `JLabel(String text, Icon icon, int horizontalAlignment)`
   - 创建具有 JLabel文本，图像和水平对齐的JLabel实例。
- `JLabel(String text, int horizontalAlignment)`
    - 创建一个具有指定文本和水平对齐的 JLabel实例。

#### 文本类

##### JTextArea类

- 可编辑的，无格式文本
- 默认不自动换行

```java
package com.zjk.java;

import java.awt.Color;
import java.awt.FlowLayout;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;

import javax.swing.JButton;
import javax.swing.JFrame;
import javax.swing.JLabel;
import javax.swing.JTextArea;

public class myGUI {
	public static void main(String[] args) {
		JFrame.setDefaultLookAndFeelDecorated(true);

		JFrame frame = new JFrame("myGUI");
		frame.setLayout(new FlowLayout());

		JTextArea textArea = new JTextArea("从这开始编辑");
		frame.add(textArea);

		JLabel label = new JLabel();
		frame.add(label);

		JButton button = new JButton("OK");
		frame.add(button);
		button.addMouseListener(new MouseAdapter() {
			@Override
			public void mouseClicked(MouseEvent e) {

				button.setForeground(Color.green);

				// getText() 从JTextArea对象中获取内容
				String text = textArea.getText();
				// setText() 设置内容
				label.setText(text);
			}
		});

		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
		frame.pack();
		frame.setVisible(true);
	}
}
```


#### 选择类

##### JComboBox类 选择框

![](C:/NoteBook/pictures/540921511236840.png =323x)

**构造器**

![](C:/NoteBook/pictures/243341411247616.png =414x)

**方法**

- `setModel(new DefaultComboBoxModel(数组))` 
   - 设置选项框的内容 
- `int getSelectedIndex()`
   - 返回当前选项框在setModel()的数组中的索引 

##### JCheckBox 复选框

![](C:/NoteBook/pictures/295231511240285.png =344x)

**构造器**

![](C:/NoteBook/pictures/18201311239583.png =518x)

##### JRadioButton 单选钮

![](C:/NoteBook/pictures/195341611232594.png =351x)

**构造器**

![](C:/NoteBook/pictures/376401311227450.png =518x)

##### ButtonGroup类 组合按钮

- 使选择之间互斥，一次只能选择一个。

![](C:/NoteBook/pictures/565802011250474.png =216x)

**构造器**

- ButtonGroup()

**方法**

![](C:/NoteBook/pictures/265392111248078.png =563x)

```java
package com.zjk.java1;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ItemEvent;
import java.awt.event.ItemListener;

public class GUITest {
    public static void main(String[] args) {
        JFrame.setDefaultLookAndFeelDecorated(true);
        JFrame frame = new JFrame("Test Title");
        frame.setSize(400,300);

        frame.setLayout(null);

        JLabel jobShow = new JLabel();
        jobShow.setBounds(20, 50, 100, 40);
        frame.add(jobShow);

        JComboBox job = new JComboBox();
        String[] jobs = {"军人", "学生", "教师", "办公", "无业"};
        job.setModel(new DefaultComboBoxModel(jobs));
        job.setBounds(50, 50, 100, 40);
        job.addItemListener(new ItemListener() {
            @Override
            public void itemStateChanged(ItemEvent e) {
                jobShow.setText(jobs[job.getSelectedIndex()]);
            }
        });
        frame.add(job);

        JRadioButton choice1 = new JRadioButton("断网");
        choice1.setBounds(20,10,100,40);
        frame.add(choice1);
        JRadioButton choice2 = new JRadioButton("有网");
        choice2.setBounds(70,10,100,40);
        frame.add(choice2);
        ButtonGroup choices = new ButtonGroup();
        choices.add(choice1);
        choices.add(choice2);

        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
```

### 菜单组件

- JComponent
    - JMenuBar
    - JPopupMenu
    - JAbsta\ractButton
    - JSeparator
        - JMenuItem
           - JMenu
           - JCheckboxMenuItem
           - JRadioButtonMenuItem 

**结构**

- 根菜单--> 次级菜单 -->菜单选项

#### 根菜单

##### JMenuBar 下拉式菜单

![](c:/notebook/pictures/Snipaste_2022-11-21_10-25-00.png =300x)



##### JPopupMenu 弹出式菜单/上下文菜单

#### 次级菜单

##### JMenu

#### 菜单选项

##### JMenuItem

![](c:/notebook/pictures/Snipaste_2022-11-21_11-24-19.png =500x)

```java
JMenuBar toolbars = new JMenuBar();
frame.setJMenuBar(toolbars);

//设置-菜单
JMenu settingMenu = new JMenu("设置(s)");
settingMenu.setMnemonic('s');
toolbars.add(settingMenu);
//设置选项-背景颜色
JMenu backGround = new JMenu("背景颜色");
settingMenu.add(backGround);
//粉色
JMenuItem setBackPink = new JMenuItem("粉色(p)", 'p');
setBackPink.addMouseListener(new MouseAdapter() {
    @Override
    public void mouseClicked(MouseEvent e) {
        frame.setBackground(Color.RED);
    }
});
backGround.add(setBackPink);
```

### 布局管理器

- FlowLayout 流式布局管理器 
   - Panel和Applet的默认
- BorderLayout 边界布局管理器
   - Window、Dialog、Frame的默认 
- GridLayout 网格布局管理器
- CardLayout 卡片布局管理器
- GirdBagLayout 网格包布局管理器
- BoxLayout 箱式布局管理器

**设置布局管理器**

- 每个容器都有一个默认的布局管理器，可以通过`setLayout(布局管理器对象)`方法，指定布局管理器。

#### FlowLayout 流式布局管理器

**构造器**

- `FlowLayout()` 
  - 构造一个新的 FlowLayout中心对齐和默认的5单位水平和垂直间隙 
- `FlowLayout(int align)`
  - 构造一个新的 FlowLayout具有指定的对齐和默认的5单位水平和垂直间隙。 
  - `FlowLayout.CENTER` 居中对齐
  - `FlowLayout.LEFT` 左对齐
  - `FlowLayout.RIGHT` 右对齐
  - `FlowLayout.LEADING` 该值表示组件的每一行应该对齐到容器方向的前端，例如从左到右的方向向左。
  - `FlowLayout.TRAILIN` 该值表示组件的每一行应该对齐到容器方向的后端，例如从左到右的方向向右。
- `FlowLayout(int align, int hgap, int vgap)`
  - 创建一个新的流程布局管理器，具有指示的对齐方式和指示的水平和垂直间距。

```java
package com.zjk.java;

import javax.swing.*;
import java.awt.*;

public class FlowLayoutDemo {
    public static void main(String[] args) {
        JFrame.setDefaultLookAndFeelDecorated(true);

        JFrame frame = new JFrame("流式布局管理器测试");

        //new FlowLayout()默认的流式布局管理器
        FlowLayout flowLayout = new FlowLayout(FlowLayout.CENTER,100,200);
        //setLayout() 指定布局管理器
        frame.setLayout(flowLayout);

        JLabel label1 = new JLabel("序号1");
        JLabel label2 = new JLabel("序号2");

        frame.add(label1);
        frame.add(label2);


        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.pack();
        frame.setVisible(true);
    }
}
```

```java
package com.zjk.java;

import javax.swing.*;
import java.awt.*;

public class FlowLayoutWindow extends JFrame {

    public FlowLayoutWindow() {
        //FlowLayoutWindow的setLayout()方法
        //创建流式布局管理器
        setLayout(new FlowLayout());

        //从左到右依次添加组件
        //FlowLayoutWindow的add()方法
        add(new JLabel("Buttons:"));
        //new JButton() 新建按钮
        add(new JButton("Button 1"));
        add(new JButton("2"));
        add(new JButton("Button 3"));
        add(new JButton("Long-Named Button 4"));
        add(new JButton("Button 5"));
    }

    public static void main(String args[]) {
        FlowLayoutWindow window = new FlowLayoutWindow();
        
        window.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        window.setTitle("FlowLayoutWindow Application");
        window.pack();//窗口的大小设置为适合组件最佳尺寸与布局所需的空间。
        window.setVisible(true);
    }
}
```

#### BorderLayout 边界布局管理器

**构造器**

- `BorderLayout()`
  - 组件之间没有水平间隙与垂直间隙 
- `BorderLayout(int hgap, int vgap)`
  - 指定组件之间水平间隙与垂直间隙 

**注**

- 当用户改变窗口大小时，各个组件的相对位置不变，而组件的大小改变

**add() 使用Containeri类的**

- `public Component add(String name, Component comp)`
- `public void add(Component comp, Object constraints)`

- BorderLayout布局管理器中，在向容器中加入组件时，要指定各个摆放的方位，否则组件将不能显示。
  - 默认放在中心位置 
- 若添加时，要添加的位置上已经有组件，则会执行覆盖

comp 组件

name和contraints参数: 

| 参数 | 值(String)   |
| :--- | :--- |
| BorderLayout.WEST     | "West"     |
|   BorderLayout.EAST   |    "East"  |
|   BorderLayout.NORTH   |   "North"   |
|   BorderLayout.SOUTH   |   "South"   |

```java
package com.zjk.java;

import javax.swing.*;
import java.awt.*;

public class BorderLayoutTest {
    public static void main(String[] args) {
        JFrame.setDefaultLookAndFeelDecorated(true);

        JFrame frame = new JFrame("边界布局管理器测试");

        //创建边界布局管理器
        BorderLayout borderLayout = new BorderLayout();
        frame.setLayout(borderLayout);

        JLabel labelWest = new JLabel("西边");
        JButton buttonEast = new JButton("东边");
        JButton buttonNorth = new JButton("北边");
        JButton buttonSouth = new JButton("南边");

        frame.add(labelWest,BorderLayout.WEST);
        frame.add(buttonEast,BorderLayout.EAST);
        frame.add(BorderLayout.NORTH,buttonNorth);
        frame.add(BorderLayout.SOUTH,buttonSouth);

        JButton buttonCenter = new JButton("中心");
        //默认放在中心位置，同时会覆盖原有的。
        frame.add(buttonCenter);

        frame.pack();
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
```

#### setLayout(null) 无布局管理器

**方法**

- setLocation(int x, int y)
   - 设置组件大小
- setSize(int width, int height)
   - 设置组件位置
- setBounds(int x, int y, int width, int height)
   - 设置组件大小位置

## GUI中的事件处理

### 监听器

**委托方式的事件处理机制**

- 监听器:每个事件有一个相应的监听器接口，定义了接收事件的方法。实现该接口的类，可作为监听器注册。
- 每个组件都注册有一个或多个监听器（类），该监听器包含了能接收和处理事件的事件处理。
- 事件对象只向已注册的监听器报告

1. 实现监听器接口 `ActionListener`
2. 实现监听器接口的方法 `public void actionPerformed(){}`
3. 注册监听器到组件中 `addXxxListener(监听器的实现类对象)`

```java
package com.zjk.java;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class ListenerTest {
    public static void main(String[] args) {
        JFrame.setDefaultLookAndFeelDecorated(true);
        JFrame frame = new JFrame("监听器测试");
        frame.setSize(200,100);
        frame.setLayout(new FlowLayout(FlowLayout.CENTER));

        JButton button = new JButton("Press");
        //给组件添加监听器
        button.addActionListener(new ButtonPress());

        frame.add(button);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}

//定义监听器类
class ButtonPress implements ActionListener{

    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("Action occurred");
        System.out.println("标签：" + e.getActionCommand());
    }
}
```

### 事件类

![](C:/NoteBook/pictures/160351619227442.png =414x)

| 事件             | 操作                                         | 接口                 | 适配器类            | 方法                                                                                                                                   |
| :-------------- | :------------------------------------------- | :------------------ | :----------------- | :------------------------------------------------------------------------------------------------------------------------------------ |
| ActionEvent     | 激活组件操作                                  | ActionListener      |                    | ActionPerformed()                                                                                                                     |
| AdjustmentEvent | 移动滚动条                                    | AdjustmentListener  |                    | adjustmentValueChanged()                                                                                                              |
| ComponentEvent  | 组件移动、缩放、显示、隐藏等                    | ComponentListener   | ComponentAdapter   | componentHidden() / componentMoved() / componentResized() / componentShown()                                                          |
| ContainerEvent  | 容器中增加或删除组件                           | ContainerListener   | ContainerAdapter   | componentAdded() / componentRemoved()                                                                                                 |
| FocusEvent      | 组件得到或失去聚焦                             | FocusListener       | FocusAdapter       | focusGained() / focusLost()                                                                                                           |
| ItemEvent       | 条目状态改变                                  | ItemListener        |                    | itemStateChanged()                                                                                                                    |
| KeyEvent        | 键盘输入                                      | KeyListener         | KeyAdapter         | keyPressed() / keyReleased() / keyTtyped()                                                                                            |
| MouseEvent      | 单击鼠标                                      | MouseListener       | MouseAdapter       | mouseClicked() / mouseEntered() / mouseExited() / mousePressed() / mouseReleased()                                                    |
| -               | 移动鼠标                                      | MouseMotionListener | MouseMotionAdapter | mouseDragged() / mouseMoved()                                                                                                         |
| TextEvent       | 文本域或文本区值改变                           | TestListener        |                    | testValueChanged()                                                                                                                    |
| WindowEvent     | 窗口激活、去活、打开、关闭、最小化、从图标恢复等 | WindowListener      | WindowAdapter      | windowActivated() / windowClosed() / windowClosing() / windowDeactivated() / windowDeiconified() / windowIconified() / windowOpened() |

**MoseEvent**
- MouseListener / MouseAdapter
  - mouseClicked() 鼠标点击之后
  - mouseEntered()
  - mouseExited() 鼠标离开时
  - mousePressed() 鼠标按压时
  - mouseReleassed() 

```java
package com.zjk.java;

import javax.swing.*;
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;

public class JEventTest {
    public static void main(String[] args) {
        JFrame.setDefaultLookAndFeelDecorated(true);
        JFrame frame = new JFrame("测试事件");
        FlowLayout flowLayout = new FlowLayout();
        frame.setLayout(flowLayout);
        frame.setSize(new Dimension(600,700));

        JLabel label01 = new JLabel("测试1");
        JButton button01 = new JButton("按钮1");
        JTextArea textArea01 = new JTextArea("输入：");

        JScrollPane textArea01Scroll = new JScrollPane(textArea01);
        textArea01Scroll.setSize(400,300);
        //添加事件监听器
        textArea01Scroll.addMouseListener(new MouseListener() {
            @Override
            public void mouseClicked(MouseEvent e) {
                System.out.println("鼠标点击");
                textArea01Scroll.setForeground(Color.BLUE);
                textArea01Scroll.setBackground(Color.RED);
            }

            @Override
            public void mousePressed(MouseEvent e) {
                System.out.println("鼠标Pressed");
                textArea01Scroll.setForeground(Color.GRAY);
            }

            @Override
            public void mouseReleased(MouseEvent e) {
                System.out.println("鼠标Released");
                textArea01Scroll.setForeground(Color.GREEN);
            }

            @Override
            public void mouseEntered(MouseEvent e) {

            }

            @Override
            public void mouseExited(MouseEvent e) {

            }
        });

        JSplitPane button01AndLabel01 = new JSplitPane(JSplitPane.VERTICAL_SPLIT,label01,button01);
        button01AndLabel01.addMouseListener(new MouseAdapter(){
            @Override
            public void mouseClicked(MouseEvent e) {
                button01.setBackground(Color.CYAN);
            }

            @Override
            public void mouseExited(MouseEvent e) {
                button01.setBackground(Color.RED);
            }
        });

        frame.add(textArea01Scroll);
        frame.add(button01AndLabel01);

        frame.pack();
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
```

```java
//监听多种事件的监听器
package com.zjk.java;

import javax.swing.*;
import java.awt.*;
import java.awt.event.*;

public class ComplexListener implements
        MouseMotionListener, MouseListener, ActionListener {
    JFrame f;
    JTextArea tf;
    JButton bt;
    int number = 1;


    public ComplexListener() {
        JLabel label = new JLabel("click and Drag the mouse");
        f = new JFrame("Complex Listener");
        tf = new JTextArea();
        bt = new JButton("退出");

        tf.addMouseMotionListener(this);
        tf.addMouseListener(this);
        bt.addActionListener(this);

        f.add(label, BorderLayout.NORTH);
        f.add(tf, BorderLayout.CENTER);
        f.add(bt, BorderLayout.SOUTH);
        f.setSize(300, 200);
        f.setVisible(true);
    }

    // MouseMotionListener 的方法。
    public void mouseDragged(MouseEvent e) {
        String s = number++ + " " + "The mouse Dragged: x= " + e.getX() + " y=" + e.getY() + "\n";
        tf.append(s);
    }

    //MouseListener 的方法。
    public void mouseEntered(MouseEvent e) {
        String s = number++ + " " + "The mouse entered" + "\n";
        tf.append(s);
    }

    public void mouseClicked(MouseEvent e) {
        String s = number++ + " " + "The mouse clicked." + "\n";
        tf.append(s);
    }

    public void mouseExited(MouseEvent e) {
        String s = number++ + " " + "The mouse exit." + "\n";
        tf.append(s);
    }

    // 未使用的 MouseMotionListener 方法。.
    public void mouseMoved(MouseEvent e) {
    }

    // 未使用的 MouseListener 方法。
    public void mouseReleased(MouseEvent e) {
    }

    public void mousePressed(MouseEvent e) {
    }

    //ActionListener的方法。
    public void actionPerformed(ActionEvent e) {
        System.exit(0);
    }

    public static void main(String args[]) {
        ComplexListener two = new ComplexListener();
    }
}
```

```java
package com.zjk.java;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class MultiListener extends JFrame implements ActionListener {

    JTextArea topTextArea;
    JTextArea bottomTextArea;
    JButton button1, button2;

    public MultiListener(String s) {
        super(s);

        JLabel l = null;
        GridBagLayout gridbag = new GridBagLayout();
        GridBagConstraints c = new GridBagConstraints();
        setLayout(gridbag);    //Frame设置为GridBagLayout布局管理器。

        c.fill = GridBagConstraints.BOTH;
        c.gridwidth = GridBagConstraints.REMAINDER;
        l = new JLabel("监听器听到的:");
        gridbag.setConstraints(l, c);
        add(l);

        c.weighty = 1.0;
        topTextArea = new JTextArea(5, 20);
        topTextArea.setEditable(false);
        gridbag.setConstraints(topTextArea, c);
        add(topTextArea);

        c.weightx = 0.0;
        c.weighty = 0.0;
        l = new JLabel("偷听者听到的:");
        gridbag.setConstraints(l, c);
        add(l);

        c.weighty = 1.0;
        bottomTextArea = new JTextArea(5, 20);
        bottomTextArea.setEditable(false);
        gridbag.setConstraints(bottomTextArea, c);
        add(bottomTextArea);

        c.weightx = 1.0;
        c.weighty = 0.0;
        c.gridwidth = 1;
        c.insets = new Insets(10, 10, 0, 10);
        button1 = new JButton("啦 啦 啦");
        gridbag.setConstraints(button1, c);
        add(button1);

        c.gridwidth = GridBagConstraints.REMAINDER;
        button2 = new JButton("你别说话!");
        gridbag.setConstraints(button2, c);
        add(button2);

        //当前MultiListener对象同时监听两个Button的事件。
        button1.addActionListener(this);
        button2.addActionListener(this);

        //为第二个Button再注册一个监听器。
        button2.addActionListener(new Eavesdropper());

        //向窗口注册响应关闭窗口操作的监听器。
        addWindowListener(new WindowAdapter() {
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });

        pack();
        setVisible(true);
    }

    public void actionPerformed(ActionEvent e) {
        topTextArea.append(e.getActionCommand() + "\n");
    }

    //第二个Button的监听器类。
    class Eavesdropper implements ActionListener {
        public void actionPerformed(ActionEvent e) {
            bottomTextArea.append("OK," + e.getActionCommand() + "\n");
        }
    }

    public static void main(String[] args) {

        MultiListener m = new MultiListener("Multilistener example");

    }
}
```

### 事件适配器

- Adapter类实现了相应Listener接口，但所有方法体都是空的。
- 用户可以把自己的监听器类声明为adapter类的子类，便可以只重写需要的方法

## WindowBuilder 插件的使用

**1.创建文件**

![](C:/NoteBook/pictures/13771911236831.png =487x)
![](C:/NoteBook/pictures/574291911232585.png =402x)
![](C:/NoteBook/pictures/44372111250465.png =408x)

**2.进入可视化编辑**

- Design

![](C:/NoteBook/pictures/227493211245571.png =718x)

# JDBC

- JDBC驱动管理器是Java虚拟机的一个组成部分，即负责针对各种类型DBMS的JDBC驱动程序，也负责和用户的应用程序交互，为Java建立数据库连接。
- Java应用程序通过JBDC API向JDBC驱动管理器发出请求，指定要安装的JDBC驱动程序类型和数据源。驱动管理器根据这些要求装载合适的JDBC驱动程序并使该驱动连通相应的数据源。一旦连接成功，该JDBC驱动就负责Java应用与该数据源的一切交互。

## 准备：JDBC驱动

### Oracle

1. 到Oracle的路径 C:\app\zjk10\product\11.2.0\dbhome_1\jdbc\lib
2. 在IDEA中添加ojdbc6.jar和ojdbc6_g.jar

![](C:/NoteBook/pictures/114651322247614.png =300x)
![](C:/NoteBook/pictures/87841222239581.png =512x)
![](C:/NoteBook/pictures/340241022221155.png)

## 基本方法

**DriverManager类**

1. 建立连接
- Class.forName("驱动")   //JDBC4.0新特性，可以忽略该语句
   - 加载相应数据库驱动(DriverManager类)的实例并注册到驱动管理器 
   - Oracle驱动 `oracle.jdbc.driver.OracleDriver`
- getConnection(String url, String user, String passwd) 
   - DriverManger类的方法，连接数据库
   - url : 指定使用的数据库访问协议和数据源
      - 格式`jdbc:数据库访问协议:数据源`
      - `jdbc:oracle:thin:@localhost:1521:ORCL`
         - 即：协议(jdbc):子协议(oracle:thin):数据源标识(@localhost:1521:ORCL)
         - `jdbc` 即Java数据库连接，实质是一个Java API，可以为多种关系数据库提供统一访问，它由一组用Java语言编写的类和接口组成
         - `oracle:thin` 指出连接的是oracle数据库，同时连接方式为thin方式，即瘦方式，不需要客户端的方式。与之对的另一种连接方式为胖方式：cli，这种方式需要安装客户端。 
         - `@localhost:1521:orcl`
            - `localhost`  数据库的地址，非本地时可按为数据库的IP地址；
            - `1521` orcal数据库默认的监听端口，可换为其他监听端口；
            - `orcl` orcal数据库默认的一个实例SID，可按为其他实例名。

**Statement类**

2. 执行SQL语句
- createStatement()
   - 创建Statement对象
- executeQuery(String sql)
   - 执行查询的SQL语句，返回一个结果集ResultSet对象 
- executeUpdate(String sql)
   - 执行INSERT、UPDATE、DELETE、DDL语句等的SQL语句
- 注：
   - SQL语句的空格不能忽略，即使字符串换行了，也必须留有空格。
   - SQL语句结尾不能有分号 `;`  

**ResultSet类**
 
- 在ResultSet的对象中，结果集通过游标控制具体记录的访问，游标指向结果集中的当前记录。

3. 处理结果集（ResultSet类的对象）
- next() 
   - 将游标移到下一行，并将该行作为用户操作的当前行。
   - 如果没有下一行，则返回false
- getXxx(String  colName)   
   - 通过列名，获取指定列的值
   - Xxx表示该列的数据类型 
- getXxx(int columnIndex)
   - 通过结果集中列的序号，获取指定的值。
   - 以1开始，而不是0
   
4. 关闭数据库的连接
- 一般先关闭Statement对象
- close()

```java
package com.zjk.java1;

import java.sql.*;

public class JDBCTest {
    public static void main(String[] args) {
        try {
            //1.获取驱动
            //Oracle驱动 oracle.jdbc.driver.OracleDriver
            Class.forName("oracle.jdbc.driver.OracleDriver");
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        }
        Connection connection = null;
        Statement statement = null;

        try {
            //2.建立连接
            //Oracle的url jdbc:oracle:thin:@localhost:1521:ORCL
            String url = "jdbc:oracle:thin:@localhost:1521:ORCL";
            connection = DriverManager.getConnection(url, "scott", "tiger");
            statement = connection.createStatement();

            //3.执行SQL语句
            //查询
            String selectSQL = "SELECT employee_id,department_id,last_name " +
                    "FROM employees " +
                    "WHERE department_id = 80";
            ResultSet resultSet = statement.executeQuery(selectSQL);
            //4.处理结果集
            int emp_id;
            int dep_id;
            String emp_name;
            while (resultSet.next()) {
                emp_id = resultSet.getInt(1);
                dep_id = resultSet.getInt(2);
                emp_name = resultSet.getString("last_name");
                System.out.println(emp_id + " : " + dep_id + " : " + emp_name);
            }
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                //5.关闭连接
                statement.close();
                connection.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

## 高级特性

### 预编译语句 PraparedStatement接口

**对比**

- 在创建Statement对象时，并未指定和执行SQL语句，在执行SQL语句时，需要将SQL语句发送给DBMS，再由DBMS进行编译后执行。
- 在创建PreparedStatement对象时，就指定了SQL语句并立刻发送给DBMS进行编译，当该PraparedStatement对象执行SQL语句时，DBMS只需要执行已经编译好的语句即可。

**创建预编译语句**

- prepareStatement(String sql)
  - 创建PreparedStatement对象
  - 该SQL语句可带有`参数表示符 ? `，表示SQL语句中的参数。

**设置预编译语句中的SQL语句参数**

- setXxx(int paramIndex, Xxx x)
  -  通过在SQL语句中参数的序号paramIndex（从1开始），设置相应数据类型Xxx的参数值x。
  - 预编译语句中的SQL语句参数经过设置后，会一直保留，直到被设为新值或调用了clearParameters()方法
- clearParameters()
  - 清除预编译语句的SQL语句的所有参数设置。 

```java
import java.sql.*;

public class JDBCTest2 {
    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try {
            Class.forName("oracle.jdbc.driver.OracleDriver");

            String url = "jdbc:oracle:thin:@localhost:1521:ORCL";
            connection = DriverManager.getConnection(url, "scott", "tiger");

            //创建预编译语句
            String sql = "SELECT employee_id,department_id,last_name " +
                    "FROM employees " +
                    "WHERE department_id = ? " +
                    "  AND salary < ?";
            preparedStatement = connection.prepareStatement(sql);

            //设置预编译语句SQL语句的参数
            preparedStatement.setInt(1, 10);
            preparedStatement.setInt(2, 10000);

            ResultSet resultSet = preparedStatement.executeQuery();

            while (resultSet.next()) {
                System.out.println(resultSet.getInt("employee_id") + " : " + resultSet.getInt("department_id") + " : " + resultSet.getString("last_name"));
            }
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                if (preparedStatement != null)
                    preparedStatement.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
            try {
                if (connection != null)
                    connection.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

### 存储过程的调用 CallableStatement类

**创建CallableStatement对象**

- prepareCall("{call 存储过程名}")

**执行**

- executeQuery()
   - 存储过程中执行的是查询时
- execute()
   - 存储过程中有更新操作时
- execute()
   - 存储过程中既有查询有更新时。 

```java
package com.zjk;

import java.sql.CallableStatement;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class JDBCTest3 {
    public static void main(String[] args) {
        Connection connection = null;
        CallableStatement callableStatement = null;
        try {
            Class.forName("oracle.jdbc.driver.OracleDriver");

            connection = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:ORCL","scott","tiger");
            String callSQL = "{call emp_id_sal(1002,'小红','女')}";
            callableStatement = connection.prepareCall(callSQL);
            connetion.setAutoCommit(true);

            callableStatement.executeUpdate();
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                callableStatement.close();
                connection.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }

    }
}
```

### 自动提交 Connection接口

**Connection接口的方法**

- setAutoCommit(boolean b)
  - 自动提交 
- commit()
  - 显示调用提交COMMIT
-  rollback()
  - 显示调用回滚ROLLBACK

```java
package com.zjk.java2;

import java.sql.*;

public class JDBCTest4 {
    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        try {
            Class.forName("oracle.jdbc.driver.OracleDriver");
            connection = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:ORCL", "scott", "tiger");

            connection.setAutoCommit(true);

            statement = connection.createStatement();

            ResultSet resultSet = statement.executeQuery("SELECT * FROM jdbc_table");

            while (resultSet.next()) {
                System.out.println(resultSet.getInt("id") + " : " + resultSet.getString("name") + " : " + resultSet.getString("sex"));
            }
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                statement.close();
                connection.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

## JDBC2.0和JDBC3.0新特性

### 游标定位的操作 ResultSet类

- last()
   - 将游标定位到最后一行 

### 直接通过Java实现更新操作 ResultSet类

- createStatement(ResultSet.TYPE_SCROILL_SENSITIVE, ResultSet.CONCUR_UPDATABLE)
   - 设置返回的结果集X 
- updateXxx(String colName, Xxx x)
   - 根据指定的列名colName来更新相应的数据（Xxx数据类型的x）
   - 指定的列名必须是在JDBC的SQL语句中出现的(不包括`*`)，否则：java.sql.SQLException: 对只读结果集的无效操作
- updateRow() 
   - 使更新操作生效

```java
package com.zjk.java2;

import java.sql.*;

public class JDBCTest4 {
    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        try {
            Class.forName("oracle.jdbc.driver.OracleDriver");
            connection = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:ORCL", "scott", "tiger");

            connection.setAutoCommit(true);

            statement = connection.createStatement(ResultSet.TYPE_SCROLL_SENSITIVE, ResultSet.CONCUR_UPDATABLE);

            ResultSet resultSet = statement.executeQuery("SELECT id,name,sex FROM jdbc_table");

            resultSet.last();
            resultSet.updateString("name", "小均");
            resultSet.updateRow();

        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                statement.close();
                connection.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

### 批量更新操作 Statement类

- addBatch(Stirng sql)
  - 向列表中添加SQL语句，将列表中的SQL语句作为一个单元发送到DBMS进行批量执行操作。
- executeBatch()
  - 执行批量操作 

```java
package com.zjk.java2;

import java.sql.*;

public class JDBCTest4 {
    public static void main(String[] args) {
        Connection connection = null;
        Statement statement = null;
        try {
            Class.forName("oracle.jdbc.driver.OracleDriver");
            connection = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:ORCL", "scott", "tiger");

            connection.setAutoCommit(true);

            statement = connection.createStatement();

            statement.addBatch("INSERT INTO jdbc_table VALUES(1001,'Jac','男')");
            statement.addBatch("INSERT INTO jdbc_table VALUES(1003,'Tom','男')");
            statement.addBatch("INSERT INTO jdbc_table VALUES(1004,'Ju','女')");

            statement.executeBatch();
        } catch (ClassNotFoundException e) {
            throw new RuntimeException(e);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        } finally {
            try {
                statement.close();
                connection.close();
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

## JDBC4.0新特性

### 驱动和连接管理

- 不再需要Class.forName()来显示地加载驱动。可以直接忽略。
- 当建立数据库连接时，DriverManager.getConnection()，自动再程序地CLASSPATH中找到驱动。

### 连锁异常处理的获取 forEach

```java
try{
    ...
}catch(SQLException sx){
    for(Throwable e: sx){
        System.err.println(e)
    }
}
```

### 数据类型支持

# Java8新特性

## Lambda表达式

- 在Java中的本质：作为函数式接口的实例。

**格式**

`->` Lambda操作符，箭头操作符
   - `->` 左边：Lambda形参列表（即：接口中的抽象方法的形参列表）
   - `->` 右边：Lambda体（其实就是重写的抽象方法的方法体）
- 参数名称可以随便定

**语法格式**

- 语法格式1：无参无返回值
- 语法格式2：有参无返回值
- 语法格式3：类型推断，数据类型可以省略，可由编译器推断得出
- 语法格式4：Lambda如果只需要一个参数，参数的小括号可以省略
- 语法格式5：Lambda需要两个或以上的参数，多条执行语句，并且可以有返回值
- 语法格式6：当Lambda体只有一条语句时，return与大括号可以省略

1. 左边：Lambda形参列表参数类型可以省略（类型推断），如果Lambda形参列表只有一个参数，其一对()可以省略。
2. 右边：Lambda体应该使用一对{}包裹，如果Lambda体只有一条执行语句（可能是return语句），可以省略这对{}和return关键字。(要么一起省略，要么不省略)

```java
package com.zjk.java1;

import org.junit.jupiter.api.Test;

import java.util.Comparator;
import java.util.Iterator;
import java.util.function.Consumer;

public class LambdaTest1 {
    @Test
    public void test1() {
        //语法格式1：无参无返回值
        Runnable r1 = new Runnable() {
            @Override
            public void run() {
                System.out.println("我是中国人");
            }
        };

        r1.run();

        //Lambda表达式
        Runnable r2 = () -> System.out.println("我是中国人");
        r2.run();
    }

    @Test
    public void test2() {
        //语法格式2：有参无返回值
        Consumer<String> consumer = new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println("Consumer" + s);
            }
        };
        consumer.accept("客户端");

        //lambda表达式
        Consumer<String> consumer1 = (String s) -> {
            System.out.println(s);
        };
        consumer1.accept("客服端 Lambda");
    }

    @Test
    public void test3() {
        //语法格式3：类型推断，数据类型可以省略，可由编译器推断得出。
        //lambda表达式
        Consumer<String> consumer1 = (s) -> {
            System.out.println(s);
        };
        consumer1.accept("客服端 Lambda");
    }

    @Test
    public void test4() {
        //语法格式4：Lambda如果只需要一个参数，参数的小括号可以省略。
        Consumer<String> consumer = s -> {
            System.out.println(s);
        };
        consumer.accept("你好");
    }

    @Test
    public void test5() {
        //语法格式5：Lambda需要两个或以上的参数，多条执行语句，并且可以有返回值
        Comparator<Integer> comparator = new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return Integer.compare(01, 02);
            }
        };

        //Lambda表达式
        Comparator<Integer> comparator1 = (o1, o2) -> {
            System.out.println(o1);
            System.out.println(o2);
            return o1.compareTo(o2);
        };
        System.out.println(comparator1.compare(21, 12));
    }

    @Test
    public void test6() {
        //语法格式6：当Lambda体只有一条语句时，return与大括号可以省略。
        Comparator<Integer> comparator = (o1, o2) -> Integer.compare(o1, o2);
        System.out.println(comparator.compare(12, 21));
    }
}
```

## 函数式接口

- OOF 面向函数编程
- 如果一个接口中只声明了一个抽象方法，则此接口就是函数式接口。
    - 注解：`@FunctionalInterface`，(就算不加该注解也是，注解Annotation的使用)
- 可以通过Lambda表达式创建该接口的实例
    - Lambda表达式的本质 
- 在`java.util.function`包下定义了Java8丰富的函数式接口

```java
@FunctionalInterface
public interface MyInterface {
    void method();
}
```

![](C:/NoteBook/pictures/Snipaste_2022-11-17_14-48-16.png =600x)

![](C:/NoteBook/pictures/Snipaste_2022-11-17_14-52-04.png =600x)

```java
package com.zjk.java1;

import org.junit.jupiter.api.Test;

import java.awt.event.ActionListener;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;
import java.util.function.Predicate;

public class LambdaTest2 {
    @Test
    public void test1() {
        happyTime(500, money -> System.out.println("花了" + money));
    }

    public void happyTime(double money, Consumer<Double> consumer) {
        consumer.accept(money);
    }

    @Test
    public void test2() {
        List<String> list = new ArrayList();
        list.add("java");
        list.add("python");
        list.add("redius");

        System.out.println(filterString(list, s -> s.contains("j")));
    }

    //根据给定的规则，过滤集合中的字符串。此规则由Predicate的方法决定
    public List<String> filterString(List<String> list, Predicate<String> predicate) {
        ArrayList<String> filterLister = new ArrayList<>();
        for (String s : list) {
            if (predicate.test(s)) {
                filterLister.add(s);
            }
        }
        return filterLister;
    }
}
```

## 方法引用

- 当要传递给Lambda体的操作，已经有实现的方法了，可以使用方法引用！
- 方法引用可以看做是Lambda表达式深层次的表达。换句话说，方法引用就是Lambda表达式，也就是函数式接口的一个实例，通过方法的名字来指向一个方法，可以认为是Lambda表达式的一个语法糖。

**要求：**

- 实现接口的抽象方法的参数列表和返回值类型，必须与方法引用的方法的参数列表和返回值类型保持一致！

**格式：** 

- 使用操作符 “::” 将类(或对象) 与 方法名分隔开来。
- 如果有返回值，返回值的类型也需要在泛型中声明，按顺序在泛型声明的最后。

**如下三种主要使用情况：**

- `对象::实例方法名`
- `类::静态方法名`
- `类::实例方法名`
   - 此时，在方法调用中，第一个参数作为第二个参数的调用者 

```java
package com.zjk.java2;

import org.junit.jupiter.api.Test;

import java.io.PrintStream;
import java.util.Comparator;
import java.util.function.*;

/**
 * 方法引用的使用
 * <p>
 * Created by shkstart.
 */
public class MethodRefTest {

    // 情况一：对象 :: 实例方法
    //Consumer中的void accept(T t)
    //PrintStream中的void println(T t)
    @Test
    public void test1() {
        Consumer<String> consumer = str -> System.out.println(str);

        //方法引用
        PrintStream ps = System.out;
        Consumer<String> consumer1 = ps::println;
        consumer1.accept("你好");
    }

    //Supplier中的T get()
    //Employee中的String getName()
    @Test
    public void test2() {
        Employee employee = new Employee(1001, "Tom", 24, 26000);
        Supplier<String> stringSupplier = () -> employee.getName();

        //方法引用
        Supplier<String> supplier = employee::getName;
        System.out.println(supplier.get());
    }

    // 情况二：类 :: 静态方法
    //Comparator中的int compare(T t1,T t2)
    //Integer中的int compare(T t1,T t2)
    @Test
    public void test3() {
        Comparator<Integer> comparator = (i1, i2) -> Integer.compare(i1, i2);

        //方法引用
        Comparator<Integer> comparator1 = Integer::compare;
        System.out.println(comparator1.compare(12, 23));
    }

    //Function中的R apply(T t)
    //Math中的Long round(Double d)
    @Test
    public void test4() {
        Function<Double, Long> function = Math::round;
        //需要与方法的参数列表和返回值类型一致
        System.out.println(function.apply(23.32));
    }

    // 情况三：类 :: 实例方法
    //在方法调用中，第一个参数作为第二个参数的调用者
    // Comparator中的int comapre(T t1,T t2)
    // String中的int t1.compareTo(t2)
    @Test
    public void test5() {
        Comparator<String> comparator = (t1, t2) -> t1.compareTo(t2);

        //方法引用
        Comparator<String> comparator1 = String::compareTo;

        System.out.println(comparator1.compare("12", "你好"));
    }

    //BiPredicate中的boolean test(T t1, T t2);
    //String中的boolean t1.equals(t2)
    @Test
    public void test6() {
        BiPredicate<String, String> biPredicate = String::equals;
        System.out.println(biPredicate.test("test", "test"));
    }

    // Function中的R apply(T t)
    // Employee中的String getName();
    @Test
    public void test7() {
        Function<Employee, String> function = Employee::getName;
        System.out.println(function.apply(new Employee(1002, "Tom", 24, 24000)));
    }
}
```

### 构造器引用

- 和方法引用类似，函数式接口的抽象方法的参数列表和构造器的形参列表一致。抽象方法的返回值即为构造器所属类的类型。

```java
package com.zjk.java2;

import org.junit.jupiter.api.Test;

import java.io.File;
import java.util.Objects;
import java.util.function.BiFunction;
import java.util.function.Function;
import java.util.function.Supplier;

/**
 * 一、构造器引用
 * <p>
 * 二、数组引用
 * <p>
 * <p>
 * Created by shkstart
 */
public class ConstructorRefTest {
    //构造器引用
    //Supplier中的T get()
    @Test
    public void test1() {
        Supplier<Employee> supplier = () -> new Employee();

        //构造器引用 空参构造器
        Supplier<Employee> supplier1 = Employee::new;
        System.out.println(supplier1.get());
    }

    //Function中的R apply(T t)
    @Test
    public void test2() {
        Function<Integer, Employee> function1 = id -> new Employee(id);
        System.out.println(function1.apply(1001));

        //构造器引用 带一个参数的构造器
        Function<Integer, Employee> function = Employee::new;
        System.out.println(function.apply(1002));

    }

    //BiFunction中的R apply(T t,U u)
    @Test
    public void test3() {
        //构造器引用 带两个参数的构造器
        BiFunction<Integer, String, Employee> biFunction = Employee::new;
        System.out.println(biFunction.apply(1003, "Jac"));
    }
}
```

### 数组引用 

```java
//数组引用  类似于构造器引用
//Function中的R apply(T t)
@Test
public void test4() {
    Function<Integer, String[]> function1 = length -> new String[length];
    String[] arr1 = function1.apply(5);

    //数组引用
    Function<Integer, String[]> function = String[]::new;
    String[] apply = function.apply(10);
}
```

## Stream API

- 是数据渠道，用于操作数据源（集合、数组等）所生成的元素序列。

**注意：**

1. Stream 自己不会存储元素。
2. Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。
3. Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。

**Stream 和 Collection 集合的区别：**

Collection 是一种静态的内存数据结构，而 Stream 是有关计算的。前者是主要面向内存，存储在内存中，后者主要是面向 CPU，通过 CPU 实现计算。

**Stream 的操作三个步骤**

![](C:/notebook/pictures/Snipaste_2022-11-19_13-07-50.png =600x)

### 实例化

**1. 通过集合的方式**

![](c:/notebook/pictures/Snipaste_2022-11-19_13-14-03.png =500x)

**2. 通过数组**

![](c:/notebook/pictures/Snipaste_2022-11-19_13-14-57.png =450x)

**3. 通过Stream的of()**

![](c:/notebook/pictures/Snipaste_2022-11-19_13-16-01.png =500x)

**4. 创建无限流**

![](c:/notebook/pictures/Snipaste_2022-11-19_13-16-40.png =600x)

```java
package com.zjk.java2;

import org.junit.jupiter.api.Test;

import java.util.Arrays;
import java.util.List;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class StreamAPITest {
    //1. 通过集合获取Stream实例
    @Test
    public void test1() {
        List<Employee> employees = EmployeeData.getEmployees();
        //stream() 返回顺序流
        Stream<Employee> stream = employees.stream();
        //parallelStream 返回并行流
        Stream<Employee> employeeStream = employees.parallelStream();
    }

    //2. 通过数组获取Stream实例
    @Test
    public void test2() {
        //调用Arrays类中的stream(arr)
        int[] arr = {1, 2, 3};
        IntStream stream = Arrays.stream(arr);
        Employee[] employees = {new Employee(), new Employee()};
        Stream<Employee> stream1 = Arrays.stream(employees);
    }

    //3. 通过Stream的of()
    @Test
    public void test3() {
        Stream<Integer> integerStream = Stream.of(1, 2, 3, 4);
    }

    //4. 创建无限流
    @Test
    public void test4() {
        //iterate() 迭代
        Stream.iterate(0, t -> t + 2).forEach(System.out::println);
        //generate() 生成
        Stream.generate(Math::random).limit(10).forEach(System.out::println);
    }
}
```

### Stream 的中间操作

多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行任何的处理！而在终止操作时一次性全部处理，称为“惰性求值”

**1-筛选与切片**

![](c:/notebook/pictures/Snipaste_2022-11-19_13-17-52.png =650x)

**2-映 射**

![](c:/notebook/pictures/Snipaste_2022-11-19_13-18-40.png =650x)

**3-排序**

![](c:/notebook/pictures/Snipaste_2022-11-19_13-18-59.png =500x)

```java
package com.zjk.java2;

import org.junit.jupiter.api.Test;

import javax.print.DocFlavor;
import java.util.*;
import java.util.stream.Stream;

public class StreamAPITest1 {
    //1-筛选与切片
    @Test
    public void test1() {
        List<Employee> employees = EmployeeData.getEmployees();
        //filter(Predicate p) 接收 Lambda ， 从流中排除某些元素
        Stream<Employee> stream = employees.stream();
        stream.filter(e -> e.getSalary() > 7000).forEach(System.out::println);
        //limit(long maxSize) 截断流，使其元素不超过给定数量
        employees.stream().limit(3).forEach(System.out::println);
        //skip(long n) 跳过元素，返回一个扔掉了前 n 个元素的流。若流中元素不足 n 个，则返回一个空流。与 limit(n) 互补
        employees.stream().skip(3).forEach(System.out::println);
        //distinct() 筛选，通过流所生成元素的 hashCode() 和 equals() 去除重复元素
        employees.stream().distinct().forEach(System.out::println);
    }

    //2-映射
    @Test
    public void test2() {
        List<String> list = Arrays.asList("aa", "bb", "cc");
        //map(Function f) 接收一个函数作为参数，该函数会被应用到每个元素上，并将其映射成一个新的元素。
        list.stream().map(str -> str.toUpperCase()).forEach(System.out::println);
//        获取员工姓名长度大于3的
        Stream<String> stringStream = EmployeeData.getEmployees().stream().map(Employee::getName);
        stringStream.filter(name -> name.length() > 3).forEach(System.out::println);
        //mapToDouble(ToDoubleFunction f) 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 DoubleStream。
        //mapToInt(ToIntFunction f)接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 IntStream。
        //mapToLong(ToLongFunction f) 接收一个函数作为参数，该函数会被应用到每个元素上，产生一个新的 LongStream。
//
        Stream<Stream<Character>> streamStream = list.stream().map(StreamAPITest1::fromStringToStream);
        streamStream.forEach(s -> {
            s.forEach(System.out::println);
        });
        //flatMap(Function f) 接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流
        Stream<Character> characterStream = list.stream().flatMap(StreamAPITest1::fromStringToStream);
        characterStream.forEach(System.out::println);

    }

    public static Stream<Character> fromStringToStream(String str) {
        ArrayList<Character> list = new ArrayList<>();
        for (Character c : str.toCharArray()) {
            list.add(c);
        }
        return list.stream();
    }

    //3-排序
    @Test
    public void test3() {
        //sorted() 自然排序
        List<Integer> integers = Arrays.asList(12, 43, 23, 344, 32);
        integers.stream().sorted().forEach(System.out::println);

        //sorted(Comparator comparator) 定制排序
//        Comparator comparator = new Comparator() {
//            @Override
//            public int compare(Object o1, Object o2) {
//                if (o1 instanceof Employee && o2 instanceof Employee) {
//                    Employee e1 = (Employee) o1;
//                    Employee e2 = (Employee) o2;
//                    return e1.getAge() - e2.getAge();
//                } else {
//                    throw new RuntimeException("数据类型不一致");
//                }
//            }
//        };
        List<Employee> employees = EmployeeData.getEmployees();
//        employees.stream().sorted(comparator).forEach(System.out::println);
        employees.stream().sorted((e1, e2) -> Integer.compare(e1.getAge(), e2.getAge())).forEach(System.out::println);

        employees.stream().sorted((e1, e2) -> {
            int num;
            if ((num = Integer.compare(e1.getAge(), e2.getAge())) != 0) {
                return num;
            } else {
                return Double.compare(e1.getSalary(), e2.getSalary());
            }
        }).forEach(System.out::println);
    }
}
```

### Stream 的终止操作

- 终端操作会从流的流水线生成结果。其结果可以是任何不是流的值，例如：List、Integer，甚至是 void 。
- 流进行了终止操作后，不能再次使用。

**1-匹配与查找**

![](c:/notebook/pictures/Snipaste_2022-11-19_13-22-27.png =500x)
![](c:/notebook/pictures/Snipaste_2022-11-19_13-20-38.png =500x)

**2-归约**

![](c:/notebook/pictures/Snipaste_2022-11-19_13-23-10.png =550x)

**3-收集**

![](c:/notebook/pictures/Snipaste_2022-11-19_13-24-04.png =550x)

- Collector 接口中方法的实现决定了如何对流执行收集的操作(如收集到 List、Set、Map)。
- 另外， Collectors 实用类提供了很多静态方法，可以方便地创建常见收集器实例，具体方法与实例如下表：
![](c:/notebook/pictures/Snipaste_2022-11-19_23-43-27.png =650x)
![](c:/notebook/pictures/Snipaste_2022-11-19_23-44-41.png =650x)

```java
package com.zjk.java2;

import org.junit.jupiter.api.Test;

import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class StreamAPITest2 {
    //1-匹配与查找
    @Test
    public void test1() {
        List<Employee> employeeList = EmployeeData.getEmployees();
        //allMatch(Predicate p) 检查是否匹配所有元素
//        判断是否所有的员工的年龄大于18
        System.out.println(employeeList.stream().allMatch(e -> e.getAge() > 18)); //false
        //anyMatch(Predicate p) 检查是否至少匹配一个元素
        System.out.println(employeeList.stream().anyMatch(e -> e.getName().equals("马云"))); //true
        //noneMatch(Predicate p) 检查是否没有匹配所有元素
        System.out.println(employeeList.stream().noneMatch(e -> e.getSalary() > 0)); //false
        //findFirst() 返回第一个元素
        System.out.println(employeeList.stream().sorted((e1, e2) -> Integer.compare(e1.getAge(), e2.getAge())).findFirst());
//        Optional[Employee{id=1002, name='马云', age=12, salary=9876.12}]
        //findAny() 返回当前流中的任意元素
        System.out.println(employeeList.parallelStream().findAny());
        //count() 返回流中元素总数
        System.out.println(employeeList.stream().count()); //8
//        工资大于5000的人个数
        System.out.println(employeeList.stream().filter(e -> e.getSalary() > 5000).count()); //5
        //max(Comparator c) 返回流中最大值
//        返回最大年龄的
        System.out.println(employeeList.stream().max((e1, e2) -> Integer.compare(e1.getAge(), e2.getAge())));
//        Optional[Employee{id=1005, name='李彦宏', age=65, salary=5555.32}]
        Stream<Integer> integerStream = employeeList.stream().map(Employee::getAge);
        System.out.println(integerStream.max(Integer::compare)); //Optional[65]
        //min(Comparator c) 返回流中最小值
//        返回工资最低的
        System.out.println(employeeList.stream().min((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary())));
//        Optional[Employee{id=1008, name='扎克伯格', age=35, salary=2500.32}]
        System.out.println(employeeList.stream().map(Employee::getSalary).min(Double::compare));
//        Optional[2500.32]
        //forEach(Consumer c) 内部迭代(使用 Collection 接口需要用户去做迭代，称为外部迭代。
        // 相反，Stream API 使用内部迭代——它帮你把迭代做了)
    }

    //2-归约
    @Test
    public void test2() {
        //reduce(T iden, BinaryOperator b) 可以将流中元素反复结合起来，得到一个值。返回 T
//        计算1~10的和
        List<Integer> integers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        System.out.println(integers.stream().reduce(0, Integer::sum)); //55
//        0 --设置的初始值
        //reduce(BinaryOperator b) 可以将流中元素反复结合起来，得到一个值。返回 Optional<T>
        System.out.println(integers.stream().reduce(Integer::sum)); //Optional[55]

//        计算员工工资的和
        System.out.println(EmployeeData.getEmployees().stream().map(Employee::getSalary).reduce(Double::sum));
//        Optional[48424.08]
        System.out.println(EmployeeData.getEmployees().stream().map(Employee::getSalary).reduce((e1, e2) -> e1 + e2));
    }

    //3-收集
    @Test
    public void test3() {
        //collect(Collector c) 将流转换为其他形式。接收一个 Collector接口的实现，用于给Stream中元素做汇总的方法
//        返回工资大于6000 的集合
        List<Employee> collect = EmployeeData.getEmployees().stream().filter(e -> e.getSalary() > 6000).collect(Collectors.toList());
        collect.stream().forEach(System.out::println);
    }
}
```

## Optional 类

- `Optional<T> `类(java.util.Optional) 是一个容器类，它可以保存类型T的值，代表这个值存在。或者仅仅保存null，表示这个值不存在。原来用 null 表示一个值不存在，现在 Optional 可以更好的表达这个概念。并且可以避免空指针异常。
- Optional类的Javadoc描述如下：这是一个可以为null的容器对象。如果值存在则`isPresent()`方法会返回true，调用`get()`方法会返回该对象。

![](c:/notebook/pictures/Snipaste_2022-11-19_13-28-19.png =650x)

![](c:/notebook/pictures/Snipaste_2022-11-20_16-50-36.png =550x)
![](c:/notebook/pictures/Snipaste_2022-11-20_16-59-09.png =400x)
![](c:/notebook/pictures/Snipaste_2022-11-20_17-02-46.png =550x)
![](c:/notebook/pictures/Snipaste_2022-11-20_17-04-33.png =550x)

# Java9 新特性

![](c:/notebook/pictures/Snipaste_2022-11-20_17-11-53.png =400x)

## JDK 和 JRE 目录结构的改变

![](c:/notebook/pictures/Snipaste_2022-11-20_17-13-20.png =600x)
![](c:/notebook/pictures/Snipaste_2022-11-20_17-14-03.png =600x)

## 模块化系统: Jigsaw --> Modularity



# Java10 新特性
# Java11新特性






























