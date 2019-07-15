---
title: JS 内置对象（原生方法）
date: 2019-03-01 13:58:37
author: T.2stt
comments: true
tags:
- js
- 对象OBJ
categories:
- [js,ms]
---
>`JS` 内置对象中我们常用的有关于 
数据封装类对象 - `Object` 、`Array` 、`Boolean` 、`Number` 、`String`
其他对象：`Function` 、`Arguments` 、`Math` 、`Date`、`RegExp`、`Error`
ES6 新增对象：`Symbol` 、`Map` 、`Set` 、`Promises` 、`Proxy` 、`Reflect` 详细介绍 {% post_link js-es6 %}
界面对象： `鼠标事件`、`表单事件` 、`键盘事件` 、 `窗口事件`、 `Event`、 `Window对象`、 `定时器`
[MDN学习地址](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)
# 数据封装类对象
## Object 基础对象
`es6新增加的方法` 【{% post_link js-es6 %}】
Object 含有所有 JavaScript 对象的特性的超类
Object.constructor 对象的构造函数
Object.hasOwnProperty( ) 检查属性是否被继承;
Object.isPrototypeOf( ) 一个对象是否是另一个对象的原型;
Object.propertyIsEnumerable( ) 是否可以通过 for/in循环看到属性;
Object.toLocaleString( ) 返回对象的本地字符串表示;
Object.toString( ) 定义一个对象的字符串表示;
Object.valueOf( ) 指定对象的原始值

## Array对象
### 属性
.length      获得数组的长度；

### 方法
.concat（） 连接内容或者数组，`组成新的数组`；
.join（n）  用 n `连接数组`的每一项组成字符串，可以是空字符串；
.unshift() 往数组 `前面` 添加一个或多个数组元素，长度要改变；
.shift() `删除` 数组中第一个元素，返回删除的那个值，并将长度减 1；
.push（）  在数组 `最后 ``加入`新的内容返回新的数组的长度
.pop（） `删除` 数组的 `最后一项` 数据，并返回删除的数据；
.reverse（） 翻转数组；
.delete  只能删除数组元素的值，`而所占空间还在`，总长度没变( arr.length )；
.slice( ) 返回数组的一部分；
.sort( ) 对数组元素进行排序；
.splice( ) 插入、删除或替换数组的元素；
.toLocaleString( ) 把数组转换成局部字符串；
.toString( ) 将数组转换成一个字符串；

## Boolean 布尔对象
Boolean.toString( ) 将布尔值转换成字符串;
Boolean.valueOf( ) Boolean对象的布尔值;

## Number 数值对象
Number.toFixed( ) 采用定点计数法格式化数字
Number.MAX_VALUE 最大数值
Number.MIN_VALUE 最小数值
Number.NaN 特殊的非数字值
Number.NEGATIVE_INFINITY 负无穷大
Number.POSITIVE_INFINITY 正无穷大
Number.toExponential( ) 用指数计数法格式化数字
Number.toLocaleString( ) 把数字转换成本地格式的字符串
Number.toPrecision( ) 格式化数字的有效位
Number.toString( ) 将—个数字转换成字符串
Number.valueOf( ) 返回原始数值

## String对象
### 属性
.length  获取字符串的长度。
### 方法
.charAt（n）  找到位置在 n（索引）上的字符;
.charCodeAt（n）  找到索引位置上的字符的编码：a = 97，A = 65;
.indexOf（'m'） 查找字符 m 在字符串中`第一次出现的索引`,如果没有找到返回 `-1`;
.lastIndexOf（'m'）  查找字符 m 在字符串中最后一次出现的索引；如果没有找到返回 -1;
.split（'n'）  以字符 n `分割字符串`，并返回一个数组，空字符串时分割每个字符，如果字符串中没有该字符，同样返回数组;
.substr（n，m）   截取字符串，从索引是 n 的位置开始截取，截取 m 个字符；如果只有一个参数 n ，那就是从 n 开始截取，截取到最后;
.substring（n，m）  截取字符串，从索引是 n 的位置开始截取，截取到第 m 个字符（m 取不到）,如果只有一个参数 n ，那就是从 n 开始截取，截取到最后;
.toLowerCase()   把字符串中的字母转化成小写;
.toUpperCase()   把字符串中的字母转化成大写;
--- 

# 其他对象
## Function 函数构造器
Function.apply( ) 将函数作为一个对象的方法调用；
Function.call( ) 将函数作为对象的方法调用；
Function.bind () 生成一个新函数；
Function.arguments[] 传递给函数的参数；
Function.caller 调用当前函数的函数；
Function.length 已声明的参数的个数；
Function.prototype 对象类的原型；
Function.toString( ) 把函数转换成字符串；

