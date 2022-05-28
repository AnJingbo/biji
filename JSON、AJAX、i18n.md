

## 一、JSON
### 1.1 什么是 JSON
![图示](https://img-blog.csdnimg.cn/2021062709440318.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 1.2 JSON 在 JavaScript 中的使用
#### 1.2.1 JSON 的定义
JSON 是由键值对组成，并且由花括号包围。每一个键由引号引起来。键和值之间使用冒号进行分隔，多组键值对之间使用逗号进行分隔。

```javascript
// json 的定义
var jsonObj = {
    "key1" : 12,
    "key2" : "abc",
    "key3" : true,
    "key4" : [11, "arr", false],
    "key5" : {
        "key5_1" : 51,
        "key5_2" : "key5_2_value",
    },
    "key6" : [{
        "key6_1_1" : 611,
        "key6_1_2" : "key6_1_value",
    },{
        "key6_2_1" : 621,
        "key6_2_2" : "key6_2_value",
    }]
};
```
#### 1.2.2 JSON 的访问
json 本身就是一个对象。json 中的 key 我们可以理解为是对象中的一个属性。

json 中的 key 的访问就跟访问对象的属性一样：json 对象.key

```javascript
// json 的访问
alert(jsonObj.key1);
alert(jsonObj.key2);
alert(jsonObj.key3);

alert(jsonObj.key4);
for(var i = 0; i < jsonObj.key4.length; i++){
    alert(jsonObj.key4[i]);
}

alert(jsonObj.key5.key5_1);
alert(jsonObj.key5.key5_2);

var jsonItem = jsonObj.key6[0];
alert(jsonItem.key6_1_1);
alert(jsonItem.key6_1_2);
```
#### 1.2.3 JSON 的两个常用方法
json 的存在有两种形式：
1. 对象的形式存在，我们叫它 json 对象。一般我们要操作 json 中的数据的时候，需要 json 对象的格式。
2. 字符串的形式存在，我们叫它 json 字符串。一般我们要操作 json 中的数据的时候，需要 json 对象的格式。一般我们要在客户端和服务器之间进行数据交换的时候，使用 json 字符串。

JSON.stringify()：把 json 对象转换成为 json 字符串。

JSON.parse()：把 json 字符串转换成为 json 对象。
```javascript
// 把 json 对象转换成为 json 字符串
var jsonObjString = JSON.stringify(jsonObj);
alert(jsonObjString);
// 把 json 字符串，转换成为 json 对象
var jsonObj2 = JSON.parse(jsonObjString);
alert(jsonObj2);
```
### 1.3 JSON 在 Java 中的使用
```java
import com.atjingbo.pojo.Person;
import com.google.gson.Gson;
import com.google.gson.reflect.TypeToken;
import org.junit.Test;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class JsonTest {
    // JavaBean 和 json 的互转
    @Test
    public void test1(){
        Person person = new Person(1, "小明");

        // 创建 Gson 对象实例
        Gson gson = new Gson();

        // toJson() 方法可以把 Java 对象转换成为 Json 字符串
        String personJsonString = gson.toJson(person);
        System.out.println(personJsonString);

        // fromJson() 方法可以把 Json 字符串转换成为 Java 对象
        Person person1 = gson.fromJson(personJsonString, Person.class);
        System.out.println(person1);
    }
    
    // List 和 json 的互转
    @Test
    public void test2(){
        List<Person> personList = new ArrayList<>();
        personList.add(new Person(1, "小明"));
        personList.add(new Person(2, "丹尼"));

        // 创建 Gson 对象实例
        Gson gson = new Gson();

        // toJson() 方法可以把 List 转换成为 Json 字符串
        String personListJsonString = gson.toJson(personList);
        System.out.println(personListJsonString);

        // fromJson() 方法可以把 Json 字符串转换成为 List
        // List<Person> list = gson.fromJson(personListJsonString, personList.getClass());
        List<Person> list = gson.fromJson(personListJsonString, new TypeToken<List<Person>>(){}.getType());
        System.out.println(list);
        System.out.println(list.get(0));
    }
    
    // Map 和 json 的互转
    @Test
    public void test3(){
        Map<Integer, Person> personMap = new HashMap<>();
        personMap.put(1, new Person(1, "小明"));
        personMap.put(2, new Person(2, "丹尼"));

        // 创建 Gson 对象实例
        Gson gson = new Gson();

        // toJson() 方法可以把 Map 转换成为 Json 字符串
        String personMapJsonString = gson.toJson(personMap);
        System.out.println(personMapJsonString);

        // fromJson() 方法可以把 Json 字符串转换成为 Map
        Map<Integer, Person> map = gson.fromJson(personMapJsonString, new TypeToken<Map<Integer, Person>>(){}.getType());
        System.out.println(map);
        System.out.println(map.get(1));
    }
}
```
## 二、AJAX
### 2.1 什么是 AJAX 请求
![图示](https://img-blog.csdnimg.cn/20210627145313522.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 2.2 原生 AJAX 请求的示例
```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        function ajaxRequest(){
            // 1、创建 XMLHttpRequest 对象
            var xmlHttpRequest = new XMLHttpRequest();
            // 2、调用 open 方法设置请求参数（true 是异步请求，false 是同步请求）
            xmlHttpRequest.open("GET", "http://localhost:8080/json_ajax_i18n/ajaxServlet?action=javaScriptAjax", true);
            // 4、在 send 方法之前绑定 onreadystatechange 事件，处理请求完成后的操作
            xmlHttpRequest.onreadystatechange = function(){
                if(xmlHttpRequest.readyState == 4 && xmlHttpRequest.status == 200){
                    // 把响应的数据显示在页面上
                    var jsonObj = JSON.parse(xmlHttpRequest.responseText);
                    document.getElementById("div01").innerHTML = "编号：" + jsonObj.id + "，姓名：" + jsonObj.name;
                }
            }
            // 3、调用 send 方法发送请求
            xmlHttpRequest.send();
        }
    </script>
</head>
<body>
    <button onclick="ajaxRequest()">ajax request</button>
    <div id="div01"></div>
</body>
</html>
```
```java
public class AjaxServlet extends BaseServlet {
    protected void javaScriptAjax(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("Ajax 请求过来了");

        // 给客户端响应数据
        Person person = new Person(1, "李明");
        // 转成 JSON 格式的字符串
        Gson gson = new Gson();
        String personJsonString = gson.toJson(person);
        response.getWriter().write(personJsonString);
    }
}
```
### 2.3 AJAX 请求的特点说明
Ajax 请求的局部更新，浏览器地址栏不会发生变化。

局部更新不会舍弃原来页面的内容

### 2.4 jQuery 中的 Ajax 请求
$.ajax 方法

url ：表示请求的地址

type ：表示请求的类型GET/POST请求

data ：表示发送给服务器的数据。有两种格式：1.name=value&name=value 2. {key:value}

success ：请求成功，响应的回调函数

dataType ：响应的数据类型。比如：text 表示纯文本；xml 表示 xml 文件；json 表示 json 对象。
```javascript
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
        <script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
        <script type="text/javascript">
            $(function (){
                // ajax 请求
                $("#ajaxBtn").click(function (){
                    $.ajax({
                        url:"http://localhost:8080/json_ajax_i18n/ajaxServlet",
                        data:"action=jQueryAjax",
                        // data:{action:"jQueryAjax"}
                        type:"GET",
                        success:function (data){
                            $("#msg").html("ajax 编号：" + data.id + "，姓名：" + data.name);
                        },
                        dataType:"json"
                        // dataType:"text"
                    });
                });           
            });
        </script>
    </head>

    <body>
        <button id="ajaxBtn">ajax方法</button>
        <div id="msg"></div>
    </body>
</html>
```
### 2.5 get 方法和 post 方法
url ：请求的 url 地址

data ：发送的数据

callback ：成功的回调函数

type ：返回的数据类型

```javascript
// get 请求
$("#getBtn").click(function (){
    $.get("http://localhost:8080/json_ajax_i18n/ajaxServlet", "action=jQueryGet", function (data){
        $("#msg").html("get 编号：" + data.id + "，姓名：" + data.name);
    }, "json");
});

// post 请求
$("#postBtn").click(function (){
    $.get("http://localhost:8080/json_ajax_i18n/ajaxServlet", "action=jQueryPost", function (data){
        $("#msg").html("post 编号：" + data.id + "，姓名：" + data.name);
    }, "json");
});
```
### 2.6 getJSON方法
url ：请求的 url 地址

data ：发送给服务器的数据

callback ：成功的回调函数
```javascript
// getJSON 请求
$("#getJSONBtn").click(function (){
    $.getJSON("http://localhost:8080/json_ajax_i18n/ajaxServlet", "action=jQueryGetJSON", function (data){
        $("#msg").html("getJSON 编号：" + data.id + "，姓名：" + data.name);
    });
});
```
### 2.7 表单序列化 serialize 方法
serialize() 可以把表单中所有表单项的内容都获取到，并以 name=value&name=value 的形式进行拼接。

```javascript
$("#form01").serialize();
```
## 三、i18n
### 3.1 什么是 i18n 国际化
![图示](https://img-blog.csdnimg.cn/20210628101643831.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 3.2 i18n 国际化三要素
![图示](https://img-blog.csdnimg.cn/20210628101745235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)