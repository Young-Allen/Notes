# 第一章 XML&Tomcat&Http协议

## 学习目标

* 了解配置文件的作用
* 了解常见的配置文件类型
* 掌握properties文件的编写规范
* 掌握xml文件的编写
* 了解xml文件的约束  
* 掌握xml文件的解析
* 掌握Tomcat的安装
* 掌握Tomcat的使用
* 掌握Tomcat在IDEA中的使用
* 了解HTTP协议的发展历程
* 了解HTTP1.0和HTTP1.1的区别
* 掌握请求报文和响应报文的格式和内容

## 1. xml解析(了解)

### 1.1 配置文件

#### 1.1.1 配置文件的作用

配置文件是用于给应用程序提供配置参数以及初始化设置的一些有特殊格式的文件

#### 1.1.2 常见的配置文件类型

1. properties文件,例如druid连接池就是使用properties文件作为配置文件
2. XML文件,例如Tomcat就是使用XML文件作为配置文件
3. YAML文件,例如SpringBoot就是使用YAML作为配置文件
4. json文件,通常用来做文件传输，也可以用来做前端或者移动端的配置文件

### 1.2 properties文件

#### 1.2.1 文件示例

```properties
atguigu.jdbc.url=jdbc:mysql://192.168.198.100:3306/bj1026
atguigu.jdbc.driver=com.mysql.cj.jdbc.Driver
atguigu.jdbc.username=root
atguigu.jdbc.password=root
```

#### 1.2.2 语法规范

- 由键值对组成
- 键和值之间的符号是等号
- 每一行都必须顶格写，前面不能有空格之类的其他符号

### 1.3 XML文件

