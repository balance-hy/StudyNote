# SpringMVC

## 回顾MVC

- MVC是模型(Model：==dao service==)、视图(View：==jsp==)、控制器(Controller：===servlet==)的简写，是一种软件设计规范。

- 是将业务逻辑、数据、显示分离的方法来组织代码。
- MVC主要作用是降低了视图与业务逻辑间的双向偶合。
- MVC不是一种设计模式，MVC是一种架构模式。当然不同的MVC存在差异。

**Model（模型）：**数据模型，提供要展示的数据，因此包含数据和行为，可以认为是领域模型或JavaBean组件（包含数据和行为），不过现在一般都分离开来：Value Object（数据Dao） 和 服务层（行为Service）。也就是模型提供了模型数据查询和模型数据的状态更新等功能，包括数据和业务。

**View（视图）：**负责进行模型的展示，一般就是我们见到的用户界面，客户想看到的东西。

**Controller（控制器）：**接收用户请求，委托给模型进行处理（状态改变），处理完毕后把返回的模型数据返回给视图，由视图负责展示。也就是说控制器做了个调度员的工作。

**最典型的MVC就是JSP + servlet + javabean的模式。**

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy91SkRBVUtyR0M3S3dQT1BXcTAwcE1KaWFLODZsRjZCaklYVzdXbW05S1ZFVjFGWFVmSk1EMEt6dVlaN2ljNVVIZ2dzWkRBenlZeXJkNHBMdm5CSVZNNXpBLzY0MA?x-oss-process=image/format,png)

## 回顾servlet

 父项目包如下

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.1</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.3.24</version>
    </dependency>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>javax.servlet.jsp-api</artifactId>
        <version>2.3.3</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
    </dependency>
