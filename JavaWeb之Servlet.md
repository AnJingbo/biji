

## 一、Servlet 概述
### 1.1 什么是Servlet？
1、Servlet 是 JavaEE 规范之一。规范就是接口。
2、Servlet 是 JavaWeb 三大组件之一。三大组件分别是：Servlet 程序、Filter 过滤器、Listener 监听器。
3、Servlet 是运行在服务器上的一个 Java 小程序。**它可以接收客户端发送过来的请求，并响应数据给客户端。**
### 1.2 手动实现 Servlet 程序
1、编写一个类去实现 Servlet 接口
2、实现 service 方法，处理请求，并响应数据
3、到 web.xml 中去配置 Servlet 程序的访问地址
![图示](https://img-blog.csdnimg.cn/20210517214516581.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

**如何配置：**
![图示](https://img-blog.csdnimg.cn/2021051721080557.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
注：默认打开的网页的url是：
![图示](https://img-blog.csdnimg.cn/202105172109519.png)
因此还要在该地址后面加上 hello 。

**注：/ 斜杠表示地址为：http://ip:port/工程名/，映射到 IDEA代码的 web 目录**

### 1.3 url地址到Servlet程序的访问
为什么输入一个url地址就能访问到一个 Servlet 程序呢？

通过 ip 地址定位网络中的一个服务器。服务器里面有Tomcat服务器，这是个软件。它监听的端口是8080端口。
**在Tomcat里面有好多工程，到底要访问哪个工程，就是由工程路径决定的。**
**到了工程以后，到底要访问哪个资源，由资源路径决定。web.xml 里面配置的路径会优先检查。**
![图示](https://img-blog.csdnimg.cn/20210517220630265.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 1.4 Servlet的生命周期
1、执行 Servlet 构造方法
2、执行 init 初始化方法
3、执行 service 方法
4、执行 destroy 方法
**注：第一、二步，是在第一次访问，创建Servlet程序时会调用。
第三步，每次访问都会调用。
第四步，只有在 web 工程停止的时候调用。**
### 1.5 Get 和 Post 请求的分发处理
一般情况下，Get 请求和 Post 请求干的事情是不一样的

```java
/**
     * service 方法是专门用来处理请求和响应的
     * @param servletRequest
     * @param servletResponse
     * @throws ServletException
     * @throws IOException
     */
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("3、hello servlet!!!");

        // 类型转换，因为它有getMethod()方法
        HttpServletRequest hsr = (HttpServletRequest) servletRequest;

        // 得到请求的类型
        String method = hsr.getMethod();

        if("GET".equals(method)){
            // 如果此处执行的代码比较多，会使可读性变差，可以在外面再写一个方法，比如doGet()
            System.out.println("get请求执行");
        }else if("POST".equals(method)){
            // 如果此处执行的代码比较多，会使可读性变差，可以在外面再写一个方法，比如doPost()
            System.out.println("post请求执行");
        }
    }
```
### 1.6 通过继承 HttpServlet 实现 Servlet 程序
一般在实际开发中，都是使用继承 HttpServlet 类的方式去实现 Servlet 程序
1、编写一个类去继承 HttpServlet 类
2、根据业务需要重写 doGet 或者 doPost 方法
3、到 web.xml 中去配置 Servlet 程序的访问地址
```java
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class Fact extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("get请求执行！！！");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("post请求执行！！！");
    }
}
```
![图示](https://img-blog.csdnimg.cn/20210518081548141.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 1.7 使用 IDEA 创建 Servlet 程序
![图示](https://img-blog.csdnimg.cn/20210520084338399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
![图示](https://img-blog.csdnimg.cn/20210520084441945.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 1.8 Servlet 类的继承体系
![图示](https://img-blog.csdnimg.cn/20210518084708503.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 1.9 ServletConfig 类
ServletConfig 类从类名上来看，就知道是 Servlet 程序的配置信息类。

Servlet 程序和 ServletConfig 类都是由 Tomcat 负责创建，我们负责使用。

Servlet 程序默认是第一次访问的时候创建，ServletConfig 是每个 Servlet 程序创建时，就创建一个对应的 ServletConfig 对象。
##### ServletConfig 类的三大作用
1、获取 Servlet 程序的别名 servlet-name 的值
2、获取初始化参数 init-param
3、获取 ServletContext 对象

示例：
```java
@Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("HelloServlet 程序的别名 servlet-name 的值：" + servletConfig.getServletName());
        System.out.println("初始化参数 username 的值是：" + servletConfig.getInitParameter("username"));
        System.out.println("获取 ServletContext 对象：" + servletConfig.getServletContext());
    }
```
![图示](https://img-blog.csdnimg.cn/20210518091011956.png)
### 1.10 ServletContext 类
##### 什么是ServletContext？
1、ServletContext 是一个接口，它表示 Servlet 上下文对象。
2、一个 web 工程，只有一个 ServletContext 对象实例
3、ServletContext 对象是一个域对象。
4、ServletContext 是在 web 工程部署的时候创建，在 web 工程停止的时候销毁。
5、整个工程都能用 ServletContext 的数据

什么是域对象？
域对象，是可以像 Map 一样存取数据的对象，叫域对象。这里的域指的是存取数据的操作范围，整个 web 工程。
![图示](https://img-blog.csdnimg.cn/20210518205120156.png)
##### ServletContext 类的四个作用
1、获取 web.xml 中配置的上下文参数 context-param
2、获取当前工程的路径，格式：/工程路径
3、获取工程部署在服务器硬盘上的绝对路径
4、像 Map 一样存取数据
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class ServletContextTest extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // ServletContext servletContext = getServletConfig().getServletContext();
        ServletContext servletContext = getServletContext();

        /* 注：使用ServletContext对象不能访问init-param，它只能用ServletConfig对象访问 
          1、获取 web.xml 中配置的上下文参数 context-param。
        */
        System.out.println("context-param 参数 username 的值是" + servletContext.getInitParameter("username"));

        // 2、获取当前工程的路径，格式：/工程路径
        System.out.println("当前工程的路径是：" + servletContext.getContextPath());

        // 3、获取工程部署在服务器硬盘上的绝对路径
        System.out.println("工程部署的路径是" + servletContext.getRealPath("/"));/* / 斜杠被服务器解析地址为：http://ip:post/工程名/，映射到IDEA代码的web目录 */

        // 4、像 Map 一样存取数据
        servletContext.setAttribute("key", "val");
        System.out.println("获取域数据 key 的值：" + servletContext.getAttribute("key"));
    }
}
```
## 二、HTTP协议
### 2.1 什么是HTTP协议？
协议是指双方或多方，相互约定好，大家都需要遵守的规则，叫协议。

所谓 HTTP 协议，就是指，客户端和服务器之间通信时，发送的数据，需要遵守的规则，就是 HTTP 协议。

HTTP协议中的数据也叫做报文。
### 2.2 GET 请求
![图示](https://img-blog.csdnimg.cn/20210520082500354.png)
![图示](https://img-blog.csdnimg.cn/20210520081911354.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 2.3 POST请求
![图示](https://img-blog.csdnimg.cn/20210520084154828.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210520083954868.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 2.4 常用的服务器请求头
Accept：表示客户端可以接收的数据类型
Accept-Language：表示客户端可以接收的语言类型
User-Agent：表示客户端浏览器的信息
Host：表示请求时的服务器 ip 和 端口号
### 2.5 哪些是GET请求，哪些是POST请求
GET 请求：
（1）form 标签 method=get
（2）a 标签
（3）link 标签引入 css
（4）Script 标签引入 JS 文件
（5）img 标签引入图片
（6）iframe 引入 html 页面
（7）在浏览器地址栏中输入地址后敲回车

POST 请求：
&nbsp;&nbsp;&nbsp;&nbsp; form 标签 method=post
### 2.6 响应的 HTTP 协议格式
![图示](https://img-blog.csdnimg.cn/20210520090145803.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
![图示](https://img-blog.csdnimg.cn/20210520093635679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 2.7 常用的响应码说明
200&nbsp;&nbsp;&nbsp;&nbsp; 表示请求成功
302&nbsp;&nbsp;&nbsp;&nbsp; 表示请求重定向
404&nbsp;&nbsp;&nbsp;&nbsp; 表示请求服务器已经收到了，但是你要的数据不存在（请求地址错误）
500&nbsp;&nbsp;&nbsp;&nbsp; 表示服务器已经收到请求，但是服务器内部错误（代码错误）
### 2.8 MIME类型说明
MIME 是 HTTP 协议中的数据类型。
MIME 的英文全称是"Multipurpose Internet Mail Extensions"，多功能 Internet 邮件扩充服务。MIME 类型的格式是”大类型/小类型"，并与某一种文件的扩展名相对应。
![图示](https://img-blog.csdnimg.cn/20210520093501250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
## 三、HttpServletRequest 类
### 3.1 HttpServletRequest 类有什么作用
每次只要有请求进入 Tomcat 服务器，Tomcat 服务器就会把请求过来的 HTTP 协议信息解析好封装到 Request 对象中。然后传递到 service 方法（doGet 和 doPost）中给我们使用。我们可以通过 HttpServletRequest 对象，获取到所有请求的信息。

Request 对象是域对象。每次请求都会封装成一个新的 Request 对象，因此对数据的存取范围只在一个请求内有效。

如果是一次请求，那么只会有一个 Request 对象，Request 域中的数据就是共享的。

如果是两次请求，那么就会有两个 Request 对象，也就有两个 Request 域，这两次请求的数据各自是各自的。
### 3.2 HttpServletRequest 类的常用方法
（1）getRequestURI()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 获取请求的资源路径
（2）getRequestURL()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 获取请求的统一资源定位符（绝对路径）
（3）getRemoteHost()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 获取客户端的 ip 地址
（4）getHeader()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 获取请求头
（5）getParameter()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 获取请求的参数
（6）getParameterValues()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 获取请求的参数（多个值的时候使用）
（7）getMethod()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 获取请求的方式 (Get 或者 Post)
（8）setAttribute(key, value)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 设置域数据
（9）getAttribute(key)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 获取域数据
（10）getRequestDispatcher()&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 获取请求转发对象
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class HttpServletRequestTest extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("获取请求的资源路径：" + request.getRequestURI());// /工程名/资源名
        System.out.println("获取请求的统一资源定位符（绝对路径）：" + request.getRequestURL());
        System.out.println("获取客户端的 ip 地址：" + request.getRemoteHost());
        System.out.println("获取请求头User-Agent：" + request.getHeader("User-Agent"));
        System.out.println("获取请求的方式 (Get 或者 Post)：" + request.getMethod());
    }
}
```
### 3.3 如何获取请求参数
![图示](https://img-blog.csdnimg.cn/2021052116061614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;
import java.util.Arrays;

public class HttpServletRequestTest01 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取请求参数
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        String[] hobbies = request.getParameterValues("hobby");

        System.out.println("用户名：" + username);
        System.out.println("密码：" + password);
        System.out.println("兴趣爱好：" + Arrays.toString(hobbies));
    }

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // String password = request.getParameter("password"); 放在设置字符集之前会出错，还是会出现乱码
        
        // 设置请求体的字符集为 UTF-8，从而解决 post 请求的中文乱码问题
        // 要在 获取请求参数 之前调用才有效！！！
        request.setCharacterEncoding("UTF-8");// 如果使用 post 方式提交，而且提交的数据带有中文，就会出现乱码。因此需要这一行
        // 获取请求参数
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        String[] hobbies = request.getParameterValues("hobby");

        System.out.println("用户名：" + username);
        System.out.println("密码：" + password);
        System.out.println("兴趣爱好：" + Arrays.toString(hobbies));
    }
}
```
### 3.4 请求转发
##### 什么是请求的转发？
请求转发是指，服务器收到请求后，从一个资源跳到另一个资源的操作叫请求转发。
>比如两个 Servlet 程序共同完成一个完整的业务功能。可以类比去某个部门办理业务，该业务需要两个柜台一起完成。（1）我先来到柜台 1，他就会查看你带的材料。（2）看你的材料没有问题，他就会在处理完自己的业务之后给你盖个章（因为这个章还要带给下一个柜台看，下个柜台看你有章才会给你办）。（3）问路，柜台 2 怎么走。（4）走到柜台 2。
>（1）然后我来到柜台2，他先要查看你的材料（2）然后会检查有没有柜台 1 的章（3）然后柜台 2 就处理自己的业务

![图示](https://img-blog.csdnimg.cn/20210521221315267.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class Servlet1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取请求参数（查看办事的材料）
        String username = request.getParameter("username");
        System.out.println("在Servlet1（柜台1）中查看参数（材料）：" + username);

        // 给材料盖个章，并传递给 Servlet2（柜台2）查看
        request.setAttribute("key1", "柜台 1 的章");

        // 问路，Servlet2（柜台 2） 怎么走
        /* 请求转发必须要以斜杠开头。/ 斜杠表示地址为：http://ip:port/工程名/，映射到 IDEA代码的 web 目录 */
        RequestDispatcher requestDispatcher = request.getRequestDispatcher("/servlet2");
        // 走到 Servlet2（柜台 2）
        requestDispatcher.forward(request, response);
    }
}
```
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class Servlet2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 获取请求参数（查看办事的材料）
        String username = request.getParameter("username");
        System.out.println("在Servlet2（柜台2）中查看参数（材料）：" + username);
        // 查看 柜台1 的盖章
        Object key1 = request.getAttribute("key1");
        System.out.println("柜台1是否有盖章：" + key1);

        // 处理自己的业务
        System.out.println("Servlet2 处理自己的业务");
    }
}
```
**注：a 目录和 e.html 页面在同一个目录下**
![图示](https://img-blog.csdnimg.cn/20210522111514866.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
![图示](https://img-blog.csdnimg.cn/20210522111750175.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
public class ForwardC extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("向前！");
        request.getRequestDispatcher("/a/b/c.html").forward(request, response);
    }
}
```
![图示](https://img-blog.csdnimg.cn/20210522115147763.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 3.5 Web 中的相对路径和绝对路径
在 JavaWeb 中，路径分为相对路径和绝对路径两种：
相对路径：
（1）**.**  &nbsp;&nbsp;&nbsp; 表示当前目录
（2）**. .** &nbsp;&nbsp;表示上一级目录
（3）资源名表示当前目录下的资源名 

绝对路径：http://ip:port/工程路径/资源路径
### 3.6 Web 中斜杠的不同意义
在 Web 中，斜杠 / 是一种绝对路径。

（1）/ 斜杠如果被浏览器解析，得到的地址是：http://ip:port/
```html
比如：<a href="/">斜杠</a>
```
（2）/ 斜杠如果被服务器解析，得到的地址是：http://ip:port/工程路径/
```java
比如：
1、<url-pattern>/servlet1</url-pattern>
2、servletContext.getRealPath("/");
3、request.getRequestDispatcher("/");
特殊情况：response.sendRediect("/"); 把斜杠发给浏览器解析。得到：http://ip:port/
```
## 四、HttpServletResponse 类
### 4.1 HttpServletResponse 类的作用
HttpServletResponse 类和 HttpServletRequest 类一样。每次请求进来，Tomcat 服务器都会创建一个 Response 对象传递给 Servlet 对象去使用。 HttpServletRequest 表示请求过来的信息， HttpServletResponse 表示所有响应的信息。

我们如果需要设置返回给客户端的信息，都可以通过  HttpServletResponse 对象来进行设置
### 4.2 两个输出流的说明
**响应通过流传给客户端。**

字节流 &nbsp;&nbsp;&nbsp;&nbsp; getOutputStream(); &nbsp;&nbsp;&nbsp;&nbsp; 常用于下载（传递二进制数据）
字符流 &nbsp;&nbsp;&nbsp;&nbsp; getWriter(); &nbsp;&nbsp;&nbsp;&nbsp; 常用于回传字符串（常用）

两个流同时只能使用一个。使用了字节流，就不能再使用字符流。反之亦然。否则就会报错
### 4.3 如何往客户端回传数据
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.PrintWriter;

public class HttpServletResponseTest extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 往客户端回传 字符串 数据
        PrintWriter writer = response.getWriter();
        //writer.println("response's content");
        writer.write("response's content!!!");
    }
}
```
### 4.4 响应乱码的解决
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.PrintWriter;

public class HttpServletResponseTest extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println(response.getCharacterEncoding());// 服务器默认的字符集 ISO-8859-1
        // 方法一：
        
        // 设置服务器支持的字符集
        // response.setCharacterEncoding("UTF-8");

        // 通过响应头，设置浏览器也使用字符集UTF-8。因为如果浏览器和服务器使用的字符集不同，会出现显示效果不对的情况
        // response.setHeader("Content-Type", "text/html; charset=UTF-8");

        // 方法二：（推荐使用）
        // 它会同时设置服务器和浏览器同时使用UTF-8字符集，还设置了响应头
        // 此方法一定要在获取流对象之前使用才有效
        response.setContentType("text/html; charset=UTF-8");

        // 往客户端回传 字符串 数据
        PrintWriter writer = response.getWriter();
        writer.write("我是中国人");
    }
}
```
### 4.5 请求重定向
![图示](https://img-blog.csdnimg.cn/20210523133616896.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class Response1 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("进入 Response1");

        // 请求重定向第一种方案：

        // 设置响应状态码302，表示重定向（已搬迁）
        // response.setStatus(302);

        // 设置响应头，说明新的地址在哪里
        // response.setHeader("Location", "http://localhost:8080/Servlet/response2");

        // 请求重定向第二种方案：（推荐使用）
        response.sendRedirect("http://localhost:8080/Servlet/response2");
    }
}
```
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class Response2 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.getWriter().write("Response2!!!");
    }
}
```

```java
http://localhost:8080/book/manager/bookServlet

请求转发的 / 表示到 工程名
request.getRequestDispatcher("/manager/bookServlet").forward(request, response);

请求重定向的 / 表示到 端口号
request.getContextPath() 获得的值是：/book，其中 / 会被浏览器解析，表示到端口号
response.sendRedirect(request.getContextPath() + "/manager/bookServlet");
```