## Arguments 函数参数集合
arguments[ ] 函数参数的数组;
Arguments 一个函数的参数和其他属性;
Arguments.callee 当前正在运行的函数;
Arguments.length 传递给函数的参数的个数;

## Math 数学对象
### 方法
Math.PI         圆周率；
Math.pow(n,m)   n 的 m `次方`;
Math.sqrt()     求 `平方根`;
Math.abs(n)     n 到原点的距离（ n 的 `绝对值`）;
Math.round(n)   `四舍五入`取整;
Math.floor(n)   地板函数（`向下取整`）;
Math.ceil(n)    天花板函数 （`向上取整`）;
Math.random()   返回 0-1 的随机数（`取不到1`）;
【注意：】[n，m] 随机数为了能取到 n，m   Math.floor( Math.random() ) * ( m - n + 1 ) + n );
### 特殊字符
\r  回车
\n  换行

## Date对象 
var date = new Date（）
### 方法
.toLocaleString()   以当前本地格式显示时间；
date.getFullYear()   获取date对象的年份；
date.getMonth()   获取月份（0 - 11）对应 1 - 12 月；
date.getDate()   获取日期；
date.getHours（） 获取小时；
date.getMinutes（） 获取分钟；
date.getSeconds（） 获取多少秒；
date.getMilliSeconds（） 毫秒数；
date.getDay（）  获取星期几（ 0 - 6 ） 对应 周天至周六；
date.getTime()   从 `1970年` 开始到时间日期的毫秒值（时间戳）；
date.setFullYear   设置年份；

## RegExp 正则表达式对象
RegExp.test( ) 检测一个字符串是否匹配某个模式;
RegExp.exec( ) 通用的匹配模式;
RegExp.global 正则表达式是否全局匹配;
RegExp.ignoreCase 正则表达式是否区分大小写;
RegExp.lastIndex 下次匹配的起始位置;
RegExp.source 正则表达式的文本;
RegExp.toString( ) 把正则表达式转换成字符串;

## Error 异常对象
Error.message 可以读取的错误消息;
Error.name 错误的类型;
Error.toString( ) 把Error 对象转换成字符串;
EvalError 在不正确使用 eval()时抛出;
SyntaxError 抛出该错误用来通知语法错误;
RangeError 在数字超出合法范围时抛出;
ReferenceError 在读取不存在的变量时抛出;
TypeError 当一个值的类型错误时，抛出该异常;
URIError 由URl的编码和解码方法抛出;

# 界面对象
## 鼠标事件
onclick  鼠标点击事件
onmouseover   鼠标放上
onmouseout   鼠标离开
ondblclick   双击事件
onmousedown   鼠标按下
onmouseup    鼠标抬起
onmousemove    鼠标移动 

## 表单事件
onfocus  获得焦点
onblur  失去焦点
onsubmit   提交事件
onchange  发生改变时候
onreset  重置事件

## 键盘事件
onkeyup   按键抬起
onkeydown     按键 按下
onkeypress    键盘按下一次

## 窗口事件
onload  事件（页面加载完成后立即执行）
两种方法
- <script>window.onload=”init”；</script>   //init 为函数名
- <body onload=”init()”></body>   // init()  是一个完整的函数

## Event
保存事件发生时的相关信息
event.clientX:   事件发生时的X的坐标
event.clientY:   事件发生时Y的坐标
event.target     事件源
注意：event必须通过以实参传递给函数才能使用
Var obj = document.createElement(“标签名”);
document.body.appendChild(obj); 

## Window对象
alert（）
confirm （）   弹窗显示 确定  取消
prompt （）    输入框
window .open(“链接”，“name”，“设置”);
1、width
2、height
3、left
4、top
5、scrollbars    yes  no  滚动条
6、toolbar   yes no 工具栏
7、location    地址栏（浏览器）   
8、close（）   只有通过js代码打开的窗口才能关闭
每个个设置属性用“，”隔开，7和8需要用户允许

## 定时器：
`一次性定时器`
Var name1=window.setTimeout(“js代码”，时间t)
执行： 时间t之后执行js代码 。
周期：毫秒计算。
`反复性定时器`
Var name2=window.setInterval(“js代码”，时间t)
执行：每时间t就执行一次
周期以毫秒计算
`清除定时器：必须给定时器一个名字`
一次性定时器  windowclearTimeout(name1);
反复性定时器  window.clearInterval(name2);