

* 系统结构
1. B / S 架构：
    Browser /  Server（浏览器 / 服务器的交互形式）
    Briwser支持哪些语言：HTML、CSS、JavaScript
    写 HTML、CSS、JavaScript代码的这些人叫做：web前端工程师。
    Server端的语言有很多：C、C++、Java、Python等
    B / S架构的优缺点：
    优点：升级方便。直升级服务器端的代码即可，维护成本低。
    缺点：速度慢，体验不好，界面不酷炫。
    企业内部都是采用B / S架构的系统，因为企业内部办公需要的一些系统不需要酷炫，不需要特别好的用户体验。并且企业内部更注重维护的成本。
2. C / S 架构：
    Client / Server （客户端 / 服务器端的交互形式）
    优点：速度快、体验好、界面酷炫。
    缺点：升级麻烦，维护成本较高。

* 什么是HTML：
    HTML：Hyper Text Markup Language(超文本标记语言)
    由大量的标签组成，每一个标签都有开始标签和结束标签，结束标签有一个 /
```html
<标签>
    <标签>
        <标签 属性名="属性值" 属性名="属性值">
        </标签>
    </标签>
</标签>
```
* 怎么开发HTML：
    HTML开发的时候使用普通的文本编辑器就行，创建的文件扩展名是 .html 或者 .htm。
    HTML也有专业的开发工具，比如：DreamMeaver、HBuilder......
* 如何运行HTML：
    直接采用浏览器打开HTML文件就是运行。
* HTML是谁制定的：
    W3C：世界万维网联盟。W3C制定了HTML的规范，每个浏览器生产厂家都会遵守规范。HTML程序员也会按照这个规范去写代码。
    HTML规范目前最高的版本是HTML5.0，简称H5。

```html
<!--
    1、这是HTML的注释
	2、第一行加上以下代码就表示HTML5语法，去掉就表示HTML4.0
	3、HTML不区分大小写，语法松散，不严格
-->
<!doctype html>
<!--根-->
<html>
	
	<!--头-->
	<head>
	    <!--网页标题-->
	    <title>网页的标题</title>
	</head>
	
	<!--体-->
	<body>
	    网页的主体内容
	</body>
	
</html>
```

```html
<!doctype html>

<html>
    <head>
	    <title>HTML的基本标签</title>
	</head>
	
	<body>
	
	    <!--段落标记-->
		<p>你在干什么</p><p>你在干什么</p><p>你在干什么</p>
	    
		<!--标题字：是HTML预留的格式，和word中的标题子相同-->
		<h1>标题字</h1>
		<h2>标题字</h2>
		<h3>标题字</h3>
		<h4>标题字</h4>
		<h5>标题字</h5>
		<h6>标题字</h6>
		
		<!--换行标记,br标签是一个独目标记-->
		hello <br>world!
		
		<!--水平线,独目标记-->
		<hr>
		
		<!--color和width都是hr标签的属性-->
		<hr color="red" width="50%">
		<!--语法松散-->
		<hr color='green' width=30%>
		
		<!--预留格式，pre里面是什么格式，到网页里面就是什么格式-->
		<pre>
		for(int i = 0; i < 10; i++){
		    System.out.println("i = " + i);
		}
		</pre>
		
		<del>删除字</del>
		<ins>插入字</ins>
		<b>粗体字</b>
		<i>斜体字</i>
		
		<!--右上角加字-->
		10<sup>2</sup>
		
		<!--右下角加字-->
		a<sub>n</sub>
		
		<!--字体标签-->
		<font color="red" size="50">字体标签</font>
	</body>

</html>
```

```html
<!doctype html>

<html>

    <head>
	    <title>HTML的实体符号</title>
	</head>
	
	<body>
	<!--以下表示b<a>c。实体符号：以&开始，以;结束。&lt;是小于号，&gt;是大于号-->
	b&lt;a&lt;c
	
	<!--&nbsp; 空格-->
	a&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;bc
	</body>

</html>
```

