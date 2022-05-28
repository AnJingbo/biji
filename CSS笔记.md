

1、什么是CSS？
    CSS(Cascading Style Sheet)：层叠样式表。
    
2、CSS有什么作用？
    修饰HTML页面，设置HTML页面中的某些元素的样式，让HTML页面更好看。
3、在HTML页面中嵌套使用CSS的三种方式？

```html
1、内联定义方式：在标签内部使用style属性来设置属性的CSS样式。

语法格式：
<标签 style="样式名:样式值;样式名:样式值;样式名:样式值"; ...></标签>
```
```html
2、样式块方式：在head标签中使用style块。

语法格式：
<head>
    <style type="text/css">
        选择器{
            样式名:样式值;
            样式名:样式值;
            ......
        }
        选择器{
            样式名:样式值;
            样式名:样式值;
            ......
        }
    </style>
</head>
```
```html
3、链入外部样式表文件：这种方式最常用，将样式写到一个独立的xxx.css文件当中，在需要的网页上直接引入css文件，
样式就引入了。这种方式易维护，维护成本低

语法格式：
<link type="text/css" rel="styleheet" href="css文件的路径" />
```

```html
<!doctype html>

<html>

    <head>
	    <title>HTML中引入CSS样式的第一种方式：内联定义方式</title>
	</head>
	
	<body>
	    
		<!--width 宽度样式
		    height 高度样式
			background-color 背景色样式
			display 布局样式(none表示隐藏，block表示显示)
		-->
	    <div style="width:300px; height:300px; background-color:#CCFFFF; display:block;
		border-color:red; border-width:1px; border-style:solid;"></div>
		
		<br><br>
		
		<!--样式还能这样写：
		        border:1px solid black
		--> 
		<div style="width:300px; height:300px; background-color:#CCFFFF; display:block;
		border:1px solid black;"></div>
	
	</body>

</html>
```

```html
<!doctype html>

<html>

    <head>
	    <title>HTML中引入CSS样式的第二种方式：样式块</title>
		
		<style style="text/css">
			/*
			    这是css的注释
			*/
			
			/*
			    id选择器
				语法格式：
                    #id名{
					    样式名 : 样式值;
						样式名 : 样式值;
						样式名 : 样式值;
						.....
					}				
			*/
			#usernameErrorMsg{
			    color : red;
				font-size : 10px;
			}
			
			/*
			    标签选择器
				语法格式：
				标签名{
				    样式名 : 样式值;
					样式名 : 样式值;
					样式名 : 样式值;
					.....
				}
				标签选择器作用的范围比id选择器广
			*/
			div{
			    background : black;
				border : 1px solid red;
				width : 100px;
				height : 100px;
			}
			
			/*
			    类选择器
				语法格式：
				.类名{
				    样式名 : 样式值;
					样式名 : 样式值;
					样式名 : 样式值;
					.....
				}
			*/
			.myclass{
			    border : 1px solid red;
				width : 100px;
				height : 30px;
			}
		</style>
	</head>
	
	<body>
	    
		<span id="usernameErrorMsg">对不起，用户名不能为空</span>
		
		<br>
		<div></div>
		<div></div>
		<div></div>
		
		<!--class相同的标签可以认为是同一类标签-->
		<br>
		<input type="text" class="myclass"/>
		
		<br>
		<select class="myclass">
		    <option>本科</option>
			<option>专科</option>
		</select>
	</body>

</html>
```

![图示](https://img-blog.csdnimg.cn/20210410131115401.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
```html
<!doctype html>

<html>
    <head>
	    <title>CSS-HTML中引入CSS样式的第三种方式,引入外部独立的css文件</title>
	    
		<!--引入css-->
		<link rel="stylesheet" type="text/css" href="D:\test.css"/>
	</head>
	
	<body>
	    <a href="http://www.baidu.com">百度</a>
		
		<span id="mySpan">点击我！！</span>
	</body>

</html>
```

```html
<!doctype html>

<html>

    <head>
	    <title>列表样式</title>
		
		<style type="text/css">
		    ul{
			    /*list-style-type : none;*/
				list-style-type : circle;
			}
		</style>
	</head>
	
	<body>
	    
		<ul>
		    <li>中国
			    <ul>
				    <li>上海</li>
					<li>西安</li>
					<li>石家庄</li>
			</li>
			<li>美国</li>
			<li>俄国</li>
		</ul>
	
	</body>

</html>
```

```html
<!doctype html>

<html>

    <head>
	    <title>CSS样式的绝对定位</title>
		
		<style type="text/css">
		    #div1{
			    background : red;
				border : 1=x solid black;
				width : 300px;
				height : 300px;
				position : absolute;/*绝对定位*/
				left : 100px;
				top : 300px;
			}
		</style>
		
	</head>
	
	<body> 
		<div id="div1"></div>
	</body>

</html>
```