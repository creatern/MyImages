# JavaWeb 概述

![](C:/NoteBook/pictures/26080321227450.png =709x)



![](C:/NoteBook/pictures/Snipaste_2022-11-17_01-43-35.png =600x)

# HTML

- HTML是解释型的标记语言，不区分大小写
- 浏览器是容错的
- 所有和设置页面，页面美观的标签/属性都已经被淘汰。
   - 被CSS取代 

## 基本标签

**注解**

- `<!--` 注解开头
- `-->` 注解结尾

**页面**

- html页面中由一对标签组成：`<html></html>`
  `<html>` 开始标签
  `</html> `结束标签

**网页标签**

- `<head></head>`

**表示网页的标题**

- `<title><title/>` 

**设置编码格式**

- `<meta>` 
    - charset 编码集

**表示换行**

- `<br/>` 
    - br标签是一个单标签。开始和结束标签是同一个，斜杠放在单词后面

**表示段落标签**

- `<p></p>` 

**表示图片标签**

- `<img/> `
    - src 图片文件的路径
    - width height 设置图片的大小缩放，注意看图片的分辨率，防止图片变形
    - alt 图片的提示，图片显示失败后，在图片位置显示的文字

**标题**

- `<h></h>` 标题 h1~h6
   - 没有h7,最多h6

**列表标签**

- `<ol><ol/>` 有序列表
   - start 表示从显示类型的第n个开始
   - type 显示的类型 A、a、I、i、1
      - A、a 26个字母
      - I、i 罗马字母
      - 1 数字，默认
- `<ul></ul>` 无序列表
   - type 显示的类型 disc、square
   - disc 实心圆点
      - square 实心方块
      - circle 空心圆点
- `<li></li>` 列表内的每一列

**字体** 可嵌套，注意嵌套的内外层需要一致

- `<b></b>` 字体加粗
- `<i></i>` 字体倾斜
- `<u></u> `下划线

**上下标**

- `<sub></sub>` 下标
- `<sup></sup> `上标

**符号** HTML字符实体

- `&gt;` >
- `&lt;` <
- `&ge; `>=
- `&le;` <=
- `&reg; `注册商标®
- `&copy; `版权符号©
- `&nbsp;` 空格

**独立修饰**

- `<span></span> `不换行的块标记，表示要独立修饰

**超链接**

- `<a></a> `
    - href 链接的地址
    - target 窗口打开的方式
        - _self 在本窗口打开，默认
        - _blank 打开一个新的标签页
        - _parent 在父窗口打开
        - _top 在顶层窗口打开
        
![](C:/NoteBook/pictures/Snipaste_2022-11-17_02-00-52.png =600x)

**层**

- `<div></div>` 

**路径的问题**

- 绝对路径 从盘符开始，从协议开始
- 相对路径 从该HTML文件所在位置开始

```html
<html>
	<head>
		<title>我的第一个网页</title>
		<meta charset="UTF-8"> 
	</head>
	<body>
		Hello World ! <br/> 你好 世界 !
		<p> 第一段落 </P>
		<P> 第二段落 </p>
		<img src="G:\\HTML\\images\\th.jpg" width="500" height="260" alt="剪短你的网线"/>
		<h1>标题一</h1>
		<h2>标题二</h2>
		<h3>标题三</h3>
		<h4>标题四</h4>
		<h5>标题五</h5>
		<h6>标题六</h6>

		武林高手排行榜
		<ol type="i" start="3">
			<li>扫地僧</li>
			<li>萧远山</li>
			<li>慕容博</li>
			<li>虚竹</li>
			<li>阿紫</li>
		</ol>

		武林大会成员名单
		<ul type="circle">
			<li>路人甲</li>
			<li>路人乙</li>
			<li>龙套丙</li>
		</ul>

		你是喜欢<b>甜</b>月饼还是<i>咸</i><b><i><u>月饼</u></i></b>

		<br/>
		
		水分子的化学式：H<sub>2</sub>O<br/>
		2的n次方 2<sup>n</sup><br/>

		5&lt;10<br/>
		5&le;10<br/>
		&reg;<br/>
		&copy;<br/>

		<span>emmm</span>

		<a href="http://www.baidu.com" target="_blank">百度</a>
	</body>
</html>
```

```html
<html>
	<head>
		<title>我的第一个网页</title>
		<meta charset="UTF-8"> 
		<style type="text/css">
			div{
				width:200px;
				height:200px;
				position:absolute;
			}

			#div1{
				background-color:#AF8;
			}
			#div2{
				background-color:#BCA;
				left:100px;
				top:100px;
			}
			#div3{
				background-color:#FF8;
				left:200px;
				top:200px;
			}
		</style>
	</head>
	<body>
		<div id="div1">div1</div>
		<div id="div2">div2</div>
		<div id="div3">div3</div>
	</body>
</html>
```

## 表格 `<table></table>`

**表格**

- `<table></table>` 
  - 已淘汰的属性：
     - borde 表格边框的粗细
     - width 表格的宽度
     - cellspacing 单元格间隙
     - cellpadding 单元格填充

**表头**

- `<th></th>`

**行**
 
- `<tr></tr>`

**列**
 
- `<td></td> `
   - rowspan 跨行合并 （需要保证合并的下一行的对应单元格上没有数据）
   - colspan 跨列合并
   - 已淘汰的属性：
      - align 居中

```html
<html>
	<head>
		<title>我的网页</title>
		<meta charset="UTF-8">
		<style type="text/css"></style>
	</head>
	<body>
		<table border="1" width="400" cellspacing="0" cellpadding="">
			<tr>
				<th>姓名</th>
				<th>门派</th>
				<th>绝技</th>
				<th>内功</th>
			</tr>
			<tr align="center">
				<td>乔峰</td>
				<td>丐帮</td>
				<td>降龙十八掌</td>
				<td>1000</td>
			</tr>
			<tr align="center">
				<td>虚竹</td>
				<td>灵柩</td>
				<td>北冥</td>
				<td>800</td>
			</tr>
			<tr align="center">
				<td>扫地僧</td>
				<td>少林寺</td>
				<td>七十二绝技</td>
				<td>???</td>
			</tr>
		</table>

		<hr/>

		<table border="1" cellspacing="0" cellpadding="4" width="600">
			<tr>
				<th>名称</th>
				<th>单价</th>
				<th>数量</th>
				<th>小计</th>
				<th>操作</th>
			</tr>
			<tr align="center">
				<td>苹果</td>
				<td rowspan="2">5</td>
				<td>20</td>
				<td>100</td>
				<td><img src="G:\\HTML\\images\\垃圾桶.png" width="20" height="25"/></td>
			</tr>
			<tr align="center">
				<td>菠萝</td>
				<td>40</td>
				<td>200</td>
				<td><img src="G:\\HTML\\images\\垃圾桶.png" width="20" height="25"/></td>
			</tr>
			<tr align="center">
				<td>香蕉</td>
				<td>2</td>
				<td>10</td>
				<td>20</td>
				<td><img src="G:\\HTML\\images\\垃圾桶.png" width="20" height="25"/></td>
			</tr>
			<tr>
				<td>总计</td>
				<td colspan="4">320</td>
			</tr>
		</table>
	</body>
</html>
```

## 表单标签 `<form></form>`

- 表单就是一个容器，

![](C:/NoteBook/pictures/Snipaste_2022-11-17_20-14-04.png =600x)

当属性名和属性值相同是，属性值可以省略

**表单**

- `<form></form>`
   - action 用户在表单里填写的信息需要发送到服务器端，对于Java项目来说就是交给Java代码来处理。那么在页面上我们就必须正确填写服务器端的能够接收表单数据的地址。这个地址要写在form标签的action属性中。不写则仍然是提交到当前页面
   - method 用来定义提交表单的『请求方式』。method属性只有两个可选值：get或post
      - get 默认，信息都显示在地址栏，且最多2KB
      - post 

### 表单的下级标签

**通用属性**

- name 必须要指定，否则这个文本框的数据将来是不会发送给服务端的。
    - 建议在同一系列的单选框和复选框保持相同的name 
    - 需要name的：与同级的标签：`<input/>``<select></select>``<textarea></textarea>`
    - 以下显示的都是name具有属性的，没有name属性的不显示（不发送）
![](C:/NoteBook/pictures/Snipaste_2022-11-17_21-08-32.png =400x)

- value 发送给服务器的值 
   - `<textarea></textarea>`的value值是开始和结束标签之间的区域。 

**输入 `<input/>`**