#### 1.3.1 文件示例

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!-- 配置SpringMVC前端控制器 -->
    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <!-- 在初始化参数中指定SpringMVC配置文件位置 -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:spring-mvc.xml</param-value>
        </init-param>

        <!-- 设置当前Servlet创建对象的时机是在Web应用启动时 -->
        <load-on-startup>1</load-on-startup>

    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>

        <!-- url-pattern配置斜杠表示匹配所有请求 -->
        <!-- 两种可选的配置方式：
                1、斜杠开头：/
                2、包含星号：*.atguigu
             不允许的配置方式：前面有斜杠，中间有星号
                /*.app
         -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

#### 1.3.2 概念介绍

XML是extensible Markup Language的缩写，翻译过来就是<span style="color:green;font-weight:bold;">可扩展标记语言</span>。所以很明显，XML和HTML一样都是标记语言，也就是说它们的基本语法都是标签。

**可扩展**

<span style="color:green;font-weight:bold;">可扩展</span>三个字<span style="color:green;font-weight:bold;">表面上</span>的意思是XML允许<span style="color:green;font-weight:bold;">自定义格式</span>。但是别美，这<span style="color:green;font-weight:bold;">不代表</span>你<span style="color:green;font-weight:bold;">可以随便写</span>。

![image-20220829221839950](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829221839950.png)

在XML基本语法规范的基础上，你使用的那些第三方应用程序、框架会通过设计<span style="color:green;font-weight:bold;">『XML约束』</span>的方式<span style="color:green;font-weight:bold;">『强制规定』</span>配置文件中可以写什么和怎么写，规定之外的都不可以写。

XML基本语法这个知识点的定位是：我们不需要从零开始，从头到尾的一行一行编写XML文档，而是在第三方应用程序、框架<span style="color:green;font-weight:bold;">已提供的配置文件</span>的基础上<span style="color:green;font-weight:bold;">修改</span>。要改成什么样取决于你的需求，而怎么改取决于<span style="color:green;font-weight:bold;">XML基本语法</span>和<span style="color:green;font-weight:bold;">具体的XML约束</span>。

#### 1.3.3 XML的基本语法

- XML文档声明

这部分基本上就是固定格式，要注意的是文档声明一定要从第一行第一列开始写

```xml
<?xml version="1.0" encoding="UTF-8"?>
```

- 根标签

根标签有且只能有一个。

- 标签关闭
  - 双标签：开始标签和结束标签必须成对出现。
  - 单标签：单标签在标签内关闭。
- 标签嵌套
  - 可以嵌套，但是不能交叉嵌套。
- 注释不能嵌套
- 标签名、属性名建议使用小写字母
- 属性
  - 属性必须有值
  - 属性值必须加引号，单双都行

看到这里大家一定会发现XML的基本语法和HTML的基本语法简直如出一辙。其实这不是偶然的，XML基本语法+HTML约束=HTML语法。在逻辑上HTML确实是XML的子集。

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
```

从HTML4.01版本的文档类型声明中可以看出，这里使用的DTD类型的XML约束。也就是说http://www.w3.org/TR/html4/loose.dtd这个文件定义了HTML文档中可以写哪些标签，标签内可以写哪些属性，某个标签可以有什么样的子标签。

#### 1.3.4 XML的约束(稍微了解)

将来我们主要就是根据XML约束中的规定来编写XML配置文件，而且会在我们编写XML的时候根据约束来提示我们编写, 而XML约束主要包括DTD和Schema两种。

- DTD

- Schema

Schema约束要求我们一个XML文档中，所有标签，所有属性都必须在约束中有明确的定义。

下面我们以web.xml的约束声明为例来做个说明：

```xml
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
```

#### 1.3.5 XML解析

##### ① XML解析的作用

用Java代码读取xml中的数据

##### ② DOM4J的使用步骤

1. 导入jar包 dom4j.jar
2. 创建解析器对象(SAXReader)
3. 解析xml 获得Document对象
4. 获取根节点RootElement
5. 获取根节点下的子节点

##### ③ DOM4J的API介绍

1. 创建SAXReader对象

```java
SAXReader saxReader = new SAXReader();
```

2. 解析XML获取Document对象: 需要传入要解析的XML文件的字节输入流

```java
Document document = reader.read(inputStream);
```

3. 获取文档的根标签

```java
Element rootElement = documen.getRootElement()
```

4. 获取标签的子标签

```java
//获取所有子标签
List<Element> sonElementList = rootElement.elements();
//获取指定标签名的子标签
List<Element> sonElementList = rootElement.elements("标签名");
```

5. 获取标签体内的文本

```java
String text = element.getText();
```

6. 获取标签的某个属性的值

```java
String value = element.AttributeValue("属性名");
```

## 2. Tomcat(最重要)

### 2.1 Web服务器

- Web服务器通常由硬件和软件共同构成。

  - 硬件：电脑，提供服务供其它客户电脑访问

    ![1561995738943](https://raw.githubusercontent.com/Young-Allen/pic/main/1561995738943.png)

  - 软件：电脑上安装的服务器软件，安装后能提供服务给网络中的其他计算机，**将本地文件映射成一个虚拟的url地址供网络中的其他人访问。**

- Web服务器主要用来接收客户端发送的请求和响应客户端请求。

- 常见的JavaWeb服务器：

  - **Tomcat（Apache）**：当前应用最广的JavaWeb服务器
  - JBoss（Redhat红帽）：支持JavaEE，应用比较广EJB容器 –> SSH轻量级的框架代替
  - GlassFish（Orcale）：Oracle开发JavaWeb服务器，应用不是很广
  - Resin（Caucho）：支持JavaEE，应用越来越广
  - Weblogic（Orcale）：要钱的！支持JavaEE，适合大型项目
  - Websphere（IBM）：要钱的！支持JavaEE，适合大型项目

### 2.2 Tomcat服务器

#### 2.2.1 Tomcat简介

Tomcat是Apache 软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，由Apache、Sun 和其他一些公司及个人共同开发而成。由于有了Sun 的参与和支持，最新的Servlet 和JSP 规范总是能在Tomcat 中得到体现，因为Tomcat 技术先进、性能稳定，而且免费，因而深受Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的Web 应用服务器。

#### 2.2.2 Tomcat下载

- Tomcat官方网站：<http://tomcat.apache.org/>
- 安装版：需要安装，一般不考虑使用。
- 解压版: 直接解压缩使用，我们使用的版本。
- **因为tomcat服务器软件需要使用java环境，所以需要正确配置JAVA_HOME**。

#### 2.2.3 Tomcat的版本

- 版本：企业用的比较广泛的是7.0和8.0的。授课我们使用8.0。
- tomcat6以下都不用了，所以我们从tomcat6开始比较：
  - tomcat6 	支持servlet2.5、jsp2.1、el
  - tomcat7 	支持servlet3.0、jsp2.2、el2.2、websocket1.1
  - tomcat8	 支持servlet3.1、jsp2.3、el3.0、websocket1.1
  - tomcat9	 支持servlet4.0、jsp2.3、el3.0、websocket1.1

#### 2.2.4 安装

解压apache-tomcat-8.5.27-windows-x64.zip到**非中文无空格**目录中

![1557499687108](https://raw.githubusercontent.com/Young-Allen/pic/main/1557499687108.png)

D:\developer_tools\apache-tomcat-8.5.27，这个目录下直接包含Tomcat的bin目录，conf目录等，我们称之为**Tomcat的安装目录或根目录**。

- bin：该目录下存放的是二进制可执行文件，如果是安装版，那么这个目录下会有两个exe文件：tomcat6.exe、tomcat6w.exe，前者是在控制台下启动Tomcat，后者是弹出GUI窗口启动Tomcat；如果是解压版，那么会有startup.bat和shutdown.bat文件，startup.bat用来启动Tomcat，但需要先配置JAVA_HOME环境变量才能启动，shutdawn.bat用来停止Tomcat；
- conf：这是一个非常非常重要的目录，这个目录下有四个最为重要的文件：
  - **server.xml：配置整个服务器信息。例如修改端口号。默认HTTP请求的端口号是：8080**
  - tomcat-users.xml：存储tomcat用户的文件，这里保存的是tomcat的用户名及密码，以及用户的角色信息。可以按着该文件中的注释信息添加tomcat用户，然后就可以在Tomcat主页中进入Tomcat Manager页面了；
  - web.xml：部署描述符文件，这个文件中注册了很多MIME类型，即文档类型。这些MIME类型是客户端与服务器之间说明文档类型的，如用户请求一个html网页，那么服务器还会告诉客户端浏览器响应的文档是text/html类型的，这就是一个MIME类型。客户端浏览器通过这个MIME类型就知道如何处理它了。当然是在浏览器中显示这个html文件了。但如果服务器响应的是一个exe文件，那么浏览器就不可能显示它，而是应该弹出下载窗口才对。MIME就是用来说明文档的内容是什么类型的！
  - context.xml：对所有应用的统一配置，通常我们不会去配置它。
- lib：Tomcat的类库，里面是一大堆jar文件。如果需要添加Tomcat依赖的jar文件，可以把它放到这个目录中，当然也可以把应用依赖的jar文件放到这个目录中，这个目录中的jar所有项目都可以共享之，但这样你的应用放到其他Tomcat下时就不能再共享这个目录下的jar包了，所以建议只把Tomcat需要的jar包放到这个目录下；
- logs：这个目录中都是日志文件，记录了Tomcat启动和关闭的信息，如果启动Tomcat时有错误，那么异常也会记录在日志文件中。
- temp：存放Tomcat的临时文件，这个目录下的东西可以在停止Tomcat后删除！
- **webapps：存放web项目的目录，其中每个文件夹都是一个项目**；如果这个目录下已经存在了目录，那么都是tomcat自带的项目。其中ROOT是一个特殊的项目，在地址栏中访问：http://127.0.0.1:8080，没有给出项目目录时，对应的就是ROOT项目。<http://localhost:8080/examples，进入示例项目。其中examples>就是项目名，即文件夹的名字。
- work：运行时生成的文件，最终运行的文件都在这里。通过webapps中的项目生成的！可以把这个目录下的内容删除，再次运行时会生再次生成work目录。当客户端用户访问一个JSP文件时，Tomcat会通过JSP生成Java文件，然后再编译Java文件生成class文件，生成的java和class文件都会存放到这个目录下。
- LICENSE：许可证。
- NOTICE：说明文件。



#### 2.2.5 配置

启动Tomcat前，需要配置如下的环境变量

① 配置JAVA_HOME环境变量

![1557501069603](https://raw.githubusercontent.com/Young-Allen/pic/main/1557501069603.png)

② 在Path环境变量中加入JAVA_HOME\bin目录

![1561975042021](https://raw.githubusercontent.com/Young-Allen/pic/main/1561975042021.png)

#### 2.2.6 启动 

在命令行中运行**catalina run**或者 Tomcat解压目录下**双击startup.bat** 启动Tomcat服务器，在浏览器地址栏访问如下地址进行测试

**http://localhost:8080**



如果启动失败，查看如下的情况：

情况一：如果双击startup.bat后窗口一闪而过，请查看JAVA_HOME是否配置正确。

> startup.bat会调用catalina.bat，而catalina.bat会调用setclasspath.bat，setclasspath.bat会使用JAVA_HOME环境变量，所以我们必须在启动Tomcat之前把JAVA_HOME配置正确。

情况二：如果启动失败，提示端口号被占用，则将默认的8080端口修改为其他未使用的值，例如8989等。

【方法】 打开：解压目录\conf\server.xml，找到第一个Connector标签，修改port属性

> web服务器在启动时，实际上是监听了本机上的一个端口，当有客户端向该端口发送请求时，web服务器就会处理请求。但是如果不是向其所监听的端口发送请求，web服务器不会做任何响应。例如：Tomcat启动监听了8989端口，而访问的地址是<http://localhost:8080>，将不能正常访问。

#### 2.2.7 在IDEA中创建Tomcat

在IDEA中配置好Tomcat后，可以直接通过IDEA控制Tomcat的启动和停止，而不用再去操作startup.bat和shutdown.bat。

① 点击File-->Settings  或者直接点击图标

![image-20220829222233792](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222233792.png)

下一步

![image-20220829222246313](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222246313.png)

下一步：

![image-20220829222304278](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222304278.png)



### 2.3 动态Web工程部署与测试

#### 2.3.1 创建**动态**Web工程

![image-20220829222326228](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222326228.png)

接着：

![1592227253541](https://raw.githubusercontent.com/Young-Allen/pic/main/1592227253541.png)

![1592227294518](https://raw.githubusercontent.com/Young-Allen/pic/main/1592227294518.png)



#### 2.3.2 开发项目目录结构说明

- **src：存放Java源代码的目录。**

- web：存放的是需要部署到服务器的文件

  - WEB-INF：**这个目录下的文件，是不能被客户端直接访问的。**
    - **lib：用于存放该工程用到的库。粘贴过来以后**
    - **web.xml：web工程的配置文件，完成用户请求的逻辑名称到真正的servlet类的映射。**

  > 凡是客户端能访问的资源(*.html或 *.jpg)必须跟WEB-INF在同一目录，即放在Web根目录下的资源，从客户端是可以通过URL地址直接访问的。

![image-20220829222344625](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222344625.png)

#### 2.3.3 tomcat实例的基础设置

由于每次创建项目随之创建的tomcat实例名字都类似，所以建议修改一下tomcat实例的名称

![1592228000124](https://raw.githubusercontent.com/Young-Allen/pic/main/1592228000124.png)

在如下界面进行基础设置

![image-20220829222403372](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222403372.png)



## 3. HTTP协议简介

![img](https://raw.githubusercontent.com/Young-Allen/pic/main/timg.jpg)

- **HTTP 超文本传输协议** (HTTP-Hypertext transfer protocol)，是一个属于应用层的面向对象的协议，由于其简捷、快速的方式，适用于分布式超媒体信息系统。它于1990年提出，经过十几年的使用与发展，得到不断地完善和扩展。**它是一种详细规定了浏览器和万维网服务器之间互相通信的规则**，通过因特网传送万维网文档的数据传送协议。

- 客户端与服务端通信时传输的内容我们称之为**报文**。**HTTP协议就是规定报文的格式。**

- HTTP就是一个通信规则，这个规则规定了客户端发送给服务器的报文格式，也规定了服务器发送给客户端的报文格式。实际我们要学习的就是这两种报文。客户端发送给服务器的称为”**请求报文**“，服务器发送给客户端的称为”**响应报文**“。

- 类比于生活中案例：① 毕业租房，签署租房协议，规范多方需遵守的规则。②与远方的朋友的写信。信封的规范。

  实际互联网：

  - 客户端 与 服务端进行通信。比如：用户 ---> 访问京东（就是一个数据传输的过程），数据传输需要按照一种协议去传输。就如，用户给服务器写信；服务器给用户回信。有格式：协议。HTTP协议规定通信规则。规定互联网之间如何传输数据。
    - 信：报文。
    - 写信：用户给服务器写信，用户给服务器发请求。把发的请求所有数据，请求报文
    - 回信：服务器回信给用户，回给浏览器。把服务器响应浏览器的所有数据，响应报文



### 3.1 HTTP协议的发展历程

- 超文本传输协议的前身是世外桃源(Xanadu)项目，超文本的概念是泰德·纳尔森(Ted Nelson)在1960年代提出的。进入哈佛大学后，纳尔森一直致力于超文本协议和该项目的研究，但他从未公开发表过资料。1989年，**蒂姆·伯纳斯·李**(Tim Berners Lee)在CERN(欧洲原子核研究委员会 = European Organization for Nuclear Research)担任软件咨询师的时候，开发了一套程序，**奠定了万维网(WWW = World Wide Web)**的基础。1990年12月，超文本在CERN首次上线。1991年夏天，继Telnet等协议之后，超文本转移协议成为互联网诸多协议的一分子。
- 当时，**Telnet协议**解决了一台计算机和另外一台计算机之间一对一的控制型通信的要求。邮件协议解决了一个发件人向少量人员发送信息的通信要求。**文件传输协议**解决一台计算机从另外一台计算机批量获取文件的通信要求，但是它不具备一边获取文件一边显示文件或对文件进行某种处理的功能。**新闻传输协议**解决了一对多新闻广播的通信要求。而**超文本要解决的通信要求是**：在一台计算机上获取并显示存放在多台计算机里的文本、数据、图片和其他类型的文件；它包含两大部分：超文本转移协议和超文本标记语言(HTML)。HTTP、HTML以及浏览器的诞生给互联网的普及带来了飞跃。

### 3.2 HTTP协议的会话方式

- 浏览器与服务器之间的通信过程要经历四个步骤

![image-20220829222458796](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222458796.png)

- 浏览器与WEB服务器的连接过程是短暂的，每次连接只处理一个请求和响应。对每一个页面的访问，浏览器与WEB服务器都要建立一次单独的连接。
- 浏览器到WEB服务器之间的所有通讯都是完全独立分开的请求和响应对。



### 3.3 HTTP1.0和HTTP1.1的区别

在HTTP1.0版本中，浏览器请求一个带有图片的网页，会由于下载图片而与服务器之间开启一个新的连接；但在HTTP1.1版本中，允许浏览器在拿到当前请求对应的全部资源后再断开连接，提高了效率。

![image-20220829222507053](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222507053.png)

### 3.4 不同浏览器监听HTTP操作

#### 3.4.1 IE浏览器中F12可查看

![image-20220829222516114](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222516114.png)

#### 3.4.2 Chrome中F12可查看

![1561904121561](https://raw.githubusercontent.com/Young-Allen/pic/main/1561904121561.png)

#### 3.4.3 FireFox中F12可查看

![1561904179928](https://raw.githubusercontent.com/Young-Allen/pic/main/1561904179928.png)

### 3.5 报文

#### 3.5.1 报文格式

![1557672592385](https://raw.githubusercontent.com/Young-Allen/pic/main/1557672592385.png)

- 报文：
  - 请求报文：浏览器发给服务器
  - 响应报文：服务器发回给浏览器

#### 3.5.2 请求报文

##### ① 报文格式 (4部分)

- 请求首行（**请求行**）；
- 请求头信息（**请求头**）；
- 空行；
- 请求体；

##### ② GET请求

1、由于请求参数在请求首行中已经携带了，所以没有请求体，也没有请求空行
2、请求参数拼接在url地址中，地址栏可见[url?name1=value1&name2=value2]，不安全
3、由于参数在地址栏中携带，所以由大小限制[地址栏数据大小一般限制为4k]，只能携带纯文本
4、get请求参数只能上传文本数据
5、没有请求体。所以封装和解析都快，效率高， 浏览器默认提交的请求都是get请求[比如：① 地址栏输入url地址回车，②点击超链接a ， ③ form表单默认方式...]

- **请求首行**

```http
GET /05_web_tomcat/login_success.html?username=admin&password=123213 HTTP/1.1

请求方式 	访问的服务器中的资源路径？get请求参数	协议版本
```

- **请求头**

```http
Host: localhost:8080   主机虚拟地址
Connection: keep-alive 长连接
Upgrade-Insecure-Requests: 1  请求协议的自动升级[http的请求，服务器却是https的，浏览器自动会将请求协议升级为https的]
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.75 Safari/537.36
- 用户系统信息
Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
- 浏览器支持的文件类型
Referer: http://localhost:8080/05_web_tomcat/login.html
- 当前页面的上一个页面的路径[当前页面通过哪个页面跳转过来的]：   可以通过此路径跳转回上一个页面， 广告计费，防止盗链
Accept-Encoding: gzip, deflate, br
- 浏览器支持的压缩格式
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
- 浏览器支持的语言
```



##### ③ POST请求

- POST请求要求将form标签的method的属性设置为post

![image-20220829222558032](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222558032.png)

1、POST请求有请求体，而GET请求没有请求体。
2、post请求数据在请求体中携带，请求体数据大小没有限制，可以用来上传所有内容[文件、文本]
3、只能使用post请求上传文件
4、post请求报文多了和请求体相关的配置[请求头]
5、地址栏参数不可见，相对安全
6、post效率比get低

- 请求首行

```http
POST /05_web_tomcat/login_success.html HTTP/1.1
```

- 请求头

```http
Host: localhost:8080
Connection: keep-alive
Content-Length: 31 		-请求体内容的长度
Cache-Control: max-age=0  -无缓存
Origin: http://localhost:8080
Upgrade-Insecure-Requests: 1  -协议的自动升级
Content-Type: application/x-www-form-urlencoded   -请求体内容类型[服务器根据类型解析请求体参数]
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/68.0.3440.75 Safari/537.36
Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Referer: http://localhost:8080/05_web_tomcat/login.html
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7
Cookie:JSESSIONID-
```

- 请求空行
- 请求体：浏览器提交给服务器的内容

```http
username=admin&password=1232131
```



#### 3.5.3 响应报文

##### ① 报文格式(4部分)

- 响应首行（**响应行**）；
- 响应头信息（**响应头**）；
- 空行；
- **响应体；**

##### ② 具体情况

- **响应首行：**

  ```http
  HTTP/1.1 200 OK
  
  说明：响应协议为HTTP1.1，响应状态码为200，表示请求成功； 
  ```

- **响应头：**

  ```http
  Server: Apache-Coyote/1.1   服务器的版本信息
  Accept-Ranges: bytes
  ETag: W/"157-1534126125811"
  Last-Modified: Mon, 13 Aug 2018 02:08:45 GMT
  Content-Type: text/html    响应体数据的类型[浏览器根据类型解析响应体数据]
  Content-Length: 157   响应体内容的字节数
  Date: Mon, 13 Aug 2018 02:47:57 GMT  响应的时间，这可能会有8小时的时区差
  ```

- **响应空行**

- **响应体**

  ```html
  <!--需要浏览器解析使用的内容[如果响应的是html页面，最终响应体内容会被浏览器显示到页面中]-->
  
  <!DOCTYPE html>
  <html>
  	<head>
  		<meta charset="UTF-8">
  		<title>Insert title here</title>
  	</head>
  	<body>
  		恭喜你，登录成功了...
  	</body>
  </html>
  ```

##### ③ 响应码

响应码对浏览器来说很重要，它告诉浏览器响应的结果。比较有代表性的响应码如下：

- **200：**请求成功，浏览器会把响应体内容（通常是html）显示在浏览器中；

- **404：**请求的资源没有找到，说明客户端错误的请求了不存在的资源；

- **500：**请求资源找到了，但服务器内部出现了错误；

  ![1561905801690](https://raw.githubusercontent.com/Young-Allen/pic/main/1561905801690.png)

- **302：**重定向，当响应码为302时，表示服务器要求浏览器重新再发一个请求，服务器会发送一个响应头Location，它指定了新请求的URL地址；

除此之外，其它一些响应码如下：

```
200 - 服务器成功返回网页 
404 - 请求的网页不存在 
503 - 服务不可用 
详细分解：

1xx（临时响应） 
表示临时响应并需要请求者继续执行操作的状态代码。

代码 说明 
100 （继续） 请求者应当继续提出请求。服务器返回此代码表示已收到请求的第一部分，正在等待其余部分。 
101 （切换协议） 请求者已要求服务器切换协议，服务器已确认并准备切换。

2xx （成功） 
表示成功处理了请求的状态代码。

代码 说明 
200 （成功） 服务器已成功处理了请求。通常，这表示服务器提供了请求的网页。 
201 （已创建） 请求成功并且服务器创建了新的资源。 
202 （已接受） 服务器已接受请求，但尚未处理。 
203 （非授权信息） 服务器已成功处理了请求，但返回的信息可能来自另一来源。 
204 （无内容） 服务器成功处理了请求，但没有返回任何内容。 
205 （重置内容） 服务器成功处理了请求，但没有返回任何内容。 
206 （部分内容） 服务器成功处理了部分 GET 请求。

3xx （重定向） 
表示要完成请求，需要进一步操作。 通常，这些状态代码用来重定向。

代码 说明 
300 （多种选择） 针对请求，服务器可执行多种操作。服务器可根据请求者 (user agent) 选择一项操作，或提供操作列表供请求者选择。 
301 （永久移动） 请求的网页已永久移动到新位置。服务器返回此响应（对 GET 或 HEAD 请求的响应）时，会自动将请求者转到新位置。 
302 （临时移动） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。 
303 （查看其他位置） 请求者应当对不同的位置使用单独的 GET 请求来检索响应时，服务器返回此代码。 
304 （未修改） 自从上次请求后，请求的网页未修改过。服务器返回此响应时，不会返回网页内容。 
305 （使用代理） 请求者只能使用代理访问请求的网页。如果服务器返回此响应，还表示请求者应使用代理。 
307 （临时重定向） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。

4xx（请求错误） 
这些状态代码表示请求可能出错，妨碍了服务器的处理。

代码 说明 
400 （错误请求） 服务器不理解请求的语法。 
401 （未授权） 请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。 
403 （禁止） 服务器拒绝请求。 
404 （未找到） 服务器找不到请求的网页。 
405 （方法禁用） 禁用请求中指定的方法。 
406 （不接受） 无法使用请求的内容特性响应请求的网页。 
407 （需要代理授权） 此状态代码与 401（未授权）类似，但指定请求者应当授权使用代理。 
408 （请求超时） 服务器等候请求时发生超时。 
409 （冲突） 服务器在完成请求时发生冲突。服务器必须在响应中包含有关冲突的信息。 
410 （已删除） 如果请求的资源已永久删除，服务器就会返回此响应。 
411 （需要有效长度） 服务器不接受不含有效内容长度标头字段的请求。 
412 （未满足前提条件） 服务器未满足请求者在请求中设置的其中一个前提条件。 
413 （请求实体过大） 服务器无法处理请求，因为请求实体过大，超出服务器的处理能力。 
414 （请求的 URI 过长） 请求的 URI（通常为网址）过长，服务器无法处理。 
415 （不支持的媒体类型） 请求的格式不受请求页面的支持。 
416 （请求范围不符合要求） 如果页面无法提供请求的范围，则服务器会返回此状态代码。 
417 （未满足期望值） 服务器未满足”期望”请求标头字段的要求。

5xx（服务器错误） 
这些状态代码表示服务器在尝试处理请求时发生内部错误。 这些错误可能是服务器本身的错误，而不是请求出错。

代码 说明 
500 （服务器内部错误） 服务器遇到错误，无法完成请求。 
501 （尚未实施） 服务器不具备完成请求的功能。例如，服务器无法识别请求方法时可能会返回此代码。 
502 （错误网关） 服务器作为网关或代理，从上游服务器收到无效响应。 
503 （服务不可用） 服务器目前无法使用（由于超载或停机维护）。通常，这只是暂时状态。 
504 （网关超时） 服务器作为网关或代理，但是没有及时从上游服务器收到请求。 
505 （HTTP 版本不受支持） 服务器不支持请求中所用的 HTTP 协议版本。

HttpWatch状态码Result is

200 - 服务器成功返回网页，客户端请求已成功。 
302 - 对象临时移动。服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。 
304 - 属于重定向。自上次请求后，请求的网页未修改过。服务器返回此响应时，不会返回网页内容。 
401 - 未授权。请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。 
404 - 未找到。服务器找不到请求的网页。 
2xx - 成功。表示服务器成功地接受了客户端请求。 
3xx - 重定向。表示要完成请求，需要进一步操作。客户端浏览器必须采取更多操作来实现请求。例如，浏览器可能不得不请求服务器上的不同的页面，或通过代理服务器重复该请求。 
4xx - 请求错误。这些状态代码表示请求可能出错，妨碍了服务器的处理。 
5xx - 服务器错误。表示服务器在尝试处理请求时发生内部错误。 这些错误可能是服务器本身的错误，而不是请求出错。
```



# 第二章 Servlet组件

## 1 我们为什么需要Servlet？

### 1.1 Web应用基本运行模式

- 生活中的例子

![1557676745259](https://raw.githubusercontent.com/Young-Allen/pic/main/1557676745259.png)

- Web应用运行模

![image-20220829222646258](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222646258.png)

### 1.2 Web服务器中Servlet作用举例

- 举例一：插入数据

![1557751488465](https://raw.githubusercontent.com/Young-Allen/pic/main/1557751488465.png)

- 举例二：查询数据

通过网页驱动服务器端的Java程序。在网页上显示Java程序返回的数据。

![image-20220829222656143](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222656143.png)



## 2 什么是Servlet？

**如果把Web应用比作一个餐厅，Servlet就是餐厅中的服务员**——负责接待顾客、上菜、结账。

![1557753017639](https://raw.githubusercontent.com/Young-Allen/pic/main/1557753017639.png)

- 从广义上来讲，Servlet规范是Sun公司制定的一套技术标准，包含与Web应用相关的一系列接口，是Web应用实现方式的宏观解决方案。而具体的Servlet容器负责提供标准的实现。
- 从狭义上来讲，Servlet指的是javax.servlet.Servlet接口及其子接口，也可以指实现了Servlet接口的实现类。
- Servlet（**Server Applet**）作为服务器端的一个组件，它的本意是“服务器端的小程序”。
  - **Servlet的实例对象由Servlet容器负责创建；**
  - **Servlet的方法由容器在特定情况下调用；**
  - **Servlet容器会在Web应用卸载时销毁Servlet对象的实例。**

## 3 如何使用Servlet？

### 3.1 操作步骤

- 复习：使用一个接口的传统方式：

  - 创建一个类实现接口
  - new 实现类的对象
  - 调用类的方法等

  

- 使用Servlet接口的方式：

  ① 搭建Web开发环境

  ② 创建动态Web工程

  ③ 创建javax.servlet.Servlet接口的实现类：com.atguigu.servlet.MyFirstServlet

  ④ 在service(ServletRequest, ServletResponse)方法中编写如下代码，输出响应信息：

```java
@Override
	public void service(ServletRequest req, ServletResponse res)
			throws ServletException, IOException {
		//1.编写输出语句，证明当前方法被调用
		System.out.println("Servlet worked...");
		//2.通过PrintWriter对象向浏览器端发送响应信息
		PrintWriter writer = res.getWriter();
		writer.write("Servlet response");
		writer.close();
	}

```

​	⑤ 在web.xml配置文件中**注册**MyFirstServlet

```xml
<!-- 声明一个Servlet，配置的是Servlet的类信息 -->
<servlet>
	<!-- 这是Servlet的别名，一个名字对应一个Servlet。相当于变量名 -->
	<servlet-name>MyFirstServlet</servlet-name>
	<!-- Servlet的全类名，服务器会根据全类名找到这个Servlet -->
	<servlet-class>com.atguigu.servlet.MyFirstServlet</servlet-class>
</servlet>

<!-- 建立Servlet的请求映射信息 -->
<servlet-mapping>
	<!-- Servlet的别名，说明这个Servlet将会响应下面url-pattern的请求 -->
	<servlet-name>MyFirstServlet</servlet-name>
	<!-- Servlet响应的请求路径。如果访问这个路径，这个Servlet就会响应 -->
	<url-pattern>/MyFirstServlet</url-pattern>
</servlet-mapping>

```

> 说明：
>
> - <url-pattern>：这个url-pattern可以配置多个，这时表示的就是访问这些url都会触发这个Servlet进行响应，运行浏览器，访问刚才配置的url路径，Servlet的service方法就会被调用。
>
> - <url-pattern>中的文本内容必须以 / 或 *. 开始书写路径。相当于将资源映射到项目根目录下形成虚拟的资源文件。
> - <servlet-mapping>中的<url-pattern>可以声明多个，可以通过任意一个都可以访问。但是开发中一般只会配置一个。

​	⑥ 在WebContent目录下创建index.html

​	⑦ 在index.html中加入超链接 \<a href="MyFirstServlet">To Servlet</a>

​	⑧ 点击超链接测试Servlet

### 3.2 运行分析(执行原理)

- index.html

![image-20220829222858912](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222858912.png)

- web.xml

![image-20220829222905412](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222905412.png)

- 如果配置文件一旦修改，需要重启服务器来重新部署web项目。


### 3.3 Servlet作用总结

- 接收请求 【解析请求报文中的数据：请求参数】

- 处理请求 【DAO和数据库交互】

- 完成响应 【设置响应报文】

![1557755002982](https://raw.githubusercontent.com/Young-Allen/pic/main/1557755002982.png)

## 4 Servlet生命周期

### 4.1 Servlet生命周期概述

- 应用程序中的对象不仅在空间上有层次结构的关系，在时间上也会因为处于程序运行过程中的不同阶段而表现出不同状态和不同行为——这就是对象的生命周期。
- 简单的叙述生命周期，就是对象在容器中从开始创建到销毁的过程。

### 4.2 Servlet容器

Servlet对象是Servlet容器创建的，生命周期方法都是由容器调用的。这一点和我们之前所编写的代码有很大不同。在今后的学习中我们会看到，越来越多的对象交给容器或框架来创建，越来越多的方法由容器或框架来调用，开发人员要尽可能多的将精力放在业务逻辑的实现上。

### 4.3 Servlet生命周期的主要过程

![1560591043883](https://raw.githubusercontent.com/Young-Allen/pic/main/1560591043883.png)

#### ① Servlet对象的创建：构造器

- 默认情况下，**Servlet容器第一次收到HTTP请求时创建对应Servlet对象。**
- 容器之所以能做到这一点是由于我们在注册Servlet时提供了全类名，容器使用反射技术创建了Servlet的对象。

#### ② Servlet对象初始化：init()

- Servlet容器**创建Servlet对象之后，会调用init(ServletConfig config)**方法。
- 作用：是在Servlet对象创建后，执行一些初始化操作。例如，读取一些资源文件、配置文件，或建立某种连接（比如：数据库连接）
- init()方法只在创建对象时执行一次，以后再接到请求时，就不执行了
- 在javax.servlet.Servlet接口中，public void init(ServletConfig config)方法要求容器将ServletConfig的实例对象传入，这也是我们获取ServletConfig的实例对象的根本方法。

#### ③ 处理请求：service()

- 在javax.servlet.Servlet接口中，定义了**service(ServletRequest req, ServletResponse res)**方法处理HTTP请求。
- 在每次接到请求后都会执行。
- 上一节提到的Servlet的作用，主要在此方法中体现。
- 同时要求容器将ServletRequest对象和ServletResponse对象传入。

#### ④ Servlet对象销毁：destroy()

- 服务器重启、服务器停止执行或Web应用卸载时会销毁Servlet对象，会调用public void destroy()方法。
- 此方法用于销毁之前执行一些诸如释放缓存、关闭连接、保存内存数据持久化等操作。

### 4.4 Servlet请求过程

- 第一次请求
  - 调用构造器，创建对象
  - 执行init()方法
  - 执行service()方法
- 后面请求
  - 执行service()方法
- 对象销毁前
  - 执行destroy()方法

## 5 Servlet的两个接口

官方API中声明如下：

![1560591446414](https://raw.githubusercontent.com/Young-Allen/pic/main/1560591446414.png)

### 5.1 ServletConfig接口

![image-20220829222928794](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222928794.png)

- **ServletConfig接口封装了Servlet配置信息**，这一点从接口的名称上就能够看出来。

- **每一个Servlet都有一个唯一对应的ServletConfig对象**，代表当前Servlet的配置信息。

- 对象由Servlet容器创建，并传入生命周期方法init(ServletConfig config)中。可以直接获取使用。

- 代表当前Web应用的ServletContext对象也封装到了ServletConfig对象中，使ServletConfig对象成为了获取ServletContext对象的一座桥梁。

- ServletConfig对象的主要功能

  - **获取Servlet名称：getServletName()**

  - **获取全局上下文ServletContext对象：getServletContext()**

  - **获取Servlet初始化参数：getInitParameter(String) / getInitParameterNames()。**

  - 使用如下：

    ![1560581763744](https://raw.githubusercontent.com/Young-Allen/pic/main/1560581763744.png)

    ```java
    通过String info = config.getInitParameter("url");的方式获取value值
    ```

    

### 5.2 ServletContext接口

![1557750386422](https://raw.githubusercontent.com/Young-Allen/pic/main/1557750386422.png)

- Web容器在启动时，它会为**每个Web应用程序都创建一个唯一对应的ServletContext对象**，意思是Servlet上下文，**代表当前Web应用。**

- 由于**一个Web应用程序中的所有Servlet都共享同一个ServletContext对象**，所以ServletContext对象也被称为 application 对象（Web应用程序对象）。

- **对象由Servlet容器在项目启动时创建**，通过ServletConfig对象的getServletContext()方法获取。在项目卸载时销毁。

- ServletContext对象的主要功能

  ① 获取项目的上下文路径(带/的项目名):  **getContextPath()**

  ```java
  @Override
  public void init(ServletConfig config) throws ServletException {
  	ServletContext application = config.getServletContext();
  	System.out.println("全局上下文对象："+application);
  	String path = application.getContextPath();
  	System.out.println("全局上下文路径："+path);// /06_Web_Servlet
  }
  ```

  ② 获取虚拟路径所映射的本地真实路径：**getRealPath(String path)**

  - 虚拟路径：浏览器访问Web应用中资源时所使用的路径。

  - 本地路径：资源在文件系统中的实际保存路径。

  - 作用：将用户上传的文件通过流写入到服务器硬盘中。

    ```java
    @Override
    public void init(ServletConfig config) throws ServletException {
    	//1.获取ServletContext对象
    	ServletContext context = config.getServletContext();
    	//2.获取index.html的本地路径
    	//index.html的虚拟路径是“/index.html”,其中“/”表示当前Web应用的根目录，
    	//即WebContent目录
    	String realPath = context.getRealPath("/index.html");
    	//realPath=D:\DevWorkSpace\MyWorkSpace\.metadata\.plugins\
    	//org.eclipse.wst.server.core\tmp0\wtpwebapps\MyServlet\index.html
    	System.out.println("realPath="+realPath);
    }
    ```

  ③ 获取WEB应用程序的全局初始化参数（基本不用）

  - 设置Web应用初始化参数的方式是在web.xml的根标签下加入如下代码

    ```xml
    <web-app>
    	<!-- Web应用初始化参数 -->
    	<context-param>
    		<param-name>ParamName</param-name>
    		<param-value>ParamValue</param-value>
    	</context-param>
    </web-app>
    ```

  - 获取Web应用初始化参数

    ```java
    @Override
    public void init(ServletConfig config) throws ServletException {
    	//1.获取ServletContext对象
    	ServletContext application = config.getServletContext();
    	//2.获取Web应用初始化参数
    	String paramValue = application.getInitParameter("ParamName");
    	System.out.println("全局初始化参数paramValue="+paramValue);
    }
    ```

  ④ 作为域对象共享数据

  - 作为最大的域对象在整个项目的不同web资源内共享数据。

  ![1557992888615](https://raw.githubusercontent.com/Young-Allen/pic/main/1557992888615.png)

  其中，

  - setAttribute(key,value)：以后可以在任意位置取出并使用
  - getAttribute(key)：取出设置的value值

## 6 Servlet技术体系

### 6.1 Servlet接口

![1557677292072](https://raw.githubusercontent.com/Young-Allen/pic/main/1557677292072.png)

### 6.2 Servlet接口的常用实现类

![image-20220829222944264](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222944264.png)

- 为什么要扩展Servlet接口？
  - 封装不常用方法
- 实现类体系
  - GenericServlet实现Servlet接口
  - HttpServlet继承GenericServlet
- 创建Servlet的最终方式
  - 继承HttpServlet

![image-20220829222954555](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829222954555.png)

#### 6.2.1 GenericServlet抽象类

![image-20220829223003769](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223003769.png)

- GenericServlet对Servlet功能进行了封装和完善，重写了init(ServletConfig config)方法，用来获取 ServletConfig对象。此时如果GenericServlet的子类（通常是自定义Servlet）又重写了init(ServletConfig config)方法有可能导致ServletConfig对象获取不到，所以子类不应该重写带参数的这个init()方法。
- 如果想要进行初始化操作，可以重写GenericServlet提供的无参的init()方法，这样就不会影响ServletConfig对象的获取。
- 将service(ServletRequest req,ServletResponse res)保留为抽象方法，让使用者仅关心业务实现即可。


![image-20220829223010911](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223010911.png)

#### 6.2.2 HttpServlet抽象类

![image-20220829223022452](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223022452.png)

![1557677406592](https://raw.githubusercontent.com/Young-Allen/pic/main/1557677406592.png)

- 专门用来处理Http请求的Servlet。

- 对GenericServlet进行进一步的封装和扩展，在service(ServletRequest req, ServletResponse res)方法中，将ServletRequest和ServletResponse转换为HttpServletRequest和HttpServletResponse，根据不同HTTP请求类型调用专门的方法进行处理。

- **今后在实际使用中继承HttpServlet抽象类创建自己的Servlet实现类即可。**重写doGet(HttpServletRequest req, HttpServletResponse resp)和doPost(HttpServletRequest req, HttpServletResponse resp)方法实现请求处理，不再需要重写service(ServletRequest req, ServletResponse res)方法了。

- 又因为我们业务中get，post的处理方式又都是一样的，所以我们只需要写一种方法即可，使用另外一种方法调用我们写好的doXXX方法。web.xml配置与之前相同。

  ```java
  //处理浏览器的get请求
  doGet(HttpServletRequest request, HttpServletResponse response){
  	//业务代码
  }
  //处理浏览器的post请求
  doPost(HttpServletRequest request, HttpServletResponse response){
      doGet(request, response);
  }
  ```




## 7 处理请求响应两个重要的接口

### 7.1 HttpServletRequest接口

- 该接口是ServletRequest接口的子接口，封装了HTTP请求的相关信息。

- 浏览器请求服务器时会封装请求报文交给服务器，服务器接受到请求会将请求报文解析生成request对象。
- 由Servlet容器创建其实现类对象并传入service(HttpServletRequest req, HttpServletResponse res)方法中。

- 以下我们所说的HttpServletRequest对象指的是容器提供的HttpServletRequest实现类对象。

HttpServletRequest对象的主要功能有：

#### 7.1.1 获取请求参数

- 什么是请求参数？

  - 请求参数就是浏览器向服务器提交的数据。

- 浏览器向服务器如何发送数据？

  ① 附在url后面(和get请求一致，拼接的形式就行请求数据的绑定)，如：

  [http](http://localhost:8989/MyServlet/MyHttpServlet?userId=20)[://](http://localhost:8989/MyServlet/MyHttpServlet?userId=20)[localhost:8080/MyServlet/MyHttpServlet?userId=20](http://localhost:8080/MyServlet/MyHttpServlet?userId=20)

  ② 通过表单提交

  ```html
  <form action="MyHttpServlet" method="post">
  	你喜欢的足球队<br /><br />
  	巴西<input type="checkbox" name="soccerTeam" value="Brazil" />
  	德国<input type="checkbox" name="soccerTeam" value="German" />
  	荷兰<input type="checkbox" name="soccerTeam" value="Holland" />
  	<input type="submit" value="提交" />
  </form>
  ```
  
- 使用HttpServletRequest对象获取请求参数

  ```java
  //一个name对应一个值
  String userId = request.getParameter("userId");
  ```

  ```java
  //一个name对应一组值
  String[] soccerTeams = request.getParameterValues("soccerTeam");
  for(int i = 0; i < soccerTeams.length; i++){
  	System.out.println("team "+i+"="+soccerTeams[i]);
  }
  ```

#### 7.1.2 获取url地址参数

```java
String path = request.getContextPath();//重要
System.out.println("上下文路径："+path);
System.out.println("端口号："+request.getServerPort());
System.out.println("主机名："+request.getServerName());
System.out.println("协议："+request.getScheme());
```

#### 7.1.3 获取请求头信息

![image-20220829223037898](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223037898.png)

```java
String header = request.getHeader("User-Agent");
System.out.println("user-agent:"+header);
String referer = request.getHeader("Referer");
System.out.println("上个页面的地址："+referer);//登录失败，返回登录页面让用户继续登录
```

#### 7.1.4 请求的转发

将请求转发给另外一个URL地址，参见第8节-请求的转发与重定向。

```java
//获取请求转发对象
RequestDispatcher dispatcher = request.getRequestDispatcher("success.html");
dispatcher.forward(request, response);//发起转发
```

#### 7.1.5 向请求域中保存数据

```java
//将数据保存到request对象的属性域中
request.setAttribute("attrName", "attrValueInRequest");
//两个Servlet要想共享request对象中的数据，必须是转发的关系
request.getRequestDispatcher("/ReceiveServlet").forward(request, response);

```

```java
//从request属性域中获取数据
Object attribute = request.getAttribute("attrName");
System.out.println("attrValue="+attribute);
```

### 7.2 HttpServletResponse接口

- 该接口是ServletResponse接口的子接口，封装了服务器针对于HTTP响应的相关信息。(暂时只有服务器的配置信息，没有具体的和响应体相关的内容)

- 由Servlet容器创建其实现类对象，并传入service(HttpServletRequest req, HttpServletResponse res)方法中。

- 后面我们所说的HttpServletResponse对象指的是容器提供的HttpServletResponse实现类对象。

HttpServletResponse对象的主要功能有：

#### 7.2.1 使用PrintWriter对象向浏览器输出数据

```java
//通过PrintWriter对象向浏览器端发送响应信息
PrintWriter writer = res.getWriter();
writer.write("Servlet response");
writer.close();
```

- 写出的数据可以是页面、页面片段、字符串等

- 当写出的数据包含中文时，浏览器接收到的响应数据就可能有乱码。为了避免乱码，可以使用Response对象在向浏览器输出数据前设置响应头。

#### 7.2.2 设置响应头

- 响应头就是浏览器解析页面的配置。比如：告诉浏览器使用哪种编码和文件格式解析响应体内容

```java
response.setHeader("Content-Type", "text/html;charset=UTF-8");
```

- 设置好以后，会在浏览器的响应报文中看到设置的响应头中的信息。

#### 7.2.3重定向请求

- 实现请求重定向，参见第8章-请求的转发与重定向。
- 举例：用户从login.html页面提交登录请求数据给LoginServlet处理。如果账号密码正确，需要让用户跳转到成功页面，通过servlet向响应体中写入成功页面过于复杂，通过重定向将成功页面的地址交给浏览器并设置响应状态码为302，浏览器会自动进行跳转。

```java
//注意路径问题，加上/会失败，会以主机地址为起始，重定向一般需要加上项目名
response.sendRedirect(“success.html”);
```



## 8 请求的转发与重定向

请求的转发与重定向是web应用页面跳转的主要手段，在Web应用中使用非常广泛。所以我们一定要搞清楚他们的区别。

![1562000421414](https://raw.githubusercontent.com/Young-Allen/pic/main/1562000421414.png)

### 8.1 请求的转发

![1557754164834](https://raw.githubusercontent.com/Young-Allen/pic/main/1557754164834.png)

- 第一个Servlet接收到了浏览器端的请求，进行了一定的处理，然后没有立即对请求进行响应，而是将请求“交给下一个Servlet”继续处理，下一个Servlet处理完成之后对浏览器进行了响应。**在服务器内部将请求“交给”其它组件继续处理就是请求的转发。**对浏览器来说，一共只发了一次请求，服务器内部进行的“转发”浏览器感觉不到，同时浏览器地址栏中的地址不会变成“下一个Servlet”的虚拟路径。
- HttpServletRequest代表HTTP请求，对象由Servlet容器创建。转发的情况下，两个Servlet可以共享同一个Request对象中保存的数据。
- 当需要将后台获取的数据传送到JSP上显示的时候，就可以先将数据存放到Request对象中，再转发到JSP从属性域中获取。此时由于是“转发”，所以它们二者共享Request对象中的数据。
- 转发的情况下，可以访问WEB-INF下的资源。
- **转发以“/”开始表示项目根路径，重定向以”/”开始表示主机地址。**
- 功能：
  - 获取请求参数
  - 获取请求路径即URL地址相关信息
  - 在请求域中保存数据
  - 转发请求
- 代码举例

```java
protected void doGet(HttpServletRequest request,HttpServletResponse response) throws ServletException, IOException {
	//1.使用RequestDispatcher对象封装目标资源的虚拟路径
	RequestDispatcher dispatcher = request.getRequestDispatcher("/index.html");
	//2.调用RequestDispatcher对象的forward()方法“前往”目标资源
	//[注意：传入的参数必须是传递给当前Servlet的service方法的
	//那两个ServletRequest和ServletResponse对象]
	dispatcher.forward(request, response);
}

```

### 8.2 请求的重定向

![1557754122187](https://raw.githubusercontent.com/Young-Allen/pic/main/1557754122187.png)

- 第一个Servlet接收到了浏览器端的请求，进行了一定的处理，然后给浏览器一个特殊的响应消息，这个特殊的响应消息会通知浏览器去访问另外一个资源，这个动作是服务器和浏览器自动完成的。**整个过程中浏览器端会发出两次请求**，且在**浏览器地址栏里面能够看到地址的改变**，改变为下一个资源的地址。

- 重定向的情况下，原Servlet和目标资源之间就不能共享请求域数据了。

- HttpServletResponse代表HTTP响应，对象由Servlet容器创建。

- 功能：

  - 向浏览器输出数据
  - 重定向请求

- 重定向的响应报文的头

  ```
  HTTP/1.1 302 Found
  Location: success.html
  ```

- 应用：

  - 用户从login.html页面提交登录请求数据给LoginServlet处理。

    如果账号密码正确，需要让用户跳转到成功页面，通过servlet向响应体中写入成功页面过于复杂，通过重定向将成功页面的地址交给浏览器并设置响应状态码为302，浏览器会自动进行跳转

- 代码举例：

```java
protected void doGet(HttpServletRequest request,HttpServletResponse response) throws ServletException, IOException {
	//1.调用HttpServletResponse对象的sendRedirect()方法
	//2.传入的参数是目标资源的虚拟路径
	response.sendRedirect("index.html");
}
```

### 8.3 对比请求的转发与重定向

|                         | 转发                             | 重定向                                              |
| ----------------------- | -------------------------------- | --------------------------------------------------- |
| 浏览器感知              | 在服务器内部完成，浏览器感知不到 | 服务器以302状态码通知浏览器访问新地址，浏览器有感知 |
| 浏览器地址栏            | 不改变                           | 改变                                                |
| 整个过程发送请求次数    | 一次                             | 两次                                                |
| 能否共享request对象数据 | 能                               | 否                                                  |
| WEB-INF下的资源         | 能访问                           | 不能访问                                            |
| 目标资源                | 必须是当前web应用中的资源        | 不局限于当前web应用                                 |

> 说明1：默认情况下，浏览器是不能访问服务器web-inf下的资源的，而服务器是可以访问的。
>
> 说明2：浏览器默认的绝对路径：http://localhost:8080/
>
> ​              服务器项目的代码中的绝对路径：http://localhost:8080/项目名/

## 9 请求与响应中的字符编码设置

### 9.1 字符编码问题

- 我们web程序在接收请求并处理过程中，如果不注意编码格式及解码格式，很容易导致中文乱码，引起这个问题的原因到底在哪里？如何解决？我们这个小节将会讨论此问题。
- 说到这个问题我们先来说一说字符集。
  - 什么是字符集，就是各种字符的集合，包括汉字，英文，标点符号等等。各国都有不同的文字、符号。这些文字符号的集合就叫字符集。
  - 现有的字符集ASCII、GB2312、BIG5、GB18030、Unicode、UTF-8、ISO-8859-1等
- 这些字符集，集合了很多的字符，然而，字符要以二进制的形式存储在计算机中，我们就需要对其进行编码，将编码后的二进制存入。取出时我们就要对其解码，将二进制解码成我们之前的字符。这个时候我们就需要制定一套编码解码标准。否则就会导致出现混乱，也就是我们的乱码。

### 9.2 编码与解码

- 编码：将字符转换为二进制数

| 汉字 | 编码方式   | 编码       | 二进制                                         |
| ---- | ---------- | ---------- | ---------------------------------------------- |
| ‘中' | **GB2312** | **D6D0**   | **1101 0110-1101 0000**                        |
| ‘中' | **UTF-16** | **4E2D**   | **0100 1110-0010 1101**                        |
| ‘中' | **UTF-8**  | **E4B8AD** | **1110** **0100-** **1011** **1000-1010 1101** |

- 解码：将二进制数转换为字符

1110 0100-1011 1000-1010 1101 → E4B8AD → ’中’

- 乱码：一段文本，使用A字符集编码，使用B字符集解码，就会产生乱码。所以解决乱码问题的根本方法就是统一编码和解码的字符集。

### 9.3 解决请求乱码问题

解决乱码的方法：就是统一字符编码。

![1558009756944](https://raw.githubusercontent.com/Young-Allen/pic/main/1558009756944.png)

#### 9.3.1 GET请求（Tomcat7及以下的需要处理）

- GET请求参数是在地址后面的。我们需要修改tomcat的配置文件。需要在server.xml文件修改Connector标签，添加URIEncoding="utf-8"属性。

![image-20220829223106330](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223106330.png)

- 一旦配置好以后，可以解决当前工作空间中所有的GET请求的乱码问题。

#### 9.3.2 POST请求

- post请求提交了中文的请求体，服务器解析出现问题。

- 解决方法：在获取参数值之前，设置请求的解码格式，使其和页面保持一致。

  ```java
  request.setCharacterEncoding("utf-8");
  ```

- POST请求乱码问题的解决，只适用于当前的操作所在的类中。不能类似于GET请求一样统一解决。因为请求体有可能会上传文件。不一定都是中文字符。

### 9.4 解决响应乱码问题

- 向浏览器发送响应的时候，要告诉浏览器，我使用的字符集是哪个，浏览器就会按照这种方式来解码。如何告诉浏览器响应内容的字符编码方案。很简单。

- 解决方法一：

  ```java
  response.setHeader("Content-Type", "text/html;charset=utf-8");
  ```

- 解决方法二

  ```java
  response.setContentType("text/html;charset=utf-8");
  ```

  > 说明：有的人可能会想到使用response.setCharacterEncoding(“utf-8”)，设置reponse对象将UTF-8字符串写入到响应报文的编码为UTF-8。只这样做是不行的，还必须手动在浏览器中设置浏览器的解析用到的字符集。

## 10 Web应用路径设置

### 10.1url的概念

​	url是`uniform Resource Locater`的简写，中文翻译为`统一资源定位符`，它是某个互联网资源的唯一访问地址，客户端可以通过url访问到具体的互联网资源

完整的url构成如下图：

![image-20220829223114703](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223114703.png)

**相对路径和绝对路径**

**相对路径：虚拟路径如果不以“/”开始，就是相对路径**，浏览器会以当前资源所在的虚拟路径为基准对相对路径进行解析，从而生成最终的访问路径。此时如果通过转发进入其他目录，再使用相对路径访问资源就会出错。所以为了防止路径出错，我们经常将相对路径转化为绝对路径的形式进行请求。

**绝对路径：虚拟路径以“/”开始，就是绝对路径。**
**① 在服务器端**：虚拟路径最开始的“/”表示当前Web应用的根目录。只要是服务端解析的绝对路径，都是以web根目录为起始的。由服务器解析的路径包括：<1> web.xml的配置路径、<2>request转发的路径。
**② 在浏览器端**：虚拟路径最开始的“/”表示当前主机地址。
例如：链接地址“/Path/dir/b.html”经过浏览器解析后为：
相当于http://localhost:8989/Path/dir/b.html
由浏览器解析的路径包括：
<1>重定向操作：response.sendRedirect("/xxx")
<2>所有HTML标签：\<a href="/xxx"> 、\<form action="/xxx"> 、link、img、script等
这些最后的访问路径都是 http://localhost:8989/xxx

所以我们可以看出，如果是浏览器解析的路径，我们必须加上项目名称才可以正确的指向资源。[http://localhost:8989](http://localhost:8989/Path/xxx)[/Path](http://localhost:8989/Path/xxx)[/xxx](http://localhost:8989/Path/xxx)

<1>重定向操作：

```java
response.sendRedirect(request.getContextPath()+"/xxx");
```

<2>所有HTML标签：\<a href="/项目名/xxx">；  \<form action=“/项目名/xxx">

- 在浏览器端，除了使用绝对路径之外，我们还可以使用base标签+相对路径的方式来确定资源的访问有效。
- base标签影响当前页面中的所有相对路径，不会影响绝对路径。相当于给相对路径设置了一个基准地址。
- 习惯上在html的<head>标签内，声明：

```html
<!-- 给页面中的相对路径设置基准地址 -->
<base href="http://localhost:8080/Test_Path/"/>
```

接着html中的路径就可以使用相对路径的方式来访问。比如：

```html
<h4> base+相对路径</h4>
<!-- <base href="http://localhost:8080/Test_Path/"/> -->
<a href="1.html">1.html</a><br/>
<a href="a/3.html">a/3.html</a><br/>
<!-- servlet映射到了项目根目录下，可以直接访问 -->
<a href="PathServlet">PathServlet</a><br/>
```

## 作业

### 1. 简答题

- Servlet的作用
- 原生的Servlet的创建步骤
- Servlet的生命周期方法
- 理解doGet()方法是如何被调用的
- 转发和重定向的区别
- 请求乱码和响应乱码产生的原因及解决办法
- Web项目中如何使用绝对路径，base标签如何使用



### 2. 编程题

#### 功能1：登录

说明:  

1. 系统中只有一个用户(用户名: admin,密码: 123456)

2. 相关资源:

   login.html : 登录表单页面

   LoginServlet: 处理登录请求的Servlet

   login_success.html : 登录成功页面(提示: 登录成功)

   login_error.html : 登录失败页面(提示: 用户名或密码不正确)

#### 功能2：注册

说明:  

1. 系统中只有一个用户(用户名: admin,密码: 123456)

2. 相关资源:

   register.html : 注册表单页面

   RegistServlet: 处理注册请求的Servlet

   regist_success.html :注册成功页面(提示:注册成功)

   regist_error.html :注册失败页面(提示: 用户名已存在)  



# 第三章 书城项目第二阶段

## 1. 注册登录准备工作

### 1.1 实现步骤

1. 创建动态Web工程
2. 将第一版书城的静态资源拷贝到web文件夹中
3. 统一页面的基础访问路径


### 1.2 内容讲解

#### 1.2.1 创建动态Web工程

![image-20220829223155734](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223155734.png)

#### 1.2.2 拷贝静态资源

![images](https://raw.githubusercontent.com/Young-Allen/pic/main/img002.png)

#### 1.2.3 在HTML中使用base标签统一页面基础访问路径

##### ①  为什么要使用base标签统一页面基础访问路径

因为在页面中有很多的a标签、表单以及Ajax请求(以后会学)都需要写访问路径，而在访问路径中**项目路径**是一样的，所以如果不统一编写**项目路径**的话，就会发生当项目路径发生改变的时候该页面所有用到项目路径的地方都需要更改的情况

##### ② base标签的语法规则

- base标签要写在head标签内
- base标签必须写在所有其他有路径的标签的前面
- base标签使用href属性设置路径的基准
- base标签生效的机制是：最终的访问地址=base标签href属性设置的基准+具体标签内的路径
- 如果某个路径想要基于base中的路径进行路径编写，那么它不能以`/`开头

##### ③ base标签使用举例

```html
<head>
    <meta charset="UTF-8"/>
    <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
    <title>书城首页</title>
    <base href="/bookstore/"/>
    <link rel="stylesheet" href="static/css/minireset.css"/>
    <link rel="stylesheet" href="static/css/common.css"/>
    <link rel="stylesheet" href="static/css/iconfont.css"/>
    <link rel="stylesheet" href="static/css/index.css"/>
    <link rel="stylesheet" href="static/css/swiper.min.css"/>
</head>
```

##### ④ 基于base标签调整整个页面的路径

在需要进行基础路径统一的页面做如下修改

 base标签的代码

```html
<base href="/bookstore/"/>
```

 对需要统一调整的路径执行替换

调出替换操作窗口，并做如下替换

![image-20220829223206013](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223206013.png)

## 2. 完成带数据库的注册登录

### 2.1 学习目标

* 了解三层架构
* 了解MD5加密
* 完成带数据库的登录校验
* 完成带数据库的注册功能

### 2.2 内容讲解

#### 2.2.1 三层架构

##### ① 为什么要使用三层架构

如果不做三层架构形式的拆分：

![image-20220829223217069](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223217069.png)

所有和当前业务功能需求相关的代码全部耦合在一起，如果其中有任何一个部分出现了问题，牵一发而动全身，导致其他无关代码也要进行相应的修改。这样的话代码会非常难以维护。

所以为了提高开发效率，需要对代码进行模块化的拆分。整个项目模块化、组件化程度越高，越容易管理和维护，出现问题更容易排查。

##### ② 三层架构的划分

![image-20220829223225887](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223225887.png)

- 表述层：又可以称之为控制层，负责处理浏览器请求、返回响应、页面调度
- 业务逻辑层：负责处理业务逻辑，根据业务逻辑把持久化层从数据库查询出来的数据进行运算、组装，封装好后返回给表述层，也可以根据业务功能的需要调用持久化层把数据保存到数据库、修改数据库中的数据、删除数据库中的数据
- 持久化层：根据上一层的调用对数据库中的数据执行增删改查的操作

##### ③ 三层架构和数据模型的关系

![image-20220829223233264](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223233264.png)

模型对整个项目中三层架构的每一层都提供支持，具体体现是使用模型对象<span style="color:blue;font-weight:bold;">封装业务功能数据</span>

其实数据模型就是我们之前学习的JavaBean，也是Java实体类，当然他还有很多其他的名称:

- POJO：Plain old Java Object，传统的普通的Java对象
- entity：实体类
- bean或Java bean
- domain：领域模型

#### 2.2.2 持久层

##### ① 数据建模

a. 创建数据库和表

```sql
CREATE DATABASE bookstore CHARACTER SET utf8;
USE `bookstore`;
CREATE TABLE users(
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(100),
    password VARCHAR(100),
    email VARCHAR(100)
);
```

b. 创建JavaBean

![images](https://raw.githubusercontent.com/Young-Allen/pic/main/img011.png)



```java
package com.atguigu.bean;

public class User {
    private int id;
    private String username;
    private String password;
    private String email;

    public User() {
    }

    public User(int id, String username, String password, String email) {
        this.id = id;
        this.username = username;
        this.password = password;
        this.email = email;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}

```

##### ② 加入所需jar包

![image-20220829223253284](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223253284.png)

##### ③ 创建外部属性文件

在idea的工程结构中，我们通常将配置文件放在resources目录下

![image-20220829223307166](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223307166.png)

1. 在当前module中创建一个directory，并且命名为resources
2. 然后将这个目录标记为Resources Root

![image-20220829223315083](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223315083.png)

3. 编写jdbc.properties文件

```properties
driverClassName=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/bookstore
username=root
password=root
initialSize=10
maxActive=20
maxWait=10000
```

##### ④ 加密方式介绍

a. 加密方式介绍

- 对称加密：加密和解密使用的相同的密钥，常见的对称加密算法有:DES、3DES
- 非对称加密：加密和解密使用的密钥不同，常见的非对称加密算法有:RSA
  - 加密：使用私钥加密
  - 解密：使用公钥解密
- 消息摘要:  消息摘要算法的主要特征是加密过程不需要密钥，并且经过加密的数据无法被解密，只有相同的原文经过消息摘要算法之后，才能得到相同的密文，所以消息摘要通常用来校验原文的真伪。常用的消息摘要算法有:MD5、SHA、MAC

我们在书城项目中采用MD5算法对密码进行加密

b. 封装执行加密的工具类(直接复制过去)

```java
public class MD5Util {

    /**
     * 针对明文字符串执行MD5加密
     * @param source
     * @return
     */
    public static String encode(String source) {

        // 1.判断明文字符串是否有效
        if (source == null || "".equals(source)) {
            throw new RuntimeException("用于加密的明文不可为空");
        }

        // 2.声明算法名称
        String algorithm = "md5";

        // 3.获取MessageDigest对象
        MessageDigest messageDigest = null;
        try {
            messageDigest = MessageDigest.getInstance(algorithm);
        } catch (NoSuchAlgorithmException e) {
            e.printStackTrace();
        }

        // 4.获取明文字符串对应的字节数组
        byte[] input = source.getBytes();

        // 5.执行加密
        byte[] output = messageDigest.digest(input);

        // 6.创建BigInteger对象
        int signum = 1;
        BigInteger bigInteger = new BigInteger(signum, output);

        // 7.按照16进制将bigInteger的值转换为字符串
        int radix = 16;
        String encoded = bigInteger.toString(radix).toUpperCase();

        return encoded;
    }
}
```

##### ⑤ 创建连接数据库的工具类

```java
package com.atguigu.util;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.util.Properties;

