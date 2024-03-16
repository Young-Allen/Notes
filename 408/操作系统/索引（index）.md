# 索引（index）

## 1.1 什么是索引

索引是在数据库表的字段上添加的，是为了提高查询效率存在的一种机制。

一张表的一个字段可以添加一个索引，当然，多个字段联合起来也可以添加索引。

索引相当于一本书的目录，是为了缩小扫描范围而存在的一种机制。

![image-20211205093441031](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20211205093441031.png)

## 1.2 索引的实现原理

![image-20211205095216932](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20211205095216932.png)

![image-20211205095232687](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20211205095232687.png)



 ## 1.3 什么条件下会添加索引

在MYSQL当中，主键上，以及unique字段上都会自动添加索引的。

条件1：数据量庞大（到底有多么庞大，这个需要测试，因为每个硬件环境不同）

条件2：改字段经常出现在where后面，以条件的形式存在，也就是说这个字段总是被扫描。

条件3：该字段很少的DML（insert、delete、update）操作，因为DML之后索引需要重新排序。



建议不要随意添加索引，因为索引也是需要维护的，太多反而会降低系统的性能。

建议通过主键或者的unique约束的字段进行查询，效率是比较高的。



## 1.4 索引创建的语法

==创建索引：==

create index <索引名> on <表名> （字段名）；

create index emp_ename_index on emp(ename);

给emp表的ename字段添加索引，起名为： emp_ename_index

==删除索引：==

drop index emp_ename_index on emp;

将emp表上的emp_ename_index索引对象删除。



==查看是否使用索引：==

使用explain描述查询。

**explain** select * from emp where ename = ‘king’;

没添加索引的查询：

row=14（查询记录）

![image-20211205101919210](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20211205101919210.png)

添加索引后的查询：

row=1

![image-20211205102146698](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20211205102146698.png)



## 1.5 索引的失效

第一种情况：

![image-20211205102544013](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20211205102544013.png)

第二种情况：

使用or的时候会失效，如果使用or那么要求or两边的条件字段都要有索引，才会走索引。

如果其中一边有一个字段没有索引，那么另一个字段上的索引也会失效。



第三种情况：

使用复合索引的时候，没有使用左侧的列查找，索引失效

**什么是复合索引**：两个字段，或者更多的字段联合起来添加一个索引，叫做复合索引。

![image-20211205104257304](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20211205104257304.png)



第四种情况：

在where当中索引列参加了运算，索引失效

![image-20211205104446139](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20211205104446139.png)



第五种情况：

在where当中索引列使用了函数

![image-20211205104536732](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20211205104536732.png)



## 1.6 索引的分类

索引是各种数据库进行优化的重要手段。优化的时候优先考虑的因素就是索引。

索引的分类：

==单一索引：==一个字段上添加索引

==复合索引：==两个或更多字段上添加索引

==主键索引：==主键上添加索引

==唯一性索引：==具有unique约束的字段上添加索引

**注意**：唯一性比较弱的字段上添加索引用处不大。

