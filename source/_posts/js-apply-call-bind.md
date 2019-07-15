---
title: Function对象方法apply-call-bind理解与比较
date: 2019-02-25 17:20:43
author: T.2stt
comments: true
tags:
- [js]
categories:
- [js,ms]
---
>改变函数内部this的指向
每一个Function对象都有一个apply()方法和一个call()方法 
apply() 方法调用一个具有给定this值的函数，以及作为一个数组（或类似数组对象）提供的参数。
call() 方法调用一个函数, 其具有一个指定的this值和分别地提供的参数(参数的列表)。
bind() 方法创建一个新的函数，在调用时设置this关键字为提供的值。并在调用新函数时，将给定参数列表作为原函数的参数序列的前若干项。
注意：apply和call的功能是一样的，只是传入的参数列表形式不同,call()方法接受的是若干个参数的列表，而apply()方法接受的是一个包含多个参数的数组。
[学习网址](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

## apply
func.apply(thisArg, [argsArray])：调用一个具有给定this值的函数
### 个人理解：隐含使用for循环
```bash
参数：
thisArg表示在 func 函数运行时使用的 this 值-可选。请注意，this可能不是该方法看到的实际值：如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象，原始值会被包装。
argsArray表示一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 func 函数-可选。如果该参数的值为 null 或  undefined，则表示不需要传入任何参数。
返回值：
调用有指定this值和参数的函数的结果。
```
### demo（将数组添加到另一个数组）
1.使用push将元素追加到数组中；
2.concat；
3.apply方案
```bash
var array = ['a', 'b'];
var elements = [0, 1, 2];
array.push.apply(array, elements);
console.info(array); // ["a", "b", 0, 1, 2]
```
### 使用apply和内置函数
会使用Math.max/Math.min来找出一个数组中的最大/最小值
### 使用apply来链接构造器

## bind
func.bind(thisArg[, arg1[, arg2[, ...]]]：创建一个新的函数
[学习地址](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)
```bash
参数：
thisArg:调用绑定函数时作为this参数传递给目标函数的值。 如果使用new运算符构造绑定函数，则忽略该值。当使用bind在setTimeout中创建一个函数（作为回调提供）时，作为thisArg传递的任何原始值都将转换为object。如果bind函数的参数列表为空，执行作用域的this将被视为新函数的thisArg。
arg1, arg2, ...:当目标函数被调用时，预先添加到绑定函数的参数列表中的参数。
返回值：
返回一个原函数的拷贝，并拥有指定的this值和初始参数。
```
### demo
1.react项目中方法使用需要在构造函数中bind一下，用原始的对象创建一个绑定函数；

### 注意
1.bind只生效一次！
```bash
function f(){
  return this.a;
}

var g = f.bind({a:"azerty"});
console.log(g()); // azerty

var h = g.bind({a:'yoo'}); // bind只生效一次！
console.log(h()); // azerty

var o = {a:37, f:f, g:g, h:h};
console.log(o.f(), o.g(), o.h()); // 37, azerty, azerty
```
## call
fun.call(thisArg, arg1, arg2, ...)：调用一个函数, 其具有一个指定的this值和分别地提供的参数(参数的列表)
[学习地址](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/call)
```bash
参数：
thisArg:在fun函数运行时指定的this值。需要注意的是，指定的this值并不一定是该函数执行时真正的this值，如果这个函数处于non-strict mode，则指定为null和undefined的this值会自动指向全局对象(浏览器中就是window对象)，同时值为原始值(数字，字符串，布尔值)的this会指向该原始值的自动包装对象。
arg1, arg2, ...:指定的参数列表。
返回值：
使用调用者提供的this值和参数调用该函数的返回值。若该方法没有返回值，则返回undefined。
```
### 常用用法
#### 验证是否是数组
```javascript
function   isArray(obj){ 
    return Object.prototype.toString.call(obj) === '[object Array]' ;
}
```

## 个人总结
```bash
bind- 返回对应函数，便于稍后调用；
call、apply- 立即调用
            1. 调用某个函数，并制定函数内this是谁，apply传数祖为参数，call传多个参数，
                如果要想把 this 的值从一个环境传到另一个，就要用 call 或者apply 方法;
             2. 调用一个对象的一个方法，用另一个对象替换当前对象，例如：B.apply(A, arguments);即A对象应用B对象的方法。
             3. 调用一个对象的一个方法，用另一个对象替换当前对象，例如：B.call(A, args1,args2);即A对象调用B对象的方法。
```