- `<input type="text"/>` 文本框
- `<input type="password">` 密码框
- `<input type="radio">` 单选框
     - name的属性值需要保持一致，这样才会有互斥的效果
     - 可以通过checked属性设置默认选中的项
- `<input type="checkbox">` 复选框
     - name的属性值建议保持一致，以便将来获取数组
     - 可以通过checked属性设置默认选中的项
- `<input type="submit">` 提交
     - 通过form标签的action属性
- `<input type="reset">` 重置
    - reset 重置 恢复到默认状态，不等于清空
- `<input type="button">` 普通按钮 
    - 需要通过JavaScript绑定单击响应函数
- `<input type="hidden" name="userId" value="2233"/>` 表单隐藏域
    - 通过表单隐藏域设置的表单项不会显示到页面上，用户看不到。但是提交表单时会一起被提交。用来设置一些需要和表单一起提交但是不希望用户看到的数据


**下拉选项 `<select></select>`**

- `<select></select>`  
   - selected 默认选中 
- `<option></option>` 选项

**多行文本框/文本域 `<textarea></textarea>`**

- `<textarea></textarea>` 
    - rows 默认显示n行
    - cols 默认显示n列

- 其value值就是开始`<textarea>`和结束`</textarea>`标签之间的内容

```html
<html>
	<head>
		<title>我的网页</title>
		<meta charset="UTF-8">
		<style type="text/css"></style>
	</head>
	<body>
		<form action="demo04.html">
			昵称：<input type="text" name="nicName"/><br/><br/>
			密码：<input type="password" name="pwd"/><br><br/>
			性别：<input type="radio" name="gender" value="male"/> 男
				  <input type="radio" name="gender" value="female" checked/> 女<br/><br/>
			爱好：<input type="checkbox" name="hobby" value="basketball"/> 篮球
	         	  <input type="checkbox" name="hobby" value="football"/> 足球
				  <input type="checkbox" name="hobby" value="ping-pong"/> 乒乓球 <br/><br/>
			星座：<select name="start">
					<option value="1">据星</option>
					<option value="2" selected>明星</option>
					<option value="3">后星</option>
			      </select><br/><br/>
		    备注：<textarea name="remark" rows="4" cols="50"></textarea><br/><br/>
			<input type="submit" value="注册"/>
			<input type="reset" value="重置"/>
			<input type="button" value="普通按钮"/>
		</form>
	</body>
</html>
```

## frame和iframe

## 框架 `	<frameset></frameset>`

- 已淘汰的标签
- 替换了body标签，即：使用frame时，不能有body标签

```html
<html>
	<head>
		
	</head>
	<frameset rows="20%,*">  <!--frameborder="no"-->
		<frame src="demo05/top.html"/>
		<frameset cols="15%,*">
			<frame src="demo05/left.html"/>
			<frameset rows="80%,*">
				<frame src="demo05/main.html"/>
				<frame src="demo05/botton.html"/>
			</frameset>
		</frameset>
	</frameset>
</html>
```

## 嵌入子页面 `<iframe/>`

```html
<html>
	<head>
		<meta charset="UTF-8">
	</head>
	<body>
		demo06页面内容
		<iframe src="demo05/top.html"/>
	</body>
</html>
```

# CSS

## CSS代码语法

- CSS样式由选择器和声明组成
- 声明必须放在`{}`内，并且声明可以是多条。
- 每条声明由一对属性和值组成，属性和值之间用冒号`:`分开，每条声明以分号`;`结尾
- 使用`/* */`声明注释。

**选择器**

1. 标签样式表 标签
2. 类样式表 class属性 `.类名`
3. ID样式表 id属性 `#ID名`

![](c:/notebook/pictures/Snipaste_2022-11-18_15-59-57.png =400x)

- 组合样式表

## CSS从位置上的分类

1. 嵌入式样式表(行内样式表)  
     - `style`作为标签内属性
     - `<p><span style="font-size:60px;font-weight:bolder;color:yellow";>HELLO</span></p>`
2. 内部样式表   
     - `<style></style>`作为`<head></head>`内部标签区域
3. **外部样式表** 重点
- 就近原则：如果有相同的样式设置，采用就近的

```html
<html>
	<head>
		<meta charset="UTF-8">
		<!--内部样式表-->
		<style type="text/css">
			/* 被style标签包围的是CSS环境 在这里编写CSS代码 */

			/* 标签样式 */
			p{
				color:red;
			}

			/* . class 类样式 */
			.f20{
				font-size:20px;
			}

			/* # id ID样式 */
			/* id属性在整个html中尽量唯一，虽然重复不报错*/
			#p4{
				background-color:pink;
				font-size:24px;
				font-weight:bolder;
				font-style:italic;
				font-family:"华文彩云";
			}

			/* 组合样式*/
			div p{
				color:blue;
			}

			div .f32{
				font-size:32px;
				font-family:"黑体";
			}
		</style>
	</head>
	<body>
		<p>段落1</p>
		<p>段落2</p>
		<p class="f20">段落3</p>
		<p id="p4">段落4</p>

		<div>
			<!--嵌入式样式表-->
			<p><span style="font-size:60px;font-weight:bolder;color:yellow";>HELLO</span></p>
			<span class="f32">WORLD </span>
			<p class="f32">!!!</p>
		</div>
	</body>
</html>
```

## CSS文件 外部样式表 

- 在CSS文件中不需要`<style>`标签，直接编写样式即可
     
```css
/* # id ID样式 */
/* id属性在整个html中尽量唯一，虽然重复不报错*/
#p4{
	background-color:pink;
	font-size:24px;
	font-weight:bolder;
	font-style:italic;
	font-family:"华文彩云";
}

/* 组合样式*/
div p{
	color:blue;
}

div .f32{
	font-size:32px;
	font-family:"黑体";
}
```

### 1. 链接外部表
     
-  在`<head></head>`标签区域内部: `<link/>`
    - `<link rel="stylesheet" href="CSS文件路径" type="text/css"/>`
        - `rel="stylesheet"` 在这个页面中使用这个外部样式表
        - `href` CSS文件路径 
        - `type="text/css"` 指定为样式表文件 (可省略)
  
**html文件**

```html
<html>
	<head>
		<meta charset="UTF-8">
		<style type="text/css">
			/* 被style标签包围的是CSS环境 在这里编写CSS代码 */

			/* 标签样式 */
			p{
				color:red;
			}

			/* . class 类样式 */
			.f20{
				font-size:20px;
			}
		</style>
		<link rel="stylesheet" href="CSS/demo07.css"/>
	</head>
	<body>
		<p>段落1</p>
		<p>段落2</p>
		<p class="f20">段落3</p>
		<p id="p4">段落4</p>

		<div>
			<p><span>HELLO</span></p>
			<span class="f32">WORLD </span>
			<p class="f32">!!!</p>
		</div>
	</body>
</html>
```

**CSS文件**

```css
/* # id ID样式 */
/* id属性在整个html中尽量唯一，虽然重复不报错*/
#p4{
	background-color:pink;
	font-size:24px;
	font-weight:bolder;
	font-style:italic;
	font-family:"华文彩云";
}

/* 组合样式*/
div p{
	color:blue;
}

div .f32{
	font-size:32px;
	font-family:"黑体";
}
```

### 2. 导入外部表

- `@import url("CSS文件路径")`
- 必须是在`<head></head>`内的`<style></style>`中导入

```html
<html>
    <head>
        <style>
            @import url("G:/html/css/demo07.css")
        </style>
    </head>
    <body></body>
</html>
```

### 对比链接和导入
  
1. `<link/>`标签属于XHTML范筹，而`@import` 是CSS2.1中独有的。
2. 使用`<link/>`标签链接，在客户端浏览网页时，先加载CSS文件，再加载HTML文件。
3. 使用`@import`导入，在客户端浏览网页时，先加载HTML文件，再加载CSS文件，当网速慢时，可能会先展示没有经过CSS文件渲染的原始HTML页面。

## CSS盒子模型

- border 边框
- margin 间隙
- padding 填充

