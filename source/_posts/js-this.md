---
title: JS的this相关
date: 2019-02-27 12:29:29
author: T.2stt
comments: true
tags:
- this问题
categories:
- [js,ms]
---
> `this` 的不同问题 + ES6 下面相关的 `this` 指向问题,从数据存储的方向解释 `this` 的原理, 以及相关的注意事项，进一步的了解运行环境的指向问题

## 问题的由来
```javascript
var obj = {
foo: function () {}
};
var foo = obj.foo;
// 写法一
obj.foo()

// 写法二
foo()
```
上面代码中，虽然 `obj.foo` 和 `foo` 指向同一个函数，但是执行结果可能不一样。请看下面的例子。
```javascript
var obj = {
    foo: function () { console.log(this.bar) },
    bar: 1
};
var foo = obj.foo;
var bar = 2;
obj.foo() 
foo() 

// 输出的分别是1和2
```
【解释】
`this指的是函数运行时所在的环境` 。对于 `obj.foo()` 来说，`foo` 运行在 `obj` 环境，所以 `this` 指向 `obj` ；对于 `foo()` 来说，`foo` 运行在全局环境，所以 `this` 指向全局环境。所以，两者的运行结果不一样。

## 内部存储的结构
### 数据的存储结构
JavaScript 语言之所以有this的设计，跟内存里面的数据结构有关系
`var obj = { foo:  5 };`
上面的代码将一个对象赋值给变量 `obj`。JavaScript 引擎会先在内存里面，生成一个对象 `{ foo: 5 }`，然后把这个对象的内存地址赋值给变量obj。
{% asset_img varSaveImg.png 数据的存储结构 %}
也就是说，变量 `obj` 是一个地址（reference）。后面如果要读取 `obj.foo`，引擎先从obj拿到内存地址，然后再从该地址读出原始的对象，返回它的 `foo` 属性。
原始的对象以字典结构保存，每一个属性名都对应一个属性描述对象。举例来说，上面例子的foo属性，实际上是以下面的形式保存的。
{% asset_img varReallySaveImg.png 原始的对象以字典结构保存，每一个属性名都对应一个属性描述对象。举例来说，上面例子的foo属性，实际上是以下面的形式保存的。 %}
```javascript
{
    foo: {
      [[value]]: 5
      [[writable]]: true
      [[enumerable]]: true
      [[configurable]]: true
    }
}
```
【注意】:`foo` 属性的值保存在属性描述对象的 `value` 属性里面。

### 函数的存储结构
`var obj = { foo: function () {} };`
这时，引擎会将函数单独保存在内存中，然后再将函数的地址赋值给 `foo` 属性的 `value` 属性。
{% asset_img funSaveImg.png 方法的存储结构 %}
```javascript
{
    foo: {
      [[value]]: 函数的地址
      ...
    }
}
// 由于函数是一个单独的值，所以它可以在不同的环境（上下文）执行。
var f = function () {};
var obj = { f: f };

// 单独执行
f()

// obj 环境执行
obj.f()
```

## 环境变量（this的出现）
`JavaScript` 允许在函数体内部，引用当前环境的其他变量。
```javascript
var f = function () {
    console.log(x);
};
```
上面代码中，函数体里面使用了变量x。该变量由运行环境提供。现在问题就来了，由于函数可以在不同的运行环境执行，所以需要有一种机制，能够在函数体内部获得当前的运行环境（context）。所以，**this** 就出现了，**它的设计目的就是在函数体内部，<font color="red">指代函数当前的运行环境。</font>**
```javascript
var f = function () {
   console.log(this.x);
}
// 上面代码中，函数体里面的this.x就是指当前运行环境的x。
```
```javascript
var f = function () {
    console.log(this.x);
}
var x = 1;
var obj = {
    f: f,
    x: 2,
};
f()
obj.f()

// f() // 单独执行 =>1
// obj.f()在obj 环境执行 =》 2
// 上面代码中，函数f在全局环境执行，this.x指向全局环境的x。
```
上面代码中，函数f在全局环境执行，this.x指向全局环境的x。
{% asset_img optionImg.png 具体执行过程 %}
{% asset_img objThis.png 在 obj 环境执行，this.x 指向 obj.x %}
回到本文开头提出的问题，`obj.foo()` 是通过 `obj` 找到 `foo`，所以就是在 `obj` 环境执行。一旦 `var foo = obj.foo`，变量 `foo` 就直接指向函数本身，所以 `foo()` 就变成在全局环境执行。

## 在构造函数的prototype中，this代表着什么
{% asset_img prototypeDemo.png %}
在Fn.prototype.getName函数中，this指向的是f1对象。因此可以通过this.name获取f1.name的值。其实，不仅仅是构造函数的prototype，即便是在整个原型链中，this代表的也都是当前对象的值。

## 【注意】+ this 使用场景
### this 使用场景
- 全局对象中的this：指向window
- 函数中的this
- 使用call，apply显示指定this
- 构造函数与原型方法上的this
　　　　通过new操作符调用构造函数，会经历以下4个阶段。
　　　　（1）**创建一个新的对象** ；
　　　　（2）将构造函数的this指向这个新对象；（obj.__proto__ = Fn.prototype;）
　　　　（3）指向构造函数的代码，为这个对象添加属性，方法等；
　　　　（4）返回新对象。
因此，当new操作符调用构造函数时，this其实指向的是这个新创建的对象，最后又将新的对象返回出来，被实例对象接收。因此，我们可以说，这个时候，`构造函数的this，指向了新的实例对象`。

- 箭头函数中的this
### 【注意】
-  谁调用，`this` 指向谁 `xxx.fun()`（<font color="red">自执行函数的this永远是window，不管它是否在哪里被调用;</font>）
```javascript
var myObject = {
  foo: "bar",
  func: function() {
      var self = this;
      (function() {
          console.log(this);
      }());
  }
};
myObject.func();
输出：Window {postMessage: ƒ, blur: ƒ, focus: ƒ, close: ƒ, frames: Window, …}
```
- new一个对象，`this` 指向实例本身 `var c = new fun()` => 实例化后的fun;
- 使用`call / apply / bind `修改 `this` 指向。{% post_link js-apply-call-bind %};
- `prototype` 的 `this` 代表当前对象的值;
- 补充关于箭头函数的this:箭头函数其实是没有 `this` 的，这个函数中的 `this` 只取决于他外面的第一个不是箭头函数的函数的 `this`。在这个例子中，因为调用 a 符合前面代码中的第一个情况，所以 this 是 window。并且 `this` 一绑旦定了上下文，就不会被任何代码改变。
```javascript
function a() {
    return () => {
        return () => {
            console.log(this)
        }
    }
}
console.log(a()()())
```