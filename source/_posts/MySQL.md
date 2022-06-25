---
title: MySQL
time:  12-31
date:  2022-04-15 12:31:45
categories:
- MySQL
tags: 
- MySQL
---

 本文抄袭之 Melody 博客 [MySQL](https://i-melody.github.io/2022/01/22/Java/%E5%85%A5%E9%97%A8%E9%98%B6%E6%AE%B5/22%20MySQL/)

MySQL-韩顺平，讲的非常细。

<!-- more -->

**MySQL**

> [下载地址](http://dev.mysql.com/get/Downloads/MySQL-5.7.19-winx64.zip)
>
> [安装方法](https://www.bilibili.com/video/BV1fh411y7R8?p=732&t=0.5)

连接到 MySQL 服务的指令：`mysql -h 主机名 -P 端口 -u 用户名 -p密码`

1. -p密码 不要有空格
2. -p 后面不写密码，回车会要求输入密码
3. 如果不写 -h 默认是本机
4. 如果不写 -p 默认是 3306

**Navicat**

> 可视化 MySQL 管理软件
>
> [下载地址](http://www.navicat.com.cn/products/)

SQLyog

> 可视化 MySQL 管理软件 (韩老师讲课用这款)
>
> [下载地址](https://sqlyog.en.softonic.com/ )

# 1 数据库

1. 安装 MySQL 数据库，就是在主机安装一个数据库管理系统。该系统可以管理多个数据库。DBMS（database manage system）

2. 一个数据库中可以拥有多张表，以保存数据（信息）。这些表的本质仍是文件

3. 表的一行称为一条记录。在 Java 中，一行记录往往用对象表示

4. SQL 语句分类
   * DDL（数据定义语句）：[create 表，库]
   * DML（数据操作语句）：[增加 insert]、[修改 update]、[删除 delete]
   * DQL（数据查询语句）：[select]
   * DCL（数据控制语句）：[管理数据库：如用户权限 revoke grant]

5. cmd开启数据库

   net start mysql

   net stop mysql

## 1.1 创建数据库

> ```
> CREATE DATABASE IF NOT EXISTS `Melody` CHARACTER SET 'utf8' COLLATE 'utf8_bin';
> ```
>
> 1. `CHARACTER SET`：指定数据库采用的字符集。
>
>    不指定的场合，默认 UTF-8
>
> 2. `COLLATE`：指定数据库字符集校对规则
>
>    * `utf8_bin` [区分大小写]（常用）
>    * `utf8_general_ci` [不区分大小写]（默认）
>
> 3. 在创建数据库 · 表时，为了规避关键字，可以使用反引号 ``` `
>
> 4. 创建表时，不指定字符集 · 校对规则的场合，默认和数据库相同

## 1.2 查看 · 删除 数据库

> ```
> SHOW DATABASES;										#1
> SHOW CREATE DATABASE `Melody`;						#2
> DROP DATABASE [IF EXISTS] `Melody`;					#3
> USE `sys`											#4
> ```
>
> 1. 显示数据库 语句
> 2. 显示数据库创建 语句（当初创建时的语句）
> 3. 数据库删除 语句（必须慎用）
> 4. 切换数据库

## 1.3 备份 · 恢复 数据库

> 备份（DOS）：`mysqldump -u 用户名 -p -B 数据库1 数据库2 > 路径\文件名.sql`
>
> 恢复（DOS，进入mysql）：`Source 路径\文件名.sql;`
>
> 备份库的表：`mysqldunp -u 用户名 -p 数据库 表1 表2 > 路径\文件名.sql`

# 2 MySQL 常用数据类型（列类型）

**数值类型**

* 整形
  * bit(M) [M 指定位数，默认1，范围 1 ~ 64]
  * tinyint [1 byte]
  * smallint [2 byte]
  * mediumint [3 byte]
  * int [4 byte]（常用）
  * bigint [8 byte]
* 小数类型
  * float [4 byte 单精度]
  * double [8 byte 双精度]（常用）
  * decimal(M,D) [大小不确定]（常用）

**文本类型（字符串）**

* char [0 ~ 255]（常用）
* carchar [0 ~ 65535]（常用）
* text [0 ~ 2 ^16^ - 1]（常用）
* longtext [ 0 ~ 2 ^32^ - 1]

**二进制数据类型**

* blob [0 ~ 2 ^16^ - 1]
* longblob [0 ~ 2 ^32^ - 1]

**时间日期类型**

* date [YYYY-MM-DD]
* time [HH:mm:SS]
* datetime [YYYY-MM-DD HH:mm:SS]（常用）
* timestamp [时间戳]（常用）
* year [年]

## 2.1 数组类型

### 2.1.1 整形

1. 使用规范：在满足需求的情况下，尽量使用占用空间小的类型

2. 如何定义一个无符号整数：在后面加入 `unsigned`

   > ```
   > CREATE TABLE `T1` (`ID` INT UNSIGNED);
   > ```

   无符号是咋呢？以 tinyint 为例，有符号的范围是 [-128, 127]，无符号范围是 [0, 255]

### 2.1.2 bit（位类型）

1. `bit(m)` 中 m 的范围在 [1, 64]
2. 添加数据的范围按照给定的位数（m）来确定。m = 8 的场合表示一个字节，范围是 255
3. 显示时，按照 bit 格式,例如`bit(8` 的场合 `7` 就显示为 `00000111`）
4. 查询时，仍能按照十进制数查询

### 2.1.3 小数类型

1. `float` 单精度，`double` 双精度

2. `decimal(M,D)` 可以支持更加精确的小数位。其中 M 是小数位数的总数（精度），D 是小数点后的位数（标度）。

   D = 0 的场合，值没有小数部分。M 最大值是 65，D 最大值是 30

   M 省略的场合，默认为 10；D 省略的场合，默认为 0

   希望小数精度高的场合，推荐使用 `decimal(M,D)`

## 2.2 文本类型(字符串)

1. `char(size)`：固定长度字符串，size 范围 [0, 255]，最大 255 **字符**。

2. `varchar(size)`：可变长度字符串，（UTF-8）size 范围 [0, 21844]，最大 65532 **字节**（1 ~ 3 字节要用于记录大小；UTF8 最大为 (65535 - 3) / 3 = 21844 **字符**）

3. size 表示字符数，不是字节数。无论中文英文，都最多存放 size 个字符

4. `char(4)` 是定长。这个场合，不管输入什么（如 ‘a’）都会占用 4 个字符的空间

   相对的，`varchar(4)` 是变长。实际占用空间取决于输入的字符（实际数据大小 + 额外的 1 ~ 3 字节）

5. 存放文本时，也可以使用 `text` 数据类型。可以把 `text` 列视为 `varchar` 列。`text` 不能有默认值，最大 2^16^ 字节

   希望存放更多字符，还能选择 `mediumtext`（最大 2^24^ 字节）或 `longtext`（最大 2^32^ 字节）

## 2.3 时间日期类型

> ```
> CREATE TABLE `T2` 
> 		(T1 DATETIME, 
>      	    T2 TIMESTAMP #①
>          		NOT NULL DEFAULT CURRENT_TIMESTAMP 
>          		ON UPDATE CURRENT_TIMESTAMP);
> ```
>
> ①后面意为：默认时间戳为当前时间戳

# 3 表

## 3.1 创建表

> ```
> CREATE TABLE `table_yuheng` (
> 		`name` VARCHAR(255), 
> 		`id` INT, `age` INT) CHARACTER SET 'utf8' COLLATE 'utf8_bin' ENGINE INNODB;
> ```
>
> field：指定列名
>
> datatype：指定列类型
>
> character set：字符集
>
> collate：校对规则
>
> engine：引擎

> ```
> CREATE TABLE genshin2 LIKE genshin;
> ```
>
> 这个意思是：以 genshin 的结构创建一个表 genshin2

## 3.2 修改·删除 表



* **添加列**

  > ```
  > ALTER TABLE emp 
  > 	  ADD image VARCHAR(32) NOT NULL DEFAULT '' 
  > 	  AFTER RESUME
  > ```
  >
  > 在员工表emp 增加一个image列，varchar类型(要求在resume后面)。

* **修改列**

  > ```
  > --  修改job列，使其长度为60。
  > ALTER TABLE emp 
  > 	MODIFY job VARCHAR(60) NOT NULL DEFAULT ''
  > ```
  >
  > ``` 
  > --  列名name修改为user_name
  > ALTER TABLE employee 
  > 	CHANGE `name` `user_name` VARCHAR(64) NOT NULL DEFAULT ''
  > DESC employee
  > ```
  >
  > 

* **删除列**

  ```
  --  删除id列。
  ALTER TABLE emp
  	DROP id
  ```

* **查看表的结构**

  > ```
  > DESC `表名`;
  > ```

* **修改表名**

  > ```
  > RENAME TABLE `表名` TO `新表名`;
  > --  表名改为employee。
  > RENAME TABLE emp TO employee
  > ```

* **修改表字符集**

  > ```
  > ALTER TABLE `表名` CHARACTER SET '字符集';
  > ```

* **修改存储引擎**

  
  
  > ```
  > ALTER TABLE `表名` ENGINE = 引擎;
  > ```
  
  

## 表复制与去重

{% spoiler "表复制与去重.sql" %}

```sql
-- 表的复制
-- 为了对某个sql语句进行效率测试，我们需要海量数据时，可以使用此法为表创建海量数据

CREATE TABLE my_tab01 
	( id INT,
	  `name` VARCHAR(32),
	  sal DOUBLE,
	  job VARCHAR(32),
	  deptno INT);
DESC my_tab01
SELECT * FROM my_tab01;

-- 演示如何自我复制
-- 1. 先把emp 表的记录复制到 my_tab01
INSERT INTO my_tab01 
	(id, `name`, sal, job,deptno)
	SELECT empno, ename, sal, job, deptno FROM emp;
-- 2. 自我复制
INSERT INTO my_tab01
	SELECT * FROM my_tab01;
SELECT COUNT(*) FROM my_tab01;

-- 如何删除掉一张表重复记录
-- 1. 先创建一张表 my_tab02, 
-- 2. 让 my_tab02 有重复的记录

CREATE TABLE my_tab02 LIKE emp; -- 这个语句 把emp表的结构(列)，复制到my_tab02

DESC my_tab02;

INSERT INTO my_tab02
	SELECT * FROM emp;
SELECT * FROM my_tab02;
-- 3. 考虑去重 my_tab02的记录
/*
	思路 
	(1) 先创建一张临时表 my_tmp , 该表的结构和 my_tab02一样
	(2) 把my_tmp 的记录 通过 distinct 关键字 处理后 把记录复制到 my_tmp
	(3) 清除掉 my_tab02 记录
	(4) 把 my_tmp 表的记录复制到 my_tab02
	(5) drop 掉 临时表my_tmp
*/
-- (1) 先创建一张临时表 my_tmp , 该表的结构和 my_tab02一样

create table my_tmp like my_tab02
-- (2) 把my_tmp 的记录 通过 distinct 关键字 处理后 把记录复制到 my_tmp
insert into my_tmp 
	select distinct * from my_tab02;

-- (3) 清除掉 my_tab02 记录
delete from my_tab02;
-- (4) 把 my_tmp 表的记录复制到 my_tab02
insert into my_tab02
	select * from my_tmp;
-- (5) drop 掉 临时表my_tmp
drop table my_tmp;

select * from my_tab02;
```

{% endspoiler %}

# 4 数据库的增删改查

> C（create 创建）R（read 查找）U（update 修改）D（delete 删除）

## 4.1 INSERT 语句

> ```
> INSERT INTO `表名` (列名1, 列名2, 列名3)
> 		VALUES (值1, 值2, 值3), (值1a, 值2a, 值3a);
> ```

1. 当不给某个字段值时，如有默认值会添加默认值，否则会报错。

2. 另外，给表中 所有字段 添加数据的场合，可以不写列名：

   > ```
   > INSERT INTO `表名` VALUES (值1, 值2, 值3);
   > ```

3.  细节说明

## 4.2 UPDATE 语句

> ```
> UPDATE `表名` 
> 		SET `列名` = 值, `列名2` = 值2 
> 				WHERE `条件列` = 条件值;
> 
> -- 例如：将姓名为 小妖怪 的员工薪水修改为 3000 元。
> UPDATE employee
> 		SET salary = 3000
> 				WHERE user_name = '小妖怪
> ```

1. 对于所有 `条件列` = `条件值` 的记录，将 `列名` 列改为 `值`
2. SET 子句指示要修改哪些列和给予哪些值
3. **如果不带 where 条件，会修改所有记录**

## 4.3 DELETE 语句

> ```
> DELETE FROM `表名` 
> 		WHERE `条件列` = 条件值;
> 
> -- 删除表中名称为’老妖怪’的记录。
> DELETE FROM employee
> 		WHERE user_name = '老妖怪'; 
> -- 删除表中所有记录, 老师提醒，一定要小心
> DELETE FROM employee; 
> -- Delete 语句不能删除某一列的值（可使用 update 设为 null 或者 ''）
> UPDATE employee 
> 		SET job = '' 
> 			WHERE user_name = '老妖怪';
> -- 要删除这个表
> DROP TABLE employee;
> ```

1. 不能删除一列的值。若要如此做，可以使用 update 语句将一列置空
2. delete 语句仅能删除记录，不能删除表本身。要如此做，可以使用 drop 语句
3. **如果不带 where 条件，会删除所有记录**

## 4.4 SELECT 语句

### （单表）

> ```
> SELECT 【DISTINCT】 `列1`, `列2` FROM `表名` 
> -- 查询表中所有学生的姓名和对应的英语成绩。
> SELECT `name`,english FROM student
> 
> SELECT * FROM `表名2`;
> - 查询表中所有的信息。
> SELECT * FROM student;
> ```

1. DISTINCT 是可选的。表示显示结果时，是否去掉重复数据（查询记录的所有字段都相同才会去重）
2. SELECT 列1 指定查询哪些列的数据。
3. SELECT * FROM 表名; 代表查询 哪张表所有列

###    **常用的运算符**

|            | 运算符                          | 说明                                  |
| ---------- | ------------------------------- | ------------------------------------- |
| 比较运算符 | `>` `<` `<=` `>=` `=` `!=` `<>` | 大于、小于、大于等于、不等于          |
|            | `BETWWEEN ... AND ...`          | 显示某一区间的值                      |
|            | `IN(值1, 值2, 值3)`             | 显示在 `IN` 列表中的值                |
|            | `LIKE '值'` `NOT LIKE '值'`     | 模糊查询（`LIKE 'A%'` 即 A 开头就行） |
|            | `IS NULL`                       | 判断是否为空                          |
| 逻辑运算符 | `AND`                           | 多个条件同时成立                      |
|            | `OR`                            | 多个条件任一成立                      |
|            | `NOT`                           | 不成立                                |



### **使用表达式对查询的列进行运算**

> ```
> SELECT `name`, (chinese + math + english) FROM `student`;
> ```
>
> 这个场合，显示的时候是 `name` 和 `chinese + math + english` 两列

**`AS` 语句**

可以用 AS 语句起个别名

> ```
> SELECT `name`, (chinese + math + english) AS `all` FROM `student`;
> ```
>
> 这个场合，显示的时候是 `name` 和 `all` 两列

### **使用 where 子句，进行过滤查询** 

```
-- select 语句
-- 查询姓名为赵云的学生成绩
SELECT * FROM student 
	WHERE `name` = '赵云'
-- 查询英语成绩大于90分的同学
SELECT * FROM student 
	WHERE english > 90
-- 查询总分大于200分的所有同学

SELECT * FROM student 
	WHERE (chinese + english + math) > 200
	
-- 查询math大于60 并且(and) id大于4的学生成绩
SELECT * FROM student
	WHERE math >60 AND id > 4
-- 查询英语成绩大于语文成绩的同学
SELECT * FROM student
	WHERE english > chinese
-- 查询总分大于200分 并且 数学成绩小于语文成绩,的姓赵的学生.
-- 赵% 表示 名字以韩开头的就可以
SELECT * FROM student
	WHERE (chinese + english + math) > 200 AND 
		math < chinese AND `name` LIKE '赵%'
-- 查询英语分数在 80－90之间的同学。
SELECT * FROM student
	WHERE english >= 80 AND english <= 90;
SELECT * FROM student
	WHERE english BETWEEN 80 AND 90; -- between .. and .. 是 闭区间
-- 查询数学分数为89,90,91的同学。
SELECT * FROM student 
	WHERE math = 89 OR math = 90 OR math = 91;
SELECT * FROM student 
	WHERE math IN (89, 90, 91);
-- 查询所有姓李的学生成绩。
SELECT * FROM student 
	WHERE `name` LIKE '韩%'
-- 查询数学分>80，语文分>80的同学
```

**`LIKE` 字符**

> ```
> SELECT * FROM `genshin` 
> 		WHERE `name` LIKE '_E%';
> ```
>
> 下划线 `_` 代表一个任意字符
>
> 百分号 `%` 代表任意个任意字符



### **用 `ORDER BY` 语句排序查询结果**

> ```
> SELECT * FROM `student` ORDER BY `math` DESC;
> ```
>
> 以 `math` 降序排列
>
> * 降序：DESC
> * 升序：ASC（不写也是默认升序）
>
> ```
> SELECT * FROM `genshin` 
> 		ORDER BY `age` ASC, `salary` DESC;
> ```
>
> 以 `age` 升序并以 `salary` 降序排列

### 分组语句 `GROUP BY`

> ```
> SELECT AVG(`salary`), `age` FROM `genshin` 
> 		group by `age`;
> ```
>
> 按 `age` 分组统计
>
> ```
> SELECT MAX(`salary`) FROM `genshin` 
> 		group by `age`, `name`;
> ```
>
> 按 `age` 和 `name` 分组统计

### **用 `HAVING` 语句过滤**(分组后)

> ```
> SELECT AVG(salary) FROM genshin 
> 		GROUP BY country 
> 		HAVING AVG(age) > 18;
> ```
>
> 按 `country` 分组统计，显示其中 `AVG(age) > 18` 的组

# 5 函数

##  统计函数

* 计数：`COUNT(列)`
* 平均值：`AVG(列)`
* 合计值：`SUM(列)`
* 最大 · 最小值：`MAX(列)` `MIN(列)`

### **统计函数 ****`COUNT`**

> 60`   SELECT COUNT(*) FROM`student` WHERE`math` > 60;   统计表中有多少记录，排除 为null的数目   -- 解释 :count(*) 返回满足条件的记录的行数 -- count(列): 统计满足条件的某列有多少个，但是会排除 为 null SELECT COUNT(`name`) FROM `student`;

### **sum 函数**

```text
-- 演示sum函数的使用
-- 统计一个班级数学总成绩？
SELECT SUM(math) FROM student;
-- 统计一个班级语文、英语、数学各科的总成绩
SELECT SUM(math) AS math_total_score,SUM(english),SUM(chinese) FROM student;
-- 统计一个班级语文、英语、数学的成绩总和
SELECT SUM(math + english + chinese) FROM student;
-- 统计一个班级语文成绩平均分
SELECT SUM(chinese)/ COUNT(*)  FROM student;
SELECT SUM(`name`) FROM student;

-- 演示avg的使用
-- 练习：
-- 求一个班级数学平均分？
SELECT AVG(math) FROM student;
-- 求一个班级总分平均分
SELECT AVG(math + english + chinese) FROM student;

-- 演示max 和 min的使用
-- 求班级最高分和最低分（数值范围在统计中特别有用）
SELECT MAX(math + english + chinese), MIN(math + english + chinese) 
  FROM student;

-- 求出班级数学最高分和最低分
SELECT MAX(math) AS math_high_socre, MIN(math)  AS math_low_socre
  FROM student;
```

## 字符串函数

![img](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1649926068094.png)

{% spoiler "字符串函数演示代码" %}

```text
-- 演示字符串相关函数的使用  ， 使用emp表来演示
-- CHARSET(str)  返回字串字符集
SELECT CHARSET(ename) FROM emp;
-- CONCAT (string2  [,... ])  连接字串, 将多个列拼接成一列
SELECT CONCAT(ename, ' 工作是 ', job) FROM emp;

-- INSTR (string ,substring )  返回substring在string中出现的位置,没有返回0
-- dual 亚元表, 系统表 可以作为测试表使用
SELECT INSTR('hanshunping', 'ping') FROM DUAL; 

-- UCASE (string2 )  转换成大写
SELECT UCASE(ename) FROM emp;

-- LCASE (string2 )  转换成小写

SELECT LCASE(ename) FROM emp;
-- LEFT (string2 ,length )  从string2中的左边起取length个字符
-- RIGHT (string2 ,length )  从string2中的右边起取length个字符
SELECT LEFT(ename, 2) FROM emp;

-- LENGTH (string )  string长度[按照字节]
SELECT LENGTH(ename) FROM emp;
-- REPLACE (str ,search_str ,replace_str )   
-- 在str中用replace_str替换search_str
-- 如果是manager 就替换成 经理
SELECT ename, REPLACE(job,'MANAGER', '经理')  FROM emp;

-- STRCMP (string1 ,string2 )  逐字符比较两字串大小
SELECT STRCMP('hsp', 'hsp') FROM DUAL;
-- SUBSTRING (str , position  [,length ])  
-- 从str的position开始【从1开始计算】,取length个字符
-- 从ename 列的第一个位置开始取出2个字符
SELECT SUBSTRING(ename, 1, 2) FROM emp;

-- LTRIM (string2 ) RTRIM (string2 )  TRIM(string)
-- 去除前端空格或后端空格
SELECT LTRIM('  韩顺平教育') FROM DUAL;
SELECT RTRIM('韩顺平教育   ') FROM DUAL;
SELECT TRIM('    韩顺平教育   ') FROM DUAL;
```

{% endspoiler %}

* 练习题代码

```text
-- 练习: 以首字母小写的方式显示所有员工emp表的姓名
-- 方法1 
-- 思路先取出ename 的第一个字符，转成小写的
-- 把他和后面的字符串进行拼接输出即可

SELECT CONCAT(LCASE(SUBSTRING(ename,1,1)),  SUBSTRING(ename,2)) AS new_name
  FROM emp;  

SELECT CONCAT(LCASE(LEFT(ename,1)),  SUBSTRING(ename,2)) AS new_name
  FROM emp; 
```

## 数学函数

![img](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1649940290031.png)

{% spoiler "math.sql" %}

```Vim
-- 演示数学相关函数

-- ABS(num)  绝对值
SELECT ABS(-10) FROM DUAL;
-- BIN (decimal_number )十进制转二进制
SELECT BIN(10) FROM DUAL;
-- CEILING (number2 )  向上取整, 得到比num2 大的最小整数
SELECT CEILING(-1.1) FROM DUAL;

-- CONV(number2,from_base,to_base)  进制转换
-- 下面的含义是 8 是十进制的8, 转成 2进制输出
SELECT CONV(8, 10, 2) FROM DUAL;
-- 下面的含义是 8 是16进制的8, 转成 2进制输出
SELECT CONV(16, 16, 10) FROM DUAL;

-- FLOOR (number2 )  向下取整,得到比 num2 小的最大整数
SELECT FLOOR(-1.1) FROM DUAL;

-- FORMAT (number,decimal_places )  保留小数位数(四舍五入)
SELECT FORMAT(78.125458,2) FROM DUAL;

-- HEX (DecimalNumber )  转十六进制

-- LEAST (number , number2  [,..])  求最小值
SELECT LEAST(0,1, -10, 4) FROM DUAL;
-- MOD (numerator ,denominator )  求余
SELECT MOD(10, 3) FROM DUAL;

-- RAND([seed])  RAND([seed]) 返回随机数 其范围为 0 ≤ v ≤ 1.0
-- 老韩说明
-- 1. 如果使用 rand() 每次返回不同的随机数 ，在 0 ≤ v ≤ 1.0
-- 2. 如果使用 rand(seed) 返回随机数, 范围 0 ≤ v ≤ 1.0, 如果seed不变，
--    该随机数也不变了
SELECT RAND() FROM DUAL;
```

{% endspoiler %}

## 日期函数

![img](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1649996367012.png)

{% spoiler "date.sql" %}

```SQL
 -- 日期时间相关函数

-- CURRENT_DATE (  )  当前日期
SELECT CURRENT_DATE() FROM DUAL;
-- CURRENT_TIME (  )  当前时间
SELECT CURRENT_TIME()  FROM DUAL;
-- CURRENT_TIMESTAMP (  ) 当前时间戳
SELECT CURRENT_TIMESTAMP()  FROM DUAL;

-- 创建测试表 信息表
CREATE TABLE mes(
  id INT , 
  content VARCHAR(30), 
  send_time DATETIME);
  
  
-- 添加一条记录
INSERT INTO mes 
  VALUES(1, '北京新闻', CURRENT_TIMESTAMP()); 
INSERT INTO mes VALUES(2, '上海新闻', NOW());
INSERT INTO mes VALUES(3, '广州新闻', NOW());

SELECT * FROM mes;
SELECT NOW() FROM DUAL;

-- 上应用实例
-- 显示所有新闻信息，发布日期只显示 日期，不用显示时间.
SELECT id, content, DATE(send_time) 
  FROM mes;
-- 请查询在10分钟内发布的新闻, 思路一定要梳理一下.
SELECT * 
  FROM mes
  WHERE DATE_ADD(send_time, INTERVAL 10 MINUTE) >= NOW()

SELECT * 
  FROM mes
  WHERE send_time >= DATE_SUB(NOW(), INTERVAL 10 MINUTE) 

-- 请在mysql 的sql语句中求出 2011-11-11 和 1990-1-1 相差多少天
SELECT DATEDIFF('2011-11-11', '1990-01-01') FROM DUAL;
-- 请用mysql 的sql语句求出你活了多少天? [练习] 1986-11-11 出生
SELECT DATEDIFF(NOW(), '1986-11-11') FROM DUAL;
-- 如果你能活80岁，求出你还能活多少天.[练习] 1986-11-11 出生
-- 先求出活80岁 时, 是什么日期 X
-- 然后在使用 datediff(x, now()); 1986-11-11->datetime
-- INTERVAL 80 YEAR ： YEAR 可以是 年月日，时分秒
-- '1986-11-11' 可以date,datetime timestamp 
SELECT DATEDIFF(DATE_ADD('1986-11-11', INTERVAL 80 YEAR), NOW()) 
  FROM DUAL;
  
SELECT TIMEDIFF('10:11:11', '06:10:10') FROM DUAL;

-- YEAR|Month|DAY| DATE (datetime )
SELECT YEAR(NOW()) FROM DUAL;
SELECT MONTH(NOW()) FROM DUAL;
SELECT DAY(NOW()) FROM DUAL;
SELECT MONTH('2013-11-10') FROM DUAL;
-- unix_timestamp() : 返回的是1970-1-1 到现在的秒数
SELECT UNIX_TIMESTAMP() FROM DUAL;
-- FROM_UNIXTIME() : 可以把一个unix_timestamp 秒数[时间戳]，转成指定格式的日期
-- %Y-%m-%d 格式是规定好的，表示年月日
-- 意义：在开发中，可以存放一个整数，然后表示时间，通过FROM_UNIXTIME转换
--   
SELECT FROM_UNIXTIME(1618483484, '%Y-%m-%d') FROM DUAL;
SELECT FROM_UNIXTIME(1618483100, '%Y-%m-%d %H:%i:%s') FROM DUAL; 
```

{% endspoiler %}

##  加密函数和系统函数

![img](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1649998321667.png)

{% spoiler "pwd.sql" %}

```SQL
-- 演示加密函数和系统函数

-- USER()  查询用户
-- 可以查看登录到mysql的有哪些用户，以及登录的IP
SELECT USER() FROM DUAL; -- 用户@IP地址
-- DATABASE()  查询当前使用数据库名称
SELECT DATABASE();

-- MD5(str)  为字符串算出一个 MD5 32的字符串，常用(用户密码)加密
-- root 密码是 hsp -> 加密md5 -> 在数据库中存放的是加密后的密码
SELECT MD5('hsp') FROM DUAL;
SELECT LENGTH(MD5('hsp')) FROM DUAL;

-- 演示用户表，存放密码时，是md5
CREATE TABLE hsp_user
  (id INT , 
  `name` VARCHAR(32) NOT NULL DEFAULT '', 
  pwd CHAR(32) NOT NULL DEFAULT '');
INSERT INTO hsp_user 
  VALUES(100, '韩顺平', MD5('hsp'));
SELECT * FROM hsp_user; -- csdn

SELECT * FROM hsp_user  -- SQL注入问题
  WHERE `name`='韩顺平' AND pwd = MD5('hsp')  


-- PASSWORD(str) -- 加密函数, MySQL数据库的用户密码就是 PASSWORD函数加密

SELECT PASSWORD('hsp') FROM DUAL; -- 数据库的 *81220D972A52D4C51BB1C37518A2613706220DAC


-- select * from mysql.user \G   从原文密码str 计算并返回密码字符串
-- 通常用于对mysql数据库的用户密码加密
-- mysql.user 表示 数据库.表 
SELECT * FROM mysql.user
```

{% endspoiler %}

## 流程控制函数

**主要**

![img](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650000194473.png)

{% spoiler "control.sql" %}

```SQL
# 演示流程控制语句

# IF(expr1,expr2,expr3)  如果expr1为True ,则返回 expr2 否则返回 expr3
SELECT IF(TRUE, '北京', '上海') FROM DUAL;
# IFNULL(expr1,expr2)  如果expr1为空NULL,则返回expr2;如果expr1不为空NULL,则返回expr2
SELECT IFNULL( NULL, '韩顺平教育') FROM DUAL;

# SELECT CASE WHEN expr1 THEN expr2 WHEN expr3 THEN expr4 ELSE expr5 END; [类似多重分支.]
# 如果expr1 为TRUE,则返回expr2,如果expr2 为t, 返回 expr4, 否则返回 expr5

SELECT CASE 
  WHEN TRUE THEN 'jack'  -- jack
  WHEN FALSE THEN 'tom' 
  ELSE 'mary' END

-- 练习
-- 1. 查询emp 表, 如果 comm 是null , 则显示0.0
--    老师说明，判断是否为null 要使用 is null, 判断不为空 使用 is not
SELECT ename, IF(comm IS NULL , 0.0, comm)
  FROM emp;
SELECT ename, IFNULL(comm, 0.0)
  FROM emp;
-- 2. 如果emp 表的 job 是 CLERK 则显示 职员， 如果是 MANAGER 则显示经理
--     如果是 SALESMAN 则显示 销售人员，其它正常显示

SELECT ename, (SELECT CASE 
    WHEN job = 'CLERK' THEN '职员' 
    WHEN job = 'MANAGER' THEN '经理'
    WHEN job = 'SALESMAN' THEN '销售人员' 
    ELSE job END) AS 'job'
  FROM emp; 

SELECT * FROM emp;
SELECT * FROM dept;
SELECT * FROM salgrade;
```

{% endspoiler %}

---

# select（单表加强）

## 简单加强

{% spoiler "select.strong" %}

```sql
-- 查询加强
-- ■ 使用where子句
-- 	?如何查找1992.1.1后入职的员工
-- 老师说明： 在mysql中,日期类型可以直接比较, 需要注意格式
SELECT * FROM emp
	WHERE hiredate > '1992-01-01'
-- ■ 如何使用like操作符(模糊)
-- 	%: 表示0到多个任意字符 _: 表示单个任意字符
-- 	?如何显示首字符为S的员工姓名和工资
SELECT ename, sal FROM emp
	WHERE ename LIKE 'S%'
-- 	?如何显示第三个字符为大写O的所有员工的姓名和工资
SELECT ename, sal FROM emp
	WHERE ename LIKE '__O%'

-- ■ 如何显示没有上级的雇员的情况
SELECT * FROM emp
	WHERE mgr IS NULL;
-- ■ 查询表结构 
DESC emp 

-- 使用order by子句
--   ?如何按照工资的从低到高的顺序[升序]，显示雇员的信息
SELECT * FROM emp
	ORDER BY sal 
--   ?按照部门号升序而雇员的工资降序排列 , 显示雇员信息

SELECT * FROM emp
	ORDER BY deptno ASC , sal DESC;
```

{% endspoiler %}

## 分页查询

{% spoiler "page.sql" %}

```sql
-- 分页查询 limit
-- 按雇员的id号升序取出， 每页显示3条记录，请分别显示 第1页，第2页，第3页

-- 第1页
  
-- 第2页
SELECT * FROM emp 
	ORDER BY empno DESC
	LIMIT 3, 3;
-- 第3页
SELECT * FROM emp 
	ORDER BY empno 
	LIMIT 6, 3;
-- 推导一个公式 
SELECT * FROM emp
	ORDER BY empno 
	LIMIT (第几页- 1) * 每页显示记录数   , 每页显示记录数
		
-- 测试
SELECT job, COUNT(*) FROM emp GROUP BY  job;
-- 显示雇员总数，以及获得补助的雇员数
SELECT COUNT(*) FROM emp  WHERE mgr IS NOT NULL;
SELECT MAX(sal) - MIN(sal) FROM emp;
	
```

{% endspoiler %}

## 增强 `GROUP BY`

{% spoiler "groupby2.sql" %}

```sql
-- 增强group by 的使用

SELECT COUNT(*) 
	FROM emp 

-- (1) 显示每种岗位的雇员总数、平均工资。
SELECT COUNT(*), AVG(sal), job 
	FROM emp 
	GROUP BY job; 
-- (2) 显示雇员总数，以及获得补助的雇员数。
--  思路: 获得补助的雇员数 就是 comm 列为非null, 就是count(列)，如果该列的值为null, 是
--  不会统计 , SQL 非常灵活，需要我们动脑筋.
SELECT COUNT(*), COUNT(comm)
	FROM emp 

--  老师的扩展要求：统计没有获得补助的雇员数
SELECT COUNT(*), COUNT(IF(comm IS NULL, 1, NULL))
	FROM emp 

SELECT COUNT(*), COUNT(*) - COUNT(comm)
	FROM emp 

-- (3) 显示管理者的总人数。小技巧:尝试写->修改->尝试[正确的]
SELECT COUNT(DISTINCT mgr) 
	FROM emp; 

-- (4) 显示雇员工资的最大差额。
-- 思路： max(sal) - min(sal)
SELECT MAX(sal) - MIN(sal) 
	FROM emp;
```

{% endspoiler %}

## 总结

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650007266968.png)



# select（多表）

> 多表查询是基于两个和两个以上的表查询。

**笛卡尔积**

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650009886424.png)

> ```sql
> SELECT *  FROM emp, dept 
> ```
>
> 这个场合，输出的是两个表相乘（即：取出第一张表的每一行，与第二张表的每一行进行组合）
>
> ……这样，就需要加入筛选条件，比如
>
> ```sql
> SELECT ename,sal,dname,emp.deptno
> 	FROM emp, dept 
> 	WHERE emp.deptno = dept.deptno
> ```
>
> **多表查询的条件不能少于 表的个数 - 1，否则会出现笛卡尔积**

{% spoiler "many_tab" %}

```sql
-- 多表查询
-- ?显示雇员名,雇员工资及所在部门的名字 【笛卡尔集】
/*
	老韩分析
	1. 雇员名,雇员工资 来自 emp表
	2. 部门的名字 来自 dept表
	3. 需求对 emp 和 dept查询  ename,sal,dname,deptno
	4. 当我们需要指定显示某个表的列是，需要 表.列表
*/
SELECT ename,sal,dname,emp.deptno
	FROM emp, dept 
	WHERE emp.deptno = dept.deptno
	
SELECT * FROM emp;
SELECT * FROM dept;
SELECT * FROM salgrade;
-- 老韩小技巧：多表查询的条件不能少于 表的个数-1, 否则会出现笛卡尔集
-- ?如何显示部门号为10的部门名、员工名和工资 
SELECT ename,sal,dname,emp.deptno
	FROM emp, dept 
	WHERE emp.deptno = dept.deptno AND emp.deptno = 10

-- ?显示各个员工的姓名，工资，及其工资的级别

-- 思路 姓名，工资 来自 emp 13
--      工资级别 salgrade 5
-- 写sql , 先写一个简单，然后加入过滤条件...
SELECT ename, sal, grade 
	FROM emp , salgrade
	WHERE sal BETWEEN losal AND hisal; 

```

{% endspoiler %}

## 多表查询的 自连接

{% spoiler "代码" %}

```sql
-- 多表查询的 自连接

-- 思考题: 显示公司员工名字和他的上级的名字

-- 老韩分析： 员工名字 在emp, 上级的名字的名字 emp
-- 员工和上级是通过 emp表的 mgr 列关联
-- 这里老师小结：
-- 自连接的特点 1. 把同一张表当做两张表使用
--               2. 需要给表取别名 表名  表别名 
--		 3. 列名不明确，可以指定列的别名 列名 as 列的别名		
SELECT worker.ename AS '职员名' ,  boss.ename AS '上级名'
	FROM emp worker, emp boss
	WHERE worker.mgr = boss.empno;
SELECT * FROM emp;
```

{% endspoiler %}

##  子查询(行)

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650029759397.png)

{% spoiler "代码" %}

```sql
-- 子查询的演示
-- 请思考：如何显示与SMITH同一部门的所有员工?
/*
	1. 先查询到 SMITH的部门号得到
	2. 把上面的select 语句当做一个子查询来使用
*/
SELECT deptno 
	FROM emp 
	WHERE ename = 'SMITH'

-- 下面的答案.	
SELECT * 
	FROM emp
	WHERE deptno = (
		SELECT deptno 
		FROM emp 
		WHERE ename = 'SMITH'
	)

-- 课堂练习:如何查询和部门10的工作相同的雇员的
-- 名字、岗位、工资、部门号, 但是不含10号部门自己的雇员.

/*
	1. 查询到10号部门有哪些工作
	2. 把上面查询的结果当做子查询使用
*/
SELECT DISTINCT job 
	from emp 
	where deptno = 10;
	
--  下面语句完整

select ename, job, sal, deptno
	from emp
	where job in (
		SELECT DISTINCT job 
		FROM emp 
		WHERE deptno = 10
	) and deptno <> 10 
```

{% endspoiler %}

## all any

{% spoiler "代码" %}

```sql
-- all 和 any的使用

-- 请思考:显示工资比部门30的所有员工的工资高的员工的姓名、工资和部门号

SELECT ename, sal, deptno
	FROM emp
	WHERE sal > ALL(
		SELECT sal 
			FROM emp
			WHERE deptno = 30
		) 
-- 可以这样写
SELECT ename, sal, deptno
	FROM emp
	WHERE sal > (
		SELECT MAX(sal) 
			FROM emp
			WHERE deptno = 30
		) 

-- 请思考:如何显示工资比部门30的其中一个员工的工资高的员工的姓名、工资和部门号

SELECT ename, sal, deptno
	FROM emp
	WHERE sal > ANY(
		SELECT sal 
			FROM emp
			WHERE deptno = 30
		)

 SELECT ename, sal, deptno
	FROM emp
	WHERE sal > (
		SELECT MIN(sal) 
			FROM emp
			WHERE deptno = 30
		)

```

{% endspoiler %}



## 把子查询当做一张临时表

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650176725297.png)

{% spoiler "代码" %}

```sql
-- 查询ecshop中各个类别中，价格最高的商品

-- 查询 商品表
-- 先得到 各个类别中，价格最高的商品 max + group by cat_id, 当做临时表
-- 把子查询当做一张临时表可以解决很多很多复杂的查询

SELECT cat_id , MAX(shop_price) 
	FROM ecs_goods
	GROUP BY cat_id
	
	
-- 这个最后答案	
SELECT goods_id, ecs_goods.cat_id, goods_name, shop_price 
	FROM (
		SELECT cat_id , MAX(shop_price) AS max_price
		FROM ecs_goods
		GROUP BY cat_id
	) temp , ecs_goods
	WHERE  temp.cat_id = ecs_goods.cat_id 
	AND temp.max_price = ecs_goods.shop_price 
```

{% endspoiler %}

## 多列子查询

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650178385975.png)

{% spoiler "多列子查询" %}

```sql
-- 多列子查询

-- 请思考如何查询与allen的部门和岗位完全相同的所有雇员(并且不含allen本人)
-- (字段1， 字段2 ...) = (select 字段 1，字段2 from 。。。。)

-- 分析: 1. 得到smith的部门和岗位

SELECT deptno , job
	FROM emp 
	WHERE ename = 'ALLEN'
	
-- 分析: 2  把上面的查询当做子查询来使用，并且使用多列子查询的语法进行匹配
SELECT * 
	FROM emp
	WHERE (deptno , job) = (
		SELECT deptno , job
		FROM emp 
		WHERE ename = 'ALLEN'
	) AND ename != 'ALLEN'



-- 请查询 和宋江数学，英语，语文   
-- 成绩 完全相同的学生
SELECT * 
	FROM student
	WHERE (math, english, chinese) = (
		SELECT math, english, chinese
		FROM student
		WHERE `name` = '宋江'
	)

SELECT * FROM student;
```

{% endspoiler %}

## 子查询练习

{% spoiler "代码我就没写过" %}

```sql
-- 子查询练习

-- 请思考：查找每个部门工资高于本部门平均工资的人的资料
-- 这里要用到数据查询的小技巧，把一个子查询当作一个临时表使用

-- 1. 先得到每个部门的 部门号和 对应的平均工资

SELECT deptno, AVG(sal) AS avg_sal
	FROM emp GROUP BY deptno
	
-- 2. 把上面的结果当做子查询, 和 emp 进行多表查询
--    
SELECT ename, sal, temp.avg_sal, emp.deptno
	FROM emp, (
		SELECT deptno, AVG(sal) AS avg_sal
		FROM emp 
		GROUP BY deptno
	) temp 
	WHERE emp.deptno = temp.deptno AND emp.sal > temp.avg_sal
	
-- 查找每个部门工资最高的人的详细资料

SELECT ename, sal, temp.max_sal, emp.deptno
	FROM emp, (
		SELECT deptno, MAX(sal) AS max_sal
		FROM emp 
		GROUP BY deptno
	) temp 
	WHERE emp.deptno = temp.deptno AND emp.sal = temp.max_sal
	

-- 查询每个部门的信息(包括：部门名,编号,地址)和人员数量,我们一起完成。

-- 1. 部门名,编号,地址 来自 dept表
-- 2. 各个部门的人员数量 -》 构建一个临时表

SELECT COUNT(*), deptno 
	FROM emp
	GROUP BY deptno;
	

SELECT dname, dept.deptno, loc , tmp.per_num AS '人数'
	FROM dept, (
		SELECT COUNT(*) AS per_num, deptno 
		FROM emp
		GROUP BY deptno
	) tmp 
	WHERE tmp.deptno = dept.deptno

-- 还有一种写法 表.* 表示将该表所有列都显示出来, 可以简化sql语句
-- 在多表查询中，当多个表的列不重复时，才可以直接写列名

SELECT tmp.* , dname, loc
	FROM dept, (
		SELECT COUNT(*) AS per_num, deptno 
		FROM emp
		GROUP BY deptno
	) tmp 
	WHERE tmp.deptno = dept.deptno
```

{% endspoiler %}

## 合并查询

```sql
-- 合并查询

SELECT ename,sal,job FROM emp WHERE sal>2500 -- 5

SELECT ename,sal,job FROM emp WHERE job='MANAGER' -- 3

-- union all 就是将两个查询结果合并，不会去重
SELECT ename,sal,job FROM emp WHERE sal>2500 -- 5
UNION ALL
SELECT ename,sal,job FROM emp WHERE job='MANAGER' -- 3

-- union  就是将两个查询结果合并，会去重
SELECT ename,sal,job FROM emp WHERE sal>2500 -- 5
UNION 
SELECT ename,sal,job FROM emp WHERE job='MANAGER' -- 3
```



## 表外连接

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650259140807.png)



{% spoiler "表外连接.sql" %}

```sql
-- 外连接

-- 比如：列出部门名称和这些部门的员工名称和工作，
-- 同时要求 显示出那些没有员工的部门。

-- 使用我们学习过的多表查询的SQL， 看看效果如何?

SELECT dname, ename, job 
	FROM emp, dept
	WHERE emp.deptno = dept.deptno
	ORDER BY dname
SELECT * FROM dept;

SELECT * FROM emp;


-- 创建 stu
/*
id  name   
1   Jack
2   Tom
3   Kity
4   nono

*/
CREATE TABLE stu (
	id INT,
	`name` VARCHAR(32));
INSERT INTO stu VALUES(1, 'jack'),(2,'tom'),(3, 'kity'),(4, 'nono');
SELECT * FROM stu;
-- 创建 exam
/*
id   grade
1    56
2    76
11   8

*/
CREATE TABLE exam(
	id INT,
	grade INT);
INSERT INTO exam VALUES(1, 56),(2,76),(11, 8);
SELECT * FROM exam;

-- 使用左连接
-- （显示所有人的成绩，如果没有成绩，也要显示该人的姓名和id号,成绩显示为空）

SELECT `name`, stu.id, grade
	FROM stu, exam
	WHERE stu.id = exam.id;
	
-- 改成左外连接
SELECT `name`, stu.id, grade
	FROM stu LEFT JOIN exam
	ON stu.id = exam.id;
	
	
-- 使用右外连接（显示所有成绩，如果没有名字匹配，显示空)
-- 即：右边的表(exam) 和左表没有匹配的记录，也会把右表的记录显示出来
SELECT `name`, exam.id, grade
	FROM stu RIGHT JOIN exam
	ON stu.id = exam.id;

-- 列出部门名称和这些部门的员工信息(名字和工作)，
-- 同时列出那些没有员工的部门名。5min
-- 使用左外连接实现
SELECT dname, ename, job
	FROM dept LEFT JOIN emp
	ON dept.deptno = emp.deptno
	
-- 使用右外连接实现

SELECT dname, ename, job
	FROM emp RIGHT JOIN dept
	ON emp.deptno = dept.deptno

SELECT dname, ename, job
	FROM emp RIGHT JOIN dept
	ON dept.deptno = emp.deptno
	
-- 
```

{% endspoiler %}

> 在实际的开发中，我们绝大多数情况下使用的是前
> 面学过的连接,外连接用得少。



# 6 mysqls约束

## primary key(主键)

> **基本介绍**
> 约束用于确保数据库的数据满足特定的商业规则。
> 在mysqli中，约束包括：not null、.unique,
> primary key,foreign key,和check五种.

> **primary key(主键)-基本使用**
> 字段名 字段类型 primary key
> 用于唯一的标示表行的数据，当定义主键约束后，**该列不能重复**

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650260967325.png)

{% spoiler "主键使用" %}

```sql
-- 主键使用

-- id	name 	email
CREATE TABLE t17
	(id INT PRIMARY KEY, -- 表示id列是主键 
	`name` VARCHAR(32),
	email VARCHAR(32));
	
-- 主键列的值是不可以重复
INSERT INTO t17
	VALUES(1, 'jack', 'jack@sohu.com');
INSERT INTO t17
	VALUES(2, 'tom', 'tom@sohu.com');

INSERT INTO t17
	VALUES(1, 'hsp', 'hsp@sohu.com');
	
SELECT * FROM t17;

-- 主键使用的细节讨论
-- primary key不能重复而且不能为 null。
INSERT INTO t17
	VALUES(NULL, 'hsp', 'hsp@sohu.com');
-- 一张表最多只能有一个主键, 但可以是复合主键(比如 id+name)
CREATE TABLE t18
	(id INT PRIMARY KEY, -- 表示id列是主键 
	`name` VARCHAR(32), PRIMARY KEY -- 错误的
	email VARCHAR(32));
-- 演示复合主键 (id 和 name 做成复合主键)
CREATE TABLE t18
	(id INT , 
	`name` VARCHAR(32), 
	email VARCHAR(32),
	PRIMARY KEY (id, `name`) -- 这里就是复合主键
	);

INSERT INTO t18
	VALUES(1, 'tom', 'tom@sohu.com');
INSERT INTO t18
	VALUES(1, 'jack', 'jack@sohu.com');
INSERT INTO t18
	VALUES(1, 'tom', 'xx@sohu.com'); -- 这里就违反了复合主键
SELECT * FROM t18;

-- 主键的指定方式 有两种 
-- 1. 直接在字段名后指定：字段名  primakry key
-- 2. 在表定义最后写 primary key(列名); 
CREATE TABLE t19
	(id INT , 
	`name` VARCHAR(32) PRIMARY KEY, 
	email VARCHAR(32)
	);

CREATE TABLE t20
	(id INT , 
	`name` VARCHAR(32) , 
	email VARCHAR(32),
	PRIMARY KEY(`name`) -- 在表定义最后写 primary key(列名)
	);
 
-- 使用desc 表名，可以看到primary key的情况

DESC t20 -- 查看 t20表的结果，显示约束的情况
DESC t18
```

{% endspoiler %}

## not null

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650262494259.png){% spoiler "" %}



{% endspoiler %}

## unique

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650262476376.png)

{% spoiler "unique.sql" %}

```sql
-- unique的使用

CREATE TABLE t21
	(id INT UNIQUE ,  -- 表示 id 列是不可以重复的.
	`name` VARCHAR(32) , 
	email VARCHAR(32)
	);
	
INSERT INTO t21
	VALUES(1, 'jack', 'jack@sohu.com');

INSERT INTO t21
	VALUES(1, 'tom', 'tom@sohu.com');
	
-- unqiue使用细节
-- 1. 如果没有指定 not null , 则 unique 字段可以有多个null
-- 如果一个列(字段)， 是 unique not null 使用效果类似 primary key
INSERT INTO t21
	VALUES(NULL, 'tom', 'tom@sohu.com');
SELECT * FROM t21;
-- 2. 一张表可以有多个unique字段

CREATE TABLE t22
	(id INT UNIQUE ,  -- 表示 id 列是不可以重复的.
	`name` VARCHAR(32) UNIQUE , -- 表示name不可以重复 
	email VARCHAR(32)
	);
DESC t22


```

{% endspoiler %}

## 外键

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650269588695.png)

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650270094468.png)

{% spoiler "foreign.sql" %}

```sql
-- 外键演示

-- 创建 主表 my_class
CREATE TABLE my_class (
	id INT PRIMARY KEY , -- 班级编号
	`name` VARCHAR(32) NOT NULL DEFAULT '');
 
-- 创建 从表 my_stu
CREATE TABLE my_stu (
	id INT PRIMARY KEY , -- 学生编号
	`name` VARCHAR(32) NOT NULL DEFAULT '',
	class_id INT , -- 学生所在班级的编号
	-- 下面指定外键关系
	FOREIGN KEY (class_id) REFERENCES my_class(id))
	-- 测试数据
INSERT INTO my_class 
	VALUES(100, 'java'), (200, 'web');
INSERT INTO my_class 
	VALUES(300, 'php');
	
SELECT * FROM my_class;
INSERT INTO my_stu 
	VALUES(1, 'tom', 100);
INSERT INTO my_stu 
	VALUES(2, 'jack', 200);
INSERT INTO my_stu 
	VALUES(3, 'hsp', 300);
INSERT INTO my_stu 
	VALUES(4, 'mary', 400); -- 这里会失败...因为400班级不存在

INSERT INTO my_stu 
	VALUES(5, 'king', NULL); -- 可以, 外键 没有写 not null
SELECT * FROM my_class;

-- 一旦建立主外键的关系，数据不能随意删除了
DELETE FROM my_class
	WHERE id = 100; 
```

{% endspoiler %}



## check

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/Snipaste_2022-04-18_16-24-44.png)



{% spoiler "演示check" %}

```sql
-- 演示check的使用
-- mysql5.7目前还不支持check ,只做语法校验，但不会生效
-- 了解 
-- 学习 oracle, sql server, 这两个数据库是真的生效.

-- 测试
CREATE TABLE t23 (
	id INT PRIMARY KEY,
	`name` VARCHAR(32) ,
	sex VARCHAR(6) CHECK (sex IN('man','woman')),
	sal DOUBLE CHECK ( sal > 1000 AND sal < 2000)
	);
	
-- 添加数据
INSERT INTO t23 
	VALUES(1, 'jack', 'mid', 1);
SELECT * FROM t23;
```

{% endspoiler %}

## 商店售货系统表设计案例(约束练习)

{% spoiler "" %}

```sql
-- 使用约束的课堂练习

CREATE DATABASE shop_db;

-- 现有一个商店的数据库shop_db，记录客户及其购物情况，由下面三个表组成：
-- 商品goods（商品号goods_id，商品名goods_name，单价unitprice，商品类别category，
-- 供应商provider);
-- 客户customer（客户号customer_id,姓名name,住址address,电邮email性别sex,身份证card_Id);
-- 购买purchase（购买订单号order_id，客户号customer_id,商品号goods_id,购买数量nums);
-- 1 建表，在定义中要求声明 [进行合理设计]：
-- (1)每个表的主外键；
-- (2)客户的姓名不能为空值；
-- (3)电邮不能够重复;
-- (4)客户的性别[男|女] check 枚举..
-- (5)单价unitprice 在 1.0 - 9999.99 之间 check

-- 商品goods
CREATE TABLE goods (
	goods_id INT PRIMARY KEY,
	goods_name VARCHAR(64) NOT NULL DEFAULT '',
	unitprice DECIMAL(10,2) NOT NULL DEFAULT 0 
		CHECK (unitprice >= 1.0 AND unitprice <= 9999.99),
	category INT NOT NULL DEFAULT 0,
	provider VARCHAR(64) NOT NULL DEFAULT '');
	
-- 客户customer（客户号customer_id,姓名name,住址address,电邮email性别sex,
-- 身份证card_Id);
CREATE TABLE customer(
	customer_id CHAR(8) PRIMARY KEY, -- 程序员自己决定
	`name` VARCHAR(64) NOT NULL DEFAULT '',
	address VARCHAR(64) NOT NULL DEFAULT '',
	email VARCHAR(64) UNIQUE NOT NULL,
	sex ENUM('男','女') NOT NULL ,  -- 这里老师使用的枚举类型, 是生效
	card_Id CHAR(18)); 
	
