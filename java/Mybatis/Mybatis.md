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

## CRUD

### namespace

namespace中的包名要和Dao/Mapper 接口的包名一致！

1个Dao接口类对应1个mapper，也对应1个namespace，

1个Dao接口中的方法对应1个namespace中一个SQL语句

### 增删改查

- id：对应的namespace接口中的方法名
- resultType：SQL语句执行的返回值——承载数据的实体类
- parameterType：参数类型

#### 编写接口

```java
public interface UserMapper {
    List<User> getUserList();

    //增加用户
    int addUser(User user);

    //修改用户信息
    int updateUser(User user);

    //删除用户
    int deleteUser(int id);
}
```

**注意**：接口方法**默认是用public abstract修饰**，可省略

这里增删改会返回数据表中受影响的行数，故可以用int接收，若大于0，代表操作成功

#### 编写对应mapper.xml

```xml
<mapper namespace="com.balance.dao.UserMapper">
    <select id="getUserList" resultType="com.balance.pojo.User">
        select * from jdbc01.users
    </select>

    <update id="updateUser" parameterType="com.balance.pojo.User">
        UPDATE jdbc01.users SET name=#{name} WHERE id=#{id}
    </update>

    <insert id="addUser" parameterType="com.balance.pojo.User">
        INSERT INTO jdbc01.users VALUES(#{id},#{name},#{password},#{email},#{birthday})
    </insert>

    <delete id="deleteUser" parameterType="int">
        DELETE FROM jdbc01.users WHERE id=#{id}
    </delete>
</mapper>
```

**注意**：#{}为占位符，里面填接口中具体方法的参数名，**若参数为实体类，可以直接写字段名称,若参数为map，直接写键名**

#### 测试

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


    @Test
    public void add(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        int hhh6 = mapper.addUser(new User(6, "hhh6", "123456", "hhh6@qq.com",new Date(Instant.now().toEpochMilli())));
        if(hhh6>0){
            System.out.println("插入用户成功");
            sqlSession.commit();
        }
        sqlSession.close();
    }

    @Test
    public void delete(){
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        int i = mapper.deleteUser(6);
        if(i>0){
            System.out.println("删除用户成功");
            sqlSession.commit();
        }
        sqlSession.close();
    }

    @Test
    public void update() throws ParseException {
        SqlSession sqlSession = MybatisUtils.getSqlSession();
        UserMapper mapper = sqlSession.getMapper(UserMapper.class);
        long time = new SimpleDateFormat("yyyy-MM-dd").parse("2000-08-16").getTime();
        int balance777 = mapper.updateUser(new User(1,"balance777","123456","balance@qq.com",new Date(time)));
        if(balance777>0){
            System.out.println("修改成功");
            sqlSession.commit();
        }
        sqlSession.close();
    }
}
```

#### 几个注意事项

- 当增删改时，必须使用`sqlSession.commit()`提交事务，否则没有真正影响数据库。
- 当字段有`Date`类型时，`java.sql.date`和`java.util.date`是不一样的,因为实体类为`java.sql.date`在构造时需要接收一个long类型的参数。
  - 方式一使用`Instant.now().toEpochMilli()`这会返回一个`long`类型的值，表示自1970年1月1日午夜（GMT）以来的毫秒数。
  - 方式二使用`date.getTime()`方法用于获取`Date`对象表示的时间点（以毫秒为单位）自1970年1月1日午夜（GMT）以来的时间戳。

#### 万能的Map

刚才更新用户(其他时候也一样，比如插入，查找)的时候就引出了一个问题，当我们只想更新User中的几个字段时，我们需要构造出全部的属性，这显然很麻烦也很冗余。

可以设置parameterType=Map来解决这个问题，在map中封装好要用的字段，直接传递使用即可，示例如下

UserMapper

```java
int updateUser2(Map<String,Object> map);
```

UserMapper.xml

```xml
<update id="updateUser2" parameterType="map">
        UPDATE jdbc01.users SET name=#{name} WHERE id=#{id}
</update>
```

test

```java
@Test
public void update2() {
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    HashMap<String, Object> map = new HashMap<>();
    map.put("id",1);
    map.put("name","balance999");
    int balance777 = mapper.updateUser2(map);
    if(balance777>0){
        System.out.println("修改成功");
        sqlSession.commit();
    }
    sqlSession.close();
}
```

#### 模糊查询

在sql查询时，往往需要使用模糊查询，具体语句如下

```sql
SELECT * FROM users where name like '%h%'
```

这里推荐使用单引号。

那如何在java中进行模糊查询呢？

**方式一，sql语句中拼接,不推荐，有sql注入问题**

```java
List<User> getLikeUser(String name);
public void test2(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    List<User> userList = mapper.getLikeUser("h");
    for (User user : userList) {
        System.out.println(user);
    }
    sqlSession.close();
}
```

```xml
<select id="getLikeUser" parameterType="String" resultType="com.balance.pojo.User">
    select * from jdbc01.users where name like "%"#{name}"%"
