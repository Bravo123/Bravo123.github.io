---
layout:     post
title:      "移动端适配"
subtitle:   "各种不适配问题"
date:       2018-09-10 10:21:04
author:     "Thq"
header-img: "img/contact-bg.jpg"
catalog: true
tags:
    - 移动端 
---
## 前言
最近做的项目需要在android和ios上面运行，然后就出现了各种不适配的问题，天花乱坠的。样式各种崩，绝望.jpg。
<br>列举一下遇到的问题，以及是如何解决的。

## min-height
问题：在ios 9.3.1系统下，min-height设置但是没有设置height的时候，循环出来的列表有可能是白页。因为夫元素没有设置height的时候，子元素的min-height和height是不会生效的。
<br>解决：设置了min-height,同时也设置一下height.

## 点击高亮
问题：ios点击的时候会出现黑块
<br>解决：
```
// 移动端点击高亮
-webkit-tap-highlight-color:rgba(0,0,0,0);
```

## 图片问题
在浏览器中如果不加协议http/https://的话会自动给补上，但是在安卓和ios中并不会补上，所以前缀记得加上。
<br>ios9.3及以下低版本的系统对于base64格式的图片可能会不支持，所以在webpack打包的时候需要将转换成base64模式关闭。

## flex
ios9.3及以下对与flex布局的一些属性可能会不支持，目前遇见的有justify-content: space-evenly;flex: 1;
<br>且在ios8一下的时候需要加上-webkit-来保证兼容性。

## 样式写全不漏写
随着浏览器的不断改进，所以在pc端的时候即使很多属性不写、漏写，浏览器都会自动给补上，但是在低版本的移动端可就不一定了。所以为了向下兼容，还是规范一下，把样式都写出来比较好。

## 判断app是否联网
navigator.onLine // true or false

## 总结
移动端向下兼容这一块，目前只遇到了这些问题，不过随着移动端系统不断升级改进，低系统的会逐渐被取代，到时候的兼容这一块应该也会越来越成熟。