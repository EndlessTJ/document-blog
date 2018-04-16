>在某些管理界面的布局中可能经常会遇到这样的问题，一列或两列宽度固定，另一列自适应宽度。 或者上下高度固定，中间部分高度自适应。
这写布局有很多方法可以实现，我这里做一些总结。（当然，现在已经2018了，就暂时不考虑兼容IE6了。可以自行百度或google）

### 两列或三列宽度自适应布局

##### 一、利用`float`和`calc()`函数进行布局

> 首先我们来解释一下`calc()`函数,MDN doc的解释是`calc()`是一个css function，让我们可以使用计算来指定某些css属性。在括号中你可以使用`+` `-` `*` `/`甚至是复数或者变量来计算某些css属性的值。这个属性相当方便。当然他也是有一些兼容性的问题的,[详情查看](https://developer.mozilla.org/en-US/docs/Web/CSS/calc)

下面直接上代码：（由于编辑器关系，这里没有图片展示，想看的可以复制代码自己测试）

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
			height: 400px;
			overflow: hidden;
		}
		.left{
			width: 200px;
			height: 100%;
			float: left;
			background: #434edd;
		}
		.right{
		
			width: calc(100% - 200px);
			float:left;
			height: 100%;
			background: #cd43da;
		}
	</style>

</head>
<body>
	<div class="container">
		<div class="left"></div>
		<div class="right"></div>
	</div>
</body>
</html>
```

**注意：** *因为负值得关系calc中的符号和数值之间一定要有空格（*和/可以没有，但推荐使用），否则浏览器不识别*


##### 二、利用`float`或`position`和`margin`值来布局


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
			height: 400px;
			overflow: hidden;
		}
		.left{
			width: 200px;
			height: 100%;
			float: left;
			/*position: absolute;这里也可以设置position不设置float*/
			background: #434edd;
		}
		.right{
			margin-left: 200px;
			height: 100%;
			background: #cd43da;
		}
	</style>

</head>
<body>
	<div class="container">
		<div class="left"></div>
		<div class="right"></div>
	</div>
</body>
</html>
```

这种方法利用了元素设置了`float`和`postion`属性后会脱离文档流的特性。

##### 三、利用flex在进行布局

> 这种方法可能会有一些浏览器兼容性的问题

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
			height: 400px;
			display: flex;
		}
		.left{
			width: 200px;
			height: 100%;
			background: #434edd;
		}
		.right{
			height: 100%;
			width: 100%;
			background: #cd43da;
		}
	</style>

</head>
<body>
	<div class="container">
		<div class="left"></div>
		<div class="right"></div>
	</div>
</body>
</html>
```

##### 四、利用table进行布局

> `display:table;`的特性是所有单元格的宽度之和等于表格的宽度，所以将表格宽度设置100%，一列固定，另一列就和自适应布局了。见代码：

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
			height: 400px;
			display: table;
			width: 100%;
		}
		.left{
			width: 200px;
			height: 100%;
			display:table-cell;
			background: #434edd;
		}
		.right{
			display: table-cell;
			height: 100%;
			background: #cd43da;
		}
	</style>

</head>
<body>
	<div class="container">
		<div class="left"></div>
		<div class="right"></div>
	</div>
</body>
</html>
```


### 头尾固定高度中间自适应高度布局

> 这种布局是一种常见的后台管理系统布局。一般常用的方法：（1）用`position:absolute`设置`top` `bottom`然后上下高度固定。(2)利用`boxsizing`来布局。这种方法的好处主要是可以兼容到IE6，但是我们这里不考虑IE6了，所以这里不做详细介绍，有兴趣的可以查看[这里](http://www.cnblogs.com/pigtail/archive/2012/11/25/2787508.html)（3）利用js来布局

这里我们只列出第一种的代码示例：

```
<!DOCTYPE html>
<html>
<head>
	<title></title>
	<style type="text/css">
		html,body{
			padding: 0;
			margin: 0;	
			height: 100%
		}
		.container{
			height: 100%;
			position: relative;
			overflow: hidden;
		}
		.top{
			position: absolute;
			height: 200px;
			width: 100%;
			top: 0;
			background: #434edd;
		}
		.bottom{
			position: absolute;
			height: 200px;
			width: 100%;
			bottom: 0;
			background: #cd43da;
		}
		.center{
			position: absolute;
			top: 200px;
			width: 100%;
			bottom: 200px;
			background: #8b8e79;
		}
	</style>

</head>
<body>
	<div class="container">
		<div class="top"></div>
		<div class="center"></div>
		<div class="bottom"></div>
	</div>
</body>
</html>
```


