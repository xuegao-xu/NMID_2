## 动态网页

1静态、动态：
主要看是否随着时间、地点、用户操作的改变而改变

动态网页需要使用到服务端脚本语言（JSP）

2架构
CS：Client Server
CS不足：
a.如果软件升级， 那么全部软件都需要升级
b.维护麻烦：需要维护每一台客户端软件
c.每一台客户端都需要安装客户端软件



BS ：Broswer Server
客户端可以通过浏览器直接访问服务端

注意：bs和cs各有优势。

## Tomcat解压后目录：

bin:可执行文件（startup.bat shutdown.bat）
conf:配置文件（server.xml）
lib：tomcat依赖的jar文件
log:日志文件（记录出错等信息）
temp:临时文件
webapps：可执行的项目（将我们开发的项目放入该目录）
work:存放由jsp翻译成的java,以及编辑成的class文件(jsp ->java ->class)

## 配置Tomcat

a.配置jdk (必须配置JAVA_HOME)
	Java_home、ClassPath Path
b.配置Catalina_home

双击bin/startup.bat启动Tomacat，
常见错误： 可能与其他服务的端口号冲突
tomcat端口号默认8080（此端口号较为常见，容易冲突），可以修改默认的端口 

## 访问Tomcat

1.启动Tomcat :进入命令行cmd，输入startup.bat

