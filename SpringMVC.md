

## 一、SpringMVC 概述
### 1.1 SpringMVC 简介
* SpringMVC：是 spring 框架的一部分，专门用来做 web 开发的。可以理解为 servlet 的一个升级。
* SpringMVC 能够创建对象，放到 SpringMVC 容器中。**SpringMVC 容器里放的是控制器对象**。
我们要做的是使用 **@Controller  创建控制器对象**，把这个对象放入到 SpringMVC 容器中，把控制器对象当作控制器使用。这个**控制器对象能接受用户的请求，显示处理结果**，就当作是一个 servlet 使用。
* 使用 @Controller 注解创建的是一个普通类的对象，不是 Servlet，SpringMVC 赋予了控制器对象一些额外的功能。
* web 开发**底层是 servlet**，SpringMVC 中有一个对象是 Servlet：DispatcherServlet（中央调度器）。**DispatcherServlet（中央调度器）： 负责接收用户的所有请求，用户把请求给了 DispatcherServlet，之后 DispatcherServlet 把请求转发给我们的控制器对象，最后是控制器对象处理请求。**
* DispatcherServlet 作用：
（1）创建 WebApplicationContext 对象，读取配置文件，进而创建控制器对象。
（2）它是一个 servlet，需要接收用户请求，显示处理结果。
### 1.2 SpringMVC 优点
（1）基于 MVC 架构，功能分工明确，解耦合。
（2）容易理解，上手快，使用简单。SpringMVC 也是轻量级的，jar 很小，不依赖特定的接口和类。
（3）作为 Spring 框架的一部分，能够使用 Spring 的 IOC 和 AOP，方便整合其它框架。
（4）SpringMVC 强化注解的使用，在控制器、Service、Dao 都可以使用注解，方便灵活。（使用 @Controller 创建处理器对象，@Service 创建业务对象，@Autowired 或者 @Resource 在控制器类中注入 Service、在 Service 类中注入 Dao）
### 1.3 SpringMVC 的 MVC 组件
![图示](https://img-blog.csdnimg.cn/5ba4254587ea41cba8cc4cf0456da9bd.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 1.4 三层框架的对应
SpringMVC：页面层，接受用户请求，显示处理结果。
Spring：业务层，处理业务逻辑，用 spring 创建 service、dao、工具类等对象。
MyBatis：持久层，访问数据库，对数据库进行增删改查。
### 1.5 第一个 SpringMVC 程序
* 实现步骤：
1. 新建 web 的 maven 工程。
2. 加入依赖。
spring-webmvc 依赖（间接把 spring 的依赖都加入到项目）
jsp 依赖、servlet 依赖。
3. 重点：在 web.xml 中注册 SpringMVC 框架的核心对象 DispatcherServlet。
（1）DispatcherServlet 叫做中央调度器，是一个 servlet，它的父类是 HttpServlet。
（2）DispatcherServlet 也叫做前端控制器（front controller）
（3）DispatcherServlet 负责接受用户提交的请求，调用其它的控制器对象，并把请求的处理结果显示给用户。
4. 创建一个发起请求的页面 index.jsp。
5. 创建控制器（处理器）类：
（1）在类的上面加入 @Controller 注解，创建对象，并放入到 SpringMVC 容器中。（@Controller：创建控制器类的对象，接收请求，处理请求）
（2）在类中的方法上面加入 @RequestMapping 注解。
6. 创建一个作为结果的 jsp，显示请求的处理结果。
7. 创建 SpringMVC 的配置文件（和 spring 的配置文件一样）
（1）声明组件扫描器，指定 @Controller 注解所在的包名。
（2）声明视图解析器，是来帮助处理视图的。
* 具体实现：
**3、web.xml**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!-- 声明(注册) springmvc 的核心对象 DispatcherServlet。
        需要在 tomcat 服务器启动后，就创建 DispatcherServlet 对象的实例。
        为什么要创建 DispatcherServlet 对象的实例呢？
        因为 DispatcherServlet 在它的创建过程中，会同时创建 springmvc 容器对象，
        读取 springmvc 的配置文件，把这个配置文件中的对象都创建好，当用户发起请求时
        就可以直接使用对象了。

        servlet 的初始化会执行 init() 方法，DispatcherServlet 在 init() 方法中{
            // 创建容器，读取配置文件
            WebApplicationContext ctx = new ClassPathXmlApplicationContext("springmvc.xml");
            // 把容器对象放入 ServletContext 中
            getServletContext().setAttribute(key, ctx)
        }
    -->
    <servlet>
        <servlet-name>myweb</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <!-- 自定义 springmvc 读取的配置文件的位置。
            springmvc 创建容器对象时，读取的配置文件的路径是：/WEB-INF/<servlet-name>-servlet.xml
        -->
        <init-param>
            <!-- springmvc 的配置文件的位置的树形 -->
            <param-name>contextConfigLocation</param-name>
            <!-- 指定自定义文件的位置 -->
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>

        <!-- 在 tomcat 启动后，创建 Servlet 对象
            load-on-startup：表示 tomcat 启动后创建对象的顺序。它的值是大于等于 0 的整数。
                             数值越小，tomcat 创建对象的时间越早。
        -->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>myweb</servlet-name>
        <!--
             使用框架的时候，url-pattern 可以使用两种值：
             （1）使用扩展名的方式。语法：*.xxxx，xxxx是自定义的扩展名。常用的方式：*.do, *.action, *.mvc 等。
                 http://localhost:8080/myweb/some.do
             （2）使用斜杠 "/"
        -->
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
</web-app>
```
**4、index.jsp**
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <p><a href="some.do">发起some.do的请求</a></p>
</body>
</html>
```
**5、控制器类 MyController**
```java
/*
* @Controller：创建处理器对象，对象放在 springmvc 容器中
* 位置：在类的上面
*
* 能处理请求的都是控制器（处理器）：MyController 能处理请求，叫做后端控制器（back controller）
* */
@Controller
public class MyController {
    /*
    * 处理用户提交的请求，springmvc 中是使用方法来处理的。
    * 方法是自定义的，可以有多种返回值、多种参数，方法名称自定义。
    * */

    /*
    * 准备使用 doSome() 方法处理 some.do 请求。
    *
    * @RequestMapping：请求映射，作用是把一个请求地址和一个方法绑定在一起。
    *                  一个请求指定一个方法处理。
    *         放在方法上面时的属性：1、value 是一个 String，表示请求的 uri 地址（比例中：some.do）
    *                 value 的值必须是唯一的，不能重复，在使用时推荐地址以 "/" 开头
    *         放在类上面时的属性：value：所有请求地址的公共部分，叫做模块名称。
    * 
    *         位置：1、在方法的上面（常用）
    *              2、在类的上面
    *
    * 说明：使用 @RequestMapping 修饰的方法叫做处理器方法（或者控制器方法）
    *      使用 @RequestMapping 修饰的方法是用来处理请求的，类似 Servlet 中的 doGet、doPost
    *
    * 返回值：ModelAndView：表示本次请求的处理结果。
    *        Model：数据，请求处理完成后，要显示给用户的数据。
    *        View：视图，比如 jsp 等等。
     * */
    @RequestMapping(value="/some.do")
    public ModelAndView doSome(){
        // 处理 some.do 的请求

        ModelAndView mv = new ModelAndView();

        // 添加数据，框架在请求的最后把数据放入到 request 作用域。
        // request.setAttribute("msg", "欢迎使用 springmvc")
        mv.addObject("msg", "欢迎使用 springmvc");

        // 指定视图，指定视图的完整路径。
        // 框架对视图执行的 forward 操作，request.getRequestDispatcher("/show.jsp").forward(...)
        //mv.setViewName("/WEB-INF/view/show.jsp");

        // 当配置了视图解析器后，可以使用逻辑名称（文件名），指定视图
        // 框架会使用视图解析器的 前缀+逻辑名称+后缀 组成完整路径，这里就是字符串连接操作。
        // WEB-INF/view/ + show + .jsp
        mv.setViewName("show");

        // 返回 mv
        return mv;
    }
}
```
**6、show.jsp**
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h3>/WEB-INF/view/show.jsp 从 request 域中获取数据</h3>
    <h3>msg中的数据：${msg}</h3>
</body>
</html>
```
**7、 SpringMVC 配置文件**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 声明组件扫描器，指定 @Controller 注解所在的包名 -->
    <context:component-scan base-package="com.atjingbo.controller" />

    <!-- 声明 springmvc 框架中的视图解析器，帮助开发人员设置视图文件的路径 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 前缀：视图文件的路径 -->
        <property name="prefix" value="/WEB-INF/view/" />
        <!-- 后缀：视图文件的扩展名 -->
        <property name="suffix" value=".jsp" />
    </bean>
</beans>
```
### 1.6 springmvc 请求的处理流程
（1）用户发起 some.do 请求。
（2）请求先发送给 tomcat 服务器，tomcat 会读取 web.xml，根据该文件中的 url-pattern 可以知道 *.do 的请求是给中央调度器 DispatcherServlet 的。
（3）DispatcherServlet 会读取 springmvc 的配置文件，根据该配置文件知道 some.do 请求对应的是 doSome() 方法。
（4）DispatcherServlet 把 some.do 转发给 MyController 的 doSome() 方法。
（5）框架执行 doSome() 方法，把得到的 ModelAndView 进行处理，转发给 show.jsp。
* 图示：
![图示](https://img-blog.csdnimg.cn/a88a0bee92f14b9284d9e7a91ac302c7.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 1.7 sprigmvc 执行过程分析
* （1）tomcat 启动，创建容器的过程。
通过 load-on-start 标签指定的 1，创建 DispatcherServlet 对象，DispatcherServlet 的父类是继承 HttpServlet 的，它是一个 servlet，**DispatcherServlet 在被创建时，会执行 init() 方法。**
```java
在 init() 方法中：
    // 创建容器，读取配置文件
    WebApplicationContext ctx = new ClassPathXmlApplicationContext("springmvc.xml");
    // 把容器对象放入 ServletContext 中
    getServletContext().setAttribute(key, ctx)
```
上面创建容器的作用：创建 @Controller 注解所在类的对象，本例中就是创建 MyController 对象，这个对象会放入到 springmvc 的容器中。容器是一个 map，类似 map.put("myController", MyController 对象)
* （2）请求的处理过程
执行 servlet 的 service()，会在 DispatcherServlet 的 doDispatch() 方法中，调用 MyController 的 doSome() 方法。doDispatch() 方法是 DispatcherServlet 类中的核心方法，所有的请求都在这个方法中完成。
## 二、SpringMVC 注解式开发
### 2.1 指定请求提交方式（method 属性）
```java
@Controller
public class MyController {
    /**
     * @RequestMapping：请求映射
     *     属性：method，表示请求的方式。它的值是 RequestMethod 类的枚举值
     *             例如：表示 get 请求方式：RequestMethod.GET
     *                  表示 post 请求方式：RequestMethod.POST
     * 
     */
     
    // 指定 some.do 使用 get 请求方式。
    @RequestMapping(value="/some.do", method = RequestMethod.GET)
    public ModelAndView doSome(){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg", "欢迎使用 springmvc");
        mv.setViewName("show");
        return mv;
    }
    // 指定 other.do 使用 post 请求方式。
    @RequestMapping(value="/other.do", method = RequestMethod.POST)
    public ModelAndView doOther(){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg", "欢迎使用 springmvc");
        mv.setViewName("show");
        return mv;
    }
    // 不指定请求方式，没有限制。
    @RequestMapping(value="/first.do")
    public ModelAndView doFirst(){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg", "欢迎使用 springmvc");
        mv.setViewName("show");
        return mv;
    }
}
```
### 2.2 处理器方法的参数
* 接受请求参数，使用的是处理器方法的形参。
* 处理器方法可以包含以下四类参数，**这些参数会在系统调用时由系统自动赋值**，即程序员可以在方法内直接使用。
（1）HttpServletRequest
（2）HttpServletResponse
（3）HttpSession
（4）请求中所携带的请求参数：逐个接收；对象接收。这两种方式最常用。
```java
@Controller
public class MyController {
    @RequestMapping(value="/first.do")
    public ModelAndView doFirst(HttpServletRequest req, HttpServletResponse resp, HttpSession session){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg", "欢迎使用 springmvc");
        mv.setViewName("show");
        return mv;
    }
}
```
#### 2.2.1 逐个接收方式
注意：在提交参数时，get 请求方式中不会出现中文乱码问题，但使用 post 方式提交请求会出现中文乱码问题，因此需要在 web.xml 中使用过滤器处理乱码的问题。
过滤器可以自定义，也可以使用框架中提供的过滤器 CharacterEncodingFilter。

index.jsp：
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <p>提交参数给 Controller</p>

    <form action="first.do" method="post">
        姓名：<input type="text" name="name" /> <br/>
        年龄：<input type="text" name="age" /> <br/>
        <input type="submit" value="提交参数" />
    </form>
</body>
</html>
```
web.xml：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!-- 声明(注册) springmvc 的核心对象 DispatcherServlet。
        需要在 tomcat 服务器启动后，就创建 DispatcherServlet 对象的实例。
        为什么要创建 DispatcherServlet 对象的实例呢？
        因为 DispatcherServlet 在它的创建过程中，会同时创建 springmvc 容器对象，
        读取 springmvc 的配置文件，把这个配置文件中的对象都创建好，当用户发起请求时
        就可以直接使用对象了。

        servlet 的初始化会执行 init() 方法，DispatcherServlet 在 init() 方法中{
            // 创建容器，读取配置文件
            WebApplicationContext ctx = new ClassPathXmlApplicationContext("springmvc.xml");
            // 把容器对象放入 ServletContext 中
            getServletContext().setAttribute(key, ctx)
        }
    -->
    <servlet>
        <servlet-name>myweb</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <!-- 自定义 springmvc 读取的配置文件的位置。
            springmvc 创建容器对象时，读取的配置文件的路径是：/WEB-INF/<servlet-name>-servlet.xml
        -->
        <init-param>
            <!-- springmvc 的配置文件的位置的树形 -->
            <param-name>contextConfigLocation</param-name>
            <!-- 指定自定义文件的位置 -->
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>

        <!-- 在 tomcat 启动后，创建 Servlet 对象
            load-on-startup：表示 tomcat 启动后创建对象的顺序。它的值是大于等于 0 的整数。
                             数值越小，tomcat 创建对象的时间越早。
        -->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>myweb</servlet-name>
        <!--
             使用框架的时候，url-pattern 可以使用两种值：
             （1）使用扩展名的方式。语法：*.xxxx，xxxx是自定义的扩展名。常用的方式：*.do, *.action, *.mvc 等。
                 http://localhost:8080/myweb/some.do
             （2）使用斜杠 "/"
        -->
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>


    <!-- 声明（注册）过滤器，解决 post 请求乱码的问题 -->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!-- 设置项目中使用的字符编码 -->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <!-- 强制请求对象（HttpServletRequest）使用 encoding 编码的值-->
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <!-- 强制应答对象（HttpServletResponse）使用 encoding 编码的值-->
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <!-- /*：表示强制所有的请求先通过过滤器处理 -->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```
```java
public class MyController {
    /*
    * 逐个接收请求参数：
    *     要求：处理器（控制器）方法的形参名和请求中的参数名必须一致
    *         同名的请求参数赋值给同名的形参。
    * 框架是如何接收请求参数的：
    *     1、使用 request 对象接收请求参数。
    *         String strName = request.getParameter("name");
    *         String strAge = request.getParameter("age");
    *     2、springmvc 框架通过 DispatcherServlet 调用 MyController 的 doSome() 方法。
    *         调用方法时，按名称对应，把接收的参数赋值给形参。
    *         doSome(strName, Integer.valueOf(strAge));
    *         框架会提供类型转换的功能
    * */
    @RequestMapping(value="/first.do")
    public ModelAndView doFirst(String name, Integer age){
        ModelAndView mv = new ModelAndView();
        mv.addObject("myName", name);
        mv.addObject("myAge", age);
        mv.setViewName("show");
        return mv;
    }
}
```
#### 2.2.2 校正请求参数名 @RequestParam
* 校正请求参数名：是指如果请求 url 所携带的参数名称与处理器方法中指定的参数名不相同时，需要在处理器方法的形参前，添加一个 @RequestParam 注解。
* @RequestParam 是用在逐个接收的方式中。
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <p>请求参数名和处理器方法的形参名不一样</p>
    <form action="second.do" method="post">
        姓名：<input type="text" name="rname" /> <br/>
        年龄：<input type="text" name="rage" /> <br/>
        <input type="submit" value="提交参数" />
    </form>
</body>
</html>
```
```java
@Controller
public class MyController {
    /**
     * 请求中参数名和处理器方法的形参名不一样：
     * @RequestParam：逐个接收请求参数的方法中，解决请求中参数名和形参名不一样的问题。
     *      属性：（1）value：请求中的参数名称
     *           （2）required：是一个布尔类型的值，默认是 true。
     *                     true：表示请求中必须包含此参数。
     *       位置：在处理器方法的形参定义的前面。
     * @param name
     * @param age
     * @return
     */
    @RequestMapping(value="/second.do")
    public ModelAndView doSecond(@RequestParam(value="rname", required = false)String name,
                                 @RequestParam(value="rage", required = false)Integer age){
        System.out.println("name = " + name + ", age = " + age);
        ModelAndView mv = new ModelAndView();
        mv.addObject("myName", name);
        mv.addObject("myAge", age);
        mv.setViewName("show");
        return mv;
    }
}
```
#### 2.2.3 对象接收方式
* 将处理器方法的形参定义为一个对象，只要保证请求参数名与这个对象的属性同名即可。
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="third.do" method="post">
        姓名：<input type="text" name="name" /> <br/>
        年龄：<input type="text" name="age" /> <br/>
        <input type="submit" value="提交参数" />
    </form>
</body>
</html>
```
```java
@Controller
public class MyController {
   /**
     * 处理器方法的形参是 java 对象，这个对象的属性名和请求中的参数名是一样的。
     * 框架会创建形参的 java 对象，给属性赋值。请求中的参数是 name，框架会调用 setName()
     */
    @RequestMapping(value="/third.do")
    public ModelAndView doThird(Student student){
        System.out.println("name = " + student.getName() + ", age = " + student.getAge());
        ModelAndView mv = new ModelAndView();
        mv.addObject("myName", student.getName());
        mv.addObject("myAge", student.getAge());
        mv.setViewName("show");
        return mv;
    }
```
### 2.3 处理器方法的返回值
* 处理器方法的返回值表示请求的处理结果.
* 使用 @Controller 注解的处理器的处理器方法，其返回值常用的有四种类型：
（1）返回 ModelAndView
（2）返回 String
（3）返回 void（了解）
（4）返回对象 Object
#### 2.3.1 返回 ModelAndView
* 若处理器方法处理完后，需要跳转到其它资源，且又要在跳转的资源间传递数据，此时处理器方法返回 ModelAndView 比较好。当然，若要返回 ModelAndView，则在处理器方法中需要定义 ModelAndView 对象。
* 在使用时，若该处理器方法只是进行跳转而不传递数据，或者只是传递数据而并不向任何资源跳转（如对页面的 Ajax 异步响应），若此时返回 ModelAndView，则将总是有一部分多余，要么 Model 多余，要么 View 多余。即此时返回 ModelAndView 并不合适。 
* ModelAndView：视图和数据，对视图执行 forward。
#### 2.3.2 返回 String
* 处理器方法返回的字符串可以指定逻辑视图名，通过视图解析器解析可以将其转换为物理视图地址。
* **返回内部资源逻辑视图名** 
若要跳转的资源为内部资源，则视图解析器可以使用 InternalResourceViewResolver 内部资源视图解析器。此时处理器方法返回的字符串就是要跳转页面的文件名去掉文件扩展名后的部分。这个字符串与视图解析器中的 prefix、suffix 相结合，即可形成要访问的 URI。
* String：表示视图，可以是逻辑名称，也可以是完整视图路径。
* 示例：
index.jsp：
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="returnString.do" method="post">
        姓名：<input type="text" name="name" /> <br/>
        年龄：<input type="text" name="age" /> <br/>
        <input type="submit" value="提交参数" />
    </form>
</body>
</html>
```
MyController：
```java
@Controller
public class MyController {
   @RequestMapping(value="/returnString.do")
    public String doRturnView(HttpServletRequest request, String name, Integer age){
        System.out.println("name = " + name + ", age = " + age);

        // 可以自己手工添加数据到 request 工作域
        request.setAttribute("myName", name);
        request.setAttribute("myAge", age);

        /**
         * 处理器方法返回的 String 表示逻辑视图名称时，需要配置视图解析器。
         * show：逻辑视图名称，项目中配置了视图解析器
         * 框架对视图执行 forward 转发操作。
         */
        return "show";
        /**
         * 处理器方法返回的 String 表示完整视图路径时，不能配置视图解析器。
         * 框架对视图执行 forward 转发操作。
         */
        // return "/WEB-INF/view/show.jsp";
    }
```
show.jsp：
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h3>/WEB-INF/view/show.jsp 从 request 域中获取数据</h3>
    <h3>name中的数据：${myName}</h3>
    <h3>age中的数据：${myAge}</h3>
</body>
</html>
```
#### 2.3.3 返回 void（了解）
* 对于处理器方法返回 void 的应用场景：在处理 AJAX 的时候，可以使用 void。通过 HttpServletResponse 输出数据，响应 AJAX 请求。AJAX 请求在服务器端返回的只有数据，和视图无关。
* 若处理器对请求处理后，无需跳转到其它任何资源，此时可以让处理器方法返回 void。
* 返回 void 既不表示数据，也不表示视图。
#### 2.3.4 返回对象 Object
* 处理器方法也可以返回 Object 对象。这个 Object 可以是 Integer、String、自定义对象、Map、List 等。对象有属性，属性就是数据。所以**返回 Object 表示数据，和视图无关**。可以使用对象表示的数据，响应 ajax 请求。返回的对象不是作为逻辑视图出现的，而是作为直接在页面显示的数据出现的。
* 返回对象，需要使用 @ResponseBody 注解，将转换后的 JSON 数据放入到响应体中。
##### 响应 AJAX 请求，主要使用 json 的数据格式。实现步骤：
（1）加入处理 json 的工具库的依赖。springmvc 默认使用的是 jackson。
（2）在 springmvc 配置文件中加入 \<mvn:annotation-driven> （注解驱动）标签。
（3）在处理器方法的上面加入 @ResponseBody 注解。
##### springmvc 处理器方法返回 Object，可以转化为 json 输出到浏览器。响应 AJAX 的内部原理：
* **\<mvn:annotation-driven> （注解驱动）：注解驱动的功能是：完成 java 对象到 json、java 对象到 xml、java 对象到二进制等数据格式的转换**。
\<mvn:annotation-driven> 在加入到 springmvc 的配置文件中后，会自动创建 HttpMessageConverter 接口的 7 个实现类，包括 **MappingJackson2HttpMessageConverter（这个类负责读取和写入 json 格式的数据，使用 jackson 工具库中的 ObjectMapper 读写 json 数据，操作 Object 类型数据，可读取 application/json，响应媒体类型为 application/json）；StringHttpMessageConverter（这个类负责读取字符串格式的数据和写出字符串格式的数据）** 等。     
* **HttpMessageConverter 接口：消息转换器。功能：定义了 java 转为 json、xml 等数据格式的方法**。这个接口有很多实现类，这些实现类完成了 java 对象到 json、java 对象到 xml、java 对象到二进制数据的转换。下面两个方法是控制器类把结果输出给浏览器时使用的：
1、boolean canWrite(Class<?> var1, @Nullable MediaType var2)：检查处理器方法的返回值，能不能换为 var2 表示的数据格式。
2、write：把处理器方法的返回值对象，调用 jackson 中的 ObjectMapper 转为 json 字符串。
* **@ResponseBody 注解：放在处理器方法的上面，通过 HttpServletResponse 出输出数据，响应 ajax 请求。**
##### 返回自定义类型对象
（1）加入依赖。jackson 的依赖、引入 jQuery 库。
（2）在 springmvc 配置文件中加入 \<mvn:annotation-driven> （注解驱动）标签。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvn="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 声明组件扫描器 -->
    <context:component-scan base-package="com.atjingbo.controller" />

    <!-- 声明 springmvc 框架中的视图解析器，帮助开发人员设置视图文件的路径 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 前缀：视图文件的路径 -->
        <property name="prefix" value="/WEB-INF/view/" />
        <!-- 后缀：视图文件的扩展名 -->
        <property name="suffix" value=".jsp" />
    </bean>

    <!-- 添加注解驱动 -->
    <mvc:annotation-driven />
</beans>
```
（3）在处理器方法的上面加入 @ResponseBody 注解。
```java
@Controller
public class MyController {
    /**
     * 处理器方法返回一个 Student，通过框架转为 json，响应 ajax 请求。
     * @ResponseBody：
     *     作用；把处理器方法返回的对象转为 json 后，通过 HttpServletResponse 输出给浏览器。
     *     位置；在方法定义的上面，和其他注解没有顺序的先后关系。
     * 返回 Object，框架的处理过程：
     *     （1）框架会把返回值 Student，调用框架中的 ArrayList<HttpMessageConverter> 中的每个类的 canWrite() 方法
     *         来检查哪个 HttpMessageConverter 接口的实现类能处理 Student 类型的数据（MappingJackson2HttpMessageConverter）
     *     （2）框架会调用实现类 MappingJackson2HttpMessageConverter 的 write() 方法，把 Student 对象转为 json，
     *         调用 Jackson 的 ObjectMapper 实现转为 json。
     *         输出的 contentType：application/json;charset=utf-8。
     *     （3）框架会调用 @ResponseBody，把（2）的结果数据输出到浏览器，ajax 请求处理完成。
     */
    @RequestMapping(value="/returnStudentJson.do")
    @ResponseBody
    public Student doStudentJsonObject(String name, Integer age){
        // 调用 service，获取请求结果数据，Student 对象表示结果数据。
        Student student = new Student("李四", 20);
        return student; // 会被框架转为 json。
    }
}
```
ajax 请求页面：
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script type="text/javascript" src="js/jquery-1.12.4.js"></script>
    <script type="text/javascript">
        $(function (){
            $("button").click(function (){
                // alert("button click");
                $.ajax({
                    url:"returnStudentJson.do",
                    data:{
                        name:"zhangsan",
                        age:20
                    },
                    type:"post",
                    dataType:"json",
                    success:function (resp){
                        // resp 从服务器端返回的是 json 格式的字符串，jQuery 会把这个字符串转为 json 对象，赋值给 resp 形参。
                        alert(resp.name + ", " + resp.age);
                    }
                });
            })
        })
    </script>
</head>

<body>
    <button id="btn">发起 ajax 请求</button>
</body>
</html>
```
##### 返回 List 集合
```java
@Controller
public class MyController {
    /**
     * 处理器方法返回 List。
     * 返回 Object，框架的处理过程：
     *     （1）框架会把返回值 List<Student>，调用框架中的 ArrayList<HttpMessageConverter> 中每个类的 canWrite() 方法
     *         来检查哪个 HttpMessageConverter 接口的实现类能处理 Student 类型的数据（MappingJackson2HttpMessageConverter）
     *     （2）框架会调用实现类 MappingJackson2HttpMessageConverter 的 write() 方法，把 Student 对象转为 json，
     *         调用 Jackson 的 ObjectMapper 实现转为 json 的数组。
     *         输出的 contentType：application/json;charset=utf-8。
     *     （3）框架会调用 @ResponseBody，把（2）的结果数据输出到浏览器，ajax 请求处理完成。
     */
    @RequestMapping(value="/returnStudentJsonArray.do")
    @ResponseBody
    public List<Student> doStudentJsonObjectArray(String name, Integer age){
        // 调用 service，获取请求结果数据，Student 对象表示结果数据。
        Student student1 = new Student("李四", 20);
        Student student2 = new Student("张三", 18);
        List<Student> list = new ArrayList<>();
        list.add(student1);
        list.add(student2);
        return list; // 会被框架转为 json 的数组
    }
}
```
ajax 请求页面：
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script type="text/javascript" src="js/jquery-1.12.4.js"></script>
    <script type="text/javascript">
        $(function (){
            $("button").click(function (){
                $.ajax({
                    url:"returnStudentJsonArray.do",
                    data:{
                        name:"zhangsan",
                        age:20
                    },
                    type:"post",
                    dataType:"json",
                    success:function (resp){
                        $.each(resp, function (i, n){
                            alert(n.name + ", " + n.age);
                        })
                    }
                });
            })
        })
    </script>
</head>

<body>
    <button id="btn">发起 ajax 请求</button>
</body>
</html>
```
##### 返回字符串对象
```java
@Controller
public class MyController {
    /**
     * 处理器方法返回 String，String 表示数据，而不是视图。
     * 区分返回值 String 是数据还是视图，看有没有 @ResponseBody 注解；
     *     如果有 @ResponseBody 注解，返回值 String 就是数据，否则就是视图。
     *
     * 默认使用 "text/plain;charset=IOS-8859-1" 作为 contentType，导致中文有乱码。
     *  解决方案：给 @RequestMapping 增加啊一个属性 produces，使用这个属性指定新的 contentType。
     *
     * 返回 Object，框架的处理过程：
     *     （1）框架会把返回值 String，调用框架中的 ArrayList<HttpMessageConverter> 中每个类的 canWrite() 方法
     *         来检查哪个 HttpMessageConverter 接口的实现类能处理 String 类型的数据（StringHttpMessageConverter）
     *     （2）框架会调用实现类 StringHttpMessageConverter 的 write() 方法，把字符按照指定的编码来处理。
     *         text/plain;charset=IOS-8859-1
     *     （3）框架会调用 @ResponseBody，把（2）的结果数据输出到浏览器，ajax 请求处理完成。
     */
    @RequestMapping(value="/returnStringData.do", produces="text/plain;charset=utf-8")
    @ResponseBody
    public String doStringData(String name, Integer age){
        return "hello springmvc 返回 String，表示数据";
    }
}
```
ajax 请求页面：
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
    <script type="text/javascript" src="js/jquery-1.12.4.js"></script>
    <script type="text/javascript">
        $(function (){
            $("button").click(function (){
                $.ajax({
                    url:"returnStringData.do",
                    data:{
                        name:"zhangsan",
                        age:20
                    },
                    type:"post",
                    dataType:"text",
                    success:function (resp){
                        alert(resp);
                    }
                });
            })
        })
    </script>
</head>

<body>
    <button id="btn">发起 ajax 请求</button>
</body>
</html>
```
### 2.4 中央调度器的 url-pattern 设置为 "/"
* tomcat 本身能处理静态资源的访问。例如：html、图片、js 文件都是静态资源。
* tomcat 的 web.xml 文件有一个 servlet，名称是 default，在 tomcat 服务器启动时就被创建了。
* default 这个 servlet 的作用：
（1）处理静态资源。
（2）处理未映射到其他 servlet 的请求。

web.xml：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!-- 声明(注册) springmvc 的核心对象 DispatcherServlet。
        需要在 tomcat 服务器启动后，就创建 DispatcherServlet 对象的实例。
        为什么要创建 DispatcherServlet 对象的实例呢？
        因为 DispatcherServlet 在它的创建过程中，会同时创建 springmvc 容器对象，
        读取 springmvc 的配置文件，把这个配置文件中的对象都创建好，当用户发起请求时
        就可以直接使用对象了。

        servlet 的初始化会执行 init() 方法，DispatcherServlet 在 init() 方法中{
            // 创建容器，读取配置文件
            WebApplicationContext ctx = new ClassPathXmlApplicationContext("springmvc.xml");
            // 把容器对象放入 ServletContext 中
            getServletContext().setAttribute(key, ctx)
        }
    -->
    <servlet>
        <servlet-name>myweb</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <!-- 自定义 springmvc 读取的配置文件的位置。
            springmvc 创建容器对象时，读取的配置文件的路径是：/WEB-INF/<servlet-name>-servlet.xml
        -->
        <init-param>
            <!-- springmvc 的配置文件的位置的树形 -->
            <param-name>contextConfigLocation</param-name>
            <!-- 指定自定义文件的位置 -->
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>

        <!-- 在 tomcat 启动后，创建 Servlet 对象
            load-on-startup：表示 tomcat 启动后创建对象的顺序。它的值是大于等于 0 的整数。
                             数值越小，tomcat 创建对象的时间越早。
        -->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>myweb</servlet-name>
        <!--
             使用框架的时候，url-pattern 可以使用两种值：
             （1）使用扩展名的方式。语法：*.xxxx，xxxx是自定义的扩展名。常用的方式：*.do, *.action, *.mvc 等。
                 http://localhost:8080/myweb/some.do
             （2）使用斜杠 "/"。当你的项目中使用了"/"，它会替代 tomcat 中的 default。表示静态资源和未映射的请求
                 都由你的项目中的这个 servlet 处理。
                 也就是所有的静态资源都给 DispatcherServlet 处理。但默认情况下 DispatcherServlet 没有处理静态资源的能力，
                 没有控制器对象能处理静态资源的访问，所以静态资源（html、js、图片、css）都是 404。
                 动态资源 some.do 可以访问，因为我们的程序中有 MyController 控制器对象，能处理 some.do 的请求。

                第一种处理静态资源的方式：
                    在 springmvc 的配置文件中加入 <mvc:default-servlet-handler> 和 <mvc:annotation-driven>
                    原理：加入这个标签后，框架会创建控制器对象 DefaultServletHttpRequestHandler（类似我们自己创建的 MyController）
                        DefaultServletHttpRequestHandler 这个对象可以把接收的请求转发给 tomcat 的 default 这个 servlet。
                    注意：default-servlet-handler 和 @RequestMapping 注解有冲突，需要加入 annotation-driven 解决问题。
                第二种处理静态资源的方式：
                    在 springmvc 的配置文件中加入 <mvc:resources> 和 <mvc:annotation-driven>
                    mvc:resources 加入后框架会创建 ResourceHttpRequestHandler 这个处理器对象。
                    让这个对象处理静态资源的访问，不依赖 tomcat 服务器。
                    注意：mvc:resources 和 @RequestMapping 注解有冲突，需要加入 annotation-driven 解决问题。
        -->
        <url-pattern>/</url-pattern>
        <!-- <url-pattern>*.do</url-pattern> -->
    </servlet-mapping>


    <!-- 声明（注册）过滤器，解决 post 请求乱码的问题 -->
    <filter>
        <filter-name>characterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <!-- 设置项目中使用的字符编码 -->
        <init-param>
            <param-name>encoding</param-name>
            <param-value>utf-8</param-value>
        </init-param>
        <!-- 强制请求对象（HttpServletRequest）使用 encoding 编码的值-->
        <init-param>
            <param-name>forceRequestEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
        <!-- 强制应答对象（HttpServletResponse）使用 encoding 编码的值-->
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>characterEncodingFilter</filter-name>
        <!-- /*：表示强制所有的请求先通过过滤器处理 -->
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```
springmvc.xml：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvn="http://www.springframework.org/schema/mvc" xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 声明组件扫描器 -->
    <context:component-scan base-package="com.atjingbo.controller" />

    <!-- 声明 springmvc 框架中的视图解析器，帮助开发人员设置视图文件的路径 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 前缀：视图文件的路径 -->
        <property name="prefix" value="/WEB-INF/view/" />
        <!-- 后缀：视图文件的扩展名 -->
        <property name="suffix" value=".jsp" />
    </bean>

    <!-- 第一种处理静态资源的方式 -->
    <!-- 添加注解驱动 -->
    <mvc:annotation-driven />
    <mvc:default-servlet-handler />

    <!-- 第二种处理静态资源的方式
            mapping：访问静态资源的 uri 地址，使用通配符 **。
            location：静态资源在你的项目中的目录位置。
         static/** 表示：static/p1.jpg，static/user/logo.gif，static/order/his/list.png
     -->
    <mvc:resources mapping="/static/**" location="/static/" />
    <!-- 添加注解驱动 -->
    <mvc:annotation-driven />
</beans>
```
### 2.5 绝对路径和相对路径
* 前端不加斜杠 "/"，后端加斜杠 "/"。
* 在 jsp、html 中使用的地址，都是在前端页面中的地址，都是相对地址。
* 地址分类：
（1）绝对地址：带有协议名称的是绝对地址。比如，http://www.baidu.com。
（2）相对地址：没有协议开头的是相对地址。比如：user/some.do、/user/some.do。相对地址不能独立使用，必须有一个参考地址，通过**参考地址+相对地址**才能指定资源。
* 在你的页面中，访问地址不加 "/"：
>页面：http://localhost:8080/ch06_path/index.jsp。
>路径：http://localhost:8080/ch06_path/，资源：index.jsp。
>在 index.jsp 页面发起 user/some.do 请求，访问地址变为 http://localhost:8080/ch06_path/user/some.do。
>也就是说访问的地址没有斜杠开头时，例如：user/some.do，当你点击链接时，访问地址是当前页面的地址加上链接的地址：http://localhost:8080/ch06_path/ + user/some.do。

存在问题：如果在 index.jsp 发起 user/some.do 请求，访问后现在的地址是 http://localhost:8080/ch06_path/user/some.do。此时再发起一次 user/some.do 请求，地址就变为 http://localhost:8080/ch06_path/user/user/some.do，而该地址是错的。

解决方案：
（1）加入 \${pageContext.request.contextPath}。（也就是访问地址加 "/" 的情况）
（2）加入一个 base 标签，是 html 语言中的标签，表示当前页面中访问地址的基地址。你的页面中所有没有以 "/" 开头的地址，都是以 base 标签中的地址为参考地址。使用 base 中的地址 + user/some.do 组成访问地址。
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    String basePath = request.getScheme() + "://" + 
            request.getServerName() + ":" + request.getServerPort() + 
            request.getContextPath() + "/";
%>
<html>
<head>
    <title>Title</title>
    <base href="<%=basePath%>" />
</head>
<body>
    <a href="user/some.do">发起请求</a>
</body>
</html>
```
* 在你的页面中，访问地址加 "/"：
>页面：http://localhost:8080/ch06_path/index.jsp。
>路径：http://localhost:8080/ch06_path/，资源：index.jsp。
>在 index.jsp 页面发起 /user/some.do 请求，访问地址变为 http://localhost:8080/user/some.do。
>也就是说访问的地址有斜杠开头时，例如：/user/some.do，当你点击链接时，访问地址是你的服务器地址加上链接的地址：http://localhost:8080 + /user/some.do。
>如果你的资源不能访问，加入 EL 表达式：\${pageContext.request.contextPath}。
>示例：\<a href="${pageContext.request.contextPath}/user/some.do">发起请求\</a>
## 三、SSM 整合开发
* SSM 编程，即 SpringMVC + Spring + MyBatis 集合，是当前最为流行的 JavaEE 开发技术架构。其实 SSM 整合的实质，其实就是将 MyBatis 整合入 Spring，因为 SpringMVC 原本就是 Spring 的一部分，不用专门整合。
* SSM 分别的作用： 
（1）SpringMVC：界面层/视图层。负责接受请求，显示处理结果。
（2）Spring：业务逻辑层。管理 service、dao、工具类对象。
（3）MyBatis：持久层。访问数据库。
``用户发起请求 -- SpringMVC接收 -- Spring中的service对象 -- MyBatis处理数据``
* SSM 整合的实现方式可以分为两种：基于 XML 配置方式；基于注解方式。
### 3.1 SSM 整合开发
* SSM 整合也叫 SSI（IBatis 是 mybatis 的前身），整合中有容器。
（1）第一个容器是 SpringMVC 容器，用来管理 Controller 控制器对象的。
（2）第二个容器是 Spring 容器，用来管理 service、dao、工具类对象的。
* 我们要做的就是把使用的对象交给合适的容器来创建、管理。
（1）把 Controller 还有 web 开发的相关对象交给 SpringMVC 容器，这些 web 用的对象写在 SpringMVC 配置文件中。
（2）把 service、dao 对象定义在 spring 的配置文件中，让 spring 管理这些对象。
* **springmvc 容器是 spring 容器的子容器**，类似于 Java 中的继承，子可以访问父的内容。在子容器中的 Controller 可以访问父容器中的 service 对象，就可以实现 Controller 使用 service 对象。
### 3.2 实现步骤
1. 创建 mysql 表。
2. 新建 maven 项目。
3. 加入依赖。
包括：springmvc、spring、mybatis 三个框架的依赖、jackson 依赖、mysql 驱动、druid 连接池、jsp、servlet 依赖。
4. 写 web.xml。
（1）注册 DispatcherServlet，目的：创建 SpringMVC 容器对象，才能创建 Controller 类对象。创建的是 Servlet，可以接收用户的请求。
（2）注册 spring 的监听器 ContextLoaderListener，目的：创建 Spring 容器对象，才能创建 service、dao 等对象。
（3）注册字符集过滤器，解决 post 请求乱码的问题。
5. 创建包。Controller 包、service、dao、实体类包都要创建好。
6. 写 springmvc、spring、mybatis 的配置文件以及数据库的属性配置文件。
7. 写代码。dao 接口和 mapper 文件、service 和实现类、controller、实体类。
8. 写 jsp 页面。
### 3.3 具体实现
pom.xml 部分内容:
```xml
<dependencies>
    <!-- servlet 依赖-->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>4.0.1</version>
        <scope>provided</scope>
    </dependency>
    <!-- jsp 依赖 -->
    <dependency>
        <groupId>javax.servlet.jsp</groupId>
        <artifactId>jsp-api</artifactId>
        <version>2.2</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
        <scope>test</scope>
    </dependency>
    <!-- springmvc 依赖 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>5.2.12.RELEASE</version>
    </dependency>
    <!-- jackson 依赖 -->
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.12.3</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-core</artifactId>
        <version>2.12.3</version>
    </dependency>
    <!-- mybatis 依赖 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.7</version>
    </dependency>
    <!-- mybatis 和 spring 整合 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.6</version>
    </dependency>
    <!-- mysql 驱动 -->
        <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.25</version>
    </dependency>
    <!-- 德鲁伊连接池 -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.2.6</version>
    </dependency>
    <!-- 事务管理 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>5.2.16.RELEASE</version>
    </dependency>

    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.2.16.RELEASE</version>
    </dependency>
</dependencies>
<build>
    <resources>
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
Spring 配置文件 applicationContext：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- Spring 配置文件：才声明 service、dao、工具类等对象。 -->

    <context:property-placeholder location="classpath:conf/jdbc.properties" />

    <!-- 声明数据源，连接数据库 -->
    <bean id="myDataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}" />
        <property name="password" value="${jdbc.passwd}" />
    </bean>

    <!-- 声明 SqlSessionFactoryBean，创建 SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="myDataSource" />
        <property name="configLocation" value="classpath:conf/mybatis.xml" />
    </bean>

    <!-- 声明 mybatis 的扫描器，创建 dao 对象 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
        <property name="basePackage" value="com.atjingbo.dao" />
    </bean>

    <!-- 声明组件扫描器，指定 @Service 注解所在的包名 -->
    <context:component-scan base-package="com.atjingbo.service" />

    <!-- 事务配置：注解或者 aspectJ 的配置-->

</beans>
```
SpringMVC 配置文件 dispatcherServlet：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvn="http://www.springframework.org/schema/mvc" xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- SpringMVC 配置文件，用来声明 Controller 和其它 web 相关的对象 -->

    <!-- 声明组件扫描器 -->
    <context:component-scan base-package="com.atjingbo.controller" />

    <!-- 声明 springmvc 框架中的视图解析器，帮助开发人员设置视图文件的路径 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/" />
        <property name="suffix" value=".jsp" />
    </bean>

    <!-- (1)响应 ajax 请求，返回json。
         (2)解决静态资源访问问题。 -->
    <mvc:annotation-driven />
</beans>
```
mybatis 的主配置文件 mybatis.xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!-- mybatis 的主配置文件，主要定义了数据库的配置信息，以及 sql 映射文件的位置 -->

<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">


<configuration>
    <!--<settings>
        &lt;!&ndash; 设置 mybatis 输出日志到控制台 &ndash;&gt;
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>-->

    <mappers>
        <!--<mapper resource="com/atjingbo/dao/StudentDao.xml"/>-->
    </mappers>
</configuration>
```
## 四、SpringMVC 核心技术
### 4.1 请求转发和重定向
* 当处理器对请求处理完毕后，向其他资源进行跳转时，有两种跳转方式：请求转发和请求重定向。而根据所要跳转的资源类型，又可分为两类：跳转到页面和跳转到其它处理器。
* 注意，对于**请求转发**的页面，**可以是 WEB-INF 中的页面**；而**重定向**的页面，则**不能是 WEB-INF 中的页面**。因为重定向相当于用户再次发出一次请求，而用户是不能直接访问 WEB-INF 中的资源的。
![图示](https://img-blog.csdnimg.cn/cccbb555cca746d1a77dbff0b5691871.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_16,color_FFFFFF,t_70,g_se,x_16)
* SpringMVC 框架把原来 Servlet 中的请求转发和重定向操作进行了封装，现在可以使用简单的方式实现转发和重定向：
（1）forward：表示转发，实现 request.getRequestDispatcher("xxx.jsp").forward();
（2）redirect：表示重定向，实现 response.sendRedirect("xxx.jsp");
#### 4.1.1 请求转发
index 页面：
```html
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
  <title>JSP - Hello World</title>
</head>
<body>
<br/>
    <p>当处理器方法返回 ModelAndView，实现 forward 操作</p>
    <form action="some.do" method="post">
      姓名：<input type="text" name="name"><br/>
      年龄：<input type="text" name="age">
      <input type="submit" value="提交">
    </form>
</body>
</html>
```
MyController 类：
```java
@Controller
public class MyController {
    /*
    * 处理器方法返回 ModelAndView，实现转发 forward。
    *     语法：setViewName("forward:视图文件完整路径");
    *     forward 特点：不和视图解析器一起使用，就当项目中没有视图解析器。
    * */
    @RequestMapping(value="/some.do")
    public ModelAndView doSome(String name, int age){
        ModelAndView mv = new ModelAndView();
        mv.addObject("myName", name);
        mv.addObject("myAge", age);
        // 显示转发
        mv.setViewName("forward:/WEB-INF/view/show.jsp");
        return mv;
    }
}
```
show.jsp：
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    name：${myName}
    age：${myAge}
</body>
</html>
```
#### 4.1.2 请求重定向
index 页面：
```html
<%@ page contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<!DOCTYPE html>
<html>
<head>
  <title>JSP - Hello World</title>
</head>
<body>
<br/>
    <p>当处理器方法返回 ModelAndView，实现 redirect 操作</p>
    <form action="other.do" method="post">
      姓名：<input type="text" name="name"><br/>
      年龄：<input type="text" name="age">
      <input type="submit" value="提交">
    </form>
</body>
</html>
```
MyController 类：
```java
@Controller
public class MyController {
    /*
     * 处理器方法返回 ModelAndView，实现转发 redirect。
     *     语法：setViewName("redirect:视图文件完整路径");
     *     redirect 特点：不和视图解析器一起使用，就当项目中没有视图解析器。
     * 框架对重定向的操作：
     *     （1）框架会把 Model 中的简单类型的数据，转为 String 使用，作为 hello.jsp 的 get 请求参数使用。
     *         目的是在 other.do 和 hello.jsp 两次请求之间传递参数。
     *     （2）在目标 hello.jsp 页面可以使用参数集合对象 ${param} 获取请求参数值。
     *         例如：${param.myName}。
     * */
    @RequestMapping(value="/other.do")
    public ModelAndView doOther(String name, int age){
        ModelAndView mv = new ModelAndView();
        mv.addObject("myName", name);
        mv.addObject("myAge", age);

        // http://localhost:8080/springmvc1/hello.jsp?myName=张三&myAge=13
        mv.setViewName("redirect:/hello.jsp");

        // 重定向不能访问 WEB-INF 中的资源
        // mv.setViewName("redirect:/WEB-INF/view/show.jsp");
        return mv;
    }
}
```
hello.jsp：
```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    name：${param.myName}
    age：${param.myAge}
</body>
</html>
```
### 4.2 异常处理
* SpringMVC 框架采用的是统一、全局的异常处理。把 controller 中的所有异常都集中到一个地方。采用的是 AOP 的思想。把业务逻辑和异常处理代码分开，解耦合。
* 需要使用两个注解：
（1）@ExceptionHandler；
（2）@ControllerAdvice。
* **步骤：**
（1）创建一个普通类，作为全局异常处理类。
① 在类的上面加入 @ControllerAdvice 注解；
② 在类中定义的方法上面加入 @ExceptionHandler 注解。
（2）创建处理异常的视图页面。
（3）创建 springmvc 的配置文件。
① 组件扫描器，扫描 @Controller 注解。
② 组件扫描器，扫描 @ControllerAdvice 所在的包名。
③ 声明注解驱动。
#### 4.2.1 @ExceptionHandler 注解
* 使用注解 @ExceptionHandler 可以将一个方法指定为异常处理方法，该注解只有一个可选属性 value，为一个 class<?> 数组，用于指定该注解的方法所要处理的异常类，即所要匹配的异常。
* 而被注解的方法，其返回值可以是 ModelAndView、String 或者 void，方法名随意，方法参数可以是 Exception 及其子类对象、HttpServletRequest、HttpServletResponse 等。系统会自动为这些方法参数赋值。
* 对于异常处理注解的用法，也可以直接将异常处理方法注解于 Controller 之中。
#### 4.2.2 具体实例
MyController 类：
```java
@Controller
public class MyController {
    @RequestMapping(value="/first.do")
    public ModelAndView doFirst(String name, int age) throws IOException{
        ModelAndView mv = new ModelAndView();
        mv.addObject("myName", name);
        mv.addObject("myAge", age);
        if("zs".equals(name)){
            throw new IOException("名字不能是 zs！");
        }
        mv.setViewName("show");
        return mv;
    }
}
```
全局异常处理类 GlobalExceptionHandler：
```java
/*
* @ControllerAdvice：控制器增强（也就是说给控制器类增加功能：异常处理功能）
*     位置：在类的上面
*     特点：必须让框架知道这个注解所在的包名，需要在 springmvc 配置文件中声明
*         组件扫描器，指定 @ControllerAdvice 所在的包名。
* */
@ControllerAdvice
public class GlobalExceptionHandler {
    /*
    * 定义方法，处理发生的异常。
    * 该方法和控制器方法的定义一样，可以有多个参数，返回值可以是
    * ModelAndView、String、void、对象类型。
    *
    * 形参：Exception，表示 Controller 中抛出的异常对象。
    * 通过形参可以获取发生的异常信息。
    *
    * @ExceptionHandler(异常的 class)：表示异常的类型，当发生此类型异常时，
    * 由当前方法处理。
    * */
    @ExceptionHandler(value = IOException.class)
    public ModelAndView doIOException(Exception e){
        /*
        * 异常发生时处理的逻辑：
        * （1）需要把异常记录下来，记录到数据库、日志文件。
        * 记录异常发生的时间、哪个方法发生的、异常错误的内容。
        * （2）发送通知，把异常的信息通过邮件、短信、微信发送给相关人员。
        * （3）给用户友好的提示。
        * */
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg", "姓名必须是 zs。");
        mv.addObject("exception", e);
        mv.setViewName("IOError");
        return mv;
    }

    /*
    * 处理其它异常，IOException 以外的不知类型的异常。
    * */
    @ExceptionHandler
    public ModelAndView doOtherException(Exception e){
        ModelAndView mv = new ModelAndView();
        mv.addObject("msg", "其它异常");
        mv.addObject("exception", e);
        mv.setViewName("defaultError");
        return mv;
    }
}
```
springmvc 配置文件：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvn="http://www.springframework.org/schema/mvc" xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 声明组件扫描器，指定 @Controller 注解所在的包名 -->
    <context:component-scan base-package="com.atjingbo.controller" />
    
    <!-- 声明组件扫描器，指定 @ControllerAdvice 注解所在的包名 -->
    <context:component-scan base-package="com.atjingbo.handler" />

    <!-- 声明 springmvc 框架中的视图解析器，帮助开发人员设置视图文件的路径 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 前缀：视图文件的路径 -->
        <property name="prefix" value="/WEB-INF/view/" />
        <!-- 后缀：视图文件的扩展名 -->
        <property name="suffix" value=".jsp" />
    </bean>

    <!-- 添加注解驱动 -->
    <mvc:annotation-driven />
</beans>
```
### 4.3 拦截器
* 拦截器是 springmvc 中的一种对象，需要实现 HandlerInterceptor 接口。
* 拦截器和过滤器类似，功能方向的侧重点不同。
（1）过滤器是用来过滤请求参数、设置编码字符集等工作的。
（2）拦截器是用来拦截用户的请求，并对请求做判断处理的。
* 拦截器是全局的，可以对多个 Controller 做拦截。
* 一个项目中可以有 0 个或多个拦截器，它们在一起拦截用户的请求。
* 拦截器常用在：用户登录处理、权限检查、记录日志。
* 可以将多个 Controller 中公共的功能，集中到拦截器中统一处理。使用的是 aop 的思想。
* **拦截器的使用步骤：**
（1）定义类实现 HandlerInterceptor 接口，并实现接口中的三个方法。
（2）在 springmvc 配置文件中，声明拦截器，让框架知道拦截器的存在。
① 声明组件扫描器，扫描 @Controller 注解。
② 声明拦截器，并指定拦截的请求的 uri 地址。
* **拦截器的执行时间：**
（1）在请求处理之前，也就是 controller 类中的方法执行之前先被拦截。
（2）在控制器方法执行之后也会执行拦截器。
（3）在请求处理完成后也会执行拦截器。
#### 4.3.1 具体实例
MyController 类：
```java
@Controller
public class MyController {
    @RequestMapping(value="/second.do")
    public ModelAndView doSecond(String name, int age) throws IOException{
        System.out.println("doSecond() 方法执行");
        ModelAndView mv = new ModelAndView();
        mv.addObject("myName", name);
        mv.addObject("myAge", age);
        mv.setViewName("show");
        return mv;
    }
}
```
MyInterceptor 类：
```java
// 拦截器类：拦截用户的请求
public class MyInterceptor implements HandlerInterceptor{

    /**
     * preHandle：预处理方法。
     * 参数：
     *     Object handler：被拦截的控制器对象（本例中是 MyController）
     * 返回值 boolean：
     *     true：表示请求通过了拦截器的验证，可以执行控制器方法。
     *          方法执行顺序：preHandle()、doSecond()控制器方法、postHandle()、afterCompletion()
     *     false：表示请求没有通过拦截器的验证，请求到此方法（preHandle）就截止了，请求没有被处理。
     *           方法执行顺序：preHandle()
     * 特点：
     *     （1）预处理方法在控制器方法之前先执行，用户的请求先到达此方法。
     *     （2）在预处理方法中可以获取请求的参数，验证请求是否符合要求，
     *         可以验证用户是否登陆，验证用户是否有权限访问某个连接地址（url）
     *     如果验证成功，可以放行请求，此时控制器方法才能执行。
     *     如果验证失败，可以截断请求，请求不能被处理。
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("MyInterceptor 的 preHandle 方法执行");
        // 方法里有计算的业务逻辑，根据计算结果，返回 true 或 false
        return true;
    }

    /**
     * postHandle：后处理方法。
     * 参数：
     *     Object handler：被拦截的控制器对象（本例中是 MyController）
     *     ModelAndView mv：控制器方法的返回值。
     * 特点：
     *     （1）后处理方法在控制器方法之后执行。
     *     （2）能够获取到控制器方法的返回值 ModelAndView，可以修改
     *         ModelAndView 中的数据和视图，可以影响到最后的执行结果。
     *     （3）主要是对原来的执行结果做二次修正。
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView mv) throws Exception {
        System.out.println("MyInterceptor 的 postHandle 方法执行");

        if(mv != null){
            mv.addObject("myDate", new Date());
            mv.setViewName("/other");
        }
    }

    /**
     * afterCompletion：最后执行的方法。
     * 参数：
     *     Object handler：被拦截的控制器对象（本例中是 MyController）
     *     Exception ex：程序中发生的异常。
     * 特点：
     *     （1）在请求处理完成之后执行。框架中的规定是：当你的视图处理完成之后，也就是对视图执行了 forward，就认为请求处理完成。
     *     （2）一般用来做资源回收工作。程序请求过程中创建的一些对象，可以在这里删除，把占用的内存回收。
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("MyInterceptor 的 afterCompletion 方法执行");
    }
}
```
springmvc 配置文件：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvn="http://www.springframework.org/schema/mvc" xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 声明组件扫描器，指定 @Controller 注解所在的包名 -->
    <context:component-scan base-package="com.atjingbo.controller" />

    <!-- 声明 springmvc 框架中的视图解析器，帮助开发人员设置视图文件的路径 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 前缀：视图文件的路径 -->
        <property name="prefix" value="/WEB-INF/view/" />
        <!-- 后缀：视图文件的扩展名 -->
        <property name="suffix" value=".jsp" />
    </bean>

    <!-- 声明拦截器：拦截器可以有0个或多个 -->
    <mvn:interceptors>
        <!-- 声明第一个拦截器 -->
        <mvn:interceptor>
            <!-- 指定拦截的请求的 uri 地址
                 path：就是 uri 地址，可以使用通配符 **
                 **：表示任意的字符、文件或者多级目录和目录中的文件
            -->
            <mvn:mapping path="/**"/>
            <!-- 声明拦截器对象 -->
            <bean class="com.atjingbo.handler.MyInterceptor" />
        </mvn:interceptor>
    </mvn:interceptors>
</beans>
```
拦截器中方法与处理器方法的执行顺序：
![图示](https://img-blog.csdnimg.cn/717b3b9cfdf5409c86f3e15a3d9ef0bd.png)
#### 4.3.2 多个拦截器
* 当有多个拦截器时，形成拦截器链。多个拦截器中方法与控制器方法的执行顺序：
![图示](https://img-blog.csdnimg.cn/cedb105d1edb410e8178ba7f86457526.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)

springmvc 配置文件：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvn="http://www.springframework.org/schema/mvc" xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/mvc https://www.springframework.org/schema/mvc/spring-mvc.xsd">

    <!-- 声明组件扫描器，指定 @Controller 注解所在的包名 -->
    <context:component-scan base-package="com.atjingbo.controller" />

    <!-- 声明 springmvc 框架中的视图解析器，帮助开发人员设置视图文件的路径 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 前缀：视图文件的路径 -->
        <property name="prefix" value="/WEB-INF/view/" />
        <!-- 后缀：视图文件的扩展名 -->
        <property name="suffix" value=".jsp" />
    </bean>

    <!-- 声明拦截器：拦截器可以有0个或多个。
         多个拦截器：先声明的先执行，后声明的后执行。
         在框架中多个拦截器保存在 ArrayList 中，按照声明的先后顺序放入到 ArrayList。
    -->
    <mvn:interceptors>
        <!-- 声明第一个拦截器 -->
        <mvn:interceptor>
            <!-- 指定拦截的请求的 uri 地址
                 path：就是 uri 地址，可以使用通配符 **
                 **：表示任意的字符、文件或者多级目录和目录中的文件
            -->
            <mvn:mapping path="/**"/>
            <!-- 声明拦截器对象 -->
            <bean class="com.atjingbo.handler.MyInterceptor1" />
        </mvn:interceptor>

        <!-- 声明第二个拦截器 -->
        <mvn:interceptor>
            <mvn:mapping path="/**"/>
            <bean class="com.atjingbo.handler.MyInterceptor2" />
        </mvn:interceptor>
    </mvn:interceptors>
</beans>
```
* 第一个拦截器的 preHandle() 返回 true、第二个拦截器的 preHandle() 返回 true 的执行顺序：
（1）第 1 个拦截器的 preHandle() 方法执行；
（2）第 2 个拦截器的 preHandle() 方法执行；
（3）doSecond() 控制器方法执行；
（4）第 2 个拦截器的 postHandle() 方法执行；
（5）第 1 个拦截器的 postHandle() 方法执行；
（6）第 2 个拦截器的 afterCompletion() 方法执行；
（7）第 1 个拦截器的 afterCompletion() 方法执行。
* 第一个拦截器的 preHandle() 返回 true、第二个拦截器的 preHandle() 返回 false 的执行顺序：
（1）第 1 个拦截器的 preHandle() 方法执行；
（2）第 2 个拦截器的 preHandle() 方法执行；
（3）第 1 个拦截器的 afterCompletion() 方法执行。
* 第一个拦截器的 preHandle() 返回 false、第二个拦截器的 preHandle() 返回 true/false  的执行顺序：
第 1 个拦截器的 preHandle() 方法执行。
#### 4.3.2 过滤器和拦截器的区别
（1）过滤器是 servlet 中的对象；拦截器是框架中的对象。
（2）过滤器是实现了 Filter 接口的对象；而拦截器是实现了 HandlerInterceptor 接口的对象。
（3）拦截器是用来设置 request、response 的参数、属性的，侧重对数据的过滤；而拦截器是用来验证请求的，能截断请求。
（4）过滤器是在拦截器之前先执行的。
（5）过滤器是 tomcat 服务器创建的对象；而拦截器是 springmvc 容器中创建的对象。
（6）过滤器是一个执行时间点；而拦截器有三个执行时间点。
（7）过滤器可以处理 jsp、js、html 等；拦截器是侧重于拦截对 Controller 的请求。如果你的请求不能被 DispatcherServlet 接收，则这个请求不会执行拦截器的内容。
## 五、SpringMVC 执行流程（理解）
* 流程图：
![图示](https://img-blog.csdnimg.cn/c4473193adc749d5b8136a259469f85f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_17,color_FFFFFF,t_70,g_se,x_16)
#### springmvc 内部请求的处理过程（即 springmvc 从接受请求到处理完成的过程）：
![图示](https://img-blog.csdnimg.cn/12d5cdbd567747388517aad97eb17921.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
1. 用户发起请求 some.do 到中央调度器 DispatcherServlet。
2. DispatcherServlet 接收请求 some.do，并把请求转交给处理器映射器。
（1）处理器映射器：springmvc 框架中的一种对象，框架把实现了 HandlerMapping 接口的类都叫做映射器（多个）
（2）处理器**映射器**作用：根据请求，从 springmvc 容器对象中**获取控制器对象**，框架把找到的处理器对象放到一个叫做处理器执行链（HandlerExecutionChain）的类中保存。
（3）**HandlerExecutionChain 类中保存着 1、处理器对象（MyController）2、项目中所有的拦截器（List\<HandlerInterceptor>）**
3. DispatcherServlet 把步骤（2）中的处理器执行链中的处理器对象交给处理器适配器对象（多个）
（1）处理器适配器：springmvc 框架中的对象，需要实现 HandlerAdapter 接口。
（2）处理器**适配器**作用：**执行处理器方法**（调用 MyController.doSome() 方法，得到返回值 ModelAndView）
4. DispatcherServlet 把步骤（3）中获取的 ModelAndView 交给视图解析器对象。
（1）视图解析器：springmvc 中的对象，需要实现 ViewResoler 接口（可以有多个）
（2）视图解析器作用：使用前缀、后缀组成视图完整路径，并创建 View 对象。
（3）View 是一个接口，用来表示视图的，在框架中 jsp、html 不是 String 表示，而是使用 View 和它的实现类来表示。
5. DispatcherServlet 把步骤（4）中创建的 View 对象获取到，调用 View 类自己的方法，把 Model 数据放入到 request 作用域，执行对视图的 forward，请求结束。