public class JDBCUtils {
    private static DataSource dataSource =null;
    private static ThreadLocal<Connection> threadLocal = new ThreadLocal<>();
    //创建连接池
    static{
        //读取属性文件
        Properties prop = new Properties();
        //System.out.println("prop1:"+prop);
        InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("db.properties");
        try {
            prop.load(is);
            //System.out.println("prop2:"+prop);
            //根据属性文件创建连接池
            dataSource = DruidDataSourceFactory.createDataSource(prop);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    //获取数据库连接
    public static Connection getConnection(){
        //先从ThreadLocal中获取
        Connection conn = threadLocal.get();
        //如果没有连接，说明是该线程中第一次访问，
        if(conn ==null ){
            try {
                //从连接池中获取一个连接
                conn = dataSource.getConnection();
                //放入到threadLocal中
                threadLocal.set(conn);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        //返回连接
        return conn;
    }
    //关闭数据库连接（如果采用了连接池，就是归还连接）
    public static void releaseConnection(){
        //从threadLocal中获取
        Connection conn = threadLocal.get();
        try {
            if(conn !=null){
                conn.close(); //不是物理关闭，而是放入到连接池中，置为空闲状态
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //这个语句不要少
            //threadLocal.set(null);//连接已经放回连接池，不使用了。ThreadLocal也不需要再保存了
            threadLocal.remove();
        }
    }
}

```

##### ⑥ 创建BaseDao

```java
package com.atguigu.dao;

import com.atguigu.util.JDBCUtils;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;
import org.apache.commons.dbutils.handlers.ScalarHandler;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;

/**
 * 功能：对数据库的任意表格进行增删改查
 *      ① 增删改
 *      ② 三个查询
 */
public class BaseDao<T> {
    //2. 创建QueryRunner对象
    private QueryRunner runner=new QueryRunner();
    /**
     * 功能：对数据库进行增删改的操作
     * @param sql
     * @param params
     * @return
     */
    public boolean update(String sql,Object...params){
        //1. 获得数据库连接
        Connection connection = JDBCUtils.getConnection();
        //3. 执行
        try {
            int update = runner.update(connection, sql, params);
            if(update>0)
                return true;
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            //4. 释放资源
            JDBCUtils.releaseConnection();
        }
        return false;
    }

    /**
     * 功能：查询多条数据
     * @param type
     * @param sql
     * @param params
     * @return
     */
    public List<T> getBeanList(Class type,String sql,Object...params){
        //1. 获得数据库连接
        Connection connection = JDBCUtils.getConnection();
        //3. 执行
        try {
            return runner.query(connection, sql, new BeanListHandler<T>(type), params);
        } catch (SQLException e) {
            e.printStackTrace();
        }finally{
            //4. 释放资源
            JDBCUtils.releaseConnection();
        }
        return null;
    }

    /**
     * 功能：查询一条结果
     * @param type
     * @param sql
     * @param params
     * @return
     */
    public T getBean(Class type,String sql,Object...params){
        //1. 获得数据库连接
        Connection connection = JDBCUtils.getConnection();
        //3. 执行
        try {
            return runner.query(connection,sql,new BeanHandler<T>(type),params);
        } catch (SQLException e) {
            e.printStackTrace();
        }finally{
            //4. 释放资源
            JDBCUtils.releaseConnection();
        }
        return null;
    }

    /**
     * 功能：查询一个结果
     * @param sql
     * @param params
     * @return
     */
    public Object getObject(String sql,Object...params){
        //1. 获得数据库连接
        Connection connection = JDBCUtils.getConnection();
        //3. 执行
        try {
            return runner.query(connection,sql,new ScalarHandler(),params);
        } catch (SQLException e) {
            e.printStackTrace();
        }finally{
            //4. 释放资源
            JDBCUtils.releaseConnection();
        }
        return null;
    }
}

```

##### ⑦ 完成登录和注册的业务需求

a. 登录页面

```html
<!DOCTYPE html>
<html>
  <head>
    <base href="/BookStore02_war_exploded/">
    <meta charset="UTF-8" />
    <title>尚硅谷会员登录页面</title>
    <link type="text/css" rel="stylesheet" href="static/css/style.css" />
    <script src="static/script/vue.js"></script>
  </head>
  <body>
  <div id="app">
    <div id="login_header">
      <a href="index.html">
        <img class="logo_img" alt="" src="static/img/logo.gif" />
      </a>
    </div>

    <div class="login_banner">
      <div id="l_content">
        <span class="login_word">欢迎登录</span>
      </div>

      <div id="content">
        <div class="login_form">
          <div class="login_box">
            <div class="tit">
              <h1>尚硅谷会员</h1>
            </div>
            <div class="msg_cont">
              <b></b>
              <span class="errorMsg">{{errMsg}}</span>
            </div>
            <div class="form">
              <form action="login" @submit="checkLogin">
                <label>用户名称：</label>
                <input
                  class="itxt"
                  type="text"
                  placeholder="请输入用户名"
                  autocomplete="off"
                  tabindex="1"
                  name="username"
                  id="username"
                  v-model="username"
                />
                <br />
                <br />
                <label>用户密码：</label>
                <input
                  class="itxt"
                  type="password"
                  placeholder="请输入密码"
                  autocomplete="off"
                  tabindex="1"
                  name="password"
                  id="password"
                  v-model.trim="password"
                />
                <br />
                <br />
                <input type="submit" value="登录" id="sub_btn" />
              </form>
              <div class="tit">
                <a href="pages/user/regist.html">立即注册</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div id="bottom">
      <span>
        尚硅谷书城.Copyright &copy;2015
      </span>
    </div>
  </div>
  <script>
    new Vue({
      "el":"#app",
      "data":{
        "username":"",
        "password":"",
        "errMsg":"请输入用户名和密码"
      },
      "methods":{
        checkLogin(){
          if(this.username==""){
            this.errMsg = "用户名不能为空";
            event.preventDefault();
            return;
          }
          if(this.password==""){
            this.errMsg="密码不能为空";
            event.preventDefault();
          }
        }
      }
    });
  </script>
  </body>
</html>

```

b. LoginServlet

```java
package com.atguigu.servlet;

import com.atguigu.bean.User;
import com.atguigu.service.UserService;
import com.atguigu.service.impl.UserServiceImpl;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class LoginServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request,response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 获得数据
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        //2. 去验证
        UserService userService=new UserServiceImpl();
        User loginUser = userService.login(username, password);
        //3. 给响应
        if(loginUser!=null){
            response.sendRedirect(request.getContextPath()+"/pages/user/login_success.html");
        }else{
            response.sendRedirect(request.getContextPath()+"/pages/user/login.html");
        }
    }
}

```

c. 注册页面

```html
<!DOCTYPE html>
<html>
  <head>
    <base href="/BookStore02_war_exploded/">
    <meta charset="UTF-8" />
    <title>尚硅谷会员注册页面</title>
    <link type="text/css" rel="stylesheet" href="static/css/style.css" />
    <link rel="stylesheet" href="static/css/register.css" />
    <style type="text/css">
      .login_form {
        height: 420px;
        margin-top: 25px;
      }
    </style>
    <script src="static/script/vue.js"></script>
  </head>
  <body>
  <div id="app">
    <div id="login_header">
      <a href="index.html">
        <img class="logo_img" alt="" src="static/img/logo.gif" />
      </a>
    </div>

    <div class="login_banner">
      <div class="register_form">
        <h1>注册尚硅谷会员</h1>
        <form action="regist" @submit="checkAll">
          <div class="form-item">
            <div>
              <label>用户名称:</label>
              <input type="text" placeholder="请输入用户名" name="username" v-model="username" @blur="checkUsername"/>
            </div>
            <span class="errMess" :style="usernameCss">{{usernameErrMsg}}</span>
          </div>
          <div class="form-item">
            <div>
              <label>用户密码:</label>
              <input type="password" placeholder="请输入密码" name="password" v-model="password" @blur="checkPassword"/>
            </div>
            <span class="errMess" :style="passwordCss">{{passwordErrMsg}}</span>
          </div>
          <div class="form-item">
            <div>
              <label>确认密码:</label>
              <input type="password" placeholder="请输入确认密码" v-model="confirmPassword" @blur="checkConfirmPassword"/>
            </div>
            <span class="errMess" :style="confirmPasswordCss">{{confirmPasswordErrMsg}}</span>
          </div>
          <div class="form-item">
            <div>
              <label>用户邮箱:</label>
              <input type="text" placeholder="请输入邮箱" name="email" v-model="email" @blur="checkEmail"/>
            </div>
            <span class="errMess" :style="emailCss">{{emailErrMsg}}</span>
          </div>
          <div class="form-item">
            <div>
              <label>验证码:</label>
              <div class="verify">
                <input type="text" placeholder="" />
                <img src="static/img/code.bmp" alt="" />
              </div>
            </div>
            <span class="errMess"></span>
          </div>
          <button class="btn">注册</button>
        </form>
      </div>
    </div>
    <div id="bottom">
      <span>
        尚硅谷书城.Copyright &copy;2015
      </span>
    </div>
  </div>
  <script>
    new Vue({
      "el":"#app",
      "data":{
        "username":"",
        "usernameErrMsg":"用户名应为6~16位数组和字母组成",
        "usernameCss":{"visibility":"hidden"},
        "password":"",
        "passwordErrMsg":"密码的长度至少为8位",
        "passwordCss":{"visibility":"hidden"},
        "confirmPassword":"",
        "confirmPasswordErrMsg":"密码两次输入不一致",
        "confirmPasswordCss":{"visibility":"hidden"},
        "email":"",
        "emailErrMsg":"请输入正确的邮箱格式",
        "emailCss":{"visibility":"hidden"},
      },
      "methods":{
        checkUsername(){
          this.usernameCss={"visibility":"visible"}
          var reg=/^[a-zA-Z0-9]{6,16}$/;
          if(reg.test(this.username)){
            this.usernameErrMsg="√";
            return true;
          }else{
            this.usernameErrMsg="用户名应为6~16位数组和字母组成"
            return false;
          }
        },
        checkPassword(){
          this.passwordCss={"visibility":"visible"};
          var reg=/^[a-zA-Z0-9]{8,}$/;
          if(reg.test(this.password)){
            this.passwordErrMsg="√";
            return true;
          }else{
            this.passwordErrMsg="密码的长度至少为8位";
            return false;
          }
        },
        checkConfirmPassword(){
          this.confirmPasswordCss={"visibility":"visible"};
          if(this.confirmPassword==""){
            this.confirmPasswordErrMsg="密码两次输入不一致";
            return false;
          }
          if(this.password==this.confirmPassword){
            this.confirmPasswordErrMsg="√";
            return true;
          }else{
            this.confirmPasswordErrMsg="密码两次输入不一致";
            return false;
          }
        },
        checkEmail(){
          this.emailCss={"visibility":"visible"};
          var reg=/^[a-zA-Z0-9_\.-]+@([a-zA-Z0-9-]+[\.]{1})+[a-zA-Z]+$/;
          if(reg.test(this.email)){
            this.emailErrMsg="√"
            return true;
          }else{
            this.emailErrMsg="请输入正确的邮箱格式"
            return false;
          }
        },
        checkAll(){
          if(!(this.checkUsername()&this.checkPassword()&this.checkConfirmPassword()&this.checkEmail())){
            event.preventDefault()
          }
        }
      }
    })
  </script>
  </body>
</html>

```

d. RegistServlet

```java
package com.atguigu.servlet;

import com.atguigu.bean.User;
import com.atguigu.service.impl.UserServiceImpl;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.InvocationTargetException;
import java.util.Map;

public class RegistServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request,response);
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 获得请求参数(注册的数据)
        Map<String, String[]> parameterMap = request.getParameterMap();
        User user=new User();
        try {
            BeanUtils.populate(user,parameterMap);
        } catch (Exception e) {
            e.printStackTrace();
        }
        //2. 执行注册的业务
        boolean regist = new UserServiceImpl().regist(user);
        //3. 给响应
        if(regist){
            response.sendRedirect(request.getContextPath()+"/pages/user/regist_success.html");
        }else{
            response.sendRedirect(request.getContextPath()+"/pages/user/regist.html");
        }

    }
}

```



# 第四章 Thymeleaf

## 学习目标

* 掌握MVC
* 了解Thymeleaf的简介
* 掌握引入Thymeleaf
* 掌握Thymeleaf的入门案例
* 掌握th名称空间
* 掌握表达式语法 
* 掌握访问域对象
* 获取请求参数
* 掌握内置对象
* 掌握OGNL表达式
* 掌握分支和迭代
* 掌握使用Thymeleaf包含其它文件
* 使用Thymeleaf练习CRUD

### 1. 内容讲解

#### 1.1 MVC

##### 1.1.1 为什么需要MVC

曾经编写过下面这段代码

![image-20220829223344341](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223344341.png)

这段代码虽然说可以实现在登录失败之后跳转回到登录页面，并且展示失败信息，但是代码实在是太恶心了，根本没法维护，所以我们需要将视图展示抽取出来，单独作为一个View视图层

但是我们如果只使用HTML作为视图的话，它是无法展示动态数据的，所以我们对HTML的新的期待：既能够正常显示页面，又能在页面中包含动态数据部分。而动态数据单靠HTML本身是无法做到的，所以此时我们需要引入服务器端动态视图模板技术。

##### 1.1.2 MVC概念

M：Model模型（javabean）

V：View视图（html+动态数据）

C：Controller控制器（servlet）

MVC是在表述层开发中运用的一种设计理念。主张把**封装数据的『模型』**、**显示用户界面的『视图』**、**协调调度的『控制器』**分开。

好处：

- 进一步实现各个组件之间的解耦
- 让各个组件可以单独维护
- 将视图分离出来以后，我们后端工程师和前端工程师的对接更方便

##### 1.1.3 MVC和三层架构之间关系

![image-20220829223355854](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223355854.png)

#### 1.2 Thymeleaf的简介

##### 1.2.1 Thymeleaf的概念

Thymeleaf是一款用于渲染XML/XHTML/HTML5内容的模板引擎。类似JSP，Velocity，FreeMaker等， 它也可以轻易的与Spring MVC等Web框架进行集成作为Web应用的模板引擎。它的主要作用是在静态页面上渲染显示动态数据

##### 1.2.2 Thymeleaf的优势

- SpringBoot官方推荐使用的视图模板技术，和SpringBoot完美整合。

- 不经过服务器运算仍然可以直接查看原始值，对前端工程师更友好。

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    <p th:text="${username}">Original Value</p>

</body>
</html>
```

##### 1.2.3 物理视图和逻辑视图

###### ① 物理视图

在Servlet中，将请求转发到一个HTML页面文件时，使用的完整的转发路径就是<span style="color:blue;font-weight:bold;">物理视图</span>。![image-20220829223408783](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223408783.png)

> /pages/user/login_success.html

如果我们把所有的HTML页面都放在某个统一的目录下，那么转发地址就会呈现出明显的规律：

> /pages/user/login.html
> /pages/user/login_success.html
> /pages/user/regist.html
> /pages/user/regist_success.html
>
> ……

路径的开头都是：/pages/user/

路径的结尾都是：.html

所以，路径开头的部分我们称之为<span style="color:blue;font-weight:bold;">视图前缀</span>，路径结尾的部分我们称之为<span style="color:blue;font-weight:bold;">视图后缀</span>。



###### ② 逻辑视图

### 2. 逻辑视图

物理视图=视图前缀+逻辑视图+视图后缀

上面的例子中：

| 视图前缀     | 逻辑视图      | 视图后缀 | 物理视图                       |
| ------------ | ------------- | -------- | ------------------------------ |
| /pages/user/ | login         | .html    | /pages/user/login.html         |
| /pages/user/ | login_success | .html    | /pages/user/login_success.html |

#### 2.1 Thymeleaf的HelloWorld

##### 2.1.1 加入jar包

![image-20220829223443638](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223443638.png)

##### 2.1.2 配置上下文参数

![image-20220829223457387](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223457387.png)

物理视图=视图前缀+逻辑视图+视图后缀

```xml
<!-- 在上下文参数中配置视图前缀和视图后缀 -->
<context-param>
    <param-name>view-prefix</param-name>
    <param-value>/WEB-INF/view/</param-value>
</context-param>
<context-param>
    <param-name>view-suffix</param-name>
    <param-value>.html</param-value>
</context-param>
```

说明：param-value中设置的前缀、后缀的值不是必须叫这个名字，可以根据实际情况和需求进行修改。

> 为什么要放在WEB-INF目录下？
>
> 原因：WEB-INF目录不允许浏览器直接访问，所以我们的视图模板文件放在这个目录下，是一种保护。以免外界可以随意访问视图模板文件。
>
> 访问WEB-INF目录下的页面，都必须通过Servlet转发过来，简单说就是：不经过Servlet访问不了。
>
> 这样就方便我们在Servlet中检查当前用户是否有权限访问。
>
> 那放在WEB-INF目录下之后，重定向进不去怎么办？
>
> 重定向到Servlet，再通过Servlet转发到WEB-INF下。

##### 2.1.3 创建Servlet基类

这个类大家直接<span style="color:blue;font-weight:bold;">复制粘贴</span>即可，将来使用框架后，这些代码都将被取代。

```java
import org.thymeleaf.TemplateEngine;
import org.thymeleaf.context.WebContext;
import org.thymeleaf.templatemode.TemplateMode;
import org.thymeleaf.templateresolver.ServletContextTemplateResolver;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class ViewBaseServlet extends HttpServlet {

    private TemplateEngine templateEngine;

    @Override
    public void init() throws ServletException {

        // 1.获取ServletContext对象
        ServletContext servletContext = this.getServletContext();

        // 2.创建Thymeleaf解析器对象
        ServletContextTemplateResolver templateResolver = new ServletContextTemplateResolver(servletContext);

        // 3.给解析器对象设置参数
        // ①HTML是默认模式，明确设置是为了代码更容易理解
        templateResolver.setTemplateMode(TemplateMode.HTML);

        // ②设置前缀
        String viewPrefix = servletContext.getInitParameter("view-prefix");

        templateResolver.setPrefix(viewPrefix);

        // ③设置后缀
        String viewSuffix = servletContext.getInitParameter("view-suffix");

        templateResolver.setSuffix(viewSuffix);

        // ④设置缓存过期时间（毫秒）
        templateResolver.setCacheTTLMs(60000L);

        // ⑤设置是否缓存
        templateResolver.setCacheable(true);

        // ⑥设置服务器端编码方式
        templateResolver.setCharacterEncoding("utf-8");

        // 4.创建模板引擎对象
        templateEngine = new TemplateEngine();

        // 5.给模板引擎对象设置模板解析器
        templateEngine.setTemplateResolver(templateResolver);

    }

    protected void processTemplate(String templateName, HttpServletRequest req, HttpServletResponse resp) throws IOException {
        // 1.设置响应体内容类型和字符集
        resp.setContentType("text/html;charset=UTF-8");

        // 2.创建WebContext对象
        WebContext webContext = new WebContext(req, resp, getServletContext());

        // 3.处理模板数据
        templateEngine.process(templateName, webContext, resp.getWriter());
    }
}
```

##### 2.1.4 入门案例代码

###### ① 创建index.html文件

![image-20220829223506786](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223506786.png)

###### ② index.html编写超链接访问Servlet

```html
<a href="/webday08/TestThymeleafServlet">初步测试Thymeleaf</a>
```

###### ③ 创建Servlet

![image-20220829223513504](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223513504.png)

```xml
<servlet>
    <servlet-name>testThymeleafServlet</servlet-name>
    <servlet-class>com.atguigu.servlet.TestThymeleafServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>testThymeleafServlet</servlet-name>
    <url-pattern>/testThymeleaf</url-pattern>
</servlet-mapping>
```

###### ④ 修改Servlet让其继承ViewBaseServlet

![image-20220829223521269](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223521269.png)

###### ⑤ 在doPost()方法中跳转到Thymeleaf页面

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author chenxin
 * 日期2021-06-13  09:15
 */
public class TestThymeleafServlet extends ViewBaseServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setAttribute("username","奥巴马");
        //请求转发跳转到/WEB-INF/view/target.html
        processTemplate("target",request,response);
    }
}
```

###### ⑥ Thymeleaf页面代码

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>目标页面</title>
    </head>
    <body>
        <h1 th:text="${username}">这里要显示一个动态的username</h1>
    </body>
</html>
```

## 3. Thymeleaf的基本语法

#### 3.1 th名称空间

![image-20220829223529835](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223529835.png)

#### 3.2 表达式语法

##### 3.2.1 修改标签文本值

代码示例：

```html
<p th:text="标签体新值">标签体原始值</p>
```

###### ① th:text作用

- 不经过服务器解析，直接用浏览器打开HTML文件，看到的是『标签体原始值』
- 经过服务器解析，Thymeleaf引擎根据th:text属性指定的『标签体新值』去<span style="color:blue;font-weight:bold;">替换</span>『标签体原始值』

##### 3.2.2 修改指定属性值

代码示例：

```html
<input type="text" name="username" th:value="文本框新值" value="文本框旧值" />
```

语法：任何HTML标签原有的属性，前面加上『th:』就都可以通过Thymeleaf来设定新值。

##### 3.2.3 解析URL地址

代码示例：

```html
<!--
使用Thymeleaf解析url地址
-->
<a th:href="@{/index.html}">访问index.html</a>
```

经过解析后得到：

> /webday08/index.html

所以@{}的作用是<span style="color:blue;font-weight:bold;">在字符串前附加『上下文路径』</span>

>  这个语法的好处是：实际开发过程中，项目在不同环境部署时，Web应用的名字有可能发生变化。所以上下文路径不能写死。而通过@{}动态获取上下文路径后，不管怎么变都不怕啦！

###### ① 首页使用URL地址解析

![image-20220829223537211](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223537211.png)

如果我们直接访问index.html本身，那么index.html是不需要通过Servlet，当然也不经过模板引擎，所以index.html上的Thymeleaf的任何表达式都不会被解析。

解决办法：通过Servlet访问index.html，这样就可以让模板引擎渲染页面了：

![image-20220829223543798](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223543798.png)

> 进一步的好处：
>
> 通过上面的例子我们看到，所有和业务功能相关的请求都能够确保它们通过Servlet来处理，这样就方便我们统一对这些请求进行特定规则的限定。

###### ② 给URL地址后面附加请求参数

参照官方文档说明：

![image-20220829223552611](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223552611.png)

#### 3.3 域对象在Thymeleaf中的使用

##### 3.3.1 回顾域对象

域对象是在服务器中有一定作用域范围的对象，在这个范围内的所有动态资源都能够共享域对象中保存的数据

##### 3.3.2 回顾域对象的类型

###### ① 请求域

在请求转发的场景下，我们可以借助HttpServletRequest对象内部给我们提供的存储空间，帮助我们携带数据，把数据发送给转发的目标资源。

请求域：HttpServletRequest对象内部给我们提供的存储空间

###### ② 会话域(后面学)

会话域的范围是一次会话

![image-20220829223609980](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223609980.png)

###### ③ 应用域(后面学)

应用域的范围是整个项目全局

![image-20220829223624904](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223624904.png)

##### 3.3.3 在Thymeleaf中操作域对象

我们通常的做法是，在Servlet中将数据存储到域对象中，而在使用了Thymeleaf的前端页面中取出域对象中的数据并展示

###### ① 操作请求域

Servlet中代码：

```java
String requestAttrName = "helloRequestAttr";
String requestAttrValue = "helloRequestAttr-VALUE";

request.setAttribute(requestAttrName, requestAttrValue);
```

Thymeleaf表达式：

```html
<p th:text="${helloRequestAttr}">request field value</p>
```

###### ② 操作会话域(后期讲)

###### ③ 操作应用域(后期讲)

#### 3.4 获取请求参数

##### 3.4.1 获取请求参数的语法

```html
${param.参数名}
```

##### 3.4.2 根据一个参数名获取一个参数值

页面代码：

```html
<p th:text="${param.username}">这里替换为请求参数的值</p>
```

页面显示效果：

![image-20220829223637096](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223637096.png)

##### 3.4.3 根据一个参数名获取多个参数值

页面代码：

```html
<p th:text="${param.team}">这里替换为请求参数的值</p>
```

页面显示效果：

![image-20220829223642540](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223642540.png)

如果想要精确获取某一个值，可以使用数组下标。页面代码：

```html
<p th:text="${param.team[0]}">这里替换为请求参数的值</p>
<p th:text="${param.team[1]}">这里替换为请求参数的值</p>
```

页面显示效果：

![image-20220829223648537](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223648537.png)

#### 3.5 内置对象

##### 3.5.1 内置对象的概念

所谓内置对象其实就是在Thymeleaf的表达式中<span style="color:blue;font-weight:bold;">可以直接使用</span>的对象

##### 3.5.2 基本内置对象

![image-20220829223654782](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223654782.png)

用法举例：

```html
<h3>表达式的基本内置对象</h3>
<p th:text="${#request.getContextPath()}">调用#request对象的getContextPath()方法</p>
<p th:text="${#request.getAttribute('helloRequestAttr')}">调用#request对象的getAttribute()方法，读取属性域</p>
```

基本思路：

- 如果不清楚这个对象有哪些方法可以使用，那么就通过getClass().getName()获取全类名，再回到Java环境查看这个对象有哪些方法
- 内置对象的方法可以直接调用
- 调用方法时需要传参的也可以直接传入参数

##### 3.5.3 公共内置对象

![image-20220829223701766](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223701766.png)

Servlet中将List集合数据存入请求域：

```java
request.setAttribute("aNotEmptyList", Arrays.asList("aaa","bbb","ccc"));
request.setAttribute("anEmptyList", new ArrayList<>());
```

页面代码：

```html
<p>#list对象isEmpty方法判断集合整体是否为空aNotEmptyList：<span th:text="${#lists.isEmpty(aNotEmptyList)}">测试#lists</span></p>
<p>#list对象isEmpty方法判断集合整体是否为空anEmptyList：<span th:text="${#lists.isEmpty(anEmptyList)}">测试#lists</span></p>
```

公共内置对象对应的源码位置：

![image-20220829223709130](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223709130.png)

#### 3.6 OGNL

##### 3.6.1 OGNL的概念

OGNL：Object-Graph Navigation Language对象-图 导航语言

##### 3.6.2 对象图的概念

从根对象触发，通过特定的语法，逐层访问对象的各种属性。

![image-20220829223716114](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223716114.png)

##### 3.6.3 OGNL语法

###### ① 起点

在Thymeleaf环境下，${}中的表达式可以从下列元素开始：

- 访问属性域的起点
  - 请求域属性名
  - session
  - application
- param
- 内置对象
  - request
  - session
  - lists
  - strings

###### ② 属性访问语法

- 访问对象属性：使用getXxx()、setXxx()方法定义的属性
  - 对象.属性名
- 访问List集合或数组
  - 集合或数组[下标]
- 访问Map集合
  - Map集合.key
  - Map集合['key']

#### 3.7 分支与迭代

##### 3.7.1 分支

###### ① if和unless

让标记了th:if、th:unless的标签根据条件决定是否显示。

示例的实体类：

```java
package com.atguigu.bean;

/**
 * 包名:com.atguigu.bean
 *
 * @author chenxin
 * 日期2021-06-13  10:58
 */
public class Teacher {
    private String teacherName;

    public Teacher() {
    }

    public Teacher(String teacherName) {
        this.teacherName = teacherName;
    }

    public String getTeacherName() {
        return teacherName;
    }

    public void setTeacherName(String teacherName) {
        this.teacherName = teacherName;
    }
}
```

示例的Servlet代码：

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    // 1.创建ArrayList对象并填充
    List<Employee> employeeList = new ArrayList<>();

    employeeList.add(new Employee(1, "tom", 500.00));
    employeeList.add(new Employee(2, "jerry", 600.00));
    employeeList.add(new Employee(3, "harry", 700.00));

    // 2.将集合数据存入请求域
    request.setAttribute("employeeList", employeeList);

    // 3.调用父类方法渲染视图
    super.processTemplate("list", request, response);
}
```

示例的HTML代码：

```html
<table>
    <tr>
        <th>员工编号</th>
        <th>员工姓名</th>
        <th>员工工资</th>
    </tr>
    <tr th:if="${#lists.isEmpty(employeeList)}">
        <td colspan="3">抱歉！没有查询到你搜索的数据！</td>
    </tr>
    <tr th:if="${not #lists.isEmpty(employeeList)}">
        <td colspan="3">有数据！</td>
    </tr>
    <tr th:unless="${#lists.isEmpty(employeeList)}">
        <td colspan="3">有数据！</td>
    </tr>
</table>
```

if配合not关键词和unless配合原表达式效果是一样的，看自己的喜好。

###### ② switch

```html
<h3>测试switch</h3>
<div th:switch="${user.memberLevel}">
    <p th:case="level-1">银牌会员</p>
    <p th:case="level-2">金牌会员</p>
    <p th:case="level-3">白金会员</p>
    <p th:case="level-4">钻石会员</p>
</div>
```

##### 3.7.2 迭代

在迭代过程中，可以参考下面的说明使用迭代状态：

![image-20220829223727299](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223727299.png)

```html
<!--遍历显示请求域中的teacherList-->
<table border="1" cellspacing="0" width="500">
    <tr>
        <th>编号</th>
        <th>姓名</th>
    </tr>
    <tbody th:if="${#lists.isEmpty(teacherList)}">
        <tr>
            <td colspan="2">教师的集合是空的!!!</td>
        </tr>
    </tbody>

    <!--
集合不为空，遍历展示数据
-->
    <tbody th:unless="${#lists.isEmpty(teacherList)}">
        <!--
使用th:each遍历
用法:
1. th:each写在什么标签上？ 每次遍历出来一条数据就要添加一个什么标签，那么th:each就写在这个标签上
2. th:each的语法    th:each="遍历出来的数据,数据的状态 : 要遍历的数据"
3. status表示遍历的状态，它包含如下属性:
 index 遍历出来的每一个元素的下标
 count 遍历出来的每一个元素的计数

-->
        <tr th:each="teacher,status : ${teacherList}">
            <td th:text="${status.count}">这里显示编号</td>
            <td th:text="${teacher.teacherName}">这里显示老师的名字</td>
        </tr>
    </tbody>
