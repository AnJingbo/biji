

## 1、jQuery介绍
什么是jQuery？
jQuery，顾名思义，也就是 JavaScript 和查询（Query），它就是辅助 JavaScript开发的 js 类库。

jQuery的核心思想是 write less，do more(写的更少，做得更多)，所以它实现了很多浏览器的兼容问题

jQuery现在已经成为最流行的JavaScript库

常见问题：
1、使用jQuery一定引入 jQuery 库吗？  答案：是，必须引入。
2、jQuery中的 $ 到底是什么？  答案：它是一个函数。
3、怎么为按钮添加点击响应函数的？
 (1) 使用 jQuery 查询到标签对象
 (2) 使用 标签对象.click(function(){});
```javascript
<!doctype html>

<html>
    <head>
        <title></title>
         
		<script type="text/javascript" src="D:/jQuery/jquery-1.12.4.js"></script>
		
		<script type="text/javascript">
		    window.onload = function(){
			    var butObj = document.getElementById("but1");
				butObj.onclick = function(){
				    alert("原生");
				}
			} 
			
			
			$(function(){// 表示页面加载完成之后，相当于 window.onload = function(){}
			    var $butObj = $("#but2");// 表示按id查询标签对象
				$butObj.click(function(){// 绑定单击事件
				    alert("jQuery 的单击事件");
				});
			});
		</script>
		
    </head> 
    
	<body>
	    <input type="button" id="but1" value="按钮1"/>
	    <input type="button" id="but2" value="按钮2">
	</body>

</html>
```

$ jQuery的核心函数，能完成jQuery的很多功能。$() 就是调用 $ 这个函数。

1、传入参数为 函数 时：表示页面加载完成之后。相当于 window.onload = function(){}

2、传入参数为 HTML对象时：会为我们创建 html 标签对象

3、传入参数为 选择器字符串 时：
$("# id属性值"); id选择器，根据id查询标签对象。
$("标签名"); 标签名选择器，根据指定的标签名查询标签对象。
$(".class属性值"); 类型选择器，根据class属性查询标签对象。

4、传入参数为 DOM 对象时：会把这个dom对象转换为jQuery对象。

```javascript
<!doctype html>

<html>
    <head>
        <title></title>
         
		<script type="text/javascript" src="D:/jQuery/jquery-1.12.4.js"></script>
		
		<script type="text/javascript">
		    // 传入参数为函数时，在文档加载完成之后执行这个函数
		    $(function(){
			    alert("页面加载完成之后，自动调用！");
				
				// 传入参数为HTML字符串时，根据这个字符串创建元素节点对象
				$("<button>啊哈</botton>").appendTo("body");
				
				// 传入参数是选择器字符串时，根据这个字符串查找元素节点对象
				alert($("input").length);
				
				// 传入参数是dom对象时，将dom对象转换为jQuery对象
				var butObj = document.getElementById("but1");// 获取dom对象
				alert($(butObj));
			});
		</script>
		
    </head> 
    
	<body>
	    <input type="button" id="but1" value="按钮1"/>
		<input type="button" id="but2" value="按钮2"/>
	</body>

</html>
```

jQuery对象和dom对象的区分：

Dom对象：
1、通过 getElementById() 查询出来的标签对象是 Dom 对象
2、通过 getElementsByName() 查询出来的标签对象是 Dom 对象
3、通过 getElementsByTagName() 查询出来的标签对象是 Dom 对象
4、通过 createElement() 方法创建的对象是 Dom 对象

Dom 对象 alert 出来的效果是：[Object HTML 标签名Element]

jQuery对象：
1、通过 jQuery 提供的 API 创建的对象，是 jQuery对象
2、通过 jQuery 包装的 Dom 的对象，是 jQuery对象
3、通过 jQuery 提供的 API 查询到的对象，是 jQuery对象

jQuery
 对象 alert 出来的效果是：[object Object]

jQuery 对象的本质是什么？
jQuery 对象是 dom对象的数组 + jQuery 提供的一系列的功能函数。

jQuery 对象和 Dom 对象的使用区别？
jQuery 对象不能使用 Dom 对象的属性和方法。
Dom 对象不能使用 jQuery 对象的属性和方法。

