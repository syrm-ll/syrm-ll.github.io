# Servlet 3.0 规范

## 名词定义

### Servlet

Servlet 是基于 Java 技术的 web 组件，容器托管的，用于生成动态内容。像其他基于 Java 的组件技术一样， Servlet 也是基于平台无关的 Java 类格式，被编译为平台无关的字节码，可以被基于 Java 技术的 web server 动态加载并运行。容器，有时候也叫做 servlet 引擎，是 web server 为支持 servlet 功能扩展的部分。客户端 通过 Servlet 容器实现的请求/应答模型与 Servlet 交互。 

### Servlet 容器

Servlet 容器是 web server 或 application server 的一部分，提供基于请求/响应发送模型的网络服务，解码基 于 MIME 的请求，并且格式化基于 MIME 的响应。Servlet 容器也包含了管理 Servlet 生命周期。

 Servlet 容器可以嵌入到宿主的 web server 中，或者通过 Web Server 的本地扩展 API 单独作为附加组件安装。 Servelt 容器也可能内嵌或安装到包含 web 功能的 application server 中。 所有 Servlet 容器必须支持基于 HTTP 协议的请求/响应模型，比如像基于 HTTPS（HTTP over SSL）协议的 请求/应答模型可以选择性的支持。容器必须实现的 HTTP 协议版本包含 HTTP/1.0 和 HTTP/1.1。因为容器 或许支持 RFC2616 (HTTP/1.1)描述的缓存机制，缓存机制可能在将客户端请求交给 Servlet 处理之前修改它 们，也可能在将 Servlet 生成的响应发送给客户端之前修改它们，或者可能根据 RFC2616 规范直接对请求 作出响应而不交给 Servlet 进行处理。

 Servlet 容器应该使 Servlet 执行在一个安全限制的环境中。在 Java 平台标准版（J2SE, v.1.3 或更高） 或者 Java 平台企业版(Java EE, v.1.3 或更高) 的环境下，这些限制应该被放置在 Java 平台定义的安全许可架构 中。比如，高端的 application server 为了保证容器的其他组件不受到负面影响可能会限制 Thread 对象的创 建。

 Java SE 6 是构建 Servlet 容器低的 Java 平台版本。 

> 以下是一个典型的事件序列：
>
>  1、客户端（如 web 浏览器）发送一个 HTTP 请求到 web 服务器；
>
>  2、Web 服务器接收到请求并且交给 servlet 容器处理，servlet 容器可以运行在与宿主 web 服务器同一个进 程中，也可以是同一主机的不同进程，或者位于不同的主机的 web 服务器中，对请求进行处理。
>
>  3、servlet 容器根据 servlet 配置选择相应的 servlet，并使用代表请求和响应对象的参数进行调用。
>
>  4、servlet 通过请求对象得到远程用户，HTTP POST 参数和其他有关数据可能作为请求的一部分随请求一 起发送过来。Servlet 执行我们编写的任意的逻辑，然后动态产生响应内容发送回客户端。发送数据到客户 端是通过响应对象完成的。
>
>  5、一旦 servlet 完成请求的处理，servlet 容器必须确保响应正确的刷出，并且将控制权还给宿主 Web 服务 器。 
>
> 
>
> --摘自：Servlet 3.0规范

## Servelt 接口

Servlet 接口是 Java Servlet API 的核心抽象。所有 Servlet 类必须直接或间接的实现该接口，或者更通常做法 是通过继承一个实现了该接口的类从而复用许多共性功能。目前有 GenericServlet 和 HttpServlet 这两个类实 现了 Servlet 接口。大多数情况下，开发者只需要继承 HttpServlet 去实现自己的 Servlet 即可。 



### 2.1 请求处理方法 

Servlet 基础接口定义了用于客户端请求处理的 service 方法。当有请求到达时，该方法由 servlet 容器路由到 一个 servlet 实例。 Web 应用程序的并发请求处理通常需要 Web 开发人员去设计适合多线程执行的 Servlet，从而保证 service 方法能在一个特定时间点处理多线程并发执行。（注：即 Servlet 默认是线程不安全的，需要开发人员处理 多线程问题） 通常 Web 容器对于并发请求将使用同一个 servlet 处理，并且在不同的线程中并发执行 service 方法。 

