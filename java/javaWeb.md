# JavaWeb

## Http

HTTP的头域包括通用头、请求头、响应头和实体头四个部分。每个头域由一个域名，冒号（:）和域值三部分组成（说白了就是键值对）。

- 通用头：是客户端和服务器都可以使用的头部，可以在客户端、服务器和其他应用程序之间提供一些非常有用的通用功能，如Date头部。
- 请求头：是请求报文特有的，它们为服务器提供了一些额外信息，比如客户端希望接收什么类型的数据，如Accept头部。
- 响应头：便于客户端提供信息，比如，客服端在与哪种类型的服务器进行交互，如Server头部。
- 实体头：指的是用于应对实体主体部分的头部，比如，可以用实体头部来说明实体主体部分的数据类型，如Content-Type头部。

### HTTP通用头

通用头域包含请求和响应消息都支持的头域，通用头域包含缓存头部Cache-Control、Pragma及信息性头部Connection、Date、Transfer-Encoding、Update、Via。

| **字段名**            | **含义**                                                     |
| --------------------- | ------------------------------------------------------------ |
| **Cache-Control**     | 告诉所有的缓存机制是否可用缓存及哪种类型。例：Cache-Control：no-cache |
| **Pragma**            | Pragma头域用来包含实现特定的指令，最常用的是Pragma:no-cache。在HTTP/1.1协议中，它的含义和Cache- Control:no-cache相同。 |
| **Connection**        | Connection表示是否需要持久连接。如果Servlet看到这里的值为“Keep-Alive”，或者看到请求使用的是HTTP 1.1（HTTP 1.1默认进行持久连接），它就可以利用持久连接的优点，当页面包含多个元素时（例如Applet，图片），显著地减少下载所需要的时间。 |
| **Date**              | Date头域表示消息发送的时间，服务器响应中要包含这个头部，因为缓存在评估响应的新鲜度时要用到，其时间的描述格式由RFC822定义。 |
| **Transfer-Encoding** | WEB 服务器表明自己对本响应消息体（不是消息体里面的对象）作了怎样的编码，比如是否分块（chunked），例如：Transfer-Encoding: chunked |
| **Upgrade**           | 它可以指定另一种可能完全不同的协议，如HTTP/1.1客户端可以向服务器发送一条HTTP/1.0请求，其中包含值为“HTTP/1.1”的Update头部，这样客户端就可以测试一下服务器是否也使用HTTP/1.1了。 |
| **Via**               | 列出从客户端到 OCS 或者相反方向的响应经过了哪些代理服务器，他们用什么协议（和版本）发送的请求。 |

### **请求头**

请求头用于说明是谁或什么在发送请求、请求源于何处，或者客户端的喜好及能力。服务器可以根据请求头部给出的客户端信息，试着为客户端提供更好的响应。