</dependencies>
```

> jstl的groupId是javax.servlet，另一个groupId为javax.servlet.jsp.jstl的包会出现ClassNotFound的异常
>
> 参考你的Tomcat版本选择servlet和jsp依赖的版本，网址为http://tomcat.apache.org/whichversion.html，我的是9.0.x，故选择4.0.1的javax.servlet-api和2.3.3的javax.servlet.jsp-api，这里的两个依赖scope建议设置为provided，表示不会被打包，因为Tomcat自身会提供

具体回顾此处参见javaWeb相关笔记，或者参见

> https://blog.csdn.net/ceotaojie/article/details/108638293

## 初识SpringMVC

Spring MVC是Spring Framework的一部分，是基于Java实现MVC的轻量级Web框架。

查看官方文档：https://docs.spring.io/spring-framework/docs/5.3.31/reference/html/web.html#spring-web

Spring MVC的特点：

- 轻量级，简单易学

- 高效 , 基于请求响应的MVC框架
- 与Spring兼容性好，无缝结合
- 约定大于配置
- 功能强大：RESTful、数据验证、格式化、本地化、主题等
- 简洁灵活

spring的web框架围绕DispatcherServlet [ 调度Servlet ] 设计。

**DispatcherServlet的作用是将请求分发到不同的处理器**。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解形式进行开发，十分简洁；

正因为SpringMVC好 , 简单 , 便捷 , 易学 , 天生和Spring无缝集成(使用SpringIoC和Aop) , 使用约定优于配置 . 能够进行简单的junit测试 . **支持Restful风格** .异常处理 , 本地化 , 国际化 , 数据验证 , 类型转换 , 拦截器 等等…所以我们要学习 .

**最重要的一点还是用的人多 , 使用的公司多**

### 中心控制器

 Spring的web框架围绕DispatcherServlet设计。DispatcherServlet的作用是将请求分发到不同的处理器。从Spring 2.5开始，使用Java 5或者以上版本的用户可以采用基于注解的controller声明方式。

 Spring MVC框架像许多其他MVC框架一样, 以请求为驱动 , 围绕一个中心Servlet分派请求及提供其他功能，**DispatcherServlet是一个实际的Servlet (它继承自HttpServlet 基类)。**

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy91SkRBVUtyR0M3S3dQT1BXcTAwcE1KaWFLODZsRjZCakk3RU51MGpOaWJQaWFpYWlhQmh5eDZvOVVVeVU4Mk1kZGc0RGp3em5pYWN6bVRMUmJBdEk5cEtKcTF0US82NDA?x-oss-process=image/format,png)

SpringMVC的原理如下图所示：

 当发起请求时被前置的控制器拦截到请求，根据请求参数生成代理请求，找到请求对应的实际控制器，控制器处理请求，创建数据模型，访问数据库，将模型响应给中心控制器，控制器使用模型与视图渲染视图结果，将结果返回给中心控制器，再将结果返回给请求者

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy91SkRBVUtyR0M3S3dQT1BXcTAwcE1KaWFLODZsRjZCaklhb3NWemljbFdMRUpRa3pvYnhIcnBIY210dTJ5VGVWV1BtRUk0WXE1UGFpY1M1MlZhSnQ4ZFlmUS82NDA?x-oss-process=image/format,png)

### SpringMVC执行原理

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9tbWJpei5xcGljLmNuL21tYml6X3BuZy91SkRBVUtyR0M3S3dQT1BXcTAwcE1KaWFLODZsRjZCaklibVBPa1k4VHhGNnF2R0FHWHhDN2RBclljcjh1SmxXb1ZDNGFGNGJmeGdDR0NEOHNIZzhtZ3cvNjQw?x-oss-process=image/format,png)

图为SpringMVC的一个较完整的流程图，实线表示SpringMVC框架提供的技术，不需要开发者实现，虚线表示需要开发者实现。

**简要分析执行流程**

1. DispatcherServlet表示前置控制器，是整个SpringMVC的控制中心。用户发出请求，DispatcherServlet接收请求并拦截请求。

   我们假设请求的url为 : http://localhost:8080/SpringMVC/hello

   如上url拆分成三部分：

   http://localhost:8080服务器域名

   SpringMVC部署在服务器上的web站点

   hello表示控制器

   通过分析，如上url表示为：请求位于服务器localhost:8080上的SpringMVC站点的hello控制器。

2. HandlerMapping为**处理器映射**。DispatcherServlet调用HandlerMapping,**HandlerMapping根据请求url查找Handler**。

3. HandlerExecution表示具体的Handler,其主要作用是根据url查找控制器，如上url被查找控制器为：hello。

4. HandlerExecution将解析后的信息传递给DispatcherServlet,如解析控制器映射等。

5. HandlerAdapter表示**处理器适配器，其按照特定的规则去执行Handler**。

6. Handler让具体的Controller执行。

7. Controller将具体的执行信息返回给HandlerAdapter,如ModelAndView。

8. HandlerAdapter将视图逻辑名或模型传递给DispatcherServlet。

9. **DispatcherServlet调用视图解析器(ViewResolver)来解析HandlerAdapter传递的逻辑视图名**。

10. 视图解析器将解析的逻辑视图名传给DispatcherServlet。

11. DispatcherServlet根据视图解析器解析的视图结果，调用具体的视图。

12. 最终视图呈现给用户。


## 第一个springmvc程序

创建普通maven项目，添加web支持，新版本添加web支持，在项目名右键

![image-20240309150836737](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240309150836737.png)

然后选择

![image-20240309150918651](https://raw.githubusercontent.com/balance-hy/typora/master/thinkbook/image-20240309150918651.png)

**注意一定要确保web目录是可以被识别的，也就是有标记的文件夹**

配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--1.注册DispatcherServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!--启动级别-1-->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!--/ 匹配所有的请求；（不包括.jsp）-->
    <!--/* 匹配所有的请求；（包括.jsp）-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>

</web-app>
```

注意在初始参数需要一个springmvc-servlet.xml即spring配置文件，在resource目录下新建该文件,注册三个bean，分别为

处理映射器：帮你寻找到底是哪一个映射

处理适配器：通过映射执行相应的控制器