### 2.1.1 基于 Http 规范的请求处理方法 

HttpServlet 抽象子类在 Servlet 接口基础之上添加了些协议相关的方法，并且这些方法能根据 HTTP 请求类 型自动的由 HttpServlet 中实现的 service 方法转发到相应的协议相关的处理方法上。这些方法是：

 ■ doGet 处理 HTTP GET 请求

 ■ doPost 处理 HTTP POST 请求

 ■ doPut 处理 HTTP PUT 请求 

■ doDelete 处理 HTTP DELETE 请求 

■ doHead 处理 HTTP HEAD 请求 

■ doOptions 处理 HTTP OPTIONS 请求

 ■ doTrace 处理 HTTP TRACE 请求 

一般情况下，当开发基于 HTTP 协议的 Servlet 时，Servlet 开发人员将仅去实现 doGet 和 doPost 请求处理 方法即可。如果开发人员想使用其他处理方法，其使用方式跟之前的是类似的，即 HTTP 编程都是类似。 

### 2.1.3 有条件 GET 支持 

HttpServlet 定义了用于支持有条件 GET 操作的 **getLastModified** 方法。所谓的有条件 GET 操作是指客户端 通过 GET 请求获取资源时，当资源自第一次获取那个时间点发生更改后才再次发生数据，否则将使用客户 端缓存的数据。在一些适当的场合，实现此方法可以更有效的利用网络资源，减少不必要的数据发送。 

## Servelt 实例和生命周期

### 实例化规则

通过注解描述的（第 8 章 注解和可插拔性）或者在 Web 应用程序的部署描述符（即web.xml）（第 14 章 部署描述符） 中描述的 servlet 声明，控制着 servlet 容器如何提供 servlet 实例。 

对于未托管在分布式环境中（默认）的 servlet 而言，servlet 容器对于每一个 Servlet 声明必须且只能产生一 个实例。不过，如果 Servlet 实现了 SingleThreadModel 接口，servlet 容器可以选择实例化多个实例以便处 理高负荷请求或者串行化请求到一个特定实例。

 如果 servlet 以分布式方式进行部署，容器可以为每个虚拟机（JVM）的每个 Servlet 声明产生一个实例。但 是，如果在分布式环境中 servlet 实现了 SingleThreadModel 接口，此时容器可以为每个容器的 JVM 实例化 多个 Servlet 实例。 

> SingleThreadModel  接口，略。



### 生命周期

声明周期可以通过 javax.servlet.Servlet 接口中的 **init、service 和 destroy** 这些 API 来表示，所有 Servlet 必须直接或间接的实现 GenericServlet 或 HttpServlet 抽象类。 

#### 加载和实例化

Servlet 容器负责加载和实例化 Servlet。加载和实例化可以发生在容器启动时，或者延迟初始化直到容器决 定有请求需要处理时。

#### 初始化

一旦一个 Servlet 对象实例化完毕，容器接下来必须在处理客户端请求之前初始化该 Servlet 实例。

初始化 的目的是以便 Servlet 能读取持久化配置数据，初始化一些代价高的资源（比如 JDBC API 连接），或者执 行一些一次性的动作。容器通过调用 Servlet 实例的 init 方法完成初始化，init 方法定义在 Servlet 接口中， 并且提供一个唯一的 ServletConfig 接口实现的对象作为参数，该对象每个 Servlet 实例一个。 

配置对象允许 Servlet 访问由 Web 应用配置信息提供的键-值对的初始化参数。该配置对象也提供给 Servlet 去访问一个ServletContext对象，ServletContext描述了Servlet的运行时环境。

**在初始化阶段，servlet 实现可能抛出 UnavailableException 或 ServletException 异常。在这种情况下，Servlet 不能放置到活动服务中，而且 Servlet 容器必须释放它。如果初始化没有成功，destroy 方法不应该被调用。**

#### 请求处理

Servlet 完成初始化后，Servlet 容器就可以使用它处理客户端请求了。客户端请求由 ServletRequest 类型的 request 对象表示。Servlet 封装响应并返回给请求的客户端，该响应由 ServletResponse 类型的 response 对象 表示。这两个对象（request 和 response）是由容器通过参数传递到 Servlet 接口的 service 方法的。 

在 HTTP 请求的场景下，容器提供的请求和响应对象具体类型分别是 HttpServletRequest 和 HttpServletResponse。 

