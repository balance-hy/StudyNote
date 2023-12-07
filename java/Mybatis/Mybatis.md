# Mybatis

## 简介

### 什么是Mybatis

- MyBatis 是一款优秀的持久层框架，是一个半自动化的 ORM 框架

- 不同于其他的对象关系映射框架，MyBatis 并未将 Java 对象和数据库表关联，而是将 Java 方法与 SQL 语句关联。

- 它支持自定义 SQL、存储过程以及高级映射。

- MyBatis 免除了几乎所有的 JDBC 代码以及设置参数和获取结果集的工作。
  - 对**jdbc的操作数据库的过程进行封装，使开发者只需要关注SQL本身**，而不需要花费精力去处理例如注册驱动、创建connection、创建statement、手动设置参数、结果集检索等jdbc繁杂的过程代码。

- MyBatis 可以通过简单的 XML 或注解来配置和映射原始类型、接口和 Java POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。
  - Mybatis通过xml或注解的方式将要执行的各种statement（statement、preparedStatemnt）配置起来，并通过java对象和statement中的sql进行映射生成最终执行的sql语句，最后由mybatis框架执行sql并将结果映射成java对象并返回。
- **MyBatis 的真正强大在于它的(SQL)语句映射**

### 数据持久化

- ***持久化（Persistence） ：***将程序的数据的在持久状态和瞬时状态间转化的机制。即将数据（如内存中的对象）存储在可持久保存的存储介质上（如数据库、磁盘、文件系统等）
  - “持久化”这个概念是和“暂时”等概念相对的，数据在计算机中有一般有两个存储地，内存为暂存，因为电源关机就会数据丢失，如果需要反复使用，就要持久保存，实现持久化了。

  - 内存：断电即失

  - 数据持久化 : 将内存中的数据模型转换为存储模型,以及将存储模型转换为内存中的数据模型的统称.

    - 数据模型可以是任何数据结构或对象模型；存储模型可以是关系模型、XML、二进制流等。

    - cmp和Hibernate只是对象模型到关系模型之间转换的不同实现。只不过对象模型和关系模型应用广泛，所以就会误认为数据持久化就是对象模型到关系型数据库的转换罢了。

  - 持久化的主要应用是将内存中的对象存储在关系型的数据库中，当然也可以存储在磁盘文件中、XML数据文件中等等。

  - JDBC就是一种持久化机制。文件IO也是一种持久化机制。

  - 生活例子：冷藏、罐头

- **持久化对象 **：是指在程序中表示的对象，可以存储在持久化存储介质（如数据库、文件系统等）中并能够长期保留。简单来说，持久化对象是指在内存中的对象通过某种方式保存到外部存储介质中，并且在需要时可以重新加载到内存中进行使用。
- ***持久层（Persistence Layer）：***专注于实现数据持久化应用领域的某个特定系统的一个逻辑层面，将数据使用者和数据实体相关联。(就是用于完成持久化工作的代码块（dao 层【DAO（Data Access Object）】）)

**为什么需要持久化？**

之所以需要持久化，是由于内存自身缺陷导致。我们知道，内存在遇到某些外界因素影响后会丢失，但是我们的一些数据是绝对不能丢失的，但我们又无法保证不收外界因素影响。

同时内存成本较高，比起硬盘、光盘等外存，其价格要高上几个数量级，而且维持成本也较高。在这种情况下，我们不得不寻求另一种方案来存储数据对象，而持久化就是其中的一种选择，我们能够通过持久化将数据缓存到外存，从而降低成本。
### Mybatis优点

- **简单易学**：自身小且简单，无任何第三方依赖（最简单安装只要两个jar文件+配置几个sql映射文件）；
- **灵活**：MyBatis 不会对应用程序或数据库的现有设计强加任何影响，写在 XML 中，便于统一管理和优化；
- **解除 SQL 与代码程序的耦合**：通过提供 DAO 层，将业务逻辑与数据访问逻辑分离，使系统设计更加清晰、易维护、易于单元测试。sql和代码的分离，提高了程序的可维护性；
- **提供 XML 标签，支持编写动态 SQL；**
- 提供映射标签，支持对象与数据库的ORM字段关系映射。

