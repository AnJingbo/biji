

# MyBatis 笔记
## 一、软件开发常用结构
### 1.1 三层架构
* 界面层（表示层，视图层）：和用户打交道的，主要接受用户的请求参数，显示请求的处理结果（jsp、html、servlet）。
* 业务逻辑层：接受了界面层传递的数据，计算业务逻辑，调用数据访问层获取数据。
* 数据访问层（持久层）：访问数据库，主要实现对数据的增、删、查、改。
### 1.2 三层对应的包
* 界面层： controller 包（servlet）
* 业务逻辑层：service 包（XXXService 类）
* 数据访问层：dao 包（XXXDao 类）
### 1.3 三层中类的交互
用户使用的界面层 ---> 业务逻辑层 ---> 数据访问层（持久层） ---> 数据库（MySQL）
### 1.4 三层对应的处理框架
* 界面层 --- servlet --- SpringMVC（框架）
* 业务逻辑层 --- service 类 --- spring（框架）
* 数据访问层 --- dao 类 --- mybatis（框架）
### 1.5 框架
* 框架是一个模板：
（1）框架中定义好了一些功能，这些功能是可用的。
（2）可以加入项目中自己的功能，这些功能可以利用框架中写好的功能。
* 框架是一个半成品软件，定义好了一些基础功能，需要加入你的功能之后就是完善的。
（1）框架一般不是全能的，不能做所有事情。
（2）框架是针对某一个领域有效，特长在某一方面，比如 mybatis 做数据库操作强，但是它不能做其它的。
### 1.6 使用 JDBC 的缺陷
1、代码比较多，开发效率低。
2、需要关注 Connection、Statement、ResultSet 对象的创建和销毁。
3、对 ResultSet 查询的结果，需要自己封装为 List。
4、重复的代码比较多。
5、业务代码和数据库的操作混在了一起。
## 二、MyBatis 框架快速入门（重点！！！）
### 2.1 MyBatis 框架
* MyBatis 是一个**基于 Java 的持久层框架**，提供的持久层框架包括 sql mapper 和 Data Access Objects（DAOs）。早期叫做 ibatis。现在代码在 GitHub 上。
* MyBatis 是 MyBatis SQL Mapper Framework for Java（sql 映射框架）
（1）sql mapper ：sql 映射。可以把数据库表中的一行数据，映射为一个 Java 对象。也就是一行数据可以看作是一个 Java 对象，操作这个对象，相当于操作表中的数据。
（2）Data Access Objects（DAOs）：数据访问。对数据库执行增删查改。
* MyBatis 提供了哪些功能：
（1）提供了创建 Connection、Statement、ResultSet 的能力，不用开发人员创建这些对象了。
（2）提供了执行 sql 语句的能力，不用你执行 sql 语句。
（3）提供了循环 sql，把 sql 的结果转换为 Java 对象，不用开发人员创建这些对象了。
（4）提供了关闭资源的能力，不用你关闭 Connection、Statement、ResultSet。
* 开发人员只需要提供 sql 语句即可，剩下的 MyBatis 会处理 sql 语句。
* **总结：MyBatis 是一个 sql 映射框架，提供了数据库的操作能力，可以看作是增强的 JDBC。使用 MyBatis 让开发人员集中精神写 sql 就可以了，不必关心 Connection、Statement、ResultSet 的创建、销毁以及 sql 的执行。**
### 2.2 MyBatis 入门案例
* 实现步骤：
（1）新建的 student 表。
（2）加入 maven 的mybatis 坐标和 mysql 驱动的坐标。
（3）创建 Student 实体类，这个实体类用来保存表中的一行数据。
（4）创建持久层的 dao 接口，定义操作数据库的方法。
（5）**创建 mybatis 使用的配置文件，这个文件叫 sql 映射文件，是用来写 sql 语句的。一般一个表一个 sql 映射文件。这个文件是 xml 文件。这个文件写在 dao 的目录之下，也就是接口所在的目录，同时文件名称和接口保持一致。**
（6）**创建一个 mybatis 的主配置文件。一个项目中就一个主配置文件，主配置文件提供了数据库的连接信息和 sql 映射文件的位置信息。这个文件放在 resources 根目录之下**
（7）创建使用 mybatis 的类，通过 mybatis 访问数据库。

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!-- sql 映射文件（sql mapper），用来写 sql 语句的，mybatis 会执行这些 sql -->

