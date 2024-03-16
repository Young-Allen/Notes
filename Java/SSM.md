##   ①Maven

### 回顾：JavaSE   JavaWeb   SSM阶段   高级   项目阶段 ....

> #### JavaSE
>
> - JDK  变量  数据类型  运算符  流程控制  数组   类和对象【JavaOOP】 集合与泛型  IO  常用类  **反射**  设计模式
>
>   网络编程   JDBC  Mysql ...
>
> #### JavaWeb【B/S】
>
> - 浏览器：HTML、CSS、JavaScript、Vue、Axios、Cookie ...
> - 服务器：Tomcat、Http、**Servlet、Filter、Listener**、Session、Thymeleaf...
>
> ### SSM【Spring  SpringMVC  Mybatis】

### 第一章 为什么使用Maven

- 获取jar包

  - 使用Maven之前，自行在网络中下载jar包，效率较 低。如【谷歌、百度、CSDN....】
  - 使用Maven之后，统一在一个地址下载资源jar包【阿里云镜像服务器等...】

- 添加jar包

  - 使用Maven之前，将jar复制到项目工程中，jar包添加到项目中，相对浪费存储空间
  - 使用Maven之后，jar包统一存储Maven本地仓库，使用坐标方式将jar包从仓库引入到项目中
    ![image-20220826001159068](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220826001159068.png)

- 使用Maven便于解决jar包**冲突及依赖**问题

### 第二章 什么是Maven

- Maven字面意：专家、内行
- Maven是一款自动化构建工具，专注服务于Java平台的**项目构建**和**依赖管理**。
- 依赖管理：jar之间的依赖关系，jar包管理问题统称为依赖管理
- **项目构建**：项目构建不等同于项目创建
  - 项目构建是一个过程【7步骤组成】，项目创建是瞬间完成的
    1. 清理（mvn clean）：删除以前的编译结果，为重新编译做好准备
    2. 编译（mvn compile）：将java源程序编译为字节码文件
    3. 测试（mvn test）：针对项目中的关键点进行测试，却白项目在迭代开发过程中关键点的正确性
    4. 报告：在每一次测试后以标准的格式记录和展示测试结果
    5. 打包（mvn package）：将一个包含诸多文件的工程封装为一个压缩文件用于安装或部署。java工程对他jar包，web工程对于wer包
    6. 安装（mvn install）：在mavne环境下特指将打包的结果——jar包或war包安装到本地仓库中
    7. 部署：将打包的结果部署到远程仓库或将war包部署到服务器上运行

### 第三章 Maven基本使用

#### 3.1 Maven准备

> 注意：IDEA2019.1.x 最高支持Maven的3.6.0

- 下载地址：http://maven.apache.org/
- Maven底层使用Java语言编写的，所有需要配置JAVA_HOME环境变量及Path
- 将Maven解压**非中文无空格**目录下
- 配置**MAVEN_HOME**环境变量及Path
- 输入【cmd】,进入命令行窗口，输入**【mvn   -v】** ，检查Maven环境是否搭建成功

#### 3.2 Maven基本配置

- Maven配置文件位置：maven根目录/conf/settings.xml

- 设置本地仓库【默认：C:/用户家目录/.m2/repository】

  ```xml
  <!-- localRepository
     | The path to the local repository maven will use to store artifacts.
     |
     | Default: ${user.home}/.m2/repository
    <localRepository>/path/to/local/repo</localRepository>
    -->
    <localRepository>E:\SG_220106\LocalRepository</localRepository>
  ```

- 设置阿里云镜像服务器

  ```xml
  <mirrors>
      <!-- mirror
       | Specifies a repository mirror site to use instead of a given repository. The repository that
       | this mirror serves has an ID that matches the mirrorOf element of this mirror. IDs are used
       | for inheritance and direct lookup purposes, and must be unique across the set of mirrors.
       |
      <mirror>
        <id>mirrorId</id>
        <mirrorOf>repositoryId</mirrorOf>
        <name>Human Readable Name for this Mirror.</name>
        <url>http://my.repository.com/repo/path</url>
      </mirror>
       -->
  	 <mirror>
          <id>nexus-aliyun</id>
          <mirrorOf>central</mirrorOf>
          <name>Nexus aliyun</name>
          <url>http://maven.aliyun.com/nexus/content/groups/public</url>
      </mirror>
    </mirrors>
  ```

- 设置使用JDK版本【1.8|JDK8】

  ```xml
  <profiles>
  <profile>
        <id>jdk-1.8</id>
        <activation>
          <activeByDefault>true</activeByDefault>
          <jdk>1.8</jdk>
        </activation>
        <properties>
          <maven.compiler.source>1.8</maven.compiler.source>
          <maven.compiler.target>1.8< /maven.compiler.target>
          <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
        </properties>
      </profile>
    </profiles>
  ```

#### 3.3 Maven之Helloworld

> 约束>配置>代码

- Maven工程目录结构约束
  - 项目名
    - src【书写源代码】
      - main【书写主程序代码】
        - java【书写java源代码】
        - resources【书写配置文件代码】
      - test【书写测试代码】
        - java【书写测试代码】
    - pom.xml【书写Maven配置】

- 测试步骤
  - **进入项目名根目录【在根目标输入cmd即可】**
  - mvn clean 
  - mvn compile
  - mvn test-compile
  - mvn test
  - mvn package
  - mvn install

### 第四章 Maven及Idea的相关应用

#### 4.1 将Maven整合到IDEA中

![image-20220826001214577](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20220826001214577.png)

![image-20220826001231531](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20220826001231531.png)

#### 4.2 在IDEA中新建Maven工程

![image-20220826001249597](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220826001249597.png)

![](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220826001249597.png)

### 第五章 Maven核心概念

#### 5.1 Maven的POM

- POM全称：Project Object Model【项目对象模型】，将项目封装为对象模型，便于使用Maven管理【构建】项目

- pom.xml常用标签

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <!--    设置父工程坐标-->
      <parent>
          <artifactId>maven_demo</artifactId>
          <groupId>com.atguigu</groupId>
          <version>1.0-SNAPSHOT</version>
      </parent>
      <modelVersion>4.0.0</modelVersion>
  
      <artifactId>maven_helloworld</artifactId>
  
      <dependencies>
          <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>4.12</version>
              <scope>test</scope>
          </dependency>
      </dependencies>
  </project>
  ```

#### 5.2 Maven约定的目录结构

- 项目名
  - src【书写java源代码】
    - main【书写java主程序代码】
      - java【书写java代码】
      - resources【书写配置文件代码】
    - test【书写测试代码】
      - java【书写测试java代码】
  - pom.xml【书写配置文件代码】
  - target【编译后目录结构】

#### 5.3 Maven生命周期

- Maven生命周期：按照顺序执行各个命令，Maven生命周期包含以下三个部分组成
  - Clean LifeCycle：在进行真正的构建之前进行一些清理工作。
  - **Default LifeCycle：构建的核心部分，编译，测试，打包，安装，部署等等。**
  - Site LifeCycle：生成项目报告，站点，发布站点。

![image-20220826001313822](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220826001313822.png)

#### 5.4 Maven插件和目标

- 插件：插件本质是由jar包和配置文件组成
- 目标：每个插件都能实现多个功能，每个功能就是一个插件目标。

#### 5.5 Maven的仓库【重要】

- 仓库分类
  - 本地仓库：为当前计算机提供maven服务
  - 远程仓库：为其他计算机也可以提供maven服务
    - 私服：架设在当前局域网环境下，为当前局域网范围内的所有Maven工程服务。
    - 中央仓库：架设在Internet上，为全世界所有Maven工程服务。
    - 中央仓库的镜像：架设在各个大洲，为中央仓库分担流量。减轻中央仓库的压力，同时更快的响应用户请求。
- 仓库中的文件类型【jar包】
  - Maven的插件
  - 第三方框架或工具的jar包
  - 自己研发的项目或模块

#### 5.6 Maven的坐标【重要】

- **作用：使用坐标引入jar包**

- 坐标由g-a-v组成

  [1]**groupId**：公司或组织的域名倒序+当前项目名称

  [2]**artifactId**：当前项目的模块名称

  [3]**version**：当前模块的版本

- 注意

  - g-a-v：本地仓库jar包位置
  - a-v：jar包全名

- 坐标应用

  - **坐标参考网址：http://mvnrepository.com**

  - 语法，示例

    ```xml
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    
        <!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>4.0.1</version>
            <scope>provided</scope>
        </dependency>
    
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.17</version>
        </dependency>
    </dependencies>
    ```

### 第六章 Maven的依赖管理

#### 6.1 依赖范围

- 依赖语法：\<scope>
  - compile【默认值】：在main、test、Tomcat【服务器】下均有效。
  - test：只能在test目录下有效
    - junit
  - provided：在main、test下均有效，Tomcat【服务器】无效。
    - servlet-api

#### 6.2 依赖传递性

- **路径最短者有先【就近原则】**
- **先声明者优先**

- 注意：Maven可以自动解决jar包之间的依赖问题

### 第七章 Maven中统一管理版本号

- 语法

  ```xml
  <properties>
      <spring-version>5.3.17</spring-version>
  </properties>
  <dependencies>
      <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-beans</artifactId>
              <version>${spring-version}</version>
      </dependency>
  </dependencies>
  ```

### 第七章 Maven的继承

#### 7.1 为什么需要继承

- 如子工程大部分都共同使用jar包，可以提取父工程中，使用【继承原理】在子工程中使用
- 父工程打包方式，必须是pom方式

#### 7.2 Maven继承方式一

- 在父工程中的pom.xml中导入jar包，在子工程中统一使用。【所有子工程强制引入父工程jar包】

- 示例代码

  ```xml
  <packaging>pom</packaging>
  <dependencies>
          <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>4.12</version>
              <scope>test</scope>
          </dependency>
      </dependencies>
  ```

#### 7.3 Maven继承方式二

- 在父工程中导入jar包【pom.xml】

  ```xml
  <packaging>pom</packaging>
  <dependencyManagement>
      <dependencies>
          <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>4.12</version>
              <scope>test</scope>
          </dependency>
      </dependencies>
  </dependencyManagement>
  ```

- 在子工程引入父工程的相关jar包

  ```xml
  <parent>
      <artifactId>maven_demo</artifactId>
      <groupId>com.atguigu</groupId>
      <version>1.0-SNAPSHOT</version>
      <relativePath>../pom.xml</relativePath>
  </parent>
   <dependencies>
          <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
          </dependency>
  </dependencies>
  ```

- **注意：在子工程中，不能指定版本号**

### 第八章 Maven的聚合

- 为什么使用Maven的聚合

  - 优势：只要将子工程聚合到父工程中，就可以实现效果：安装或清除父工程时，子工程会进行同步操作。
  - 注意：Maven会按照依赖顺序自动安装子工程

- 语法

  ```xml
  <modules>
      <module>maven_helloworld</module>
      <module>HelloFriend</module>
      <module>MakeFriend</module>
  </modules>
  ```

## ②Mybatis

###   第一章 初识Mybatis

#### 1.1 框架概述

- #### 生活中“框架”

  - 买房子
  - 笔记本电脑

- 程序中框架【代码半成品】

  - Mybatis框架：持久化层框架【dao层】
  - SpringMVC框架：控制层框架【Servlet层】
  - Spring框架：全能...

#### 1.2 Mybatis简介

- Mybatis是一个**半自动化**持久化层**ORM**框架
- ORM：Object Relational Mapping【对象  关系  映射】
  - 将Java中的**对象**与数据库中**表**建议**映射关系**，优势：操作Java中的对象，就可以影响数据库中表的数据

- Mybatis与Hibernate对比
  - Mybatis是一个半自动化【需要手写SQL】
  - Hibernate是全自动化【无需手写SQL】

- Mybatis与JDBC对比
  - JDBC中的SQL与Java代码耦合度高
  - Mybatis将SQL与Java代码解耦
- Java POJO（Plain Old Java Objects，普通老式 Java 对象）
  - JavaBean  等同于  POJO

#### 1.3 官网地址

- 文档地址：https://mybatis.org/mybatis-3/
- 源码地址：https://github.com/mybatis/mybatis-3

### 第二章 搭建Mybatis框架

> 导入jar包
>
> 编写配置文件
>
> 使用核心类库

#### 2.1 准备

- 建库建表建约束
- 准备maven工程

#### 2.2 搭建Mybatis框架步骤

1. 导入jar包

   ```xml
   <!--导入MySQL的驱动包-->
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>5.1.37</version>
   </dependency>
   <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>8.0.26</version>
   </dependency>
   
   <!--导入MyBatis的jar包-->
   <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis</artifactId>
       <version>3.5.6</version>
   </dependency>
   <!--junit-->
   <dependency>
       <groupId>junit</groupId>
       <artifactId>junit</artifactId>
       <version>4.12</version>
       <scope>test</scope>
   </dependency>
   ```

2. 编写**核心配置文件**【mybatis-config.xml】

   - 位置：resources目标下

   - 名称：推荐使用mybatis-config.xml

   - 示例代码

     ```xml
     <?xml version="1.0" encoding="UTF-8" ?>
     <!DOCTYPE configuration
             PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
             "http://mybatis.org/dtd/mybatis-3-config.dtd">
     
     <configuration>
         <environments default="development">
             <environment id="development">
                 <transactionManager type="JDBC"/>
                 <dataSource type="POOLED">
     <!--                mysql8版本-->
     <!--                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>-->
     <!--                <property name="url" value="jdbc:mysql://localhost:3306/db220106?serverTimezone=UTC"/>-->
     <!--                mysql5版本-->
                     <property name="driver" value="com.mysql.jdbc.Driver"/>
                     <property name="url" value="jdbc:mysql://localhost:3306/db220106"/>
                     <property name="username" value="root"/>
                     <property name="password" value="root"/>
                 </dataSource>
             </environment>
         </environments>
         <!--    设置映射文件路径-->
         <mappers>
             <mapper resource="mapper/EmployeeMapper.xml"/>
         </mappers>
     </configuration>
     ```

3. 书写相关接口及**映射文件**

   - 映射文件位置：resources/mapper

   - 映射文件名称：XXXMapper.xml

   - **映射文件作用：主要作用为Mapper接口书写Sql语句**

     - 映射文件名与接口名一致
     - 映射文件namespace与接口全类名一致
     - 映射文件SQL的Id与接口的方法名一致

   - 示例代码

     ```xml
     <?xml version="1.0" encoding="UTF-8" ?>
     <!DOCTYPE mapper
             PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
             "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
     <mapper namespace="com.atguigu.mybatis.mapper.EmployeeMapper">
         <select id="selectEmpById" resultType="com.atguigu.mybatis.pojo.Employee">
             SELECT
                 id,
                 last_name,
                 email,
                 salary
             FROM
                 tbl_employee
             WHERE
                 id=#{empId}
         </select>
     </mapper>
     ```

4. 测试【SqlSession】

   - 先获取SqlSessionFactory对象
   - 再获取SqlSession对象
   - 通过SqlSession对象获取XXXMapper代理对象
   - 测试

#### 2.3 添加Log4j日志框架

- 导入jar包

  ```xml
  <!-- log4j -->
  <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.17</version>
  </dependency>
  ```

- 编写配置文件

  - 配置文件名称：log4j.xml

  - 配置文件位置：resources

  - 示例代码

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
    
    <log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
    
        <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
            <param name="Encoding" value="UTF-8" />
            <layout class="org.apache.log4j.PatternLayout">
                <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS} %m  (%F:%L) \n" />
            </layout>
        </appender>
        <logger name="java.sql">
            <level value="debug" />
        </logger>
        <logger name="org.apache.ibatis">
            <level value="info" />
        </logger>
        <root>
            <level value="debug" />
            <appender-ref ref="STDOUT" />
        </root>
    </log4j:configuration>
    ```



### 第三章 Mybatis核心配置详解【mybatis-config.xml】

#### 3.1 核心配置文件概述

- MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。

#### 3.2 核心配置文件根标签

- 没有实际语义，主要作用：所有子标签均需要设置在跟标签内部

#### 3.3 核心配置文件常用子标签

- properties子标签

  - 作用：定义或引入外部属性文件

  - 示例代码

    ```properties
    #key=value
    db.driver=com.mysql.jdbc.Driver
    db.url=jdbc:mysql://localhost:3306/db220106
    db.username=root
    db.password=root
    ```

    ```xml
    <properties resource="db.properties"></properties>
    
    <environments default="development">
            <environment id="development">
                <transactionManager type="JDBC"/>
                <dataSource type="POOLED">
    <!--                mysql8版本-->
    <!--                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>-->
    <!--                <property name="url" value="jdbc:mysql://localhost:3306/db220106?serverTimezone=UTC"/>-->
    <!--                mysql5版本-->
                    <property name="driver" value="${db.driver}"/>
                    <property name="url" value="${db.url}"/>
                    <property name="username" value="${db.username}"/>
                    <property name="password" value="${db.password}"/>
                </dataSource>
            </environment>
        </environments>
    ```

- settings子标签

  - 作用：这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。 

  - **mapUnderscoreToCamelCase**属性：是否开启驼峰命名自动映射，默认值false，如设置true会自动将

    字段a_col与aCol属性自动映射

    - **注意：只能将字母相同的字段与属性自动映射**

- 类型别名（typeAliases）

  - 作用：类型别名可为 Java 类型设置一个缩写名字。

  - 语法及特点

    ```xml
    <typeAliases>
    <!--        为指定类型定义别名-->
    <!--        <typeAlias type="com.atguigu.mybatis.pojo.Employee" alias="employee"></typeAlias>-->
    <!--        为指定包下所有的类定义别名
                    默认将类名作为别名，不区分大小写【推荐使用小写字母】
    -->
            <package name="com.atguigu.mybatis.pojo"/>
        </typeAliases>
    ```

  - Mybatis自定义别名

    | 别名            | 类型      |
    | --------------- | --------- |
    | _int            | int       |
    | integer或int    | Integer   |
    | string          | String    |
    | list或arraylist | ArrayList |
    | map或hashmap    | HashMap   |

    