```html
<html>
	<head>
		<meta charset="utf-8">
		<style type="text/css">
			#div1{
				width:400px;
				height:400px;
				background-color:greenyellow;

				/* border 边框样式*/
				
				/*border-width:4px;*/    /*边框粗细*/
				/*border-style:solid;*/  /*边框样式 solid-实线 dotted-点状线*/
				/*border-color:blue;*/   /*边框颜色*/

				border:4px double blue;

				/*border-top:4px dotted blue;*/
			}
			#div2{
				width:200px;
				height:200px;
				background-color:red;

				
				margin-top:100px;
				margin-left:100px;
				
				/*margin:100px 100px 50px 150px;*/ 
				/*提供值的个数 1: 四个方向统一 2:(上下)(左右) 3:上(左右)下 4:上右下左*/

				/*padding 填充*/
				/*padding-top:50px;
				padding-left:50px;*/
			}
			#div3{
				width:100px;
				height:100px;
				background-color:orange;
				margin:50px; /*以div1为参照物*/
			}
			body{
				margin:0px;
				padding:0px;
			}
		</style>
	</head>
	<body>
		<div id="div1">
			<div id="div2">
				<div id="div3">&nbsp;</div>
			</div>
		</div>
	</body>
</html>
```

## CSS布局

- position 定位
   - absolute 绝对定位，需要配合使用left、top
   - relative 相对定位，一般会和float、margin、padding ... 一起使用


```html
<html>
	<head>
		<meta charset="utf-8">
		<style>
			#div1{
				width:200px;
				height:50px;
				background-color:greenyellow;

				/*绝对定位*/
				position:absolute;
				left:100px;
				top:100px;
			}

			#div2{
				width:200px;
				height:50px;
				background-color:yellow;
				
				/*相对定位*/
				position:relative;
				float:left;
				margin-left:20px;
			}
			#div3{
				height:50px;
				background-color:pink;
			}
			#div4{
				width:200px;
				height:50px;
				background-color:blue;
				float:left;
			}
			#div5{
				width:200px;
				height:50px;
				background-color:black;
				float:left;
			}
			div{
				position:relative;
			}
		</style>
	</head>
	<body>
		<div id="div1">&nbsp;</div>
		<div id="div2">&nbsp;</div>
		<div id="div3">
			<div id="div4">&nbsp;</div>
			<div id="div5">&nbsp;</div>
		</div>
	</body>
</html>
```

# Javascript

- 解释型的脚本语言
   - 不会产生编译出来的字节码文件，而是在程序的运行过程中对源文件逐行进行解释。
- 基于对象
   - 基于对象的脚本语，能够实现封装，可以模拟继承，不支持多态
- 弱类型语言
  - 变量的数据类型由所赋的值决定
  - 有明确的数据类型，但声明一个变量后它可以接收任何类型的数据，并且会在程序执行过程中根据上下文自动转换类型。
- 采用事件驱动的脚本语言，它不需要经过Web服务器就可以对用户的输入做出响应
- 不依赖于操作系统，仅需要浏览器的支持
  
## HTML中的使用

### HTML文档内

- JavaScript代码要写在script标签内`<script type="text/javascript"></script>`
- script标签可以写在文档内的任意位置
- 为了能够方便查询或操作HTML标签（元素）script标签可以写在body标签后面

```html
<html>
    <head>
        <script type="text/javascript">
            <!--
               javascript语句;        
            -->
        </script>
    </head>
    <body></body>
</html>
```

### 使用外部JS文件

- 在script标签内通过src属性指定外部xxx.js文件的路径即可。
**注意**

1. 引用外部JavaScript文件的script标签里面不能写JavaScript代码
2. 引用外部JavaScript文件的script标签不能改成单标签
3. 外部JavaScript文件一定要先引入再使用

```html
<html>
    <head>
        <script type="text/javascript" src="JS文件路径">
            <!--
               javascript语句;        
            -->
        </script>
    </head>
    <body></body>
</html>
```

```html
<body>
</body>

<!-- 引入外部JS文件 -->
<script src="/pro02-JavaScript/scripts/outter.js" type="text/javascript" charset="utf-8"></script>
<!-- HTML内部的JS -->
<script type="text/javascript">
	// 调用外部JavaScript文件中声明的方法
	showMessage();
</script>
```

## 数据类型和变量

### 数据类型

**基本数据类型**

- number 数值型：JavaScript不区分整数、小数
- string 字符串：JavaScript不区分字符、字符串；单引号、双引号意思一样。
- boolean 布尔型：true、false
    - 在JavaScript中，其他类型和布尔类型的自动转换。
    - true：非零的数值，非空字符串，非空对象
    - false：零，空字符串，null，undefined
- null 空
- undefined 未定义

**引用类型**

- 所有new出来的对象
- 用[]声明的数组
- 用{}声明的对象

### 变量

- 关键字：var
- 数据类型：JavaScript变量可以接收任意类型的数据
- 标识符：严格区分大小写
- 可以不声明变量类型，但使用时必须声明（即：被赋值）

**变量使用规则**

- 如果使用了一个没有声明的变量，那么会在运行时报错 Uncaught ReferenceError: b is not defined
- 如果声明一个变量没有初始化，那么这个变量的值就是undefined

```html
<html>
	<head>
		<meta charset="utf-8">
		<script language="javascript">
			var str = "hello Javascript";
			alert(typeof str);
			str = 9999;
			alert(typeof str);
		</script>
	</head>
	<body></body>
</html>
```

## 函数

- 函数本身就是一个对象

### 系统函数

- parseInt()
- parseFloat()
- isNaN() 判断非数字
- eval() 计算表达式的值

### 自定义函数

#### 声明函数

```javascript
function 函数名(参数1, 参数2) {
    ...语句;
	return 返回值;
}
```

#### 调用函数

1. 直接调用

```javascript
var 变量 = 函数名(参数1, 参数2);
```

2. 事件调用

```javascript
事件名 = "函数名()"
```

3. 直接使用

```javascript
函数名(参数);
```

#### 匿名函数

- 声明一个函数，相当于创建了一个函数对象，将这个对象的引用赋值给变量。

```javascript
var 变量 = function(参数1, 参数2) {
    ...语句;
	return 返回值;
};
```

## 对象

- JavaScript中没有『类』的概念，对于系统内置的对象可以直接创建使用。

### 创建对象

#### 使用new关键字创建对象

- 也可以省略new关键字

```javascript
var 实例 = new 对象名(参数);
```

```javascript
//创建对象
var obj = new Object();
//控制台输出
console.log(obj);
```

#### 使用{}创建对象

- 在创建对象的同时，设置对象属性

```javascript
var 对象名{
    "对象属性1":"值1",
    ...
    "对象属性n":"值n" //最后一个不要逗号 ,
};
```

```javascript
// 创建对象
var obj02 = {
    "soldierName":"john",
    "soldierAge":35,
    "soldierWeapon":"gun"
};

console.log(obj02);
```

### 设置对象属性

**普通属性**

```javascript
对象.属性 = "值";
```

**函数属性**

- 可以通过对象的函数属性调用函数 ：`对象名.函数属性(参数)`

```javascript
对象.函数属性 = 函数名 | 匿名函数 ;
```

1. new关键字创建

```javascript
obj = new Object();

obj.name = "Tom";
obj.age = "12";
obj.sex = "男";
obj.show = function () {
    console.log(this.sex);
};

//控制台输出对象
console.log(obj);
//调用对象的函数属性
obj.show();
```

2. {}创建

```javascript
obj = {
    "name": "Tom",
    "age": 12,
    "sex": "男",
    "show": function () {
        console.log(this.sex);
    }
}

//控制台输出
console.log(obj);
//调用obj对象的函数属性
obj.show();
```

### this关键字

**this关键字只有两种情况：**

- 在函数外面：this关键字指向window对象（代表当前浏览器窗口）
- 在函数里面：this关键字指向调用函数的对象

### 系统对象

| 对象    | 说明         |
| :----- | :---------- |
| Date   | 日期时间处理 |
| Array  | 数组处理     |
| String | 字符串处理   |
| Math   | 数学处理     |

#### Date对象

**创建**

1. 指定日期时间

```javascript
var 日期实例 = new Date("MM DD,YYYY,hh:mm:ss);
```

2. 当前日期时间

```javascript
var 日期实例 = new Date();
```

**常用方法**

| 方法                | 说明                                              |
| :----------------- | :------------------------------------------------ |
| getDate()          | 	返回月中的第几天（从 1 到 31）。                   |
| getDay()           | 	返回星期几（0-6）。                               |
| getFullYear()      | 	返回年份。                                        |
| getHours()	     | 返回小时（从 0-23）。                              |
| getMilliseconds()	 | 返回毫秒（0-999）。                                |
| getMinutes()	     | 返回分钟（从 0-59）。                              |
| getMonth()	     | 返回月份（从 0-11）。                              |
| getSeconds()	     | 返回秒数（从 0-59）。                              |
| getTime()	         | 返回自 1970 年 1 月 1 日午夜以来与指定日期的毫秒数。 |

