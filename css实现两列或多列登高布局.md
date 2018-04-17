很多时候，在网页布局中可能会有等高布局这种需求，即所有列的高度按最高列的高度来自适应。实现等高布局的方法有很多，这里来做一下总结。

### 一、假等高列

> 假等高列，顾名思义就是不是真的每一列都等高，而是在父元素中使用背景图片来在y轴上的repeat来达到视觉上的等高效果


类似于这种的背景图片，来做假等高效果        

![](https://tianjin-blog-1253531556.cos.ap-beijing.myqcloud.com/denggaolayout/denggao-background.png)

下面是示例代码：
```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<style type="text/css">
		html,body{
			padding: 0;
			margin: 0;	
		}
		.container{
			position: relative;
			overflow: hidden;
			background: url(https://example.png) repeat-y;
			background-size: contain;
		}
		.item{
			width: 20%;
			height: 100%;
			float: left;
			background: transparent;
		}
	</style>

</head>
<body>
	<div class="container">
		<div class="item item1">
			<br>
			<br>
			<br>
			<br>
			<br>
			<br>
			<br>
			<br>
		</div>	
		<div class="item item2"></div>
		<div class="item item3"></div>
		<div class="item item4"></div>
		<div class="item item5"></div>
	</div>
</body>
</html>
```

> 优点：实现简单，兼容性强      
> 缺点：这种方法不适合做流体列等高布局，而且如果改变列数，必须重新制作背景图

### 二、给容器div使用单独背景色

![图例](https://tianjin-blog-1253531556.cos.ap-beijing.myqcloud.com/denggaolayout/denggao-example.png)

> 这种方法就如上图所示，每一列都有一个容器作为设置背景，将这些容器嵌套，如图所示，无论那一列的高度增加，容器的高度都会随之增加，及搭档了等高布局的效果。下面是示例代码：

```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<style type="text/css">
		html,body{
			padding: 0;
			margin: 0;	
		}
		.container{
			position: relative;
			width: 960px;
			height: 500px;
		}
		.rightBack{
			width: 100%;
			float: left;
			position: relative;
			overflow: hidden;
			background: green;
		}
		.contentBack{
			width: 100%;
			float: left;
			position: relative;
			right: 320px;/*相当于right的宽度*/
			background: orange;
		}
		.leftBack{
			width: 100%;
			float: left;
			position: relative;
			right: 420px;/*相当于content的宽度*/
			background: lime;
		}
		.left{
			float: left;
			width: 220px;
			overflow: hidden;
			position: relative;
			left: 740px;
		}
		.content{
			float: left;
			width: 420px;
			overflow: hidden;
			position: relative;
			left: 740px;
		}
		.right{
			float: left;
			overflow: hidden; 
			width: 320px;
			position: relative; 
			left: 740px;
		}
	</style>

</head>
<body>
	<div class="container">
		<div class="rightBack">
			<div class="contentBack">
				<div class="leftBack">
					<div class="left">
						left<br>
						left<br>
						left<br>
						left<br>

						left<br>
						left<br>
						left<br>
						left<br>

					</div>
					<div class="content">content</div>
					<div class="right">rgiht</div>
				</div>
			</div>
		</div>
	</div>
</body>
</html>
```
这种方式理解起来可能有点抽象，可以结合模型图和代码进行实践俩理解。

### 三、常见带边框的两列等高布局

> 平时的开发中我们可能还会遇到等高布局的情况就是每列带有边框，这种布局使用上面的方法可能就不太适用了。所以这里介绍一种两列带边框的等高布局的实现方法。

```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<style type="text/css">
		html,body{
			padding: 0;
			margin: 0;	
		}
		html{
			background: #45473f; 
			height: auto;
		}
		body{
			width: 960px;
			margin: 20px auto;
			background: #ffe3a6;
			border: 1px solid #ccc;
		}
		.wrapper{
			display: inline-block;
			border-left: 200px solid #d4c376;
			position: relative;
			vertical-align: bottom;
		}
		.left{
			float: left;
			width: 200px;
			margin-left: -200px;
			margin-right: -1px;
			border-right: 1px solid #888;
			position: relative;
		}
		.right{
			float: left;
			border-left: 1px solid #888;
		}
		.left,.right{
			padding-bottom: 2em;
		}
	</style>

</head>
<body>
	<div class="wrapper">
		<div class="left">left</div>
		<div class="right">right</div>
	</div>
</body>
</html>
```
这种方法可以兼容所有浏览器，但是他也有很大的缺陷，就是一旦超过三列，这种布局就不适用了。所以如果有多列并且需要加边框的话，可以借助JavaScript，或者下面介绍的这种方法。

### 四、使用正padding和负margin对冲实现多列布局

> 这种方法及利用正负padding、margin病情设置一个容器，并设置overflow:hiden,把溢出的背景隐藏掉。下面是代码：
```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<style type="text/css">
		html,body{
			padding: 0;
			margin: 0;	
		}
		.wrapper{
			width: 90%;
			overflow: hidden;
			margin: 0 auto;
		}
		.column{
			width: 30%;
			background:#ccc;
			margin-bottom: -9999px;
			padding-bottom: 9999px;
			float: left;
		}
		.content{
			background: #aaa;
		}
	</style>

</head>
<body>
	<div class="wrapper">
		<div class="left column">left</div>
		<div class="content column">
			content<br>
			content<br>
			content<br>
			content<br>
			content<br>
			content<br>
			content<br>
		</div>
		<div class="right column">right</div>
	</div>


</body>
</html>
```

这种方式可以实现多列有边框的布局，但是如果有底部边框`border-bottom`的话，则还需要一些辅助的方法才能实现

#### 底部边框添加方法

> 可以在每一列中添加一个div元素来代替地边框，并且容器元素wrapper设置`position:relative`,每一列不用设置。代码实例如下

```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<style type="text/css">
		html,body{
			padding: 0;
			margin: 0;	
		}
		.wrapper{
			width: 90%;
			overflow: hidden;
			margin: 0 auto;
			position: relative;
			padding: 
		}
		.column{
			width: 30%;
			background:#ccc;
			margin-bottom: -9999px;
			padding-bottom: 9999px;
			float: left;
			border:1px solid red;
		}
		.content{
			background: #aaa;
			border-left: none;
			border-right: none;
		}
		.line{
			height: 1px;
			width: 30%;
			background: red;
			position: absolute;
			bottom: 0;
		}
	
		
	</style>

</head>
<body>
	<div class="wrapper">
		<div class="left column">left
			<div class="line"></div>
		</div>
		<div class="content column">
			content<br>
			content<br>
			content<br>
			content<br>
			content<br>
			content<br>
			content<br>
			<div class="line"></div>
		</div>
		<div class="right column">right
		<div class="line"></div>
	</div>
	</div>


</body>
</html>
```

### 五、使用边框和定位来模拟等高

> 这种方式和上面的第三种布局的原理才不多，这里就不详细介绍，只列出实例代码

```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<style type="text/css">
		html,body{
			padding: 0;
			margin: 0;	
		}
		#wrapper { 
			width: 960px;
		 	margin: 0 auto; 
		} 
		#mainContent { 
			border-right: 220px solid #dfdfdf; 
			position: absolute; 
			background: red;
			width: 740px; 
		} 
		#sidebar { 
			background: #dfdfdf; 
			margin-left: 740px; 
			position: absolute; 
			width: 220px; 
		}
	
		
	</style>

</head>
<body>
	<div id="wrapper">
		<div id="mainContent">left
<br>
<br>
		</div>
		<div id="sidebar">right</div>
	</div>



</body>
</html>
```
### 六、边框模仿等高列

> 和上面三、五原理差不多
```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<style type="text/css">
		html,body{
			padding: 0;
			margin: 0;	
		}
		#container{ 
			background-color:#0ff; 
			overflow:hidden; 
			width:960px; 
			margin: 0 auto; 
			} 
		#content{ 
			background-color:#0ff; 
			width:740px; 
			border-right:220px solid #f00; /* 边框大小和颜色设置和right大小与颜色一样 */ 
			margin-right:-220px; /*此负边距大小与right边栏宽度一样*/ 
			float:left; 
			} 
		#right{ 
			background-color:#f00; 
			width:220px; 
			float:left; 
		} 

</head>
<body>
	<div id="container">
		<div id="content">This is<br />some content</div>
		<div id="right">This is the right</div>
	</div>
</body>
</html>


```