- 环境配置（environments）

  - 作用：设置数据库连接环境

  - 示例代码

    ```xml
    <!--    设置数据库连接环境-->
        <environments default="development">
            <environment id="development">
                <transactionManager type="JDBC"/>
                <dataSource type="POOLED">
    <!--                mysql8版本-->
    <!--                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>-->
    <!--                <property name="url" value="jdbc:mysql://localhost:3306/db220106?serverTimezone=UTC"/>-->
    <!--                mysql5版本-->
                    <property name="driver" value="${db.driver}"/>
                    <property name="url" value="${db.url}"/>
                    <property name="username" value="${db.username}"/>
                    <property name="password" value="${db.password}"/>
                </dataSource>
            </environment>
        </environments>
    ```

- mappers子标签

  - 作用：设置映射文件路径

  - 示例代码

    ```xml
    <!--    设置映射文件路径-->
        <mappers>
            <mapper resource="mapper/EmployeeMapper.xml"/>
            <!-- 要求：接口的包名与映射文件的包名需要一致-->
    <!--        <package name="com.atguigu.mybatis.mapper"/>-->
        </mappers>
    ```

- 注意：核心配置中的子标签，是有顺序要求的。

  ![image-20220826001444661](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20220826001444661.png)

### 第四章 Mybatis映射文件详解

#### 4.1 映射文件概述

- MyBatis 的真正强大在于它的语句映射，这是它的魔力所在。
- 如果拿它跟具有相同功能的 JDBC 代码进行对比，你会立即发现省掉了将近 **95%** 的代码。

#### 4.2 映射文件根标签

- mapper标签
- mapper中的namespace要求与接口的全类名一致

#### 4.3 映射文件子标签

> 子标签共有9个，注意学习其中8大子标签

- insert标签：定义添加SQL
- delete标签：定义删除SQL
- update标签：定义修改SQL
- select标签：定义查询SQL
- sql标签：定义可重用的SQL语句块
- cache标签：设置当前命名空间的缓存配置
- cache-ref标签：设置其他命名空间的缓存配置
- **resultMap标签：**描述如何从数据库结果集中加载对象
  - resultType解决不了的问题，交个resultMap。

#### 4.4 映射文件中常用属性

- resultType：设置期望结果集返回类型【全类名或别名】
  - 注意：如果返回的是集合，那应该设置为**集合包含的类型**，而不是集合本身的类型。 
  - resultType 和 resultMap 之间只能同时使用一个。

#### 4.5 获取主键自增数据

- useGeneratedKeys：启用主键生成策略

- keyProperty：设置存储属性值

  ```mysql
  <insert id="insertEmployee" useGeneratedKeys="true" keyProperty="id">
          insert into tbl_employee(last_name, email, salary)
          values ("tony", "tony@163.com", 12410);
  </insert>
  ```

#### 4.6 获取数据库受影响行数

- 直接将接口中方法的返回值设置为int或boolean即可
  - int：代表受影响行数
  - boolean
    - true：表示对数据库有影响
    - false：表示对数据库无影响

### 第五章 Mybatis中参数传递问题

#### 5.1 单个普通参数

- 可以任意使用：参数数据类型、参数名称不用考虑

#### 5.2 多个普通参数

- Mybatis底层封装Map结构，封装key为 、param2....【支持：arg0、arg1、...】

#### 5.3 命名参数

- 语法：

  - @Param(value="参数名")
  - @Param("参数名")

- 位置：参数前面

- 注意：

  - 底层封装Map结构
  - 命名参数，依然支持参数【param1,param2,...】

- 示例代码

  ```java
  /**
   * 通过员工姓名及薪资查询员工信息【命名参数】
   * @return
   */
  public List<Employee> selectEmpByNamed(@Param("lName")String lastName,
                                         @Param("salary") double salary);
  ```

  ```xml
  <select id="selectEmpByNamed" resultType="employee">
      SELECT
          id,
          last_name,
          email,
          salary
      FROM
          tbl_employee
      WHERE
          last_name=#{param1}
      AND
          salary=#{param2}
  </select>
  ```

- 源码分析

  - MapperMethod对象：142行代码【命名参数底层代码入口】

  - **命名参数底层封装map为ParamMap，ParamMap继承HashMap**

  - ParamNameResolver对象：130行代码，命名参数底层实现逻辑

    ```java
    //130行
    final Map<String, Object> param = new ParamMap<>();
    int i = 0;
    for (Map.Entry<Integer, String> entry : names.entrySet()) {
      param.put(entry.getValue(), args[entry.getKey()]);
      // add generic param names (param1, param2, ...)
      final String genericParamName = GENERIC_NAME_PREFIX + (i + 1);
      // ensure not to overwrite parameter named with @Param
      if (!names.containsValue(genericParamName)) {
        param.put(genericParamName, args[entry.getKey()]);
      }
      i++;
    }
    return param;
    ```

#### 5.4 POJO参数

- Mybatis支持POJO【JavaBean】入参，参数key是POJO中属性

#### 5.5 Map参数

- Mybatis支持直接Map入参，map的key=参数key

#### 5.6 Collection|List|Array等参数

- 参数名：collection、list、array

### 第六章 Mybatis参数传递【#与$区别】

#### 6.1 回顾JDBC

- DriverManager
- Connection
- **Statement**：执行SQL语句，入参使用SQL【String】拼接方式
- **PreparedStatement**执行SQL语句【预编译SQL】，入参使用占位符方式
- ResultSet

#### 6.2 #与$区别

- 【#】底层执行SQL语句的对象，使用**PreparedStatementd**，预编译SQL，防止SQL注入安全隐患，相对比较安全。
- 【$】底层执行SQL语句的对象使用**Statement**对象，未解决SQL注入安全隐患，相对不安全。

#### 6.3 #与$使用场景

> 查询SQL：select col,col2 from table1 where col=? and col2=?  group by ?, order by ?  limit ?,?

- #使用场景，sql占位符位置均可以使用#

- $使用场景，#解决不了的参数传递问题，均可以交给$处理【如：form 动态化表名】

  ```java
  /**
   * 测试$使用场景
   */
  public List<Employee> selectEmpByDynamitTable(@Param("tblName") String tblName);
  ```

  ```xml
  <select id="selectEmpByDynamitTable" resultType="employee">
      SELECT
          id,
          last_name,
          email,
          salary
      FROM
          ${tblName}
  </select>
  ```

### 第七章 Mybatis查询中返回值四种情况

#### 7.1 查询单行数据返回单个对象

```java
/**
 * 通过id获取员工信息
 */
public Employee selectEmpById(int empId);
```

```xml
<select id="selectEmpById" resultType="employee">
    SELECT
        id,
        last_name,
        email,
        salary
    FROM
        tbl_employee
    WHERE
        id=#{empId}
</select>
```

#### 7.2 查询多行数据返回对象的集合

```java
/**
 * 查询所有员工信息
 */
public List<Employee> selectAllEmps();
```

```xml
<select id="selectAllEmps" resultType="employee">
    SELECT
        id,
        last_name,
        email,
        salary
    FROM
        tbl_employee
</select>
```

- 注意：如果返回的是集合，那应该设置为**集合包含的类型**，而不是集合本身的类型。 

#### 7.3 查询单行数据返回Map集合

- Map<String key,Object value>

  - 字段作为Map的key，查询结果作为Map的Value

- 示例代码

  ```java
  /**
   * 查询单行数据返回Map集合
   * @return
   */
  public Map<String,Object> selectEmpReturnMap(int empId);
  ```

  ```xml
  <!--    查询单行数据返回Map集合-->
  <select id="selectEmpReturnMap" resultType="map">
      SELECT
          id,
          last_name,
          email,
          salary
      FROM
      	tbl_employee
      WHERE
      	id=#{empId}
  </select>
  ```

#### 7.4 查询多行数据返回Map集合

- Map<Integer key,Employee value>

  - 对象的id作为key
  - 对象作为value

- 示例代码

  ```java
  /**
   * 查询多行数据返回Map
   * Map<Integer,Object>
   * Map<Integer,Employee>
   *      对象Id作为：key
   *      对象作为：value
   * @return
   */
  @MapKey("id")
  public Map<Integer,Employee> selectEmpsReturnMap();
  ```

  ```xml
  <select id="selectEmpsReturnMap" resultType="map">
      SELECT
          id,
          last_name,
          email,
          salary
      FROM
          tbl_employee
  </select>
  ```

### 第八章 Mybatis中自动映射与自定义映射

> 自动映射【resultType】
>
> 自定义映射【resultMap】

#### 8.1 自动映射与自定义映射

- 自动映射【resultType】：指的是自动将表中的字段与类中的属性进行关联映射
  - 自动映射解决不了两类问题	
    - **多表连接查询时，需要返回多张表的结果集**
    - 单表查询时，不支持驼峰式自动映射【不想为字段定义别名】
- 自定义映射【resultMap】：自动映射解决不了问题，交给自定义映射
- 注意：resultType与resultMap只能同时使用一个

#### 8.2 自定义映射-级联映射

```xml
<!--    自定义映射 【员工与部门关系】-->
<resultMap id="empAndDeptRes ultMap" type="employee">
    <!--  定义主键字段与属性关联关系 -->
    <id column="id" property="id"></id>
    <!--  定义非主键字段与属性关联关系-->
    <result column="last_name" property="lastName"></result>
    <result column="email" property="email"></result>
    <result column="salary" property="salary"></result>
    <!--        为员工中所属部门，自定义关联关系-->
    <result column="dept_id" property="dept.deptId"></result>
    <result column="dept_name" property="dept.deptName"></result>
</resultMap>
<select id="selectEmpAndDeptByEmpId" resultMap="empAndDeptResultMap">
   SELECT
        e.`id`,
        e.`email`,
        e.`last_name`,
        e.`salary`,
        d.`dept_id`,
        d.`dept_name`
    FROM
        tbl_employee e,
        tbl_dept d
    WHERE
        e.`dept_id` = d.`dept_id`
    AND
        e.`id` = #{empId}
</select>
```

#### 8.3 自定义映射-association映射

- 特点：解决一对一映射关系【多对一】

- 示例代码

  - ```xml
    <!--    自定义映射 【员工与部门关系】-->
    <resultMap id="empAndDeptResultMapAssociation" type="employee">
        <!--  定义主键字段与属性关联关系 -->
        <id column="id" property="id"></id>
        <!--  定义非主键字段与属性关联关系-->
        <result column="last_name" property="lastName"></result>
        <result column="email" property="email"></result>
        <result column="salary" property="salary"></result>
        <!--  为员工中所属部门，自定义关联关系-->
        <association property="dept"
                    javaType="com.atguigu.mybatis.pojo.Dept">
            <id column="dept_id" property="deptId"></id>
            <result column="dept_name" property="deptName"></result>
        </association>
    </resultMap>
    <select id="selectEmpAndDeptByEmpIdass" resultMap="empAndDeptass">
            select e.id,
                   e.last_name,
                   e.salary,
                   e.email,
                   d.dept_id,
                   d.dept_name
            from tbl_employee e,
                 tbl_dept d
            where e.dept_id = d.dept_id
              and e.id = #{id};
        </select>
    ```

#### 8.4 自定义映射-collection映射

多表连接查询
特点：解决一对多映射关系

- 示例代码

  ```java
  /**
   * 通过部门id获取部门信息，及部门所属员工信息
   */
  public Dept selectDeptAndEmpByDeptId(int deptId);
  ```

  ```xml
  <resultMap id="deptAndempResultMap" type="dept">
      <id property="deptId" column="dept_id"></id>
      <result property="deptName" column="dept_name"></result>
      <collection property="empList"
                  ofType="com.atguigu.mybatis.pojo.Employee">
          <id column="id" property="id"></id>
          <result column="last_name" property="lastName"></result>
          <result column="email" property="email"></result>
          <result column="salary" property="salary"></result>
      </collection>
  </resultMap>
  <select id="selectDeptAndEmpByDeptId" resultMap="deptAndempResultMap">
      SELECT
          e.`id`,
          e.`email`,
          e.`last_name`,
          e.`salary`,
          d.`dept_id`,
          d.`dept_name`
      FROM
          tbl_employee e,
          tbl_dept d
      WHERE
          e.`dept_id` = d.`dept_id`
      AND
          d.dept_id = #{deptId}
  </select>
  ```

#### 8.5 ResultMap相关标签及属性

- resultMap标签：自定义映射标签
  - id属性：定义唯一标识
  - type属性：设置映射类型

- resultMap子标签
  - id标签：定义主键字段与属性关联关系
  - result标签：定义非主键字段与属性关联关系
    - column属性：定义表中字段名称
    - property属性：定义类中属性名称
  - **association标签**：定义一对一的关联关系
    - property：定义关联关系属性
    - javaType：定义关联关系属性的类型
    - select：设置分步查询SQL全路径
    - colunm：设置分步查询SQL中需要参数
    - fetchType：设置局部延迟加载【懒加载】是否开启
  - **collection标签**：定义一对多的关联关系
    - property：定义一对一关联关系属性
    - ofType：定义一对一关联关系属性类型
    - fetchType：设置局部延迟加载【懒加载】是否开启

#### 8.6 Mybatis中分步查询

- 为什么使用分步查询【分步查询优势】？

  - 将多表连接查询，改为【分步单表查询】，从而提高程序运行效率

- 示例代码

  - 一对一

    ```java
    /**
     * 通过员工id获取员工信息及员工所属的部门信息【分步查询】
            1. 先通过员工id获取员工信息【id、last_name、email、salary、dept_id】
            2. 再通过部门id获取部门信息【dept_id、dept_name】
     */
    public Employee selectEmpAndDeptByEmpIdAssociationStep(int empId);
    ```

    ```mysql
    <resultMap id="empAndDeptResultMapAssocationStep" type="employee">
            <id column="id" property="id"></id>
            <result column="last_name" property="lastName"></result>
            <result column="email" property="email"></result>
            <result column="salary" property="salary"></result>
    
            <association property="dept"
                         select="com.maven_web.mapper.DeptMapper.selectDeptByDeptId"
                         column="dept_id"
                         fetchType="eager">
            </association>
    </resultMap>
    ```

    ```xml
    <select id="selectEmpAndDeptByEmpIdAssociationStep" resultMap="empAndDeptResultMapAssocationStep">
        select
            id,
            last_name,
            email,
            salary,
            dept_id
        from
            tbl_employee
        where
            id=#{empId}
    </select>
    ```

    ```java
    /**
     * 通过部门id获取部门信息
     */
    public Dept selectDeptByDeptId(int deptId);
    ```

    ```xml
    <select id="selectDeptByDeptId" resultType="dept">
        select
            dept_id,
            dept_name
        from
            tbl_dept
        where
            dept_id=#{deptId}
    </select>
    ```

- 一对多

  ```java
  /**
   * 通过部门id获取部门信息，及部门所属员工信息【分步查询】
          1. 通过部门id获取部门信息
          2. 通过部门id获取员工信息
   */
  public Dept selectDeptAndEmpByDeptIdStep(int deptId);
  ```

  ```xml
  <!--    通过部门id获取部门信息，及部门所属员工信息【分步查询】-->
  <!--    1. 通过部门id获取部门信息-->
  <!--    2. 通过部门id获取员工信息-->
      <select id="selectDeptAndEmpByDeptIdStep" resultMap="deptAndEmpResultMapStep">
          select
              dept_id,
              dept_name
          from
              tbl_dept
          where
              dept_id=#{deptId}
      </select>
  ```

  ```java
  /**
   * 通过部门Id获取员工信息
   * @param deptId
   * @return
   */
  public List<Employee> selectEmpByDeptId(int deptId);
  ```

  ```xml
  <select id="selectEmpByDeptId" resultType="employee">
      select
          id,
          last_name,
          email,
          salary,
          dept_id
      from
          tbl_employee
      where
          dept_id=#{deptId}
  </select>
  ```

#### 8.7 Mybatis延迟加载【懒加载】

- 需要时加载，不需要暂时不加载

- 优势：提升程序运行效率

- 语法

  - 全局设置

    ```xml
    <!-- 开启延迟加载 -->
    <setting name="lazyLoadingEnabled" value="true"/>
    <!-- 设置加载的数据是按需加载3.4.2及以后的版本该步骤可省略-->
    <setting name="aggressiveLazyLoading" value="false"/>
    ```

  - 局部设置

    - fetchType

      - eager：关闭局部延迟加载
      - lazy：开启局部延迟加载

    - 示例代码

      ```xml
      <association property="dept"
                  select="com.atguigu.mybatis.mapper.DeptMapper.selectDeptByDeptId"
                  column="dept_id"
                  fetchType="eager">
      </association>
      ```

#### 8.8 扩展

- 如果分步查询时，需要传递给调用的查询中多个参数，则需要将多个参数封装成

  Map来进行传递，语法如下**: {k1=v1, k2=v2....}**

### 第九章 Mybatis动态SQL【重点】

> SQL中注释
>
> ```xml
> //方式一
> -- 1=1
> //方式二【推荐使用】
> <!-- 1=1 -->
> ```

#### 9.1 动态SQL概述

- 动态SQL指的是：SQL语句可动态化
- Mybatis的动态SQL中支持OGNL表达式语言，OGNL（ Object Graph Navigation Language ）对象图导航语言

#### 9.2 常用标签

- **if标签**：用于完成简单的判断

- **where标签**：用于解决where关键字及where后第一个and或or的问题

- **trim标签**： 可以在条件判断完的SQL语句前后添加或者去掉指定的字符
  - prefix: 添加前缀

  - prefixOverrides: 去掉前缀

  - suffix: 添加后缀

  - suffixOverrides: 去掉后缀

- **set标签**：主要用于解决set关键字及多出一个【，】问题

- **choose标签**：类似java中if-else【switch-case】结构

- **foreach标签**：类似java中for循环

  - collection: 要迭代的集合
  - item: 当前从集合中迭代出的元素
  - separator: 元素与元素之间的分隔符
  - open: 开始字符
  - close:结束字符

- **sql标签**：提取可重用SQL片段

#### 9.3 示例代码

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.atguigu.mybatis.mapper.EmployeeMapper">
    <sql id="emp_col">
        id,
        last_name,
        email,
        salary
    </sql>
    <sql id="select_employee">
        select
            id,
            last_name,
            email,
            salary
        from
            tbl_employee
    </sql>
