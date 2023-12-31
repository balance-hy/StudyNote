# MySQL

## 服务启动与连接

![image-20231025161855329](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202310251618922.png)

![image-20231025161923212](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202310251619716.png)

## 创建数据库

**在创建数据库和表的时候，可以使用反引号来规避关键字**

![image-20231026141924509](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202310261419355.png)

```sql
# 演示数据库操作
# 创建一个名称为balance的数据库 使用指令
CREATE DATABASE balance;# 默认字符集是utf8 mb4 排序规则是 utf8mb4_0900_ai_ci
# 删除数据库
DROP DATABASE balance;

# 创建一个名为balance01的数据库 设置字符集是utf8mb3 排序规则是 utf8mb3_bin
CREATE DATABASE balance01 CHARACTER SET utf8mb3 COLLATE utf8mb3_bin
```

##  删除和查询数据库

```sql
# 查看数据库语句 注意此处有s
SHOW DATABASES;

# 显示数据库创建语句 SHOW CREATE DATABASE db_name
SHOW CREATE DATABASE balance01;

# 数据库删除语句 DROP DATABASE [IF EXISTS] db_name
DROP DATABASE [IF EXISTS] balance01;
```

## 数据库的备份和恢复

![image-20231026145431090](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202310261454383.png)

恢复数据库，也可以直接复制语句，执行一遍。

### 备份表

```shell
mysqldump -u 用户名 -p 数据库 表1 表2 表n > 文件名.sql
```

## 创建表/删除表

![image-20231026151805639](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202310261518054.png)

```sql
CREATE TABLE `user`(
	id INT,# id INT UNSIGNED, 无符号
	userName VARCHAR(255),
	`password` VARCHAR(255),
	birthday DATE
)CHARACTER SET utf8mb3 COLLATE utf8mb3_bin ENGINE INNODB;

# 删除表
drop table xxx.xxx;# 第一个xxx为数据库名，第二个xxx为表名
```

## [数据类型](https://www.cnblogs.com/xrq730/p/8446246.html)

### 整型

**MySQL中整型默认是带符号的**。

| 数据类型  | 字节数 | 带符号最小值         |    带符号最大值     | 不带符号最小值 | 不带符号最大值       |
| :-------- | :----: | -------------------- | :-----------------: | -------------- | -------------------- |
| TINYINT   |   1    | -128                 |         127         | 0              | 255                  |
| SMALLINT  |   2    | -32768               |        32767        | 0              | 65535                |
| MEDIUMINT |   3    | -8388608             |       8388607       | 0              | 16777215             |
| INT       |   4    | -2147483648          |     2147483647      | 0              | 4294967295           |
| BIGINT    |   8    | -9223372036854775808 | 9223372036854775807 | 0              | 18446744073709551616 |

从实际开发的角度，我们**一定要为合适的列选取合适的数据类型**，即到底用不用得到这种数据类型？举个例子：

- 一个枚举字段明明只有0和1两个枚举值，选用TINYINT就足够了，但在开发场景下却使用了BIGINT，这就造成了资源浪费
- 简单计算一下，假使该数据表中有100W数据，那么总共浪费了700W字节也就是6.7M左右，如果更多的表这么做了，那么浪费的更多

要知道，**MySQL本质上是一个存储**，以Java为例，可以使用byte类型的地方使用了long类型问题不大，因为绝大多数的对象在程序中都是短命对象，方法执行完毕这块内存区域就被释放了，7个字节实际上不存在浪不浪费一说。但是MySQL作为一个存储，8字节的BIGINT放那儿就放那儿了，占据的空间是实实在在的。

### 浮点型 decimal

| **数据类型**            | **字节数** | **备注**                                                  |
| ----------------------- | ---------- | --------------------------------------------------------- |
| float                   | 4          | 单精度浮点型                                              |
| double                  | 8          | 双精度浮点型                                              |
| decimal(M，D)[UNSIGNED] |            | M为总位数(<=65)，D为小数位数(<=30),M默认或溢出时为10,D为0 |

我们总结一下float(M,D)、double(M、D)的用法规则：

- D表示浮点型数据小数点之后的精度，假如超过D位则四舍五入，即1.233四舍五入为1.23，1.237四舍五入为1.24
- M表示浮点型数据总共的位数，D=2则表示总共支持五位，即小数点前只支持三位数，所以我们并没有看到1000.23、10000.233、100000.233这三条数据的插入，因为插入都报错了

