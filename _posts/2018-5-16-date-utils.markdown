---
layout:     post
title:      "常用的工具函数"
subtitle:   ""
date:       2018-5-16 11:12
author:     "Thq"
header-img: "img/home-bg-o.jpg"
catalog: true
tags:
    - JavaScript
    - es6
---
> 常见的一些工具函数,会陆陆续续添加的
```
/**
 * 格式化成大概时间
 * @param {Date} dateTimeStamp 时间戳 s
 * @return 大概时间
 */
export function timeDiff(dateTimeStamp) {
	dateTimeStamp *= 1000;
	let minute = 1000 * 60;
	let hour = minute * 60;
	let day = hour * 24;
	// let halfamonth = day * 15;
	let month = day * 30;
	let now = new Date().getTime();
	let diffValue = now - dateTimeStamp;
	if (diffValue < 0) { return; }
	let monthC = diffValue / month;
	let weekC = diffValue / (7 * day);
	let dayC = diffValue / day;
	let hourC = diffValue / hour;
	let minC = diffValue / minute;
	let result = '';
	if (monthC >= 1) {
		result = '' + parseInt(monthC) + '月前';
	} else if (weekC >= 1) {
		result = '' + parseInt(weekC) + '周前';
	} else if (dayC >= 1) {
		result = '' + parseInt(dayC) + '天前';
	} else if (hourC >= 1) {
		result = '' + parseInt(hourC) + '小时前';
	} else if (minC >= 1) {
		result = '' + parseInt(minC) + '分钟前';
	} else {
		result = '刚刚';
	}
	return result;
};
``` 
```
/**
 * 格式化成指定格式的时间
 * @param {Date} time 时间戳 s
 * @param {string} fmt 格式化的格式
 * @return {string} 格式后的时间
 */
export function timeFormat(time, fmt = 'yyyy-MM-dd hh:mm:ss') {
	time = new Date(time * 1000);
	let o = {
		'M+': time.getMonth() + 1, // 月份
		'd+': time.getDate(), // 日
		'h+': time.getHours(), // 小时
		'm+': time.getMinutes(), // 分
		's+': time.getSeconds(), // 秒
		'q+': Math.floor((time.getMonth() + 3) / 3), // 季度
		'S': time.getMilliseconds() // 毫秒
	};
	if (/(y+)/.test(fmt)) {
		fmt = fmt.replace(RegExp.$1, (time.getFullYear() + '').substr(4 - RegExp.$1.length));
	}
	for (var k in o) {
		if (new RegExp('(' + k + ')').test(fmt)) fmt = fmt.replace(RegExp.$1, (RegExp.$1.length === 1) ? (o[k]) : (('00' + o[k]).substr(('' + o[k]).length)));
	}
	return fmt;
};
```
```
/**
 * 获取倒计时
 * @param {date} haveTime (ms)
 */
export function getCountTime(haveTime, isData) {
	if (typeof haveTime !== 'number') {
		return false;
	}
	haveTime = Math.floor(haveTime / 1000);
	if (haveTime <= 0) {
		return false;
	}
	let day = Math.floor(haveTime / (3600) / 24);
	let hour = Math.floor(haveTime / (3600) - 24 * day);
	let minute = Math.floor((haveTime - 3600 * hour - 24 * 3600 * day) / 60);
	let second = haveTime % 60;
	const completenum = num => num < 10 ? ('0' + num) : num;
	if (isData) {
		return [day, hour, minute, second].map(item => completenum(item));
	}
	return `${day}天${hour}小时${minute}分钟${second}秒`;
};
```
```
//判断一个对象是否是空对象
自己常用的方法:
Object.keys(obj).length > 0

其他方法:
obj.valueOf() === '{}' // 不能使用toString() => 会变成 [Object, Object]

Object.getOwnPropertyNames(obj).length === 0
```
```
// 从a = [{key: 1, value: 'a'}, {key: 2, value: 'b'}]的结构中找出key为1的对象value
a.find(item => {return item.key === 1}).value;
```

> 当前宿主环境是否为浏览器
```
const inBrowser = typeof window !== 'undefined';
```

> console.dir // 可以用来查看node的具体信息

> 浅拷贝
let shallowCopy = obj => {
	if (typeof obj !== 'object') return;
	let newObj = obj instanceof Array ? [] : {};
	for (const key in obj) {
		if (obj.hasOwnProperty(key)) {
			newObj[key] = obj[key]''
		}
	}
	return newObj;
}

> 深拷贝
let deepCopy = obj => {
	if (typeof obj !== 'object') return;
	// let newObj = obj instanceof Array ? [] : {};
	let newObj = obj.constructor === Array ? [] : {};
	for (var key in obj) {
		if (typeof obj[key] === 'object') {
			deepCopy(obj[key]);
		} else {
			newObj[key] = obj[key];	
		}
	}
	return newObj；
}