<!-- 按条件查询员工信息【条件不确定】-->
    **if标签**：用于完成简单的判断
    **where标签**：用于解决where关键字及where后第一个and或or的问题
    <select id="selectEmpByOpr" resultType="employee">
        <include refid="select_employee"></include>
        <where>
            <if test="id != null">
               and id = #{id}
            </if>
            <if test="lastName != null">
                and last_name = #{lastName}
            </if>
            <if test="email != null">
                and email = #{email}
            </if>
            <if test="salary != null">
                and salary = #{salary}
            </if>
        </where>
    </select>

    <select id="selectEmpByOprTrim" resultType="employee">
        <include refid="select_employee"></include>
        <trim prefix="where" suffixOverrides="and">
            <if test="id != null">
                id = #{id} and
            </if>
            <if test="lastName != null">
                last_name = #{lastName} and
            </if>
            <if test="email != null">
                email = #{email} and
            </if>
            <if test="salary != null">
                salary = #{salary}
            </if>
        </trim>
    </select>
    
    <update id="updateEmpByOpr">
        update
            tbl_employee
        <set>
            <if test="lastName != null">
                last_name=#{lastName},
            </if>
            <if test="email != null">
                email=#{email},
            </if>
            <if test="salary != null">
                salary=#{salary}
            </if>
        </set>
        where
            id = #{id}
    </update>
        
    <select id="selectEmpByOneOpr" resultType="employee">
        select
            <include refid="emp_col"></include>
        from
            tbl_employee
        <where>
            <choose>
                <when test="id != null">
                    id = #{id}
                </when>
                <when test="lastName != null">
                    last_name = #{lastName}
                </when>
                <when test="email != null">
                    email = #{email}
                </when>
                <when test="salary != null">
                    salary = #{salary}
                </when>
                <otherwise>
                    1=1
                </otherwise>
            </choose>
        </where>
    </select>

    <select id="selectEmpByIds" resultType="employee">
        select
            id,
            last_name,
            email,
            salary
        from
            tbl_employee
        <where>
            id in(
            <foreach collection="ids" item="id" separator=",">
                #{id}
            </foreach>
            )
        </where>

    </select>

    <insert id="batchInsertEmp">
        INSERT INTO
            tbl_employee(last_name,email,salary)
        VALUES
            <foreach collection="employees" item="emp" separator=",">
                (#{emp.lastName},#{emp.email},#{emp.salary})
            </foreach>
    </insert>
</mapper>
```



### 第十章 Mybatis中缓存机制

#### 10.1 缓存概述

- 生活中缓存
  - 缓存一些音频、视频优势
    - 节约数据流量
    - 提高播放性能
- 程序中缓存【Mybatis缓存】
  - 使用缓存优势
    - 提高查询效率
    - 降低服务器压力  

#### 10.2 Mybatis中的缓存概述

- 一级缓存

- 二级缓存

- 第三方缓存

  ![image-20220826001518931](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220826001518931.png)

#### 10.3 Mybatis缓存机制之一级缓存

- 概述：一级缓存【本地缓存（Local Cache）或SqlSession级别缓存】

- 特点

  - 一级缓存默认开启
  - 不能关闭
  - 可以清空

- 缓存原理

  1. 第一次获取数据时，先从数据库中加载数据，将数据缓存至Mybatis一级缓存中【缓存底层实现原理Map，key：hashCode+查询的SqlId+编写的sql查询语句+参数】

  以后再次获取数据时，先从一级缓存中获取，**如未获取到数据**，再从数据库中获取数据。

- **一级缓存五种失效情况**

  1) 不同的SqlSession对应不同的一级缓存

  2) 同一个SqlSession但是查询条件不同

  3) **同一个SqlSession两次查询期间执行了任何一次增删改操作** （清空一级缓存）

  4) 同一个SqlSession两次查询期间手动清空了缓存 （sqlSession.clearCache()）

  5) 同一个SqlSession两次查询期间提交了事务 （sqlSession.commit()）


#### 10.4 Mybatis缓存机制之二级缓存

- 二级缓存【second level cache】概述

  - 二级缓存【全局作用域缓存】
  - SqlSessionFactory级别缓存

- 二级缓存特点

  - 二级缓存默认关闭，需要开启才能使用
  - 二级缓存需要提交sqlSession或关闭sqlSession时，才会缓存。

- 二级缓存使用的步骤:

  ① 全局配置文件中开启二级缓存<setting name="cacheEnabled" value="true"/>

  ② 需要使用二级缓存的**映射文件处**使用cache配置缓存<cache />

  ③ 注意：POJO需要实现Serializable接口

  ![image-20220826001531421](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20220826001531421.png)

  ④ **关闭sqlSession或提交sqlSession时，将数据缓存到二级缓存**

- 二级缓存底层原理

  - 第一次获取数据时，先从数据库中获取数据，将数据缓存至一级缓存；当提交或关闭SqlSession时，将数据缓存至二级缓存
  - 以后再次获取数据时，先从一级缓存中获取数据，如一级缓存没有指定数据，再去二级缓存中获取数据。如二级缓存也没有指定数据时，需要去数据库中获取数据，......

- 二级缓存相关属性

  - eviction=“FIFO”：缓存清除【回收】策略。
    - LRU – 最近最少使用的：移除最长时间不被使用的对象。
    - FIFO – 先进先出：按对象进入缓存的顺序来移除它们。
  - flushInterval：刷新间隔，单位毫秒
  - size：引用数目，正整数
  - readOnly：只读，true/false

- 二级缓存的失效情况

  - 在两次查询之间，执行增删改操作，会同时清空一级缓存和二级缓存
  - sqlSession.clearCache()：只是用来清除一级缓存。

#### 10.5 Mybatis中缓存机制之第三方缓存

- 第三方缓存：EhCache

- EhCache 是一个纯Java的进程内缓存框架

- 使用步骤

  - 导入jar包

    ```xml
    <!-- mybatis-ehcache -->
    <dependency>
        <groupId>org.mybatis.caches</groupId>
        <artifactId>mybatis-ehcache</artifactId>
        <version>1.0.3</version>
    </dependency>
    
    <!-- slf4j-log4j12 -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-log4j12</artifactId>
        <version>1.6.2</version>
        <scope>test</scope>
    </dependency>
    ```

  - 编写配置文件【ehcache.xml】

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
        <!-- 磁盘保存路径 -->
        <diskStore path="E:\mybatis\ehcache" />
    
        <defaultCache
                maxElementsInMemory="512"
                maxElementsOnDisk="10000000"
                eternal="false"
                overflowToDisk="true"
                timeToIdleSeconds="120"
                timeToLiveSeconds="120"
                diskExpiryThreadIntervalSeconds="120"
                memoryStoreEvictionPolicy="LRU">
        </defaultCache>
    </ehcache>
    ```

  - 加载第三方缓存【映射文件】

  - 开始使用

- 注意事项

  - 第三方缓存，需要建立在二级缓存基础上【需要开启二级缓存，第三方缓存才能生效】
  - 如何让第三方缓存失效【将二级缓存设置失效即可】



### 第十一章 Mybatis逆向工程

#### 11.1 逆向工程概述

1. 正向工程：应用程序中代码影响数据库表中数据（java对象影响表）
2. 逆向工程：数据库中标影响程序中代码（表影响java对象——POJO、XXXMapper、XXXMapper.xml)

#### 11.2 MBG简介

1. Mybatis Generator：简称MBG
2. 是一个专门为MyBatis框架使用者定制的代码生成器
3. **可以快速的根据表生成对应的映射文件，接口，以及Bean类**
4. 只可以生成单表CRUD，但是表连接、存储过程等这些复杂sql的定义需要我们手工编写



官方文档地址：https://mybatis.org/generator/quickstart.html#MyBatis3Simple

#### 11.3 MBG基本使用

1. 导包

   ```xml
   <!-- MBG的包-->
   <dependency>
       <groupId>org.mybatis.generator</groupId>
       <artifactId>mybatis-generator-core</artifactId>
       <version>1.3.6</version>
   </dependency>
   ```

2. 编写配置文件（mbg.xml）

   ```xml
   <!DOCTYPE generatorConfiguration PUBLIC
           "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
           "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
   <generatorConfiguration>
       <!--    id：设置一个唯一标识-->
       <!--    targetRuntime：属性值说明 （MyBatis3Simple基本的增删改查，MyBatis3带条件的增删改查）-->
       <context id="simple" targetRuntime="MyBatis3Simple">
           <!-- 设置连接数据库的相关信息-->
           <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                           connectionURL="jdbc:mysql://localhost:3306/mybatisdb?serverTimezone=UTC"
                           userId="root"
                           password="522166">
               <!-- 如果是mysql8需要添加下面的标签-->
               <property name="nullCatalogMeansCurrent" value="true"/>
           </jdbcConnection>
   
           <!-- 设置pojo的生成策略-->
           <javaModelGenerator targetPackage="com.mbg.pojo" targetProject="src/main/java"/>
   
           <!-- 设置sql映射文件的生成策略-->
           <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources"/>
   
           <!-- 设置Mapper接口文件的生成策略-->
           <javaClientGenerator type="XMLMAPPER" targetPackage="com.mbg.mapper" targetProject="src/main/java"/>
   
           <!-- 逆向分析的表-->
           <table tableName="tbl_employee" domainObjectName="Employee"/>
           <table tableName="tbl_dept" domainObjectName="Department"/>
       </context>
   </generatorConfiguration>
   ```

3. 运行代码生成器的代码

   ```java
    public void test01() throws XMLParserException, IOException, InvalidConfigurationException, SQLException, InterruptedException {
           ArrayList<String> warnings = new ArrayList<>();
           boolean overwrite = true;
        	//注意这个文件的路径问题
           File configFile = new File("src/main/resources/mbg.xml");
           ConfigurationParser cp = new ConfigurationParser(warnings);
           Configuration configuration = cp.parseConfiguration(configFile);
           DefaultShellCallback defaultShellCallback = new DefaultShellCallback(overwrite);
           MyBatisGenerator myBatisGenerator = new MyBatisGenerator(configuration, defaultShellCallback, warnings);
           myBatisGenerator.generate(null);
       }
   ```

   

### 第十二章 Mybatis分页插件

#### 12.1 分页基本概念（为什么使用分页）

1. 提高用户体验度
2. 降低服务器压力

#### 12.2 设计Page类

> 47/60 47：当前页码 60：总页数
>
> select # from tbl_employee where 1=1 limit x,y;  x：开启下标 y：每页显示数据数量

- pageNum：当前页码
- pages：总页数（总页数=总数据数量/每页显示数据数量）
- total：总数据数量
- pageSize：每页显示数据数量
- List<T>：当前也显示数据集合

#### 12.3 PageHelper插件使用

1. 导包

   ```xml
   <!-- 分页插件的jar-->
   <dependency>
       <groupId>com.github.pagehelper</groupId>
       <artifactId>pagehelper</artifactId>
       <version>5.0.0</version>
   </dependency>
   ```

2. 在mybatis-config.xml中配置分页插件

   ```xml
   <!-- 插件配置-->
   <plugins>
       <!-- 配置分页插件-->
       <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
   </plugins>
   ```

3. 开启分页

   ```java
   //第一个参数pageNum
   //第二个参数pageSize
   PageHelper.startPage(1, 3);
   ```

4. 获取分页后结果的参数

   ```java
   PageInfo<Employee> employeePageInfo = new PageInfo<>(employees, 5);
   
   System.out.println(employeePageInfo.getPageNum() + "/" + employeePageInfo.getPages());
   System.out.println("总数据数量" + employeePageInfo.getTotal());
   System.out.println("每页显示数据数量" + employeePageInfo.getPageSize());
   System.out.println("当前页显示数据集合");
   for (Employee emp : employeePageInfo.getList()) {
       System.out.println("emp = " + emp);
   }
   System.out.println("是否有上一页：" + employeePageInfo.isHasPreviousPage());
   System.out.println("上一页是" + employeePageInfo.getPrePage());
   System.out.println("是否有下一页" + employeePageInfo.isHasNextPage());
   System.out.println("下一页是" + employeePageInfo.getNextPage());
   System.out.println("导航页的第一个页码是" + employeePageInfo.getNavigateFirstPage());
   System.out.println("导航页的最后一个页码是" + employeePageInfo.getNavigateLastPage());
   System.out.println("导航页的总页码是" + employeePageInfo.getNavigatePages());
   ```

5. PageInfo类的属性参数

   ```java
   //当前页
   private int pageNum;
   //每页的数量
   private int pageSize;
   //当前页的数量
   private int size;
   //由于startRow和endRow不常用，这里说个具体的用法
   //可以在页面中"显示startRow到endRow 共size条数据"
   //当前页面第一个元素在数据库中的行号
   private int startRow;
   //当前页面最后一个元素在数据库中的行号
   private int endRow;
   //总记录数
   private long total;
   //总页数
   private int pages;
   //结果集(每页显示的数据)
   private List<T> list;
   //第一页
   private int firstPage;
   //前一页
   private int prePage;
   //是否为第一页
   private boolean isFirstPage = false;
   //是否为最后一页
   private boolean isLastPage = false;
   //是否有前一页
   private boolean hasPreviousPage = false;
   //是否有下一页
   private boolean hasNextPage = false;
   //导航页码数
   private int navigatePages;
   //所有导航页号
   private int[] navigatepageNums;
   ```

   

## ③Spring

### 第一章 初识Spring

#### 1.1 Spring简介

- Spring是一个为简化企业级开发而生的**开源框架**。
- Spring是一个**IOC(DI)**和**AOP**容器框架。
- IOC全称：Inversion of Control【控制反转】
  - 将对象【万物皆对象】控制权交个Spring
- DI全称：(Dependency Injection)：依赖注入
- AOP全称：Aspect-Oriented Programming，面向切面编程

- 官网：https://spring.io/

#### 1.2 搭建Spring框架步骤

- 导入jar包

  ```xml
  <!--导入spring-context-->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.3.1</version>
  </dependency>
  <!--导入junit4.12-->
  <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
  </dependency>
  ```