```html
<!doctype html>

<html>
    <head>
	    <title>HTML的表格</title>
	</head>
    
	<body>
	    <!-- <h1 align="center">员工信息列表</h1> -->
		<center><h1>员工信息列表</h1></center>
	    
	    <!--以下3行两列。table标签 代表表格，tr标签 代表表格当中的行，td标签 代表行上的格子-->
		<!--border="1px" 设置表格的边框为1像素宽.width="30%" 占当前窗口宽度的30%，同比例缩放-->
		<!-- <table border="1px" width="300px" height="120px">也可以 -->
		<table border="1px" width="30%" height="120px" align="center">
		    
			<!--align 对齐方式-->
		    <tr align="center">
			    <td>a</td>
				<td>b</td>
				<td>c</td>
			</tr>
			
			<tr>
			    <td>d</td>
				<td>e</td>
				<td>f</td>
			</tr>
			
			<tr>
			    <td>g</td>
				<td>h</td>
				<td align="center">i</td>
			</tr>
		
		</table>
			
	</body>

</html>
```

```html
<!doctype html>

<html>
    <head>
	    <title>HTML的表格单元格合并，以及th标签</title>
	</head>
    
	<body>
	    
		<table border="1px" width="30%" height="120px" align="center">
		    <tr>
			    <!--th也是单元格，比td多的是居中、加粗-->
			    <th>名字</th>
				<th>班级</th>
				<th>学号</th>
			</tr>
			
			<!--
			    1、row合并的时候，删除下面的单元格。
				2、col删除的时候，对删除哪个没有要求。
			-->
		    <tr>
			    <td colspan="2">a</td>
				<!--
				    <td>b</td>
				-->
				<td>c</td>
			</tr>
			
			<tr>
			    <td>d</td>
				<td>e</td>
				<td rowspan="2">f</td>
			</tr>
			
			<tr>
			    <td>g</td>
				<td>h</td>
				<!--
				    <td>i</td>
				-->
			</tr>
		
		</table>
			
	</body>

</html>
```

```html
<!doctype html>

<html>
    <head>
	    <title>thead、tbody、tfoot在table中不是必须的，只是这样做便于后期JS代码的编写</title>
	</head>
    
	<body>
	    
		<table border="1px" width="30%" height="120px" align="center">
		    <thead>
		        <tr>
			        <th>名字</th>
				    <th>班级</th>
				    <th>学号</th>
		     	</tr>
		    </thead>
			
			<tbody>
		        <tr>
			        <td colspan="2">a</td>
				    <!--
				        <td>b</td>
				    -->
				    <td>c</td>
			    </tr>
			</tbody>
			
			<tfoot>
      			<tr>
			        <td>d</td>
				    <td>e</td>
				    <td rowspan="2">f</td>
			    </tr>
			
			    <tr>
			        <td>g</td>
				    <td>h</td>
				    <!--
				        <td>i</td>
				    -->
			    </tr>
			</tfoot>
		
		</table>
			
	</body>

</html>
```

```html
<!doctype html>

<html>
    <head>
	    <!-- 这行代码的作用是告诉浏览器采用哪一种字符集打开当前页面，而不是设置当前页面的字符编码方式 -->
	    <meta charset="GBK">
	    <title>背景色和背景图片</title>
	</head>
	
	<!--bgcolor 设置背景颜色
	    background 设置背景图片-->
	<body bgcolor="red" background="E:\桌面美化/静态美图.jpeg">
	
	</body>
  
</html>
```