</select>
```

**注意这里是双引号**

**方式二，直接写在传递的参数中**

```java
List<User> userList = mapper.getLikeUser("%h%");

<select id="getLikeUser" parameterType="String" resultType="com.balance.pojo.User">
    select * from jdbc01.users where name like #{name}
</select>
```

## Mybatis配置解析

### 核心配置 mybatis-config.xml

MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。 配置文档的顶层结构如下：

- configuration（配置）
  - [properties（属性）](https://mybatis.org/mybatis-3/zh/configuration.html#properties)
  - [settings（设置）](https://mybatis.org/mybatis-3/zh/configuration.html#settings)
  - [typeAliases（类型别名）](https://mybatis.org/mybatis-3/zh/configuration.html#typeAliases)
  - [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
  - [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
  - [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
  - environments（环境配置）
    - environment（环境变量）
      - transactionManager（事务管理器）
      - dataSource（数据源）
  - [databaseIdProvider（数据库厂商标识）](https://mybatis.org/mybatis-3/zh/configuration.html#databaseIdProvider)
  - [mappers（映射器）](https://mybatis.org/mybatis-3/zh/configuration.html#mappers)

**注意，标签书写必须按序，xml中标签是有序的。**

### 环境配置

- MyBatis 可以配置成适应多种环境，根据id识别不同环境

- 尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。

- 学会配置多套运行环境-----更改id

```xml
<environments default="id">
```

- Mybatis默认的事务管理器就是JDBC，连接池：POOLED
- 连接池：使并发 Web 应用快速响应请求

```xml
<!--默认使用的环境 ID（比如：default="development"）-->
<environments default="development">
    <!--每个 environment 元素定义的环境 ID（比如：id="development"）-->
    <environment id="development">
        <!--事务管理器的配置 type="JDBC(默认)|MANAGED"）-->
        <transactionManager type="JDBC"/>
        <!--数据源的配置 type="POOLED(默认)|UNPOOLED|JDN"）连接池：使并发 Web 应用快速响应请求-->
        <dataSource type="POOLED">
            <!--driver – 这是 JDBC 驱动的 Java 类全限定名（并不是 JDBC 驱动中可能包含的数据源类）-->
            <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
            <!--url – 这是数据库的 JDBC URL 地址。 &amp;表示转义后的&-->
            <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=Asia/Shanghai"/>
            <!--username – 登录数据库的用户名-->
            <property name="username" value="root"/>
            <!--password – 登录数据库的密码-->
            <property name="password" value="niit1234"/>
        </dataSource>
    </environment>
</environments>
```

- 事务管理器`<transactionManager type="[ JDBC | MANAGED ]"/>`

- 有三种内建的数据源类型` <dataSource type="[UNPOOLED|POOLED|JNDI]">`

  - unpooled： 这个数据源的实现只是每次被请求时打开和关闭连接。

  - pooled： 这种数据源的实现利用“池”的概念将 JDBC 连接对象组织起来 , 这是一种使得
    并发 Web 应用快速响应请求的流行处理方式。
  - jndi：这个数据源的实现是为了能在如 Spring 或应用服务器这类容器中使用，容器可以集中或在外部配置数据源，然后放置一个 JNDI 上下文的引用。

  - 数据源也有很多第三方的实现，比如dbcp，c3p0，druid等等…

### 属性优化

- 数据库这些属性都是可外部配置且可动态替换的

- 我们可以通过properties属性来实现引用属性文件

- 这些属性可以在外部进行配置，并可以进行动态替换。你既可以在典型的 Java 属性文件【db.properties】中配置这些属性，也可以在 properties 元素的子元素中设置

可以新建db.properties作为属性的配置

```
driver=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/jdbc01?useSSL=true&characterEncoding=UTF-8&useUnicode=true&serverTimezone=GMT
username=root
password=
```

**注意，url中`amp;`需删除，因为这个转义字符在Properties中不需要**

然后重新编写 mybatis-config.xml

```xml
<!--核心配置文件-->
<configuration>
    <!--外部属性配置文件映射-->
    <properties resource="db.properties"/>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="com/balance/dao/UserMapper.xml"/>
    </mappers>
</configuration>
```

属性中具体值通过`${}`来取

也可以在mybatis-config.xml的properties标签中写配置，注意外部配置（db.properties）优先级高，其次才是properties标签中的配置

```xml
<properties resource="db.properties">
    <property name="username" value="root"/>
    <property name="password" value="niit12"/>
</properties>
```

也可以在 SqlSessionFactoryBuilder.build() 方法中传入属性值。例如：

```java
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, props);

// ... 或者 ...

SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment, props);
```

**通过方法参数传递的属性具有最高优先级，resource/url 属性中指定的配置文件次之，最低优先级的则是 properties 标签中指定的属性**

###  别名优化

- 类型别名可为 Java 类型设置一个缩写名字。
- 它仅用于 XML 配置，意在降低冗余的全限定类名书写。

#### 方法一：`<typeAlias>`标签

```xml
<!--给实体类起别名-->
<typeAliases>
    <typeAlias type="com.balance.pojo.User" alias="User"/>
