# SpringBoot

## 什么是Spring

Spring是一个开源框架，2003 年兴起的一个轻量级的Java 开发框架，作者：Rod Johnson 。

**Spring是为了解决企业级应用开发的复杂性而创建的，简化开发。**

## Spring如何简化java开发

为了降低Java开发的复杂性，Spring采用了以下4种关键策略：

1、基于POJO的轻量级和最小侵入性编程，所有东西都是bean；

2、通过IOC，依赖注入（DI）和面向接口实现松耦合；

3、基于切面（AOP）和惯例进行声明式编程；

4、通过切面和模版减少样式代码，RedisTemplate，xxxTemplate；

## 什么是SpringBoot

学过javaweb的同学就知道，开发一个web应用，从最初开始接触Servlet结合Tomcat, 跑出一个Hello Wolrld程序，是要经历特别多的步骤；后来就用了框架Struts，再后来是SpringMVC，到了现在的SpringBoot，过一两年又会有其他web框架出现；你们有经历过框架不断的演进，然后自己开发项目所有的技术也在不断的变化、改造吗？建议都可以去经历一遍；

言归正传，什么是SpringBoot呢，就是一个javaweb的开发框架，和SpringMVC类似，对比其他javaweb框架的好处，官方说是简化开发，约定大于配置， you can "just run"，能迅速的开发web应用，几行代码开发一个http接口。

所有的技术框架的发展似乎都遵循了一条主线规律：从一个复杂应用场景 衍生 一种规范框架，人们只需要进行各种配置而不需要自己去实现它，这时候强大的配置功能成了优点；发展到一定程度之后，人们根据实际生产应用情况，选取其中实用功能和设计精华，重构出一些轻量级的框架；之后为了提高开发效率，嫌弃原先的各类配置过于麻烦，于是开始提倡“约定大于配置”，进而衍生出一些一站式的解决方案。

是的这就是Java企业级应用->J2EE->spring->springboot的过程。

随着 Spring 不断的发展，涉及的领域越来越多，项目整合开发需要配合各种各样的文件，慢慢变得不那么易用简单，违背了最初的理念，甚至人称配置地狱。Spring Boot 正是在这样的一个背景下被抽象出来的开发框架，目的为了让大家更容易的使用 Spring 、更容易的集成各种常用的中间件、开源软件；

Spring Boot 基于 Spring 开发，Spirng Boot 本身并不提供 Spring 框架的核心特性以及扩展功能，只是用于快速、敏捷地开发新一代基于 Spring 框架的应用程序。也就是说，它并不是用来替代 Spring 的解决方案，而是和 Spring 框架紧密结合用于提升 Spring 开发者体验的工具。Spring Boot 以**约定大于配置的核心思想**，默认帮我们进行了很多设置，**多数 Spring Boot 应用只需要很少的 Spring 配置。同时它集成了大量常用的第三方库配置（例如 Redis、MongoDB、Jpa、RabbitMQ、Quartz 等等**），Spring Boot 应用中这些第三方库几乎可以零配置的开箱即用。

简单来说就是SpringBoot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar包，spring boot整合了所有的框架 。

Spring Boot 出生名门，从一开始就站在一个比较高的起点，又经过这几年的发展，生态足够完善，Spring Boot 已经当之无愧成为 Java 领域最热门的技术。

**Spring Boot的主要优点：**

- 为所有Spring开发者更快的入门
- **开箱即用**，提供各种默认配置来简化项目配置
- 内嵌式容器简化Web项目
- 没有冗余代码生成和XML配置的要求

## 第一个SpringBoot程序

注意高版本的SpringBoot（3.0以上）不再支持选择java 8。

两种方式解决：