视图解析器：这其中有前缀和后缀，其实就是帮你拼接成具体的页面路径

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--添加 处理映射器 BeanNameUrlHandlerMapping根据名字找-->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    <!--添加 处理器适配器-->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>

    <!--视图解析器:DispatcherServlet给他的ModelAndView-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver" id="InternalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

然后实现一个controller接口

```java
package com.balance.controller;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloController implements Controller {

    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        //ModelAndView 模型和视图
        ModelAndView mv = new ModelAndView();

        //封装对象，放在ModelAndView中。Model
        mv.addObject("msg","HelloSpringMVC!");
        
        //封装要跳转的视图，放在ModelAndView中
        mv.setViewName("hello"); //: /WEB-INF/jsp/hello.jsp
        
        return mv;
    }

}
```

在spring配置文件中注册该controller

```xml
<!--Handler-->
<bean id="/hello" class="com.balance.controller.HelloController"/>
```

写要跳转的jsp页面，显示ModelandView存放的数据       web/web-inf/jsp/hello.jsp **放在web-inf目录对外不可见**

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>   
<title>title</title>
</head>
<body>
${msg}
</body>
</html>
```

配置Tomcat 启动测试！注意如果是后来添加web支持，会发现没法选择 artifacts 此时在pom.xml中添加打包方式即可

```xml
<packaging>war</packaging>
```

**可能遇到的问题：访问出现404，排查步骤：**

1. 查看控制台输出，看一下是不是缺少了什么jar包。
2. 如果jar包存在，显示无法输出，就在IDEA的项目发布中，添加lib依赖！
3. 重启Tomcat 即可解决！

 IDEA的项目发布：右键打开项目结构，选择Artifacts，查看在web-inf下是否有lib目录，若无则创建，并点击+号将library files中所有包加入进去。

**建议直接创建webapp项目，就没这么多事了**

看这个估计大部分同学都能理解其中的原理了，但是我们实际开发才不会这么写，不然就疯了，还学这个玩意干嘛！我们来看个注解版实现，这才是SpringMVC的精髓。

## 注解实现第一个mvc

首先注意maven导包问题 pom.xml

```xml
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.properties</include>
                <include>**/*.xml</include>
            </includes>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

在pom.xml文件引入相关的依赖：主要有Spring框架核心库、Spring MVC、servlet , JSTL等。我们在父依赖中已经引入了！

**配置web.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!--1.注册DispatcherServlet-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--关联一个springmvc的配置文件:【servlet-name】-servlet.xml-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc-servlet.xml</param-value>
        </init-param>
        <!--启动级别-1-->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <!--/ 匹配所有的请求；（不包括.jsp）-->
    <!--/* 匹配所有的请求；（包括.jsp）-->
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

**添加Spring MVC配置文件**

首先指定要扫描的包

其次不让其处理静态资源

然后**添加mvc注解驱动支持 就不用再写映射器和适配器了**

最后写视图解析器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!--指定要扫描的包，这个包下注解（spring特有）就会生效-->
    <context:component-scan base-package="com.balance.controller"/>

    <!--让Spring MVC不处理静态资源 例如css、js等-->
    <mvc:default-servlet-handler/>
    <!--
   支持mvc注解驱动
       在spring中一般采用@RequestMapping注解来完成映射关系
       要想使@RequestMapping注解生效
       必须向上下文中注册DefaultAnnotationHandlerMapping 即映射器
       和一个AnnotationMethodHandlerAdapter实例 即适配器
       这两个实例分别在类级别和方法级别处理。
       而annotation-driven配置帮助我们自动完成上述两个实例的注入。
    -->
    <mvc:annotation-driven />

    <!-- 视图解析器 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
          id="internalResourceViewResolver">
        <!-- 前缀 -->
        <property name="prefix" value="/WEB-INF/jsp/" />
        <!-- 后缀 -->
        <property name="suffix" value=".jsp" />
    </bean>

</beans>
```

**创建Controller**

注意RequestMapping也可以写在类上，如果类上和方法上都有，实际路径为类和方法的拼接

@Controller是为了让Spring IOC容器初始化时自动扫描到；

方法中声明Model类型的参数是为了把Controller中的数据带到视图中

方法返回的结果是视图的名称hello，加上配置文件中的前后缀变成WEB-INF/jsp/**hello**.jsp。

```java
package com.balance.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller //被这个注解的类中所有方法，若返回值为字符串，且有具体页面可以跳转，那么就会被视图解析器解析
public class HelloController {
    @RequestMapping("/hello")
    public String hello(Model model){
        //向模型中添加属性msg与值，可以在JSP页面中取出并渲染
        model.addAttribute("msg","hello world");
        return "hello";
    }
}
```

**创建视图层** 在WEB-INF/ jsp目录中创建hello.jsp ， 视图可以直接取出并展示从Controller带回的信息；

最后测试

实现步骤其实非常的简单：

- 新建一个web项目

- 导入相关jar包
- 编写web.xml , 注册DispatcherServlet
- 编写springmvc配置文件
- 接下来就是去创建对应的控制类 , controller
- 最后完善前端视图和controller之间的对应
- 测试运行调试.

使用springMVC必须配置的三大件：

**处理器映射器、处理器适配器、视图解析器**

通常，我们只需要手动配置视图解析器，而处理器映射器和处理器适配器只需要开启注解驱动即可，而省去了大段的xml配置

## Controller和Restful风格

### Controller

- 控制器复杂提供访问应用程序的行为，通常通过接口定义或**注解定义（建议）**两种方法实现。
- 控制器负责解析用户的请求并将其转换为一个模型。
- 在Spring MVC中一个Controller可以包含多个方法
- 在Spring MVC中，对于Controller的配置方式有很多种

两种方式参见上面的笔记

**注意如果修改了java代码，需要redeploy，即重新部署**

**修改了配置文件，重新发布**

**修改了前端页面，刷新即可**

### Restful风格

#### 概念

Restful就是一个资源定位及资源操作的风格。不是标准也不是协议，只是一种风格。基于这个风格设计的软件可以更简洁，更有层次，更易于实现缓存等机制。

#### **功能**

资源：互联网所有的事物都可以被抽象为资源

资源操作：**使用POST、DELETE、PUT、GET，使用不同方法对资源进行操作**。

分别对应 **添加、 删除、修改、查询**。

**传统方式操作资源** ：通过不同的参数来实现不同的效果！方法单一，post 和 get

 http://127.0.0.1/item/queryItem.action?id=1 查询,GET

 http://127.0.0.1/item/saveItem.action 新增,POST

 http://127.0.0.1/item/updateItem.action 更新,POST

 http://127.0.0.1/item/deleteItem.action?id=1 删除,GET或POST

**使用RESTful操作资源** ：可以通过不同的请求方式来实现不同的效果！如下：**请求地址一样，但是功能可以不同**！

 http://127.0.0.1/item/1 查询,GET

 http://127.0.0.1/item 新增,POST

 http://127.0.0.1/item 更新,PUT

 http://127.0.0.1/item/1 删除,DELETE

#### 示例

在下面的情况中，我们访问是 http://localhost:8080/test?a=1&b=2

```java
@Controller
public class RestFulController {
    @RequestMapping("/test")
    public String test(int a , int b, Model m) {
        int res=a+b;
        m.addAttribute("msg",res);
        return "test";
    }
}
```

如果使用RestFul风格编码 访问路径就变成 http://localhost:8080/test/1/2

```java
@Controller
public class RestFulController {
    @RequestMapping("/test/{a}/{b}")
    public String test(@PathVariable int a ,@PathVariable int b, Model m) {
        int res=a+b;
        m.addAttribute("msg",res);
        return "test";
    }
}
```

但是现在这样依旧未实现url的复用

>  http://127.0.0.1/item 新增,POST
>
>  http://127.0.0.1/item 更新,PUT

RequestMapping支持两个参数，一个是value别名为path，一个是method，通过指定method，我们就可以实现复用

```java
@Controller
public class RestFulController {
    //也可以写成@RequestMapping(value = "/test/{a}/{b}",method = RequestMethod.GET)
    @RequestMapping(path = "/test/{a}/{b}",method = RequestMethod.GET)
    public String test(@PathVariable int a ,@PathVariable int b, Model m) {
        int res=a+b;
        m.addAttribute("msg",res);
        return "test";
    }
}
```

更简单的，我们可以使用提供的组合注解来对应不同的方法

```
@GetMapping
@PostMapping
@PutMapping
@DeleteMapping
@PatchMapping
```

比如上述例子 就可以改写成以下代码，路径依旧为 http://localhost:8080/test/1/2

```java
@Controller
public class RestFulController {
    @GetMapping( "/test/{a}/{b}")
    public String test(@PathVariable int a ,@PathVariable int b, Model m) {
        int res=a+b;
        m.addAttribute("msg",res);
        return "test";
    }
}
```

#### 注解含义

##### @PathVariable

在Spring MVC中，`@PathVariable` 注解用于从请求 URL 中提取参数值。通常，我们使用 `@PathVariable` 来捕获 URL 中的动态部分，并将其作为方法参数传递给处理请求的方法。

例如，考虑以下 URL 模式：`/users/{userId}`，其中 `{userId}` 是一个动态的路径参数。如果我们希望在处理该请求的方法中获取 `userId` 的值，我们可以使用 `@PathVariable` 注解。

以下是一个简单的示例：

```java
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping("/{userId}")
    public String getUserById(@PathVariable Long userId) {
        // 根据 userId 获取用户信息
        return "User ID: " + userId;
    }
}
```

在上面的例子中，`@GetMapping("/{userId}")` 声明了一个处理 `/users/{userId}` 请求的方法。`@PathVariable Long userId` 将 `{userId}` 的值映射到方法的参数中，以供方法内部使用。

##### @RestController

```java
@RestController
```

- `@RestController` 注解是 `@Controller` 的特殊形式，用于创建 RESTful Web 服务。
- 与 `@Controller` 不同，`@RestController` 不会将方法的返回值视图名称，而是直接将其返回作为 HTTP 响应体。
- 通常情况下，`@RestController` 中的方法**会返回 JSON 或 XML 等格式的数据，而不是视图**。

示例：

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyRestController {

    @GetMapping("/api/hello")
    public String hello() {
        return "Hello, World!"; // 直接返回字符串作为响应体
    }
}
```

总之，`@Controller` 用于创建传统的 MVC 控制器，而 `@RestController` 用于创建 RESTful Web 服务，直接返回数据而不是视图。

## Spring结果跳转方式

### ModelAndView

设置ModelAndView对象 , 根据view的名称 , 和视图解析器跳到指定的页面 .

页面 : {视图解析器前缀} + viewName +{视图解析器后缀}

```xml
<!-- 视图解析器 -->
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"
     id="internalResourceViewResolver">
   <!-- 前缀 -->
   <property name="prefix" value="/WEB-INF/jsp/" />
   <!-- 后缀 -->
   <property name="suffix" value=".jsp" />
</bean>
```

对应的controller类

```java
public class ControllerTest1 implements Controller {

   public ModelAndView handleRequest(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse) throws Exception {
       //返回一个模型视图对象
       ModelAndView mv = new ModelAndView();
       mv.addObject("msg","ControllerTest1");
       mv.setViewName("test");
       return mv;
  }
}
```

**此时是最基本的实现接口来做跳转**

### ServletAPI

因为视图解析器本质上就是一个Servlet，而Servlet可以实现转发或者重定向，**注释掉视图解析器**

1、通过HttpServletResponse进行输出

2、通过HttpServletResponse实现重定向

3、通过HttpServletResponse实现转发

```java
@Controller
public class ResultGo {

   @RequestMapping("/result/t1")
   public void test1(HttpServletRequest req, HttpServletResponse rsp) throws IOException {
       rsp.getWriter().println("Hello,Spring BY servlet API");
  }

   @RequestMapping("/result/t2")
   public void test2(HttpServletRequest req, HttpServletResponse rsp) throws IOException {
       rsp.sendRedirect("/index.jsp");
  }

   @RequestMapping("/result/t3")
   public void test3(HttpServletRequest req, HttpServletResponse rsp) throws Exception {
       //转发
       req.setAttribute("msg","/result/t3");
       req.getRequestDispatcher("/WEB-INF/jsp/test.jsp").forward(req,rsp);
  }

}
```

**注意此种方式，需要参数里面有请求和响应**

### SpringMVC -无解析器

将视图解析器注释掉

```java
@Controller
public class ResultSpringMVC {
    
   @RequestMapping("/rsm/t2")
   public String test2(){
       //转发二
       return "forward:/index.jsp";
  }

   @RequestMapping("/rsm/t3")
   public String test3(){
       //重定向
       return "redirect:/index.jsp";
  }
}
```

此种方式如果是转发需要加上 `forward:` 如果是重定向需要加上`redirect:`

### SpringMVC -有解析器

```java
@Controller
public class ResultSpringMVC2 {
   @RequestMapping("/rsm2/t1")
   public String test1(){
       //转发
       return "test";
  }
   @RequestMapping("/rsm2/t2")
   public String test2(){
       //重定向
       return "redirect:/index.jsp";
       //return "redirect:hello.do"; //hello.do为另一个请求/
  }
}
```

此时转发直接写即可，重定向依旧加上 `redirect:`

## 乱码问题解决

1. 方式一自己实现过滤器，javaweb中
2. spring提供的过滤器

将乱码过滤器组测到web.xml:

```xml
<!--配置SpringMVC的乱码过滤-->
<filter>
    <filter-name>encoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>encoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

注意 url-pattern 处是 `/*`而非`/ ` `/*`可以匹配jsp等格式

## Json

### 什么是Json

- JSON(JavaScript Object Notation, JS 对象标记) 是一种轻量级的数据交换格式，目前使用特别广泛。
- 采用完全独立于编程语言的**文本格式**来存储和表示数据。
- 简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。
- 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率。

在 JavaScript 语言中，一切都是对象。因此，任何JavaScript 支持的类型都可以通过 JSON 来表示，例如字符串、数字、对象、数组等。看看他的要求和语法格式：

- 对象表示为键值对，数据由逗号分隔
- 花括号保存对象
- 方括号保存数组

**JSON 键值对**是用来保存 JavaScript 对象的一种方式，和 JavaScript 对象的写法也大同小异，键/值对组合中的键名写在前面并用双引号 "" 包裹，使用冒号 : 分隔，然后紧接着值：

```json
{"name": "QinJiang"}
{"age": "3"}
{"sex": "男"}
```

很多人搞不清楚 JSON 和 JavaScript 对象的关系，甚至连谁是谁都不清楚。其实，可以这么理解：

**JSON 是 JavaScript 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串**。

```js
var obj = {a: 'Hello', b: 'World'}; //这是一个对象，注意键名也是可以使用引号包裹的
var json = '{"a": "Hello", "b": "World"}'; //这是一个 JSON 字符串，本质是一个字符串
```

**JSON 和 JavaScript 对象互转**

要实现从JSON字符串转换为JavaScript 对象，使用 JSON.parse() 方法：

```js
var obj = JSON.parse('{"a": "Hello", "b": "World"}');
//结果是 {a: 'Hello', b: 'World'}
```

要实现从JavaScript 对象转换为JSON字符串，使用 JSON.stringify() 方法：

```js
var json = JSON.stringify({a: 'Hello', b: 'World'});
//结果是 '{"a": "Hello", "b": "World"}'
```

### Controller返回JSON数据

Jackson应该是目前比较好的json解析工具了

当然工具不止这一个，比如还有阿里巴巴的 fastjson 等等。

我们这里使用Jackson，使用它需要导入它的jar包；

```xml
<!-- https://mvnrepository.com/artifact/com.fasterxml.jackson.core/jackson-databind -->
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.16.1</version>
</dependency>

<!--lomb-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.30</version>
    <scope>provided</scope>
</dependency>
```