2.打开浏览器，输入：[http://localhost:8080](http://localhost:8080/), (如果修改了端口号，加上相应端口)看tomcat是否正常启动

3.关闭Tomcat:进入命令行cmd，输入shutdown.bat

常见状态码：
200：一切正常
300/301: 页面重定向 （跳转）
404:资源不存在
403：权限不足 （如果访问a目录，但是a目录设置不可见）
500：服务器内部错误（代码有误）
其他编码：积累中……



## 使用Eclipse开发Web项目

(JSP项目) Tomcat

1.在Eclipse中创建的Web项目：
浏览器可以直接访问 WebContent中的文件，
例如http://localhost:8080/MyJspProject/index1.jsp
其中的index1.jsp就在WebContent目录中；
但是WEB-INF中的文件 无法通过客户端（浏览器）直接访问，只能通过请求转发来访问

注意：

> 并不是任何的内部跳转都能访问WEB-INF；原因是 跳转有2种方式：请求转发 、重定向

2.配置tomcat运行时环境
JSP<=>Servlet

- 将tomcat/lib中的`servlet-api.jar`加入项目的构建路径
- 右键项目->Build Path -> Add library ->Server Runtime

3.部署tomcat
在servers面板新建一个 tomcat实例 ， 再在该实例中部署项目（右键-add）之后运行

注意：一般建议将eclipse中的tomcat与本地tomcat的配置信息保持一致： 将eclipse中的tomcat设置为托管模式：

【第一次】创建tomcat实例之后， 双击选择Server Location的第二项

4.统一字符集编码

编码分类：
设置jsp文件的编码（jsp文件中的pageEncoding属性）： jsp -> java
设置浏览器读取jsp文件的编码（jsp文件中content属性）
一般将上述设置成 一致的编码，推荐使用UTF-8
文本编码：
将整个eclipse中的文件统一设置（推荐方式）

# JSP

JAVA serve pages

## JSP的页面元素

1. HTML
2. JAVA代码（脚本Scriptlet）
3. 指令
4. 注释

### 脚本Scriptlet

1. 

```
<%     局部变量、java语句  %>
```

2. 

```
<%!  全局变量、定义方法  %>
```

3. 

```
<%=输出表达式 %>
```

一般而言，修改web.xml、配置文件、java 需要重启tomcat服务

但是如果修改 Jsp\html\css\js ，不需要重启

> 注意: out.println()不能回车； 要想回车：“
> ”，即out.print() <%= %> 可以直接解析html代码

### 指令

page指令

`<%@ page ....%>`page指定的属性：
language: jsp页面使用的脚本语言
import: 导入类
pageEncoding: jsp文件自身编码 jsp ->java
contentType:浏览器解析jsp的编码

```
<%@ page language="java" contentType="text/html; charset=UTF-8"  
    pageEncoding="UTF-8"  import="java.util.Date" %>
```

### 注释

HTML注释

```
<!-- -->
```

可以被客户通过浏览器查看源码所观察到

JAVA注释

```
//     /*...*/
```

JSP注释

```
<%-- --%>
```

## JSP九大内置对象

自带的，不需要new也能使用的对象

### request：请求对象

存储“客户端向服务端发送的请求信息”
request对象的常见方法：

| name                                                     | description                                                  |
| :------------------------------------------------------- | :----------------------------------------------------------- |
| String getParameter(String name)                         | 根据请求的字段名key （input标签的name属性值） ，返回字段值value （input标签的value属性值） |
| String[] getParameterValues(String name)                 | 根据请求的字段名key ，返回多个字段值value （checkbox）       |
| void setCharacterEncoding                                | (“编码格式utf-8”) ：设置post方式的请求编码 （tomcat7以前默认iso-8859-1，tomcat8以后改为了utf-8） |
| getRequestDispatcher(“b.jsp”) .forward(request,response) | 请求转发 的方式跳转页面 A - > B                              |
| ServletContext getServerContext()                        | 获取项目的ServletContext对象                                 |
|                                                          |                                                              |

### response :响应对象

提供的方法

| function                                                | description                                         |
| :------------------------------------------------------ | :-------------------------------------------------- |
| `void addCookie( Cookie cookie );`                      | 服务端向客户端增加cookie对象                        |
| void sendRedirect(String location ) throws IOException; | 页面跳转的一种方式（重定向）                        |
| void setContetType(String type)                         | 设置服务端响应的编码（设置服务端的contentType类型） |

### session 会话对象

```
import javax.servlet.http.HttpSession;
```

session(服务端) , Cookie（客户端，不是内置对象）:Cookie是由服务端生成的 ，再发送给客户端保存。
相当于 本地缓存的作用： 客户端(hello.mp4,zs/abc)->服务端(hello.mp4；zs/abc)
作用：提高访问服务端的效率，但是安全性较差。

#### Cookie

- 不是内置对象，要使用必须new
- 但是，服务端会自动生成一个(服务端自动new一个cookie) `name=JSESIONID`的cookie并返回给客户端

Cookie： name=value

```
javax.servlet.http.Cookie
```

------

| function                                | description       |
| :-------------------------------------- | :---------------- |
| public Cookie(String name,String value) |                   |
| String getName()                        | 获取name          |
| String getValue()                       | 获取value         |
| void setMaxAge(int expiry)              | 最大有效期 （秒） |
|                                         |                   |

服务端准备Cookie：

```
response.addCookie(Cookie cookie)
```

页面跳转（转发，重定向）

```
客户端获取cookie:  request.getCookies();
```

- 服务端增加cookie :response对象；客户端获取对象：request对象
- 不能直接获取某一个单独对象，只能一次性将部的cookie拿到

通过F12可以发现除了自己设置的Cookie对象外，还有一个name为JsessionId的cookie

建议cookie只保存英文数字，否则需要进行编码、解码

1.使用Cookie实现记住用户名，功能

2.session :会话
a.浏览网站：开始-关闭
b.购物： 浏览、付款、退出
c.电子邮件：浏览、写邮件、退出
开始-结束

#### session机制

客户端第一次请求服务端时，（sessionId`-`sessionId）服务端会产生一个session对象（用于保存该客户的信息）；
并且每个session对象 都会有一个唯一的`sessionId`( 用于区分其他session);
服务端由会产生一个cookie，并且该cookie的name=`JsessionId`,value=服务端`sessionId`的值；
然后服务端会在响应客户端的同时将该cookie发送给客户端，至此客户端就有了 一个cookie(`JsessionId`)；
因此，客户端的cookie就可以和服务端的session一一对应（`JsessionId`- `sessionId`）

客户端第二/n次请求服务端时:服务端会先用客户端cookie种的`JsessionId` 去服务端的session中匹配`sessionId`,如果匹配成功（cookie `jsessionId`和sesion `sessionId`），说明此用户不是第一次访问,无需登录；

例子：
客户端： 顾客（客户端）
服务端: 存包处 - 商场(服务端)

顾客第一次存包：商场判断此人是之前已经存过包（通过你手里是否有钥匙）。
如果是新顾客（没钥匙） ，分配一个钥匙给该顾客； 钥匙会和柜子 一一对应；

第二/n次 存包：商场判断此人是之前已经存过包（通过你手里是否有钥匙）
如果是老顾客（有钥匙），则不需要分配；该顾客手里的钥匙会和柜子自动一一对应。

#### session:

- session存储在服务端
- session是在同一个用户（客户）请求时共享
- 实现机制：第一次客户请求时产生一个`sessionId` 并复制给cookie的`JsessionId`然后发给客户端。最终通过session的`sessionId`-cookie的`JsessionId`

#### session方法

| function                                     | description                      |
| :------------------------------------------- | :------------------------------- |
| String getId()                               | 获取`sessionId`                  |
| boolean isNew()                              | 判断是否是 新用户（第一次访问）  |
| void invalidate()                            | 使session失效 （退出登录、注销） |
| void setAttribute(String name, Object value) |                                  |
| Object getAttribute();                       |                                  |
| void setMaxInactiveInterval(秒)              | 设置最大有效 非活动时间          |
| int getMaxInactiveInterval()                 | 获取最大有效 非活动时间          |

示例：
登录

客户端在第一次请求服务端时，如果服务端发现此请求没有 J`sessionId`,则会创建一个name=JSESIONID的cookie并返回给客户端

cookie和session的区别：

|            | session | cookie   |
| :--------- | :------ | :------- |
| 保存的位置 | 服务端  | 客户端   |
| 安全性     | 较安全  | 较不安全 |
| 保存的内容 | Object  | String   |

### application全局对象

String getContextPath() 虚拟路径
String getRealPath(String name): 绝对路径（虚拟路径相对的绝对路径）

### 其他五种不常用对象

| Object      | description                           |
| :---------- | :------------------------------------ |
| pageContext | JSP页面容器                           |
| config      | 配置对象（服务器配置信息）            |
| page        | 当前JSP页面对象（相当于java中的this） |
| exception   | 异常对象                              |
| out         | 输出对象，向客户端输出内容            |
|             |                                       |

## get与post请求方式的区别：

get提交方式

[http://localhost:8080/MyJspProject/show.jsp?uname=aa&upwd=123&uage=22&uhobbies=%E7%AF%AE%E7%90%83](http://localhost:8888/MyJspProject/show.jsp?uname=aa&upwd=123&uage=22&uhobbies=篮球)

```
连接/文件?参数名1=参数值1 & 参数名2=参数值2 & 参数名1=参数值1
```

method=”get”和地址栏 、超链接(`<a href="xx">`)请求方式 默认都属于get提交方式

- get方式在地址栏显示请求信息(但是地址栏能够容纳的 信息有限，4-5KB；如果请求数据存在大文件，图片等会出现地址栏无法容纳全部的数据而出错) ；post不会显示
- 文件上传操作，必须是post
  推荐使用post

统一请求的编码request
get方式请求如果出现乱码，解决：

- 统一每一个变量的编码 （不推荐）

```
new String(  旧编码，新编码);  
name = new String(name.getBytes("iso-8859-1"),"utf-8");
```

- 修改server.xml ，一次性的更改tomcat默认get提交方式的编码 （utf-8）
  建议使用tomcat时， 首先在server.xml中统一get方式的编码.. `URIEncoding="UTF-8"`

tomcat7(iso-8859-1)

tomcat8（utf-8）

post

```
request.setCharacterEncoding("utf-8")
```

## 请求转发和重定向

请求转发

```
request.getRequestDispatcher("b.jsp").forward(request,response)
```

重定向

```
reponse.sendRedirect("b.jsp")
```

|                            | 请求转发        | 重定向                 |
| :------------------------- | :-------------- | :--------------------- |
| 地址栏是否改变             | 不变(check.jsp) | 改变(success.jsp)      |
| 是否保留第一次请求时的数据 | 保留            | 不保留                 |
| 请求的次数                 | 1               | 2                      |
| 跳转发生的位置             | 服务端          | 客户端发出的第二次跳转 |
|                            |                 |                        |

![](https://image-1306027094.cos.ap-nanjing.myqcloud.com/md/request.png)

转发、重定向：

转发：
张三（客户端） -> 【 服务窗口 A （服务端 ） -> 服务窗口B 】

重定向：
张三（客户端） -> 服务窗口 A （服务端 ） ->去找B

 张三（客户端） -> 服务窗口 B （服务端 ） ->结束

## 四种范围对象

（小->大）

| object      | description                | scope                    |
| :---------- | :------------------------- | :----------------------- |
| pageContext | JSP页面容器 （page对象）； | 当前页面有效             |
| request     | 请求对象                   | 同一次请求有效           |
| session     | 会话对象                   | 同一次会话有效           |
| application | 全局对象                   | 全局有效（整个项目有效） |
|             |                            |                          |

以上4个对象共有的方法：

| function                                    | description              |
| :------------------------------------------ | :----------------------- |
| `Object getAttribute(String name)`          | 根据属性名，获取属性值   |
| `void setAttribute(String name,Object obj)` | 设置属性值（新增，修改） |
| `void removeAttribute(String name)`         | 根据属性名，删除对象     |

- pageContext 当前页面有效(页面跳转后无效)

- request 同一次请求有效；其他请求无效 （请求转发后有效；重定向后无效）

- session 同一次会话有效 （无论怎么跳转，都有效；关闭/切换浏览器后无效 ； 从 登陆->退出之间全部有效）

- application全局变量；整个项目运行期间都有效(切换浏览器 仍然有效)；关闭服务、其他项目无效

  -->多个项目共享、重启后仍然有效：JNDI

1. 以上的4个范围对象，通过 setAttribute()复制，通过getAttribute()取值；
2. 以上范围对象，尽量使用最小的范围。因为对象的范围越大，造成的性能损耗越大。

# Servlet

Java类必须符合一定的规范：

1. 必须继承javax.servlet.http.HttpServlet
2. 重写其中的service()之类的方法

```
service(): 接受并识别和处理相应的所有提交的方法； 
doGet(): 接受并处理所有get提交方式的请求  
doPost(): 接受并处理所有post提交方式的请求
```

Servlet要想使用，必须配置
Serlvet2.5：web.xml

```
<web-app>
    ...
  <servlet>
  		<servlet-name>WelcomeServlet</servlet-name>
  		<servlet-class>org.lanqiao.servlet.WelcomeServlet</servlet-class>
  </servlet>
  
  <servlet-mapping>
  		<servlet-name>WelcomeServlet</servlet-name>
  		<url-pattern>/WelcomeServlet</url-pattern>
  </servlet-mapping>
</web-app>
```

Servlet3.0： `@WebServlet`

```
@WebServlet( value="/WelcomeServlet" ,loadOnStartup=1,initParams= {@WebInitParam(name="serveltparaname30",value="servletparavalue30")   }   )
```

Serlvet2.5：web.xml:

项目的根目录：WebContent 、src

<a href="WelcomeServlet">所在的jsp是在WebContent目录中，因此发出的请求WelcomeServlet 是去请求项目的根目录。

Servlet流程：

请求 ->`<url-pattern>` -> 根据`<servlet-mapping>`中的`<servlet-name>` 去匹配 `<servlet>`中的`<servlet-name>`，然后寻找到，求中将请求交由该执行。 2个/:
jsp:/ localhost:8080
web.xml: / http://localhost:8080/项目名/

## 编写Servlet步骤：

```
import javax.servlet.http.HttpServlet;
```

1.编写一个类继承HttpServlet

![](https://image-1306027094.cos.ap-nanjing.myqcloud.com/md/Servlet-hierarchy.png)

```
public class LoginServlet extends HttpServlet {
```

2.重写相应方法(doGet()、doPost())

3.配置Servlet

编写web.xml中的servlet映射关系

Servlet3.0，与Servlet2.5的区别：

Servlet3.0不需要在web.xml中配置，但需要在 Servlet类的定义处之上编写

匹配流程： 请求地址与`@WebServlet`中的值进行匹配，如果匹配成功 ，则说明 请求的就是该注解所对应的类

> 注解:@WebServlet(“url-pattern的值”)
>
> 注意:url-pattern 要带斜杆

2.借助于Eclipse快速生成Servlet
直接新建Servlet即可！（继承、重写、web.xml 可以借助Eclipse自动生成）

3.项目根目录：WebContent、src（所有的构建路径）
例如：
WebContent中有一个文件index.jsp
src中有一个Servlet.java

如果: index.jsp中请求`<a href="abc">...</a>`，则寻找范围：既会在src根目录中找也会在WebContent根目录中找

如果：index.jsp中请求`<a href="a/abc"></a>`，寻找范围：先在src或WebContent中找a目录，然后再在a目录中找abc

web.xml中的 /:代表项目根路径
http://localhost:8080/Servlet25Project/jsp中的/: 服务器根路径
http://localhost:8080/

构建路径、WebContent:根目录

## Servlet生命周期

![](https://image-1306027094.cos.ap-nanjing.myqcloud.com/md/Servlet-life-cycle.png)

**5个阶段**

### 加载、初始化：

init() ，该方法会在 Servlet被加载并实例化的以后执行
服务 ：service() ->doGet() doPost

init():

- 默认第一次访问 Servlet时会被执行（只执行这一次）
- 可以修改为Tomcat启动时自动执行

### 销毁 ：destroy()

Servlet被系统回收时执行

卸载

1. Servlet2.5： web.xml

```
<servlet>  
    ...  
    <load-on-startup>1</load-on-startup>  
</servlet>
```

其中的“1”代表第一个。

2. Servlet3.0

```
@WebServlet( value="/WelcomeServlet" ,loadOnStartup=1  )
```

service() ->doGet() doPost ：调用几次，则执行几次

destroy()：关闭tomcat服务时，执行一次。

Servlet API ： 由两个软件包组成： 对应于HTTP协议的软件包、对应于除了HTTP协议以外的其他软件包
即Servlet API可以适用于任何通信协议。
我们学习的Servlet,是位于javax.servlet.http包中的类和接口，是基础HTTP协议。

Servlet继承关系

ServletConfig:接口
ServletContext getServletContext():获取Servlet上下文对象 application
String getInitParameter(String name):在当前Servlet范围内，获取名为name的参数值（初始化参数）

a.ServletContext中的常见方法(application)：
getContextPath():相对路径
getRealPath()：绝对路径
setAttribute() 、getAttribute()

String getInitParameter(String name);在当前Web容器范围内，获取名为name的参数值（初始化参数）

Servlet3.0方式 给当前Servlet设置初始值：

```
@WebServlet( .... , initParams= {@WebInitParam(name="serveltparaname30",value="servletparavalue30")   }   )
```

注意，此注解只隶属于某一个具体的Servlet ，因此无法为整个web容器设置初始化参数 （如果要通过3.0方式设置 web容器的初始化参数，仍然需要在web.xml中设置）

HttpServletRequest中的方法：(同request)，例如setAttrite()、getCookies()、getMethod()
HttpServletResponse中的方法：同response

Servlet使用层面：

Eclipse中在src创建一个Servlet，然后重写doGet() doPost()就可以 （doGet() doPost()只需要编写一个）。

# 使用idea创建一个Maven java Web项目

教程：https://blog.csdn.net/qq_37856300/article/details/85776785



# HTTP 消息结构

HTTP是基于客户端/服务端（C/S）的架构模型，通过一个可靠的链接来交换信息，是一个无状态的请求/响应协议。

一个HTTP"客户端"是一个应用程序（Web浏览器或其他任何客户端），通过连接到服务器达到向服务器发送一个或多个HTTP的请求的目的。

一个HTTP"服务器"同样也是一个应用程序（通常是一个Web服务，如Apache Web服务器或IIS服务器等），通过接收客户端的请求并向客户端发送HTTP响应数据。

HTTP使用统一资源标识符（Uniform Resource Identifiers, URI）来传输数据和建立连接。

------

## 客户端请求消息

客户端发送一个HTTP请求到服务器的请求消息包括以下格式：请求行（request line）、请求头部（header）、空行和请求数据四个部分组成，下图给出了请求报文的一般格式。

![](https://image-1306027094.cos.ap-nanjing.myqcloud.com/md/2012072810301161.png)

## 服务器响应消息

HTTP响应也由四个部分组成，分别是：状态行、消息报头、空行和响应正文。

![](https://image-1306027094.cos.ap-nanjing.myqcloud.com/md/httpmessage.jpg)

# HTTP 请求方法

根据 HTTP 标准，HTTP 请求可以使用多种请求方法。

HTTP1.0 定义了三种请求方法： GET, POST 和 HEAD方法。

HTTP1.1 新增了六种请求方法：OPTIONS、PUT、PATCH、DELETE、TRACE 和 CONNECT 方法。

| 序号 | 方法    | 描述                                                         |
| :--- | :------ | :----------------------------------------------------------- |
| 1    | GET     | 请求指定的页面信息，并返回实体主体。                         |
| 2    | HEAD    | 类似于 GET 请求，只不过返回的响应中没有具体的内容，用于获取报头 |
| 3    | POST    | 向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST 请求可能会导致新的资源的建立和/或已有资源的修改。 |
| 4    | PUT     | 从客户端向服务器传送的数据取代指定的文档的内容。             |
| 5    | DELETE  | 请求服务器删除指定的页面。                                   |
| 6    | CONNECT | HTTP/1.1 协议中预留给能够将连接改为管道方式的代理服务器。    |
| 7    | OPTIONS | 允许客户端查看服务器的性能。                                 |
| 8    | TRACE   | 回显服务器收到的请求，主要用于测试或诊断。                   |
| 9    | PATCH   | 是对 PUT 方法的补充，用来对已知资源进行局部更新 。           |

# HTTP 响应头信息

| 应答头           | 说明                                                         |
| :--------------- | :----------------------------------------------------------- |
| Allow            | 服务器支持哪些请求方法（如GET、POST等）。                    |
| Content-Encoding | 文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定的内容类型。利用gzip压缩文档能够显著地减少HTML文档的下载时间。Java的GZIPOutputStream可以很方便地进行gzip压缩，但只有Unix上的Netscape和Windows上的IE 4、IE 5才支持它。因此，Servlet应该通过查看Accept-Encoding头（即request.getHeader("Accept-Encoding")）检查浏览器是否支持gzip，为支持gzip的浏览器返回经gzip压缩的HTML页面，为其他浏览器返回普通页面。 |
| Content-Length   | 表示内容长度。只有当浏览器使用持久HTTP连接时才需要这个数据。如果你想要利用持久连接的优势，可以把输出文档写入 ByteArrayOutputStream，完成后查看其大小，然后把该值放入Content-Length头，最后通过byteArrayStream.writeTo(response.getOutputStream()发送内容。 |
| Content-Type     | 表示后面的文档属于什么MIME类型。Servlet默认为text/plain，但通常需要显式地指定为text/html。由于经常要设置Content-Type，因此HttpServletResponse提供了一个专用的方法setContentType。 |
| Date             | 当前的GMT时间。你可以用setDateHeader来设置这个头以避免转换时间格式的麻烦。 |
| Expires          | 应该在什么时候认为文档已经过期，从而不再缓存它？             |
| Last-Modified    | 文档的最后改动时间。客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件GET，只有改动时间迟于指定时间的文档才会返回，否则返回一个304（Not Modified）状态。Last-Modified也可用setDateHeader方法来设置。 |
| Location         | 表示客户应当到哪里去提取文档。Location通常不是直接设置的，而是通过HttpServletResponse的sendRedirect方法，该方法同时设置状态代码为302。 |
| Refresh          | 表示浏览器应该在多少时间之后刷新文档，以秒计。除了刷新当前文档之外，你还可以通过setHeader("Refresh", "5; URL=http://host/path")让浏览器读取指定的页面。 注意这种功能通常是通过设置HTML页面HEAD区的＜META HTTP-EQUIV="Refresh" CONTENT="5;URL=http://host/path"＞实现，这是因为，自动刷新或重定向对于那些不能使用CGI或Servlet的HTML编写者十分重要。但是，对于Servlet来说，直接设置Refresh头更加方便。  注意Refresh的意义是"N秒之后刷新本页面或访问指定页面"，而不是"每隔N秒刷新本页面或访问指定页面"。因此，连续刷新要求每次都发送一个Refresh头，而发送204状态代码则可以阻止浏览器继续刷新，不管是使用Refresh头还是＜META HTTP-EQUIV="Refresh" ...＞。  注意Refresh头不属于HTTP 1.1正式规范的一部分，而是一个扩展，但Netscape和IE都支持它。 |
| Server           | 服务器名字。Servlet一般不设置这个值，而是由Web服务器自己设置。 |
| Set-Cookie       | 设置和页面关联的Cookie。Servlet不应使用response.setHeader("Set-Cookie", ...)，而是应使用HttpServletResponse提供的专用方法addCookie。参见下文有关Cookie设置的讨论。 |
| WWW-Authenticate | 客户应该在Authorization头中提供什么类型的授权信息？在包含401（Unauthorized）状态行的应答中这个头是必需的。例如，response.setHeader("WWW-Authenticate", "BASIC realm=＼"executives＼"")。 注意Servlet一般不进行这方面的处理，而是让Web服务器的专门机制来控制受密码保护页面的访问（例如.htaccess）。 |