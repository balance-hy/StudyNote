# JavaWeb

## Tomcat

### 安装与配置

> https://www.cnblogs.com/winton-nfs/p/13831076.html

注意一下**jdk版本需要对应tamcat版本**，否则会不成功

### 目录说明

bin目录下

![image-20231102160939947](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311021609501.png)

![image-20231102161256050](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311021612147.png)

![image-20231102161346358](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311021613400.png)

## Maven

为什么要使用**Maven**？它能帮助我们解决什么问题？

- 添加第三方 jar 包:

  在今天的 JavaEE 开发领域，有大量的第三方框架和工具可以供我们使用。要使用这些 jar 包最简单 的方法就是复制粘贴到 WEB-INF/lib 目录下。但是这会导致每次创建一个新的工程就需要将 jar 包重复复 制到 lib 目录下，从而造成工作区中存在大量重复的文件，让我们的工程显得很臃肿。 而使用 Maven 后每个 jar 包本身只在本地仓库中保存一份，需要 jar 包的工程只需要以坐标的方式 简单的引用一下就可以了。不仅极大的节约了存储空间，让项目更轻巧，更避免了重复文件太多而造成 的混乱。

- jar 包之间的依赖关系

  jar 包往往不是孤立存在的，很多 jar 包都需要在其他 jar 包的支持下才能够正常工作，我们称之为 jar 包之间的依赖关系。引入 Maven 后，Maven 就可以替我们自动的将当前 jar 包所依赖的其他所有 jar 包全部导入进来， 无需人工参与，节约了我们大量的时间和精力。用实际例子来说明就是：通过 Maven 导入 commons-fileupload-1.3.jar 后，commons-io-2.0.1.jar 会被自动导入，程序员不必了解这个依赖关系。

### 配置

新增环境变量名：MAVEN_HOME 和 M2_HOME；

```
MAVEN_HOME: 安装目录bin目录的上一级目录
M2_HOME:安装目录bin目录
```

在系统变量Path中新增

```
 Path：%MAVEN_HOME%\bin            
```

更改镜像源，加速下载

在maven安装目录conf目录下，修改settings.xml,在mirrors中注释原有镜像新增阿里云镜像

```xml
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

修改本地仓库，同样是在maven安装目录conf目录下，修改settings.xml，默认安装在

```xml
Default: ${user.home}/.m2/repository
```

也就当前用户的.m2目录下，本地仓库的作用是可以直接引用下载到本地的jar包，无需远程重新下载。

### idea中使用

高版本无法在创建项目时更改maven配置，会使用默认的maven，**注意**

![image-20231104163049800](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041630204.png)

一个干净的maven项目结构

![image-20231104163417506](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041634564.png)

maven项目之webapp项目结构，只有web应用才会有

![image-20231104163449496](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041640506.png)

#### idea配置本地Tomcat

##### 步一：

![image-20231104163907453](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041639645.png)

##### 步二：

![image-20231104163950713](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041639912.png)

##### 步三：

![image-20231104164042117](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041640406.png)

##### 步四：

![image-20231104164149364](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041641114.png)

##### 步五：添加artifacts作为服务启动项

![image-20231104165104010](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041651805.png)

![image-20231104164802228](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041648442.png)

##### maven项目

![image-20231104165332984](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041653813.png)

##### pom.xml

```xml
<!--maven版本和头文件-->
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <!--配置的GAV-->
  <groupId>com.balance</groupId>
  <artifactId>learnMaven</artifactId>
  <version>1.0-SNAPSHOT</version>

  <!--项目打包方式-->
  <packaging>war</packaging>

  <name>learnMaven Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <!--项目依赖-->
  <dependencies>
  <!--具体依赖的jar包-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <!--build项目构建所用 这里配置，来防止我们资源导出失败的问题-->
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
</project>
```

## Servlet

Servlet就是Sun公司开发动态web的一门技术

Sun在这些API中提供一个接口叫做：Servlet，如果你想开发一个Servlet程序，只需要两个步骤

- 编写一个类，实现Servlet接口
- 把开发好的java类部署到web服务器中

把实现了Servlet接口的java程序叫做：Servlet

### 第一个servlet程序

创建一个空maven项目，若没有空项目maven选项，创建一个webapp原型项目，删除不需要的文件即可。

在该项目pom.xml中添加servlet依赖

```xml
<dependencies>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
    </dependency>
</dependencies>
```

之后根项目右键创建module，命名为servlet-01，webapp原型项目

**上面的操作创建了父子项目，父项目的依赖子项目可以直接用**

之后在src/main/java包下，新建包，servlet，在其中创建HelloServlet类，完成后目录如下

![image-20231106161327792](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311061613364.png)

HelloServlet类**继承默认实现的HttpServlet类**，重写doGet方法

```java
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        PrintWriter writer = resp.getWriter();
        writer.print("this is servlet");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

**在WEB-INF目录下修改web.xml让其和tomcat中xml一致**

再在web.xml的web-app标签中编写servlet的映射如下

```xml

<servlet>
    <servlet-name>hello</servlet-name>
    <servlet-class>com.balance.servlet.HelloServlet</servlet-class>
</servlet>
<!--servlet的请求路径-->
<servlet-mapping>
    <servlet-name>hello</servlet-name>
    <url-pattern>/hello</url-pattern>
</servlet-mapping>
```

**为什么需要编写这个映射：**

- 我们写的是java程序，但是要通过浏览器访问，浏览器需要连接web服务器，所以我们需要再web服务中即web.xml中注册我们所写的Servlet,并且要给他一个浏览器可以访问的路径