</table>
```

#### 3.8 Thymeleaf包含其他模板文件

##### 3.8.1 应用场景

抽取各个页面的公共部分：

![image-20220829223734391](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223734391.png)

##### 3.8.2 操作步骤

###### ① 创建页面的公共代码片段

使用th:fragment来给这个片段命名：

```html
<div th:fragment="header">
    <p>被抽取出来的头部内容</p>
</div>
```

###### ② 在需要的页面中进行包含

| 语法       | 效果                                                     | 特点                                       |
| ---------- | -------------------------------------------------------- | ------------------------------------------ |
| th:insert  | 把目标的代码片段整个插入到当前标签内部                   | 它会保留页面自身的标签                     |
| th:replace | 用目标的代码替换当前标签                                 | 它不会保留页面自身的标签                   |
| th:include | 把目标的代码片段去除最外层标签，然后再插入到当前标签内部 | 它会去掉片段外层标记，同时保留页面自身标记 |

页面代码举例：

```html
<!-- 代码片段所在页面的逻辑视图 :: 代码片段的名称 -->
<div id="badBoy" th:insert="segment :: header">
    div标签的原始内容
</div>

<div id="worseBoy" th:replace="segment :: header">
    div标签的原始内容
</div>

<div id="worstBoy" th:include="segment :: header">
    div标签的原始内容
</div>
```



## 4. CRUD练习

### 4.1 数据建模

#### 4.1.1 物理建模

```sql
CREATE DATABASE `view-demo`CHARACTER SET utf8;
USE `view-demo`;
CREATE TABLE t_soldier(
    soldier_id INT PRIMARY KEY AUTO_INCREMENT,
    soldier_name CHAR(100),
    soldier_weapon CHAR(100)
);
```

#### 4.1.2 逻辑建模

```java
public class Soldier {
    
    private Integer soldierId;
    private String soldierName;
    private String soldierWeapon;
    ... 
```

### 4.2 总体架构

![image-20220829223746366](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223746366.png)

### 4.3 搭建环境

#### 4.3.1 搭建持久层环境

1. 拷贝持久层的jar包: mysql驱动、druid、dbutils、junit、BeanUtils
2. 拷贝JDBCUtils工具类、jdbc.properties文件、BaseDao类

#### 4.3.2 搭建Thymeleaf环境

1. 拷贝Thymeleaf所需的jar包

2. 拷贝ViewBaseServlet类

3. 配置web.xml

   ```xml
   <!-- 在上下文参数中配置视图前缀和视图后缀 -->
   <context-param>
       <param-name>view-prefix</param-name>
       <param-value>/WEB-INF/view/</param-value>
   </context-param>
   <context-param>
       <param-name>view-suffix</param-name>
       <param-value>.html</param-value>
   </context-param>
   ```

4. 创建view目录

   ![image-20220829223753572](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223753572.png)

### 4.4 需要实现的功能列表

- 显示首页：浏览器通过index.html访问首页Servlet，然后再解析对应的模板视图
- 显示列表：在首页点击超链接，跳转到目标页面把所有士兵的信息列表显示出来
- 删除信息：在列表上点击删除超链接，执行信息的删除操作
- 新增信息：
  - 在列表页面点击超链接跳转到新增士兵信息的表单页面
  - 在新增信息的表单页面点击提交按钮执行保存
- 更新信息：
  - 在列表上点击更新超链接，跳转到更新士兵信息的表单页面：表单回显
  - 在更新信息的表单页面点击提交按钮执行更新

### 4.5 显示首页功能

#### 4.5.1 目标

浏览器访问index.html，通过首页Servlet，渲染视图，显示首页。

#### 4.5.2 思路

![image-20220829223800972](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223800972.png)

#### 4.5.3 代码

##### ① 创建PortalServlet

```xml
<servlet>
    <servlet-name>PortalServlet</servlet-name>
    <servlet-class>com.atguigu.servlet.PortalServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>PortalServlet</servlet-name>
    <url-pattern>/portal</url-pattern>
</servlet-mapping>
```

Servlet代码：

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author chenxin
 * 日期2021-06-13  14:07
 */
public class PortalServlet extends ViewBaseServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //跳转到首页
        processTemplate("index",request,response);
    }
}
```

##### ②创建portal.html

![image-20220829223807983](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223807983.png)

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>首页</title>
    </head>
    <body>
        <!--
查询士兵列表
-->
        <a th:href="@{/soldier(method='showAll')}">查看士兵列表</a>
    </body>
</html>
```

### 4.6 显示列表

#### 4.6.1 目标

在目标页面显示所有士兵信息，士兵信息是从数据库查询出来的

#### 4.6.2 思路

![image-20220829223815561](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223815561.png)

#### 4.6.3 代码

##### ① ModelBaseServlet

创建这个基类的原因是：我们希望每一个模块能够对应同一个Servlet，这个模块所需要调用的所有方法都集中在同一个Servlet中。如果没有这个ModelBaseServlet基类，我们doGet()、doPost()方法可以用来处理请求，这样一来，每一个方法都需要专门创建一个Servlet（就好比咱们之前的LoginServlet、RegisterServlet其实都应该合并为UserServlet）。

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.lang.reflect.Method;

/**
 * @author chenxin
 * 日期2021-06-13  16:31
 */
public class ModelBaseServlet extends ViewBaseServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");
        //获取请求参数method的值
        String method = request.getParameter("method");
        //method参数的值就是要调用的方法的方法名，那就是已知方法名要去查找调用本对象的方法
        try {
            Method declaredMethod = this.getClass().getDeclaredMethod(method, HttpServletRequest.class, HttpServletResponse.class);

            //暴力反射
            declaredMethod.setAccessible(true);
            //调用方法
            declaredMethod.invoke(this,request,response);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

##### ② SoldierDao.selectSoldierList()

![image-20220829223824780](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223824780.png)

接口方法：

```java
public interface SoldierDao {
   /**
     * 查询所有士兵
     * @return
     */
    List<Soldier> findAll() throws SQLException;
}
```

实现类方法：

```java
public class SoldierDaoImpl extends BaseDao<Soldier> implements SoldierDao {
    @Override
    public List<Soldier> findAll() throws SQLException {
        String sql = "select soldier_id soldierId,soldier_name soldierName,soldier_weapon soldierWeapon from t_soldier";
        return getBeanList(Soldier.class,sql);
    }
}
```

##### ③ SoldierService.getSoldierList()

![image-20220829223831618](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223831618.png)

接口方法：

```java
public interface SoldierService {

    /**
     * 查询所有士兵信息
     * @return
     */
    List<Soldier> findAllSoldier() throws Exception;

}
```

实现类方法：

```java
public class SoldierServiceImpl implements SoldierService {

    private SoldierDao soldierDao = new SoldierDaoImpl();

