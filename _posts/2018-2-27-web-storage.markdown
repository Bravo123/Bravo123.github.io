---
layout:     post
title:      "聊一聊前端的存储方式"
subtitle:   "cookie localStorage sessionStorage"
date:       2018-02-27 17:05:04
author:     "Thq"
header-img: "img/post-bg-rwd.jpg"
catalog: true
tags:
    - web storage 
---

# Cookie
<br>英文意思是“饼干、小甜点”；直接印象就是能存储容量不大；
<br>H5之前，它是主要的存储方式；Cookie数据会带到请求头的cookie字段中，每次同主域名的请求中，都会传递数据，增加了网络请求的数据量，而且会造成主域的污染，而且在不同的浏览器上数量和大小都有限制；
<br>同时，cookie的操作比较麻烦，需要自己实现(来源自网络，侵删)
<br>
```
var cookie = {
        /**设置cookie 
         ** name 标识
         ** value 值
         ** options {
         **   'path': '访问路径',
         **   'domain' : '域名',
         **   'expire' : 过期时间
         }
         **/
        setCookie : function(name,value,options){
          var options = options ? options : {},
              path = options.path ? options.path : '/',
              domain = options.domain ? options.domain : document.domain,
              time = options.expire ? (new Date().getTime() + options.expire * 1000) : '',
              expire = new Date(time).toUTCString();
          document.cookie = encodeURIComponent(name) + "="+ encodeURIComponent(value) + ";expires=" + expire + 
                            ";domain=" + domain + ";path=" + path;
        },
        //获取cookie
        getCookie: function(name){
          var arr,
              reg=new RegExp("(^| )"+name+"=([^;]*)(;|$)");
          if(arr=document.cookie.match(reg)){
             console.log(arr);
             return unescape(arr[2]);
          }
          return null;
          
        },
        //移除cookie
        removeCookie: function(name){
          var val = this.getCookie(name);
          if(val != null){
            this.setCookie(name,val, {
              expire : - 1
            })
          }
        }
      }
```
<br>虽然Cookie小还比较麻烦，但是也是不可或缺的；它的作用是与服务器进行交互，作为http规范的一部分存在的。

# sessionStorage
<br>sessionStorage是h5新引入的，用于本地存储一个会话(session)中的数据；这些数据只有在同一个会话中的页面才能访问，并且当会话结束后数据也随之销毁；因此sessionStorage不是一种持久化的本地存储，仅仅是会话级别的存储

# localStorage
<br>同上，localStorage也是h5引入的，也是存储在本地。web storage不像cookie携带在http请求头部字段中，有效的节省了带宽；localStorage不同与sessionStorage的是本地持久化，如果不手动删除是不会消失的；基于web storage与cookie的特性不同，所以web storage一般用来存储一些不需要和服务器进行交互的一些数据；比如导航栏当前状态，一些普通的数据进行缓存；
<br>localStorage的一些API
<br>localstorage.length: 获取当前存储的键值对数量
<br>localstorage.key(n):获取第n项的键值
<br>localstorage.getItem(key):获取对应键值的数据
<br>localstorage.setItem(key,value):设置对应的键值对
<br>localstorage.remove(key):清除某个数据
<br>localstorage.clear():清除存储的所有数据