| **字段名**            | **含义**                                                     |
| --------------------- | ------------------------------------------------------------ |
| **Accept**            | 指定客户端能够接受的内容类型。例：Accept: text/plain, text/html |
| **Accept-Charset**    | 浏览器可以接受的字符编码集。                                 |
| **Accept-Encoding**   | 指定浏览器可以支持web服务器返回内容压缩编码类型              |
| **Accept-Language**   | 浏览器可以接受的语言                                         |
| **Authorization**     | 当客户端接收到来自WEB服务器的 WWW-Authenticate 响应时，用该头部来回应自己的身份验证信息给WEB服务器。 |
| **cookie**            | HTTP请求发送时，会把保存在该请求域名下所有的cookie值一起发送给web服务器。 |
| **If-Modified-Since** | 如果请求的部分时间修改则请求成功，未被修改返回[304](https://so.csdn.net/so/search?q=304&spm=1001.2101.3001.7020)代码 |
| **If-None-Match**     | 如果内容未改变返回304代码，参数为服务器先前发送的Etag，与服务器回应的Etag比较判断是否改变 |
| **Range**             | 浏览器（比如 Flashget 多线程下载时）告诉 WEB 服务器自己想取对象的哪部分。 |
| **User-Agent**        | User-Agent的内容包含发出的用户信息                           |
| **Content-Type**      | 请求的与实体对应MIME信息。例：Content-type：application/x-www-form-urlencoded |
| **Host**              | 指定请求的服务器和端口。例：Host：www.baidu.com              |
| **Referer**           | 先前网页的地址，告诉服务器来源。例：Referer：http://www.baidu.com/1.html |
| **User-Agent**        | 浏览器表明自己的身份（是哪种浏览器）。                       |

### **响应头**

响应头向客户端提供一些额外信息，比如谁在发送响应、响应者的功能，甚至与响应相关的一些特殊指令。这些头部有助于客户端处理响应，并在将来发起更好的请求。

| 字段名            | 含义                                                         |
| ----------------- | ------------------------------------------------------------ |
| **Age**           | 当代理服务器用自己缓存的实体去响应请求时，用该头部表明该实体从产生到现在经过多长时间了。 |
| **Server**        | WEB 服务器表明自己是什么软件及版本等信息。例如：Server：Apache/2.0.61 (Unix) |
| **Accept-Ranges** | WEB服务器表明自己是否接受获取其某个实体的一部分（比如文件的一部分）的请求。bytes：表示接受，none：表示不接受。 |
| **Vary**          | WEB服务器用该头部的内容告诉 Cache 服务器，在什么条件下才能用本响应所返回的对象响应后续的请求。假如源WEB服务器在接到第一个请求消息时，其响应消息的头部为：Content-Encoding: gzip; Vary: Content-Encoding，那么Cache服务器会分析后续请求消息的头部，检查其Accept-Encoding，是否跟先前响应的Vary头部值一致，即是否使用相同的内容编码方法，这样就可以防止Cache服务器用自己Cache 里面压缩后的实体响应给不具备解压能力的浏览器。例如：Vary：Accept-Encoding。 |

### 实体头

实体头部提供了有关实体及其内容的大量信息，从有关对象类型的信息，到能够对资源使用的各种有效的请求方法。总之，实体头部可以告知接收者它在对什么进行处理。请求消息和响应消息都可以包含实体信息，实体信息一般由**实体头域**和**实体**组成。

实体头域包含关于实体的原信息

实体头包括

- 信息性头部Allow、Location
- 内容头部Content-Base、Content-Encoding、Content-Language、Content-Length、Content-Location、Content-MD5、Content-Range、Content-Type
- 缓存头部Etag、Expires、Last-Modified、extension-header。

| **字段名**           | **含义**                                                     |
| -------------------- | ------------------------------------------------------------ |
| **Allow**            | 服务器支持哪些请求方法（如GET、POST等）。                    |
| **Location**         | 表示客户应当到哪里去提取文档，用于将接收端定位到资源的位置（URL）上。Location通常不是直接设置的，而是通过HttpServletResponse的sendRedirect方法，该方法同时设置状态代码为302。 |
| **Content-Base**     | 解析主体中的相对URL时使用的基础URL。                         |
| **Content-Encoding** | web服务支持的返回内容压缩编码类型                            |
| **Content-Language** | 响应体语言。例：Content-Language：en,ch                      |
| **Content-Length**   | 响应长度                                                     |
| **Content-type**     | WEB 服务器告诉浏览器自己响应的对象的类型。例如：Content-Type：application/xml |
| **Set-Cookie**       | 设置HTTP cookie。例Set-Cookie:UserID=JonhnDoe;Max-Age=3600;Version=1 |
| **ETag**             | 请求变量的实体标签的当前值。例：ETag：“737060cd8c284d8af7ad3082f209582d” |
| **Expires**          | WEB服务器表明该实体将在什么时候过期，对于过期了的对象，只有在跟WEB服务器验证了其有效性后，才能用来响应客户请求。是 HTTP/1.0 的头部。例如：Expires：Sat, 23 May 2009 10:02:12 GMT |

## **Tomcat**

### **安装与配置**

> **https://www.cnblogs.com/winton-nfs/p/13831076.html**

**注意一下jdk版本需要对应tamcat版本，否则会不成功**

### **目录说明**

**bin目录下**

**![image-20231102160939947](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311021609501.png)**

**![image-20231102161256050](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311021612147.png)**

**![image-20231102161346358](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311021613400.png)**

## **Maven**

**为什么要使用Maven？它能帮助我们解决什么问题？**

- **添加第三方 jar 包:**

  **在今天的 JavaEE 开发领域，有大量的第三方框架和工具可以供我们使用。要使用这些 jar 包最简单 的方法就是复制粘贴到 WEB-INF/lib 目录下。但是这会导致每次创建一个新的工程就需要将 jar 包重复复 制到 lib 目录下，从而造成工作区中存在大量重复的文件，让我们的工程显得很臃肿。 而使用 Maven 后每个 jar 包本身只在本地仓库中保存一份，需要 jar 包的工程只需要以坐标的方式 简单的引用一下就可以了。不仅极大的节约了存储空间，让项目更轻巧，更避免了重复文件太多而造成 的混乱。**

- **jar 包之间的依赖关系**

  **jar 包往往不是孤立存在的，很多 jar 包都需要在其他 jar 包的支持下才能够正常工作，我们称之为 jar 包之间的依赖关系。引入 Maven 后，Maven 就可以替我们自动的将当前 jar 包所依赖的其他所有 jar 包全部导入进来， 无需人工参与，节约了我们大量的时间和精力。用实际例子来说明就是：通过 Maven 导入 commons-fileupload-1.3.jar 后，commons-io-2.0.1.jar 会被自动导入，程序员不必了解这个依赖关系。**

### **配置**

**新增环境变量名：MAVEN_HOME 和 M2_HOME；**

```
MAVEN_HOME: 安装目录bin目录的上一级目录
M2_HOME:安装目录bin目录
```

**在系统变量Path中新增**

```
 Path：%MAVEN_HOME%\bin            
```

**更改镜像源，加速下载**

**在maven安装目录conf目录下，修改settings.xml,在mirrors中注释原有镜像新增阿里云镜像**

```xml
<mirror>
    <id>alimaven</id>
    <name>aliyun maven</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
    <mirrorOf>central</mirrorOf>
</mirror>
```

**修改本地仓库，同样是在maven安装目录conf目录下，修改settings.xml，默认安装在**

```xml
Default: ${user.home}/.m2/repository
```

**也就当前用户的.m2目录下，本地仓库的作用是可以直接引用下载到本地的jar包，无需远程重新下载。**

### **idea中使用**

**高版本无法在创建项目时更改maven配置，会使用默认的maven，注意**

**![image-20231104163049800](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041630204.png)**

**一个干净的maven项目结构**

**![image-20231104163417506](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041634564.png)**

**maven项目之webapp项目结构，只有web应用才会有**

**![image-20231104163449496](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041640506.png)**

#### **idea配置本地Tomcat**

##### **步一：**

**![image-20231104163907453](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041639645.png)**

##### **步二：**

**![image-20231104163950713](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041639912.png)**

##### **步三：**

**![image-20231104164042117](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041640406.png)**

##### **步四：**

**![image-20231104164149364](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041641114.png)**

##### **步五：添加artifacts作为服务启动项**

**![image-20231104165104010](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041651805.png)**

**![image-20231104164802228](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041648442.png)**

##### **maven项目**

**![image-20231104165332984](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311041653813.png)**

##### **pom.xml**

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

## **Servlet**

**Servlet就是Sun公司开发动态web的一门技术**

**Sun在这些API中提供一个接口叫做：Servlet，如果你想开发一个Servlet程序，只需要两个步骤**

- **编写一个类，实现Servlet接口**
- **把开发好的java类部署到web服务器中**

**把实现了Servlet接口的java程序叫做：Servlet**

### **第一个servlet程序**

**创建一个空maven项目，若没有空项目maven选项，创建一个webapp原型项目，删除不需要的文件即可。**

**在该项目pom.xml中添加servlet依赖**

```xml
<dependencies>
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
    </dependency>
</dependencies>
```

**之后根项目右键创建module，命名为servlet-01，webapp原型项目**

**上面的操作创建了父子项目，父项目的依赖子项目可以直接用**

**之后在src/main/java包下，新建包，servlet，在其中创建HelloServlet类，完成后目录如下**

**![image-20231106161327792](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311061613364.png)**

**HelloServlet类继承默认实现的HttpServlet类，重写doGet方法**

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

**再在web.xml的web-app标签中编写servlet的映射如下**

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

- **我们写的是java程序，但是要通过浏览器访问，浏览器需要连接web服务器，所以我们需要再web服务中即web.xml中注册我们所写的Servlet,并且要给他一个浏览器可以访问的路径**

**最后配置Tomcat，运行**

```sql
http://localhost:8080/servlet_01_war/hello

# /servlet_01_war 配置tomcat指定的访问路径
# /hello 我们定义的Servlet请求路径
```

### **mapping相关问题**

**可以使用通配符来响应不在定义之内的页面**

```xml
<servlet-mapping>
    <servlet-name>error</servlet-name>
    <url-pattern>/*</url-pattern>
</servlet-mapping>
```

**设定了固有的路径优先级最高，若找不到才会返回通配符响应的页面。**

**还可以使用如下形式**

```xml
<servlet-mapping>
    <servlet-name>hello</servlet-name>
    <url-pattern>*.xxx</url-pattern>
</servlet-mapping>
```

**以上述方式进行映射，注意前面不能加具体路径，如/hello/*.xxx，会报错。**

### **ServletContext**

**web容器在启动的时候，会为每个web程序都创建一个对应的ServleContext对象，它代表了当前的web应用**

#### **共享数据**

**在一个web程序之中会有多个servlet，我们在一个servlet中向ServleContext存放数据后，在另一个servlet中也可以访问。**

**在第一个servlet中存**

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

**在第二个servlet取**

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

**在web.xml中配置路径**

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

**注意需要先访问第一个再访问第二个，因为存了才能取。**

#### **获取初始化参数**

**web.xml 可以有一些初始化参数，配置如下**

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

**可以通过ServletContext获取到**

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

#### **请求转发**

**servletContext可以帮助我们完成请求转发(307)功能。注意，这不是重定向(302)，不会改变路径**

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

**![image-20231109152958466](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311091530299.png)**

#### **读取资源文件**

**当我们有两份资源文件，我们如何读取呢？**

**![image-20231109153240804](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311091532455.png)**

**首先，当项目运行时，这两份文件都会被打包到target文件目录下，classes中，我们俗称为类路径**

**注意在com.balance包下可能不会正常打包，可以在pom.xml加入**

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

**![image-20231109153706813](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311091537042.png)**

**使用servletContext.getResourceAsStream将资源转换为流，从而Properties对象可以加载**

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

**当我们使用上述的代码完成了任务之后，可以思考可不可以直接读取而不用servletContext呢**

```java
properties.load(new FileReader("/WEB-INF/classes/db.properties"));
```

**不幸的是，这是不可以的，因为FileReader使用的是文件系统路径，而servletContext.getResourceAsStream使用的是web路径，网站会报找不到资源文件的错误。**

**如果真的想用文件路径，也是有方法的，如下**

```java
String path =this.getServletContext().getRealPath("/WEB-INF/classes/db.properties");
FileInputStream in=new FileInputStream(path);
Properties props=new Properties();
props.load(in);
```

**getRealPath将web路径转为实际文件系统路径。**

![image-20231109164803246](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311091648255.png)

### **HttpServletResponse**

**web服务器接收到客户端的http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest，和一个代表响应的一个HttpServletResponse**

- **如果要获取客户端请求过来的参数：HttpServletRequest**
- **如果要给客户端响应一些信息：HttpServletResponse**

#### **下载文件**

```java
public class FileDownLoader extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //1.获取下载路径
        String realPath = this.getServletContext().getRealPath("/WEB-INF/classes/img.png");
        System.out.println(realPath);
        //2.获取下载文件名 通过lastIndexOf截取最后的文件名
        String fileName = realPath.substring(realPath.lastIndexOf("\\") + 1);
        //3.设置响应头信息 注意设置编码，以免出现中文名称乱码等问题
        resp.setHeader("Content-Disposition" , "attachment;filename="+ URLEncoder.encode(fileName,"UTF-8"));
        //4.获取下载文件的输入流
        FileInputStream in = new FileInputStream(realPath);
        //5.创建缓冲区
        int len=0;
        byte[] bytes = new byte[1024];
        //6.获取下载文件的输出流
        ServletOutputStream outputStream = resp.getOutputStream();
        while((len=in.read(bytes))!=-1){
            outputStream.write(bytes,0,len);
        }
        in.close();
        outputStream.close();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

```xml
<servlet>
    <servlet-name>fileDown</servlet-name>
    <servlet-class>FileDownLoader</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>fileDown</servlet-name>
    <url-pattern>/fileDown</url-pattern>
</servlet-mapping>
```

#### **验证码**

```java
public class ImageServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //让浏览器 5s 刷新一次
        resp.setHeader("refresh","5");

        //在内存中创建一个图片
        BufferedImage img = new BufferedImage(80, 20, BufferedImage.TYPE_INT_RGB);
        //得到图片
        Graphics2D graphics = (Graphics2D) img.getGraphics();//笔
        //设置图片的背景颜色
        graphics.setColor(Color.white);//设置颜色
        graphics.fillRect(0,0,80,20);//填充
        //给图片写数据
        graphics.setColor(Color.BLUE);
        graphics.setFont(new Font(null,Font.BOLD,20));
        graphics.drawString(makeNum(),0,20);

        //告诉浏览器，这个请求用图片形式打开
        resp.setContentType("image/jpg");
        //让浏览器不缓存
        resp.setDateHeader("expires",-1);
        resp.setHeader("Cache-Control","no-cache");
        resp.setHeader("Pragma","np-cache");

        //把图片写给浏览器
        ImageIO.write(img,"jpg",resp.getOutputStream());
    }

    private String makeNum(){
        Random random = new Random();
        String num = random.nextInt(9999999)+"";
        StringBuffer stringBuffer = new StringBuffer();
        for (int i = 0; i <7-num.length() ; i++) {//保证是7位数，不足7位在前面填充0
            stringBuffer.append("0");
        }
        num= stringBuffer +num;
        return num;
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}

```

```xml
<servlet>
    <servlet-name>ImageServlet</servlet-name>
    <servlet-class>ImageServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>ImageServlet</servlet-name>
    <url-pattern>/img</url-pattern>
</servlet-mapping>
```

#### **实现重定向**

```java
public class RedirectHtml extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        /*  相当于
            resp.setHeader("Location","/servlet_03_down_war/img");
            resp.setStatus(302);
         */
        resp.sendRedirect("/servlet_03_down_war/img");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

```xml
<servlet>
        <servlet-name>RedirectHtml</servlet-name>
        <servlet-class>RedirectHtml</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>RedirectHtml</servlet-name>
        <url-pattern>/red</url-pattern>
    </servlet-mapping>
```

### **HttpServletRequest**

#### 获取前端传递的参数，请求转发

![image-20231115134108238](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202311151341161.png)

```java
public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //后台中文乱码问题 后面会学过滤器预处理请求，这部分会放在那里
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");

        String username=req.getParameter("username");
        String password=req.getParameter("password");
        String[] hobbies= req.getParameterValues("hobbies");//获得同名多个值用这个
        System.out.println("========================");
        System.out.println(username);
        System.out.println(password);
        System.out.println(Arrays.toString(hobbies));
        System.out.println("========================");

        //通过请求转发
        //这里的 / 代表当前的web应用
        req.getRequestDispatcher("/success.jsp").forward(req,resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<h1>登录</h1>
    <div style="text-align: center">
        <%--这里表单表示的意思：以post方式提交表单，提交到我们的login请求--%>
        <form action="${pageContext.request.contextPath}/loginServlet" method="post">
            用户名：<input type="text" name="username"> <br>
            密码：<input type="password" name="password"> <br>
            爱好：
            <input type="checkbox" name="hobbies" value="concert">演唱会
            <input type="checkbox" name="hobbies" value="sing">唱歌
            <input type="checkbox" name="hobbies" value="draw">画画
            <input type="checkbox" name="hobbies" value="dance">跳舞
            <br>
            <input type="submit">
        </form>
    </div>
</body>
</html>
```

```xml
<servlet>
        <servlet-name>loginServlet</servlet-name>
        <servlet-class>LoginServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>loginServlet</servlet-name>
        <url-pattern>/loginServlet</url-pattern>
    </servlet-mapping>
```

## Cookie & Session

### Cookie

Cookie是浏览器在本地存储数据的一种方式，理论上是可以存储任何数据的，前提是数据的类型必须是String类型。

关于Cookie的三个问题：Cookie从哪来？Cookie到哪去？Cookie有什么用？

- **Cookie从哪来**：Cookie是从浏览器来的，服务器在响应时就会通过Set-Cookie字段将Cookie返回给浏览器，浏览器在下次访问服务器时，就会带上Cookie；
- **Cookie到哪去**：Cookie还是到服务器去，在下一次的浏览器访问服务器的时候，浏览器会带上服务器返回的Cookie去访问服务器；
- **Cookie有什么用**：Cookie是可以携带任何数据的，一般情况下我们是令Cookie来保存用户的登录信息，用来识别用户的身份，这样用户就不需要一直执行登录操作了。

#### **常用操作**

```java
Cookie[] cookies=req.getCookies();//获得cookie
cookie.getName();//获得cookie中的key
cookie.getValue();//获得cookie中的Value
Cookie cookie=new Cookie("lastLoginTime",System.currentTimeMillis()+"");//新建一个cookie
cookie.setMaxAge(24*60*60);//设置cookie的有效期
resp.addCookie(cookie);//响应给客户端一个cookie
```

#### **示例**

```java
public class SetCookies extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决中文乱码
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setHeader("Content-Type","text/html;charset=utf-8");

        PrintWriter out = resp.getWriter();
        //服务器从客户端获取cookie，有多个数组接收
        Cookie[] cookies = req.getCookies();

        //判断是否有cookie，第一次是没有的，需要设置
        if(cookies!=null){
            out.write("你上次访问的时间是：");
            for (int i = 0; i < cookies.length; i++) {
                Cookie cookie=cookies[i];
                //获取cookie的名字
                if(cookie.getName().equals("lastLoginTime")){
                    //获取cookie中的值
                    long lastLoginTime=Long.parseLong(cookie.getValue());
                    Date date=new Date(lastLoginTime);
                    out.write(date.toLocaleString());
                }
            }
        }else{
            out.print("这是第一次访问");
        }
        //添加最新的cookie
        Cookie lastLoginTime = new Cookie("lastLoginTime", System.currentTimeMillis() + "");
        resp.addCookie(lastLoginTime);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doGet(req, resp);
    }
}
```

```xml
<servlet>
    <servlet-name>setCookies</servlet-name>
    <servlet-class>SetCookies</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>setCookies</servlet-name>
    <url-pattern>/s1</url-pattern>
</servlet-mapping>
```

**注意上述代码中设置了 Content-Type ，因为仅仅设置 setCharacterEncoding ，只是响应回去的编码规则为utf-8，但是浏览会按默认规则进行解释，依旧会导致乱码**

#### 一些问题

**一个网站cookie是否存在上限！细节问题**

- 一个cookie只能保存一个信息
- 一个Web站点可以给浏览器发送多个cookie，IE5-IE6:20个，IE7:50个
- cookie大小限制为4kb

**删除cookie**：

- 不设置有效期，关闭浏览器，自动失效
- 设置有效期为0

**编解码问题：**

`URLDecoder` 和 `URLEncoder` 是 Java 中用于处理 URL 编码和解码的两个类。

1. **`URLEncoder`：**

   - 用于将字符串编码为符合 URL 规范的格式。主要用途是将字符串转换成适合放置在 URL 中的安全字符串。

   - 在 URL 中，一些字符（例如空格、问号、等号等）是不允许直接出现的，需要通过编码替换。`URLEncoder` 提供了 `encode(String s, String encoding)` 方法，将字符串进行 URL 编码。

     ```java
     String encodedString = URLEncoder.encode("This is a test string", "UTF-8");
     ```

   - 在上面的例子中，`encodedString` 将包含被替换成 `%` 后跟两位十六进制值的字符，以确保在 URL 中的正确传输。

2. **`URLDecoder`：**

   - 用于解码已经在 URL 中编码的字符串。当你从 URL 中获取参数时，这些参数可能已经被编码，需要使用 `URLDecoder` 将其还原。

   - 提供了 `decode(String s, String encoding)` 方法，将已编码的字符串进行解码。

     ```java
     String decodedString = URLDecoder.decode("This%20is%20a%20test%20string", "UTF-8");
     ```

   - 在上面的例子中，`decodedString` 将包含被还原成空格的原始字符串。

这些类是在处理涉及 URL 的网络编程或者 Web 开发时非常有用的工具，确保 URL 中的参数和数据正确传输，同时避免由于特殊字符而导致的问题。

### Session

上面的Cookie中，我们说到Cookie从服务器来，到服务器去，可以保存任何数据，一般是用来保存用户的登录信息。但是，服务器这边到底具体如何来区分用户的信息的呢？这里就需要用到Session(也称为会话)了，**Session就是服务器这边用来实现用户身份区分的一种机制**，一般和Cookie配合使用（也可以不和Cookie配合使用）。

什么是Session:

- 服务器会给每一个用户（浏览器）创建一个Session对象；
- 一个Session独占一个浏览器，只要浏览器没有关闭，这个Session就存在
- 用户登录后，整个网站它都可以访问！—>保存用户的信息；保存购物车的信息

#### 示例

**增加session属性，得到session id**

```java
public class SessionDemo01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码问题
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8");

        //得到Session
        HttpSession session=req.getSession();
        //给Session中存东西
        session.setAttribute("name",new Person("balance",20));
        //获取Session的id
        String sessionId=session.getId();
        //判断Session是不是新创建
        if(session.isNew()){
            resp.getWriter().write("session创建成功，ID为："+sessionId);
        }else{
            resp.getWriter().write("session已经存在，ID为："+sessionId);
        }

        // Session创建的时候做了什么事情
        // Cookie cookie=new Cookie("name","达西");
        // resp.addCookie(cookie);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

```xml
<servlet-mapping>
    <servlet-name>sessionDemo01</servlet-name>
    <url-pattern>/session01</url-pattern>
</servlet-mapping>
```

**获得刚刚设置的session属性**

```java
public class SessionDemo02 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码问题
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8");

        //得到Session
        HttpSession session = req.getSession();
        //取session存储的属性值
        Person person = (Person) session.getAttribute("name");
        System.out.println(person.toString());
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}
```

**移除Session中属性，设置Session为无效**

```java
public class SessionDemo03 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //解决乱码问题
        req.setCharacterEncoding("utf-8");
        resp.setCharacterEncoding("utf-8");
        resp.setContentType("text/html;charset=utf-8");

        //得到Session
        HttpSession session = req.getSession();
        //移除session中属性
        session.removeAttribute("name");
        //注销session
        session.invalidate();
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        super.doPost(req, resp);
    }
}