</typeAliases>
```

#### 方法二：`<package>`标签

```xml
<!--给实体类起别名-->
<typeAliases>
    <package name="com.balance.pojo"/>
</typeAliases>
```

即扫描实体类所在的包，包下类的类名就是该类的别名（别名默认首字母小写，大写也行）。

每一个在包 `com.balance.pojo.User` 中的 Java Bean，在没有注解的情况下，会使用 Bean 的首字母小写的非限定类名来作为它的别名。 比如 `com.balance.pojo.Author` 的别名为 `author`；

**若有注解，则别名为其注解值**。如下,Author的别名为hello

```java
@Alias("hello")
public class Author {
    ...
}
```

两种方法区别：

- 实体类比较少的时候，使用第一种方式
- 如果实体类十分多，建议使用第二种
- 第一种可以DIY别名，第二种则不行，如果非要改，需要在实体类上增加注解

#### **Mybatis自带别名**

下面是一些为常见的 Java 类型内建的类型别名。它们都是不区分大小写的，注意，为了应对原始类型的命名重复，采取了特殊的命名风格。（八大基本数据类型前加下划线，对应的包装类小写即可）

| 别名                      | 映射的类型   |
| ------------------------- | ------------ |
| _byte                     | byte         |
| _char (since 3.5.10)      | char         |
| _character (since 3.5.10) | char         |
| _long                     | long         |
| _short                    | short        |
| _int                      | int          |
| _integer                  | int          |
| _double                   | double       |
| _float                    | float        |
| _boolean                  | boolean      |
| string                    | String       |
| byte                      | Byte         |
| char (since 3.5.10)       | Character    |
| character (since 3.5.10)  | Character    |
| long                      | Long         |
| short                     | Short        |
| int                       | Integer      |
| integer                   | Integer      |
| double                    | Double       |
| float                     | Float        |
| boolean                   | Boolean      |
| date                      | Date         |
| decimal                   | BigDecimal   |
| bigdecimal                | BigDecimal   |
| biginteger                | BigInteger   |
| object                    | Object       |
| date[]                    | Date[]       |
| decimal[]                 | BigDecimal[] |
| bigdecimal[]              | BigDecimal[] |
| biginteger[]              | BigInteger[] |
| object[]                  | Object[]     |
| map                       | Map          |
| hashmap                   | HashMap      |
| list                      | List         |
| arraylist                 | ArrayList    |
| collection                | Collection   |
| iterator                  | Iterator     |

### 设置 settings标签

这是 MyBatis 中极为重要的调整设置，它们会改变 MyBatis 的运行时行为。 下表描述了设置中各项设置的含义、默认值等。（部分重要设置，官网有完整版）

| 设置名                   | 描述                                                         | 有效值                                                       | 默认值 |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------ |
| cacheEnabled             | 全局性地开启或关闭所有映射器配置文件中已配置的任何缓存。     | true \| false                                                | true   |
| lazyLoadingEnabled       | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 `fetchType` 属性来覆盖该项的开关状态。 | true \| false                                                | false  |
| mapUnderscoreToCamelCase | 是否开启驼峰命名自动映射，即从经典数据库列名 A_COLUMN 映射到经典 Java 属性名 aColumn。 | true \| false                                                | False  |
| logImpl                  | 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。        | SLF4J \| LOG4J（3.5.9 起废弃） \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \| NO_LOGGING | 未设置 |

一个配置完整的 settings 元素的示例如下：

```xml
<settings>
  <setting name="cacheEnabled" value="true"/>
  <setting name="lazyLoadingEnabled" value="true"/>
  <setting name="aggressiveLazyLoading" value="true"/>
  <setting name="multipleResultSetsEnabled" value="true"/>
  <setting name="useColumnLabel" value="true"/>
  <setting name="useGeneratedKeys" value="false"/>
  <setting name="autoMappingBehavior" value="PARTIAL"/>
  <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
  <setting name="defaultExecutorType" value="SIMPLE"/>
  <setting name="defaultStatementTimeout" value="25"/>
  <setting name="defaultFetchSize" value="100"/>
  <setting name="safeRowBoundsEnabled" value="false"/>
  <setting name="safeResultHandlerEnabled" value="true"/>
  <setting name="mapUnderscoreToCamelCase" value="false"/>
  <setting name="localCacheScope" value="SESSION"/>
  <setting name="jdbcTypeForNull" value="OTHER"/>
  <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
  <setting name="defaultScriptingLanguage" value="org.apache.ibatis.scripting.xmltags.XMLLanguageDriver"/>
  <setting name="defaultEnumTypeHandler" value="org.apache.ibatis.type.EnumTypeHandler"/>
  <setting name="callSettersOnNulls" value="false"/>
  <setting name="returnInstanceForEmptyRow" value="false"/>
  <setting name="logPrefix" value="exampleLogPreFix_"/>
  <setting name="logImpl" value="SLF4J | LOG4J | LOG4J2 | JDK_LOGGING | COMMONS_LOGGING | STDOUT_LOGGING | NO_LOGGING"/>
  <setting name="proxyFactory" value="CGLIB | JAVASSIST"/>
  <setting name="vfsImpl" value="org.mybatis.example.YourselfVfsImpl"/>
  <setting name="useActualParamName" value="true"/>
  <setting name="configurationFactory" value="org.mybatis.example.ConfigurationFactory"/>