- 提供对象关系映射标签，支持对象关系组建维护。

## 第一个Mybatis程序

### 创建maven项目

需要删除src目录，主项目不需要，后导入maven依赖

pom.xml

```xml
<!--父工程-->
<dependencies>
    <!--mysql驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.31</version>
    </dependency>
    <!--mybatis-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.13</version>
    </dependency>
    <!--junit-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### 创建module

配置mybatis-config.xml,**注意mysql8.0后driver为com.mysql.cj.jdbc.Driver**

**同时注意时区等问题，具体参考下面代码**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<!--核心配置文件-->
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/jdbc01?useSSL=true&amp;characterEncoding=UTF-8&amp;useUnicode=true&amp;serverTimezone=GMT"/>
                <property name="username" value="root"/>
                <property name="password" value=""/>
            </dataSource>
        </environment>
    </environments>
</configuration>
```

编写Mybatis工具类，之后可以重复使用

```java
public class MybatisUtils {
    private static SqlSessionFactory sqlSessionFactory;
    static {
        String resource = "mybatis-config.xml";
        InputStream inputStream = null;
        try {
            inputStream = Resources.getResourceAsStream(resource);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    }

    //既然有了 SqlSessionFactory，顾名思义，我们可以从中获得 SqlSession 的实例。
    // SqlSession 提供了在数据库执行 SQL 命令所需的所有方法。
    // 你可以通过 SqlSession 实例来直接执行已映射的 SQL 语句。例如：
    public static SqlSession getSqlSession(){
        return sqlSessionFactory.openSession();
    }
}
```

![image-20231207183644883](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202312071836435.png)

### 编写代码

#### 1.编写实体类

新建pojo包，包下建User类

**这里注意和数据库中字段一一对应，并给出无参，有参构造，重写toString方法，这里贴的代码略去了**

```java
public class User {
    private int id;
    private String name;
    private String password;
    private String email;
    private Date birthday;
}
```

#### 2.Dao接口，即Mapper接口

Data Access Object，数据存取对象，所谓Dao层就是封装对数据库的访问。

UserMapper.xml

```java
public interface UserMapper {
    public List<User> getUserList();
}
```

#### 3.编写UserMapper.xml完成与接口的映射

**注意：这里的 Id 就是你接口中定义的方法名，这里的 resultType 就是你编写的与数据库对应的实体类，jdbc01是数据库名，users是表名**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.balance.dao.UserMapper">
    <select id="getUserList" resultType="com.balance.pojo.User">
        select * from jdbc01.users
    </select>
</mapper>
```

#### 4.在mybatis-config.xml注册UserMapper.xml

```xml
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/jdbc01?useSSL=true&amp;characterEncoding=UTF-8&amp;useUnicode=true&amp;serverTimezone=GMT"/>
                <property name="username" value="root"/>
                <property name="password" value=""/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/balance/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```

#### 5.测试

先通过MybatisUtils工具类获取到sqlSession对象，利用sqlSession对象的getMapper方法获得UserMapper接口对象，之后调用方法完成查找。

**注意：不要忘记关闭sqlSession**

```java
public class UserDaoTest {
    @Test
    public void test(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        List<User> userList = mapper.getUserList();
        for (User user : userList) {
            System.out.println(user);
        }

        sqlSession.close();
    }
}
```

![image-20231207184746806](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202312071847835.png)

#### 注意事项

有时候我们的xml并没有写在resources目录下，导致资源无法导出，可以在pom.xml中加上

```xml
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```

还可以设置一下编码和默认项目SQL，防止奇怪的问题

![image-20231207185233325](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202312071852185.png)

![image-20231207185145669](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202312071851735.png)