-- 购买purchase（购买订单号order_id，客户号customer_id,商品号goods_id,
-- 购买数量nums);
CREATE TABLE purchase(
	order_id INT UNSIGNED PRIMARY KEY,
	customer_id CHAR(8) NOT NULL DEFAULT '', -- 外键约束在后
	goods_id INT NOT NULL DEFAULT 0 , -- 外键约束在后
	nums INT NOT NULL DEFAULT 0,
	FOREIGN KEY (customer_id) REFERENCES customer(customer_id),
	FOREIGN KEY (goods_id) REFERENCES goods(goods_id));
DESC goods;
DESC customer;
DESC purchase;
```

{% endspoiler %}

# 自增长

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650284615480.png)

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650284984709.png)

{% spoiler "" %}

```sql
-- 演示自增长的使用
-- 创建表
CREATE TABLE t24
	(id INT PRIMARY KEY AUTO_INCREMENT,
	 email VARCHAR(32)NOT NULL DEFAULT '',
	 `name` VARCHAR(32)NOT NULL DEFAULT ''); 
DESC t24
-- 测试自增长的使用
INSERT INTO t24
	VALUES(NULL, 'tom@qq.com', 'tom');

INSERT INTO t24
	(email, `name`) VALUES('hsp@sohu.com', 'hsp');