```

**还可以在web.xml中设置session有效时间**

```xml
<!--设置Session默认的失效时间-->
<session-config>
    <!--15分钟之后Session自动失效，以分钟为单位-->
    <session-timeout>15</session-timeout>
</session-config>
```

### 两者区别

**相同点：**

1. **状态维护：** Cookies和Session都是用于在不同的HTTP请求之间维护状态信息的机制。
2. **存储信息：** 两者都可以用于存储用户的特定信息，以便在用户访问网站的不同页面时使用。

**不同点：**

1. **存储位置：**
   - **Cookie：** 存储在客户端（浏览器）中，以文本文件的形式存储在用户的计算机上。
   - **Session：** 存储在服务器上，通常存储在内存中，尽管有时也可能被持久化到数据库或文件系统中。
2. **安全性：**
   - **Cookie：** 相对较不安全，因为存储在客户端，可以被用户查看或修改。
   - **Session：** 相对较安全，因为存储在服务器端，客户端无法直接访问或修改。
3. **存储容量：**
   - **Cookie：** 有限制，通常每个域名下的Cookie总大小限制为4KB。
   - **Session：** 理论上可以存储更多的数据，取决于服务器的配置和可用资源。
4. **生命周期：**
   - **Cookie：** 可以设置过期时间，可以是会话级别（浏览器关闭即失效）或持久性的（在一定时间内有效）。
   - **Session：** 通常与用户的会话周期相同，**会话结束时失效**。
5. **使用场景：**
   - **Cookie：** 适用于需要在客户端保持状态信息的场景，例如记住用户登录状态、用户偏好设置等。
   - **Session：** 适用于需要在服务器端保持状态信息的场景，例如跟踪用户在网站上的活动、购物车信息等。

在实际应用中，通常会结合使用Cookies和Session，以发挥它们各自的优势。例如，可以使用Session来存储敏感信息，而使用Cookie来标识会话。