```html
<!doctype html>

<html>
    <head>
	    <!-- 这行代码的作用是告诉浏览器采用哪一种字符集打开当前页面，而不是设置当前页面的字符编码方式 -->
	    <meta charset="GBK">
	    <title>图片</title>
	</head>
	
	<body>
	    
		<!-- 1、设置图片宽度和高度的时候，只设置宽度，高度会进行等比例缩放
		     2、img标签就是图片标签
			 3、src属性是图片的路径
			 4、width设置宽度，height设置高度
			 5、title设置鼠标悬停在图片上时的所显示的信息
			 6、alt设置图片加载失败时显示的提示信息
		-->
	    <img src="E:\桌面美化/图片.png" width="100px" title="我是图片" alt="图片找不到" />
		<img src="E:\桌面美化/图片.png" width="100px" title="我是图片" alt="图片找不到"></img>
	
	</body>
  
</html>
```

```html
<!doctype html>

<html>
    <head>
	    <meta charset="GBK">
	    <title>超链接或者热链接</title>
	</head>
	
	<!-- 
		超链接作用：
		    通过超链接可以从浏览器向服务器发送请求。
			浏览器向服务器发送数据：请求（request）
			服务器向浏览器发送数据：响应（response）
		B/S结构的系统：每一个请求都会对应一个响应。
			
		用户点击超链接和用户在浏览器地址上直接输入URL有什么区别：
			本质上没有区别，都是向服务器发送请求。但从操作上来讲，超链接使用更方便
	-->
	<body>
	    
		<!--href：hot reference 热引用
		    href属性后面一定是一个资源的地址
		-->
		<a href="http://www.baidu.com">百度</a>
		<a href="http://www.tmall.com">天猫</a>
		<a href="E:\桌面美化/静态美图.jpeg">美图</a>
		
		<!-- 图片超链接 -->
		<a href="http://www.baidu.com"><img src="E:\桌面美化/图片.png" width="120px">
		</a>
		
		<!--超链接有一个target属性：
			    可取值：
				    _blank：新窗口
					_self：当前窗口（默认是这种方式）
					_top：顶级窗口
					_parent：父窗口
		-->
		<a href="http://www.baidu.com" target="_blank"><img src="E:\桌面美化/图片.png" width="120px">
		</a>
	
	</body>
  
</html>
```

