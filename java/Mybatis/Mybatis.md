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