当我们不指定M、D的时候，会按照实际的精度来处理。

**float、double类型存在精度丢失问题**，即**写入数据库的数据未必是插入数据库的数据**，而decimal无论写入数据中的数据是多少，都不会存在精度丢失问题，这就是我们要引入decimal类型的原因，decimal类型常见于银行系统、互联网金融系统等对小数点后的数字比较敏感的系统中。

最后讲一下decimal和float/double的区别，个人总结**主要体现在两点上**：

- float/double在db中存储的是近似值，而decimal则是以字符串形式进行保存的
- decimal(M,D)的规则和float/double相同，但区别在float/double在不指定M、D时默认按照实际精度来处理而decimal在不指定M、D时默认为decimal(10, 0)

### 日期

| 数据类型  | 字节数 | 格式                | 备注                      |
| --------- | ------ | ------------------- | ------------------------- |
| date      | 3      | yyyy-MM-dd          | 存储日期值                |
| time      | 3      | HH:mm:ss            | 存储时分秒                |
| year      | 1      | yyyy                | 存储年                    |
| datetime  | 8      | yyyy-MM-dd HH:mm:ss | 存储日期+时间             |
| timestamp | 4      | yyyy-MM-dd HH:mm:ss | 存储日期+时间，可作时间戳 |

MySQL的时间类型的知识点比较简单，这里重点关注一下datetime与timestamp两种类型的区别：

- 上面列了，datetime占**8**个字节，timestamp占**4**个字节
- 由于大小的区别，datetime与timestamp能存储的时间范围也不同，datetime的存储范围为1000-01-01 00:00:00——9999-12-31 23:59:59，timestamp存储的时间范围为19700101080001——20380119111407
- **datetime默认值为空，当插入的值为null时，该列的值就是null；timestamp默认值不为空，当插入的值为null的时候，mysql会取当前时间**
- datetime**存储的时间与时区无关**，timestamp**存储的时间及显示的时间都依赖于当前时区**

在实际工作中，一张表往往我们会有两个默认字段，一个记录创建时间而另一个记录最新一次的更新时间，这种时候可以使用timestamp类型来实现：

```sql
create_time timestamp default current_timestamp comment "创建时间",
update_time timestamp default current_timestamp on update current_timestamp comment "修改时间",
```

### 字符

| 数据类型    | 字节长度 | 范围或用法                                              |
| ----------- | -------- | ------------------------------------------------------- |
| Char(M)     | M字符    | 定长字符串。意味着会存M个字符                           |
| VarChar(M)  | M字符    | 变长字符串，要求M<=255，意味着存实际字符的字节长度而非M |
| Tiny Text   | Max:255  | 大小写不敏感                                            |
| Text        | Max:64K  | 大小写不敏感                                            |
| Medium Text | Max:16M  | 大小写不敏感                                            |
| Long Text   | Max:4G   | 大小写不敏感                                            |

## 修改表

![image-20231027150549701](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202310271505279.png)

### 添加列

在user表上增加一个image列，varchar类型，不允许为空，默认空字符串（在birthday之后）

```sql
ALTER TABLE `user` ADD image VARCHAR(32) NOT NULL DEFAULT '' AFTER birthday
```

### 修改列

修改user表的password字段长度，不允许为空，默认空字符串

```sql
ALTER TABLE `user` MODIFY `password` VARCHAR(100) NOT NULL DEFAULT ''
```

修改user表的password字段名，变为pwd

```sql
ALTER TABLE user01 CHANGE `password` pwd VARCHAR(60) NOT NULL DEFAULT '';
```

### 删除列

删除user表的image列

```sql
ALTER TABLE `user` DROP image
```

### 修改表名

将user表改为user01

```sql
RENAME TABLE `user` TO user01
```

### 修改表字符集

将user表字符集改为utf8

```sql
ALTER TABLE user01 CHARACTER SET utf8
```

## 增删改查

### insert

![](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202310301305775.png)

