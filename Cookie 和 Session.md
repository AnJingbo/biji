

## Cookie
### 什么是Cookie？
1. Cookie 翻译过来是饼干的意思。
2. Cookie 是服务器发送到用户浏览器的一小块数据，形如``name=value``，**存储在浏览器**。
3. Cookie 会在浏览器下次向同一服务器再次发起请求时被携带并发送到服务器上。
4. 每个 Cookie 的大小不能超过 4 kb。
### 如何创建 Cookie
![图示](https://img-blog.csdnimg.cn/20210531080523889.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class CookieServlet extends BaseServlet {
    protected void createCookie(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1、创建 Cookie 对象
        Cookie cookie = new Cookie("key1", "value1");
        // 2、通知客户端保存 Cookie
        response.addCookie(cookie);
        
        response.getWriter().write("Cookie 创建成功");
    }
}
```
### 服务器如何获取 Cookie
服务器获取客户端的 Cookie 只需要一行代码：request.getCookies()，返回值是 Cookie[] 数组
![图示](https://img-blog.csdnimg.cn/20210531084806679.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
protected void getCookie(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    Cookie[] cookies = request.getCookies();
    for(Cookie cookie : cookies){
        // getName()方法返回 Cookie 的 key
        // getValue()方法返回 Cookie 的 value
        System.out.println("Cookie[" + cookie.getName() + "=" + cookie.getValue() + "] <br/>");
        }

    // 获得指定名称的 Cookie 对象
    Cookie target = null;
    for(Cookie cookie : cookies){
        if("key2".equals(cookie.getName())){
            target = cookie;
            break;
        }
    }
    if(target != null){
        response.getWriter().write("找到了指定名称的Cookie");
    }
}
```
### Cookie 值的修改
方案一：
1、先创建一个与要修改的 Cookie 同名的 Cookie 对象
2、在构造器中，同时赋予新的 Cookie 值
3、调用 response.addCookie(cookie);

