---
title: Javascript数据类型/深浅拷贝
date: 2017-3-31 17:05:06
author: T.2stt
comments: true
tags:
- typeof
- object
- js数据类型/深浅拷贝
categories:
- [js,ms]
---

> 数据类型的分类 + 如何判断的数据类型 + 基本类型和引用类型的区别（本质区别在于存储方式的不同造成了深拷贝浅拷贝的问题）
区别：存储位置不同
**基本类型** 的访问是 `按值` 访问的，就是说你可以操作保存在变量中的 `实际的值`---`栈内存`
**引用类型** 的访问是 `按指针` 访问的，需要 `栈和堆` 的值一起完成。

## 基本概念
**原始（基本）数据类型** 是直接存储在 `栈(stack)` 中的简单数据段，占据空间小、大小固定，属于被频繁使用数据；
**引用数据类型** 存储在 `堆(heap)` 中的对象，`占据空间大、大小不固定` ，如果存储在栈中，将会影响程序运行的性能；
**引用数据类型在** `栈` 中存储了 `指针` ，该指针 `指向堆中该实体的起始地址`。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。
**浅拷贝**： 对于 `基本数据` 的赋值，比如数组的元素或者对象的的值是Number型，String型。
**深拷贝**： 对于 `引用类型` 的赋值，比如数组、对象。

## 变量的基本类型
### 基本类型(5个)
基本类型是指简单的数据段，有 `Number`、`String`、`Boolean`、`Undefined`、`Null`
#### 赋值基本类型变量
`基本类型` 的数据是存放在 **栈内存** 中的，如果从一个变量向另一个变量复制基本类型的值，会在变量对象上 `创建一个新值`，然后把该值复制到 `为新变量分配的位置上` ，`两个变量之间相互独立`，互不影响。
```javascript
var num1 = 5;
var num2 = num1;
console.log(num2)  //5
num2 = 3;
console.log(num2)  //3
console.log(num1)  //5
```
{% asset_img base1.png 基本数据类型解释 %}

### 引用类型（5个）
引用类型指那些可能包含多个值的对象，有 `Object`、`Array`、`Function`、`Date`、`RegExp` 等
也可以说是就是对象了，`对象` 是 `属性` + `方法` 的集合也就是说引用类型可以拥有属性和方法，属性又可以包含基本类型和引用类型。
#### 赋值引用类型变量
`引用类型` 的数据是存放在 **堆内存** 中的，赋值实际是将存储在变量对象中的 `地址（指针）` 变为新变量存于 `栈`中。即两个变量指向通一个地址。因此，改变其中一个变量，就会影响另 一个变量，如下面的代码所示:
`var a = {a: 12}` `a` 存于栈中 `{a : 12}` 存于堆中

{% asset_img data3.png 引用数据类型赋值图解 %}
```javascript
var obj1 = new Object();
var obj2 = obj1;
obj1.name = "Nicholas";
console.log(obj2.name);  //"Nicholas"
```
{% asset_img data2.png 解释以上引用赋值的过程 %}

