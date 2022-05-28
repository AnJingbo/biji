

## 一、JVM 与 Java 体系结构
### 1.1 Java VS C++
Java 和 C++ 之间有一堵由内存动态分配和垃圾收集技术所围成的高墙。
![图示](https://img-blog.csdnimg.cn/3f7be674d9404f1abb543b984bea0b86.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_17,color_FFFFFF,t_70,g_se,x_16)
### 1.2 Java：跨平台的语言
![图示](https://img-blog.csdnimg.cn/c3885806e5584c56b6c32330cc399d16.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
### 1.3 JVM：跨语言的平台
![图示](https://img-blog.csdnimg.cn/845cfb844c2d47bfbc81ece6ec567bd1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* 随着 Java7 的正式发布，Java 虚拟机的设计者们通过 JSR-292 规范基本实现在 **Java 虚拟机平台上运行非 Java 语言编写的程序。**
* Java 虚拟机根本不关心运行在其内部的程序到底是使用何种编程语言编写的，**它只关心“字节码”文件**。也就是说 Java 虚拟机拥有语言无关性，并不会单纯地与 Java 语言“终身绑定”，只要其它编程语言的编译结果满足并包含 Java 虚拟机的内部指令集、符号表以及其它的辅助信息，它就是一个有效的字节码文件，就能够被虚拟机所识别并装载运行。
### 1.3 字节码
* 我们平时说的 Java 字节码，指的是使用 Java 语言编译成的字节码。准确地说，任何能在 jvm 平台上执行的字节码格式都是一样的，所以应该统称为：**jvm 字节码**。
* 不同的编译器，可以编译出相同的字节码文件，字节码文件也可以在不同的 JVM 上运行。
* Java 虚拟机与 Java 语言并没有必然的联系，它只与特定的二进制文件格式 -- Class 文件格式所关联，Class 文件中包含了 Java 虚拟机指令集（或者称为字节码、Bytecodes）和符号表，还有一些其它辅助信息。
### 1.4 多语言混合编程
![图示](https://img-blog.csdnimg.cn/b645306c688c4f3baab0f3030a765990.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_17,color_FFFFFF,t_70,g_se,x_16)
### 1.5 Java 发展的重大事件
![图示](https://img-blog.csdnimg.cn/10ec88fe562043899f674930964f5f2e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
### 1.6 虚拟机
![图示](https://img-blog.csdnimg.cn/fb3cc8bd49b04a8186fb029ccc7c6db5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_17,color_FFFFFF,t_70,g_se,x_16)
### 1.7 Java 虚拟机
![图示](https://img-blog.csdnimg.cn/c15fb95820b34d5fbaf97d9f4f1a8ce1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* **一个进程对应着一个 JVM 的实例。**
### 1.8 JVM 的位置
![图示](https://img-blog.csdnimg.cn/5a4988b8f091464bb96873b9d3396964.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_14,color_FFFFFF,t_70,g_se,x_16)
### 1.9 JVM 的整体结构
![图示](https://img-blog.csdnimg.cn/00b27e9b635741129327b052dc331e61.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 注：橙色部分（即**方法区和堆**）**是所有线程共享的数据区**，其余的是每个线程都有一份。
### 1.10 Java 代码执行流程
![图示](https://img-blog.csdnimg.cn/b5654aa0da0649d89b4ececbc18a7ef5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
![图示](https://img-blog.csdnimg.cn/d8d1ce5aaaaf4a5e9f53b2331272b6f6.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
![图示](https://img-blog.csdnimg.cn/b2107c23b8db4a7297df151820581093.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
* Java 编译器的编译过程中，任何一个结点执行失败就会造成编译失败。
* 虽然各个平台的 Java 虚拟机内部实现细节不尽相同，但是**它们共同执行的字节码内容却是一样的。**
* JVM 的主要任务就是负责将字节码装载到其内部，解释/编译为对应平台的机器指令。
* Java 虚拟机使用类加载器（Class Loader）装载 class 文件。
* 类加载完成之后，会进行字节码校验，字节码校验通过后 JVM 解释器会把字节码翻译成机器码交给操作系统执行。
* 但不是所有的代码都是解释执行的，JVM 对此做了优化。比如，以 Hotspot 虚拟机来说，它本身也提供了 JJT（Just In Time，即时编译器）
### 1.11 JVM 的架构模型
![图示](https://img-blog.csdnimg.cn/f0318696f71947f1aae7248c55b6f674.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
* 注：正常执行指令的时候，需要两部分：地址和操作数。
（1）零地址指令：指令中没有地址，只有操作数。因为基于栈时我们只对栈顶进行操作，其它数据不进行操作，因此我们只需要操作数就行。
（2）一地址指令：有一个地址和一个操作数；二地址指令：指令中给出两个地址。
* 总结：**由于跨平台性的设计，Java 的指令都是根据栈来设计的**。不同平台 CPU 架构不同，所以不能设计为基于寄存器的，因为**基于寄存器的话它和具体的 CPU 耦合度高**。**优点**是跨平台，指令集小，编译器容易实现。**缺点**是性能比基于寄存器的差，实现同样的功能需要更多的指令。
### 1.12 JVM 的生命周期
1. 虚拟机的启动。
Java 虚拟机的启动是通过引导类加载器（bootstrap class loader）创建一个初始类（initial class）来完成的，这个类是由虚拟机的具体实现指定的。
2. 虚拟机的执行。
（1）一个运行中的 Java 虚拟机有着一个清晰的任务：执行 Java 程序。
（2）程序开始执行时它才运行，程序结束时它就停止。
（3）**执行一个所谓的 Java 程序的时候，真正在执行的是一个叫做 Java 虚拟机的进程。**
3. 虚拟机的退出。
有如下几种情况：
（1）程序正常执行结束。
（2）程序在执行过程中遇到了异常或错误而异常终止。
（3）由于操作系统出现错误而导致 Java 虚拟机进程终止。
（4）某线程调用 Runtime 类或 System 类的 exit 方法，或 Runtime 类的 halt 方法，并且 Java 安全管理器也允许这次 exit 或 halt 操作。
（5）另：JNI（Java Native Interface）规范描述了用 JNI Invocation API 来加载或卸载 Java 虚拟机时，Java 虚拟机的退出情况。
### 1.13 JVM 发展历程
#### Sun Classic VM
* 早在 1996 年 Java1.0 版本的时候，Sun 公司发布了一款名为 Sun Classic VM 的 Java 虚拟机，它同时也是**世界上第一款商用的 Java 虚拟机**，jdk1.4 时完全被淘汰。
* 这款虚拟机**内部只提供解释器**。
* 如果要使用 JIT 编译器，就需要进行外挂。但是一旦使用了 JIT 编译器，JIT 就会接管虚拟机的执行系统。解释器就不再工作。**解释器和编译器不能配合工作**。
* 现在 hotspot 内置了此虚拟机。
#### Exact VM
* 为了解决上一个虚拟机的问题，jdk1.2 时，Sun 提供了此虚拟机。
* Exact Memory Management：准确式内存管理
（1）也可以叫 Non-Conservative/Accurate Memory Management。
（2）虚拟机可以知道内存中某个位置的数据具体是什么类型。
* 其具有现代高性能虚拟机的雏形
（1）热点探测。**探测出来哪些代码属于高频的执行代码（热点代码），只针对热点代码进行即时编译。**
（2）编译器与解释器混合工作模式。
* 只在 Solaris 平台短暂使用过，其它平台还是 classic vm，之后被 Hotspot 虚拟机替换。
#### SUN 公司的 HotSpot VM（重点）
* HotSpot 历史：
（1）最初由一家名为“Longview Technologies”的小公司设计。
（2）1997 年，此公司被 Sun 收购；2019年，Sun 公司被甲骨文收购。
（3）JDK1.3 时，HotSpot VM 称为默认虚拟机。
* 目前 HotSpot 占有绝对的市场地位
（1）不管是现在仍在广泛使用的 jdk6，还是使用比例较多的 jdk8 中，默认的虚拟机都是 HotSpot。
（2）Sun/Oracle JDK 和 OpenJDK 的默认虚拟机。
* 从服务器、桌面到移动端、嵌入式都有应用。
* **名称中的 HotSpot 指的就是它的热点代码探测技术**。
（1）通过计数器找到最具编译价值的代码，触发即时编译或栈上替换。即**探测出来哪些代码属于高频的执行代码（热点代码），只针对热点代码进行即时编译。**
（2）通过解释器和编译器协同工作，在最优化的程序响应时间与最佳执行性能中取得平衡。**解释器主要负责响应时间，它的响应时间比较快；编译器主要负责执行性能。**
#### BEA 的 JRockit
* **专注于服务器端应用**。它可以不太关注程序启动速度，因此 **JRockit 内部不包含解析器实现**，全部代码都靠即时编译器编译后执行。
* 大量的行业基准测试显示：**JRockit JVM 是世界上最快的 JVM。**
* 优势：全面的 Java 运行时解决方案组合
（1）JRockit 面向延迟敏感型应用的解决方案 JRockit Real Time 提供以毫秒或微秒级的 JVM 响应时间，适合财务、军事指挥、电信网络需要。
（2）MissionControl 服务套件，它是一组以极低的开销来监控、管理和分析生产环境中的应用程序的工具。
* 2008年，BEA 被 Oracle 收购。
* Oracle 表达了整合两大优秀虚拟机的工作，大致在 jdk8 中完成。整合的方式是在 HotSpot 的基础上，移植 JRockit 的优秀特性。
#### IBM 的 J9
* 全称：IBM Technology for Java Virtual Machine，简称 IT4J，内部代号：J9。
* 市场定位与 HotSpot 相同，服务器端、桌面应用、嵌入式等多用途 VM。
* 广泛应用于 IBM 的各种 Java 产品。
* **目前最有影响力的三大商用虚拟机之一。**
* 2017 年左右，IBM 发布了开源 J9 VM，命名为 OpenJ9，交给 Eclipse 基金会管理，也称为 Eclipse OpenJ9。
## 二、类加载子系统
### 2.1 内存结构概述
![图示](https://img-blog.csdnimg.cn/9a5a722ca8e24d28bc3f560d39c0a5cd.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
![图示](https://img-blog.csdnimg.cn/0e8a7ba85e1a4efaaa63ec2d045c3d5d.jpg?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 2.2 类加载器与类的加载过程
#### 2.2.1 类加载器子系统作用
* 类加载器子系统结构：
![图示](https://img-blog.csdnimg.cn/69912cdd94ac42b0b1781f563e38f9e9.png)
* 类加载器子系统负责从文件系统或者网络中**加载 Class 文件**，Class 文件在文件开头有特定的文件标识（CA FE BA BE）
* ClassLoader 只负责 Class 文件的加载，至于它**是否可以运行，则由执行引擎（Execution Engine）决定。**
* **加载的类信息存放于一块称为方法区的内存空间**。除了类的信息外，方法区中还会存放运行时常量池信息，可能还包括字符串字面量和数字常量（这部分常量信息是 Class 文件中常量池部分的内存映射）
#### 2.2.2 类的加载过程
![图示](https://img-blog.csdnimg.cn/e46b162616e1429d82d1b92f83ad3a69.png)
![图示](https://img-blog.csdnimg.cn/47eb166a7f3c44449ce1a7ac226dfc4c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_14,color_FFFFFF,t_70,g_se,x_16)
##### 加载（Loading）
* 通过一个**类的全限定名称**获取定义此类的二进制字节流。
* 将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构。即把类存放到方法区当中。
* **在内存中生成一个代表这个类的 java.lang.Class 对象**，作为方法区这个类的各种数据的访问入口。
##### 链接（Linking）
1、验证（Verify）
* 目的在于确保 Class 文件的字节流中包含信息符合当前虚拟机的要求，保证被加载类的正确性，不会危害虚拟机自身安全。
* 验证阶段主要包括：类数据信息的格式验证、语义检查、字节码验证、符号引用验证。
![tushi](https://img-blog.csdnimg.cn/15ea00097459449ea178e278c11a14c0.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
（1）其中**格式验证会和加载阶段一起执行**，验证通过之后，类加载器才会成功地将类的二进制数据信息加载到方法区中。
（2）**格式验证之外的验证操作将会在方法区中进行。**

2、准备（Prepare）
* **为静态变量分配内存并且设置该静态变量的默认值。**
* **这里不包含用 final 修饰的 static，因为 final 在编译的时候就会分配了，准备阶段会显式初始化。**
* **这里不会为实例变量分配初始化**，静态变量会分配在方法区中，而实例变量是会随着对象一起分配到 Java 堆中。
* 示例：
```java
public class Test01 {
    private static int a = 3; // prepare阶段：a = 0；Initial：a = 3
    public static void main(String[] args) {
        System.out.println(a);
    }
}
```
3、解析（Resolve）
* 将常量池内的符号引用转换为直接引用的过程。
* 事实上，解析操作往往会伴随着 JVM 在执行完初始化之后再执行。
* 符号引用就是一组符号来描述所引用的目标。符号引用的字面量形式明确定义在《Java虚拟机规范》的 Class 文件格式中。直接引用就是直接指向目标的指针、相对偏移量或一个直接定位到目标的句柄。
* 解析动作主要针对类或接口、字段、类方法、接口方法、方法类型等。对应常量池中的 CONSTANT_Class_info、CONSTANT_Fieldref_info、CONSTANT_Methodref_info
##### 初始化（Initialization）
* **初始化阶段就是执行类构造器方法\<clinit>()的过程。**
* \<clinit>() 方法不需要定义，它是 Javac 编译器的自动生成物，它是由编译器自动收集**类中的所有静态变量的赋值动作和静态代码块中的语句**合并而来。**如果一个类中没有静态语句块，也没有对静态变量的赋值操作，那么编译器可以不为这个类生成 \<clinit>() 方法。**
* 类构造器方法中的指令是按照语句在源文件中出现的顺序来执行的。
* **\<clinit>() 方法不同于类的构造方法（即在虚拟机视角下的实例构造器方法\<init>()）**
* 若该类具有父类，JVM 会保证**子类的 \<clinit>() 执行前，父类的 \<clinit>() 已经执行完毕。**
* JVM 虚拟机必须保证一个类的类构造器 \<clinit>() 在多线程环境中被正确地加锁同步。如果多个线程同时去初始化一个类，那么只会有其中一个线程去执行这个类的 \<clinit>() 方法，其他线程都需要阻塞等待，直到活动线程执行 \<clinit>() 方法完毕，而且其他线程在被唤醒后不会再执行 \<clinit>() 方法。所以**在同一个类加载器下，一个类只会被初始化一次。**
* 示例：
```java
public class Test02 {
    private static int num = 1; // linking 的 prepare 阶段: num = 0; initial: num = 1 -> num = 2
    // 静态代码块只能访问到定义在静态代码块之前的变量，定义在它之后的变量，静态代码块可以赋值，但是不能访问
    static {
        num = 2;
        number = 20;
        System.out.println("->" + num);
        // System.out.println(number); 报错：非法的前向引用
    }
    private static int number = 10; // linking 的 prepare 阶段: number = 0; initial: number = 20 -> number = 10
    public static void main(String[] args) {
        System.out.println(num); // 2
        System.out.println(number); // 10
    }
}
```
### 2.3 几种类加载器
#### 2.3.1 类加载器的分类
* JVM 支持两种类型的类加载器，分别是**引导类加载器（Bootstrap ClassLoader）和自定义类加载器（User-Defined ClassLoader）**
* 从概念上来说，自定义类加载器一般指的是程序中由开发人员自定义的一类类加载器，但是 Java 虚拟机规范却没有这么定义，而是**将所有派生于抽象类 ClassLoader 的类加载器都划分为自定义类加载器。**
* 图示：
![图示](https://img-blog.csdnimg.cn/a296875b30204c1fb3645c4ec9ccf7e2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 下面的类都间接地继承自 ClassLoader 类。因此按 Java 虚拟机规范，下面所有的类都属于自定义类加载器。
* 引导类加载器是使用 C/C++ 语言来实现的；下面的这些引导类加载器都是使用 Java 语言来实现的。
* 示例：
```java
public class ClassLoaderTest {
    public static void main(String[] args) {
        // 获取系统类加载器
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println(systemClassLoader); // sun.misc.Launcher$AppClassLoader@18b4aac2

        // 获取其上层：扩展类加载器
        ClassLoader extClassLoader = systemClassLoader.getParent();
        System.out.println(extClassLoader); // sun.misc.Launcher$ExtClassLoader@1b6d3586

        // 获取不到其上层：引导类加载器
        ClassLoader bootstrapClassLoader = extClassLoader.getParent();
        System.out.println(bootstrapClassLoader); // null

        // 对于用户自定义类来说：默认使用系统类加载器进行加载
        ClassLoader classLoader = ClassLoaderTest.class.getClassLoader();
        System.out.println(classLoader); // sun.misc.Launcher$AppClassLoader@18b4aac2

        // String 类使用引导类加载器进行加载 ---> Java 的核心类库都是使用引导类加载器进行加载的
        ClassLoader classLoader1 = String.class.getClassLoader();
        System.out.println(classLoader1); // null
    }
}
```
#### 2.3.2 引导类加载器（启动类加载器，Bootstrap ClassLoader）
* **这个类加载器是使用 C/C++ 语言实现的**，镶嵌在 JVM 内部，**我们无法直接获取到引导类加载器（会返回 null）。**
* 它**用来加载 Java 的核心库（JAVA_HOME/jre/lib/rt.jar、resources.jar 或 sun.boot.class.path 路径下的内容）**，用于提供 JVM 自身需要的类。
* **并不继承自 java.lang.ClassLoader，没有父加载器**（这里的父加载器并不是指 Java 中的继承关系，而是指这个类是由哪个加载器加载到内存的）
* 加载扩展类和系统类加载器，并指定为它们的父类加载器。即**扩展类加载器和系统类加载器是由引导类加载器加载的。**
* 出于安全考虑，引导类加载器**只加载包名为 java、javax、sun 等开头的类**。
#### 2.3.3 扩展类加载器（Extension ClassLoader）
* **Java 语言编写**，由 sun.misc.Launcher$ExtClassLoader 实现。
* **派生于 ClassLoader 类**。
* **父类加载器为启动类加载器。注：这里的父类加载器并不是指 Java 中的继承关系，而是指这个类是由哪个加载器加载到内存的。即扩展类加载器是由启动类加载器加载的**
* 从 java.ext.dirs 系统属性所指定的目录中加载类库，或从 JDK 的安装目录的 jre/lib/ext 子目录（扩展目录）下加载类库。**如果用户创建的 JAR 放在此目录下，也会自动由扩展类加载器加载。**
#### 2.3.4 系统类加载器（应用程序类加载器，AppClassLoader）
* **Java 语言编写**，由 sun.misc.Launcher$AppClassLoader 实现。
* **派生于 ClassLoader 类**。
* **父类加载器为启动类加载器。注：这里的父类加载器并不是指 Java 中的继承关系，而是指这个类是由哪个加载器加载到内存的。即系统类加载器是由启动类加载器加载的**
* 它**负责加载环境变量 classpath 或系统属性  java.class.path 指定路径下的类库**。
* **系统类加载器是程序中默认的类加载器**，一般来说，Java 应用的类都是由它来完成加载。
* 通过 ClassLoader.getSystemClassLoader() 方法可以获取到该类加载器。
#### 2.3.5 用户自定义的类加载器
* 在 Java 的日常应用程序开发中，类的加载几乎是由上述 3 中类加载器相互配合执行的，在必要时，我们还可以自定义类加载器，来定制类的加载方式。
* 为什么要自定义类加载器：
（1）防止源码泄露
（2）隔离加载类
（3）修改类加载的方式
（4）扩展加载源
* 用户自定义类加载器的实现方式：
（1）开发人员可以通过继承抽象类 java.lang.ClassLoader 类的方式，实现自己的类加载器，以满足一些特殊的需求。
（2）在 jdk1.2 之前，在自定义类加载器时，总会去继承 ClassLoader 类并重写 loadClass() 方法，从而实现自定义的类加载器，但是在 jdk1.2 之后已不再建议用户去覆盖 loadClass() 方法，而是**建议把自定义的类加载逻辑写在 findClass() 方法中。**
（3）在编写自定义类加载器时，**如果没有太过于复杂的需求，可以直接继承 URLClassLoader 类。这样就可以避免自己去编写 findClass() 方法及其获取字节码流的方式**，使自定义的类加载器编写更加简洁。 
#### 2.3.6 关于 ClassLoader
* ClassLoader 类，它是一个抽象类，其后**所有的类加载器都继承自 ClassLoader（不包括启动类加载器）**
* 内部方法：
![图示](https://img-blog.csdnimg.cn/929d843fb4684105978282cd881b3eca.png)
* 体系结构：
![图示](https://img-blog.csdnimg.cn/778eaee6b18744f0b11d6ff935711217.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 获取 ClassLoader 的途径
![图示](https://img-blog.csdnimg.cn/4aabb7a14db2410c9362d53e501dba66.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
### 2.4 双亲委派机制
* Java 虚拟机对 class 文件采用的是**按需加载**的方式，也就是说当需要使用该类时才会将它的 class 文件加载到内存生成 class 对象。而且加载某个类的 class 文件时，Java 虚拟机采用的是**双亲委派模式**，即把请求交由父类处理，它是一种任务委派模式。
* **工作原理：**
![图示](https://img-blog.csdnimg.cn/edc9e0f629aa47c39d78545aded9744d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 形象化理解：如果小明有一个苹果，小明会先把苹果给他妈妈问她吃不吃，他的妈妈则会先把苹果给他奶奶并问他奶奶吃不吃，如果奶奶吃，那苹果就没了；如果奶奶不吃，妈妈吃，那苹果也就没了；如果奶奶和妈妈都不吃，那就小明吃。
* 优点：
（1）避免类的重复加载。
（2）保护程序安全，防止核心 API 被随意篡改。例如：自定义类：java.lang.String、java.lang.ShkStart。
```java
package java.lang;
public class String {
    public static void main(String[] args) {
        // 在类 java.lang.String 中找不到 main 方法
        System.out.println("自定义 java.lang.String");
    }
}
```
```java
package java.lang;
public class ShkStart {
    public static void main(String[] args) {
        // Prohibited package name: java.lang
        System.out.println("java.lang.ShkStart");
    }
}
```
* **沙箱安全机制：**
自定义 String 类，但是在加载自定义 String 类的时候会率先使用引导类加载器加载，而引导类加载器在加载的过程中会先加载 jdk 自带的文件（rt.jar 包中 java\lang\String.class），报错信息说没有 main 方法就是因为加载的是 rt.jar 包中的 String 类。这样可以保证对 Java 核心源代码的保护，这就是**沙箱安全机制**。
### 2.5 其他
* 在 JVM 中表示两个 class 对象是否为一个类有两个必要条件：
（1）类的完整类名必须一致，包括包名。
（2）加载这个类的 ClassLoader（指 ClassLoader 实例对象）必须相同。
#### 2.5.1 对类加载器的引用
JVM 必须知道一个类型是由启动类加载器加载还是由用户类加载器加载的。如果一个类型是由用户类加载器加载的，那么 JVM 会**将这个类加载器的一个引用作为类型信息的一部分保存在方法区中**。当解析一个类型到另一个类型的引用的时候，JVM 需要保证这两个类型的类加载器是相同的。
#### 2.5.2 类的主动使用和被动使用
* Java 程序对类的使用方式分为主动使用和被动使用。
* 主动使用，又分为七种情况：
![图示](https://img-blog.csdnimg.cn/3db11d364620405c94889262f2ec6f32.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
* 除了以上七种情况，其他使用 Java 类的方式都被看作是对**类的被动使用**，都**不会导致类的初始化**。
* **类的主动使用会执行初始化（Initialization）操作，类的被动使用不会执行初始化（Initialization）操作。**
## 三、运行时数据区概述及线程
### 3.1 运行时数据区概述
* 内存是非常重要的系统资源，是硬盘和 CPU 的中间仓库及桥梁，承载着操作系统和应用程序的实时运行。JVM 内存布局规定了 Java 在运行过程中内存申请、分配、管理的策略，保证了 JVM 的高效稳定运行。**不同的 JVM 对于内存的划分方式和管理机制存在着部分差异。**
* 一个进程对应着一个 JVM 实例，也就对应着一个运行时数据区。一个运行时数据区里只有一个堆和一个方法区。
#### 3.1.1 运行时数据区的结构
![图示](https://img-blog.csdnimg.cn/b662fe87b940474688cae34c683e9e2e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_16,color_FFFFFF,t_70,g_se,x_16)
![图示](https://img-blog.csdnimg.cn/800fc7b7ae624591a57f6a6b646626e9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* Java 虚拟机定义了若干种程序运行期间会使用到的运行时数据区，其中有**一些会随着虚拟机启动而创建，随着虚拟机退出而销毁。另外一些则是与线程一一对应的**，这些与线程对应的数据区域会随着线程开始和结束而创建和销毁。
* 每个线程：独立包括程序计数器、栈、本地栈。
* 线程间共享：堆、堆外内存（永久代或元空间、代码缓存）
* 下图中，灰色的为每个线程私有的，红色的为多个线程共享的：
![图示](https://img-blog.csdnimg.cn/3c446c91954b481abf28850446ab9f8c.png)
* **注：95% 的垃圾回收集中在堆区，5% 集中在方法区。方法区在 jdk8 以后换成叫元空间。**
### 3.2 线程
* 线程是一个程序里的执行单元，JVM 允许一个应用有多个线程并行地执行。
* 在 Hotspot JVM 里，每个线程都与操作系统的本地线程直接映射。当一个 Java 线程**准备好执行**以后，此时一个操作系统的本地线程也同时创建。Java 线程执行终止后，本地线程也会回收。
* 操作系统负责所有线程的安排调度到任何一个可用的 CPU 上。一旦本地线程初始化成功，它就会调用 Java 线程中的 run() 方法。
#### 3.2.1 JVM 系统线程
![图示](https://img-blog.csdnimg.cn/f35f5a8163bb4ec196154dcdbd9dbfb6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
## 四、运行时数据区 -- 程序计数器（PC寄存器）
### 4.1 PC Register 介绍
![图示](https://img-blog.csdnimg.cn/b13be63ee4914e1e91224063122a4852.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 作用：**PC 寄存器是用来存储下一条要执行的指令的地址，由存储引擎读取下一条指令**。如下图：
![图示](https://img-blog.csdnimg.cn/c3114a67f86042bdbc795019dc147ec5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_13,color_FFFFFF,t_70,g_se,x_16)
* 程序计数器**是一块很小的内存空间**，几乎可以忽略不计。也是**运行速度最快的存储区域。**
* 在 JVM 规范中，**每个线程都有它自己的程序计数器**，是线程私有的，生命周期与线程的生命周期保持一致。
* 任何时间一个线程都只有一个方法在执行，也就是所谓的**当前方法**。程序计数器会存储当前线程正在执行的 Java 方法的 JVM 指令地址；如果是在执行 native 方法，则是未指定值（undefined）
* 程序计数器是程序控制流的指示器，分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成。
* 字节码解释器工作时就是通过改变这个程序计数器的值来选取下一条需要执行的字节码指令。
* **存在 GC 的：堆和方法区；存在 OOM 的：栈（虚拟机栈、本地方法栈）、堆和方法区。** 也就是说程序计数器既没有 GC，也没有 OOM（Out of Memory）。
### 4.2 举例说明
下面是一段 Java 程序的字节码指令：
![图示](https://img-blog.csdnimg.cn/c0780ba90dd047d7bcbe74decfcde279.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
* **注：执行引擎会读取 PC 寄存器里的值，这个值就是下一条要执行的指令的地址，然后去该地址取出对应的操作指令。** 本例中是 istore_2。
### 4.3 两个常见问题
* 为什么使用 PC 寄存器记录当前线程的执行地址呢？
因为 CPU 需要不停地切换各个线程，这时候切换回来以后，就需要之道接着从哪里开始继续执行。
JVM 的字节码解释器就需要通过改变 PC 寄存器的值来明确下一条应该执行什么样的字节码指令。
* 为什么 PC 寄存器会被设定为线程私有？
线程在并发执行过程中，CPU 需要切换各个线程，这样必然导致经常中断或恢复。**为了能够准确地记录各个线程当前正在执行的字节码指令的地址，方便切换或恢复线程后知道该从哪条指令继续执行，最好的办法是为每一个线程都分配一个 PC 寄存器**。这样一来各个线程之间就可以进行独立计算，从而不会出现相互干扰的情况。
## 五、运行时数据区 -- 虚拟机栈
### 5.1 虚拟机栈概述
#### 5.1.1 虚拟机栈出现的背景
**由于跨平台性的设计，Java 的指令都是根据栈来设计的**。不同平台 CPU 架构不同，所以不能设计为基于寄存器的，因为**基于寄存器的话它和具体的 CPU 耦合度高**。

**优点**是跨平台，指令集小，编译器容易实现。**缺点**是性能比基于寄存器的差，实现同样的功能需要更多的指令。
#### 5.1.2 内存中的堆与栈
* **栈是运行时的单位，而堆是存储的单位。栈管运行，堆管存储。**
* 即：栈解决程序的运行问题，即程序如何执行，或者说如何处理数据。堆解决的是数据存储的问题，即数据怎么放、放在哪。
#### 5.1.3 虚拟机栈的基本内容
* Java 虚拟机栈是什么？
Java 虚拟机栈（Java Virtual Machine Stack），早期也叫做 Java 栈。**每个线程在创建时都会创建一个虚拟机栈，一个线程一个栈。其内部保存着一个个栈帧（Stack Frame），一个方法对应一个栈帧。**
* **虚拟机栈的生命周期和线程一致。**
* 作用：主管 Java 程序的运行，它**保存方法的局部变量（八种基本数据类型、对象的引用地址）**、部分结果，并参与方法的调用和返回。
* 栈的特点：
（1）栈是一种快速有效的分配存储方式，**访问速度仅次于程序计数器。**
（2）JVM 直接对 Java 栈的操作只有两个：**方法执行时压栈，方法结束后弹栈。**
（3）对于栈来说不存在垃圾回收问题。**存在 GC 的：堆和方法区；存在 OOM 的：栈（虚拟机栈、本地方法栈）、堆和方法区。**
#### 5.1.4 栈中可能出现的异常
* Java 虚拟机规范允许 **Java 栈的大小是动态的或者是固定不变的。**
* 如果采用**固定大小**的 Java 虚拟机栈，那每一个线程的 Java 虚拟机栈容量可以在线程创建的时候独立选定。如果线程请求分配的栈容量超过 Java 虚拟机栈允许的最大容量，Java 虚拟机将会抛出一个 **StackOverflowError** 异常。
*  如果 Java 虚拟机栈**可以动态扩展**，并且在尝试扩展的时候无法申请到足够的内存，或者在创建新的线程时没有足够的内存去创建对应的虚拟机栈，那 Java 虚拟机将会抛出一个 **OutOfMemoryError** 异常。
#### 5.1.5 设置栈的内存大小
* 我们可以使用参数 ``-Xss`` 选项来设置线程的最大栈空间，栈的大小直接决定了函数调用的最大可达深度。可以在 idea 的 ``Run->Edit Configurations->VM options（例如填入：-Xss256k）``
### 5.2 栈的存储单位
#### 5.2.1 栈中存储什么
* 每个线程都有自己的栈，栈中的数据都是以**栈帧（Stack Frame）的格式存在。**
* 在这个线程上正在执行的每个方法都各自对应一个栈帧。
* 栈帧是一个内存区块，是一个数据集，维系着方法执行过程中的各种数据信息。
#### 5.2.2 栈的运行原理
* JVM 直接对 Java 栈的操作只有两个，就是**对栈帧的压栈和弹栈**，遵循先进后出的原则。
* 在一个活动线程中，一个时间点只会有一个活动的栈帧。即只有当前正在执行的方法的栈帧（**栈顶栈帧**）是有效的，这个栈帧**被称为当前栈帧**，与当前栈帧对应的方法就是**当前方法**，定义这个方法的类就是**当前类**。
* 执行引擎运行的所有字节码指令只针对当前栈帧进行操作。
* 如果在该方法中调用了其它方法，对应的新的栈帧就会被创建出来，并进行压栈，成为新的当前栈帧。
* 不同线程中所包含的栈帧是不允许存在互相引用的，即不可能在一个栈帧中引用另外一个线程的栈帧。
* 如果当前方法调用了其他方法，方法返回之际，当前栈帧会传回此方法的执行结果给前一个栈帧，接着，虚拟机会将当前栈帧弹栈，使得前一个栈帧成为当前栈帧。
* Java 方法中有两种返回函数的方式，**一种是正常的函数返回，使用 return 指令；另外一种是抛出异常。不管是用哪种方式，都会导致栈帧被弹出。**
#### 5.2.3 栈帧的内部结构
![图示](https://img-blog.csdnimg.cn/c14f58f227aa40cd9d3b3e31d63f796f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* **方法返回地址、动态链接、一些附加信息这三部分也被称为帧数据区。**
![图示](https://img-blog.csdnimg.cn/f16ba48a1e1a4129a007af1a29bfe091.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_13,color_FFFFFF,t_70,g_se,x_16)
### 5.3 局部变量表
* 局部变量表（local variables）也被称为局部变量数组或本地变量表。
* **局部变量表被定义为一个数字数组，主要用于存储方法参数和定义在方法体内的局部变量**，这些数据类型包括各类基本数据类型、对象引用（reference）以及 returnAddress 类型。
* 由于局部变量表是建立在线程的栈上，是线程的私有数据，因此**不存在数据安全问题。**
* **局部变量表所需的容量大小是在编译期确定下来的**，并保存在方法的 Code 属性的 maximum local variables 数据项中。在方法运行期间是不会改变局部变量表的大小的。
* **方法嵌套调用的次数由栈的大小决定**。一般来说，**栈越大，方法嵌套调用次数越多**。对一个函数而言，它的参数和局部变量越多，局部变量表就越大，它的**栈帧也就越大**，以此来满足方法调用所需传递的信息增大的需求。进而函数调用就会占用更多的栈空间，导致其**嵌套调用次数就会减少**。
* **局部变量表中的变量只在当前方法调用中有效**。在方法执行时，虚拟机通过使用局部变量表完成参数值到参数变量列表的传递过程。**当方法调用结束后，随着方法栈帧的销毁，局部变量表也会随之销毁。**
#### 5.3.1  Slot 概念
* 参数值的存放总是在局部变量数组的 index0 开始，到 数组长度-1 的索引结束。
* **局部变量表，最基本的存储单元是 Slot（变量槽）**
* 局部变量表中存放编译器可知的各种基本数据类型（8种）、引用类型（reference）、returnAddress 类型的变量。
* 在局部变量表里，**32 位以内的类型只占用一个 slot（包括 returnAddress 类型），64 位的类型（long 和 double）占用两个 slot。**
* **byte、short、char 在存储前被转换成 int，boolean 也被转换为 int：0 表示 false，1 表示 true。**
#### 5.3.2 对 Slot 的理解
![图示](https://img-blog.csdnimg.cn/bb105e4ba11842c9bc2c5d92708c023c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 为什么在静态方法中没有 this？
**因为 this 变量不存在于静态方法的局部变量表中。**
#### 5.3.3 Slot 的重复利用
* **栈帧中的局部变量表中的槽位是可以重用的**。如果一个局部变量过了其作用域，那么在其作用域之后申明的新的局部变量就很有可能会复用过期局部变量的槽位，从而**达到节省资源的目的。**
* 示例：
```java
public class SlotTest {
    public void test(){
        int a = 0;
        {
            // 变量 b 出了大括号后就销毁了，但数组空间已经开辟了
            int b = 1;
        }
        // 变量 c 是使用之前已经销毁的变量 b 占用的 slot 的位置
        int c = 2;
    }
}
```
![图示](https://img-blog.csdnimg.cn/c31573bcc24f41819a1435ea9ba57e67.png)
#### 5.3.4 成员变量和局部变量的对比
* 成员变量：在使用前，都经历过默认初始化赋值
（1）静态变量：Linking 的 prepare 阶段，给静态变量赋默认值；initial 阶段：给静态变量显示赋值。
（2）实例变量：随着对象的创建，会在堆空间中分配实例变量的空间，并进行默认赋值。
* 局部变量：在使用前，必须要显式赋值，否则无法使用。
#### 5.3.5 补充说明
* 在栈桢中，与性能调优关系最为密切的部分就是前面提到的局部变量表。在方法执行时，虚拟机使用局部变量表完成方法的传递。
* **局部变量表中的变量也是重要的垃圾回收根结点，只要被局部变量表中直接或间接引用的对象都不会被回收。**
### 5.4 操作数栈
* 每一个独立的栈帧中除了包含局部变量表以外，还包含一个后进先出的操作数栈，也可以称之为**表达式栈**。
* **操作数栈在方法的执行过程中，根据字节码指令，往栈中写入数据或提取数据，即入栈/出栈、**
* 某些字节码指令将值压入操作数栈，其余的字节码指令将操作数取出栈，使用它们后再把结果压入栈中。比如：指令复制、交换、求和等操作。
* 操作数栈，**主要用于保存计算过程的中间结果，同时作为计算过程中变量临时的存储空间。**
* 操作数栈就是 JVM 执行引擎的一个工作区，当一个方法刚开始执行的时候，一个新的栈帧就会随之被创建出来，**这个方法的操作数栈是空的**。
* 每一个操作数栈都会拥有一个明确的栈深度用于存储数值，其**所需的最大深度在编译器就定义好了**，保存在方法的 Code 属性中，是 max_stack 的值。
* 栈中的任何一个元素可以是任意的 Java 数据类型。
（1）32 bit 的类型占用一个栈深度单位。
（2）64 bit 的类型占用两个栈深度单位。
* 操作数栈**并非采用访问索引的方式来进行数据访问**的，而是**只能通过入栈、弹栈操作**来完成数据访问。
* **如果被调用的方法带有返回值的话，其返回值将会被压入当前栈帧的操作数栈中**，并更新 PC 寄存器中下一条需要执行的字节码指令。
* 操作数栈中元素的数据类型必须与字节码指令的序列严格匹配，这由编译器在编译期间进行验证，同时在类加载过程中的类检验阶段的数据流分析阶段要再次验证。
* 我们说 **Java 虚拟机的解释引擎是基于栈的执行引擎，其中的栈指的就是操作数栈。**
### 5.5 栈顶缓存技术
基于栈式架构的虚拟机所使用的零地址指令更加紧凑，但完成一项操作的时候必然需要使用更多的入栈和出栈指令，这意味着将需要更多的指令分派和内存读/写次数。

由于操作数是存储在内存中的，因此频繁地执行内存读/写操作必然会影响执行速度。为了解决这个问题，HotSpot JVM 的设计者们提出了**栈顶缓存（ToS）技术：将栈顶元素全部缓存在物理 CPU 的寄存器中，以此降低对内存的读/写次数，提升执行引擎的执行效率。**
### 5.6 动态链接
* **动态链接：指向运行时常量池的方法引用。**
* 每个栈帧内部都包含着一个指向运行时常量池中该栈帧对应方法的引用。包含这个引用的目的就是为了支持当前方法的代码能够实现动态链接（Dynamic Linking）
* 在 Java 源文件被编译到字节码文件中时，所有的变量和方法引用都作为符号引用保存在 class 文件的常量池里。比如：描述一个方法调用了另外的方法时，就是通过常量池中指向方法的符号引用来表示的，那么**动态链接的作用就是为了将这些符号引用转换为调用方法的直接引用。**
* 图示：
![图示](https://img-blog.csdnimg.cn/4429117f461040c6bce0f70b6b13df67.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)

### 5.7 方法的调用：解析与分派
#### 5.7.1 方法的调用
* 在 JVM 中，将符号引用转换为调用方法的直接引用与方法的绑定机制相关。
* **静态链接：**
当一个字节码文件被装载进 JVM 内部时，如果**被调用的目标方法在编译期可知，并且在运行期保持不变**时，这种情况下将调用方法的符号引用转换为直接引用的过程称之为静态链接。
* **动态链接：**
如果**被调用的目标方法在编译期无法被确定下来，也就是说，只能够在程序运行期将调用方法的符号引用转换为直接引用**，由于这种引用转换过程具备动态性，因此也就被称之为动态链接。
* 对应的方法绑定机制为：早期绑定和晚期绑定。**绑定是一个字段、方法或者类在符号引用被替换位直接引用的过程，这仅仅发生一次。**
* **早期绑定：**
早期绑定就是指**被调用的目标方法在编译期可知，并且在运行期保持不变**时，即可将这个方法与所属的类型进行绑定，这样一来，由于明确了被调用的目标方法究竟是哪一个，因此也就可以使用静态链接的方式将符号引用转换为直接引用。
* **晚期绑定：**
如果**被调用的目标方法在编译期无法被确定下来，只能够在程序运行期根据实际的类型绑定相关的方法**，这种绑定方式也就被称之为晚期绑定。
* 随着高级语言的横空出世，类似于 Java 一样的基于面向对象的编程语言如今越来越多，尽管这类编程语言在语法风格上存在一定的差别，但是它们彼此之间始终保持着一个共性，那就是都支持封装、继承和多态等面向对象的特性。既然**这一类的编程语言具备多态特性，那么自然也就具备早期绑定和晚期绑定两种方式。**
* Java 种任何一个普通的方法其实都具备虚函数的特性，他们相当于 C++ 语言中的虚函数（C++ 种则需要使用关键字 virtual 来显示定义）。如果在 Java 程序中不希望某个方法拥有虚函数的特征时，则可以使用关键字 final 来标记这个方法。
#### 5.7.2 虚方法与实方法
* 如果方法在编译器就确定了具体的调用版本，这个版本在运行时是不可变的，这样的方法称为**非虚方法**。
* **静态方法、私有方法、final 方法、实例构造器、父类方法都是非虚方法。**
* 其他方法称为虚方法。
![图示](https://img-blog.csdnimg.cn/0c323aa3066d4b45b5971d1a7d95d254.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 5.7.3 关于 invokedynamic 指令
* JVM 字节码指令一直比较稳定，一直到 Java7 中才增加了一个 invokedynamic 指令，这是**Java为了实现动态类型语言支持而做的一种改进。**
* 但是在 Java7 中并没有提供直接生成 invokedynamic 指令的方法，需要借助 ASM 这种底层字节码工具来产生 invokedynamic 指令。**直到 Java8 的 Lambda 表达式的出现，invokedynamic 指令在 Java 中才有了直接的生成方式。**
* Java7 中增加的动态语言类型支持的本质是对 Java 虚拟机规范的修改，而不是对 Java 语言规则的修改。增加了虚拟机中的方法调用，最直接的受益者就是运行在 Java 平台上的动态语言的编译器。
#### 5.7.4 动态类型语言和静态类型语言
* 动态类型语言和静态类型语言两者的区别就在于对类型的检查是在编译器还是在运行期，满足前者就是静态类型语言，满足后者就是动态类型语言。
* 说得再直白一点就是，**静态类型语言是判断变量自身的类型信息；而动态类型语言是判断变量值的类型信息，变量本身没有类型信息。**
* **Java 是静态类型语言。**
* 例如：
```java
// 变量本身有类型信息
Java: String s = "abc";
// 变量本身没有类型信息，变量值有
JS: var name = "abc";   var name = 123;
```
### 5.8 方法返回地址
* 存放调用该的 pc 寄存器的值。
* 一个方法的结束，有两种方式：
（1）正常执行完成。
（2）出现未处理的异常，非正常退出。
* 无论哪种方式退出，在方法退出后都返回该方法被调用的位置。方法正常退出时，**调用者的 pc 寄存器的值作为返回地址，即调用该方法的指令的下一条指令的地址**。而通过异常退出的，返回地址是要通过异常表来确定，栈帧中一般不会保存这部分信息。
#### 5.8.1 正常退出
* 执行引擎遇到任意一个方法返回的字节码指令，会有返回值传递给上层的方法调用者，简称**正常完成出口**。
* 一个方法在正常调用完成之后需要使用哪一个返回指令还要根据方法返回值的数据类型来决定。比如：
（1）返回值是 boolean、byte、char、short、int 类型，对应的字节码返回指令是 ireturn。
（2）返回值是 long 类型，对应的字节码返回指令是 lreturn；返回值是 float 类型，对应的字节码返回指令是 freturn；返回值是 double 类型，对应的字节码返回指令是 dreturn；返回值是引用类型，对应的字节码返回指令是 ireturn。
（3）返回值是 void 的方法、实例初始化方法、类和接口的初始化方法，对应的字节码返回指令是 return。
#### 5.8.2 遇到异常
* 在方法的执行过程中遇到了异常，并且这个异常没有在方法内进行处理，也就是只要在本方法的异常表中没有搜索到匹配的异常处理器，就会导致方法退出。简称**异常完成出口**。
* 方法执行过程中抛出异常时的异常处理，存储在一个异常处理表，方便在发生异常的时候找到处理异常的代码。
* 异常处理表示例：
![图示](https://img-blog.csdnimg.cn/3893707d93a54857aa11f76917616dc2.png)
#### 5.8.3 两者的区别
* 本质上，方法的退出就是当前栈帧出栈的过程。此时需要恢复上层方法的局部变量表、操作数栈、将返回值压入调用者栈帧的操作数栈、设置 PC 寄存器值等，让调用者方法继续执行下去。
* **正常完成出口和异常完成出口的区别在于：通过异常完成出口退出的不会给他的上层调用者产生任何的返回值。**

### 5.9 一些附加信息
栈帧中还允许携带与 Java 虚拟机实现相关的一些附加信息。例如：对程序调试提供支持的信息。
### 5.10 栈的相关面试题
* 方法中定义的局部变量是否线程安全？
答：具体问题具体分析。
```java
public class ThreadSafty {
    // 该方法中 s 的声明方式是线程安全的
    public static void method1(){
        StringBuilder s = new StringBuilder();
        s.append("a");
        s.append("b");
    }
    // s 的操作过程：是线程不安全的。因为 s 是从外面传进来的，有可能由多个线程所调用
    // 严格上 s 不算是方法内定义变量，算是形参的变量
    public static void method2(StringBuilder s){
        s.append("a");
        s.append("b");
    }
    // s 的操作过程：是线程不安全的。因为将 s 返回出去后就有可能被其他位置上的多个线程所调用
    public static StringBuilder method3(){
        StringBuilder s = new StringBuilder();
        s.append("a");
        s.append("b");
        return s;
    }
    // s 的操作过程：是线程安全的。因为 s 其实就在该方法内部消亡了，没有传到外面去。
    public static String method4(){
        StringBuilder s = new StringBuilder();
        s.append("a");
        s.append("b");
        return s.toString();
    }
}
```
## 六、本地方法接口
### 6.1 什么是本地方法
* 简单地讲，**一个 Native Method 就是一个 Java 调用非 Java 代码的接口**。一个 Native Method 是这样的一个方法：该方法的实现由非 Java 语言实现，比如 C。这个特征并非 Java 所特有，很多其他的编程语言都有这一机制，比如在 C++ 中，你可以使用 extern "C" 告知 C++ 编译器去调用一个 C 的函数。
* 在定义一个 native method 时，并不提供方法体，因为其方法体是由非 Java 语言在外面实现的。
* 本地接口的作用是融合不同的编程语言为 Java 所用，他的初衷是融合 C/C++ 程序。
* 举例：
![图示](https://img-blog.csdnimg.cn/b488f140dee947c4b206185c3cc5da0f.png)
### 6.2 为什么要使用 Native Mehtod
* Java 使用起来非常方便，然而有些层次的任务用 Java 实现起来不容易，或者我们对程序的效率很在意时，问题就来了。
![图示](https://img-blog.csdnimg.cn/71e87890b4d544bb851377589641c75b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 6.3 现状
目前本地方法使用的越来越少了，除非是与硬件有关的应用，比如通过 Java 程序驱动打印机或者 Java 系统管理生产设备。在企业级应用中已经比较少见，因为现在的异构领域间的通信很发达，比如可以使用 Socket 通信，也可以使用 Web Service 等等。

## 七、运行时数据区 -- 本地方法栈
* **Java 虚拟机栈用于管理 Java 方法的调用，而本地方法栈用于管理本地方法的调用。**
* 本地方法栈，也是线程私有的。
* 允许被实现成固定或者是可动态扩展的内存大小
* 如果采用**固定大小**的 Java 虚拟机栈，那每一个线程的本地方法栈容量可以在线程创建的时候独立选定。如果线程请求分配的栈容量超过本地方法栈允许的最大容量，Java 虚拟机将会抛出一个 **StackOverflowError** 异常。
*  如果本地方法栈**可以动态扩展**，并且在尝试扩展的时候无法申请到足够的内存，或者在创建新的线程时没有足够的内存去创建对应的本地方法栈，那 Java 虚拟机将会抛出一个 **OutOfMemoryError** 异常。
* 它的具体做法是 Native Method Stack 中等级 native 方法，在 Execution Engine 执行时加载本地方法库。
* **本地方法栈主要是跟本地方法接口，进而跟本地方法库打交道的，是对它们里面的相关方法入栈的。**
![图示](https://img-blog.csdnimg.cn/09b36c6bb4aa436aa1e4430dc47150dc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
* **当某个线程调用一个本地方法时，它就进入了一个全新的并且不再受虚拟机限制的世界，它和虚拟机拥有同样的权限。**
（1）本地方法可以通过本地方法接口来**访问虚拟机内部的运行时数据区。**
（2）它甚至可以直接使用本地处理器中的寄存器。
（3）直接从本地内存的堆中分配任意数量的内存。
* **并不是所有的 JVM 都支持本地方法**。因为 Java 虚拟机规范并没有明确要求本地方法栈的使用语言、具体实现方式、数据结构等。**如果 JVM 产品不打算支持 native 方法，也可以无需实现本地方法栈。**
* 在 Hotspot JVM 中，直接将本地方法栈和虚拟机栈合二为一。
## 八、运行时数据区 -- 堆
### 8.1 堆的核心概念
* **一个 JVM 实例只存在一个堆空间**，堆也是 Java 内存管理的核心区域。
* Java 堆区在 JVM 启动的时候即被创建，其空间大小也就确定了，堆空间的大小也是可以调节的。
* 《Java 虚拟机规范》规定：**堆可以处于物理上不连续的内存空间中，但逻辑上它应该被视为连续的。**
* 所有的线程共享 Java 堆，在这里还可以划分线程私有的缓冲区（Thread Local Allocation Buffer，TLAB）
* 《Java 虚拟机规范》中对 Java 堆的描述是：所有的对象实例以及数组都应当分配在堆上。但从实际使用的角度看，应该是几乎所有的对象实例都在堆分配内存。
* **在方法结束时，堆中的对象不会马上被移除，仅仅在垃圾收集的时候才会被移除。**
* 堆是 GC（Garbage Collection，垃圾收集器） 执行垃圾回收的重点区域。
#### 堆内存细分
![图示](https://img-blog.csdnimg.cn/32b6d81f52d54c86b17457226db7dd4c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 图示：
![图示](https://img-blog.csdnimg.cn/36156d417efb4b71aacf8086db94d482.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_14,color_FFFFFF,t_70,g_se,x_16)
* Java8 和 Java7 的区别
![图示](https://img-blog.csdnimg.cn/98a8b91def8e4189984db04642169eff.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 8.2 设置堆内存的大小与 OOM
#### 8.2.1 堆空间大小的设置
* Java 堆区用于存储 Java 对象实例，那么堆的大小在 JVM 启动的时候就已经设置好了。
* **设置堆空间大小的参数：**
（1）``-Xms``：用来设置堆空间（年轻代+老年代）的初始内存大小。
（2）``-Xmx``：用来设置堆空间（年轻代+老年代）的最大内存大小。
* 默认情况下，初始内存大小：物理电脑内存大小/64；最大内存大小：物理电脑内存大小/4。
* 通常会**将 -Xms 和 -Xmx 两个参数配置相同的值**，其目的是为了能够在 Java 垃圾回收机制清理完堆区后不再需要重新调整堆区的大小，从而提升性能，不会造成系统额外的压力。
* 一旦**堆区中的内存大小超过 “-Xmx” 所指定的最大内存**时，将会抛出 OutOfMemoryError 异常。
* 如何查看设置的参数：``-XX:+PrintGCDetails``
#### 8.2.2 实例
* 参数设置：
![图示](https://img-blog.csdnimg.cn/b41f28c42e7245ffa27cab9c079caa8f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
* Java 代码：
```java
public class HeapSpace {
    public static void main(String[] args) {
        // 返回 Java 虚拟机中的初始堆内存大小
        long initialSize = Runtime.getRuntime().totalMemory() / 1024 / 1024;
        // 返回 Java 虚拟机中的最大堆内存大小
        long maxSize = Runtime.getRuntime().maxMemory() / 1024 / 1024;

        System.out.println("-Xms: " + initialSize + "M"); // -Xms: 575M
        System.out.println("-Xmx: " + maxSize + "M"); // -Xmx: 575M
    }
}
```
* 运行结果：
![图示](https://img-blog.csdnimg.cn/11bad49af5ee4e8290433f289ccd0902.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 为什么设置的堆内存空间是 600m，但实际只有 575m ？
因为**新生代认为有一个幸存者区是不放数据的**，因此算内存的时候只算一份幸存者区。比如通过``-XX:+PrintGCDetails``我们可知，新生代的 total 是 179200K，而 eden + from + to 一共有 204 800 k，但因为只算一份幸存者区的内存，所以结果是 153600 + 25600 = 179200。
所以 600m - 25600/1024 = 575m
### 8.3 年轻代与老年代
![图示](https://img-blog.csdnimg.cn/b3accffe2d33462b8a006d065b8534f1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 配置新生代与老年代的占比
![图示](https://img-blog.csdnimg.cn/00847cb83d0f49d6845d83029e2638d5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 配置新生代内部区域的占比
* 在 HotSpot 中，Eden 空间和另外两个 Survivor 空间**默认所占的比例是 8:1:1，但是在实际使用时是 6:1:1**，因为存在自适应的机制。**如果想要在实际使用时是 8:1:1，可以使用参数``-XX:SurvivorRatio=8``**
* 开发人员可以通过选项“-XX:SurvivorRatio=8” 调整这个空间比例。
* **几乎所有的** Java 对象都是在 Eden 区被 new 出来的。
* 绝大部分的 Java 对象的销毁都在新生代进行了。IBM 公司研究表明：新生代中 80% 的对象都是朝生夕死的。
* 可以使用选项 “-Xmn” 设置新生代最大内存大小。这个参数一般使用默认值就好了，因为在设置新生代与老年代的比例的时候相当于已经设置好新生代的大小了。

### 8.4 对象的分配过程
* 为新对象分配内存是一件非常严谨和复杂的任务，JVM 的设计者们不仅需要考虑内存如何分配、在哪里分配等问题，并且由于内存分配算法与内存回收算法密切相关，所以还需要考虑 GC 执行完内存回收后是否会在内存空间中产生内存碎片。
#### 8.4.1 对象分配的一般过程
1. new 的对象先放到伊甸园区。此区有大小限制。
2. 当伊甸园区的空间满了之后，如果程序又需要创建对象，此时 JVM 的垃圾回收器会对伊甸园区进行垃圾回收（YGC/Minor GC），将伊甸园区中的不再被其他对象所引用的对象进行销毁，然后将伊甸园区中剩余的对象移动到幸存者区 0 区，**此时伊甸园区就完全为空了**。
3. 接下来我们再在伊甸园区中存放对象。如果在放的过程中伊甸园区又满了，此时又会触发 YGC：将垃圾回收掉，并且将剩余的对象放到幸存者 1 区，同时还会将上次幸存下来放到幸存者 0 区的对象，如果还需要被使用（也就是没被回收），也会放到幸存者 1 区。此时它们的 age 会自动增加。
4. 什么时候能去养老区呢？可以设置次数，**默认（阈值）是 15 次**。可以设置参数：``-XX:MaxTenuringThreshold=<N>``进行设置。
5. 在养老区，相对悠闲。当养老区内存不足时，再次触发 GC：Major GC，进行养老区的内存清理。
6. 若养老区执行了 Major GC 之后发现依然无法进行对象的保存，就会产生 OOM 异常。``java.lang.OutOfMemoryError: Java heap space``
7. 总结：
（1）**我们为每个对象分配了一个年龄计数器 age。在伊甸园区放到幸存者区的时候我们把 age 赋值为 1。**
（2）**当 Eden 区满的时候会触发YGC（Minor GC），YGC 会将 Eden 区和 Survivor 区一起进行垃圾回收。Survivor 区满的时候不会触发 YGC**
（3）**进行完一次 YGC 后，伊甸园区就会为空。**
（4）**针对幸存者 S0、S1 区的总结：复制之后有交换，谁空谁是 to。**
（5）**关于垃圾回收：频繁在新生代收集，很少在养老区收集，几乎不在永久代/元空间收集。**
* 图示：
![图示](https://img-blog.csdnimg.cn/1bc74c5009034627a1725bc7ddec96e5.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 8.4.2 对象分配的特殊情况
1. 如果进行完 YGC 之后，伊甸园区仍然放不下新的对象，此时这个新的对象会直接存放到老年代。如果老年代存放的下就放在老年代；如果老年代也存放不下，此时有两种情况：第一种是本来老年代空间足够，但此时老年代已经存放了其他对象，导致存放不了新的对象，因此会先进行 FGC，如果垃圾回收完之后能放进老年代就放进，如果还是不够那就直接报 OOM。第二种是老年代空间本来就放不下这个新的对象，因此也会报 OOM。
2. 如果空的 to 区放不下从伊甸园区中过来的对象，那么我们会直接把对象放到老年代。
![图示](https://img-blog.csdnimg.cn/0a077bd45b674aa08a277a2d3342ee6a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_16,color_FFFFFF,t_70,g_se,x_16)
### 8.5 Minor GC、Major GC、Full GC
#### 8.5.1 概述
![图示](https://img-blog.csdnimg.cn/c3c73ae976b44916831448c14064724c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
#### 8.5.2 年轻代GC（Minor GC） 的触发机制
* 当年轻代空间不足时，就会触发 Minor GC，这里的年轻代指的是 Eden 区满，Survivor 区满不会引发 GC。每次 Minor GC 会清理年轻代的内存。
* 因为 Java 对象**大多都具备朝生夕死**的特性，所以 Minor GC 非常频繁，一般回收速度也比较快。这一定义既清晰又易于理解。
* Minor GC 会**引发 STW**，**暂停其它用户的线程**，等垃圾回收结束，用户线程才恢复运行。
#### 8.5.3 老年代GC（Major GC） 的触发机制
* 指发生在老年代的 GC，对象从老年代消失时，我们说 Major GC 发生了。
* 出现了 Major GC，经常会伴随至少一次的 Minor GC（但非绝对的，在 Parallel Scavenge 收集器的收集策略里就有直接进行 Major GC 的策略选择过程）
* 也就是在老年代空间不足时，会先尝试触发 Minor GC，如果之后空间还不足，则触发 Major GC。
* Major GC 的速度一般会比 Minor GC 慢 10 倍以上，STW 的时间更长。
* 如果 Major GC 后，内存还不足，就报 OOM 了。
#### 8.5.4 Full GC 的触发机制
* 触发 Full GC 执行的情况有如下五种：
（1）调用 System.gc() 时，系统建议执行 Full GC，但是不必然执行。
（2）老年代空间不足。
（3）方法区空间不足。
（4）通过 Minor GC 后进入老年代的平均大小大于老年代的可用内存。
（5）由 Eden 区、survivor space0(From Space)区向 survivor space1(To Space)区复制时，对象大小大于 To Space 可用内存，则把该对象转存到老年代，且老年代的可用内存小于该对象大小。
* 注：**full gc 是开发或调优中要尽量避免的，这样暂时时间会短一点。**
### 8.6 堆空间的分代思想
#### 为什么需要把 Java 堆分代？
* 经研究，不同对象的生命周期不同。70% - 99% 的对象是临时对象。
（1）新生代：由 Eden、两块大小相同的 Survivor（又称为 from/to，s0/s1）构成，to 总为空。
（2）老年代：存放新生代中经历多次 GC 仍然存活的对象。
* 其实不分代完全可以，分代的唯一理由就是**优化 GC 性能**。如果没有分代，那所有的对象都在一块，就如同把一个学校的人都关在一个教室，GC 的时候要找到哪些对象没用，这样就会对堆的所有区域进行扫描，而很多对象都是朝生夕死的，如果分代的话，把新创建的对象放到某一地方，当 GC 的时候先把这块存储 “朝生夕死对象” 的区域进行回收，这样就会腾出很大的空间出来。
### 8.7 内存分配策略（对象提升规则）
* 如果对象在 Eden 出生并经过第一次 Minor GC 后仍然存活，并且能被 Survivor 容纳的话，将被移动到 Surviror 空间中，并将对象的年龄设置为 1.对象在 Survivor 区中每过一次 Minor GC，年龄就增加 1 岁，当它的年龄增加到一定程度（默认是 15 岁，其实每个 JVM、每个 GC 都有所不同，就会被晋升到老年代）
* 对象晋升老年代的年龄阈值，可以通过选项``-XX:MaxTenuringThreshold``来设置。
![图示](https://img-blog.csdnimg.cn/7353208865764efdb029f6a7a8a3e6d4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_17,color_FFFFFF,t_70,g_se,x_16)
### 8.8 为对象分配内存：TLAB
#### 8.8.1 为什么有 TLAB（Thread Local Allocation Buffer）
* 堆区是**线程共享区域**，任何线程都可以访问到堆区中的共享数据。
* 由于线程实例的创建在 JVM 中非常频繁，因此在并发环境下从堆区中划分内存空间是**线程不安全**的。
* 为避免多个线程操作同一地址，需要使用加锁等机制，进而影响分配速度。
#### 8.8.2 什么是 TLAB
* 从内存模型而不是垃圾收集的角度，对 Eden 区域继续进行划分，JVM 为**每个线程分配了一个私有缓存区域**，它包含在 Eden 空间内。
* 多线程同时分配内存时，使用 TLAB 可以避免一系列的非线程安全问题，同时还能够提升内存分配的吞吐量，因此我们可以将这种内存分配方式称之为**快速分配策略**。
* 尽管不是所有的对象实例都能够在 TLAB 中成功分配内存，但 **JVM 确实是将 TLAB 作为内存分配的首选。**
* 默认情况下，TLAB 空间的内存非常小，**仅占有整个 Eden 空间的 1%**，当然我们可以通过选项``-XX:TLABWasteTargetPercent`` 设置 TLAB 空间所占 Eden 空间的百分比大小。
* 在程序中，开发人员可以通过选项``-XX:UseTLAB``设置是否开启 TLAB 空间，默认是开启了 TLAB 空间。
* 一旦对象在 TLAB 空间分配内存失败时，JVM 就会尝试着通过**使用加锁机制**确保数据操作的原子性，从而直接在 Eden 空间中分配内存。
* 目前所有 OpenJDK 衍生出来的 JVM 都提供了 TLAB 的设计。
* TLAB 示意图：
![图示](https://img-blog.csdnimg.cn/13f93c15ef004e81965fd7de9b91333d.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_16,color_FFFFFF,t_70,g_se,x_16)
#### 8.8.3 对象分配过程（加上 TLAB）
![图示](https://img-blog.csdnimg.cn/eacddb739ed0428aa49ed8e03b1afc65.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 8.9 小结堆空间的参数设置
#### 8.9.1 常见的参数设置
![图示](https://img-blog.csdnimg.cn/3650918f7d5c43efa752dd07be6f12bc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 8.9.2 空间分配担保
![图示](https://img-blog.csdnimg.cn/f6e00b360b5d4628a0d3e607f7f82c52.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 在 JDK Update24 之后（JDK7），HandlePromotionFailure 参数不会再影响到虚拟机的空间分配担保策略（可以理解为**HandlePromotionFailure 一直等于 true**）。观察 OpenJDK 中的源码变化，虽然源码中还定义了 HandlePromotionFailure 参数，但是在代码中已经不会再使用它了。JDK6 Update24 之后的规则变为：**只要老年代最大可用的连续空间大于新生代所有对象的总大小或者大于历次晋升到老年代的对象的平均大小，就会进行 Minor GC，否则将进行 Full GC。**
### 8.10 堆是分配对象的唯一选择吗？
* 在《深入理解Java虚拟机》中关于 Java 堆内存有这样一段描述：随着 JIT 编译器的发展和 **逃逸分析技术（Escape Analysis）** 逐渐成熟，**栈上分配、标量替换优化技术**将会导致一些微妙的变化，所有的对象都分配到堆上也渐渐变得不那么绝对了。
* 在 Java 虚拟机中，对象是在 Java 堆中分配内存的，这是一个普遍的常识。但是，有一种特殊情况，那就是**如果经过逃逸分析后发现，一个对象并没有逃逸出方法的话，那么就可能被优化成栈上分配**。这样就无需在堆上分配内存，也无需进行垃圾回收了。这就是最常见的堆外存储技术。
* 此外，基于 OpenJDK 深度定制的 TaoBaoVM，其中创新的 GCIH（GC invisible heap）技术实现 off-heap，将生命周期较长的 Java 对象从 heap 中移至 heap 外，并且 GC 不能管理 GCIH 内部的 Java 对象，以此达到降低 GC 的回收频率和提升 GC 的回收效率的目的。
#### 逃逸分析概述
![图示](https://img-blog.csdnimg.cn/a876d8fc60fc4b82ba1a9f79b4e09499.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 参数设置：
（1）**在 JDK7 之后，HotSpot 中默认就已经开启了逃逸分析。**
（2）如果使用的是较早的版本，开发人员可以通过选项``-XX:+DoEscapeAnalysis``显示开启逃逸分析，还可以通过选项``-XX:+PrintEscapeAnalysis``查看逃逸分析的筛选结果。
* **如何快速判断是否发生逃逸：大家就看 new 的对象实体是否有可能在方法外被使用。**
* 示例：
```java
public class EscapeAnalysis {
    // 如何快速判断是否发生逃逸：大家就看 new 的对象实体是否有可能在方法外被使用。
    public EscapeAnalysis obj;
    // 方法返回 EscapeAnalysis 对象，发生逃逸。
    public EscapeAnalysis getInstance(){
        return obj == null ? new EscapeAnalysis() : obj;
    }
    // 为成员属性赋值，发生逃逸
    public void setObj(){
        obj = new EscapeAnalysis();
    }
    // 对象的作用域仅在当前方法内有效，没有发生逃逸
    public void useEscapeAnalysis(){
        EscapeAnalysis e = new EscapeAnalysis();
    }
    // 引用成员变量的值，发生逃逸。因为判断的是对象实体能否被方法外调用，对象实体才是放在堆空间中的。变量 e 对应的对象实体可以通过 obj 在方法外被调用
    public void useEscapeAnalysis1(){
        EscapeAnalysis e = getInstance();
    }
}
```
* 结论：开发中能使用局部变量的，就不要在方法外定义。
#### 逃逸分析：代码优化
* 使用逃逸分析，编译器可以对代码做如下优化：
（1）**栈上分配**。将堆分配转化为栈分配。如果一个对象在子程序中被分配，要使指向该对象的指针永远不会逃逸，对象可能是栈分配的候选，而不是堆分配。
（2）**同步省略**。如果一个对象被发现只能从一个线程被访问到，那么对于这个对象的操作可以不考虑同步。
（3）**分离对象或标量替换**。有的对象可能不需要作为一个连续的内存结构存在也可以被访问到，那么对象的部分（或全部）可以不存储在内存，而是存储在 CPU 寄存器中。
##### 代码优化之栈上分配
* JIT 编译器在编译期间根据逃逸分析的结果，发现如果一个对象并没有逃逸出方法的话，就可能被优化成栈上分配。分配完成后，继续在调用栈内执行，最后线程结束，栈空间被回收，局部变量对象也被回收。这样就**无需进行垃圾回收**了。
##### 代码优化之同步省略（锁消除）
* 线程同步的代价是相当高的，同步的后果是降低并发性和性能。
* 在动态编译同步代码块的时候，JIT 编译器可以借助逃逸分析来**判断同步代码块所使用的锁对象是否只能够被一个线程访问而没有被发布到其他线程**。如果没有，那么 JIT 编译器在编译这个同步代码块的时候就会取消对这部分代码的同步。这样就能大大提高并发性和性能。这个取消同步的过程就叫同步省略，也叫锁消除。
##### 代码优化之标量替换
* **标量（Scalar）** 是指一个无法再分解成更小的数据的数据。比如：Java 中的基本数据类型就是标量。
* 相对的，那些还可以分解的数据叫做**聚合量（Aggregate）**，Java 中的对象就是聚合量，因为它可以分解成其它聚合两和标量。
* 在 JIT 阶段，如果经过逃逸分析，发现一个对象不会被外界访问的话，那么经过 JIT 优化，就会把这个对象拆解成其中包含的若干个成员变量来代替，这个过程就是**标量替换**。
* 标量替换的参数设置：``-XX:+EliminateAllocations``：**开启了标量替换（默认打开）**，允许将对象打散分配在栈上。
* 示例：
![图示](https://img-blog.csdnimg.cn/a1d7440eec5b449ca2b5f834ead10e41.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_17,color_FFFFFF,t_70,g_se,x_16)
#### 逃逸分析小结：逃逸分析并不成熟
![图示](https://img-blog.csdnimg.cn/3cfd348f7d4f41ef885d28501c0d0979.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
## 九、运行时数据区 -- 方法区
### 9.1 栈、堆、方法区的交互关系
#### 9.1.1 运行时数据区结构图
![图示](https://img-blog.csdnimg.cn/8c299e652bc24d12bd085bbad0e3ecbc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 从线程共享与否的角度来看：
![图示](https://img-blog.csdnimg.cn/72859babf22c4791ad268c416670dbda.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
**注：堆、元空间既有 GC，又有 OOM；虚拟机栈、本地方法栈有 OOM，没有 GC；程序计数器既没有 GC，也没有 OOM。**
#### 9.1.2 栈、堆、方法区交互关系
![图示](https://img-blog.csdnimg.cn/ec1eb8a155cd46acb6c58abcd20cb9e0.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 9.2 方法区的理解
#### 9.2.1 方法区在哪里
* 《Java虚拟机规范》中明确说明：“尽管**所有的方法区在逻辑上是属于堆的一部分**，但一些简单的实现可能不会选择去进行垃圾收集或者进行压缩。” 但对于 HotSpotJVM 而言，方法区还有一个别名叫作 Non-Heap（非堆），目的就是要和堆分开。所以，**方法区看作是一块独立于 Java 堆的内存空间。**
![图示](https://img-blog.csdnimg.cn/061b3c910e7b40dd92cb31f79aac4dd9.png)
#### 9.2.2 方法区的基本理解
* 方法区与 Java 堆一样，是各个线程共享的内存区域。
* 方法区在 JVM 启动的时候被创建，**并且它的实际物理内存空间和 Java 堆区一样都是可以不连续的。**
* 方法区的大小和堆空间一样，可以选择固定大小或者可扩展。
* **方法区的大小决定了系统可以保存多少个类**，如果系统定义了太多的类，导致方法区溢出，虚拟机同样会抛出内存溢出错误：java.lang.OutOfMemoryError: PermGen space 或者java.lang.OutOfMemoryError: Metaspace。
* 方法区在 JVM 启动的时候被创建好，关闭 JVM 就会释放方法区的内存。
#### 9.2.3 HotSpot 中方法区的演进
* **在 jdk7 及以前，习惯上把方法区称为永久代。jdk8 开始，使用元空间取代了永久代。**
* 注：我们可以把方法区看成是接口，把永久代和元空间看作是方法区不同的实现。
* 本质上，方法区和永久代并不等价，它们仅在 HotSpot 虚拟机中等价。《Java虚拟机规范》对如何实现方法区不做统一要求。例如：BEA JRockit / IBM J9 中不存在永久代的概念。
* 现在看来，当年使用永久代不是一个好的选择，它导致了 Java 程序更容易 OOM（超过 -XX:MaxPermSize 上限） ，因为永久代使用的是 Java 虚拟机的内存。
* 在 jdk8 中，HotSpot 完全废弃了永久代的概念，改用和 JRockit、J9 一样**在本地内存中实现的元空间（Metaspace）** 来代替。
* 元空间的本质和永久代类似，**都是对 JVM 规范中方法区的实现**。但是元空间与永久代最大的区别在于：**元空间不在虚拟机设置的内存中，而是使用本地内存。**
* 永久代、元空间不只是名字变了，内部结构也调整了。
* 根据《Java虚拟机规范》的规定，如果方法区无法满足新的内存分配需求时，将抛出 OOM 异常。
* jdk7 和 jdk8 的区别：
![图示](https://img-blog.csdnimg.cn/d7d906d2589c4eb5809f739ea3cb1fd9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_16,color_FFFFFF,t_70,g_se,x_16)
### 9.3 设置方法区大小
#### 9.3.1 设置方法区内存的大小
* 方法区的大小可以是固定不变的，也可以设置成根据应用的需要动态调整。
##### JDK7 及以前
* **通过 ``-XX:PermSize`` 来设置永久代初始分配空间。默认值是 20.75 M。**
* **通过 ``-XX:MaxPermSize`` 来设置永久代最大可分配空间。32 位机器默认是 64 M，64 位及其默认是 82 M。**
* 当 JVM 加载的类信息容量超过了最大可分配空间，会报异常：OutOfMemoryError: PermGen space
##### JDK8 及以后
* **通过 ``-XX:MetaspaceSize`` 来设置元空间初始分配空间；通过 ``-XX:MaxMetaspaceSize`` 来设置元空间最大可分配空间。默认值依赖于平台。Windows 下，``-XX:MetaspaceSize`` 是 21 M，``-XX:MaxMetaspaceSize`` 的值是 -1，表示没有限制**
* 与永久代不同，如果不指定大小，默认情况下，虚拟机会耗尽所有的可用系统内存。如果元空间发生溢出，虚拟机一样会抛出异常：OutOfMemoryError: Metaspace
* ``-XX:MetaspaceSize``：设置元空间的初始大小。对于一个 64 位的服务器端 JVM 来说，**``-XX:MetaspaceSize`` 的默认值是 21 M。这就是初始的高水位线，一旦触及这个水位线，Full GC 将会被触发并卸载没用的类**（即这些类对应的类加载器不再存活），然后这个高水位线将会重置。新的高水位线的值取决于 GC 后释放了多少元空间。如果释放的空间不足，那么在不超过 MaxMetaspaceSize 时，适当提高该值。如果释放空间过多，则适当降低该值。
* 如果初始化的高水位线设置过低，上述高水位线调整情况会发生很多次。通过垃圾回收的日志可以观察到 Full GC 多次调用。**为了避免频繁地 GC，建议将 ``-XX:MetaspaceSize`` 设置为一个相对较高的值。**
### 9.4 方法区的内部结构
#### 9.4.1 方法区中存储什么
* 《深入理解Java虚拟机》中堆方法区存储内容的描述如下：它用于存储已被虚拟机加载的**类型信息、常量、静态变量、即时编译器编译后的代码缓存等。**
##### 类型信息
![图示](https://img-blog.csdnimg.cn/2fe00c2a7b0547c6a01ee9f9e1082f70.png)
##### 域（Field、成员变量、属性）信息
* JVM 必须在方法区中保存类型的所有域的相关信息以及域的声明顺序。
* 域的相关信息包括：域名称、域类型、域修饰符（public、private、protected、static、final、volatile、transient 的一个子集）
##### 方法信息
![图示](https://img-blog.csdnimg.cn/df57c81de7e44b8e8ec11e80433df939.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
##### non-final 的静态变量
* 静态变量和类关联在一起，**随着类的加载而加载**，他们成为类数据在逻辑上的一部分。
* 类变量被类的所有实例共享，即使没有类的实例你也可以访问它。
##### 全局常量：static final
* 被声明为 final 的静态变量的处理方法则不同，**每个全局常量在编译的时候就被赋上值了。**
#### 9.4.2 运行时常量池 和 常量池
* 方法区内部包含了运行时常量池；字节码文件内部包含了常量池。
* **字节码文件当中的常量池被加载到方法区以后对应的结构就称为运行时常量池。**
* 字节码文件：
![图示](https://img-blog.csdnimg.cn/96048213afbc4c829a9386ef101ba5f9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
一个**有效的字节码文件中除了包含类的版本信息、字段、方法以及接口等描述信息外**，还包含一项信息那就是**常量池表**（Constant Pool Table），**包括各种字面量以及对类型、域和方法的符号引用**。

* **注：常量池可以看作是一张表，虚拟机指令根据这张常量表找到要执行的类名、方法名、参数类型、字面量等数据。**
#### 为什么需要常量池
* 一个 Java 源文件中的类、接口，编译后会产生一个字节码文件。而 Java 中的字节码需要数据支持，通常这种数据会很大以至于不能直接存到字节码里，因此可以换一种方式：存到常量池里。这个字节码包含了指向常量池的引用
#### 运行时常量池
* 运行时常量池是方法区的一部分。
* 常量池表是字节码文件的一部分，**用于存放编译期生成的各种字面量和符号引用，这部分内容在类加载后存放到方法区的运行时常量池中。**
* **运行时常量池**相对于字节码文件中的常量值的另一个重要特征是：**具备动态性**。
* **JVM 为每个已加载的类型（类或者接口）都维护一个常量池**。池中的数据项像数组项一样，通过索引访问。
* 运行时常量池包含多种不同的常量，包括编译器就已经明确的数值字面量，也包括到运行期解析后才能够获得的方法或者字段引用。此时不再是常量池中的符号地址了，而是真实地址。
* 在加载类或接口到虚拟机后，就会创建对应的运行时常量池。
* 运行时常量池类似于传统编程语言中的符号表，但是它所包含的数据却比符号表要更丰富一些。
* 当创建类或接口的运行时常量池时，如果构造运行时常量池所需要的内存空间超出了方法区所能提供的最大值，那么 JVM 会抛出 OutOfMemoryError 异常。

### 9.5 方法区的演进细节
#### 方法区的演进细节
* 首先明确：**只有 HotSpot 才有永久代**。对 BEA JRockit、IBM J9 等来说，是不存在永久代的概念的。原则上如何实现方法区属于虚拟机实现细节，不受《Java虚拟机规范》管束，并不要求统一。
* HotSpot 中方法区的变化：
![图示](https://img-blog.csdnimg.cn/35913aa677a142c4989d5cb6127c2e50.png)
![图示](https://img-blog.csdnimg.cn/e4515bc9119840a781bfd5c1891362d6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 注：静态引用对应的对象实体（就是 new 出来的对象）始终存放在堆空间中，而静态变量本身在 jdk7 以后就存放在堆中了。
#### 为什么要用元空间替换永久代？
* 随着 Java8 的到来，HotSpot VM 中再也见不到永久代了。但是这并不意味着类的元数据信息也消失了。这些数据被移到了一个**与堆不相连的本地内存区域：元空间。**
* 由于类的元数据分配在本地内存中，元空间的最大可分配空间就是系统可用的内存空间。
* 这项改动是很有必要的，原因有：
（1）**很难确定永久代的空间大小，难以设置**。在某些场景下，如果动态加载类过多，容易产生 Perm 区的 OOM。比如某个实际 Web 工程中，因为功能点比较多，在运行过程中需要不断动态加载很多类，经常出现致命错误。而元空间和永久代之间的最大区别在于：元空间并不在虚拟机中，而是使用本地内存。因此默认情况下，元空间的大小仅受本地内存的限制。
（2）**对永久代进行调优也比较困难。**
#### StringTable 为什么要调整？
* jdk7 中将 StringTable 放到了堆空间中。因为永久代的回收效率很低，而且只有在 full gc 的时候才会触发永久代的回收。而 full gc 是老年代的空间不足或者永久代不足时才会触发。这就导致了 StringTable 的回收效率不高。而我们开发中会有大量的字符串被创建，回收效率低，导致永久代的内存不足。放到堆里，能够及时回收内存。
### 9.6 方法区的垃圾回收
* 有些人认为方法区（如 HotSpot 虚拟机中的元空间或者永久代）是没有垃圾收集行为的，其实不然。《Java虚拟机规范》对方法区的的约束是非常宽松的，提到过可以不要求虚拟机在方法区中实现垃圾收集。事实上也确实有未实现或未能完整实现方法区类型卸载的收集器存在（如 JDK11 时期的 ZGC 收集器就不支持类卸载）
* 一般来说**方法区的回收效果比较难令人满意，尤其是类型的卸载，条件相当苛刻**。但是这部分区域的回收**有时又确实是必要**的。以前 Sun 公司的 Bug 列表中，有许多严重的 Bug 就是由于低版本的 HotSpot 虚拟机对此区域未完全回收而导致内存泄漏。
* **方法区的垃圾收集主要回收两部分内容：常量池中废弃的常量和不再使用的类型。**
#### 废弃常量的回收
* 方法区内常量池中主要存放的两大类常量：字面量和符号引用。字面量比较接近 Java 语言层次的常量概念，比如文本字符串、被声明为 final 的常量值等。而符号引用则属于编译原理方面的概念，包括下面三类常量：
（1）类和接口的全限定名
（2）字段的名称和描述符
（3）方法的名称和描述符
* HotSpot 虚拟机对常量池的回收策略是很明确的，**只要常量池中的常量没有被任何地方引用，就可以被回收。**
* 回收废弃常量与回收 Java 堆中的对象非常类似。
#### 不再使用的类型的回收
![图示](https://img-blog.csdnimg.cn/7a19d0c424de4d1ebaaf568cc6dae96f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 9.7 总结
![图示](https://img-blog.csdnimg.cn/e84611e1e7464d9691b5f98b08d45fa3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
## 十、对象的实例化、内存布局与访问定位
### 10.1 对象的实例化
![图示](https://img-blog.csdnimg.cn/c90aaefa73724084a4821fd4f476610a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 创建对象的具体步骤：
![图示](https://img-blog.csdnimg.cn/ee72caddb393418b9848bcdf9bb160b1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 10.2 对象的内存布局
![图示](https://img-blog.csdnimg.cn/cdb6663ef5b14d1a991dd0c9d13f3e21.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 示例
```java
public class CustomerTest {
    public static void main(String[] args) {
        Customer cust = new Customer();
    }
}
class Customer{
    int id = 1001;
    String name;
    Account acct;
    {
        name = "匿名用户";
    }
    public Customer(){
        acct = new Account();
    }
}
class Account{}
```
![图示](https://img-blog.csdnimg.cn/c5b9dc18b1184bb9aae115e2de26b03b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 10.3 对象的访问定位
#### 10.3.1 JVM 是如何通过栈帧中的对象引用访问到其内部的对象实例的呢？
![图示](https://img-blog.csdnimg.cn/d9811efabd86483abd553de85068fae1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_14,color_FFFFFF,t_70,g_se,x_16)
#### 10.3.2 对象访问的方式
* 对象访问的方式主要有两种：
（1）句柄访问
（2）直接指针（HotSpot 采用）
##### 句柄访问
![图示](https://img-blog.csdnimg.cn/f76d57b951044876ab69ebe4552f8d40.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 优点：对象引用 reference 中存储稳定的句柄地址，对象被移动（比如垃圾回收时移动对象很普遍，比如对象在 s0 区和 s1 区的移动）时只需要改变句柄中的实例数据指针即可，而 reference 本身不需要被修改。
#### 直接指针（HotSpot 采用）
![图示](https://img-blog.csdnimg.cn/79bf93dc0ed64912bd3254ac3b5dd49a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 优点：
（1）句柄访问必须开辟一块空间来存放句柄，浪费空间；而直接指针方式不会。
（2）如果想访问对象的话，句柄访问方式必须先通过对象引用找到句柄，再通过句柄访问对象实例，效率较低；而直接指针方式则可以直接通过对象引用访问到对象实例。
## 十一、直接内存
* 直接内存不是虚拟机运行时数据区的一部分，也不是《Java虚拟机规范》中定义的内存区域。
* **直接内存是在 Java 堆外的、直接向系统申请的内存空间。**
* 来源于 NIO，通过存在堆中的 DirectByteBuffer 来操作 Native 内存。
* 通常，**访问直接内存的速度会优于 Java 堆**，即读写性能高
（1）因此出于性能考虑，读写频繁的场所可能会考虑使用直接内存。
（2）Java 的 NIO 库允许 Java 程序使用直接内存，用于数据缓冲区。
* 直接内存也可能导致 OutOfMemoryError：Direct buffer memory
* 由于直接内存在 Java 堆外，因此它的大小不会直接受限于 -Xmx 指定的最大堆大小，但是系统内存是有限的，**Java 堆和直接内存的总和依然受限于操作系统能给出的最大内存。**
* 直接内存的缺点：
（1）分配回收成本较高。
（2）不受 JVM 内存回收管理。
* 直接内存的大小可以通过``-XX:MaxDirectMemorySize``设置，如果不指定，**默认与堆的最大值 -Xmx 参数值一致**。
## 十二、执行引擎（Execution Engine）
### 12.1 执行引擎概述
* 执行引擎是 Java 虚拟机核心的组成部分之一。
* “虚拟机” 是一个相对于“物理机”的概念，这两种机器都有代码执行能力，其区别是物理机的执行引擎是直接建立在处理器、缓存、指令集和操作系统层面上的，而**虚拟机的执行引擎则是由软件自行实现的**，因此可以不受物理条件制约地定制指令集与执行引擎的结构体系，**能够执行那些不被硬件直接支持的指令集格式**。
* JVM 的主要内容是负责**装载字节码到其内部，但字节码并不能够直接运行在操作系统之上**，因为字节码指令并非等价于本地机器指令，它内部包含的仅仅只是一些能够被 JVM 所识别的字节码指令、符号表，以及其他辅助信息。
那么，如果想要让一个 Java 程序运行起来，**执行引擎的任务就是将字节码指令解释/编译为对应平台上的本地机器指令**。简单来说，JVM 中的执行引擎充当了将高级语言翻译成机器语言的译者。
* 从外观上来看，所有 Java 虚拟机的执行引擎输入、输出都是一致的：输入的是字节码二进制流，处理过程是字节码解析执行的等效过程，输出的是执行结果。
#### 示图
![图示](https://img-blog.csdnimg.cn/d821ea31eede47df82ebb7578ddb0715.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 执行引擎的工作过程
![图示](https://img-blog.csdnimg.cn/e6ecf56837df4c83935635d1ee9bc764.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 12.2 Java 代码编译和执行过程
#### 前端编译与后端编译
* 前端编译：将 Java 程序编译成字节码文件。
* 后端编译：将字节码文件翻译成为机器指令去执行。
#### 示图
![图示](https://img-blog.csdnimg.cn/a1ac757abe3a4e7d9402edc6ac3bdcca.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 什么是解释器，什么是 JIT 编译器？
* 解释器：当 Java 虚拟机启动时会根据预定义的规范**对字节码采用逐行解释的方式执行**，将每条字节码文件中的内容 “翻译” 为对应平台的本地机器指令执行。
* JIT（Just In Time Compiler）编译器：就是虚拟机将源代码直接编译成和本地机器平台相关的机器语言。
* **解释和执行都是将字节码指令翻译成为机器指令。解释：逐条翻译，直接就执行；编译：将所有的字节码指令翻译为机器指令后，会为机器指令做一个缓存，之后执行时读取这个缓存即可。**
* 解释：边翻译为本地机器指令边执行；编译：全部翻译为本地机器指令后再执行。
#### 为什么说 Java 是半编译半解释型语言？
* JDK1.0 时代，将 Java 语言定位为 “解释执行” 还是比较准确的。再后来，Java 也发展出可以直接生成本地代码的编译器。
* **JVM 在执行 Java 代码的时候，通常都会将解释执行与编译执行二者结合起来进行。**
* **执行引擎在解释字节码文件的时候，我们既可以使用解释器，也可以使用即时编译器，所以叫半解释半编译型语言。**
![图示](https://img-blog.csdnimg.cn/5c1902b821004d6a951bd43f1dc2b028.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 12.3 机器码、指令、汇编语言
#### 12.3.1 机器码
![图示](https://img-blog.csdnimg.cn/8726289eec5641b7b16cdeb6f9a758eb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
#### 12.3.2 指令和指令集
![图示](https://img-blog.csdnimg.cn/a3a0d2777b1d4ff08513b9298695b9e2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 12.3.3 汇编语言
![图示](https://img-blog.csdnimg.cn/0f55ef936c0441be824ee2e0a62652c4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 12.3.4 高级语言
![图示](https://img-blog.csdnimg.cn/3d63a647618145ed9bedebbf64f17a77.png)
#### 12.3.5 图示
![图示](https://img-blog.csdnimg.cn/f7f8572622c44b72b34163e370176214.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_17,color_FFFFFF,t_70,g_se,x_16)
#### 12.3.5 字节码
![图示](https://img-blog.csdnimg.cn/e2898b1fee3a4e77bebc2d5fb1e98157.png)
### 12.4 解释器
#### 解释器的工作任务
* 解释器真正意义上所承担的角色就是一个运行时 “翻译者”，**将字节码文件中的内容 “翻译” 为对应平台的本地机器指令执行**。
* 当一条字节码指令被解释执行完成后，接着再根据 PC 寄存器中记录的下一条需要被执行的字节码指令执行解释操作。
#### 解释器的分类
* 在 Java 的发展历史里，一共有两套解释执行器，即古老的**字节码解释器**、现在普遍使用的**模板解释器**。
* 字节码解释器在执行时通过**纯软件代码**模拟字节码的执行，效率非常低下。
* 模板解释器将**每一条字节码和一个模板函数相关联**，模板函数中直接产生这条字节码执行时的机器码，从而很大程度上提高了解释器的性能。
* 在 HotSpot VM 中，解释器主要由 Interpreter 模块和 Code 模块构成。
（1）Interpreter 模块：实现了解释器的核心功能。
（2）Code 模块：用于管理 HotSpot VM 在运行时生成的本地机器指令。
#### 现状
* 由于解释器在设计和实现上非常简单，因此除了 Java 语言之外，还有许多高级语言同样也是基于解释器执行的，比如 Python、Ruby 等。但是在今天，**基于解释器执行已经沦落为低效的代名词**，并且时常被一些 C/C++ 程序员所调侃。
* 为了解决这个问题，JVM 平台支持一种叫作即时编译的技术。即时编译的目的是避免函数被解释执行，而是**将整个函数体编译成为机器码，每次函数执行时，只执行编译后的机器码即可**，这种方式可以使执行效率大幅度提升。
### 12.5 JIT 编译器
* HotSpot VM 是目前市面上高性能虚拟机的代表作之一。它**采用解释器与即时编译器并存的架构**。在 Java 虚拟机运行时，解释器和即时编译器能够相互协作，各自取长补短，尽力去选择最合适的方式来权衡编译本地代码的时间和直接解释执行代码的时间。
* 在今天，Java 程序的运行性能早已脱胎换骨，已经达到了可以和 C/C++ 程序一较高下的地步。
#### 12.5.1 问题
* 有些开发人员可能会感觉诧异，**既然 HotSpot VM 中已经内置了 JIT 编译器了，那么为什么还需要再使用解释器来 “拖累” 程序的执行性能呢？** 比如 JRockit VM 内部就不包含解释器，字节码全部都依靠即时编译器编译后执行。
* 当程序启动后，解释器可以马上发挥作用，逐行解释执行字节码。即**解释器的响应速度快**。
* 编译器要想发挥作用，把代码编译成本地代码，需要一定的执行时间，但**编译为本地代码后，执行效率高**。
* 所以，尽管 JRockit VM 中程序的执行性能会非常高效，但程序在启动时不然需要花费更长的时间来进行编译。对于服务器端应用来说，启动时间并非是关注的重点，但对于那些看重启动时间的应用场景来说，或许就需要采用解释器与即时编译器并存的架构来换取一个平衡点。再此模式下，**当 Java 虚拟机启动时，解释器可以首先发挥作用，而不必等待即时编译器全部编译完成后再执行，这样可以省去许多不必要的编译时间。随着时间的推移，即时编译器发挥作用，把越来越多的字节码编译成本地代码，获得更高的执行效率。**
* 同时，解释执行在编译器进行激进优化不成立的时候，作为编译器的 “逃生门”。
#### 12.5.2 HotSpot JVM 的执行方式
* 当虚拟机启动的时候，**解释器可以首先发挥作用**，而不必等待即时编译器全部编译完成后再执行，这样可以**省去许多不必要的编译时间**。并且随着程序运行时间的推移，即时编译器发挥作用，根据热点探测功能，**将有价值的字节码编译为本地机器指令**，以获得更高的程序执行效率。
#### 12.5.3 概念解释
* Java 语言的 “编译期” 其实是一段 “不确定” 的操作过程，因为它可能是指一个**前端编译器**（其实叫 “编译器的前端” 更准确）把 .java 文件转变成 .class 字节码文件的过程，也可能是指虚拟机的**后端运行期编译器（JIT 编译器）** 把字节码转变成机器码的过程。
* 还可能是指使用**静态提前编译器**（AOT 编译器）把一个字节码文件在程序运行之前转换成 .so 机器码文件
![图示](https://img-blog.csdnimg.cn/96d93c91a21c4abcb00d6d4740f030af.png)
#### 热点代码及探测方式
* 是否需要启动 JIT 编译器将字节码直接编译为对应平台的本地机器指令，则**需要根据代码被调用执行的频率而定。执行频率比较高的代码，就称为 “热点代码”**。JIT 编译器在运行时会针对那些频繁被调用的 “热点代码” 做出**深度优化**，将其直接编译为对应平台的本地机器指令，以此提升 Java 程序的执行性能。
#### 热点代码及探测方式
* **一个被多次调用的方法，或者是一个方法体内部循环次数较多的循环体都可以被称之为热点代码**，因此都可以通过 JIT 编译器编译为本地机器指令。由于这种编译方式发生在方法的执行过程中，因此也被称之为**栈上替换（OSR）编译**。
* **一个方法究竟要被调用多少次，或者一个循环体究竟需要执行多少次循环才可以达到这个标准**？必然需要一个明确的阈值，JIT 编译器才会将这些热点代码编译为本地机器指令执行。这里主要依靠**热点探测功能。**
* **目前 HotSpot VM 所采用的热点探测方式是基于计数器的热点探测。**
* 采用基于计数器的热点探测，**HotSpot VM 将会为每一个方法都建立 2 个不同类型的计数器**，分别为方法调用计数器（Invocation Counter）和回边计数器（Back Edge Counter）
（1）**方法调用计数器用于统计方法的调用次数。**
（2）**回边计数器用于统计循环体执行的循环次数。**
#### 方法调用计数器
* 这个计数器用于统计方法被调用的次数，它的**默认阈值在 Client 模式下是 1500 次，在 Server 模式下是 10000 次。超过这个阈值，就会触发 JIT 编译。**
* 这个阈值可以通过虚拟机参数 ``-XX:CompileThreshold``来人为设定。
* 当一个方法被调用时，会先检查该方法是否存在被 JIT 编译过的版本，如果存在，则优先使用编译后的本地代码来执行。如果不存在已被编译过的版本，则将此方法的调用计数器值加 1，然后判断**方法调用计数器与回边计数器值之和是否超过方法调用计数器的阈值**。如果已超过阈值，那么将会向即时编译器提交一个该方法的代码编译请求。
* 示图：
![图示](https://img-blog.csdnimg.cn/79ad0234172d4d598f0bb34330127332.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
##### 热度衰减
![图示](https://img-blog.csdnimg.cn/0e4f5f24e9664c75a62676b21f1684be.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 回边计数器
* 它的作用是统计一个方法中**循环体代码执行的次数**，在字节码中遇到控制流向后跳转的指令称为 “回边”。显然，建立回边计数器统计的目的就是为了触发 OSR 编译。
* 示图：
![图示](https://img-blog.csdnimg.cn/e6356c9aa445447cbc78850dd13251d6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_13,color_FFFFFF,t_70,g_se,x_16)
#### HotSpot VM 可以设置程序执行方式
* 默认情况下 HotSpot VM 是采用解释器与即时编译器并存的架构，当然开发人员可以根据具体的应用场景，通过命令显式地为 Java 虚拟机指定在运行时到底是完全采用解释器执行，还是完全采用即时编译器执行：
（1）``-Xint``：**完全采用解释器模式执行程序**
（2）``-Xcomp``：**完全采用即时编译器模式执行程序。如果即时编译出现问题，解释器会介入执行**
（3）``-Xint``：**采用解释器和即时编译器的混合模式共同执行程序**
#### HotSpot VM 中 JIT 的分类
* **在 HotSpot VM 中内嵌有两个 JIT 编译器，分别为 Client Compiler（C1 编译器）和 Server Compiler（C2 编译器）**。开发人员可以通过如下命令显示指定 Java 虚拟机在运行时到底使用哪一种即时编译器。
（1）``-client``：指定 Java 虚拟机运行在 Client 模式下，并使用 C1 编译器；C1 编译器会对字节码进行**简单和可靠的优化，耗时短**，以达到更快的编译速度。
（2）``-server``：指定 Java 虚拟机运行在 Server 模式下，并使用 C2 编译器；C2 编译器会进行**耗时较长的优化，以及激进优化**，但优化的代码执行效率更高。
* **分层编译策略**：程序解释执行（**不开启性能监控**）可以**触发 C1 编译**，将字节码编译成机器码，可以进行简单优化。也可以**加上性能监控，C2 编译会根据性能监控信息进行激进优化。**
* 不过在 jdk7 版本之后，一旦开发人员在程序中显式指定命令 “-server” 时，默认将会开启分层编译策略，由 C1 编译器和 C2 编译器相互协作共同来执行编译任务。
#### C1 和 C2 编译器不同的优化策略
![图示](https://img-blog.csdnimg.cn/5fa11f0356564cf98e664a9f9597633b.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 总结
* 一般来说，JIT 编译出来的机器码性能比解释器高。
* C2 编译器启动时长比 C1 编译器慢，系统稳定执行之后，C2 编译器执行速度远远快于 C1 编译器。
### 12.6 Graal 编译器和 AOT 编译器
#### 12.6.1 Graal 编译器
* 自 JDK10 起，HotSpot VM 又加入一个全新的**即时编译器：Graal 编译器。**
* 编译效果短短几年时间就追平了 C2 编译器。
* 目前还带着 “实验状态” 标签，需要使用开关参数：``-XX:+UnlockExperimentalVMOptions -XX:+UseJVMCICompiler``去激活，才可以使用。
#### 12.6.2 AOT 编译器
* JDK9 引入了 AOT 编译器（静态提前编译器）
* Java9 引入了实验性 AOT 编译工具 jaotc。它借助了 Graal 编译器，将所输入的 Java 字节码文件转换为机器码，并存放至生成的动态共享库之中。
* 所谓的 AOT 编译，是与即时编译相对立的一个概念。我们知道，即时编译指的是在**程序的运行过程中**，将字节码转换为可在硬件上直接运行的机器码，并部署至托管环境中的过程。而 AOT 编译指的则是，在**程序运行之前**，便将字节码转换为机器码的过程。
* 好处：Java 虚拟机加载已经预编译成二进制库，可以直接执行。不必等待即时编译器的预热，减少 Java 应用给人带来 “第一次运行慢” 的不良体验。
* 缺点：
（1）破坏了 Java “一次编译，到处运行”，必须为每个不同硬件、OS 编译对应的发行包。因为是 ``.java -> .class -> .so``，字节码文件可以跨平台，而 .so 文件已经是机器码了，与具体硬件相关。
（2）**降低了 Java 链接过程的动态性**，加载的代码在编译期就必须全部已知。
（3）还需要继续优化，最初只支持 Linux x64 java base。

## 十三、String Table
### 13.1 String 的基本特性 
* String 声明为 final，表示不可被继承。
* String 实现了 Serializable 接口：表示字符串是支持序列化的；实现了Comparable 接口：表示 String 可以比较大小。
* **String 在 jdk8 及以前内部定义了 final char[] value 用于存储字符串数据。jdk9 时改为了 byte[] 加上编码标记，节约了一些空间。其它与字符串相关的类比如 StringBuilder 等也同样做了修改**
* String 具有不可变性。
* **字符串常量池中是不会存储相同内容的字符串的**（如何实现的：String Poll 的底层是一个 Map）。
* **String 的 String Poll 是一个固定大小的 Hashtable**。如果放进 String Poll 的 String 很多，就会造成 Hash 冲突严重，从而导致链表会很长，而链表长了之后直接会造成的影响就是当调用 String.intern 时性能会大幅下降。
* 使用 ``-XX:StringTableSize``可设置 StringTable 的长度。
* 在 jdk6 中 StringTable 的默认长度是 1009；在 jdk7 中，StringTable 的长度默认值是 60013；在 jdk8 中，StringTable 的长度默认值仍然是 60013，但是设置 StringTable 的长度的话，1009 是可设置的最小值。

#### 为什么 jdk9 及之后修改为 byte 数组？
![图示](https://img-blog.csdnimg.cn/d9c3aa735243491e8be192230880ffe8.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 动机
String 类的当前实现将字符存储在 char 数组中，每个字符使用两个字节（十六位）。从许多不同应用程序收集的数据表明，字符串是堆使用的主要组成部分，而且，大多数 String 对象仅包含 Latin-1 字符。此类字符仅需要一个字节的存储空间，因此此类 String 对象的内部字符数组中的一半空间将被闲置。
* 描述
我们**建议将 String 类的内部表示从 UTF-16 字符数组更改为字节数组加上编码标志字段**。新的 String 类将根据字符串的内容存储编码为 ISO-8859-1/Latin-1（每个字符一个字节）或 UTF-16（每个字符两个字节）的字符。**编码标志将指示使用哪种编码**。
### 13.2 String 的内存分配
* 在 Java 语言中有 8 种基本数据类型和一种比较特殊的类型 String，这些类型为了使它们在运行过程中速度更快、更节省内存，都提供了一种常量池的概念。
* 常量池就类似一个 Java 系统级别提供的缓存。8 种基本数据类型的常量池都是系统协调的，String 类型的常量池比较特殊。它的主要使用方法有两种：
![图示](https://img-blog.csdnimg.cn/2aac5de336d548b8b068513710c6985b.png)
#### 字符串常量池在内存中位置的变化
![图示](https://img-blog.csdnimg.cn/1ac95db2853e414ca4605d9368d8f88f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 为什么要调整 StringTable 的位置从永久代到堆
1. permSize 默认比较小，放大量的字符串可能导致永久代报 OOM
2. 永久代的垃圾回收频率很低
### 13.4 字符串拼接操作
#### 结论
1. **常量与常量的拼接结果在常量池**，原理是**编译期优化**
2. 常量池中不会存在相同内容的常量。
3. **只要其中有一个是变量，结果就在堆中（不是常量池的区域）。变量拼接的原理是 StringBuilder**
4. 如果拼接的结果调用 intern() 方法，则主动将常量池中还没有的字符串对象放入池中，并返回此对象地址。**当通过语句 str.intern() 调用 intern() 方法后，JVM 就会在常量池中查找是否存在与 str 等值的 String，若存在则直接返回常量池中相应 String 的地址；若不存在，则会在常量池中创建一个等值的String，然后返回这个 String 在常量池中的地址。**
#### 经典面试题
```java
public class StringTest {
    // 常量与常量的拼接
    public void test1(){
        // 在生成字节码文件的时候，就直接将 "a" + "b" + "c" 等同于 "abc"（可查看字节码文件，或者查看反编译的结果）
        String s1 = "a" + "b" + "c"; // 等同于 "abc"
        String s2 = "abc"; // "abc" 一定是放在字符串常量池中的，然后将其地址赋给 s2

        System.out.println(s1 == s2); // true
        System.out.println(s1.equals(s2)); // true
    }
    // "+" 两边至少有一个是变量
    public void test2(){
        String s1 = "a";
        String s2 = "b";
        String s3 = "ab";
        /*
         如下的 s1 + s2 细节：(变量 s 是我临时定义的，底层没说变量叫 s)
             (1) StringBuilder s = new StringBuilder()
             (2) s.append("a")
             (3) s.append("b")
             (4) s.toString()  --> 约等于 new String("ab")，但是字符串常量池中并不会存在 "ab"（通过 StringBuilder 中的 toString() 的字节码可知）
         补充：jdk5 及之后使用的是 StringBuilder，jdk5 之前使用的是 StringBuffer
        */
        String s4 = s1 + s2; // s4 变量记录的地址为：new String("ab")，但是字符串常量池中并不会存在 "ab"
        System.out.println(s3 == s4); // false
    }
    @Test
    public void test3(){
        String s1 = "javaEE";
        String s2 = "hadoop";

        String s3 = "javaEEhadoop";
        String s4 = "javaEE" + "hadoop";
        // 如果拼接符号的前后出现了变量，则相当于在堆空间中 new String()，具体的内容为拼接后的结果：javaEEhadoop
        String s5 = s1 + "hadoop";
        String s6 = "javaEE" + s2;
        String s7 = s1 + s2;
        System.out.println(s3 == s4); // true
        System.out.println(s3 == s5); // false
        System.out.println(s3 == s6); // false
        System.out.println(s3 == s7); // false
        System.out.println(s5 == s6); // false
        System.out.println(s5 == s7); // false
        System.out.println(s6 == s7); // false
        /* intern()：判断字符串常量池中是否存在 javaEEhadoop 值，如果存在，则返回常量池中 javaEEhadoop 的地址；
                    如果不存在，则在常量池中加载一份 javaEEhadoop，并返回加载的 javaEEhadoop 在常量池中的地址 */
        String s8 = s6.intern();
        System.out.println(s3 == s8); // true
    }
    // 常量与常量的拼接
    public void test4(){
        final String s1 = "a";
        final String s2 = "b";
        String s3 = "ab";
        // 下面的字符串拼接操作仍然是编译期优化，可以看成是两个常量相加（final 修饰），而不是使用 StringBuilder 的方式
        String s4 = s1 + s2; // 因为加了 final 修饰，所以 s1 和 s2 就是常量
        System.out.println(s3 == s4); // true
    }
}
```
* 常量与常量拼接（test1）
![图示](https://img-blog.csdnimg.cn/3bac3aaffb8f4f23a8484538019ebdaa.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_19,color_FFFFFF,t_70,g_se,x_16)
* "+" 两边至少有一个是变量（test2）
![图示](https://img-blog.csdnimg.cn/8ea3b87247a040b0ac4557fbb4611d51.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### append() 方式和 "+" 方式的效率对比
```java
public class StringTest {
    /**
     * 执行效率：使用 StringBuilder 的 append() 方式添加字符串的效率要远高于使用 String 的字符串拼接方式
     * 为什么：
     *     StringBuilder 的 append() 方式，自始至终只创建过一个 StringBuilder 对象
     *     使用 String 的字符串拼接方式：创建了许多 StringBuilder 对象和 String 对象，占用内存过大；如果进行 GC，还要花费额外的时间
     *
     * StringBuilder 的改进：
     *     StringBuilder s = new StringBuilder() 的方式，value 数组的默认大小是 16。如果字符串长度过大则需要对数组进行扩容。
     *     因此如果可以确定前前后后添加的字符串长度不高于某个最大值 highLevel，则可以使用 StringBuilder s = new StringBuilder(highLevel);
     */
    @Test
    public void test5(){
        long start = System.currentTimeMillis();
        // method1(100000); // 6092 ms
        method2(100000); // 9 ms
        long end = System.currentTimeMillis();
        System.out.println(end - start);
    }
    public void method1(int highLevel){
        String s = "";
        for(int i = 0; i < highLevel; i++){
            s = s + "a"; // 每次循环都会创建一个 StringBuilder 和 String (调用 StringBuilder 的 toString() 方法时会 new String)
        }
    }
    public void method2(int highLevel){
        StringBuilder s = new StringBuilder();
        for(int i = 0; i < highLevel; i++){
            s.append("a");
        }
    }
}
```
### 13.5 intern() 的使用
* 当通过语句 str.intern() 调用 intern() 方法后，JVM 就会在常量池中查找是否存在与 str 等值的 String，若存在则直接返回常量池中相应 String 的地址；若不存在，则会在常量池中创建一个等值的String，然后返回这个 String 在常量池中的地址。
* 比如："abc".intern()，如果常量池中已经存在 "abc"，则直接返回常量池中 "abc" 的地址；如果常量池中不存在 "abc"，那么就先在常量池中创建一个 "abc"，然后再返回常量池中 "abc" 的地址。
* s.intern() == t.intern() 为 true 等价于 s.equals(t) 为 true
#### 经典面试题
##### （1）new String("ab") 会创建多少个对象
```java
public class StringTest {
    /**
     *  new String("ab") 会创建多少个对象
     *    看字节码文件，就知道是两个：
     *      （1）一个对象是：new 关键字在堆空间创建的
     *      （2）另一个对象是：字符串常量池中的对象 "ab"。字节码指令：ldc
     *  其实严谨点也可能是一个，因为有可能在 new String("ab") 之前字符串常量池中已经有 "ab"，此时就直接用已有的 "ab"
     */
    public void test6(){
        String s = new String("ab");
    }
}
```
字节码文件：
![图示](https://img-blog.csdnimg.cn/b3950a6e4a824cdc9c14dfc9fd12cf01.png)
##### （2）new String("a") + new String("b") 会创建多少个对象
```java
public class StringTest {
    /**
     *  new String("a") + new String("b") 会创建多少个对象：
     *      （1）new StringBuilder()
     *      （2）new String("a")
     *      （3）常量池中的 "a"
     *      （4）new String("b")
     *      （5）常量池中的 "b"
     *  深入剖析：StringBuilder 的 toString()：
     *      （6）new String("ab")
     *      注意：toString() 的调用，在字符串常量池中，并没有生成 "ab"
     */
    public void test6(){
        String s = new String("a") + new String("b");
    }
}
```
字节码文件：
![图示](https://img-blog.csdnimg.cn/5caa966009eb430687e7b727c013abbe.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 由以上分析可知，**new String("a") + new String("b") 这个操作并不会使字符串常量池中存在 "ab"**
#### （3）intern()
```java
public class StringTest1 {
    public static void main(String[] args) {
        String s1 = new String("1");
        s1.intern(); // 调用此方法之前，字符串常量池中已经存在了 "1"
        String s2 = "1";
        System.out.println(s1 == s2); // jdk6：false  jdk7/8：false

        String s3 = new String("1") + new String("1"); // s3 变量记录的地址为：new String("11")
        // 执行完上一行代码以后，字符串常量池中不存在 "11"
        s3.intern(); // 在字符串常量池中生成 "11"。如何理解：jdk6：在常量池中创建了一个新的对象 "11"；
                                                      // jdk7/8：常量池中并没有创建 "11"，而是创建了一个指向堆空间中 new String("11") 的地址
        String s4 = "11";
        System.out.println(s3 == s4); // jdk6：false  jdk7/8：true
    }
}
```
```java
public class StringTest2 {
    public static void main(String[] args) {
        String s = new String("a") + new String("b"); // s 变量记录的地址为：new String("ab")，但是字符串常量池中并不存在 "ab"
        String s2 = s.intern(); // jdk6中：在字符串常量池中创建一份 "ab"
                                // jdk7/8 中：字符串常量池中并没有创建 "ab"，而是创建了一个引用，指向 new String("ab")，并返回该引用地址
        System.out.println(s2 == "ab"); // jdk6：true  jdk7/8：true
        System.out.println(s == "ab"); // jdk6：false  jdk7/8：true
    }
}
```
#### intern() 的空间效率
* 如果内存中需要存储大量的字符串，尤其存在许多重复的字符串时，**使用 intern() 方法可以显著节省内存空间。**
### 13.6 G1 中的 String 去重操作
## 十四、垃圾回收概述
### 14.1 什么是垃圾
* 垃圾是指**在运行程序中没有任何指针指向的对象**，这个对象就是需要被回收的垃圾。
* 如果不及时对内存中的垃圾进行清理，那么这些垃圾对象所占的内存空间会一直保留到应用程序结束，被保留的空间无法被其他对象使用。甚至**可能导致内存溢出**。
### 14.2 为什么需要 GC
* 对于高级语言来说，一个基本的认知是**如果不进行垃圾回收，内存迟早都会被消耗光**。因为不断地分配内存而不进行回收，就好像不停的生产生活垃圾而从来不打扫一样。
* 除了释放没用的对象，垃圾回收也可以**清理内存中的记录碎片**。碎片整理将所占用的堆内存移到堆的一端，以便 **JVM 将整理出的内存分配给新的对象。**
* 随着应用程序所应付的业务越来越庞大、复杂，**没有 GC 就不能保证应用程序的正常进行**。而经常造成 STW 的 GC 又跟不上实际的需求，所以才会不断地尝试对 GC 进行优化。
### 14.3 早期垃圾回收
![图示](https://img-blog.csdnimg.cn/63dd4d0bb4b64050b4aebf6d0660b0e2.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 有了垃圾回收机制后，上述代码块极有可能变成这样：
![图示](https://img-blog.csdnimg.cn/4ed35072ce58477faa0ec2c318f75f6a.png)
* 现在，除了 Java 之外，C#、Python、Ruby 等语言也都使用了自动垃圾回收的思想，也是未来发展的趋势。可以说，这种**自动化的内存分配和垃圾回收的方式**已经成为现代开发语言必备的标准。
### 14.4 Java 垃圾回收机制
* 自动内存管理：**无需开发人员手动参与内存的分配与回收，这样降低内存泄漏和内存溢出的风险。**
* 如果没有垃圾回收器，Java 也会像 cpp 一样，各种野指针、泄露问题让你头疼痛不已。
* 自动内存管理机制，将程序员从繁重的内存管理中释放出来，**可以更专心地专注于业务开发。**
* 注：垃圾回收器可以对年轻代回收，也可以对老年代回收，甚至是全堆和方法区的回收。其中，**Java 堆是垃圾收集器的工作重点。**
* 从次数上讲：
**（1）频繁收集年轻代
（2）较少收集老年代
（3）基本不动元空间**
#### 担忧
![图示](https://img-blog.csdnimg.cn/1611106e503649f983bb34f4371307d4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
## 十五、垃圾回收的相关算法
### 垃圾标记阶段：对象存活判断
* 在堆中存放着几乎所有的 Java 对象实例，在 GC 执行垃圾回收之前，首先**需要区分出内存中哪些是存活对象，哪些是已经死亡的对象**。只有被标记为已经死亡的对象，GC 才会在执行垃圾回收时，释放掉其所占用的内存空间，因此这个过程我们可以称为**垃圾标记阶段**。
* 那么在 JVM 中是如何标记一个死亡对象的呢？简单来说，**当一个对象已经不再被任何存活的对象继续引用时，就可以宣判为已经死亡。**
* 判断对象是否存活一般有两种方式：**引用计数算法和可达性分析算法。**
### 垃圾清除阶段
* 当成功区分出内存中的存活对象和死亡对象后，GC 接下来的任务就是执行垃圾回收，释放掉无用对象所占用的内存空间，以便有足够的可用内存空间为新对象分配内存。
* 目前在 JVM 中比较常见的三种垃圾收集算法是：标记 - 清除算法（Mark - Sweep）、复制算法（Copying）、标记 - 压缩算法（Marked - Compact）
### 15.1 标记阶段：引用计数算法
* 引用计数算法（Reference Counting）比较简单，对每个对象保存一个整型的**引用计数器属性，用于记录对象被引用的情况。**
* 对于一个对象 A，只要有任何一个对象引用了 A，则 A 的引用计数器就加 1；当引用失效时，引用计数器就减 1，只要对象 A 的引用计数器的值为 0，即表示对象 A 不可能再被使用，可进行回收。
* 优点：**实现简单，垃圾对象便于辨识；判定效率高，回收没有延迟性。**
* 缺点：
（1）它需要单独的字段存储计数器，这样的做法**增加了存储空间的开销。**
（2）每次赋值都需要更新计数器，伴随着加法和减法操作，这**增加了时间开销。**
（3）引用计数器有一个严重的问题，即**无法处理循环引用**的问题。这是一个致命缺陷，导致在 Java 的垃圾回收器中并没有使用这类算法。
#### 循环引用
* 循环引用，也就是**互相引用，导致计数器永远无法变为 0**
* A 引用了 B，B 引用了 A，但 A 和 B 都不被其他所引用，也就是说，AB 你已经无法从外部再访问到它们了，是应该被回收的，但它们的引用计数却不是0。
![图示](https://img-blog.csdnimg.cn/c713951c0a9849b49640ede1cb1fd1b9.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* Python 如何解决循环引用：
（1）手动解除：很好理解，就是在合适的时机，解除引用关系。比如 A 引用 B，B 引用 A，那么我显式地将这两个引用干掉，将引用计数变为 0，就可以进行垃圾回收了。
（2）使用弱引用 weakref。weakref 是 Python 提供的标准库，旨在解决循环引用。发生 gc 时，只要发现是弱引用，不管内存空间够还是不够，都回收掉。
### 15.2 标记阶段：可达性分析算法
* 可达性分析算法，又叫根搜索算法、追踪性垃圾收集算法
* 相对于引用计数算法而言，可达性分析算法不仅同样具备实现简单和执行高效等特点，更重要的是该算法可以有效地**解决在引用计数算法中循环引用的问题，防止内存泄漏的发生。**
* 相较于引用计数算法，这里的**可达性分析就是 Java、C# 所选择的**。
#### 具体思路
* 所谓的 “GC Roots” 根集合就是一组必须活跃的引用。
* 基本思路：
![图示](https://img-blog.csdnimg.cn/5f144b5b22364fa8af7c8a438d749b44.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 图示：
![图示](https://img-blog.csdnimg.cn/cfe1706ae49f4399894042d285ff7e4e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
#### GC Roots
* 在 Java 语言中，GC Roots 包含以下几类元素：
![图示](https://img-blog.csdnimg.cn/ca375e1439404d2ab46ec9aa6cf3a8c1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 为什么会有**临时的 GC Roots**：目前的垃圾回收大部分都是**分代收集和局部回收**，如果只针对某一部分区域进行局部回收，那么就必须考虑到**当前区域的对象有可能正被其他区域的对象所引用**，这时候就要将这部分关联的对象也添加到 GC Roots 中去确保根可达算法的准确性。
* 小技巧：由于 Root 采用栈方式存放变量和指针，所以如果一个指针，它保存了堆内存里面的对象，但是自己又不存放在堆内存里面，那它就是一个 Root。
#### 注意事项
* 如果要使用可达性分析算法来判断内存是否可以回收，那么分析工作必须在一个能保障**一致性**的快照中进行。如果这点不能满足的话，那么分析结果的准确性就无法保证。
* 一致性：在分析期间，整个系统就像是被冻结在某个时间点上。不可以在分析的过程中，对象的引用关系还一直在变化。
* 这点也是导致 GC 进行时必须 “Stop  The World” 的一个重要原因。即使是号称（几乎）不会发生停顿的 CMS 收集器中，枚举根结点时也是必须要停顿的。
![图示](https://img-blog.csdnimg.cn/cc42c75ace7446309c4119ab8edf85ca.png)
### 15.3 对象的 finalization 机制
* Java 语言提供了对象终止（finalization）机制来允许开发人员提供**对象被销毁之前的自定义处理逻辑。**
* 当垃圾回收器发现没有引用指向一个对象，即**垃圾回收此对象之前，总会先调用这个对象的 finalize() 方法。**
* finalize() 方法允许在子类中被重写，**通常是用于在对象被回收前进行资源释放**。通过在这个方法中进行一些资源释放和清理的工作，比如关闭文件、套接字和数据库连接等。
* **永远不要主动调用某个对象的 finalize() 方法，应该交给垃圾回收机制调用**。理由包括下面三点：
（1）在 finalize() 方法中可能会导致对象复活。
（2）即使主动调用 finalize() 方法，它也不一定会马上执行，因为它是由一个优先级比较低的 Finalizer 线程来执行的。在极端情况下，如果不发生 GC，则 finalize() 方法将没有执行机会。
（3）一个糟糕的 finalize() 会严重影响 GC 的性能。
* 由于 finalize() 方法的存在，**虚拟机中的对象一般处于三种可能的状态。**
![图示](https://img-blog.csdnimg.cn/374b74a6c69e4c27bc924b302009a399.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 对象的所有引用都被释放，也就是该对象是不可达的。
#### 判断一个对象是否可回收的具体过程
* 判定一个对象 objA 是否可回收，**至少要经历两次标记过程：**
![图示](https://img-blog.csdnimg.cn/946a130f588043448ca33666214cc9c1.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 注：**一个对象的 finalize() 方法只会被调用一次。**
### 15.4 清除阶段：标记-清除算法
#### 15.4.1 背景
* 标记 - 清除算法（Marked - Sweep）是一种非常基础和常见的垃圾收集算法，该算法被 J.McCarthy 等人在 1960 年提出并应用于 Lisp 语言。
#### 15.4.2 执行过程
* 当堆中的有效内存空间被耗尽的时候，就会**停止整个程序（也被称为 stop the world）**，然后进行两项工作，第一项是标记，第二项是清除。
* **标记**：垃圾收集器从引用根节点开始遍历，标记所有被引用的对象（即**标记所有 GC Roots 可达的对象**）。一般是在对象的 Header 中记录为可达对象。
* **清除**：垃圾收集器对堆内存从头到尾进行线性遍历，如果发现某个对象在其 Header 中没有被标记为可达对象，则将其回收。
* 图示：
![图示](https://img-blog.csdnimg.cn/ac2f6d4bdb064d64a34ecb20ced8bcd3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_17,color_FFFFFF,t_70,g_se,x_16)
#### 15.4.3 缺点
* **效率不算高**。因为标记时需要遍历所有可达对象，清除时需要把堆内存中的对象全遍历一遍，也就是一共需要遍历两次，效率不高。
* 在进行 GC 的时候，需要停止整个应用程序，导致用户体验差。
* 这种方式清理出来的空闲内存是不连续的，**会产生内存碎片**。需要维护一个空闲列表。如果加载进来一个比较大的对象，可能会无法找到足够的连续内存空间来存放，从而提前触发另一次垃圾收集动作。
#### 15.4.4 何为清除
* 这里所谓的**清除并不是真的置空，而是把需要清除的对象地址保存在空闲的地址列表里**。下次再有新对象需要加载时，判断垃圾的位置空间是否够，如果够，就直接覆盖掉原有的数据。

### 15.5 清除阶段：复制算法
#### 15.5.1 背景
![图示](https://img-blog.csdnimg.cn/88150eccabd047f3bb9ed44e7bbecf81.png)
#### 15.5.2 核心思想
* 将可用内存按容量划分为大小相等的两块，每次只使用其中一块。当垃圾回收时，将正在使用的内存中的存活对象**复制**到未被使用的内存块中，之后清除正在使用的内存块中的所有对象。然后交换两个内存的角色。最终完成垃圾回收。
* 图示：
![图示](https://img-blog.csdnimg.cn/2d6009eb61244171a978cc587a517f5e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
* 先将 A 中的存活对象复制到 B 中，然后清除 A 中的所有对象。之后在需要放数据时放到 B 中。如果 B 需要垃圾回收的时候，则将 B 中的存活对象复制到 A 中，然后清除 B 中的所有对象。
#### 15.5.3 优缺点
* **优点：**
（1）没有标记和清除的过程，实现简单，运行高效。
（2）复制过去以后保证了空间的连续性，不会出现碎片问题。
* **缺点：** 
（1）**浪费过多的内存，使现有的可用内存空间变为原来的一半。**
（2）**如果系统中的存活对象很多，复制算法就不太理想**，因为需要复制的存活对象数量会很多。
#### 15.5.4 应用场景
* 在新生代，对常规应用的垃圾回收，一次通常可以回收 70% ~ 99% 的内存空间，也就是说存活对象很少。因此 Survivor 区使用复制算法的性价比是很高的。
### 15.6 清除阶段：标记-压缩算法
#### 15.6.1 背景
* 复制算法的高效性是建立在存活对象少、垃圾对象多的前提下的。这种情况在新生代经常发生，但是在老年代，更常见的情况是大部分对象都是存活对象。如果依然使用复制算法，由于存活对象较多，复制的成本也将很高。因此，**基于老年代垃圾回收的特性，需要使用其它的算法。**
* 标记 - 清除算法的确可以应用在老年代中，但是该算法不仅执行效率低下，而且在执行完内存后还会产生内存碎片，所以 JVM 的设计者需要在此基础之上进行改进。标记 - 压缩算法由此诞生。
* 1970 年前后，G.L.Steele、C.J.Chene 和 D.S.Wise 等研究者发布标记 - 压缩算法。在许多现代的垃圾收集器中，人们都使用了标记 - 压缩算法或其改进版本。
#### 15.6.2 执行过程
* 其标记过程和标记 - 清除算法一样，然后让所有的存活对象都向内存空间的一端移动，之后直接清理掉边界以外的内存。
![图示](https://img-blog.csdnimg.cn/a2f58d2b89c84e0f82ea5d6151470d44.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* **标记 - 清除算法和标记 - 压缩算法的本质差异在于前者是一种非移动式的回收算法，后者是移动式的**。是否移动回收后的存活对象是一项优缺点并存的风险决策。
（1）如果移动存活对象，则该存活对象的地址需要改变，而且引用该存活对象的地方也需要更改相应的地址。
（2）如果不移动存活对象，则会导致空间碎片化问题，需要维护一个空闲列表。
#### 15.6.3 优缺点
* **优点：**
（1）消除了标记 - 清除算法当中，内存区域分散的缺点，我们需要给新对象分配内存时，JVM 只需要持有一个内存的起始地址即可。
（2）消除了复制算法当中，内存减半的高额代价。
* **缺点：**
（1）从效率上来说，标记-整理算法要低于复制算法。
（2）移动对象的同时，如果对象被其他对象引用，则还需要调整引用的地址。
（3）移动存活对象到内存一端的过程中，需要全程暂停用户应用程序，即：STW。
### 15.7 小结
![图示](https://img-blog.csdnimg.cn/3a51659547bd4c72b3b7e57cafd24b9b.png)
* 从效率上来说，复制算法是当之无愧的老大，但是却浪费了太多的内存。
* 而为了尽量兼顾上面提到的三个指标，标记 - 整理算法相对来说更平滑一些，但是效率上不尽如人意，它比复制算法多了一个标记的阶段，比标记 - 清除多了一个内存整理的阶段。
### 15.8 分代收集算法
* 前面的三种算法中，并没有一种算法可以完全替代其他算法，它们都具有自己独特的优势和特点。分代收集算法应运而生。
* 分代收集算法是基于这样一个事实：不同对象的生命周期是不一样的。因此，**不同生命周期的对象可以采取不同的垃圾收集方式，以便提高回收效率**。一般是把 Java 堆分为新生代和老年代，这样就可以根据各个代的特点使用不同的回收算法，以提高垃圾回收的效率。
* 在 Java 程序的运行过程中，会产生大量的对象，其中有些对象是与业务信息相关。比如 Http 请求中的 Session 对象、线程、Socket 链接，这类对象跟业务直接挂钩，因此生命周期比较长。但是还有一些对象，主要是程序运行过程中生成的临时变量，这些对象的生命周期会比较短。
* **目前几乎所有的 GC 都是采用分代收集算法执行垃圾回收的。**
![图示](https://img-blog.csdnimg.cn/2bd672e429bd4952ba9622ba780b1557.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 以 HotSpot 中的 CMS 回收器为例，CMS 是基于 Mark-Sweep 实现的，对于对象的回收效率很高，而面对碎片问题，CMS 采用基于 Mark-Compact 算法的 Serial Old 回收器作为补偿措施：当内存回收不佳（碎片导致的 Concurrent Mode Failure 时），将采用 Serial Old 执行 Full GC 以达到对老年代的内存的整理。
* 分代的思想被现有的虚拟机广泛使用，几乎所有的垃圾回收器都区分新生代和老年代。

### 15.9 增量收集算法、分区算法
#### 15.9.1 增量收集算法
* 上述现有的算法，在垃圾回收过程中，应用软件将处于一种 Stop the World 的状态。在 **Stop the World** 状态下，应用程序所有的线程都会挂起，暂停一切正常的工作，等待垃圾回收的完成。**如果垃圾回收时间过长，应用程序会被挂起很久，将严重影响用户体验或者系统的稳定性**。为了解决这个问题，即对实时垃圾收集算法的研究导致了增量收集算法的诞生。
* 基本思想：
![图示](https://img-blog.csdnimg.cn/d44af198da814d29a964151ac2b9da14.png)
* 缺点：使用这种方式，由于在垃圾回收过程中，间断性地还执行了应用程序代码，所以能减少系统的停顿时间。但是，因为线程切换和上下文转换的消耗，会使得垃圾回收的总体成本上升，**造成系统吞吐量的下降。**
#### 15.9.2 分区算法
* 一般来说，在相同条件下，堆空间越大，一次 GC 时所需要的时间就越长，有关 GC 产生的停顿也越长。为了更好地控制 GC 产生的停顿时间，将一块大的内存区域分割成多个小块，根据目标停顿时间，每次合理地回收若干个小区间，而不是整个堆空间，从而减少一次 GC 所产生的停顿。
* 分代算法将堆空间按照对象的生命周期长短划分成两个部分：新生代和老年代，分区算法将整个堆空间划分成连续的不同小区间 region。
* 每一个小区间都独立使用，独立回收。这种算法的好处是可以控制一次回收多少个小区间。
* 图示：
![图示](https://img-blog.csdnimg.cn/63690beb417d4bf7868ae1cb1b116dcb.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_15,color_FFFFFF,t_70,g_se,x_16)
## 十六、垃圾回收相关概念
### 16.1 System.gc() 的理解
* 在默认情况下，通过 System.gc() 或者 Runtime.getRuntime().gc() 的调用，**会显式触发 Full GC**，同时对堆空间和方法区进行回收，尝试释放被丢弃对象占用的内存。
* System.gc() 方法的作用只是提醒虚拟机，**希望进行一次垃圾回收。至于什么时候进行回收还是取决于虚拟机，而且也不能保证一定进行回收**（如果 ``-XX:+DisableExplicitGC`` 设置成true，则不会进行回收）。
* JVM 实现者可以通过 System.gc() 调用来决定 JVM 的 GC 行为。而一般情况下，**垃圾回收应该是自动进行的，无需手动触发，否则就太过于麻烦了**。在一些特殊情况下，如我们正在编写一个性能基准，我们可以在运行之前调用 System.gc()
```java
public class SystemGCTest {
    public static void main(String[] args) {
        new SystemGCTest();
        System.gc(); // 提醒 JVM 的垃圾回收器执行 GC，但是不保证会马上执行
        System.runFinalization(); // 强制调用失去引用的对象的 finalize() 方法
    }

    @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("重写了 finalize() 方法");
    }
}
```
* 示例：
```java
public class LocalVarGC {
    public void localvarGC1(){
        byte[] buffer = new byte[10 * 1024 * 1024]; // 10MB
        System.gc(); // buffer 不会被回收掉空间
    }
    public void localvarGC2(){
        byte[] buffer = new byte[10 * 1024 * 1024];
        buffer = null;
        System.gc(); // buffer 会被回收掉空间
    }
    public void localvarGC3(){
        {
            byte[] buffer = new byte[10 * 1024 * 1024];
        }
        System.gc(); // buffer 不会被回收掉空间。在 gc 时，buffer 还占用着局部变量表中下标为 1 的位置，因此回收不了
    }
    public void localvarGC4(){
        {
            byte[] buffer = new byte[10 * 1024 * 1024];
        }
        int value = 1;
        System.gc(); // buffer 会被回收掉空间。由于 buffer 过了作用域，导致 value 会复用局部变量表中索引为 1 的位置（slot 重用），导致 buffer 这个引用就不存在了
    }
    public void localvarGC5(){
        localvarGC1();
        System.gc(); // 会回收掉 localvarGC1() 中的 buffer 的空间
    }

    public static void main(String[] args) {
        LocalVarGC local = new LocalVarGC();
        local.localvarGC5();
    }

}
```
### 16.2 内存溢出与内存泄漏
#### 16.2.1 内存溢出（OOM）
* 由于 GC 一直在发展，所以一般情况下，除非应用程序占用的内存增长速度非常快，造成垃圾回收已经跟不上内存消耗的速度，否则不太容易出现 OOM 的情况。
* 在大多数情况下，GC 会进行各个年龄段的垃圾回收，实在不行了就来一次独占式的 Full GC 操作，这时候会回收大量的内存，供应用程序继续使用。
* javadoc 中对 **OutOfMemoryError 的解释是：没有空闲空间，并且垃圾收集器也无法提供更多的内存。**
* 这里面隐含着一层意思是，在抛出 OutOfMemoryError 之前，通常垃圾收集器会被触发，尽其所能去清理出空间。
（1）例如：在引用机制分析中，涉及到 JVM 会去尝试回收软引用指向的对象等。
（2）在 java.nio.BIts.reserveMemory() 方法中，我们能清楚地看到，System.gc() 会被调用，以清理空间。
* 当然，也不是在任何情况下垃圾收集器都会被触发的。比如，我们去分配一个超大的对象，以至于超过了堆的最大值，JVM 可以判断出垃圾收集并不能解决这个问题，所以直接抛出 OutOfMemoryError。
* 没有空闲内存：说明 Java 虚拟机的堆内存不够。原因如下：
![图示](https://img-blog.csdnimg.cn/d4f48896199c42e28107be3b4d2ef2c7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 16.2.2 内存泄漏
* **严格来说，只有对象不会再被程序用到了，但是 GC 又不能回收它们的情况，才叫内存泄露。**
* 但在实际情况中，一些不太好的实践（或疏忽）会导致对象的生命周期变得很长甚至导致 OOM，也可以叫做**宽泛意义上的内存泄漏**。比如：一个变量本来可以定义在方法内，作为一个局部变量出现，这样的话该方法结束时这个变量就会被回收了。但是如果你将该变量定义为一个实例变量，就会导致生命周期长一些，甚至如果将该变量定义为静态变量（类变量），这样就意味着该变量会随着类的加载而加载，随着类的消亡而消亡，那么这个变量的生命周期就会非常长。
* 尽管内存泄露并不会立刻引起程序崩溃，但是一旦发生内存泄漏，程序中的可用内存就会被逐步蚕食，直至耗尽所有内存，最终出现 OutOfMemoryError，导致程序崩溃。
* 注意：这里的存储空间并不是指物理内存，而是指虚拟内存大小，这个虚拟内存大小取决于磁盘交换区设定的大小。
##### 内存泄露举例：
* 单例模式
（1）单例模式就是我们创建了一个对象，这个对象中只有一个对象，该对象是被 static 修饰的。
（2）静态变量（类变量）会随着类的加载而加载、随着类的消亡而消亡，那么这个变量的生命周期是很长的。如果这个静态变量持有对外部对象的引用的话，那么这个外部对象是不能被回收的，但是这个外部对象可能用一段时间就不用了，此时就会导致内存泄漏的发生。
* 一些提供 close 的资源未关闭导致内存泄漏。
数据库连接（dataSource.getConnection()）、网络连接（socket）和 IO 连接必须手动 close，此时 GC 时才能回收这些对象。否则这些对象由于没有关闭资源连接，就不能及时被回收，只有在程序结束时才会回收。**在不使用的时候没能及时回收，就会发生内存泄露。** 
* 注意：在面试时尽量不要举循环引用的例子。因为 Java 采用的是可达性分析算法，而不是引用计数算法。
### 16.3 Stop the world
* Stop the world，简称 STW，指的是 **GC 事件发生过程中**，会产生应用程序的停顿。**停顿产生时整个应用程序线程都会被暂停，没有任何响应**，像卡死一样，这个停顿称为 STW。
* 被 STW 中断的应用程序线程会在完成 GC 之后恢复，频繁中断会让用户感觉像是网速不快造成电影卡顿一样，所以我们需要减少 STW 的发生。
* STW 事件和采用哪款垃圾回收器无关，**所有的垃圾回收器都会发生 STW**。只能说垃圾回收器越来越优秀，回收效率越来越高，尽可能地缩短了暂停时间。
* STW 是 JVM 在**后台自动发起和自动完成**的。在用户不可见的情况下，把用户线程全部停掉。
* 开发中**不要使用 System.gc()，因为会触发 full gc，从而导致 STW 的发生。**
### 16.4 垃圾回收的并发与并行
#### 16.4.1 并发（Concurrent）
* 在操作系统中，是指**一个时间段内**有几个程序都处于已启动运行到运行完毕之间，且这几个程序都是在同一个处理器上运行。
* 并发不是真正意义上的 “同时运行”，只是 CPU 把一个时间段划分成几个时间片段（时间区间），然后在这几个时间区间之间来回切换，由于 CPU 的处理速度非常快，只要时间间隔处理得当，就可以让用户感觉是多个应用程序在同时运行。
#### 16.4.2 并行（Parallel）
* 当系统中有两个及以上 CPU 时，当一个 CPU 执行一个进程时，另一个 CPU 可以执行另一个进程，两个进程互不抢占 CPU 资源，可以同时进行，我们称之为并行
* 其实**决定并行的因素**不是 CPU 的数量，而**是 CPU 的核心数量**，比如一个 CPU 多个核也可以并行。
* 适合科学运算、后台处理等弱交互场景。
#### 16.4.3 并发和并行的对比
![图示](https://img-blog.csdnimg.cn/cc31485b5ec84429aef39e2dcd568f55.png)
#### 16.4.4 垃圾回收的并发与并行
* **并发：指用户线程与垃圾收集线程 “同时” 执行，垃圾回收线程在执行时不会停顿用户程序的运行**。如：CMS、G1
* **并行：指多条垃圾收集线程并行工作，但此时用户线程仍处于等待状态**。如：ParNew、Parallel Scavenge、Parallel Old
* 串行：相较于并行的概念，**单线程执行**。如果内存不够，则程序暂停，启动 JVM 垃圾回收器进行垃圾回收。回收完，再启动程序的线程。
### 16.5 安全点与安全区域
#### 16.5.1 安全点（Safepoint）
* 程**序执行时并非在所有的地方都能停顿下来开始 GC，只有在特定的位置才能停顿下来开始 GC，这些位置称为安全点。**
* 安全点的选择很重要，**如果太少可能导致 GC 等待的时间太长，如果太频繁可能导致运行时的性能问题**。大部分指令的执行时间都非常短暂，通常会根据 “**是否具有让程序长时间执行的特征**” 为标准。比如：选择一些执行时间较长的指令作为安全点，如：方法调用、循环跳转和异常跳转等。
#### 16.5.2 安全区域（Safe Region）
* 安全点机制保证了程序执行时，在不太长的时间内就会遇到可进入 GC 的安全点。但是程序不执行的时候呢？例如线程处于 Sleep 状态或 Blocked 状态，这时候线程无法响应 JVM 的中断请求，不能再走到安全点去中断挂起，JVM 也不太能等待线程被唤醒。对于这种情况，就需要安全区域来解决。
* **安全区域是指能够确保在某一段代码片段中，对象的引用关系不会发生变化，在这个区域中的任何位置开始 GC 都是安全的**。我们也可以把安全区域看作被拉伸了的安全点。
* 实际执行时：
（1）当用户线程执行到安全区域的代码时，首先会标识已经进入了安全区域，那样当这段时间里虚拟机要发起垃圾收集时就不必去管这些已经声明自己在安全区域内的线程。
（2）当用户线程即将离开安全区域时，会检查 JVM 是否已经完成 GC，如果完成了，则继续运行，否则线程必须等待直到收到可以离开安全区域的信号为止。
### 再谈引用
![图示](https://img-blog.csdnimg.cn/a4fc776f53314621b067b8c8d3cb5b9e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* Reference 子类中只有终结器引用是包内可见的，其它 3 中引用类型均为 public，可以在应用程序中直接使用。
![图示](https://img-blog.csdnimg.cn/8cc3ee3576b9474ab7c1faae3faeacf3.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 16.6 再谈引用：强引用
* 强引用（StrongReference）：最传统的引用的定义，是指在程序代码之中普遍存在的引用赋值，即类似 “Object obj = new Object()” 这种引用关系。**无论任何情况下，只要强引用关系还存在（即可达的），垃圾收集器就永远不会回收掉被引用的对象。**
* 在 Java 程序中，最常见的引用类型是强引用（**普通系统 99% 以上都是强引用**），也就是我们最常见的普通对象引用，**也是默认的引用类型。**
* **只要强引用的对象是可触及的（可达的），垃圾收集器就永远不会回收掉被引用的对象。**
* 相对的，**软引用、弱引用和虚引用的对象是软可触及、弱可触及、虚可触及的，在一定条件下，都是可以被回收的（软引用、弱引用和虚引用的对象即使可达，在一定情况下也会被回收）。所以，强引用是造成 Java 内存泄漏的主要原因之一。**
* 强引用所指向的对象在任何时候都不会被系统回收，虚拟机宁愿抛出 OOM 异常，也不会回收强引用所指向的对象。
### 16.7 再谈引用：软引用
* 软引用（SoftReference）：**在系统将要发生内存溢出之前，即使软引用的对象是可达的，也会把这些对象列入回收范围之中进行第二次回收（第一次回收指的是那些不可达的对象）**，如果这次回收后还没有足够的内存，才会抛出内存溢出异常。
* **当内存足够时，不会回收软引用的可达对象；当内存不够时，才会回收软引用的可达对象。**
* 软引用是用来描述一些还有用，但非必需的对象。
* 软引用通常用来实现内存敏感的缓存。比如：**高速缓存**就有用到软引用。如果还有空闲内存，就可以暂时保存缓存。当内存不足时清理掉缓存，这样就保证了内存不会被耗尽。
* 垃圾回收器在某个时刻决定回收软可达的对象的时候，会清理软引用，并可选地把引用存放到一个引用队列。
* 在 JDK1.2 版本后提供了 **java.lang.ref.SoftReference 类来实现软引用。**
### 16.8 再谈引用：弱引用
* 弱引用（WeakReference）：被弱引用关联的对象只能生存到下一次垃圾收集之前。也就是说，**当垃圾收集器工作时，无论内存空间是否足够，都会回收掉被弱引用关联的对象。**
* 但是，由于垃圾回收器的线程通常优先级很低，因此并不一定能很快地发现持有弱引用的对象。在这种情况下，弱引用对象可以存在较长的时间。
* 弱引用和软引用一样，在构造弱引用时，也可以指定一个引用队列，当弱引用对象被回收时，就会加入指定的引用队列，通过这个队列可以跟踪对象的回收情况。
* **软引用和弱引用都非常适合来保存那些可有可无的缓存数据**。当系统内存不足时，这些缓存数据会被回收，不会导致内存溢出。而当内存资源充足时，这些缓存数据又可以存在相当长的时间，从而起到加速系统的作用。
* 在 JDK1.2 版本后提供了 **java.lang.ref.WeakReference 类来实现软引用。**
### 16.9 再谈引用：虚引用
* 虚引用（PhantomReference）：一个对象是否有虚引用的存在，完全不会决定对象的生命周期，如果一个对象仅持有虚引用，那么它和没有引用几乎是一样的，随时都可能被垃圾回收器回收。**为一个对象设置虚引用关联的唯一目的就是跟踪垃圾回收过程。比如：能在这个对象被收集器回收时收到一个系统通知。**
* 当试图通过虚引用的 get() 方法获取对象时，会得到 null
* **虚引用必须和引用队列一起使用。虚引用在创建时必须提供一个引用队列作为参数。当我们发现垃圾回收器准备回收的对象是一个虚引用的时候，就会在回收对象后，将这个虚引用加入引用队列**，以通知应用程序对象的回收情况。
* 由于虚引用可以跟踪对象的回收时间，因此，也可以将一些资源释放操作放置在虚引用中执行和记录。
* 在 JDK1.2 版本后提供了 **java.lang.ref.PhantomReference 类来实现软引用。**
### 16.10 再谈引用：终结器引用
* 终结器引用（FinalReference）：它用于实现对象的 finalize() 方法。
* 无需手动编码，其内部配合引用队列使用。
* 在 GC 时，终结器引用加入队列，由 Finalizer 线程通过终结器引用找到被引用的对象并调用它的 finalize() 方法，第二次 GC 时才能回收被引用的对象。
## 十七、垃圾回收器
* **垃圾收集器**没有在规范中进行过多的规定，**可以由不同的厂商、不同版本的 JVM 来实现。**
### 17.1 GC 分类与性能指标
#### 17.1.1 GC 分类
* 按照**工作模式**分，可以分为**并发式垃圾回收器和独占式垃圾回收器。**
（1）并发式垃圾回收器与用户线程交替工作，以尽可能减少应用程序的停顿时间。
（2）独占式垃圾回收器（Stop the world）一旦运行，就停止应用程序中的所有用户线程，直到垃圾回收过程完全结束。
* 按照**线程数**分，可以分为**串行垃圾回收器和并行垃圾回收器。**
![图示](https://img-blog.csdnimg.cn/87129124607643cab839d455aedddc47.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
**注：并行回收和串行回收一样，采用独占式，使用了 Stop the world 机制。**
* 按照**碎片处理方式**分，可分为**压缩式垃圾回收器和非压缩式垃圾回收器。**
（1）压缩式垃圾回收器会在回收完成后，对存活对象进行压缩整理，消除回收后的碎片。
（2）非压缩式的垃圾回收器不进行这步操作。
* 按照**工作的内存区间**分，可分为**年轻代垃圾回收器和老年代垃圾回收器。**
#### 17.1.2 评价 GC 的性能指标
* **吞吐量：运行用户代码的时间占总运行时间的比例**。总运行时间 = 程序的运行时间 + 内存回收的时间。
* **暂停时间：执行垃圾收集时，程序的工作线程被暂停的时间。**
* **内存占用：Java 堆区所占的内存大小。**
* 这三者共同构成一个 “不可能三角”。三者总体的表现会随着技术进步而越来越好。一款优秀的收集器通常最多同时满足其中的两项。
* 这三项里，暂停时间的重要性日益凸显。因为随着硬件的发展，内存越来越便宜，可以分配的内存空间也越来越大了。内存空间大了，垃圾回收就不用特别频繁，相应的吞吐量就提升了，但是一次垃圾回收所需要的时间就更长了（因为内存大了，垃圾也就更多了），因此会对延迟带来负面影响。
* 简单来说，主要抓住两点：
（1）吞吐量
（2）暂停时间
* 吞吐量优先时，应用程序能容忍较高的暂停时间，快速响应是不必考虑的。
![tushi](https://img-blog.csdnimg.cn/20931493cb194879825c9fd046b37d1b.png)
* **暂停时间优先，意味着尽可能让单次 STW 的时间最短**
![图示](https://img-blog.csdnimg.cn/bd7519bb5508441a98dcd8e41397ffc6.png)
* 高吞吐量较好是因为这会让应用程序的用户感觉只有应用程序线程在做 “生产性” 工作。直觉上，吞吐量越高程序运行越快。
* 低暂停时间（低延迟）较好是因为从用户的角度来看，不管是 GC 还是其他原因导致一个应用被挂起始终是不好的，这取决于应用程序的类型，有时候甚至短暂的 200 毫秒暂停都可能打断终端用户体验。因此，具有低的较大暂停时间是非常重要的，特别是对于一个**交互式应用程序。**
* 高吞吐量和低暂停时间是一对相互竞争的目标（矛盾）
**（1）如果选择吞吐量优先，那么必然需要降低内存回收的执行频率，但是这样会导致 GC 时需要更长的暂停时间来执行内存回收。
（2）如果选择低延迟优先，那么为了降低每次执行内存回收时的暂停时间，也只能频繁地执行内存回收，但是这又会导致程序吞吐量的下降。**
* 在设计 GC 算法时，我们必须明确我们的目标：一个 GC 算法只可能针对两个目标之一（即只专注较大吞吐量或较小暂停时间），或尝试找到两者的折衷。
* 现在的标准：**在最大吞吐量优先的情况下，降低暂停时间。**
### 17.2 不同的垃圾回收器概述
#### 17.2.1 垃圾收集器发展史
![图示](https://img-blog.csdnimg.cn/9f2c1638f615491489f01d78e36e0b3a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 17.2.2 七款经典的垃圾收集器
* 串行回收器：Serial、Serial Old
* 并行回收器：ParNew、Parallel Scavenge、Parallel Old
* 并发回收器：CMS、G1
#### 17.2.3 七款经典收集器与垃圾分代之间的关系
![图示](https://img-blog.csdnimg.cn/a7fae05a0da94669902612568c6d92f9.png)
* 新生代收集器：Serial、ParNew、Parallel Scavenge
* 老年代收集器：Serial Old、Parallel Old、CMS
* 整堆收集器：G1
#### 17.2.4 垃圾收集器的组合关系
* 一个新生代的垃圾回收器要对应着一个老年代的垃圾回收器一起使用。
![图示](https://img-blog.csdnimg.cn/31cb19bde2e54e04943627e725f05863.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
![图示](https://img-blog.csdnimg.cn/a1378fb2ad37451b968c30f6fd6d045e.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 为什么要有很多收集器，一个不够吗？
因为 Java 的使用场景很多：移动端、服务器等。所以我们就需要**针对不同的场景，选择合适的垃圾收集器，以提高垃圾收集的性能。**
#### 17.2.5 如何查看默认的垃圾收集器
* ``-XX:+PrintCommandLineFlags``：查看命令行相关参数（包含使用的垃圾收集器）
* 使用命令行指令：``jinfo -flag 相关垃圾回收器参数 进程ID``。使用命令行指令 ``jps`` 查看进程ID
### 17.3 Serial 回收器：串行回收
* Serial 收集器是最基本、历史最悠久的垃圾收集器了。JDK1.3 之前回收新生代唯一的选择。
* Serial 收集器作为 HotSpot 中 Client 模式下的默认新生代垃圾收集器。
* **Serial 收集器采用复制算法、串行回收和 Stop-the-World 机制的方式执行内存回收。**
* 除了年轻代以外，Serial 收集器还提供用于执行老年代垃圾收集的 Serial Old 收集器。**Serial Old 收集器同样采用了串行回收和 Stop the World 机制，只不过内存回收算法使用的是标记-压缩算法。**
* Serial Old 在 Server 模式下主要有两个用途：
（1）与新生代的 Parallel Scavenge 配合使用
（2）作为老年代 CMS 收集器的后备垃圾收集方案。
* **Serial 收集器是一个单线程的收集器**，但是它的单线程的意义并不仅仅说明它**只会使用一个 CPU 或一条收集线程去完成垃圾收集工作**，更重要的是**它进行垃圾收集时，必须暂停其他所有的工作线程（Stop the world）**，直到它收集结束。
![图示](https://img-blog.csdnimg.cn/dfbdcbf4bc3e42ddb18fd27803d19af6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 优势：**简单而高效**（与其它收集器的单线程相比）。对于限定单个 CPU 的环境来说，Serial 收集器由于没有线程交互的开销，专心做垃圾收集自然可以获得更高的单线程收集效率。运行在 Client 模式下的虚拟机是个不错的选择。
* 在用户的桌面应用场景中，可用内存一般不大（几十 MB 至一两百 MB），可以在较短的时间内完成垃圾收集（几十 ms 至一百多 ms），只要不频繁发生，使用串行回收器是可以接受的。
* 在 HotSpot 虚拟机中，使用 ``-XX:+UseSerialGC``参数可以指定年轻代和老年代都使用串行收集器。也就是**新生代使用 Serial GC，老年代使用 Serial Old GC。**
* 这种垃圾收集器大家了解一下就行，现在基本上已经不再使用串行的垃圾收集器了。而且限定在单核的 CPU 场景下才可以用，现在都不是单核的了。
* 对于交互较强的应用而言，这种垃圾收集器是不能接受的。一般在 Java web 应用程序中是不会采用串行垃圾收集器的。
### 17.4 ParNew 回收器：并行回收
* **Par 是 Parallel 的缩写，New 表示只能处理新生代。**
* 如果说 Serial 收集器是年轻代中的单线程垃圾收集器，那么 ParNew 收集器则是 Serial 收集器的多线程版本。
* ParNew 收集器除了采用**并行回收**的方式**回收年轻代**以外，和 Serial 收集器几乎没有任何区别。ParNew 收集器**在年轻代同样也是采用复制算法、Stop-the-Woeld 机制。**
* ParNew 是很多 JVM 运行在 Server 模式下新生代的默认垃圾收集器。
* 对于新生代，回收次数频繁，采用并行的方式更高效；对于老年代，回收次数少，使用串行的方式节省资源（CPU 并行需要切换线程，串行可以省去切换线程的资源）
![图示](https://img-blog.csdnimg.cn/aba5c78f1dd54814a4910acf278bda35.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 由于 ParNew 收集器是基于并行回收，那么是否可以断定 ParNew 收集器的回收效率在任何场景下都会比 Serial 收集器更高效？
（1）ParNew 收集器运行在多 CPU 的环境下，由于可以充分利用多 CPU、多核心等物理硬件资源优势，可以更快速地完成垃圾收集，提升程序的吞吐量。
（2）但是**在单个 CPU 的环境下，ParNew 收集器不比 Serial 收集器更高效**。虽然 Serial 收集器是基于串行回收，但是 CPU 不需要频繁地做任务切换，因此可以有效避免多线程交互过程中产生的一些额外开销。
* ``-XX:+UseParNewGC``：指定新生代使用 ParNew 收集器。
* ``-xx:ParallelGCThreads``：限制垃圾收集线程的数量，默认开启和 CPU 数目相同的线程数。
### 17.5 Parallel 回收器：吞吐量优先
* HotSpot 的年轻代中除了 ParNew 收集器是基于并行回收的以外，Parallel Scavenge 收集器同样也是采用了**复制算法、并行回收和 Stop the World 机制。**
* 那么 Parallel 收集器的出现是否多此一举？
（1）和 ParNew 收集器不同，Parallel Scavenge 收集器的目标则是达到一个**可控制的吞吐量**，它也被称为吞吐量优先的垃圾收集器。
（2）**自适应调节策略**也是 Parallel Scavenge 与 ParNew 的一个重要区别。
* **高吞吐量**可以高效率地利用 CPU 时间，尽快完成程序的运行任务，主要**适合在后台运算而不需要太多交互的任务**。因此，常见在服务器环境中使用。**例如：执行批量处理、订单处理、科学计算的应用程序。**
* Parallel 收集器在 JDK1.6 时提供了用于执行老年代垃圾收集的 Parallel Old 收集器，用来代替老年代的 Serial Old 收集器。
* Parallel Old 收集器采用了**标记-压缩算法**，但同样也是基于**并行回收和 Stop the Woeld 机制。**
![图示](https://img-blog.csdnimg.cn/dbf89c77bd27437e8af116568a7bae6a.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 在程序吞吐量优先的应用场景中，Parallel 收集器和 Parallel Old 收集器的组合，在 Server 模式下的内存回收性能很不错。
* 在 Java8 中，默认使用的是 Parallel 垃圾收集器。
#### 参数配置
* ``-XX:+UseAdaptiveSizePolicy``：**设置 Parallel Scavenge 收集器具有自适应调节策略。**
（1）这种模式下，**年轻代的大小、Eden 和 Survivor 的比例、晋升老年代的对象年龄等参数会被自动调整，以达到在堆大小、吞吐量和停顿时间之间的平衡点。**
（2）在手动调优比较困难的场合，可以直接使用这种自适应的方式，仅指定虚拟机的最大堆、目标的吞吐量（GCTimeRatio）和停顿时间（MaxGCPauseMills），让虚拟机自己完成调优工作。
![图示](https://img-blog.csdnimg.cn/a0c1c39c977144e3a39a87588a26dad7.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
### 17.6 CMS 回收器：低延时
* 在 JDK1.5 时期，HotSpot 推出了一款在**强交互应用**中几乎可以认为有划时代意义的垃圾收集器：**CMS（Concurrent-Mark-Sweep）**收集器，这款收集器**是 HotSpot 虚拟机中第一款真正意义上的并发收集器，它第一次实现了让垃圾收集线程与用户线程同时工作。**
* CMS 收集器的关注点是**尽可能缩短垃圾收集时用户线程的停顿时间。停顿时间越短（低延迟）就越适合与用户交互的程序**，良好的响应速度能提升用户体验。
* 目前**很大一部分的 Java 应用集中在互联网站或者 B/S 系统的服务器上**，这类引用尤其重视服务的响应速度，希望系统停顿时间最短，以给用户带来较好的体验。CMS 收集器就非常符合这类应用的需求。
* CMS 的垃圾收集算法**采用标记-清除算法，并且也会 Stop-the-World**
* 不幸的是，CMS 作为老年代的收集器，却无法与 JDK1.4 中已经存在的新生代收集器 Parallel Scavenge 配合工作，所以在 JDK1.5 中使用 CMS 来收集老年代的时候，新生代只能选择 ParNew 或者 Serial 收集器中的一个。
* 在 G1 出现之前，CMS 的使用还是非常广泛的。一直都今天，仍然有很多系统使用 CMS GC。
#### CMS 的工作原理
![图示](https://img-blog.csdnimg.cn/0980bae1c8844d2eab3ef3a74a6926bc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* CMS 整个过程比之前的收集器要复杂，整个过程分为 4 个主要阶段：**初始标记阶段、并发标记阶段、重新标记阶段和并发清除阶段。**
![图示](https://img-blog.csdnimg.cn/b1b78a89aae344c3a4dccf97d7324526.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 注：**重新标记阶段处理的是那些本该存活但被错误标记为垃圾的对象**。比如：在并发标记阶段，在遍历对象图时发现某个对象没有引用（垃圾），但是由于用户线程在并发执行，这期间可能导致遍历过的这个对象又被其他对象所引用（又不是垃圾了）。所以才需要重新标记阶段再遍历一遍看看有没有漏标记的，否则就会导致被重新引用的对象被误清理掉。**主要用到增量更新算法做重新标记。**
* 尽管 CMS 收集器采用的是并发回收，但是在其**初始化标记和重新标记这两个阶段中仍然需要执行 Stop the World 机制来暂停程序中的工作线程**，不过暂停时间不会太长。因此可以说明目前所有的垃圾收集器都做不到完全不需要 Stop the World，只是尽可能地缩短暂停时间。
* **由于最耗费时间的并发标记阶段与并发清除阶段都不需要暂停工作，所以整体的回收是低停顿的。**
* 另外，由于在垃圾收集阶段用户线程没有中断，还有可能产生垃圾，所以**在 CMS 回收过程中，还应该确保应用程序的用户线程有足够的内存可用**。因此，CMS 收集器不能像其他垃圾收集器一样等到老年代几乎被填满了再进行回收，而是**当堆内存使用率达到某一阈值时，便开始进行回收**，以确保应用程序在 CMS 工作过程中仍然有足够的空间支持应用程序运行。要是 **CMS 运行期间预留的内存无法满足程序的需要**，就会出现一次 “**Concurrent Mode Failure**” 失败。这时虚拟机将启动后备预案：**临时启动 Serial Old收集器来重新进行老年代的垃圾收集，这样停顿时间就很长了。**
* 如果**内存增长较慢**，则可以**设置一个稍大的阈值**。大的阈值可以有效降低 CMS 的触发频率，减少老年代回收的次数，获得更好的性能。如果应用程序**内存使用率增长很快**，则应该**降低这个阈值**，以避免频繁触发老年代的串行收集器。
* CMS 收集器的垃圾收集算法采用的是**标记-清除算法**，这意味着每次执行完内存回收后，不可避免地**会产生一些内存碎片**。那么 CMS 在为新对象分配内存空间时，将无法使用指针碰撞技术，而只能够选择空闲列表执行内存分配。
* 既然 Mark Sweep 会造成内存碎片，那么为什么不把该算法换成 Mark Compact 呢？
在并发清理这一阶段，我们在清除垃圾的同时，用户线程还会继续执行。**要保证用户程序能够继续执行的前提是它运行的资源不受影响**。但是如果要使用标记-压缩算法的话，我们就需要对内存进行重新的整合，将对象压缩到内存的一端，此时对象的地址会发生改变，因此不能够使用标记-压缩算法。**标记-压缩算法更适合 Stop the World 这种场景下使用。**
#### CMS 的优缺点总结
* CMS 的优点：
（1）并发收集。
（2）低延迟。
* CMS 的缺点：
（1）**会产生内存碎片**，导致并发清除后，用户线程可用的空间不足。在没有足够大的连续空间来分配大对象的情况下，不得不提前触发 Full GC。
（2）**CMS 收集器对 CPU 资源非常敏感**。在并发阶段，它虽然不会导致用户线程停顿，但是会因为占用了一部分线程而导致应用程序变慢，总吞吐量降低。
（3）**CMS 收集器无法处理浮动垃圾**，有可能出现 “Concurrent Mode Failure” 失败而导致另一次 Full GC 的产生。在**并发标记阶段和并发清理阶段，由于用户线程还在继续运行，程序在运行自然就会伴随有新的垃圾对象不断产生**，但这一部分垃圾对象是出现在标记过程结束以后，CMS 无法在当次收集中处理掉它们，只好留在下一次垃圾收集时在清理掉，这一部分垃圾就称为**浮动垃圾**。
#### CMS 收集器的参数设置
![图示](https://img-blog.csdnimg.cn/628c22de238c4f9cb798b517ceb3fa55.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_18,color_FFFFFF,t_70,g_se,x_16)
#### JDK 后续版本中 CMS 的变化
![图示](https://img-blog.csdnimg.cn/50d3e57b381147adb00fc7030607c74f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 小结
* HotSopt 有那么多垃圾回收器，那么如果有人问，Serial GC、Parallel GC、Concurrent Mark Sweep GC 这三个 GC 有什么不同呢？
* 请记住以下口令：
（1）如果你想要最小化地使用内存和并行开销，请选 Serial GC；
（2）如果你想要最大化应用程序的吞吐量，请选 Parallel GC；
（3）如果你想要最小化 GC 的中断或停顿时间，请选 CMS GC；
### 17.7 G1 回收器：区域化分代式
* 既然我们已经有了前面几个强大的 GC，为什么还要发布 Garbage First GC ？
![图示](https://img-blog.csdnimg.cn/7deb69242dc641e982f678733eda42bb.png)
* **官方给 GC 设定的目标是在延迟可控的情况下获得尽可能高的吞吐量，所以才担当起 “全功能收集器” 的重任与期望。**
* GC 是一个并行回收器，它**把堆内存划分成多个大小相等的不相关的区域 Region（物理上可以不连续）**。每个 Region 都可以根据需要，来表示 Eden、幸存者 0 区、幸存者 1 区、老年代。
* G1 GC 有计划地避免在整个 Java 堆中进行全区域的垃圾收集，而是将 Region 作为单次回收的最小单元。GC 收集器会跟踪各个 Region 里面的垃圾堆积的价值大小（**价值即回收所获得的空间大小以及回收所需要时间的经验值**），然后**在后台维护一个优先级列表，每次根据允许的暂停时间，优先回收价值最大的 Region。这也就是 Garbage First 名字的由来。**
* G1 是一款面向服务端应用的垃圾收集器，**主要针对配备多核 CPU 及大容量内存的机器**，在以极其高概率满足 GC 停顿时间的同时，还兼具高吞吐量的性能特征。
* 在 JDK1.7 时，G1 移除了 Experimental 的表示。**在 JDK1.9 时 G1 成为了默认的垃圾回收器**，取代了 Parallel Scavenge 加 Parallel Old 的组合，同时 CMS 在 JDK1.9 时被声明为废弃（Deprecated）。
#### 17.7.1 G1 回收器的优势
* **并发与并行**
（1）并发性：GC 拥有与应用程序交替执行的能力，部分工作可以和应用程序同时执行。因此，一般来说不会在整个回收阶段发生完全阻塞应用程序的情况。
（2）并行性：G1 在回收期间，可以有多个 GC 线程同时执行，有效利用多核的计算能力。此时的用户线程会 STW。
* **分代收集**
（1）**G1 仍然是遵循分代收集理论设计的**，它会区分年轻代和老年代，年轻代依然有 Eden 区和 Survivor 区。但从堆内存的结构上看，它不要求整个 Eden 区、年轻代或者老年代都是连续的，也不再坚持固定大小和固定数量的分代区域划分。
（2）**将堆空间分为若干个区域（Region），这些区域中包含了逻辑意义上的年轻代和老年代。**
（3）G1 收集器同时**兼顾年轻代和老年代**。对比其他回收器，要么只能工作在年轻代，要么只能工作在老年代。
* **空间整合**
G1 将内存划分为一个个的 Region。内存的回收是以 Region 作为基本单位的。**Region 之间是复制算法**，但整体上实际可看作是**标记-压缩算法**，这两种算法都可以避免内存碎片。这种特性有利于程序长时间运行，分配大对象时不会因为无法找到连续的内存空间而提前触发一次 GC。尤其是当 Java 堆非常大的时候，G1 的优势更加明显。
* **可预测的停顿时间模型**
（1）停顿时间模型的意思是能够支持**指定在一个长度为 M 毫秒的时间片段内，消耗在垃圾收集上的时间大概率不超过 N 毫秒**这样的目标。
（2）在 G1 收集器出现之前的所有其它收集器，垃圾收集的目标范围要么是整个新生代（Minor GC），要么是整个老年代（Major GC），要么就是整个 Java 堆（Full GC）。而 G1 可以只选取堆的部分区域进行内存回收，这样就缩小了回收的范围，因此对于发生全局停顿的情况也能得到较好的控制。
（3）G1 跟踪各个 Region 里面的垃圾堆积的价值大小（价值即回收所获得的空间大小以及回收所需时间的经验值），在后台维护一个优先列表，**每次根据允许的暂停时间，优先回收价值最大的 Region**，保证了 G1 收集器**在有限的时间内可以获取尽可能高的收集效率**。
（4）相对于 CMS GC，G1 未必能做到 CMS 在最好情况下的延时停顿，但是最差情况要好很多。
#### 17.7.2 G1 回收器的劣势
* 相较于 CMS，G1 还不具备全方位、压倒性的优势。比如在用户程序运行过程中，G1 无论是为了垃圾收集产生的内存占用（Footprint）还是程序运行时的额外执行负载（Overload），都要比 CMS 高。
* **G1 收集器要比其他的传统垃圾收集器有着更高的内存占用负担。根据经验，G1 至少要消耗大约相当于 Java 堆容量 10% 至 20% 的额外内存来维持收集器工作。**
* 从经验上来说， **在小内存应用上 CMS 的表现大概率会优于 G1，而 G1 在大内存应用上则发挥其优势**。平衡点在 6 - 8 GB 之间。
#### 17.7.3 G1 回收器的参数设置
![图示](https://img-blog.csdnimg.cn/c2144568b8dd4dbfa054d75042989d27.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 17.7.4 G1 回收器的常见操作步骤
* G1 的设计原则就是简化 JVM 性能调优，开发人员只需要简单的三步即可完成调优：
（1）开启 G1 垃圾收集器
（2）设置堆的最大内存
（3）设置最大的停顿时间
* G1 中提供了三种垃圾回收模式：Young GC、Mixed GC 和 Full GC，在不同的条件下被触发。
#### 17.7.5 G1 回收器的适用场景
* 面向服务端应用，针对**具有大内存、多处理器的机器**（在普通大小的堆里表现并不惊喜）
* 最主要的应用是**需要低 GC 延迟，并具有大堆的应用程序**提供解决方案。
* 如：在堆大小大约 6 GB 或更大时，可预测的暂停时间可以低于 0.5 秒（G1 通过每次只清理一部分而不是全部的 Region 的增量式清理来保证每次 GC 的停顿时间不会过长）
* 用来替换掉 JDK1.5 中的 CMS 收集器。在以下的情况时，使用 G1 可能比 CMS 更好：
（1）超过 50% 的 Java 堆被活跃数据占用。
（2）对象分配频率或年代提升频率变化很大。
（3）GC 停顿时间过长（长于 0.5 至 1 秒）
* HotSpot 垃圾收集器里，除了 G1 以外，其它的垃圾收集器使用的是内置的 JVM 线程执行 GC 的多线程操作，而 **G1 可以采用应用线程承担后台运行的 GC 工作，即当 JVM 的 GC 线程处理速度慢时，系统会调用应用程序线程来帮助加速垃圾回收过程。**
#### 17.7.6 Region 介绍
* 使用 G1 收集器时，它将整个 Java 堆划分成约 2048 个大小相同的独立 Region 块，每个 Region 块大小根据堆空间的实际大小而定。**每个 Region 的大小**可以通过参数``-XX:G1HeapRegionSize``来设定，**取值范围为 1MB~32MB，且应为 2 的 N 次幂**，即 1MB、2MB、4MB、8MB、16MB、32MB。**所有的 Region 大小相同，且在 JVM 生命周期内不会被改变。**
* 一个 Region 可能之前是年轻代，进行了垃圾回收之后，可能又会变成老年代，也就是说 Region 的区域功能可能会动态变化。
* 虽然还保留有年轻代和老年代的概念，但不再是物理隔离的了，它们都是（不需要连续）Region 的集合。通过 Region 的动态分配方式实现逻辑上的连续。
![图示](https://img-blog.csdnimg.cn/e0aa910b1a1d454db0f709840e3c2bc4.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* Humongous 区域专门用来存储大对象。G1 认为只要**超过 region 大小一半的对象就是大对象。**
* 设置 H 区的原因：对于堆中的大对象，默认直接会被分配到老年代，但是如果它是一个短期存在的大对象，就会对垃圾收集器造成负面影响（老年代的垃圾回收频率比较低，导致该对象会一直占据内存）。为了解决这个问题，G1 划分了 Humongous 区，它专门用来存放大对象。**如果一个 H 区装不下一个大对象，那么 G1 会寻找连续的 H 区来存储**。为了能找到连续的 H 区，有时候不得不启动 Full GC。G1 的大多数行为都把 H 区作为老年代的一部分来看待。 
* Region 的细节：
![图示](https://img-blog.csdnimg.cn/119c92d2dbe14e629d224e0138b51cce.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 17.7.7 记忆集（Remembered Set）
* **跨代引用：一个对象被不同的区域引用**。一个 Region 不可能是孤立的，一个 Region 中的对象有可能正被其它 Region 中的对象引用。
* 例如：如果年轻代中的某个对象被老年代中的对象所引用，那么我们在进行 Young GC 时，不仅要把年轻代遍历一遍，还要把所有的老年代也都遍历一遍，看一下该对象有没有被老年代中的对象所引用，这样效率是很低的。因为我们在进行 YGC 时，相当于是将全堆作为 GC Roots 扫描。
* **记忆集：记录从非收集区到收集区的指针集合**。引入记忆集可以避免把整个老年代加入 GCRoots 的扫描范围。事实上不只是新生代、老年代之间才有跨代引用的问题，所有涉及部分区域收集行为的垃圾收集器，如：G1、ZGC 都会面临相同的问题。
* 在垃圾收集场景中，**垃圾收集器只需要通过记忆集判断出某一块非收集区域是否存在执行收集区域的指针即可，无需了解跨代引用指针的全部细节。**
* **hotspot 使用一种叫做卡表的方式来实现记忆集**。关于卡表与记忆集的关系，可以类比为 Java 语言中 HashMap 和 Map 的关系。
* **卡表是使用一个字节数组来实现的：CARD_TABLE[]，每个元素对应着其表示的内存区域一块特定大小的内存块，称为 “卡页”。HotSpot 使用的卡页大小是 512 字节。**
* 一个卡页中可能包含多个对象，**只要有一个对象的字段存在跨代指针，那就将其对应卡表的元素的值标识为 1，表示该元素变脏**，否则为 0。在 GC 时，只要筛选出卡表中变脏的元素加入 GCRoots 中。
* 上边已经解决了GC Roots扫描范围的问题，但是还没有解决卡表元素如何维护的问题，例如他们何时变脏、谁把他们变脏等。
（1）卡表元素变脏时机是很明确的，**在其他分代区域中对象引用了本区域对象时，其对应的卡表元素就应该变脏**，变脏的时间点原则上应该发生在引用字段赋值的那一刻。
（2）**在HotSpot虚拟机里是通过写屏障（Write Barrier）技术去维护卡表状态的**。应用写屏障后，虚拟机就会对所有的赋值操作生成相应的指令，一旦收集器在写屏障中增加了更新卡表的操作，无论更新的是否为老年代对新生代的引用，每次只要对引用进行更新，就会产生额外的开销，不过这个开销与Minor GC时扫描整个老年代的代价相比还是低的多的。
* 解决方案：无论 G1 还是其它分代收集器，JVM 都是使用记忆集来避免全堆扫描。
（1）**每个 Region 都维护一个自己的记忆集。**
（2）每次引用类型字段赋值时，都会产生一个写屏障（Write Barrier）来暂时中断操作。
（3）然后检查将要写入的引用对象是否和该引用对象在不同的 Region（其它收集器：检查老年代对象是否引用了新生代对象）
（4）如果不同，那么把相关引用信息记录到引用指向对象所在 Region 对应的卡表（CardTable）中。
（5）当进行垃圾收集时，将记忆集加入到 GC 根结点的枚举范围内，这样就可以保证即使不进行全堆扫描，也不会有遗漏。
![图示](https://img-blog.csdnimg.cn/bf95e9bc5ce94f7b8d65cd5fa3784712.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
#### 17.7.8 G1 垃圾回收过程
* （1）初始标记
仅仅是标记 GC Roots 能直接关联到的对象。该阶段需要停顿用户线程，但速度很快。
* （2）并发标记
从 GC Roots 的直接关联对象开始递归遍历整个堆里的对象图的过程。该阶段耗时较长，但是可与用户线程并发执行。
* （3）最终标记
主要是为了修正在并发标记期间，因为用户线程继续运行而导致标记记录产生变动的那一部分对象的标记记录（原本存活的对象被错误标记为垃圾）。需要 stop the word。采用了原始快照的方式。
* （4）筛选回收
对各个 Region 分区的回收价值和成本进行排序，根据用户所期望的停顿时间来制定回收计划。该阶段需要停顿用户线程，由多条收集器线程并行完成。
* 比如说老年代此时有 1000 个 Region 满了，但是因为预期停顿时间，本次垃圾回收只能停顿 200 毫秒，那么通过之前回收成本计算得知，可能回收其中 800 个 Region 正好需要 200 ms，那么就只会回收 800 个 Region（**Collection Set，要回收的集合**），即使另外 200 个 Region 满了，这一次也不会对其进行回收。
#### 17.7.9 G1 回收器优化建议
* 年轻代大小
避免使用``-Xmn``或``-XX:NewRatio``等相关选项显式设置年轻代大小。因为**固定年轻代的大小会覆盖暂停时间目标**。年轻代的 YGC 是独占式的，如果设置的年轻代大小不合理就会导致无法达到暂停时间的目标，所以需要让 JVM 动态地调整。
* 暂停时间目标不要太过严苛
（1）G1 的吞吐量目标是 90% 的应用程序时间和 10% 的垃圾回收时间。
（2）暂停时间目标太过严苛，会**导致垃圾收集的频率更高**，使得垃圾回收的开销变大，从而直接**影响到吞吐量**。
### 17.8 垃圾回收器总结
* 截止到 JDK1.8，一共有 7 款不同的垃圾收集器。每一款垃圾收集器都有各自的特点。在具体使用的时候，需要根据具体的情况选用不同的垃圾收集器。
![图示](https://img-blog.csdnimg.cn/1397d59778014aa1bf93e5a843ac8bcb.png)
#### 怎么选择垃圾回收器
![图示](https://img-blog.csdnimg.cn/41ba4626ab034c9ebd2cbdb070863175.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 17.9 GC 日志分析
* 通过阅读 GC 日志，我们可以了解 Java 虚拟机内存分配与回收策略。
![图示](https://img-blog.csdnimg.cn/bfaa607242a84858bb8b42bc78bea9cd.png)
* 常见的 GC 日志分析工具：GCEasy、GCViewer
#### GC 日志说明
![图示](https://img-blog.csdnimg.cn/4f9efa9db4ed49369af089047dfedbba.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
![图示](https://img-blog.csdnimg.cn/404f41bacacf42ce8ccda3f2a6fb8236.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
### 17.10 垃圾回收器的新发展
* GC 仍然处于快速发展之中，目前的默认选项 **G1 还在不断的改进中**。很多我们原来认为的缺点，例如：串行的 Full GC、Card Table 扫描的低效等，都已经被大幅改进。例如：JDK10 以后，Full GC 已经是并行运行，在很多场景下，其表现还略优于 Parallel GC 的并行 Full GC 实现。
* 即使是 Serial GC，虽然比较古老，但是简单的设计和实现未必就是过时的。它本身的开销，不管是 GC 相关数据结构的开销，还是线程的开销，都是非常小的。所以随着云计算的兴起，**在 Serverless 等新的应用场景下，Serial GC 找到了新的舞台。**
* 比较不幸的是 CMS，因为其算法的理论缺陷等原因，虽然现在还有非常大的用户群体，但在 JDK9 中已经被标记为废弃，并在 JDK14 版本中移出。
#### OpenJDK12 的 shenandoah GC
![图示](https://img-blog.csdnimg.cn/9c910b02bdc64962abb10d7ca16b4e13.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
![图示](https://img-blog.csdnimg.cn/7ef1f2f827d44cb0b7f5d1a18810f8dc.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
* 总结：
（1）Shenandoah GC 的强项：**低延迟时间。**
（2）Shenandoah GC 的弱项：**高运行负担下的吞吐量下降。**
（3）Shenandoah GC 的工作过程大致分为 9 个阶段。
#### 革命性的 ZGC
* ZGC 与 Shenandoah GC 的目标高度相似，**在尽可能对吞吐量影响不大的前提下，实现在任意堆内存大小下都可以把垃圾收集的停顿时间限制在十毫秒以内的低延迟。**
* 《深入理解Java虚拟机》一书中这样定义 ZGC：ZGC 收集器是一款基于 Region 内存布局的，（暂时）不设分代的，使用了读屏障、染色指针和内存多重映射等技术来实现**可并发的标记-压缩算法**的，**以低延迟为首要目标**的一款垃圾收集器。
* ZGC 的工作过程可以分为 4 个阶段：**并发标记 - 并发预备重分配 - 并发重分配 - 并发重映射**等。
* ZGC 几乎在所有地方都并发执行，除了**初始标记是 STW 的**。所以停顿时间几乎就耗费在初始标记上，这部分的实际时间是非常少的。
#### 其它厂商的垃圾回收器
* AliGC 是阿里巴巴 JVM 团队基于 G1 算法，面向大堆的应用场景。
* 当然，其它厂商也提供了各种别具一格的 GC 实现，例如比较有名的低延迟 GC：Zing。