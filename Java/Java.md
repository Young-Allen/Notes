#    1. Java简介

## Jdk1.5之后的三大版本

- **Java SE（J2SE，Java 2 Platform Standard Edition，标准版）** Java SE 以前称为 J2SE。它允许开发 和部署在桌面、服务器、嵌入式环境和实时环境中使 用的 Java 应用程序。Java SE 包含了支持 Java Web 服务开发的类，并为Java EE和Java ME提供基础。
- **Java EE（J2EE，Java 2 Platform Enterprise Edition，企业版**） Java EE 以前称为 J2EE。企业版本 帮助开发和部署可移植、健壮、可伸缩且安全的服务器 端Java 应用程序。Java EE 是在 Java SE 的 基础上构建的，它提供 Web 服务、组件模型、 管理和通信 API，可以用来实现企业级的面向服务 体系结构（service-oriented architecture，SOA）和 Web2.0应用程序。2018年2月，Eclipse 宣 布正式将 JavaEE 更名 为 JakartaEE。
- **Java ME（J2ME，Java 2 Platform Micro Edition，微型版）** Java ME 以前称为 J2ME。Java ME 为 在移动设备和嵌入式设备（比如手机、PDA、电视 机顶盒和打印机）上运行的应用程序提供一个健 壮且灵活的环境。Java ME 包括灵活的用 户界面、健壮的安全模型、许多内置的网络协议以及对可 以动态下载的连网和离线应用程序 的丰富支持。基于 Java ME 规范的应用程序只需编写一次，就 可以用于许多设备，而且可 以利用每个设备的本机功能。



## JVM、JRE和JDK的关系

- **JVM**：Java Virtual Machine是Java虚拟机，Java程序需要运行在虚拟机上，不同的平 台有自己的虚拟机，因此 Java语言可以实现跨平台。
- **JRE**：Java Runtime Environment包括**==Java虚拟机==**和==**Java程序所需的核心类库**==等。核心类库主要是java.lang 包：包含了运行Java程序必不可少的系统类，如基本数据类型、基本数学函数、字符串处理、线程、异常处理类等，系统缺省加载这个包
  **如果想要运行一个开发好的Java程序，计算机中只需要安装JRE即可。**
- **JDK**：Java Development Kit是提供给Java开发人员使用的，其中包含了Java的开发 工具，也包括了JRE。所以 安装了JDK，就无需再单独安装JRE了。其中的开发工 具：编译工具(javac.exe)，打包工具(jar.exe)等