```sql
CREATE TABLE `goods`(
	id INT,
	goods_name VARCHAR(10),
	price DOUBLE
)CHARACTER SET utf8mb3 COLLATE utf8mb3_bin ENGINE INNODB;

-- Insert添加数据
INSERT INTO goods (id,goods_name,price) VALUES(10,'huawei',2000);

-- Insert添加多条数据
INSERT INTO goods (id,goods_name,price) VALUES(10,'huawei',2000),(20,'vivo',3000),(30,'wei',4000);

-- 若是给表中所有列添加数据，可以不写列名
INSERT INTO goods VALUES(40,'oppo',4000);
```

![image-20231102132211504](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311021322148.png)

### delete

![image-20231102135903373](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311021359807.png)

```sql
-- 删掉 goods_name='huawei'的记录
delete from goods where goods_name='huawei';
-- 删掉所有记录
delete from goods;
```

### update

![image-20231102135241756](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311021352313.png)

```sql
UPDATE goods SET price=10000 where goods_name='huawei';

-- 若无where限制，会更改所有
UPDATE goods SET price=10000;
-- 可以用基本运算
UPDATE goods SET price=price+1100;
-- 可以同时更改多个
UPDATE goods SET price=price+1100,id=id+100;
```

### select 单表

![image-20231102140639387](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311021406415.png)

![image-20231102142410050](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311021424708.png)

```sql
-- select 使用运算
SELECT `goods_name`,(id+price) FROM goods;

-- (id+price)会直接在结果里显示，可以取别名
SELECT `goods_name`,(id+price) AS total FROM goods;
```

#### 条件查询

![image-20231102143154908](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311021431602.png)

| like 中通配符 | 作用                                                         |
| ------------- | :----------------------------------------------------------- |
| `%`           | 匹配零个或多个字符的任意字符串，like’Mc%’ 将搜索以字母 Mc 开头的所有字符串 |
| `_`           | 匹配任何单个字符，like’_heryl’ 将搜索以字母 heryl 结尾的所有六个字母的名称 |
| `[]`          | 指定范围 ([a-f]) 或集合 ([abcdef]) 中的任何单个字符          |
| `[^]`         | 不属于指定范围 ([a-f]) 或集合 ([abcdef]) 的任何单个字符      |
| `*`           | 同于DOS命令中的通配符，代表多个字符                          |
| `?`           | 同于DOS命令中的？通配符，代表单个字符                        |
| `#`           | 大致同上，不同的是代只能代表单个数字。k#k代表k1k,k8k,k0k     |

#### 排序

![image-20231102144433348](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311021444583.png)

```sql
-- price降序显示
SELECT * FROM goods ORDER BY price DESC;
```

#### 分组 groupby having

![image-20231105154055054](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311051540746.png)

```sql
#演示group by + having
GROUPby用于对查询的结果分组统计，(示意图)
-- having子句用于限制分组显示结果.
-- ?如何显示每个部门的平均工资和最高工资
-- avg(sal) max(sal)
-- 按照部门来分组查询
 SELECT AVG(sal), MAX(sal),deptno
 FROM emp GROUP BY deptno;
-- ?显示每个部门的每种岗位的平均工资和最低工资
#分组的标准变成两个,先按照部门再按照岗位分
SELECT AVG(sal),MIN(sal),deptno,job
FROM emp GROUP BY deptno,job


-- ?显示平均工资低于2000的部门号和它的平均工资//别名
SELECT AVG(sal),deptno
FROM emp GROUP BY deptno
 HAVING AVG(sal)<2000
#使用别名,效率更高,函数不用计算两次
SELECT AVG(sal) AS avg_sal,deptno
FROM emp GROUP BY deptno 
HAVING avg_sal<2000
```

##### having 和 where

- having是对一个表的数据进行分组之后，对组信息进行相应条件筛选

  having筛选时，只能根据select子句中可出现的字段（数据）来进行条件设定

  having子句与where子句一样，都是用于条件判断

- where是判断数据从磁盘读入内存的时候，having是判断分组统计之前的所有条件

- having子句中可以使用字段别名，而where不能使用

#### 单表查询加强



### select 多表



## 各类函数

### 统计函数count

![image-20231105152243004](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311051522391.png)

```sql
-- 统计有多少条记录
SELECT COUNT(*) FROM goods;

-- 条件过滤
SELECT COUNT(*) FROM goods WHERE price>9999;

-- count(*)和count(列)的区别
-- count(*)是返回满足条件的记录数
-- count(列)是返回列满足条件的记录数，但排除NULL的情况
```