SELECT * FROM t24;

-- 修改默认的自增长开始值
ALTER TABLE t25 AUTO_INCREMENT = 100
CREATE TABLE t25
	(id INT PRIMARY KEY AUTO_INCREMENT,
	 email VARCHAR(32)NOT NULL DEFAULT '',
	 `name` VARCHAR(32)NOT NULL DEFAULT ''); 
INSERT INTO t25
	VALUES(NULL, 'mary@qq.com', 'mary');
INSERT INTO t25
	VALUES(666, 'hsp@qq.com', 'hsp');
SELECT * FROM t25;
```

{% endspoiler %}

# 索引

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650286533952.png)

索引的原理

索引的分类

索引小结

 ![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650456865606.png)

# 事务

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650519726692.png)

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650520274480.png)

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650523701241.png)

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650524024619.png)

没有必要不要修改

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650524276207.png)

---

# 存储引擎

![image-20220421150920125](C:/Users/28336/AppData/Roaming/Typora/typora-user-images/image-20220421150920125.png)

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650525115781.png)

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650525299900.png)

![image-20220421155302544](C:/Users/28336/AppData/Roaming/Typora/typora-user-images/image-20220421155302544.png)

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650527661067.png)

# 视图

![image-20220421160343930](C:/Users/28336/AppData/Roaming/Typora/typora-user-images/image-20220421160343930.png)

![image-20220421161022305](C:/Users/28336/AppData/Roaming/Typora/typora-user-images/image-20220421161022305.png)

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650529458394.png)

![image-20220421163143285](C:/Users/28336/AppData/Roaming/Typora/typora-user-images/image-20220421163143285.png)

# MySQL管理

![](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650530908960.png)

![image-20220421170055446](C:/Users/28336/AppData/Roaming/Typora/typora-user-images/image-20220421170055446.png)

# MySQL权限管理

