---
title: 一道题目引发对js支持正则表达式相关方法的探讨
date: 2017-11-24 9:57:10
author: T.2stt
comments: true
tags:
- Regex
- javascript
categories:
- [正则Regex]
---

> 此文说一说关于javascript中关于正则的几个方法中被很多人忽略的地方。
-工具

# 正则常用工具

- [regexper](https://regexper.com) 我最常用的一个，个人觉得UI做得比其他好
- [regulex](https://jex.im/regulex/) 备选，他有一个很舒心的功能，可以提供一段js，嵌套到你的网站，生成正则可视化图

---

# 题目

> 写一个方法使得数字末尾的连续0变成9，如1230000变成1239999

一道很简单的题目，直接正则就能搞定，也许你会写：

``` javascript
function zoreToNine(num){
    return (num + '').replace(/0/g,9);
}
//或者
function zoreToNine(num){
    return (num + '').replace(/[1-9]0+$/,9);
}
```

这也是此题的陷阱所在，按照上面的方法，1023000就会被转化成1923999，这样是不符合要求的，所以改进一下：

``` javascript
function zoreToNine(num){
    return (num + '').replace(/[1-9]0+$/,function($1){
        return $1.replace(/0/g,9);
    });
}
zoreToNine(1223000); //1223999
zoreToNine(1023000); //1023999
```

关于这个问题的解决方案[@微醺岁月](https://segmentfault.com/u/jawil)同学提供了一种，位置匹配的方法，简单了很多，厉害！

``` javascript
"12300100000".replace(/0(?=(0+$)|\b)/g,9); //12300199999
```
`javascript`支持正则表达式相关方法，注意并不是正则对象的方法。上述方法使用了正则，<font color="red">在回调函数里有一个$1</font>，这个$1到底是什么？所有的匹配规则匹配后都有$1这个变量么？...以下进行解释

---
# 主角

`javascript`中正则对象有三个方法：`test`、`exec`和`compile`，
但是此次讨论的是能够使用正则表示的相关方法：`search`、`match`、`replace`和`split`，注意它们都是`String`对象的方法，使用它们必须要是<font color="red">String</font>``类型.

## replace
```javascript
replace(rule[regexp/substr], replacement)
`replace`是一个用于替换字符串的方法，具体分析一下它的特点:
1. 它接收两个参数
2. 无副作用不影响原始变量
3. 返回被改变的字符串(一定是字符串类型)
```

定义一些变量，方便全文取用。

``` javascript
let a = '12309800', b = '12309800[object Object]', b = '12309800{}';
```

### 参数rule

在一般情况，rule参数一般是正则(如果是正则，则按照正则的规则进行匹配)、字符串(如果是字符串，将会在匹配到第一个符合条件的目标，结束方法)、数字。


``` javascript
//匹配第一个0替换成5
a.replace(0,5); //'12359800'
//匹配所有的0替换成5
a.replace(/0/g,5); //'12359855'
```

### 参数replacement

```javascript
在一般情况，replacement参数是字符串、数字、者回调。
```

#### 包含$的字符串

当参数rule为正则，并且正则至少包含有一对完整的`()`时，如果`replacement`包含有$的字符串，那么对于`$n`(n为大于0的整数，n的长度取决于正则中括号的对数)，会被解析成一个变量。但是也仅仅只是作为一个变量，无法在字符串中进行计算，此时更类似特别的字符串模板变量。

一般情况下，`$n`中n的长度取决于正则中括号的对数，$1表示第1对括号匹配的结果，$2表示第2对匹配的结果...在正则所有的括号对中，左括号出现在第几个位置（或者说从左往右），则它就是第几对括号，以此类推。姑且我们把这种规则成为`正则匹配分割规则`（ps:这完全是我自己取的一个名字，方便文章后面使用和记忆）。

``` javascript
a.replace(0,'$0'); //'123$09800'
a.replace(/00/g,'$0'); //'123098$0'
a.replace(/[1-9]0+$/,'$1'); //'12309$1'
a.replace(/([1-9](0+$))/,'$1'); //'12309800'，此时$1为[1-9](0+$)匹配到的内容，$2为0+$匹配到的内容
a.replace(/([1-9])(0+$)/,'$1'); //'123098'，此时$1为[1-9]匹配到的内容，$2为0+$匹配到的内容
a.replace(/([1-9])(0+$)/,'$1*$2'); //'123098*00'，此处的$1和$2不会安照期待的情况进行乘法计算，要进行计算可以用回调
```
<font color="red">【注意：】</font>
虽然目前参数replacement中携带有$n仍然能正常使用，但是这种方式已经不被[规范](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Deprecated_and_obsolete_features#RegExp_Properties)所推荐，更应该使用回调来完成这个操作如果正则中包含有全局匹配标志(g)，那么每次匹配的都符合上述规则

#### 回调函数

demo：

``` javascript
a.replace(/[1-9]0+$/,function(){
    console.log(arguments); //["800",5,"12309800"]、
});
a.replace(/([1-9])0+$/,function(){
    console.log(arguments); //["800","8",5,"12309800"]
});
a.replace(/([1-9])(0+$)/,function(){
    console.log(arguments); //["800","8","00",5,"12309800"]
});
a.replace(/(([1-9])(0+$))/,function(){
    console.log(arguments); //["800","800","8","00",5,"12309800"]
});
```

回调函数的`arguments`数组部分组成：[完整匹配的字符串,$1,$2,...,$n,匹配的开始位置,原始字符串],`$1...$n`表示每个括号对的匹配，规则和前面的相同。
所以有一下规律：

``` javascript
let arr = [...arguments], len = arr.length;
(len >= 3) === true;
arr[0] = 完整匹配的字符串;
arr[len-2] = 匹配的开始位置;
arr[len-1] = 原始字符串;
```

<font color="red">【注意：】</font>
除了匹配的开始位置是`Number`类型外，其余的都是`String`类型

### 非常规类型参数

如果参数类型不是上述两种情况，会发生什么呢？看看下面的例子：

``` javascript
a.replace(0,null); //123null9800
a.replace(0,undefined); //123null9800
a.replace(0,[]); //1239800
a.replace(0,Array); //1230,3,123098009800
b.replace({},5); //123098005
c.replace({},5); //'12309800{}'
a.replace(0,{}); //123[object Object]9800
a.replace(0,Object); //12309800
```

由上面的例子可以看出，如果非正则也非字符串，则有以下规则:
    <font color="orange">null</font> 变量，则会转换成`'null'`字符串;
    <font color="orange">undefined</font> 变量，则会转换成`'undefined'`字符串;
    <font color="orange">[]</font> 变量，则会调用`join()`方法转换成字符串，默认以`,`分割，值得注意的是空数组将会被转换成空字符串（没有任何字符），通常会被匹配源字符串的开始位置（默认开始位置为空字符串）;
    <font color="orange">Array</font> 变量，则会先转成成一个匹配的数组，形如`[完整匹配的字符串,$1,$2,...,$n,匹配的开始位置,原始字符串]`,然后对它调用`join()`方法转换成字符串，默认以`,`分割;
    <font color="orange">{}</font> 变量，则会调用`Object.protype.toString.call()`方法把`{}`转换成`[object Object]`;
    `Object`变量，则貌似什么都没做

虽然可以传入这些非正常参数，但大多数情况下这些类型的参数对实际是毫无意义的，所以不建议传入以上类型的参数。

## match
```javascript
match(rule[regex/substr])
`match`方法可在字符串内检索指定的值，或找到一个或多个正则表达式的匹配。
该方法类似`indexOf`和`lastIndexOf`，但是它返回指定的值，而不是字符串的位置；
```

### 参数rule

参数的传递除了常规的正则和字符串以外，其余所有类型的参数都会按照上述的`正则匹配参数转换规则`转换成字符串形式来匹配。

### 返回值

返回值根据传入的参数类型和规则的不同，返回的内容不同，但总体来说，它是返回一个对象，而不是索引，如果没匹配到任何符合条件的字符串，则返回`null`。

### 非全局匹配正则

如果匹配规则是一个非全局匹配规则，那么，它此时的返回值是一个伪数组对象(likeArr)，形如:[一个展开的匹配到的字符串数组, 匹配到的字符串位置， 原始字符串]，它有如下规律：

``` javascript
var likeArr = a.match(regex);
likeArr[0] = 匹配到的字符串;
likeArr[1...n] = 正则匹配分割规则匹配的字符串;
likeArr.index = 匹配到字符串的位置
likeArr.inupt = 原始字符串
```

看例子：

``` javascript
a.match(/[1-9]0+$/); //[0:'800',index:5,input:'12309800']
a.match(/([1-9])0+$/); //[0:'800',1:'8',index:5,input:'12309800']
a.match(/[1-9](0+$)/); //[0:'800',1:'00',index:5,input:'12309800']
a.match(/([1-9])(0+$)/); //[0:'800',1:'8',2:'00',index:5,input:'12309800']
```

### 全局匹配正则

如果匹配规则是一个全局匹配规则(正在携带有g标志)，那么，它此时的返回值是一个数组对象(arr)，形如:[匹配到的字符串数1,匹配到的字符串数2,匹配到的字符串数3];
看例子：

``` javascript
a.match(/[1-9]0/); //[0:'30',index:2,input:'12309800']
a.match(/[1-9]0/g); //[0:'30',1:'80']
```

## search
```javascript
search(rule[regex/substr])
`search`方法用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。
`stringObject`中第一个与`rule`相匹配的子串的起始位置。如果没有找到任何匹配的子串，则返回`-1`。

```
<font color="red">【注意:】</font>
-`search`方法不执行全局匹配，它将忽略标志`g`。
-忽略`regexp`的`lastIndex`属性，总是从字符串的开始进行检索，这意味着它总是返回`stringObject`的第一个匹配的位置

同样，`search`可以传入任何参数类型，它会遵循`正则匹配参数转换规则`进行转换

## split
```javascript
split(rule[regex/substr],len)
这个方法就不用多说，很常用的字符串分割方法。
第二个参数的作用就是限制返回值的长度，表示返回值的最大长度

当然，它依然可以传入任何参数类型，会遵循`正则匹配参数转换规则`进行转换
```
demo:有一段加密的后的密码，我们需要分离出字符串'12a344gg333tt445656ffa6778ii99'中的前三组数字，通过某种计算才能得出正确的密码

``` javascript
'12a344gg333tt445656ffa6778ii99'.split(/[a-zA-Z]+/g,3); //['12','334','333']
```