最后配置Tomcat，运行

```sql
http://localhost:8080/servlet_01_war/hello

# /servlet_01_war 配置tomcat指定的访问路径
# /hello 我们定义的Servlet请求路径
```

### mapping相关问题

可以使用通配符来响应不在定义之内的页面

```xml
<servlet-mapping>
    <servlet-name>error</servlet-name>
    <url-pattern>/*</url-pattern>
</servlet-mapping>
```

设定了固有的路径优先级最高，若找不到才会返回通配符响应的页面。

还可以使用如下形式

```xml
<servlet-mapping>
    <servlet-name>hello</servlet-name>
    <url-pattern>*.xxx</url-pattern>
</servlet-mapping>
```

以上述方式进行映射，注意前面不能加具体路径，如/hello/*.xxx，会报错。

### ServletContext

web容器在启动的时候，会为每个web程序都创建一个对应的ServleContext对象，它代表了当前的web应用

#### 共享数据

在一个web程序之中会有多个servlet，我们在一个servlet中向ServleContext存放数据后，在另一个servlet中也可以访问。

在第一个servlet中存

```java
public class servlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // this.getInitParameter(); 初始化参数
        // this.getServletConfig(); servlet配置
        // this.getServletContext(); servlet上下文
        ServletContext servletContext = this.getServletContext();
        String userName="balance";
        servletContext.setAttribute("userName",userName);
    }
}
```

在第二个servlet取

```java
public class GetServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
        String userName = (String) servletContext.getAttribute("userName");
        resp.setContentType("text/html");
        resp.setCharacterEncoding("UTF-8");
        resp.getWriter().print("userName="+userName);
    }
}
```

在web.xml中配置路径

```xml
<servlet>
    <servlet-name>hello</servlet-name>
    <servlet-class>com.balance.servlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>hello</servlet-name>
    <url-pattern>/hello</url-pattern>
</servlet-mapping>
<servlet>
    <servlet-name>getS</servlet-name>
    <servlet-class>com.balance.GetServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>getS</servlet-name>
    <url-pattern>/getS</url-pattern>
</servlet-mapping>
```

注意需要先访问第一个再访问第二个，因为存了才能取。

#### 获取初始化参数

web.xml 可以有一些初始化参数，配置如下

```xml
<!--配置web应用初始化参数-->
<context-param>
    <param-name>url</param-name>
    <param-value>jdbc:mysql://localhost:3306</param-value>
</context-param>

<!--配置映射-->
<servlet>
    <servlet-name>getParams</servlet-name>
    <servlet-class>com.balance.GetParams</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>getParams</servlet-name>
    <url-pattern>/getParams</url-pattern>
</servlet-mapping>
```

可以通过ServletContext获取到

```java
public class GetParams extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
        String url = servletContext.getInitParameter("url");
        resp.getWriter().print(url);
    }
}
```

#### 请求转发

servletContext可以帮助我们完成请求转发功能。**注意，这不是重定向，不会改变路径**

```java
public class ServletDispatcher extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
        RequestDispatcher requestDispatcher = servletContext.getRequestDispatcher("/getParams");//转发的具体路径
        requestDispatcher.forward(req,resp);//调用forward实现具体转发
    }
}
```

```xml
<servlet>
    <servlet-name>dispatch</servlet-name>
    <servlet-class>com.balance.ServletDispatcher</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>dispatch</servlet-name>
    <url-pattern>/dispatch</url-pattern>
</servlet-mapping>
```

![image-20231109152958466](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311091530299.png)

#### 读取资源文件

当我们有两份资源文件，我们如何读取呢？

![image-20231109153240804](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311091532455.png)

首先，当项目运行时，这两份文件都会被打包到target文件目录下，classes中，我们俗称为类路径

注意在com.balance包下可能不会正常打包，可以在pom.xml加入

```xml
<!--build项目构建所用 这里配置，来防止我们资源导出失败的问题-->
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

![image-20231109153706813](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311091537042.png)

使用servletContext.getResourceAsStream将资源转换为流，从而Properties对象可以加载

```java
public class getResources extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        ServletContext servletContext = this.getServletContext();
        InputStream resourceAsStream = servletContext.getResourceAsStream("/WEB-INF/classes/db.properties");//注意这是相对路径 / :当前项目 /WEB-INF:当前项目下的WEB-INF目录
        Properties properties = new Properties();
        properties.load(resourceAsStream);
        String userName = (String) properties.get("username");
        String password = (String) properties.get("password");

        resp.getWriter().print(userName+password);

    }
}
```

```xml
<servlet>
    <servlet-name>getRes</servlet-name>
    <servlet-class>com.balance.getResources</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>getRes</servlet-name>
    <url-pattern>/getRes</url-pattern>
</servlet-mapping>
```

当我们使用上述的代码完成了任务之后，可以思考可不可以直接读取而不用servletContext呢

```java
properties.load(new FileReader("/WEB-INF/classes/db.properties"));
```

不幸的是，这是不可以的，因为FileReader使用的是文件系统路径，而servletContext.getResourceAsStream使用的是web路径，网站会报找不到资源文件的错误。

如果真的想用文件路径，也是有方法的，如下

```java
String path =this.getServletContext().getRealPath("/WEB-INF/classes/db.properties");
FileInputStream in=new FileInputStream(path);
Properties props=new Properties();
props.load(in);
```

getRealPath将web路径转为实际文件系统路径。

![image-20231109164803246](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311091648255.png)
