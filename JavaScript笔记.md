

1、什么是JavaScript，有什么用？
JavaScript是运行在浏览器上的脚本语言，由网景公司的布兰登艾奇开发。简称JS。最初叫LiveScript。
LiveScript的出现让浏览器更加生动了，不再是单纯的静态页面了，整体更有交互性。

**注：Java程序运行在JVM中，JavaScript运行在浏览器的内存中。**

JavaScript程序不需要我们手动编译，编写完源代码之后，浏览器直接打开解释执行。**JavaScript的“目标程序”以普通文本的形式保存，这种语言称为"脚本语言"。**

Java的目标程序以.class的形式存在，不能使用文本编辑器打开，不是脚本语言。

2、JSP和JS有啥区别？
JSP：JavaServer Pages（隶属于Java语言，运行在JVM中）
JS：JavaScript（运行在浏览器上）

```javascript
<!doctype html>

<html>
    <head>
	    <title>HTML中嵌入JS代码的第一种方式</title>
	</head>
	
	<body>
	    
		<!--
		    1、要实现的功能：
			    用户点击按钮之后，弹出消息框
				
			2、JS是一门事务驱动型的编程语言，依靠事件去驱动，然后执行对应的程序。
			在JS中会有很多事件，其中有一个事件叫做：鼠标单击，单词：click。并且任何
			事件都会对应一个事件句柄叫做：onclick。【注意：事件和事件句柄的区别是：
			事件句柄是在事件单词前添加一个on。】，而事件句柄是以HTML中标签的属性存在的。
			
			3、onclick="JS代码"，执行原理是什么？
			页面打开的时候，JS代码并不会执行，只是把这段JS代码注册到按钮的click事件上了。
		    等这个按钮发生click事件之后，注册在onclick后面的JS代码会被浏览器自动调用。
			
			4、怎么使用JS代码弹出消息框？
			    在JS中有一个内置的对象叫做window，全部小写，可以直接拿来使用，window代表的是浏览器对象。
			window对象有一个函数叫做alert，用法是：window.alert("消息");这样就可以弹窗了。
			
			5、JS中的字符串可以使用双引号，也可以使用单信号。
			
			6、JS中的一条语句结束之后可以使用分号";"，也可以不用。
		-->
	    <input type="button" value="hello" onclick="window.alert('hello js')"/>
		
		<input type="button" value="hello" onclick='window.alert("hello js")'/>
		
		<input type="button" value="hello" onclick="window.alert('hello zhangsan')
		                                            window.alert('hello lisi')
													window.alert('hello wangwu')">
		
		<!--window. 可以省略-->
		<input type="button" value="hello" onclick="alert('hello zhangsan');
		                                            alert('hello lisi');
													alert('hello wangwu');">
	</body>

</html>
```
```javascript
<!doctype html>

<html>
    <head>
	    <title>HTML中嵌入JS代码的第二种方式</title>
		
		<!--
		    javascript的脚本块在一个页面当中可以出现多次。
		    javascript的脚本块的出现位置也没有要求，随意
		-->
		<script type="text/javascript">
		    window.alert("first....")
		</script>
		
	</head>
	
	<body>
	    <input type="button" value="我是一个按钮1" />
		
		<!--第二种方式：脚本块的方式-->
		
		<script type="text/javascript">
		    /*注：暴露在脚本块中的程序，在页面打开的时候执行，
			并且遵守自上而下的顺序一次逐行执行。（这个代码的执行不需要事件）*/
		    window.alert("hello world")
		    // 这是JS代码第一种注释
			/*这是JS代码第二种注释*/
			window.alert("hello javascript")
		</script>
		
		<input type="button" value="我是一个按钮" />
	    
	</body>

</html>

<script type="text/javascript">
    window.alert("last....")
</script>
```

```javascript
<!doctype html>

<html>
    <head>
	    <title>HTML中嵌入JS代码的第三种方式：引入外部独立的js文件</title>	
	</head>
	
	<body>
	
	    <script type="text/javascript" src="D:\test.js"></script>
		
		<!--同一个js文件可以被引入多次-->
		<script type="text/javascript" src="D:\test.js"></script>
		
		<!--这种方式不行，结束的script标签必须有-->
		<script type="text/javascript" src="D:\test.js" />
	    
		<!--如果是引入js文件的方式的话，那么脚本块的代码是不会执行的，
		即下面的window.alert("hello js")是不会执行的-->
		<script type="text/javascript" src="D:\test.js">window.alert("hello js")</script>
	</body>

</html>
```