```javascript
//当前日期时间
var toDay = new Date();
console.log(toDay); //2022-11-22T11:01:18.711Z
//指定日期时间
var myDay = new Date("12 31,2001,12:12:12");
console.log(myDay);

//getDate()
console.log(myDay.getDate()); //2001-12-31T04:12:12.000Z
//getDay()
console.log(myDay.getDay()); //31
//getFullYear()
console.log(myDay.getFullYear()); //1
//getHours()
console.log(myDay.getHours()); //2001
//getMilliseconds()
console.log(myDay.getMilliseconds()); //0
//getMonth()
console.log(myDay.getMonth()); //11
//getSeconds()
console.log(myDay.getSeconds()); //12
//getTime()
console.log(myDay.getTime()); //1009771932000
```

##### 定时函数

| 函数                                    | 说明                         |
| :------------------------------------- | :--------------------------- |
| setTimeout("要执行的语句",时间)          | 在指定毫秒时间后执行          |
| clearTimeout(timeoutID)                | 清除定时设置                  |
| setInterval("要执行的语句",周期间隔时间) | 按照指定的周期（毫秒时间）执行 |
| clearInterval()                        | 清除定时设置                  |

```javascript
function myShowTime(){  
    console.log(new Date());
}

function myRun(){
    console.log("等待3秒");    
    setTimeout(3000);
    myShowTime();
    console.log("结束");
}

var counter = 0;

function myCycle(){
    setInterval(1000);
    console.log("开始" + ++counter + "次");
    myShowTime();
}

myRun();
myCycle();
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>JavaScript</title>
</head>
<body>
    <p>当前时间为：<span id="clock"></span></p>
    <button onclick="stopClock(this);">停止</button><hr>
    <button onclick="delayedAlert(this);">2秒后弹出一个提示框</button>
    <button onclick="clearAlert();">取消</button>
    <script type="text/javascript">
        var intervalID;
        function showTime() {
            var d = new Date();
            document.getElementById("clock").innerHTML = d.toLocaleTimeString();
        }

        function stopClock(e) {
            clearInterval(intervalID);
            e.setAttribute('disabled', true)
        }
        intervalID = setInterval(showTime, 1000);

        var timeoutID;
        var that;
        function delayedAlert(e) {
            that = e;
            timeoutID = setTimeout(showAlert, 2000);
            e.setAttribute('disabled', true)
        }
        function showAlert() {
            alert('这是一个提示框。');
            that.removeAttribute('disabled');
        }
        function clearAlert() {
            clearTimeout(timeoutID);
            that.removeAttribute('disabled');
        }
    </script>
</body>
</html>
```

## 数组

### 创建数组

#### new关键字 Array对象

```javascript
var 数组名 = new Array();
```

```javascript
// 1.创建数组对象
var arr01 = new Array();
```

#### []创建

```javascript
var 数组名 = [值1,值2,...];
```

```javascript
var arr = ["cat","dog","tiger"];
console.log(arr);
```

### 数组方法

| 方法            | 说明                                          |
| :-------------- | :-------------------------------------------- |
| push(value)    | 向数组中压入value值                            |
| reverse()       | 数组元素反序                                   |
| join("分隔符")  | 数组元素拼接成字符串，每个元素之间以分隔符连接    |
| split("分隔符") | 以分隔符将字符串拆分为数组                      |
| pop()           | （删除）弹出数组中的最后一个元素，同时返回该元素  |

```javascript
arr01 = new Array();

// 压入数据
arr01.push("apple");
arr01.push("orange");
arr01.push("banana");
arr01.push("grape");

// for循环 遍历数组
for (var i = 0; i < arr01.length; i++) {
    console.log(arr01[i]);
}

// 数组元素反序
arr01.reverse();
for (var i = 0; i < arr01.length; i++) {
    console.log(arr01[i]);
}

// 数组元素拼接成字符串
var arrStr = arr01.join(",");
console.log(arrStr);

// 字符串拆分成数组
var arr02 = arrStr.split(",");
for (var i = 0; i < arr02.length; i++) {
    console.log(arr02[i]);
}

// 弹出数组中最后一个元素
var ele = arr01.pop();
console.log(ele);
console.log(arr01);
```

## BOM 浏览器对象模型

![](c:/notebook/pictures/Snipaste_2022-11-22_14-58-35.png =600x)

**主要包含的对象**

| 对象      | 说明     |
| :------- | :------- |
| window   | 整个窗口 |
| history  | 历史页面 |
| location | 地址栏   |
| document | 网页内容 |

### window对象

**属性**

| 属性           | 说明                                                 |
| :------------ | :-------------------------------------------------- |
| closed        | 返回窗口是否已被关闭。                                 |
| defaultStatus | 设置或返回窗口状态栏中的默认文本。                      |
| document      | 对 Document 对象的只读引用。请参阅 Document 对象。     |
| history       | 对 History 对象的只读引用。请参数 History 对象。       |
| innerheight   | 返回窗口的文档显示区的高度。                           |
| innerwidth    | 返回窗口的文档显示区的宽度。                           |
| length        | 设置或返回窗口中的框架数量。                           |
| location      | 用于窗口或框架的 Location 对象。请参阅 Location 对象。  |
| name          | 设置或返回窗口的名称。                                 |
| Navigator     | 对 Navigator 对象的只读引用。请参数 Navigator 对象。   |
| opener        | 返回对创建此窗口的窗口的引用。                         |
| outerheight   | 返回窗口的外部高度。                                  |
| outerwidth    | 返回窗口的外部宽度。                                  |
| pageXOffset   | 设置或返回当前页面相对于窗口显示区左上角的 X 位置。      |
| pageYOffset   | 设置或返回当前页面相对于窗口显示区左上角的 Y 位置。      |
| parent        | 返回父窗口。                                          |
| Screen        | 对 Screen 对象的只读引用。请参数 Screen 对象。         |
| self          | 返回对当前窗口的引用。等价于 Window 属性。              |
| status        | 设置窗口状态栏的文本。                                 |
| top           | 返回最顶层的先辈窗口。                                 |
| window        | window 属性等价于 self 属性，它包含了对窗口自身的引用。 |

- screenXxx只读整数。声明了窗口的左上角在屏幕上的的 x 坐标和 y 坐标
    - IE、Safari 和 Opera 支持 screenLeft 和 screenTop
    - 而 Firefox 和 Safari 支持 screenX 和 screenY

**方法**

| 方法    | 说明    |
| :-- | :-- |
|alert() |显示带有一段消息和一个确认按钮的警告框。|
|blur() | 把键盘焦点从顶层窗口移开。|
|clearInterval() |取消由 setInterval() 设置的 timeout。|
|clearTimeout()  |取消由 setTimeout() 方法设置的 timeout。|
|close() |关闭浏览器窗口。|
|confirm()   |显示带有一段消息以及确认按钮和取消按钮的对话框。|
|createPopup()  | 创建一个 pop-up 窗口。|
|focus()| 把键盘焦点给予一个窗口。|
|moveBy()    |可相对窗口的当前坐标把它移动指定的像素。|
|moveTo()   | 把窗口的左上角移动到一个指定的坐标。|
|open() | 打开一个新的浏览器窗口或查找一个已命名的窗口。|
|print() |打印当前窗口的内容。|
|prompt() |  显示可提示用户输入的对话框。|
|resizeBy() | 按照指定的像素调整窗口的大小。|
|resizeTo() | 把窗口的大小调整到指定的宽度和高度。|
|scrollBy() | 按照指定的像素值来滚动内容。|
|scrollTo() | 把内容滚动到指定的坐标。|
|setInterval()  | 按照指定的周期（以毫秒计）来调用函数或计算表达式。|
|setTimeout() |   在指定的毫秒数后调用函数或计算表达式。|

#### 事件

| 事件         | 说明                     |
| :---------- | :---------------------- |
| onload      | 一个页面/一副图像完成加载 |
| onmouseover | 鼠标指针移到某元素上      |
| onclick     | 鼠标点击                 |
| onmouseout  | 鼠标移开                 |
| onkeydown   | 某个键盘按键             |
| onchange    | 域的内容改变             |

![](c:/notebook/pictures/Snipaste_2022-11-22_21-34-58.png =900x)

**例1**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!--HTML内部-->
<button id="test" onclick="myShow()"> 按钮2</button>
<!--外部JS文件-->
<button id="testId" onclick="changeContent()"> 按钮1</button>
<script type="text/javascript" charset="UTF-8" src="EventTest01.js"></script>
<script type="text/javascript">
    function myShow(){
        alert("按钮2");
    }
