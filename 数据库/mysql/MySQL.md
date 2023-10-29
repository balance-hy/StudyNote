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

## 创建表

![image-20231026151805639](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202310261518054.png)

```sql
CREATE TABLE `user`(
	id INT,# id INT UNSIGNED, 无符号
	userName VARCHAR(255),
	`password` VARCHAR(255),
	birthday DATE
)CHARACTER SET utf8mb3 COLLATE utf8mb3_bin ENGINE INNODB;
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

