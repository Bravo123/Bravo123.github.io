---
layout:     post
title:      "cross domain request"
subtitle:   "JSONP CORS postMessage"
date:       2018-3-1 12:20:04
author:     "Thq"
header-img: "img/post-bg-os-metro.jpg"
catalog: true
tags:
    - Http 
    - JavaScript
---

> 整理了几种跨域请求

# 浏览器的同源策略
<br>何为同源？只有当协议、端口、域名都相同的页面，才称为具有相同的源。网站之前的数据传输与请求构成了跨域请求，会受到同源策略的限制；

# JSONP跨域
JSONP(JSON with Padding)是数据格式JSON的一种“使用模式”，可以让网页从别的网域要数据；根据 XmlHttpRequest 对象受到同源策略的影响，而利用 ```<script>```元素的这个开放策略，网页可以得到从其他来源动态产生的JSON数据，而这种使用模式就是所谓的 JSONP。用JSONP抓取的数据并非JSON，而是任意的JavaScript。
<br>缺点：
<br>1.只能使用get请求；
<br>2.不能注册success、error等事件监听，不能很容易确定JSONP是否请求失败；
<br>3.JSONP是从其他域中加载代码执行，容易受到跨站请求伪造的攻击，其安全性无法确保；

# CORS
Cross-Origin Resource Sharing跨域资源共享是一份浏览器技术的规范，提供了 Web 服务从不同域传来沙盒脚本的方法，以避开浏览器的同源策略，确保安全的跨域数据传输。它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。<br>
<strong>简介</strong><br>
<br>CORS需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。
<br>整个CORS通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。
<br>两种请求方式：简单请求(simple request)和非简单请求(not-so-simple request)
<br>当满足一下两大条件就是简单请求
```
	(1) 请求方式是HEAD、GET、POST之一
	(2) HTTP头部信息不超出以下几种字段：Accept、Accept-Language、Content-language、Last-Event-ID、Content-Type(application/x-www-form-unlencoded、multipart/form-data、text/plain)
```
<br>浏览器对这两种不同的请求有不同的处理方式。
<br><strong>简单请求</strong>
<br>浏览器直接发出请求，在头信息多加一个Origin字段表明本次请求来自哪个源(协议+域名+端口)；服务器根据这个值来决定是否同意这次请求。如果同意这次请求，返回的头部会多出几个信息字段
<br>(1)Access-Control-Allow-Origin: 该字段必须，要么是请求时Origin的值，要么是 * 表示接收任意域名的请求；
<br>(2)Access-Control-Allow-Credentials:可选，表示是否允许发送Cookie，默认为false不允许；
<br>(3)Access-Control-Expose-Headers:可选CORS请求时，XMLHttpRequest对象的getResponseHeader()方法只能拿到6个基本字段：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma。如果想拿到其他字段，就必须在Access-Control-Expose-Headers里面指定；
<br>(4)withCredential，如果发送cookie给服务器的话，不仅需要服务器将Access-Control-Allow-Credetials设为true还需要开发者在ajax中把withCredentials设为true；需要注意的是，如果发送cookie，origin就不能是*
<br><strong>非简单请求</strong>
<br>非简单请求是那种对服务器有特殊要求的请求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json。
<br>非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight）。
<br>预检请求头部中除了origin还需要包含两个特殊字段Access-Control-Request-Method:表明用的是什么请求方式；Access-Control-Request-Headers：指定会额外发送的头部信息;预检通过则返回正常的HTTP回应，包含了能请求的方式以及额外的头部信息;
<br>CORS与JSONP的使用目的相同，但是比JSONP强大；
<br>JSONP只支持GET请求，CORS支持所有类型的HTTP请求；
<br>JSONP的优势在于支持老式浏览器，以及可访问不支持CORS的网站数据请求；

<br>CORS知识参考了阮老师的CORS详解;
<br>项目中其他关于Response Headers中的字段
<br>Access-Control-Max-Age: 预检有效时间（秒）
<br>

# postMessage
<br>HTML5新特性，可以用来向其他所有的 window 对象发送消息。需要注意的是我们必须要保证所有的脚本执行完才发送 MessageEvent，如果在函数执行的过程中调用了它，就会让后面的函数超时无法执行。