**详见HttpServletRequest 和 HttpServletResponse的API。**

Servelt的请求处理一定是多线程的， 并发同步问题由编程保证！

请求处理可能会抛出ServletException  和 UnavailableException 异常， Servlet容器处理这些异常返回404 或 503



**异步处理，略。**



#### 终止

当 Servlet 容器确定 servlet 应该从服务中移除时（比如UnavailableException 异常为用不可用，或者容器关闭时），将调用 Servlet 接口的 destroy 方法以允许 Servlet 释放它使 用的任何资源和保存任何持久化的状态。

在 servlet 容器调用 destroy 方法之前，它必须让当前正在执行 service 方法的任何线程完成执行，或者超过 了服务器定义的时间限制。 
一旦调用了 servlet 实例的 destroy 方法，容器无法再路由其他请求到该 servlet 实例了。



## Request：请求对象

请求对象封装了客户端请求的所有信息。在 HTTP协议中，这些信息是从客户端发送到服务器请求的 HTTP 头部和消息体。 

### HTTP协议参数​：查询参数 和 请求数据​

当请求是HttpServletRequest对象时，有如下API：(详见HttpServletRequest API文档)

```java
getParameter
getParameterNames
getParameterValues
getParameterMap 
getParameterValues
```

> 查询字符串和 POST请求的数据被汇总到请求参数集合中。查询字符串数据在 POST数据之前发送。例如， 如果请求由查询字符串 a =hello 和 POST 数据 a=goodbye&a=world 组成，得到的参数集合顺序将是 =(hello,goodbye,world)。
>
>  这些API不会暴露GET请求（HTTP 1.1所定义的）的路径参数。他们必须从getRequestURI方法或getPathInfo 方法返回的字符串值中解析。 
>
> **以下是在 POST表单数据填充到参数集前必须满足的条件：**
>
>  1。该请求是一个 HTTP或 HTTPS请求。
>
>  2。HTTP方法是 POST。
>
>  3。内容类型是 application/x-www-form-urlencoded。 
>
> 4。该 servlet已经对 request对象的任意 getParameter方法进行了初始调用。  
>
> **如果不满足这些条件，而且参数集中不包括 POST表单数据，那么 servlet必须可以通过 request对象的输入 流得到 POST数据。**
>
> **如果满足这些条件，那么从 request 对象的输入流中直接读取 POST 数据将不再有效。**   

### HTTP 属性

属性是与请求相关联的对象。属性可以由容器设置来表达信息，否则无法通过 API表示，或者由 servlet设 置将信息传达给另一个 servlet（通过 RequestDispatcher）。属性通过 ServletRequest 接口中下面的方法来访 问：

```java
getAttribute
getAttributeNames
setAttribute 
```

> 只有一个属性值可与一个属性名称相关联。
>
> 以前缀 java.和 javax.开头的属性名称是本规范的保留定义。 
> 同样地，以前缀 sun.和 com.sun.，oracle 和 com.oracle 开头的属性名是 Oracle Corporation 的保留定 义。
>
> 建议属性集中所有属性的命名与 Java 编程语言的规范 1 为包命名建议的反向域名约定一致。 

### HTTP 头信息： 请求头

servlet可以通过 HttpServletRequest接口的下面方法访问 HTTP请求的头部信息：

```java
getHeader
getHeaders
getHeaderNames 
```

### HTTP 请求路径

引导 servlet服务请求的请求路径由许多重要部分组成。以下元素从请求 URI路径得到，并通过 request对象 公开： 

**Content Path：**（主机名/ 应用程序上下文路径）

与 ServletContext 相关联的路径前缀是这个 servlet 的一部分。如果这个上下文是基于 Web 服务器的 URL命名空间基础上的“默认”上下文，那么这个路径将是一个空字符串。否则，如果上下文不是 基于服务器的命名空间，那么这个路径以/字符开始，但不以/字符结束。 

**Servlet Path：**

路径部分直接与激活请求的映射对应。这个路径以“/”字符开头，如果请求与“/ *”或“”模式 匹配，在这种情况下，它是一个空字符串。 

**Path info：**

请求路径的一部分，不属于 Context Path 或 Servlet Path。如果没有额外的路径，它要么是 null， 要么是以'/'开头的字符串。 