```java
protected void UpdateCookie(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    // 1、先创建一个与要修改的 Cookie 同名的 Cookie 对象
    // 2、在构造器中，同时赋予新的 Cookie 值
    Cookie cookie = new Cookie("key1", "newValue1");
    // 3、调用 response.addCookie(cookie); 通知客户端保存修改
    response.addCookie(cookie);
}
```
方案二：
1、先查找到需要修改的 Cookie 对象
2、调用 setValue() 方法赋予新的 Cookie 值
3、调用 response.addCookie() 通知客户端保存修改
### 谷歌浏览器如何查看 Cookie
![图示](https://img-blog.csdnimg.cn/20210531090955247.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### Cookie 生命控制
* Cookie 的生命控制指的是如何管理 Cookie 什么时候被销毁（删除）
* setMaxAge()：
&nbsp;&nbsp;&nbsp; 正数：表示在指定的秒数后删除
&nbsp;&nbsp;&nbsp; 负数：表示浏览器一关，Cookie 就会被删除
&nbsp;&nbsp;&nbsp; 零：表示马上删除 Cookie
**注：默认值是 -1**
### Cookie 有效路径 Path 的设置
Cookie 的 path 属性可以有效地过滤哪些 Cookie 可以发送给服务器，哪些不发。

path 属性是通过请求的地址来进行有效的过滤。
![图示](https://img-blog.csdnimg.cn/20210531095001553.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### Cookie：免输入用户名登录
场景：第一次登录时输入框的用户名和密码框都是空的，当登陆成功退出后，第二次再登录的时候就会发现用户名已经填写好了。
![图示](https://img-blog.csdnimg.cn/20210531164833126.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="http://localhost:8080/Cookie_Session/loginServlet" method="get">
        用户名：<input type="text" name="username" value="${cookie.username.value}" /><br>
        密码：<input type="password" name="password" /><br>
        <input type="submit" value="登陆" />
    </form>
</body>
</html>
```
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class LoginServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        if("admin123".equals(username) && "123456".equals(password)){
            Cookie cookie = new Cookie("username", username);
            cookie.setMaxAge(60 * 60 * 24 * 7);// 当前 cookie 一周内有效
            response.addCookie(cookie);
            System.out.println("登陆成功");
        }else{
            System.out.println("登陆失败");
        }
    }
}
```
## Session
### 什么是 Session 会话
1. Session 是一个接口（HttpSession）。
2. Session 就是会话，它是用来维护一个客户端和服务器之间关联的一种技术。
3. 每个客户端都有自己的 Session 会话。
4. Session 会话中，我们经常用来保存用户登录的信息。
5. **Session 是保存在服务器，而 Cookie 是保存在客户端**。
### Session 的创建和获取（id 号，是否为新）
![图示](https://img-blog.csdnimg.cn/20210601154603547.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class SessionServlet extends BaseServlet {

    protected void createOrGetSession(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 创建和获取 Session 会话对象
        HttpSession session = request.getSession();
        // 判断当前的 Session 会话是否是新创建出来的
        boolean aNew = session.isNew();
        // 获取 Session 会话的唯一标识 id
        String id = session.getId();

        response.getWriter().write("得到的 Session 会话的 id 是：" + id);
        response.getWriter().write("这个 Session 是否是新创建出来的：" + aNew);
    }
}
```
### Session 域对象的存取

```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class SessionServlet extends BaseServlet {
    protected void setAttribute(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.getSession().setAttribute("key1", "value1");
        response.getWriter().write("Session 中保存数据成功！");
    }
    protected void getAttribute(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Object key1 = request.getSession().getAttribute("key1");
        response.getWriter().write("从 Session 中获取出 key1 的数据是：" + key1);
    }
}
```
### Session 的生命周期

**Session 默认的超时时长是：30分钟**
![图示](https://img-blog.csdnimg.cn/20210601153419897.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
如果说，你希望你的web工程，默认的 Session 的超时时长为其它时长，那么你可以在你自己的 web.xml 配置文件中做以下配置，就可以修改你的 web 工程中所有 Session 的默认超时时长。
```xml
<!--表示在当前的 web 工程里，创建出来的所有 Session 的默认超时时长是 20 分钟-->
<session-config>
    <session-timeout>20</session-timeout>
</session-config>
```
**Session 超时概念的介绍：**
![图示](https://img-blog.csdnimg.cn/20210601153846116.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;
public class SessionServlet extends BaseServlet {
    protected void life(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
       // 获取 Session 对象
        HttpSession session = request.getSession();

        // 获取 Session 的默认超时时长
        int maxInactiveInterval = session.getMaxInactiveInterval();

        // 设置当前 Session 3秒后超时
        session.setMaxInactiveInterval(3);

        // 让 Session 会话马上超时
        session.invalidate();
    }
}
```
### 浏览器 和 Session 之间关联的技术内幕
Session 技术，底层其实是基于 Cookie 技术来实现的。
![图示](https://img-blog.csdnimg.cn/20210601160651512.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
## Cookie 和 Session 的不同
* 作用范围不同。Cookie 保存在客户端（浏览器）；Session 保存在服务器端。
* 存取方式不同。Cookie 只能保存 ASCII；Session 可以存任意数据类型。
* 有效期不同。Cookie 可以设置为长时间保持，比如我们经常使用的默认登陆功能；Session 一般失效时间较短，客户端关闭或者 Session 超时都会失效。
* 隐私策略不同。Cookie 存储在客户端，比较容易遭到不法窃取；Session 存储在服务端，安全性相对高一点。
* 存储大小不同。单个 Cookie 保存的数据不能超过 4K；Session 可存储数据远高于 Cookie。
## 为什么需要 Cookie 和 Session
* **Cookie 的出现是因为 HTTP 是无状态的一种协议**。换句话说，服务器记不住你，可能你每刷新一次网页，就要重新输入一次账号密码进行登录，这显然让人无法接受。
* Cookie 和 Session 是如何配合的？
![图示](https://img-blog.csdnimg.cn/d931576a7c5741fcab829e5f4e43ff8d.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
（1）用户第一次请求服务器的时候，服务器**根据用户提交的相关信息，创建对应的 Session**，请求返回时将此 Session 的唯一标识信息 SessionID 返回给浏览器，浏览器接收到服务器返回的 SessionID 信息后，会将此信息存入到 Cookie 中，同时 Cookie 记录此 SessionID 属于哪个域名。
（2）当用户第二次访问服务器时，请求会自动判断该域名下是否存在 Cookie 信息，如果存在自动将 Cookie 信息也发送给服务端，服务端会从 Cookie 中获取 SessionID，再根据 SessionID 查找对应的 Session 信息，如果没有找到说明用户没有登陆或者登陆失效，如果找到 Session 证明用户已经登录可以执行后面的操作。
## 如果浏览器禁止了 Cookie 怎么办
* 第一种方案：每次请求中都携带一个 SessionID 的参数，也可以以 Post 的方式提交，也可以在请求的地址后面拼接``xxx?SessionID=123456``
* 第二种方案：Token 机制。
（1）Token 的意思是“令牌”，是服务端生成的一串字符串，作为客户端进行请求的一个标识。
（2）当用户第一次登陆后，服务器根据提交的用户信息生成一个 Token，响应时将 Token 返回给客户端，以后客户端只需带上这个 Token 前来请求数据即可，无需再次登陆验证。
## 分布式 Session 问题
* 如果用户在 A 服务器登陆了，第二次请求跑到服务器 B 就会出现登录失效的问题。
* 一般采用 **共享Session** 的解决方案。将用户的 Session 等信息使用缓存中间件来统一管理，保障分发到每一个服务器的响应结果都一致。