</settings>
```

### 其他配置

- [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
- [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
- plugins（常用插件如下）
  - mybatis-generator-core
  - mybatis-plus
  - 通用mapper

### 映射 mappers标签

MapperRegistry：注册绑定我们的Mapper文件，每写一个mapper接口就要写一个Mapper文件

- 映射器 : 定义映射SQL语句文件，既然 MyBatis 的行为其他元素已经配置完了，我们现在就要定义 SQL 映射语句了。但是首先我们需要告诉 MyBatis 到哪里去找到这些语句。Java 在自动查找这方面没有提供一个很好的方法，所以最佳的方式是告诉 MyBatis 到哪里去找映射文件。

- 1张表——1个Entity类——1个EntityDao/EntityMapper接口——1个Mapper.xml——核心配置文件中的1个
- Dao/Maapper接口中的1个方法——Mapper.xml中的1个sql语句

#### 方法一：使用`resource属性`

使用相对于类路径的资源引用（推荐）

```xml
<!--每一个Mapper.xml都需要在MyBatis核心配置文件中注册-->    
<mappers>
    <mapper resource="com/balance/dao/UserMapper.xml"/>
</mappers>
```

#### 方法二：使用`class属性`

使用映射器接口实现类的完全限定类名

- 接口和它的Mapper配置文件必须同名  
- 接口和它的Mapper配置文件必须在同一个包下

```xml
<mappers>
    <!--<mapper class="com.balance.dao.UserMapper"/>--><!--报错-->
    <mapper class="com.balance.dao.UserMapper"/>
</mappers>
```

#### 方法三：`<package>元素`

将包内的映射器接口实现全部注册为映射器

- 接口和它的Mapper配置文件必须同名
- 接口和它的Mapper配置文件必须在同一个包下

```xml
<mappers>
  <package name="com.balance.dao"/>
</mappers>
```

#### 方式4：`url属性`(不常用)

```xml
<!-- 使用完全限定资源定位符（URL） -->
<mappers>
  <mapper url="file:///var/mappers/AuthorMapper.xml"/>
  <mapper url="file:///var/mappers/BlogMapper.xml"/>
  <mapper url="file:///var/mappers/PostMapper.xml"/>
</mappers>
```

### 生命周期和作用域

![image-20231213165706378](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202312131657858.png)

不同作用域和生命周期类别是至关重要的，因为错误的使用会导致非常严重的并发问题。

- SqlSessionFactoryBuilder

  - 作用：创建 SqlSessionFactory

  - 一旦创建了 SqlSessionFactory，就不再需要它了。

  - SqlSessionFactoryBuilder 实例的最佳作用域是方法作用域（也就是局部方法变量）。

- SqlSessionFactory

  - 可以理解为数据库连接池

  - SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，没有任何理由丢弃它或重新创建另一个实例。

  - 一般的应用中我们往往希望 SqlSessionFactory 作为一个单例，让它在应用中被共享。

  - SqlSessionFactory 的最佳作用域是应用作用域(Application Scope)。

  - 最简单的就是使用单例模式或者静态单例模式

- SqlSession（相当于从连接池中获取一个连接）

  - 连接到连接池的一个请求，相当于一个数据库连接（Connnection对象）

  - SqlSession 的实例不是线程安全的，因此是不能被共享的，它的最佳的作用域是请求或方法作用域

  - 用完之后需要赶紧关闭，否则资源被占用。用 try…catch…finally… 语句来保证其正确关闭。

  - 可以在一个事务里面执行多条 SQL，然后通过它的 commit、rollback 等方法，提交或者回滚事务。

  - 一个 SqlSession 可以多次使用它的 getMapper 方法，获取多个 mapper 接口实例

- 打个比方：

  - SqlSessionFactoryBuilder 是造车公司

  - 造了 100 台车，然后卖给租车公司（SQLSessionFactory），然后倒闭

  - SQLSession 用户 租车，使用车

  - mapper 用户的使用，用户租到车之后可以开去这，开去那，任凭使用

  - 用户（SQLSession） 执行完想做的事之后，必须归还 “汽车” 给租车公司（SQLSessionFactory）

![image-20231213165821662](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202312131658864.png)

这里面的每一个 mapper 就代表一个具体的业务。

## 解决属性名和字段名不一致的问题

现在我们数据库中表的字段和实体类的属性是一一对应的

```java
private int id;
private String name;
private String password;
private String email;
private Date birthday;
```

假如不对应怎么办，比如 password 在实体类中是 pwd

这样查出来的结果是

```
User{id=1, name='balance1111', pwd='null', email='balance@qq.com', birthday=2000-01-01}
```

可以看到pwd为null，这肯定是错误的，如何解决呢？

**一种直接的想法是起别名，更改sql语句**

比如写成

```sql
select id,name,password as pwd,email,birthday from jdbc01.users where id =#{id}
```

但如果太多不一致，我们写sql语句都要考虑这些太麻烦，且容易出错

**Mybatis提供结果集映射来帮助我们解决这一问题：**

在UserMapper.xml的元素中添加

```xml
<!--结果集映射-->
<resultMap id="UserMap" type="User">
    <!--column:数据库中的字段;property:实体类中的属性-->
    <!--<result column="id" property="id"/>-->
    <!--<result column="name" property="name"/>-->
    <result column="pwd" property="password"/>