* 通过修改服务器URL解决 (推荐)，在当前页面将 https://start.spring.io 替换为阿里的 https://start.aliyun.com，如截图所示：

  ![在这里插入图片描述](https://img-blog.csdnimg.cn/direct/fb6d229d15d64954bccdf612fd18f5d8.png)

* 通过代码解决

  直接随便选一个版本，创建完成后到pom.xml中修改springboot版本和java版本

  ```xml
  <parent>
  	<groupId> org.springframework.boot</groupId>
  	<artifactId>spring-boot-starter-parent</artifactId>
  	<version> 3.0以下 </version>
  </parent>
  
  <properties>
  	<!-- 修改java版本 -->
  	<java.version>1.8</java.version>
  </properties> 
  ```

Spring官方提供了非常方便的工具让我们快速构建应用

### **项目创建方式一：**spring

使用Spring Initializr 的 Web页面创建项目

1、打开 https://start.spring.io/

2、填写项目信息

3、点击”Generate Project“按钮生成项目；下载此项目

4、解压项目包，并用IDEA以Maven项目导入，一路下一步即可，直到项目导入完毕。

5、如果是第一次使用，可能速度会比较慢，包比较多、需要耐心等待一切就绪。

**项目结构分析：**

通过上面步骤完成了基础项目的创建。就会自动生成以下文件。

1、程序的主启动类

2、一个 application.properties 配置文件

3、一个 测试类

4、一个 pom.xml

**pom.xml文件分析**

打开pom.xml，看看Spring Boot项目的依赖：

**可以看到有一个parent项目，然后依赖中含有web依赖和测试相关（可以换成junit）,然后有一个打包所用插件**

```xml
<!-- 父依赖 -->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.5.RELEASE</version>
    <relativePath/>
</parent>

<dependencies>
    <!-- web场景启动器 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <!-- springboot单元测试 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <!-- 剔除依赖 -->
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>

<build>
    <plugins>
        <!-- 打包插件 -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

### 项目创建方式二：idea

![image-20240314140358509](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240314140358509.png)

**删除package后面多余项和不删除区别如下**

![image-20240314140517691](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240314140517691.png)



**注意demos文件夹是演示代码可以删除**

新建一个controller包新建hellocontroller

![image-20240314141339942](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240314141339942.png)

测试

![image-20240314141411661](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240314141411661.png)

可以修改启动端口，直接在application.properties中修改即可

```properties
# 应用服务 WEB 访问端口
server.port=8080
```

### 如何打包

![image-20240314135859808](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240314135859808.png)

**可以配置打包时 跳过项目运行测试用例**

```xml
<!--
    在工作中,很多情况下我们打包是不想执行测试用例的
    可能是测试用例不完事,或是测试用例会影响数据库数据
    跳过测试用例执
    -->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <!--跳过项目运行测试用例-->
        <skipTests>true</skipTests>
    </configuration>
</plugin>
```

**如果打包成功，则会在target目录下生成一个 jar 包,打成了jar包后，就可以在任何地方运行了**

运行命令如下

```
java -jar 你的jar包
```

**如果显示 xxxx.jar中没有主清单属性**

查找之后发现阿里云生成的pom.xml文件默认跳过了主类，删除后成功运行

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
                <encoding>UTF-8</encoding>
            </configuration>
        </plugin>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>${spring-boot.version}</version>
-            <configuration>
-                <mainClass>com.balance.Springboot01HelloApplication</mainClass>
-                <skip>true</skip>
            </configuration>
            <executions>
                <execution>
                    <id>repackage</id>
                    <goals>
                        <goal>repackage</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

### 设置banner

将banner.txt放在resources目录下即可

## SpringBoot原理初探

我们之前写的HelloSpringBoot，到底是怎么运行的呢，Maven项目，我们一般从pom.xml文件探究起；

### 父依赖

其中它主要是依赖一个父项目，主要是管理项目的资源过滤及插件！

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.2.5.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```

点进去，发现还有一个父依赖

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.2.5.RELEASE</version>
    <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
```

**以后我们导入依赖默认是不需要写版本；但是如果导入的包没有在依赖中管理着就需要手动配置版本了**

### 启动器 spring-boot-starter

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**springboot-boot-starter-xxx**：就是spring-boot的场景启动器

**spring-boot-starter-web**：帮我们导入了web模块正常运行所依赖的组件；

SpringBoot将所有的功能场景都抽取出来，做成一个个的starter （启动器），只需要在项目中引入这些starter即可，所有相关的依赖都会导入进来 ， 我们要用什么功能就导入什么样的场景启动器即可 ；我们未来也可以自己自定义 starter；

分析完了 pom.xml 来看看这个启动类

```java
//@SpringBootApplication 来标注一个主程序类
//说明这是一个Spring Boot应用
@SpringBootApplication
public class SpringbootApplication {

   public static void main(String[] args) {
     //以为是启动了一个方法，没想到启动了一个服务
      SpringApplication.run(SpringbootApplication.class, args);
   }

}
```

#### @SpringBootApplication

作用：标注在某个类上说明这个类是SpringBoot的主配置类 ， SpringBoot就应该运行这个类的main方法来启动SpringBoot应用；

进入这个注解：可以看到上面还有很多其他注解！

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    // ......
}
```

##### @ComponentScan

这个注解在Spring中很重要 ,它对应XML配置中的元素。

作用：自动扫描并加载符合条件的组件或者bean ， 将这个bean定义加载到IOC容器中

##### @SpringBootConfiguration

作用：SpringBoot的配置类 ，标注在某个类上 ， 表示这是一个SpringBoot的配置类；

我们继续进去这个注解查看

```java
// 点进去得到下面的 @Component
@Configuration
public @interface SpringBootConfiguration {}

@Component
public @interface Configuration {}
```

这里的 @Configuration，说明这是一个配置类 ，配置类就是对应Spring的xml 配置文件；

里面的 @Component 这就说明，启动类本身也是Spring中的一个组件而已，负责启动应用！

我们回到 SpringBootApplication 注解中继续看。

##### @EnableAutoConfiguration

```java
@AutoConfigurationPackage
@Import({AutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```

**开启自动配置功能**

以前我们需要自己配置的东西，而现在SpringBoot可以自动帮我们配置 ；@EnableAutoConfiguration告诉SpringBoot开启自动配置功能，这样自动配置才能生效；

点进注解继续查看：

###### **@AutoConfigurationPackage**

**自动配置包**

```java
@Import({AutoConfigurationPackages.Registrar.class})
public @interface AutoConfigurationPackage {
}
```

**@import** ：Spring底层注解@import ， 给容器中导入一个组件

Registrar.class 作用：将主启动类的所在包及包下面所有子包里面的所有组件扫描到Spring容器 ；

###### @Import({AutoConfigurationImportSelector.class})

AutoConfigurationImportSelector ：自动配置导入选择器，那么它会导入哪些组件的选择器呢？我们点击去这个类看源码：

1、这个类中有一个这样的方法

```java
// 获得候选的配置
protected List<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes) {
    //这里的getSpringFactoriesLoaderFactoryClass（）方法
    //返回的就是我们最开始看的启动自动导入配置文件的注解类；EnableAutoConfiguration
    List<String> configurations = SpringFactoriesLoader.loadFactoryNames(this.getSpringFactoriesLoaderFactoryClass(), this.getBeanClassLoader());
    Assert.notEmpty(configurations, "No auto configuration classes found in META-INF/spring.factories. If you are using a custom packaging, make sure that file is correct.");
    return configurations;
}
```

2、这个方法又调用了 SpringFactoriesLoader 类的静态方法！我们进入SpringFactoriesLoader类loadFactoryNames() 方法

```java
public static List<String> loadFactoryNames(Class<?> factoryClass, @Nullable ClassLoader classLoader) {
    String factoryClassName = factoryClass.getName();
    //这里它又调用了 loadSpringFactories 方法
    return (List)loadSpringFactories(classLoader).getOrDefault(factoryClassName, Collections.emptyList());
}
```

3、我们继续点击查看 loadSpringFactories 方法

```java
private static Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader) {
    //获得classLoader ， 我们返回可以看到这里得到的就是EnableAutoConfiguration标注的类本身
    MultiValueMap<String, String> result = (MultiValueMap)cache.get(classLoader);
    if (result != null) {
        return result;
    } else {
        try {
            //去获取一个资源 "META-INF/spring.factories"
            Enumeration<URL> urls = classLoader != null ? classLoader.getResources("META-INF/spring.factories") : ClassLoader.getSystemResources("META-INF/spring.factories");
            LinkedMultiValueMap result = new LinkedMultiValueMap();

            //将读取到的资源遍历，封装成为一个Properties
            while(urls.hasMoreElements()) {
                URL url = (URL)urls.nextElement();
                UrlResource resource = new UrlResource(url);
                Properties properties = PropertiesLoaderUtils.loadProperties(resource);
                Iterator var6 = properties.entrySet().iterator();

                while(var6.hasNext()) {
                    Entry<?, ?> entry = (Entry)var6.next();
                    String factoryClassName = ((String)entry.getKey()).trim();
                    String[] var9 = StringUtils.commaDelimitedListToStringArray((String)entry.getValue());
                    int var10 = var9.length;

                    for(int var11 = 0; var11 < var10; ++var11) {
                        String factoryName = var9[var11];
                        result.add(factoryClassName, factoryName.trim());
                    }
                }
            }

            cache.put(classLoader, result);
            return result;
        } catch (IOException var13) {
            throw new IllegalArgumentException("Unable to load factories from location [META-INF/spring.factories]", var13);
        }
    }
}
```

**4、发现一个多次出现的文件：spring.factories，全局搜索它**

### spring.factories

我们根据源头打开spring.factories ， 看到了很多自动配置的文件；这就是自动配置根源所在！

![image-20240314153427813](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240314153427813.png)

**WebMvcAutoConfiguration**

我们在上面的自动配置类随便找一个打开看看，比如 ：WebMvcAutoConfiguration

![image-20240314153550658](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240314153550658.png)

可以看到这些一个个的都是JavaConfig配置类，而且都注入了一些Bean，**注意看到上面有@ConditionOnClass的注解，只有类似这样的注解条件满足才会自动导入该配置类**

所以，自动配置真正实现是**从classpath中搜寻所有的META-INF/spring.factories配置文件** ，并将其中对应的 org.springframework.boot.autoconfigure 包下的配置项，**通过反射实例化为对应标注了 @Configuration的JavaConfig形式的IOC容器配置类** ， 然后将这些都**汇总成为一个实例并加载到IOC容器**中。

**结论：**

1. SpringBoot在启动的时候从类路径下的META-INF/spring.factories中获取EnableAutoConfiguration指定的值
2. 将这些值作为自动配置类导入容器 ， 自动配置类就生效 ， 帮我们进行自动配置工作；
3. 整个J2EE的整体解决方案和自动配置都在springboot-autoconfigure的jar包中；
4. 它会给容器中导入非常多的自动配置类 （xxxAutoConfiguration）, 就是给容器中导入这个场景需要的所有组件 ， 并配置好这些组件 ；
5. 有了自动配置类 ， 免去了我们手动编写配置注入功能组件等的工作；

### 不简单的run方法

我最初以为就是运行了一个main方法，没想到却开启了一个服务；

```java
@SpringBootApplication
public class Springboot01HelloApplication {

    public static void main(String[] args) {
        SpringApplication.run(Springboot01HelloApplication.class, args);
    }

}
```

**SpringApplication.run分析**

分析该方法主要分两部分，一部分是SpringApplication的实例化，二是run方法的执行；

#### SpringApplication

**这个类主要做了以下四件事情：**

1、推断应用的类型是普通的项目还是Web项目

2、查找并加载所有可用初始化器 ， 设置到initializers属性中

3、找出所有的应用程序监听器，设置到listeners属性中

4、推断并设置main方法的定义类，找到运行的主类

查看构造器：

```java
public SpringApplication(ResourceLoader resourceLoader, Class... primarySources) {
    // ......
    this.webApplicationType = WebApplicationType.deduceFromClasspath();
    this.setInitializers(this.getSpringFactoriesInstances();
    this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class));
    this.mainApplicationClass = this.deduceMainApplicationClass();
}
```

#### run方法流程分析

![img](https://img2020.cnblogs.com/blog/1905053/202004/1905053-20200412221135119-1843322351.png)

## yaml

yaml语法学习

### 3.1、配置文件

SpringBoot使用一个全局的配置文件 ， 配置文件名称是固定的

- application.properties

  语法结构 ：key=value

- application.yml（或者写成application.yaml，没有区别）

  语法结构 ：key：空格value

**配置文件的作用 ：**修改SpringBoot自动配置的默认值，因为SpringBoot在底层都给我们自动配置好了；

比如我们可以在配置文件中修改Tomcat 默认启动的端口号！测试一下！

```ini
server.port=8081
```

### yaml 概述

YAML是 "YAML Ain't a Markup Language" （YAML不是一种标记语言）的递归缩写。

在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）

**其实就是为了强调这种语言以数据作为中心，而不是以标记语言为重点！**

以前的配置文件，大多数都是使用xml来配置；比如一个简单的端口配置，我们来对比下yaml和xml

传统xml配置：

```xml
<server>
    <port>8081<port>
</server>
```

yaml配置：

```yml
server：
  prot: 8080
```

### 基础语法

说明：语法要求严格！

1、空格不能省略

2、以缩进来控制层级关系，只要是左边对齐的一列数据都是同一个层级的。

3、属性和值的大小写都是十分敏感的。

**字面量：普通的值 [ 数字，布尔值，字符串 ]**

字面量直接写在后面就可以 ， **字符串默认不用加上双引号或者单引号；**

```yml
k: v
```

注意：

- “ ” 双引号，不会转义字符串里面的特殊字符 ， 特殊字符会作为本身想表示的意思；

  比如 ：name: "kuang \n shen" 输出 ：kuang 换行 shen

- '' 单引号，会转义特殊字符 ， 特殊字符最终会变成和普通字符一样输出

  比如 ：name: ‘kuang \n shen’ 输出 ：kuang \n shen

#### **对象、Map（键值对）**

```yml
#对象、Map格式
k: 
    v1:
    v2:
```

在下一行来写对象的属性和值的关系，注意缩进；比如：

```yml
student:
    name: qinjiang
    age: 3
```

行内写法

```yml
student: {name: qinjiang,age: 3}
```

#### **数组（ List、set ）**

用 - 值表示数组中的一个元素,比如：

```yml
pets:
 - cat
 - dog
 - pig
```

行内写法

```yml
pets: [cat,dog,pig]
```

#### **修改SpringBoot的默认端口号**

配置文件中添加，端口号的参数，就可以切换端口；

```yml
server:  
  port: 8082
```

注入配置文件

**yaml文件更强大的地方在于，它可以给我们的实体类直接注入匹配值！**

### yaml注入配置文件

1、在springboot项目中的resources目录下新建一个文件 application.yml

2、编写一个实体类 Dog；

```java
@Component  //注册bean到容器中
public class Dog {
    private String name;
    private Integer age;
    
    //有参无参构造、get、set方法、toString()方法  
}
```

3、思考，我们原来是如何给bean注入属性值的！@Value，给狗狗类测试一下：

```java
@Component //注册bean
public class Dog {
    @Value("旺财")
    private String name;
    @Value("18")
    private Integer age;
}
```

4、在SpringBoot的测试类下注入狗狗输出一下；

```java
@SpringBootTest
class DemoApplicationTests {

    @Autowired //将狗狗自动注入进来
    Dog dog;

    @Test
    public void contextLoads() {
        System.out.println(dog); //打印看下狗狗对象
    }

}
```

结果成功输出，@Value注入成功，这是我们原来的办法。

![image-20240314162213372](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240314162213372.png)

5、我们在编写一个复杂一点的实体类：Person 类

```java
@Component //注册bean到容器中
public class Person {
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
    
    //有参无参构造、get、set方法、toString()方法  
}
```

6、我们来使用yaml配置的方式进行注入，大家写的时候注意区别和优势，我们编写一个yaml配置！

```yml
person:
  name: qinjiang
  age: 3
  happy: false
  birth: 2000/01/01
  maps: {k1: v1,k2: v2}
  lists:
   - code
   - girl
   - music
  dog:
    name: 旺财
    age: 1
```

7、我们刚才已经把person这个对象的所有值都写好了，我们现在来注入到我们的类中！

```java
/*
@ConfigurationProperties作用：
将配置文件中配置的每一个属性的值，映射到这个组件中；
告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定
参数 prefix = “person” : 将配置文件中的person下面的所有属性一一对应
*/
@Component //注册bean
@ConfigurationProperties(prefix = "person")
public class Person {
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```

8、确认以上配置都OK之后，我们去测试类中测试一下：

```java
@SpringBootTest
class DemoApplicationTests {

    @Autowired
    Person person; //将person自动注入进来

    @Test
    public void contextLoads() {
        System.out.println(person); //打印person信息
    }

}
```

结果：所有值全部注入成功！

![image-20240314162626669](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240314162626669.png)

### 加载指定的配置文件

**@PropertySource ：**加载指定的配置文件；

**@configurationProperties**：默认从全局配置文件中获取值；

1、我们去在resources目录下新建一个**person.properties**文件

```properties
name=kuangshen
```

2、然后在我们的代码中指定加载person.properties文件

**注意此处取是以 `${}` 形式取**

```java
@PropertySource(value = "classpath:person.properties")
@Component //注册bean
public class Person {

    @Value("${name}")
    private String name;

    ......  
}
```

3、再次输出测试一下：指定配置文件绑定成功！

### 配置文件占位符

配置文件还可以编写占位符生成随机数

```yml
person:
    name: qinjiang${random.uuid} # 随机uuid
    age: ${random.int}  # 随机int
    happy: false
    birth: 2000/01/01
    maps: {k1: v1,k2: v2}
    lists:
      - code
      - girl
      - music
    dog:
      name: ${person.hello:other}_旺财
      age: 1
```

`name: ${person.hello:other}`这一行的意思是如果 person.hello有值则值为person.hello 否则为 other

### 回顾properties配置

我们上面采用的yaml方法都是最简单的方式，开发中最常用的；也是springboot所推荐的！那我们来唠唠其他的实现方式，道理都是相同的；写还是那样写；配置文件除了yml还有我们之前常用的properties ， 我们没有讲，我们来唠唠！

【注意】properties配置文件在写中文的时候，会有乱码 ， 我们需要去IDEA中设置编码格式为UTF-8；settings-->FileEncodings 中配置；

**测试步骤：**

1、新建一个实体类User

```java
@Component //注册bean
public class User {
    private String name;
    private int age;
    private String sex;
}
```

2、编辑配置文件 user.properties

```properties
user1.name=kuangshen
user1.age=18user1.sex=男
```

3、我们在User类上使用@Value来进行注入！

```java
@Component //注册bean
@PropertySource(value = "classpath:user.properties")
public class User {
    //直接使用@value
    @Value("${user.name}") //从配置文件中取值
    private String name;
    @Value("#{9*2}")  // #{SPEL} Spring EL表达式
    private int age;
    @Value("男")  // 字面量
    private String sex;
}
```

4、Springboot测试

```ini
user1.name=kuangshen
user1.age=18
user1.sex=男
```

测试！

### 对比 

@Value 使用起来并不友好！我们需要为每个属性单独注解赋值，比较麻烦；我们来看个功能对比图

![img](https://img2020.cnblogs.com/blog/1905053/202004/1905053-20200412221528114-1258238964.png)

1、**@ConfigurationProperties只需要写一次即可 ， @Value则需要每个字段都添加**

2、松散绑定：这个什么意思呢? 比如我的**yml中写的last-name，这个和lastName是一样的， 后面跟着的字母默认是大写的。这就是松散绑定。可以测试一下**

3、JSR303数据校验 ， 这个就是我们**可以在字段是增加一层过滤器验证 ， 可以保证数据的合法性**

4、复杂类型封装，**yml中可以封装对象 ， 使用value就不支持**

**结论：**

配置yml和配置properties都可以获取到值 ， 强烈推荐 yml；

如果我们在某个业务中，只需要获取配置文件中的某个值，可以使用一下 @value；

如果说，我们专门编写了一个JavaBean来和配置文件进行一一映射，就直接@configurationProperties，不要犹豫！

### JSR303校验

#### 使用示例

Springboot中可以用@validated来校验数据，如果数据异常则会统一抛出异常，方便异常中心统一处理。我们这里来写个注解让我们的name只能支持Email格式；

```java
@Component
@ConfigurationProperties(prefix = "person")
@Validated
public class Person {
    @Email
    private String name;
    
    
@SpringBootTest
class Springboot01HelloApplicationTests {
    @Autowired
    private Person person;

    @Test
    void contextLoads() {
        System.out.println(person);
    }

}
```

运行结果 ：default message [不是一个合法的电子邮件地址];

若注解报错，需要导包

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

#### 常见参数

```java
@NotNull(message="名字不能为空")
private String userName;
@Max(value=120,message="年龄最大不能查过120")
private int age;
@Email(message="邮箱格式错误")
private String email;

空检查
@Null       验证对象是否为null
@NotNull    验证对象是否不为null, 无法查检长度为0的字符串
@NotBlank   检查约束字符串是不是Null还有被Trim的长度是否大于0,只对字符串,且会去掉前后空格.
@NotEmpty   检查约束元素是否为NULL或者是EMPTY.
    
Booelan检查
@AssertTrue     验证 Boolean 对象是否为 true  
@AssertFalse    验证 Boolean 对象是否为 false  
    
长度检查
@Size(min=, max=) 验证对象（Array,Collection,Map,String）长度是否在给定的范围之内  
@Length(min=, max=) string is between min and max included.

日期检查
@Past       验证 Date 和 Calendar 对象是否在当前时间之前  
@Future     验证 Date 和 Calendar 对象是否在当前时间之后  
@Pattern    验证 String 对象是否符合正则表达式的规则

.......等等
除此以外，我们还可以自定义一些数据校验规则
```

其实用 Pattern 写正则表达式就可以完成校验

### 多环境配置

> 工作中常常需要写多个环境

我们在主配置文件编写的时候，文件名可以是 application-{profile}.properties/yml , 用来指定多个环境版本；

**例如：**

application-test.properties 代表测试环境配置

application-dev.properties 代表开发环境配置

但是Springboot并不会直接启动这些配置文件，它**默认使用application.properties主配置文件**；

我们需要通过一个配置来选择需要激活的环境：

```properties
#比如在配置文件中指定使用dev环境，我们可以通过设置不同的端口号进行测试；
#我们启动SpringBoot，就可以看到已经切换到dev下的配置了；
spring.profiles.active=dev
```

#### yaml的多环境切换

和properties配置文件中一样，但是使用yml去实现不需要创建多个配置文件，更加方便了 !

```yml
server:
  port: 8081
#选择要激活那个环境块
spring:
  profiles:
    active: prod

---
server:
  port: 8083
spring:
  profiles: dev #配置环境的名称


---

server:
  port: 8084
spring:
  profiles: prod  #配置环境的名称
```

**注意：如果yml和properties同时都配置了端口，并且没有激活其他环境 ， 默认会使用properties配置文件的！**

#### 配置文件加载位置及优先级

**外部加载配置文件的方式十分多，我们选择最常用的即可，在开发的资源文件中进行配置！**

官方外部配置文件说明参考文档

![img](https://img2020.cnblogs.com/blog/1905053/202004/1905053-20200412221627864-281289739.png)

springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件：

```lua
优先级1：项目路径下的config文件夹配置文件
优先级2：项目路径下配置文件
优先级3：资源路径下的config文件夹配置文件
优先级4：资源路径下配置文件
```

优先级由高到底，高优先级的配置会覆盖低优先级的配置；

**SpringBoot会从这四个位置全部加载主配置文件；互补配置；**

![image-20240320130532426](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240320130532426.png)

#### 拓展，运维小技巧

指定位置加载配置文件

我们还可以通过spring.config.location来改变默认的配置文件位置

项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；

这种情况，一般是后期运维做的多，**相同配置，外部指定的配置文件优先级最高**

## SpringBoot自动配置原理

自动配置原理

配置文件到底能写什么？怎么写？

SpringBoot官方文档中有大量的配置，我们无法全部记住

### 分析自动配置原理

我们以**HttpEncodingAutoConfiguration（Http编码自动配置）**为例解释自动配置原理；

```java
//表示这是一个配置类，和以前编写的配置文件一样，也可以给容器中添加组件；
@Configuration 

//启动指定类的ConfigurationProperties功能；
  //进入这个HttpProperties查看，将配置文件中对应的值和HttpProperties绑定起来；
  //并把HttpProperties加入到ioc容器中
@EnableConfigurationProperties({HttpProperties.class}) 

//Spring底层@Conditional注解
  //根据不同的条件判断，如果满足指定的条件，整个配置类里面的配置就会生效；
  //这里的意思就是判断当前应用是否是web应用，如果是，当前配置类生效
@ConditionalOnWebApplication(
    type = Type.SERVLET
)

//判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；
@ConditionalOnClass({CharacterEncodingFilter.class})

//判断配置文件中是否存在某个配置：spring.http.encoding.enabled；
  //如果不存在，判断也是成立的
  //即使我们配置文件中不配置pring.http.encoding.enabled=true，也是默认生效的；
@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
)

public class HttpEncodingAutoConfiguration {
    //他已经和SpringBoot的配置文件映射了
    private final Encoding properties;
    //只有一个有参构造器的情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpProperties properties) {
        this.properties = properties.getEncoding();
    }
    
    //给容器中添加一个组件，这个组件的某些值需要从properties中获取
    @Bean
    @ConditionalOnMissingBean //判断容器没有这个组件？
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));
        return filter;
    }
    //。。。。。。。
}
```

**一句话总结 ：根据当前不同的条件判断，决定这个配置类是否生效！**

- 一但这个配置类生效；这个配置类就会给容器中添加各种组件；
- 这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；
- 所有在配置文件中能配置的属性都是在xxxxProperties类中封装着；
- 配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
//从配置文件中获取指定的值和bean的属性进行绑定
@ConfigurationProperties(prefix = "spring.http") 
public class HttpProperties {
    // .....
}
```

### 精髓

1、SpringBoot启动会加载大量的自动配置类

2、我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中；

3、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在在其中，我们就不需要再手动配置了）

4、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性。我们只需要在配置文件中指定这些属性的值即可；

**xxxxAutoConfigurartion：自动配置类；**给容器中添加组件

**xxxxProperties:封装配置文件中相关属性；**

### @Conditional

了解完自动装配的原理后，我们来关注一个细节问题，**自动配置类必须在一定的条件下才能生效；**

**@Conditional派生注解（Spring注解版原生的@Conditional作用）**

作用：必须是@Conditional指定的条件成立，才给容器中添加组件，配置配里面的所有内容才生效；

![img](https://img2020.cnblogs.com/blog/1905053/202004/1905053-20200412221747954-591282661.png)

**那么多的自动配置类，必须在一定的条件下才能生效；**也就是说，我们加载了这么多的配置类，但不是所有的都生效了。

我们怎么知道哪些自动配置类生效？

**我们可以通过启用 debug=true属性；来让控制台打印自动配置报告，这样我们就可以很方便的知道哪些自动配置类生效；**

```properties
#开启springboot的调试类
debug=true
```

**Positive matches:**（自动配置类启用的：正匹配）

**Negative matches:**（没有启动，没有匹配成功的自动配置类：负匹配）

**Unconditional classes: **（没有条件的类）

【演示：查看输出的日志】

掌握吸收理解原理，即可以不变应万变！

## SpringBootWeb开发

### 一、首先要解决的问题

1. 导入静态资源
2. 首页
3. jsp–模板引擎 Thymeleaf
4. 装配扩展SpringMVC
5. 增删改查
6. 拦截器
7. 国际化–中英翻译

### 二、静态资源访问

#### 2.1、什么是webjars

 WebJars是**将web前端资源（js，css等）打成jar包文件**，然后借助Maven工具，以jar包形式对web前端资源进行统一依赖管理，保证这些Web资源版本唯一性。WebJars的jar包部署在Maven中央仓库上。

#### 2.2、方式一（webjars一般不使用这种方式）

找到`WebMvcAutoConfiguration`类，这其中有一个`WebMvcAutoConfigurationAdapter`内部类，里面是关于mvc相关配置，这其中有一个方法

![image-20240320140940486](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240320140940486.png)

了解可以从webjars中寻找后，去webjars官网，在pom中导入相关依赖

![image-20240320141134440](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240320141134440.png)

使用`http://localhost:8080/webjars/**` 方式对静态资源进行访问！

比如`http://localhost:8080/webjars/jquery/3.6.2/package.json`

#### 2.3、方式二

![image-20240320141552484](C:\Users\18356\AppData\Roaming\Typora\typora-user-images\image-20240320141552484.png)

ctrl点击后发现

![image-20240320141648977](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240320141648977.png)

ctrl点击进入webproperties便可以发现，有以下四条路径可以访问

![image-20240320141818515](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240320141818515.png)

它们的优先级如下

![image-20240320141928361](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240320141928361.png)

#### 2.4、方式三（自定义资源路径）

 可以通过配置SpringBoot的默认配置文件，来自定义静态资源路径

### 三、首页

到`WebMvcAutoConfiguration`类，可以发现有关首页/欢迎页相关的方法

![image-20240320142349524](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240320142349524.png)

研读可以发现访问`http://localhost:8080/`就是在静态资源路径下找名为index的页面

可以在static文件夹下加入favicon.ico更换图标

### 四、Thymeleaf 模板引擎 

>https://docs.spring.io/spring-boot/docs/2.7.18/reference/html/index.html springboot文档
>
>https://www.thymeleaf.org/ 官网

#### 4.1、引入

前端交给我们的页面，是html页面。如果是我们以前开发，我们需要把他们转成jsp页面，jsp好处就是当我们查出一些数据转发到JSP页面以后，我们可以用jsp轻松实现数据的显示，及交互等。

jsp支持非常强大的功能，包括能写Java代码，但是SpringBoot这个项目首先是以jar的方式，不是war。

第二，我们用的还是嵌入式的Tomcat，所以呢，他现在默认是不支持jsp的。
不支持jsp，如果我们直接用纯静态页面的方式，那给我们开发会带来非常大的麻烦，那怎么办呢？

#### 4.2、模板引擎

 SpringBoot推荐使用模板引擎来进行开发，基本思想参考下图：

![img](https://img-blog.csdnimg.cn/a6c6823337cd48d59d6a362dee00884e.png#pic_center)

模板引擎的作用就是我们来写一个页面模板，比如有些值是动态的，我们需要写一些表达式。

而这些值从哪来？其实就是我们在后台封装一些数据。然后把这个模板和这个数据交给我们模板引擎，模板引擎按照我们这个数据帮你把这表达式解析、填充到我们指定的位置，然后把这个数据最终生成一个我们想要的内容给我们写出去。

#### 4.3、引入Thymeleaf

```xml
<!--thymeleaf-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

![image-20240320150106172](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240320150106172.png)

#### 4.4 简单测试

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class HelloController {
    @RequestMapping("/hello")
    public String hello() {
        return "index";
    }
}
```

**当开启了thymeleaf支持，此时会自动匹配resources目录下templates文件夹下的页面进行跳转**

![image-20240320150317202](C:\Users\18356\AppData\Roaming\Typora\typora-user-images\image-20240320150317202.png)

**注意templates文件夹下页面无法直接访问**

#### 4.5 Thymeleaf自动配置类：ThymeleafProperties

```java
//部分代码
public class ThymeleafProperties {
  private static final Charset DEFAULT_ENCODING;
  //默认前缀
  public static final String DEFAULT_PREFIX = "classpath:/templates/";
  //默认后缀
  public static final String DEFAULT_SUFFIX = ".html";
  private boolean checkTemplate = true;
  private boolean checkTemplateLocation = true;
  //前缀
  private String prefix = "classpath:/templates/";
  //后缀
  private String suffix = ".html";
}
```

-  从源码可以发现，这些代码和我们之前学习的SpringMVC的视图解析器配置很相似

- 可以看到视图解析器默认的前缀和后缀
- 只需要把我们的html页面放在类路径下的templates下，thymeleaf就可以帮我们自动渲染了。使用thymeleaf什么都不需要配置，只需要将他放在指定的文件夹下即可！

具体的thymeleaf基本语法格式，还需要我们参考thymeleaf官方文档练习。

## MVC自动配置及扩展

![image-20240320151308351](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240320151308351.png)

**双击shift全局搜索**

现在是SpringBoot自动帮我们配置了MVC，以前我们自己需要配置 映射器 适配器 和视图解析器，我们如何扩展SpringBoot帮我们自动所做的呢？可以看上面图片最后一段

### 实现视图解析器

```java
@Configuration
public class MyMvcConfig implements WebMvcConfigurer {
    //如果想要自己定义一些功能，只要写个组件，然后把它交给SpringBoot，SpringBoot就会自动装配
    //扩展SpringMVC

    //interface ViewResolver：实现了视图解析器接口的类，就可以看做是视图解析器
    @Bean
    public ViewResolver myViewResolver(){
        return new MyViewResolver();
    }


    /*
    * 自定义了一个自己的视图解析器MyViewResolver
    *   1、实现视图解析器接口：ViewResolver
    *   2、重写接口中的方法
    * */
    public static class MyViewResolver implements ViewResolver {

        @Override
        public View resolveViewName(String viewName, Locale locale) throws Exception {
            return null;
        }
    }
}
```

## 自定义starter

> Spring Boot Starters启动器依赖:
> 官方文档:
> https://docs.spring.io/spring-boot/docs/2.0.3.RELEASE/reference/htmlsingle/#using-boot-starter

我们分析完毕了源码以及自动装配的过程，我们可以尝试自定义一个启动器来玩玩！

### 6.1、说明

启动器模块是一个 空 jar 文件，仅提供辅助性依赖管理，这些依赖可能用于自动装配或者其他类库；

**命名归约：**

官方命名：

- 前缀：spring-boot-starter-xxx
- 比如：spring-boot-starter-web....

自定义命名：

- xxx-spring-boot-starter
- 比如：mybatis-spring-boot-starter

###  6.2、编写启动器

1、在IDEA中新建一个空项目 spring-boot-starter-diy

2、新建一个普通Maven模块：kuang-spring-boot-starter

![img](https://img2020.cnblogs.com/blog/1905053/202004/1905053-20200412221856069-1152361375.png)

3、新建一个Springboot模块：kuang-spring-boot-starter-autoconfigure

![img](https://img2020.cnblogs.com/blog/1905053/202004/1905053-20200412221930344-363603356.png)

4、点击apply即可，基本结构

![img](https://img2020.cnblogs.com/blog/1905053/202004/1905053-20200412222010544-802039453.png)

5、在我们的 starter 中 导入 autoconfigure 的依赖！

```xml
<!-- 启动器 -->
<dependencies>
    <!--  引入自动配置模块 -->
    <dependency>
        <groupId>com.kuang</groupId>
        <artifactId>kuang-spring-boot-starter-autoconfigure</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </dependency>
</dependencies>
```

6、将 autoconfigure 项目下多余的文件都删掉，Pom中只留下一个 starter，这是所有的启动器基本配置！

![img](https://img2020.cnblogs.com/blog/1905053/202004/1905053-20200412222056275-508024261.png)

7、我们编写一个自己的服务

```java
package com.kuang;

public class HelloService {

    HelloProperties helloProperties;

    public HelloProperties getHelloProperties() {
        return helloProperties;
    }

    public void setHelloProperties(HelloProperties helloProperties) {
        this.helloProperties = helloProperties;
    }

    public String sayHello(String name){
        return helloProperties.getPrefix() + name + helloProperties.getSuffix();
    }

}
```

8、编写HelloProperties 配置类

```java
package com.kuang;

import org.springframework.boot.context.properties.ConfigurationProperties;

// 前缀 kuang.hello
@ConfigurationProperties(prefix = "kuang.hello")
public class HelloProperties {

    private String prefix;
    private String suffix;

    public String getPrefix() {
        return prefix;
    }

    public void setPrefix(String prefix) {
        this.prefix = prefix;
    }

    public String getSuffix() {
        return suffix;
    }

    public void setSuffix(String suffix) {
        this.suffix = suffix;
    }
}
```

9、编写我们的自动配置类并注入bean，测试！

```java
package com.kuang;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.condition.ConditionalOnWebApplication;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@ConditionalOnWebApplication //web应用生效
@EnableConfigurationProperties(HelloProperties.class)
public class HelloServiceAutoConfiguration {

    @Autowired
    HelloProperties helloProperties;

    @Bean
    public HelloService helloService(){
        HelloService service = new HelloService();
        service.setHelloProperties(helloProperties);
        return service;
    }

}
```

10、在resources编写一个自己的 META-INF\spring.factories

```xml
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.kuang.HelloServiceAutoConfiguration
```

11、编写完成后，可以安装到maven仓库中！

![img](https://img2020.cnblogs.com/blog/1905053/202004/1905053-20200412223705466-546919127.png)

### 6.3、新建项目测试我们自己写的启动器

1、新建一个SpringBoot 项目

2、导入我们自己写的启动器

```xml
<dependency>
    <groupId>com.kuang</groupId>
    <artifactId>kuang-spring-boot-starter</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

3、编写一个 HelloController 进行测试我们自己的写的接口！

```java
package com.kuang.controller;

@RestController
public class HelloController {

    @Autowired
    HelloService helloService;

    @RequestMapping("/hello")
    public String hello(){
        return helloService.sayHello("zxc");
    }

}
```

4、编写配置文件 application.properties

```java
kuang.hello.prefix="ppp"
kuang.hello.suffix="sss"
```

5、启动项目进行测试，结果成功 !

![img](https://img2020.cnblogs.com/blog/1905053/202004/1905053-20200412222355347-1685568990.png)
