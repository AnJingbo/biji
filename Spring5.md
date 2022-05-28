

# Spring5 学习笔记
## 一、Spring 框架概述
1. Spring 是**轻量级**的开源的 JavaEE 框架。
2. **Spring 是为了解决企业级应用开发的复杂性而创建的**，简化开发。
3. Spring 有两个核心的部分：IOC 和 AOP
 （1）IOC：控制反转，把创建对象的过程交给 Spring 进行管理。
    （2）Aop：面向切面，不修改源代码就能进行功能增强
4. Spring 的特点：
（1）轻量。运行占用资源少，运行效率高
（2）方便解耦，简化开发。spring 提供了控制反转，由容器管理对象和对象的依赖关系
（3）Aop 编程支持，方便面向切面的编程
（4）方便集成各种优秀的框架
（5）方便程序的测试
（6）方便进行事务的操作
（7）降低 API 开发难度（对里面很多东西进行了封装）
5. 类 a 中使用了类 b 的属性或者方法，叫做类 a 依赖类 b。
6. spring 体系框架
![图示](https://img-blog.csdnimg.cn/cd79c01aa7c54bf98cbdeae472b5f6ee.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
7. spring 何时应用
![图示](https://img-blog.csdnimg.cn/8f7f49ba820b483bb457ab03a96f2ae7.png)
## 二、IOC 容器
### 2.1 什么是 IOC
（1）控制反转，把对象创建、赋值和管理工作都交给代码之外的**容器**进行实现，也就是对象的创建是由其它外部资源完成。
（2）使用 IOC 的目的：为了减少对代码的改动，也能实现不同的功能。也就是实现解耦合。具体是实现了业务对象之间的解耦合，例如 dao 和 service 对象之间的解耦合。

**注：容器可以是一个服务器软件，也可以是一个框架（spring）**
### 2.2 IOC 底层原理
* DI：依赖注入，是 IOC 的技术实现。依赖注入：只需要在程序中提供要使用的对象名称就可以，至于对象的创建、赋值和查找都由容器内部实现。spring 是使用的 DI 实现了 IOC 的功能。
* **spring 是一个容器，用来管理对象，给属性赋值。** 底层是通过反射机制创建对象。
* IOC 底层主要用到三门技术：xml 解析、工厂模式、反射。
![图示](https://img-blog.csdnimg.cn/7912528e1ac546a192e41a622b2e86ad.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
* IOC 思想基于 IOC 容器完成，IOC 容器底层就是对象工厂。
* Spring 提供 IOC 容器的两种实现方式（两个接口）
（1）BeanFactory：IOC 容器的基本实现，是 Spring 内部的使用接口，不提供开发人员进行使用。**注：使用 BeanFactory 的话，在加载配置文件的时候不会创建对象，在获取（使用）对象的时候才去创建对象。**
（2）ApplicationContext：BeanFactory 接口的子接口，提供了更多更强大的功能，一般由开发人员进行使用。**注：使用 ApplicationContext 的话，在加载配置文件的时候就会把在配置文件中的所有对象进行创建。**
* ApplicationContext 接口有两个主要的实现类：FileSystemXmlApplicationContext 和 ClassPathXmlApplicationContext。FileSystemXmlApplicationContext 在 new 对象的时候，传进去的参数是配置文件在系统盘里面的路径。而 **ClassPathXmlApplicationContext 表示是从类路径（target/classes）中加载 spring 的配置文件**。
* spring 默认创建对象的时间：在创建 spring 的容器的时候，会创建配置文件中的**所有**的对象。spring 创建对象：默认调用的是无参构造方法。

### 2.3、入门案例
* 实现步骤
（1）创建 maven 项目。
（2）导入相应的 maven 依赖。
（3）创建一个普通类。
（4）创建 Spring 配置文件(名字一般为 applicationContext.xml，放在 main/resources 下)，在配置文件中配置创建的对象。
##### （2）创建一个普通类
```java
public class User {
    public void add(){
        System.out.println("add ....");
    }
}
```
##### （3）创建 Spring 配置文件，在配置文件中配置创建的对象
**注：Spring 配置文件使用 xml 格式。**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!-- spring 配置文件
      1、beans：是根标签，spring 把 java 对象称为 bean。
      2、spring-beans.xsd 是约束文件，和 mybatis 中的 dtd 是一样的
-->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 告诉 spring 创建对象
         声明 bean 方式：就是告诉 spring 要创建某个类的对象。
         id：对象的自定义名称，是唯一值。spring 通过这个名称找到对象。
         class：类的全限定名称（不能是接口，因为 spring 通过反射机制创建对象，必须使用类）

         spring 会把创建好的对象放入到 map 中，spring 框架中有一个 map 是存放对象的。
         springMap.put(id的值, 对象);

         一个 bean 标签声明一个对象。
     -->
    <bean id="user" class="com.atjingbo.spring.User"></bean>
</beans>
```
##### （4）进行测试代码的编写
```java
public class TestUser {
    /*
    * spring 默认创建对象的时间：在创建 spring 的容器的时候，会创建配置文件中的所有的对象。
    * spring 创建对象：默认调用的是无参构造方法。
    * */
    @Test
    public void testUser(){
        // 1、创建表示 spring 容器的对象：ApplicationContext（加载 spring 配置文件）
        // ClassPathXmlApplicationContext：表示从类路径（target/classes）中加载 spring 的配置文件
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 2、从容器中获取某个对象
        // getBean("配置文件中 bean 的id值")
        User user = context.getBean("user", User.class);
        System.out.println(user);
        user.add();
    }
    // 获取容器中对象信息的 api
     @Test
    public void testUser1(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 获取容器中定义的对象的数量
        int num = context.getBeanDefinitionCount();
        System.out.println(num);
        // 获取容器中定义的每个对象的名称
        String[] names = context.getBeanDefinitionNames();
        System.out.println(Arrays.toString(names));
    }
}
```
### 2.4 基于 XML 的 DI
*  基于 XML 的 DI：通过配置文件完成 java 对象的创建、属性赋值。
* di：依赖注入，表示创建对象，给属性赋值。
* **di 的实现有两种：**
（1）在 spring 的配置文件中，使用标签和属性完成，叫做基于 XML 的 di 实现。
（2）使用 spring 中的注解，完成属性赋值，叫做基于注解的 di 实现。
* **di 的语法分类：**
（1）set 注入（设值注入）：spring 调用类中的 set 方法，在 set 方法中可以实现属性的赋值。
（2）构造注入：spring 调用类的有参构造方法，创建对象。
（3）注入：就是赋值的意思。
#### 下面示例中用到的两个 JavaBean 类
* Student.java
```java
package com.atjingbo.spring;
public class Student {
    private String name;
    private Integer age;
    private School school;

    public Student(){}

    public Student(String name, Integer age, School school) {
        this.name = name;
        this.age = age;
        this.school = school;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public School getSchool() {
        return school;
    }

    public void setSchool(School school) {
        this.school = school;
    }

    @Override
    public String toString() {
        return "Student{" + "name='" + name + '\'' + ", age=" + age + ", school=" + school + '}';
    }
}
```
* School.java
```java
package com.atjingbo.spring;
public class School {
    private String name;
    private String address;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "School{" + "name='" + name + '\'' + ", address='" + address + '\'' + '}';
    }
}
```
#### 2.4.1 简单类型的 set 注入
```xml
set（设值）注入：spring 调用类的 set 方法，你可以在 set 方法中完成属性赋值。

语法格式：
    <bean id="对象的自定义名称" class="类的全限定名称" >
        <property name="属性名称" value="赋给属性的值" />
        一个 property 只能给一个属性赋值。
    </bean>
```
示例：
```xml
<!-- 声明 Student 对象。一个 bean 标签声明一个对象。
      注入：就是赋值的意思。
      简单类型：spring 中规定 java 的基本数据类型和 String 都是简单类型。
      di：给属性赋值。
      set（设值）注入：spring 调用类的 set 方法，你可以在 set 方法中完成属性赋值。
-->
<bean id="mystudent" class="com.atjingbo.spring.Student" >
    <property name="name" value="张三" /><!-- setName("张三") -->
    <property name="age" value="23" /><!-- setAge(23) -->
</bean>
```
```java
public class TestStudent {
    @Test
    public void test01(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        Student student = (Student)context.getBean("myStudent");
        System.out.println(student);
    }
}
```
#### 2.4.2 引用类型的 set 注入
```xml
set（设值）注入：spring 调用类的 set 方法，你可以在 set 方法中完成属性赋值。

语法格式：
    <bean id="对象的自定义名称" class="类的全限定名称" >
        <property name="属性名称" ref="bean 的 id" />
    </bean>
```
示例：
```xml
<!-- 一个 bean 标签声明一个对象。 -->
<bean id="myStudent" class="com.atjingbo.spring.Student" >
    <property name="name" value="张三" /><!-- setName("张三") -->
    <property name="age" value="23" /><!-- setAge(23) -->
    <!-- 引用类型 -->
    <property name="school" ref="mySchool" /> <!-- setSchool(mySchool) -->
</bean>
<!-- 声明 School 对象 -->
<bean id="mySchool" class="com.atjingbo.spring.School">
    <property name="name" value="哈佛" />
    <property name="address" value="US" />
</bean>
```
```java
public class TestStudent {
    @Test
    public void test02(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        Student student = (Student)context.getBean("myStudent");
        System.out.println(student);
    }
}
```
#### 2.4.3 构造注入
```xml
构造注入：spring 调用类的有参构造方法，在创建对象的同时，在构造方法中给属性赋值。

构造注入使用 <constructor-arg> 标签：
    <constructor-arg> 标签：一个 <constructor-arg> 标签表示构造方法的一个参数。
    <constructor-arg> 标签的属性：
        name：表示构造方法的形参名。
        index：表示构造方法的参数的位置，参数从左往右位置是：0，1，2。
        value：构造方法的形参类型是简单类型的，使用 value。
        ref：构造方法的形参类型是引用类型的，使用 ref。
```
示例：
```xml
<!-- 声明 School 对象。一个 bean 标签声明一个对象。 -->
<bean id="mySchool" class="com.atjingbo.spring.School">
    <property name="name" value="哈佛" />
    <property name="address" value="US" />
</bean>

<!-- 使用 name 属性 -->
<bean id="myStudent" class="com.atjingbo.spring.Student" >
    <constructor-arg name="name" value="张三"></constructor-arg>
    <constructor-arg name="age" value="28"></constructor-arg>
    <constructor-arg name="school" ref="mySchool"></constructor-arg>
</bean>
<!-- 使用 index 属性-->
<bean id="myStudent1" class="com.atjingbo.spring.Student" >
    <constructor-arg index="0" value="张三"></constructor-arg>
    <constructor-arg index="1" value="28"></constructor-arg>
    <constructor-arg index="2" ref="mySchool"></constructor-arg>
</bean>
<!-- 省略 index 属性，但此时位置就不能随便写了 -->
<bean id="myStudent2" class="com.atjingbo.spring.Student" >
    <constructor-arg value="张三"></constructor-arg>
    <constructor-arg value="28"></constructor-arg>
    <constructor-arg ref="mySchool"></constructor-arg>
</bean>
```
#### 2.4.3 构造注入创建文件对象
```xml
<!-- 创建 File，使用构造注入-->
<bean id="myFile" class="java.io.File">
    <constructor-arg name="parent" value="D:\" />
    <constructor-arg name="child" value="test.txt" />
</bean>
```
#### 2.4.4 自动注入
* 自动注入（只用于引用类型）：spring 框架根据某些规则可以给引用类型赋值，不用你再给引用类型赋值了。
* 自动注入使用的规则常用的是 byName 和 byType。
（1）**byName**：按名称注入。java 类中引用类型的属性名和 spring 配置文件中的 id 名一样，并且数据类型是一致的，这样的 bean，spring 能够赋值给引用类型。
```java
语法：
    <bean id="对象的自定义名称" class="类的全限定名称" autowire="byName">
        简单类型的属性赋值
    </bean>
```
**示例：**
```xml
<!-- 声明 School 对象。一个 bean 标签声明一个对象。 -->
<bean id="school" class="com.atjingbo.spring.School">
    <property name="name" value="哈佛" />
    <property name="address" value="US" />
</bean>
<!-- byName：我们要把 Student 类中所有引用类型按照 byName 规则让 spring 完成赋值。
     规则：本例中，bean 标签的 id 值需要和 Student 类中的引用类型的属性名一致（都是 school），并且它俩的数据类型也是一致的（都是 School）
-->
<bean id="myStudent" class="com.atjingbo.spring.Student" autowire="byName">
    <property name="name" value="张三" />
    <property name="age" value="28" />
</bean>
```
（2）**byType**：按类型注入。java 类中引用类型的数据类型和 spring 配置文件中 \<bean> 的 class 属性值是同源关系的，这样的 bean 能够赋值给引用类型。
**同源关系：**
&nbsp;&nbsp;&nbsp;&nbsp;1、java 类中引用类型的数据类型和 bean 的 class 的值是一样的。
&nbsp;&nbsp;&nbsp;&nbsp;2、java 类中引用类型的数据类型和 bean 的 class 的值是父类和子类的关系。
&nbsp;&nbsp;&nbsp;&nbsp;3、java 类中引用类型的数据类型和 bean 的 class 的值是接口和实现类的关系。
```xml
语法：
    <bean id="对象的自定义名称" class="类的全限定名称" autowire="byType">
        简单类型的属性赋值
    </bean>
注：在 byType 中，在 xml 配置文件中声明 bean 的时候，只能有一个符合条件，多于一个是错误的。
```
示例：
```xml
<!-- 声明 School 对象。一个 bean 标签声明一个对象。 -->
<bean id="mySchool" class="com.atjingbo.spring.School">
    <property name="name" value="哈佛" />
    <property name="address" value="US" />
</bean>
        
<bean id="myStudent" class="com.atjingbo.spring.Student" autowire="byType">
    <property name="name" value="张三" />
    <property name="age" value="28" />
</bean>
```
### 2.5 多配置文件
* 多配置文件的优势：
（1）每个文件的大小比一个文件要小很多，效率高。
（2）避免多人竞争带来的冲突。
* 多个文件的分配方式：
（1）按功能模块，一个模块一个配置文件。
（2）按类的功能，做数据库相关功能的一个配置文件，做事务功能的一个配置文件，做 service 功能的一个配置文件。
* **示例：**
spring-student.xml 配置文件：
```xml
<!-- student 模块的所有 bean 的声明 -->
<bean id="myStudent" class="com.atjingbo.spring.Student" autowire="byType">
    <property name="name" value="张三" />
    <property name="age" value="28" />
</bean>
```
spring-school.xml 配置文件：
```xml
<!-- school 模块的所有 bean 的声明 -->
<bean id="mySchool" class="com.atjingbo.spring.School">
    <property name="name" value="哈佛" />
    <property name="address" value="US" />
</bean>
```
total.xml 配置文件：
```xml
<!--
    包含关系的配置文件：
        total 表示主配置文件：用来包含其他配置文件的。主配置文件一般不定义对象。
        语法：<import resource="其他配置文件的路径" />
        关键字："classpath:" 表示类路径（class 字节码文件所在目录）
        在 spring 的配置文件中要指定其他文件的位置，需要使用 classpath，告诉 spring 到哪去加字读取文件。
-->
<import resource="classpath:bao/spring-school.xml" />
<import resource="classpath:bao/spring-student.xml" />

<!-- 
     在包含关系的配置文件中，可以使用通配符（*：表示任意字符）
     注：主配置文件的名称不能包含在通配符的范围内（不能叫做 spring-total.xml）。
     同时如果要使用通配符的话，配置文件必须要放到一个目录中，比如本例中都放到了 bao 这个目录中。
-->
<import resource="classpath:bao/spring-*.xml" />
```
### 2.6 基于注解的 DI
* 基于注解的 DI：通过注解完成 java 对象的创建、属性赋值。
* 使用注解的步骤：
（1）加入 maven 的依赖：spring-context，在你加入 spring-context 的同时，简介加入了 spring-aop 的依赖。使用注解必须使用 spring-aop 依赖。
（2）在类中加入 spring 的注解。
（3）在 spring 的配置文件中，加入一个组件扫描器的标签，说明注解在你的项目中的位置。
* 要学习的注解：@Component、@Repository、@Service、@Controller、@Value、@Autowired、@Qualifier、@Resource。
#### 2.6.1 @Component
**@Component：是用来创建对象的。**

**（1）加入依赖。**

**（2）创建一个普通类，在类中加入注解**
```java
/*
* @Component：是用来创建对象的，等同于 <bean> 的功能。
*     属性：value 就是对象的名称，也就是 bean 的 id 值。
*           value 的值是唯一的，创建的对象在整个 spring 容器中就一个
*     位置：在类的上面。
* @Component(value = "myStudent") 等同于 <bean id="myStudent" class="com.atjingbo.spring.Student" />
* */

// @Component(value = "myStudent") 使用 value 属性，指定对象的名称。
// @Component 不指定对象的名称，由 spring 提供默认名称：类名的首字母小写。
@Component("myStudent") // 省略 value
public class Student {
    private String name;
    private Integer age;
    public void setName(String name) { this.name = name; }
    public void setAge(Integer age) { this.age = age; }
    @Override
    public String toString() {
        return "Student{" + "name='" + name + '\'' + ", age=" + age + '}';
    }
}
```
**（3）创建配置文件。声明组件扫描器的标签，指定注解在你的项目中的位置。**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 声明组件扫描器（component-scan），组件就是 java 对象。
         base-package：指定注解在你的项目中的包名。
         component-scan 的工作方式：spring 会扫描遍历 base-package 指定的包，
                                   把包中和子包中所有的类，找到类中的注解，按照注解的功能创建对象或给属性赋值。
    -->
    <context:component-scan base-package="com.atjingbo.spring" />
</beans>
```
**（4）使用注解创建对象，创建容器 ApplicationContext。**
```java
public class MyTest {
    @Test
    public void test1(){
        // 创建表示 spring 容器的对象：ApplicationContext（加载 spring 配置文件）
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 从容器中获取对象
        Student student = (Student) context.getBean("myStudent");
        System.out.println(student);
    }
}
```
#### 2.6.2 @Repository、@Service、@Controller
* spring 中和 @Component 功能一致，创建对象的注解还有：
 （1）@Repository（用在持久层类的上面）：放在 dao 的实现类上面。表示创建 dao 对象，dao 对象是能访问数据库的。
  （2）@Service（用在业务层类的上面）：放在 service 的实现类上面。表示创建 service 对象，service 对象是做业务处理，可以有功能等功能。
  （3）@Controller（用在控制器的上面）：放在控制器（处理器）类的上面。表示创建控制器对象，控制器对象能够接受用户提交的参数，显示请求的处理结果。
* 以上三个注解的使用语法和 @Component 一样，都能创建对象，但是这三个注解还有额外的功能： @Repository、@Service、@Controller 是给项目的对象分层的，可以给不同的类赋予不同的角色。
* 什么时候使用 @Component：当 @Repository、@Service、@Controller 不能用的时候，使用 @Component。
#### 2.6.3 @Value（给简单类型赋值）
@Value 是用来给简单类型赋值的。
```java
// @Component：是用来创建对象的，等同于 <bean> 的功能。属性：value 就是对象的名称，也就是 bean 的 id 值。value 的值是唯一的，创建的对象在整个 spring 容器中就一个。位置：在类的上面。@Component(value = "myStudent") 等同于 <bean id="myStudent" class="com.atjingbo.spring.Student" />
@Component("myStudent")
public class Student {
    /*
     * @Value：简单类型的属性赋值
     *   属性：value 是 String 类型的，表示简单类型的属性值。
     *   位置：1、在属性定义的上面，无需 set 方法（推荐使用）
     *        2、在 set 方法的上面，会调用 set 方法。
     * */
    @Value(value="张三")
    private String name;
    @Value(value="29")
    private Integer age;
    
    public void setName(String name) { this.name = name; }
    public void setAge(Integer age) { this.age = age; }
    @Override
    public String toString() {
        return "Student{" + "name='" + name + '\'' + ", age=" + age + '}';
    }
}
```
### 2.7 @Autowired 与 @Qualifier（给引用类型赋值）

#### 下面示例中会用到的 School 类：
```java
package com.atjingbo.spring;
// @Component：是用来创建对象的，等同于 <bean> 的功能。属性：value 就是对象的名称，也就是 bean 的 id 值。value 的值是唯一的，创建的对象在整个 spring 容器中就一个。位置：在类的上面。@Component(value = "myStudent") 等同于 <bean id="myStudent" class="com.atjingbo.spring.Student" />
@Component("mySchool")
public class School {
    @Value("hf")
    private String name;
    @Value("USA")
    private String address;

    public String getName() { return name; }

    public void setName(String name) { this.name = name; }

    public String getAddress() { return address; }

    public void setAddress(String address) { this.address = address; }

    @Override
    public String toString() { return "School{" + "name='" + name + '\'' + ", address='" + address + '\'' + '}';
    }
}
```

**byType 自动注入（@Autowired 默认使用的）：**
```java
@Component("myStudent")
public class Student {
    /*
    * @Autowired：spring 框架提供的注解，实现引用类型的赋值。
    * spring 中通过注解给引用类型赋值，使用的是自动注入原理，支持 byName 和 byType。
    * @Autowired：默认使用的是 byType 自动注入。
    * 属性：required：是一个 boolean 类先锋的，默认是 true
    *     required=true：表示引用类型赋值失败的时候，程序报错，并终止执行。
    *     required=false：表示引用类型赋值失败的时候，程序正常执行，引用类型是 null。
    * 位置：1）在属性定义的上面，无需 set 方法（推荐使用）
    *      2）在 set 方法的上面，会调用 set 方法
    * */
    @Autowired
    private School school;
    public void setSchool(School school) {
        this.school = school;
    }
}
```
**byName 自动注入：**
```java
@Component("myStudent")
public class Student {
    /*
    * 如果要使用 byName 的方式：
    * （1）在属性上面加入 @Autowired
    * （2）在属性上面加入 @Qualifier(value="bean的id")：表示使用指定名称的 bean 完成赋值。
    * */
    @Autowired
    @Qualifier("mySchool")
    private School school;
    public void setSchool(School school) {
        this.school = school;
    }
}
```
### 2.8 @Resource（给引用类型赋值）
**默认使用 byName：先使用 byName 自动注入，如果 byName 赋值失败，再使用 byType：**
```java
@Component("myStudent")
public class Student {
    /*
    * @Resource：来自 jdk 中的注解，spring 框架提供了对这个注解的功能支持，可以使用它给引用类型赋值。
    *            使用的也是自动注入的原理，支持 byName、byType。
    * 位置：1）在属性定义的上面，无需 set 方法（推荐使用）
    *      2）在 set 方法的上面，会调用 set 方法
    * 默认使用 byName：先使用 byName 自动注入，如果 byName 赋值失败，再使用 byType。
    * */
    @Resource
    private School school;
    public void setSchool(School school) {
        this.school = school;
    }
}
```
**只使用 byName：**
```java
@Component("myStudent")
public class Student {
    /*
    * @Resource 只使用 byName 的方式：需要增加一个属性 name。
    *    name="bean的id(名称)"
    * */
    @Resource(name = "mySchool")
    private School school;
    public void setSchool(School school) {
        this.school = school;
    }
}
```
### 2.9 扫描多个包的方式
```xml
<!-- 指定多个包的三种方式 -->
<!-- 第一种：使用多次组件扫描器，指定不同的包 -->
<context:component-scan base-package="com.atjingbo.spring1" />
<context:component-scan base-package="com.atjingbo.spring2" />
<!-- 第二种：使用分隔符（; 或 ,）分隔多个包名 -->
<context:component-scan base-package="com.atjingbo.spring1;com.atjingbo.spring2" />
<!-- 第三种：指定父包 -->
<context:component-scan base-package="com.atjingbo" />
```
### 2.10 注解与 XML 的对比
* **不经常改的使用注解，经常改的使用配置文件。**
* 注解
（1）优点：1、使用快捷和方便。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2、在浏览代码的时候，就能知道类的信息，可读性好。
（2）缺点：注解是嵌在代码里的，对于经常要改动的代码使用不方便，对代码具有”侵入性“。
* 配置文件
（1）优点：代码和值是分开的，对于经常要改动的值放到配置文件中是很有效的。
（2）缺点：1、使用配置文件的话，代码量大，用起来费时费力，效率低。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2、因为代码和值是分开的，在浏览代码的时候并不知道它的值是多少，不够直观、清晰。
## 三、AOP 面向切面编程
### 3.1 动态代理
* 动态代理是指：**程序在整个运行过程中根本就不存在目标类的代理类**，目标对象的代理对象只是由代理生成工具（不是真实定义的类）在程序运行时由 JVM 根据反射等机制动态生成的。代理对象与目标对象代理关系在程序运行时才确立。
* **动态代理的作用：**
（1）在目标类源代码不改变的情况下，增加功能。
（2）减少代码的重复。
（3）专注业务逻辑代码。
（4）**解耦合。让你的业务功能和日志、事务等非业务功能分离**。
#### 3.1.1 jdk 动态代理
* **jdk 的动态代理要求目标对象必须实现接口**，这是 java 设计上的要求。
* java 语言通过 java.lang.reflect 包提供的三个类支持代理模式：Proxy、Method 和 InvocationHandler。
#### 3.1.2 CGLIB 动态代理
CGLIB 是一个开源项目。**CGLIB 代理的生成原理是生成目标类的子类，而子类是增强过的，这个子类对象就是代理对象。所以，使用 CGLIB 生成动态代理，要求目标类必须能够被继承，即不能是 final 的类。**
#### 3.1.3 动态代理的实现
jdk 动态代理实现步骤：
（1）创建目标类：SomeServiceImpl。需要给它的 doSome、doOther 增加输出时间、提交事务的功能。
（2）创建 InvocationHandler 接口的实现类，在这个类中实现给目标方法增加功能。
（3）使用 jdk 中的类 Proxy，创建代理对象，实现创建对象的能力。

**创建目标类：**
```java
public class SomeServiceImpl implements SomeService {
    @Override
    public void doSome() {
        System.out.println("执行业务方法doSome");
    }

    @Override
    public void doOther() {
        System.out.println("执行业务方法doOther");
    }
}
```
**创建 InvocationHandler 接口的实现类：**
```java
public class MyInvocationHandler implements InvocationHandler {
    private Object target;// SomeServiceImpl 对象

    public MyInvocationHandler(Object target){
        this.target = target;
    }

    // 通过代理对象执行方法时，会调用执行这个 invoke() 方法
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("非业务方法，方法执行的时间：" + new Date());// 新增的功能

        // 执行目标类的方法，通过 Method 类实现
        Object res = method.invoke(target, args);// 执行的是 SomeServiceImpl.doSome()、doOther()

        System.out.println("方法执行完毕后，提交事务");// 新增的功能
        return res;
    }
}
```
**使用 jdk 中的类 Proxy，创建代理对象：**
```java
public class MyTest {
    @Test
    public void test1(){
        // 创建目标对象
        SomeService target = new SomeServiceImpl();

        // 创建 InvocationHandler 对象
        InvocationHandler handler = new MyInvocationHandler(target);

        // 使用 Proxy 创建代理
        SomeService proxy = (SomeService) Proxy.newProxyInstance(
                target.getClass().getClassLoader(),
                target.getClass().getInterfaces(), handler
        );
        // 通过代理执行方法，底层会调用 handler 中的 invoke() 方法
        proxy.doSome();
    }
}
```
### 3.2 AOP 基础知识
* AOP（Aspect Oriented Programming）：**面向切面编程，它的底层就是基于动态代理的**。可以使用 jdk 和 cglib 两种代理方式。
* **AOP 就是动态代理的规范化**，它把动态代理的实现步骤、方式都定义好了，让开发人员用一种统一的方式去使用动态代理。
* **作用：在不改变原来代码的前提下，给已存在的类和方法提供额外的功能。**
* 怎么理解面向切面编程？
（1）需要在分析项目功能的时候，找出切面。
（2）合理地安排切面的执行时间（在目标方法前，还是在目标方法后）。
（3）合理地安排切面执行的位置（是在哪个类、哪个方法增加功能）。
* 术语：
（1）Aspect：**切面，就是给你的目标类新增加的功能**。切面的特点：**一般都是非业务方法**，可以独立使用的。常见的切面功能有：日志、事务、统计信息、参数检查、权限验证。
（2）JoinPoint：连接点，指连接业务方法和切面的位置，其实就是要加入切面功能的业务方法。
（3）Pointcut：切入点，指多个连接点方法的集合，其实就是多个业务方法。
（4）目标对象：给哪个类的方法增加功能，这个类就是目标对象。
（5）Advice：**通知，表示切面功能的执行时间。**
* **切面的三个关键要素：**
（1）切面的功能代码（切面用来干什么）
（2）切面的执行时间（使用 Advice 来表示时间，在目标方法之前还是之后）
（3）切面的执行位置（使用 Pointcut 表示切面执行的位置）
* **什么时候使用 AOP 技术：**
（1）当你要给一个功能不完善的类修改功能，而你又没有这个类的源代码时，需要使用 AOP 增加功能。
（2）当你要给项目中的多个类增加一个相同的功能，使用 AOP。
（3）给业务方法增加事务、日志输出等功能，使用 AOP。
* AOP 的技术实现框架：
（1）spring：spring 在内部实现了 AOP 规范，可以做 AOP 的工作。spring 主要在事务处理的时候使用 AOP。我们项目开发中很少使用 spring 的 AOP 实现，因为 spring 的 AOP 比较笨重。
（2）aspectJ：一个开源的专门做 AOP 的框架，**spring 框架中集成了 aspectJ 框架**，通过 spring 就能使用 aspectJ 的功能。
### 3.3 aspectJ 框架的使用
* **aspectJ 框架实现 AOP 有两种方式：**
（1）使用 xml 配置文件。（主要用于事务）
（2）使用注解。（在项目中一般使用注解做 AOP 的功能。aspectJ 有5个注解）
* **Advice（通知、增强）**：切面执行的时间，在 aspectJ 中使用注解表示：**@Before、@AfterReturning、@Around、@AfterThrowing、@After**。也可以使用 xml 配置文件中的标签。
* **表示切面执行的目标方法的位置，使用的是切入点表达式。**
##### 3.3.1 aspectJ 的切入点表达式
**切入点表达式是用来表示切面执行的目标方法的位置的。**
![图示](https://img-blog.csdnimg.cn/37716b5ef3984b87b7bfae854d9afd81.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
#### 3.3.2 常用的通知注解
* **通知方法：用通知注解修饰的方法，叫做通知方法**。
* @Aspect：表示当前类是切面类。
* @Before：前置通知注解，表示在目标方法之前先执行切面的功能。
* @AfterReturning：后置通知注解，表示在目标方法之后执行切面的功能。
* @Around：环绕通知注解，可以在目标方法之前或之后执行切面的功能。
* @AfterThrowing：异常通知注解，在目标方法抛出异常时执行。
* @After：最终通知注解，在目标方法之后执行。
#### 3.3.3 使用 aspectJ 实现 AOP 的基本步骤（@Before 前置通知举例的）
1. 新建 maven 项目
2. 加入依赖（spring 依赖、aspectJ 依赖等）
3. 创建目标类：接口和它的实现类。需要给类中的方法增加功能。
4. 创建切面类：普通类
（1）在类的上面加入 @Aspect。
（2）在类中定义方法，方法就是切面要执行的功能代码。在方法的上面加入 aspectJ 中的通知注解，比如：@Before。
（3）指定切入点表达式 execution()
5. 创建 spring 的配置文件：声明对象，把对象交给容器统一管理。
（1）声明目标对象。
（2）声明切面类对象。
（3）声明 aspectJ 框架中的自动代理生成器标签。**自动代理生成器：是用来完成代理对象的自动创建功能的。**
6. 创建测试类，从 spring 容器中获取目标对象（实际上是代理对象）。通过代理执行方法，实现 AOP 的功能增强。

**创建目标类：**
```java
// 目标类
public class SomeServiceImpl implements SomeService {
    // 给 doSome() 方法增加一个功能：在 doSome() 方法执行之前，输出方法的执行时间
    @Override
    public void doSome(String name, Integer age) {
        System.out.println("目标方法doSome()");
    }
}
```
**创建切面类：**
```java
/*
 * @Aspect：是 aspectJ 框架中的注解。
 *     作用：表示当前类是切面类。
 *     切面类：是用来给业务方法增加功能的类，在这个类中有切面的功能代码。
 *     位置：在类定义的上面
 * */
@Aspect
public class MyAspect {
    /*
    * 前置通知定义方法，该方法是实现切面功能的。
    * 方法的定义的要求：
    *     （1）公共方法 public。
    *     （2）方法没有返回值。
    *     （3）方法名称自定义。
    *     （4）方法可以有参数，也可以没有参数。如果有参数，参数不能是自定义的，必须是 JoinPoint。
    * */

    /*
    * @Before：前置通知注解，在目标方法之前先执行切面的功能。
    *     属性：value，值是切入点表达式，来表示切面功能执行的目标方法的位置。
    *     位置：方法的上面。
    * 特点：
    *     （1）在目标方法之前执行。
    *     （2）不会影响目标方法的执行。
    * */
    @Before(value = "execution(public void com.atjingbo.service.impl.SomeServiceImpl.doSome(String,Integer))")
    public void myBefore(){
        // 切面要执行的功能代码
        System.out.println("切面功能：在目标方法之前输出执行时间：" + new Date());
    }
}
```
**创建 spring 的配置文件：**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">


    <!-- 把对象交给 spring 容器，由 spring 容器统一创建、管理对象 -->
    <!-- 声明目标对象 -->
    <bean id="someService" class="com.atjingbo.service.impl.SomeServiceImpl"/>
    <!-- 声明切面类对象 -->
    <bean id="myAspect" class="com.atjingbo.service.MyAspect" />

    <!-- 声明自动代理生成器：使用 aspectJ 框架内部的功能，创建目标对象的代理对象。
         创建代理对象是在内存中实现的，修改目标对象的内存中的结构，创建为代理对象。
         所以代理对象就是被修改后的目标对象。

         aspectj-autoproxy：会扫描容器中的对象，然后会去找 aspectJ 框架里面的注解，创建代理对象。
    -->
    <aop:aspectj-autoproxy />
</beans>
```
**创建测试类：**
```java
public class MyTest {
    @Test
    public void test1(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 从容器中获取目标对象
        SomeService proxy = (SomeService) context.getBean("someService");
        // 通过代理对象执行方法，实现目标方法的执行，增强了功能。
        proxy.doSome("张三", 20);
    }
}
```
#### 3.3.4 通知方法的参数：JoinPoint
* 通知方法：**用通知注解修饰的方法，叫做通知方法**。
* JoinPoint 在所有通知方法中都可以使用。
```java
@Aspect
public class MyAspect {
    /*
    * 指定通知方法中的参数：JoinPoint
    * JoinPoint：要加入切面功能的业务方法。
    *     作用：可以在通知方法中获取目标方法执行时的信息，例如方法名称、方法的实参。
    *     如果你的切面功能需要用到目标方法的信息，就加入 JoinPoint。
    * JoinPoint 参数的值是由框架赋予的，必须是第一个位置的参数。
    * */
    @Before(value = "execution(public void com.atjingbo.service.impl.SomeServiceImpl.doSome(String,Integer))")
    public void myBefore(JoinPoint joinPoint){
        // 输出方法的签名（完整定义）
        System.out.println(joinPoint.getSignature());
        // 切面要执行的功能代码
        System.out.println("切面功能：在目标方法之前输出执行时间：" + new Date());
    }
}
```
#### 3.3.5 @AfterReturning 后置通知
```java
// 目标类
public class SomeServiceImpl implements SomeService {
    @Override
    public String doOther(String name, Integer age) {
        System.out.println("目标方法doOther()");
        return "abcd";
    }
}
```
```java
@Aspect
public class MyAspect {
    /*
     * 后置通知定义方法，该方法是实现切面功能的。
     * 方法的定义的要求：
     *     （1）公共方法 public。
     *     （2）方法没有返回值。
     *     （3）方法名称自定义。
     *     （4）方法有参数，推荐是 Object，参数名自定义。
     * */

    /*
     * @AfterReturning：后置通知注解，在目标方法之后执行切面的功能。
     *     属性：1、value：值是切入点表达式，表示切面的功能执行的位置。
     *          2、returning：自定义的变量，表示目标方法的返回值。自定义变量名必须和通知方法的形参名一致。
     *     位置：方法的上面。
     * 特点：
     *     （1）在目标方法之后执行。
     *     （2）能够获取到目标方法的返回值，可以根据这个返回值做不同的处理功能。
     *         Object res = doOther();
     *     （3）可以修改这个返回值。
     * 后置通知的执行：
     *     （1）先执行目标方法 doOther()：Object res = doOther();
     *     （2）再执行通知方法：myAfterReturning(res)
     * */
    @AfterReturning(value = "execution(public String com.atjingbo.service.impl.SomeServiceImpl.doOther(String,Integer))", returning = "res")
    public void myAfterReturning(Object res){
        // Object res：是目标方法执行后的返回值，可以根据返回值做你的切面的功能处理。
        System.out.println("后置通知：在目标方法之后执行的返回值：" + res);
    }
}
```
```java
public class MyTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 从容器中获取目标对象
        SomeService proxy = (SomeService) context.getBean("someService");
        // 通过代理对象执行方法，实现目标方法的执行，增强了功能。
        String str= proxy.doOther("张三", 20);
        System.out.println(str);// str 的值是 doOther() 的返回值。
    }
```
#### 3.3.6 @Around 环绕通知
```java
// 目标类
public class SomeServiceImpl implements SomeService {
    @Override
    public String doFirst(String name, Integer age) {
        System.out.println("目标方法doFirst()");
        return "doFirst";
    }
}
```
```java
@Aspect
public class MyAspect {
    /*
     * 环绕通知定义方法，该方法是实现切面功能的。
     * 方法的定义的要求：
     *     （1）公共方法 public。
     *     （2）必须有一个返回值，推荐使用 Object。
     *     （3）方法名称自定义。
     *     （4）方法有参数，固定的参数 ProceedingJoinPoint。
     * */

    /*
     * @Around：环绕通知注解，功能最强的通知。
     *     属性：1、value：值是切入点表达式，表示切面的功能执行的位置。
     *     位置：方法定义的上面。
     * 特点：
     *     （1）在目标方法的之前和之后都能增强功能。
     *     （2）控制目标方法是否被调用执行。
     *     （3）修改原来的目标方法的执行结果。影响最后的调用结果。
     * 环绕通知：经常用来做事务，在目标方法前开启事务，执行目标方法，在目标方法之后提交事务。
     *
     * 环绕通知等同于 jdk 动态代理的 InvocationHandler 接口。
     *
     * 参数 ProceedingJoinPoint 等同于 Method。作用是：执行目标方法。
     *
     * 返回值：就是目标方法的返回值，可以被修改。
     * */
    @Around(value = "execution(public String com.atjingbo.service.impl.SomeServiceImpl.doFirst(String,Integer))")
    public Object myAround(ProceedingJoinPoint pjp) throws Throwable {
        System.out.println("环绕通知：在目标方法前输出时间");
        // 1、目标方法的调用
        Object res = pjp.proceed();
        System.out.println("代理方法：" + res);
        // 2、在目标方法的前或后加入功能。
        System.out.println("环绕通知：在目标方法后提交事务");
        // 3、修改目标方法的执行结果。影响最后的调用结果。
        res = "hello";
        // 返回目标方法的执行结果
        return res;
    }
}
```
```java
public class MyTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        SomeService proxy = (SomeService) context.getBean("someService");
        // 通过代理对象执行方法，实现目标方法的执行，增强了功能。
        Object str = proxy.doFirst("张三", 20);// 实际上执行的是 myAround() 方法
        System.out.println(str);// str 的值实际上是 myAround() 方法的返回值
    }
}
```
#### 3.3.7 @AfterThrowing 异常通知（了解）
```java
// 目标类
public class SomeServiceImpl implements SomeService {
    @Override
    public void doSecond() {
        System.out.println("目标方法doSecond()" + (10 / 0));
    }
}
```
```java
@Aspect
public class MyAspect {
   /*
     * 异常通知定义方法，该方法是实现切面功能的。
     * 方法的定义的要求：
     *     （1）公共方法 public。
     *     （2）方法没有返回值。
     *     （3）方法名称自定义。
     *     （4）方法有一个参数 Exception，如果还有参数，那么就是 JoinPoint。
     * */

    /*
     * @AfterThrowing：异常通知注解
     *     属性：1、value，值是切入点表达式，表示切面的功能执行的位置。
     *          2、throwing：自定义的变量，表示目标方法抛出的异常对象。
     *                       值必须和方法的参数名一样。
     *     位置：方法的上面。
     * 特点：
     *     （1）在目标方法抛出异常时执行。
     *     （2）可以做异常的监控程序，监控目标方法执行时是不是有异常。
     *         如果有异常，可以发送邮件、短信等进行通知。
     * 执行就是：
     *     try{
     *         SomeServiceImpl.doSecond()
     *     } catch(Exception e){
     *         myAfterThrowing(e)
     *     }
     * */
    @AfterThrowing(value = "execution(public void com.atjingbo.service.impl.SomeServiceImpl.doSecond())", throwing="e")
    public void myAfterThrowing(Exception e){
        // 切面要执行的功能代码
        System.out.println("异常通知：在目标方法抛出异常时执行：" + e.getMessage());
    }
```
#### 3.3.8 @After 最终通知（了解）
```java
// 目标类
public class SomeServiceImpl implements SomeService {
    @Override
    public void doThird() {
        System.out.println("目标方法doThird()" + (10 / 0));
    }
}
```
```java
@Aspect
public class MyAspect {
   /*
     * 最终通知定义方法，该方法是实现切面功能的。
     * 方法的定义的要求：
     *     （1）公共方法 public。
     *     （2）方法没有返回值。
     *     （3）方法名称自定义。
     *     （4）方法没有参数，如果有的话就是 JoinPoint。
     * */

    /*
     * @After：最终通知注解
     *     属性：value，值是切入点表达式，表示切面的功能执行的位置。
     *     位置：方法的上面。
     * 特点：
     *     （1）总是会执行。
     *     （2）在目标方法之后执行。
     * 一般用来做资源清除工作。
     *
     * 执行就是：
     *     try{
     *         SomeServiceImpl.doThird()
     *     } catch(Exception e){
     *
     *     } finally{
     *         myAfter()
     *     }
     * */
    @After(value = "execution(public void com.atjingbo.service.impl.SomeServiceImpl.doThird())")
    public void myAfter(){
        // 切面要执行的功能代码
        System.out.println("执行最终通知，总是会被执行的代码");
    }
```
#### 3.3.9 @Pointcut 注解
```java
@Aspect
public class MyAspect {
    @After(value = "myPt()")
    public void myAfter(){
        // 切面要执行的功能代码
        System.out.println("执行最终通知，总是会被执行的代码");
    }

    /*
    * @Pointcut：定义和管理切入点。如果你的项目中有多个切入点表达式是相同的，那么可以使用 @Pointcut。
    *     属性：value，切入点表达式。
    *     位置：在自定义的方法的上面
    * 特点：
    *     当使用 @Pointcut 定义在一个方法的上面，此时这个方法的名称就是切入点表达式的别名。
    *     其他的通知中，value 的属性值就可以使用这个方法的名称，代替切入点表达式了。
    * */
    @Pointcut(value = "execution(public void com.atjingbo.service.impl.SomeServiceImpl.doThird())")
    private void myPt(){
        // 无需代码
    }
}
```
### 3.4 cblib 代理的使用
* 目标类没有接口，使用的是 cglib 动态代理，spring 框架会自动应用 cglib。
* 目标类有接口，也能使用 cblib 代理，因为**只要目标类可以被继承就可以使用 cglib 代理**。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">
    <!-- 目标类有接口，使用 cglib 代理
         proxy-target-class="true"：告诉框架，要使用 cglib 动态代理
    -->
    <aop:aspectj-autoproxy proxy-target-class="true" />
</beans>
```
## 四、Spring 集成 MyBatis
* 使用的技术：IOC。将 mybatis 和 spring 集成在一起，像一个框架一样。
* 为什么使用 IOC：因为 IOC 能创建对象，可以把 mybatis 框架中的对象交给 spring 统一创建，我们只需要从 spring 中获取对象即可。
* 我们需要让 spring 创建以下对象：
（1）**数据源 DataSource**（用来连接数据库），使用 druid 连接池
（2）**SqlSessionFactory 对象**。使用 SqlSessionFactoryBean 在内部创建的 SqlSessionFactory。
（3）**dao 对象**。使用的 MapperScannerConfigurer，在这个类的内部会调用 getMapper() 方法去创建接口的 dao 对象。
* 实现步骤
（1）新建 maven 项目。
（2）加入 maven 的依赖。spring 依赖、mybatis 依赖、mysql 驱动、spring 的事务、的依赖、mybatis 和 spring 集成的依赖（mybatis 官方提供的，用来在 spring 项目中创建 mybatis 的 SqlSessionFactory、dao 对象的）
（3）创建实体类。
（4）创建 dao 接口和 mapper 文件。
（5）创建 mybatis 主配置文件。
（6）创建 service 接口和实现类，属性是 dao。
（7）创建 spring 的配置文件，声明 mybatis 的对象交给 spring 创建。（数据源 DataSource、SqlSessionFactory、dao 对象、声明自定义的 service）
（8）创建测试类，获取 service 对象，通过 service 调用 dao 完成数据库的访问。
### 4.1 入门案例
**（3）创建实体类：**
```java
public class Student {
    private Integer id;
    private String name;
    private String email;
    private Integer age;
    // 省略了构造方法、set()方法、get()方法以及toString()方法
}
```
**（4）创建 dao 接口和 mapper 文件（StudentDao.xml）：**
```java
public interface StudentDao {
    Integer insertStudent(Student student);
    List<Student> queryStudents();
}
```
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.atjingbo.dao.StudentDao">

    <insert id="insertStudent">
        insert into student values(#{id},#{name},#{email},#{age})
    </insert>
    
    <select id="queryStudents" resultType="com.atjingbo.pojo.Student">
        select id,name,email,age from student order by id
    </select>
</mapper>
```
**（5）创建 mybatis 主配置文件（mybatis.xml）：**
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <settings>
        <setting name="logImpl" value="STDOUT_LOGGING"/>
    </settings>

    <mappers>
        <mapper resource="com/atjingbo/dao/StudentDao.xml"/>
    </mappers>
</configuration>
```
**（6）创建 service 接口和实现类，属性是 dao：**
```java
public interface StudentService {
    Integer insertStudent(Student student);
    List<Student> queryStudents();
}
```
```java
public class StudentServiceImpl implements StudentService {
    private StudentDao studentDao;
    // 使用 set 注入，赋值
    public void setStudentDao(StudentDao studentDao) {
        this.studentDao = studentDao;
    }
    @Override
    public Integer insertStudent(Student student) {
        Integer num = studentDao.insertStudent(student);
        return num;
    }
    @Override
    public List<Student> queryStudents() {
        List<Student> studentList = studentDao.queryStudents();
        return studentList;
    }
}
```
**（7）创建 spring 的配置文件（applicationContext.xml），声明 mybatis 的对象交给 spring 创建：（数据源 DataSource、SqlSessionFactory、dao 对象、声明自定义的 service）**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 声明数据源 DataSource，作用是连接数据库 -->
    <bean id="myDataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <!-- 使用 set 注入给 DruidDataSource 提供连接数据库的信息 -->
        <property name="url" value="jdbc:mysql://localhost:3306/bjpowernode" /> <!-- setUrl() -->
        <property name="username" value="root" /> <!-- setUsername() -->
        <property name="password" value="123456" /> <!-- setPassword() -->
        <property name="maxActive" value="20" /> <!-- setMaxActive() -->
    </bean>

    <!-- 声明 mybatis 中提供的 SqlSessionFactoryBean 类，这个类内部创建 SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- set 注入，把数据库连接池赋给了 dataSource 属性-->
        <property name="dataSource" ref="myDataSource" />
        <!-- mybatis 主配置文件的位置
            configLocation 属性是 Resource 类型的，用来读取配置文件。
            它的赋值使用的是 value，指定文件的路径。使用 classpath: 表示文件的位置
        -->
        <property name="configLocation" value="classpath:mybatis.xml" />
    </bean>

    <!-- 创建 dao 对象，使用 SqlSession.getMapper(StudentDao.class) 方法
        MapperScannerConfigurer：在内部调用 getMapper() 生成每个 dao 接口的代理对象。
    -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 指定 SqlSessionFactory 对象的 id -->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
        <!-- 指定包名，包名是 dao 接口所在的包名
            MapperScannerConfigurer 会扫描这个包中的所有接口，把每个接口都执行一次 getMapper() 方法，得到每个接口的 dao 对象。
            创建好的 dao 对象放入到 spring 的容器中，dao 对象的默认名称是：接口名首字母小写。
        -->
        <property name="basePackage" value="com.atjingbo.dao" />
    </bean>

    <!-- 声明 service -->
    <bean id="studentService" class="com.atjingbo.service.impl.StudentServiceImpl">
        <property name="studentDao" ref="studentDao" />
    </bean>
</beans>
```
**（8）创建测试类，获取 service 对象，通过 service 调用 dao 完成数据库的访问：**
```java
public class MyTest {
    @Test
    public void test(){
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        // 获取 spring 容器中的 service 对象
        StudentService service = (StudentService) context.getBean("studentService");
        // spring 和 mybatis 集成在一起使用，事务是自动提交的，无需执行 SqlSession.commit()
        service.insertStudent(new Student(1009, "王五", "wangwu@qq.com", 29));
    }
}
```
### 4.2 使用属性配置文件
* jdbc.properties 属性配置文件内容：
```java
jdbc.url=jdbc:mysql://localhost:3306/bjpowernode
jdbc.username=root
jdbc.passwd=123456
jdbc.max=20
```
* spring 配置文件内容：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--
         把数据库的配置信息，写在一个独立的文件中，便于修改数据库的配置内容。
         spring 需要知道 jdbc.properties 的位置。
    -->
    <context:property-placeholder location="classpath:jdbc.properties" />

    <bean id="myDataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <!-- 使用属性配置文件中的数据，格式：${key} -->
        <property name="url" value="${jdbc.url}" />
        <property name="username" value="${jdbc.username}" />
        <property name="password" value="${jdbc.passwd}" />
        <property name="maxActive" value="${jdbc.max}" />
    </bean>

    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="myDataSource" />
        <property name="configLocation" value="classpath:mybatis.xml" />
    </bean>

    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
        <property name="basePackage" value="com.atjingbo.dao" />
    </bean>

    <bean id="studentService" class="com.atjingbo.service.impl.StudentServiceImpl">
        <property name="studentDao" ref="studentDao" />
    </bean>
</beans>
```
## 五、Spring 事务
### 5.1 spring 处理事务的相关知识点
* 什么是事务？
事务是指一组 sql 语句的集合，集合中有多条 sql 语句，这些 sql 语句的执行要么全部成功，要么全部失败，这些 sql 语句的执行是一致的，作为一个整体执行。
* 什么时候需要使用事务？
当我的操作涉及到多个表或者多个 sql 语句的 增删改时，并且需要保证这些语句都是成功或者失败才能完成我的功能，那么需要使用事务。
* 在 java 代码中，控制事务需要写在哪里？
service 类的业务代码上，因为业务方法会调用多个 dao 方法，执行多个 sql 语句。
* JDBC 访问数据库，处理事务：Connection conn; conn.commit(); conn.rollback();
 MyBatis 访问数据库，处理事务： SqlSession.commit(); SqlSession.rollback();
  Hibernate 访问数据库，处理事务： Session.commit(); Session.rollback();
* 以上两种事务处理方式，有什么不足？
不同的数据库访问技术有不同的事务处理机制。
* 怎么解决不足？
（1）spring 提供了一种统一的事务处理模型，能够使用统一的步骤、方式去完成多种不同的数据库访问技术的事务处理。
（2）spring 处理事务的模型，使用的步骤都是固定的，只需要把事务要使用的信息直接提供给 spring 就可以了。
（3）spring 内部提交、回滚事务，使用的是事务管理器对象，代替你完成 commit、rollback。**spring 使用事务管理器和它的实现类来管理事务。**
（4）**事务管理器是一个接口和它的众多实现类。接口：PlateformTransactionManager**，它定义了事务的重要方法：commit 和 rollback。**实现类：spring 把每一种数据库访问技术对应的事务处理类都创建好了**。比如：`mybatis 访问数据库 --- spring 创建好的是：DataSourceTransactionManager`。Hibernate 访问数据库 --- spring 创建好的是：HibernateTransactionManager。
* 声明式事务：把事务相关的资源和内容都提供给 spring，spring 就能处理事务提交、回滚了，几乎不用代码。
* 怎么使用 spring 的事务处理机制？
你需要告诉 spring 你用的是哪种数据库访问技术，通过**声明数据库访问技术对应的事务管理器的实现类，在 spring 的配置文件中使用 \<bean> 声明**就可以了。
* 事务定义接口（TransactionDefinition）中定义了事务描述相关的三类常量：事务隔离级别、超时时间、事务传播行为以及对它们的操作。
* 事务的隔离级别：
（1）**DEFAULT （默认）**：采用 DB 默认的事务隔离级别。MySql 的默认为 REPEATABLE_READ（可重复读）；Oracle 默认为 READ_COMMITTED （读已提交）。
（2）**READ_UNCOMMITTED （读未提交）：** 这是事务最低的隔离级别，它允许另外一个事务可以看到这个事务未提交的数据。这种隔离级别会产生脏读，不可重复读和幻像读。
（3）**READ_COMMITTED （读已提交）：** 保证一个事务修改的数据提交后才能被另外一个事务读取，另外一个事务不能读取该事务未提交的数据。这种事务隔离级别可以避免脏读出现，但是可能会出现不可重复读和幻像读。
（4） **REPEATABLE_READ （可重复读）：** 这种事务隔离级别可以防止脏读、不可重复读，但是可能出现幻像读。它除了保证一个事务不能读取另一个事务未提交的数据外，还保证了不可重复读。
（5） **SERIALIZABLE（串行化）：** 这是花费最高代价但是最可靠的事务隔离级别，事务被处理为顺序执行。除了防止脏读、不可重复读外，还避免了幻像读。
* 事务的超时时间：表示一个方法的最长执行时间，如果方法执行时超过了这个时间，事务就回滚。单位是秒，整数值，默认是 -1，表示没有时限。
* 事务的传播行为（掌握前三个）：表示处于不同事务中的方法在相互调用时，事务在各方法之间是如何使用的。
（1）**PROPAGATION_REQUIRED（默认）：** 指定的方法必须在事务内执行。如果当前存在事务，就加入到当前事务中；如果当前没有事务，那么就创建一个新事务。
（2）**PROPAGATION_REQUIRES_NEW：** 指定的方法支持当前事务，但如果当前没有事务，也可以以非事务的方式执行。
（3）**PROPAGATION_SUPPORTS：** 总是新建一个事务，如果当前存在事务，就将当前事务挂起，直到新事务执行完毕。
（4）PROPAGATION_MANDATORY
（5）PROPAGATION_NESTED
（6）PROPAGATION_NEVER
（7）PROPAGATION_NOT_SUPPORTED
* spring 提交事务、回滚事务的时机：
（1）当你的业务方法执行成功，没有抛出异常时，spring 在方法执行完毕后提交事务。
（2）当你的业务方法抛出非运行时异常，主要是编译时异常时，提交事务。
（3）当你的业务方法抛出运行时异常或者 ERROR，spring 执行回滚。
* 如何使用 spring 事务：
（1）指定要使用的事务管理器的实现类，使用 \<bean>。
（2）指定哪些类、哪些方法需要加入事务的功能。
（3）指定方法需要的隔离级别、传播行为、超时时间。
### 5.2 spring 提供的事务处理方案
方案一：适合中小型项目使用的：注解方案。
方案二：适合大型项目使用的：使用 aspectJ 的 aop 配置管理事务。
#### 5.2.1 注解方案
* spring 框架自己用 AOP 实现了给业务方法增加事务的功能，使用 @Transactional 注解来增加事务。**@Transactional 注解**是 spring 框架自己的注解，**只能放在 public 方法的上面**，表示当前方法有事务。
* 可以给注解的属性赋值，表示具体的隔离级别、传播行为、异常消息等。
![图示](https://img-blog.csdnimg.cn/1d6f5950ca2f4d30b50f9a1d97dac460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
* 使用 @Transactional 的步骤：
（1）需要声明事务管理器对象。`<bean id="xx" class="DataSourceTransactionManager">`
（2）开启事务注解驱动。告诉 spring 框架，我要使用注解的方式来管理事务，spring 就会使用 AOP 机制，创建 @Transactional 所在类的代理对象，给方法加入事务的功能。spring 给业务方法加入事务：使用的是 AOP 的**环绕通知**，在你的业务方法执行之前先开启事务，在业务方法之后提交或者回滚事务。
（3）在你的方法上面加入 @Transactional。
* 具体实现：
**（1）、（2）：**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!--
         把数据库的配置信息，写在一个独立的文件中，便于修改数据库的配置内容。
         spring 需要知道 jdbc.properties 的位置。
    -->
    <context:property-placeholder location="classpath:jdbc.properties" />

    <!-- 声明数据源 DataSource，作用是连接数据库 -->
    <bean id="myDataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <!-- 使用 set 注入给 DruidDataSource 提供连接数据库的信息 -->

        <!-- 使用属性配置文件中的数据，格式：${key} -->
        <property name="url" value="${jdbc.url}" /> <!-- setUrl() -->
        <property name="username" value="${jdbc.username}" /> <!-- setUsername() -->
        <property name="password" value="${jdbc.passwd}" /> <!-- setPassword() -->
        <property name="maxActive" value="${jdbc.max}" /> <!-- setMaxActive() -->
    </bean>

    <!-- 声明 mybatis 中提供的 SqlSessionFactoryBean 类，这个类内部创建 SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- set 注入，把数据库连接池赋给了 dataSource 属性-->
        <property name="dataSource" ref="myDataSource" />
        <!-- mybatis 主配置文件的位置
            configLocation 属性是 Resource 类型的，用来读取配置文件。
            它的赋值使用的是 value，指定文件的路径。使用 classpath: 表示文件的位置
        -->
        <property name="configLocation" value="classpath:mybatis.xml" />
    </bean>

    <!-- 创建 dao 对象，使用 SqlSession.getMapper(StudentDao.class) 方法
        MapperScannerConfigurer：在内部调用 getMapper() 生成每个 dao 接口的代理对象。
    -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 指定 SqlSessionFactory 对象的 id -->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
        <!-- 指定包名，包名是 dao 接口所在的包名
            MapperScannerConfigurer 会扫描这个包中的所有接口，把每个接口都执行一次 getMapper() 方法，得到每个接口的 dao 对象。
            创建好的 dao 对象放入到 spring 的容器中，dao 对象的默认名称是：接口名首字母小写。
        -->
        <property name="basePackage" value="com.atjingbo.dao" />
    </bean>

    <!-- 声明 service -->
    <bean id="buyGoodService" class="com.atjingbo.service.impl.BuyGoodServiceImpl">
        <property name="saleDao" ref="saleDao" />
        <property name="goodDao" ref="goodDao" />
    </bean>

    <!-- 使用 spring 的事务处理 -->
    <!-- 1、声明事务管理器对象 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!-- 连接哪个数据库，需要指定数据源 -->
        <property name="dataSource" ref="myDataSource" />
    </bean>
    <!-- 2、开启事务注解驱动，告诉 spring 使用注解管理事务，创建代理对象
                transaction-manager：事务管理器对象的 id
        -->
    <tx:annotation-driven transaction-manager="transactionManager" />
</beans>
```
**（3）：**
```java
public class BuyGoodServiceImpl implements BuyGoodService {
    private SaleDao saleDao;
    private GoodDao goodDao;
    /*    @Transactional(
                propagation = Propagation.REQUIRED,
                isolation = Isolation.DEFAULT,
                readOnly = false,
                rollbackFor = {NullPointerException.class, RuntimeException.class}
          )

          rollbackFor：表示发生指定的异常后一定会回滚。
              处理逻辑：
                  （1）spring 框架会首先检查方法抛出的异常是不是在 rollbackFor 的属性值中，
                      如果异常在 rollbackFor 列表中，那么不管是什么异常，一定会回滚。
                  （2）如果你抛出的异常不在 rollbackFor 列表中，spring 会判断一场是不是 RuntimeException，
                  如果是则一定会回滚。
    */

    /*
     * 使用的是事务控制的默认值。默认的传播行为：REQUIRED，默认的管理级别：DEFAULT，
     * 默认抛出运行时异常时，回滚事务。
     * */
    @Transactional
    @Override
    public void buy(Integer goodId, Integer num) {
        // 记录销售信息，向 sale 表中添加信息
        saleDao.insertSale(new Sale(goodId, num));

        // 更新库存
        Good good = goodDao.queryGood(goodId);
        if (good == null) {
            throw new NullPointerException("编号为【" + goodId + "】的商品不存在");
        } else if (good.getAmount() < num) {
            throw new RuntimeException("编号为【" + goodId + "】的商品库存不足");
        }

        Good newGood = new Good();
        newGood.setAmount(num);
        newGood.setId(goodId);
        goodDao.updateGood(newGood);
    }
    public void setSaleDao(SaleDao saleDao) { this.saleDao = saleDao; }
    public void setGoodDao(GoodDao goodDao) { this.goodDao = goodDao; }
}
```
#### 5.2.2 使用 aspectJ 的 aop 配置管理事务
* 适合大型项目，有很多的类和方法，需要大量地配置事务。我们需要使用 aspectJ 框架的功能。**在 spring 配置文件中声明类、方法需要的事务**，这种方式的业务方法和事务配置是完全分离的。
* 实现步骤（都是在 xml 配置文件中实现的）：
（1）要使用的是 aspectJ 框架，首先需要加入依赖。
（2）声明事务管理器对象。`<bean id="xx" class="DataSourceTransactionManager">`
（3）声明方法需要的事务类型（配置方法的事务属性【隔离级别、传播行为、超时】）
（4）配置 AOP，指定哪些类要创建代理。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--
         把数据库的配置信息，写在一个独立的文件中，便于修改数据库的配置内容。
         spring 需要知道 jdbc.properties 的位置。
    -->
    <context:property-placeholder location="classpath:jdbc.properties" />

    <!-- 声明数据源 DataSource，作用是连接数据库 -->
    <bean id="myDataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <!-- 使用 set 注入给 DruidDataSource 提供连接数据库的信息 -->

        <!-- 使用属性配置文件中的数据，格式：${key} -->
        <property name="url" value="${jdbc.url}" /> <!-- setUrl() -->
        <property name="username" value="${jdbc.username}" /> <!-- setUsername() -->
        <property name="password" value="${jdbc.passwd}" /> <!-- setPassword() -->
        <property name="maxActive" value="${jdbc.max}" /> <!-- setMaxActive() -->
    </bean>

    <!-- 声明 mybatis 中提供的 SqlSessionFactoryBean 类，这个类内部创建 SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- set 注入，把数据库连接池赋给了 dataSource 属性-->
        <property name="dataSource" ref="myDataSource" />
        <!-- mybatis 主配置文件的位置
            configLocation 属性是 Resource 类型的，用来读取配置文件。
            它的赋值使用的是 value，指定文件的路径。使用 classpath: 表示文件的位置
        -->
        <property name="configLocation" value="classpath:mybatis.xml" />
    </bean>

    <!-- 创建 dao 对象，使用 SqlSession.getMapper(StudentDao.class) 方法
        MapperScannerConfigurer：在内部调用 getMapper() 生成每个 dao 接口的代理对象。
    -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 指定 SqlSessionFactory 对象的 id -->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
        <!-- 指定包名，包名是 dao 接口所在的包名
            MapperScannerConfigurer 会扫描这个包中的所有接口，把每个接口都执行一次 getMapper() 方法，得到每个接口的 dao 对象。
            创建好的 dao 对象放入到 spring 的容器中，dao 对象的默认名称是：接口名首字母小写。
        -->
        <property name="basePackage" value="com.atjingbo.dao" />
    </bean>

    <!-- 声明 service -->
    <bean id="buyGoodService" class="com.atjingbo.service.impl.BuyGoodServiceImpl">
        <property name="saleDao" ref="saleDao" />
        <property name="goodDao" ref="goodDao" />
    </bean>

    <!-- 声明式事务处理：和源代码完全分离的 -->
    <!-- 1、声明事务管理器对象 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!-- 连接哪个数据库，需要指定数据源 -->
        <property name="dataSource" ref="myDataSource" />
    </bean>
    <!-- 2、声明业务方法的事务属性（隔离级别、传播行为、超时时间）
              id：自定义名称，表示 <tx:advice> 和 </tx:advice> 之间的配置内容。
              transaction-manager：事务管理器对象的 id
    -->
    <tx:advice id="myAdvice" transaction-manager="transactionManager">
        <!-- tx:attributes：配置事务的属性 -->
        <tx:attributes>
            <!-- tx:method：给具体的方法配置属性，method 可以有多个，分别给不同的方法设置事务属性
                   name：方法名称 （1）完整的方法名称，不带包和类。（2）方法可以使用通配符，* 表示任意字符。
                   propagation：传播行为
                   isolation：隔离级别
                   rollback-for：你指定的异常类名，全限定名称
            -->
            <tx:method name="buy" propagation="REQUIRED" isolation="DEFAULT" rollback-for="java.lang.NullPointerException,java.lang.RuntimeException" />

            <!-- 使用通配符，指定很多的方法 -->
            <tx:method name="query*" read-only="true" />
        </tx:attributes>
    </tx:advice>

    <!-- 3、配置 AOP -->
    <aop:config>
        <!-- 配置切入点表达式：指定哪些包中的类，要使用事务
             id：切入点表达式的名称，唯一值。
             expression：切入点表达式，指定哪些类要使用事务，aspectJ 会创建代理对象。
        -->
        <aop:pointcut id=" servicePt" expression="execution(* *..service..*.*(..))"/>

        <!-- 配置增强器：关联 advice 和 pointcut
             advice-ref：通知，就是上面 tx:advice 那里的配置
             pointcut-ref：切入点表达式的 id
        -->
        <aop:advisor advice-ref="myAdvice" pointcut-ref=" servicePt" />
    </aop:config>
</beans>
```
## 六、Spring 与 Web
* 要求：web 项目中容器对象只需要创建一次。把容器对象放入到全局作用域 ServletContext 中。
* 如何实现：使用监听器，当全局作用域对象被创建时，创建容器，存入 ServletContext 中。
* 监听器作用：
（1）创建容器对象，执行：ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
（2）把容器对象放入到 ServletContext 中，ServletContext.setAttribute(key, context);
* 监听器可以自己去创建，也可以使用框架中提供好的：ContextLoaderListener。
* 具体实现：
**web.xml：**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>RegisterServlet</servlet-name>
        <servlet-class>com.atjingbo.servlet.RegisterServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>RegisterServlet</servlet-name>
        <url-pattern>/register</url-pattern>
    </servlet-mapping>

    <!-- 注册监听器 ContextLoaderListener。
         监听器被创建后，会读取 /WEB-INF/applicationContext.xml。
         为什么要读取文件：因为在监听器中要创建 ApplicationContext 对象，需要加载配置文件。
         /WEB-INF/applicationContext.xml 就是监听器默认读取的 spring 配置文件路径。

         可以修改默认的文件位置，使用 context-param 重新指定文件的位置。

         配置监听器的目的：创建容器对象，创建了容器对象，就能把 spring 配置文件中的所有对象都创建好，
                         用户发起请求就直接可以使用对象了。
    -->
    <context-param>
        <!-- contextConfigLocation：表示 spring 配置文件的路径 -->
        <param-name>contextConfigLocation</param-name>
        <!-- 自定义 spring 配置文件的路径-->
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
</web-app>
```
**spring 配置文件 applicationContext.xml：**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--
         把数据库的配置信息，写在一个独立的文件中，便于修改数据库的配置内容。
         spring 需要知道 jdbc.properties 的位置。
    -->
    <context:property-placeholder location="classpath:jdbc.properties" />

    <!-- 声明数据源 DataSource，作用是连接数据库 -->
    <bean id="myDataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <!-- 使用 set 注入给 DruidDataSource 提供连接数据库的信息 -->

        <!-- 使用属性配置文件中的数据，格式：${key} -->
        <property name="url" value="${jdbc.url}" /> <!-- setUrl() -->
        <property name="username" value="${jdbc.username}" /> <!-- setUsername() -->
        <property name="password" value="${jdbc.passwd}" /> <!-- setPassword() -->
        <property name="maxActive" value="${jdbc.max}" /> <!-- setMaxActive() -->
    </bean>

    <!-- 声明 mybatis 中提供的 SqlSessionFactoryBean 类，这个类内部创建 SqlSessionFactory -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!-- set 注入，把数据库连接池赋给了 dataSource 属性-->
        <property name="dataSource" ref="myDataSource" />
        <!-- mybatis 主配置文件的位置
            configLocation 属性是 Resource 类型的，用来读取配置文件。
            它的赋值使用的是 value，指定文件的路径。使用 classpath: 表示文件的位置
        -->
        <property name="configLocation" value="classpath:mybatis.xml" />
    </bean>

    <!-- 创建 dao 对象，使用 SqlSession.getMapper(StudentDao.class) 方法
        MapperScannerConfigurer：在内部调用 getMapper() 生成每个 dao 接口的代理对象。
    -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 指定 SqlSessionFactory 对象的 id -->
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
        <!-- 指定包名，包名是 dao 接口所在的包名
            MapperScannerConfigurer 会扫描这个包中的所有接口，把每个接口都执行一次 getMapper() 方法，得到每个接口的 dao 对象。
            创建好的 dao 对象放入到 spring 的容器中，dao 对象的默认名称是：接口名首字母小写。
        -->
        <property name="basePackage" value="com.atjingbo.dao" />
    </bean>

    <!-- 声明 service -->
    <bean id="studentService" class="com.atjingbo.service.impl.StudentServiceImpl">
        <property name="studentDao" ref="studentDao" />
    </bean>
</beans>
```
**RegisterServlet 类：**
```java
public class RegisterServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        request.setCharacterEncoding("UTF-8");

        Integer id = Integer.parseInt(request.getParameter("id"));
        String name = request.getParameter("name");
        String email = request.getParameter("email");
        Integer age = Integer.parseInt(request.getParameter("age"));

        /*
            // 创建 spring 容器对象：
            ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        */
        

        /*
            WebApplicationContext context = null;
            // 获取 ServletContext 中的容器对象（创建好的容器对象，拿来就用）
            String key = WebApplicationContext.ROOT_WEB_APPLICATION_CONTEXT_ATTRIBUTE;
            Object attribute = getServletContext().getAttribute(key);
            if(attribute != null){
                context = (WebApplicationContext) attribute;
            }
        */
         

        // 使用框架中的方法，获取容器对象
        ServletContext sc = getServletContext();
        WebApplicationContext context = WebApplicationContextUtils.getRequiredWebApplicationContext(sc);
        
        StudentService studentService = (StudentService)context.getBean("studentService");
        studentService.insertStudent(new Student(id, name, email, age));
        request.getRequestDispatcher("/result.jsp").forward(request, response);
    }
}
```