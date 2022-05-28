

## 1. JavaWeb的概念
### 什么是javaWeb
JavaWeb是指，所有通过 Java 语言编写的，可以通过浏览器访问的程序的总称。
JavaWeb是基于请求和响应来开发的。
### 什么是请求
请求是指客户端给服务器发送数据，叫做请求Request。
### 什么是响应
响应是指服务器给客户端回传数据，叫响应Response。
### 请求和响应的关系
请求和响应是成对出现的。有请求就有响应。
![图示](https://img-blog.csdnimg.cn/20210512201912614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
## 2. Web资源的分类
web 资源按实现的技术和呈现的效果的不同，又分为静态资源和动态资源两种。

静态资源：html、css、js、txt、mp4视频、jpg图片
动态资源：jsp页面、Servlet程序
![图示](https://img-blog.csdnimg.cn/20210512202956508.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
## 4. Tomcat 服务器 和 Servlet版本的对应关系
![图示](https://img-blog.csdnimg.cn/20210512203440656.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 如何启动Tomcat服务器
找到 Tomcat 目录下的 bin 目录下的 startup.bat文件，双击就可以启动 Tomcat 服务器。
### 如何测试 Tomcat 服务器启动成功了？
打开浏览器，在浏览器地址栏输入以下地址测试：
1、http://localhost:8080
2、http://127.0.0.1:8080
3、http://真实ip:8080
### 常见的启动失败原因
双击 startup.bat 文件，就会出现一个小黑窗口一闪而过。这个时候，失败的原因基本上都是因为没有配置好 JAVA_HOME 环境变量。
### 启动 Tomcat 服务器的另一种方式
1、打开命令行
2、cd 到 你的 Tomcat 的 bin 目录下
3、敲入启动命令：catalina run
### Tomcat 的停止
1、点击 Tomcat 服务器窗口的关闭按钮
2、把 Tomcat 服务器窗口置为当前窗口，然后按快捷键 Ctrl + C
**3、找到 Tomcat 目录下的 bin 目录下的 shutdowm.bat 双击，就可以停止 Tomcat 服务器**
### 如何修改 Tomcat 的端口号
Mysql 默认的端口号是：3306
Tomcat 默认的端口号是：8080
找到 Tomcat 目录下的 conf 目录，找到 server.xml 配置文件
![图示](https://img-blog.csdnimg.cn/20210513202408515.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
HTTP 协议默认的端口号是：80
比如：http://www.baidu.com:80。就是不写端口号，默认的也是80
### 如何部署 web 工程到 Tomcat 中
**第一种方式：只需要把 web 工程的目录拷贝到 Tomcat 的webapps 目录下即可。**

例如：在 webapps 目录下新建一个 book工程

如何访问：http://loccalhost:8080/book/temp.html（注：当输入：http://loccalhost:8080 的时候，表示访问到 webapps 里面来了）

**第二种方式：找到 Tomcat 下的conf\Catalina\localhost\下，创建如下配置文件（例如创建 abc.xml 配置文件）**
![图示](https://img-blog.csdnimg.cn/20210513205720290.png)
启动的时候，工程的访问路径是由 abc.xml 中配置的 path 所决定的。

如何访问：http://loccalhost:8080/abc/temp.html。这里面的 abc 就表示要访问 磁盘中 E:\book 这个目录，它就映射过去了。
### 手拖 HTML 页面到浏览器和在浏览器中输入 http://ip:端口号/工程名/访问 的区别
![图示](https://img-blog.csdnimg.cn/20210514193056677.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### ROOT 的工程的访问，以及 默认 index.html 页面的访问
当我们在浏览器地址栏中输入以下访问地址时：
http://ip:port/ ---> 没有工程名的时候，默认访问的是 ROOT 工程
http://ip:port/ --->工程名 没有资源名的时候，默认访问的是 index .html 页面
### IDEA 整合 Tomcat 服务器
在 IDEA 中按下面的找：File | Settings | Build, Execution, Deployment | Application Servers

### IDEA中动态 Web 工程的操作
#### 1、IDEA中如何动态创建 Web 工程
1、创建一个新模块

2、选择你要创建什么类型的模块
![图示](https://img-blog.csdnimg.cn/20210515211544482.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
#### 2、Web 工程的目录介绍
注：通常在 WEB-INF 目录下，再新建一个 lib 目录（directory），用来存放 jar 包
![图示](https://img-blog.csdnimg.cn/20210515214436507.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

#### 如何在 IDEA 中部署工程到 Tomcat 上运行
1、建议修改 Web 工程对应的 Tomcat 运行实例名称。
![图示](https://img-blog.csdnimg.cn/20210515215356829.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
注：建议和当前的工程名一致
![图示](https://img-blog.csdnimg.cn/20210515215446760.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
2、确认你的 Tomcat 实例中有你要部署的运行的 web 工程模块
![图示](https://img-blog.csdnimg.cn/20210517210322370.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
3、你还可以修改你的 Tomcat 实例启动后默认的访问地址
![图示](https://img-blog.csdnimg.cn/20210517210454688.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

### 修改工程的访问路径
![图示](https://img-blog.csdnimg.cn/20210515222001501.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 修改运行的端口号
![图示](https://img-blog.csdnimg.cn/2021051522235345.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 配置资源热部署
![图示](https://img-blog.csdnimg.cn/2021051522272236.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)