# 字符串相关函数

使用emp表演示：

![image-20220806093637191](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220806093637191.png)

相关函数：

![image-20220806093657822](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220806093657822.png)



```mysql
-- CHARSET(str) 返回字串字符集
SELECT CHARSET(ename) FROM emp; 

-- CONCAT (string2 [,... ]) 连接字串, 将多个列拼接成一列
SELECT CONCAT(ename, ' 工作是 ', job) FROM emp; 

-- INSTR (string ,substring ) 返回 substring 在 string 中出现的位置,没有返回 0
-- dual 亚元表, 系统表 可以作为测试表使用
SELECT INSTR('hanshunping', 'ping') FROM DUAL;

-- UCASE (string2 ) 转换成大写
SELECT UCASE(ename) FROM emp; 
-- LCASE (string2 ) 转换成小写
SELECT LCASE(ename) FROM emp; 

-- LEFT (string2 ,length )从 string2 中的左边起取 length 个字符
-- RIGHT (string2 ,length ) 从 string2 中的右边起取 length 个字符
SELECT LEFT(ename, 2) FROM emp;

-- LENGTH (string )string 长度[按照字节]
SELECT LENGTH(ename) FROM emp;

-- REPLACE (str ,search_str ,replace_str )
-- 在 str 中用 replace_str 替换 search_str
-- 如果是 manager 就替换成 经理
SELECT ename, REPLACE(job,'MANAGER', '经理') FROM emp;

-- STRCMP (string1 ,string2 ) 逐字符比较两字串大小
SELECT STRCMP('hsp', 'hsp') FROM DUAL; 

-- SUBSTRING (str , position [,length ])
-- 从 str 的 position 开始【从 1 开始计算】,取 length 个字符
-- 从 ename 列的第一个位置开始取出 2 个字符
SELECT SUBSTRING(ename, 1, 2) FROM emp; 

-- LTRIM (string2 ) RTRIM (string2 ) TRIM(string)
-- 去除前端空格或后端空格
SELECT LTRIM(' 韩顺平教育') FROM DUAL;
SELECT RTRIM('韩顺平教育 ') FROM DUAL;
SELECT TRIM(' 韩顺平教育 ') FROM DUAL;
```



# 数学相关函数

相关函数：

![image-20220806094743817](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220806094743817.png)



# 时间日期相关函数

相关函数：

![image-20220806102241483](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220806102241483.png)

```

```



# 加密和系统函数

![image-20220806111247709](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220806111247709.png)

```mysql
-- USER() 查询用户
-- 可以查看登录到 mysql 的有哪些用户，以及登录的 IP
SELECT USER() FROM DUAL; -- 用户@IP 地址

-- DATABASE()查询当前使用数据库名称
SELECT DATABASE();

-- MD5(str) 为字符串算出一个 MD5 32 的字符串，常用(用户密码)加密
-- root 密码是 hsp -> 加密 md5 -> 在数据库中存放的是加密后的密码
SELECT MD5('hsp') FROM DUAL;
SELECT LENGTH(MD5('hsp')) FROM DUAL; -- 演示用户表，存放密码时，是 md5

-- PASSWORD(str) -- 加密函数, MySQL 数据库的用户密码就是 PASSWORD 函数加密
SELECT PASSWORD('hsp') FROM DUAL; -- 数据库的 *81220D972A52D4C51BB1C37518A2613706220DAC
-- select * from mysql.user \G 从原文密码 str 计算并返回密码字符串
-- 通常用于对 mysql 数据库的用户密码加密
-- mysql.user 表示 数据库.表
SELECT * FROM mysql.use
```



# 流程控制函数

![image-20220806113930760](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220806113930760.png)

```mysql
SELECT IF(TRUE, 1, 2) FROM DUAL;

SELECT IFNULL(NULL,2);

SELECT CASE
    WHEN TRUE THEN 'jack' -- jack
    WHEN FALSE THEN 'tom'
    ELSE 'mary' END;
    
SELECT ename, (CASE 
	WHEN job = "clerk" THEN "职员"
	WHEN job = "manager" THEN "经理"
	WHEN job = "salesman" THEN "销售人员"
	ELSE job END) as new_job
		FROM emp;
```



# 分页查询

![image-20220806123619805](https://raw.githubusercontent.com/Young-Allen/pic/main/image-20220806123619805.png)