</resultMap>


```

我们只需要事先**定义好不一致的属性和字段间对应关系**即可。

resultMap 元素是 MyBatis 中最重要最强大的元素

- ResultMap 的设计思想是，对简单的语句做到零配置，对于复杂一点的语句，只需要描述语句之间的关系就行了

- MyBatis 会在幕后自动创建一个 ResultMap，再根据属性名来映射列到 JavaBean 的属性上,这就是**为什么是null的原因**。

- 在之前的学习中resultMap都是被隐式创建的，因此需保证实体类的属性名和数据库的列名或列别名对应相同

- select、update、insert、delete中的属性可以使用 resultType 或 resultMap，但不能同时使用。

所以UserMapper.xml可以如下书写

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.balance.dao.UserMapper">
    <resultMap id="UserMap" type="user">
        <result column="password" property="pwd"/>
    </resultMap>
    <!--修改resultType为resultMap（两者不能同时使用）-->
    <select id="getUserById"  parameterType="int" resultMap="UserMap">
        select * from jdbc01.users where id =#{id}
    </select>
</mapper>
```

## 日志

### 日志工厂

如果一个数据库操作出现了异常，我们需要排错。日志就是最好的助手！
曾经：debug、sout
现在：日志工厂

![image-20231219154116857](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202312191541239.png)

```xml
<setting name="logImpl" value="STDOUT_LOGGING"/>
```

value属性只能为以下值：

- SLF4J
- LOG4J 【掌握】
- LOG4J2
- JDK_LOGGING
- COMMONS_LOGGING
- STDOUT_LOGGING【掌握】
- NO_LOGGING

### `STDOUT_LOGGING`—标准日志输出

在核心配置文件中配置日志实现

```xml
<settings>
    <!--标准的日志工厂实现-->
    <setting name="logImpl" value="STDOUT_LOGGING"/>
</settings>
```

![image-20231219154741524](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202312191547597.png)

### Log4j

什么是LOG4J

- Log4j是Apache的一个开源项目

- 通过使用Log4j，我们可以控制日志信息输送的目的地是控制台、文件、GUI组件

- 我们可以控制每一条日志的输出格式；

- 通过定义每一条日志信息的级别，我们能够更加细致地控制日志的生成过程。

- 通过**一个配置文件**来灵活地进行配置，而不需要修改应用的代码。

**导包后**在resource目录下新建log4j.properties配置文件

```xml
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

```properties
#将等级为DEBUG的日志信息输出到console和file这两个目的地，console和file的定义在下面的代码
log4j.rootLogger=DEBUG,console,file

#控制台输出的相关设置
log4j.appender.console = org.apache.log4j.ConsoleAppender
log4j.appender.console.Target = System.out
log4j.appender.console.Threshold=DEBUG
log4j.appender.console.layout = org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=【%c】-%m%n

#文件输出的相关设置
log4j.appender.file = org.apache.log4j.RollingFileAppender
log4j.appender.file.File=./log/balance.log
log4j.appender.file.MaxFileSize=10mb
log4j.appender.file.Threshold=DEBUG
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=【%p】【%d{yy-MM-dd}】【%c】%m%n

#日志输出级别
log4j.logger.org.mybatis=DEBUG
log4j.logger.java.sql=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.ResultSet=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```

修改mybatis-config.xml

```xml
<settings>
    <setting name="logImpl" value="LOG4J"/>