</script>
</body>

</html>
```

```javascript
function changeContent() {
    window.alert("按钮1");
}

alert("成功导入");
```

**例2**

```html
<html>
	<head>
		<meta charset="utf-8">
		<link rel="stylesheet" href="css/demo05.css">
		<script type="text/javascript" src="js/demo08.js"></script>
	</head>
	<body>
		<div id="div_container">
			<div id="div_fruit_list">
				<table id="tbl_fruit">
					<tr>
						<th class="w20">名称</th>
						<th class="w20">单价</th>
						<th class="w20">数量</th>
						<th class="w20">小计</th>
						<th>操作</th>
					</tr>
					<tr>
						<td>苹果</td>
						<td>5</td>
						<td>20</td>
						<td>100</td>
						<td><img src="imgs/del.jpg" class="delImg"/></td>
					</tr>
					<tr>	
						<td>西瓜</td>
						<td>3</td>
						<td>20</td>
						<td>60</td>
						<td><img src="imgs/del.jpg" class="delImg"/></td>
					</tr>
					<tr>
						<td>菠萝</td>
						<td>4</td>
						<td>25</td>
						<td>100</td>
						<td><img src="imgs/del.jpg" class="delImg"/></td>
					</tr>
					<tr>
						<td>榴莲</td>
						<td>3</td>
						<td>30</td>
						<td>90</td>
						<td><img src="imgs/del.jpg" class="delImg"/></td>
					</tr>
					<tr>
						<td>总计</td>
						<td colspan="4">999</td>
					</tr>
				</table>
			</div>
		</div>
	</body>
</html>
```

```javascript
window.onload=function(){
	updateZJ();
	//当页面加载完成，我们需要绑定各种事件
	//根据id获取到表格
	var fruitTbl =  document.getElementById("tbl_fruit");
	//获取表格中的所有的行
	var rows = fruitTbl.rows ;
	for(var i = 1 ; i<rows.length-1 ; i++){
		var tr = rows[i];
		//1.绑定鼠标悬浮以及离开时设置背景颜色事件
		tr.onmouseover=showBGColor;
		tr.onmouseout=clearBGColor;
		//获取tr这一行的所有单元格
		var cells = tr.cells;
		var priceTD = cells[1];
		//2.绑定鼠标悬浮在单价单元格变手势的事件
		priceTD.onmouseover = showHand ;
		//3.绑定鼠标点击单价单元格的事件
		priceTD.onclick=editPrice;
	}

}

//当鼠标点击单价单元格时进行价格编辑
function editPrice(){
	if(event && event.srcElement && event.srcElement.tagName=="TD"){
		var priceTD = event.srcElement ;
		//目的是判断当前priceTD有子节点，而且第一个子节点是文本节点 ， TextNode对应的是3  ElementNode对应的是1
		if(priceTD.firstChild && priceTD.firstChild.nodeType==3){
			//innerText 表示设置或者获取当前节点的内部文本
			var oldPrice = priceTD.innerText ;
			//innerHTML 表示设置当前节点的内部HTML
			priceTD.innerHTML="<input type='text' size='4'/>";
			// <td><input type='text' size='4'/></td>
			var input = priceTD.firstChild;
			if(input.tagName=="INPUT"){
				input.value = oldPrice ;
				//选中输入框内部的文本
				input.select();
				//4.绑定输入框失去焦点事件 , 失去焦点，更新单价
				input.onblur=updatePrice ;
			}
		}
		
	}
}

function updatePrice(){
	if(event && event.srcElement && event.srcElement.tagName=="INPUT"){
		var input = event.srcElement ;
		var newPrice = input.value ;
		//input节点的父节点是td
		var priceTD = input.parentElement ;
		priceTD.innerText = newPrice ;

		//更新当前行的小计这一个格子的值
		//priceTD.parentElement td的父元素是tr
		updateXJ(priceTD.parentElement);

	}
}

//更新指定行的小计
function updateXJ(tr){
	if(tr && tr.tagName=="TR"){
		var tds = tr.cells;
		var price = tds[1].innerText ;
		var count = tds[2].innerText ;
		//innerText获取到的值的类型是字符串类型，因此需要类型转换，才能进行数学运算
		var xj = parseInt(price) * parseInt(count);
		tds[3].innerText = xj ;

		//更新总计
		updateZJ();
	}

}

//更新总计
function updateZJ(){
	var fruitTbl = document.getElementById("tbl_fruit");
	var rows = fruitTbl.rows ;
	var sum = 0 ;
	for(var i = 1; i<rows.length-1 ; i++){
		var tr = rows[i];
		var xj = parseInt(tr.cells[3].innerText);		//NaN    not a number 不是一个数字
		sum = sum + xj ;
	}
	rows[rows.length-1].cells[1].innerText = sum ;
}




//当鼠标悬浮时，显示背景颜色
function showBGColor(){
	//event : 当前发生的事件
	//event.srcElement : 事件源
	//alert(event.srcElement);
	//alert(event.srcElement.tagName);	--> TD
	if(event && event.srcElement && event.srcElement.tagName=="TD"){
		var td = event.srcElement ;
		//td.parentElement 表示获取td的父元素 -> TR
		var tr = td.parentElement ;
		//如果想要通过js代码设置某节点的样式，则需要加上 .style
		tr.style.backgroundColor = "navy" ;

		//tr.cells表示获取这个tr中的所有的单元格
		var tds = tr.cells;
		for(var i = 0 ; i<tds.length ; i++){
			tds[i].style.color="white";
		}
	}
}

//当鼠标离开时，恢复原始样式
function clearBGColor(){
	if(event && event.srcElement && event.srcElement.tagName=="TD"){
		var td = event.srcElement ;
		var tr = td.parentElement ;
		tr.style.backgroundColor="transparent";
		var tds = tr.cells;
		for(var i = 0 ; i<tds.length ; i++){
			tds[i].style.color="threeddarkshadow";
		}
	}
}

