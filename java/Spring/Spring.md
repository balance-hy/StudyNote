# Spring

> https://www.kuangstudy.com/course?cid=1
>
> https://www.cnblogs.com/renxuw/p/12994080.html
>
> https://docs.spring.io/spring-framework/docs/5.3.31/reference/html/overview.html#overview

## 简介

> SpringFramework 5.3
>
> https://docs.spring.io/spring-framework/docs/5.3.31/reference/html/
>
> 看官网，spring 5.3 依旧支持jdk8 ，再向上不支持了，导包也注意一下导springmvc 5.3.31

Spring框架是由于软件开发的复杂性而创建的。Spring使用的是基本的[JavaBean](https://baike.baidu.com/item/JavaBean/529577?fromModule=lemma_inlink)来完成以前只可能由[EJB](https://baike.baidu.com/item/EJB/144195?fromModule=lemma_inlink)完成的事情。然而，Spring的用途不仅仅限于[服务器端](https://baike.baidu.com/item/服务器端/3369401?fromModule=lemma_inlink)的开发。从简单性、[可测试性](https://baike.baidu.com/item/可测试性/1459715?fromModule=lemma_inlink)和松[耦合性](https://baike.baidu.com/item/耦合性/4297612?fromModule=lemma_inlink)角度而言，绝大部分[Java](https://baike.baidu.com/item/Java/85979?fromModule=lemma_inlink)应用都可以从Spring中受益。

**Spring的初衷：**

* JAVA EE开发应该更加简单。

* 使用接口而不是使用类，是更好的编程习惯。Spring将使用接口的复杂度几乎降低到了零。

* 为JavaBean提供了一个更好的应用配置框架。

* 更多地强调面向对象的设计，而不是现行的技术如JAVA EE。

* 尽量减少不必要的异常捕捉。

* 使应用程序更加容易测试。

**Spring的目标：**

* 可以令人方便愉快的使用Spring。

* 应用程序代码并不依赖于Spring APIs。

* Spring不和现有的解决方案竞争，而是致力于将它们融合在一起。

**Spring的基本组成：**

* 最完善的轻量级核心框架。

* 通用的事务管理抽象层。

* JDBC抽象层。

* 集成了Toplink, Hibernate, JDO, and iBATIS SQL Maps。

* AOP功能。

* 灵活的MVC Web应用框架。

### 优点

1、Spring是一个开源免费的框架 , 容器 .

2、Spring是一个轻量级的框架 , 非侵入式的 .

**3、控制反转 IoC , 面向切面 Aop**

4、对事物的支持 , 对框架的支持

.......

一句话概括：

**Spring是一个轻量级的控制反转(IoC)和面向切面(AOP)的容器（框架）。**

==IoC：Inversion of Control==  ==AOP：Aspect Oriented Programming==

### 组成

Spring 框架是一个分层架构，由 7 个定义良好的模块组成。Spring 模块构建在核心容器之上，核心容器定义了创建、配置和管理 bean 的方式 .

![img](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202401071521619.png)

组成 Spring 框架的每个模块（或组件）都可以单独存在，或者与其他一个或多个模块联合实现。每个模块的功能如下：

- **核心容器**：核心容器提供 Spring 框架的基本功能。核心容器的主要组件是 BeanFactory，它是工厂模式的实现。BeanFactory 使用*控制反转*（IOC） 模式将应用程序的配置和依赖性规范与实际的应用程序代码分开。
- **Spring 上下文**：Spring 上下文是一个配置文件，向 Spring 框架提供上下文信息。Spring 上下文包括企业服务，例如 JNDI、EJB、电子邮件、国际化、校验和调度功能。
- **Spring AOP**：通过配置管理特性，Spring AOP 模块直接将面向切面的编程功能 , 集成到了 Spring 框架中。所以，可以很容易地使 Spring 框架管理任何支持 AOP的对象。Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中。
- **Spring DAO**：JDBC DAO 抽象层提供了有意义的异常层次结构，可用该结构来管理异常处理和不同数据库供应商抛出的错误消息。异常层次结构简化了错误处理，并且极大地降低了需要编写的异常代码数量（例如打开和关闭连接）。Spring DAO 的面向 JDBC 的异常遵从通用的 DAO 异常层次结构。
- **Spring ORM**：Spring 框架插入了若干个 ORM 框架，从而提供了 ORM 的对象关系工具，其中包括 JDO、Hibernate 和 iBatis SQL Map。所有这些都遵从 Spring 的通用事务和 DAO 异常层次结构。
- **Spring Web 模块**：Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文。所以，Spring 框架支持与 Jakarta Struts 的集成。Web 模块还简化了处理多部分请求以及将请求参数绑定到域对象的工作。
- **Spring MVC 框架**：MVC 框架是一个全功能的构建 Web 应用程序的 MVC 实现。通过策略接口，MVC 框架变成为高度可配置的，MVC 容纳了大量视图技术，其中包括 JSP、Velocity、Tiles、iText 和 POI。

### 拓展

![image-20240107152345871](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202401071523019.png)

**Spring Boot与Spring Cloud**

- Spring Boot 
  - 是 Spring 的一套快速配置脚手架，
  - 可以基于Spring Boot 快速开发单个微服务;
  - 约定大于配置
- Spring Cloud
  - 基于Spring Boot实现的；

Spring Boot专注于快速、方便集成的单个微服务个体，Spring Cloud关注全局的服务治理框架；

SpringBoot在SpringClound中起到了承上启下的作用，**如果你要学习SpringCloud必须要学习SpringBoot。**

Spring Boot使用了**约定优于配置的理念**，很多集成方案已经帮你选择好了，能不配置就不配置 , Spring Cloud很大的一部分是**基于Spring Boot来实现**，**Spring Boot可以离开Spring Cloud独立使用开发项目**，**但是Spring Cloud离不开Spring Boot，属于依赖的关系**。

## IOC

### 原型

新建一个空白的maven项目

我们先用我们原来的方式写一段代码 .

1、先写一个UserDao接口

```java
public interface UserDao {
   public void getUser();
}
```

2、再去写Dao的实现类

```java
public class UserDaoImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("获取用户数据");
  }
}
```

3、然后去写UserService的接口

```java
public interface UserService {
   public void getUser();
}
```

4、最后写Service的实现类

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao = new UserDaoImpl();

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

5、测试一下`(这里每次更换不同接口都需要在service层更改new的接口)`

```java
@Test
public void test(){
   UserService service = new UserServiceImpl();
   service.getUser();
}
```

这是我们原来的方式 , 开始大家也都是这么去写的对吧 . 那我们现在修改一下 .

把Userdao的实现类增加一个 .

```java
public class UserDaoMySqlImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("MySql获取用户数据");
  }
}
```

紧接着我们要去使用MySql的话 , 我们就需要去service实现类里面修改对应的实现

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao = new UserDaoMySqlImpl();

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

在假设, 我们再增加一个Userdao的实现类 .

```java
public class UserDaoOracleImpl implements UserDao {
   @Override
   public void getUser() {
       System.out.println("Oracle获取用户数据");
  }
}
```

那么我们要使用Oracle , 又需要去service实现类里面修改对应的实现 . 假设我们的这种需求非常大 , 这种方式就根本不适用了, 甚至反人类对吧 , 每次变动 , 都需要修改大量代码 . 这种设计的耦合性太高了, 牵一发而动全身 .

**那我们如何去解决呢 ?**

我们可以在需要用到他的地方 , 不去实现它 , 而是留出一个接口 , 利用set , 我们去代码里修改下 .

```java
public class UserServiceImpl implements UserService {
   private UserDao userDao;
// 利用set实现
   public void setUserDao(UserDao userDao) {
       this.userDao = userDao;
  }

   @Override
   public void getUser() {
       userDao.getUser();
  }
}
```

现在去我们的测试类里 , 进行测试 ;`（有了set方法就可以在调用的时候由用户选择调用的接口）`

```java
@Test
public void test(){
   UserServiceImpl service = new UserServiceImpl();
   service.setUserDao( new UserDaoMySqlImpl() );
   service.getUser();
   //那我们现在又想用Oracle去实现呢
   service.setUserDao( new UserDaoOracleImpl() );
   service.getUser();
}
```

大家发现了区别没有 ? 可能很多人说没啥区别 . 但是同学们 , 他们已经发生了根本性的变化 , 很多地方都不一样了 . 仔细去思考一下 , **以前所有东西都是由程序去进行控制创建** , **而现在是由我们自行控制创建对象 , 把主动权交给了调用者** . 程序不用去管怎么创建,怎么实现了 . 它只负责提供一个接口 .

这种思想 , 从本质上解决了问题 , 我们程序员不再去管理对象的创建了 , 更多的去关注业务的实现 . 耦合性大大降低 . 这也就是IOC的原型 !

### 本质

**控制反转IoC(Inversion of Control)，是一种设计思想，DI(依赖注入)是实现IoC的一种方法**，也有人认为DI只是IoC的另一种说法。没有IoC的程序中 , 我们使用面向对象编程 , 对象的创建与对象间的依赖关系完全硬编码在程序中，对象的创建由程序自己控制，控制反转后将对象的创建转移给第三方，个人认为所谓控制反转就是：获得依赖对象的方式反转了。

![img](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202401071551479.webp)

**IoC是Spring框架的核心内容**，使用多种方式完美的实现了IoC，可以使用XML配置，也可以使用注解，新版本的Spring也可以零配置实现IoC。

Spring容器在初始化时先读取配置文件，根据配置文件或元数据创建与组织对象存入容器中，程序使用时再从Ioc容器中取出需要的对象。

![img](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202401071552637.webp)

采用XML方式配置Bean的时候，Bean的定义信息是和实现分离的，而采用注解的方式可以把两者合为一体，Bean的定义信息直接以注解的形式定义在实现类中，从而达到了零配置的目的。

**控制反转是一种通过描述（XML或注解）并通过第三方去生产或获取特定对象的方式。在Spring中实现控制反转的是IoC容器，其实现方法是依赖注入（Dependency Injection,DI）。**

## HelloSpring

导入jar包,注意需要是spring-webmvc，因为这个会自动帮我们导入其他依赖的jar包

```xml
<dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-webmvc</artifactId>
   <version>5.2.0.RELEASE</version>
</dependency>
```

编写一个Hello实体类

```java
public class Hello {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Hello{" +
                "name='" + name + '\'' +
                '}';
    }
}

```

编写我们的spring文件 , 这里我们命名为beans.xml

*resources/beans.xml*

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--
        使用spring来创建对象，在spring中这些都称为bean
        id = 变量名
        class = new 的对象
        property 相当于给属性设置值
    -->
    <bean id="hello" class="com.balance.pojo.Hello">
        <property name="name" value="Spring"/>
    </bean>

</beans>
```

测试

```java
public class Test {
    public static void main(String[] args) {
        //解析beans.xml，获取spring上下文对象
        ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
        //我们的对象现在都在spring中管理，如果要使用，直接去里面取
        //getBean，参数为配置文件中bean标签的id
        Hello hello = (Hello) context.getBean("hello");
        System.out.println(hello);
    }
}
```

问题：

- Hello 对象是谁创建的 ? hello 对象是由Spring创建的
- Hello 对象的属性是怎么设置的 ? hello 对象的属性是由Spring容器设置的

这个过程就叫控制反转 :

- 控制 : 谁来控制对象的创建 , 传统应用程序的对象是由程序本身控制创建的 , 使用Spring后 , 对象是由Spring来创建的
- 反转 : 程序本身不创建对象 , 而变成被动的接收对象 .

依赖注入 : 就是利用set方法来进行注入的。IOC是一种编程思想，由主动的编程变成被动的接收

**对IOC原型进行修改**

新建*resources/beans.xml* **注意八大基本类型用value，其他用ref**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd">

   <bean id="MysqlImpl" class="com.kuang.dao.impl.UserDaoMySqlImpl"/>
   <bean id="OracleImpl" class="com.kuang.dao.impl.UserDaoOracleImpl"/>

   <bean id="ServiceImpl" class="com.kuang.service.impl.UserServiceImpl">
       <!--注意: 这里的name并不是属性 , 而是set方法后面的那部分 , 首字母小写-->
       <!--引用另外一个bean , 不是用value 而是用 ref-->
       <property name="userDao" ref="OracleImpl"/><!--具体使用哪个接口这里可以直接配置-->
   </bean>

</beans>
```

测试

```java
@Test
public void test2(){
   ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
   //不需要再在代码中写出调用哪个接口，只需要在配置文件中指明调用的接口即可。
   UserServiceImpl serviceImpl = (UserServiceImpl) context.getBean("ServiceImpl");
   serviceImpl.getUser();
    //原来的步骤
    //UserService userService = new UserServiceImpl();
    //userService.setUserDao(new UserDaoMysqlImpl());//原先需要在代码中调用特定的方法
	//userService.getUser();
}
```

到了现在 , 我们彻底不用再程序中去改动了 , 要实现不同的操作 , 只需要在xml配置文件中进行修改 ,

**所谓的IoC,一句话搞定 : 对象由Spring 来创建 , 管理 , 装配 !**

## Ioc创建对象方式

### 无参构造创建

就是在HelloSpring中创建的方式

类中为无参构造，xml中用`<property>`标签

```xml
public Hello() {}

<bean id="hello" class="com.balance.pojo.Hello">
    <property name="name" value="Spring"/>
</bean>
```

### 有参构造

当类中构造为有参构造时 

beans.xml 有三种方式编写

下标方式

```xml
<!-- 第一种根据index参数下标设置 -->
<bean id="hello" class="com.kuang.pojo.Hello">
   <!-- index指构造方法 , 下标从0开始 -->
   <constructor-arg index="0" value="balance"/>
</bean>
```

参数名方式

```xml
<!-- 第二种根据参数名字设置 -->
<bean id="hello" class="com.kuang.pojo.Hello">
   <!-- name指参数名 -->
   <constructor-arg name="name" value="balance"/>
</bean>
```

参数类型方式-**不推荐**

```xml
<!-- 第三种根据参数类型设置(不推荐使用) -->
<bean id="hello" class="com.balance.pojo.Hello">
    <constructor-arg type="java.lang.String" value="balance"/>
</bean>
```

**结论：在配置文件加载的时候。其中管理的所有对象都已经初始化了！即所有的bean标签**

## Spring配置

### 别名

alias 设置别名 , 为bean设置别名 , 可以设置多个别名

```xml
<bean id="hello" class="com.balance.pojo.Hello">
    <constructor-arg name="name" value="balance"/>
    <constructor-arg name="age" value="1"/>
</bean>

<alias name="hello" alias="helloNew"/>
```

此后getbean时就可以使用别名了

```java
public static void main(String[] args) {
    //获取spring上下文对象
    ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
    //我们的对象现在都在spring中管理，如果要使用，直接去里面取
    Hello hello = (Hello) context.getBean("helloNew");
    System.out.println(hello);
}
```

### Bean的配置

```xml
<!--bean就是java对象,由Spring创建和管理-->

<!--
   id 是bean的标识符,要唯一,如果没有配置id,name就是默认标识符
   如果配置id,又配置了name,那么name是别名
   name可以设置多个别名,可以用逗号,分号,空格隔开

   class是bean的全限定名=包名+类名
-->
<bean id="hello" name="hello2 h2,h3;h4" class="com.balance.pojo.Hello">
   <property name="name" value="Spring"/>
</bean>
```

在bean的属性name，也是别名

 **如果不配置id和name,且类对应的bean只有一个，可以根据applicationContext.getBean(类名.class)获取对象;**

### import

团队的合作通过import来实现，假设有多个bean文件，可以将多个配置文件，导入合并为一个

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <import resource="beans1.xml"/>
    <import resource="beans2.xml"/>
    <import resource="beans3.xml"/>
</beans>
```

## 依赖注入

Dependency Injection

### 构造器注入

在Ioc创建对象方式章节说过，略

### set方式注入

依赖注入：本质就是set注入，这里是指get/set的set

* **依赖：bean对象的创建依赖于容器**
* **注入：bean对象中的所有属性，由容器来注入**

#### 环境

Student.java

```java
public class Student {
    private String name;
    private Address address;
    private String[] books;
    private List<String> hobbies;
    private Map<String, String> card;
    private Set<String> games;
    private String wife;
    private Properties info;
}
```

Address.java

```java
public class Address {
    private String address;
}
```

applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="student" class="com.balance.pojo.Student">
        <!--普通属性注入 直接用value-->
        <property name="name" value="balance"/>
    </bean>

</beans>
```

测试

```java
public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        Student student = (Student) context.getBean("student");
        System.out.println(student);
    }
}
```

**后面所有的注入其实都是跟java类型相关，比如map键值可以是对象可以是基本类型等等，都可以使用标签注入，使用时自己灵活应用**

#### bean注入

```xml
<bean id="address" class="com.balance.pojo.Address">
    <property name="address" value="balanceHome"/>
    <!--
		这样也可以
        <property name="address">
            <value>balanceHome</value>
        </property>
	-->
</bean>
<bean id="student" class="com.balance.pojo.Student">
    .........
    <!--第二种，Bean注入 使用ref-->
    <property name="address" ref="address"/>
</bean>
```

#### 数组注入

```xml
<bean id="student" class="com.balance.pojo.Student">
    .......
    <!--第三种 数组注入-->
    <property name="books">
        <array>
            <value>红楼梦</value>
            <value>红楼梦2</value>
            <value>红楼梦3</value>
        </array>
    </property>
</bean>
```

#### List注入

```xml
<!--第四种 List注入-->
<property name="hobbies">
    <list>
        <value>看电视</value>
        <value>打游戏</value>
        <value>敲代码</value>
    </list>
</property>
```

#### map注入

![image-20240108163912009](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202401081639533.png)

```xml
<!--第五种 Map注入-->
<property name="card">
    <map>
        <entry key="身份证" value="342522"/>
        <entry key="身份证2" value="442522"/>
        <entry key="身份证3" value="542522"/>
    </map>
</property>
```

#### Set注入

```xml
<!--第六种 Set注入-->
<property name="games">
    <set>
        <value>hhhh</value>
    </set>
</property>
```

#### Null和空值注入

```xml
<!--第六种 NUll注入-->
<property name="wife">
    <null/>
</property>

<!--空值，直接字符串为空即可-->
<bean class="ExampleBean">
    <property name="email" value=""/>
</bean>
```

#### Properties注入

注意和map不同，key在里面，值在外面

```xml
<!--第七种 Properties注入-->
<property name="info">
    <props>
        <prop key="username">zhangsan</prop>
        <prop key="passwd">123456</prop>
    </props>
</property>
```

### 拓展方式注入

#### p命名空间注入

p命名空间注入就是无参构造属性注入

注意文件开头加上约束

```
xmlns:p="http://www.springframework.org/schema/p"
```

```xml
<bean id="address" class="com.balance.pojo.Address" p:address="balanceP"/>

<bean id="address" class="com.balance.pojo.Address">
    <property name="address" value="balanceHome"/>
</bean>
```

#### c命名空间注入

c命名空间注入就是有参构造摒弃`constructor-arg`标签

注意文件开头加上约束,且类需要有参构造

```
xmlns:c="http://www.springframework.org/schema/c"
```

```xml
<bean id="address2" class="com.balance.pojo.Address" c:address="balanceC"/>

<bean id="address" class="com.balance.pojo.Address">
    <constructor-arg name="address" value="balance"/>
</bean>
```

### Bean的作用域-Scope

在Spring中，那些组成应用程序的主体及由Spring IoC容器所管理的对象，被称之为bean。简单地讲，**bean就是由IoC容器初始化、装配及管理的对象 .**

![image-20240108181043523](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202401081810091.png)

![img](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202401081805656.webp)

几种作用域中，request、session作用域仅在基于web的应用中使用（不必关心你所采用的是什么web应用框架），只能用在基于web的Spring ApplicationContext环境。

#### Singleton

当一个bean的作用域为Singleton，那么Spring IoC容器中只会存在一个共享的bean实例，并且所有对bean的请求，只要id与该bean定义相匹配，则只会返回bean的同一实例。Singleton是单例类型，就是在创建起容器时就同时自动创建了一个bean的对象，不管你是否使用，他都存在了，每次获取到的对象都是同一个对象。注意，Singleton作用域是Spring中的缺省作用域。要在XML中将bean定义成singleton，可以这样配置：

```xml
<bean id="ServiceImpl" class="cn.csdn.service.ServiceImpl" scope="singleton">
```

单例模式也就是只new一次对象，之后getBean的都直接获取第一次new的对象

测试：

```java
 @Test
 public void test03(){
     ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
     User user = (User) context.getBean("user");
     User user2 = (User) context.getBean("user2");//第二次getBean
     System.out.println(user==user2);
 }
```

#### Prototype

当一个bean的作用域为Prototype，表示一个bean定义对应多个对象实例。Prototype作用域的bean会导致在每次对该bean请求（将其注入到另一个bean中，或者以程序的方式调用容器的getBean()方法）时都会创建一个新的bean实例。Prototype是原型类型，它在我们创建容器的时候并没有实例化，而是当我们获取bean的时候才会去创建一个对象，而且我们每次获取到的对象都不是同一个对象。根据经验，对有状态的bean应该使用prototype作用域，而对无状态的bean则应该使用singleton作用域。在XML中将bean定义成prototype，可以这样配置：

```xml
 <bean id="account" class="com.foo.DefaultAccount" scope="prototype"/>  
  或者
 <bean id="account" class="com.foo.DefaultAccount" singleton="false"/>
```

原型模式也就是在之后的getBean时重新new一个对象

#### Request

当一个bean的作用域为Request，表示在一次HTTP请求中，一个bean定义对应一个实例；即每个HTTP请求都会有各自的bean实例，它们依据某个bean定义创建而成。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

```xml
 <bean id="loginAction" class=cn.csdn.LoginAction" scope="request"/>
```

针对每次HTTP请求，Spring容器会根据loginAction bean的定义创建一个全新的LoginAction bean实例，且该loginAction bean实例仅在当前HTTP request内有效，因此可以根据需要放心的更改所建实例的内部状态，而其他请求中根据loginAction bean定义创建的实例，将不会看到这些特定于某个请求的状态变化。当处理请求结束，request作用域的bean实例将被销毁。

#### Session

当一个bean的作用域为Session，表示在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。考虑下面bean定义：

```xml
<bean id="userPreferences" class="com.foo.UserPreferences" scope="session"/>
```

针对某个HTTP Session，Spring容器会根据userPreferences bean定义创建一个全新的userPreferences bean实例，且该userPreferences bean仅在当前HTTP Session内有效。与request作用域一样，可以根据需要放心的更改所创建实例的内部状态，而别的HTTP Session中根据userPreferences创建的实例，将不会看到这些特定于某个HTTP Session的状态变化。当HTTP Session最终被废弃的时候，在该HTTP Session作用域内的bean也会被废弃掉。