```javascript
<!doctype html>
<html>
    <head>
	    <title>关于JS当中的变量</title>
	</head>

    <body>
	    <script type="text/javascript">
		
		    var i = false;
			i = 3.14;
			alert("i = " + i);
			
			//一个变量没有手动赋值的时候，系统默认赋值undefined。undefined在JS中是一个具体的值
			var k;
			alert("k = " + k);// k = undefined
			
			var t = undefined;
			alert(t);
			
			//一个变量没有声明直接使用，会出现语法错误
			alert(j);// 语法错误： j is not defined
		
		</script>
	</body>
</html>
```

```javascript
<!doctype html>

<html>
    <head>
	    <title>JS函数</title>
	</head>

    <body>
	    <script type="text/javascript">
	    
		    //JS中的函数不需要指定返回值类型，返回什么类型都行
			
			// 定义函数的第一种方式：
		    function sum(a, b){//a 和 b都是局部变量，他们都是形参。a 和 b都是变量名，变量名随意
			    alert(a + b);
			}
			//函数必须调用才能执行
			sum(10, 20);
			
			//定义函数的第二种方式：
			sayHello = function(username){
			    alert("hello " + username);
			}
			sayHello("zhangsan");
		</script>
		
		<input type="button" value="hello" onclick="sayHello('lisi');" />
		<input type="button" value="计算10和20的和" onclick="sum(10, 20);" />
	
	</body>
</html>
```

```javascript
<!doctype html>

<html>

    <head>
	    <title>JS函数</title>
	</head>
	
	<body>
	    <script type="text/javascript">
		    function sum(a, b){
			    return a + b;
			}
			
			var returnValue1 = sum(1, 2);
			alert(returnValue1);
			
			var returnValue2 = sum("jack");// 将 jack变量赋值给a变量，b变量没有赋值系统默认赋值undefined
			alert(returnValue2);// jackundefined
			
			var returnValue3 = sum();
			alert(returnValue3);// NaN(NaN是一个具体存在的值，该值表示不是数字。Not a Number)
			
			var returnValue4 = sum(1, 2, 5);// 1赋给变量a，2赋给变量b，5没赋上
			alert(returnValue4);// 3
			
			// 在JS当中，函数的名字不能重复，当函数重名的时候，后声明的函数会将之前声明的同名函数覆盖掉。
			function text(username){
			    alert("text");
			}
			
			function text(){
		        alert("texttext");
			}
			
			text("zhangsan");// 这个调用的是第二个text函数
		</script>
	
	</body>
</html>
```

```javascript
<!doctype html>

<html>

    <head>
	    <title>JS当中的局部变量和全局变量</title>
	</head>
	
	<body>
	    <script type="text/javascript">
		    /*
			    全局变量：在函数体之外声明的变量属于全局变量，全局变量的生命周期是：
				浏览器打开的时候声明，浏览器关闭的时候释放。尽量少用，比较耗费内存。
				
				局部变量：在函数体内声明的变量，包括一个函数的形参都属于局部变量。生命周期：
				函数开始执行时局部变量的内存开辟，函数执行结束之后，局部变量的内存空间释放。
				生命周期较短。
			*/
			
			var i = 100;
			function accessI(){
			    alert("i = " + i);
			}
			accessI();
			
			var username = "jack";// 全局变量
			function accessUername(){
			    var username = "liming";// 局部变量
			    //就近原则，访问局部变量
			    alert("username = " + username);
			}
			accessUername();
			//访问全局变量
			alert("username = " + username);
			
			/* 当一个变量声明的时候没有使用var关键字，
			那么不管这个变量是在哪里声明的，都是全局变量*/
			
			function myfun(){
			    myname = "danny";
			}
			// 访问函数
			myfun();// 必须有这一句，要是没有这一句还是会出现myname is not defined错误
			alert("myname = " + myname);
		</script>
	
	</body>
</html>
```