//当鼠标悬浮在单价单元格时，显示手势
function showHand(){
	if(event && event.srcElement && event.srcElement.tagName=="TD"){
		var td = event.srcElement ;
		//cursor : 光标
		td.style.cursor="hand";
	}

}
```

### hitory对象

**方法**

| 属性       | 说明                              |
| :-------- | :-------------------------------- |
| back()    | 加载 history 列表中的前一个 URL。   |
| forward() | 加载 history 列表中的下一个 URL。   |
| go()      | 加载 history 列表中的某个具体页面。 |

### location对象

**属性**

| 属性      | 说明                                       |
| :------- | :----------------------------------------- |
| hash     | 设置或返回从井号 (#) 开始的 URL（锚）。       |
| host     | 设置或返回主机名和当前 URL 的端口号。         |
| hostname | 设置或返回当前 URL 的主机名。                |
| href     | 设置或返回完整的 URL。                       |
| pathname | 设置或返回当前 URL 的路径部分。              |
| port     | 设置或返回当前 URL 的端口号。                |
| protocol | 设置或返回当前 URL 的协议。                  |
| search   | 设置或返回从问号 (?) 开始的 URL（查询部分）。 |

**方法**

| 方法       | 说明                    |
| :-------- | :--------------------- |
| assign()  | 加载新的文档。          |
| reload()  | 重新加载当前文档。       |
| replace() | 用新的文档替换当前文档。 |

### document对象

**属性**

|    属性     | 说明                     |
| :------- | :------------------- |
| referrer | 返回载入当前文档的URL |
| URL      | 返回当前文档的URL     |

**方法**

| 方法                   | 说明                                   |
| :--------------------- | :------------------------------------- |
| getElementById()       | 返回对拥有指定id的第一个对象的引用        |
| getElementsByName()    | 返回带有指定名称的对象的集合              |
| getElementsByTagName() | 返回带有指定标签名的对象的集合            |
| write()                | 向文档写文本、HTML表达式、JavaScript代码 |

#### DOM 文档对象模型

**组成**

- Core DOM 核心DOM编程，定义了一套标准的针对任何结构化文档的对象，包括HTML、XHTML、XML
- XML DOM 定义了一套标准的针对XML文档的对象
- HTML DOM 定义了一套标准的针对HTML文档的对象

**文档节点树**

- 数据结构：整个HTML文档中标签、文本、属性、注释等等节点对象组成的一个树形结构

![](c:/notebook/pictures/Snipaste_2022-11-22_21-21-35.png =300x)

![](c:/notebook/pictures/Snipaste_2022-11-22_21-14-08.png =300x)

##### Core DOM操作节点

**访问节点**

|  方法   | 说明    |
| :-- | :-- |
|  getElementById()   |   返回对拥有指定id的第一个对象的引用  |
| getElementByName()    |   返回带有指定名称的对象的集合  |
|  getElementByTagName()   |  返回带有指定标签名的对象的集合   |

| 属性        | 说明                                                            |
| :--------- | :------------------------------------------------------------- |
| parentNode | 该节点的父节点                                                  |
| firstChild | 首个子节点，（如果某有，文本和属性节点返回空数组，元素节点返回null） |
| lasstChild | 最后一个字节点                                                   |

**操作节点属性值**

| 方法                   | 说明       |
| :--------------------- | :-------- |
| getAttribute("属性名") | 获取属性值 |
| setAttribute("属性名") | 设置属性值 |

**创建和增加属性**

| 方法                           | 说明                                                           |
| :---------------------------- | :------------------------------------------------------------- |
| createElement(tagName)        | 按照给定的标签名创建一个新的元素节点                              |
| appendChild(nodeName)         | 向已存在节点列表的末尾添加新的节点                                |
| insertBefore(newNode,oldNode) | 向指定的节点之前插入一个新的节点                                  |
| cloneNode(boolean)            | 复制某个指定的节点（true复制该节点及其字节点，false只复制当前节点） |

**删除和替换节点**

| 方法                           | 说明                     |
| :---------------------------- | :----------------------- |
| removeChild(node)             | 删除指定的节点            |
| replaceChild(newNode,oldNode) | 用其他的节点替换指定的节点 |

##### HTML DOM

**table对象**

| 属性    | 说明                       |
| :----- | :------------------------- |
| rows[] | 返回该表格中所有行的一个数组 |

| 方法         | 说明                |
| :---------- | :----------------- |
| insertRow() | 从表格中插入一个新行 |
| deleteRow() | 从表格中删除一行     |

**tableRow对象**

| 属性     | 说明                           |
| :------- | :---------------------------- |
| cells[]  | 返回包含行中所有单元格的一个数组 |
| rowIndex | 返回该行在表中的位置                |

| 方法               | 说明                                   |
| :---------------- | :------------------------------------ |
| insertCell(index) | 在一行中的指定位置插入一个空的`<td>`标签 |
| deleteCell()      | 删除行中指定的单元格                    |

**tableCell对象**

| 属性       | 说明                                |
| :-------- | :---------------------------------- |
| cellIndex | 返回单元格在某行单元格集合中的位置     |
| innerHTML | 设置或返回单元格                     |
| align     | 设置或返回单元格内部数据的水平排列方式 |
| className | 设置或返回元素的class属性             |

### navigator对象

### screen对象

# CS和BS架构的异同

![](c:/notebook/pictures/Snipaste_2022-11-19_00-02-13.png =600x)

# Tomcat

## Tomcat安装和配置

**目录结构说明**

![](c:/notebook/pictures/Snipaste_2022-11-19_00-15-23.png =300x)

**配置环境变量**

1. 需要配置JAVA_HOME（安装JDK时配置的），否则Tomcat打开一闪而过，
  - 因为Tomcat是用C和Java编写的，需要JRE。 
  - 需要指向JDK的JAVA_HOME，否则不行。

![](c:/notebook/pictures/Snipaste_2022-11-19_00-58-11.png =400x)
![](c:/notebook/pictures/Snipaste_2022-11-19_01-00-14.png =400x)


2.  配置Tomcat环境变量
    1. 新建变量名：CATALINA_HOME，变量值：E:\Tomcat8
    2. 打开PATH，添加变量值：%CATALINA_HOME%\lib;%CATALINA_HOME%\bin
    3.  将命令行转入到Tomcat安装Bin目录，例如：E:\tomcat8\bin  ，cmd输入命令：`service.bat install`

**启动Tomcat，访问主页**

- 在Tomcat解压的bin目录中，打开startup.bat

![](c:/notebook/pictures/Snipaste_2022-11-19_00-24-36.png =500x)

##  部署项目

### Web项目

**部署**

- 到Tomcat解压目录的webapps目录中创建文件目录
- 并且在该目录下创建文件目录：`WEB-INFO`

![](c:/notebook/pictures/Snipaste_2022-11-19_01-17-51.png =200x)

**打开**

`http://localhost:8080/myTestWeb/demo1.html`

## IDEA 部署

1. 先新建一个项目Project，再新建一个Module，然后将Moudle设置为Web
![](c:/notebook/pictures/Snipaste_2022-11-19_10-24-42.png =300x)
![](c:/notebook/pictures/Snipaste_2022-11-19_10-27-17.png =550x)
![](c:/notebook/pictures/Snipaste_2022-11-19_10-28-00.png =300x)

2. 配置IDEA的Tomcat设置
![](c:/notebook/pictures/Snipaste_2022-11-19_10-16-09.png =300x)
![](c:/notebook/pictures/Snipaste_2022-11-19_10-17-22.png =300x)
![](c:/notebook/pictures/Snipaste_2022-11-19_10-19-28.png =550x)
![](c:/notebook/pictures/Snipaste_2022-11-19_10-32-24.png =550x)

![](c:/notebook/pictures/Snipaste_2022-11-19_10-39-54.png =550x)


### 404和`$END$`

- 修改web.xml中的内容，添加`<welcome-file-list></welcome-file-list>`
![](C:/notebook/pictures/Snipaste_2022-11-24_14-53-50.png =550x)
![](c:/notebook/pictures/Snipaste_2022-11-24_11-20-55png.png =550x)

### 405

- 当前请求的方法不支持，如POST必须对应doPost()。

### 空指针 NumberFormatException

- 获取的是null时，却进行了操作。

### 乱码问题

#### 控制台编码

![](c:/notebook/pictures/Snipaste_2022-11-19_11-17-35.png =550x)
![](c:/notebook/pictures/Snipaste_2022-11-19_11-25-51.png =550x)
![](c:/notebook/pictures/Snipaste_2022-11-19_11-26-57.png =550x)

### Servlet 输出中文字符

# Servlet

## 导入对应的API

**所需要的API不在JDK内，需要从Tomcat中导入**

- 方式1
   - 从路径"E:\Tomcat8\lib\servlet-api.jar"导入

- 方式2
   - 从IDEA中导入
![](c:/notebook/pictures/Snipaste_2022-11-21_23-17-14.png =600x)


## Servlet

### 处理Web请求的过程

1. 服务器接收从客服端发出的请求
2. 服务器将请求信息发送至Servlet
3. Servlet经过处理之后，生成响应的内容
4. 服务器将响应的内容返回给客服端

![](c:/notebook/pictures/Snipaste_2022-11-21_23-49-44.png =650x)

### Servlet结构体系

![](c:/notebook/pictures/Snipaste_2022-11-23_16-02-35.png =600x)

#### HttpServlet类

**HttpServlet容器响应Web客户请求流程如下：**

![](c:/notebook/pictures/Snipaste_2022-11-25_09-43-35.png =550x)

1. Web客户向Servlet容器发出Http请求；
2. Servlet容器解析Web客户的Http请求；
3. Servlet容器创建一个HttpRequest对象，在这个对象中封装Http请求信息；
4. Servlet容器创建一个HttpResponse对象；
5. Servlet容器调用HttpServlet的service方法，把HttpRequest和HttpResponse对象作为service方法的参数传给HttpServlet对象；
6. HttpServlet调用HttpRequest的有关方法，获取HTTP请求信息；
7. HttpServlet调用HttpResponse的有关方法，生成响应数据；
8. Servlet容器把HttpServlet的响应结果传给Web客户

**HTTP的请求方式包括DELETE,GET,OPTIONS,POST,PUT和TRACE,**

在HttpServlet类中分别提供了相应的服务方法,它们是,

- doDelete(),
- doGet(),
- doOptions(),
- doPost(),
-  doPut()
- doTrace().

#### ServletRequest 接口

| 方法                                           | 说明                         |
| :-------------------------------------------- | :--------------------------- |
| Object getAttribute(String name)              | 获取名称为name的属性值         |
| void setAttribute(String name, Object object) | 在请求中保存名称为name的属性值 |
| void removeAttibute(String name)              | 清除请求中名称为name的属性值   |
| String getParameter()                         | 获取表单中传递的参数           |

#### HttpServletRequest 接口

