

# Maven 笔记
## 一、Maven 简介
### 1.1 软件是一个工程
软件工程：为了能够实现软件的流水线式生产，在设计和构建软件的时候能够有一种规范和工程化的方法，人们便提出了软件工程的概念。
### 1.2 传统开发项目（没有使用 maven）的问题：
（1）很多模块，模块之间有关系，手动管理关系比较繁琐。
（2）需要很多第三方功能，需要很多 jar 包，需要手工从网络中获取各个 jar 包。
（3）需要管理 jar 的版本，比如你需要的是 mysql.5.1.5.jar，那你就不能拿一个 mysql.4.0.jar。
（4）管理 jar 包之间的依赖。比如你的项目要使用 a.jar，a.jar 的使用需要用到 b.jar 里面的类，所以必须要先获取 b.jar，然后才能使用 a.jar。

注：a.jar 需要使用 b.jar 的这个关系叫做依赖。或者你的项目中要使用 mysql 的驱动，也可以叫做项目依赖 mysql 驱动。a.class 使用 b.class，a 类依赖于 b 类。
### 1.3 改进项目的开发和管理，需要 maven
（1）maven 可以管理 jar 包，比如自动下载 jar 包和它的文档、源代码。

（2）maven可以把 jar 包所依赖的其他 jar 包直接下载并引入项目。比如：a.jar 需要 b.jar，maven 会自动下载 b.jar。

（3）maven 可以管理你需要的 jar 包的版本

（4）帮你编译程序，把 java 编译为 class。

（5）帮你测试你的代码是否正确。

（6）帮你打包文件，形成 jar 文件或者 war 文件。

（7）帮你部署项目。
### 1.4 构建：项目的构建
构建是面向过程的，就是一些步骤，完成项目代码
1. 清理：把之前项目编译的东西删掉，为新的编译代码做准备
2. 编译：把程序的源代码编译为执行代码，java -> class 文件。这是批量的，maven 可以把成百上千个文件同时编译为 class
3. 测试：maven 可以执行测试程序代码，验证你的功能是否正确。这也是批量的，maven 可以同时执行多个测试文件，同时测试很多功能。
4. 报告：生成测试结果的文件
5. 打包： 把你的项目中所有的 class 文件、配置文件等所有资源放到一个压缩文件中，这个压缩文件就是项目的结果文件，通常 Java 程序的压缩文件的扩展名是 .jar，web 应用的压缩文件的扩展名是 .war。
6. 安装：把 5 中生成的文件（.jar 或者 .war）安装到本机仓库
7. 部署：把程序安装好可以执行
### 1.5 maven 核心概念
（1）POM：指的是一个文件，这个文件的名称是 pom.xml，pom 翻译过来是项目对象模型。maven 把一个项目当作一个模型来使用。可以控制 maven 构建醒目的过程、管理 jar 依赖。

（2）约定的目录结构：maven 项目的目录和文件的位置是有规定的。

（3）坐标：是一个唯一的字符串，用来表示资源的。

（4）依赖管理：管理你的项目中可以使用的 jar 文件。

（5）仓库管理（了解）：你的资源存放的位置。

（6）生命周期（了解）：maven 工具构建项目的过程，就是生命周期。

（7）插件和目标（了解）：执行 maven 构建的时候用的工具是插件。

（8）继承

（9）聚合
## 二、Maven 的核心概念
### 2.1 maven 约定的目录结构 
约定是大家都遵守的一个规则。

每一个 maven 项目在磁盘中都是一个文件夹。