- 编写核心配置文件

  - 配置文件名称：**applicationContext.xml【beans.xml或spring.xml】**

  - 配置文件路径：**src/main/resources**

  - 示例代码

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
        <!-- 将对象装配到IOC容器中-->
        <bean id="stuZhenzhong" class="com.atguigu.spring.pojo.Student">
            <property name="stuId" value="101"></property>
            <property name="stuName" value="zhenzhong"></property>
        </bean>
        
    </beans>
    ```

- 使用核心类库

  ```java
  @Test
  public void testSpring(){
          //使用Spring之前
  //        Student student = new Student();
  
          //使用Spring之后
          //创建容器对象
          ApplicationContext iocObj = 
                  new ClassPathXmlApplicationContext("applicationContext.xml");
          //通过容器对象，获取需要对象
          Student stuZhenzhong = (Student)iocObj.getBean("stuZhenzhong");
          System.out.println("stuZhenzhong = " + stuZhenzhong);
  
      }
  ```

#### 1.3 Spring特性

- 非侵入式：基于Spring开发的应用中的对象可以不依赖于Spring的API。
- 容器：Spring是一个容器，因为它包含并且管理应用对象的生命周期。
- 组件化：Spring实现了使用简单的组件配置组合成一个复杂的应用。在 Spring 中可以使用XML和Java注解组合这些对象。
- 一站式：在IOC和AOP的基础上可以整合各种企业应用的开源框架和优秀的第三方类库（实际上Spring 自身也提供了表述层的SpringMVC和持久层的JDBCTemplate）。

#### 1.4 Spring中getBean()三种方式

- getBean(String beanId)：通过beanId获取对象

  - 不足：需要强制类型转换，不灵活

- getBean(Class clazz)：通过Class方式获取对象

  - 不足：容器中有多个相同类型bean的时候，会报如下错误：

    expected single matching bean but found 2: stuZhenzhong,stuZhouxu

- **getBean(String beanId,Clazz clazz)：通过beanId和Class获取对象**

  - 推荐使用

> 注意：框架默认都是通过无参构造器，帮助我们创建对象。
>
> 所以：如提供对象的构造器时，一定添加无参构造器

#### 1.5 bean标签详解

- 属性
  - id：bean的唯一标识
  - class：定义bean的类型【class全类名】
- 子标签
  - property：为对象中属性赋值【set注入】
    - name属性：设置属性名称
    - value属性：设置属性数值

### 第二章 SpringIOC底层实现

> IOC：将对象的控制器反转给Spring

#### 2.1 BeanFactory与ApplicationContexet

- BeanFactory：IOC容器的基本实现，是Spring内部的使用接口，是面向Spring本身的，不是提供给开发人员使用的。
- ApplicationContext：BeanFactory的子接口，提供了更多高级特性。面向Spring的使用者，几乎所有场合都使用ApplicationContext而不是底层的BeanFactory。

#### 2.2 图解IOC类的结构

![image-20220826001715321](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220826001715321.png)

- BeanFactory：Spring底层IOC实现【面向Spring框架】
  - ...
    - **ApplicationContext**：面向程序员
      - **ConfigurableApplicationContext：提供关闭或刷新容器对象方法**
        - ...
          - **ClassPathXmlApplicationContext：基于类路径检索xml文件**
          - **AnnotationConfigApplicationContext**：基于注解创建容器对象
          - FileSystemXmlApplicationContext：基于文件系统检索xml文件

### 第三章 Spring依赖注入数值问题【重点】

#### 3.1 字面量数值

- 数据类型：基本数据类型及包装类、String
- 语法：value属性或value标签

#### 3.2 CDATA区

- 语法：\<![CDATA[  ]]>

- 作用：在xml中定义特殊字符时，使用CDATA区（mybatis中的xml也可以使用）

  ```java
  <bean id="stuXxxtentx" class="com.spring.pojo.Student">
      <property name="stuId" value="102"></property>
      <property name="stuName">
      <value><![CDATA[<<XXXTENTX>>]]></value>
      </property>
  </bean>
  ```

  

#### 3.3 外部已声明bean及级联属性赋值

- 语法：ref

- 注意：级联属性更改数值会影响外部声明bean【ref赋值的是引用】

- 示例代码

  ```xml
  <bean id="dept1" class="com.atguigu.pojo.Dept">
      <property name="deptId" value="1"></property>
      <property name="deptName" value="研发部门"></property>
  </bean>
  
  <bean id="empChai" class="com.atguigu.pojo.Employee">
      <property name="id" value="101"></property>
      <property name="lastName" value="chai"></property>
      <property name="email" value="chai@163.com"></property>
      <property name="salary" value="50.5"></property>
      <property name="dept" ref="dept1"></property>
      <property name="dept.deptName" value="财务部门"></property>
  </bean>
  ```

#### 3.4 内部bean

- 概述

  - 内部类：在一个类中完整定义另一个类，当前类称之为内部类
  - 内部bean：在一个bean中完整定义另一个bean，当前bean称之为内部bean

- 注意：内部bean不会直接装配到IOC容器中

- 示例代码

  ```xml
  <!--    测试内部bean-->
  <bean id="empXin" class="com.atguigu.pojo.Employee">
      <property name="id" value="102"></property>
      <property name="lastName" value="xx"></property>
      <property name="email" value="xx@163.com"></property>
      <property name="salary" value="51.5"></property>
      <property name="dept">
          <bean class="com.atguigu.pojo.Dept">
              <property name="deptId" value="2"></property>
              <property name="deptName" value="人事部门"></property>
          </bean>
      </property>
  </bean>
  ```

#### 3.5 集合

- List

  ```xml
  <!--    测试集合-->
      <bean id="dept3" class="com.atguigu.pojo.Dept">
          <property name="deptId" value="3"></property>
          <property name="deptName" value="程序员鼓励师"></property>
          <property name="empList">
              <list>
                  <ref bean="empChai"></ref>
                  <ref bean="empXin"></ref>
  <!--                <bean></bean>-->
              </list>
          </property>
      </bean>
  
      <!--    测试提取List-->
      <util:list id="empList">
          <ref bean="empChai"></ref>
          <ref bean="empXin"></ref>
      </util:list>
      <bean id="dept4" class="com.atguigu.pojo.Dept">
          <property name="deptId" value="4"></property>
          <property name="deptName" value="运营部门"></property>
          <property name="empList" ref="empList"></property>
      </bean>
  ```

- Map

  ```xml
  <!--    测试Map-->
  <bean id="dept5" class="com.atguigu.pojo.Dept">
      <property name="deptId" value="5"></property>
      <property name="deptName" value="采购部门"></property>
      <property name="empMap">
          <map>
              <entry key="101" value-ref="empChai"></entry>
              <entry>
                  <key><value>103</value></key>
                  <ref bean="empChai"></ref>
              </entry>
              <entry>
                  <key><value>102</value></key>
                  <ref bean="empXin"></ref>
              </entry>
          </map>
      </property>
  </bean>
  
  <util:map id="empMap">
      <entry key="101" value-ref="empChai"></entry>
      <entry>
          <key><value>103</value></key>
          <ref bean="empChai"></ref>
      </entry>
      <entry>
          <key><value>102</value></key>
          <ref bean="empXin"></ref>
      </entry>
  </util:map>
  <bean id="dept6" class="com.atguigu.pojo.Dept">
      <property name="deptId" value="106"></property>
      <property name="deptName" value="后勤部门"></property>
      <property name="empMap" ref="empMap"></property>
  </bean>
  ```



### 第四章 Spring依赖注入方式【基于XML】

> 为属性赋值方式
>
> - 通过xxxset()方法
> - 通过构造器
> - 反射

#### 4.1 set注入

- 语法：\<property>

#### 4.2 构造器注入

- 语法：\<constructor-arg>

#### 4.3 p名称空间注入

> 导入名称空间：xmlns:p="http://www.springframework.org/schema/p"

- 语法：<bean p:xxx>

- 示例代码

  ```xml
  <bean id="stuZhouxu" class="com.atguigu.spring.pojo.Student">
      <property name="stuId" value="102"></property>
      <property name="stuName">
          <value><![CDATA[<<zhouxu>>]]></value>
      </property>
  </bean>
  
  <bean id="stuZhiFeng" class="com.atguigu.spring.pojo.Student">
      <constructor-arg name="stuId" value="103"></constructor-arg>
      <constructor-arg name="stuName" value="zhifeng"></constructor-arg>
  </bean>
  
  <bean id="stuXiaoxi"
        class="com.atguigu.spring.pojo.Student"
        p:stuId="104"
        p:stuName="xiaoxi"></bean>
  ```

### 第五章 Spring管理第三方bean

#### 5.1 Spring管理druid步骤

- 导入jar包

  ```xml
  <!--导入druid的jar包-->
  <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.10</version>
  </dependency>
  <!--导入mysql的jar包-->
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.37</version>
      <!--            <version>8.0.26</version>-->
  </dependency>
  ```

- 编写db.properties配置文件

  ```properties
  #key=value
  db.driverClassName=com.mysql.jdbc.Driver
  db.url=jdbc:mysql://localhost:3306/db220106
  db.username=root
  db.password=root
  ```

- 编写applicationContext.xml相关代码

  ```xml
  <!--    加载外部属性文件db.properties-->
  <context:property-placeholder location="classpath:db.properties"></context:property-placeholder>
  
  <!--    装配数据源-->
  <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
      <property name="driverClassName" value="${db.driverClassName}"></property>
      <property name="url" value="${db.url}"></property>
      <property name="username" value="${db.username}"></property>
      <property name="password" value="${db.password}"></property>
  </bean>
  ```

- 测试

  ```java
  @Test
  public void testDruidDataSource() throws Exception{
      //获取容器对象
      ApplicationContext ioc =
              new ClassPathXmlApplicationContext("applicationContext_druid.xml");
  
      DruidDataSource dataSource = ioc.getBean("dataSource", DruidDataSource.class);
      System.out.println("dataSource = " + dataSource);
  
      DruidPooledConnection connection = dataSource.getConnection();
      System.out.println("connection = " + connection);
  
  }
  ```



### 第六章 Spring中FactoryBean

#### 6.1 Spring中两种bean

- 一种是普通bean
- 另一种是工厂bean【FactoryBean】
  - 作用：如需我们程序员参与到bean的创建时，使用FactoryBean

#### 6.2 FactoryBean使用步骤

- 实现FactoryBean接口
- 重写方法【三个】
- 装配工厂bean
- 测试

```java
package com.atguigu.factory;

import com.atguigu.pojo.Dept;
import org.springframework.beans.factory.FactoryBean;

/**
 * @author Chunsheng Zhang 尚硅谷
 * @create 2022/3/26 14:09
 */
public class MyFactoryBean implements FactoryBean<Dept> {

    /**
     * getObject():参数对象创建的方法
     * @return
     * @throws Exception
     */
    @Override
    public Dept getObject() throws Exception {
        Dept dept = new Dept(101,"研发部门");
        //.....
        return dept;
    }

    /**
     * 设置参数对象Class
     * @return
     */
    @Override
    public Class<?> getObjectType() {
        return Dept.class;
    }

    /**
     * 设置当前对象是否为单例
     * @return
     */
    @Override
    public boolean isSingleton() {
        return true;
    }

}
```

### 第七章 Spring中bean的作用域

#### 7.1 语法

- 在bean标签中添加属性：scope属性即可

#### 7.2 四个作用域

- singleton【默认值】：单例【在容器中只有一个对象】
  - 对象创建时机：**创建容器对象时**，创建对象执行
- prototype：多例【在容器中有多个对象】
  - 对象创建时机：**getBean()方法被调用时**，创建对象执行
- request：请求域
  - 当前请求有效，离开请求域失效
  - 当前请求：**URL不变即为当前请求**
- session：会话域
  - 当前会话有效，离开当前会话失效
  - 当前会话：**当前浏览不关闭不更换即为当前会话**

### 第八章 Spring中bean的生命周期

#### 8.1 bean的生命周期

① 通过构造器或工厂方法创建bean实例

② 为bean的属性设置值和对其他bean的引用

③ 调用bean的初始化方法

④  bean可以使用了

⑤ **当容器关闭时**，调用bean的销毁方法

#### 8.2 bean的后置处理器

- 作用：在调用初始化方法前后对bean进行额外的处理。
- 实现：
  - 实现BeanPostProcessor接口
  - 重写方法
    - postProcessBeforeInitialization(Object, String)：在bean的初始化之前执行
    - postProcessAfterInitialization(Object, String)：在bean的初始化之后执行
  - 在容器中装配后置处理器

- 注意：装配后置处理器会为**当前容器中每个bean**均装配，不能为局部bean装配后置处理器

#### 8.3 添加后置处理器后bean的生命周期

① 通过构造器或工厂方法创建bean实例

② 为bean的属性设置值和对其他bean的引用

postProcessBeforeInitialization(Object, String)：在bean的初始化之前执行

③ 调用bean的初始化方法

postProcessAfterInitialization(Object, String)：在bean的初始化之后执行

④  bean可以使用了

⑤ **当容器关闭时**，调用bean的销毁方法



### 第九章 Spring中自动装配【基于XML】

#### 9.1 Spring中提供两种装配方式

- 手动装配
- 自动装配

#### 9.2 Spring自动装配语法及规则

- 在bean标签中添加属性：Autowire即可

  - byName：对象中**属性名称**与容器中的**beanId**进行匹配，如果**属性名与beanId数值一致**，则自动装配成功

  - byType：对象中**属性类型**与容器中**class**进行匹配，**如果唯一匹配则自动装配成功**

    - 匹配0个：未装配

    - 匹配多个，会报错

      **expected single matching bean but found 2: deptDao,deptDao2**

- 注意：基于XML方式的自动装配，只能装配**非字面量**数值

#### 9.3 总结

- 基于xml自动装配，底层使用**set注入**
- 最终：不建议使用byName、byType，**建议使用注解方式自动装配**

### 第十章 Spring中注解【非常重要】

#### 10.1 使用注解将对象装配到IOC容器中

> 约定：约束>配置【**注解>XML**】>代码
>
> 位置：在类上面标识
>
> 注意：
>
> - Spring本身不区分四个注解【四个注解本质是一样的@Component】，提供四个注解的目的只有一个：提高代码的可读性
> - 只用注解装配对象，默认将类名首字母小写作为beanId
> - 可以使用value属性，设置beanId；当注解中只使用一个value属性时，value关键字可省略

- 装配对象四个注解

  - @Component：装配**普通组件**到IOC容器
  - @Repository：装配**持久化层组件**到IOC容器
  - @Service：装配**业务逻辑层组件**到IOC容器
  - @Controller：装配**控制层|表示层组件**到IOC容器 

- 使用注解步骤

  - 导入相关jar包【已导入】

  - 开启组件扫描（在xml中开启）

    ```xml
    <!-- 开启组件扫描 base-package：设置扫描注解包名【当前包及其子包】-->
    <context:component-scan base-package="com.atguigu"></context:component-scan>
    ```
    
  - 使用注解标识组件

#### 10.2 使用注解装配对象中属性【自动装配】

- **@Autowired注解**

  - 作用：自动装配对象中属性

  - 装配原理：反射机制

  - 装配方式

    - **先按照byType进行匹配**

      - 匹配1个：匹配成功，正常使用

      - 匹配0个：

        - 默认【@Autowired(**required=true**)】报错

          ```java
          /*expected at least 1 bean which qualifies as autowire candidate. 	Dependency annotations: {@org.springframework.beans.factory.annotation.Autowired(required=true)}
          */
          ```

        - @Autowired(**required=false**)，不会报错

      - 匹配多个

        - **再按照byName进行唯一筛选**

          - 筛选成功【对象中属性名称==beanId】，正常使用

          - 筛选失败【对象中属性名称!=beanId】，报如下错误： 

            ```java
            //expected single matching bean but found 2: deptDao,deptDao2
            ```

  - @Autowired中required属性

    - true：表示被标识的属性**必须装配数值**，如未装配，**会报错**。
    - false：表示被标识的属性**不必须装配数值**，如未装配，**不会报错**。

- @Qualifier注解

  - 作用：配合@Autowired一起使用，**将设置beanId名称装配到属性中**
  - 注意：不能单独使用，需要与@Autowired一起使用

- @Value注解

  - 作用：装配对象中属性【字面量数值】

### 第十一章 Spring中组件扫描

#### 11.1 默认使用情况

```xml
<!-- 开启组件扫描 base-package：设置扫描注解包名【当前包及其子包】-->
<context:component-scan base-package="com.atguigu"></context:component-scan>
```

#### 11.2 包含扫描

- 注意：
  - 使用包含扫描之前，必须设置use-default-filters="false"【关闭当前包及其子包的扫描】
  - type
    - annotation：设置被扫描**注解**的全类名
    - assignable：设置被扫描**实现类**的全类名

```xml
<context:component-scan base-package="com.atguigu" use-default-filters="false">
    <context:include-filter type="annotation" 			 expression="org.springframework.stereotype.Repository"/>
    <context:include-filter type="assignable" expression="com.atguigu.service.impl.DeptServiceImpl"/>
</context:component-scan>
```

#### 11.3 排除扫描

```xml
<!--    【排除扫描】   假设：环境中共有100包，不想扫描2/100-->
    <context:component-scan base-package="com.atguigu">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
<!--        <context:exclude-filter type="assignable" expression="com.atguigu.controller.DeptController"/>-->
    </context:component-scan>
```



### 第十三章 Spring完全注解开发【0配置】

#### 13.1 完全注解开发步骤

1. 创建配置类
2. 在class上面添加注解
   - @Configuration：标识当前类是一个配置类，作用：代替XML配置文件
   - @ComponentScan：设置组件扫描当前包及其子包
3. 使用AnnotationConfigApplicationContext容器对象

#### 13.2 示例代码

```java
@Configuration
@ComponentScan(basePackages = "com.atguigu")
public class SpringConfig {
}
```

```java
  @Test
    public void test0Xml(){
        //创建容器对象
//        ApplicationContext context =
//                new ClassPathXmlApplicationContext("applicationContext.xml");
        //使用AnnotationConfigApplicationContext容器对象
        ApplicationContext context =
                new AnnotationConfigApplicationContext(SpringConfig.class);
        DeptDaoImpl deptDao = context.getBean("deptDao", DeptDaoImpl.class);

        System.out.println("deptDao = " + deptDao);
    }
```

### 第十四章 Spring集成Junit4

#### 14.1 集成步骤

1. 导入jar包
   - spring-test-5.3.1.jar
   
     ```xml
     <!-- https://mvnrepository.com/artifact/org.springframework/spring-test -->
     <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-test</artifactId>
         <version>5.3.1</version>
         <scope>test</scope>
     </dependency>
     ```
2. 指定Spring的配置文件的路径
   - 【@ContextConfiguration】
3. 指定Spring环境下运行Junit4的运行器
   - @RunWith

#### 14.2 示例代码

```java
@ContextConfiguration(locations = "classpath:applicationContext.xml")
@RunWith(SpringJUnit4ClassRunner.class)
public class TestSpringJunit4 {

    @Autowired
    private DeptService deptService;
    
    @Test
    public void testService(){
        //创建容器对象
//        ApplicationContext context =
//                new ClassPathXmlApplicationContext("applicationContext.xml");
//        DeptService deptService = context.getBean("deptService", DeptServiceImpl.class);
        deptService.saveDept(new Dept());
    }
}
```

### 第十五章 AOP前奏

#### 15.1 代理模式

- 代理模式：我们需要做一件事情，又不期望自己亲力亲为，此时，可以找一个代理【中介】

- 我们【目标对象】与中介【代理对象】不能相互转换，因为是“兄弟”关系

  ![image-20220826001752917](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220826001752917.png)

#### 15.2 为什么需要代理【程序中】

- 需求：实现【加减乘除】计算器类
  - 在加减乘除方法中，添加日志功能【在计算之前，记录日志。在计算之后，显示结果。】

- 实现后发现问题如下
  - 日志代码**比较分散**，可以提取日志类
  - 日志代码**比较混乱**，日志代码【非核心业务代码】与加减乘除方法【核心业务代码】书写一处

- 总结：在核心业务代码中，**需要添加日志功能，但不期望在核心业务代码中书写日志代码**。
  - 此时：使用代理模式解决问题【**先将日志代码横向提取到日志类中，再动态织入回到业务代码中**】

#### 15.3 手动实现动态代理环境搭建

- 实现方式
  - 基于接口实现动态代理： **JDK动态代理**
  - 基于继承实现动态代理： **Cglib**、Javassist动态代理

- 实现动态代理关键步骤
  - 一个类：**Proxy**
    - 概述：Proxy代理类的基类【类似Object】
    - 作用：newProxyInstance()：创建代理对象
  - 一个接口：InvocationHandler
    - 概述：实现【动态织入效果】关键接口
    - 作用：invoke()，执行invoke()实现动态织入效果

#### 15.4 手动实现动态代理关键步骤

> 注意：代理对象与实现类【目标对象】是“兄弟”关系，不能相互转换

- 创建类【为了实现创建代理对象工具类】
- 提供属性【目标对象：实现类】
- 提供方法【创建代理对象】
- 提供有参构造器【避免目标对象为空】

```java
package com.atguigu.beforeaop;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/**
 * @author Chunsheng Zhang 尚硅谷
 * @create 2022/3/28 16:22
 */
public class MyProxy {

    /**
     * 目标对象【目标客户】
     */
    private Object target;

    public MyProxy(Object target){
        this.target = target;
    }

    /**
     * 获取目标对象的，代理对象
     * @return
     */
    public Object getProxyObject(){
        Object proxyObj = null;

        /**
            类加载器【ClassLoader loader】,目标对象类加载器
            目标对象实现接口：Class<?>[] interfaces,目标对象实现所有接口
            InvocationHandler h
         */
        ClassLoader classLoader = target.getClass().getClassLoader();
        Class<?>[] interfaces = target.getClass().getInterfaces();
        //创建代理对象
        proxyObj = Proxy.newProxyInstance(classLoader, interfaces, new InvocationHandler() {
            //执行invoke()实现动态织入效果
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                //获取方法名【目标对象】
                String methodName = method.getName();
                //执行目标方法之前，添加日志
                MyLogging.beforeMethod(methodName,args);
                //触发目标对象目标方法
                Object rs = method.invoke(target, args);
                //执行目标方法之后，添加日志
                MyLogging.afterMethod(methodName,rs);
                return rs;
            }
        });
        return proxyObj;
    }