#### 解决引用类型赋值的问题（深拷贝）
**解决的办法：新建一个新的对象来保存需要赋值的对象数据。**
##### 数组
- 针对简单的数组对象，可以采取遍历的方式
```javascript
var array1 = [1,2,3,4];
var array2 = [];
array1.forEach( function ( item ) {
  array2.push(item)
})
console.log(array2)  // [1,2,3,4]
array2[0] = 0
console.log(array1)  // [1,2,3,4]
```
- 除此之外还有更简单的方法，使用Array对象的方法：**slice函数 、 concat函数，JQuery的$.makeArray()函数**
```javascript
// slice方法
var array1 = [1,2,3,4];
var array2 = array1.slice(0);
console.log(array2)  // [1,2,3,4]
array2[0] = 0;
console.log(array2)  // [0,2,3,4]
console.log(array1)  // [1,2,3,4]
// connact方法
var array1 = [1,2,3,4];
var array2 = array1.concat([]);
console.log(array2)  // [1,2,3,4]
array2[0] = 0;
console.log(array2)  // [0,2,3,4]
console.log(array1)  // [1,2,3,4]
// $.makeArray方法
var array1 = [1,2,3,4];
var array2 = $.makeArray(array1);
console.log(array2)  // [1,2,3,4]
array2[0] = 0;
console.log(array2)  // [0,2,3,4]
console.log(array1)  // [1,2,3,4]
// 三个函数的原理都是返回数组的一个副本（相当于另外开辟内存空间），所以并不会改变数组本身的的值
```
----
##### 对象
其实JavaScript 中的所有事物都是对象：**包括上面的数组，字符串、数值、函数...**
针对对象的赋值采取的方法也是 `遍历` 拷贝的方式
【注意如下】
```javascript
//copy函数
function copy(obj){
    var newObj = {};
    for ( var key in obj) {
        newObj[key] = obj[key];
    }
    return newObj;
}

var obj1 = {
  name:'Bob',
  country:'America'
};
var obj2 = copy(obj1);
console.log(obj2)  //{name:'Bob',country:'America'}
obj2.country = 'Chinese';
console.log(obj1)  //{name:'Bob',country:'America'}
// 其实上述代码还有一个问题，当对象中的country对象时一个对象或者数组的时候，就会导致存在对象引用的问题。我们队上述代码做一些改动


//copy函数
function copy(obj){
    var newObj = {};
    for ( var key in obj) {
        newObj[key] = obj[key];
    }
    return newObj;
}
var obj1 = {
  name:'Bob',
  country:['America','French']
};
var obj2 = copy(obj1);
console.log(obj2)  //{name:'Bob',country:['America','French']}
obj2.country[0] = 'Chinese';
console.log(obj1)  //{name:'Bob',country:['Chinese','French']}  //obj2改变了country的值，导致了obj1中的country也发生改变
// 这是因为copy函数只是对第一层进行拷贝，无法拷贝深层的对象，这个copy为浅拷贝函数，我们需要通过递归，来拷贝深层的对象。将copy改造成递归函数。


function deepCopy(obj) {
    if (typeof obj != 'object') {
        return obj;
    }
    var newObj = {};
    if (obj instanceof Array) {
        newObj = [];
    } 
    for (var key in obj) {
        if (obj.hasOwnProperty(key)) {
            newObj[key] = deepCopy(obj[key]);
        }
    }
    return newobj;
}
```
【深拷贝的思路是】：先判断参数是不是对象，如果不是对象直接return 返回，如果参数是对象，则新建一个对象，然后在区分是不是Array数组，最后for in遍历。

### 类型demo
`JavaScript` 是弱语言类型，我们无法在申明的时候规定他的类型，`JavaScript` 变量的类型是随变量的值改变而改变的。所以我们要判断变量值的类型。下图列举一些常见的类型：

|      类型      |       举例        |
| :----------: | :-------------: |
|   **Null**   |      null       |
| **Undefined** | undefined |
| **Boolean**  |   true、false    |
|  **Number**  |  -1、0 、 1、 NaN  |
|  **String**  |     '1'、'a'     |
|  **Array**   | []、new Array()  |
|  **Object**  | {}、new Object() |
| **Function** |  function(){}   |

用`typeof`去检测它们的类型时，你就崩溃了：明明是`Null`为什么结果却是`oject`，明明是`Array`为什么还是`obejct`？...所以除了使用`typeof`方法外我们必须要寻找其他的方法，那么这些方法有哪些呢？请继续往下读(`para`表示要判断的变量)：

- isNaN(para)
- !para
- typeof para
- Object.prototype.toString.call(para);

## 判断类型的方法
### isNaN(para)
用来确定一个值是否为 `NaN` 。**isNaN(para)和 Number.isNaN(para)（升级版）功能一样** 值是否为 `number` 类型的专有方法。但是需要注意的是，如果使用 `typeof` 判断那么结果会是 `number`。

### !para
常用来判断一个变量是否存在，面对 `Arry`、`Object` 等 **引用类型变量** 时无论是否为空都会被转换成 `true`

### typeof para
**返回字符串**
事实证明typeof并不是万能的，在对除 `Null` 以外的 **基本类型** 变量是相当有威力的，但是对 **引用类型变量** 和 `null` 时都会被识别成   `object`(指针问题，被误认为obj)

### Object.prototype.toString.call(para)
前面的 `typeof` 死在半路，无法打探到 **引用类型变量** 和 `null` 的真实情报，但是我们得出了另一个情报：**他们都是`obejct`** 。那么我们就完全可以利用 `Object` 原型上的 `toString()`方法来判断

### instanceof
**object instanceof constructor**[官网解释](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/instanceof)
- `object` : 要检测的对象
- `constructor` : 某个构造函数
```javascript
`instanceof` 运算符用来检测 `constructor.prototype` 是否存在于参数 `object` 的原型链上
function Car(make, model, year) {
  this.make = make;
  this.model = model;
  this.year = year;
}
var auto = new Car('Honda', 'Accord', 1998);

console.log(auto instanceof Car);
// expected output: true

console.log(auto instanceof Object);
// expected output: true
```
### 判断结果比较表