| 增加的方法                 | 说明                                                                         |
| :------------------------ | :--------------------------------------------------------------------------- |
| String getContextPath()   | 返回请求URL中表示请求上下文的路径（URL开始部分）                                 |
| Cookie[] getCookies()     | 返回客户端在此次请求中发送的所有Cookie对象                                      |
| HttpSession  getSession() | 返回和此次请求相关联的Session，如果没有给客户端分配Session，则创建一个新的Session |
| String getMethod()        | 返回此次请求使用的Http方法的名称，如GET、POST                                   |


#### ServletResponse 接口

| 方法                              | 说明                                                 |
| :------------------------------- | :-------------------------------------------------- |
| PrintWriter getWriter()          | 返回PrintWriter对象，用于向客户端发送文本              |
| String getCharacterEncoding()    | 返回在响应中发送的正文所使用的字符编码                  |
| void setCharacterEncoding()      | 设置发送到客户端的响应的字符集编码                     |
| void setContentType(String type) | 设置发送到客户端的响应的内容类型，此时响应的状态尚未提交 |

#### HttpServletResponse 接口

| 增加的方法                                 | 说明                                                                    |
| :---------------------------------------- | :---------------------------------------------------------------------- |
| void addCookie(Cookie cookie)             | 增加一个Cookie到响应，此方法可以多次调用，设置多个Cookie                    |
| void addHeader(String name, String value) | 将一个名称为name，值为value的响应报头添加到响应中                          |
| void sendRedirect(String location)        | 发送一个临时的重定向响应到客户端，以便客户端访问新的URL，抛出一个IOException |
| void encodeURL(String url)                | 使用SessionID对用于重定向的URL进行编码，以便用于sendRedirect()方法中        |

### Servlet生命周期

![](c:/notebook/pictures/Snipaste_2022-11-23_15-52-30.png =400x)


- 第一次接收请求时，这个Servlet会进行实例化(调用构造方法)、初始化(调用init())、然后服务(调用service())
- 从第二次请求开始，每一次都是服务。
- 当容器关闭时，其中的所有的servlet实例会被销毁，调用销毁方法

**Servlet的初始化时机：**
        
- 默认情况下，第一次请求时，tomcat才会去实例化，初始化。 第一次请求时，耗时较长。如果需要提高系统的启动速度，当前默认情况就是这样。如果需要提高响应速度，我们应该设置Servlet的初始化时机。
- 我们可以通过`<servlet>`元素中的`<load-on-startup>`来设置servlet启动的先后顺序，数字越小，启动越靠前，最小值0

**Servlet在容器中是：单例的、线程不安全的**

- 单例：Servlet实例tomcat只会创建一个，所有的请求都是这个实例去响应。
- 线程不安全：

### 使用Servlet

#### 1.创建Servlet类

1. 实现Servlet接口
2. 继承GenericServlet类
3. 继承HttpSevlet类

**创建HttpServlet的步骤——“四部曲”**

1. 扩展HttpServlet抽象类； 
2. 覆盖HttpServlet的部分方法，如覆盖doGet()或doPost()方法； 
3. 获取HTTP请求信息。通过HttpServletRequest对象来检索HTML表单所提交的数据或URL上的查询字符串； 
4. 生成HTTP响应结果。通过HttpServletResponse对象生成响应结果，它有一个getWriter()方法，该方法返回一个PrintWriter对象。

#### 2.Servlet的部署 web.xml

- `<servlet>`元素，将Servlet内部名映射到一个Servlet类名，"包名+类名"
   - `<servlet-name>类名</servlet-name>`
   - `<servlet-class>包名+类名（即：完整的类位置）<servlet-class>`  
- `<servlet-mapping>`元素，将用户访问的URL映射到Servlet内部名
   - 一个 `<servlet>`可以对应多个`<servlet-mapping>`，即：可以将同一个Servlet程序应用于多个不同的页面
   - `<servlet-name></servlet-name>` 必须与`<servlet>`元素中的`<servlet-name>`一致
   - `<url-pattern></url-pattern>` 容器无法识别同时拥有两种匹配规则的pattern
      1. 精确匹配：类似于/myServlet的精确路径
      2. 通配符匹配：`/*`
      3. 扩展名匹配：`*.html，*.jpg ，.do ，.action`之类的
      4. 默认匹配（`/`）——当之前匹配都不成功时

#### 注：

**一个html只能有一个对应的Servlet，但可以有多个Filter**

**编码**

- post方式下需要设置编码，防止中文乱码
  - `req.setCharacterEncoding("utf-8")` 
  - 必须在获取参数之前完成设置
- get方式不需要设置编码，基于Tomcat8
  - Tomcat8之前
![](c:/notebook/pictures/Snipaste_2022-11-24_15-13-46.png =600x)

**例**

- MyServletTest.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>注册页面</title>
</head>
<body>
<!--
    action:提交到服务器的路径
    method:提交的方式
 -->
<form action="MyServlet" method="post">
    <!-- servlet通过name属性获取用户输入的数据 -->
    用户名:<input type="text" name="username"><br>
    密码:<input type="password" name="userpwd"><br>
    <input type="submit" value="注册">
</form>
</body>
</html>
```

- MyServlet.class

```java
package com.zjk.JSP;

import javax.servlet.*;
import javax.servlet.http.*;
import javax.servlet.annotation.*;
import java.io.IOException;

@WebServlet(name = "MyServlet", value = "/MyServlet")
public class MyServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    /* 对应form表单的get请求方式
     * HttpServletRequest：请求对象
     * HttpServletResponse：响应对象
     */
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //设定请求的字符集
        req.setCharacterEncoding("utf-8");
        //设置响应的文本类型
        resp.setContentType("text/html;charset=utf-8");
        //通过请求对象获取用户输入的内容
        String username = req.getParameter("username");
        String password = req.getParameter("userpwd");

        System.out.println(username + " : " + password);
        //如果输入的用户名是abc，密码是123，则表示注册成功，反之注册失败
//        if ("abc".equals(username) && "123".equals(password)) {
//            //使用响应对象，重定向到成功页面
//            //resp.sendRedirect("success.html");
//            //请求转发
//            req.getRequestDispatcher("success.html").forward(req, resp);
//        } else {
//            //使用响应对象，重定向到注册页面
//            resp.sendRedirect("MyServletTest.html");
//        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //设定请求的字符集
        req.setCharacterEncoding("utf-8");
        //设置响应的文本类型
        resp.setContentType("text/html;charset=utf-8");
        //通过请求对象获取用户输入的内容
        String username = req.getParameter("username");
        String password = req.getParameter("userpwd");
        System.out.println(username + " " + password);
        //如果输入的用户名是abc，密码是123，则表示注册成功，反之注册失败
//        if ("abc".equals(username) && "123".equals(password)) {
//            //使用响应对象，重定向到成功页面
//            resp.sendRedirect("success.html");
//        } else {
//            //使用响应对象，重定向到注册页面
//            resp.sendRedirect("MyServletTest.html");
//        }
    }
}
```

- web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>MyServlet</servlet-name> <!--用来映射的名字，可以随便起-->
        <servlet-class>com.zjk.JSP.MyServlet</servlet-class> <!--指向该类的地址 包名+类名-->
    </servlet>
    <servlet-mapping>
        <servlet-name>MyServlet</servlet-name> <!--映射中的servlet-name要与servlet中的一致-->
        <url-pattern>/MyServlet</url-pattern> <!--指向的网页的URL（地址）-->
    </servlet-mapping>
</web-app>
```

### 服务器内部转发以及重定向

| 服务器内部转发                                           |          客户端重定向           |
| :------------------------------------------------------ | ------------------------------ |
| req.getRequestDispatcher("网页路径").forward(req,resp); | resp.sendRedirect("网页路径"); |
| 一次请求响应的过程，客户端不知道服务器内部的转发            | 两次请求响应的过程              |
| 地址栏不变                                               | 地址栏改变                      |

**服务器内部转发**

![](c:/notebook/pictures/Snipaste_2022-11-27_10-06-35.png =600x)

**客服端重定向**

![](c:/notebook/pictures/Snipaste_2022-11-27_10-09-04.png =600x)

```java
package com.zjk.redirect;


import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class ServletDemo01 extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("demo01");
        //服务器内部转发
        //req.getRequestDispatcher("demo02");

        //客户端重定向
        resp.sendRedirect("demo02");
    }
}
```

## HTTP 超文本传输协议

- Http是无状态的
- Http请求响应包含两个部分：请求和响应

### 请求
    
- 请求包含三个部分： 1.请求行 ； 2.请求消息头 ； 3.请求主体

![](c:/notebook/pictures/Snipaste_2022-11-25_11-26-34.png =500x)


#### 请求行

**1. 请求的方式 ； 2.请求的URL ； 3.请求的协议（一般都是HTTP1.1）**