//    class invocationImpl implements InvocationHandler{
//    }

}
```

```java
@Test
    public void testBeforeAop(){

//        int add = calc.add(1, 2);
//        System.out.println("add = " + add);

        //目标对象
        Calc calc = new CalcImpl();
        //代理工具类
        MyProxy myProxy = new MyProxy(calc);
        //获取代理对象
        Calc calcProxy = (Calc)myProxy.getProxyObject();
        //测试
//        int add = calcProxy.add(1, 2);
        int div = calcProxy.div(2, 1);

    }
```

### 第十六章 Spring中AOP【重点】

#### 16.1 AspectJ框架【AOP框架】

- AspectJ是Java社区里最完整最流行的AOP框架。
- 在Spring2.0以上版本中，可以使用基于AspectJ注解或基于XML配置的AOP。

#### 16.2 使用AspectJ步骤

1. 添加jar包支持

   ```xml
   <!-- 添加AspectJ-->
   <!--spirng-aspects的jar包-->
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-aspects</artifactId>
       <version>5.3.1</version>
   </dependency>
   ```

2. 配置文件

   - 开启组件扫描
   - 开启AspectJ注解支持

   ```xml
   <!--    开启组件扫描-->
   <context:component-scan base-package="com.atguigu"></context:component-scan>
   <!--    开启AspectJ注解支持-->
   <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
   ```

3. 将MyLogging类上面添加注解

   - @Component：将当前类标识为一个组件
   - @Aspect：将当前类标识为**切面类**【非核心业务提取类】

4. 将MyLogging中的方法中添加**通知注解**

   - @Before

   ```java
   @Component      //将当前类标识为一个组件
   @Aspect         //将当前类标识为【切面类】【非核心业务提取类】
   public class MyLogging {
       /**
        * 方法之前
        */
       @Before(value = "execution(public int com.atguigu.aop.CalcImpl.add(int, int) )")
       public void beforeMethod(JoinPoint joinPoint){
           //获取方法名称
           String methodName = joinPoint.getSignature().getName();
           //获取参数
           Object[] args = joinPoint.getArgs();
           System.out.println("==>Calc中"+methodName+"方法(),参数："+ Arrays.toString(args));
       }
   }
   ```
   
5. 测试

   ```java
   @Test
   public void testAop(){
       //创建容器对象
       ApplicationContext context =
               new ClassPathXmlApplicationContext("applicationContext_aop.xml");
   
       Calc calc = context.getBean("calc", Calc.class);
   
        //错误的，代理对象不能转换为目标对象【代理对象与目标对象是兄弟关系】
   	 //CalcImpl calc = context.getBean("calc", CalcImpl.class);
   
       int add = calc.add(1, 2);
   }
   ```

#### 16.3 Spring中AOP概述

- AOP：Aspect-Oriented Programming，面向切面编程【面向对象一种补充】
  - 优势：
    - 解决代码**分散问题**
    - 解决代码**混乱问题**
- OOP：Object-Oriented Programming，面向对象编程

#### 16.4 Spring中AOP相关术语

1. 横切关注点：非核心业务代码【日志】，称之为横切关注点
2. **切面(Aspect)**：将横切关注点提取到类中，这个类称之为**切面类**
3. **通知(Advice)**：将横切关注点提取到类中之后，横切关注点更名为：通知
4. 目标(Target)：目标对象，指的是需要被代理的对象【实现类（CalcImpl）】
5. 代理(Proxy)：代理对象可以理解为：中介
6. 连接点(Joinpoint)：通知方法需要指定通知位置，这个位置称之为：连接点【通知之前】
7. **切入点(pointcut)**：通知方法需要指定通知位置，这个位置称之为：切入点【通知之后】

### 第十七章 AspectJ详解【重点】

#### 17.1 AspectJ中切入点表达式

- 语法：@Before(value="execution(权限修饰符   返回值类型   包名.类名.方法名(参数类型))")

- 通配符

  【*】：

  ​			【*】：可以代表任意权限修饰符&返回值类型

  ​			【*】：可以代表任意包名、任意类名、任意方法名

  【..】：

  ​			【..】：代表任意参数类型及参数个数

- 重用切入点表达式

  1. 使用@Pointcut注解，提取可重用的切入点表达式

     ```java
     @Pointcut("execution(* com.atguigu.aop.CalcImpl.*(..) )")
     public void myPointCut(){}
     ```

  2. 使用**方法名()**引入切入点表达式

     ```java
     @Before(value = "myPointCut()")
     public void beforeMethod(JoinPoint joinPoint){}
     ```

#### 17.2 AspectJ中JoinPoint对象

- JoinPoint【切入点对象】

- 作用：

  - 获取方法名称

    ```java
    //获取方法签名【方法签名=方法名+参数列表】
    joinPoint.getSignature();
    //获取方法名称
    String methodName = joinPoint.getSignature().getName();
    ```

  - 获取参数

    ```java
    Object[] args = joinPoint.getArgs();
    ```

#### 17.3 AspectJ中通知

- 前置通知

  - 语法：@Before

  - 执行时机：指定方法**执行之前**执行【如目标方法中有异常，会执行】

    - 指定方法：切入点表达式设置位置

  - 示例代码

    ```java
    //重用切入点表达式
    @Pointcut("execution(* com.atguigu.aop.CalcImpl.*(..) )")
    public void myPointCut(){}
    @Before(value = "myPointCut()")
    public void beforeMethod(JoinPoint joinPoint){
        //获取方法名称
        String methodName = joinPoint.getSignature().getName();
        //获取参数
        Object[] args = joinPoint.getArgs();
        System.out.println("==>Calc中"+methodName+"方法(),参数："+ Arrays.toString(args));
    }
    ```

- 后置通知

  - 语法：@After

  - 执行时机：指定方法所有通知**执行之后**执行【如目标方法中有异常，会执行】

  - 示例代码

    ```java
    /**
     * 后置通知
     */
    @After("myPointCut()")
    public void afterMethod(JoinPoint joinPoint){
        //获取方法名称
        String methodName = joinPoint.getSignature().getName();
        //获取参数
        Object[] args = joinPoint.getArgs();
        System.out.println("==>Calc中"+methodName+"方法,之后执行!"+Arrays.toString(args));
    }
    ```

- 返回通知

  - 语法：@AfterReturnning

  - 执行时机：指定方法**返回结果时**执行，【如目标方法中有异常，不执行】

  - 注意事项：@AfterReturnning中returning属性与入参中参数名一致

  - 示例代码

    ```java
    /**
     * 返回通知
     */
    @AfterReturning(value = "myPointCut()",returning = "rs")
    public void afterReturnning(JoinPoint joinPoint,Object rs){
        //获取方法名称
        String methodName = joinPoint.getSignature().getName();
        //获取参数
        Object[] args = joinPoint.getArgs();
    
        System.out.println("【返回通知】==>Calc中"+methodName+"方法,返回结果执行!结果："+rs);
    
    }
    ```

- 异常通知

  - 语法：@AfterThrowing

  - 执行时机：指定方法**出现异常时**执行，【如目标方法中**无异常**，不执行】

  - 注意事项：@AfterThrowing中的throwing属性值与入参参数名一致

  - 示例代码：

    ```java
    /**
     * 异常通知
     */
    @AfterThrowing(value = "myPointCut()",throwing = "ex")
    public void afterThrowing(JoinPoint joinPoint,Exception ex){
        //获取方法名称
        String methodName = joinPoint.getSignature().getName();
        //获取参数
        Object[] args = joinPoint.getArgs();
    
        System.out.println("【异常通知】==>Calc中"+methodName+"方法,出现异常时执行!异常："+ex);
    
    }
    ```

  - 总结

    - 有异常：前置通知=》异常通知=》后置通知
    - 无异常：前置通知=》返回通知=》后置通知

- 环绕通知【前四个通知整合】

  - 语法：@Around

  - 作用：整合前四个通知

  - 注意：

    - 参数中必须使用ProceedingJoinPoint
    - 环绕通知必须将返回结果，作为返回值

  - 示例代码

    ```java
    @Around(value = "myPointCut()")
    public Object aroundMethod(ProceedingJoinPoint pjp){
        //获取方法名称
        String methodName = pjp.getSignature().getName();
        //获取参数
        Object[] args = pjp.getArgs();
        //定义返回值
        Object rs = null;
        try {
            //前置通知
            System.out.println("【前置通知】==>Calc中"+methodName+"方法(),参数："+ Arrays.toString(args));
            //触发目标对象的目标方法【加减乘除方法】
            rs = pjp.proceed();
            //返回通知【有异常不执行】
            System.out.println("【返回通知】==>Calc中"+methodName+"方法,返回结果执行!结果："+rs);
        } catch (Throwable throwable) {
            throwable.printStackTrace();
            //异常通知
            System.out.println("【异常通知】==>Calc中"+methodName+"方法,出现异常时执行!异常："+throwable);
        } finally {
            //后置通知【有异常执行】
            System.out.println("【后置通知】==>Calc中"+methodName+"方法,之后执行!"+Arrays.toString(args));
        }
        return rs;
    }
    ```

#### 17.4 定义切面优先级

- 语法：@Order(value=index)

  - index是int类型，默认值是int可存储的最大值
  - 数值越小，优先级越高【一般建议使用正整数】

- 示例代码

  ```java
  @Component
  @Aspect
  @Order(value = 1)
  public class MyValidate {}
  
  @Component      //将当前类标识为一个组件
  @Aspect         //将当前类标识为【切面类】【非核心业务提取类】
  @Order(2)
  public class MyLogging {}
  ```

#### 17.5 基于XML方式配置AOP

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--配置计算器实现类-->
    <bean id="calculator" class="com.atguigu.spring.aop.xml.CalculatorImpl"></bean>

    <!--配置切面类-->
    <bean id="loggingAspect" class="com.atguigu.spring.aop.xml.LoggingAspect"></bean>

    <!--AOP配置-->
    <aop:config>
        <!--配置切入点表达式-->
        <aop:pointcut id="pointCut"
                      expression="execution(* com.atguigu.spring.aop.xml.Calculator.*(..))"/>
        <!--配置切面-->
        <aop:aspect ref="loggingAspect">
            <!--前置通知-->
            <aop:before method="beforeAdvice" pointcut-ref="pointCut"></aop:before>
            <!--返回通知-->
            <aop:after-returning method="returningAdvice" pointcut-ref="pointCut" returning="result"></aop:after-returning>
            <!--异常通知-->
            <aop:after-throwing method="throwingAdvice" pointcut-ref="pointCut" throwing="e"></aop:after-throwing>
            <!--后置通知-->
            <aop:after method="afterAdvice" pointcut-ref="pointCut"></aop:after>
            <!--环绕通知-->
            <aop:around method="aroundAdvice" pointcut-ref="pointCut"></aop:around>
        </aop:aspect>
    </aop:config>
</beans>

```



### 第十八章 Spring中JdbcTemplate

#### 18.1 JdbcTemplate简介

- Spring提供的**JdbcTemplate**是一个小型持久化层框架，简化Jdbc代码。
  - Mybatis是一个半自动化的ORM持久化层框架

#### 18.2 JdbcTemplate基本使用

- 导入jar包

  ```xml
  <!--spring-context-->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.3.1</version>
  </dependency>
  <!--spring-jdbc-->
  <!--spring-orm-->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-orm</artifactId>
      <version>5.3.1</version>
  </dependency>
  <!--导入druid的jar包-->
  <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.10</version>
  </dependency>
  <!--导入mysql的jar包-->
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.37</version>
  </dependency>
  <!--junit-->
  <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
  </dependency>
  ```

- 编写配置文件

  - db.properties：设置连接数据库属性

  - applicationContext.xml【spring配置文件】

    - 加载外部属性文件
    - 装配数据源【DataSources】
    - 装配JdbcTemplate

  - 示例代码

    ```xml
    <!--    加载外部属性文件-->
    <context:property-placeholder location="classpath:db.properties"></context:property-placeholder>
    
    <!--    - 装配数据源【DataSources】-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${db.driverClassName}"></property>
        <property name="url" value="${db.url}"></property>
        <property name="username" value="${db.username}"></property>
        <property name="password" value="${db.password}"></property>
    </bean>
    
    <!--    - 装配JdbcTemplate-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>
    ```

- 使用核心类库【JdbcTemplate】

#### 18.3 JdbcTemplate的常用API 

> JdbcTemplate默认：自动提交事务

- jdbcTemplate.**update**(String sql,Object... args)：通用的**增删改**方法
- jdbcTemplate.**batchUpdate**(String sql,List<Object[]> args)：通用**批处理增删改**方法
- jdbcTemplate.**queryForObject**(String sql,Class clazz,Object... args)：查询**单个数值**
  - String sql = "select  count(1)  from tbl_xxx";
- jdbcTemplate.**queryForObject**(String sql,RowMapper<T> rm,Object... args)：查询**单个对象**
  - String sql = "select  col1,col2...  from tbl_xxx";
- jdbcTemplate.**query**(String sql,RowMapper<T> rm,Obejct... args)：查询**多个对象**

#### 18.4 使用JdbcTemplate搭建Service&Dao层

- Service层依赖Dao层

- Dao层依赖JdbcTemplate

- 示例代码

  ```java
  @Repository
  public class DeptDaoImpl implements DeptDao {
  
      @Autowired
      @Qualifier("jdbcTemplate")
      private JdbcTemplate jdbcTemplate;
  
      @Override
      public List<Dept> selectAllDepts() {
  
          String sql = "select dept_id,dept_name from tbl_dept";
          RowMapper<Dept> rowMapper = new BeanPropertyRowMapper<>(Dept.class);
          List<Dept> list = jdbcTemplate.query(sql, rowMapper);
  
          return list;
      }
  }
  
  
  @Service("deptService")
  public class DeptServiceImpl implements DeptService {
  
      @Autowired
      @Qualifier("deptDaoImpl")
      private DeptDao deptDao;
  
      @Override
      public List<Dept> getAllDepts() {
          return deptDao.selectAllDepts();
      }
  }
  ```

### 第十九章 Spring声明式事务管理

> 回顾事务
>
> 1. 事务四大特征【ACID】
>    - 原子性
>    - 一致性
>    - 隔离性
>    - 持久性
> 2. 事务三种行为
>    - 开启事务：connection.setAutoCommit(false)
>    - 提交事务：connection.commit()
>    - 回滚事务：connection.rollback()

#### 19.1 Spring中支持事务管理

- 编程式事务管理【传统事务管理】

  1) 获取数据库连接Connection对象

  2) 取消事务的自动提交【开启事务】

  3) **执行操作**

  4) 正常完成操作时手动提交事务

  5) 执行失败时回滚事务

  6) 关闭相关资源

  - 不足：
    - 事务管理代码【非核心业务】与核心业务代码相耦合
      - 事务管理代码分散
      - 事务管理代码混乱

- **声明式事务管理【使用AOP管理事务】**

  - 先横向提取【事务管理代码】，再动态织入

#### 19.2 使用声明式事务管理

> 不用事务管理代码，发现：同一个业务中，会出现局部成功及局部失败的现象【不正常】

- 添加支持【AspectJ的jar包】

  ```xml
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aspects</artifactId>
      <version>5.3.1</version>
  </dependency>
  ```