```javascript
<!doctype html>

<html>

    <head>
	    <title>JS当中的局部变量和全局变量</title>
	</head>
	
	<body>
	    <script type="text/javascript">
		    /*
			    1、虽然JS中的变量在声明的时候不需要指定数据类型，但是每一个数据还是有类型的。
				JS中的数据类型有：原始类型、引用类型。
				原始类型：Undefined、Number、String、Boolean、Null
				引用类型：Object以及Object的子类
				
				2、ES规范(ECMAScript规范)，在ES6之后，又基于以上6种类型之外添加了一个新的类型：Symbol
				
				3、JS中有一个运算符叫typeof，这个运算符可以在程序运行阶段动态获取变量的数据类型
				typeof语法格式：typeof 变量名。
				typeof运算符的运行结果是以下6个字符串之一。注意字符串都是小写：
				"undefined"、"number"、"string"、"boolean"、"object"、"function"
				
				4、在JS当中比较字符串是否相等使用"=="完成，而不是equals
			*/
			
			//要求a和b的数据类型必须是数字，不能是其他类型
			function sum(a, b){
			    if(typeof a == "number" && typeof b == "number"){
				    return a + b;
				}
				alert(a + ", " + b + "必须都为数字！");
			}
			var returnValue1 = sum(1, 2);// 3
			alert(returnValue1);
			
			var returnValue2 = sum(false, "abc");// undefined
			alert(returnValue2);
			
			
			var i;
			alert(typeof i);// undefined
			
			var k = 10;
			alert(typeof k);// number
			
			var f = "abc";
			alert(typeof f);// string
			
			var d = null;
			alert(typeof d);// object null属于Null类型，但是typeof运算符的结果是"object"
			
			var flag = false;
			alert(typeof flag);// boolean
			
			var obj = new Object();
			alert(typeof obj);// object
			
			function sayHello(){
			
			}
			alert(typeof sayHello);// function
		</script>
	
	</body>
</html>
```

```javascript
<!doctype html>

<html>
    <head>
	    <title>JS当中的局部变量和全局变量</title>
	</head>
	
	<body>
	    <script type="text/javascript">
		    /*
			    Undefined类型只有一个值，这个值就是undefined。
				当一个变量没有手动赋值，系统默认赋值undefined，
				或者也可以给一个变量手动赋值undefined
			*/
			var i; // undefined
			
			var k = undefined;
			
			alert(i == k);// true
				
		</script>
	
	</body>
</html>
```

```javascript
<!doctype html>

<html>
    <head>
	    <title>JS当中的局部变量和全局变量</title>
	</head>
	
	<body>
	    <script type="text/javascript">
		    /*
			    1、Number类型包括哪些值？
				    -1 0 1 2 2.3 3.14 100 ...NaN Infinity
					整形、小数、正数、负数、不是数字、无穷大都属于Number类型
					
				2、关于NaN：表示Not a Number，什么情况下结果是一个NaN？
				    预算结果本来应该是一个数字，但最后算完不是一个数字的时候，结果是NaN。
					
				3、关于Infinity：当除数是0的时候，运算结果为无穷大。
				
				4、isNaN()函数：is Not a Number。true表示不是一个数字，false表示是一个数字。
				5、parseInt()函数：将字符串自动转换成数字，并且取整数位。
				6、parseFloat()函数：将字符串自动转换成数字。
				7、Math.ceil()函数：作用是向上取整。
			*/
		    var v1 = 1;
		    var v2 = 3.14;
		    var v3 = -1;
		    var v4 = NaN;
		    var v5 = Infinity;
			
			// number
			alert(typeof v1);
			alert(typeof v2);
			alert(typeof v3);
			alert(typeof v4);
			alert(typeof v5);
			
			var a = 100;
			var b = "abc";
			alert(a / b);//NaN 除号的最后结果本来应该是一个数字，但是在运算过程中导致最后结果不是一个数字
			
			alert(100 / 0);// Infinity
			
			alert(10 / 3);// 3.3333333333333335
			
			function sum(a, b){
			    if(isNaN(a) || isNaN(b)){
				    alert("参与运算的必须都是数字");
					return;
				}
				return a + b;
			}
			sum(100, "abc");
			alert(sum(100, 10));
			
			alert(parseInt("3.647"));
			
			alert(parseFloat("3.1415926"));
			
			alert(Math.ceil("2.15"));
		</script>
	
	</body>
</html>
```
以下是在尚硅谷里面学的：

**JavaScript中的比较运算：**

等于  ==：等于是简单地做字面值的比较。
全等于  ===：除了做字面值的比较外，还会比较两个两个变量的数据类型。
```javascript
比如：
var a = "12";
var b = 12;
alert(a == b); // true
alert(a === b); // false
```

**逻辑运算**：在JavaScript中，所有的变量都可以作为一个boolean类型的变量去使用。0、null、undefined、""都认为是false。

