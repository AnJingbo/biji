

* JDBC：Java DataBase Connectivity（Java语言连接数据库）
* JDBC的本质：JDBC是SUN公司制定的一套接口(interface)。
* 为什么要制定一套JDBC接口呢？
        因为每一个数据库的底层实现原理都不一样
* 接口都有调用者和实现者
* 多态机制是非常典型的：面向抽象编程。（不要面向具体编程）

![图示](https://img-blog.csdnimg.cn/20210320164309516.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
* **JDBC编程六步**：
1. **注册驱动**（告诉Java程序，即将要连接的是哪个数据库，比如MySql、Oracle等）
2. **获取连接**（表示JVM的进程和数据库进程之间的通道打开了，这属于进程之间的通信，使用完后一定关闭通道）。
3. **获取数据库操作对象**（专门执行sql语句的对象）
4. **执行SQL语句**（DQL,DML…）
5. **处理查询结果集** （只有当第四步执行的是select语句的时候，才有本步）
6. **释放资源**（使用完资源后一定要关闭资源，Java和数据库之间属于进程间的通信，开启之后一定要记得关闭）

详解：
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBCTest01 {
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try {
            //1、注册驱动
            java.sql.Driver driver = new com.mysql.cj.jdbc.Driver();
            DriverManager.registerDriver(driver);

            //2、获取连接
            /*url：统一资源定位符(网络中某个资源的绝对路径) url包含：协议、IP、PORT(端口)、资源名
            localhost 和 127.0.0.1 都是本机IP地址*/
            String url = "jdbc:mysql://localhost:3306/MySql?useUnicode=true&characterEncoding=UTF8&serverTimezone=GMT";
            String name = "root";
            String password = "135821";
            conn = DriverManager.getConnection(url, name, password);
            System.out.println(conn);

            //3、获取数据库操作对象
            stmt = conn.createStatement();

            //4、执行sql
            String sql = "insert into dept(deptno, dname, loc) values(50, '人事部', '北京')";
            //专门执行DML语句(insert、delete、update)
            //返回值是"影响数据库中的记录条数"
            int count = stmt.executeUpdate(sql);
            System.out.println("count = " + count);
            
            //5、处理查询结果集

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            //6、释放资源
            /*为了保证资源一定释放，在finally语句块中关闭资源
            并且要遵循从小到大依次关闭*/
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
    }
}
```
简略版：
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBCTest02 {
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        try {
            //1、注册驱动
            DriverManager.registerDriver(new com.mysql.cj.jdbc.Driver());

            //2、获取连接
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/bjpowernode?useUnicode=true&characterEncoding=UTF8&serverTimezone=GMT", "root", "135821");

            //3、获取数据库操作对象
            stmt = conn.createStatement();

            //4、执行sql
            /*String sql = "update dept set dname = '销售部', loc = '天津' where deptno = 20";
            String sql = "delete from dept where deptno = 40";*/
            String sql = "insert into dept(deptno, dname, loc) values(50, '人事部', '北京')";
            int count = stmt.executeUpdate(sql);//专门执行DML语句(insert、delete、update)，返回值是"影响数据库中的记录条数"
            System.out.println("count = " + count);
            //5、处理查询结果集

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            //6、释放资源
            /*为了保证资源一定释放，在finally语句块中关闭资源
            并且要遵循从小到大依次关闭*/
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
    }
}
```
**将连接数据库的所有信息配置到属性配置文件中 (常用)**
![图示](https://img-blog.csdnimg.cn/20210327102000718.png)
增删改语句:
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ResourceBundle;

public class JDBCTest04 {
    public static void main(String[] args) {
        //使用资源绑定器绑定属性配置文件
        ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        Connection conn = null;
        Statement stmt = null;
        try {
            //1、注册驱动
            Class.forName(driver);//用类加载机制，执行静态代码块，在静态代码块中完成了驱动的注册

            //2、获取连接
            conn = DriverManager.getConnection(url, user, password);

            //3、获取数据库操作对象
            stmt = conn.createStatement();

            //4、执行sql
            /*String sql = "update dept set dname = '销售部', loc = '天津' where deptno = 20";
            String sql = "delete from dept where deptno = 40";*/
            String sql = "insert into dept(deptno, dname, loc) values(50, '人事部', '北京')";
            int count = stmt.executeUpdate(sql);//专门执行DML语句(insert、delete、update)，返回值是"影响数据库中的记录条数"
            System.out.println("count = " + count);
            //5、处理查询结果集

        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            //6、释放资源
            /*为了保证资源一定释放，在finally语句块中关闭资源
            并且要遵循从小到大依次关闭*/
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
    }
}
```
**处理查询结果集：**
```java
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.Connection;
import java.util.ResourceBundle;

public class JDBCTest05 {
    public static void main(String[] args) {
        ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            //1、注册驱动
            Class.forName(driver);

            //2、获取链接
            conn = DriverManager.getConnection(url, user, password);

            //3、获取数据库操作对象
            stmt = conn.createStatement();

            //4、执行sql
            String sql = "select empno as a, ename, sal from emp";
            /*int executeUpdate(insert/delete/update)
            ResultSet executeQuery(select)*/
            rs = stmt.executeQuery(sql);//专门执行DQL语句的方法

            //5、处理查询结果集
            while(rs.next()){//next()方法：光标向前移动一位，若当前行有数据，返回true，否则返回false。最初光标指向所有数据的前一行。
                /*以列的下标获取。JDBC中的所有下标是从1开始的
                int empno = rs.getInt(1);
                String ename = rs.getString(2);
                double sal = rs.getDouble(3);*/
                int empno = rs.getInt("a");//注意：以列的名字获取。列名称不是表中的列名称，而是查询结果集里的列名称
                String ename = rs.getString("ename");
                double sal = rs.getDouble("sal");
                System.out.println(empno + ",  " + ename + ", " + sal);
            }
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } finally{
            //6、释放资源。一定要从小到大依次关闭
            if(rs != null){
                try {
                    rs.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if(stmt != null){
                try {
                    stmt.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
    }
}
```
用户登录功能：

```java
import java.sql.*;
import java.util.HashMap;
import java.util.Map;
import java.util.ResourceBundle;
import java.util.Scanner;

/*
  1、模拟用户登录功能的实现
  2、存在问题：
        用户名：fdas
        密码：fdsa' or '1'='1
        登陆成功
    这种现象被称为sql注入。
    以上输入的sql语句是：String sql = "select * from t_user where loginName = 'fdsa' and loginPwd = 'fdsa' or '1' = '1'"
    导致sql注入的根本原因是：
        用户输入的信息中含有sql语句的关键字，并且这些关键字参与了sql语句的编译过程，导致了sql语句的原意被扭曲，进而达到sql注入
*/
public class JDBCTest06 {
    public static void main(String[] args) {
        //初始化界面
        Map<String, String> userLoginInfo = initUI();
        //验证用户名和密码
        boolean loginSuccess = login(userLoginInfo);
        //输出结果
        System.out.println(loginSuccess ? "登陆成功" : "登陆失败");
    }

    /**
     * @param userLoginInfo 用户登录信息
     * @return false表示登陆失败，true表示登陆成功
     */
    private static boolean login(Map<String, String> userLoginInfo) {
        //打标记
        boolean loginSuccess = false;
        //单独定义变量
        String loginName = userLoginInfo.get("loginName");
        String loginPwd = userLoginInfo.get("loginPwd");
        //JDBC代码
        ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            //1、注册驱动
            Class.forName(driver);
            //2、获取连接
            conn = DriverManager.getConnection(url, user, password);
            //3、获取数据库操作对象
            stmt = conn.createStatement();
            //4、执行sql语句
            String sql = "select * from t_user where loginName = '" + loginName + "' and loginPwd = '" + loginPwd + "'";
            //以上正好完成了sql语句的拼接，以下代码的含义是发送sql语句给DBMS，DBMS进行sql编译
            //正好将用户提供的"非法信息"编译进去，导致了原sql语句的含义被扭曲了
            rs = stmt.executeQuery(sql);
            //5、处理查询结果集
            if (rs.next()) {
                loginSuccess = true;
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally {
            //关闭资源，从小到大
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
        return loginSuccess;
    }

    /**
     * 初始化用户揭秘那
     *
     * @return 用户输入的用户名和密码等信息
     */
    private static Map<String, String> initUI() {
        Scanner sc = new Scanner(System.in);
        System.out.print("用户名：");
        String loginName = sc.nextLine();
        System.out.print("密码：");
        String loginPwd = sc.nextLine();
        Map<String, String> userLoginInfo = new HashMap<>();
        userLoginInfo.put("loginName", loginName);
        userLoginInfo.put("loginPwd", loginPwd);
        return userLoginInfo;
    }
}
```

```java
import java.sql.*;
import java.util.HashMap;
import java.util.Map;
import java.util.ResourceBundle;
import java.util.Scanner;

/*
   1、模拟用户登录功能的实现
   2、解决sql注入问题：
       只要用户提供的信息不参与sql语句的编译过程，问题就解决了。
       即使用户提供的信息中含有sql语句的关键字，但是没有参与编译，不起作用了。
       要想用户信息不参与sql语句的编译，必须使用java.sql.PreparedStatement
       java.sql.PreparedStatement接口继承了java.sql.Statement
       java.sql.PreparedStatement是属于预编译的数据库操作对象
       java.sql.PreparedStatement的原理是，预先对sql语句的框架进行编译，然后再给sql语句传"值"。
   3、测试结果：
       用户名：fdas
       密码：fdsa' or '1'='1
       登陆失败
   4、解决sql注入的关键是什么？
       用户提供的信息即使含有sql语句的关键字，但是这些关键字并没有参与编译，不起作用。
*/
public class JDBCTest07 {
        public static void main(String[] args) {
            //初始化界面
            Map<String, String> userLoginInfo = initUI();
            //验证用户名和密码
            boolean loginSuccess = login(userLoginInfo);
            //输出结果
            System.out.println(loginSuccess ? "登陆成功" : "登陆失败");
        }

        /**
         * @param userLoginInfo 用户登录信息
         * @return false表示登陆失败，true表示登陆成功
         */
        private static boolean login(Map<String, String> userLoginInfo) {
            //打标记
            boolean loginSuccess = false;
            //单独定义变量
            String loginName = userLoginInfo.get("loginName");
            String loginPwd = userLoginInfo.get("loginPwd");
            //JDBC代码
            ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
            String driver = bundle.getString("driver");
            String url = bundle.getString("url");
            String user = bundle.getString("user");
            String password = bundle.getString("password");
            Connection conn = null;
            PreparedStatement ps = null;
            ResultSet rs = null;
            try {
                //1、注册驱动
                Class.forName(driver);
                //2、获取连接
                conn = DriverManager.getConnection(url, user, password);
                //3、获取预编译的数据库操作对象
                //sql语句的框子，其中一个 ? 表示一个占位符，一个 ? 将来接受一个"值"。注意：占位符?不能使用单引号括起来。
                String sql = "select * from t_user where loginName = ? and loginPwd = ?";
                ps = conn.prepareStatement(sql);//程序执行到此处，会发送sql语句的框子给DBMS，然后DBMS进行sql语句的预先编译。
                //给占位符?传值（第一个问号的下标是1，第二个问号的下标是2，JDBC中所有下标从1开始）。
                ps.setString(1, loginName);//这里是调用setString给?传值，那么?就会自动加一个单引号。
                ps.setString(2, loginPwd);
                //4、执行sql语句
                rs = ps.executeQuery();
                //5、处理查询结果集
                if (rs.next()) {
                    loginSuccess = true;
                }
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            } catch (SQLException throwables) {
                throwables.printStackTrace();
            } finally {
                //关闭资源，从小到大
                if (rs != null) {
                    try {
                        rs.close();
                    } catch (SQLException throwables) {
                        throwables.printStackTrace();
                    }
                }
                if (ps != null) {
                    try {
                        ps.close();
                    } catch (SQLException throwables) {
                        throwables.printStackTrace();
                    }
                }
                if (conn != null) {
                    try {
                        conn.close();
                    } catch (SQLException throwables) {
                        throwables.printStackTrace();
                    }
                }
            }
            return loginSuccess;
        }

        /**
         * 初始化用户揭秘那
         *
         * @return 用户输入的用户名和密码等信息
         */
        private static Map<String, String> initUI() {
            Scanner sc = new Scanner(System.in);
            System.out.print("用户名：");
            String loginName = sc.nextLine();
            System.out.print("密码：");
            String loginPwd = sc.nextLine();
            Map<String, String> userLoginInfo = new HashMap<>();
            userLoginInfo.put("loginName", loginName);
            userLoginInfo.put("loginPwd", loginPwd);
            return userLoginInfo;
        }
}
```
**对比Statement和PreparedStatement？**
* Statement存在sql注入问题，而PreparedStatement解决了sql注入问题。
* Statement是编译一次执行一次，而PreparedStatement是编译一次，可执行多次。PreparedStatement效率较高一些。
* PreparedStatement会在编译阶段做类型的安全检查。
* 综上所述，PreparedStatement，只有极少数情况下需要使用Statement。
**什么情况下需要使用Statement?**
业务方面要求必须支持sql注入的时候。Statement支持sql注入，凡是业务方面要求是需要进行sql语句拼接的，必须使用Statement。

示例: 升降序，必须使用sql注入

```java
import java.sql.*;
import java.util.ResourceBundle;
import java.util.Scanner;

public class JDBCTest08 {
    public static void main(String[] args) {
        System.out.print("请输入asc或者desc（asc表示升序，desc表示降序）: ");
        Scanner sc = new Scanner(System.in);
        String keyWord = sc.nextLine();

        ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            //注册驱动
            Class.forName(driver);
            //获取连接
            conn = DriverManager.getConnection(url, user, password);
            //获取数据库操作对象
            stmt = conn.createStatement();
            //执行sql语句
            String sql = "select ename from emp order by ename " + keyWord;
            rs = stmt.executeQuery(sql);
            //处理查询结果集
            while(rs.next()){
                String ename = rs.getString("ename");
                System.out.println(ename);
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally{
            //关闭资源
            if(rs != null){
                try {
                    rs.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if(stmt != null){
                try {
                    stmt.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
    }
}
```
**使用PreparedStatement会出现语法错误，因为setString()方法会自动给asc或者desc加一个单引号。**
```java
import java.sql.*;
import java.util.ResourceBundle;
import java.util.Scanner;

public class JDBCTest08 {
    public static void main(String[] args) {
        System.out.print("请输入asc或者desc（asc表示升序，desc表示降序）: ");
        Scanner sc = new Scanner(System.in);
        String keyWord = sc.nextLine();

        ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        Connection conn = null;
        PreparedStatement ps = null;
        ResultSet rs = null;
        try {
            //注册驱动
            Class.forName(driver);
            //获取连接
            conn = DriverManager.getConnection(url, user, password);
            //获取预编译的数据库操作对象
            String sql = "select ename from emp order by ename ?";
            ps = conn.prepareStatement(sql);
            ps.setString(1, keyWord);//注意：这里会给asc或者desc自动加一个单引号，因此语法错误
            //执行sql语句
            rs = ps.executeQuery();
            //处理查询结果集
            while(rs.next()){
                String ename = rs.getString("ename");
                System.out.println(ename);
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally{
            //关闭资源
            if(rs != null){
                try {
                    rs.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if(ps != null){
                try {
                    ps.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if(conn != null){
                try {
                    conn.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
    }
}
```
使用PreparedStatement完成增删改：
```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.ResourceBundle;

public class JDBCTest09 {
    public static void main(String[] args) {
        ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        Connection conn = null;
        PreparedStatement ps = null;
        try {
            //注册驱动
            Class.forName(driver);
            //获取连接
            conn = DriverManager.getConnection(url, user, password);
            //获取预编译的数据库操作对象
            /*增：
            String sql = "insert into dept(deptno, dname, loc) values(?,?,?)";
            ps = conn.prepareStatement(sql);
            ps.setInt(1, 60);
            ps.setString(2, "销售部");
            ps.setString(3, "上海");*/
            /*删：
            String sql = "delete from dept where deptno = ?";
            ps = conn.prepareStatement(sql);
            ps.setInt(1, 60);*/
            //改：
            String sql = "update dept set dname = ?,loc = ? where deptno = ?";
            ps = conn.prepareStatement(sql);
            ps.setString(1, "研发一部");
            ps.setString(2, "北京");
            ps.setInt(3, 50);
            //执行sql语句
            int count = ps.executeUpdate();
            System.out.println(count);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally{
            //关闭资源
            if (ps == null) {
                try {
                    ps.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if (conn == null) {
                try {
                    conn.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
    }
}
```

>**JDBC的事务是自动提交的，什么是自动提交？**
>只要执行一条DML语句，就自动提交一次，这是JDBC默认的事务行为。但是在实际的业务中，通常都是N条DML语句共同联合才能完成的，必须保证这些DML语句在同一个事务中同时成功或者同时失败。
>**重点三行代码**：
>conn.setAutoCommit(false);//将自动提交机制修改为手动提交
>conn.commit();//提交事务
>conn.rollback();//回滚事务

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.ResourceBundle;

/*
* 两个账户：111 20000
*          222 0
* 现在让111账户往222账户转账10000
* */
public class JDBCTest10 {
    public static void main(String[] args) {
        ResourceBundle bundle = ResourceBundle.getBundle("jdbc");
        String driver = bundle.getString("driver");
        String url = bundle.getString("url");
        String user = bundle.getString("user");
        String password = bundle.getString("password");
        Connection conn = null;
        PreparedStatement ps = null;
        try {
            //注册驱动
            Class.forName(driver);
            //获取连接
            conn = DriverManager.getConnection(url, user, password);
            conn.setAutoCommit(false);//将自动提交机制修改为手动提交
            //获取数据库操作对象
            String sql = "update t_act set balance = ? where actno = ?";
            ps = conn.prepareStatement(sql);

            ps.setDouble(1, 10000);
            ps.setInt(2, 111);
            int count = ps.executeUpdate();

            ps.setDouble(1, 10000);
            ps.setInt(2, 222);
            count += ps.executeUpdate();
            System.out.println(count == 2 ? "转账成功" : "转账失败");
            //提交事务
            conn.commit();//程序能够执行到这里说明以上程序没有异常，事务结束，手动提交数据
        } catch (ClassNotFoundException e) {
            //回滚事务
            if(conn != null){
                try {
                    conn.rollback();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            e.printStackTrace();
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        } finally{
            //关闭资源
            if (ps == null) {
                try {
                    ps.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if (conn == null) {
                try {
                    conn.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
    }
}
```
工具类中的构造方法都是私有的。因为工具类中的方法都是静态的，不需要new对象，直接采用类名调用。
![图示](https://img-blog.csdnimg.cn/2021032812550654.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)