### 和函数 sum

![image-20231105153059797](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311051531508.png)

```sql
-- 单列的统计，并取别名
SELECT SUM(price) AS total FROM goods;

-- 多列的统计，并取别名
SELECT SUM(id) AS id_total,SUM(price) AS price_total FROM goods;
```

### 最大最小值 max/min

![image-20231105153736765](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311051537021.png)

### 平均值 avg

![image-20231105153810461](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311051538770.png)

### 字符串函数

标红意味着常使用

![image-20231105162228386](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311051622664.png)

```sql
-- CHARSET(str)    返回字串字符集
SELECT CHARSET(ename) FROM emp #utf8
-- CONCAT (string2 [,... ])    连接字串
SELECT CONCAT(ename,' job is ',job) FROM emp; #把名字和工作连接在一起

-- INSTR (string ,substring )    返回substring在string中出现的位置，没有返回0，下标从1开始
# DUAL叫亚元表,系统表,可以作为测试使用
SELECT INSTR('balance','an') FROM DUAL; #返回4

-- UCASE (string2 )    转换成大写
SELECT UCASE(ename) FROM emp

-- LCASE (string2 )    转换成小写
SELECT LCASE(ename) FROM emp

-- LEFT (string2,length )    从string2中的左边起取length个字符
-- RIGHT 右边也可以，类似
SELECT LEFT(ename,1) FROM emp

-- SUBSTRING (str，position [, length]) 从str的position开始[从1开始计算] ,取length个字符
SELECT SUBSTRING(ename,2,2) FROM emp #从第二个开始获取两个字符 注意最后一个参数不给，
														# 选取结尾所有

-- LENGTH (string )    string长度[按照字节]
SELECT LENGTH (ename) FROM emp

-- REPLACE (str ,search_ str,replace__str )     在str中用replace_ str 替换search_ str
SELECT REPLACE('balance','ance','high') FROM DUAL; # balhigh

-- STRCMP (string1 ,string2 )    逐字符比较两字串大小，
SELECT STRCMP('balance','ance') FROM DUAL;  #返回1,-1,0

-- LTRIM (string2 ) RTRIM (string2 )    去除左端空格或右端空格
-- trim 去除左端和右端空格
#去除左端空格
SELECT LTRIM('  张无忌学java') FROM DUAL
#去除右端空格
SELECT RTRIM('  张无忌学java   ') FROM DUAL

SELECT TRIM(' 张无忌学java ') FROM DUAL
#细节1:即使没有空格也不会报错
#注意:函数后面不要带空格再接括号
```

以首字母小写的方式显示所有员工emp表的姓名

```sql
# 截取拼接-left right
SELECT CONCAT(LCASE(LEFT(ename,1)),RIGHT(ename,LENGTH(ename)-1)) FROM emp;
# 截取拼接-substring
SELECT CONCAT(LCASE(SUBSTRING(ename,1,1)),SUBSTRING(ename,2)) FROM emp;
```

### 数学函数

| 函数                               | 功能                             |
| ---------------------------------- | -------------------------------- |
| CEIL(x)                            | 向上取整                         |
| FLOOR(x)                           | 向下取整                         |
| MOD(x, y)                          | 返回x/y的模                      |
| RAND()                             | 返回0~1内的随机数                |
| ROUND(x, y)                        | 求参数x的四舍五入值，保留y位小数 |
| ABS(num)                           | 绝对值                           |
| BIN (decimal number)               | 十进制转二进制                   |
| FORMAT (number,decimal _places )   | 保留小数位数                     |
| CONV( number2,from_ base,to_ base) | 进制转换                         |
| HEX (DecimalNumber)                | 转十六进制                       |
| LEAST (number , number2…)          | 求最小值                         |