```html
<!doctype html>

<html>
    <head>
	    <meta charset="GBK">
	    <title>有序列表 无序列表</title>
	</head>
	
	<body>
	
	    <!--有序列表-->
		<ol type="1">
		    <li>水果
			    <ol>
				    <li>香蕉</li>
				    <li>西瓜</li>
				    <li>苹果</li>
				</ol>
			
			</li>
		    <li>蔬菜</li>
		    <li>甜点</li>
		</ol>
		
		<!--无序列表-->
		<ul type="circle">
			<li>中国
			    <ul>
    		        <li>北京</li>
					<li>天津</li>
					<li>上海</li>
			    </ul>	
			</li>
			<li>美国</li>
			<li>英国</li>
		</ul>
	
	</body>

</html>
```
**表单示例：**
![表单示例](https://img-blog.csdnimg.cn/20210406223939512.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```html
<!doctype html>

<html>
    <head>
	    <meta charset="GBK">
	    <title>表单form</title>
	</head>
	
    <body>
	    <!--
		    1、表单有什么作用？
			    主要负责数据采集。表单展现之后，用户填写表单，点击提交按钮提交数据给服务器
			2、怎么画一个表单？
			    使用form标签画表单
			3、一个网页当中可以有多个表单form
			4、表单最终是要提交数据给服务器的，form标签有一个action属性，这个属性用来指定服务器地址：action属性用来指定数据提交给哪个服务器。
			action属性和超链接中的href属性一样。都可以向服务器发送请求（request）
			5、http://192.168.111.3:8080/oa/save 这是请求路径
			表单提交数据最终提交给：192.168.111.3机器上的8080端口对应的软件。	
		-->
	    <form action="http://192.168.111.3:8080/oa/save">
		    <!--画一个提交按钮，这个按钮可以提交表单-->
			<!--画按钮可以使用input输入域，type="submit"的时候表示该按钮是一个提交按钮，具有提交表单的能力
			    对于按钮来说，value属性用来设置按钮上显示的文本。下面没有name只有value，不会提交-->
			<input type="submit" value="登陆"/>
			
			<!--这是一个普通按钮，不具备提交表单的能力-->
			<input type="button" value="设置按钮上显示的文本" />
			
			<!--文本框。type可以跟好多东西，不能随便写-->
			<input type="text"/>
		</form>
		
		
		<!--这个按钮和普通的超链接没什么区别（超链接和表单都可以向服务器发送请求，只不过表单发送请求的同时可以携带数据）-->
		<form action="http://www.baidu.com">
		    <input type="submit" value="百度" />
		</form>
		
		
		<!--表单是以什么格式提交数据给服务器的？
		    http://local:8080/jd/login?username=abc&userpwd=111
			格式：action?name=value&name=value&name=value&name=value...
			HTTP协议规定的，必须以这种格式提交给服务器。
			
			重点强调：表单项写了name属性的，一律会提交给服务器，不想提交这一项，就不要写name属性。
			
			文本框和密码框的value不需要程序员指定，用户输入什么，value就是什么。
			
			当name没有写的时候，该项不会提交给服务器。
			但当value没有写的时候，value的默认值是空字符串""，会将空字符串提交给服务器。
		-->
		<form action="http://local:8080/jd/login">
		    用户名<input type="text" name="username"/>
			<br>
			密码<input type="password" name="userpwd"/>
		    <input type="submit" value="登陆"/>
		</form>
		
	</body>

</html>
```

```html
<!doctype html>

<html>
    <head>
	    <meta charset="GBK">
	    <title>用户注册的表单</title>
	</head>


    <body>
	    <!--用户注册：用户名、密码、确认密码、性别、兴趣爱好、学历、简介
		
		    form表单的method属性：
			    get：采用get方式提交的时候，用户提交的信息会显示在浏览器的地址栏上（默认）
			    post：采用post方式提交的时候，用户提交的信息不会显示在浏览器地址栏上
				当用户提交的信息中含有敏感信息的时候，建议采用post方式提交
		-->
		
		<form action="http://local:8080/jd/register" method="post">
		    用户名
			<input type="text" name="username" />
		    
			<br>
			密码
			<input type="password" name="userpwd" />
			
			<br>
			确认密码
			<input type="password" />
			
			<br>
			性别
			<input type="radio" name="gender" value="1" checked />男 <!--checked表示默认选中-->
			<input type="radio" name="gender" value="0" />女 <!--单选按钮的name需要相等，value必须手动指定，因为后面的男/女不会提交给服务器，提交的是1/0-->
			
			<br>
			兴趣爱好
			<input type="checkbox" name="interest" value="smoke" checked />抽烟
			<input type="checkbox" name="interest" value="drink" />喝酒
			<input type="checkbox" name="interest" value="fireHair" />烫头
			
			<br>
			学历
			<select name="grade">
			    <option value="gz">高中</option>
			    <option value="dz">大专</option>
			    <option value="bk" selected>本科</option> <!--默认选中-->
			    <option value="ss">硕士</option>
			</select>
			
			<br>
			简历<!--文本域，文本域没有value属性，用户填写的内容就是value-->
			<textarea rows="10" cols="30" name="introduce"></textarea>
			
			<br>
			<input type="submit" value="注册" />
			<input type="reset" value="清空" />
		
		</form>
	
	</body>

</html>
```
```html
<!doctype html>

<html>
    <head>
	    <meta charset="GBK">
	    <title>下拉列表支持多选</title>
	</head>

    <body>
	    <!--multiple="multiple"支持多选 size设置显示条目数量-->
        <select multiple="multiple" size="5">
		    <option>河北</option>
		    <option>河南</option>
		    <option>山东</option>
		    <option>陕西</option>
		    <option>山西</option>
		</select>	
		
	</body>

</html>
```

```html
<!doctype html>

<html>
    <head>
	    <meta charset="GBK">
	    <title>file控件</title>
	</head>

    <body>
	
	    <!--file控件，文件上传专用-->
        <input type="file" />
		
	</body>

</html>
```

```html
<!doctype html>

<html>

    <head>
	    <meta charset="GBK">
	    <title>隐藏域hidden控件</title>
	</head>

    <body>
	
	    <form action="http://local:8080/jd/register">
		    <!--隐藏域。网页上看不到，但是表单在提交的时候会自动提交给服务器-->
		    <input type="hidden" name="userid" value="111" />
		    <input type="text" name="usercode" />
			<input type="submit" />
		</form>
	
	</body>

</html>
```
```html
<!doctype html>

<html>
    <head>
	    <meta charset="GBK">
	    <title>readonly disabled</title>
	</head>
	
    <body>
	    <!--readonly和disabled有共同点：都是只读不能修改
		    但是readonly可以提交给服务器，disabled数据不会提交（即使有name属性也不会提交）
		-->
	    <form action="http://localhost:8080/taobao/save">
		    用户代码<input type="text" name="usercode" value="111"readonly />
			<br>
			用户名<input type="text" name="username" value="zhangsan" disabled />
			<br>
			<input type="submit" />
		</form>
	
	</body>

</html>
```
```html
<!doctype html>

<html>
    <head>
	    <meta charset="GBK">
	    <title>input控件的maxlength属性</title>
	</head>
	
    <body>
	    <!--readonly和disabled有共同点：都是只读不能修改
		    但是readonly可以提交给服务器，disabled数据不会提交（即使有name属性也不会提交）
		-->
	    <form action="http://localhost:8080/taobao/save">
		    <!--maxlength：设置文本框中可输入的最大字符数量-->
		    用户代码<input type="text" name="usercode" maxlength="5" />
			<br>
			<input type="submit" />
		</form>
	
	</body>

</html>
```
```html
<!doctype html>

<html>

    <head>
        <meta charset="GBK">	
	    <title>HTML中元素的id属性</title>
	</head>
	
	<body id="mybody">
	    
		<form id="myform" action="http://localhost:8080/taobao/save">
		    <!--
			    1、在HTML文档中，任何元素（结点）都有id属性,id属性是该结点的唯一标识，所以在同一个HTML文档中id值不能重复
				2、注意：表单提交数据的时候，只和name有关，和id无关
				3、id有什么用？
				    JavaScript可以对HTML文档当中的任意结点进行增删改，那么增删改之前需要先拿到这个节点，通常我们通过id来拿节点对象，
					id的存在让我们获取元素（结点）更方便。
				4、HTML文档是一棵树，树上有很多节点，每一个节点都有唯一的id。
				   比如：html是根节点，下面有head结点和body结点，body结点下面还有若干节点......
				   JavaScript主要就是对这棵DOM树的结点进行增删改的。DOM(Document)树
			-->
		    <input type="text" id="username" name="username" />    
		    <input type="password" id="userpwd" name="userpwd" />
		
		</form>
	
	</body>

</html>
```
```html
<!doctype html>

<html>

    <head>
	    <meta charset="GBK">
	    <title>HTML中的div和span</title>
	</head>

    <body>
	    <!--
		    1、div和span是什么？
			    div和span都可以称为“图层”
			2、div和span的作用？
			    图层的作用是为了保证页面可以灵活的布局
				图层就是一个一个的盒子，div嵌套div就是盒子套盒子
				div和span是可以定位的，只要定下div的左上角的x轴和y轴的坐标即可
			3、其实最早的网页是采用table布局的，但是table不灵活，太死板。
			    现代的网页开发中div布局使用最多，几乎很少使用table进行布局了。
			4、div和span的区别？
			    div独自占用一行（默认情况下）
				span不会独自占用一行。
		-->
		<div id="div1">我是一个div</div>
		<div id="div2">我是一个div</div>
		
		<span id="span1">我是一个span</span>
		<span id="span2">我是一个span</span>
		
		<div id="div3">
		    <div>
			    <div>我是一个div</div>
			</div>
		</div>
	
	</body>

</html>
```