</settings>
```

运行并测试

![image-20231219161948003](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202312191619109.png)

![image-20231219162035973](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202312191620973.png)

#### 简单使用

1. 在要使用log4j的类中导入Apache的包

   ```java
   import org.apache.log4j.Logger;
   ```

2. 日志对象，参数为当前类的class

   ```java
   static Logger logger = Logger.getLogger(UserDaoTest.class);
   ```

3. 使用日志级别

   ```java
   @Test
   public void testLog4j(){
       //相当与sout，但输出的日志级别不同
       logger.info("info:进入testLog4j");
       logger.debug("debug:进入了testLog4j");
       logger.error("error:进入了testLog4j");
   }
   
   //控制台输出：（日志文件中也会添加以下输出）
   //21:07:25,697  INFO UserDaoTest:63 - info:进入testLog4j
   21:07:25,702 DEBUG UserDaoTest:64 - debug:进入了testLog4j
   21:07:25,702 ERROR UserDaoTest:65 - error:进入了testLog4j
   ```


## 分页

为什么要分页？减少数据的处理量

### 使用Limit分页—通过sql实现

```sql
SELECT * FROM users limit startIndex,pageSize;
SELECT * FROM users limit 0,2;--从第1行开始(包括第1行)显示2条记录，即(1,2]或[2,2]
SELECT * FROM users limit 3; --[0,n]显示前3条记录
```

编写UserMapper接口（方法的参数为Map）

```java
List<User> getUserByLimit(Map<String,Integer> map);
```

xml映射文件

```xml
<select id="getUserByLimit" parameterType="map" resultMap="UserMap">
    select * from users limit #{startIndex},#{pageSize}
</select>
```

测试

```java
@Test
public void getUserLimitTest(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();
    UserMapper mapper = sqlSession.getMapper(UserMapper.class);
    HashMap<String, Integer> map = new HashMap<>();
    map.put("startIndex",0);
    map.put("pageSize",5);
    List<User> userByLimit = mapper.getUserByLimit(map);
    for (User user : userByLimit) {
        System.out.println(user);
    }
    sqlSession.close();
}
```

### RowBounds实现分页—了解

**也就是在java代码中控制分页**

UserMapper接口

```java
List<User> getUserByRowBounds();
```

UserMapper.xml映射文件

```xml
<select id="getUserByRowBounds"  resultMap="UserMap">
    select * from users
</select>
```

测试

```java
@Test
public void getUserLimitByRowBounds(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();

    //RowBounds实现分页
    RowBounds rowBounds = new RowBounds(1, 2);

    List<User>
        userList=sqlSession.selectList("com.balance.dao.UserMapper.getUserByRowBounds",null,rowBounds);
    for (User user : userList) {
        System.out.println(user);
    }
    sqlSession.close();
}
```

### 分页插件【了解】

MyBatis 分页插件 PageHelper

如何使用----[如何使用分页插件](https://pagehelper.github.io/docs/howtouse/)

## 使用注解开发

#### 面向接口编程

我们之前都学过面向对象编程，也学习过接口，但在真正的开发中，很多时候我们会选择面向接口编程

- 根本原因 : 解耦 , 可拓展 , 提高复用 , 分层开发中 , 上层不用管具体的实现 , 大家都遵守共同的标准 , 使得开发变得容易 , 规范性更好

- 在一个面向对象的系统中，系统的各种功能是由许许多多的不同对象协作完成的。在这种情况下，各个对象内部是如何实现自己的,对系统设计人员来讲就不那么重要了；

- 而各个对象之间的协作关系则成为系统设计的关键。小到不同类之间的通信，大到各模块之间的交互，在系统设计之初都是要着重考虑的，这也是系统设计的主要工作内容。面向接口编程就是指按照这种思想来编程。


关于接口的理解

- 接口从更深层次的理解，应是定义（规范，约束）与实现（名实分离的原则）的分离。

- 接口的本身反映了系统设计人员对系统的抽象理解。

- 接口应有两类：

  - 第一类是对一个个体的抽象，它可对应为一个抽象体(abstract class)；

  - 第二类是对一个个体某一方面的抽象，即形成一个抽象面（interface）；

  - 一个体有可能有多个抽象面。抽象体与抽象面是有区别的。


三个面向区别

- 面向对象是指，我们考虑问题时，以对象为单位，考虑它的属性及方法 .

- 面向过程是指，我们考虑问题时，以一个具体的流程（事务过程）为单位，考虑它的实现 .

- 接口设计与非接口设计是针对复用技术而言的，与面向对象（过程）不是一个问题.更多的体现就是对系统整体的架构


#### 注解-简单使用

UserMapper接口

```java
@Select("select * from users")
List<User> getUsers();
```

核心配置文件中绑定接口

```xml
<!--绑定接口-->
<mappers>
    <mapper class="com.balance.dao.UserMapper"/>
</mappers>
```

测试

```java
@Test
public void getUsers(){
    SqlSession sqlSession = MybatisUtils.getSqlSession();

    UserMapper mapper = sqlSession.getMapper(UserMapper.class);

    List<User> userList = mapper.getUsers();

    for (User user : userList) {
        System.out.println(user);
    }

    sqlSession.close();
}
```

本质：反射机制实现

底层：动态代理

#### Mybatis详细的执行流程（分析源码）

![img](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202312201542242.png)

#### 注解-CRUD

我们可以在工具类MybatisUtil中创建sqlSession对象的时候实现自动提交事务！（但实际上不推荐）

```java
public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession(true); //事务自动提交
}
```

接口

```java
public interface UserMapper {
    @Select("select * from users")
    List<User> getUsersAnnotation();
    