Dom 对象和 jQuery 对象互转：
1、dom 对象转换为 jQuery 对象
（1）先有dom对象。
（2）$(dom对象); 就可以转化称为 jQuery 对象。
2、jQuery 对象转化为 dom 对象
（1）先有 jQuery 对象。
（2）jQuery 对象[下标取出相应的dom对象]。
![图示](https://img-blog.csdnimg.cn/20210503224703889.png)

## jQuery选择器 重点
![图示](https://img-blog.csdnimg.cn/20210505175622646.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
![图示](https://img-blog.csdnimg.cn/2021050517581017.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 过滤选择器
包括 基本过滤选择器、内容过滤选择器、属性过滤选择器、表单过滤选择器、元素筛选
![图示](https://img-blog.csdnimg.cn/20210505160353445.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
![图示](https://img-blog.csdnimg.cn/20210505180238493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
![图示](https://img-blog.csdnimg.cn/20210505180011434.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
![图示](https://img-blog.csdnimg.cn/20210505180248412.png)
![图示](https://img-blog.csdnimg.cn/20210505175959565.png)

## jQuery的属性操作
html()：它可以设置和获取起始标签和结束标签中的内容。和dom属性的 innerHTML 一样。
test()：它可以设置和获取起始标签和结束标签中的文本。和dom属性的 innerText 一样。
val()：它可以设置和获取**表单项**的value属性值。和dom属性的 value 一样。val可以设置多个表单项的选中状态。
```javascript
<!doctype html>

<html>
    <head>
        <title></title>   
		<script type="text/javascript" src="D:/jQuery/jquery-1.12.4.js"></script>
		<script type="text/javascript">
			$(function(){
			    // 不传参数是获取，传参数是设置
			    alert( $("div").html() );
				$("div").html("aaaa");
				
				// 不传参数是获取，传参数是设置
				alert($("div").text());
				$("div").text("bbbb");
				
				// 不传参数是获取，传参数是设置
				$("button").click(function(){
				    alert($("#username").val());
					$("#username").val("cccc");
				});
				
				$(":radio").val(["radio1"]);
			})
			
		</script>	
    </head> 
    
	<body>
	    <div>我是div标签 <span>我是div中的span标签</span></div>
		
		<input type="text" id="username" />
		<button>按钮</button>
		
		<input name="radio" type="radio" value="radio1" />radio1
		<input name="radio" type="radio" value="radio2" />radio2
	</body>
</html>
```
attr()：可以获取和设置属性的值，不推荐操作checked、readOnly、selected、disabled等等。attr方法还可以操作非标准的属性。比如自定义属性。
prop()：可以获取和设置属性的值，只推荐操作checked、readOnly、selected、disabled等等。

```javascript
<!doctype html>

<html>
    <head>
        <title></title>       
		<script type="text/javascript" src="D:/jQuery/jquery-1.12.4.js"></script>
		<script type="text/javascript">
			$(function(){
			    alert( $(":radio:first").attr("name") );// 获取
				$(":radio:first").attr("name", "aaaa");// 设置
				
				alert( $(":radio:first").attr("checked") );// 官方觉得返回 undefined 是一个错误
				alert( $(":radio:first").prop("checked") );// 返回 false
				
				$(":radio:first").attr("abc", "abcValue");
			})
			
		</script>
		
    </head> 
    
	<body>
		<input name="radio" type="radio" value="radio1" />radio1
		<input name="radio" type="radio" value="radio2" />radio2
	</body>
</html>
```
## DOM的增删改
1、内部插入：
appendTo()：a.appendTo(b)：把 a 插入到 b 子元素末尾，成为最后一个子元素。
prependTo()：a.prependTo(b)：把 a 插入到 b 所有子元素前面，成为第一个子元素。

2、外部插入：
insertAfter()：a.insertAfter(b)：得到 ba
insertBefore()：a.insertBefore(b)：得到 ab

3、替换：
replaceWith()：a.replaceWith(b)：用 b 替换掉 a
replaceAll()：a.replaceAll(b)：用 a 替换掉所有 b

4、删除：
remove()：a.remove()：删除 a 标签
empty()：a.empty()：清空 a 标签里的内容，但是 a 标签还在

注：在事件响应的function函数中，有一个this对象，这个this对象是当前正在响应事件的dom对象。
## CSS样式操作
addClass()：添加样式
removeClass()：删除样式
toggleClass()：有就删除，没有就添加样式
offset()：获取和设置元素的坐标

```javascript
当前这个样式只能div使用
div.blueBorder{
    border: 5px blue solid;
}
使用该样式的标签，其后代里面必须有一个 a 标签
.promoted a {
    color:#F50;
}
```

## jQuery动画
**基本动画**
show()：将隐藏的元素显示
hide()：将可见的元素隐藏
toggle()：可见就隐藏，不可见就显示

以上动画方法都可以添加参数：
1、第一个参数是 动画的执行时长，以毫秒为单位
2、第二个参数是 动画的回调函数（动画完成后自动调用的函数）

**淡入淡出动画**
fadeIn()：淡入（慢慢可见）
fadeOut()：淡出（慢慢消失）
fadeTo()：在指定时长内慢慢的将透明度修改到指定的值。0为透明，1为完全可见，0.5为半透明
fadeToggle()：淡入/淡出 切换

## jQuery事件操作
$(function(){}); （jQuery的页面加载完成之后） 和 window.onload = function(){} （原生 js 的页面加载完成之后） 的区别？

它们分别在什么时候触发？
1、jQuery的页面加载完成之后，是浏览器的内核解析完页面的标签，创建好DOM对象之后就会马上执行。
2、原生 JS 的页面加载完成之后，出了要等浏览器的内核解析完成，标签创建好 DOM 对象之外，还要等标签显示时需要的内容加载完成。

它们触发的顺序？
jQuery的页面加载完成之后 先执行，原生 JS 的页面加载完成之后 后执行。

它们执行的次数？
1、jQuery的页面加载完成之后，是把全部注册的function函数，依次按顺序全部执行。
2、原生 JS 的页面加载完成之后，只会执行最后一次的赋值函数。
## jquery中其他事件的处理方法
click()：它可以绑定单击事件，以及触发单击事件
mouseover()：鼠标移入事件
mouseout()：鼠标移出事件
bind()：可以给元素一次性绑定一个或多个事件
one()：使用上跟 bind() 一样，但是 one 方法绑定的每个事件都只会响应一次。
unbind()：跟 bind 方法相反的操作，解除事件的绑定
live()：也是用来绑定事件。它可以用来绑定选择匹配的所有元素的事件，即使这个元素是后面动态创建出来的。
## 事件的冒泡
什么是事件的冒泡？
事件的冒泡是指：父子元素同时监听同一个事件。当触发子元素的事件的时候，同一个事件也被传递到了父元素的事件里面去响应。

如何阻止事件的冒泡？
在子元素的事件函数体内，return false 就可以阻止事件的冒泡传递。

## jQuery事件对象
事件对象，是封装有触发的事件信息的一个javascript对象。
如何获取JavaScript事件对象呢？
在给元素绑定事件的时候，在事件的function(event)参数列表中添加一个参数，这个参数名，我们习惯取名为event。这个event就是JavaScript传递参数事件处理函数的事件对象。
```javascript
<!doctype html>

<html>
    <head>
        <title></title>
         
		<script type="text/javascript" src="D:/jQuery/jquery-1.12.4.js"></script>
		<script type="text/javascript">
		
			// 原生javascript获取事件对象
			window.onload = function(){
			    document.getElementById("div1").onclick = function(event){
				    console.log(event);
				}
			}
			
			// jQuery代码获取事件对象
			$(function(){
			    $("#div1").click(function(event){
				    console.log(event);
				})
			});
			
			// 使用bind同时对多个事件绑定同一个函数，怎么获取当前操作是什么事件
			$(function(){
			    $("#div1").bind("mouseover mouseout", function(event){
				    if(event.type == "mouseover"){
					    console.log("鼠标移入");
					}else if(event.type == "mouseout"){
					    console.log("鼠标移出");
					}
				})
			});
		</script>
		
    </head> 
    
	<body>
		<div id="div1" style="width:300px; height:300px; background-color:#CCFFFF; border-color:red;"></div>
	</body>

</html>
```