|      类型      |      值       | isNaN(para) |   !para    | typeof para |  toString(para)   |
| :----------: | :----------: | :---------: | :--------: | :---------: | :---------------: |
|   **Null**   |     null     |    true     |    true    |   'obejct'    |   [obejct Null]   |
| **Undefined** |   undefined |    true     |    true    |  'undefined'  | [obejct Undefined] |
| **Boolean**  |  true/false  |    true     | false/true |   'boolean'   | [obejct Boolean]  |
|  **Number**  |      -1      |    false    |   false    |   'number'    |  [obejct Number]  |
|  **Number**  |      0       |    false    |    true    |   'number'   |  [obejct Number]  |
|  **Number**  |      1       |    false    |   false    |   'number'    |  [obejct Number]  |
|  **Number**  |     NaN      |    true     |    true    |   'number'    | [obejct  Number]  |
|  **String**  |     '1'      |    false    |   false    |   'string'    |  [obejct String]  |
|  **String**  |     'a'      |    true     |   false    |   'string'    |  [obejct String]  |
|  **String**  |      ''      |    true     |    true    |   'string'    |  [obejct String]  |
|  **String**|' '(中间包含空格)|    true     |   false    |   'string'    |  [obejct String]  |
|  **Array**   |    []/[4]    |    true     |   false    |   'obejct'    |  [obejct Array]   |
|  **Object**  |   {}/{n:4}   |    true     |   false    |   'obejct'    |  [obejct Object]  |
| **Function** | function(){} |    true     |   false    |   'function'    | [obejct Function] |
| **Symbol(es6)** | - |     -    |   -    |   'symbol'    | - |

### 方法选择
根据上面的表格对比，我整理了一些常见的方法。并且再比较结果精准的情况下尽可能的简化比较过程.

#### 判断数字 isNaN + typeof
##### 非严格方法 isNaN
字符串'`1`'会被识别成`number`
``` javascript
function isNumber(para){
    return !isNaN(para);
};
```

##### 严格方法 isNaN + typeof
在必要的情况下使用：此方法会把字符串'`1`'识别成`string`类型
``` javascript
function isStrictNumber(para){
    return !isNaN(para) && typeof para === 'number';
};
```

#### 判断字符串 isNaN + typeof
##### 非严格方法 typeof
``` javascript
function isString(para){
    return typeof para === 'string';
};
```

##### 严格方法 isNaN + typeof
在必要的情况下使用：此种方法会把字符串'`1`'识别成`number`类型
``` javascript
function isStrictString(para){
    return isNaN(para) && typeof para === 'string';
};
```

#### 判断基本数据类型(非引用类型)
注意：使用typeof判断`null`结果为`object`
``` javascript
function isBasicType(para){
    return typeof para !== 'obejct';
};
```

#### 判断是否为null(不能识别'')
此方法只能识别`null`，如果要包含''，请结合方法`isStringNull()`一起使用
``` javascript
function isNull(para){
    return !para && typeof para === 'object';
};
```

#### 判断是否为空字符串(不包含空格)
此方法只能识别`''`，如果要包含`null`，请结合方法`isNull()`一起使用
``` javascript
function isStringtNull(para){
    return !para && typeof para === 'string';
};
```

#### 判断是否为`undefined`
``` javascript
function isUndefined(para){
    return typeof para === 'undefined';
};
```

#### 判断是否为`false`
当为`null`,`undefined`,`''`,`0`,`-0`,`false`,`NaN`
``` javascript
function isFalse(para){
    return !para;
};
```

#### 判断对象（非严格1--所有的obejct对象）
``` javascript
function isAllObject(_v){
    return typeof _v === 'obejct';
};
```

#### 判断对象（非严格2--除去null的所有object对象）<--> 判断引用类型
``` javascript
function isObject(_v){
    return !!v && typeof _v === 'obejct';
};
```

#### 判断对象（严格--只识别{}JSON对象）
``` javascript
function isStrictObject(_v){
    return Object.prototype.toString.call(_v) === '[object Object]';
};
```

#### 判断数组
``` javascript
function isArray(para){
    return Object.prototype.toString.call(para) === '[object Array]';
};
```

#### 判断对象
这里特指{}类`JSON`对象
``` javascript
function isObject(para){
    return Object.prototype.toString.call(para) === '[object Object]';
};
```

#### 判断可执行函数
``` javascript
function isFunction(para){
    return typeof para === 'function';
};
```
------------
## 总结
1. 确定一个值是哪种`基本类型` 可以使用 `typeof` 操作符(无法检测对象或 null)，而确定一个值是哪种 `引用类型` 可以使用`instanceof` 操作符
2. 当前的一些库`underscore.js`、`lodash.js`等~
