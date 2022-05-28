

### 什么是 JSP，它的作用是什么
JSP 的全称是 Java Server Pages，Java 的服务器页面。

JSP 的主要作用是代替 Servlet 程序回传 html 页面的数据。
因为 Servlet 程序回传 html 页面数据是一件很繁琐的事情，开发成本和维护成本都极高。

### JSP 的本质
JSP 本质上是一个 Servlet 程序。
当我们第一次访问 JSP 页面的时候，Tomcat 服务器会帮我们把 JSP 页面翻译成为一个 Java 源文件。并且对它编译成为 .class 字节码文件。
### JSP 头部的 page 指令
JSP 的 page 指令可以修改 JSP 页面中一些重要的属性，或者行为。

<%@ page contentType="text/html;charset=UTF-8" language="java" %>
1. language 属性：表示 JSP翻译后是什么语言的文件。暂时只支持 Java
2. contentType 属性：表示 JSP 返回的数据类型是什么。也是源码中 response.setContentType() 的参数值
3. pageEncoding 属性：表示当前 JSP 页面文件本身的字符集。
4. import 属性：跟 Java 源代码中的一样，用于导包，导类
5. autoFlush 属性：设置当 out 输出流缓冲区满了之后，是否自动刷新缓冲区。默认值是 true。
6. buffer 属性，设置 out 缓冲区的大小，默认值是 8kb。
7. errorPage 属性：设置当 JSP 页面运行出错的时候，自动跳转去的错误页面的路径。这个路径一般都是以斜杠 / 打头，它表示请求地址为：http://ip:port/工程路径/，映射到代码的 Web 目录。
8. isErrorpage 属性：设置当前 JSP 页面是否是错误信息页面。默认时 false。如果是 true 可以获取异常信息。
9. session 属性：设置访问当前 JSP 页面，是否会创建 HttpSession 对象。默认是 true。
10. extends 属性：设置 JSP 翻译出来的 Java 类默认继承谁
### JSP中常用脚本
#### 1、声明脚本（极少使用）
格式：<%! &nbsp;&nbsp;  java代码 &nbsp;&nbsp; %>

作用：可一个 JSP 翻译出来的 Java 类定义属性和方法，甚至是静态代码块，内部类等。
#### 2、表达式脚本（常用）
格式：<%= 表达式 %>

作用：在 JSP 页面上输出数据。

表达式脚本的特点：
1、所有表达式脚本都会被翻译到 _jspService() 方法中。

2、表达式脚本都会被翻译成为 out.print() 输出到页面上。

3、由于表达式脚本翻译的内容都在 _jspService() 方法中，所以 _jspService() 方法中的对象都可以直接使用。

4、表达式脚本中的表达式不能以分号结束。

```java
示例：
    <%=123%>
    <%=123.321%>
    <%="这是JSP"%>
    <%=request.getParameter("username")%>
    
    如果是：<%=request.getParameter("username");%>，则会报错。
    因为翻译出来会是这样：out.print(request.getParameter("username"););
```
#### 3、代码脚本
格式：<% java语句 %>

作用：可以在 JSP 页面中，编写我们自己需要的功能（写的是 Java 语句）

代码脚本的特点：
1、代码脚本翻译之后都在 _jspService() 方法中。

2、代码脚本由于翻译到  _jspService() 方法中，所以 _jspService() 方法中的现有对象都可以直接使用。

3、还可以由多个代码脚本组合完成一个完整的 Java 语句。

4、代码脚本还可以和表达式脚本一起组合使用，在 JSP 页面上输出数据。

```java
示例：
    if 语句：
    <%
        int i = 11;
        if(i == 10){
            System.out.println("i 等于 10");
        }else{
            System.out.println("i 不等于10");
        }
    %>
    for 语句：
    <%
        for(int j = 0; j < 10; j++){
            System.out.println(j);
        }
    %>
    翻译后 Java 文件中 _jspService() 方法内的对象都可以写：
    <%
        System.out.println("用户名的请求参数是：" + request.getParameter("username"));
    %>
    代码脚本 和 表达式脚本 一起使用：
    <%
      for(int k = 0; k < 5; k++){
    %>
    <%=k%>
    <%
      }
    %>
```
### JSP 中的三种注释
1. html 注释

\<!-- 这是 html 注释 -->

html 注释会被翻译到 Java 源代码中。在 _jspService() 方法里，以 out.writer() 输出到客户端

2. Java 注释

<%
// 单行Java注释
/\* 多行Java注释 \*/
%>

Java 注释会被翻译到 Java 源代码中

3. JSP 注释

<%-- 这是JSP注释 --%>

JSP 注释可以注掉 JSP 页面中的所有代码。
### JSP 九大内置对象
![图示](https://img-blog.csdnimg.cn/20210527152145558.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### JSP 四大域对象
![图示](https://img-blog.csdnimg.cn/20210527155119984.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### JSP 中的 out 输出和 response 输出的区别
response 是表示响应，我们经常用于设置返回给客户端的内容（输出）

out 也是给用户做输出使用的。![图示](https://img-blog.csdnimg.cn/20210527193412786.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
由于 JSP 翻译之后，底层源代码都是使用 out 来进行输出，所以一般情况下，我们在 JSP 页面中统一使用 out 来进行输出，避免打乱页面输出内容的顺序。

out.write() 输出字符串没有问题（如果要输出整型，在源码里会先把整型转换成字符型再输出）

out.print() 输出任意数据都没有问题（都转换成为字符串之后调用的 write() 输出）
### 常用标签之静态包含
![图示](https://img-blog.csdnimg.cn/20210528081645278.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```java
格式：
<%--
        <%@include file=""%> 就是静态包含
        filter 指定你要包含的jsp页面的路径
        地址中的第一个斜杠表示：http://ip:port/工程路径/ 映射到代码的web路径

        静态包含的特点：
            1、静态包含不会翻译被包含的 JSP 页面
            2、静态包含其实是把被包含的 JSP 页面的代码拷贝到包含的位置执行输出
    --%>
    
    <%@include file="/a.jsp"%>
```

### 常用标签之动态包含
```java
<%--
        <jsp:include page=""></jsp:include> 就是动态包含
        page属性指定你要包含的jsp页面的路径
        动态包含也可以像静态包含一样，将被包含的内容执行输出到包含位置

        动态包含的特点：
            1、动态包含会把包含的JSP页面也翻译成为java代码
            2、动态包含底层代码使用如下代码去调用被包含的JSP页面执行输出：
                JspRuntimeLibrary.include(request, response, "/b.jsp", false);
            3、动态包含，还可以传递参数
    --%>
    
    <jsp:include page="/b.jsp">
        <jsp:param name="username" value="hhh"/>
    </jsp:include>
```
**动态包含的底层原理：**
![图示](https://img-blog.csdnimg.cn/20210528084744398.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 常用标签之请求转发
```java
    <%--
        <jsp:forward page=""></jsp:forward> 是请求转发标签，它的功能就是请求转发
        page属性设置请求转发的路径
    --%>
    <jsp:forward page="a.jsp"></jsp:forward>
```
#### 请求转发的使用说明
![图示](https://img-blog.csdnimg.cn/20210528092929582.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 什么是 Listener 监听器？
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