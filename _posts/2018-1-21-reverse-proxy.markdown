---
layout:     post
title:      "http"
subtitle:   "just a little"
date:       2018-01-21 20:18:04
author:     "Thq"
header-img: "img/contact-bg.jpg"
catalog: true
tags:
    - Http
---

# 反向代理
<br>其实一张图片就可以说清楚！！！
![图片](/img/reverse_proxy.jpg)
<br> 图片来源知乎，侵删。

# 常见的请求方式
<br>1. get(得到) url中会将信息暴露
<br>2. post(提交) url中会乱码，提交数据进行处理请求
<br>3. put(放置) 向指定位置上传
<br>4. delete(删除) 请求服务器删除某资源
<br>5. trace(跟踪) 回显服务器收到的请求，用来测试

# get与post的区别
<br>get: url=baseURL + "?username" + name + "&age" + age;
<br>post: 使用基本的url，将参数放在请求实体中;
<br>get是从服务器上获取数据，post是向服务器发送数据;
<br>get是把参数数据队列加到提交表单的action属性所指的URL中，值和表单内各个字段一一对应，在URL中可以看到。
post是通过HTTP post机制，将表单内各个字段与其内容放置在HTML header内一起传送到action属性所指的URL地址,用户看不到这个过程。
<br>get传送的数据量较小，不能大于2KB。post传送的数据量较大，一般被默认为不受限制。
<br>get的安全性低，效率高可分享链接;post安全性高，效率不如get，不可分享链接。

# 常见的http状态码
<br> 1XX: 临时响应，需要请求者继续执行操作;
<br> 100: 继续执行操作;
<br> 101: 需要切换协议;
<br> 2XX: 成功;
<br> 200 请求成功;
<br> 

<br> 302 请求的资源临时从不同的url响应请求
<br> 400 语义有误,当前请求无法被服务器理解
<br> 403 服务器已经理解，但是拒绝执行
<br> 404 请求失败，请求所希望得到的资源未发现
<br> 500 服务器遇到了一个未曾预料的状况，无法对请求进行处理