<!-- 用来指定约束文件
        mybatis-3-mapper.dtd 是约束文件的名称，扩展名是 dtd。
        约束文件的作用：是用来限制和检查在当前文件中出现的标签和属性必须符合 mybatis 的要求。
-->
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    mapper 是当前文件的根标签，必须的。
    namespace：命名空间，这个空间是唯一值。可以是自定义的字符串，但是要求你使用 dao 接口的全限定名称（Copy Reference）
-->
<mapper namespace="com.atjingbo.dao.StudentDao">
    <!--
        在当前文件中，可以使用特定的标签来表示数据库的特定操作。
            <select> 表示要执行查询，执行 select 语句
            <update> 表示更新数据库的操作，执行 update 语句
            <insert> 表示插入数据，执行 insert 语句
            <delete> 表示删除数据，执行 delete 语句
        id：你要执行的 sql 语句的唯一标识，你的 mybatis 会使用这个 id 的值来找到要执行的 sql 语句。
            它是一个唯一值，可以自定义，但是要求你使用接口中的方法名称。
        resultType：表示结果类型的，是 sql 语句执行后得到的 ResultSet，遍历这个 ResultSet 得到的 Java 对象的类型。
                    值：写的是类型的全限定名称
    -->
    <select id="queryStudents" resultType="com.atjingbo.pojo.Student">
        select id,name,email,age from student order by id
    </select>
    <insert id="insertStudent">
        insert into student values(#{id},#{name},#{email},#{age})
    </insert>
</mapper>
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!-- mybatis 的主配置文件，主要定义了数据库的配置信息，以及 sql 映射文件的位置 -->

<!-- 约束文件的说明
        mybatis-3-mapper.dtd 是约束文件的名称，扩展名是 dtd。
        约束文件的作用：是用来限制和检查在当前文件中出现的标签和属性必须符合 mybatis 的要求。
-->
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!-- configuration 根标签 -->
<configuration>
    <!-- settings：控制 mybatis 全局行为 -->
    <settings>
        <!-- 设置 mybatis 输出日志到控制台 -->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>
    
    <!-- 环境配置：数据库的连接信息
             default：必须和某个 environment 的id 值一样。它告诉 mybatis 使用哪个数据库的连接信息，
                      也就是访问那个数据库。
    -->
    <environments default="mydev">
        <!-- environment（环境）：一个数据库信息的配置。
             id：是一个唯一值，自定义的，表示环境的名称。
        -->
        <environment id="mydev">
            <!-- transactionManager：mybatis 的事务类型，也就是提交事务、回滚事务的方式。
                     type：事务的处理的类型
                     （1）JDBC：表示 mybatis 底层调用的是 JDBC 中的 Connection 对象的 commit、rollback 做事务处理。
                     （2）MANAGED：表示把 mybatis 的事务处理委托给其他的容器（一个服务器软件、一个框架（spring））
            -->
            <transactionManager type="JDBC"/>
            <!-- dataSource：表示数据源，用来链接数据库。java 体系中，规定实现了 javax.sql.DataSource 接口的都是数据源。数据源表示 Connection 对象。
                     type：指定数据源的类型
                     （1）POOLED：表示使用连接池，mybatis 会创建 PooledDataSource 类。
                     （2）UPOOLED：表示不使用连接池，每次执行 sql 语句时，先创建连接，执行完 sql 后，再关闭连接。
                                   mybatis 会创建 UnPooledDataSource 类，来管理 Connection 对象的使用。
                     （3）JNDI：java 命名和目录服务（类似windows注册表）
            -->
            <dataSource type="POOLED">
                <!-- driver、url、username、password 是固定的，不能自定义-->

                <!-- 数据库的驱动类名 -->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <!-- 连接数据库的 url 字符串 -->
                <property name="url" value="jdbc:mysql://localhost:3306/bjpowernode"/>
                <!-- 访问数据库的数据名称 -->
                <property name="username" value="root"/>
                <!-- 密码 -->
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 用来指定 sql 映射文件（sql mapper）的位置 -->
    <mappers>
        <!-- 一个 mapper 标签指定一个 sql 映射文件的位置，
             这个位置是从类路径下开始的路径信息。类路径：target/classes。
             因此需要在 pom 文件中指定资源位置，让 .xml 文件也在 target/classes 下
        -->
        <mapper resource="com/atjingbo/dao/StudentDao.xml"/>
    </mappers>
</configuration>
```

```java
import com.atjingbo.pojo.Student;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class MyApp {
    // 访问 mybatis 读取 student 数据
    public static void main(String[] args) throws IOException {
        // 1、定义 mybatis 主配置文件的名称，从类路径的根开始（target/classes）
        String config = "mybatis.xml";
        // 2、读取这个 config 表示的文件
        InputStream in = Resources.getResourceAsStream(config);
        // 3、创建 SqlSessionFactoryBuilder 对象
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        // 4、创建 SqlSessionFactory 对象
        SqlSessionFactory factory = builder.build(in);
        // 5、根据 SqlSessionFactory 获取 SqlSession
        SqlSession sqlSession = factory.openSession();
        // 6、指定要执行的 sql 语句的标识。sql 映射文件中的 namespace + "." + 标签的 id 值。【重要】
        String sqlId = "com.atjingbo.dao.StudentDao" + "." + "queryStudents";
        // 7、执行 sql 语句，通过 sqlId 找到语句【重要】
        List<Student> studentList = sqlSession.selectList(sqlId);
        // 8、输出结果
        for (Student student : studentList){
            System.out.println(student);
        }
        // 9、关闭 SqlSession 对象
        sqlSession.close();
    }
}
```
**提交、更新、删除操作在编写代码时需要注意：**
```java
// mybatis 默认是不自动提交事务的，所以在 insert、update、delete 后要在执行完 sql 语句后手动提交事务
sqlSession.commit();
// 或者 直接设定自动提交事务
SqlSession sqlSession = factory.openSession(true);
```
### 2.3 主要类的介绍
* Resources：mybatis 中的一个类，负责读取主配置文件。
* SqlSessionFactoryBuilder：创建 SqlSessionFactory 对象。
* **SqlSessionFactory：重量级对象，也就是说程序创建一个对象耗时较长，使用资源比较多。在整个项目中，有一个就够用了。作用是用来获取 SqlSession 对象。** SqlSessionFactory 接口的实现类：DefaultSqlSessionFactory。
**openSession() 方法说明：** 1、openSession() 以及 openSession(false)，获取的是非自动提交事务的 SqlSession 对象。2、openSession(true)：获取自动提交事务的 SqlSession 对象。**
* **SqlSession：定义了操作数据库的方法。** 比如：selectOne()、selectList()、selectOne()、insert()、delete()、update()、commit()、rollback()。SqlSession 接口的实现类是：DefaultSqlSession。
**使用要求：SqlSession 对象不是线程安全的，需要在方法内部使用。在执行 sql 语句之前，使用 openSession() 方法获取 SqlSession 对象。在执行完 sql 语句后，需要执行 sqlSsession.close() 关闭它，这样才能保证它的使用是线程安全的。**
### 2.4 配置日志功能
![图示](https://img-blog.csdnimg.cn/20210714204546712.png)
### 2.5 工具类的使用
```java
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class MyBatisUtils {
    private static SqlSessionFactory factory = null;
    static{
        String config = "mybatis.xml";
        try {
            InputStream in = Resources.getResourceAsStream(config);
            factory = new SqlSessionFactoryBuilder().build(in);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public static SqlSession getSqlSession(){
        SqlSession sqlSession = null;
        if(factory != null){
            sqlSession = factory.openSession();// mybatis 默认不自动提交事务
        }
        return sqlSession;
    }
}
```
### 2.6 MyBatis 的动态代理
* MyBatis 的动态代理：MyBatis 帮你创建 dao 接口的实现类，并且在实现类中调用 SqlSession 中的方法执行 sql 语句。
* 使用 MyBatis 的动态代理机制，使用 **sqlSession.getMapper(dao接口.class) 方法，该方法能够获取 dao 接口对应的实现类对象。也就是说你的程序中不需要再手动创建 dao 接口对应的实现类了，而由 MyBatis 根据 dao 接口在内部将实现类创建出来。**
* 使用动态代理的要求：
（1）dao 接口和 mapper 文件放在一起，同一个目录。
（2）dao 接口和 mapper 文件名称一致。
（3）mapper 文件中的 namespace 的值是 dao 接口的全限定名称。
（4）mapper 文件中的 \<select>、\<insert> 等的 id 值是接口中的方法名称。
（5）dao 接口中不要使用重载方法。
```java
public class TestStudentDao {
    @Test
    public void queryStudents() {
        // 使用动态代理机制，不需要手动实现 StudentDao 接口对应的实现类。
        SqlSession sqlSession = MyBatisUtils.getSqlSession();
        StudentDao studentDao = sqlSession.getMapper(StudentDao.class);

        List<Student> students = studentDao.queryStudents();
        for(Student student : students){
            System.out.println(student);
        }
        sqlSession.close();
    }

    @Test
    public void insertStudent() {
        SqlSession sqlSession = MyBatisUtils.getSqlSession();
        StudentDao studentDao = sqlSession.getMapper(StudentDao.class);

        Student student = new Student(1008, "孙权", "sunquan@qq.com", 52);
        studentDao.insertStudent(student);
        sqlSession.commit();
        sqlSession.close();
    }
}
```
### 2.7 深入理解参数
**传入参数：从 java 代码中把参数传递到 mapper.xml 文件。**
#### 2.7.1 parameterType：写在 mapper 文件中的一个属性。表示 dao 接口中方法参数的数据类型。
```xml
<!-- parameterType：dao 接口中方法参数的数据类型。
     parameterType 它的值是 java 的数据类型的全限定名称或者是 mybatis 定义的别名。
     注：parameterType 不是强制的，因为 mybatis 通过反射机制能够发现接口中方法参数的数据类型，所以我们一般不写 parameterType。
-->
<select id="queryStudentById" parameterType="java.lang.Integer" resultType="com.atjingbo.pojo.Student">
    select id,name,email,age from student where id=#{id}
</select>
```
#### 2.7.2【掌握】 传入一个简单类型的参数：
（1）**简单类型：mybatis 把 Java 的基本数据类型和 String 都叫简单类型。**
（2）当 dao 接口中方法的参数只有一个简单类型的参数，在 mapper 文件中获取一个简单类型的参数值，格式：``#{任意字符}``。
```java
public interface StudentDao {
    public Student queryStudentById(Integer id); // mapper 文件中的内容在上面。把参数中的 id 值传到 mapper.xml 文件。
}
```
```xml
<!-- 使用 #{} 之后，mybatis 执行 sql 使用的是 PreparedStatement 对象。
     由 mybatis 执行一下的代码：
         1、mybatis 创建 Connection、PreparedStatement 等对象。
           String sql = "select id,name,email,age from student where id=?"
           PreparedStatement pst = conn.preparedStatement(sql);
           pst.setInt(1, 1001);
         2、执行 sql 封装为 resultType="com.atjingbo.pojo.Student" 这个对象，并将该对象返回
-->
<select id="queryStudentById" parameterType="java.lang.Integer" resultType="com.atjingbo.pojo.Student">
    select id,name,email,age from student where id=#{id}
</select>
```
#### 2.7.3【掌握】 多个参数 - 使用 @Param("自定义参数名")
**当 dao 接口中的方法有多个参数，那么需要通过名称使用参数（通过 @Param 注解给参数命名）。在方法形参前面加入 @Param("自定义参数名")，mapper 文件使用 #{自定义参数名}。注意：两个自定义参数名要一致。**
```java
public interface StudentDao {
    // 多个参数：在形参定义的前面加入：@Param("自定义参数名称")
    public List<Student> queryStudentsByNameAndAge(@Param("myname") String name, @Param("myage") Integer age);
}
```
```xml
<select id="queryStudentsByNameAndAge" resultType="com.atjingbo.pojo.Student">
    select id,name,email,age from student where name=#{myname} or age=#{myage}
</select>
```
#### 2.7.4【掌握】 多个参数 - 使用对象的方式
**使用 java 对象传递参数，java 对象的属性值就是 sql 需要的值。每一个属性就是一个参数。** 完整格式：#{属性名,javaType=java 中属性的数据类型,jdbcType=在数据库中的数据类型}。javaType、jdbcType 的类型 mybatis 可以通过反射机制获取，一般不需要设置。因此**常用格式：**``#{属性名}``。**注意：#{属性名} 里面的一定是属性的名称。**
```java
public interface StudentDao {
// 多个参数，使用 java 对象的属性值，作为参数的实际值。
    public List<Student> queryStudentsByNameAndAge(Student student);
}
```
```xml
<select id="queryStudentsByNameAndAge" resultType="com.atjingbo.pojo.Student">
    <!-- #{属性名} 里面的东西一定要和 Student 对象里面的属性名称一致 -->
    select id,name,email,age from student where name=#{name} or age=#{age}
</select>
```
#### 2.7.5【了解】 多个参数 - 按位置
参数位置从 0 开始，引用参数语法：``#{arg 位置}``。例如：第一个参数是 ``#{arg0}``，第二个参数是``#{arg1}``。
```java
public interface StudentDao {
    // 多个参数，按位置传值
    public List<Student> queryStudentsByNameAndAge2(String name, Integer age);
}
```
```xml
<select id="queryStudentsByNameAndAge2" resultType="com.atjingbo.pojo.Student">
    select id,name,email,age from student where name=#{arg0} or age=#{arg1}
</select>
```
#### 2.7.6【了解】 多个参数 - 使用 Map
Map 集合可以存储多个值，使用 Map 向 mapper 文件一次传入多个参数。Map<String, Object>。在 mapper 文件中使用 ``#{map 的 key}`` 引用参数值。
```java
public interface StudentDao {
    // 多个参数，使用 Map 存放多个值
    public List<Student> queryStudentsByNameAndAge2(Map<String, Object> map);
}
```
```xml
<select id="queryStudentsByNameAndAge2" resultType="com.atjingbo.pojo.Student">
    <!-- 多个参数，使用 map，语法：#{map 的 key}-->
    select id,name,email,age from student where name=#{myname} or age=#{age}
</select>
```
### 2.8 # 和 $ 的比较
**\# 和 $ 的区别（重点）**
（1）# 使用 ? 在 sql 语句中做占位的，并使用 PreparedStatement 对象执行 sql 语句，效率高。
（2）# 能够避免 sql 注入，更加安全。
（3）$ 不使用占位符，而是使用字符串连接方式，使用 Statement 对象执行 sql 语句，效率低。
（4）$ 有 sql 注入的风险，缺乏安全性。
（5）$ 可以替换列名或者表名。或者如果你能确定你的数据是安全的，可以使用 $。
```java
select id,name,email,age from student where id=#{id}
使用 # 的结果：select id,name,email,age from student where id=?

select id,name,email,age from student where id=${id}
使用 $ 的结果：select id,name,email,age from student where id=1001
```
**$ 替换列名或者表名的示例：**
```java
public interface StudentDao {
    // 按照列名进行排序。如果传一个 id 就是根据 id 进行排序
    public List<Student> queryStudentsOrder(String colName);
}
```
```xml
<select id="queryStudentsOrder" resultType="com.atjingbo.pojo.Student">
    select id,name,email,age from student order by ${colName}
</select>
```
### 2.9 MyBatis 的输出结果
所谓输出结果就是指：mybatis 执行 sql 语句后，得到了 java 对象。

**注意：resultType 和 resultMap 不能同时使用。**
#### 2.9.1 resultType
resultType：结果类型，表示执行完 sql 语句后，sql 语句的执行结果转为的 java 对象的数据类型，这个 java 对象的数据类型是任意的。**resultType 的默认原则是 sql 语句中同名的列值赋值给 java 对象中同名的属性。** 

**注意：resultType 的值和接口中方法的返回值是一致的。如果返回的是集合，那应该将 resultType 设置为集合包含的类型，而不是集合本身。**

**resultType 里面的值是类型的全限定名称或者类型的别名**。例如：java.lang.Integer 的别名是 int。

**定义自定义类型的别名**
（1）在 mybais 的主配置文件中，使用 \<typeAliases> 定义别名。
（2）可以在 resultType 中使用自定义别名。
```xml
<typeAliases>
    <!--
        第一种方式：可以指定一个类型一个自定义别名。type：自定义类型的全限定名称；alias：别名。
    -->
    <!--<typeAlias type="com.atjingbo.pojo.Student" alias="stu" />-->
        
    <!-- 第二种方式：<package> name 是包名，这个包中的所有类，类名就是别名（类名不区分大小写）-->
    <package name="com.atjingbo.pojo"/>
</typeAliases>
```
**接口中方法的返回值是 Map<Object,Object>：**
（1）map 的 key 是列名，map 的 value 是列值。
（2）如果使用 Map<Object,Object> 要返回多行结果，那么需要把它装进 List 集合中，也就是接口中方法的返回值需要是 List<Map<Object,Object>>。
#### 2.9.2 resultMap
* resultMap：结果映射，指定 sql 语句中的列名和 java 对象中的属性的对应关系，可以更灵活地把列值赋值给指定属性。
* 两种用法：
（1）你可以自定义 sql 语句中的列值赋值给哪个属性。resultType 默认原则是列名的列值赋值给同名的属性，而使用 resultMap 可以把列名赋值给其它的属性。
（2）当你的列名和属性名不一样时，一定要使用 resultMap。因为resultType 默认是只有列名和属性名一样时才可以赋值上去。

```xml
<!-- 使用 resultMap
     （1）先定义 resultMap。
     （2）在 select 标签，使用 resultMap 来引用（1）中定义的。
-->
<!-- 定义 resultMap
     id：自定义的名称，表示你定义的这个 resultMap。type：java 类型的全限定名称。
-->
<resultMap id="studentMap" type="com.atjingbo.pojo.Student">
    <!-- 主键列：使用 id 标签
         column：列名。property：java 类型的属性名。
    -->
    <id column="id" property="id" />
    <!-- 非主键列：使用 result -->
    <result column="name" property="name" />
    <result column="email" property="email" />
    <result column="age" property="age" />
</resultMap>
<select id="queryStudentsByResultMap" resultMap="studentMap">
    select id,name,email,age from student order by id
</select>
```
### 2.10 列名和属性名不同的两种解决方式：
（1）使用 resultMap。（推荐）
（2）使用列别名。resultType 的默认原则是 sql 语句中同名的列值赋值给 java 对象中同名的属性。示例如下：
```xml
<select id="queryStudents" resultType="com.atjingbo.pojo.MyStudent">
    select id stuid,name stuname,email stuemail,age stuage from student
</select>
```
### 2.11 模糊查询的两种方案
第一种方式：java 代码指定 like 的内容。
```java
// 第一种方式：java 代码指定 like 的内容。比如传入："%刘%"
public List<Student> queryStudentsLike(String name);
```
```xml
<select id="queryStudentsLike" resultType="com.atjingbo.pojo.Student">
    select id,name,email,age from student where name like #{name}
</select>
```
第二种方式：在 mapper 文件中拼接 like 的内容。
```java
// 第二种方式：在 mapper 文件中拼接 like 的内容。比如传入："刘"
public List<Student> queryStudentsLike2(String name);
```
```xml
<select id="queryStudentsLike2" resultType="com.atjingbo.pojo.Student">
    select id,name,email,age from student where name like "%" #{name} "%"
</select>
```
## 三、MyBatis 框架动态 sql
动态 sql：sql 的内容是变化的，可以根据条件获取到不同的 sql 语句。主要是 where 的部分发生变化。

动态 sql 的实现，使用的是 mybatis 提供的标签。\<if>、\<where> 和 \<foreach>。

**动态 sql 一定要使用 java 对象作为接口中方法的参数。**
### 3.1 if 标签
**\<if> 标签是判断条件的。当 test 的值为 true 的时候，会将其包含的 SQL 片段拼接到其所在的 SQL 语句中。**

**3.1.1 语法格式：**
``<if test="使用参数java对象的属性值作为判断条件">
    部分SQL语句
</if>``

**3.1.2 使用示例：**
```java
public interface StudentDao{
    // 动态 sql，要使用 java 对象作为参数。
    public List<Student> queryStudentsIf(Student student);
}
```
```xml
<!-- <if:test="使用参数 java 对象的属性值作为判断条件"> -->
<select id="queryStudentsIf" resultType="com.atjingbo.pojo.Student">
    select id,name,email,age from student where 1=1
    <if test="name!=null and name!=''">
        name=#{name}
    </if>
    <if test="age>0">
        and age > #{age}
    </if>
</select>
```
### 3.2 where 标签
\<if> 标签存在一个比较麻烦的地方：需要在 sql 语句的 where 后面手工添加 1=1 的子句。因为如果 where 后面的所有 \<if> 条件均为 false，而 where 后面又没有 1=1 子句，那么 sql 语句中就会只剩下一个空的 where，sql 语句就会出错。另外在上面的例子中，如果只有第二个 if 成立，因为有 1=1，所以没事。但是如果只有第一个 if 成立，那么因为第一个的 sql 语句前面没有 and 或者 or 等，所以也会报错。

**\<where> 标签是用来包含多个 \<if> 标签的。当多个 if 中有一个成立的时候， \<where> 标签会在 sql 语句中自动增加一个 where 关键字，并去掉 if 中多余的 and、or 等。如果一个 if 都不成立，那么就不会添加 where 关键字。**

**3.2.1 语法格式：**``<where> <if><if>... </where>``

**3.2.2 使用示例：**
```java
public interface StudentDao{
    // 动态 sql，要使用 java 对象作为参数。
    public List<Student> queryStudentsWhere(Student student);
}
```
```xml
<!-- <where> <if><if>... </where> -->
<select id="queryStudentsWhere" resultType="com.atjingbo.pojo.Student">
    select id,name,email,age from student
    <where>
        <if test="name!=null and name!=''">
            name=#{name}
        </if>
        <if test="age>0">
            and age > #{age}
        </if>
    </where>
</select>
```
### 3.3 foreach 标签
**\<foreach> 标签是用来循环 java 中的数组、list 集合的。重要用在 sql 的 in 语句中。**

**3.3.1 语法格式：**
```xml
<!--
collection：表示接口中方法参数的类型。如果是数组则使用 array，如果是 list 集合则使用 list。
item：自定义的，表示数组或集合成员的变量。
open：循环年开始时的字符。
close：循环结束时的字符。
separator：集合成员之间的分隔符。
-->
<foreach collection="" item="" open="" close="" separator="">
</foreach>
```
**3.3.2 使用示例：**
```java
public interface StudentDao{
    // foreach 用法 1
    public List<Student> queryStudentsForOne(List<Integer> list);
}
```
```xml
<select id="queryStudentsForOne" resultType="com.atjingbo.pojo.Student">
    select id,name,email,age from student where id in
    <foreach collection="list" item="myId" open="(" close=")" separator=",">
        #{myId}
    </foreach>
</select>
```
```java
public interface StudentDao{
    // foreach 用法 2
    public List<Student> queryStudentsForTwo(List<Student> list);
}
```
```xml
<select id="queryStudentsForTwo" resultType="com.atjingbo.pojo.Student">
    select id,name,email,age from student where id in
    <foreach collection="list" item="stu" open="(" close=")" separator=",">
        #{stu.id}
    </foreach>
</select>
```
### 3.4 sql 代码片段
* \<sql /> 标签用于定义 sql 片段，以便于其它标签复用该 sql 片段。
* 使用步骤：
（1）先定义。``<sql id="自定义名称唯一"> sql语句、表名、字段等 </sql>``。
（2）再使用。``<include refid="id的值" />``。
## 四、MyBatis 配置文件
### 4.1 数据库的属性配置文件
* 数据库的属性配置文件：把数据库连接信息放到一个单独的文件中。和 mybatis 主配置文件分开。目的是便于修改、保存和处理多个数据库的信息。
* 使用步骤
（1）在 `resources 目录中`定义一个属性配置文件，xxx.properties。在属性配置文件中，定义数据，格式是 key=value。key：一般使用 `.` 做多级目录。例如：jdbc.mysql.driver。
（2）在 mybatis 的主配置文件，使用 `<properties>` 指定文件的位置（从类路径的根开始找文件）。在需要使用值的地方，格式是：``${key}``。
* 示例：
（1）jdbc.properties 的内容：
```java
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/bjpowernode
jdbc.username=root
jdbc.password=123456
```
（2）主配置文件的内容

```xml
<?xml version="1.0" encoding="UTF-8" ?>

<!-- mybatis 的主配置文件，主要定义了数据库的配置信息，以及 sql 映射文件的位置 -->

<!-- 约束文件的说明
        mybatis-3-mapper.dtd 是约束文件的名称，扩展名是 dtd。
        约束文件的作用：是用来限制和检查在当前文件中出现的标签和属性必须符合 mybatis 的要求。
-->
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<!-- configuration 根标签 -->
<configuration>

    <!-- 指定 properties 文件的位置，从类路径的根开始找文件-->
    <properties resource="jdbc.properties" />
    <!-- settings：控制 mybatis 全局行为 -->
    <settings>
        <!-- 设置 mybatis 输出日志到控制台 -->
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <typeAliases>
        <!--
            第一种方式：可以指定一个类型一个自定义别名。type：自定义类型的全限定名称；alias：别名。
        -->
        <!--<typeAlias type="com.atjingbo.pojo.Student" alias="stu" />-->

        <!-- 第二种方式：<package> name 是包名，这个包中的所有类，类名就是别名（类名不区分大小写）-->
        <package name="com.atjingbo.pojo"/>
    </typeAliases>

    <!-- 环境配置：数据库的连接信息
             default：必须和某个 environment 的id 值一样。它告诉 mybatis 使用哪个数据库的连接信息，
                      也就是访问那个数据库。
    -->
    <environments default="mydev">
        <!-- environment（环境）：一个数据库信息的配置。
             id：是一个唯一值，自定义的，表示环境的名称。
        -->
        <environment id="mydev">
            <!-- transactionManager：mybatis 的事务类型，也就是提交事务、回滚事务的方式。
                     type：事务的处理的类型
                     （1）JDBC：表示 mybatis 底层调用的是 JDBC 中的 Connection 对象的 commit、rollback 做事务处理。
                     （2）MANAGED：表示把 mybatis 的事务处理委托给其他的容器（一个服务器软件、一个框架（spring））
            -->
            <transactionManager type="JDBC"/>
            <!-- dataSource：表示数据源，用来链接数据库。java 体系中，规定实现了 javax.sql.DataSource 接口的都是数据源。数据源表示 Connection 对象。
                     type：指定数据源的类型
                     （1）POOLED：表示使用连接池，mybatis 会创建 PooledDataSource 类
                     （2）UPOOLED：表示不使用连接池，每次执行 sql 语句时，先创建链接，执行完 sql 后，再关闭连接。
                                   mybatis 会创建 UnPooledDataSource 类，来管理 Connection 对象的使用。
                     （3）JNDI：java 命名和目录服务（类似windows注册表）
            -->
            <dataSource type="POOLED">
                <!-- driver、url、username、password 是固定的，不能自定义-->

                <!-- 数据库的驱动类名 -->
                <property name="driver" value="${jdbc.driver}"/>
                <!-- 连接数据库的 url 字符串 -->
                <property name="url" value="${jdbc.url}"/>
                <!-- 访问数据库的数据名称 -->
                <property name="username" value="${jdbc.username}"/>
                <!-- 密码 -->
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>

    <!-- 用来指定 sql 映射文件（sql mapper）的位置 -->
    <mappers>
        <!-- 一个 mapper 标签指定一个 sql 映射文件的位置，
             这个位置是从类路径下开始的路径信息。类路径：target/classes
        -->
        <mapper resource="com/atjingbo/dao/StudentDao.xml"/>
    </mappers>

</configuration>
```
### 4.2 指定多个 mapper 文件的方式

```xml
<mappers>
    <!-- 使用包名
          name：xml文件（mapper文件）所在的包名，这个包中所有的 xml 文件一次都能加载给 mybatis。
          使用 package 的条件：
          （1）mapper 文件名称需要和接口名称一样，区分大小写。
          （2）mapper 文件和 dao 接口需要在同一目录。
    -->
    <package name="com/atjingbo/dao"/>
</mappers>
```
## 五、扩展
### 5.1 PageHelper
* PageHelper 是做数据分页的。
* 使用步骤：
（1）在 pom.xml 文件中加入 maven 依赖。
（2）在主配置文件中加入 plugin 配置。
```xml
在 <environments> 之前加入：
<plugins>
    <plugin interceptor="com.github.pagehelper.PageInterceptor" />
</plugins>
```
* 使用示例：
```java
@Test
public void testQueryStudentsAll() {
    SqlSession sqlSession = MyBatisUtils.getSqlSession();
    StudentDao studentDao = sqlSession.getMapper(StudentDao.class);
    // 获取第 1 页的 3 条内容。pageNum：第几页，从 1 开始；pageSize：一页中有多少行数据。
    PageHelper.startPage(1, 3);
    List<Student> students = studentDao.queryStudentsAll();
    for(Student stu : students){
        System.out.println(stu);
    }
    sqlSession.close();
}
```