    //方法存在多个参数，所有的参数前面都要加上@Param("id")
    @Select("select * from users where id=#{id}")
    List<User> getUsersByIdAnnotation(@Param("id") int id);

    @Update("update users set name=#{name},password=#{pwd} where id=#{id}")
    int updateAnnotation(User user);
    
    @Insert("insert into users(id,name,password,email,birthday) values(#{id},#{name},#{pwd},#{email},#{birthday})")
    int addUserAnnotation(User user);
    
    @Delete("delete from users where id=#{id}")
    int deleteUserAnnotation(@Param("id") int id);
}
```

配置文件绑定Mapper接口

```xml
<mappers>
    <mapper class="com.balance.dao.UserMapper"/>
</mappers>
```

测试代码略

#### `@Param`注解

[MyBatis（十五）：@Param()注解 - 谁知道水烫不烫 - 博客园](https://www.cnblogs.com/jmsstudy/p/16695315.html)

为什么要使用`@Param`注解？因为当传递多个参数时，我们无法使用parameterType去指定参数类型（只能指定一个），此时可以封装成map，但每次去封装map无疑很繁琐，而且不一定需要，此时可以用此注解，我们就不需要去写parameterType了。

简单总结一下：

- sql的#{}中的参数就是@Param(“”)中设置的参数，而不是方法的形参

- 传入单个基本类型或String类型的时候，使用不使用@Param()注解都可以。

- 如果参数是 JavaBean ， 则不能使用@Param。

- 需要传入多个参数时，有三种方法：JavaBean、@Param()注解、Map。个人认为优先使用JavaBean，当需要传入的数据较多但又不能完全将JavaBean利用起来的时候使用@Param()注解或Map。

- 单个基本类型，SQL对应的就是形参的名字；使用JavaBean，SQL中对应的就是JavaBean在结果集映射的属性（没有显性映射就是默认的属性）；使用@Param()，SQL中对应的就是@Param()注解中的名字；使用Map，SQL中对应的就是Map的键名。

- 使用@Param()注解的时候，Mapper.xml文件无需再设置parameterType属性。

## Lombok

Lombok项目是一个**java库**，它可以自动插入到编辑器和构建工具中，增强java的性能。不需要再写getter、setter或equals方法，只要有一个注解，你的类就有一个功能齐全的构建器、自动记录变量等等。

> 是一个在Java开发过程中用注解的方式，简化了 JavaBean(实体类) 的编写，避免了冗余和样板式代码而出现的插件，让编写的类更加简洁。

### 使用步骤

1. 在IDEA中安装lombok插件：File —> Settings —> Plugins —> Browse repositories —> 搜索lombok（**新版idea已集成**）
2. 在pom.xml中导入lombok的maven依赖（包）

3. 在实体类上加注解即可

**常用注解如下：**

- @Data : 自动生成set/get方法，toString方法，equals方法，hashCode方法，不带参数的构造方法

- @NoArgsConstructor/@RequiredArgsConstructor/@AllArgsConstructor
  自动生成无参/有参构造方法

- @Setter/@Getter : 自动生成set和get方法

- @ToString : 自动生成toString方法

- @EqualsAndHashcode : 从对象的字段中生成hashCode和equals方法


**以下为不常用注解：**

- @NonNull : 用在成员方法或者构造方法的参数前面，会自动产生一个关于此参数的非空检查，如果参数为空，则抛出一个空指针异常

- @CleanUp : 自动资源管理：不用再在finally中添加资源的close方法

- @Value : 用于注解final类

- @Builder : 产生复杂的构建器api类

- @SneakyThrows : 异常处理（谨慎使用）

- @Synchronized : 同步方法安全的转化

- @Getter(lazy=true) :

- @Log: 支持各种logger对象，使用时用对应的注解，如：@Log4j

**优点:**

能通过注解的形式自动生成构造器、getter/setter、equals、hashcode、 toString等方法，提高了一定的开发效率

让代码变得简洁，不用过多的去关注相应的方法

属性做修改时，也简化了维护为这些属性所生成的getter/setter方法等

**缺点:**

不支持多种参数构造器的重载

虽然省去了手动创建getter/seter方法的麻烦，但大大降低了源代码的可读性和完整性，降低了阅读源代码的舒适度

## 一对多和多对一查询

### 复杂查询环境搭建

创建示例数据表

```sql
CREATE TABLE `teacher` (
   `id` INT(10) NOT NULL,
   `name` VARCHAR(30) DEFAULT NULL,
   PRIMARY KEY (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;

INSERT INTO teacher(`id`, `name`) VALUES (1, '秦老师');

CREATE TABLE `student` (
   `id` INT(10) NOT NULL,
   `name` VARCHAR(30) DEFAULT NULL,
   `tid` INT(10) DEFAULT NULL,
   PRIMARY KEY (`id`),
   KEY `fktid` (`tid`),
   CONSTRAINT `fktid` FOREIGN KEY (`tid`) REFERENCES `teacher` (`id`)
) ENGINE=INNODB DEFAULT CHARSET=utf8;


INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('1', '小明', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('2', '小红', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('3', '小张', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('4', '小李', '1');
INSERT INTO `student` (`id`, `name`, `tid`) VALUES ('5', '小王', '1');
```

创建pojo类和对应mapper接口

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Teacher {
    private int id;
    private String name;
}

public interface TeacherMapper {
    Teacher getTeacher(@Param("id") int id);
}


@Data
@AllArgsConstructor
@NoArgsConstructor
public class Student {
    private int id;
    private String name;

    //学生需要关联一个老师，注意数据库里是外键的形式，这里是老师类的对象
    private Teacher teacher;
}

public interface StudentMapper {
}
```

注意*Mapper.xml创建其实和mybatis-config.xml格式差不多

如下只需要更改头部两处`!DOCTYPE configuration`中的configuration和`https://mybatis.org/dtd/mybatis-3-config.dtd`中的config，然后改下面的标签即可

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="">

</mapper>

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">
<!--核心配置文件-->
<configuration>
    
</configuration>
```

当Mapper文件过多时，都放在dao层里显然不太合适，在resources目录下新建相同的包名，**注意要一个个建**

![image-20231221162432858](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202312211624443.png)

然后使用时注意在mybatis-config.xml中注册，以TeacherMapper.xml为例**,如果使用resources属性这里的注册也需一个个注册。MyBatis 的 `<mapper>` 元素中的 `resource` 属性不支持通配符**

```xml
<mappers>
    <mapper resource="com/balance/dao/TeacherMapper.xml"/>
</mappers>
```

或者使用package标签，全部注册。

这种写法可以的原因是当程序运行后，文件会被打包生成到target/classes目录下，也就是说源程序中java目录的包会原封不动输出，而resources目录下的文件会直接放在target/classes目录下，此时若java目录中文件路径和resources目录路径相同，就会合并到同一个路径下。

```xml
<mappers>
    <package name="com.balance.dao"/>
</mappers>
```

**通常情况下，推荐将 XML Mapper 文件放置在 resources 目录下，因为这是 Maven 和其他构建工具默认会扫描的位置。**

### 多对一

多对一的理解：

- 多个学生对应一个老师

- 如果对于学生这边，就是一个多对一的现象，即从学生这边**关联**一个老师！


结果映射（resultMap）：

- association

  - 一个复杂类型的关联；许多结果将包装成这种类型
  - 嵌套结果映射 —— 关联可以是 resultMap 元素，或是对其它结果映射的引用
- collection
  - 一个复杂类型的集合
  - 嵌套结果映射 —— 集合可以是 resultMap 元素，或是对其它结果映射的引用

**需求：查所有学生的信息以及其对应的老师姓名**

对应sql语句

```sql
select 
	s.id AS sid ,
	s.name AS sname ,
	t.name AS tname 
from 
	student AS s,
	teacher AS t 
where s.tid=t.id
```

#### 法一 子查询

因为student表中存放的是teacher id，而实体类只能建teacher对象，当多对一时，可以考虑使用嵌套查询，首先查询出所有学生信息，然后定义resultMap，在其中使用association标签，指定实体类属性和表字段名，以及指定实体类属性javaType，然后嵌套一个查询teacher的语句。

**也就是先查一张表，用这张表的字段去查另一张表**

```xml
<select id="getStudent" resultMap="StudentTeacher">
    select * from student
</select>
<resultMap id="StudentTeacher" type="student">
    <!--复杂的属性，需要单独处理——对象：<association>  集合：<collection>-->
    <association property="teacher" javaType="teacher" column="tid" select="getTeacher"/>
</resultMap>
<select id="getTeacher" resultType="teacher">
    select * from teacher where id=#{id}
</select>
```

#### 法二 联表查询 更直接

可以看到法一是使用了两个查询，如果我们就想写原来的sql语句应该怎么办？这时用到联表查询

```xml
<select id="getStudent2" resultMap="StudentTeacher2">
    select s.id as sid, s.name as sname,t.name as tname from student as s,teacher as t where s.tid=t.id
</select>
<resultMap id="StudentTeacher2" type="student">
    <result property="id" column="sid"/>
    <result property="name" column="sname"/>
    <association property="teacher" javaType="teacher">
        <result property="name" column="tname"/>
    </association>
</resultMap>
```

我们从数据库查出三个字段sid，sname，tname，前两个很好对应，但第三个，我们的实体类要求是对象，使用association标签，定义其属性和其javaType，在association标签内用result进行映射。此时association中不需要写column，因为是按照结果去映射。