maven 中约定的目录结构：
![图示](https://img-blog.csdnimg.cn/20210703110357666.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
mvn compile 命令用来编译 src/main 目录下的所有 Java 文件。

（1）为什么要下载：maven 工具执行的操作需要很多插件（Java类，形式都是 jar 文件）完成。

（2）下载什么东西了：jar 文件，在 maven 中叫插件。插件是完成某些功能的。

（3）下载的东西存放到哪里了：**默认仓库/本机仓库。地址：C:\Users\\(登陆操作系统的用户名)Administrator\\.m2\repository。**

**执行 mvn compile，结果是在项目的根目录下生成 target 目录（结果目录），maven 编译的 java 程序，最后的 class 文件都放在 target 目录中。**

maven 小结：
![图示](https://img-blog.csdnimg.cn/20210703144957726.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 2.2 仓库
* 仓库是什么？
仓库是用来存放东西的。存放 maven 使用的 插件（各种 jar 包）和我们项目使用的 jar 包。
* 仓库的分类
（1）本地仓库： 就是你的个人计算机上的文件夹，存放各种 jar 包。**可以在 maven安装目录/conf/settings.xml 中配置本地仓库的位置。** 例如：配置本地仓库位置：``<localRepository>D:/Maven/repository</localRepository>``
（2） 远程仓库： 在互联网上，只有使用网络才能使用的仓库。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;① 中央仓库： 最权威的，所有开发人员都共享使用一个集中的仓库。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;② 中央仓库的镜像：就是中央仓库的备份。在各大洲或者重要的城市都有中央仓库的镜像。主要用来分担中央仓库的压力。
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;③ 私服： 在公司的局域网内部使用，不对外开放。
* 仓库的使用
**仓库的使用不需要人为参与。**
比如：开发人员需要使用 mysql 驱动 ---> maven 首先查看本地仓库 ---> 本地仓库没有就查看私服（如果有私服的话）---> 私服没有就查看中央仓库的镜像 ---> 镜像也没有就查看中央仓库
### 2.3 pom
pom：项目对象模型，是一个 pom.xml 文件

（1）坐标（gav）：唯一值，在互联网中唯一标识一个项目。

Maven 把任何一个插件都作为仓库中的一个项目进行管理，用一组（三个）向量组成的坐标来表示。坐标在仓库中可以唯一定位一个 maven 项目。

项目在仓库中的位置是由坐标来决定的：groupId、artifactId 和 version 决定项目在仓库中的路径。artifactId 和 version 决定 jar 包的名称。
```xml
<groupId>公司域名的倒写</groupId>
<artifactId>自定义的项目名称</artifactId>
<version>自定义的版本号</version>
```
（2）packaging：打包后压缩文件的扩展名，默认是 jar，web 应用是 war

（3）依赖：dependencies 和 dependency，是你的项目中要使用的各种资源的说明。

一个 maven 项目正常运行需要其他项目的支持，maven 会根据坐标自动到本地仓库中进行查找，对于程序员自己的 maven 项目需要进行安装，才能保存到仓库中。

不用 maven 的时候所有的 jar 包都不是你的，需要去各个地方下载拷贝。用了 maven，所有的 jar 包都是你的，想要谁，叫谁的名字就行，maven 帮你下载。

**比如我的项目要使用 mysql 驱动，那么就在 pom.xml 中加入依赖：**
```xml
<dependencies>                                                                                                                                                    
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.9</version>
    </dependency>      
</dependencies>
```
（4）properties：设置属性
（5）build：maven 在进行项目的构建时配置的信息
![图示](https://img-blog.csdnimg.cn/20210703162537951.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 2.4 maven 的生命周期
maven 的生命周期就是构建项目的过程：清理、编译、测试、报告、打包、安装、部署。

对项目的构建是建立在生命周期模型上的，它明确定义项目生命周期的各个阶段，并且对于每一个阶段都提供相应的命令，对开发者而言仅仅需要掌握一小堆的命令就可以完成各个阶段的构建工作。

构建项目时按照生命周期的顺序构建，每一个阶段都由特定的插件来完成。不论现在要执行的是生命周期中的哪个阶段，都是从这个生命周期的最初阶段开始的。

对于我们程序员而言，无论我们要进行哪个阶段的构建，直接执行响应的命令即可，无需担心它前面的阶段是否构建，maven 都会自动构建，这也就是 maven 这种自动化构建工具给我们带来的好处。
### 2.5 maven 的命令
maven 的命令：maven 可以独立使用，通过命令，完成 maven 的生命周期的执行。maven 可以使用命令，完成项目的清理、编译、测试等等。

Maven 对所有的功能都提供相对应的命令，要想知道 maven 都有哪些命令，那就要先看 maven 都有哪些功能。**Maven 三大功能：管理依赖、构建项目、管理项目信息。** 管理依赖：只需要声明就可以自动到仓库去下载；管理项目信息：其实就是生成一个站点文档，一个命令就可以解决；maven 功能的主体其实就是项目构建。

Maven 提供一个项目构建的模型，把编译、测试、打包、部署等都对应成一个个的生命周期阶段，并对每一个阶段提供相应的命令。程序员只需要掌握一小堆的命令，就可以完成项目的构建过程。

**注：执行以下命令必须在命令行进入 pom.xml 所在目录。**

* mvn clean：清理（会删除原来编译和测试的目录，即 target 目录，但是已经 install 到仓库的包不会删除）。

* mvn compile：编译主程序（会在当前目录下生成一个 target，里边存放编译主程序之后生成的字节码文件。会把 main/java 目录下的 java 程序编译为 class 文件，同时把 class 拷贝到 target/classes 目录下面；把 main/resources 目录下的所有文件都拷贝到 target/classes 目录下）。

* mvn test-compile：编译测试程序（会在当前目录下生成一个 target，里边存放编译测试程序之后生成的字节码文件。会把 test/java 目录下的 java 程序编译为 class 文件，同时把 class 拷贝到 target/test-classes 目录下面）。

* mvn test：测试（会生成一个目录 surefire-reports，保存测试结果）。

* mvn package：打包主程序（会编译、编译测试、测试，并且按照 pom.xml 配置把主程序打包成 jar 包或者 war 包。只包含 src/main 下的所有东西）。

* mvn install：安装主程序（会把本工程打包，并且按照本工程的坐标保存到本地仓库中）。

* mvn deploy：部署主程序（会把本工程打包，并且按照本工程的坐标保存到本地仓库中，并且还会保存到私服仓库中。还会自动把项目部署到 web 容器中）。
### 2.6 maven 的插件
maven 命令执行的时候，真正完成功能的是插件，插件就是一些 jar 文件，一些类。

### 2.7 单元测试
单元测试（测试方法）：用的是 junit，junit 是一个专门用来测试的框架（工具）。

junit 测试的内容：测试的是类中的方法，每一个方法都是独立测试的。**方法是测试的基本单位（单元）**。

maven 可以借助单元测试，批量地测试你的类中的大量方法是否符合预期。

**在 pom.xml 中加入单元测试依赖：**
![图示](https://img-blog.csdnimg.cn/20210712160224498.png)
## 三、在 IDEA 中使用 Maven
idea 中内置了 maven，但我们一般不使用内置的，因为用内置的 maven 的话，修改配置不方便。

使用自己安装的maven，需要覆盖 idea 中的默认的设置，让 idea 知道 maven 的安装位置等信息。
![图示](https://img-blog.csdnimg.cn/20210704140216190.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
使用模板创建项目
（1）maven-archetype-quickstart：普通的 java 项目
（2）maven-archetype-webapp：web 工程

## 四、依赖管理
### 4.1 依赖的范围
依赖范围使用 scope 表示。

scope 的值有：compile、test、provided。

scope 表示：依赖使用的范围，也就是在 maven 构建项目的哪些阶段中起作用。
![图示](https://img-blog.csdnimg.cn/20210704163124302.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
## 五、Maven 常用设置
### 5.1 全局变量
（1）maven 的属性设置
`<properties>` 设置 maven 的常用属性

（2）maven 的全局变量
自定义的属性：
* 语法规则：1、在`<properties>`中，通过自定义标签声明变量（标签名就是变量名）。
2、在 pom.xml 文件中的其他位置，使用 ${标签名} 使用变量的值。

* 作用：自定义全局变量一般是定义依赖的版本号。当你的项目中要使用多个相同的版本号的时候，先使用全局变量定义，再使用 ${标签名}。
### 5.2 指定资源位置
![图示](https://img-blog.csdnimg.cn/20210704170959625.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
作用：1、默认没有使用 resources 的时候，maven 执行编译代码时，会把 src/main/resources 目录中的文件以及 src/main/java 目录下的 java 文件生成的字节码文件拷贝到 target/classes 目录中，对于 src/main/java 目录下的非 java 文件不处理，不拷贝到 target/classes 目录中。

2、我们的程序有需要把一些非 java 文件放在 src/main/java 目录中，并且当我在执行 java 程序的时候，需要用到 src/main/java 目录下的非 java 文件。那么我们就需要**告诉 maven 在 mvn compile src/main/java 目录下的程序时，需要把这些非 java 文件一同拷贝到 target/classes 目录中，此时就需要在 pom.xml 文件下的** `<build>` **中加入** `<resources>`。

```xml
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