```sql
-- 数学函数
-- ABS(num)        绝对值
SELECT ABS(-10) FROM DUAL #10
-- BIN (decimal_ number )    十进制转二进制
SELECT BIN(20) FROM DUAL #10100

-- CEILING (number2 )    向上取整，得到比num2大的最小整数
SELECT CEILING(1.3) FROM DUAL #2

-- CONV( number2,from_ base,to_ bas)    进制转换
#把12从10进制转成8进制
SELECT CONV(12,10,8) FROM DUAL #14

-- FLOOR (number2 )    向下取整，得到比num2小的最大整数
SELECT FLOOR(-1.2) FROM DUAL #-2

-- FORMAT (number,decimal_ places )    保留小数位数(四舍五入)
SELECT FORMAT(12.3456,2) FROM DUAL #把12.3456保留两位小数 12.34

-- HEX (DecimalNumber )        转十六进制
SELECT HEX(22) FROM DUAL #16
-- LEAST (number，number2 [..])    求最小值
SELECT LEAST(2,5,1,6) FROM DUAL #1
-- MOD (numerator ,denominator )    求余
#10对3取余,得到1
SELECT MOD(10,3) FROM DUAL

-- RAND( [seed] )    RAND([seed])    其范围为0≤v≤1.0

SELECT RAND() FROM DUAL #随机生成0-1之间的小数,再次运行会改变
SELECT RAND(10) FROM DUAL #放了种子之后生成随机数之后再运行不会改变,要重新切换种子
```

### 时间函数

| 函数                               | 功能                                              |
| ---------------------------------- | ------------------------------------------------- |
| CURRENT_DATE ( )                   | 当前日期                                          |
| CURRENT_TIME ( )                   | 当前时间                                          |
| DATE(datetime)                     | 返回datetime的日期部分                            |
| DATE_ADD(date, INTERVAL expr type) | 返回一个日期/时间值加上一个时间间隔expr后的时间值 |
| DATE_SUB(date，INTERVAL expr type) | 在date上减去一个时间                              |
| DATEDIFF(date1, date2)             | 返回起始时间date1和结束时间date2之间的天数        |
| TIMEDIFF(date1,date2)              | 两个时间差(多少小时多少分钟多少秒)                |
| NOW()                              | 返回当前日期和时间                                |
| YEAR(date)                         | 获取指定date的年份                                |
| MONTH(date)                        | 获取指定date的月份                                |
| DAY(date)                          | 获取指定date的日期                                |
| unix_timestamp()                   | 返回的是1970-1-1到现在的秒数                      |
| FROM_UNIXTIME()                    | 可以把一个 秒数[时间戳],转换成指定格式的日期      |

上面函数的细节说明:

- DATE ADD()中的interval 后面可以是 year minute second day等

- DATE SUB()中的interval 后面可以是 year minute second hour day等

- DATEDIFF(date1,date2) 得到的是天数，而且是date1-date2 的天数，因此可以取负数

- 这四个函数(还有一个Date)的日期类型可以是date, datetime或者timestamp

```sql
-- 日期时间相关函数
-- CURRENT_DATE() 当前日期 括号可以有也可以没有
SELECT CURRENT_DATE FROM DUAL #2023-02-04

-- CURRENT_TIME() 当前时间
SELECT CURRENT_TIME FROM DUAL #12:03:19

-- CURRENT_TIMESTAMP()当前时间戳
SELECT CURRENT_TIMESTAMP FROM DUAL #2023-02-04 12:03:57

-- DATE(datetime)返回datetime的日期部分
SELECT DATE(CURRENT_TIMESTAMP) FROM DUAL #2023-02-04

-- 在date2加上日期或者时间
#在当前日期加上了10天
SELECT DATE_ADD(CURRENT_DATE,INTERVAL 10 DAY) FROM DUAL #2023-02-14

-- 在date2减去日期或者时间
#在当前时间减去10分钟
SELECT DATE_SUB(CURRENT_TIME,INTERVAL 10 MINUTE) FROM DUAL #12:02:45

-- DATEDIFF(date1,date2)两个日期相差的天数
#是date1减去date2得到结果
SELECT DATEDIFF(CURRENT_DATE,DATE_ADD(CURRENT_DATE,INTERVAL 10 DAY)) FROM DUAL #-10

-- 两个时间差,精确到时分秒
SELECT TIMEDIFF(CURRENT_TIME,DATE_SUB(CURRENT_TIME,INTERVAL 10 MINUTE)) FROM DUAL #00:10:00

-- now() 当前时间
SELECT NOW() FROM DUAL #2023-02-04 12:18:11

-- YEAR MONTH DATE(DATETIME) 获取年月日
#获取NOW()的月份
SELECT MONTH (NOW()) FROM DUAL

 -- 如果你能活80岁，求出你还能活多少天. [练习]
#先求出活到80岁是什么日期
SELECT DATE_ADD('1999-8-8',INTERVAL 80 YEAR) FROM DUAL
#再用80岁的日期减去现在的日期就是还能活的天数
SELECT DATEDIFF(DATE_ADD('1999-8-8',INTERVAL 80 YEAR),NOW()) FROM DUAL #20639

-- unix_timestamp() : 返回的是1970-1-1到现在的秒数
SELECT UNIX_TIMESTAMP() FROM DUAL; #1675524073

-- FROM_UNIXTIME() :可以把一个 秒数[时间戳],转换成指定格式的日期
#细节:指定格式为 %Y-%m-%d %H:%m:%s ,这里就是年月日时分秒的格式
#意义:在开发中,可以存放一个整数,然后表示时间,通过它来转换
SELECT FROM_UNIXTIME(1675524073) FROM DUAL #2023-02-04 23:21:13
```

