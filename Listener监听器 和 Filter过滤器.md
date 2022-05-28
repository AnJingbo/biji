

#### 什么是 Listener 监听器？
1. Listener 监听器，它是 JavaWeb 三大组件之一。JavaWeb 的三大组件分别是：Servlet 程序、Filter 过滤器、Listener 监听器。
2.  Listener监听器，它是 JavaEE 的规范，就是接口。
3. Listener监听器的作用是，监听某种事物的变化，然后通过回调函数，反馈给用户（程序）去做一些响应的处理。
#### ServletContextListener 监听器
ServletContextListener 它可以监听 ServletContext 对象的创建和销毁。

ServletContext 对象在 web 工程启动的时候创建，在 web 工程停止的时候销毁。

监听到创建和销毁之后都会分别调用 ServletContextListener 监听器的方法反馈。

**如何使用 ServletContextListener 监听器监听 ServletContext 对象？**

1、编写一个类去实现 ServletContextListener

2、实现其两个回调方法

3、到 web.xml 中去配置监听器
#### 什么是过滤器 Filter
1. Filter 过滤器，它是 JavaWeb 的三大组件之一。三大组件分别是：Servlet 程序、Listener 监听器、Filter 过滤器。
2. Filter 过滤器，它是 JavaEE 的规范，也就是接口。
3. Filter 过滤器，它的作用是：**拦截请求**，过滤响应。
拦截请求常见的应用场景有：
1、权限检查
2、日记操作
3、事务管理
等等
#### Filter 过滤器的使用步骤
1. 编写一个类去实现 Filter 接口。
2. 实现过滤方法 doFilter()。
3. 到 web.xml 中去配置 Filter 的拦截路径。

问题：要求在你的 web 工程下，有一个 admin 目录。这个 admin 目录下的所有资源（比如 html页面、jpg图片、jsp文件等等）都必须是用户登录之后才允许访问。

思路：用户登录之后都会把用户登录的信息保存到 Session 域中，所以要检查用户是否登陆，可以判断 Session 中是否包含有用户登陆的信息即可。
![图示](https://img-blog.csdnimg.cn/20210602154327838.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```xml
<!-- filter 标签用于配置一个 Filter 过滤器 -->
<filter>
    <!--给filter起一个别名-->
    <filter-name>AdminFilter</filter-name>
    <!--配置filter的全类名-->
    <filter-class>com.atjingbo.admin.AdminFilter</filter-class>
</filter>
<!--filter-mapping配置filter过滤器的拦截路径-->
<filter-mapping>
    <!--filter-name表示当前的拦截路径给哪个filter使用-->
    <filter-name>AdminFilter</filter-name>
    <!--url-pattern配置拦截路径
        / 表示请求地址为 http://ip:port/工程路径/ 映射到 idea 的web目录
        /admin/* 表示请求地址为：http://ip:port/工程路径/admin/*
    -->
    <url-pattern>/admin/*</url-pattern>
</filter-mapping>
```
```java
import javax.servlet.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class AdminFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {}
    
    // doFilter() 方法，专门用于拦截请求，可以做权限检查
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
    
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        HttpSession session = httpServletRequest.getSession();
        Object user = session.getAttribute("user");

        // 如果等于 null，说明还没有登陆
        if(user == null){
            servletRequest.getRequestDispatcher("/login.jsp").forward(servletRequest, servletResponse);
        }else{
            // 让程序继续往下访问用户的目标资源
            filterChain.doFilter(servletRequest, servletResponse);
        }
    }
    @Override
    public void destroy() {}
}
```
```java
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        if("admin".equals(username) && password.equals(password)){
            req.getSession().setAttribute("user", username);
            System.out.println("登陆成功");
        }else{
            req.getRequestDispatcher("/login.jsp").forward(req, resp);
        }
    }
}
```
#### Filter 的生命周期
Filter 的生命周期包含以下几个方法：
1. 构造器方法
2. init() 初始化方法
第 1、2 步，在 web 工程启动的时候执行（Filter 已经创建）
3. doFilter() 过滤方法
第 3 步，每次拦截到请求，就会执行
4. destroy() 销毁方法
第 4 步，停止 web 工程的时候，就会执行（停止 web 工程，也会销毁 Filter 过滤器）
#### FilterConfig 类
FilterConfig 类见名知意，它是 Filter 过滤器的配置文件。

Tomcat 每次创建 Filter 的时候，也会同时创建一个 FilterConfig 类，这里包含了 Filter 配置文件的配置信息

FilterConfig 类的作用是获取 Filter 过滤器的配置文件
1. 获取 Filter 的名称 filter-name 的内容
2. 获取在 web.xml 中配置的 init-param 初始化参数
3. 获取 ServletContext 对象
```java
public void init(FilterConfig filterConfig) throws ServletException {
        // 1. 获取 Filter 的名称 filter-name 的内容
        System.out.println("filter-name 的值是：" + filterConfig.getFilterName());
        // 2. 获取在 web.xml 中配置的 init-param 初始化参数
        System.out.println("username 的值是" + filterConfig.getInitParameter("username"));
        System.out.println("password 的值是" + filterConfig.getInitParameter("password"));
        // 3. 获取 ServletContext 对象
        System.out.println(filterConfig.getServletContext());
    }
```
#### FilterChain 过滤器链
FilterChain：就是过滤器链（多个过滤器如何一起工作）
![图示](https://img-blog.csdnimg.cn/20210603150356550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
#### Filter 的拦截路径
##### 精确匹配
```xml
<url-pattern>/a.jsp</url-pattern>

以上配置的路径，表示请求地址必须为：http://ip:port/工程路径/a.jsp
```
##### 目录匹配
```xml
<url-pattern>/admin/*</url-pattern>

以上配置的路径，表示请求地址必须为：http://ip:port/工程路径/admin/*
```
##### 后缀名匹配
```xml
<url-pattern>/*.html</url-pattern>
以上配置的路径，表示请求地址必须以 .html 结尾才会拦截到。

<url-pattern>/*.abc</url-pattern>
以上配置的路径，表示请求地址必须以 .abc 结尾才会拦截到。
```
**注：Filter 过滤器它只关心请求的地址是否匹配，不关心请求的资源是否存在！**