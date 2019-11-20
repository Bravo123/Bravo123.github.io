---
layout:     post
title:      "打开网页到展示经历了什么"
subtitle:   ""
date:       2019-02-14 11:19:28
author:     "Thq"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - http
---

> “if you're going to try, go all the way ”

scheme://host.domain:port/path/filename

scheme: 因特网服务类型 http/https/ftp/file等
host: 主机名 www
domian: 域名 baidu.com
port: 端口 8080
path: 服务器上路径
filename: 资源名称

DNS解析
TCP连接
发送HTTP请求
服务器处理完请求并返回HTTP报文
浏览器解析渲染页面（css Tree、 dom Tree）
连接结束

三次握手：
第一次通知服务器要发送请求
第二次服务器告诉浏览器准备好了，赶紧发送
第三次告诉服务器我要准备发送了(第三次看起来像是多余的可以直接发送请求，但是为了防止该连接已失效而且还把请求发送给了服务器)

四次挥手：
第一次：浏览器通知服务器请求报文发完了
第二次：服务器通知浏览器请求报文收到了
第三次：服务器通知浏览器响应报文发送完了
第四次：浏览器通知服务器响应报文收到了