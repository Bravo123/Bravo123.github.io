---
layout:     post
title:      "package-lock.json是什么"
subtitle:   "与package.json有什么区别"
date:       2018-06-26 11:11:30
author:     "Thq"
header-img: "img/post-bg-alitrip.jpg"
catalog: true
tags:
    - package
---

> 对配置文件这些东西还是有点陌生的，多学习学习

<br>问题描述：在一次npm install中，发现项目中突然多了一个package-lock.json的文件，是什么呢？跟package.json有怎样的关系呢？
<br>对package.json的认知程度就限于：详细描述了整个项目中的依赖以及一些启动的相关命令，以及一些项目描述之类的，在生成项目的时候通过npm init会生成一个package.json的文件。
<br>package-lock.json:记录当前项目中各个依赖的具体来源以及版本号。生成这个的目的是什么呢？
<br>开发者可以在package.json中标出项目对npm包的依赖，比如说向后兼容了某个依赖，大多数情况下安装最新的依赖都没有问题。但是npm是一个开源的世界，并不是所有的包都能保证大版本下的接口符合兼容，所以就导致不同时间下的npm包可能有所不同，导致出现bug。
<br>所以package-lock.json就是记录当前项目各个包的具体来源以及版本号，根据这个package-lock.json保证不同电脑上安装的npm包都是一样一样的。