- 编写配置文件

  - **配置事务管理器**
  - **开启事务注解支持**

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:tx="http://www.springframework.org/schema/tx"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context
         https://www.springframework.org/schema/context/spring-context.xsd
         http://www.springframework.org/schema/tx
         http://www.springframework.org/schema/tx/spring-tx.xsd">	
  <!--  配置事务管理器-->
      <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <property name="dataSource" ref="dataSource"></property>
      </bean>
  
  	<!-- 开启事务注解支持
         transaction-manager默认值：transactionManager-->
      <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
  </beans>
  ```

- 在需要事务管理的业务方法上，添加注解**@Transactional**

  ```java
  @Transactional
  public void purchase(String username, String isbn) {
      //查询book价格
      Integer price = bookShopDao.findBookPriceByIsbn(isbn);
      //修改库存
      bookShopDao.updateBookStock(isbn);
      //修改余额
      bookShopDao.updateUserAccount(username, price);
  }
  ```

- 总结：

  - 添加声明式事务管理之后，获取是代理对象，代理对象不能转换为目标对象【实现类】
  - 要使用接口来接收

#### 19.3 Spring声明式事务管理属性

> @Transactional注解属性

- **事务传播行为【Propagation】**

  - 当事务方法被另一个事务方法调用时，必须指定事务应该如何传播。

    - 如：执行事务方法method()1【事务x】之后，调用事务方法method2()【事务y】，此时需要设置method()2方法的事务传播行为。

  - Spring的7种传播行为

    | 传播属性         | 描述                                                         |
    | ---------------- | ------------------------------------------------------------ |
    | **REQUIRED**     | 如果有事务在运行，当前的方法就在这个事务内运行；否则就启动一个新的事务，并在自己的事务内运行。 |
    | **REQUIRES_NEW** | 当前的方法***\*必须\****启动新事务，并在自己的事务内运行；如果有事务正在运行，应该将它挂起。 |
    | SUPPORTS         | 如果有事务在运行，当前的方法就在这个事务内运行，否则可以不运行在事务中。 |
    | NOT_SUPPORTED    | 当前的方法不应该运行在事务中，如果有运行的事务将它挂起       |
    | MANDATORY        | 当前的方法必须运行在事务中，如果没有正在运行的事务就抛出异常。 |
    | NEVER            | 当前的方法不应该运行在事务中，如果有正在运行的事务就抛出异常。 |
    | NESTED           | 如果有事务正在运行，当前的方法就应该在这个事务的嵌套事务内运行，否则就启动一个新的事务，并在它自己的事务内运行。 |

  - 图解事务传播行为

    - **REQUIRED**
    ![image-20220828161830965](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220828161830965.png)
    
  - **REQUIRES_NEW**
       ![image-20220828161850953](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220828161850953.png)

  - 使用场景

    ```java
    /**
    	1. 去结账时判断余额是否充足，余额不足：一本书都不能卖
    */
    @Transactional(propagation=Propagation.REQUIRED)
    public void purchase(String username, String isbn) {
        //查询book价格
        Integer price = bookShopDao.findBookPriceByIsbn(isbn);
        //修改库存
        bookShopDao.updateBookStock(isbn);
        //修改余额
        bookShopDao.updateUserAccount(username, price);
    }
    
    /**
    	2. 去结账时判断余额是否充足，余额不足：最后导致余额不足的那本书，不让购买
    */
    @Transactional(propagation=Propagation.REQUIRES_NEW)
        public void purchase(String username, String isbn) {
            //查询book价格
            Integer price = bookShopDao.findBookPriceByIsbn(isbn);
            //修改库存
            bookShopDao.updateBookStock(isbn);
            //修改余额
            bookShopDao.updateUserAccount(username, price);
        }
    ```
  
- 事务隔离级别【Isolation】

  1. 隔离级别概述：一个事务与其他事务之间的隔离等级【1、2、4、8】
  2. 隔离等级
     1. 读未提交【1】：READ UNCOMMITTED
        1. 存在问题：脏读【读取到了未提交的数据】
     2. 读已提交【2】：READ COMMITTED
        1. 存在问题：可能出现不可重复读
     3. 可重复读【4】：REPEATABLE READ
        1. 存在问题：可能出现幻读
     4. 串行化【8】：SERIALIZABLE

- 事务超时【timeout】

  - 设置事务超时时间，到达指定时间后会强制回滚
  - 类型：int，单位：秒

- 事务只读【readonly】

  - 一般事务方法中只有查询操作时，才将事务设置为readonly

- 事务回滚【不回滚】



## ④SpringMVC

### 第一章 初识SpringMVC

#### 1.1 SpringMVC概述

- SpringMVC是Spring子框架

- SpringMVC是Spring 为**【展现层|表示层|表述层|控制层】**提供的基于 MVC 设计理念的优秀的 Web 框架，是目前最主流的MVC 框架。

- SpringMVC是非侵入式：可以使用注解让普通java对象，作为**请求处理器【Controller】**。

- SpringMVC是用来代替Servlet

  > Servlet作用
  >
  >  	1. 处理请求
  >  	  - 将数据共享到域中
  >  	2. 做出响应
  >  	  - 跳转页面【视图】

#### 1.2 SpringMVC处理请求原理简图

- 请求
- DispatcherServlet【前端控制器】
  - 将请求交给Controller|Handler
- Controller|Handler【请求处理器】
  - 处理请求
  - 返回数据模型
- ModelAndView
  - Model：数据模型
  - View：视图对象或视图名
- DispatcherServlet渲染视图
  - 将数据共享到域中
  - 跳转页面【视图】
- 响应

![image-20220906172814074](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220906172814074.png)

### 第二章 SpringMVC搭建框架

#### 2.1 搭建SpringMVC框架

- 创建工程【web工程】

- 导入jar包

  ```xml
  <!--spring-webmvc-->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.1</version>
  </dependency>
  
  <!-- 导入thymeleaf与spring5的整合包 -->
  <dependency>
      <groupId>org.thymeleaf</groupId>
      <artifactId>thymeleaf-spring5</artifactId>
      <version>3.0.12.RELEASE</version>
  </dependency>
  
  <!--servlet-api-->
  <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
  </dependency>
  ```

- 编写配置文件

  - **web.xml注册DispatcherServlet**
    - url配置：/
    - init-param:contextConfigLocation，设置springmvc.xml配置文件路径【管理容器对象】
    - \<load-on-startup>：设置DispatcherServlet优先级【启动服务器时，创建当前Servlet对象】
  - **springmvc.xml**
    - 开启组件扫描
    - 配置视图解析器【解析视图（设置视图前缀&后缀）】

- 编写请求处理器【Controller|Handler】

  - 使用**@Controller**注解标识请求处理器
  - 使用**@RequestMapping**注解标识处理方法【URL】

- 准备页面进行，测试

### 第三章 @RequestMapping详解

> @RequestMapping注解作用：为指定的类或方法设置相应URL

#### 3.1 @RequestMapping注解位置

- 书写在类上面
  - 作用：为当前类设置映射URL
  - 注意：不能单独使用，需要与方法上的@RequestMapping配合使用
- 书写在方法上面
  - 作用：为当前方法设置映射URL
  - 注意：可以单独使用

#### 3.2 @RequestMapping注解属性

- value属性

  - 类型：String[]
  - 作用：设置URL信息

- path属性

  - 类型：String[]
  - 作用：与value属性作用一致

- method属性

  - 类型：RequestMethod[]

    > ```java
    > public enum RequestMethod {
    > GET, HEAD, POST, PUT, PATCH, DELETE, OPTIONS, TRACE
    > }
    > ```

  - 作用：为当前URL【类或方法】设置请求方式【POST、DELETE、PUT、GET】

  - 注意：

    - 默认情况：所有请求方式均支持
    - 如请求方式不支持，会报如下错误
      -  405【Request method 'GET' not supported】

- params

  - 类型：String[]
  - 作用：为当前URL设置请求参数
  - 注意：如设置指定请求参数，但URL中未携带指定参数，会报如下错误
    - 400【Parameter conditions "lastName" not met for actual request parameters:】

- headers

  - 类型：String[]
  - 作用：为当前URL设置请求头信息
  - 注意：如设置指定请求头，但URL中未携带请求头，会报如下错误
    - 404：请求资源未找到

- 示例代码

  ```java
  @RequestMapping(value = {"/saveEmp","/insertEmp"},
                  method = RequestMethod.GET,
                  params = "lastName=lisi",
                  headers = "User-Agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/99.0.4844.84 Safari/537.36")
  public String saveEmp(){
      System.out.println("添加员工信息！！！！");
  
      return SUCCESS;
  }
  ```

> ```java
> @RequestMapping(method = RequestMethod.POST)
> public @interface PostMapping {}
> @RequestMapping(method = RequestMethod.GET)
> public @interface GetMapping {}
> @RequestMapping(method = RequestMethod.PUT)
> public @interface PutMapping {}
> @RequestMapping(method = RequestMethod.DELETE)
> public @interface DeleteMapping {}
> ```

#### 3.3 @RequestMapping支持Ant 风格的路径（了解）

- #### 常用通配符

  a) ?：匹配一个字符

  b) *：匹配任意字符

  c) **：匹配多层路径 

- 示例代码

  ```java
  @RequestMapping("/testAnt/**")
  public String testAnt(){
      System.out.println("==>testAnt!!!");
      return SUCCESS;
  }
  ```

### 第四章 @PathVariable 注解

#### 4.1 @PathVariable注解位置

> ```
> @Target(ElementType.PARAMETER)
> ```

- 书写在参数前面

#### 4.2 @PathVariable注解作用

- 获取URL中占位符参数

- 占位符语法：{}

- 示例代码

  ```html
  <a th:href="@{/EmpController/testPathVariable/1001}">测试PathVariable注解</a><br>
  ```

  ```java
  /**
   * testPathVariable
   * @return
   */
  @RequestMapping("/testPathVariable/{empId}")
  public String testPathVariable(@PathVariable("empId") Integer empId){
      System.out.println("empId = " + empId);
      return SUCCESS;
  }
  ```

#### 4.3 @PathVariable注解属性

- value属性
  - 类型：String
  - 作用：设置占位符中的参数名
- name属性
  - 类型：String 
  - 作用：与name属性的作用一致
- required属性
  - 类型：boolean
  - 作用：设置当前参数是否必须入参【默认值：true】
    - true：表示当前参数必须入参，如未入参会报如下错误
      -  Missing URI template variable 'empId' for method parameter of type Integer
    - false：表示当前参数不必须入参，如未入参，会装配null值

### 第五章 REST【RESTful】风格CRUD

#### 5.1 REST的CRUD与传统风格CRUD对比

- 传统风格CRUD
  - 功能 						URL															请求方式
  - 增                             /insertEmp			       						    POST
  - 删                             /deleteEmp?empId=1001                      GET
  - 改                             /updateEmp                                             POST
  - 查                             /selectEmp?empId=1001                       GET

- REST风格CRUD
  - 功能 						URL															请求方式
  - 增                             /emp			       						               **POST**
  - 删                             /emp/1001                                                **DELETE**
  - 改                             /emp                                                          **PUT**
  - 查                             /emp/1001                                                **GET**

#### 5.2 REST风格CRUD优势

- 提高网站排名
  - 排名方式
    - **竞价排名**
    - 技术排名
- 便于第三方平台对接

#### 5.3 实现PUT&DELETE提交方式步骤

- 注册过滤器HiddenHttpMethodFilter
- 设置表单的提交方式为POST
- 设置参数：\_method=PUT或\_method=DELETE

#### 5.4 源码解析HiddenHttpMethodFilter

```java
public static final String DEFAULT_METHOD_PARAM = "_method";

private String methodParam = DEFAULT_METHOD_PARAM;

@Override
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
      throws ServletException, IOException {

   HttpServletRequest requestToUse = request;

   if ("POST".equals(request.getMethod()) && request.getAttribute(WebUtils.ERROR_EXCEPTION_ATTRIBUTE) == null) {
      String paramValue = request.getParameter(this.methodParam);
      if (StringUtils.hasLength(paramValue)) {
         String method = paramValue.toUpperCase(Locale.ENGLISH);
         if (ALLOWED_METHODS.contains(method)) {
            requestToUse = new HttpMethodRequestWrapper(request, method);
         }
      }
   }

   filterChain.doFilter(requestToUse, response);
}
/**
	 * Simple {@link HttpServletRequest} wrapper that returns the supplied method for
	 * {@link HttpServletRequest#getMethod()}.
	 */
	private static class HttpMethodRequestWrapper extends HttpServletRequestWrapper {

		private final String method;

		public HttpMethodRequestWrapper(HttpServletRequest request, String method) {
			super(request);
			this.method = method;
		}

		@Override
		public String getMethod() {
			return this.method;
		}
	}
```

### 第六章 SpringMVC处理请求数据

> 使用Servlet处理请求数据
>
> 1. 请求参数
>    - String param = request.getParameter();
> 2. 请求头
>    - request.getHeader();
> 3. Cookie
>    - request.getCookies();

#### 6.1 处理请求参数

- 默认情况：可以将请求参数名，与入参参数名一致的参数，自动入参【自动类型转换】

- SpringMVC支持POJO入参

  - 要求：请求参数名与POJO的属性名保持一致

  - 示例代码

    ```html
    <form th:action="@{/saveEmp}" method="POST">
        id:<input type="text" name="id"><br>
        LastName:<input type="text" name="lastName"><br>
        Email:<input type="text" name="email"><br>
        Salary:<input type="text" name="salary"><br>
        <input type="submit" value="添加员工信息">
    </form>
    ```

    ```java
    /**
     * 获取请求参数POJO
     * @return
     */
    @RequestMapping(value = "/saveEmp",method = RequestMethod.POST)
    public String saveEmp(Employee employee){
        System.out.println("employee = " + employee);
        return  SUCCESS;
    }
    ```

- **@RequestParam注解**

  - 作用：如请求参数与入参参数名不一致时，可以使用@RequestParam注解设置入参参数名

  - 属性

    - value
      - 类型：String
      - 作用：设置需要入参的参数名
    - name
      - 类型：String
      - 作用：与value属性作用一致
    - required
      - 类型：Boolean
      - 作用：设置当前参数，是否必须入参
        - true【默认值】：表示当前参数必须入参，如未入参会报如下错误
          - 400【Required String parameter 'sName' is not present】
        - false：表示当前参数不必须入参，如未入参，装配null值
    - defaultValue
      - 类型：String
      - 作用：当装配数值为null时，指定当前defaultValue默认值

  - 示例代码

    ```java
    /**
     * 获取请求参数
     * @return
     */
    @RequestMapping("/requestParam1")
    public String requestParam1(@RequestParam(value = "sName",required = false,
                                            defaultValue = "zhangsan")
                                            String stuName,
                                Integer stuAge){
        System.out.println("stuName = " + stuName);
        System.out.println("stuAge = " + stuAge);
        return SUCCESS;
    }
    ```

#### 6.2 处理请求头

- 语法：**@RequestHeader注解**

- 属性

  - value
    - 类型：String
    - 作用：设置需要获取请求头名称
  - name
    - 类型：String
    - 作用：与value属性作用一致
  - required
    - 类型：boolean
    - 作用：【默认值true】
      - true：设置当前请求头是否为必须入参，如未入参会报如下错误
        - 400【Required String parameter 'sName' is not present】
      - false：表示当前参数不必须入参，如未入参，装配null值
  - defaultValue
    - 类型：String
    - 作用：当装配数值为null时，指定当前defaultValue默认值

- 示例代码

  ```html
  <a th:href="@{/testGetHeader}">测试获取请求头</a>
  ```

  ```java
  /**
   * 获取请求头
   * @return
   */
  @RequestMapping(value = "/testGetHeader")
  public String testGetHeader(@RequestHeader("Accept-Language")String al,
                              @RequestHeader("Referer") String ref){
      System.out.println("al = " + al);
      System.out.println("ref = " + ref);
      return SUCCESS;
  }
  ```

#### 6.3 处理Cookie信息

- 语法：**@CookieValue**获取Cookie数值

- 属性

  - value
    - 类型：String
    - 作用：设置需要获取Cookie名称
  - name
    - 类型：String
    - 作用：与value属性作用一致
  - required
    - 类型：boolean
    - 作用：【默认值true】
      - true：设置当前Cookie是否为必须入参，如未入参会报如下错误
        - 400【Required String parameter 'sName' is not present】
      - false：表示当前Cookie不必须入参，如未入参，装配null值
  - defaultValue
    - 类型：String
    - 作用：当装配数值为null时，指定当前defaultValue默认值

- 示例代码

  ```html
  <a th:href="@{/setCookie}">设置Cookie信息</a><br>
  <a th:href="@{/getCookie}">获取Cookie信息</a><br>
  ```

  ```java
  /**
       * 设置Cookie
       * @return
       */
      @RequestMapping("/setCookie")
      public String setCookie(HttpSession session){
  //        Cookie cookie = new Cookie();
          System.out.println("session.getId() = " + session.getId());
          return SUCCESS;
      }
  
      /**
       * 获取Cookie
       * @return
       */
      @RequestMapping("/getCookie")
      public String getCookie(@CookieValue("JSESSIONID")String cookieValue){
          System.out.println("cookieValue = " + cookieValue);
          return SUCCESS;
      }
  ```

#### 6.4 使用原生Servlet-API

- 将原生Servlet相关对象，入参即可

```java
@RequestMapping("/useRequestObject")
public String useRequestObject(HttpServletRequest request){}
```

### 第七章 SpringMVC处理响应数据

#### 7.1 使用ModelAndView

- 使用ModelAndView对象作为方法返回值类型，处理响应数据

- ModelAndView是**模型数据**与**视图对象**的集成对象，源码如下

  ```java
  public class ModelAndView {
  
     /** View instance or view name String. */
     //view代表view对象或viewName【建议使用viewName】
     @Nullable
     private Object view;
  
     /** Model Map. */
     //ModelMap集成LinkedHashMap,存储数据
     @Nullable
     private ModelMap model;
      
      /**
      	设置视图名称
  	 */
  	public void setViewName(@Nullable String viewName) {
  		this.view = viewName;
  	}
  
  	/**
  	 * 获取视图名称
  	 */
  	@Nullable
  	public String getViewName() {
  		return (this.view instanceof String ? (String) this.view : null);
  	}
  
      /**
  	 获取数据，返回Map【无序，model可以为null】
  	 */
  	@Nullable
  	protected Map<String, Object> getModelInternal() {
  		return this.model;
  	}
  
  	/**
  	 * 获取数据，返回 ModelMap【有序】
  	 */
  	public ModelMap getModelMap() {
  		if (this.model == null) {
  			this.model = new ModelMap();
  		}
  		return this.model;
  	}
  
  	/**
  	 * 获取数据，返回Map【无序】
  	 */
  	public Map<String, Object> getModel() {
  		return getModelMap();
  	}
      
      /**
      	设置数据
      */
      public ModelAndView addObject(String attributeName, @Nullable Object attributeValue) {
  		getModelMap().addAttribute(attributeName, attributeValue);
  		return this;
  	}
      
      
  }
       
  ```

- 示例代码

  ```java
  @GetMapping("/testMvResponsedata")
  public ModelAndView testMvResponsedata(){
      ModelAndView mv = new ModelAndView();
      //设置逻辑视图名
      mv.setViewName("response_success");
      //设置数据【将数据共享到域中（request\session\servletContext）】
      mv.addObject("stuName","zhouxu");
      return mv;
  }
  ```

#### 7.2 使用Model、ModelMap、Map

- 使用Model、ModelMap、Map作为方法入参，处理响应数据

- 示例代码

  ```java
  /**
       * 使用Map、Model、ModelMap处理响应数据
       * @return
       */
      @GetMapping("/testMapResponsedata")
      public String testMapResponsedata(Map<String,Object> map
                                           /* Model model
                                          ModelMap modelMap*/){
          map.put("stuName","zhangsan");
  //        model.addAttribute("stuName","lisi");
  //        modelMap.addAttribute("stuName","wangwu");
  
          return "response_success";
      }
  ```

#### 7.3 SpringMVC中域对象

- SpringMVC封装数据，（Model）默认使用request域对象

- session域的使用

  - 方式一

    ```java
    /**
     * 测试响应数据【其他域对象】
     * @return
     */
    @GetMapping("/testScopeResponsedata")
    public String testScopeResponsedata(HttpSession session){
        session.setAttribute("stuName","xinlai");
        return "response_success";
    }
    ```

  - 方式二

    ```java
    @Controller
    @SessionAttributes(value = "stuName") //将request域中数据，同步到session域中
    public class TestResponseData {
        /**
         * 使用ModelAndView处理响应数据
         * @return
         */
        @GetMapping("/testMvResponsedata")
        public ModelAndView testMvResponsedata(){
            ModelAndView mv = new ModelAndView();
            //设置逻辑视图名
            mv.setViewName("response_success");
            //设置数据【将数据共享到域中（request\session\servletContext）】
            mv.addObject("stuName","zhouxu");
            return mv;
        }
    }
    ```

### 第八章 SpringMVC处理请求响应乱码

#### 8.1 源码解析CharacterEncodingFilter

```java
public class CharacterEncodingFilter extends OncePerRequestFilter {

