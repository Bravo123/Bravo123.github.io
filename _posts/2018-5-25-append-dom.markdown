---
layout:     post
title:      "如何添加大量节点"
subtitle:   "要求：流畅"
date:       2018-05-25 20:29:04
author:     "Thq"
header-img: "img/post-bg-js-version.jpg"
catalog: true
tags:
    - JavaScript
---

> 任务： 如何在添加大量的节点的情况下，使得浏览器不卡顿，点击对应节点输出相应的innerHTML。

<br> 关键点： 事件委托 requestAnimationFrame documentFragement
<br> 代码中有很多细节，都写在注释里面了！

```
	(() => {
		// 'js-ul' 比较以前起名,'ul'、'wrapper-ul',前者很明显能够知道是要用js来操作改ul标签的，提高了可读性、可维护性,同理nd表示node的意思,根据个人习惯命名
		let ndUl = document.getElementById('js-ul');
		// 错误检测判断
		if (!ndUl) {
			return;
		}

		const total = 30000;
		const onceSize = 4;
		const count = total / onceSize;
		let doneCount = 0;

		// 通过一次添加比较少个的节点来提高流畅度
		// documentFragement不是真实DOM树的一部分,它的变化不会引起DOM树的重新渲染reflow,且不会导致性能问题。
		// 当插入多个节点的时候，使用documentFragement只会发生一个reflow操作，提高性能
		function appendItems() {
			const fragment = document.createDocumentFragment();
			for (let i=0; i<onceSize; i++) {
				let ndItem = document.createElement('li');
				ndItem.innerText = (doneCount * onceSize) + i;
				fragment.appendChild(ndItem);
			}

			ndUl.appendChild(fragment);
			doneCount++;
			doAppend();
		}

		// window.requestAnimationFrame() 方法告诉浏览器您希望执行动画并请求浏览器在下一次重绘之前调用指定的函数来更新动画。该方法使用一个回调函数作为参数，这个回调函数会在浏览器重绘之前调用。
		function doAppend() {
			if (doneCount < count) {
				window.requestAnimationFrame(appendItems);
			}
		}

		doAppend();

		// 事件委托输出InnerHTML,并非每一个节点都绑定事件
		ndUl.addEventListener('click', function(e) {
			const target = e.target;
			if (target.tagName === 'LI') {
				alert(target.innerHTML);
			}
		});

	})();
```