```java
使用 HttpServletRequest接口中的下面方法来访问这些信息：
getContextPath
getServletPath
getPathInfo
```

> 重要的是要注意，除了请求 URI和路径部分的 URL编码差异外，下面的等式永远为真： 	**requestURI = contextPath + servletPath + pathInfo**
>
> 在 API中有两个方便的方法，允许开发者获得与某个特定的路径等价的文件系统路径。这些方法是：
>
> ServletContext.getRealPath
>
> HttpServletRequest.getPathTranslated 

### 非阻塞IO： 略

### 国际化

客户可以选择希望 Web 服务器用什么语言来响应。该信息可以和使用 Accept-Language头与 HTTP/1.1规范 中描述的其他机制的客户端通信。ServletRequest接口提供下面的方法来确定发送者的首选语言环境：

```java
getLocale
getLocales 
```

### Cookies

HttpServletRequest接口提供了 getCookies方法来获得请求中的 cookie的一个数组。这些 cookie是从客户端 发送到服务器端的客户端发出的每个请求上的数据。典型地，客户端发送回的作为 cookie的一部分的唯一 信息是 cookie的名称和 cookie值。当 cookie发送到浏览器时可以设置其他 cookie属性，诸如注释，这些信 息不会返回到服务器。该规范还允许的 cookies是 HttpOnly cookie。HttpOnly cookie暗示客户端它们不会暴 露给客户端脚本代码（它没有被过滤掉，除非客户端知道如何查找此属性）。使用 HttpOnly cookie有助于减 少某些类型的跨站点脚本攻击。 

### SSL

如果请求已经通过一个安全协议发送过，如 HTTPS，必须通过 ServletRequest接口的 isSecure方法公开该信 息。Web 容器必须公开下列属性给 servlet程序员： 表 3-3 协议属性 

| 属性         | 属性名称                             | Java 类型 |
| ------------ | ------------------------------------ | --------- |
| 密码套件     | javax.servlet.request.cipher_suite   | String    |
| 算法的位大小 | javax.servlet.request.key_size       | Integer   |
| SSL 会话 id  | javax.servlet.request.ssl_session_id | String    |

如果有一个与请求相关的 SSL 证书，它必须由 servlet 容器以 java.security.cert.X509Certificate 类型的对象数 组暴露给 servlet 程序员并可通过一个 javax.servlet.request.X509Certificate 类型的 ServletRequest 属性访问。  

这个数组的顺序是按照信任的升序顺序。证书链中的第一个证书是由客户端设置的，第二个是用来验证第 一个的，等等。 

### 字符编码

目前，许多浏览器不随着 Content-Type 头一起发送字符编码限定符，而是根据读取 HTTP 请求确定字符编 码。**如果客户端请求没有指定请求默认的字符编码，容器用来创建请求读取器和解析 POST 数据的编码必 须是“ISO-8859-1”。**然而，为了向开发人员说明客户端没有指定请求默认的字符编码，在这种情况下，客户端发送字符编码失败，容器从 getCharacterEncoding方法返回 null。 

如果客户端没有设置字符编码，并使用不同的编码来编码请求数据，而不是使用上面描述的默认的字符编 码，那么可能会发生破坏。为了弥补这种情况，ServletRequest 接口添加了一个新的方法 **setCharacterEncoding(String enc)。**

开发人员可以通过调用此方法来覆盖由容器提供的字符编码。**必须在解析 任何 post数据或从请求读取任何输入之前调用此方法。**此方法一旦调用，将不会影响已经读取的数据的编码。

### 声明周期

> **每个 request对象只在 servlet的 service方法的作用域内，或过滤器的 doFilter方法的作用域内有效**，除非该 组件启用了异步处理并且调用了 request对象的 startAsync方法。
>
> 在发生异步处理的情况下，request对象一 直有效，直到调用 AsyncContext 的 complete方法。容器通常会重复利用 request对象，以避免创建 request 对象的性能开销。
>
> 开发人员必须注意的是，不建议在上述范围之外保持 startAsync 方法还没有被调用的请 求对象的引用，因为这样可能产生不确定的结果。 
>
> 在升级情况下，如上描述仍成立。 

## Servlet Context 上下文： 暂略

含应用程序上下文和编程式创建Servlet，Filter，Listener

## Response