   //需要设置字符集
   @Nullable
   private String encoding;
   //true:处理请乱码
   private boolean forceRequestEncoding = false;
   //true:处理响应乱码
   private boolean forceResponseEncoding = false;
    
    public String getEncoding() {
		return this.encoding;
	}
    
    public boolean isForceRequestEncoding() {
		return this.forceRequestEncoding;
	}
    
    public void setForceResponseEncoding(boolean forceResponseEncoding) {
		this.forceResponseEncoding = forceResponseEncoding;
	}
    
    public void setForceEncoding(boolean forceEncoding) {
		this.forceRequestEncoding = forceEncoding;
		this.forceResponseEncoding = forceEncoding;
	}
    
 	@Override
	protected void doFilterInternal(
			HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
			throws ServletException, IOException {

		String encoding = getEncoding();
		if (encoding != null) {
			if (isForceRequestEncoding() || request.getCharacterEncoding() == null) {
				request.setCharacterEncoding(encoding);
			}
			if (isForceResponseEncoding()) {
				response.setCharacterEncoding(encoding);
			}
		}
        
		filterChain.doFilter(request, response);
	
    }
    
    
}
```

#### 8.2 处理请求与响应乱码

- SpringMVC底层默认处理响应乱码

- SpringMVC处理请求乱码步骤

  1. 注册CharacterEncodingFilter
     - 注册CharacterEncodingFilter必须是第一Filter位置
  2. 为CharacterEncodingFilter中属性encoding赋值
  3. 为CharacterEncodingFilter中属性forceRequestEncoding赋值

- 示例代码

  ```xml
  <!--    必须是第一过滤器位置-->
  <filter>
      <filter-name>CharacterEncodingFilter</filter-name>
      <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
      <init-param>
          <param-name>encoding</param-name>
          <param-value>UTF-8</param-value>
      </init-param>
      <init-param>
          <param-name>forceRequestEncoding</param-name>
          <param-value>true</param-value>
      </init-param>
  </filter>
  <filter-mapping>
      <filter-name>CharacterEncodingFilter</filter-name>
      <url-pattern>/*</url-pattern>
  </filter-mapping>
  ```



### 第九章 源码解析SpringMVC工作原理

#### 9.1 Controller中方法的返回值问题

- 无论方法返回是ModelAndView还是String，最终SpringMVC底层，均会封装为ModelAndView对象

  ```java
  //DispatcherServlet的1061行代码
  ModelAndView mv = null;
  mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
  ```

- SpringMVC解析mv【ModelAndView】

  ```java
  //DispatcherServlet的1078行代码
  processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
  ```

- ThymeleafView对象中344行代码【SpringMVC底层处理响应乱码】

  ```java
  //computedContentType="text/html;charset=UTF-8"
  response.setContentType(computedContentType);
  ```

- WebEngineContext对象中783行代码【SpringMVC底层将数据默认共享到request域】

  ```java
  this.request.setAttribute(name, value);
  ```

#### 9.2 视图及视图解析器源码

- 视图解析器将View从ModelAndView中解析出来

  - ThymeleafViewResolver的837行代码

    ```java
    //底层使用反射的方式，newInstance()创建视图对象
    final AbstractThymeleafView viewInstance = BeanUtils.instantiateClass(getViewClass());
    ```

### 第十章 SpringMVC视图及视图解析器

#### 10.1 视图解析器对象【ViewResolver】

- 概述：ViewResolver接口的实现类或子接口，称之为视图解析器

- 作用：将ModelAndView中的View对象解析出来

  ![image-20220402111105304](F:\SSM\笔记\04_SpringMVC-day11笔记\04_SpringMVC.assets\image-20220402111105304.png)

#### 10.2 视图对象【View】

- 概述：View接口的实现类或子接口，称之为视图对象
- 作用：视图渲染
  1. 将数据共享域中
  2. 跳转路径【转发或重定向】

### 第十一章 视图控制器&重定向&加载静态资源

#### 11.1 视图控制器

- 语法：view-controller
- 步骤
  1. 添加\<mvc:view-controller>标签：为指定URL映射html页面
  2. 添加\<mvc:annotation-driven>
     - 有20+种功能
     - 配置了\<mvc:view-controller>标签之后会导致其他请求路径都失效，添加\<mvc:annotation-driven>解决

#### 11.2 重定向

- 语法：return "**redirect:/**xxx.html";

#### 11.3 加载静态资源

- 由**DefaultServlet**加载静态资源到服务器

  - 静态资源：html、css、js等资源
  - tomcat->conf->web.xml关键代码如下：

  ```xml
  <servlet>
          <servlet-name>default</servlet-name>
          <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
          <init-param>
              <param-name>debug</param-name>
              <param-value>0</param-value>
          </init-param>
          <init-param>
              <param-name>listings</param-name>
              <param-value>false</param-value>
          </init-param>
          <load-on-startup>1</load-on-startup>
      </servlet>
  <servlet-mapping>
          <servlet-name>default</servlet-name>
          <url-pattern>/</url-pattern>
      </servlet-mapping>
  ```

- 发现问题

  - DispatcherServlet与DefaultServlet的URL配置均为：/，导致DispatcherServlet中的配置将DefaultServlet配置的/覆盖了【**DefaultServlet失效，无法加载静态资源**】

- 解决方案

  ```xml
  <!--    解决静态资源加载问题-->
  <mvc:default-servlet-handler></mvc:default-servlet-handler>
  <!-- 添加上述标签，会导致Controller无法正常使用，需要添加mvc:annotation-driven解决 -->
  <mvc:annotation-driven></mvc:annotation-driven>
  ```

#### 11.4 源码解析重定向原理

- 创建RedirectView对象【ThymeleafViewResolver的775行代码】

  ```java
  // Process redirects (HTTP redirects)
  if (viewName.startsWith(REDIRECT_URL_PREFIX)) {
      vrlogger.trace("[THYMELEAF] View \"{}\" is a redirect, and will not be handled directly by ThymeleafViewResolver.", viewName);
      final String redirectUrl = viewName.substring(REDIRECT_URL_PREFIX.length(), viewName.length());
      final RedirectView view = new RedirectView(redirectUrl, isRedirectContextRelative(), isRedirectHttp10Compatible());
      return (View) getApplicationContext().getAutowireCapableBeanFactory().initializeBean(view, REDIRECT_URL_PREFIX);
  }
  ```

- RedirectView视图渲染

  - RedirectView对象URL处理【330行代码】

    ![image-20220402144319392](F:\SSM\笔记\04_SpringMVC-day11笔记\04_SpringMVC.assets\image-20220402144319392.png)

  - 执行重定向【RedirectView的627行代码】

    ![image-20220402144419221](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220402144419221.png)



### 第十二章 REST风格CRUD练习

#### 12.1 搭建环境

- 导入相关jar包

  ```xml
  <!--spring-webmvc-->
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.1</version>
  </dependency>
  
  <!-- 导入thymeleaf与spring5的整合包 -->
  <dependency>
      <groupId>org.thymeleaf</groupId>
      <artifactId>thymeleaf-spring5</artifactId>
      <version>3.0.12.RELEASE</version>
  </dependency>
  
  <!--servlet-api-->
  <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>4.0.1</version>
      <scope>provided</scope>
  </dependency>
  ```

- 编写配置文件

  - web.xml
    - CharacterEncodingFilter
    - HiddenHttpMethodFilter
    - DispatcherServlet
  - springmvc.xml
    - 开启组件扫描
    - 装配视图解析器
    - 装配视图控制器
    - 解决静态资源加载问题
    - 装配annotation-driver

- dao&pojo

#### 12.2 实现功能思路

- 实现添加功能思路

  1. 跳转添加页面【查询所有部门信息】
  2. 实现添加功能

- 实现删除功能思路

  1. 方式一：直接使用表单实现DELETE提交方式

  2. 方式二：使用超链接【a】实现DELETE提交方式

     - 使用Vue实现单击超链接，后提交对应表单

     - 取消超链接默认行为

     - 示例代码

       ```html
       <div align="center" id="app">
           <a href="#" @click="deleteEmp">删除</a>
           <form id="delForm" th:action="@{/emps/}+${emp.id}" method="post">
               <input type="hidden" name="_method" value="DELETE">
           </form>
       </div>
       <script type="text/javascript" src="static/js/vue_v2.6.14.js"></script>
       <script type="text/javascript">
           new Vue({
               el:"#app",
               data:{},
               methods:{
                   deleteEmp(){
                       alert("hehe");
                       //获取响应表单
                       var formEle = document.getElementById("delForm");
                       formEle.submit();
                       //取消超链接默认行为
                       event.preventDefault();
                   }
               }
           });
       </script>
       ```

### 第十三章 SpringMVC消息转换器

#### 13.1 消息转换器概述

- HttpMessageConverter<T>（消息转换器）的主要作用：
  - 将java对象与请求报文及响应报文的相互转换
    ![image-20220901120023123](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220901120023123.png)
  - 使用HttpMessageConverter<T>将请求信息转化并绑定到处理方法的入参中或将相应结果转为对应类型的相应信息，Spring提供了两种途径：
    - 使用@RequestBody / @ResponseBody对处理方法进行标注
    - 使用HttpEntity<T> / ResponseEntity<T> 作为处理方法的入参或返回值

#### 13.2 使用消息转换器处理请求报文

- 使用@RequestBody 获取请求体

  - 语法

    ```java
    //@RequestBody String reqBody的参数reqBody即可获取到请求体
    @RequestMapping(value = "/getRequsetBody" ,method = RequestMethod.POST)
    public String testRequestBody(@RequestBody String reqBody) {
        System.out.println("reqBody = " + reqBody);
        return SUCCESS;
    }
    
    //执行结果
    //reqBody = stuName=wxd&stuNumber=0204414
    ```

  - 注意：使用@RequestBody 必须以POST方式提交，不能使用GET方式【GET提交方式，没有请求体】

- 使用HttpEntity<T>对象，获取请求体即请求头

  - 语法

    ```java
    @RequestMapping("/getHttpEntity")
    public String testgetHttpEntity(HttpEntity<String> httpEntity) {
        //获取请求头
        HttpHeaders headers = httpEntity.getHeaders();
        System.out.println("headers = " + headers);
    
        //获取请求体
        String body = httpEntity.getBody();
        System.out.println("body = " + body);
        return SUCCESS;
    }
    ```

    ```javascript
    //运行控制台输出结果
    headers = [host:"localhost:8080", connection:"keep-alive", content-length:"29", cache-control:"max-age=0", sec-ch-ua:""Chromium";v="104", " Not A;Brand";v="99", "Google Chrome";v="104"", sec-ch-ua-mobile:"?0", sec-ch-ua-platform:""Windows""]
    body = stuName=wxd&stuNumber=0204414
    ```

  - 可以获取请求体及请求头

#### 13.3 使用消息转换器处理响应报文

- @ResponseBody

  - 使用位置

    - 书写在class类上面【当前类所有方法，均返回文本，不调整页面】
    - 书写在方法上 面【一般使用这一种方法】

  - 语法

    ```java
    @RequestMapping("/testResponseBody")
    @ResponseBody
    public String testResponseBody() {
        System.out.println("testResponseBody");
        return "hello";
    }
    ```

  - 作用：将指定数据，直接以响应流的方式，响应数据

    - 类似response.getWrtier().write();

#### 13.4 使用消息转换器处理Json格式数据

- 使用步骤：

1. 导包：添加jackson支持

   ```xml
   <!-- jackson-databind-->
   <dependency>
       <groupId>com.fasterxml.jackson.core</groupId>
       <artifactId>jackson-databind</artifactId>
       <version>2.12.3</version>
   </dependency>
   ```

2. 装配MappingJackSon2HttpMessageConverter消息转换器：添加jackson的jar包之后多装配了下面的转换器
   ![image-20220901131120489](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220901131120489.png)

   ​	注意：必须配置 `<mvc:annotation-driven/>` 标签才能装配jackson的转换器

3. 在需要转换json数据的方法上，添加@ResponseBody
   （需要被转换的数据作为方法的返回值）

4. 代码：

   ```java
   @RequestMapping("/testJson")
   @ResponseBody
   public Student testJson() {
       System.out.println("==>处理json");
       //        将Student对象转换为jsnon格式响应
       Student wxd = new Student("wxd", 21, "0204414");
   
       //之前的javaweb返回json的方法
       //        Gson gson = new Gson();
       //        String jsonStr = gons.toJson(wxd);
       //        response.getWriter.write(jsonStr);
       return wxd;
   }
   ```

- 底层实现原理【MappingJackson2HttpMessageConverter】 
  - 添加jar包
  - 装配mvc:annotation-driven
  - 添加支持之前
    ![image-20220901132727769](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220901132727769.png)
  - 添加支持之后
    ![image-20220901132743538](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220901132743538.png)



### 第十四章 SpringMVC的文件上传和下载

> 文件上传的底层思路
>
> ![image-20220901202403583](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220901202403583.png)

#### 14.1 文件下载

1. 实现文件下载步骤：
   1. 准备文件下载相关资源
   2. 将ResponseEntity<T>对象，作为方法返回值
   3. 为ResponseEntity<T>对象，设置三个参数

![image-20220901191410341](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220901191410341.png)

```html
<div align="center">
    <h1>文件下载页面</h1>
    <a th:href="@{/fileDownLoadController(filename='0.jpg')}">0.jpg</a><br>
    <a th:href="@{/fileDownLoadController(filename='01_Maven.md')}">01_Maven.md</a><br>
    <a th:href="@{/fileDownLoadController(filename='day03_03Mybatis获取数据库受影响行数.mp4')}">day03_03Mybatis获取数据库受影响行数.mp4</a><br>
</div>
```

```java
@RequestMapping("/fileDownLoadController")
public ResponseEntity<byte[]> fileDownLoadController(HttpServletRequest httpServletRequest, String filename) {
    ResponseEntity<byte[]> responseEntity = null;
    try {
        //1. 文件下载byte[]
        //获取文件真实路径[requset|session.servletContext]
        String realPath = httpServletRequest.getServletContext().getRealPath("/WEB-INF/static/" + filename);
        //创建输入流
        InputStream fileInputStream = new FileInputStream(realPath);
        byte[] bytes = new byte[fileInputStream.available()];
        fileInputStream.read(bytes);
        //2. 设置响应头
        HttpHeaders httpHeaders = new HttpHeaders();
        //设置下载文件的名字【及文件格式为附件格式，通知服务器下载当前资源，而不是打开】
        httpHeaders.add("Content-Disposition", "attachment;filename=" + filename);
        //处理中文文件名问题
        httpHeaders.setContentDispositionFormData("attachment", new String(filename.getBytes("utf-8"), "ISO-8859-1"));
        //3. 状态码HttpStatus.OK

        responseEntity = new ResponseEntity<>(bytes, httpHeaders, HttpStatus.OK);
        fileInputStream.close();
    } catch (IOException e) {
        e.printStackTrace();
    }
    return responseEntity;
}
```

#### 14.2 文件上传

1. 实现文件上传思路

   ①准备工作：

   1. 准备文件上传页面

      1. 表单的提交方式必须为POST

      2. 设置表单enctype属性值为`multipart/form-data`

      3. 表单中包含文件域【type=file】

         ```html
         <div align="center">
             <h1>上传文件页面</h1>
             <form th:action="@{/fileUpLoadController}" method="post" enctype="multipart/form-data">
                 上传姓名:<input type="text" name="username"><br>
                 上传文件：<input type="file" name="uploadFile"><br>
                 <input type="submit" value="文件上传">
             </form>
         </div>
         ```

   2. 导入jar包

      ```xml
      <dependency>
          <groupId>commons-fileupload</groupId>
          <artifactId>commons-fileupload</artifactId>
          <version>1.4</version>
      </dependency>
      ```

   3. 装配解析器：CommonsMultipartResolver

      1. id必须是：multipartResolver
      2. 设置字符集：`<property name="defaultEncoding" value="utf-8"></property>`

      在springmvc.xml中配置以下xml

      ```xml
      <!--    装配CommonsMultipartResolver-->
      <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
          <!--        设置字符集-->
          <property name="defaultEncoding" value="utf-8"></property>
          <!--        设置总文件的大小-->
          <property name="maxUploadSize" value="102400"></property>
      </bean>
      ```

   ②实现步骤：

   1. 将type=file[文件域]直接入参，使用MultipartFile类型接收
   2. 获取文件名称
   3. 获取上传文件真实路径
   4. 实现文件上传即可

​	③实例代码：

```java
@RequestMapping("/fileUpLoadController")
public String fileUpload(String usernam, MultipartFile uploadFile, HttpSession session) {
    try {
        //获取文件名称
        String filename = uploadFile.getOriginalFilename();
        //获取上传文件真实路径
        String realPath = session.getServletContext().getRealPath("/WEB-INF/static/");
        //判断上传路径是否存在【如果不存在就创建】
        File file = new File(realPath);
        if (!file.exists()) {
            file.mkdirs();
        }
        //实现文件上传
        File file1 = new File(realPath + File.separator + filename);
        uploadFile.transferTo(file1);
    } catch (IOException e) {
        e.printStackTrace();
    }

    return "success";
}
```

④实现优化：

1.  运行同名文件上传

   1. 使用UUID解决文件重复问题：UUID是一个32位16进制的随机数【特点：全球唯一】

      ```java
       //创建UUID
      String uuid = UUID.randomUUID().toString().replace("-", "");
      //实现文件上传
      File file1 = new File(realPath + File.separator + uuid + filename);
      ```

   2. 使用时间戳解决文件重复问题：`System.currentTimeMillis()`

2. 设置上传文件大小上限：在springmvc.xml限制文件大小的property

   ```xml
   <!--    装配CommonsMultipartResolver-->
   <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
       <!--        设置字符集-->
       <property name="defaultEncoding" value="utf-8"></property>
       <!--        设置总文件的大小，单位是k-->
       <property name="maxUploadSize" value="102400"></property>
   </bean>
   ```

   

### 第十五章 SpringMVC中拦截器

#### 15.1 拦截器与过滤器的区别

- 过滤器【filter】属于web服务器组件
  - 过滤器注意作用：过滤servlet的请求
  - 执行时机：两处执行时机【serlvet前、servlet后】
- 拦截器【Interceptor】属于框架【SpringMVC】
  - 拦截器的主要作用：拦截Controller的请求
  - 执行时机：三处执行时机
    ①执行DispatcherServlet之后，Controller之前
    ②执行Controller之后，DispatcherServlet之前
    ③执行DispatcherServlet之后【渲染视图之后】
- 图解
  ![image-20220901232111759](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220901232111759.png)

#### 15.2 拦截器概述

- SpringMVC可以使用拦截器实现拦截Controller请求，用户可以自定义拦截器来实现特定功能
- 实现拦截器的两种方式：
  - 实现接口：HandlerInterceptor（推荐使用）
  - 继承适配器类：HandlerInterceptorAdapter（已经淘汰）
- 拦截器中的三个方法
  - **preHandle()**：这个方法在业务处理器处理请求之前被调用，可以在此方法中做一些权限的校验。如果程序员决定该拦截器对请求进行拦截处理后还要调用其他的拦截器，或者是业务处理器去进行处理，则返回true；如果程序员决定不需要再调用其他的组件去处理请求，则返回false。
  - **postHandle()**：这个方法在业务处理请求之后，渲染视图之前调用。在此方法中可以对ModelAndView中的模型和视图进行处理。
  - **afterCompletion()**：这个方法在DispatcherServlet完全处理完请求后被调用，可以在该方法中进行一些资源清理的操作。

#### 15.3 实现拦截器步骤

1. 实现接口：HandlerInterceptor

2. 重写三个方法

   ```java
   public class myInterceptor1 implements HandlerInterceptor {
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
   
           return true;
       }
   
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {
       }
   
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {
       }
   }
   ```

3. 在springmvc.xml配置文件中，装配拦截器

   ```xml
   <!--    装配拦截器-->
   <mvc:interceptors>
       <!-- 全局配置-->
       <bean class="com.interceptor.myInterceptor1"></bean>
       <ref bean="myInterceptor1"></ref>
   
       <!-- 局部配置-->
       <mvc:interceptor>
           <!-- 要拦截的Controller的url-->
           <mvc:mapping path="/testInterceptor"/>
           <ref bean="myInterceptor1"/>
       </mvc:interceptor>
   </mvc:interceptors>
   ```

#### 15.4 拦截器的工作原理

- **单个拦截器的原理**
  - 浏览器向服务器发送请求
  - 执行拦截器第一个方法preHandle()
  - 执行Controller中方法，处理请求做出响应
  - 执行拦截器第二个方法postHandle()
  - 执行DispatcherServlet中渲染视图
  - 执行拦截器第三个方法afterCompletion()
  - 响应
- **多个拦截器的原理**
  - 浏览器向服务器发送请求
  - 执行<font color="skyblue">拦截器1</font>第一个方法<font color="skyblue">preHandle()
    </font>
  - 执行<font color="skyblue">拦截器2
    </font>第一个方法<font color="skyblue">preHandle()
    </font>
  - 执行Controller中方法，处理请求做出响应
  - 执行<font color="skyblue">拦截器2</font>第二个方法<font color="skyblue">postHandle()</font>
  - 执行<font color="skyblue">拦截器1</font>第二个方法<font color="skyblue">postHandle()</font>
  - 执行DispatcherServlet中渲染视图
  - 执行<font color="skyblue">拦截器2</font>第三个方法<font color="skyblue">afterCompletion()</font>
  - 执行<font color="skyblue">拦截器1</font>第三个方法<font color="skyblue">afterCompletion()</font>
  - 响应

> 拦截器的顺序由配置的顺序决定

#### 15.5 拦截器的preHandle()方法返回值

- 当第一个拦截器preHandle() 方法返回false时， 执行当前方法后，程序终止
- 当不是第一个拦截器preHandle()方法返回false时
  - 执行当前拦截器及之前拦截器的preHandle() 方法
  - 执行之前拦截器的afterCompletion() 方法



### 第十六章 SpringMVC异常处理器

#### 16.1 为什么需要处理异常

- 如程序中出现异常未处理，会导致程序运行终止
- JavaSE阶段异常处理机制
  - try-catch-finally
  - throw或throws

#### 16.2 SpringMVC中异常处理器

- SpringMVC通过HandlerExceptionResolver处理程序的异常，包含Handler映射、数据绑定以及目标方法执行时发送的异常

- 需要掌握两个异常处理器实现类

  - DefaultHandleExceptionResolver：默认异常处理器，默认开启，可以支持10多中异常处理

  - SimpleMappingExceptionResolver：

    - 映射自定义异常处理器，作用：将指定的异常映射到指定页面

    - 在 springmvc.xml 中装配异常处理器【SimpleMappingExceptionResolver】

      ```xml
      <!--    装配异常处理器-->
      <bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
          <property name="exceptionMappings">
              <props>
                  <!-- 设置配置的异常类型，和转的页面的逻辑视图名 -->
                  <prop key="java.lang.ArithmeticException">error/error_arith</prop>
                  <prop key="java.lang.NullPointerException">error/error_null</prop>
              </props>
          </property>
          <!--        设置响应的异常的参数名称-->
          <property name="exceptionAttribute" value="ex"/>
      </bean>
      ```

    - 显示异常：

      ```html
      异常：<span th:text="${ex}"></span>
      ```

      

> 注意：
>
> ①如果出现异常，不会执行postHandle()
>
> ![image-20220904193647892](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220904193647892.png)
>
> ②出现异常，也会返回ModelAndView



### 第十七章 SpringMVC工作原理

#### 17.1 扩展三个对象

- HandlerMapping
  - 概述：请求处理器映射器对象	
  - 作用：通过HandlerMapping可以获取HandlerExecutionChain对象
- HandlerExecutionChain
  - 概述：请求处理器执行链对象
  - 作用：通过HandlerExecutionChain对象可以获取HandlerAdapter对象
- HandlerAdapter
  - 概述：请求处理器适配器对象
  - 作用：通过HandlerAdapter的ha.handle() 调用请求处理器中相应方法

#### 17.2 SpringMVC工作原理【URL不存在】

1. 请求【浏览器向服务器发送请求，携带URL(/test)】
2. 通过DispatcherServlet加载SpringMVC容器对象，从而加载Controller【请求处理器】
   - 判断是否配置 `<mvc:default-servlet-handler>`
     - 配置：出现404现象，同时提示URL不可用
     - 未配置：出现404显现，但不会提示

#### 17.3 SpringMVC工作原理【URL存在】

1. 请求【浏览器向服务器发送请求，携带URL(/test)】

2. 通过DispatcherServlet加载SpringMVC容器对象，从而加载Controller【请求处理器】

   - 加载三个对象【HandlerMapping、HandlerExecutionChain、HandlerAdapter】

3. URL存在

4. 执行Interceptor【拦截器】第一个方法【preHandle()]

   ```java
   if(ImappedHandler.applyPreHandle(precessedRequset, response)){
   	return;
   }
   ```

5. 执行Controller【请求处理器】中相应方法【处理请求，做出响应】

   ```java
   mv = ha.handle(processedRequset, response, mappedHandler.getHandler());
   ```

6. 判断Controller中是否存在异常

   - 存在异常：通过<font color="skyblue">HandlerExceptionResolver</font>异常处理器处理异常，并返回<font color="skyblue">ModelAndView</font>
   - 不存在异常：Controller返回ModelAndView，触发拦截器第二个方法【postHandle()]

7. 通过ViewResolver【视图解析对象】将View【视图对象】从ModelAndView中解析出来

8. View对象开始渲染视图：

   - 将数据共享
   - 路径跳转

9. 执行拦截器第三个方法【afterCompletion()]

10. 响应



### 第十八章 SSM【Spring + SpringMVC + Mybatis】整合

#### 18.1 SSM整合思路

- Spring + SpringMVC
  - 容器对象的管理问题：
    - SpringMVC容器对象，有DispatcherServlet管理
    - <font color="skyblue">Spring容器对象，由ContextLoaderListener管理</font>
  - 解决组件扫描的冲突问题：
    - SpringMVC只扫描Controller层
    - Spring扫描排除Controller层
- Spring + Mybatis
  - 关于数据源、事务管理的代码冲突问题
    - 统一交给Spring管理
  - Spring管理Mybatis核心对象
    - SQLSessionFactory
    - Mapper代理对象



#### 18.2 SSM整合步骤

- Spring + SpringMVC

  - 导入jar包

    ```xml
    <!--spring-webmvc-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.1</version>
    </dependency>
    <!-- 导入thymeleaf与spring5的整合包 -->
    <dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring5</artifactId>
        <version>3.0.12.RELEASE</version>
    </dependency>
    <!--servlet-api-->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
    </dependency>
    ```

  - 配置文件

    - web.xml

      - 注册CharacterEncodingFilter,解决请求乱码问题

      - 注册HiddenHttpMethodFilter,支持PUT&DELETE提交【REST风格】

      - 注册DispatcherServlet【前端控制器】，管理springMVC容器对象

      - 注册一个上下文参数【contextConfigLocation】,设置spring.xml配置文件路径

      - 注册<font color="skyblue">ContextLoaderListener</font>,管理spring容器对象

        ```xml
        <?xml version="1.0" encoding="UTF-8"?>
        <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
                 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
                 version="4.0">
        
            <!--    注册一个上下文参数【contextConfigLocation】,设置spring.xml配置文件路径-->
            <context-param>
                <param-name>contextConfigLocation</param-name>
                <param-value>classpath:spring.xml</param-value>
            </context-param>
        
            <!--    注册ContestLoaderListener，管理spring容器对象-->
            <listener>
                <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
            </listener>
        
            <!--    处理请求和响应乱码的过滤器-->
            <filter>
                <filter-name>CharacterEncodingFilter</filter-name>
                <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
                <init-param>
                    <param-name>encoding</param-name>
                    <param-value>UTF-8</param-value>
                </init-param>
                <init-param>
                    <param-name>forceEncoding</param-name>
                    <param-value>true</param-value>
                </init-param>
            </filter>
            <filter-mapping>
                <filter-name>CharacterEncodingFilter</filter-name>
                <url-pattern>/*</url-pattern>
            </filter-mapping>
        
            <!--    rest风格的CRUD的过滤器-->
            <filter>
                <filter-name>HiddenHttpMethodFilter</filter-name>
                <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
            </filter>
            <filter-mapping>
                <filter-name>HiddenHttpMethodFilter</filter-name>
                <!--        所有请求都需要经过过滤器-->
                <url-pattern>/*</url-pattern>
            </filter-mapping>
        
            <!--    注册DIspatcherServlet【前端控制器】，管理SpringMVC容器对象-->
            <servlet>
                <servlet-name>DispatcherServlet</servlet-name>
                <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
                <!-- 设置springmvc.xml配置文件路径【管理容器对象】-->
                <init-param>
                    <param-name>contextConfigLocation</param-name>
                    <param-value>classpath:springmvc.xml</param-value>
                </init-param>
                <!-- 设置DispatcherServlet优先级【启动服务器时，创建当前Servlet对象】-->
                <load-on-startup>1</load-on-startup>
            </servlet>
            <servlet-mapping>
                <servlet-name>DispatcherServlet</servlet-name>
                <url-pattern>/</url-pattern>
            </servlet-mapping>
        </web-app>
        ```

    - springmvc.xml

      - 开启组件扫描【只扫描Controller层】
      - 装配视图解析器
      - 装配视图控制器【view-controller】
      - 装配default-servlet-handler,解决静态资源加载问题
      - 装配annotation-driven,解决后续问题
        - 解决view-controllerl问题
        - 解决default-servlet-.handlerl问题
        - 解决jackson装配消息转换器问题【等23+种】

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xmlns:context="http://www.springframework.org/schema/context"
             xmlns:mvc="http://www.springframework.org/schema/mvc"
             xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">
      
          <!--    开启组件扫描-->
          <context:component-scan base-package="com" use-default-filters="false">
              <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
          </context:component-scan>
      
          <!--    装配视图解析器-->
          <bean class="org.thymeleaf.spring5.view.ThymeleafViewResolver" id="viewResolver">
              <property name="characterEncoding" value="utf-8"/>
              <property name="templateEngine">
                  <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
                      <property name="templateResolver">
                          <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                              <!-- 配置前缀-->
                              <property name="prefix" value="/WEB-INF/pages/"/>
                              <!-- 配置后缀-->
                              <property name="suffix" value=".html"/>
                              <!-- 配置字符集-->
                              <property name="characterEncoding" value="utf-8"/>
                          </bean>
                      </property>
                  </bean>
              </property>
          </bean>
      
          <!--    添加视图控制器-->
          <mvc:view-controller path="/" view-name="index"/>
      
          <!--    解决静态资源加载问题-->
          <mvc:default-servlet-handler/>
      
          <!--    解决添加上面标签之后会导致其他请求路线失效的问题-->
          <mvc:annotation-driven/>
      </beans>
      ```

    - spring.xml

      - 开启组件扫描【排除Controller层】

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    
        <!--    开启组件扫描【排除Controller层】-->
        <context:component-scan base-package="com">
            <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        </context:component-scan>
    </beans>
    ```

    

- Spring + Mybatis

  - 导入jar包

    - spring的jar包

      ```xml
      <!--spring-jdbc-->
      <!--spring-orm-->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-orm</artifactId>
          <version>5.3.1</version>
      </dependency>
      ```

    - mybatis的jar包

      ```xml
      <!--导入druid的jar包-->
      <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>1.1.10</version>
      </dependency>
      <!--导入MySQL的驱动包-->
      <dependency>
          <groupId>mysql</groupId>
          <artifactId>mysql-connector-java</artifactId>
          <version>8.0.30</version>
      </dependency>
      <!--导入MyBatis的jar包-->
      <dependency>
          <groupId>org.mybatis</groupId>
          <artifactId>mybatis</artifactId>
          <version>3.5.10</version>
      </dependency>
      <!-- 分页插件的jar包-->
      <dependency>
          <groupId>com.github.pagehelper</groupId>
          <artifactId>pagehelper</artifactId>
          <version>5.0.0</version>
      </dependency>
      ```

    - spring与mybatis整合jar包

    ```xml
    <!--        mybatis-srping-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.6</version>
    </dependency>
    ```

  - 配置文件：

    - spring.xml

      - 开启组件扫描【排除Controller层】
      - 加载外部属性文件
      - 装配数据源【DruidDataSource】
      - 装配事务管理器【DataSourceTransactionManager】
      - 开启声明式事务管理注解支持
      - 装配SqlSessionFactoryBean,管理SqlSessionFactory
      - 装配MapperScannerConfigurer,管理Mapper代理对象

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <beans xmlns="http://www.springframework.org/schema/beans"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:util="http://www.springframework.org/schema/util"
             xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
             xmlns:mybatis-spring="http://mybatis.org/schema/mybatis-spring"
             xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/util https://www.springframework.org/schema/util/spring-util.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd http://mybatis.org/schema/mybatis-spring http://mybatis.org/schema/mybatis-spring.xsd">
      
          <!--    开启组件扫描【排除Controller层】-->
          <context:component-scan base-package="com">
              <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
          </context:component-scan>
      
          <!--    加载外部属性文件db.properties-->
          <context:property-placeholder location="classpath:db.properties"></context:property-placeholder>
      
          <!--    装配数据源【DruidDataSource】-->
          <bean id="dateSoure" class="com.alibaba.druid.pool.DruidDataSource">
              <property name="driverClassName" value="${db.driverClassName}"></property>
              <property name="url" value="${db.url}"></property>
              <property name="username" value="${db.username}"></property>
              <property name="password" value="${db.password}"></property>
          </bean>
      
          <!--    装配事务管理器【DataSourceTransactionManager】-->
          <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
              <property name="dataSource" ref="dateSoure"></property>
          </bean>
      
          <!--    开启声明式事务管理注解支持-->
          <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
      
          <!--    装配SqlSessionFactoryBean,管理SqlSessionFactory-->
          <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
              <!--        设置数据源-->
              <property name="dataSource" ref="dateSoure"></property>
              <!--        设置mybatis-config.xml核心配置文件路径-->
              <property name="configLocation" value="classpath:mybatis-config.xml"></property>
              <!--        设置类型别名-->
              <property name="typeAliasesPackage" value="com.pojo"></property>
              <!--        设置映射文件路径-->
              <property name="mapperLocations" value="classpath:mapper/*.xml"></property>
          </bean>
      
          <!--    装配MapperScannerConfigurer,管理Mapper代理对象-->
          <mybatis-spring:scan base-package="com.mapper"></mybatis-spring:scan>
      </beans>
      ```

    - mybatis-config.xml【核心配置文件】

      - 设置别名
      - 开启驼峰式命名
      - 设置

      ```xml
      <?xml version="1.0" encoding="UTF-8" ?>
      <!DOCTYPE configuration
              PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
              "http://mybatis.org/dtd/mybatis-3-config.dtd">
      
      <configuration>
          <!--作用：这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。-->
          <settings>
              <!--是否开启驼峰命名自动映射，默认值false，如设置true会自动将 字段a_col与aCol属性自动映射-->
              <setting name="mapUnderscoreToCamelCase" value="true"/>
              <!-- 开启延迟加载 -->
              <setting name="lazyLoadingEnabled" value="true"/>
              <!-- 设置加载的数据是按需加载3.4.2及以后的版本该步骤可省略-->
              <setting name="aggressiveLazyLoading" value="false"/>
              <!-- 开启二级缓存-->
              <setting name="cacheEnabled" value="true"/>
          </settings>
      
          <!--    添加分页插件-->
          <plugins>
              <plugin interceptor="com.github.pagehelper.PageInterceptor"/>
          </plugins>
      </configuration>
      ```



#### 18.3 添加分页插件































































































