    @Override
    public List<Soldier> findAllSoldier() throws Exception {
        return soldierDao.findAll();
    }
}
```

##### ④ SoldierServlet.showList()

```java
/**
     * 处理查询所有士兵信息的请求
     * @param request
     * @param response
     */
public void showAll(HttpServletRequest request,HttpServletResponse response){
    try {
        //调用业务层的方法查询士兵列表
        List<Soldier> soldierList = soldierService.findAllSoldier();
        //将soldierList存储到域对象
        request.setAttribute("soldierList",soldierList);
        //跳转到展示页面进行展示
        processTemplate("list",request,response);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

##### ⑤ 显示士兵列表的list.html页面

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>士兵列表展示页面</title>
</head>
<body>
    <table border="1" cellspacing="0" width="800">
        <tr>
            <th>士兵的编号</th>
            <th>士兵的姓名</th>
            <th>士兵的武器</th>
            <th>删除信息</th>
            <th>修改信息</th>
        </tr>
        <tbody th:if="${#lists.isEmpty(soldierList)}">
            <tr>
                <td th:colspan="5">没有士兵数据，请添加士兵</td>
            </tr>
        </tbody>

        <tbody th:unless="${#lists.isEmpty(soldierList)}">
            <tr th:each="soldier : ${soldierList}">
                <td th:text="${soldier.soldierId}">士兵的编号</td>
                <td th:text="${soldier.soldierName}">士兵的姓名</td>
                <td th:text="${soldier.soldierWeapon}">士兵的武器</td>
                <td><a th:href="@{/soldier(method='deleteSoldier',id=${soldier.soldierId})}">删除信息</a></td>
                <td><a th:href="@{/soldier(method='toUpdatePage',id=${soldier.soldierId})}">修改信息</a></td>
            </tr>
        </tbody>
        <tfoot>
            <tr>
                <td th:colspan="5" align="center">
                    <a th:href="@{/soldier(method='toAddPage')}">添加士兵信息</a>
                </td>
            </tr>
        </tfoot>
    </table>
</body>
</html>
```

### 4.7 删除功能

#### 4.7.1 目标

点击页面上的超链接，把数据库表中的记录删除。

#### 4.7.2 思路

![image-20220829223841432](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223841432.png)

#### 4.7.3 代码

##### ① 完成删除超链接

![image-20220829223848183](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223848183.png)

```html
<td><a th:href="@{/soldier(method='deleteSoldier',id=${soldier.soldierId})}">删除信息</a></td>
```

关于@{地址}附加请求参数的语法格式：

- 只有一个请求参数：@{地址(请求参数名=普通字符串)}或@{地址(请求参数名=${需要解析的表达式})}
- 多个请求参数：@{地址(名=值,名=值)}

官方文档中的说明如下：

![image-20220829223856793](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223856793.png)

##### ② Servlet方法

```java
/**
     * 删除士兵信息
     * @param request
     * @param response
     * @throws IOException
     */
public void deleteSoldier(HttpServletRequest request,HttpServletResponse response) throws IOException {
    //1. 获取请求参数：id
    Integer id = Integer.valueOf(request.getParameter("id"));
    //2. 调用业务层的方法，根据id删除士兵
    try {
        soldierService.deleteSoldierById(id);
        //3. 删除成功,重新查询所有
        response.sendRedirect(request.getContextPath()+"/soldier?method=showAll");
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

##### ③ Service方法

```java
@Override
public void deleteSoldierById(Integer id) throws Exception{
    soldierDao.deleteSoldierById(id);
}
```

##### ④ Dao方法

```java
@Override
public void deleteSoldierById(Integer id) throws SQLException {
    String sql = "delete from t_soldier where soldier_id=?";
    update(sql,id);
}
```

### 4.8 前往新增信息的表单页面

#### 4.8.1 创建超链接

```html
<a th:href="@{/soldier(method='toAddPage')}">添加士兵信息</a>
```

#### 4.8.2 Servlet

```java
/**
     * 跳转到添加页面
     * @param request
     * @param response
     */
public void toAddPage(HttpServletRequest request,HttpServletResponse response) throws IOException {
    processTemplate("add",request,response);
}
```

#### 4.8.3 创建表单页面

![image-20220829223907320](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223907320.png)

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>添加士兵的页面</title>
    </head>
    <body>
        <form th:action="@{/soldier(method='addSoldier')}" method="post">
            士兵姓名<input name="soldierName"/><br/>
            士兵武器<input name="soldierWeapon"/><br/>
            <input type="submit" value="添加">
        </form>
    </body>
</html>
```

### 4.9 执行保存

#### 4.9.1 目标

提交表单后，将表单数据封装为Soldier对象，然后将Soldier对象保存到数据库。

#### 4.9.2 思路

![image-20220829223915167](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223915167.png)

#### 4.9.3 代码

##### ① Servlet方法

```java
/**
     * 添加士兵信息
     * @param request
     * @param response
     * @throws IOException
     */
public void addSoldier(HttpServletRequest request,HttpServletResponse response) throws IOException {
    //1. 获取请求参数
    Map<String, String[]> parameterMap = request.getParameterMap();
    //2. 将请求参数封装到Soldier对象
    Soldier soldier = new Soldier();
    try {
        BeanUtils.populate(soldier,parameterMap);
        //3. 调用业务层的方法处理添加士兵的功能
        soldierService.addSoldier(soldier);

        //4. 跳转到查看所有士兵信息列表
        response.sendRedirect(request.getContextPath()+"/soldier?method=showAll");
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

##### ② Service方法

```java
@Override
public void addSoldier(Soldier soldier) throws Exception {
    soldierDao.addSoldier(soldier);
}
```

##### ③ Dao方法

```java
@Override
public void addSoldier(Soldier soldier) throws SQLException {
    String sql = "insert into t_soldier (soldier_name,soldier_weapon) values (?,?)";
    update(sql,soldier.getSoldierName(),soldier.getSoldierWeapon());
}
```

### 4.10 前往修改信息的表单页面

![image-20220829223922267](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223922267.png)

#### 4.10.1 创建超链接

```html
<a th:href="@{/soldier(method='toUpdatePage',id=${soldier.soldierId})}">修改信息</a>
```

#### 4.10.2 Servlet方法

```java
/**
     * 跳转到修改页面
     * @param request
     * @param response
     * @throws IOException
     */
public void toUpdatePage(HttpServletRequest request,HttpServletResponse response) throws IOException {
    //获取要修改的士兵的id
    Integer id = Integer.valueOf(request.getParameter("id"));
    //查询出当前士兵的信息
    try {
        Soldier soldier = soldierService.findSoldierById(id);
        //将soldier存储到请求域中
        request.setAttribute("soldier",soldier);

        //跳转到修改页面
        processTemplate("update",request,response);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

#### 4.10.3 Service方法

```java
@Override
public Soldier findSoldierById(Integer id) throws Exception {

    return soldierDao.findSoldierById(id);
}
```

#### 4.10.4 Dao方法

```java
@Override
public Soldier findSoldierById(Integer id) throws SQLException {
    String sql = "select soldier_id soldierId,soldier_name soldierName,soldier_weapon soldierWeapon from t_soldier where soldier_id=?";

    return getBean(Soldier.class,sql,id);
}
```

#### 4.10.5 表单页面

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>修改页面</title>
    </head>
    <body>
        <form th:action="@{/soldier(method='updateSoldier')}" method="post">
            <input type="hidden" name="soldierId" th:value="${soldier.soldierId}"/>
            士兵姓名<input type="text" th:value="${soldier.soldierName}" name="soldierName"/><br/>
            士兵武器<input type="text" th:value="${soldier.soldierWeapon}" name="soldierWeapon"/><br/>
            <input type="submit" value="修改"/>
        </form>
    </body>
</html>
```

### 4.11 执行更新

![image-20220829223930387](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223930387.png)

#### 4.11.1 Servlet方法

```java
/**
     * 修改士兵信息
     * @param request
     * @param response
     * @throws IOException
     */
public void updateSoldier(HttpServletRequest request,HttpServletResponse response) throws IOException {
    //1. 获取请求参数
    Map<String, String[]> parameterMap = request.getParameterMap();
    //2. 将请求参数封装到Soldier对象
    Soldier soldier = new Soldier();
    try {
        BeanUtils.populate(soldier,parameterMap);
        //3. 调用业务层的方法执行修改
        soldierService.updateSoldier(soldier);
        //4. 修改成功之后重新查询所有
        response.sendRedirect(request.getContextPath()+"/soldier?method=showAll");
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

#### 4.11.2 Service方法

```java
@Override
public void updateSoldier(Soldier soldier) throws Exception {
    soldierDao.updateSoldier(soldier);
}
```

#### 4.11.3 Dao方法

```java
@Override
public void updateSoldier(Soldier soldier) throws SQLException {
    String sql = "update t_soldier set soldier_name=?, soldier_weapon=? where soldier_id=?";
    update(sql,soldier.getSoldierName(),soldier.getSoldierWeapon(),soldier.getSoldierId());
}
```





# 第五章  书城项目第三阶段 

## 1. 项目准备工作 

### 1.1 创建Module

![images](https://raw.githubusercontent.com/Young-Allen/pic/main/img001.png)

### 1.2 拷贝jar包

1. 数据库jar包

   <img src="https://raw.githubusercontent.com/Young-Allen/pic/main/img006.png" style="zoom:50%;" />

2. Thymeleaf的jar包

   ![image-20220829223951017](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829223951017.png)

### 1.3 从V2版本项目迁移代码

#### 1.3.1 迁移src目录下的Java源代码

- 拷贝resources目录，然后将resource目录标记成Resources Root
- 拷贝src目录下的内容，并且将原有的Servlet全部删除
- 创建两个子包
  - 存放Servlet基类：com.atguigu.bookstore.servlet.base
  - 存放Servlet子类：com.atguigu.bookstore.servlet.model
- 从资料中将两个基类拷贝过来，放置到com.atguigu.bookstore.servlet.base包里面
  - 视图基类：ViewBaseServlet
  - 方法分发基类：BaseServlet

#### 1.3.2 迁移前端代码

- 将V02中的pages目录整体复制到V03 module的<span style="color:blue;font-weight:bold;">WEB-INF目录</span>下
- 将V02中的static目录整体复制到V03 module的<span style="color:blue;font-weight:bold;">web目录</span>下
- 将V02中的index.html复制到V03 module的WEB-INF/pages目录下，将来通过Servlet访问

### 1.4 显示首页

#### 1.4.1 修改web.xml

```xml
<!-- 在上下文参数中配置视图前缀和视图后缀 -->
<context-param>
    <param-name>view-prefix</param-name>
    <param-value>/WEB-INF/pages/</param-value>
</context-param>
<context-param>
    <param-name>view-suffix</param-name>
    <param-value>.html</param-value>
</context-param>
```

<span style="color:blue;font-weight:bold;">注意</span>：这里需要将WEB-INF下的view改成pages，和当前项目环境的目录结构一致。

#### 1.4.2 创建PortalServlet

<span style="color:blue;font-weight:bold;">注意</span>：这个PortalServlet映射的地址是/index.html，这样才能保证访问首页时访问它。

```xml
<servlet>
    <servlet-name>PortalServlet</servlet-name>
    <servlet-class>com.atguigu.bookstore.servlet.model.PortalServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>PortalServlet</servlet-name>
    <url-pattern>/index.html</url-pattern>
</servlet-mapping>
```

<span style="color:blue;font-weight:bold;">注意</span>：PortalServlet服务于首页的显示，为了降低用户访问首页的门槛，不能附加任何请求参数，所以不能继承ModelBaseServlet，只能继承ViewBaseServlet。

```java
package com.atguigu.servlet.model;

import com.atguigu.servlet.base.ViewBaseServlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author chenxin
 * 日期2021-06-14  09:03
 * 该Servlet只需要处理访问首页
 */
public class PortalServlet extends ViewBaseServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        processTemplate("index",request,response);
    }
}
```

#### 1.4.3 调整index.html

- 加入Thymeleaf名称空间

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

- 修改base标签

```html
<base th:href="@{/}" href="/bookstore/"/>
```

## 2. 完成用户模块

### 2.1 重构登录功能

#### 2.1.1 思路

![image-20220829224003892](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224003892.png)

#### 2.1.2 代码

##### ① 创建UserServlet

web.xml中的配置：

```xml
<servlet>
    <servlet-name>UserServlet</servlet-name>
    <servlet-class>com.atguigu.servlet.model.UserServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>UserServlet</servlet-name>
    <url-pattern>/user</url-pattern>
</servlet-mapping>
```

Java代码：

```java
package com.atguigu.servlet.model;

import com.atguigu.bean.User;
import com.atguigu.service.UserService;
import com.atguigu.service.impl.UserServiceImpl;
import com.atguigu.servlet.base.ModelBaseServlet;
import org.apache.commons.beanutils.BeanUtils;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.Map;

/**
 * @author chenxin
 * 日期2021-06-14  09:07
 */
public class UserServlet extends ModelBaseServlet {
    private UserService userService = new UserServiceImpl();
    /**
     * 跳转到登录页面
     * @param request
     * @param response
     */
    public void toLoginPage(HttpServletRequest request, HttpServletResponse response) throws IOException {
    }
}
```

<span style="color:blue;font-weight:bold;">注意</span>：记得修改UserServlet继承的类ModelBaseServlet

##### ② 前往登录页面功能

a. 修改首页中登录超链接

```html
<a href="user?method=toLoginPage" class="login">登录</a>
```

b. 完成UserServlet.toLoginPage()方法

```java
/**
     * 跳转到登录页面
     * @param request
     * @param response
     */
public void toLoginPage(HttpServletRequest request, HttpServletResponse response) throws IOException {
    processTemplate("user/login",request,response);
}
```

c. 调整登录页面代码

- 加入Thymeleaf名称空间

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
```

- 修改base标签

```html
<base th:href="@{/}" href="/bookstore/" />
```

- 修改form标签action属性

```html
<form id="loginForm" action="user" method="post">
```

- 增加method请求参数的表单隐藏域

```html
<input type="hidden" name="method" value="doLogin" />
```

- 根据条件显示登录失败消息

```html
<span class="errorMsg" th:text="${errorMessage}" style="color: red">{{errorMessage}}</span>
```

##### ③ 登录校验功能

a. UserServlet.doLogin()

```java
/**
     * 处理登录校验
     * @param request
     * @param response
     * @throws IOException
     */
public void doLogin(HttpServletRequest request, HttpServletResponse response) throws IOException {
    	String username = request.getParameter("username");
        String password = request.getParameter("password");
        User login = userService.login(username, password);
        if (login != null) {
            request.setAttribute("loginUser", login);
            this.processTemplate("user/login_success", request, response);
        } else {
            request.setAttribute("msg", "用户名或密码错误");
            this.processTemplate("user/login", request, response);
        }
}
```

b. 回显表单中的用户名

在login.html页面进行设置

遇到问题：使用th:value="${param.username}"确实实现了服务器端渲染，但是实际打开页面并没有看到。原因是页面渲染顺序：

- 服务器端渲染
- 服务器端将渲染结果作为响应数据返回给浏览器
- 浏览器加载HTML文档
- 读取到Vue代码后，执行Vue代码
- Vue又进行了一次浏览器端渲染，覆盖了服务器端渲染的值

解决办法：将服务器端渲染的结果设置到Vue对象的data属性中。

```javascript
new Vue({
	"el":"#loginForm",
	"data":{
		"username":"[[${param.username}]]",
		"password":""
	},
```

c. 修改login_success.html页面

login_success.html

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    ……
<base th:href="@{/}" href="/bookstore/"/>
```

### 2.2 重构注册功能

### 2.2.1 思路

![image-20220829224015169](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224015169.png)

#### 2.2.2 代码

##### ① 前往注册页面功能

a. 修改首页中注册超链接

```html
<a href="user?method=toRegisterPage" class="register">注册</a>
```

b. 完成UserServlet.toRegisterPage()方法

```java
/**
     * 跳转到注册页面
     * @param request
     * @param response
     * @throws IOException
     */
public void toRegisterPage(HttpServletRequest request, HttpServletResponse response) throws IOException {
    processTemplate("user/regist",request,response);
}
```

c 调整注册页面代码

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    ……
<base th:href="@{/}" href="/bookstore/"/>
    ……
    
    <form id="registerForm" action="UserServlet" method="post">
					<input type="hidden" name="method" value="doRegister" />
        ……
```

```javascript
//注册失败后回显数据
new Vue({
	"el":"#registerForm",
	"data":{
		"username":"[[${param.username}]]",
		"password":"",
		"passwordConfirm":"",
		"email":"[[${param.email}]]",
		"code":"",
		"usernameCheckMessage":""
	}
```

##### ② 注册功能

a.Servlet代码

```java
/**
     * 处理注册请求
     * @param request
     * @param response
     * @throws IOException
     */
public void doRegister(HttpServletRequest request, HttpServletResponse response) throws IOException {
   Map<String, String[]> parameterMap =request.getParameterMap();
        User user=new User();
        try {
            BeanUtils.populate(user,parameterMap);
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InvocationTargetException e) {
            e.printStackTrace();
        }
        try {
            userService.register(user);
            request.setAttribute("registerUser",user);
            this.processTemplate("user/regist_success.html",request,response);
        } catch (Exception e) {
            e.printStackTrace();
            //有异常就注册失败,往域对象中存入失败信息
            request.setAttribute("errorMessage","注册失败,"+e.getMessage());
            //跳转回到注册页面
            processTemplate("user/regist",request,response);
        }
}
```

b.  修改regist_success.html页面

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    ……
<base th:href="@{/}" href="/bookstore/"/>
```

## 3. 书城后台CRUD

### 3.1 进入后台页面

#### 3.1.1 概念辨析

![image-20220829224026398](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224026398.png)

#### 3.1.2 访问后台首页

##### ① 思路

首页→后台系统超链接→AdminServlet.toManagerPage()→manager.html

##### ② 代码

###### a.  创建AdminServlet

web.xml

```xml
<servlet>
    <servlet-name>AdminServlet</servlet-name>
    <servlet-class>com.atguigu.servlet.model.AdminServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>AdminServlet</servlet-name>
    <url-pattern>/admin</url-pattern>
</servlet-mapping>
```

Java代码：

```java
package com.atguigu.servlet.model;

import com.atguigu.servlet.base.ModelBaseServlet;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author chenxin
 * 日期2021-06-14  10:00
 */
public class AdminServlet extends ModelBaseServlet {
    /**
     * 跳转到后台管理界面
     * @param request
     * @param response
     */
    public void toManagerPage(HttpServletRequest request, HttpServletResponse response) throws IOException {
        processTemplate("manager/manager",request,response);
    }
}
```

###### b. 调整manager.html

```html
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <base th:href="@{/}" href="/bookstore/"/>
```

然后去除页面上的所有“../”

###### c. 抽取页面公共部分

1. 创建包含代码片段的页面

![images](https://raw.githubusercontent.com/Young-Allen/pic/main/img005.png)

admin-navigator.html的代码

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

    <!-- 使用th:fragment属性给代码片段命名 -->
    <div th:fragment="navigator">
        <a href="book_manager.html" class="order">图书管理</a>
        <a href="order_manager.html" class="destory">订单管理</a>
        <a href="index.html" class="gohome">返回商城</a>
    </div>
</body>
</html>
```

2. 在有需要的页面(book_edit.html、book_manager.html、order_manager.html)引入片段

```html
<div th:include="segment/admin-navigator :: navigator"></div>
```

### 3.2 后台图书CRUD

#### 3.2.1 数据建模

##### ① 物理建模

执行资料中的sql脚本

##### ② 逻辑建模

```java
package com.atguigu.bean;

/**
 * 包名:com.atguigu.bean
 *
 * @author chenxin
 * 日期2021-06-14  10:22
 */
public class Book {
    private Integer bookId;
    private String bookName;
    private String author;
    private Double price;
    private Integer sales;
    private Integer stock;
    private String imgPath;

    public Book() {
    }

    public Book(Integer bookId, String bookName, String author, Double price, Integer sales, Integer stock, String imgPath) {
        this.bookId = bookId;
        this.bookName = bookName;
        this.author = author;
        this.price = price;
        this.sales = sales;
        this.stock = stock;
        this.imgPath = imgPath;
    }

    @Override
    public String toString() {
        return "Book{" +
                "bookId=" + bookId +
                ", bookName='" + bookName + '\'' +
                ", author='" + author + '\'' +
                ", price=" + price +
                ", sales=" + sales +
                ", stock=" + stock +
                ", imgPath='" + imgPath + '\'' +
                '}';
    }

    public Integer getBookId() {
        return bookId;
    }

    public void setBookId(Integer bookId) {
        this.bookId = bookId;
    }

    public String getBookName() {
        return bookName;
    }

    public void setBookName(String bookName) {
        this.bookName = bookName;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    public Integer getSales() {
        return sales;
    }

    public void setSales(Integer sales) {
        this.sales = sales;
    }

    public Integer getStock() {
        return stock;
    }

    public void setStock(Integer stock) {
        this.stock = stock;
    }

    public String getImgPath() {
        return imgPath;
    }

    public void setImgPath(String imgPath) {
        this.imgPath = imgPath;
    }
}
```

#### 3.2.2 创建并组装组件

##### ① 创建Servlet

- 后台：BookManagerServlet

##### ② 创建BookService

- 接口：BookService
- 实现类：BookServiceImpl

##### ③ 创建BookDao

- 接口：BookDao
- 实现类：BookDaoImpl

##### ④ 组装

- 给BookManagerServlet组装BookService
- 给BookService组装BookDao

#### 3.2.3 图书列表显示功能

##### ① 思路

manager.html→图书管理超链接→BookManagerServlet→toBookManagerPage()→book_manager.html

##### ② 修改图书管理超链接

超链接所在文件位置：

> WEB-INF/pages/segment/admin-navigator.html

```html
<a href="BookManagerServlet?method=showBookList" class="order">图书管理</a>
```

##### ③ BookManagerServlet.showBookList()

```java
/**
     * 跳转到图书管理页面
     * @param request
     * @param response
     */
public void toBookManagerPage(HttpServletRequest request, HttpServletResponse response) throws IOException {
    try {
        //查询出图书列表
        List<Book> bookList = bookService.getBookList();
        //将图书列表存储到请求域
        request.setAttribute("bookList",bookList);
        processTemplate("manager/book_manager",request,response);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

##### ④ BookService.getBookList()

```java
@Override
public List<Book> getBookList() throws Exception{
    return bookDao.selectBookList();
}
```

##### ⑤ BookDao.selectBookList()

```java
@Override
public List<Book> selectBookList() throws SQLException{
    String sql = "select book_id bookId,book_name bookName,author,price,sales,stock,img_path imgPath from t_book";

    return getBeanList(Book.class,sql);
}
```

##### ⑥ 调整book_manager.html

- Thymeleaf名称空间: `xmlns:th="http://www.thymeleaf.org"`
- base标签: `<base th:href="@{/}" href="/bookstore/"/>`
- 替换页面路径中的`../`
- 包含进来的代码片段: `<div th:include="segment/admin-navigator :: navigator"></div>`

##### ⑦ 在book_manager.html中迭代显示图书列表

```html
<table>
    <thead>
        <tr>
            <th>图片</th>
            <th>商品名称</th>
            <th>价格</th>
            <th>作者</th>
            <th>销量</th>
            <th>库存</th>
            <th>操作</th>
        </tr>
    </thead>
    <tbody th:if="${#lists.isEmpty(bookList)}">
        <tr>
            <td colspan="7">图书列表为空，请先添加图书！！！</td>
        </tr>
    </tbody>
    <tbody th:unless="${#lists.isEmpty(bookList)}">
        <tr th:each="book : ${bookList}">
            <td>
                <img th:src="${book.imgPath}" alt="" />
            </td>
            <td th:text="${book.bookName}">活着</td>
            <td th:text="${book.price}">
                100.00
            </td>
            <td th:text="${book.author}">余华</td>
            <td th:text="${book.sales}">200</td>
            <td th:text="${book.stock}">400</td>
            <td>
                <a href="#">修改</a><a href="#" class="del">删除</a>
            </td>
        </tr>
    </tbody>
</table>
```

#### 3.2.4 图书删除功能

##### ① 思路

book_manager.html→删除超链接→BookManagerServlet.removeBook()→重定向显示列表功能

##### ② 删除超链接

```html
<a th:href="@{/bookManager(method='removeBook',id=${book.bookId})}" class="del">删除</a>
```

##### ③ BookManagerServlet.removeBook()

```java
/**
     * 删除图书
     * @param request
     * @param response
     * @throws IOException
     */
public void removeBook(HttpServletRequest request, HttpServletResponse response) throws IOException {
    //1. 获取要删除的图书的id
    Integer id = Integer.valueOf(request.getParameter("id"));
    //2. 调用业务层的方法根据id删除图书
    try {
        bookService.removeBook(id);
        //3. 删除成功，重新查询所有图书信息
        response.sendRedirect(request.getContextPath()+"/bookManager?method=toBookManagerPage");
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

##### ④ BookService.removeBook()

```java
@Override
public void removeBook(Integer bookId) throws Exception {
    bookDao.deleteBook(bookId);
}
```

##### ⑤ BookDao.deleteBook()

```java
@Override
public void deleteBook(Integer bookId) throws SQLException {
    String sql = "delete from t_book where book_id=?";
    update(sql,bookId);
}
```

#### 3.2.5 新增图书功能

##### ① 思路

book_manager.html→添加图书超链接→BookManagerServlet.toAddPage()→book_add.html

book_add.html→提交表单→BookManagerServlet.saveBook()→重定向显示列表功能

##### ② 添加图书超链接

修改book_manager.html页面

```html
<a href="bookManager?method=toAddPage">添加图书</a>
```

##### ③ 实现：BookManagerServlet.toAddPage()

```java
/**
* 跳转到添加图书页面
* @param request
* @param response
* @throws IOException
*/
public void toAddPage(HttpServletRequest request, HttpServletResponse response) throws IOException{
    processTemplate("manager/book_edit",request,response);
}
```

##### ④ book_edit.html

由book_edit.html复制出来，然后调整表单标签：

```html
<form action="bookManager?method=saveOrUpdateBook" method="post" th:unless="${book != null}">
    <div class="form-item">
        <div>
            <label>名称:</label>
            <input type="text" placeholder="请输入名称" name="bookName" />
        </div>
        <span class="errMess" style="visibility: hidden;">请输入正确的名称</span
            >
    </div>
    <div class="form-item">
        <div>
            <label>价格:</label>
            <input type="number" placeholder="请输入价格" name="price" />
        </div>
        <span class="errMess">请输入正确数字</span>
    </div>
    <div class="form-item">
        <div>
            <label>作者:</label>
            <input type="text" placeholder="请输入作者" name="author"/>
        </div>
        <span class="errMess">请输入正确作者</span>
    </div>
    <div class="form-item">
        <div>
            <label>销量:</label>
            <input type="number" placeholder="请输入销量" name="sales" />
        </div>
        <span class="errMess">请输入正确销量</span>
    </div>
    <div class="form-item">
        <div>
            <label>库存:</label>
            <input type="number" placeholder="请输入库存" name="stock"/>
        </div>
        <span class="errMess">请输入正确库存</span>
    </div>

    <button type="submit" class="btn">提交</button>
</form>
```

##### ⑤ BookManagerServlet.saveOrUpdateBook()

```java
/**
     * 添加或者图书信息
     * @param request
     * @param response
     * @throws IOException
     */
public void saveOrUpdateBook(HttpServletRequest request, HttpServletResponse response) throws IOException{
    //1. 获取请求参数
    Map<String, String[]> parameterMap = request.getParameterMap();
    //2. 将parameterMap中的数据封装到Book对象
    try {
        Book book = new Book();
        BeanUtils.populate(book,parameterMap);

        //判断到底是修改还是添加
        if (book.getBookId() != null && !"".equals(book.getBookId())) {
            //修改图书信息
            //TODO
        }else {
            //添加图书信息
            //设置一个固定的imgPath
            book.setImgPath("static/uploads/xiaowangzi.jpg");
            //3. 调用业务层的方法保存图书信息
            bookService.saveBook(book);
        }

        //4. 保存成功，则重新查询所有图书
        response.sendRedirect(request.getContextPath()+"/bookManager?method=toBookManagerPage");
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

##### ⑥ BookService.saveBook()

```java
@Override
public void saveBook(Book book) throws Exception {
    bookDao.insertBook(book);
}
```

##### ⑦ BookDao.insertBook()

```java
@Override
public void insertBook(Book book) throws SQLException {
    String sql = "insert into t_book (book_name,author,price,sales,stock,img_path) values (?,?,?,?,?,?)";
    update(sql,book.getBookName(),book.getAuthor(),book.getPrice(),book.getSales(),book.getStock(),book.getImgPath());
}
```

#### 3.2.6 修改图书功能

##### ① 思路

book_manager.html→修改图书超链接→BookManagerServlet.toEditPage()→book_edit.html（表单回显）

book_edit.html→提交表单→BookManagerServlet.updateBook()→重定向显示列表功能

##### ② 修改图书超链接

```html
<a th:href="@{/bookManager(method='toEditPage',id=${book.bookId})}">修改</a>
```

##### ③ BookManagerServlet.toEditPage()

```java
/**
     * 跳转到修改页面
     * @param request
     * @param response
     * @throws IOException
     */
public void toEditPage(HttpServletRequest request, HttpServletResponse response) throws IOException{
    //获取客户端传入的id
    Integer id = Integer.valueOf(request.getParameter("id"));
    try {
        //根据id查询图书详情
        Book book = bookService.getBookById(id);
        //将图书信息存储到请求域
        request.setAttribute("book",book);
        processTemplate("manager/book_edit",request,response);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

##### ④ BookService.getBookById()

```java
@Override
public Book getBookById(Integer bookId) throws Exception{

    return bookDao.selectBookByPrimaryKey(bookId);
}
```

##### ⑤ BookDao.selectBookByPrimaryKey()

```java
@Override
public Book selectBookByPrimaryKey(Integer bookId) throws SQLException {
    String sql = "select book_id bookId,book_name bookName,author,price,sales,stock,img_path imgPath from t_book where book_id=?";

    return getBean(Book.class,sql,bookId);
}
```

##### ⑥ book_edit.html（表单回显）

```html
<!--修改图书的表单-->
<form action="bookManager?method=saveOrUpdateBook" method="post" th:if="${book != null}">
    <div class="form-item">
        <!--使用隐藏域绑定bookId-->
        <input type="hidden" name="bookId" th:value="${book.bookId}"/>
        <div>
            <label>名称:</label>
            <input type="text" th:value="${book.bookName}" placeholder="请输入名称" name="bookName" />
        </div>
        <span class="errMess" style="visibility: hidden;">请输入正确的名称</span
            >
    </div>
    <div class="form-item">
        <div>
            <label>价格:</label>
            <input type="number" th:value="${book.price}" placeholder="请输入价格" name="price" />
        </div>
        <span class="errMess">请输入正确数字</span>
    </div>
    <div class="form-item">
        <div>
            <label>作者:</label>
            <input type="text" th:value="${book.author}" placeholder="请输入作者" name="author"/>
        </div>
        <span class="errMess">请输入正确作者</span>
    </div>
    <div class="form-item">
        <div>
            <label>销量:</label>
            <input type="number" th:value="${book.sales}" placeholder="请输入销量" name="sales" />
        </div>
        <span class="errMess">请输入正确销量</span>
    </div>
    <div class="form-item">
        <div>
            <label>库存:</label>
            <input type="number" th:value="${book.stock}" placeholder="请输入库存" name="stock"/>
        </div>
        <span class="errMess">请输入正确库存</span>
    </div>

    <button type="submit" class="btn">提交</button>
</form>

<!--这是添加图书的表单-->
<form action="bookManager?method=saveOrUpdateBook" method="post" th:unless="${book != null}">
    <div class="form-item">
        <div>
            <label>名称:</label>
            <input type="text" placeholder="请输入名称" name="bookName" />
        </div>
        <span class="errMess" style="visibility: hidden;">请输入正确的名称</span
            >
    </div>
    <div class="form-item">
        <div>
            <label>价格:</label>
            <input type="number" placeholder="请输入价格" name="price" />
        </div>
        <span class="errMess">请输入正确数字</span>
    </div>
    <div class="form-item">
        <div>
            <label>作者:</label>
            <input type="text" placeholder="请输入作者" name="author"/>
        </div>
        <span class="errMess">请输入正确作者</span>
    </div>
    <div class="form-item">
        <div>
            <label>销量:</label>
            <input type="number" placeholder="请输入销量" name="sales" />
        </div>
        <span class="errMess">请输入正确销量</span>
    </div>
    <div class="form-item">
        <div>
            <label>库存:</label>
            <input type="number" placeholder="请输入库存" name="stock"/>
        </div>
        <span class="errMess">请输入正确库存</span>
    </div>

    <button type="submit" class="btn">提交</button>
</form>
```

##### ⑦ BookManagerServlet.saveOrUpdateBook()

```java
/**
     * 添加或者图书信息
     * @param request
     * @param response
     * @throws IOException
     */
public void saveOrUpdateBook(HttpServletRequest request, HttpServletResponse response) throws IOException{
    //1. 获取请求参数
    Map<String, String[]> parameterMap = request.getParameterMap();
    //2. 将parameterMap中的数据封装到Book对象
    try {
        Book book = new Book();
        BeanUtils.populate(book,parameterMap);

        //判断到底是修改还是添加
        if (book.getBookId() != null && !"".equals(book.getBookId())) {
            //修改图书信息
            bookService.editBook(book);
        }else {
            //添加图书信息
            //设置一个固定的imgPath
            book.setImgPath("static/uploads/xiaowangzi.jpg");
            //3. 调用业务层的方法保存图书信息
            bookService.saveBook(book);
        }

        //4. 保存成功，则重新查询所有图书
        response.sendRedirect(request.getContextPath()+"/bookManager?method=toBookManagerPage");
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

##### ⑧ BookService.editBook()

```java
@Override
public void editBook(Book book) throws Exception {
    bookDao.updateBook(book);
}
```

##### ⑨ BookDao.updateBook()

<span style="color:blue;font-weight:bold;">注意</span>：这里不修改imgPath字段

```java
@Override
public void updateBook(Book book) throws SQLException {
    //我们暂时不修改图片路径
    String sql = "update t_book set book_name=?,price=?,author=?,sales=?,stock=? where book_id=?";

    update(sql,book.getBookName(),book.getPrice(),book.getAuthor(),book.getSales(),book.getStock(),book.getBookId());
}
```

## 4. 前台图书展示

### 4.1 思路

index.html→PortalServlet.doPost()→把图书列表数据查询出来→渲染视图→页面迭代显示图书数据

### 4.2 代码

#### 4.2.1 PortalServlet.doPost()

```java
package com.atguigu.servlet.model;

import com.atguigu.bean.Book;
import com.atguigu.service.BookService;
import com.atguigu.service.impl.BookServiceImpl;
import com.atguigu.servlet.base.ViewBaseServlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.util.List;

/**
 * @author chenxin
 * 日期2021-06-14  09:03
 * 该Servlet只需要处理访问首页
 */
public class PortalServlet extends ViewBaseServlet {
    private BookService bookService = new BookServiceImpl();
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        try {
            //查询动态数据
            List<Book> bookList = bookService.getBookList();
            //将动态数据存储到请求域
            request.setAttribute("bookList",bookList);
            processTemplate("index",request,response);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### 4.2.2 页面迭代显示图书数据

页面文件：index.html

```html
<div class="books-list ">
    <div class="w">
        <div class="list">
            <div class="list-header">
                <div class="title">图书列表</div>
                <div class="price-search">
                    <span>价格:</span>
                    <input type="text">
                    <span>-元</span>
                    <input type="text">
                    <span>元</span>
                    <button>查询</button>
                </div>
            </div>
            <div class="list-content">
                <div class="list-item" th:each="book : ${bookList}">
                    <img th:src="${book.imgPath}" alt="">
                    <p th:text="${book.bookName}">书名:活着</p>
                    <p th:text="${book.author}">作者:余华</p>
                    <p th:text="${book.price}">价格:￥66.6</p>
                    <p th:text="${book.sales}">销量:230</p>
                    <p th:text="${book.stock}">库存:1000</p>
                    <button>加入购物车</button>
                </div>

            </div>
            <div class="list-footer">
                <div>首页</div>
                <div>上一页</div>
                <ul><li class="active">1</li><li>2</li><li>3</li></ul>
                <div>下一页</div>
                <div>末页</div>
                <span>共10页</span>
                <span>30条记录</span>
                <span>到第</span>
                <input type="text">
                <span>页</span>
                <button>确定</button>
            </div>
        </div>
    </div>

</div>
```



# 第六章 会话&书城项目第四阶段

### 学习目标

* 了解为什么需要会话控制
* 了解会话的范围
* 掌握使用Cookie
* 掌握使用Session
* 完成书城项目第四阶段

### 1.会话

#### 1.1 为什么需要会话控制

![image-20220829224106007](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224106007.png)

保持用户登录状态，就是当用户在登录之后，会在服务器中保存该用户的登录状态，当该用户后续访问该项目中的其它动态资源(Servlet或者Thymeleaf)的时候，能够判断当前是否是已经登录过的。而从用户登录到用户退出登录这个过程中所发生的所有请求，其实都是在一次会话范围之内

#### 1.2 域对象的范围

##### 1.2.1 应用域的范围

![image-20220829224113391](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224113391.png)

整个项目部署之后，只会有一个应用域对象，所有客户端都是共同访问同一个应用域对象，在该项目的所有动态资源中也是共用一个应用域对象

##### 1.2.2 请求域的范围

![image-20220829224120892](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224120892.png)
每一次请求都有一个请求域对象，当请求结束的时候对应的请求域对象也就销毁了

##### 1.2.3 会话域的范围

![image-20220829224130041](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224130041.png)

会话域是从客户端连接上服务器开始，一直到客户端关闭，这一整个过程中发生的所有请求都在同一个会话域中；而不同的客户端是不能共用会话域的

#### 1.3 Cookie技术

##### 1.3.1 Cookie的概念

Cookie是一种客户端的会话技术,它是服务器存放在浏览器的一小份数据,浏览器以后每次访问该服务器的时候都会将这小份数据携带到服务器去。

<img src="https://raw.githubusercontent.com/Young-Allen/pic/main/img016.png" style="zoom:67%;" />

##### 1.3.2 Cookie的作用

1. 在浏览器中存放数据
2. 将浏览器中存放的数据携带到服务器

##### 1.3.3 Cookie的应用场景

1.记住用户名
当我们在用户名的输入框中输入完用户名后,浏览器记录用户名,下一次再访问登录页面时,用户名自动填充到用户名的输入框.

2.保存电影的播放进度  

在网页上播放电影的时候,如果中途退出浏览器了,下载再打开浏览器播放同一部电影的时候,会自动跳转到上次退出时候的进度,因为在播放的时候会将播放进度保存到cookie中

##### 1.3.4 Cookie的入门案例

###### ① 目标

实现在ServletDemo01和ServletDemo02之间共享数据，要求在会话域范围内共享

###### ② Cookie相关的API

+ 创建一个Cookie对象(cookie只能保存字符串数据。且不能保存中文)

```java
new Cookie(String name,String value);
```

+  把cookie写回浏览器

```java
response.addCookie(cookie); 
```

+ 获得浏览器带过来的所有Cookie:

```java
request.getCookies() ; //得到所有的cookie对象。是一个数组，开发中根据key得到目标cookie
```

+ cookie的 API

```java
cookie.getName() ; //返回cookie中设置的key
cookie.getValue(); //返回cookie中设置的value
```

###### ③ ServletDemo01代码

在ServletDemo01中创建Cookie数据并响应给客户端

```java
public class ServletDemo01 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 创建一个cookie对象，用于存放键值对
        Cookie cookie = new Cookie("cookie-message","hello-cookie");

        //2. 将cookie添加到response中
        //底层是通过一个名为"Set-Cookie"的响应头携带到浏览器的
        response.addCookie(cookie);
    }
}
```

![image-20220829224142103](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224142103.png)

###### ④ 浏览器发送请求携带Cookie

这里不需要我们操作，浏览器会在给服务器发送请求的时候，将cookie通过请求头自动携带到服务器

![image-20220829224147987](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224147987.png)

###### ⑤ ServletDemo02获取Cookie数据的代码

```java
public class ServletDemo02 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 从请求中取出cookie
        //底层是由名为"Cookie"的请求头携带的
        Cookie[] cookies = request.getCookies();

        //2. 遍历出每一个cookie
        if (cookies != null) {
            for (Cookie cookie : cookies) {
                //匹配cookie的name
                if (cookie.getName().equals("cookie-message")) {
                    //它就是我们想要的那个cookie
                    //我们就获取它的value
                    String value = cookie.getValue();
                    System.out.println("在ServletDemo02中获取str的值为：" + value);
                }
            }
        }
    }
}
```

##### 1.3.5 Cookie的时效性

如果我们不设置Cookie的时效性，默认情况下Cookie的有效期是一次会话范围内，我们可以通过cookie的setMaxAge()方法让Cookie持久化保存到浏览器上

- 会话级Cookie
  - 服务器端并没有明确指定Cookie的存在时间
  - 在浏览器端，Cookie数据存在于内存中
  - 只要浏览器还开着，Cookie数据就一直都在
  - 浏览器关闭，内存中的Cookie数据就会被释放
- 持久化Cookie
  - 服务器端明确设置了Cookie的存在时间
  - 在浏览器端，Cookie数据会被保存到硬盘上
  - Cookie在硬盘上存在的时间根据服务器端限定的时间来管控，不受浏览器关闭的影响
  - 持久化Cookie到达了预设的时间会被释放

`cookie.setMaxAge(int expiry)`参数单位是秒，表示cookie的持久化时间，如果设置参数为0，表示将浏览器中保存的该cookie删除

##### 1.3.6 Cookie的path

上网时间长了，本地会保存很多Cookie。对浏览器来说，访问互联网资源时不能每次都把所有Cookie带上。浏览器会使用Cookie的path属性值来和当前访问的地址进行比较，从而决定是否携带这个Cookie。

我们可以通过调用cookie的setPath()方法来设置cookie的path

![image-20220829224200624](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224200624.png)

#### 1.4 Session技术

##### 1.4.1 session概述 

session是服务器端的技术。服务器为每一个浏览器开辟一块内存空间，即session对象。由于session对象是每一个浏览器特有的，所以用户的记录可以存放在session对象中

##### 1.4.2 Session的入门案例

###### ① 目标

实现在ServletDemo01和ServletDemo02之间共享数据，要求在会话域范围内共享

###### ② Session的API介绍

- request.getSession(); 获得session(如果第一次调用的时候其实是创建session,第一次之后通过sessionId找到session进行使用)
- Object getAttribute(String name) ;获取值
- void setAttribute(String name, Object value) ;存储值
- void removeAttribute(String name)  ;移除值

###### ③ 在ServletDemo01中往Session域对象存储数据

```java
public class ServletDemo01 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 获取Session对象
        HttpSession session = request.getSession();
        //2. 往Session对象中存入数据
        session.setAttribute("session-message","hello-session");
    }
}
```

###### ④ 在ServletDemo02中从Session域对象中获取数据

```java
public class ServletDemo02 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //1. 获取Session对象
        HttpSession session = request.getSession();
        //2. 往Session对象中存入数据
        String message = (String)session.getAttribute("session-message");
        System.out.println(message);
    }
}
```

##### 1.4.3 Session的工作机制

前提：浏览器正常访问服务器

- 服务器端没调用request.getSession()方法：什么都不会发生
- 服务器端调用了request.getSession()方法
  - 服务器端检查当前请求中是否携带了JSESSIONID的Cookie
    - 有：根据JSESSIONID在服务器端查找对应的HttpSession对象
      - 能找到：将找到的HttpSession对象作为request.getSession()方法的返回值返回
      - 找不到：服务器端新建一个HttpSession对象作为request.getSession()方法的返回值返回
    - 无：服务器端新建一个HttpSession对象作为request.getSession()方法的返回值返回

![image-20220829224214240](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224214240.png)

**代码验证**

```java
// 1.调用request对象的方法尝试获取HttpSession对象
HttpSession session = request.getSession();

// 2.调用HttpSession对象的isNew()方法
boolean wetherNew = session.isNew();

// 3.打印HttpSession对象是否为新对象
System.out.println("wetherNew = " + wetherNew+"HttpSession对象是新的":"HttpSession对象是旧的"));

// 4.调用HttpSession对象的getId()方法
String id = session.getId();

// 5.打印JSESSIONID的值
System.out.println("JSESSIONID = " + id);
```

##### 1.4.4 Session的时效性

###### ① 为什么Session要设置时限

用户量很大之后，Session对象相应的也要创建很多。如果一味创建不释放，那么服务器端的内存迟早要被耗尽。

###### ② 设置时限的难点

从服务器端的角度，很难精确得知类似浏览器关闭的动作。而且即使浏览器一直没有关闭，也不代表用户仍然在使用。

###### ③ 服务器端给Session对象设置最大闲置时间

- 默认值：1800秒

![image-20220829224222933](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224222933.png)

最大闲置时间生效的机制如下：

![image-20220829224228846](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224228846.png)

###### ④ 代码验证

```java
// ※测试时效性
// 获取默认的最大闲置时间
int maxInactiveIntervalSecond = session.getMaxInactiveInterval();
System.out.println("maxInactiveIntervalSecond = " + maxInactiveIntervalSecond);

// 设置默认的最大闲置时间
session.setMaxInactiveInterval(15);
```

###### ⑤ 强制Session立即失效

```java
session.invalidate();
```

## 2. 书城项目第四阶段

### 2.1 保持登录状态

#### 2.1.1 迁移项目

创建一个新Module，将旧Module中的内容迁移

- 迁移src目录下的Java代码
- 迁移web目录下的static目录
- 迁移web/WEB-INF目录下的lib目录和pages目录
- 将lib目录下的jar包添加到运行时环境
- 将旧的web.xml中的配置复制到新module的web.xml中

#### 2.1.2 将登录成功的User存入Session中

![image-20220829224236846](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224236846.png)

```java
/**
     * 处理登录校验
     * @param request
     * @param response
     * @throws IOException
     */
public void doLogin(HttpServletRequest request, HttpServletResponse response) throws IOException {
    //还是做原来的登录校验
    //1. 获取客户端传入的请求参数
    Map<String, String[]> parameterMap = request.getParameterMap();

    //2. 将username和password封装到User对象
    User user = new User();

    //3. 调用业务层的方法处理登录
    try {
        BeanUtils.populate(user,parameterMap);

        //登录，获取登录的用户信息
        User loginUser = userService.doLogin(user);
        //将loginUser对象存储到会话域对象
        request.getSession().setAttribute("loginUser",loginUser);

        //没有出现异常，说明登录成功，那么跳转到登录成功页面
        processTemplate("user/login_success",request,response);
    } catch (Exception e) {
        e.printStackTrace();
        //出现异常表示登录失败，则往域对象中存储登录失败的信息
        request.setAttribute("errorMessage","登录失败,"+e.getMessage());
        //跳转到登录页面，显示登录失败的信息
        processTemplate("user/login",request,response);
    }
}
```

#### 2.1.3 修改欢迎信息

##### ① 登录成功页面

```html
<span>欢迎<span class="um_span" th:text="${session.loginUser.username}">张总</span>光临尚硅谷书城</span>
```

##### ② 首页

```html
<div class="topbar-right" th:if="${session.loginUser == null}">
    <a href="user?method=toLoginPage" class="login">登录</a>
    <a href="user?method=toRegisterPage" class="register">注册</a>
    <a
       href="cart/cart.html"
       class="cart iconfont icon-gouwuche
              "
       >
        购物车
        <div class="cart-num">3</div>
    </a>
    <a href="admin?method=toManagerPage" class="admin">后台管理</a>
</div>
<!--登录后风格-->
<div class="topbar-right" th:unless="${session.loginUser == null}">
    <span>欢迎你<b th:text="${session.loginUser.username}">张总</b></span>
    <a href="#" class="register">注销</a>
    <a
       href="pages/cart/cart.jsp"
       class="cart iconfont icon-gouwuche
              ">
        购物车
        <div class="cart-num">3</div>
    </a>
    <a href="admin?method=toManagerPage" class="admin">后台管理</a>
</div>
```

### 2.2 退出登录功能

#### 2.2.1 目标

用户退出登录的时候，清除会话域中保存的当前用户的所有信息

#### 2.2.2 页面超链接

```html
<a href="user?method=logout" class="register">注销</a>
```

#### 2.2.3 UserServlet.logout()

```java
/**
     * 退出登录
     * @param request
     * @param response
     * @throws IOException
     */
public void logout(HttpServletRequest request, HttpServletResponse response) throws IOException {
    //1. 立即让本次会话失效
    request.getSession().invalidate();

    //2. 跳转到PortalServlet访问首页
    response.sendRedirect(request.getContextPath()+"/index.html");
}
```

### 2.3 验证码

#### 2.3.1 目标

通过让用户填写验证码并在服务器端检查，防止浏览器端使用程序恶意访问。

#### 2.3.2 思路

![images](https://raw.githubusercontent.com/Young-Allen/pic/main/img02.png)

#### 2.3.3 操作

##### ① 导入jar包

kaptcha-2.3.2.jar

##### ② 配置KaptchaServlet

jar包中已经写好了Servlet的Java类，我们只需要在web.xml中配置这个Servlet即可。

```xml
<servlet>
    <servlet-name>KaptchaServlet</servlet-name>
    <servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>KaptchaServlet</servlet-name>
    <url-pattern>/KaptchaServlet</url-pattern>
</servlet-mapping>
```

##### ③ 通过页面访问测试

> http://localhost:8080/bookstore/KaptchaServlet

##### ④ 在注册页面显示验证码图片

![images](https://raw.githubusercontent.com/Young-Allen/pic/main/img04.png)

```html
<img src="KaptchaServlet" alt="" />
```

##### ⑤ 调整验证码图片的显示效果

a. 去掉边框

KaptchaServlet会在初始化时读取init-param，而它能够识别的init-param在下面类中：

> com.google.code.kaptcha.util.Config

web.xml中具体配置如下：

```xml
<servlet>
    <servlet-name>KaptchaServlet</servlet-name>
    <servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>

    <!-- 通过配置初始化参数影响KaptchaServlet的工作方式 -->
    <!-- 可以使用的配置项参考com.google.code.kaptcha.util.Config类 -->
    <!-- 配置kaptcha.border的值为no取消图片边框 -->
    <init-param>
        <param-name>kaptcha.border</param-name>
        <param-value>no</param-value>
    </init-param>
</servlet>
<servlet-mapping>
    <servlet-name>KaptchaServlet</servlet-name>
    <url-pattern>/KaptchaServlet</url-pattern>
</servlet-mapping>
```

> 开发过程中的工程化细节：
>
> no、false、none等等单词从含义上来说都表示『没有边框』这个意思，但是这里必须使用no。
>
> 参考的依据是下面的源码：

```java
public boolean getBoolean(String paramName, String paramValue, boolean defaultValue) {
	boolean booleanValue;
	if (!"yes".equals(paramValue) && !"".equals(paramValue) && paramValue != null) {
		if (!"no".equals(paramValue)) {
			throw new ConfigException(paramName, paramValue, "Value must be either yes or no.");
		}

		booleanValue = false;
	} else {
		booleanValue = defaultValue;
	}

	return booleanValue;
}
```

b. 设置图片大小

```html
<img style="width: 150px; height: 40px;" src="KaptchaServlet" alt="" />
```

##### ⑥ 点击图片刷新

a. 目的

验证码图片都是经过刻意扭曲、添加了干扰、角度偏转，故意增加了识别的难度。所以必须允许用户在看不出来的时候点击图片刷新，生成新的图片重新辨认。

b. 实现的代码

修改图片的img标签：

```html
<img :src="checkCodePath" width="126" height="35" alt="" @click="changeCode"/>
```

Vue代码：定义数据模型

```javascript
"data":{
          "username":"[[${param.username}]]",//用户名
          "password":"",//密码
          "passwordConfirm":"",//确认密码
          "email":"[[${param.email}]]",//邮箱
          "code":"",//验证码
          "usernameErrorMessage":"",
          "passwordErrorMessage":"",
          "confirmErrorMessage":"",
          "emailErrorMessage":"",
          "checkCodePath":"kaptcha"
      }
```

Vue代码:定义刷新验证码的函数

```javascript
changeCode(){
    //切换验证码，其实就是重新设置img标签的src
    this.checkCodePath = "kaptcha?time="+new Date()
}
```



##### ⑦ 执行注册前检查验证码

![image-20220829224256434](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224256434.png)

a. 确认KaptchaServlet将验证码存入Session域时使用的属性名

![images](https://raw.githubusercontent.com/Young-Allen/pic/main/img06.png)

通过查看源码，找到验证码存入Session域时使用的属性名是：

> KAPTCHA_SESSION_KEY

**当然我们也可以通过初始化参数配置验证码存入Session域时候使用的属性名**

如果配置了初始化参数指定了存入Session域时候使用的属性名，那么就不能使用默认的属性名"KAPTCHA_SESSION_KEY"了

```xml
<!--配置KaptchaServlet的映射路径-->
<servlet>
    <servlet-name>KaptchaServlet</servlet-name>
    <servlet-class>com.google.code.kaptcha.servlet.KaptchaServlet</servlet-class>

    <!--配置初始化参数-->
    <init-param>
        <!--固定写法-->
        <param-name>kaptcha.session.key</param-name>

        <!--这个值就是往session域对象中存储验证码时候的key-->
        <param-value>checkCode</param-value>
    </init-param>
</servlet>
```

b. 在执行注册的方法中添加新的代码

```java
/**
     * 处理注册请求
     * @param request
     * @param response
     * @throws IOException
     */
public void doRegister(HttpServletRequest request, HttpServletResponse response) throws IOException {
    try {
        //1. 获取请求参数
        Map<String, String[]> parameterMap = request.getParameterMap();

        //获取用户输入的验证码
        String code = parameterMap.get("code")[0];
        //从session总获取服务器生成的验证码
        String checkCode = (String) request.getSession().getAttribute("KAPTCHA_SESSION_KEY");
        //校验验证码:忽略大小写
        if (checkCode.equalsIgnoreCase(code)) {
            //验证码正确,才进行注册
            //2. 使用BeanUtils将parameterMap中的数据封装到User对象
            User user = new User();
            BeanUtils.populate(user,parameterMap);
            //3. 调用业务层的方法处理注册业务
            userService.doRegister(user);

            //没有异常，就是注册成功
            //跳转到注册成功页面
            processTemplate("user/regist_success",request,response);
        }else {
            //验证码错误
            throw new RuntimeException("验证码错误");
        }
    } catch (Exception e) {
        e.printStackTrace();
        //有异常就注册失败,往域对象中存入失败信息
        request.setAttribute("errorMessage","注册失败,"+e.getMessage());
        //跳转回到注册页面
        processTemplate("user/regist",request,response);
    }
}
```



# 第七章 Ajax&Axios&书城项目第五阶段

## 学习目标

* 了解服务器渲染和Ajax渲染的区别
* 了解同步和异步的区别
* 了解Axios
* 掌握Axios发送异步请求
* 掌握Axios携带json类型的请求参数
* 掌握服务器端返回json数据

### 1. Ajax

#### 1.1 服务器端渲染

![image-20220829224313113](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224313113.png)

#### 1.2 Ajax渲染（局部更新）

![image-20220829224319449](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224319449.png)

#### 1.3 前后端分离

真正的前后端分离是前端项目和后端项目分服务器部署，在我们这里我们先理解为彻底舍弃服务器端渲染，数据全部通过Ajax方式以JSON格式来传递

#### 1.4 同步与异步

Ajax本身就是Asynchronous JavaScript And XML的缩写，直译为：异步的JavaScript和XML。在实际应用中Ajax指的是：<span style="color:blue;font-weight:bold;">不刷新浏览器窗口</span>，<span style="color:blue;font-weight:bold;">不做页面跳转</span>，<span style="color:blue;font-weight:bold;">局部更新页面内容</span>的技术。

<span style="color:blue;font-weight:bold;">『同步』</span>和<span style="color:blue;font-weight:bold;">『异步』</span>是一对相对的概念，那么什么是同步，什么是异步呢？

##### 1.4.1 同步

多个操作<span style="color:blue;font-weight:bold;">按顺序执行</span>，前面的操作没有完成，后面的操作就必须<span style="color:blue;font-weight:bold;">等待</span>。所以同步操作通常是<span style="color:blue;font-weight:bold;">串行</span>的。

![image-20220829224330191](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224330191.png)

##### 1.4.2 异步

多个操作相继开始<span style="color:blue;font-weight:bold;">并发执行</span>，即使开始的先后顺序不同，但是由于它们各自是<span style="color:blue;font-weight:bold;">在自己独立的进程或线程中</span>完成，所以<span style="color:blue;font-weight:bold;">互不干扰</span>，<span style="color:blue;font-weight:bold;">谁也<span style="color:red;font-weight:bold;">不用等</span>谁</span>。

![image-20220829224339944](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224339944.png)

### 2. Axios

#### 2.1 Axios简介

使用原生的JavaScript程序执行Ajax极其繁琐，所以一定要使用框架来完成。而Axios就是目前最流行的前端Ajax框架。

Axios官网：http://www.axios-js.com/

使用Axios和使用Vue一样，导入对应的*.js文件即可。官方提供的script标签引入方式为：

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

我们可以把这个axios.min.js文件下载下来保存到本地来使用。

#### 2.2 Axios基本用法

##### 2.2.1 在前端页面引入开发环境

```html
<script type="text/javascript" src="/demo/static/vue.js"></script>
<script type="text/javascript" src="/demo/static/axios.min.js"></script>
```

##### 2.2.2 发送普通请求参数

###### ① 前端代码

HTML标签：

```javascript
<div id="app">
    <button @click="commonParam">普通请求参数</button>
</div>
```

Vue+axios代码：

```javascript
var vue = new Vue({
    "el":"#app",
    "data":{
        "message":""
    },
    "methods":{
        commonParam(){
            //使用axios发送异步请求
            axios({
                "method":"post",
                "url":"demo01",
                "params":{
                    "userName":"tom",
                    "userPwd":"123456"
                }
            }).then(response => {
                //then里面是处理请求成功的响应数据
                //response就是服务器端的响应数据,是json类型的
                //response里面的data就是响应体的数据
                this.message = response.data
            }).catch(error => {
                //error是请求失败的错误描述
                console.log(error)
            })
        }
    }
})
</script>
```

 **注意：所有请求参数都被放到URL地址后面了，哪怕我们现在用的是POST请求方式。**

###### ② 后端代码

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;


public class ServletDemo01 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("UTF-8");
        response.setContentType("text/html;charset=UTF-8");
        //1. 接收请求参数userName和userPwd
        String userName = request.getParameter("userName");
        String userPwd = request.getParameter("userPwd");
        System.out.println(userName + ":" + userPwd);

        //模拟出现异常
        //int num = 10/0;

        //2. 向浏览器响应数据
        response.getWriter().write("hello world!!!");
    }
}
```

###### ③ 服务器端处理请求失败后

```javascript
catch(error => {     // catch()服务器端处理请求出错后，会调用
    console.log(error);         // error就是出错时服务器端返回的响应数据
});
```

在给catch()函数传入的回调函数中，error对象封装了服务器端处理请求失败后相应的错误信息。其中，axios封装的响应数据对象，是error对象的response属性。response属性对象的结构如下图所示：

![image-20220829224356722](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224356722.png)

可以看到，response对象的结构还是和then()函数传入的回调函数中的response是一样的：

> 回调函数：开发人员声明，但是调用时交给系统来调用。像单击响应函数、then()、catch()里面传入的都是回调函数。回调函数是相对于普通函数来说的，普通函数就是开发人员自己声明，自己调用：
>
> ```javascript
> function sum(a, b) {
> return a+b;
> }
> 
> var result = sum(3, 2);
> console.log("result="+result);
> ```

#### 2.3 发送请求体JSON

##### 2.3.1 前端代码

HTML代码：

```html
<button @click="sendJsonBody()">请求体JSON</button>
```

Vue+axios代码：

```javascript
<script>
    var vue = new Vue({
        "el":"#app",
        "data":{
            "message":""
        },
        "methods":{
            sendJsonBody(){
                //使用axios发送异步请求，要携带Json请求体的参数
                axios({
                    "method":"post",
                    "url":"demo01",
                    //携带Json请求体参数
                    "data":{
                        "userName":"aobama",
                        "userPwd":"999999"
                    }
                }).then(response => {
                    this.message = response.data
                })
            }
        }
    })
</script>
```

##### 2.3.2 后端代码

###### ① 加入Gson包

Gson是Google研发的一款非常优秀的<span style="color:blue;font-weight:bold;">JSON数据解析和生成工具</span>，它可以帮助我们将数据在JSON字符串和Java对象之间互相转换。

![image-20220829224404583](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224404583.png)

###### ② User类

```java
package com.atguigu.bean;


public class User {
    private String userName;
    private String userPwd;

    public User() {
    }

    public User(String userName, String userPwd) {
        this.userName = userName;
        this.userPwd = userPwd;
    }

    @Override
    public String toString() {
        return "User{" +
                "userName='" + userName + '\'' +
                ", userPwd='" + userPwd + '\'' +
                '}';
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getUserPwd() {
        return userPwd;
    }

    public void setUserPwd(String userPwd) {
        this.userPwd = userPwd;
    }
}
```

###### ③ Servlet代码

```java
package com.atguigu.servlet;

import com.atguigu.bean.User;
import com.google.gson.Gson;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.BufferedReader;
import java.io.IOException;


public class ServletDemo01 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        try {
            response.setContentType("text/html;charset=UTF-8");
            //request.getParameter(name),request.getParameterValues(name),request.getParameterMap()这仨方法只能获取普通参数
            //什么是普通参数:1. 地址后面携带的参数  2. 表单提交的参数
            /*String userName = request.getParameter("userName");
            String userPwd = request.getParameter("userPwd");
            System.out.println("客户端传入的参数userName的值为:" + userName + ",传入的userPwd的值为:" + userPwd);*/

            //要获取Json请求体的参数，就必须得进行Json解析:可用来做Json解析的工具jar包有gson、fastjson、jackson(SpringMVC以及SpringBoot默认支持的)
            //做json解析其实就是:1. 将Java对象转成json字符串  2. 将json字符串转成Java对象

            //我们要获取json请求体的参数，其实就是将json请求体的参数封装到User对象中
            //1. 获取Json请求体的内容
            BufferedReader requestReader = request.getReader();
            //2. 从requestReader中循环读取拼接字符串
            StringBuilder stringBuilder = new StringBuilder();
            String buffer = "";
            while ((buffer = requestReader.readLine()) != null) {
                stringBuilder.append(buffer);
            }

            //3. 将stringBuilder转成字符串，这个字符串就是Json请求体
            String jsonBody = stringBuilder.toString();
            //4. 将jsonBody通过Json解析转成User对象
            Gson gson = new Gson();
            User user = gson.fromJson(jsonBody, User.class);

            System.out.println("客户端传入的参数userName的值为:" + user.getUserName() + ",传入的userPwd的值为:" + user.getUserPwd());
            //模拟服务器出现异常
            //int num = 10/0;

            response.getWriter().write("你好世界");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

> P.S.：看着很麻烦是吧？别担心，将来我们有了<span style="color:blue;font-weight:bold;">SpringMVC</span>之后，一个<span style="color:blue;font-weight:bold;">@RequestBody</span>注解就能够搞定，非常方便！

#### 2.4 服务器端返回JSON数据

##### 2.4.1 前端代码

```javascript
sendJsonBody(){
    //使用axios发送异步请求，要携带Json请求体的参数
    axios({
        "method":"post",
        "url":"demo01",
        //携带Json请求体参数
        "data":{
            "userName":"aobama",
            "userPwd":"999999"
        }
    }).then(response => {
        //目标是获取响应数据中的用户的用户名或者密码
        this.message = response.data.userName
    })
}
```

##### 2.4.2 后端代码

###### ① 加入Gson包

仍然需要Gson支持，不用多说

![image-20220829224417440](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224417440.png)

###### ② Servlet代码

```java
package com.atguigu.servlet;

import com.atguigu.bean.User;
import com.google.gson.Gson;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.BufferedReader;
import java.io.IOException;


public class ServletDemo01 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        try {
            response.setContentType("text/html;charset=UTF-8");
            //request.getParameter(name),request.getParameterValues(name),request.getParameterMap()这仨方法只能获取普通参数
            //什么是普通参数:1. 地址后面携带的参数  2. 表单提交的参数
            /*String userName = request.getParameter("userName");
            String userPwd = request.getParameter("userPwd");
            System.out.println("客户端传入的参数userName的值为:" + userName + ",传入的userPwd的值为:" + userPwd);*/

            //要获取Json请求体的参数，就必须得进行Json解析:可用来做Json解析的工具jar包有gson、fastjson、jackson(SpringMVC以及SpringBoot默认支持的)
            //做json解析其实就是:1. 将Java对象转成json字符串  2. 将json字符串转成Java对象

            //我们要获取json请求体的参数，其实就是将json请求体的参数封装到User对象中
            //1. 获取Json请求体的内容
            BufferedReader requestReader = request.getReader();
            //2. 从requestReader中循环读取拼接字符串
            StringBuilder stringBuilder = new StringBuilder();
            String buffer = "";
            while ((buffer = requestReader.readLine()) != null) {
                stringBuilder.append(buffer);
            }

            //3. 将stringBuilder转成字符串，这个字符串就是Json请求体
            String jsonBody = stringBuilder.toString();
            //4. 将jsonBody通过Json解析转成User对象
            Gson gson = new Gson();
            User user = gson.fromJson(jsonBody, User.class);

            System.out.println("客户端传入的参数userName的值为:" + user.getUserName() + ",传入的userPwd的值为:" + user.getUserPwd());
            //模拟服务器出现异常
            //int num = 10/0;

            //服务器端向客户端响应普通字符串
            //response.getWriter().write("你好世界");

            //在实际开发中服务器端向客户端响应的99%都会是Json字符串
            User responseUser = new User("周杰棍","ggggggg");

            //将responseUser转成json字符串
            String responseJson = gson.toJson(responseUser);

            response.getWriter().write(responseJson);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 3. 书城项目第五阶段

#### 3.1 注册页面用户名唯一性检查优化

##### 3.1.1 准备工作同步请求

- 复制module

##### 3.1.2 加入Ajax开发环境

###### ①  前端所需axios库

![image-20220829224426305](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224426305.png)

###### ② 后端所需jackson库

##### 3.1.3 拷贝Json工具类

#### 3.2 封装CommonResult

##### 3.1 模型的作用

在整个项目中，凡是涉及到给Ajax请求返回响应，我们都封装到CommonResult类型中。

##### 3.2 模型的代码

```java
package com.atguigu.bean;


public class CommonResult {
    /**
     * 服务器端处理请求的标示
     */
    private boolean flag;

    /**
     * 当服务器端处理请求成功的时候要显示给客户端的数据(主要针对于查询)
     */
    private Object resultData;
    
    /**
     * 当服务器端处理请求失败的时候要响应给客户端的错误信息
     */
    private String message;

    /**
     * 处理请求成功
     * @return
     */
    public static CommonResult ok(){
        return new CommonResult().setFlag(true);
    }

    /**
     * 处理请求失败
     * @return
     */
    public static CommonResult error(){
        return new CommonResult().setFlag(false);
    }

    public boolean isFlag() {
        return flag;
    }

    private CommonResult setFlag(boolean flag) {
        this.flag = flag;
        return this;
    }

    public Object getResultData() {
        return resultData;
    }

    public CommonResult setResultData(Object resultData) {
        this.resultData = resultData;
        return this;
    }

    public String getMessage() {
        return message;
    }

    public CommonResult setMessage(String message) {
        this.message = message;
        return this;
    }

    @Override
    public String toString() {
        return "CommonResult{" +
                "flag=" + flag +
                ", resultData=" + resultData +
                ", message='" + message + '\'' +
                '}';
    }
}
```

各个属性的含义：

| 属性名     | 含义                                             |
| ---------- | ------------------------------------------------ |
| flag       | 服务器端处理请求的结果，取值为true或者false      |
| message    | 服务器端处理请求失败之后，要响应给客户端的数据   |
| resultData | 服务器端处理请求成功之后，需要响应给客户端的数据 |



##### 3.3 模型的好处

- 作为整个团队开发过程中，前后端交互时使用的统一的数据格式
- 有利于团队成员之间的协助，提高开发效率



#### 3.3 功能实现

##### 3.3.1 定位功能的位置

在用户输入用户名之后，立即检查这个用户名是否可用。

##### 3.3.2 思路

###### ①  给用户名输入框绑定的事件类型

结论：不能在针对username设定的watch中发送Ajax请求。

原因：服务器端响应的速度跟不上用户输入的速度，而且服务器端异步返回响应数据，无法保证和用户输入的顺序完全一致。此时有下面几点问题：

- 给服务器增加不必要的压力
- 用户输入的数据在输入过程中是不断发生变化的
- 响应数据和输入顺序不对应，会发生错乱

解决办法：绑定的事件类型使用change事件。

###### ② 流程图

![image-20220829224438988](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224438988.png)

##### 3.3.3 代码实现

###### ① 在当前页面引入axios库文件

```html
<script src="static/script/axios.js"></script>
```



###### ② 给用户名输入框绑失去焦点事件

```html
<input type="text" placeholder="请输入用户名" name="username" v-model="username" @blur="checkUsername"/>
```

###### ③ JavaScript代码

```javascript
var vue = new Vue({
    "el":"#app",
    "data":{
        "user":{
            "username":"[[${param.userName}]]",
            "password":"",
            "passwordConfirm":"",
            "email":"[[${param.email}]]"
        },
        "usernameError":"",
        "passwordError":"",
        "passwordConfirmError":"",
        "emailError":"",
        "usernameFlag":false,
        "passwordFlag":false,
        "passwordConfirmFlag":false,
        "emailFlag":false
    },
    "methods":{
        checkUsername(){
            //判断用户名是否符合规范
            //用户名要满足的要求:用户名应为5~12位数字和字母组成
            var regExp = /^[a-zA-Z0-9]{5,12}$/
            //获取用户名，并且使用regExp校验用户名
            if (!regExp.test(this.user.username)) {
                //用户名校验不通过:
                //显示"用户名必须是5-12位数字或字母"
                this.usernameError = "用户名必须是5-12位数字或字母"
                this.usernameFlag = false
            }else {
                //用户名格式正确
                //校验用户名是否可用:发送异步请求给UserServlet
                axios({
                    "method":"POST",
                    "url":"user",
                    "params":{
                        "method":"checkUsername",
                        "username":this.user.username
                    }
                }).then(response => {
                    //1. 判断响应的json中的flag
                    if (!response.data.flag) {
                        //用户名不可用
                        this.usernameError = response.data.message
                    }else {
                        //用户名可用
                        this.usernameError = ""
                    }
                    this.usernameFlag = response.data.flag
                })
            }
        },
        checkPassword(){
            //判断密码是否符合规范
            //用户的密码要满足的要求:密码是8-16位的数字、字母、_
            var regExp = /^[0-9a-zA-Z_]{8,16}$/
            if (!regExp.test(this.user.password)) {
                //显示"密码必须是是8-16位的数字、字母、_"
                this.passwordError = "密码必须是是8-16位的数字、字母、_"
                this.passwordFlag = false
            }else {
                this.passwordError = ""
                this.passwordFlag = true
            }
        },
        checkPasswordConfirm(){
            //判断确认密码输入框的内容和密码输入框的内容是否一致
            if (this.user.passwordConfirm != this.user.password) {
                //确认密码和密码不一致
                this.passwordConfirmError = "两次输入的密码要一致"
                this.passwordConfirmFlag = false
            }else {
                this.passwordConfirmError = ""
                this.passwordConfirmFlag = true
            }
        },
        checkEmail(){
            //使用正则表达式判断邮箱格式
            var regExp = /^[a-zA-Z0-9_\.-]+@([a-zA-Z0-9-]+[\.]{1})+[a-zA-Z]+$/
            //校验邮箱格式
            if (!regExp.test(this.user.email)) {
                //邮箱格式错误
                this.emailError = "邮箱格式错误"
                this.emailFlag = false
            }else {
                this.emailError = ""
                this.emailFlag = true
            }
        },
        checkRegister(){
            //再加一个判断:为了防止有的人压根没有在输入框输入内容
            if (this.user.username == "") {
                this.usernameError = "用户名不能为空"
            }

            if (!this.passwordFlag) {
                this.passwordError = "密码必须是是8-16位的数字、字母、_"
            }

            if (!this.passwordConfirmFlag) {
                this.passwordConfirmError = "两次输入的密码要一致"
            }

            if (!this.emailFlag) {
                this.emailError = "邮箱格式错误"
            }

            //校验注册:只有当所有的输入框都符合规范，才能提交表单，否则就要取消控件的默认行为(阻止表单提交)
            if (!this.usernameFlag || !this.passwordFlag || !this.passwordConfirmFlag || !this.emailFlag) {
                //肯定有输入框校验是不通过的，所以阻止表单提交
                event.preventDefault()
            }
        },
        changeCode(){
            //切换验证码: 重新设置当前图片的src属性的值
            event.target.src = "kaptcha"
        }
    }
});
```



###### ④ UserServlet

```java
/**
     * 校验用户名是否已存在
     * @param request
     * @param response
     * @throws IOException
     */
public void checkUsername(HttpServletRequest request, HttpServletResponse response) throws IOException {
    CommonResult commonResult = null;
    try {
        //1. 获取请求参数username的值
        String username = request.getParameter("username");
        //2. 调用业务层的findByUsername()方法校验用户名是否已存在
        User user=userService.findByUsername(username);
        if(user==null){
             //3. 表示用户名可用
        	commonResult = CommonResult.ok();
        }else{
            //4. 用户名已存在，不可用
        	commonResult = CommonResult.error().setMessage(e.getMessage());
        }
       
    } catch (Exception e) {
        e.printStackTrace();
    }
		//将CommonResult对象转成json字符串，响应给客户端
        String responseJson = gson.toJson(commonResult);
        response.getWriter().write(responseJson);
}
```

###### ⑤ UserService

```java
@Override
public void findByUsername(String username) throws Exception {
    //调用持久层的方法根据username查询user
    User user = userDao.findByUsername(username);
    if (user != null) {
        throw new RuntimeException("用户名已存在");
    }
}
```

#### 3.4 加入购物车

##### 3.4.1 创建购物车模型

![image-20220829224452412](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224452412.png)

###### ① 购物项详情类

```java
package com.atguigu.bookstore.bean;


public class CartItem {
    /**
     * 购物项存储的那本书的id
     */
    private Integer bookId;
    /**
     * 购物项存储的那本书的书名
     */
    private String bookName;
    /**
     * 购物项存储的那本书的图片路径
     */
    private String imgPath;
    /**
     * 购物项存储的那本书的单价
     */
    private Double price;
    /**
     * 购物项的书的数量
     */
    private Integer count = 0;
    /**
     * 购物项的金额
     */
    private Double amount = 0d;

    public CartItem(Integer bookId, String bookName, String imgPath, Double price, Integer count, Double amount) {
        this.bookId = bookId;
        this.bookName = bookName;
        this.imgPath = imgPath;
        this.price = price;
        this.count = count;
        this.amount = amount;
    }

    public CartItem() {
    }

    public Integer getBookId() {
        return bookId;
    }

    public void setBookId(Integer bookId) {
        this.bookId = bookId;
    }

    public String getBookName() {
        return bookName;
    }

    public void setBookName(String bookName) {
        this.bookName = bookName;
    }

    public String getImgPath() {
        return imgPath;
    }

    public void setImgPath(String imgPath) {
        this.imgPath = imgPath;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    public Integer getCount() {
        return count;
    }

    public void setCount(Integer count) {
        this.count = count;
    }

    /**
     * 获取当前购物项的金额
     * @return
     */
    public Double getAmount() {
        //我们自己计算金额
        this.amount = this.price * this.count;
        return this.amount;
    }

    public void setAmount(Double amount) {
        this.amount = amount;
    }

    @Override
    public String toString() {
        return "CartItem{" +
            "bookId=" + bookId +
            ", bookName='" + bookName + '\'' +
            ", imgPath='" + imgPath + '\'' +
            ", price=" + price +
            ", count=" + count +
            ", amount=" + amount +
            '}';
    }

    /**
     * 将count自增1
     */
    public void countIncrease(){
        this.count ++;
    }

    /**
     * 将当前购物项的数量进行 -1
     */
    public void countDecrease(){
        if (this.count > 0) {
            this.count --;
        }
    }
}
```

###### ② 购物车类：Cart

```java
package com.atguigu.bookstore.bean;

import com.atguigu.bookstore.entity.Book;

import java.util.Collection;
import java.util.HashMap;
import java.util.Map;


public class Cart {
    /**
     * 当前购物车的总金额
     */
    private Double totalAmount = 0d;

    /**
     * 当前购物车的商品总数
     */
    private Integer totalCount = 0;

    /**
     * 存储购物项的容器
     * 以购物项的bookId作为key，以购物项CartItem作为value
     */
    private Map<Integer,CartItem> cartItemMap = new HashMap<>();

    /**
     * 将某本书添加进购物车
     * @param book
     */
    public void addBookToCart(Book book){
        //1. 判断当前购物车中是否已经有这本书了
        if (cartItemMap.containsKey(book.getBookId())) {
            //说明当前购物车已经包含了这本书，那么就只需要将这本书对应的购物项的count +1就行了
            cartItemMap.get(book.getBookId()).countIncrease();
        }else {
            //说明当前购物车中不包含这本书，就要新添加一个购物项
            CartItem cartItem = new CartItem();
            //设置cartItem中的内容
            cartItem.setBookId(book.getBookId());
            cartItem.setBookName(book.getBookName());
            cartItem.setImgPath(book.getImgPath());
            cartItem.setPrice(book.getPrice());
            cartItem.setCount(1);

            //将cartItem添加cartItemMap
            cartItemMap.put(book.getBookId(),cartItem);
        }
    }

    /**
     * 将某个购物项的数量+1
     * @param bookId
     */
    public void itemCountIncrease(Integer bookId){
        //1. 根据bookId找到对应的购物项
        //2. 调用购物项的countIncrease()方法进行数量+1
        cartItemMap.get(bookId).countIncrease();
    }

    /**
     * 将某一个购物项的数量 -1
     * @param bookId
     */
    public void itemCountDecrease(Integer bookId){
        //1. 根据bookId找到对应的购物项
        //2. 调用购物项的countDecrease()方法进行数量-1
        CartItem cartItem = cartItemMap.get(bookId);
        cartItem.countDecrease();
        //3. 判断当前购物项的数量是否大于0，如果不大于0，说明我们需要将当前购物项从购物车中删除
        if (cartItem.getCount() == 0) {
            cartItemMap.remove(bookId);
        }
    }

    /**
     * 根据bookId将购物项从购物车中移除
     * @param bookId
     */
    public void removeCartItem(Integer bookId){
        cartItemMap.remove(bookId);
    }

    /**
     * 修改某个购物项的数量
     * @param bookId
     * @param newCount
     */
    public void updateItemCount(Integer bookId,Integer newCount){
        //1. 根据bookId找到对应的购物项
        //2. 将newCount设置到购物项的count属性
        cartItemMap.get(bookId).setCount(newCount);
    }

    /**
     * 计算商品的总金额
     * @return
     */
    public Double getTotalAmount() {
        this.totalAmount = 0d;
        //计算购物车中的所有的商品总金额，其实就是累加每一个购物项的amount
        cartItemMap.forEach((k,cartItem) -> {
            this.totalAmount += cartItem.getAmount();
        });
        return this.totalAmount;
    }

    public void setTotalAmount(Double totalAmount) {
        this.totalAmount = totalAmount;
    }

    /**
     * 计算商品总数量
     * @return
     */
    public Integer getTotalCount() {
        //计算购物车中的所有的商品总数，其实就是累加每一个购物项的count
        this.totalCount = 0;
        //获取到Map中的所有的value
        Collection<CartItem> values = cartItemMap.values();
        //遍历出每一个value
        for (CartItem cartItem : values) {
            this.totalCount += cartItem.getCount();
        }

        return this.totalCount;
    }


    public void setTotalCount(Integer totalCount) {
        this.totalCount = totalCount;
    }

    public Map<Integer, CartItem> getCartItemMap() {
        return cartItemMap;
    }

    public void setCartItemMap(Map<Integer, CartItem> cartItemMap) {
        this.cartItemMap = cartItemMap;
    }
}
```



##### 3.4.2 思路

![image-20220829224505318](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224505318.png)

##### 3.4.3客户端发送异步请求

###### ① 在首页引入vue和axios

```html
<script src="static/script/vue.js" type="text/javascript" charset="utf-8"></script>
<script src="static/script/axios.js" type="text/javascript" charset="utf-8"></script>
```

###### ② 绑定单击响应函数

给加入购物车按钮绑定单击响应函数

```html
<button @click="addBookToCart()" th:value="${book.bookId}">加入购物车</button>
```

给首页顶部绑定显示购物车中商品总数,由于要考虑是否登录的情况，所以登录和未登录的标签都要绑定数据模型

```html
<!--登录前的风格-->
<div class="topbar-right" th:if="${session.loginUser == null}">
    <a href="user?method=toLoginPage" class="login">登录</a>
    <a href="user?method=toRegisterPage" class="register">注册</a>
    <a
       href="cart?method=toCartPage"
       class="cart iconfont icon-gouwuche
              "
       >
        购物车
        <div class="cart-num" v-text="totalCount">3</div>
    </a>
    <a href="admin?method=toManagerPage" class="admin">后台管理</a>
</div>
<!--登录后风格-->
<div class="topbar-right" th:unless="${session.loginUser == null}">
    <span>欢迎你<b th:text="${session.loginUser.userName}">张总</b></span>
    <a href="user?method=logout" class="register">注销</a>
    <a
       href="cart?method=toCartPage"
       class="cart iconfont icon-gouwuche
              ">
        购物车
        <div class="cart-num" v-text="totalCount">3</div>
    </a>
    <a href="pages/manager/book_manager.html" class="admin">后台管理</a>
</div>
```

###### ③ Vue代码：

```javascript
var vue = new Vue({
    "el":"#app",
    "data":{
        "totalCount":0
    },
    "methods":{
        addBookToCart(){
            //获取bookId: bookId绑定在当前标签的value属性上
            //event.target就表示拿到当前标签
            var bookId = event.target.value;

            //发送异步请求:添加书进购物车
            axios({
                "method":"post",
                "url":"cart",
                "params":{
                    "method":"addCartItem",
                    "id":bookId
                }
            }).then(response => {
                if (response.data.flag) {
                    //添加购物车成功
                    this.totalCount = response.data.resultData
                    alert("添加购物车成功")
                }else {
                    //添加购物车失败
                   alert("添加购物车失败")
                }
            })
        }
    }
});
```

##### 3.4.4 后端代码

###### ① CartServlet

```java
package com.atguigu.bookstore.servlet.model;

import com.atguigu.bookstore.bean.Cart;
import com.atguigu.bookstore.bean.CommonResult;
import com.atguigu.bookstore.constants.BookStoreConstants;
import com.atguigu.bookstore.entity.Book;
import com.atguigu.bookstore.service.BookService;
import com.atguigu.bookstore.service.impl.BookServiceImpl;
import com.atguigu.bookstore.servlet.base.ModelBaseServlet;
import com.atguigu.bookstore.utils.JsonUtils;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;


public class CartServlet extends ModelBaseServlet {
    private BookService bookService = new BookServiceImpl();
    /**
     * 将书添加进购物车
     * @param request
     * @param response
     */
    public void addCartItem(HttpServletRequest request, HttpServletResponse response){
        CommonResult commonResult = null;
        try {
            //1. 获取请求参数:书的id
            Integer id = Integer.valueOf(request.getParameter("id"));
            //2. 调用业务层的方法，根据id查询到书
            Book book = bookService.getBookById(id);
            //3. 尝试从会话域session中获取购物车信息:主要目的是判断当前是否是第一次添加商品进购物车
            HttpSession session = request.getSession();
            Cart cart = (Cart) session.getAttribute(BookStoreConstants.CARTSESSIONKEY);
            if (cart == null) {
                //说明之前没有购物车,那么就新建一个cart对象
                cart = new Cart();
                //将cart添加到session中
                session.setAttribute(BookStoreConstants.CARTSESSIONKEY,cart);
            }
            //将书添加进购物车
            cart.addBookToCart(book);

            //封装响应数据
            commonResult = CommonResult.ok().setResultData(cart.getTotalCount());
        } catch (Exception e) {
            e.printStackTrace();
            commonResult = CommonResult.error().setMessage(e.getMessage());
        }

        //将commonResult对象转成json字符串，响应给客户端
        response.getWriter().write(new Gson().toJson(commonResult));
    }
}
```

###### ② web.xml

```xml
<servlet>
    <servlet-name>CartServlet</servlet-name>
    <servlet-class>com.atguigu.bookstore.servlet.model.CartServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>CartServlet</servlet-name>
    <url-pattern>/cart</url-pattern>
</servlet-mapping>
```



# 第八章 书城项目第五阶段

## 1. 显示购物车页面

### 1.1目标

把购物车信息在专门的页面显示出来

### 1.2思路

![image-20220829224527752](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224527752.png)

### 1.3代码实现

#### 1.3.1 购物车超链接

登录状态和未登录状态

```html
<div class="topbar-right" th:if="${session.loginUser == null}">
    <a href="user?method=toLoginPage" class="login">登录</a>
    <a href="user?method=toRegisterPage" class="register">注册</a>
    <a
       href="cart?method=toCartPage"
       class="cart iconfont icon-gouwuche
              "
       >
        购物车
        <div class="cart-num" th:if="${session.cart != null}" th:text="${session.cart.totalCount}">3</div>
    </a>
    <a href="admin?method=toManagerPage" class="admin">后台管理</a>
</div>
<!--登录后风格-->
<div class="topbar-right" th:unless="${session.loginUser == null}">
    <span>欢迎你<b th:text="${session.loginUser.username}">张总</b></span>
    <a href="user?method=logout" class="register">注销</a>
    <a
       href="cart?method=toCartPage"
       class="cart iconfont icon-gouwuche
              ">
        购物车
        <div class="cart-num" th:if="${session.cart != null}" th:text="${session.cart.totalCount}">3</div>
    </a>
    <a href="admin?method=toManagerPage" class="admin">后台管理</a>
</div>
```

#### 1.3.2 CartServlet添加跳转到cart.html页面的代码

```java
/**
     * 跳转到显示购物车列表的页面
     * @param request
     * @param response
     */
public void toCartPage(HttpServletRequest request,HttpServletResponse response) throws IOException {
    processTemplate("cart/cart",request,response);
}
```

#### 1.3.3 cart.html

```html
<!--加入vue和axios-->
<script src="static/script/axios.js"></script>
<script src="static/script/vue.js"></script>

<!--vue数据绑定-->
<div class="list" id="app">
    <div class="w">
        <table>
            <thead>
                <tr>
                    <th>图片</th>
                    <th>商品名称</th>

                    <th>数量</th>
                    <th>单价</th>
                    <th>金额</th>
                    <th>操作</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(cartItem,index) in cart.cartItemList">
                    <td>
                        <img :src="cartItem.imgPath" alt="" />
                        <input type="hidden" name="bookId" v-model="cartItem.bookId"/>
                    </td>
                    <td v-text="cartItem.bookName"></td>
                    <td>
                        <span class="count">-</span>
                        <input class="count-num" type="text" v-model="cartItem.count" />
                        <span class="count">+</span>
                    </td>
                    <td v-text="cartItem.price"></td>
                    <td v-text="cartItem.amount"></td>
                    <td><a href="">删除</a></td>
                </tr>
            </tbody>
        </table>
        <div class="footer">
            <div class="footer-left">
                <a href="#" class="clear-cart">清空购物车</a>
                <a href="#">继续购物</a>
            </div>
            <div class="footer-right">
                <div>共<span v-text="cart.totalCount"></span>件商品</div>
                <div class="total-price">总金额<span v-text="cart.totalAmount"></span>元</div>
                <a class="pay" href="checkout.html">去结账</a>
            </div>
        </div>
    </div>
</div>


<!--vue代码-->
<script>
    var vue = new Vue({
        "el":"#app",
        "data":{
            "cart":{
                "cartItemList":[
                    {
                        "imgPath":"static/uploads/huozhe.jpg",
                        "bookName":"活着",
                        "bookId":1,
                        "count":1,
                        "price":36.8,
                        "amount":36.8
                    },
                    {
                        "imgPath":"static/uploads/huozhe.jpg",
                        "bookName":"活着",
                        "bookId":1,
                        "count":1,
                        "price":36.8,
                        "amount":36.8
                    }
                ],
                "totalCount":2,
                "totalAmount":73.6
            }
        },
        "methods":{
            showCart(){
                //发送异步请求获取购物车的信息
                axios({
                    "method":"post",
                    "url":"cart",
                    "params":{
                        "method":"getCartJSON"
                    }
                }).then(response => {
                    this.cart = response.data.resultData
                } )
            }
        },
        created(){
            //钩子函数，在这个钩子函数中就能使用数据模型
            this.showCart()
        }
    });
</script>
```

#### 1.3.4 修改Cart类添加getCartItemList()方法

```java
/**
     * 获取购物项列表
     * @return
     */
public List<CartItem> getCartItemList(){
    List<CartItem> cartItemList = new ArrayList<>();
    for (CartItem cartItem : cartItemMap.values()) {
        cartItemList.add(cartItem);
    }
    return cartItemList;
}
```

#### 1.3.5 CartServlet中添加getCartJSON()方法

```java
/**
     * 获取购物车的数据
     * @param request
     * @param response
     */
public void getCartJSON(HttpServletRequest request, HttpServletResponse response) {
    CommonResult commonResult = null;
    try {
        //1. 获取购物车信息
        Cart cart = (Cart) request.getSession().getAttribute(BookStoreConstants.CARTSESSIONKEY);
        //2. 创建一个Map用于封装客户端需要的数据
        Map responseMap = new HashMap();
        if (cart != null){
            responseMap.put("totalCount",cart.getTotalCount());
            responseMap.put("totalAmount",cart.getTotalAmount());

            //3. 获取cart中的所有的购物项:返回一个List<CartItem>
            responseMap.put("cartItemList",cart.getCartItemList());
        }
        //4. 将responseMap存储到CommonResult对象中
        commonResult = CommonResult.ok().setResultData(responseMap);
    } catch (Exception e) {
        e.printStackTrace();
        commonResult = CommonResult.error().setMessage(e.getMessage());
    }

    //将commonResult对象转成Json字符串输出到客户端
    response.getWriter().write(new Gson().toJson(commonResult));
}
```

## 2. 清空购物车

### 2.1 目标

当用户确定点击清空购物车，将Session域中的Cart对象移除。

### 2.2 思路

cart.html→清空购物车超链接→点击事件→confirm()确认→确定→CartServlet.clearCart()→从Session域移除Cart对象→跳转回到cart.html页面

### 2.3 代码实现

#### 2.3.1 前端页面代码

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>Document</title>
        <base th:href="@{/}"/>
        <link rel="stylesheet" href="static/css/minireset.css" />
        <link rel="stylesheet" href="static/css/common.css" />
        <link rel="stylesheet" href="static/css/cart.css" />
        <script src="static/script/vue.js"></script>
        <script src="static/script/axios.min.js"></script>
    </head>
    <body>
        <div class="header">
            <div class="w">
                <div class="header-left">
                    <a href="index.html">
                        <img src="static/img/logo.gif" alt=""
                             /></a>
                    <h1>我的购物车</h1>
                </div>
                <div class="header-right">
                    <h3>欢迎<span th:text="${session.loginUser.userName}">张总</span>光临尚硅谷书城</h3>
                    <div class="order"><a href="../order/order.html">我的订单</a></div>
                    <div class="destory"><a href="user?method=logout">注销</a></div>
                    <div class="gohome">
                        <a href="index.html">返回</a>
                    </div>
                </div>
            </div>
        </div>
        <div class="list" id="app">
            <div class="w">
                <table>
                    <thead>
                        <tr>
                            <th>图片</th>
                            <th>商品名称</th>

                            <th>数量</th>
                            <th>单价</th>
                            <th>金额</th>
                            <th>操作</th>
                        </tr>
                    </thead>
                    <tbody v-if="cart.cartItemList == null">
                        <tr>
                            <td colspan="6">
                                购物车空空如也，请添加购物车信息
                            </td>
                        </tr>
                    </tbody>
                    <tbody v-if="cart.cartItemList != null">
                        <tr v-for="(cartItem,index) in cart.cartItemList">
                            <td>
                                <img :src="cartItem.imgPath" alt="" />
                                <input type="hidden" name="bookId" v-model="cartItem.bookId"/>
                            </td>
                            <td v-text="cartItem.bookName"></td>
                            <td>
                                <span class="count">-</span>
                                <input class="count-num" type="text" v-model="cartItem.count" />
                                <span class="count">+</span>
                            </td>
                            <td v-text="cartItem.price"></td>
                            <td v-text="cartItem.amount"></td>
                            <td><a href="">删除</a></td>
                        </tr>
                    </tbody>
                </table>
                <div class="footer">
                    <div class="footer-left">
                        <a href="cart?method=cleanCart" @click="cleanCart()" class="clear-cart">清空购物车</a>
                        <a href="#">继续购物</a>
                    </div>
                    <div class="footer-right" v-if="cart.cartItemList != null">
                        <div>共<span v-text="cart.totalCount"></span>件商品</div>
                        <div class="total-price">总金额<span v-text="cart.totalAmount"></span>元</div>
                        <a class="pay" href="checkout.html">去结账</a>
                    </div>
                </div>
            </div>
        </div>
        <div class="bottom">
            <div class="w">
                <div class="top">
                    <ul>
                        <li>
                            <a href="">
                                <img src="static/img/bottom1.png" alt="" />
                                <span>大咖级讲师亲自授课</span>
                            </a>
                        </li>
                        <li>
                            <a href="">
                                <img src="static/img/bottom.png" alt="" />
                                <span>课程为学员成长持续赋能</span>
                            </a>
                        </li>
                        <li>
                            <a href="">
                                <img src="static/img/bottom2.png" alt="" />
                                <span>学员真是情况大公开</span>
                            </a>
                        </li>
                    </ul>
                </div>
                <div class="content">
                    <dl>
                        <dt>关于尚硅谷</dt>
                        <dd>教育理念</dd>
                        <!-- <dd>名师团队</dd>
<dd>学员心声</dd> -->
                    </dl>
                    <dl>
                        <dt>资源下载</dt>
                        <dd>视频下载</dd>
                        <!-- <dd>资料下载</dd>
<dd>工具下载</dd> -->
                    </dl>
                    <dl>
                        <dt>加入我们</dt>
                        <dd>招聘岗位</dd>
                        <!-- <dd>岗位介绍</dd>
<dd>招贤纳师</dd> -->
                    </dl>
                    <dl>
                        <dt>联系我们</dt>
                        <dd>http://www.atguigu.com</dd>
                        <dd></dd>
                    </dl>
                </div>
            </div>
            <div class="down">
                尚硅谷书城.Copyright ©2015
            </div>
        </div>
        <script>
            var vue = new Vue({
                "el":"#app",
                "data":{
                    "cart":{}
                },
                "methods":{
                    showCart(){
                        //发送异步请求获取购物车的信息
                        axios({
                            "method":"post",
                            "url":"cart",
                            "params":{
                                "method":"getCartJSON"
                            }
                        }).then(response => {
                            this.cart = response.data.resultData
                        } )
                    },
                    cleanCart(){
                        //弹出确认框，问是否真的要清空购物车
                        if (!confirm("你确定要清空购物车吗?")) {
                            //不想清空，点错了:要阻止a标签跳转
                            event.preventDefault()
                        }
                    }
                },
                created(){
                    //钩子函数，在这个钩子函数中就能使用数据模型
                    this.showCart()
                }
            });
        </script>
    </body>
</html>
```

#### 2.3.2 CartServlet.cleanCart()

```java
/**
     * 清空购物车
     * @param request
     * @param response
     */
public void cleanCart(HttpServletRequest request, HttpServletResponse response) throws IOException {
    //1. 将cart从session中移除
    request.getSession().removeAttribute(BookStoreConstants.CARTSESSIONKEY);
    //2. 跳转回到cart.html页面
    processTemplate("cart/cart",request,response);
}
```

## 3. 减号

### 3.1 目标

- 在大于1的数值基础上-1：执行-1的逻辑
- 在1的基础上-1：执行删除item的逻辑

### 3.2 思路

![image-20220829224547296](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224547296.png)

### 3.3 前端代码

#### 3.3.1 给减号绑定点击事件

```html
<a @click="cartItemCountDecrease(cartItem.count,cartItem.bookName,cartItem.bookId,index)" class="count" href="javascript:;">-</a>
```

#### 3.3.2 Vue代码

```javascript
countDecrease(count,bookName,bookId,index){
    //判断当前购物项的count是否为1
    if (count == 1) {
        //弹出一个确认框，询问是否真的要删除
        if (!confirm("你确定要删除这个"+bookName+"购物项吗?")) {
            return;
        }
    }
    //发送异步请求
    axios({
        "method":"post",
        "url":"cart",
        "params":{
            "method":"countDecrease",
            "id":bookId
        }
    }).then(response => {
        //判断count是否为1
        if (count == 1) {
            //1. 删除当前购物项:根据下标删除cart.cartItemList中的元素
            this.cart.cartItemList.splice(index,1)
        }else {
            //1. 重新设置当前购物项的count以及amount
            this.cart.cartItemList[index].count = response.data.resultData.count
            this.cart.cartItemList[index].amount = response.data.resultData.amount
        }
        //2. 重新设置当前购物车的totalCount以及totalAmount
        this.cart.totalCount = response.data.resultData.totalCount
        this.cart.totalAmount = response.data.resultData.totalAmount
    })
}
```

### 3.4 后端代码

CartServlet.countDecrease()方法

```java
/**
     * 购物项数量-1
     * @param request
     * @param response
     */
public void countDecrease(HttpServletRequest request, HttpServletResponse response) {
    CommonResult commonResult = null;
    try {
        //1. 获取id的值
        Integer id = Integer.valueOf(request.getParameter("id"));
        //2. 从session中获取购物车
        Cart cart = (Cart) request.getSession().getAttribute(BookStoreConstants.CARTSESSIONKEY);
        //3. 调用cart的itemCountDecrease(id)方法对某个购物项进行-1操作
        cart.itemCountDecrease(id);
        //4. 获取需要响应给客户端的数据:当前购物项的count、amount以及当前购物车的totalCount、totalAmount
        Map responseMap = getResponseMap(id, cart);
        //-1成功
        commonResult = CommonResult.ok().setResultData(responseMap);
    } catch (Exception e) {
        e.printStackTrace();
        commonResult = CommonResult.error().setMessage(e.getMessage());
    }
    //将commonResult对象转成Json响应给客户端
    response.getWriter().write(new Gson().toJson(commonResult));
    
}
```

CartServlet.getResponseMap()方法

```java
/**
     * 获取操作购物车之后的响应数据
     * @param id
     * @param cart
     * @return
     */
private Map getResponseMap(Integer id, Cart cart) {
    CartItem cartItem = cart.getCartItemMap().get(id);
    //将要响应到客户端的数据存储到Map中
    Map responseMap = new HashMap();
    if (cartItem != null) {
        Integer count = cartItem.getCount();
        responseMap.put("count",count);

        Double amount = cartItem.getAmount();
        responseMap.put("amount",amount);
    }

    Integer totalCount = cart.getTotalCount();
    responseMap.put("totalCount",totalCount);

    Double totalAmount = cart.getTotalAmount();
    responseMap.put("totalAmount",totalAmount);

    return responseMap;
}
```



## 4. 加号

### 4.1 目标

告诉Servlet将Session域中Cart对象里面对应的CartItem执行count+1操作

### 4.2 思路

![image-20220829224558300](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224558300.png)

### 4.3 代码实现

#### 4.3.1 前端代码

给加号绑定点击事件

```html
<span class="count" @click="countIncrease(cartItem.bookId,index)">+</span>
```

vue代码

```javascript
countIncrease(bookId,index){
    //发送异步请求
    axios({
        "method":"post",
        "url":"cart",
        "params":{
            "method":"countIncrease",
            "id":bookId
        }
    }).then(response => {
        //1. 重新设置当前购物项的count以及amount
        this.cart.cartItemList[index].count = response.data.resultData.count
        this.cart.cartItemList[index].amount = response.data.resultData.amount
        //2. 重新设置当前购物车的totalCount以及totalAmount
        this.cart.totalCount = response.data.resultData.totalCount
        this.cart.totalAmount = response.data.resultData.totalAmount
    })
}
```

#### 4.3.2 后端代码

CartServlet.countIncrease()

```java
/**
     * 购物项数量加一
     * @param request
     * @param response
     */
public void countIncrease(HttpServletRequest request, HttpServletResponse response) {
    CommonResult commonResult = null;
    try {
        //1. 获取要加一的购物项的id
        Integer id = Integer.valueOf(request.getParameter("id"));
        //2. 从session中获取购物车信息
        Cart cart = (Cart) request.getSession().getAttribute(BookStoreConstants.CARTSESSIONKEY);
        //3. 调用cart的itemCountIncrease(id)方法对购物项进行+1
        cart.itemCountIncrease(id);
        //4. 封装响应数据
        Map responseMap = getResponseMap(id, cart);

        commonResult = CommonResult.ok().setResultData(responseMap);
    } catch (Exception e) {
        e.printStackTrace();
        commonResult = CommonResult.error().setMessage(e.getMessage());
    }

    //将commonResult转成json响应给客户端
    response.getWriter().write(new Gson().toJson(commonResult));
}
```

## 5. 删除

### 5.1 目标

点击删除超链接后，把对应的CartItem从Cart中删除

### 5.2 思路

![image-20220829224611400](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224611400.png)

### 5.3 代码实现

#### 5.3.1 前端代码

给删除按钮绑定点击事件

```html
<a href="#" @click.prevent="removeCartItem(cartItem.bookName,cartItem.bookId,index)">删除</a>
```

vue和axios代码

```javascript
removeCartItem(bookName,bookId,index){
    if (confirm("你确定要删除" + bookName + "这个购物项吗？")) {
        axios({
            "method":"post",
            "url":"cart",
            "params":{
                "method":"removeCartItem",
                "id":bookId
            }
        }).then(response => {
            //1. 将当前购物项这行删除
            this.cart.cartItemList.splice(index,1)
            //2. 重新设置购物车的totalCount和totalAmount
            this.cart.totalCount = response.data.resultData.totalCount
            this.cart.totalAmount = response.data.resultData.totalAmount
        } )
    }
}
```

#### 5.3.2 后端代码

CartServlet.removeCartItem()

```java
/**
     * 删除购物项
     * @param request
     * @param response
     */
public void removeCartItem(HttpServletRequest request, HttpServletResponse response) {
    CommonResult commonResult = null;
    try {
        //1. 获取要删除的购物项的id
        Integer id = Integer.valueOf(request.getParameter("id"));
        //2. 从session中获取购物车
        Cart cart = (Cart) request.getSession().getAttribute(BookStoreConstants.CARTSESSIONKEY);
        //3. 调用cart的removeCartItem(id)删除购物项
        cart.removeCartItem(id);
        //4. 封装响应数据
        Map responseMap = getResponseMap(id, cart);

        commonResult = CommonResult.ok().setResultData(responseMap);
    } catch (Exception e) {
        e.printStackTrace();
        commonResult = CommonResult.error().setMessage(e.getMessage());
    }
    //将commonResult转成json响应给客户端
    JsonUtils.writeResult(response,commonResult);
}
```

## 6. 文本框修改

### 6.1 目标

用户在文本框输入新数据后，根据用户输入在Session中的Cart中修改CartItem中的count

### 6.2 思路

![image-20220829224630525](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224630525.png)

### 6.3 代码实现

#### 6.3.1 前端代码

绑定失去change事件

```html
<input class="count-num" type="text" v-model="cartItem.count" @change="updateCartItemCount(cartItem.count,cartItem.bookId,index)"/>
```

vue和axios代码

```javascript
updateCartItemCount(newCount,bookId,index){
    //判断新的数量是否符合规范
    var reg = /^[1-9][0-9]*$/

    if (reg.test(newCount)) {
        //符合规范，则发送异步请求
        axios({
            "method":"post",
            "url":"cart",
            "params":{
                "method":"updateCartItemCount",
                "id":bookId,
                "newCount":newCount
            }
        }).then(response => {
            //1. 重新渲染当前购物项的count、amount
            this.cart.cartItemList[index].count = response.data.resultData.count
            this.cart.cartItemList[index].amount = response.data.resultData.amount
            //2. 重新渲染当前购物车的totalCount、totalAmount
            this.cart.totalCount = response.data.resultData.totalCount
            this.cart.totalAmount = response.data.resultData.totalAmount
        })
    }
}
```

#### 6.3.2后端代码

CartServlet.updateCartItemCount()

```java
/**
     * 修改某个购物项的count
     * @param request
     * @param response
     */
public void updateCartItemCount(HttpServletRequest request, HttpServletResponse response) {
    CommonResult commonResult = null;
    try {
        //1. 获取要修改的购物项的id以及修改后的newCount
        Integer id = Integer.valueOf(request.getParameter("id"));
        Integer newCount = Integer.valueOf(request.getParameter("newCount"));
        //2. 从session中获取cart
        Cart cart = (Cart) request.getSession().getAttribute(BookStoreConstants.CARTSESSIONKEY);
        //3. 调用cart
        cart.updateItemCount(id,newCount);
        //4. 封装响应数据
        Map responseMap = getResponseMap(id, cart);
        commonResult = CommonResult.ok().setResultData(responseMap);
    } catch (Exception e) {
        e.printStackTrace();
        commonResult = CommonResult.error().setMessage(e.getMessage());
    }
    //将commonResult转成json响应给客户端
    JsonUtils.writeResult(response,commonResult);
}
```

## 7. Double数据运算过程中精度调整

### 7.1 问题现象

![image-20220829224641994](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224641994.png)

### 7.2 解决方案

- 使用BigDecimal类型来进行Double类型数据运算
- 创建BigDecimal类型对象时将Double类型的数据转换为字符串

Cart类：

```java
/**
     * 计算商品的总金额
     * @return
     */
public Double getTotalAmount() {
    BigDecimal bigDecimalTotalAmount = new BigDecimal("0.0");

    //累加
    Set<Map.Entry<Integer, CartItem>> entries = cartItemMap.entrySet();
    for (Map.Entry<Integer, CartItem> entry : entries) {
        CartItem cartItem = entry.getValue();
        Double amount = cartItem.getAmount();
        bigDecimalTotalAmount = bigDecimalTotalAmount.add(new BigDecimal(amount + ""));
    }
    //将bigDecimalTotalAmount转成double类型赋给this.totalAmount
    this.totalAmount = bigDecimalTotalAmount.doubleValue();
    return this.totalAmount;
}
```

CartItem类：

```java
/**
     * 这个方法获取总价:要通过计算才能获取
     * @return
     */
public Double getAmount() {
    //1. 将price和count封装成BigDecimal类型
    BigDecimal bigDecimalPrice = new BigDecimal(price + "");
    BigDecimal bigDecimalCount = new BigDecimal(count + "");
	
    //2. 使用bigDecimal的方法进行乘法
    this.amount = bigDecimalCount.multiply(bigDecimalPrice).doubleValue();
    return this.amount;
}
```

#



# 第九章 Filter&Listener&书城项目第六阶段

## 学习目标

* 了解什么是Filter
* 了解Filter的作用
* 掌握Filter的使用
* 了解Filter的生命周期
* 掌握过滤器链的使用
* 了解观察者模式
* 了解监听器的概念
* 掌握ServletContextListener的使用

### 1. Filter

#### 1.1 Filter的概念

Filter：一个实现了特殊接口(Filter)的Java类. 实现对请求资源(jsp,servlet,html,)的过滤的功能.  过滤器是一个运行在服务器的程序, 优先于请求资源(Servlet或者jsp,html)之前执行. 过滤器是javaweb技术中**最为实用**的技术之一

#### 1.2 Filter的作用

Filter的作用是对目标资源(Servlet,jsp)进行过滤，其应用场景有: 登录权限检查,解决网站乱码,过滤敏感字符等等

#### 1.3 Filter的入门案例

##### 1.3.1 案例目标

实现在请求到达ServletDemo01之前解决请求参数的中文乱码

##### 1.3.2 代码实现

###### ① 创建ServletDemo01

web.xml代码

```xml
<servlet>
    <servlet-name>servletDemo01</servlet-name>
    <servlet-class>com.atguigu.ServletDemo01</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo01</servlet-name>
    <url-pattern>/ServletDemo01</url-pattern>
</servlet-mapping>
```

ServletDemo01代码

```java
package com.atguigu.servlet;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

/**
 * @author chenxin
 * 日期2021-05-18  08:53
 */
public class ServletDemo01 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String username = request.getParameter("username");
        System.out.println("ServletDemo01接收到了一个请求..."+username);
    }
}
```

前端页面代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
    <form action="/webday12/demo01" method="post">
        用户名<input type="text" name="username"/><br/>
        <input type="submit"/>
    </form>
</body>
</html>
```

如果此时没有Filter，那么客户端发送的请求直接到达ServletDemo01,中文请求参数就会发生乱码

###### ② 创建EncodingFilter

web.xml代码

```xml
<filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>com.atguigu.filter.EncodingFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>encodingFilter</filter-name>
    <!--url-pattern表示指定拦截哪些资源-->
    <url-pattern>/demo01</url-pattern>
</filter-mapping>
```

EncodingFilter代码

```java
package com.atguigu.filter;

import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import java.io.IOException;

/**
 * @author chenxin
 * 日期2021-05-18  08:56
 * 编写过滤器的步骤:
 * 1. 写一个类实现Filter接口，并且重写方法
 * 2. 在web.xml中配置该过滤器的拦截路径
 */
public class EncodingFilter implements Filter {
    @Override
    public void destroy() {
        
    }

    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        //解决请求参数的乱码
        HttpServletRequest request = (HttpServletRequest) req;
        request.setCharacterEncoding("UTF-8");

        //每次有请求被当前filter接收到的时候，就会执行doFilter进行过滤处理
        System.out.println("EncodingFilter接收到了一个请求...");

        //这句代码表示放行
        chain.doFilter(req, resp);
    }

    @Override
    public void init(FilterConfig config) throws ServletException {
        
    }

}
```

#### 1.4 Filter的生命周期

##### 1.4.1 回顾Servlet生命周期

###### ① Servlet的创建时机

Servlet默认在第一次接收请求的时候创建，我们可以通过`<load-on-startup>`标签配置Servlet在服务器启动的时候创建

###### ② Servlet的销毁时机

Servlet会在服务器关闭或者将项目从服务器上移除的时候销毁

##### 1.4.2 Filter的生命周期和生命周期方法

| 生命周期阶段 | 执行时机         | 生命周期方法                             |
| ------------ | ---------------- | ---------------------------------------- |
| 创建对象     | Web应用启动时    | init方法，通常在该方法中做初始化工作     |
| 拦截请求     | 接收到匹配的请求 | doFilter方法，通常在该方法中执行拦截过滤 |
| 销毁         | Web应用卸载前    | destroy方法，通常在该方法中执行资源释放  |

#### 1.5 过滤器匹配规则

##### 1.5.1 过滤器匹配的目的

过滤器匹配的目的是指定当前过滤器要拦截哪些资源

##### 1.5.2 四种匹配规则

###### ① 精确匹配

指定被拦截资源的完整路径：

```xml
<!-- 配置Filter要拦截的目标资源 -->
<filter-mapping>
    <!-- 指定这个mapping对应的Filter名称 -->
    <filter-name>FilterDemo01</filter-name>

    <!-- 通过请求地址模式来设置要拦截的资源 -->
    <url-pattern>/demo01</url-pattern>
</filter-mapping>
```

上述例子表示要拦截映射路径为`/demo01`的这个资源

###### ② 模糊匹配

相比较精确匹配，使用模糊匹配可以让我们创建一个Filter就能够覆盖很多目标资源，不必专门为每一个目标资源都创建Filter，提高开发效率。

在我们配置了url-pattern为/user/*之后，请求地址只要是/user开头的那么就会被匹配。

```xml
<filter-mapping>
    <filter-name>Target02Filter</filter-name>

    <!-- 模糊匹配：前杠后星 -->
    <!--
        /user/demo01
        /user/demo02
        /user/demo03
		/demo04
    -->
    <url-pattern>/user/*</url-pattern>
</filter-mapping>
```

<span style="color:blue;font-weight:bold;">极端情况：/*匹配所有请求</span>

###### ③ 扩展名匹配

```xml
<filter>
    <filter-name>Target04Filter</filter-name>
    <filter-class>com.atguigu.filter.filter.Target04Filter</filter-class>
</filter>
<filter-mapping>
    <filter-name>Target04Filter</filter-name>
    <url-pattern>*.png</url-pattern>
</filter-mapping>
```

上述例子表示拦截所有以`.png`结尾的请求

#### 1.6 过滤器链

##### 1.6.1 过滤链的概念

一个请求可能被多个过滤器所过滤，只有当所有过滤器都放行，请求才能到达目标资源，如果有某一个过滤器没有放行，那么请求则无法到达后续过滤器以及目标资源，多个过滤器组成的链路就是过滤器链

![image-20220829224704555](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224704555.png)

##### 1.6.2 过滤器链的顺序

过滤器链中每一个Filter执行的<span style="color:blue;font-weight:bold;">顺序是由web.xml中filter-mapping配置的顺序决定</span>的。如果某个Filter是使用ServletName进行匹配规则的配置，那么这个Filter执行的优先级要更低

##### 1.6.3 过滤器链案例

###### ① 创建ServletDemo01

web.xml代码

```xml
<servlet>
    <servlet-name>servletDemo01</servlet-name>
    <servlet-class>com.atguigu.ServletDemo01</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>servletDemo01</servlet-name>
    <url-pattern>/ServletDemo01</url-pattern>
</servlet-mapping>
```

ServletDemo01代码

```java
public class ServletDemo01 extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("ServletDemo01接收到了请求...");
    }
}
```

###### ② 创建多个Filter拦截Servlet

```xml
<filter-mapping>
    <filter-name>TargetChain03Filter</filter-name>
    <url-pattern>/Target05Servlet</url-pattern>
</filter-mapping>
<filter-mapping>
    <filter-name>TargetChain02Filter</filter-name>
    <url-pattern>/Target05Servlet</url-pattern>
</filter-mapping>
<filter-mapping>
    <filter-name>TargetChain01Filter</filter-name>
    <url-pattern>/Target05Servlet</url-pattern>
</filter-mapping>
```

### 2. Listener(了解)

#### 2.1 监听器的简介

##### 2.1.1 监听器的概念

监听器：专门用于对其他对象身上发生的事件或状态改变进行监听和相应处理的对象，当被监视的对象发生情况时，立即采取相应的行动。
<span style="color:blue;font-weight:bold;">Servlet监听器</span>：Servlet规范中定义的一种特殊类，它用于监听Web应用程序中的ServletContext，HttpSession 和HttpServletRequest等域对象的创建与销毁事件，以及监听这些域对象中的属性发生修改的事件。

##### 2.1.2 Servlet监听器的分类(了解)

###### ① ServletContextListener

作用：监听ServletContext对象的创建与销毁

| 方法名                                      | 作用                     |
| ------------------------------------------- | ------------------------ |
| contextInitialized(ServletContextEvent sce) | ServletContext创建时调用 |
| contextDestroyed(ServletContextEvent sce)   | ServletContext销毁时调用 |

ServletContextEvent对象代表从ServletContext对象身上捕获到的事件，通过这个事件对象我们可以获取到ServletContext对象。

###### ② HttpSessionListener

作用：监听HttpSession对象的创建与销毁

| 方法名                                 | 作用                      |
| -------------------------------------- | ------------------------- |
| sessionCreated(HttpSessionEvent hse)   | HttpSession对象创建时调用 |
| sessionDestroyed(HttpSessionEvent hse) | HttpSession对象销毁时调用 |

HttpSessionEvent对象代表从HttpSession对象身上捕获到的事件，通过这个事件对象我们可以获取到触发事件的HttpSession对象。

###### ③ ServletRequestListener

作用：监听ServletRequest对象的创建与销毁

| 方法名                                      | 作用                         |
| ------------------------------------------- | ---------------------------- |
| requestInitialized(ServletRequestEvent sre) | ServletRequest对象创建时调用 |
| requestDestroyed(ServletRequestEvent sre)   | ServletRequest对象销毁时调用 |

ServletRequestEvent对象代表从HttpServletRequest对象身上捕获到的事件，通过这个事件对象我们可以获取到触发事件的HttpServletRequest对象。另外还有一个方法可以获取到当前Web应用的ServletContext对象。

###### ④ ServletContextAttributeListener

作用：监听ServletContext中属性的添加、移除和修改

| 方法名                                               | 作用                                 |
| ---------------------------------------------------- | ------------------------------------ |
| attributeAdded(ServletContextAttributeEvent scab)    | 向ServletContext中添加属性时调用     |
| attributeRemoved(ServletContextAttributeEvent scab)  | 从ServletContext中移除属性时调用     |
| attributeReplaced(ServletContextAttributeEvent scab) | 当ServletContext中的属性被修改时调用 |

ServletContextAttributeEvent对象代表属性变化事件，它包含的方法如下：

| 方法名              | 作用                     |
| ------------------- | ------------------------ |
| getName()           | 获取修改或添加的属性名   |
| getValue()          | 获取被修改或添加的属性值 |
| getServletContext() | 获取ServletContext对象   |

###### ⑤ HttpSessionAttributeListener

作用：监听HttpSession中属性的添加、移除和修改

| 方法名                                        | 作用                              |
| --------------------------------------------- | --------------------------------- |
| attributeAdded(HttpSessionBindingEvent se)    | 向HttpSession中添加属性时调用     |
| attributeRemoved(HttpSessionBindingEvent se)  | 从HttpSession中移除属性时调用     |
| attributeReplaced(HttpSessionBindingEvent se) | 当HttpSession中的属性被修改时调用 |

HttpSessionBindingEvent对象代表属性变化事件，它包含的方法如下：

| 方法名       | 作用                          |
| ------------ | ----------------------------- |
| getName()    | 获取修改或添加的属性名        |
| getValue()   | 获取被修改或添加的属性值      |
| getSession() | 获取触发事件的HttpSession对象 |

###### ⑥ ServletRequestAttributeListener

作用：监听ServletRequest中属性的添加、移除和修改

| 方法名                                               | 作用                                 |
| ---------------------------------------------------- | ------------------------------------ |
| attributeAdded(ServletRequestAttributeEvent srae)    | 向ServletRequest中添加属性时调用     |
| attributeRemoved(ServletRequestAttributeEvent srae)  | 从ServletRequest中移除属性时调用     |
| attributeReplaced(ServletRequestAttributeEvent srae) | 当ServletRequest中的属性被修改时调用 |

ServletRequestAttributeEvent对象代表属性变化事件，它包含的方法如下：

| 方法名               | 作用                             |
| -------------------- | -------------------------------- |
| getName()            | 获取修改或添加的属性名           |
| getValue()           | 获取被修改或添加的属性值         |
| getServletRequest () | 获取触发事件的ServletRequest对象 |

###### ⑦ HttpSessionBindingListener

作用：监听某个对象在Session域中的创建与移除

| 方法名                                      | 作用                              |
| ------------------------------------------- | --------------------------------- |
| valueBound(HttpSessionBindingEvent event)   | 该类的实例被放到Session域中时调用 |
| valueUnbound(HttpSessionBindingEvent event) | 该类的实例从Session中移除时调用   |

HttpSessionBindingEvent对象代表属性变化事件，它包含的方法如下：

| 方法名       | 作用                          |
| ------------ | ----------------------------- |
| getName()    | 获取当前事件涉及的属性名      |
| getValue()   | 获取当前事件涉及的属性值      |
| getSession() | 获取触发事件的HttpSession对象 |

#### 2.2 ServletContextListener的使用

##### 2.2.1 作用

ServletContextListener是监听ServletContext对象的创建和销毁的，因为ServletContext对象是在服务器启动的时候创建、在服务器关闭的时候销毁，所以ServletContextListener也可以监听服务器的启动和关闭

##### 2.2.2 使用场景

将来学习SpringMVC的时候，会用到一个ContextLoaderListener，这个监听器就实现了ServletContextListener接口，表示对ServletContext对象本身的生命周期进行监控。

##### 2.2.3 代码实现

###### ① 创建监听器类

```java
package com.atguigu.listener;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;

/**
 * 包名:com.atguigu.listener
 *
 * @author chenxin
 * 日期2021-06-19  10:26
 * 编写监听器的步骤:
 * 1. 写一个类实现对应的：Listener的接口(我们这里使用的是ServletContextListener),并且实现它里面的方法
 *    1.1 contextInitialized()这个方法在ServletContext对象被创建出来的时候执行，也就是说在服务器启动的时候执行
 *    1.2 contextDestroyed()这个方法会在ServletContext对象被销毁的时候执行，也就是说在服务器关闭的时候执行
 *
 * 2. 在web.xml中注册(配置)监听器
 */
public class ContextLoaderListener implements ServletContextListener {

    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("在服务器启动的时候，模拟创建SpringMVC的核心容器...");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("在服务器启动的时候，模拟销毁SpringMVC的核心容器...");
    }
}
```

###### ② 注册监听器

```xml
<listener>
    <listener-class>com.atguigu.listener.ContextLoaderListener</listener-class>
</listener>
```

### 3. 书城项目第六阶段

#### 3.1 登录检查

##### 3.1.1 目标

把项目中需要保护的功能保护起来，没有登录不允许访问

- 订单功能

##### 3.1.2 代码实现

###### ① 拦截受保护资源的请求

​	Filter拦截的地址：`/order`

###### ② LoginFilter代码

```java
@WebFilter(filterName = "LoginFilter",urlPatterns = "/order")
public class LoginFilter implements Filter {
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        //1. 检查是否处于登录状态(就是看session域内有没有user对象)
        HttpServletRequest request=(HttpServletRequest)req;
        HttpServletResponse response=(HttpServletResponse)resp;

        Object user = request.getSession().getAttribute("user");
        if(user==null){
            //未登录
            response.sendRedirect(request.getContextPath()+"/user?flag=toLoginPage");
        }else{
            chain.doFilter(req, resp);
        }
    }
    public void init(FilterConfig config) throws ServletException {}
    public void destroy() {}
}
```

# 第十章 书城项目第六阶段

## 1. 结账

### 1.1 创建订单模型

#### 1.1.1 物理建模

##### ① t_order表

```sql
CREATE TABLE t_order(
	order_id INT PRIMARY KEY AUTO_INCREMENT,
	order_sequence VARCHAR(200),
	create_time VARCHAR(100),
	total_count INT,
	total_amount DOUBLE,
	order_status INT,
	user_id INT
);
```

| 字段名         | 字段作用       |
| -------------- | -------------- |
| order_id       | 主键           |
| order_sequence | 订单号         |
| create_time    | 订单创建时间   |
| total_count    | 订单的总数量   |
| total_amount   | 订单的总金额   |
| order_status   | 订单的状态     |
| user_id        | 下单的用户的id |

- 虽然order_sequence也是一个不重复的数值，但是不使用它作为主键。数据库表的主键要使用没有业务功能的字段来担任。
- 订单的状态
  - 待支付（书城项目中暂不考虑）
  - 已支付，待发货：0
  - 已发货：1
  - 确认收货：2
  - 发起退款或退货（书城项目中暂不考虑）
- 用户id
  - 从逻辑和表结构的角度来说，这其实是一个外键。
  - 但是开发过程中建议先不要加外键约束：因为开发过程中数据尚不完整，加了外键约束开发过程中使用测试数据非常不方便，建议项目预发布时添加外键约束测试。



##### ② t_order_item表

```sql
CREATE TABLE t_order_item(
	item_id INT PRIMARY KEY AUTO_INCREMENT,
	book_name VARCHAR(20),
	price DOUBLE,
	img_path VARCHAR(50),
	item_count INT,
	item_amount DOUBLE,
	order_id VARCHAR(20)
);
```

| 字段名称    | 字段作用                     |
| ----------- | ---------------------------- |
| item_id     | 主键                         |
| book_name   | 书名                         |
| price       | 单价                         |
| item_count  | 当前订单项的数量             |
| item_amount | 当前订单项的金额             |
| order_id    | 当前订单项关联的订单表的主键 |

说明：book_name、author、price这三个字段其实属于t_book表，我们把它们加入到t_order_item表中，其实并不符合数据库设计三大范式。这里做不符合规范的操作的原因是：将这几个字段加入当前表就不必在显示数据时和t_book表做关联查询，提高查询的效率，这是一种变通的做法。

#### 1.1.2 逻辑模型

##### ① Order类

```java
package com.atguigu.bean;

/**
 * 包名:com.atguigu.bean
 *
 * @author chenxin
 * 日期2021-05-19  09:16
 */
public class Order {
    private Integer orderId;
    private String orderSequence;
    private String createTime;
    private Integer totalCount;
    private Double totalAmount;
    private Integer orderStatus;
    private Integer userId;

    @Override
    public String toString() {
        return "Order{" +
            "orderId=" + orderId +
            ", orderSequence='" + orderSequence + '\'' +
            ", createTime='" + createTime + '\'' +
            ", totalCount='" + totalCount + '\'' +
            ", totalAmount='" + totalAmount + '\'' +
            ", orderStatus=" + orderStatus +
            ", userId=" + userId +
            '}';
    }

    public Order() {
    }

    public Order(Integer orderId, String orderSequence, String createTime, Integer totalCount, Double totalAmount, Integer orderStatus, Integer userId) {
        this.orderId = orderId;
        this.orderSequence = orderSequence;
        this.createTime = createTime;
        this.totalCount = totalCount;
        this.totalAmount = totalAmount;
        this.orderStatus = orderStatus;
        this.userId = userId;
    }

    public Integer getOrderId() {
        return orderId;
    }

    public void setOrderId(Integer orderId) {
        this.orderId = orderId;
    }

    public String getOrderSequence() {
        return orderSequence;
    }

    public void setOrderSequence(String orderSequence) {
        this.orderSequence = orderSequence;
    }

    public String getCreateTime() {
        return createTime;
    }

    public void setCreateTime(String createTime) {
        this.createTime = createTime;
    }

    public Integer getTotalCount() {
        return totalCount;
    }

    public void setTotalCount(Integer totalCount) {
        this.totalCount = totalCount;
    }

    public Double getTotalAmount() {
        return totalAmount;
    }

    public void setTotalAmount(Double totalAmount) {
        this.totalAmount = totalAmount;
    }

    public Integer getOrderStatus() {
        return orderStatus;
    }

    public void setOrderStatus(Integer orderStatus) {
        this.orderStatus = orderStatus;
    }

    public Integer getUserId() {
        return userId;
    }

    public void setUserId(Integer userId) {
        this.userId = userId;
    }
}
```

##### ② OrdrItem类

```java
package com.atguigu.bean;

/**
 * 包名:com.atguigu.bean
 *
 * @author chenxin
 * 日期2021-05-19  10:13
 */
public class OrderItem {
    private Integer itemId;
    private String bookName;
    private Double price;
    private String imgPath;
    private Integer itemCount;
    private Double itemAmount;
    private Integer orderId;

    @Override
    public String toString() {
        return "OrderItem{" +
                "itemId=" + itemId +
                ", bookName='" + bookName + '\'' +
                ", price=" + price +
                ", imgPath='" + imgPath + '\'' +
                ", itemCount=" + itemCount +
                ", itemAmount=" + itemAmount +
                ", orderId=" + orderId +
                '}';
    }

    public OrderItem() {
    }

    public OrderItem(Integer itemId, String bookName, Double price, String imgPath, Integer itemCount, Double itemAmount, Integer orderId) {
        this.itemId = itemId;
        this.bookName = bookName;
        this.price = price;
        this.imgPath = imgPath;
        this.itemCount = itemCount;
        this.itemAmount = itemAmount;
        this.orderId = orderId;
    }

    public Integer getItemId() {
        return itemId;
    }

    public void setItemId(Integer itemId) {
        this.itemId = itemId;
    }

    public String getBookName() {
        return bookName;
    }

    public void setBookName(String bookName) {
        this.bookName = bookName;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    public String getImgPath() {
        return imgPath;
    }

    public void setImgPath(String imgPath) {
        this.imgPath = imgPath;
    }

    public Integer getItemCount() {
        return itemCount;
    }

    public void setItemCount(Integer itemCount) {
        this.itemCount = itemCount;
    }

    public Double getItemAmount() {
        return itemAmount;
    }

    public void setItemAmount(Double itemAmount) {
        this.itemAmount = itemAmount;
    }

    public Integer getOrderId() {
        return orderId;
    }

    public void setOrderId(Integer orderId) {
        this.orderId = orderId;
    }
}
```

### 1.2 创建组件

#### 1.2.1 持久化层

![image-20220829224736811](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224736811.png)

#### 1.2.2 业务逻辑层

![image-20220829224743243](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224743243.png)

#### 1.2.3 表述层

![image-20220829224749570](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224749570.png)

### 1.3 功能步骤

- 创建订单对象
- 给订单对象填充数据
  - 生成订单号
  - 生成订单的时间
  - 从购物车迁移总数量和总金额
  - 从已登录的User对象中获取userId并设置到订单对象中
- 将订单对象保存到数据库中
- 获取订单对象在数据库中自增主键的值
- 根据购物车中的CartItem集合逐个创建OrderItem对象
  - 每个OrderItem对象对应的orderId属性都使用前面获取的订单数据的自增主键的值
- 把OrderItem对象的集合保存到数据库
- 每一个item对应的图书增加销量、减少库存
- 清空购物车

### 1.4 案例思路

![image-20220829224759387](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224759387.png)

### 1.5 代码实现

#### 1.5.1 购物车页面结账超链接

cart.html

```html
<a class="pay" href="protected/orderClient?method=checkout">去结账</a>
```

#### 1.5.2 OrderClientServlet.checkout()

```java
package com.atguigu.bookstore.servlet.model;

import com.atguigu.bookstore.bean.Cart;
import com.atguigu.bookstore.constants.BookStoreConstants;
import com.atguigu.bookstore.entity.User;
import com.atguigu.bookstore.service.OrderService;
import com.atguigu.bookstore.service.impl.OrderServiceImpl;
import com.atguigu.bookstore.servlet.base.ModelBaseServlet;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;

/**
 * @author chenxin
 * 日期2021-06-19  14:18
 */
public class OrderClientServlet extends ModelBaseServlet {
    private OrderService orderService = new OrderServiceImpl();

    /**
     * 订单结算
     * @param request
     * @param response
     */
    public void checkout(HttpServletRequest request, HttpServletResponse response){
        try {
            //1. 从session中获取购物车信息
            HttpSession session = request.getSession();
            Cart cart = (Cart) session.getAttribute(BookStoreConstants.CARTSESSIONKEY);
            //2. 从session中获取用户信息
            User user = (User) session.getAttribute(BookStoreConstants.USERSESSIONKEY);
            //3. 调用业务层的方法，进行订单结算，并且获取订单的序列号
            String orderSequence = orderService.checkout(cart,user);
            //4. 清空购物车
            session.removeAttribute(BookStoreConstants.CARTSESSIONKEY);
            //5. 将订单序列号存储到请求域对象中，并且跳转到checkout.html页面
            request.setAttribute("orderSequence",orderSequence);
            processTemplate("cart/checkout",request,response);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

#### 1.5.3 OrderService.checkout()

```java
package com.atguigu.bookstore.service.impl;

import com.atguigu.bookstore.bean.Cart;
import com.atguigu.bookstore.bean.CartItem;
import com.atguigu.bookstore.constants.BookStoreConstants;
import com.atguigu.bookstore.dao.BookDao;
import com.atguigu.bookstore.dao.OrderDao;
import com.atguigu.bookstore.dao.OrderItemDao;
import com.atguigu.bookstore.dao.impl.BookDaoImpl;
import com.atguigu.bookstore.dao.impl.OrderDaoImpl;
import com.atguigu.bookstore.dao.impl.OrderItemDaoImpl;
import com.atguigu.bookstore.entity.Order;
import com.atguigu.bookstore.entity.User;
import com.atguigu.bookstore.service.OrderService;

import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.List;
import java.util.UUID;

/**
 * 包名:com.atguigu.bookstore.service.impl
 *
 * @author chenxin
 * 日期2021-06-19  14:19
 */
public class OrderServiceImpl implements OrderService {
    private OrderDao orderDao = new OrderDaoImpl();
    private OrderItemDao orderItemDao = new OrderItemDaoImpl();
    private BookDao bookDao = new BookDaoImpl();
    @Override
    public String checkout(Cart cart, User user) throws Exception {
        //1. 往订单表插入一条数据  
        //1.1 生成一个唯一的订单号(使用UUID)  
        String orderSequence = UUID.randomUUID().toString();
        //1.2 生成当前时间createTime
        String createTime = new SimpleDateFormat("dd-MM-yy:HH:mm:ss").format(new Date());
        //1.3 订单的totalCount就是cart的totalCount  
        Integer totalCount = cart.getTotalCount();
        //1.4 订单的totalAmount就是购物车的totalAmount
        Double totalAmount = cart.getTotalAmount();
        //1.5 设置订单的状态为0
        Integer status = BookStoreConstants.PAYED;
        //1.6 订单的userId就是user对象的id  
        Integer userId = user.getUserId();

        //将上述六个数据封装到一个Order对象中
        Order order = new Order(null,orderSequence,createTime,totalCount,totalAmount,status,userId);
        //1.7 调用持久层OrderDao的insertOrder方法添加订单数据，并且获取自增长的主键值
        Integer orderId = orderDao.insertOrder(order);

        //2. 往订单项表插入多条数据(采用批处理)
        //获取所有的购物项
        List<CartItem> cartItemList = cart.getCartItemList();
        //创建一个二维数组，用来做批量添加订单项的参数
        Object[][] orderItemArrParam = new Object[cartItemList.size()][6];

        //3. 更新t_book表中对应的书的sales和stock  
        //创建一个二维数组，用来做批量修改图书信息的参数
        Object[][] bookArrParam = new Object[cartItemList.size()][3];

        //遍历出每一个购物项
        for (int i=0;i<cartItemList.size();i++) {
            //封装批量添加订单项的二维数组参数
            //每一个购物项就对应一个订单项
            CartItem cartItem = cartItemList.get(i);
            //2.1 bookName就是CartItem的bookName
            //设置第i条SQL语句的第一个参数的值
            orderItemArrParam[i][0] = cartItem.getBookName();
            //2.2 price、imgPath、itemCount、itemAmount都是CartItem中对应的数据 
            //设置第i条SQL语句的第二个参数的值
            orderItemArrParam[i][1] = cartItem.getPrice();
            //设置第i条SQL语句的第三个参数的值
            orderItemArrParam[i][2] = cartItem.getImgPath();
            //设置第i条SQL语句的第四个参数的值
            orderItemArrParam[i][3] = cartItem.getCount();
            //设置第i条SQL语句的第五个参数的值
            orderItemArrParam[i][4] = cartItem.getAmount();
            //2.3 orderId就是第一步中保存的订单的id
            //设置第i条SQL语句的第六个参数的值
            orderItemArrParam[i][5] = orderId;

            //封装批量更新图书库存和销量的二维数组参数
            // 设置第i条SQL语句的第一个参数: 就是要增加的销量就是cartItem的count
            bookArrParam[i][0] = cartItem.getCount();
            // 设置第i条SQL语句的第二个参数: 就是要减少的库存就是cartItem的count
            bookArrParam[i][1] = cartItem.getCount();
            // 设置第i条SQL语句的第三个参数: 就是要修改的图书的bookId就是cartItem的bookId
            bookArrParam[i][2] = cartItem.getBookId();
        }
        //2.4 调用持久层OrderItemDao的insertOrderItemArr方法进行批量添加
        orderItemDao.insertOrderItemArr(orderItemArrParam);



        //3.1 调用持久层BookDao的updateBookArr方法进行批量更新
        bookDao.updateBookArr(bookArrParam);

        //4. 返回订单号
        return orderSequence;
    }
}
```



#### 1.5.4 orderDao.insertOrder(order)

```java
package com.atguigu.bookstore.dao.impl;

import com.atguigu.bookstore.dao.BaseDao;
import com.atguigu.bookstore.dao.OrderDao;
import com.atguigu.bookstore.entity.Order;
import com.atguigu.bookstore.utils.JDBCUtil;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.Statement;

/**
 * 包名:com.atguigu.bookstore.dao.impl
 *
 * @author chenxin
 * 日期2021-06-19  14:19
 */
public class OrderDaoImpl extends BaseDao<Order> implements OrderDao {
    @Override
    public Integer insertOrder(Order order) throws Exception {
        String sql = "insert into t_order (order_sequence,create_time,total_count,total_amount,order_status,user_id) values (?,?,?,?,?,?)";
        //因为使用DBUtils执行增删改的SQL语句没法获取自增长的id主键，所以我们只能使用原始的JDBC执行这个添加数据的SQL语句并且获取自增长的id
        //1. 获取连接
        Connection conn = JDBCUtil.getConnection();
        //2. 预编译SQL语句
        PreparedStatement preparedStatement = conn.prepareStatement(sql, Statement.RETURN_GENERATED_KEYS);
        //3. 设置问号处的参数
        preparedStatement.setObject(1, order.getOrderSequence());
        preparedStatement.setObject(2, order.getCreateTime());
        preparedStatement.setObject(3, order.getTotalCount());
        preparedStatement.setObject(4, order.getTotalAmount());
        preparedStatement.setObject(5, order.getOrderStatus());
        preparedStatement.setObject(6, order.getUserId());
        //4. 执行SQL语句
        preparedStatement.executeUpdate();
        //5. 获取自增长的主键值
        ResultSet rst = preparedStatement.getGeneratedKeys();
        //因为自增长的主键只有一个值，所以不需要while循环遍历
        int orderId = 0;
        if (rst.next()) {
            orderId = rst.getInt(1);
        }
        //关闭连接
        JDBCUtil.releaseConnection(conn);
        return orderId;
    }
}
```



#### 1.5.5 orderItemDao.insertOrderItemArr(insertOrderItemParamArr)

```java
package com.atguigu.bookstore.dao.impl;

import com.atguigu.bookstore.dao.BaseDao;
import com.atguigu.bookstore.dao.OrderItemDao;
import com.atguigu.bookstore.entity.OrderItem;

/**
 * 包名:com.atguigu.bookstore.dao.impl
 *
 * @author chenxin
 * 日期2021-06-19  14:19
 */
public class OrderItemDaoImpl extends BaseDao<OrderItem> implements OrderItemDao {
    @Override
    public void insertOrderItemArr(Object[][] paramArr) {
        String sql = "insert into t_order_item (book_name,price,img_path,item_count,item_amount,order_id) values (?,?,?,?,?,?)";
        batchUpdate(sql,paramArr);
    }
}
```



#### 1.5.6 bookDao.updateBookArr(updateBookParamArr)

```java
@Override
public void updateBookArr(Object[][] bookArrParam) throws Exception {
    String sql = "update t_book set sales=sales+?,stock=stock-? where book_id=?";
    batchUpdate(sql,bookArrParam);
}
```

## 2. 结账过程中使用事务

### 2.1 事务回顾

#### 2.1.1 ACID属性

- A：原子性 事务中包含的数据库操作缺一不可，整个事务是不可再分的。

- C：一致性 事务执行之前，数据库中的数据整体是正确的；事务执行之后，数据库中的数据整体仍然是正确的。
  - 事务执行成功：提交（commit）
  - 事务执行失败：回滚（rollback）

- I：隔离性 数据库系统同时执行很多事务时，各个事务之间基于不同隔离级别能够在一定程度上做到互不干扰。简单说就是：事务在并发执行过程中彼此隔离。

- D：持久性 事务一旦提交，就永久保存到数据库中，不可撤销。

#### 2.1.2 隔离级别

##### ① 并发问题

| 并发问题   | 问题描述                                                     |
| ---------- | ------------------------------------------------------------ |
| 脏读       | 当前事务读取了其他事务尚未提交的修改<br />如果那个事务回滚，那么当前事务读取到的修改就是错误的数据 |
| 不可重复读 | 当前事务中多次读取到的数据的内容不一致(数据行数一致，但是行中的具体内容不一致) |
| 幻读       | 当前事务中多次读取到的数据行数不一致                         |

##### ② 隔离级别

| 隔离级别 | 描述                                           | 能解决的并发问题       |
| -------- | ---------------------------------------------- | ---------------------- |
| 读未提交 | 允许当前事务读取其他事务尚未提交的修改         | 啥问题也解决不了       |
| 读已提交 | 允许当前事务读取其他事务已经提交的修改         | 脏读                   |
| 可重复读 | 当前事务执行时锁定当前记录，不允许其他事务操作 | 脏读、不可重复读       |
| 串行化   | 当前事务执行时锁定当前表，不允许其他事务操作   | 脏读、不可重复读、幻读 |

### 2.2 JDBC事务控制

#### 2.2.1 同一个数据库连接

只有当多次数据库操作是使用的同一个连接的时候，才能够保证这几次数据库操作在同一个事务中执行

![image-20220829224817906](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224817906.png)



#### 2.2.2 关闭事务的自动提交

```java
connection.setAutoCommit(false);
```



#### 2.2.3 提交事务

```java
connection.commit();
```



#### 2.2.4 回滚事务

```java
connection.rollBack();
```



#### 2.2.5 事务整体的代码块

```java
try{
    
    // 关闭事务的自动提交
    connection.setAutoCommit(false);
    
    // 事务中包含的所有数据库操作
    
    // 提交事务
    connection.commit();
}catch(Excetion e){
    
    // 回滚事务
    connection.rollBack();
    
} finally {
    connection.setAutoCommit(true);
    //回收到连接池
    connection.close();
}
```

### 2.3 将事务对接到书城项目中

#### 2.3.1 三层架构中事务要对接的位置

从逻辑上来说，一个事务对应一个业务方法（Service层的一个方法）。

![image-20220829224826574](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224826574.png)

#### 2.3.2 假想

每一个Service方法内部，都套用了事务操作所需要的try...catch...finally块。

#### 2.3.3 假想代码的缺陷

- 会出现大量的冗余代码：我们希望能够抽取出来，只写一次
- 对核心业务功能是一种干扰：我们希望能够在编写业务逻辑代码时专注于业务本身，而不必为辅助性质的套路代码分心
- 将持久化层对数据库的操作写入业务逻辑层，是对业务逻辑层的一种污染，导致持久化层和业务逻辑层耦合在一起

#### 2.3.4 事务代码抽取

- 只要是Filter拦截到的请求都会从doFilter()方法经过
- chain.doFilter(req, resp);可以包裹住将来要执行的所有方法
- 事务操作的try...catch...finally块只要把chain.doFilter(req, resp)包住，就能够包住将来要执行的所有方法

#### 2.3.5 编写一个TransactionFilter来统一处理事务

```java
package com.atguigu.bookstore.filter;

import com.atguigu.bookstore.utils.JDBCUtil;

import javax.servlet.*;
import java.io.IOException;
import java.sql.Connection;
import java.sql.SQLException;


public class TransactionFilter implements Filter {
    @Override
    public void destroy() {
    }

    @Override
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
        Connection conn = null;
        try {
            //开启事务
            conn = JDBCUtil.getConnection();
            conn.setAutoCommit(false);
            chain.doFilter(req, resp);
            //没有出现异常，则提交事务
            conn.commit();
        } catch (Exception e) {
            e.printStackTrace();
            //出现异常,回滚事务
            try {
                conn.rollback();
                //
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
            throw new RuntimeException(e.getMessage());
        }
    }

    @Override
    public void init(FilterConfig config) throws ServletException {

    }

}
```

#### 2.3.6 配置TransactionFilter指定其拦截要进行事务控制的请求

```xml
<filter>
    <filter-name>TransactionFilter</filter-name>
    <filter-class>com.atguigu.filter.TransactionFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>TransactionFilter</filter-name>
   	<!--
		哪些请求要使用TransactionFilter做事务控制，这里就配置哪些请求的地址
	-->
    <url-pattern>/order</url-pattern>
</filter-mapping>
```



#### 2.3.7 保证所有数据库操作使用同一个连接

<span style="color:blue;font-weight:bold;">『重要发现』</span>：在书城项目中所有执行SQL语句的代码都是通过<span style="color:blue;font-weight:bold;">JDBCUtils.getConnection()</span>方法获取数据库连接。所以我们可以通过<span style="color:blue;font-weight:bold;">重构JDBCUtils.getConnection()</span>方法实现：所有数据库操作使用同一个连接。

![image-20220829224837914](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224837914.png)

##### ① 从数据源中只拿出一个

为了保证各个需要Connection对象的地方使用的都是同一个对象，我们从数据源中只获取一个Connection。不是说整个项目只用一个Connection，而是说调用JDBCUtils.getConnection()方法时，只使用一个。所以落实到代码上就是：每次调用getConnection()方法时先检查是否已经拿过了，拿过就给旧的，没拿过给新的。

##### ② 公共区域

为了保证各个方法中需要Connection对象时都能拿到同一个对象，需要做到：将唯一的对象存入一个大家都能接触到的地方。

![image-20220829224844296](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224844296.png)

<span style="color:blue;font-weight:bold;">结论</span>：使用<span style="color:blue;font-weight:bold;">线程本地化</span>技术实现Connection对象从上到下传递。

#### 2.3.8 线程本地化

##### ① 确认同一个线程

在从Filter、Servlet、Service一直到Dao运行的过程中，我们始终都没有做类似new Thread().start()这样开启新线程的操作，所以整个过程在同一个线程中。

##### ② 一条小河

![image-20220829224852185](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224852185.png)

##### ③ 一个线程

![image-20220829224857887](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220829224857887.png)

##### ④ ThreadLocal的API

1. set(T t)方法：在当前线程中，往ThreadLocal对象中存入一个数据

2. get()方法：在当前线程中，从ThreadLocal对象中取出数据
3. remove()方法: 移除ThreadLocal中保存的当前线程的数据

##### ⑤ 结论

TheadLocal的基本结论: 一个ThreadLocal对象，在一个线程中只能存储一个数据，在该线程的任何地方调用get()方法获取到的都是同一个数据

### 2.4 代码实现

#### 2.4.1 重构JDBCUtil类

- 要点1：将ThreadLocal对象声明为静态成员变量
- 要点2：重构获取数据库连接的方法
- 要点3：重构释放数据库连接的方法

```java
package com.atguigu.bookstore.utils;

import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.SQLException;
import java.util.Properties;

/**
 * 这个工具类中会提供仨方法:
 * 1. 获取连接池对象
 * 2. 从连接池中获取连接
 * 3. 将链接归还到连接池
 */
public class JDBCUtil {
    private static DataSource dataSource;
    private static ThreadLocal<Connection> threadLocal = new ThreadLocal<>();
    static {
        try {
            //1. 使用类加载器读取配置文件，转成字节输入流
            InputStream is = JDBCUtil.class.getClassLoader().getResourceAsStream("druid.properties");
            //2. 使用Properties对象加载字节输入流
            Properties properties = new Properties();
            properties.load(is);
            //3. 使用DruidDataSourceFactory创建连接池对象
            dataSource = DruidDataSourceFactory.createDataSource(properties);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取连接池对象
     * @return
     */
    public static DataSource getDataSource(){

        return dataSource;
    }

    /**
     * 获取连接
     * @return
     */
    public static Connection getConnection() {
        try {
            Connection conn = threadLocal.get();
            if (conn == null) {
                //说明此时ThreadLocal中没有连接
                //从连接池中获取一个连接
                conn = dataSource.getConnection();
                //将连接存储到ThreadLocal中
                threadLocal.set(conn);
            }
            return conn;
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e.getMessage());
        }
    }

    /**
     * 释放连接
     */
    public static void releaseConnection(){
        try {
            //这里是获取要被关闭的连接
            Connection conn = JDBCUtil.getConnection();
            //1. 先将conn的AutoCommit设置为true
            conn.setAutoCommit(true);
            //2. 将conn从ThreadLocal中移除掉
            threadLocal.remove();
            //3. 将conn归还到连接池
            conn.close();
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e.getMessage());
        }
    }
}
```

#### 2.4.2 重构BaseDao

- 要点：去除释放数据库连接的操作（转移到过滤器中）

```java
package com.atguigu.dao;

import com.atguigu.utils.JDBCUtil;
import org.apache.commons.dbutils.QueryRunner;
import org.apache.commons.dbutils.handlers.BeanHandler;
import org.apache.commons.dbutils.handlers.BeanListHandler;

import java.sql.Connection;
import java.sql.SQLException;
import java.util.List;


public class BaseDao<T> {
    private QueryRunner queryRunner = new QueryRunner();

    /**
     * 批处理方法
     * @param sql
     * @param paramArr
     * @return
     */
    public int[] batchUpdate(String sql,Object[][] paramArr){
        Connection conn = JDBCUtil.getConnection();
        try {
            return queryRunner.batch(conn,sql,paramArr);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e.getMessage());
        }
    }
    /**
     * 执行增删改的sql语句
     * @param sql
     * @param params
     * @return
     */
    public int update(String sql,Object... params){
        Connection conn = JDBCUtil.getConnection();
        try {
            //执行增删改的sql语句，返回受到影响的行数
            return queryRunner.update(conn,sql,params);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e.getMessage());
        }
    }

    /**
     * 执行查询一行数据的sql语句，将结果集封装到JavaBean对象中
     * @param clazz
     * @param sql
     * @param params
     * @return
     */
    public T getBean(Class<T> clazz,String sql,Object... params){
        Connection conn = JDBCUtil.getConnection();
        try {
            return queryRunner.query(conn,sql,new BeanHandler<>(clazz),params);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e.getMessage());
        }
    }

    /**
     * 执行查询多行数据的sql语句，并且将结果集封装到List<JavaBean>
     * @param clazz
     * @param sql
     * @param params
     * @return
     */
    public List<T> getBeanList(Class<T> clazz, String sql, Object... params){
        Connection conn = JDBCUtil.getConnection();
        try {
            return queryRunner.query(conn,sql,new BeanListHandler<>(clazz),params);
        } catch (SQLException e) {
            e.printStackTrace();
            throw new RuntimeException(e.getMessage());
        }
    }
}
```

<span style="color:blue;font-weight:bold;">注意</span>：OrderDao中insertOrder()方法也要去掉关闭数据库连接的操作。

```java
@Override
public void insertOrder(Order order) throws Exception{
    ResultSet resultSet = null;
    Connection conn = null;
    try {
        //往t_order表中插入一条订单信息
        //使用DBUtils没法获取自增长的主键值，所以我们只能使用原始的JDBC执行SQL语句，获取自增长的主键
        String sql = "insert into t_order (order_sequence,create_time,total_count,total_amount,order_status,user_id) values (?,?,?,?,?,?)";
        conn = JDBCUtil.getConnection();

        //预编译，并且指定获取自增长主键
        PreparedStatement preparedStatement = conn.prepareStatement(sql, PreparedStatement.RETURN_GENERATED_KEYS);

        //设置参数
        preparedStatement.setObject(1, order.getOrderSequence());
        preparedStatement.setObject(2, order.getCreateTime());
        preparedStatement.setObject(3, order.getTotalCount());
        preparedStatement.setObject(4, order.getTotalAmount());
        preparedStatement.setObject(5, order.getOrderStatus());
        preparedStatement.setObject(6, order.getUserId());

        //执行sql语句
        preparedStatement.executeUpdate();

        //获取自增长主键值
        resultSet = preparedStatement.getGeneratedKeys();
        if (resultSet.next()) {
            int orderId = resultSet.getInt(1);
            //将orderId存入到order对象中
            order.setOrderId(orderId);
        }
    } catch (SQLException e) {
        e.printStackTrace();
        throw new RuntimeException(e.getMessage());
    }
}
```

#### 2.4.3 全局统一的异常处理

1. 所有的Dao和Service的方法都抛最大的异常

2. 在Servlet中对异常进行try......catch，在catch中做相应的处理(例如跳转到错误页面)，然后在当前方法中throw new RuntimeException(e.getMessage());

3. 在ModelBaseServlet的catch块里面throw new RuntimeException(e.getMessage())

4. 在LoginFilter、TransactionFilter、CloseConnectionFilter中都需要对异常进行try...catch,然后在catch块中

   throw new RuntimeException(e.getMessage());

5. 创建一个ExceptionFilter，该Filter要配置在所有的Filter之前，用来统一处理异常

   ```java
   package com.atguigu.bookstore.filter;
   
   import javax.servlet.*;
   import javax.servlet.http.HttpServletRequest;
   import java.io.IOException;
   
   public class ExceptionFilter implements Filter {
       @Override
       public void destroy() {
       }
   
       @Override
       public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException {
           HttpServletRequest request = (HttpServletRequest) req;
           try {
               chain.doFilter(req, resp);
           } catch (Exception e) {
               e.printStackTrace();
               //跳转到异常页面
               request.getRequestDispatcher("/WEB-INF/pages/error.html").forward(request, resp);
           }
       }
   
       @Override
       public void init(FilterConfig config) throws ServletException {
   
       }
   
   }
   ```

