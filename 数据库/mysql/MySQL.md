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

## 表的创建

![image-20231026151805639](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202310261518054.png)

```sql
CREATE TABLE `user`(
	id INT,
	userName VARCHAR(255),
	`password` VARCHAR(255),
	birthday DATE
)CHARACTER SET utf8mb3 COLLATE utf8mb3_bin ENGINE INNODB;
```

