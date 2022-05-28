

#### 什么是 EL 表达式，EL表达式的作用
1. EL 表达式的全称是：Expression Language，表达式脚本语言。
2. EL 表达式的作用：EL表达式的主要作用是代替 JSP 页面中的表达式脚本在 JSP 页面中进行数据的输出。因为 EL 表达式在输出数据的时候，要比 JSP 的表达式脚本要简洁得多。
3. EL表达式的格式是：${ 表达式 }
4. EL 表达式在输出 null 值的时候，输出的是空串；JSP 的表达式脚本输出的时候，输出的是 null 字符串。
#### EL 表达式搜索域数据的顺序
EL 表达式主要是在 JSP 页面中输出数据。主要是输出域对象中的数据。

当四个域中都有相同的 key 的数据的时候，EL 表达式会按照四个域的从小到大的顺序去进行搜索，找到就输出。
#### EL 表达式的输出
注：EL表达式在输出的时候实际上不是直接找的类里面的属性，而是找属性对应的 get() 方法。
![图示](https://img-blog.csdnimg.cn/20210528152306732.png)
![图示](https://img-blog.csdnimg.cn/20210528151745692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
#### EL 表达式——运算
语法：${ 运算表达式 }，EL 表达式支持如下运算符：
![图示](https://img-blog.csdnimg.cn/20210528154220785.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
#### empty 运算
empty 运算可以判断一个数据是否为空，如果为空，则输出 true，否则输出 false。

以下几种情况为空：
1、值为 null 的时候，为空
2、值为空串的时候，为空
3、值为 Object 类型的数组，长度为零的时候
4、list 集合，元素个数为零
5、map 集合，元素个数为零
#### "."点运算和 [] 中括号运算符
. 点运算，可以输出 Bean 对象中某个属性的值。

[] 中括号运算，可以输出有序集合中某个元素的值。并且[] 中括号运算，还可以输出 map 集合中 key 里面含有特殊字符的 key 的值
```java
示例：
<%
        HashMap<String, String> map = new HashMap<>();
        map.put("a.a.a", "aaaValue");
        map.put("b+b+b", "bbbValue");
        map.put("c-c-c", "cccValue");
        map.put("eee", "eeeValue");
        request.setAttribute("m", map);
    %>
    ${m['a.a.a']}
    ${m["b+b+b"]}
    ${m['c-c-c']}
    ${m.eee}
```
#### EL 表达式的 11 个隐含对象
EL 表达式中的 11 个隐含对象，是 EL 表达式中自己定义的，可以直接使用。
![图示](https://img-blog.csdnimg.cn/20210529083406101.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
#### pageContext 对象的使用
1、获取协议
2、服务器 ip
3、服务器端口
4、获取工程路径
5、获取请求方法
6、获取客户端 ip 地址
7、获取会话的 id 编号
### JSTL 标签库
JSTL 标签库 全称是指 JSP Standard Tag Library（JSP 标准标签库），是一个不断完善的开放源代码的 JSP 标签库。

EL 表达式主要是为了替换 JSP 中的表达式脚本，而标签库则是为了替换代码脚本，这样使得整个 JSP 页面变得更加简洁。
![图示](https://img-blog.csdnimg.cn/20210529092706389.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
#### JSTL 标签库的使用步骤
1、先导入 JSTL 标准库的 jar 包。
![图示](https://img-blog.csdnimg.cn/20210529093631178.png)
2、使用 taglib 指令引入标签库。
#### Core 核心库的使用
**1、< c:set />**

作用：set标签可以往域中保存数据
```java
    <%--
        往域中保存数据。
        scope 属性设置保存到那个域，默认是保存到 PageContext 域。
        var 属性设置 key 是多少
        value 属性设置 值 是多少
    --%>
    <c:set scope="request" var="key" value="request" />
    ${requestScope.key}
```
**2、< c:if />**

作用：if 标签用来做 if 判断
```java
    <%--
        if 标签用来做 if 判断
        test 属性表示判断的条件（使用 EL 表达式输出）
    --%>
    <c:if test="${12==12}">
        <h1>12等于12</h1>
    </c:if>
```
**3、< c:choose >< c:when >< c:otherwise > **

作用：多路判断。类似：switch ... case ... default
```java
<%
        request.setAttribute("height", 162);
    %>
    <%--
        使用 <c:choose><c:when><c:otherwise> 标签的注意事项：
            1、标签里面不能使用 html 注释，应该使用 JSP 注释
            2、when 标签的父标签一定要是 choose 标签
    --%>
    <c:choose>
        <c:when test="${requestScope.height > 180}">
            <h1>大于180</h1>
        </c:when>
        <c:when test="${requestScope.height > 170}">
            <h1>大于170</h1>
        </c:when>
        <c:otherwise>
            <h1>其余情况</h1>
        </c:otherwise>
    </c:choose>
```
**4、< c:forEach />**

作用：用来遍历输出。
```java
    <%--
        begin 属性设置开始的索引
        end 属性设置结束的索引
        step 属性设置遍历的步长
        var 属性表示循环的变量（也是当前正在遍历到的数据）
        varStatus 属性表示当前遍历到的属性的状态
    --%>
    <c:forEach begin="1" end="10" step="2" varStatus="status" var="i">
        ${i}
        ${status.last}
    </c:forEach>

    <%
        request.setAttribute("arr", new String[]{"asd", "fgh", "fde"});

        HashMap<String, String> map = new HashMap<>();
        map.put("key1", "value1");
        map.put("key2", "value2");
        request.setAttribute("map", map);

        ArrayList<String> list = new ArrayList<>();
        list.add("list1");
        list.add("list2");
        request.setAttribute("list", list);
    %>

    <%-- 遍历 Object 数组
        for(Object item : arr)
        items 表示要遍历的集合
        var 表示当前遍历到的数据
    --%>
    <c:forEach items="${requestScope.arr}" var="item">
        ${item}
    </c:forEach>

    <%-- 遍历 Map 集合
        for(Map.Entry<String, String> entry : map.entrySet())
    --%>
    <c:forEach items="${requestScope.map}" var="entry">
        ${entry}
    </c:forEach>

    <%-- 遍历 list 集合 --%>
    <c:forEach items="${requestScope.list}" var="l">
        ${l}
    </c:forEach>
```
![图示](https://img-blog.csdnimg.cn/20210529123050292.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)