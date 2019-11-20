---
layout:     post
title:      "常见浏览器兼容性问题"
subtitle:   ""
date:       2018-3-6 14:49:30
author:     "Thq"
header-img: "img/home-bg-o.jpg"
catalog: true
tags:
    - HTML
    - JavaScript
    - CSS
---

> 说得就是IE

<br>先熟悉一下浏览器的内核
<br>Trident: IE
<br>Gecko: Firefox、Netcapes6
<br>Presto: Opeara7
<br>Webkit: Safari、Chrome

<br>兼容性问题主要就是IE与几个主流浏览器firefox、Chrome等;一般分为HTML、JavaScript、CSS兼容；

<br>下面的一些兼容性问题有来源于网络，也有来自于自己遇见的。

>不同浏览器标签默认的padding与margin不一致

```
* {
	margin: 0;
	padding: 0;
}
```

>块级标签float + 横行margin: IE6显示的margin大

```
<!-- 将块级的标签设置为行内 -->
.float {
	display: inline;
}
```

>设置较小的高度标签的时候(10px),ie6、ie7高度超出设置

```
#height {
	overflow: hidden;
	line-height: 10px;
}
```

>标签min-height不兼容

```
<!-- min-height本身就是一个不兼容的属性 -->
.min-height {
	min-height:200px;
	 height:auto !important;
	 height:200px;
	  overflow:visible;
}
```

>css hack

<br>归类： ie6;ie7;其他(ie8+,chrome等)
<br>ie6认识的hacker是_ 和 * 
<br>ie7认识是 * 
```
.height {
	height: 300px;
	*height: 200px;
	_height: 100px;
}
<!-- 在IE6下height为100px;IE7下height为200px;其他下300px -->

color: red; //所有均可识别
_color: red; //IE6
*color: red; //IE6 IE7
+color: red; //IE6 IE7
*+color: red; //IE6 IE7
[color: red; //IE6 IE7
color: red\9; //IE6789
color: red\0; //IE89
color: red\9\0 //IE9
color: red \0; //IE9
```

> float闭合

```
.clearfix:after{ 
	content: '';
	display: block;
	height: 0;
	clear: both;
	visibility: hidden;
}
.clearfix {
	display: inline-block;
}
```

>div垂直居中

```
//父容器与本身高度未知
parentElement {
	position: relative;
}
childElement {
	position: absolute;
	top: 50%;
	transform: translateY(-50%);
}

//父容器只有一个元素，且父元素高度已知
parentElement {
	height: xxx;
}
childElement {
	position: relative;
	top: 50%;
	transform: translateY(-50%);
}


//flex
parentElement {
	display: flex;
	display: -webkit-flex;
	align-items: center;
}

//table-cell
parentElement {
	display: table;
}
childElement{
	display: table-cell;
	vertical-align: middle;
}
```