![](https://img-blog.csdnimg.cn/c158f635d284474d8472bab13df36d3b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)



## 跨平台性

一个编译好的.class文件可以在多个系统下运行，这种特性称为跨平台性。

**实现原理**：Java程序是通过java虚拟机在系统平台上运行的，只要该系统可以安装相应的java虚拟机， 该系统就可以运行java程序。



## Java执行流程

![](https://img-blog.csdnimg.cn/f6362fa00eeb44ec911f0e19193e4b6a.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)



## 字节码

**字节码：**Java源代码经过虚拟机编译器编译后产生的文件（即扩展为.class的文 件），它不面向任何特 定的处理器，只面向虚拟机。

**采用字节码的好处：**Java语言通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解 释型语言可移植的特点。所以Java程序运行时比较高效， 而且，由于字节码并不专对一种特定的机器， 因此，Java程序无须重新编译便可在多种不同的计算机上运行。

**Java中的编译器和解释器**：Java中引入了虚拟机的概念，即在机器和编译程序之间加入了一层抽象的虚拟机器。这台虚拟的机器在任何平台上都提供给编译程序一个的共同的接口。编译程序只需要面向虚拟机，生成虚拟机能够理解的 代码，然后由解释器来将虚拟机代 码转换为特定系统的机器码执行。在Java中，这种供虚拟机理解的代 码叫做字节 码（即扩展为.class的文件），它不面向任何特定的处理器，只面向虚拟机。每 一种平台的 解释器是不同的，但是实现的虚拟机是相同的。Java源程序经过编译 器编译后变成字节码，字节码由虚 拟机解释执行，虚拟机将每一条要执行的字节 码送给解释器，解释器将其翻译成特定机器上的机器码， 然后在特定的机器上运 行，这就是上面提到的Java的特点的编译与解释并存的解释。

```
Java源代码---->编译器---->jvm可执行的Java字节码(即虚拟指令)---->jvm---->jvm中 解释器----->机器可执行的二进制机器码---->程序运行。
```



# 2. Java开发注意事项和细节说明

## 注意点

1. Java源文件以 .Java 为扩展名。源文件的基本组成部分是类（class），如本类中的hello类。
2. Java应用程序的执行入口是main（）方法。它有固定的书写格式：

```java
public static void main(String[] args) {}
```

3. 一个源文件中最多只能有一个public类。其它类的个数不限。
4. 如果源文件包含一个public类，则文件名必须按该类名命名。
5. 一个源文件中最多只能有一个public类。其它类的个数不限，也可以将main方法写在非public类中，然后指定运行非public类，这样入口方法就是非public的main方法。



## Java转义字符

|    \t    | 一个制表位，实现对齐的功能 |
| :------: | -------------------------- |
|  **\n**  | **换行符**                 |
| **\\\\** | **一个**\                  |
| **\\\"** | **一个“**                  |
| **\\\‘** | **一个’**                  |
|  **\r**  | **一个回车**               |



## Java文档注释

**简介：**文档注释负责描述类、接口、方法、构造器、成员属性。可以被JDK提供的工具javadoc所解析，自动生成一套以**网页文件形式体现该程序说明文档的注释**。

**注意：**文档注释必须写在类、接口、方法、构造器、成员字段前面，写在其他位置无效。

**作用：**当开发一个大型软件时，需要定义成千上万个类，而且需要很多人参与开发。每个人都会开发一些类，并在类里定义一些方法和域提供给其他人使用，但其他人怎么知道如何使用这些类和方法呢？

这时就需要提供一份说明文档，用于说明每个类、每个方法的用途。当其他人使用一个类或者一个方法时，他无需关心这个类或这个方法的具体实现，他只要知道这个类或这个方法的功能即可，然后使用这个类或方法来实现具体的目的，也就是通过调用应用程序接口（API）来编程。

API文档就是用来说明这些应用程序接口的文档。对于java语言而言，API文档通常详细的说明了每个类、每个方法的功能及用法。



## DOS命令

常用的DOS命令：

```java
//	1.查看当前目录有什么内容
	dir  或	dir F:\Studying Code\JavaCode\test01
//	2.切换到其他盘符下
    cd /D c:
//	3.切换到上一级
	cd ..
//	4.切换到根目录
	cd \
//	5.查看指定目录下所有的子级目录        
	tree f:
//	6.清屏
	cls[]
//	7.退出DOS
	exit 
```

---

# 3. 基础语法

## 数据类型

|    数值型    | byte、short、int、long、float、double |
| :----------: | :------------------------------------ |
|  **字符型**  | **char**                              |
|  **布尔型**  | **boolean**                           |
| **引用类型** | **类、接口、数组**                    |

![](https://img-blog.csdnimg.cn/d9e5122220f743d58aeb0e9f69856c9c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

**注意：**

- Java的浮点型常量（具体值）默认为double型，声明float类型常量，须在后加 f 或 F。

  ```java
  float num1 = 1.1;	//错误
  float num2 = 1.1f;	//正确
  double num3 = 1.1;	//正确
  double num4 = 1.1f;	//正确
  ```

- 声明long类型时须在后加 ‘l’ 或 ‘L'。

```java
//注意在声明long类型时，若数值没有超过int（-2^31 ~ 2^31-1）加或者不加L不会报错
long n1 = 2147483647;
//但是如果超过了int的范围，就一定要加L，否则会报错
long n1 = 9223372036854775807L;
```

- 关于浮点数在机器中存放形式的简单说明，浮点数 = 符号位 + 指数位 + 尾数位
  尾数位部分可能丢失，造成精度损失（小数都是近似值）

---

## 字符型

使用细节：

- 字符常量是用单引号括起来的单个字符。

  ```java
  char c1 = 'a';
  char c2 = '中';
  char c3 = '9';
  ```

- Java中还允许使用转义字符 ‘\’ 来将其后的字符转变为特殊字符型常量。

  ```java
  // '\n'表示换行符
  char cr = '\n';
  ```

- 在Java中，char的本质是一个整数，在输出时，是unicode码对应的字符。

- 可以直接给char赋一个整数，然后输出时，会按照对应的unicode字符输出。

- char类型是可以进行运算的，相当于一个整数，因为它都有对应的unicode码。

- **当左右两边都是数值型时，则做加法运算**

- **当左右两边有一方为字符串，则做拼接运算**

```java
//加号的使用
//1.当左右两边都是数值型时，则做加法运算
//2.当左右两边有一方为字符串时，则做拼接运算
System.out.println(100 + 98);
System.out.println("100" + 90);
System.out.println("hello" + 90 + 100);
System.out.println(98 + 100 + "hello");
```



字符类型本质探讨：

1. 字符型存储到计算机中，需要将字符对应的码值（整数）找出来，比如 ‘a’ 
   存储： ‘a’ ——> 码值 97 ——> 二进制（110 0001）——> 存储
   读取：二进制（110 0001）——》 97 ——》’a‘ ——》显示
2. 字符和码值对应关系是通过字符编码表决定的。
   ASCII：一个字节表示，一共128个字符，实际上一个字节可以表示256个字符，只用了128个
   Unicode：固定大小的编码，使用两个字节来表示字符，字母和汉字统一都是占用两个字节，浪费空间
   utf-8：大小可变的编码，字母使用1字节，汉字使用3字节
   gbk：可以表示汉字，而且范围广，字母使用1字节，汉字2字节
   big5码：繁体中文

---

## 基本数据类型转换

### 自动类型转换

**自动类型转换：**当Java程序在进行赋值或运算时，精度小的类型自动转换为精度大的数据类型。

![](https://img-blog.csdnimg.cn/0d3ff030070043f6afe8e2bafcd689c8.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

**自动类型转换注意的细节：**

```java
 //1.有多种类型的数据混合运算时(自动提升原则)
//系统首先自动将所有数据转换成容量最大的那种数据类型，然后再进行计算
int n1 = 10;    //对
//float f1 = n1 + 1.1;    //错，1.1是double类型，n1 + 1.1 = 结果类型是double
double d1 = n1 + 1.1;   //对
float f2 = n1 + 1.1f;   //对

//2.当我们把精度（容量）大的数据类型赋值给精度（容量）小的数据类型时，就会报错
//反之就会进行自动类型转换
//int n2 = 1.1;   //错误，double-》int
double d2 = n1;

//3.(byte,short)和char之间不会相互自动转换
//当把具体数据赋给byte时，首先判断该数是否在byte范围内，如果是就可以
byte bt1 = 127;     //对，-128~127 （虽然127是int）
//int n2 = 1;     //n2是int
//byte bt2 = n2;  //错，如果是变量赋值，判断类型，高精度不能转换为低精度
//char c2 = bt1;  //错，byte不能自动转换为char

//4.byte、short、char 三者不管是单独还是混合运算，在计算时首先转换为int类型
byte bt2 = 2;
byte bt3 = 3;
short s1 = 1;
//short s2 = bt2 + s1;    //错，bt2 + s1 = int类型，不能转换为低精度short
int n3 = bt2 + s1;
//byte bt4 = bt2 + bt3;   //错，bt2 + bt3 = 提升为int类型

//5.boolean不参与转换
boolean tag = false;
//false = 0;  //错，不能将整型赋给boolean
//int num1 = tag; //错，boolean不参与类型的自动转换
```



### 强制类型转换

**强制类型转换：**

自动类型转换的逆过程，将容量大的数据类型转换为容量小的数据类型。使用时要加上强制转换符（），但可能造成精度降低或溢出，格外要注意。

```java
//1.精度降低
int num1 = (int) 1.9;    //强制类型转换导致精度降低
System.out.println("num1=" + num1);

//2.数据溢出
byte bt5 = (byte) 1000; //byte只有-128~127 导致数据溢出
System.out.println("bt5=" + bt5);

//3.强转符号只针对最近的操作数有效，往往会使用小括号提升优先级
//int x = (int) 10 * 3 + 6 * 1.5; //错误，只会强制类型转换最近的10
int y = (int) (10 * 3 + 6 * 1.5);
System.out.println(y);

//4.char类型可以保存int的常量值，但不能保存int的变量值，需要强转
char cc = 100;  //ok
int m = 100;    //ok
//char ccc = m;   //错
char cc1 = (char)m; //ok
```



### String和基本类型转换

一些String对象的方法

```java
String s1 = "abc";
String s2 = "aaa";
//1.String对象的比较方法
boolean iss = s2.equals(s1);	//iss为false
```

![](https://img-blog.csdnimg.cn/c4da897ffc4d4359800ba91845199fa1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)



**基本数据类型 ——String**

```java
//1.基本数据类型——String
byte bt1 = 100;
short sh1 = 200;
int n1 = 100;
float f1 = 1.1f;
double d1 = 4.2;
boolean b1 = false;

String s1 = bt1 + "";   //加一个“”就行
String s2 = sh1 + "";
String s3 = n1 + "";
String s4 = f1 + "";
String s5 = d1 + "";
String s6 = b1 + "";
System.out.println(s1 + s2 + s3 + s4 + s5 + s6);

//2.string到基本数据类型的转换
String ss1 = "123";
int num1 = Integer.parseInt(ss1);
double num2 = Double.parseDouble(ss1);
float num3 = Float.parseFloat(ss1);
long num4 = Long.parseLong(ss1);
byte num5 = Byte.parseByte(ss1);
boolean b = Boolean.parseBoolean("true");
short num6 = Short.parseShort(ss1);

//3.怎么把字符串转成字符char（含义是指：把字符串的第一个字符得到）
//ss1.charAt(0) 得到ss1字符串的第一个字符‘1’
System.out.println(ss1.charAt(0));
```

---

### 进制

二进制：以0b或0B开头

十进制：满十进一

八进制：以数字0开头

十六进制：以0x或0X开头

---

## 运算符

### 取模 %

```java
// % 的本质，看一个公式 a % b == a - a / b * b
System.out.println(10 % 3);   // 10 - 10 / 3 * 3 = 10 - 9 = 1
System.out.println(-10 % 3);    // -10 - (-10) / 3 * 3 = -10 - (-9) = -1
System.out.println(10 % -3);    // 10 - 10 / (-3) * (-3) = 10 - 9 = 1
System.out.println(-10 % -3);   // -10 - (-10) / (-3) * (-3) = -10 - (-9) = -1

//如果是小数 a - (int)a / b * b
-10.5 % 3 = -10.5 -(int)(-10.5) / 3 * 3 = -10.5 + 9 = -1.5
```



### 逻辑运算符

![](https://img-blog.csdnimg.cn/36127152945e459e8bdeddf705ce49ff.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
//1.短路与 &&，如果第一个条件为false，后面的条件不再判断
if (a < 1 && ++b < 40) {
    System.out.println("yes");
}
System.out.println("a=" + a + " b=" + b);

//2.逻辑与 &，如果第一个条件为false，后面的条件任然会判断
if (a < 1 & ++b < 40) {
    System.out.println("yes");
}
System.out.println("a=" + a + " b=" + b);

//3.短路或||，如果第一个条件为true，则第二个条件不会判断，最终结果为true，效率高
//4.逻辑或|，不管第一个条件是否为true，第二个条件都要判断，效率低
注意：开发中基本使用||
```

---

## 命名规范

1. 包名：多单词组成时，所有字母都小写：aaa.bbb.ccc、com.wxd.cer
2. 类名、接口名：多单词组成时，所有单词的首字母大写：ClassRoom、TankShotGame
3. 变量名、方法名：多单词组成时，第一个单词首字母小写，第二个单词开始每个单词首字母大写：tankShotGame、lookGame
4. 常量名：所有字母都大写。多单词时每个单词用下划线连接：MAX_AGE、MIN_MONEY

---

## 数学函数

- **Math.round(11.5) 等于多少？Math.round(-11.5) 等于多少**

 Math.round(11.5)的返回值是 12，Math.round(-11.5)的返回值是-11。四舍 五入的原理是在参数上加 0.5 然后进行下取整。

---

## 键盘输出

```java
//输出没有换行
System.out.print("我是没有换行输出");

//输出有换行的
System.out.println("我是有换行的");
```



## 键盘输入

```java
//导入java.util包
import java.util.Scanner;   //表示把java.util下的Scanner类导入

public class input {
    public static void main(String[] args) {
        //演示接受用户输入的步骤
        //Scanner类 表示 简单文本扫描器，在java.util包
        //1.导入Scanner类所在的包
        //2.创建Scanner对象，new创建一个对象，System.in表示从键盘输入
        Scanner input = new Scanner(System.in);
        //3.接受用户输入了，使用相关的方法
        System.out.println("请输入名字");
        String name = input.next(); //接受用户输入字符串
        System.out.println("请输入年龄");
        int age = input.nextInt();  //接受用户输入int
        System.out.println("请输入薪水");
        double sal = input.nextDouble();    //接受用户输入doubel
        System.out.println("姓名：" + name + " 年龄：" + age + " 薪水：" + sal);
    }
}

```

---

## 原码、反码、补码

对于有符号的而言：

1. 二进制的最高位是符号位：0表示正数，1表示负数（0 -> 0, 1 -> -)
2. 正数的原码、反码、补码都一样
3. 负数的反码 = 它的原码符号位不变，其他位取反（0变1,1变0）
4. 负数的补码 = 它的反码 + 1，负数的反码 = 负数的补码 - 1
5. 0的反码、补码都是0
6. Java没有无符号数，换言之，Java中的数都是有符号的
7. 在计算机运算的时候，都是以**补码的方式来运算的**
8. 当我们看运算结果时，要看它的原码

![](https://img-blog.csdnimg.cn/3c0769e74bb640f483d140c8f40eed28.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
//1.计算2&3
//先得到2的补码
//2的原码 00000000 00000000 00000000 00000010
//正数原码、反码、补码都一样 2的补码 = 00000000 00000000 00000000 00000010
//同理3的补码 = 00000000 00000000 00000000 00000011
//按位与运算 00000000 00000000 00000000 00000010
//        & 00000000 00000000 00000000 00000011
//          00000000 00000000 00000000 00000010 (得到的结果的补码)
//将结果的补码转换为原码 = 00000000 00000000 00000000 00000010  = 2(结果为2)
System.out.println(2 & 3);

//2.计算~-2
//先得到-2的原码      10000000 00000000 00000000 00000010
//将-2的原码转换为反码 11111111 11111111 11111111 11111101（负数的反码：符号位不变，其他位取反）
//负数的补码=反码+1   11111111 11111111 11111111 11111110
//按位取反操作~       00000000 00000000 00000000 00000001
//将得到的结果的反码转换为原码 00000000 00000000 00000000 00000001（结果为1）
System.out.println(~-2);

//3.计算~2
//得到2的原码 00000000 00000000 0000000 00000010
//2的补码    00000000 00000000 0000000 00000010
//按位取反   11111111 11111111 11111111 11111101
//负数的反码 = 负数的补码-1  11111111 11111111 11111111 11111100
//结果的原码（符号位不变，其他位取反）10000000 00000000 00000000 00000011
//结果为 -3
System.out.println(~2);
```

![](https://img-blog.csdnimg.cn/e6fe768695254b968b4730c7a744e134.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
//1对应的二进制：00000001
//右移两位：    00000000 = 0
System.out.println(1 >> 2);

//-1对应的二进制：10000000 00000000 00000000 00001000
//右移两位：     10000000 00000000 00000000 00000010
System.out.println(-8 >> 3);

//1对应的二进制：00000001
//左移两位：    00000100 = 4
System.out.println(1 << 2);
```

---

## switch

注意：Switch（表达式）中表达式的返回值必须是（byte、short、int、char、enum、String）

## 数组注意事项

1. 数组创建后，如果没有赋值，有默认值（int 0, short 0, byte 0, long 0, float 0.0, double 0.0, char \u0000, boolean false, String null)

2. 数组在默认情况下是引用传递，赋的值是地址，赋值方式为引用赋值（是一个地址）

   ```java
   int arr1[] = {1, 2, 3};
   int arr2[] = arr1;	//把arr1赋给arr2
   arr2[0] = 10;	//arr1里面的值也会改变
   ```

   ![](https://img-blog.csdnimg.cn/5797968960b44395b97fccc50b1567e4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)



**数组拷贝：**数组的数据空间独立

```java
int arr1[] = {1, 2, 3};
//new创建一个新的数组arr2，开辟新的数据空间,大小为arr1的length
int arr2[] = new int[arr1.length];
//在遍历arr1将对应的数据赋值给arr2
for(int i = 0; i < arr1.length; i++){
	arr2[i] = arr1[i];
}
```

**二维数组：**

```java
//二维数组有三种声明方式：
int[][] y, int[] y[], int y[][];

//1.动态初始化
int a[][] = new int[2][3];
int[] b[];	//也是二维数组的声明方式

//2.先声明，再定义
int num[][];[]
num = new int[3][4];

//3.一个有三个一位数组，每个一维数组的元素是不一样的
int arr[][] = new int[3][];	//创建二维数组，但是只是确定一维数组的个数
for(int i = 0; i < arr.length; i++){
    //给每一个一维数组开空间new
    //如果没有给一维数组new，那么arr[i]的地址就是null
    arr[i] = new int[i + 1];
    
    //遍历一维数组，并给一维数组的每个元素赋值
    for(int  j = 0; j < arr[i].length; j++){
        arr[i] [j] = i + 1;
    }
}

//4.静态初始化
int arr[][] = {{1,1,1},{2,2,2},{3}};
//解读：
//1.定义了一个二维数组arr
//2.arr有三个元素（每一个元素都是一维数组）
//3.第一个一维数组有3个元素，第二个一维数组有3个元素，第三个一维数组有1个元素
```

![](https://img-blog.csdnimg.cn/dcfcb03cd406417092dbe45a5438ef65.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

---

## Math类

### 1. 产生随机数

1可以通过Math.random()函数来获取一个0.0到1.0之间的随机数(不包括1.0)

2可以通过System.currentTimeMills()函数取余获取一个随机数

#### 1.1 产生随机字符

1. 在Java中，每一个字符都有一个独特的unicode，又因为char数据类型为两个字节，

所以其值为0-2\^16

2. 利用Math.random()产生数为0\<=x\<1.0的特点

​	char ch=(Math.random( )\*(65535+1));

​	//65536+1是因为random取不到1*

#### 1.2 生成指定区间的字符

1. 生成小写字母

1思路：  
起点：’a’

终点：’z’

区间长度：’z’-‘a’

2. 公式：

Char ch=(char)(‘a’+Math.random()\*(‘z’-‘a’+1));

### 2. 快速得到PI

- 调用Math.PI
  double PI = *Math*.*PI*;

- 使用Math.acos(-1.0)
  double Pi = *Math*.*acos*(-1.0);

### 3. 计算三角形的三个角的角度

思路：

1利用余弦公式

2利用Math.acos( )函数

---

# 4. 类和对象

---

## 不同数据类型造成的传参问题

Java中变量分为两种：

1 基本数据类型

基本数据类型的内容的存放位置就是栈

2 引用数据类型

引用数据类型的内容的存放位置是堆，栈中存放该堆区域的地址

栈区域的值的拷贝就会有两种数值：

1 实实在在的值的拷贝 //基本数据类型

2 地址值的拷贝 //引用数据类型

*//当形参的数据类型为引用数据类型时，函数接收的是一个地址*

---

## 对象内存分部

- 创建对象时，会在方法区加载Cat类信息，包括**属性信息和方法信息**。
- cat对象在栈中存放，对应的地址（0x0011），该地址指向堆中实际地址所对应的空间。
- 在堆中声明的基本数据类型（int，double之类的），就是堆中的位置；而在堆中声明的引用类型（类、接口、数组）就是一个地址，指向**方法区**中对应的空间。

![](https://img-blog.csdnimg.cn/42362544bf27499b851f91f014f3e541.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

---

## 对象分配机制

![](https://img-blog.csdnimg.cn/64e38510ce0d47fd9ff8d8bbb834a95b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

---

## 方法调用机制

注意：方法的调用，会在栈中再生成对应的独立的内存空间。

![](https://img-blog.csdnimg.cn/0714c352a4bb4984afee448c512f6249.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)



## 成员方法传参机制

- **基本数据类型：**传递的是值（值拷贝），形参的任何改变不影响实参
- **引用数据类型：**传递的是地址（传递也是值，不过值是地址），可以通过形参影响实参

---

## 方法的递归机制

**注意：**

1. 执行一个方法时，就创建一个新的受保护 的独立空间（栈空间）
2. 方法的局部变量是独立的，不会相互影响，比如n变量
3. 如果方法中使用的是引用类型变量（数组、类、接口），就会共享给引用类型的数据
4. 递归必须向退出递归的条件逼近，否则就是无限递归，出现栈溢出
5. 当一个方法执行完毕，或者遇到return，就会返回，遵守谁调用，就将结果返回给谁，同时当方法执行完毕或者返回时，该方法也就执行完毕

![](https://img-blog.csdnimg.cn/1820d543398645e1adc33699110bd2a2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

---

## 重载的使用细节

**注意：**

1. 方法名：必须一致
2. 形参列表：必须不同（形参类型或个数或顺序，至少有一样不同，参数名无要求）
3. 返回类型：无要求

（主要看方法名一致和形参列表不一致）

---

## 可变参数

**基本概念：**Java允许将同一个类中**多个同名同功能**但**参数个数不同**的方法，封装成一个方法。

**基本语法：**访问修饰符 返回类型 方法名（数据类型… 形参名）{}

**使用细节：**

1. 可变参数的实参可以为0或者是多个
2. **可变参数的实参可以为数组**
3. 可变参数的本质就是数组
4. 可变参数可以和普通类型的参数一起放在形参列表，但**必须保证可变参数在最后**
5. **一个形参列表中只能出现一个可变参数**

```java
public class overLoad {
    public static void main(String[] args) {
        Method m = new Method();
        m.sum(5,3,1,5);
    }
}

class Method {
    public int sum(int n1, int n2) {
        return n1 + n2;
    }

    public int sum(int n1, int n2, int n3) {
        return n1 + n2 + n3;
    }

    //上面的三个方法名称相同，功能相同，参数个数不同：使用可变参数优化
    //1. int... 表示接受的是可变参数，类型是int，即可以接受多个int（0~多个）
    //2. 使用可变参数时，可以当做数组来使用，即nums可以当做数组
    public int sum(int... nums) {
        System.out.println("接受的参数的个数：" + nums.length);
        return 0;
    }
}
```

---

## 作用域

**基本使用：**

1. 在java编程中，主要的变量就是属性（成员变量）和局部变量
2. 我们说的局部变量一般是指在成员方法中定义的变量。
3. java中作用域的分类
   **全局变量：**也就是属性，作用域为整个类体
   **局部变量：**也就是除了属性之外的其他变量，作用域为定义它的代码块中
4. **全局变量可以不赋值，直接使用，因为有默认值；局部变量必须赋值后，才能使用，因为没有默认值**

```java
class Method {
    //全局变量，可以不赋值，就是默认值
    int n1;
    double d1;

    public int sum(int n1, int n2) {
        //局部变量，必须赋值，否则无法使用（因为没有默认值） 
        int num = 1;
        return n1 + n2 + num;
    }
}
```



**注意细节：**

1. 属性和局部变量可以重名，访问时遵循就近原则
2. 在同一个作用域中，比如在同一个成员方法中，两个局部变量，不能重名
3. 属性生命周期较长，伴随着对象的创建而创建，伴随着对象的撤销而撤销。局部变量，生命周期较短，伴随着它的代码块的执行而创建，伴随着代码块的结束而消亡。即在一次方法调用过程中。

```java
class Method {
    //全局变量，可以不赋值，就是默认值
    int n1;
    String name = "jack";

    public int sum(int n1, int n2) {
        //局部变量，必须赋值，否则无法使用（因为没有默认值） 
        int num = 1;
        String name = "james"
        System.out.println(name);	//打印的是jame（就近原则）
        return n1 + n2 + num;
    }
    
    public void hh() {
		String adderss = "北京";
        String adress = "上海";	//错误，局部变量不能重名   
    }
}
```

4. 作用域的范围不同
   全局变量/属性：可以被本类使用，或其他类使用（通过对象调用）
   局部变量：只能在本类中对应的方法在使用

5. 修饰符不同
   全局变量可以加以修饰

   局部变量不可以加以修饰

---

## 构造方法/构造器

**基本介绍：**构造方法又叫构造器，是类似的一种特殊的方法，它的主要作用是完成对**新对象的初始化**。类似C++的构造函数。它有几个特点：

1. 方法名和类名相同
2. 没有返回值
3. 在创建对象时，系统会自动的调用该类的构造器完成对对象的初始化

```java
public class Construct {
    public static void main(String[] args) {
        Person p1 = new Person("jack", 90);
        System.out.println(p1.name + " " + p1.age);
        p1 = new Person("james");
        System.out.println(p1.name + " " + p1.age);
    }
}

class Person {
    String name;
    int age;
	//构造器，用以初始化实例对象
    public Person(String Pname, int Page) {
        name = Pname;
        age = Page;
    }
	//构造器的重载
    public Person(String Pname) {
        name = Pname;
    }
    
    //显式的定义默认的无参构造器
    Person() {};
}
```



**使用细节：**

1. **一个类可以定义多个不同的构造器，即构造器重载**
   比如：我们可以再给Person类定义一个构造器，原来创建对象的时候，只指定人名，不需要年龄
2. 构造器和类名要相同
3. 构造器没有返回值
4. 构造器是完成对象的初始化，并不是创建对象
5. 在创建对象时，系统自动的调用该类的构造方法
6. **如果程序员没有定义构造器，系统会自动给类生成一个默认无参构造器(也叫默认构造器)**，比如Person（）{}，**使用javap指令**，反编译看看
7. **一旦定义了自己的构造器，默认的构造器就覆盖了，就不能再使用默认的无参构造器，除非显式的定义一下**，即：Person（）{}

---

## 对象创建的流程分析

https://www.bilibili.com/video/BV1fh411y7R8?p=245&spm_id_from=pageDriver

```java
//看一个案例
class Person {
    int age = 90;
    String name;
    Person(String n,int a){
		name = n;
        age = a;
       
    }
}

Person p = new Person("jack",20);
```

**流程分析：**

1. 加载Person类信息（Person.class），在方法区加载一次Person类信息
2. 在堆中分配对象空间（有了地址0x1122）
3. **完成对象初始化**
   1.默认初始化 age = 0， name = null
   2.显式初始化 age = 90，name = null
   3.构造器的初始化 age 20，name = jack
4. 把对象在堆中的地址，返回给p（p是对象名，也可以理解成是对象的引用） 

![](https://img-blog.csdnimg.cn/1820d543398645e1adc33699110bd2a2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

---

## this关键字

**什么是this：**java虚拟机给每个对象分配this，代表当前对象。

**作用：**

- 作用一：引用对象自身
- 作用二：在构造方法中用于调用同一个类的其他构造方法

```java
class Cat {
    String name;
    int age;

    //作用一：引用对象自身，访问本类的属性、方法
    public void s1() {
        System.out.println("调用方法public void s1()");
    }

    public void s2() {
        System.out.println("调用方法public void s2");
        //使用this调用方法s1
        this.s1();
    }

    //作用二：在构造方法中用于调用同一个类的其他构造方法
    public Cat() {
        //以下this构造器语法必须放在该方法中所有可执行语句之前
        this("jack", 20);
        System.out.println("调用构造器public Cat()");
    }

    public Cat(String name, int age) {
        //使用this访问本类属性
        this.name = name;
        this.age = age;
        System.out.println("调用构造器public Cat(String name,int age)");
    }
}
```

**细节：**

1. this关键字可以用来访问本类的属性、方法、构造器
2. this用于区分当前类的属性和局部变量
3. 访问成员方法的语法：this.方法名（参数列表）；
4. 访问构造器语法：this（参数列表）; **注意只能在构造器中使用（即能在构造器中访问另一个构造器，必须放在第一条语句）**
5. this不能在类定义的外部使用，只能在类定义的方法中使用

---

# 5. IDEA快捷键及细节

|              运行              |            crtl + r            |
| :----------------------------: | :----------------------------: |
|          **删除一行**          |          **ctrl + d**          |
|         **生成构造器**         |        **alt + insert**        |
|    **查看一个类的层级关系**    |          **ctrl + h**          |
|       **定位方法的位置**       | **光标放在方法上，按ctrl + b** |
|       **设置封装的函数**       |       **alt  + insert**        |
| **快速查看当前类的属性和方法** |          **alt + j**           |
|        **重写toString**        |       **alt  + insert**        |
|     **快速实现接口的方法**     |          **ctrl + i**          |
|     **复制当前代码的路径**     |      **shift + ctrl + c**      |
|    **调出使用异常的快捷键**    |          **ctrl + T**          |
|                                |                                |
|                                |                                |
|                                |                                |
|                                |                                |
|                                |                                |

构造器的属性提示是否开启：

<img src="https://img-blog.csdnimg.cn/3ddde392a3594f89abf8ac2b79116844.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16" style="zoom: 67%;" />

## 调试

|     F7     | 跳入           |
| :--------: | -------------- |
|     F8     | 跳过           |
| shift + F8 | 跳出           |
|     F9     | resume，执行到 |



---

# 6. 包

## **包的三大作用**：

1. 区分相同名字的类
2. 当类很多时，可以很好的管理类
3. 控制访问范围



## 包的基本语法：
package com.hspedu;
说明：（1）package关键字：表示打包，（2）com.hspedu：表示包名



## 包的本质分析（原理）

实际上就是创建不同的文件夹/目录来保存类文件，画出示意图。



## **包的命名**

- 命名规则：只能包含字母、数字、下划线、小圆点，但不能用数字开头，不能是关键字或保留字

  ```java
  demo.class.exe1	//错误
  demo.12a	//错误
  demo.ab12.oa	//对
  ```

- 命名规范：一般是com.公司名.项目名.业务模块名

  ```java
  com.sina.crm.user	//用户模块
  com.sina.crm.order	//订单模块
  com.sina.crm.utils	//工具类
  ```

  

## 常用的包

```java
java.lang.*		//lang包是基本包，默认引入，不需要在引入
java.util.*		//util包：系统提供的工具包，工具类，使用Scnner
java.net.*		//网络包：网络开发
java.awt.*		//做java的界面开发，GUI
```



## 包的使用细节

```javascript
//建议使用哪个类就导入哪个类
import java.util.Scanner;   //表示只会引入java.util 包下的Scnner
import java.util.*;         //表示引入java.util包下的所有类
```

注意事项和细节：

- package的作用：声明当前类所在的包，需要放在类的最上面，一个类最多只有一句package
- import指令 位置放在package的下面，在类定义的前面，可以有多句且没有顺序要求

---

# 7. 访问修饰符

## 基本介绍

1. 公开级别：public，对外公开
2. 受保护级别：protected，对子类和同一个包中的类公开
3. 默认级别：没有修饰符，向同一个包的类公开
4. 私有级别：private，只有类本身可以访问，不对外公开

![](https://img-blog.csdnimg.cn/ac64f66db2d24045990e7663cbc0487f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)



## 使用的注意事项

1. 修饰符可以用来修饰类中的属性、成员方法以及类
2. 只有默认的和public才能修饰类，并且遵循上述访问权限的特点
3. 成员方法的访问规则和属性完全一致



# 8. 封装

封装的实现步骤（三步）

```
（1）将属性进行私有化private【不能直接修改属性】

（2）提供一个公共的（public）set方法，用于对属性判断并赋值
public void setXxx(类型 参数名){	//Xxx 表示某个属性
	//加入数据验证的业务逻辑
	属性 = 参数名；
}

（3）提供一个公共的（public）get方法，用于获取属性的值
public 数据类型 getXxx（）{	//权限判断，Xxx某个属性
	return XX;
}
```

```java
package com.user.Encapsulation;

//请大家看一个小程序(Encapsulation01 java),不能随便查看人的年龄,工资等隐私
// 并对设置的年龄进行合理的验证，年龄合理就设置，否则给默认年龄，必须在1-120,年龄，
// 工资不能直接查看，name的长度在 2-6字符之间

import java.util.Scanner;

public class encap {
    public static void main(String[] args) {

        Person person = new Person();
        person.setName("jack");
        person.setAge(28);
        person.setSalary(100000);
        Person person1 = new Person("jacasdasdk", 0, 8000);
        person1.saySalary();
    }
}

class Person {
    Scanner input = new Scanner(System.in);
    public String name; //姓名公开
    private int age;    //年龄私有
    private double salary;  //薪水私有

    public Person() {
    }

    public Person(String name, int age, double salary) {
        this.name = name;
        this.age = age;
        this.salary = salary;
        //将封装函数加入到构造器中，实现对 对象初始化时也能满足封装的需求
        this.setAge(age);
        this.setName(name);
        this.setSalary(salary);
    }

    public void setName(String name) {
        if (name.length() >= 2 && name.length() <= 6) {
            this.name = name;
        } else {
            System.out.println("输入的年龄的长度不正确（2~6个字符），姓名为：unknown");
            this.name = "unknow";
        }
    }

    public void setAge(int age) {
        if (age >= 1 && age <= 120) {
            this.age = age;
        } else {
            System.out.println("输入的年龄不在（1~120）之间，给默认年龄18");
            this.age = 18;
        }
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    public void saySalary() {
        System.out.println("查看薪水，请输入密码：");
        String ch = input.next();
        if (ch.equals("123456")) {
            System.out.println("薪水为：" + salary);
        } else {
            System.out.println("密码不正确");
        }
    }
}

```



# 9. 继承

## 基本介绍

继承可以解决代码复用，让我们的编程更加靠近人类思维。当多个类存在相同的属性（变量）和方法时，可以从这些类中抽象出父类，在父类中定义这些相同的属性和方法，所有的子类不需要重新定义这些属性和方法，只需通过==extends==来声明继承父类即可。

 ## 继承原理图

![](https://img-blog.csdnimg.cn/372eeb4cdb2d4719a5755047e64f488f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)



## 语法

```java
//Graduate类 继承自 Student类
public class Graduate extends Student {
	public void testing() {
		System.out.println("Is testing now");
	}
}
```



1. 子类会自动拥有父类定义的属性和方法
2. 父类又叫 超类、基类
3. 子类又叫派生类



## 注意细节

1. 子类继承了所有的属性和方法，**非私有的属性和方法可以在子类直接访问**，但是私有属性和方法不能在子类直接访问，要通过**公共的方法去访问**。

```

```

2. 子类必须调用父类的构造器，完成父类的初始化。（实际上是调用是super语句，默认调用父类的无参构造器）
3. 当创建子类对象时，不管使用子类的哪个构造器，**默认情况下总会去调用父类的无参构造器**，如果父类没有提供无参构造器，则必须在子类的构造器中用super去指定使用父类的哪个构造器完成对父类的初始化工作，否则，编译不会通过。（一定要先经过父类，才能开始子类）

```javascript
如果子类的构造器没有显示的调用父类的构造器，则将自动的调用默认的构造器，如果父类中没有不带参数的构造器，并且子类的构造器中没有显示的调用父类中的其他构造器，则会报错。
```

4. 如果希望指定去调用父类的某个构造器，则显式的调用一下：super(参数列表)
5. super在使用时，必须放在构造器的第一行（super只能在构造器是使用）
6. super() 和 this() 都只能放在构造器的第一行，因此这两个方法不能共存在一个构造器中
7. Java所有类都是Object类的子类，object是所有类的基类
8. 父类构造器的调用不限于直接父类！将一直往上追溯到object类（顶级父类）
9. 子类最多只能继承一个父类（指直接继承），即Java中是**单继承机制**
10. 不能滥用继承，子类和父类必须满足is—a的逻辑关系

## 继承的本质

当子类对象创建好后，建立**查找的关系**

1. 当new对象时，首先在方法区按照从上到下的查找顺序（object -> GrandPa -> Father -> Son）加载类的信息，到方法区里
2. 然后在堆中开辟一个空间（0X11），按照从上到下的顺序依次加载类的属性（ GrandPa -> Father -> Son）
   先是爷爷的name、hobby，然后是爸爸的name、age，最后是儿子的name（注意各自的地址都是不同的）

![](https://img-blog.csdnimg.cn/3baab4a857f14fc99bd4511367a86e76.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

 **怎么使用类中的属性呢？（查找关系）**从下到上

（1）首先看子类是否有该属性

（2）如果子类有这个属性，并且可以访问，则返回信息

（3）如果子类没有这个属性，就看父类有没有这个属性（如果父类有该属性，并且可以访问，就返回信息）

（4）如果父类没有就按照（3）的规则，继续向上查找上级父类，直到object

**注意：**

1. 不能越级访问：如果爸爸中有age，爷爷中也有age，但是爸爸的age是私有的，爷爷的age是公共的（先查找到爸爸的age，但是因为是private的，就直接报错了，不会跳过爸爸的age，直接访问爷爷的age  ）
2. **this相当于调用自身类的构造器，并且this 和 super只能存在一个**



## super

**基本介绍：**super代表父类的引用，用于访问父类的属性、方法、构造器



**基本语法：**

1. 访问父类的属性，但不能访问父类的private属性（super.属性名）

2. 访问父类的方法，不能访问父类的private方法

3. 访问父类的构造器，只能放在构造器 的第一句

4. ```java
   package com.user.super_;
   
   public class super01 {
       public static void main(String[] args) {
       }
   }
   
   class Father {
       public int n1 = 100;
       protected int n2 = 200;
       int n3 = 300;
       private int n4 = 400;
   
       public Father() {}
   
       public Father(int n1) {
           this.n1 = n1;
       }
   
       public Father(int n1, int n2, int n3, int n4) {
           this.n1 = n1;
           this.n2 = n2;
           this.n3 = n3;
           this.n4 = n4;
       }
   
       public void test1() {}
       protected void test2() {}
       void test3() {}
       private void test4() {}
   }
   
   class Son extends Father {
       //1.访问父类的属性，但不能访问父类的private属性
       public void t1() {
           System.out.println(super.n1 + " " + super.n2 + " " + super.n3);
       }
   
       //2.访问父类的方法，不能访问父类的private方法
       public void t2() {
           super.test1();
           super.test2();
           super.test3();
       }
   
       //3.访问父类的构造器，只能放在构造器的第一句，只能出现一句
       public Son() {
           //super();    //调用父类的无参构造器
           //super(100);    //调用父类的含有一个参数构造器
           super(100, 200, 300, 400); //调用父类的含有4个参数构造器
       }
   }
   ```

   

**注意：**

- ==使用**super**访问方法时，是跳过本类，直接从父类开始查找对应的方法==。（有效解决子类父类方法重名的问题）
- ==而 **this** 和 **未声明的方法** 都是直接从本类开始查找的==。
- 属性同理。
- 如果查找到有相应的属性或方法，但是不能访问（private），则会报错

```java
//父类和子类有同名的sum方法，以下是子类的方法
public void sum(){
    System.out.println("son");
}
public void cal() {
    this.sum();     //调用子类的sum
    super.sum();    //调用父类的sum
    super.cal();    //这个是调用了父类的cal方法
    System.out.println("调用子类的sum()方法");
    //son father 调用父类的cal()方法 调用子类的sum()方法
}
```



## super和this的比较

![](https://img-blog.csdnimg.cn/33bfe7f3137344a1a9fb4da208b346bd.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)



---

# 10. 方法重写/覆盖（override）

## 基本介绍：

方法覆盖（重写）就是子类有一个方法，和父类的某个方法的名称、返回类型、参数一样，那么我们就说子类的这个方法覆盖了父类的方法。



## **注意事项和使用细节：**

1. 子类的方法的**形参列表、方法名称** 要父类的 **形参列表、方法名称** 完全一样。
2. 子类方法的返回类型和父类返回**类型一样**，或者是父类返回类型的**子类**。==（子类的返回类型是父类返回类型是子集）==
   比如：父类是object，子类的返回类型是String（可以）
3. 子类方法**不能缩小**父类方法的访问权限。（public > protected > 默认 > private)



## **重写和重载的区别**

|       名称       | 发生范围 |  方法名  |              形参列表              |                           返回类型                           |               修饰符               |
| :--------------: | :------: | :------: | :--------------------------------: | :----------------------------------------------------------: | :--------------------------------: |
| 重载（overload） |   本类   | 必须一样 | 类型，个数或者顺序至少有一个不一样 |                            无要求                            |               无要求               |
| 重写（override） |  父子类  | 必须一样 |                相同                | 子类重写的方法，返回类型、和父类返回的类型一致，或者是其子类 | 子类方法不能缩小父类方法的访问范围 |



---

# 11. 多态

## 什么是多态，多态的具体体现有哪些？

**多态：**方法或对象具有多种形态，是OOP的第三大特征，是建立在封装和继承基础之上

**具体体现：**

1. 方法多态：
   （1）重载
   （2）重写
2. 对象多态：
   （1）对象的编译类型行和运行类型可以不一致，编译类型在定义时，就确定，不能变化
   （2）对象的运行类似是可以变化的，可以通过getClass() 来查看运行类型

 ## 基本介绍

方法或对象具有多种形态。是面向对象的第三大特征，多态是建立在封装和基础基础上的。



## 具体体现

1. 方法的多态：重写和重载就体现多态

2. 对象的多态：

   （1）编译类型看定义时 = 号的左边，运行类型看 = 号右边。==（编译看左，运行看右）==
   （2）一个对象的编译类型和运行类型可以不一致。

   （3）编译类型在定义对象是，就确定了，不能改变。
   （4）运行类型是可以变化的。

   ```java
   Animal animal = new Dog();	//animal编译类型是Animal，运行类型是Dog
   animal = new Cat();		//animal 运行类型变为了Cat，但是编译类型任是Animal
   ```

    

## 注意事项和细节

**多态的前提是：两个对象（类）存在继承关系**

- ==多态的向上转型==
  （1）本质：父类的引用指向了子类的对象
  （2）语法：父类类型	引用名 = new 子类类型();

  （3)特点：编译类型看左，运行类型看右
  	可以调用父类中的所有成员（需遵循访问权限），不能调用子类中特有的成员，最终运行效果看子类
  
  ```java
  //1.可以调用父类（编译类型）中的所有成员（需遵守访问权限）
  animal.eat();
  animal.run();
  animal.show();
  animal.sleep();
  
  //2.但是不能调用子类的特有的成员
  //因为在编译阶段，能调用哪些成员，是由编译类型来决定的
  animal.catchMouse();    //子类的特有的方法，无法调用
  
  //3.最终运行效果看子类（运行类型）的具体实现，即调用方法时，按照从子类（运行类型）开始查找方法
  //然后调用，规则与之前的调用规则一致（从下到上查找）
  animal.eat();   //猫吃鱼
  animal.run();   //跑 
  animal.show();  //hello，你好
  animal.sleep(); //睡
  ```
  
  

- ==多态的向下转型==
  （1）语法： 子类类型  引用名  =  （子类类型）  父类引用；
  （2）只能强转父类的引用，不能强转父类的对象
  （3）要求父类的引用必须指向的是当前目标类型的对象
  （4）可以调用子类类型中所有的成员

  ```javascript
  //怎么调用Cat的catchMouse方法？
  //多态的向下转型
  //1、语法：子类类型 引用名 = （子类类型） 父类引用;
  Cat cat = (Cat) animal;
  cat.catchMouse();   //就可以使用catchMouse方法
  cat.run();
  cat.eat();
  cat.show();
  cat.sleep();
  
  //2、要求父类的引用必须指向的是当前目标类型的对象（右边的必须之前就是指向左边的类型）
  //以下语句不行，animal之前指向的是Cat类，animal之前必须指向的是Dog类才行
  Dog dog = (Dog) animal; 
  ```



## 属性重写问题

**注意：**属性没有重写之说。属性的值看编译类型。

```javascript
public class test01 {
    public static void main(String[] args) {
        //属性没有重写，属性的值看编译类型
        Father f = new Father();
        System.out.println(f.count);    //输出20
        Son s = new Son();
        System.out.println(s.count);    //输出10
        Father f1 = new Son();
        System.out.println(f1.count);   //输出20
    }
}
class Son extends Father{
    int count = 10;
}
class Father{
    int count = 20;
}
```



## instanceOf  比较操作符

**作用：**用于判断==对象的运行类型==是否为XX类型或XX类型的子类型。

```javascript
Son s = new Son();
System.out.println(s instanceof Son);   //true
System.out.println(s instanceof Father);    //true

//f 编译类型是Father,运行类型是Son
Father f = new Son();
System.out.println(f instanceof Son);   //true
System.out.println(f instanceof Father);    //true

String str = "jack";
System.out.println(str instanceof Father);  //false
```



## 动态绑定机制（重要）

1. 当调用对象方法时，该方法会和该对象的内存地址/运行类型绑定。==（只要看到了方法，就从运行类型开始查找，不管子类父类）==
2. 当调用对象属性时，没用动态绑定机制，哪里声明，哪里使用。==（属性就只要看当前方法所在的类对应的属性就行）==



## 多态数组

```javascript
public class PloyArray {
    public static void main(String[] args) {
        Person[] person = new Person[3];
        person[0] = new Person("jack", 20);
        person[1] = new Student("james", 20, 90);
        person[2] = new Teacher("CCH", 30, 30000);
        for (int i = 0; i < person.length; i++) {
            if (person[i] instanceof Student) {
                Student student = (Student) person[i];
                student.learn();
            } else if (person[i] instanceof Teacher) {
                Teacher teacher = (Teacher) person[i];
                teacher.teach();
            }
            System.out.println(person[i].say());
        }
    }
}
```



---

# 12.Object类

## equals方法 与 ==

==：运算符

1. 既可以判断基本类型，又可以判断引用类型。
2. 如果判断基本类型，判断的是值是否相等。
3. 如果判断引用类型，判断的是地址是否相等。

equals: 

1. 是Object类中的方法，只能判断引用类型
2. 默认判断的是地址是否相等，子类中往往重写该方法，用于判断内容是否相等。
   比如：integer、String



## hashCode方法

**作用：**返回对象的哈希码值，支持此方法是为了提高哈希表的性能



**注意：**

1. 提高具有哈希结构的容器的效率
2. **两个引用，如果指向的是同一个对象，则哈希值肯定是一样的**
3. **两个引用，如果指向的是不同对象，则哈希值是不一样的**
4. 哈希值主要根据地址号类的，不能完全将哈希值等价于地址



## toString方法

**基本介绍：**

1. 默认返回：全类名 + @ + 哈希值的十六进制

```javascript
//toString源码
//1.getClass().getName() :是类似全类名（包名+类名）
//2.Integer.toHexString(this.hashCode()) :将对象的hashCode值转换为16进制字符串
public String toString() {
    return this.getClass().getName() + "@" + Integer.toHexString(this.hashCode());
}
```

2. 子类往往重写toString方法，用于返回对象的属性信息(使用快捷键 alt+ ins)

```java
@Override                                       
public String toString() {                      
	return "Monster{" +                         
		"name='" + name + '\'' +            
         ", job='" + job + '\'' +            
         ", age=" + age +                    
         '}';                                
}                                               

```

3. 输出一个对象时，会默认调用toString

```javascript
Monster monster = new Monster("jack", "code", 100);   
System.out.println(monster.toString());               
System.out.println(monster);    //自动调用toString                      
```



## finalize方法

1. 当对象被回收时，系统自动调用该对象的finalize方法。子类可以重写该方法，做一些**释放资源**的操作。

2. 什么时候被回收：当某个对象没有任何引用时，则JVM就认为这个对象是一个垃圾对象，就会使用垃圾回收机制来销毁该对象，在销毁该对象前，会先调用finalize方法

3. 垃圾回收机制的调用，是有系统来决定（即有自己的GC算法，设计到jvm垃圾回收机制），也可以通过System.gc() 主动触发垃圾回收机制。

4. 几乎不会使用

5. 但是面试可能考

   ```java
   public class Car {
       public static void main(String[] args) {
           PC dell = new PC("DELL");
           dell = null;
           //这时car对象就是 一个垃圾，垃圾回收器就会回收(销毁)对象，在销毁对象前，会调用该对象的finalize方法
           //，程序员就可以在finalize中，写自己的业务逻辑代码(比如释放资源:数据库连接，或者打开文件..)
           //，如果程序员不重写finalize,那么就会调用Object类的finalize, 即默认处理
           // ,如果程序员重写了finalize, 就可以实现自己的逻辑
           System.gc();
   
   
       }
   }
   
   class PC {
       private String name;
   
       public PC(String name) {
           this.name = name;
       }
   
       //重写finalize
   
       @Override
       protected void finalize() throws Throwable {
           System.out.println("垃圾回收了");
       }
   }
   ```




---

# 13. 类变量/静态变量、类方法

## 类变量/静态变量内存布局

详细内存布局现在不分析，之后再详细研究（不同版本的JDK存在不同的地方）

**注意：**

1. static变量的同一个类所有对象共享
2. static类变量，在类加载的时候就生成了



## 类变量的类方法

- **什么是类变量：**

  类变量也叫静态变量/静态属性，是该类的所有对象共享的变量，任何一个该类的对象去访问它时,取到的都是相同的值,同样任何一个该类的对象去修改它时修改的也是同一个变量。这个从前面的图也可看出来。

- **如何定义类变量**

  ```java
  访问修饰符 static 数据类型 变量名;	//推荐
  static 访问修饰符 数据类型 变量名;
  ```

- **如何访问类变量**

  ```javascript
  package static_;
  
  public class testStatic {
      public static void main(String[] args) {
  
          //类名.类变量名
          //说明：类变量是随着类的加载而创建，所以即使没有创建对象实例也可以访问
          System.out.println(A.name);
  
          A a = new A();
          //通过对象名.类变量名
          //前提是满足访问权限
          System.out.println(a.name);
      }
      
  }
  class A{
      public static String name = "wxd";
  }
  ```



## 类变量使用注意事项和细节讨论

1. 什么时候需要用类变量？
   当我们需要让某个类的所有对象都共享一个变量时，就可以考虑使用类变量
2. 类变量与实例变量（普通属性）区别：
   类变量是该类的所有对象共享的，而实例变量是每个对象独享的
3. 加上static称为类变量或静态变量，否则称为实例变量/普通变量/非静态变量
4. 类变量可以通过**类名.类变量名** 或 对象名.类变量名 来访问，但推荐使用 **类名.类变量名** 来访问。==（前提是满足访问修饰符的访问权限和范围）==
5. 实例变量不能通过 类名.类变量名 方式访问
6. 类变量是在类加载时就初始化了，也就是说，即使没有创建对象，只要类加载了，就可以使用类变量
7. ==类变量的生命周期是随类的加载开始，随着类的消亡而销毁==



## 类方法

1. **类方法经典的使用场景：**
   *当方法中==不涉及到任何和对象相关的成员==，则可以将方法设计成静态方法，提高开发效率*
2. **注意：**
   类方法和普通方法都随着类的加载而加载，将结构信息存储在方法区：（**类方法中无this的参数**）

```java
package static_;

public class testStatic {
    public static void main(String[] args) {

        //类名.类变量名
        //说明：类变量是随着类的加载而创建，所以即使没有创建对象实例也可以访问
        System.out.println(A.name);

        A a = new A();
        //通过对象名.类变量名
        //前提是满足访问权限
        System.out.println(a.name);
        
        //直接调用类方法
        A.payFee(100);
    }
    
}
class A{
    public static String name = "wxd";
	public static double fee = 0;
	//类方法
	public static double payFee(double fee){
        //通过类名来访问
        A.fee += fee;
    }
}
```



## 类方法使用细节

1. 类方法中不允许使用和对象有关的关键字，比如this和super。普通方法可以
2. 类方法（静态方法）只能访问 静态变量 或 静态方法。
3. 普通成员方法，既可以访问 普通变量（方法），也可以访问静态变量（方法）

==静态方法，只能访问静态的成员；非静态的方法，可以访问静态成员和非静态成员==



## main方法

**解释：**

```javascript
解释main方法的形式：public static void main(String[] args){}

//1. main方法是虚拟机调用的

//2. jvm需要调用类的main()方法，所以该方法的访问权限必须是public

//3. jvm需要调用main（）方法时不必创建对象，所以该方法必须是 static

//4. 该方法接受String类型的数组，该数组中报错执行Java命令是传递给所运行的类的参数

//5. java执行程序时，将参数传入 参数1 参数2 参数3
```



**特别提示：**

1. 在main()方法中，我们可以直接调用main方法所在类的静态方法或静态属性
2. 但是，不能直接访问给类中的非静态成员，必须创建该类的一个实例对象后，才能通过这个对象去访问类中的非静态成员



**在IDEA中怎么传入agrs实参**

下图中选中要运行的类，后在箭头所指的框中输入要传入的参数就行

![](https://img-blog.csdnimg.cn/56eb12fb80c2495690f8266a2f6abb0f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)



---

# 14. 代码块

### 基本介绍

代码块又称为**初始化块**，属于类中的成员（即是类的一部分），类似于方法，将逻辑语句封装在方法体中，通过{} 包围起来。

但和方法不同，没有方法名，没有返回，没有参数，只有方法体，而且不用通过对象或类显示调用，而是加载类时，或创建对象时隐式调用



### 说明注意

1. 修饰符可选，要写的话，也只能写static
2. 代码块分为两类，使用static修饰的叫静态代码块，没有static修饰的，叫普通代码块
3. 逻辑语句可以为任何逻辑语句（输入、输出、方法调用）



### 使用和理解

- 不管调用那个构造器，创建对象，都会先调用代码块的内容
- 代码块调用的顺序优先于构造器

```javascript
//相当于另外一种形式的构造器（对构造器的补充机制），可以做初始化的操作
//场景：如果多个构造器中都有重复的语句，可以抽取到初始化块中，提高代码的重用性

class movie {
    private String name;
    private double price;
    private String director;

    //（1）下面的三个构造器都有相同的语句，这样代码看起来比较冗余
    // (2) 这时我们可以把相同的语句，放入到一个代码块中
    //（3）这样我们不管调用那够构造器，创建对象，都会先调用代码块的内容
    //（4）代码块调用的顺序优先于构造器

    {
        System.out.println("电影开始放映");
        System.out.println("开始放映广告");
    }
    public movie(String name) {
        this.name = name;
    }

    public movie(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public movie(String name, double price, String director) {
        this.name = name;
        this.price = price;
        this.director = director;
    }
}
```



### 细节

- static代码块也叫静态代码块，作用是对类进行初始化，而且它随着**类的加载**而执行，并且**只会执行一次**。如果是普通代码块，每创建一个对象，就执行一次。
- ==类什么时候被加载==
  1. 创建对象实例时（new）
  2. 创建子类对象实例，父类也会被加载（类加载是从上到下加载）
  3. 使用类的静态成员时（静态属性、静态方法）
- **普通的代码块，在创建对象实例时，会被隐式调用，被创建一次，就会调用一次，如果只是使用类的静态成员时，普通代码块并不会执行**

**总结：**

1. static代码块是类加载时，执行，只会执行一次
2. 普通代码块是在创建对象时调用的，创建一次，调用一次
3. 类加载的3种情况，需要记清楚

### 在一个类中的加载情况

①调用静态代码块和静态属性初始化(注意:静态代码块和静态属性初始化调用的优先级一样，如果有多个静态代码块和多个静态变量初始化，则按他们定义的顺序调用) [举例说明]
②调用普通代码块和普通属性的初始化(注意:普通代码块和普通属性初始化调用的优先级一样，如果有多个普通代码块和多个普通属性初始化，则按定义顺序调用)
③调用构造方法。

**加载顺序：**==静态代码块 > 普通代码块 > 构造方法==



- 构造器的最前面其实隐含了super（）和调用普通代码块，静态相关的代码块，属性初始化，在类加载时，就执行完毕

  ```javascript
  class A{
  	public A(){//构造器
  		//这里有隐藏的执行要求
  		//(1)先执行super();
  		//(2)在执行普通代码块
  		//(3)最后执行构造器中的相关逻辑
  		sout("ok");
  	}
  }
  ```

  

### 继承中的加载关系

顺序依次为：

①父类的静态代码块和静态属性(优先级一样，按定义顺序执行)
②子类的静态代码块和静态属性(优先级一样，按定义顺序执行)
③父类的普通代码块和普通属性初始化(优先级一样，按定义顺序执行)
④父类的构造方法
⑤子类的普通代码块和普通属性初始化(优先级一样，按定义顺序执行)
⑥子类的构造方法



**静态代码块只能直接调用静态成员（静态属性和静态方法），普通代码块可以调用任意成员。**

---

### final关键字

**使用场景：**

1. 当不希望类被继承时，可以用final修饰. ( 案例演示]

   ```java
   //类A不能被继承
   final class A{}
   ```

2. 当不希望父类的某个方法被子类覆盖/重写(override)时，可以用final关键字修饰。
   (案例演示:访问修饰符final 返回类型方法名)

   ```java
   class C {
   	public final void so(){
           //该方法在子类中无法重写
   	}
   }
   
   class D extends C {
   	public void so(){
   		//无法重写，父类的so方法
   	}
   }
   ```

   

3. 当不希望类的的某个属性的值被修改，可以用final修饰. 

   ```java
   //属性不能被直接修改
   class A{
       //num不允许修改
       public fainl int num = 100;
   }
   ```

   

4. 当不希望某个局部变量被修改，可以使用final修饰



**使用细节：**

1. final修饰的属性又叫常量，一般用XX_XX_XX来命名

2. final修饰的属性在定义时，必须赋初值，并且以后不能在修改，复制可以在如下位置之一选择：
   （1）定义时：public final double TAX_RATE = 0.2;
   （2）在构造器中
   （3）在代码块中

   ```java
   //属性不能被直接修改
   class A{
       //(1)定义时赋值
       public fainl int num = 100;
       //(2)构造器中赋值
       public A (){
           num = 100;
       }
       //(3)代码块中赋值
       {
           num = 100;
       }
   }
   ```

   

3. 如果final修饰的属性是静态的，则初始化的位置只能是：（1）和（3）静态代码块中，不能在构造器中赋值

4. final类不能继承，但是可以实例化对象

5. 如果类不是final类，但是含有final方法，则该方法虽然不能重写，但是可以被继承

6. final不能修饰构造器方法

7. final和static往往搭配使用，效率更高，底层编译器做了优化处理

8. 包装类（Integer、Double、Float、Boolean等都是final），String也是final类

---

# 15. 设计模式（23）

## （1）单例设计模式

所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中， 对某个类只能存在一个对象实例， 并且该类只提供-个取得其对象实例的方法，

### 饿汉式

**实现步骤：**

1. 防止直接new
2. 类的内部创建对象
3. 向外部暴露已经静态对的公共方法

**存在的问题：**在类加载的时候就创建，可能存在紫云啊浪费的问题

```javascript
public class test {
    public static void main(String[] args) {

    }
}

class GriFriend {
    private String name;

    //为了能够在静态方法中，返回gf对象，需要将其修饰为static
    private static GriFriend gf = new GriFriend("小红");

    //如何保障我们只能创建一个GriFriend对象
    //步骤
    //1. 将构造器私有化
    //2. 在类的内部直接创建
    //3. 提供一个公共的static方法，返回gf对象
    private GriFriend(String name) {
        this.name = name;
    }

    public static GriFriend getInstance() {
        return gf;
    }
}

```



### 懒汉式

**存在的问题：**线程安全问题，后面学习线程后，进行完善

```javascript
public class t2 {
    public static void main(String[] args) {

        Cat c1 = Cat.getInstance();
        System.out.println(c1);
        Cat c2 = Cat.getInstance();
        System.out.println(c2);
    }
}

//希望在程序运行过程中，只能创建一个Cat对象
//使用单例模式
class Cat {
    private String name;
    public static Cat cat;

    //步骤
    //1. 任然使用构造器私有化
    //2. 定义一个static静态属性对象
    //3. 提供一个public的static方法，可以返回一个cat对象
    private Cat(String name) {
        this.name = name;
    }

    public static Cat getInstance() {
        if (cat == null) {//如果还没有创建cat对象
            cat = new Cat("猫咪");
        }
        return cat;
    }

    @Override
    public String toString() {
        return "Cat{" +
                "name='" + name + '\'' +
                '}';
    }
}

```



---

# 16. 抽象类

视频链接：[【零基础 快速学Java】韩顺平 零基础30天学会Java_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1fh411y7R8?p=398)

### 抽象类介绍

1. 用abstract关键字来修饰一个类时，这个类就叫抽象类；修饰方法，就叫抽象方法。

   ```java
   //有点类似C++的virtual
   public abstruct void so(){
   
   }
   ```

2. 抽象类的价值更到作用在于设计，是设计者设计好后，让子类继承并实现



### 注意事项和使用细节

1. 抽象类**不能被实例化**（不能new出来）

2. 抽象类不一定要包含abstract方法。也就是说，抽象类可以没有abstract方法

3. 一旦类包含了abstract方法，则这个类必须声明为abstract

4. **abstract只能修饰类和方法**，不能修饰属性和其他的。

5. 抽象类可以有任意成员（抽象类本质还是类），比如说：非抽象方法、构造器、静态属性等

6. 抽象方法不能有主体，即不能实现内部的方法

   ```java
   //有点类似C++的virtual
   public abstruct void so(){}
   ```

7. 如果一个类继承了抽象类，则他必须实现抽象类的所有抽象方法，除非它自己也声明为abstract类。 

8. 抽象方法不能使用private、final和static来修饰，因为这些关键字都是和重写相违背的 



---

# 17. 接口

### 基本介绍

**接口**就是给出一些没有实现的方法，封装到一起，到谋而类要使用的时候，在根据具体情况吧这些方法写出来。

**语法：**

```java
interface 接口名 {
    //属性
    public int n1;
    //方法（在接口中,不用写方法体）
    //1. 在接口中，抽象方法，可以省略abstract关键字
    public void hi();
    
    //问题？使用了以下方法实现接口方法的默认写法后，当一个类在使用接口后不能再实现那个方法了
    //2. 在JDK8后，可以有默认的实现方法，需要使用default关键字修饰
    default public void ok(){
        sou("ok...");
    }
    
    //而这个方法就可以实现含有接口含有方法体的方法
    //3. 在jdk8后，可以有静态方法
    public static void cry(){
        sou("cry...");
    }
}

//使用接口的类必须实现接口的抽象方法
class 类名 implements 接口 {
	自己的属性；
    自己的方法;
    必须实现接口的抽象方法;
}   	
```



### 注意事项和细节

1. **接口不能被实例化**

2. 接口中所有的方法是public方法（如果不写访问修饰符也是public），接口中抽象方法，可以不用abstract修饰（默认就是带有了abstruct）

3. 一个普通类实现接口，就**必须将该接口的所有方法都实现**

4. 抽象类实现接口，可以不用实现接口的方法

5. 一个类同时可以实现多个接口

   ```java
   //类就不存在，多继承写法
   interface A1{}
   interface B1{}
   class TEST implements A1,B1{
       
   }
   ```

6. 接口中的属性，只能是final的，而且是public static final 修饰符。比如

   ```java
   //必须初始化
   int a = 1;	//实际上是public static final int a = 1;
   ```

7. 接口中属性的访问形式：接口名.属性名

   ```java
   interface A1{
   	int a = 1;
   }
   A1.a;
   ```

8. 一个接口不能继承其他的类，但是可以继承多个别的接口

   ```java
   interface A1{}
   interface B1{}
   interface C1 extends A1,B1{}
   ```

9. 接口的修饰符只能是public和默认，这点和类的修饰符是一样的



### 接口VS继承类

**继承的价值主要在于：**解决代码的复用性和可维护性

**接口的价值主要在于：**设计好各种规范（方法），让其他类去实现这些方法，即更加灵活

**接口比继承更加灵活**



### 接口的多态

1. **多态参数**

   ```java
   public class duotai {
       public static void main(String[] args) {
           //接口的多态体现
           //接口类型的变量 ia 可以指向 实现了IA接口的对象实例
           IA ia = new C1();
           ia = new C2();
       }
   }
   
   interface IA {}
   class C1 implements IA {}
   class C2 implements IA {}
   ```

2. **多态数组**
   演示案例：给Usb数组中，存放Phone 和 Camera 对象，Phone类还有一个特有的方法call()，请遍历Usb数组，如果是Phone对象，除了调用Usb接口定义的方法外，还需要调用Phone特有的方法 call

   ```java
   public class array {
       public static void main(String[] args) {
           Usb usbs[] = new Usb[2];
           usbs[0] = new Phone();
           usbs[1] = new Camera();
   
           for (int i = 0; i < usbs.length; i++) {
               usbs[i].working();
               if(usbs[i] instanceof Phone){
                   //需要向下转型
                   ((Phone) usbs[i]).call();
               }
           }
       }
   }
   
   interface Usb{
       public void working();
   }
   class Phone implements Usb{
       @Override
       public void working() {
           System.out.println("手机正在工作...");
       }
       public void call(){
           System.out.println("手机可以打电话...");
       }
   }
   class Camera implements Usb{
       @Override
       public void working() {
           System.out.println("相机正在工作...");
       }
   }
   ```

3. **多态传递**

   ```java
   public class pass {
       public static void main(String[] args) {
           BB b = new TS();
           //相当于使用继承将接口AA传递给了类TS
           AA a = new TS();
       }
   }
   interface AA{
       void hi();
   }
   interface BB extends AA{}
   class TS implements BB{
       @Override
       public void hi() {
   
       }
   }
   ```



### 练习

```java
package test;

public class t1 {
    public static void main(String[] args) {
        CC1 cc1 = new CC1();
        cc1.show();
    }
}
interface It1{
    //默认为public static final int x = 10;
    int x = 10;
}
class C1{
    int x = 20;
}
class CC1 extends C1 implements It1{
    public void show(){
        //使用接口的属性就使用 It1.x
        //使用继承的属性就用 super.x
        System.out.println(It1.x);
        System.out.println(super.x);
    }
}

```



---

# 18. 内部类

### 基本介绍

==一个类的内部又完整的嵌套了另一个类结构。被嵌套的类称为内部类(inner class),嵌套其他类的类称为外部类(outer class)。==是我们类的第五大成员[思考:类的五大成员是哪些?[**属性、方法、构造器、代码块、内部类**]，内部类最大的特点就是可以直接访问私有属性，并且可以体现类与类之间的包含关系，**注意**:内部类是学习的难点，同时也是重点，后面看底层源码时，有大量的内部类.

### 内部类的分类

**➢定义在外部类局部位置上(比如方法内) :**

==（1)局部内部类(有类名)：==是定义在外部类的局部位置，比如方法中，并且有哦类名

1. 可以直接访问外部类的所有成员，包含私有的
2. 不能添加访问修饰 符,因为它的地位就是一个局部变量。局部变量是不能使用修饰符的。但是可以使用final修饰，因为局部变量也可以使用final
3. 作用域: 仅仅在定义它的方法或代码块中。
4. 局部内部类---访问---->外部类的成员 [访问方式:直接访问]
5. 外部类-- -访向---->局部内部类的成员【访问方式:创建对象，再访问(注意:必须在作用域内)】
6. 外部其他类， 不能访问 局部内部类（因为 局部内部类地位是一个局部变量）
7. 如果外部类和局部内部类的成员重名时，默认遵守就近原则，如果想访问外部类的成员，则可以使用（外部类名.this.成员）访问

```java
class Outer02 {//外部类
    private int n1 = 100;
    private void m2() {
        System.out.println("Outer02 m2()");
    }//私有方法

    public void m1() {// 方法
        //1.局部内部类是定义在外部类的局部位置，通常在方
        //3.不能添加访问修饰符,但是可以使用final修饰
        //4.作用域:仅仅在定义它的方法或代码块中
        
        final class Inner02 {//局部内部类(本质仍然是一个类)
            //2. 可以直接访问外部类的所有成员，包含私有的
            private int n1 = 200;
            public void f1 () {
                //5.局部内部类可以直接访问外部类的成员，比如下面外部类n1和m2()             
                System.out.println("n1=" + n1);
                 //7.如果外部类和局部内部类的成员重名时，默认遵守就近原则，如果想访问外部类的成员，则可以使用（外部类				   //  名.this.成员）访问
                //  理解：Outer02.this 本质就是外部类的对象，即那个对象调用了m1方法，Outer02.this就是哪个对象
                System.out.println("n1=" + Outer02.this.n1);
                m2();
            }
        }
        //6.外部类在方法中，可以创建Inner02对象，然后调用方法即可
        Inner02 inner02 = new Inner02();
        inner02.f1();
    }
}
```



==（2)匿名内部类(没有类名,重点!!!)==

```
????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????
```

**➢定义在外部类的成员位置上:**
1)成员内部类(没用static修饰)

```java
public class MemberInnerClass01 {
	public static void main(String[] args) {
        Outer08 outer08 = new Outer08();
        outer08.t1();
        //外部其他类，使用成员内部类的三种方式
        //老韩解读
        // 第一种方式
        outer08.new Inner08(); 相当于把 new Inner08()当做是 outer08 成员
        // 这就是一个语法，不要特别的纠结. Outer08.Inner08 inner08 = outer08.new Inner08();
        inner08.say();
        // 第二方式 在外部类中，编写一个方法，可以返回 Inner08 对象
        Outer08.Inner08 inner08Instance = outer08.getInner08Instance();
        inner08Instance.say();
      }
}

class Outer08 { //外部类
        private int n1 = 10;
        public String name = "张三";
        private void hi() {
        	System.out.println("hi()方法...");
    	}
        
        //1.注意: 成员内部类，是定义在外部内的成员位置上
        //2.可以添加任意访问修饰符(public、protected 、默认、private),因为它的地位就是一个成员
        public class Inner08 {//成员内部类
            private double sal = 99.8;
            private int n1 = 66;
        	public void say() {
                //可以直接访问外部类的所有成员，包含私有的
                //如果成员内部类的成员和外部类的成员重名，会遵守就近原则.可以通过 外部类名.this.属性 来访问外部类的成员
                System.out.println("n1 = " + n1 + " name = " + name + " 外部类的 n1=" + Outer08.this.n1);
                hi();
			}
		}
    
        //方法，返回一个 Inner08 实例
        public Inner08 getInner08Instance(){
            return new Inner08();
   		}
    
   		//写方法
    	public void t1() {
            //使用成员内部类
            //创建成员内部类的对象，然后使用相关的方法
            Inner08 inner08 = new Inner08();
            inner08.say();
            System.out.println(inner08.sal);
     	}
}
```

2)静态内部类(使用static修饰)





----

# 19. 枚举类和注解

### 自定义枚举类

**使用说明：**

1. 构造器私有化
2. 本类内部创建一组对象（一般都是有限个的对象）
3. 对外暴露对象（通过为对象添加 public final static 修饰符）
4.  可以提供 get 方法，但是不要提供 set

```java
public class Enumeration02 {
	public static void main(String[] args) {
        System.out.println(Season.AUTUMN);
        System.out.println(Season.SPRING);
	}
}

//演示字定义枚举实现
class Season {//类
    private String name;
    private String desc;//描述
    
    //定义了四个对象, 固定. 
    public static final Season SPRING = new Season("春天", "温暖");
    public static final Season WINTER = new Season("冬天", "寒冷");
    public static final Season AUTUMN = new Season("秋天", "凉爽");
    public static final Season SUMMER = new Season("夏天", "炎热");
    
    //1. 将构造器私有化,目的防止 直接 new
	//2. 去掉 setXxx 方法, 防止属性被修改
    //3. 在 Season 内部，直接创建固定的对象
	//4. 优化，可以加入 final 修饰符
    
    private Season(String name, String desc) {
        this.name = name;
        this.desc = desc;
	}
    
    public String getName() {
        return name;
	}
    public String getDesc() {
    	return desc;
    }
}    
```



### enum关键字

**注意：**

1. 使用enum关键字后，就不能再继承其他类了，因为已经隐式地继承了Enum类
2. 枚举类和普通类一样，可以实现接口

**使用方法**

```java
public class t1 {
    public static void main(String[] args) {
        System.out.println(Season.SPRING);
        System.out.println(Season.SUMMER);
    }
}

enum Season {
    //1. 使用关键字 enum 替代 class
    //2. public static final SPRING = new Season("春天", "温暖") 直接使用 SPRING("春天", "温暖")代替
    //3. 如果有多个常量对象，使用逗号间隔
    //4. 如果使用enum来实现枚举，要求定义常量对象，写在前面
    SPRING("春天", "温暖"), WINTER("冬天", "寒冷"), AUTUMN("秋天", "凉爽"), SUMMER("夏天", "炎热");
    private String name;
    private String desc;

    Season(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }

    public String getName() {
        return name;
    }

    public String getDesc() {
        return desc;
    }
}
```



### Enum成员方法

**说明**：使用关键字 enum 时，会隐式继承 Enum 类, 这样我们就可以使用 Enum 类相关的方法。

**enum常用方法举例：**

1. toString:Enum 类已经重写过了，返回的是当前对象 名,子类可以重写该方法，用于返回对象的属性信息 
2.  name：返回当前对象名（常量名），子类中不能重写
3.  ordinal：返回当前对象的位置号，默认从 0 开始 
4. values：返回当前枚举类中所有的常量
5.  valueOf：将字符串转换成枚举对象，要求字符串必须 为已有的常量名，否则报异常！
6.  compareTo：比较两个枚举常量，比较的就是编号！

```java
public class EnumMethod {
	public static void main(String[] args) {
        //使用 Season2 枚举类，来演示各种方法
        Season2 autumn = Season2.AUTUMN;
        
        //输出枚举对象的名字
        System.out.println(autumn.name());
        
        //ordinal() 输出的是该枚举对象的次序/编号，从 0 开始编
        //AUTUMN 枚举对象是第三个，因此输出 2
        System.out.println(autumn.ordinal());
        
        //从反编译可以看出 values 方法，返回 Season2[]
        //含有定义的所有枚举对象
        Season2[] values = Season2.values();
        System.out.println("===遍历取出枚举对象(增强 for)====");
        for (Season2 season: values) {//增强 for 循环
        	System.out.println(season);
        }
        
        //valueOf：将字符串转换成枚举对象，要求字符串必须为已有的常量名，否则报异常
        //执行流程
        //1. 根据你输入的 "AUTUMN" 到 Season2 的枚举对象去查找
        //2. 如果找到了，就返回，如果没有找到，就报错
        Season2 autumn1 = Season2.valueOf("AUTUMN");
        System.out.println("autumn1=" + autumn1);
        System.out.println(autumn == autumn1);
        
        //compareTo：比较两个枚举常量，比较的就是编号
        //老韩解读
        //1. 就是把 Season2.AUTUMN 枚举对象的编号 和 Season2.SUMMER 枚举对象的编号比较
        //2. 看看结果
        /*
        public final int compareTo(E o) 
                return self.ordinal - other.ordinal;
        }
        Season2.AUTUMN 的编号[2] - Season2.SUMMER 的编号[3]
        */
        System.out.println(Season2.AUTUMN.compareTo(Season2.SUMMER));
```



### 注解

**注解的理解：**

1. 注解(Annotation)也被称为元数据(Metadata)，用于修饰解释 包、类、方法、属性、构造器、局部变量等数据信息。
2. 和注释一样，注解不影响程序逻辑，但注解可以被编译或运行，相当于嵌入在代码中的补充信息。
3. 在 JavaSE 中，注解的使用目的比较简单，例如标记过时的功能，忽略警告等。在 JavaEE 中注解占据了更重要的角 色，例如用来配置应用程序的任何切面，代替 java EE 旧版中所遗留的繁冗代码和 XML



### 基本的 Annotation 介绍 

使用 Annotation 时要在其前面增加 @ 符号, 并把该 Annotation 当成一个修饰符使用。用于修饰它支持的程序元素

三个基本的 Annotation: 

1. ==@Override==: 限定某个方法，是重写父类方法, 该注解只能用于方法

   ![](https://img-blog.csdnimg.cn/b736efb79aad4d52bfe0471266dbff09.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

   ```javascript
   //1. @Override 注解放在 fly 方法上，表示子类的 fly 方法时重写了父类的 fly
   //2. 这里如果没有写 @Override 还是重写了父类 fly
   //3. 如果你写了@Override 注解，编译器就会去检查该方法是否真的重写了父类的
   // 	 方法，如果的确重写了，则编译通过，如果没有构成重写，则编译错误
   
   //4. 看看 @Override 的定义
   //   解读： 如果发现 @interface 表示一个 注解类
   /*
       @Target(ElementType.METHOD)
       @Retention(RetentionPolicy.SOURCE)
       public @interface Override {
       }
   */
   ```

   

2. ==@Deprecated==: 用于表示某个程序元素(类, 方法等)已过时

   ![](https://img-blog.csdnimg.cn/d6c054c6dc784b4680bfb1a2f9e1c02b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

   ```java
   //1. @Deprecated 修饰某个元素, 表示该元素已经过时，即不在推荐使用，但是仍然可以使用
   //2. 查看 @Deprecated 注解类的源码
   //3. 可以修饰方法，类，字段, 包, 参数 等等
   //4. @Deprecated 可以做版本升级过渡使用
   ```

   

3. ==@SuppressWarnings==: 抑制编译器警告

   ![](https://img-blog.csdnimg.cn/0df56ac00d244bf3b521ef87d5d0520c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

![](https://img-blog.csdnimg.cn/eb96d38aba2a4a5ca2b4c9777e119d4d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

```java
//老韩解读
//1. 当我们不希望看到这些警告的时候，可以使用 SuppressWarnings 注解来抑制警告信息
//2. 在{""} 中，可以写入你希望抑制(不显示)警告信息

//3. 可以指定的警告类型有
// all，抑制所有警告
// boxing，抑制与封装/拆装作业相关的警告
// //cast，抑制与强制转型作业相关的警告
// //dep-ann，抑制与淘汰注释相关的警告
// //deprecation，抑制与淘汰的相关警告
// //fallthrough，抑制与 switch 陈述式中遗漏 break 相关的警告
// //finally，抑制与未传回 finally 区块相关的警告
// //hiding，抑制与隐藏变数的区域变数相关的警告
// //incomplete-switch，抑制与 switch 陈述式(enum case)中遗漏项目相关的警告
// //javadoc，抑制与 javadoc 相关的警
// //nls，抑制与非 nls 字串文字相关的警告
// //null，抑制与空值分析相关的警告
// //rawtypes，抑制与使用 raw 类型相关的警告
// //resource，抑制与使用 Closeable 类型的资源相关的警告
// //restriction，抑制与使用不建议或禁止参照相关的警告
// //serial，抑制与可序列化的类别遗漏 serialVersionUID 栏位相关的警告
// //static-access，抑制与静态存取不正确相关的警告
// //static-method，抑制与可能宣告为 static 的方法相关的警告
// //super，抑制与置换方法相关但不含 super 呼叫的警告
// //synthetic-access，抑制与内部类别的存取未最佳化相关的警告
// //sync-override，抑制因为置换同步方法而遗漏同步化的警告
// //unchecked，抑制与未检查的作业相关的警告
// //unqualified-field-access，抑制与栏位存取不合格相关的警告
// unused，抑制与未用的程式码及停用的程式码相关的警告

//4. 关于 SuppressWarnings 作用范围是和你放置的位置相关
// 比如 @SuppressWarnings 放置在 main 方法，那么抑制警告的范围就是 main
// 通常我们可以放置具体的语句, 方法, 类. //5. 看看 @SuppressWarnings 源码
//(1) 放置的位置就是 TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE
//(2) 该注解类有数组 String[] values() 设置一个数组比如 {"rawtypes", "unchecked", "unused"}
/*
```



### 元注解

**作用**：JDK 的元 Annotation 用于修饰其他 Annotation



种类：

1. Retention //指定注解的作用范围，三种 SOURCE,CLASS,RUNTIME

   ```java
   @Retention 的三种值
   1) RetentionPolicy.SOURCE: 编译器使用后，直接丢弃这种策略的注释
   2) RetentionPolicy.CLASS: 编译器将把注解记录在 class 文件中. 当运行 Java 程序时, JVM 不会保留注解。 这是默认
   值
   3) RetentionPolicy.RUNTIME:编译器将把注解记录在 class 文件中. 当运行 Java 程序时, JVM 会保留注解. 程序可以
   通过反射获取该注解
   ```

   

2. Target // 指定注解可以在哪些地方使用

   ![](https://img-blog.csdnimg.cn/14169ecf19f24d56afa9c2c7e477c0ae.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

3. Documented //指定该注解是否会在 javadoc 体现

   ![](https://img-blog.csdnimg.cn/44cfae97c4904cc5b256aea00e1e0b34.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)

4. Inherited //子类会继承父类注解

![](https://img-blog.csdnimg.cn/9f3bd5c417ea459495334adf2c7d372e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)



# 20. 异常

### 基本概念

Java语言中，将程序执行中发生的不正常的情况称为“异常”（开发过程中的语法错误和逻辑错误不是异常）

- 执行过程中所发生的异常事件可分为两大类

  （1）Error（错误）：java虚拟机无法解决的严重问题。如：jvm系统内部错误，资源耗尽等严重情况。

  （2）Exception：其他因编程错误或偶然的外在因素导致的一般性问题，可以使用针对性的代码进行处理。对性的代码进行处理。例如空指针访问，试图读取不存在的文件，网络连接中断等等。

  Exception 分为两大类: ==运行时异常[程序运行时，发生的异常]== 和 ==编译时异常[编程时，编译器检查出的异常]==。



### 异常体系图

![](https://img-blog.csdnimg.cn/8225b15c54bf480bb8dbc78e9ffe043b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAWFhEVEVOVA==,size_20,color_FFFFFF,t_70,g_se,x_16)



### 五大运行时异常

1. **NullPointerException 空指针异常**

   ```java
   public class NullPointerExp {
       public static void main(String[] args) {
           String name = null;
           //空指针异常
           System.out.println(name.length());
       }
   }
   ```

2. **ArithmeticException 数学运算异常**

   ```java
   public class math {
       public static void main(String[] args) {
           int num1 = 10;
           int num2 = 0;
           //数学运行异常
           int res = num1/num2;
           System.out.println("程序继续执行...");
       }
   }
   ```

3. **ArrayIndexOutOfBoundsException 数组下标越界异常**

   不举例了，好理解

4. **ClassCastException 类型转换异常**

   ```javascript
   public class ClassCastException_ {
   	public static void main(String[] args) {
           A b = new B(); //向上转型
           B b2 = (B)b;//向下转型，这里是 OK
           C c2 = (C)b;//这里抛出 ClassCastException
   	}
   }
   class A {}
   class B extends A {}
   class C extends A {}
   ```

   

5. **NumberFormatException 数字格式不正确异常[]**

```javascript
public class NumberFormatException_ {
	public static void main(String[] args) {
        String name = "韩顺平教育";
        //将 String 转成 int
        int num = Integer.parseInt(name);//抛出 NumberFormatException
        System.out.println(num);//1234
	}
}	
```



### 编译异常

**介绍：**

编译异常是指在编译期间，就必须处理的异常，否则代码不能通过编译



**常见的编译异常：**

1. SQLException ：操作数据库时，查询表可能发生异常
2. IOException ：操作文件时，发生的异常
3. FileNotFoundException ：当操作一个不存在的文件时，发生异常
4. ClassNotFoundException ：加载类，而该类不存在时，异常
5. EOFException ：操作文件，到文件末尾，发生异常
6. IllegalArguementException ：参数异常



### 异常处理机制





# 21. 泛型

## 21.1 传统方法的问题

1. 不能对加入到集合ArrayList 中的数据类型进行约束（不安全）
2. 遍历的时候，需要进行类型转换，如果集合中的数据量较大，对效率有影响



## 21.2 泛型的语法

1. 泛型的声明

   ```java
   (1) 其中，T，K，V不代表值，而是表示类型
   (2)	任意字母都可以，常用T表示，是Type的缩写
   interface <T>{}
   class <K, V>{}
   ```

2. 泛型的实例化

   ```java
   要在类名后面指定当类型参数的值
   List<String> strList = new ArrayList<String>();
   Iterator<Preson> iterator = p1.iterator();
   ```

3. 举例说明

   ```java
   //使用泛型方式给 HashSet 放入 3 个学生对象
   HashSet<Student> students = new HashSet<Student>();
   students.add(new Student("jack", 18));
   students.add(new Student("tom", 28));
   students.add(new Student("mary", 19));
   //遍历
   for (Student student : students) {
   	System.out.println(student);
   }
   
   //使用泛型方式给 HashMap 放入 3 个学生对象
   //K -> String V->Student
   HashMap<String, Student> hm = new HashMap<String, Student>();
   
   /*
   public class HashMap<K,V> {}
   */
   hm.put("milan", new Student("milan", 38));
   hm.put("smith", new Student("smith", 48));
   hm.put("hsp", new Student("hsp", 28));
   
   
   //迭代器 EntrySet
   /*
   public Set<Map.Entry<K,V>> entrySet() {
   	Set<Map.Entry<K,V>> es;
   	return (es = entrySet) == null ? (entrySet = new EntrySet()) : es;
   }
   */
   
   /*
   public final Iterator<Map.Entry<K,V>> iterator() {
   	return new EntryIterator();
   }
   */
   Set<Map.Entry<String, Student>> entries = hm.entrySet();	
   Iterator<Map.Entry<String, Student>> iterator = entries.iterator();
   while (iterator.hasNext()) {
   	Map.Entry<String, Student> next = iterator.next();
   	System.out.println(next.getKey() + "-" + next.getValue());
   }
   ```



## 21.3 泛型使用的注意事项

1. 泛型类型只能是引用类型

   ```java
   List<Integer> list = new ArrayList<Integer>(); //对
   List<int> list1 = new ArrayList<int>();	//错，int为基本数据类型 
   ```

2. 在给泛型指定具体类型后，可以传入该类型或其子类类型

3. 泛型的使用形式

   ```java
   List<Integer> list = new ArrayList<Integer>(); 
   
   //推荐下面的写法(省略后面的类型，编译器会自己判断)
   List<Integer> list = new ArrayList<>();
   
   //如果我们按下面的写法,不给泛型，其实默认的泛型是Object
   ArrayList arrayList = new ArrayList();
   ArrayList<Object> arrayList = new ArrayList<Object>();
   
   ```

   

## 21.4 自定义泛型

### 21.4.1 自定义泛型类

```java
class Preson<T,E>{
    
}
```

**注意细节：**

1. 普通成员可以使用泛型（属性、方法）
2. 使用泛型的数组，不能初始化
3. 静态方法中不能使用类的泛型
4. 泛型类的类型，是在创建对象是确定的（因为创建对象时，需要指定确定类型）
5. 如果在创建对象时，没有指定类型，默认为Object



### 21.4.2 自定义泛型接口

```java
interface Usb<T,R> {

}
```

**注意细节：**

1. 接口中，静态成员也不能使用泛型（这个和泛型类规定一样）
2. 泛型接口的类型，在继承接口或实现接口时确定
3. 没有指定类型，默认为Object



### 21.4.3 自定义泛型方法

```java
class Bird<T,R,M>{
	public<E> void fly(E t){
		System.out.println(t);
	}
}
```

**注意细节：**

1. 泛型方法中，可以定义在普通类中，也可以定义在泛型类中
2. 当泛型方法被调用时，类型会确定
3. public void eat(E e){}，修饰符后没有<T,R>，eat方法不是泛型方法，而是使用泛型



## 21.5 泛型的继承和通配符

1. 泛型不具备继承性

   ```java
   List<Object> list = new ArrayList<String> ();	//错
   ```

2. <?>：支持任意泛型类型

   ```java
   //说明: List<?> 表示 任意的泛型类型都可以接受
   public static void printCollection1(List<?> c) {
       for (Object object : c) { // 通配符，取出时，就是 Object
           System.out.println(object);
       }
   }
   ```

3. <? extends A>：支持A类以及A类的之类，规定了泛型的上限

   ```java
   // ? extends AA 表示 上限，可以接受 AA 或者 AA 子类
   public static void printCollection2(List<? extends AA> c) {
       for (Object object : c) {
           System.out.println(object);
       }
   }
   ```

4. <? super A>：支持A类以及A类的父类，不限于直接父类，规定了泛型的下限

   ```java
   // ? super 子类类名 AA:支持 AA 类以及 AA 类的父类，不限于直接父类，
   //规定了泛型的下限
   public static void printCollection3(List<? super AA> c) {
       for (Object object : c) {
       	System.out.println(object);
       }
   }
   ```

   

# 22. 集合

## 0. 集合框架总览

如图：

<img src="https://img-blog.csdnimg.cn/20200321185804869.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29tYW4wMDE=,size_16,color_FFFFFF,t_70" style="zoom:200%;" />



集合选择：

![集合选择](https://raw.githubusercontent.com/Young-Allen/pic/main/%E9%9B%86%E5%90%88%E9%80%89%E6%8B%A9.png)

## 1. ArrayList 接口常用方法

```java
List list = new ArrayList();
```

1. 增 add

   ```java
   list.add("jack");
   //在第一个位置插入tom
   list.add(1, "tom");
   
   // boolean addAll(int index, Collection eles):从 index 位置开始将 eles 中的所有元素添加进来
   List list2 = new ArrayList();
   list2.add("jack");
   list2.add("tom");
   list.addAll(1, list2);
   ```

2. 删 remove

   ```java
   // Object remove(int index):移除指定 index 位置的元素，并返回此元素
   list.remove(0);
   ```

3. 改

   ```java
   // Object set(int index, Object ele):设置指定 index 位置的元素为 ele , 相当于是替换. list.set(1, "玛丽");
   //将第0个位置的元素修改为 sorry
   list.set(0, "sorry");
   ```

4. 查

   ```java
   // Object get(int index):获取指定 index 位置的元素
   // int indexOf(Object obj):返回 obj 在集合中首次出现的位置
   int index = list.indexOf("tom");//2
   
   // int lastIndexOf(Object obj):返回 obj 在当前集合中末次出现的位置
   list.add("韩顺平");
   int lsindex = list.lastIndexOf("韩顺平");
   
   // List subList(int fromIndex, int toIndex):返回从 fromIndex 到 toIndex 位置的子集合
   // 注意返回的子集合 fromIndex <= subList < toI
   List returnlist = list.subList(0, 2);
   
   //查找List中的下标为 n 的元素
   Object obj = list.get(5);
   ```




## 2. ArrayList 细节

### 1. 注意事项

1. ArrayList 可以加入null，并且多个
2. ArrayList 是由数组来实现数据存储的
3. ArrayList 基本等同于Vector，除了ArrayList是线程不安全（执行效率高），在多线程情况下，不建议使用ArrayList



### 2. 底层操作机制

1. ArrayList 中维护了一个Object类型的数组 elementData
   transient 关键字表示该属性不会被序列化

   ```java
    // non-private to simplify nested class access
   transient Object[] elementData;
   ```

2. 当创建ArrayList对象时，如果使用的是无参构造器，则初始化elementData容量为0。第一次添加，则扩容为10，如需再次扩容，则扩容为1.5倍。

3. 如果使用的是指定大小的构造器，则初始elementData容量为指定大小，如果需要扩容，则直接扩容为上一次的1.5倍。



## 3. Vector 底层结构和源码剖析

### 1. 源码剖析

1. 无参构造器默认开一个大小为 10 的数组，满了就按 2 倍扩容

   ```java
   //(1)
   public Vector() {
       this(10);
   }
   
   //(2)
   public Vector(int initialCapacity) {
       this(initialCapacity, 0);
   }
   
   //(3)add方法
   public synchronized boolean add(E e) {
       modCount++;
       ensureCapacityHelper(elementCount + 1);
       elementData[elementCount++] = e;
       return true;
   }
   
   //(4)扩容方法
    private void grow(int minCapacity) {
           // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + ((capacityIncrement > 0) ?
                                         capacityIncrement : oldCapacity);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
   ```

2. 如果指定大小，则每次满了后直接按 2 倍扩容



### 2. 细节

1. Vector类的定义说明

   ```java
   public	class Vector<E>
   extends AbstractList<E>
   implements List<E>, RandonmAccess, Cloneable, Serializable
   ```

2. Vector 底层也是一个对象数组，protected Object[] elementData

3. Vector 是线程同步的，即线程安全，Vector 类的操作方法都带有synchronized 

4. 在开发中，需要线程同步安全是，考虑使用Vector

### 3. ArrayList 和 Vector 的比较

![image-20220719185253284](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220719185253284.png)



## 4. LinkedList 细节说明

### 1. 说明

1. 线程不安全，没有实现同步
2. LinkedList 底层维护了一个双向链表
3. LinkedList 中维护了两个属性 first 和 last 分别指向首节点和尾结点
4. 每个节点（Node对象），里面有维护了prev、next、item三个属性，其中通过prev指向前一个，next指向后一个节点。最终实现双向链表。

### 2. add 的底层细节

```java
//节点Node维护的数据
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

public boolean add(E e) {
    linkLast(e);
    return true;
}

//使用尾插法向链表中插入数据
 void linkLast(E e) {
     final Node<E> l = last;
     final Node<E> newNode = new Node<>(l, e, null);
     last = newNode;
     if (l == null)
         first = newNode;
     else
         l.next = newNode;
     size++;
     modCount++;
 }
```

### 3. remove 的底层细节

和链表的删除方法差不多

```java
public boolean remove(Object o) {
    if (o == null) {
        for (Node<E> x = first; x != null; x = x.next) {
            if (x.item == null) {
                unlink(x);
                return true;
            }
        }
    } else {
        for (Node<E> x = first; x != null; x = x.next) {
            if (o.equals(x.item)) {
                unlink(x);
                return true;
            }
        }
    }
    return false;
}

E unlink(Node<E> x) {
    // assert x != null;
    final E element = x.item;
    final Node<E> next = x.next;
    final Node<E> prev = x.prev;

    if (prev == null) {
        first = next;
    } else {
        prev.next = next;
        x.prev = null;
    }

    if (next == null) {
        last = prev;
    } else {
        next.prev = prev;
        x.next = null;
    }

    x.item = null;
    size--;
    modCount++;
    return element;
}
```



### 4. ArrayList 和 LinkedList 的比较

![image-20220719220401233](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220719220401233.png)

**如何选择ArrayList和LinkedList**：

1. 如果我们改查的操作多，选择ArrayList
2. 如果我们的增删的操作多，选择LinkedList
3. 一般来说，在程序中，80%-90%都是查询，因此大部分情况下会选择ArrayList
4. 在一个项目中，根据业务灵活选择，也可能这样，一个模块使用的是ArrayList，另一个模块使用的是LinkedList



## 5. List 总结

1. ArrayList：底层数据结构是数组，查询快，增删慢，线程不安全，效率高，可以存储重复元素
2. LinkedList 底层数据结构是链表，查询慢，增删快，线程不安全，效率高，可以存储重复元素
3. Vector:底层数据结构是数组，查询快，增删慢，线程安全，效率低，可以存储重复元素

![](https://img-blog.csdn.net/20180803201736883?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZlaXlhbmFmZmVjdGlvbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 6. Set接口常用方法

### 1. Set接口基本介绍

1. 无序（添加和取出的顺序不一致），没有索引
2. 不允许重复元素，所以最多包含一个null
3. 不能使用索引的方式来进行遍历（因为没有索引）



## 7. HashSet 使用说明

因为HashSet的底层比较复杂，如果忘记了可以看看这个的视频复习一下：[HashSet底层源码机制讲解视频](https://www.bilibili.com/video/BV1fh411y7R8?p=522&vd_source=ebaa9b5a24bde7756de385ec80faa6a9)



1. HashSet实现了 Set接口
2. HashSet实际上是HashMap，看下源码.

  ```java
public HashSet(){
	map = new HashMap<>();
}
  ```

3. 可以存放null值，但是只能有一个null
4. HashSet不保证元素是有序的,取决于hash后，再确定索引的结果. 即不保证存放元素的顺序和取出顺序一致
5. 不能有重复元素/对象. 在前面Set接口使用已经讲过



### 1. 添加元素

1. HashSet 底层是HashMap
2. 添加一个元素时，先使用hashCode() 方法得到hash值，会转成 索引值
3. 找到存储数据表table，看这个索引位置是否已经存放的有元素，如果没有，直接加入
4. 如果有，调用equals方法（可以重载）比较，如果相同，就放弃添加，如果不相同，则添加到最后
5. 在Java8中，如果一条链表的元素个数到达 TREEIFY_THRESHOLD(默认是8)，并且table 的大小 >= MIN_TREEIFY_CAPACITY(默认64)，就会进行树化（红黑树）

### 2. 扩容机制

1. HashSet底层是HashMap，第一次添加时，table数组扩容到16，临界值（threshold) 是 16 * 加载因子（loadFactor) 0.75 = 12
2. 如果table数组使用到了临界值12，就会扩容到16 * 2  = 32，新的临界值就是 32 * 0.75 = 24，以此类推（注意：是当成功加入一个元素时size就会加一，当size到了临界值12时才会扩容，而不是单纯使用了12个table数组的位置）
3. 在Java8中，如一条链表的元素个数到达TREEIFY_THRESHOLD（默认是8），并且table 的大小 >= MIN_TREEIFY_CAPACITY(默认64)，就会进行树化（红黑树），**否则任然采用数组扩容机制**



## 8. LinkedHashSet

### 1. 使用说明

1. LinkedHashSet 是 HashSet 的子类
2. LinkedHashSet 底层是一个 LinkedHashMap，底层维护了一个数组 + 双链表
3. LinkedHashSet 根据元素的hashCode 值来决定元素的存储位置，同时使用链表维护元素的次序，这使得元素看起来是以插入顺序保存的
4. LinkedHashSet 也是不允许重复添加元素的

![image-20220720174048076](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220720174048076.png)



## 9. Map接口实现类的特点

### 1. Map接口特点

**注意：**这里将的是JDK8的Map接口特点

1. Map 用于保存具有映射关系的数据：Key-Value（类似于c++的map）
2. Map中的key 和 value 可以是任何引用类型的数据，会封装到HashMap$Node 对象中
3. Map中的key不允许重复，原因和HashSet一样。value可以重复
4. Map的key可以为null，value也可以为null，注意key为null，只能有一个，value为null，可以有多个
5. 常用String类作为Map的key
6. key 和 value 之间存在单线一对一关系，即通过指定的key总能找到对应的value



```java
//存储数据,使用put来存储数据，而不是add
Map map = new HashMap();
map.put("tom", "good boy");

//通过get，可以获得对应key的value值
map.get("tom");
```



### 2. Map接口常用方法

Map常用方法一览：

![](https://img-blog.csdn.net/20180803205119738?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ZlaXlhbmFmZmVjdGlvbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)



```java
//put：添加
Map map = new HashMap();
map.put("邓超", new Book("", 100));//OK
map.put("邓超", "孙俪");//替换-> 一会分析源码
map.put("王宝强", "马蓉");//OK
map.put("宋喆", "马蓉");//OK
map.put("刘令博", null);//OK
map.put(null, "刘亦菲");//OK
map.put("鹿晗", "关晓彤");//OK


//remove：根据键删除映射关系
map.remove(null);
map.remove("邓超");

//get：根据键获取值
Object val = map.get("鹿晗");

//size：获取元素个数
System.out.println("k-v=" + map.size());

// isEmpty:判断个数是否为 0
System.out.println(map.isEmpty());//F

// clear:清除 k-v
map.clear();
System.out.println("map=" + map);

// containsKey:查找键是否存在
System.out.println("结果=" + map.containsKey("hsp"));//T
```



### 3. Map接口遍历方法

```java
Map map = new HashMap();
map.put("邓超", "孙俪");
map.put("王宝强", "马蓉");
map.put("宋喆", "马蓉");
map.put("刘令博", null);
map.put(null, "刘亦菲");
map.put("鹿晗", "关晓彤");

//第一组: 先取出 所有的 Key , 通过 Key 取出对应的 Value
Set keyset = map.keySet();
//(1) 增强 for
System.out.println("-----第一种方式-------");
for (Object key : keyset) {
	System.out.println(key + "-" + map.get(key));
}

//(2) 迭代器
System.out.println("----第二种方式--------");
Iterator iterator = keyset.iterator();
while (iterator.hasNext()) {
    Object key = iterator.next();
    System.out.println(key + "-" + map.get(key));
}


//第二组: 把所有的 values 取出
Collection values = map.values();
//这里可以使用所有的 Collections 使用的遍历方法
//(1) 增强 for
System.out.println("---取出所有的 value 增强 for----");
for (Object value : values) {
	System.out.println(value);
}
//(2) 迭代器
System.out.println("---取出所有的 value 迭代器----");
Iterator iterator2 = values.iterator();
while (iterator2.hasNext()) {
	Object value = iterator2.next();
    System.out.println(value);
}


//第三组: 通过 EntrySet 来获取 k-v
Set entrySet = map.entrySet();// EntrySet<Map.Entry<K,V>>
//(1) 增强 for
for (Object entry : entrySet) {
//将 entry 转成 Map.Entry
Map.Entry m = (Map.Entry) entry;
	System.out.println(m.getKey() + "-" + m.getValue());
}
//(2) 迭代器
Iterator iterator3 = entrySet.iterator();
while (iterator3.hasNext()) {
	Object entry = iterator3.next();
    //System.out.println(next.getClass());//HashMap$Node -实现-> Map.Entry (getKey,getValue)
    //向下转型 Map.Entry
	Map.Entry m = (Map.Entry) entry;
	System.out.println(m.getKey() + "-" + m.getValue());
}
```



## 10. HashMap 类

### 1. HashMap小结

1. Map接口的常用实现类: HashMap、 Hashtable和Properties。
2. HashMap是Map接口使用频率最高的实现类。
3. HashMap是以key-val对的方式来存储数据(HashMap$Node类型) [案例Entry]
4. key不能重复，但是值可以重复，允许使用null键和null值。
5. 如果添加相同的key，则会覆盖原来的key-val ,等同于修改.(key不会替换，val会替换)
6. 与HashSet一样，不保证映射的顺序，因为底层是以hash表的方式来存储的. (jdk8的HashMap底层数组+链表+红黑树)
7. HashMap没有实现同步，因此是线程不安全的，方法没有做同步互斥的操作，没有synchronized



### 2. 底层机制

可以看看这几个视频，讲解十分详细：[HashMap底层机制](https://www.bilibili.com/video/BV1fh411y7R8?p=537&vd_source=ebaa9b5a24bde7756de385ec80faa6a9)



## 11. Hashtable 类

### 1. 基本介绍

1. 存放的元素是键值对: 即K-V
2. Hashtable 的键和值都不能为 null, 否则会抛出 NullPointerException
3. Hashtable 使用方法基本上和 HashMap 一样
4. Hashtable 是线程安全的(synchronized), HashMap 是线程不安全的

详细可以看看这个博客：[Hashtable原理和底层实现](https://cloud.tencent.com/developer/article/1520582)



### 2. 扩容机制

简单说明一下Hashtable的底层

1. 底层有数组Hashtable$Entry[] 初始化大小为11
2. 临界值 threshold 8 = 11*0.75
3. 扩容：按照自己的扩容机制来进行即可。
4. 执行方法 addEntry (hash，key，value，index); 添加K-V封装到Entry。当if (count >= threshold) 满足时，就进行扩容
5. 按照 int newCapacity = (oldCapacity << 1) + 1;的大小扩容。（就是原来的大小 * 2 + 1）

### 3. Hashtable 和 HashMap 对比

![image-20220721121339452](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220721121339452.png)



## 12. Properties 类

### 1. 基本介绍

1. Properties类继承自Hashtable类并且实现了 Map接口，也是使用一种键值对的形式来保存数据。
2. 他的使用特点和Hashtable类似
3. Properties 还可以用于从xxx.properties文件中，加载数据到Properties类对象，并进行读取和修改
4. 说明:工作后xxx.properties 文件通常作为配置文件，这个知识点在I0流举例，有兴趣可先看文章

[[Java 读写Properties配置文件]](https://www.cnblogs.com/xudong-bupt/p/3758136.html)



### 2. 基本使用

```java
//老韩解读
//1. Properties 继承 Hashtable
//2. 可以通过 k-v 存放数据，当然 key 和 value 不能为 null

//增加
Properties properties = new Properties();
//properties.put(null, "abc");//抛出 空指针异常
//properties.put("abc", null); //抛出 空指针异常
properties.put("john", 100);//k-v
properties.put("lucy", 100);
properties.put("lic", 100);
properties.put("lic", 88);//如果有相同的 key ， value 被替换


//通过 k 获取对应值
System.out.println(properties.get("lic"));//88

//删除
properties.remove("lic");
System.out.println("properties=" + properties);

//修改
properties.put("john", "约翰");
System.out.println("properties=" + properties
```



## 13. Collections 工具类

```java
//创建 ArrayList 集合，用于测试. 
List list = new ArrayList();
list.add("tom");
list.add("smith");
list.add("king");
list.add("milan");
list.add("tom");

```



1. reverse（List）:反转 List 中元素的顺序

   ```java
   Collections.reverse(list);
   ```

2. shuffle(List): 对List 集合元素进行随机排序

   ```java
   Collections.shuffle(list);
   ```

3. sort(List): 根据元素的自然顺序对指定List集合元素按升序排序

   ```java
   Collections.sort(list);
   ```

   sort(List, Comparator): 根据指定的Comparator产生的顺序对List 集合元素进行排序

   ```java
   Collections.sort(list, new Comparator() {
       @Override
       public int compare(Object o1, Object o2) {
       	//可以加入校验代码. 
           return ((String) o2).length() - ((String) o1).length();
       }
   });
   ```

4. swap(List, int, int): 将指定list集合中的i处元素和j处元素进行交换

   ```java
   Collections.swap(list, 0, 1);
   ```

5. Object max(Collection):根据元素的自然顺序，返回给定集合中的最大元素

   ```java
   Collections.max(list);
   ```

   Object max(Collection, Comparator): 根据Comparator指定的顺序，返回给定集合中的最大元素

   ```java
   Object maxObject = Collections.max(list, new Comparator() {
       @Override
       public int compare(Object o1, Object o2) {
       	return ((String)o1).length() - ((String)o2).length();
       });
   ```

6. Object min(Collection) 和 Object min(Collection, Comparator) 原理和max是差不多的

7. int frequency(Collection, Object): 返回指定集合中指定元素的出现次数

   ```java
   int cnt = Collections.frequency(list, "tom");
   ```

8. void copy(List dest, List src)：将 src 中的内容复制到 dest 中

   ```java
   ArrayList dest = new ArrayList();
   //为了完成一个完整拷贝，我们需要先给 dest 赋值，大小和 list.size()一样
   for(int i = 0; i < list.size(); i++) {
   	dest.add("");
   }
   //拷贝
   Collections.copy(dest, list);
   ```

9. boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所有旧值

   ```java
   //将所有的 tom 换成 汤姆
   Collections.replaceAll(list, "tom", "汤姆");
   ```



# 23. 多线程基础

## 1. 程序相关概念

1. 程序：是为完成特定任务、用某种语言编写的一-组指令的集合。简单的说:就是我们写的代码
2. 进程：进程是指运行中的程序，比如我们使用QQ，就启动了一个进程，操作系统就会为该进程分配内存空间。当我们使用迅雷，又启动了-一个进程，操作系统将为迅雷分配新的内存空间。
   进程是程序的一次执行过程，或是正在运行的一个程序。是动态过程:有它自身的产生、存在和消亡的过程
3. 线程：是有进程创建的，是进程的一个实体



## 2. 线程的基本使用

线程继承关系图：

![image-20220723090108688](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220723090108688.png)

1. **继承Thread类**

   ```java
   public class Thread01 {
   public static void main(String[] args) throws InterruptedException {
   	//创建 Cat 对象，可以当做线程使用
   	Cat cat = new Cat();
   
       /*
       (1)
           public synchronized void start() {
           	start0();
           }
       (2)//start0() 是本地方法，是 JVM 调用, 底层是 c/c++实现
       	//真正实现多线程的效果， 是 start0(), 而不是 run
       	private native void start0();
       */
       
   	cat.start();//启动线程-> 最终会执行 cat 的 run 方法
   	//cat.run();//run 方法就是一个普通的方法, 没有真正的启动一个线程，就会把 run 方法执行完毕，才向下执行
   	//说明: 当 main 线程启动一个子线程 Thread-0, 主线程不会阻塞, 会继续执行
   	//这时 主线程和子线程是交替执行.. System.out.println("主线程继续执行" + Thread.currentThread().getName());//名字 main
   	for(int i = 0; i < 60; i++) {
   		System.out.println("主线程 i=" + i);
   		//让主线程休眠
   		Thread.sleep(1000);
   	}
   }
   }
   
   //老韩说明
   //1. 当一个类继承了 Thread 类， 该类就可以当做线程使用
   //2. 我们会重写 run 方法，写上自己的业务代码
   //3. run Thread 类 实现了 Runnable 接口的 run 方法
   /*
   @Override
   	public void run() {
   		if (target != null) {
   			target.run();
   		}
   	}
   */
   
   class Cat extends Thread {
   	int times = 0;
       
       @Override
       public void run() {//重写 run 方法，写上自己的业务逻辑
       	while (true) {
           	//该线程每隔 1 秒。在控制台输出 “喵喵, 我是小猫咪”
           	System.out.println("喵喵, 我是小猫咪" + (++times) + " 线程名=" + Thread.currentThread().getName());
   
               //让该线程休眠 1 秒 ctrl+alt+t
               try {
                   Thread.sleep(1000);
               } catch (InterruptedException e) {
                   e.printStackTrace();
               }
               if(times == 80) {
                   break;//当 times 到 80, 退出 while, 这时线程也就退出.. }
               }
   		}
   	}
   }
   ```




## 3. 线程终止

1. 基本说明

   （1）当线程完成任务后，会自动退出

   （2）还可以通过**使用变量**来控制run 方法退出的方式停止线程，即 **通知方式**

2. 案例

   ```java
   class AThread implements Runnable{
       boolean loop = true;	//步骤1：定义标记变量，默认为true
       
       @Override
       public void run(){
           while(true){	//步骤2：将loop作为循环条件
               try{
                   Thread.sleep(50);
               }catch(InterruptedException e){
                   e.printStackTrace();
               }
               System.out.println("AThread 运行中...")
           }
       }
       
       //步骤3：提供公共的set方法，用于更新loop
       public void setLoop(boolean loop){
           this.loop = loop;
       }
   }
   
   
   main{
       AThread a = new AThread();
       new Thread(st).start();
       
       for(int i = 0; i <= 60; i++){
           Thread.sleep(50);
       	if(i == 30){
               a.setLoop(false);
           }
       }
       
   }
   ```

   

## 4. 线程常用方法

1. setName //设置线程名称，使之与参数name相同
2. getName //返回该线程的名称
3. start //使该线程开始执行; Java 虚拟机底层调用该线程的start方法
4. run //调用线程对象run方法;
5. setPriority //更改线程的优先级
6. getPriority //获取线程的优先级
7. sleep//在指定的毫秒数内让当前正在执行的线程休眠(暂停执行)
8. interrupt //中断线程



**注意事项和细节：**

1. start底层会创建新的线程，调用run, run就是一一个简单的方法调用，不会启动新
    线程
2. 线程优先级的范围
3. interrupt,中断线程，但并没有真正的结束线程。所以一般用于中断正在休眠线程
4. sleep:线程的静态方法，使当前线程休眠



## 5. 线程插队

1. yield:线程的礼让。让出cpu,让其他线程执行，但礼让的时间不确定，所以也不一定礼让成功
2. join:线程的插队。插队的线程一旦插队成功， 则肯定先执行完插入的线程所有的任务

  ```java
  案例: main线程创建一个子线程,每隔1s输出hello,输出20次，主线程每隔1秒，输出hi,输出20次.要求:两个线程同时执行，当主线程输出5次后，就让子线程运行完毕，主线程再继续，
        
  public class ThreadMethodExc {
      public static void main(String[] args) throws InterruptedException {
          for (int i = 1; i <= 10; i++) {
              System.out.println("hi " + i);
              Thread.sleep(1000);
  
              if (i == 5) {
                  T4 t4 = new T4();
                  Thread thread = new Thread(t4);
                  thread.start();
                  //说明
  				// 1.让jd插队到主线程前面，这样main就会等待jd执行完毕，再执行
  				// 2.如果没有join那么，JoinThread 和Main线程就会交替执行
                  thread.join();
              }
          }
          System.out.println("主进程结束...");
      }
  }
  ```

  

## 6. 守护线程

1. 用户线程：也叫工作线程，当线程的任务执行完或通知方式结束
2. 守护线程（ dt.setDaemon(true) ）：一般是为工作线程服务的，当所有的用户线程结束，守护线程自动结束
3. 常见的守护线程:垃圾回收机制

```java
MyDaemonThread dt = new MyDaemonThread();
//将dt设置为守护线程，当所有线程结束后，dt也就自动结束
//如果没有设置，那么即使main线程执行完毕，dt也不退出，可以体验一 下
dt.setDaemon(true);
dt.start(); 

for (inti= 1;i <= 100;i++) {
	Thread.sleep(50);
	System.out.println("-------- +" );
}
```



## 7. 线程的声明周期

1. JDK中使用Thread.State 枚举表示了线程的几种状态
   ![image-20220723210432312](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220723210432312.png)

2. 线程状态转换图：
   ![image-20220723210512548](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220723210512548.png)

3. 查看线程状态方法

   ```java
   getState();
   ```



## 8. 线程的同步

### 线程同步机制

1. 在多线程编程，一些敏感数据不允许被多个线程同时访问，此时就使用同步访问技术，保证数据在任何同一时刻，最多有一个线程访问，以保证数据的完整性。
2. 也可以这里理解：线程同步，即当有一个线程在对内存进行操作时，其他线程都不可以对这个内存地址进行操作，直到该线程完成操作，其他线程才能对该内存地址进行操作.



### 具体同步方法——Synchronized

1. 同步代码块

   ```java
   synchronized(对象){	//得到对象的锁，才能操作同步代码
   	//需要被同步的代码
   }
   ```

2. synchronized还可以放在方法声明中，表示整个方法为同步方法

   ```java
   public synchronized void m (String name){
      //需要被同步的代码
   }
   ```

   

## 9. 互斥锁

### 基本介绍

1. Java语言中，引入了对象互斥锁的概念，来保证共享数据操作的完整性。
2. 每个对象都对应于一个可称为“互斥锁"的标记，这个标记用来保证在任一时刻，只能有一个线程访问该对象。
3. 关键字synchronized来与对象的互斥锁联系。当某个对象用synchronized修饰时，表明该对象在任一时刻只能由一个线程访问
4. 同步的局限性：导致程序的执行效率要降低
5. 同步方法(非静态的)的锁可以是this,也可以是其他对象(要求是同一个对象)
6. 同步方法(静态的)的锁为当前类本身。



### 注意事项和细节

1. 同步方法如果没有使用static修饰：默认锁对象为this
2. 如果方法使用static修饰，默认锁对象：当前类.class
3. 实现的落地步骤:
   需要先分析上锁的代码
   选择同步代码块或同步方法
   要求多个线程的锁对象为同一个即可



## 10. 死锁

### 基本介绍

多个线程都占用了对方的锁资源，但不肯想让，导致了死锁，在编程是一定要避免死锁的发送



## 11. 释放锁

### 下面操作会释放锁

1. 当前线程的同步方法、同步代码块执行结束
   案例: - 上厕所，完事出来
2. 当前线程在同步代码块、同步方法中遇到break、return。
   案例:没有正常的完事，经理叫他修改bug,不得已出来
3. 当前线程在同步代码块、同步方法中出现了未处理的Error或Exception, 导致异常结束
   案例:没有正常的完事，发现忘带纸，不得已出来
4. 当前线程在同步代码块、同步方法中执行了线程对象的wait(方法， 当前线程暂停，并释
   放锁。
   案例:没有正常完事，觉得需要酝酿下，所以出来等会再进去



### 下面操作不会释放锁

1. 线程执行同步代码块或同步方法时，程序调用Thread.sleep()、 Thread.yield(）方法暂停当前线程的执行，不会释放锁
案例:上厕所，太困了，在坑位上眯了一会
2. 线程执行同步代码块时，其他线程调用了该线程的suspend()方法将该线程挂起，该线程不会释放锁。
提示:应尽量避免使用suspend()和resume()来控制线程，方法不再推荐使用



# 24. IO流

## 1. 文件操作

1. 创建文件对象相关构造器和方法

   ```java
   //方式一
   @Test
   public void create01() throws IOException {
       String filePath = "e:\\news1.txt";
       File file = new File(filePath);
       file.createNewFile();
   }
   
   //方式 2 new File(File parent,String child) //根据父目录文件+子路径构建
   //e:\\news2.txt
   @Test
   public void create02() {
       File parentFile = new File("e:\\");
       String fileName = "news2.txt";
       //这里的 file 对象，在 java 程序中，只是一个对象
       //只有执行了 createNewFile 方法，才会真正的，在磁盘创建该文件
       File file = new File(parentFile, fileName);
       try {
           file.createNewFile();
           System.out.println("创建成功~");
       } catch (IOException e) {
           e.printStackTrace();
       }
   }
   
   //方式3 new File(string parent, String child) 根据父目录+子路径构建
   @Test
   public void create03() throws IOException {
       String parentPath = "f:\\news";
       String fileName = "news3.txt";
       File file = new File(parentPath, fileName);
   
       file.createNewFile();
   }
   ```

2. 获取文件的相关信息

   ```java
   //调用相应的方法，得到对应信息
   System.out.println("文件名字=" + file.getName());
   //getName、getAbsolutePath、getParent、length、exists、isFile、isDirectory
   System.out.println("文件绝对路径=" + file.getAbsolutePath());
   System.out.println("文件父级目录=" + file.getParent());
   System.out.println("文件大小(字节)=" + file.length());
   System.out.println("文件是否存在=" + file.exists());//T
   System.out.println("是不是一个文件=" + file.isFile());//T
   System.out.println("是不是一个目录=" + file.isDirectory());//F
   ```

   

## 2. IO流原理及流的分类

### Java IO流的原理

1. I/O是Input/Output的缩写，I/0技术是非常实用的技术， 用于处理数据传输。如读/写文件，网络通讯等。
2. Java程序中，对于数据的输入/输出操作以”流(stream)" 的方式进行。
3. java.io包下提供了各种“流"类和接口，用以获取不同种类的数据，并通过方法输入或输出数据
4. 输入input: 读取外部数据（磁盘、光盘等存储设备的数据）到程序（内存）中。
5. 输出output: 将程序（内存）数据输出到磁盘、光盘等存储设备中



### 流的分类

![image-20220728115126973](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220728115126973.png)

## 3. IO流体系图——常用的类

### IO流体系图

IO流体系图：

![](https://img-blog.csdnimg.cn/20200307125658523.png)



### FileInputStream 和 FileOutputStream

1. FileInputStream

   ```java
   //方式一
   public void readFile01() throws IOException {
       String filePath = "F:\\news\\news3.txt";
       //创建了FileInputStream对象，用于读取文件
       FileInputStream fileInputStream = new FileInputStream(filePath);
   
       int readData = 0;
       while ((readData = fileInputStream.read()) != -1) {
           System.out.print((char) readData);
       }
   
       fileInputStream.close();
   }
   
   //方式二
   //使用字节数组读取
   public void readFile02() throws IOException {
       String filePath = "F:\\news\\news3.txt";
       //创建了FileInputStream对象，用于读取文件
       FileInputStream fileInputStream = new FileInputStream(filePath);
   
       int len = 0;
       byte[] buf = new byte[8];
       while ((len = fileInputStream.read(buf)) != -1) {
           System.out.print(new String(buf, 0, len));
       }
   
       fileInputStream.close();
   }
   ```

2. FileOutputStream

   ```java
   public void writeFile() throws IOException {
       String filePath = "F:\\news\\news1.txt";
       //注意：这样创建的写入数据是会覆盖原来的内容
       FileOutputStream fileOutputStream = new FileOutputStream(filePath);
   
       //这样在后面加一个true，当写入内容时，会追加到文件末尾
       FileOutputStream fileOutputStream1 = new FileOutputStream(filePath, true);
   
   
       //写一个字节
       fileOutputStream.write('a');
   
       fileOutputStream1.write('A');
   
       //写一个字符串
       String str = " hello,world ";
       //str.getBytes()     可以把字符串转换为字节数组
       fileOutputStream.write(str.getBytes(StandardCharsets.UTF_8));
   
       //写入指定位置的字符串
       fileOutputStream.write(str.getBytes(StandardCharsets.UTF_8), 1, 5);
   
       fileOutputStream.close();
   }
   ```



### FileReader 和 FileWriter

1. FileReader常用方法
   ![image-20220728135041220](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220728135041220.png)

   ```java
   //方法一：读取字符
   public void test() throws IOException {
       String filePath = "C:\\Users\\19241\\Desktop\\Java\\集合.md";
       FileReader fileReader = new FileReader(filePath);
   
       int data;
       try {
           while ((data = fileReader.read()) != -1) {
               System.out.print((char) data);
           }
       } catch (IOException e) {
           e.printStackTrace();
       } finally {
           fileReader.close();
       }
   }
   
   
   //方法二：读取字符数组
   public void test01() throws IOException {
       String filePath = "C:\\Users\\19241\\Desktop\\Java\\集合.md";
       FileReader fileReader = new FileReader(filePath);
   
       int len;
       char[] chars = new char[1024];
       try {
           while ((len = fileReader.read(chars)) != -1) {
               System.out.println(new String(chars, 0, len));
           }
       } catch (IOException e) {
           e.printStackTrace();
       } finally {
           fileReader.close();
       }
   }
   ```

2. FileWriter常用方法
   ![image-20220728135010963](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220728135010963.png)



## 4. 节点流和处理流

### 基本介绍

![image-20220728173449286](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220728173449286.png)



节点流和处理流图：

![image-20220728173522168](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220728173522168.png)



节点流和处理流的区别和联系：

1. 节点流是底层流/低级流，直接跟数据源相接。
2. 处理流(包装流)包装节点流，既可以消除不同节点流的实现差异，也可以提供更方便的方法来完成输入输出
3. 处理流(也叫包装流)对节点流进行包装，使用了修饰器设计模式，不会直接与数据源相连[模拟修饰器设计模式=》小伙伴就会非常清楚.]



处理流的功能主要体现在一下两个方面：

1. 性能的提高：主要一增加缓冲的方式来提高输入输出的效率
2. 操作的便捷：处理流可能提供了一系列便捷的方法来一次输入输出大批量的数据，使用更加灵活





### 处理流 BufferedReader 和 BufferedWriter

BufferedReader 和 BufferedWriter 属于字符流，是按照字符来读取数据的。

关闭是处理流，只需关闭外层流即可



***注意*：**BufferedWriter 和 BufferedReader 是按照字符操作，不要去操作二进制文件（声音、视频、图片、doc、pdf），可能造成文件损坏



1. BufferedReader

   ```java
    public void test01() throws IOException {
           String filePath = "C:\\Users\\19241\\Desktop\\Java\\集合.md";
   
           //创建BufferedReader
           BufferedReader bufferedReader = new BufferedReader(new FileReader(filePath));
           //读取
           String line; //按行读取, 效率高
           //bufferedReader.readLine() 是按行读取文件
           while((line = bufferedReader.readLine()) != null){
               System.out.println(line);
           }
           bufferedReader.close();
       }
   ```

2. BufferedWriter

   ```java
    public void test01() throws IOException {
           String filePath = "e:\\ok.txt";
           //创建 BufferedWriter
           //1. new FileWriter(filePath, true) 表示以追加的方式写入
           //2. new FileWriter(filePath) , 表示以覆盖的方式写入
   //        BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(filePath));
           BufferedWriter bufferedWriter = new BufferedWriter(new FileWriter(filePath, true));
           bufferedWriter.write("hello, 韩顺平教育!");
           bufferedWriter.newLine();//插入一个和系统相关的换行
           bufferedWriter.write("hello2, 韩顺平教育!");
           bufferedWriter.newLine();
           //说明：关闭外层流即可 ， 传入的 new FileWriter(filePath) ,会在底层关闭
           bufferedWriter.close();
       }
   ```

   

### 处理流 BufferedInputStream 和 BufferedOutputStream

这个是处理字节流文件的，可以使用这两个来操作二进制文件（同时也可以操作文本文件）



使用这两个方法来复制一张图片：

```java
 public void test01() throws IOException {
        String srcPath = "C:\\Users\\19241\\Pictures\\0.jpg";
        String destPath = "F:\\0.jpg";

        BufferedInputStream bufferedInputStream = new BufferedInputStream(new FileInputStream(srcPath));
        BufferedOutputStream bufferedOutputStream = new BufferedOutputStream(new FileOutputStream(destPath));

        byte[] bytes = new byte[1024];
        int len = 0;

        while ((len = bufferedInputStream.read(bytes)) != -1) {
            bufferedOutputStream.write(bytes, 0, len);
        }

        bufferedInputStream.close();
        bufferedOutputStream.close();
    }
```



## 5. 对象流 ObjectInputStream 和 ObjectOutputStream

 看一个需求：

1. 将int num = 100，这个int数据保存到文件中，注意不是100数字，而是int 100，并且能够从文件中直接恢复 int 100
2. 将Dog dog = new Dog("小黄"，10) 这个Dog对象保存到文件中，并且能够从文件中恢复
3. 上面的要求，就是能够将 基本数据类型 或者 对象 进行序列化 和 反序列化 操作



**序列化和反序列化**：

序列化：就是在保存数据时，保存 保存数据的值 和 数据类型

反序列化：就是在恢复数据时，恢复数据的值和数据类型

**注意：**需要让某个对象支持序列化机制，则必须让其类是可序列化的，所以必须实现下面的接口：

Serializable



![image-20220728194443515](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220728194443515.png)

### ObjectOutputStream

提供序列化功能

```java
public void test() throws IOException {
        //序列化后，保存的文件格式，不是存文本，而是按照他的格式来保存
        String filePath = "f:\\data.dat";
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filePath));
        //序列化数据到 e:\data.dat
        oos.writeInt(100);// int -> Integer (实现了 Serializable)
        oos.writeBoolean(true);// boolean -> Boolean (实现了 Serializable)
        oos.writeChar('a');// char -> Character (实现了 Serializable)
        oos.writeDouble(9.5);// double -> Double (实现了 Serializable)
        oos.writeUTF("韩顺平教育");//String
        //保存一个 dog 对象
        oos.writeObject(new Dog("旺财", 10));
        oos.close();
        System.out.println("数据保存完毕(序列化形式)");
    }
```



### ObjectInputStream

提供反序列化功能

```java
public void objectInputStream() throws IOException, ClassNotFoundException {
        // 1.创建流对象
        String filePath = "f:\\data.dat";
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filePath));
        // 2.读取， 注意顺序
        System.out.println(ois.readInt());
        System.out.println(ois.readBoolean());
        System.out.println(ois.readChar());
        System.out.println(ois.readDouble());
        System.out.println(ois.readUTF());
//        System.out.println(ois.readObject());
        Object o = ois.readObject();
        Dog dog = (Dog) o;
        System.out.println(dog.getName());
        System.out.println(dog.toString());

        // 3.关闭
        ois.close();
    }
```



 ## 6. 标准输入输出流

![image-20220728224157778](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220728224157778.png)



## 7. 转换流 InputStreamReader 和 OutputStreamWriter

![image-20220728224528424](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220728224528424.png)



1. 使用InputStreamReader
   将字节流 FileInputStream 转成字符流 InputStreamReader, 指定编码 gbk/utf-8

   ```java
    //将字节流 FileInputStream 转成字符流 InputStreamReader, 指定编码 gbk/utf-8
   public void test02() throws IOException {
       String filePath = "e:\\ok.txt";
       //2. 指定编码 gbk
       InputStreamReader isr = new InputStreamReader(new FileInputStream(filePath), "utf8");
       //3. 把 InputStreamReader 传入 BufferedReader
       BufferedReader br = new BufferedReader(isr);
       //4. 读取
       String s = br.readLine();
       System.out.println("读取内容=" + s);
       //5. 关闭外层流
       br.close();
   }
   ```

2. 使用 OutputStreamWriter

   ```java
   public void test03() throws IOException {
           String filePath = "e:\\ok.txt";
           OutputStreamWriter outputStreamWriter = new OutputStreamWriter(new FileOutputStream(filePath, true), "gbk");
   
           outputStreamWriter.write("this is a test\n");
   
           outputStreamWriter.close();
       }
   ```



## 8. 打印流 PrintStream 和 PrintWriter

![image-20220728225857640](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220728225857640.png)

## 9. Properties类

### 基本介绍

![image-20220728230006836](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220728230006836.png)



### 使用案例

1. 使用Properties类完成对mysql.properties的读取

   ```java
    public void test01() throws IOException {
           //使用 Properties 类来读取 mysql.properties 文件
           //1. 创建 Properties 对象
           Properties properties = new Properties();
           //2. 加载指定配置文件
           properties.load(new FileReader("src\\mysql.properties"));
           //3. 把 k-v 显示控制台
           properties.list(System.out);
           //4. 根据 key 获取对应的值
           String user = properties.getProperty("user");
           String pwd = properties.getProperty("pwd");
           System.out.println("用户名=" + user);
           System.out.println("密码是=" + pwd);
       }
   ```

2. 使用properties 类添加key-value到新文件mysql2.properties中

   ```java
    public void test02() throws IOException {
           //新建一个properties类
           Properties properties = new Properties();
   
           //写入数据
           properties.setProperty("汤姆", "21");
           properties.setProperty("jack", "22");
           properties.setProperty("11123", "1231");
           //将 k-v 存储文件中即可
           properties.store(new FileOutputStream("src\\mysql2.properties"), null);
       }
   ```



# 25. 反射

## 1. 基本介绍

1. 反射机制允许程序在执行期借助于ReflectionAPI取得任何类的内部信息(比如成员变量，构造器，成员方法等等)，并能操作对象的属性及方法。反射在设计模式和框架底层都会用到
2. 加载完类之后，在堆中就产生了一个Class类型的对象(一个类只有一个Class对象) ,这个对象包含了类的完整结构信息。通过这个对象得到类的结构。这个Class对象就像一面镜子，透过这个镜子看到类的结构，所以，形象的称之为:反射



## 2. 反射机制

1. Java反射机制原理图
   ![image-20220804184025687](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804184025687.png)
2. Java反射机制可以完成
   ![image-20220804184112551](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804184112551.png)
3. 反射相关的主要类
   ![image-20220804184850086](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804184850086.png)
4. 反射优点和缺点
   ![image-20220804185251296](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804185251296.png)
5. 反射调用优化——关闭访问检查
   ![image-20220804193213425](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804193213425.png)



## 3. Class类

1. 基本介绍
   ![image-20220804194545477](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804194545477.png)
   ![image-20220804194602331](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804194602331.png)

2. Class类常用方法
   ![image-20220804200447252](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804200447252.png)

   ```java
   //1 . 获取到 Car 类 对应的 Class 对象
   //<?> 表示不确定的 Java 类型
   Class<?> cls = Class.forName(classAllPath);
   
   //2. 输出 cls
   System.out.println(cls); //显示 cls 对象, 是哪个类的 Class 对象 com.hspedu.Car
   System.out.println(cls.getClass());//输出 cls 运行类型 java.lang.Class
   
   //3. 得到包名
   System.out.println(cls.getPackage().getName());//包名
   
   //4. 得到全类名
   System.out.println(cls.getName());
   
   //5. 通过 cls 创建对象实例
   Car car = (Car) cls.newInstance();
   System.out.println(car);//car.toString()
   
   //6. 通过反射获取属性 brand
   Field brand = cls.getField("brand");
   System.out.println(brand.get(car));//宝马
   
   //7. 通过反射给属性赋值
   brand.set(car, "奔驰");
   System.out.println(brand.get(car));//奔驰
   
   //8 我希望大家可以得到所有的属性(字段)
   System.out.println("=======所有的字段属性====");
   Field[] fields = cls.getFields();
   for (Field f : fields) {
   	System.out.println(f.getName());//名称
   }
   ```



## 4. 获取Class类对象

![image-20220804204112158](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804204112158.png)

![image-20220804204121502](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804204121502.png)

![image-20220804204131010](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804204131010.png)



## 5. 哪些类型有Class对象

![image-20220804204829150](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804204829150.png)



## 6. 类加载

### 6-1 基本介绍

1. 静态加载：编译是加载相关的类，如果没有则报错，依赖性太强
2. 动态加载：运行时加载需要的类，如果运行时不用该类，即使不存在该类，也不报错，降低了依赖性



### 6-2 类加载时机

![image-20220804210403308](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804210403308.png)



### 6-3 类加载过程图

![image-20220804210545043](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804210545043.png)



### 6-4 类加载各阶段完成任务

![image-20220804213116132](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804213116132.png)



### 6-5 加载阶段

![image-20220804213155478](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804213155478.png)



### 6-6 连接阶段——验证

![image-20220804215312272](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804215312272.png)



### 6-7 连接阶段——准备

![image-20220804215523952](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804215523952.png)



### 6-8 连接阶段——解析

![image-20220804215559448](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804215559448.png)



### 6-9 Initialization（初始化）

![image-20220804215626816](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804215626816.png)



## 7. 通过反射获取类的结构信息

1. 第一组: java.lang.Class 类
   ![image-20220804220232798](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804220232798.png)

2. 第二组: java.lang.reflect.Field 类

   ![image-20220804220301064](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804220301064.png)

3. 第三组: java.lang.reflect.Method 类
   ![image-20220804220325564](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804220325564.png)

4. 第四组: java.lang.reflect.Constructor 类
   ![image-20220804220340489](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804220340489.png)



## 8. 通过反射创建对象

![image-20220804222213942](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804222213942.png)



## 9. 通过反射访问类中的成员

### 9-1 访问属性

![image-20220804224334732](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804224334732.png)



### 9-2 访问方法

![image-20220804224511084](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220804224511084.png)

# 26. JDBC

## 1. JDBC概述

1. 基本介绍
   ![](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809133254050.png)
2. JDBC带来的好处
   ![image-20220809133508727](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809133508727.png)
3. JDBC的API
   ![image-20220809133524684](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809133524684.png)



## 2. JDBC程序

1. 注册驱动——加载Driver类
2. 获取连接——得到Connection
3. 执行增删改查——发送SQL给mysql执行
4. 释放资源——关闭相关连接

注意：要导入JDBC的相关jar包

```java
//第一种连接方式
    @Test
    public void connection01() throws SQLException {
        //1. 注册驱动
        Driver driver = new Driver();
        //2. 得到连接
        //(1) jdbc:mysql:// 规定好表示协议，通过 jdbc 的方式连接 mysql
        //(2) localhost 主机，可以是 ip
        //(3) 3306 表示 mysql 监听的端口
        //(4) hsp_db02 连接到 mysql dbms 的哪个数据库
        //(5) mysql 的连接本质就是前面学过的 socket 连接
        String url = "jdbc:mysql://localhost:3306/work01?serverTimezone=UTC";

        //将 用户名和密码放入到 Properties 对象
        Properties properties = new Properties();
        //说明 user 和 password 是规定好，后面的值根据实际情况写
        properties.setProperty("user", "root");// 用户
        properties.setProperty("password", "522166"); //密码
        Connection connect = driver.connect(url, properties);

        //3. 执行 sql
        //String sql = "insert into actor values(null, '刘德华', '男', '1970-11-11', '110')";
        //String sql = "update actor set name='周星驰' where id = 1";
        String sql = "insert into dog values (400,'allen')";
        //statement 用于执行静态 SQL 语句并返回其生成的结果的对象
        Statement statement = connect.createStatement();
        int rows = statement.executeUpdate(sql); // 如果是 dml 语句，返回的就是影响行数
        System.out.println(rows > 0 ? "成功" : "失败");

        //4. 关闭连接资源
        statement.close();
        connect.close();
    }
```



## 3. 获取数据库连接的5种方式

1. ```java
   //1. 注册驱动
   Driver driver = new Driver();
   //2. 得到连接
   //(1) jdbc:mysql:// 规定好表示协议，通过 jdbc 的方式连接 mysql
   //(2) localhost 主机，可以是 ip
   //(3) 3306 表示 mysql 监听的端口
   //(4) hsp_db02 连接到 mysql dbms 的哪个数据库
   //(5) mysql 的连接本质就是前面学过的 socket 连接
   String url = "jdbc:mysql://localhost:3306/work01?serverTimezone=UTC";
   
   //将 用户名和密码放入到 Properties 对象
   Properties properties = new Properties();
   //说明 user 和 password 是规定好，后面的值根据实际情况写
   properties.setProperty("user", "root");// 用户
   properties.setProperty("password", "522166"); //密码
   Connection connect = driver.connect(url, properties);
   ```

2. ```java
   Class<?> aClass = Class.forName("com.mysql.cj.jdbc.Driver");
   Driver driver = (Driver)aClass.getConstructor().newInstance();
   
   String url = "jdbc:mysql://localhost:3306/work01?serverTimezone=Asia/Shanghai";
   
   //将 用户名和密码放入到 Properties 对象
   Properties properties = new Properties();
   //说明 user 和 password 是规定好，后面的值根据实际情况写
   properties.setProperty("user", "root");// 用户
   properties.setProperty("password", "522166"); //密码
   Connection connect = driver.connect(url, properties);
   ```

3. ```java
   //使用反射加载 Driver
   Class<?> aClass = Class.forName("com.mysql.cj.jdbc.Driver");
   Driver driver = (Driver) aClass.newInstance();
   //创建 url 和 user 和 password
   String url = "jdbc:mysql://localhost:3306/work01";
   String user = "root";
   String password = "522166";
   DriverManager.registerDriver(driver);//注册 Driver 驱动
   Connection connection = DriverManager.getConnection(url, user, password);
   System.out.println("第三种方式=" + connection);
   ```

4. ```java
    Class<?> aClass = Class.forName("com.mysql.cj.jdbc.Driver");
   String url = "jdbc:mysql://localhost:3306/work01";
   String user = "root";
   String password = "522166";
   Connection connection = DriverManager.getConnection(url, user, password);
   System.out.println("第4种方式=" + connection);
   ```

5. ```java
   //使用配置文件来加载，也是开发中最常用的
    Properties properties = new Properties();
   properties.load(new FileInputStream("src\\mysql3.properties"));
   
   String user = properties.getProperty("user");
   String password = properties.getProperty("password");
   String driver = properties.getProperty("driver");
   String url = properties.getProperty("url");
   
   Class.forName(driver);
   
   Connection connection = DriverManager.getConnection(url, user, password);
   Statement statement = connection.createStatement();
   
   String sql = "insert into dog values(600, 'xxxtenx')";
   int i = statement.executeUpdate(sql);
   System.out.println(i);
   ```



## 4. 结果集ResuktSet

1. 基本介绍
   ![image-20220809134530645](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809134530645.png)

2. 演示

   ```java
   @Test
   public void selection() throws SQLException {
       String url = "jdbc:mysql://localhost:3306/work01";
       String user = "root";
       String password = "522166";
       Connection connection = DriverManager.getConnection(url, user, password);
       Statement statement = connection.createStatement();
   
       String sql = "select * from stu where math between 90 and 100 order by math";
       ResultSet resultSet = statement.executeQuery(sql);
   
       while (resultSet.next()) {
           //注意要根据得到的记录的列的数据类型来使用get
           int anInt = resultSet.getInt(1);    //获取当前得到的表的结果的第一列
           String string = resultSet.getString(2); //获取第二列
           System.out.println(anInt + " : " + string);
       }
   
       connection.close();
       statement.close();
   }
   ```



## 5. Statement 和 preparedStatement

1. Statement基本介绍

![image-20220809134813325](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809134813325.png)



2. preparedStatement基本介绍
   ![image-20220809135037851](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809135037851.png)

![image-20220809135047645](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809135047645.png)



## 6. JDBC相关API总结

![image-20220809141406546](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809141406546.png)



## 7. 事务

![image-20220809141457963](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809141457963.png)



使用事务解决转账的问题：

```java
@Test
public void test() throws SQLException {
    Connection connection = JDBCUtils.getConnection();

    String sql1 = "update account set balance = balance - 100 where id = 1";
    String sql2 = "update account set balance = balance + 100 where id = 2";
    PreparedStatement preparedStatement = null;

    //设置事务
    connection.setAutoCommit(false);

    try {
        preparedStatement = connection.prepareStatement(sql1);
        int i = preparedStatement.executeUpdate();

        //            int x = 1 / 0;

        preparedStatement = connection.prepareStatement(sql2);
        int i1 = preparedStatement.executeUpdate();

        //如果没有出现异常，则执行提交操作
        connection.commit();

    } catch (SQLException e) {
        e.printStackTrace();

        //出现异常则回滚到事务的开始
        connection.rollback();
    } finally {
        JDBCUtils.close(preparedStatement, connection);
    }
}
```



## 8. 批处理

![image-20220809141645366](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809141645366.png)



## 9. 数据库连接池

1. 传统的Connection获取问题
   ![image-20220809142030697](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809142030697.png)

2. 数据库连接池种类
   ![image-20220809142108139](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809142108139.png)

3. Druid（德鲁伊）工具类的实现

   ```java
   public class JDBCUtilsByDruid {
       private static DataSource ds;
   
       static {
           Properties properties = new Properties();
           try {
               properties.load(new FileInputStream("src\\druid.properties"));
               ds = DruidDataSourceFactory.createDataSource(properties);
           } catch (IOException e) {
               e.printStackTrace();
           } catch (Exception e) {
               e.printStackTrace();
           }
       }
   
       public static Connection getConnection() throws SQLException {
           return ds.getConnection();
       }
   
       public static void close(ResultSet set, Statement statement, Connection connection) {
           try {
               if (set != null) {
                   set.close();
               }
               if (statement != null) {
                   statement.close();
               }
               if (connection != null) {
                   connection.close();
               }
           } catch (SQLException e) {
               throw new RuntimeException(e);
           }
       }
   
       public static void close(Statement statement, Connection connection) {
           try {
               if (statement != null) {
                   statement.close();
               }
               if (connection != null) {
                   connection.close();
               }
           } catch (SQLException e) {
               throw new RuntimeException(e);
           }
       }
   
       public static void close(Connection connection) {
           try {
               if (connection != null) {
                   connection.close();
               }
           } catch (SQLException e) {
               throw new RuntimeException(e);
           }
       }
   }
   ```



## 10. Apache——DBUtils

**之前使用的JDBC的问题：**

1. 关闭Connection后，由sql语句获取到的结果集就无法使用了
2. resultSet不利于数据的管理

![image-20220809142452585](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809142452585.png)



DBUtils**的基本介绍：**

![image-20220809142536057](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809142536057.png)



 **表和 JavaBean 的类型映射关系**：

![image-20220809144610766](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220809144610766.png)









