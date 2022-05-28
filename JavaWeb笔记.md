

#### JavaEE三层架构
![图示](https://img-blog.csdnimg.cn/20210524215255479.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
分层的目的是为了解耦。解耦就是为了降低代码的耦合度，方便项目后期的维护和升级。
![图示](https://img-blog.csdnimg.cn/2021052422032636.png)
#### 编码编写的流程
1. 先创建书城需要的数据库和表
2. 编写数据库表对应的 JavaBean 对象
3. 编写工具类 JdbcUtils
（1）导入需要的 jar 包（数据库和连接池需要）
（2）在 src 源码目录下编写 jdbc.properties 配置文件。
（3）编写 JdbcUtils 工具类
（4）JdbcUtils 测试
4. 编写 BaseDao
5. 编写 UserDao 和测试
6. 编写 UserService 和测试

...等等步骤

#### 文件的上传
1. 要有一个 form 标签，method=post 请求
2. form 标签的 enctype 的属性值必须是 multipart/form-data 值
3. 在 form 标签中使用 input type=file 添加上传的文件
4. 编写服务器代码接收、处理上传的数据
![图示](https://img-blog.csdnimg.cn/20210530100348985.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
#### commons-fileupload.jar 常用 API 介绍
commons-fileupload.jar 需要依赖 commons-io.jar 这个包，因此我们两个包都要导入。
![图示](https://img-blog.csdnimg.cn/20210530110903274.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
**示例：**
```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <form action="http://localhost:8080/JSP/uploadTest" enctype="multipart/form-data" method="post">
        用户名：<input type="text" name="username" /><br>
        头像：<input type="file" name="photo" /><br>
        <input type="submit" value="上传"/>
    </form>
</body>
</html>
```
```java
import org.apache.commons.fileupload.FileItem;
import org.apache.commons.fileupload.FileItemFactory;
import org.apache.commons.fileupload.FileUploadException;
import org.apache.commons.fileupload.disk.DiskFileItemFactory;
import org.apache.commons.fileupload.servlet.ServletFileUpload;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.File;
import java.io.IOException;
import java.util.List;

public class UploadTest extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 先判断上传的数据是否是多段数据（只有是多段的数据，才是文件上传的）
        if(ServletFileUpload.isMultipartContent(request)){
            // 创建 FileItemFactory 接口的实现列
            FileItemFactory fileItemFactory = new DiskFileItemFactory();
            // 创建用于解析上传的数据的工具类 ServletFileUpload
            ServletFileUpload servletFileUpload = new ServletFileUpload(fileItemFactory);
            try {
                // 解析上传的数据，得到每一个表单项 FileItem
                List<FileItem> fileItems = servletFileUpload.parseRequest(request);
                // 循环判断每一个表单项，是普通类型，还是上传的文件
                for (FileItem fileItem : fileItems) {
                    if(fileItem.isFormField()){// 普通表单项（比如本例中的用户名里的内容）
                        System.out.println("表单项的 name 属性值：" + fileItem.getFieldName());
                        // 参数 UTF-8，解决乱码问题
                        System.out.println("表单项的 value 属性值：" + fileItem.getString("UTF-8"));
                    }else{// 上传的文件（比如本例中上传的图片）
                        System.out.println("表单项的 name 属性值：" + fileItem.getFieldName());
                        System.out.println("上传的文件名：" + fileItem.getName());
                        fileItem.write(new File("e:\\" + fileItem.getName()));
                    }
                }
            } catch (FileUploadException e) {
                e.printStackTrace();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```
#### 文件的下载
![图示](https://img-blog.csdnimg.cn/20210530152216199.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
**示例：**

```java
import org.apache.commons.io.IOUtils;

import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;
import java.io.InputStream;
import java.net.URLEncoder;

public class Download extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 1、获取要下载的文件名
        String downloadFileName = "a.png";

        // 2、读取要下载的文件内容（通过 ServletContext 对象可以读取）
        ServletContext servletContext = getServletContext();
        // 获取要下载的文件类型
        String mimeType = servletContext.getMimeType("/file/" + downloadFileName);

        // 4、在回传前，通过响应头告诉客户端返回的数据类型
        response.setContentType(mimeType);

        // 5、在回传前，还要通过响应头告诉客户端收到的数据是用于下载用的
        // Content-Disposition 响应头，表示收到的数据怎么处理
        // attachment 表示附件，表示下载使用；filename= 表示指定下载的文件名
        // response.setHeader("Content-Disposition", "attachment;filename=" + downloadFileName);

        // 下载的文件名可以和原来的文件名不同。含有中文会出现乱码。需要对文件名进行 URL 编码。URL 编码是把汉字转换成为 %xx%xx 的格式
        response.setHeader("Content-Disposition", "attachment;filename=" + URLEncoder.encode("中国.png", "UTF-8"));

        // 3、把下载的文件内容回传给客户端
        // 在服务器端，/ 被服务器解析表示地址 http://ip:port/工程名/，映射到代码的 web 目录
        InputStream resourceAsStream = servletContext.getResourceAsStream("/file/" + downloadFileName);//src/main/webapp/file
        // 通过响应的输出流
        ServletOutputStream outputStream = response.getOutputStream();
        // 把输入流里的内容复制给输出流，输出给客户端
        IOUtils.copy(resourceAsStream, outputStream);
    }
}
```
### MVC概念
![图示](https://img-blog.csdnimg.cn/20210616092958282.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
![图示](https://img-blog.csdnimg.cn/20210616093004805.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### Junit 的使用
* junit：单元测试，一个工具类库，是用来测试方法的。
* 单元：其实就是方法，一个类中有很多方法，一个方法称为单元。
* 其实也可以用 main 方法等方式去测试，但可能不方便。
* 使用单元测试的步骤：
 （1）需要加入 junit 依赖。
  （2）创建测试类，一般在 src/test/java 目录下创建类。
  （3）创建测试方法。
  1）public 方法。
  2）没有返回值。void。
  3）方法名称自定义，建议名称是 test + 你要测试的方法的名称。
  4）方法没有参数。
  5）方法的上面加入 @Test，这样的方法是可以单独执行的，不用使用 main 方法。