![](c:/notebook/pictures/Snipaste_2022-11-25_11-31-49.png =650x)

#### 请求消息头

- 包含了很多客户端需要告诉服务器的信息

![](c:/notebook/pictures/Snipaste_2022-11-25_11-29-04.png =500x)

#### 请求体

- get方式，没有请求体，但是有一个queryString
- post方式，有请求体，form data
- json格式，有请求体，request payload

![](c:/notebook/pictures/Snipaste_2022-11-25_11-29-54.png =650x)

### 响应：

**1. 响应行 ； 2.响应头 ； 3.响应体**

![](c:/notebook/pictures/Snipaste_2022-11-25_11-32-42.png =500x)

#### 响应行

**1.协议 2.响应状态码(200) 3.响应状态(ok)**

##### 响应状态码

![](c:/notebook/pictures/Snipaste_2022-11-25_11-33-24.png =500x)

#### 响应头

- 包含了服务器的信息；服务器发送给浏览器的信息（内容的媒体类型、编码、内容长度等）

![](c:/notebook/pictures/Snipaste_2022-11-25_11-36-19.png =500x)

#### 响应体

![](c:/NoteBook/pictures/Snipaste_2022-11-25_11-35-01.png =300x)

## session 会话

**Http是无状态的**

- HTTP 无状态 ：服务器无法判断这两次请求是同一个客户端发过来的，还是不同的客户端发过来的
- 无状态带来的现实问题：第一次请求是添加商品到购物车，第二次请求是结账；如果这两次请求服务器无法区分是同一个用户的，那么就会导致混乱
- 通过会话跟踪技术来解决无状态的问题。

### 会话跟踪技术

- 客户端第一次发请求给服务器，服务器获取session，获取不到，则创建新的，然后响应给客户端
- 下次客户端给服务器发请求时，会把sessionID带给服务器，那么服务器就能获取到了，那么服务器就判断这一次请求和上次某次请求是同一个客户端，从而能够区分开客户端

![](c:/notebook/pictures/Snipaste_2022-11-25_14-33-58.png =500x)

| 方法                              | 说明                                                                                                    |
| :------------------------------- | :------------------------------------------------------------------------------------------------------ |
| request.getSession(boolean)      | 获取当前的会话，没有则创建一个新的会话；true效果和不带参数相同；false获取当前会话，没有则返回null，不会创建新的 |
| session.getId()                  | 获取sessionID                                                                                           |
| session.isNew()                  | 判断当前session是否是新的                                                                                |
| session.getMaxInactiveInterval() | session的非激活间隔时长，默认1800秒                                                                       |
| session.setMaxInactiveInterval() | 设置session的非激活间隔时长，默认1800秒                                                                   |
| session.invalidate()             | 强制性让会话立即失效                                                                                     |

1. HttpServlet

```java
package com.zjk.sessions;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.HttpSession;
import java.io.IOException;

public class ServletSession extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取SessionID
        HttpSession session = req.getSession();
        System.out.println("SessionID: " + session.getId());
        System.out.println(session.isNew());
        System.out.println(session.getMaxInactiveInterval());
    }
}
```

2. web.xml部分配置

```xml
<servlet>
    <servlet-name>ServletSession</servlet-name>
    <servlet-class>com.zjk.sessions.ServletSession</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>ServletSession</servlet-name>
    <url-pattern>/fruit</url-pattern>
</servlet-mapping>
```

### session保存作用域

- session保存作用域是和具体的某一个session对应的，切换其他session时，保存域不同。

![](c:/notebook/pictures/Snipaste_2022-11-25_14-51-25.png =800x)

| 方法                            | 说明 |
| :----------------------------- | :--- |
| void session.setAttribute(k,v) |      |
| Object session.getAttribute(k) |      |
| void removeAttribute(k)        |      |

1. Servlet

```java
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class FirstSession extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getSession().setAttribute("fname","Tom");
        System.out.println("1");
    }
}

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;

public class SecondeSession extends HttpServlet {
    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("2" + req.getSession().getAttribute("fname"));
    }
}
```

2. web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>FirstSession</servlet-name>
        <servlet-class>FirstSession</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>FirstSession</servlet-name>
        <url-pattern>/demo1</url-pattern>
    </servlet-mapping>
    <servlet>
        <servlet-name>SecondeSession</servlet-name>
        <servlet-class>SecondeSession</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>SecondeSession</servlet-name>
        <url-pattern>/demo2</url-pattern>
    </servlet-mapping>
</web-app>
```

## Filter接口 过滤器

**过滤器的工作原理**

![](c:/notebook/pictures/Snipaste_2022-11-24_23-53-56.png =500x)

**过滤器链**

- 多个Filter接口的实现类，在web.xml配置文件中，按顺序执行
- 多个具有特定操作和功能的过滤器的组合，且在配置文件中也要存在相应的多个过滤器配置，按配置文件中的顺序，逐一进行处理，直到Web资源。

**应用**

1. 统一处理请求和响应
2. 对请求进行日志记录和审核
3. 对数据屏蔽和替换
4. 对数据加密和解密

### Filter接口 方法

**生命周期**

![](c:/notebook/pictures/Snipaste_2022-11-25_00-07-59.png =300x)

**init(FilterConfig filterConfig)**
     
1. 在web应用程序启动时，web服务器将根据 web.xml文件中的配置信息来创建每个注册的Filter实例对象，并将其保存在服务器的内存中。
2. Web容器创建Filter对象实例后，将立即调用该Filter对象的init()方法。
      - Init方法在Filter生命周期中仅执行一次
      - 传递一个包含Filter的配置和运行环境的FilterConfig对象
3. 利用FilterConfig对象可以得到ServletContext对象，以及部署描述符中配置的过滤器的初始化参数。

<mark>**doFilter(ServletRequest request, ServletResponse response, FilterChain chain)**</mark>

1. 当客户端请求目标资源的时候，容器就会调用与这个目标资源相关联的过滤器的 doFilter()方法。
2. 参数 request, response 为 web 容器或 Filter 链(即：<mark>chain参数的doFilter()</mark>使用的参数)的上一个 Filter 传递过来的请求和相应对象；参数 chain 为代表当前 Filter 链的对象，
3. 可以在当前 Filter 对象的 doFilter() 方法的内部调用 `FilterChain对象参数的 chain.doFilter(request,response)方法`把请求交付给 Filter 链中的下一个 Filter 或者目标 Servlet 程序去处理。<mark>否则无法使用其他的程序：其他的Fliter或Servlet等</mark>
   - 过滤器链中的任何一个 Filter 没有调用 `FilterChain.doFilter()` 方法，请求都不会到达目标资源。

**public void destroy()**

- 释放过滤器使用的资源。

### 使用过滤器

**1. 实现Filter接口**

**2. 配置web.xml文件**

![](c:/notebook/pictures/Snipaste_2022-11-25_10-28-42.png =700x)

```java
package com.zjk.filters;

import javax.servlet.*;
import java.io.IOException;

public class FilterTest implements Filter {

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        servletRequest.setCharacterEncoding("utf-8");
        servletResponse.setCharacterEncoding("utf-8");
        //如果没有 filterChain.doFilter(servletRequest, servletResponse)
        //则无法使用其他的应用程序：如其他的Filter、Servlet等
        filterChain.doFilter(servletRequest, servletResponse);
        System.out.println("Filter OK!");
    }

    @Override
    public void destroy() {
        System.out.println("filter is destoried");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>FruitServlet</servlet-name> <!--用来映射的名字，可以随便起-->
        <servlet-class>com.zjk.servlets.FruitServlet</servlet-class> <!--指向该类的地址 包名+类名-->
    </servlet>
    <servlet-mapping>
        <servlet-name>FruitServlet</servlet-name> <!--映射中的servlet-name要与servlet中的一致-->
        <url-pattern>/fruit</url-pattern> <!--指向的网页的URL（地址）-->
    </servlet-mapping>
    <filter>
        <display-name>FilterTest</display-name>
        <filter-name>FilterTest</filter-name> <!--用来映射的名字，可以随便起-->
        <filter-class>com.zjk.filters.FilterTest</filter-class> <!--指向该类的地址 包名+类名-->
    </filter>
    <filter-mapping>
        <filter-name>FilterTest</filter-name> <!--映射中的filter-name要与filter中的一致-->
        <url-pattern>/fruit</url-pattern> <!--指向的网页的URL（地址）-->
    </filter-mapping>
</web-app>
```

### FilterChain接口



## Servlet监听器