```javascript
var a = "";
if(a) 其实是 if(Boolean(a)), Boolean函数的作用是将非布尔类型转化成布尔类型。
```
![图示](https://img-blog.csdnimg.cn/2021042020595749.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
**JS当中的数组：**

定义格式： 
var 数组名 = []; // 空数组
var 数组名 = [1, "abc", true]; // 定义数组同时赋值元素

JavaScript中的数组，只要我们通过数组下标赋值，那么最大的下标值，就会自动给数组做扩容操作。

```javascript
var arr = [];
arr[2] = "abc";
alert(arr.length);// 3
```

**函数的 arguments 隐形参数**

就是在 function函数中不需要定义，但却可以直接用来获取所有参数的变量，我们把它叫做隐形参数。

隐形参数类似Java中的可变长参数，JS中的隐形参数和Java的可变长参数一样，操作类似数组。

```javascript
function fun(){
    for(var i = 0; i < arguments.length; i++){
        alert(arguments[i]);
    }
}
fun(1, "ad", true);
```

**JS中的自定义对象**

1、Object形式的自定义对象：
    var 变量名 = new Object(); // 对象实例，空对象
    变量名.属性名 = 值; // 定义一个属性
    变量名.函数名 = function(){} // 定义一个函数

对象的访问：变量名.属性 / 函数名()；

2、{}花括号形式的自定义对象

对象的定义：
var 变量名 = {        // 空对象 
    属性名 : 值，     // 定义一个属性
    属性名 : 值，     // 定义一个属性
    函数名 : function(){}     // 定义一个函数
}; 

对象的访问：变量名.属性 / 函数名()；

**JS中的事件**
什么是事件？事件是电脑输入设备与页面进行交互的响应，我们称之为事件。

>**常用的事件：**
>onload：加载完成事件。作用：页面全部加载完成之后才会发生，常用于做页面JS代码的初始化操作。
>onclick：单击事件。作用：常用于按钮的点击响应操作。
>onblur：失去焦点事件。作用：常用于输入框失去焦点后验证其输入的内容是否合法。
>onchange：内容发生改变事件。作用：常用于下拉列表和输入框内容发生改变年之后的操作。
>onsubmit：表单提交事件。作用：常用于表单提交前，验证所有表单项是否合法。

>**事件的注册分为静态注册和动态注册两种。**
>**什么是事件的注册**？其实就是告诉浏览器，当事件响应后要执行哪些操作代码，叫事件注册或者事件绑定。
>静态注册事件：通过HTML标签的事件属性直接赋予事件响应后的代码，这种方式叫做静态注册。
>动态注册事件：是指先通过 JS 代码得到标签的dom对象，然后再通过dom对象.事件名 = function(){} 这种形式赋予事件响应后的代码，叫动态注册。
>**动态注册基本步骤：
>1、获取标签对象
>2、标签对象.事件名 = function(){}**

```javascript
<!doctype html>
<html>
    <head>
	    <title>onclick的静态注册和动态注册</title>
		
		<script type="text/javascript">
		
		    function onclickFun(){
			    alert("静态注册onclick事件");
			}
			
			/* 页面加载完毕之后后面的函数才会自动执行(注意：后面函数里的东西只是在进行动态注册)。如果没有onload即页面没加载完
			就执行后面的函数，那么id="btn"的这个元素还没有加载到浏览器里面，因此也就无法获取到按钮对象*/
			window.onload = function(){ // 动态注册onclick事件
			    /*  1、获取标签对象
					2、标签对象.事件名=function(){} */
					
				// 第一步：获取这个按钮对象
				var buttObj = document.getElementById("btn");
				// 第二步：给按钮对象的onclick属性赋值
				buttObj.onclick = function(){
				    alert("动态注册onclick事件");
				}
			}
		</script>
		
	</head>
	
	<body>
	    <!--
	    以下是静态注册，直接在标签中使用事件句柄。
	    以下代码的含义是：将onclickFun函数注册到button这个按钮上，等待click事件发生后，该函数被浏览器调用
		-->
		<input type="button" value="按钮1" onclick="onclickFun()"/>
		
		<input type="button" value="按钮2" id = "btn" />
	
	</body>
</html>
```

```javascript
<!doctype html>
<html>
    <head>
	    <title>onblur的静态注册和动态注册</title>
		
		<script type="text/javascript">
		    function onblurFun(){
			    alert("静待注册onblur事件");
			}
			
			window.onload = function(){
			    // 1、获取标签对象
			    var pwdObj = document.getElementById("password");
				// 2、标签对象.事件名=function(){}
				pwdObj.onblur = function(){
				    alert("动态注册onblur事件");
				}
			
			}
		</script>
		
	</head>
	
	<body>
	    
		用户名：<input type="text" onblur="onblurFun()"/>
		
		密码：<input type="text" id = "password" />
	
	</body>
</html>
```

```javascript
<!doctype html>
<html>
    <head>
	    <title>onchange的静态注册和动态注册</title>
		
		<script type="text/javascript">
		    function onchangeFun(){
			    alert("静待注册onchange事件");
			}
			
			window.onload = function(){
			    // 1、获取标签对象
			    var cObj = document.getElementById("c");
				// 2、标签对象.事件名=function(){}
				cObj.onchange = function(){
				    alert("动态注册onchange事件");
				}
			}
		</script>
		
	</head>
	
	
	<body>    
		<!--<select onchange="onchangeFun()">-->
		<select id="c">
		    <option>北大</option>
			<option>清华</option>
			<option>人大</option>
			<option>复旦</option>
		</select>
	</body>
</html>
```

```javascript
<!doctype html>

<html>

    <head>
	    <title>onsubmit的静态注册和动态注册</title>
		
		<script type="text/javascript">
		
		    function onsubmitFun(){
			// 要验证所有表单项是否合法，如果有一个不合法就阻止表单提交
			    alert("静待注册onsubmit事件,并且发现不合法");
				return false;
			}
			
			window.onload = function(){
			    // 1、获取标签对象
			    var subObj = document.getElementById("sub");
				// 2、标签对象.事件名=function(){}
				subObj.onsubmit = function(){
				    alert("动态注册onsubmit事件,并且发现不合法");
					return false;
				}
			}
		</script>	
	</head>
	
	
	<body>
	    <!--onsubmit="return false"会阻止表单提交-->
	    
	    <form action="http://190.168.111.3:8080/oa/sava" onsubmit="return onsubmitFun()">
		    <input type="submit" value="静态注册" />
		</form>
		
		<form action="http://190.168.111.3:8080/oa/sava" id="sub">
		    <input type="submit" value="动态注册" />
		</form>
		
	</body>
</html>
```
**DOM模型**
DOM全称是 Document Object Model 文档对象模型。即：就是把文档中的标签、属性、文本，转换成为对象来管理
![t图示](https://img-blog.csdnimg.cn/20210422204106568.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
对Document对象的理解：
1、Document管理的所有的HTML文档内容
2、document是一种树结构的文档，有层级关系
3、它让我们把所有的标签都对象化
4、我们可以使用document访问所有的标签对象。
![图示](https://img-blog.csdnimg.cn/20210422205033687.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)

```javascript
<!doctype html>

<html>
    <head>
	    <title>验证用户</title>
		
		<!--需求：当用户点击了验证按钮，要获取文本框中的内容，然后验证内容是否合法
		    验证的规则是：必须由字母、数字、下划线组成，并且长度为5到12位
		-->
		<script type="text/javascript">
		    function onclickFun(){
			    // 1、当我们要操作一个标签的时候，一定要先获取这个标签对象
			    var usernameObj = document.getElementById("username");
				// usernameObj 这个就是dom对象
			    var valueText = usernameObj.value;
				// 3、如何验证字符串满足某个规则：需要使用正则表达式
			    var patt = /^\w{5,12}$/;
				
				var usernameSpanObj = document.getElementById("usernameSpan");
			    
				// test()方法用于测试某个字符串是否匹配我的规则
			    if(patt.test(valueText)){
			        //alert("用户名合法");
					usernameSpanObj.innerHTML = "用户名合法";
			    }else{
			        //alert("用户名不合法");
					usernameSpanObj.innerHTML = "用户名不合法";
			    }   
			}
		</script>
	</head>
	
	<body>
	    用户名：<input type="text" id="username" value="" />
		<span id="usernameSpan" style="color:red"></span>
		
		<input type="button" value="验证" onclick="onclickFun()" />
	</body>
</html>
```

```javascript
<!doctype html>

<html>
    <head>
	    <title>正则表达式</title>
		
		<script type="text/javascript">
		    // 字符串中是否有字符 e
		    //var patt = /e/;
			
			// 字符串中是否包含字符 a或b或c
			//var patt = /[abc]/;
			
			// 字符串中是否包含小写字母
			//var patt = /[a-z]/;
			
			// 字符串中是否包含大写字母
			//var patt = /[A-Z]/;
			
			// 字符串中是否包含数字
			//var patt = /[0-9]/;
			
			// 字符串中是否包含单词字符(单词字符包括字母、数字、下划线)
			//var patt = /\w/;
			
			// 字符串中是否包含至少一个a
			//var patt = /a+/;
			
			// 字符串中是否包含零个或多个a。包含的意思是：要求最小条件满足就停止检查
			//var patt = /a*/;
			
			// 字符串中是否包含零个或一个a
			//var patt = /a?/;
			
			// 字符串中是否包含连续3个a
			//var patt = /a{3}/;
			
			// 字符串中是否包含至少3个连续的a，至多5个连续的a
			//var patt = /a{3,5}/;
			
			// 字符串是否包含至少3个连续的a
			//var patt = /a{3,}/;
			
			// 字符串是否以 a 结尾
			//var patt = /a$/;
			
			// 字符串是否以 a 开头
			//var patt = /^a/;
			
			// 字符串中是否包含至少3个连续的a，至多5个连续的a。要求字符串必须从头到尾都完全匹配
			var patt = /^a{3,5}$/;
			var str = "aaa";
			alert(patt.test(str));
			
		</script>
	</head>
	
	<body></body>
</html>
```
```javascript
<!doctype html>

<html>
    <head>
	    <title>getElementsByName方法</title>
		
		<script type="text/javascript">
		    function checkAll(){
			    //让所有边框选中
				
				// document.getElementsByName(); 是根据指定的name属性来查询并返回集合
				var hobbies = document.getElementsByName("hobby");
				for(var i = 0; i < hobbies.length; i++){
				    // checked 表示复选框的选中状态，如果选中是true，不选中是false
					// checked 这个属性可读、可写
					hobbies[i].checked = true;
				}
			}
			
			function checkNone(){
			    //让所有边框都不选中
				
				var hobbies = document.getElementsByName("hobby");
				for(var i = 0; i < hobbies.length; i++){ 
					hobbies[i].checked = false;
				}
			}
			
			function checkReverse(){
			    //让选中的边框不选中，没选中的边框选中
				
				var hobbies = document.getElementsByName("hobby");
				for(var i = 0; i < hobbies.length; i++){
					if(hobbies[i].checked){
					    hobbies[i].checked = false;
					}else{
					    hobbies[i].checked = true;
					}
				}
			}
		
		</script>
	</head>
	
	<body>
	    <input type="checkbox" name="hobby" value="cpp">C++
		<input type="checkbox" name="hobby" value="java">Java
		<input type="checkbox" name="hobby" value="python">Python
	    <br/>
		<button onclick="checkAll()">全选</button>
		<button onclick="checkNone()">全不选</button>
		<button onclick="checkReverse()">反选</button>
	</body>

</html>
```

```javascript
<!doctype html>

<html>

    <head>
	    <title>getElementsByTagName方法</title>
		
		<script type="text/javascript">
		    function checkAll(){
			    //让所有边框选中
				
				// document.getElementsByName(); 是根据指定的标签名来查询并返回集合
				var inputs = document.getElementsByTagName("input");
				for(var i = 0; i < inputs.length; i++){
				    // checked 表示复选框的选中状态，如果选中是true，不选中是false
					// checked 这个属性可读、可写
					inputs[i].checked = true;
				}
			}
		
		</script>
	</head>
	
	<body>
	    <input type="checkbox" value="cpp">C++
		<input type="checkbox" value="java">Java
		<input type="checkbox" value="python">Python
	    <br/>
		<button onclick="checkAll()">全选</button>
	</body>

</html>
```
注：document对象的三个查询方法，如果有id属性，优先使用getElementById方法来进行查询。如果没有id属性，则优先使用getElementsByName方法来进行查询。如果id属性和name属性都没有最后再按标签名查询getElementsByTagName。
以上三个方法，一定要在页面加载完成之后执行，才能查询到标签对象。

```javascript
<!doctype html>

<html>

    <head>
	    <title>createElement方法</title>
		<script type="text/javascript">
			window.onload = function(){
			    // 使用js代码来创建JS标签，标签内容：<div>哈哈哈哈</div> 
		        var divObj = document.createElement("div");
				divObj.innerHTML = "哈哈哈哈";
			    document.body.appendChild(divObj);
		    }
		</script>
		
	</head>
    <body>
	
	<body>
</html>
```