### 加密和系统函数

| 函数                                  | 功能                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| USER()                                | 查询用户                                                     |
| DATABASE()                            | 查询当前数据库名称                                           |
| MD5(str)                              | 为字符串算出一个MD5，32位的字符串，可用于密码加密            |
| PASSWORD(str)select * from mysql.user | 从原文密码str计算并返回密码字符串，通常用于对mysqI数据库的用户密码加密 |

```sql
-- 加密函数
-- USER()查询用户
SELECT USER() FROM DUAL #root@localhost

-- DATABASE () 当前数据库名称
SELECT DATABASE() FROM DUAL #yjm_db02

-- MD5 (str)为字符串算出一个MD5 32的字符串，(用户密码)加密
SELECT MD5('123456') FROM DUAL #e10adc3949ba59abbe56e057f20f883e
SELECT LENGTH(MD5('123456')) FROM DUAL # MD5之后的密码长度为32位
-- PASSWORD (str)  加密函数
#细节:MYSql默认使用PASSWORD (str) 函数对数据库用户进行加密
SELECT PASSWORD('123456') FROM DUAL #*6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9

-- select * from mysql.user \G从原文密码str计算并返回密码字符串,
-- 通常用于对mysql数据用户密码进行加密
SELECT * FROM mysql.user #*6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9
```

### 流程函数

| 函数                                                         | 功能                                                    |
| ------------------------------------------------------------ | ------------------------------------------------------- |
| IF(value, t, f)                                              | 如果value为true，则返回t，否则返回f                     |
| IFNULL(value1, value2)                                       | 如果value1不为空，返回value1，否则返回value2            |
| CASE WHEN [ val1 ] THEN [ res1 ] … ELSE [ default ] END      | 如果val1为true，返回res1，… 否则返回default默认值       |
| CASE [ expr ] WHEN [ val1 ] THEN [ res1 ] … ELSE [ default ] END | 如果expr的值等于val1，返回res1，… 否则返回default默认值 |

```sql
-- IF(expr1,expr2,expr3)如果expr1为True ,则返回expr2否则返回expr3
SELECT IF(NULL,'真','假') FROM DUAL #假

-- IFNULL(expr1,expr2)如果expr1不为空NULL,则返回expr1,否则返回expr2
SELECT IFNULL('有数据','没数据') FROM DUAL #有数据
SELECT IFNULL(NULL,'没数据') FROM DUAL #没数据

-- SELECT CASE WHEN expr1 THEN expr2 WHEN expr3 THEN expr4 ELSE expr5 END; [类似多重分支.]
-- 如果expr1为TRUE,则返回expr2,如果expr3 为t,返回expr4,否则返回expr5
# 写法一
SELECT ename,(CASE
              WHEN job='CLERK' THEN
              '职员'
              WHEN job='MANAGER' THEN
              '经理'
              WHEN job='SALESMAN' THEN
              '销售人员'	
              ELSE
              job
              END) AS 'job'
FROM `emp`
# 写法二 写法二更直观，贴近java中的switch
SELECT ename,(CASE job
              WHEN 'CLERK' THEN
              '职员'
              WHEN 'MANAGER' THEN
              '经理'
              WHEN 'SALESMAN' THEN
              '销售人员'	
              ELSE
              job
              END) AS 'job'
 FROM `emp`
```

