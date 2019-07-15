---
title: Js-面试-常识温习
date: 2019-03-01 10:53:58
author: T.2stt
comments: true
tags:
- js
categories:
- [js,ms]
---
>关于 JS 的一些基础问题的统计搜集
JavaScript的组成、的基本数据类型和引用数据类型、检测浏览器版本版本方式、JS 内置对象

## JavaScript 的组成
JavaScript 由以下三部分组成：
- ECMAScript（核心）：提供核心语言功能 (Js 语言基础---语法、语句、类型、关键字...)
- DOM（文档对象模型）：提供 **访问和操作网页内容的方法和接口**
- BOM（浏览器对象模型）：提供与 **浏览器交互的方法和接口** (移动、缩放、navigator、screen、cookies...)

## JS 的基本数据类型和引用数据类型
- 基本数据类型：undefined、null、boolean、number、string、symbol
- 引用数据类型：object、array、function
- 详细介绍 {% post_link js-data-type %}

## 检测浏览器版本版本方式
- 根据 navigator.userAgent // UA.toLowerCase().indexOf('chrome')
- 根据 window 对象的成员 // 'ActiveXObject' in window
```javascript
//浏览器类型及版本
function getBrowserInfo() {
	var agent = navigator.userAgent.toLowerCase();
	var regStr_ie = /msie [\d.]+;/gi;
	var regStr_ff = /firefox\/[\d.]+/gi
	var regStr_chrome = /chrome\/[\d.]+/gi;
	var regStr_saf = /safari\/[\d.]+/gi;
	var isIE = agent.indexOf("compatible") > -1 && agent.indexOf("msie" > -1); //判断是否IE<11浏览器  
	var isEdge = agent.indexOf("edge") > -1 && !isIE; //判断是否IE的Edge浏览器  
	var isIE11 = agent.indexOf('trident') > -1 && agent.indexOf("rv:11.0") > -1;
	if (isIE) {
		var reIE = new RegExp("msie (\\d+\\.\\d+);");
		reIE.test(agent);
		var fIEVersion = parseFloat(RegExp["$1"]);
		if (fIEVersion == 7) {
			return "IE/7";
		} else if (fIEVersion == 8) {
			return "IE/8";
		} else if (fIEVersion == 9) {
			return "IE/9";
		} else if (fIEVersion == 10) {
			return "IE/10";
		} 
	} //isIE end 
	if (isIE11) {
		return "IE/11";
	}
	//firefox
	if (agent.indexOf("firefox") > 0) {
		return agent.match(regStr_ff);
	}
	//Safari
	if (agent.indexOf("safari") > 0 && agent.indexOf("chrome") < 0) {
		return agent.match(regStr_saf);
	}
	//Chrome
	if (agent.indexOf("chrome") > 0) {
		return agent.match(regStr_chrome);
	}
}
// 运用
var browser = getBrowserInfo(); // 取到完整信息
var b_name = (browser + "").replace(/[0-9./]/ig, ""); // 根据正则将所有数字、‘.’‘/’全部去掉，剩下浏览器名
var b_version = parseInt((browser + "").replace(/[^0-9.]/ig, "")); // 根据正则将所有非数字全部去掉，剩下版本
```

## 介绍 JS 有哪些内置对象
- 数据封装类对象：Object、Array、Boolean、Number、String
- 其他对象：Function、Arguments、Math、Date、RegExp、Error
- ES6 新增对象：Symbol、Map、Set、Promises、Proxy、Reflect
- 详细介绍 {% post_link js-built-in-obj %}

## 说几条写 JavaScript 的基本规范？
- Eslint语法检查规范（空格、对齐等问题、字符串用单引号）
- 代码段使用花括号 {} 包裹
- 变量和函数在使用前进行声明
- 以`大写字母开头` 命名构造函数，`全大写` 命名 `常量`
- 规范定义 JSON 对象，补全双引号

## 如何编写高性能的 JavaScript
- 遵循严格模式："use strict";
- 将 js 脚本放在页面底部，加快渲染页面
- 将 js 脚本将脚本成组打包，减少请求
- 使用非阻塞方式下载 js 脚本
- 尽量使用局部变量来保存全局变量
- 尽量减少使用闭包
- 使用 window 对象属性方法时，省略 window
- 尽量减少对象成员嵌套
- 缓存 DOM 节点的访问
- 通过避免使用 eval() 和 Function() 构造器
- 给 setTimeout() 和 setInterval() 传递函数而不是字符串作为参数
- 尽量使用直接量创建对象和数组
- 最小化重绘(repaint)和回流(reflow)


## JS 获取元素的宽高、位置等
offsetWidth / offsetHeight, clientWidth / clientHeight 与 scrollWidth / scrollHeight 的区别

`offsetWidth / offsetHeight`(IE的盒模型出去margin) : 包含 `content` + `padding` + `border`，效果与 `e.getBoundingClientRect()` 相同
`clientWidth / clientHeight` : 包含 `content` + `padding` ，如果有滚动条，也`不包含滚动条`
`scrollWidth / scrollHeight` : 包含 `content` + `padding` + `溢出内容的尺寸`

<font color="red">详细介绍</font> 【{% post_link js-size-address %}】

## DOM 元素 e 的 e.getAttribute(propName)和 e.propName 有什么区别和联系

e.getAttribute()，是标准 DOM 操作文档元素属性的方法，具有通用性可在任意文档上使用，返回元素在源文件中设置的属性
e.propName 通常是在 HTML 文档中访问特定元素的特性，浏览器解析元素后生成对应对象（如 a 标签生成 HTMLAnchorElement），这些对象的特性会根据特定规则结合属性设置得到，对于没有对应特性的属性，只能使用 getAttribute 进行访问
e.getAttribute()返回值是源文件中设置的值，类型是字符串或者 null（有的实现返回""）
e.propName 返回值可能是字符串、布尔值、对象、undefined 等
大部分 attribute 与 property 是一一对应关系，修改其中一个会影响另一个，如 id，title 等属性
一些布尔属性<input hidden/>的检测设置需要 hasAttribute 和 removeAttribute 来完成，或者设置对应 property
像<a href="../index.html">link</a>中 href 属性，转换成 property 的时候需要通过转换得到完整 URL
一些 attribute 和 property 不是一一对应如：form 控件中<input value="hello"/>对应的是 defaultValue，修改或设置 value property 修改的是控件当前值，setAttribute 修改 value 属性不会改变 value property