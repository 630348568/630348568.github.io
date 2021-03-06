---
title: 常用javascript代码片段
date: 2017-11-26 22:37:45
author: J.2ue
tags:
- sinpats
categories:
- javascript
---

> 下面记录的主要是本人在项目中遇到的一些问题的解决方案或者个人觉得精妙的代码，方案也许来自于自己的想法，也有可能来自于网络（我尽量给出出处，以便大家追源溯流），也许干脆就是一个插件库（实际在项目中对于一些小的功能个人是拒绝引入不纯粹的三方库的）。肯定，这其中有些方案也不是尽善尽美，我会一一列出。如果对于某个问题在你看来有更好的解决方案，请不吝指出！

## placeholder属性支持

有时候项目中placeholder也许要兼容某些特殊的浏览器，所以才有了下面这段代码。这种方案有一个缺点就是：如果你恰好要对这个输入框绑定focus事件并操作它的值，必须要小心处理

```javascript
//修复不支持placeholder属性 start
const isSurportPlder = "placeholder" in document.createElement("input"); // 判断浏览器是否支持 placeholder
if (!isSurportPlder) {
  $("[placeholder]").focus(function () {
    const _this = $(this);
    if (_this.val() == _this.attr("placeholder")) {
      _this.val('');
    }
  }).blur(function () {
    const _this = $(this);
    if (_this.val() == '' || _this.val() == _this.attr("placeholder")) {
      _this.val(_this.attr("placeholder"));
    }
  }).blur();
};
```

## 格式化时间

关于格式化时间有很多插件，其中比较有名的就可以列很大一堆出来，比如老牌的[moment.js](https://github.com/moment/moment)，最近比较多star的[luxon.js](https://github.com/moment/luxon); 对于为什么不选择他么，上面已经说了原因了。

```javascript
formate(fmt, date) {
    date = new Date(date).toString() === 'Invalid Date' ? new Date() : new Date(date);
    const _rules = [{
        rule: '[yY]{4}',
        value: date.getFullYear()
    }, {
        rule: 'M+',
        value: date.getMonth() + 1
    }, {
        rule: '[dD]+',
        value: date.getDate()
    }, {
        rule: 'h+',
        value: date.getHours()
    }, {
        rule: 'm+',
        value: date.getMinutes()
    }, {
        rule: 's+',
        value: date.getSeconds()
    }, {
        rule: 'ms{1,2}',
        value: date.getMilliseconds()
    }];

    _rules.forEach((_r) => {
        const rule = _r.rule, val = _r.value;
        fmt = fmt.replace(new RegExp(rule), function ($1) {
            const rLen = val.toString().length, fLen = $1.length;
            return (fLen !== 2 || rLen >= fLen) ? val : ['00', val].join('').substr(rLen);
        });
    });
    return fmt;
}
```

## 解析浏览器的版本等详细信息

浏览器的判断涉及到很多复杂的变量和参数，所以偷懒选取了一个比较好的库，这个库它唯一的功能就是识别浏览器的版本等详细信息。
如果大家有更好的库，请推荐过来！

[browser.js](https://github.com/mumuy/browser)

## 数字转换成千分位格式(如：123,456,9.89)

```javascript
function translateThree(num) {
  return num.split('').reverse().join('').replace(/(\d{3}(?=\d)(?!\d+\.|$))/g, '$1,').split('').reverse().join('');
}
```

## 判断任意数据的类型

精准的判断类型，`'2'`会识别成`string`类型，`2`会识别成`number`类型

``` javascript
function tryType(para) {
    const type = typeof para;
    if (type === "number" && isNaN(para)) return "NaN";
    if (type !== "object") return type;
    return Object.prototype.toString
        .call(para)
        .replace(/[\[\]]/g, "")
        .split(" ")[1]
        .toLowerCase();
}
```

## 判断是不是一个可计算的数字

上面的`tryType`方法会将`'2'`和`2`区别成两种类型，而这个方法将忽略这种区别

``` javascript
function isNumber(para) {
    if (window.isNumber) return window.isNumber(para);
    if (Number.isNumber) return Number.isNumber(para);
    return typeof para !== "undefined" && !isNaN(para);
}
```

## 深度取值防止代码挂掉

在项目中经常会遇到层级很深的`json`数据，这时候可能就会写类似这样的代码`a[2].list[3].name`，这种代码很不可靠，很容易由于数据的一点小错误，导致整段js代码挂掉。出的问题多了自然就会思考问题的解决方案，正当我苦思不得其解时，看到一篇文章[如何优雅安全地在深层数据结构中取值](http://www.jianshu.com/p/11fc75f28302)，他详细的分析了深层取值如何避免报错的情况。
我阅读了博主的文章，整理了一下思路，没有像原博主那样使用`xs && xs[x]`判断来打断取值，是因为这种情况可能会把`0`这种类型的值误伤

``` javascript
function getValueFromDeepData (props, target){
    if (!props || !target) return undefined;
    return props.reduce((pre, nxt) => (typeof pre === 'undefined' || typeof pre[nxt] === 'undefined' ? undefined : pre[nxt]), target);
}
```

## 反转义字符串

何谓反转义字符串？就是后端在传输HTML代码的时候往往会对字符串处理：把一些特殊符号转义了；当我们拿到HTML渲染到页面希望他按照HTML代码的格式来显示，而不是按照转义的字符串显示成文本，所以我们需要对这段字符串反转义！
网上看到很多解决方案是通过正则的方式一一替换过来，但是个人觉得这样不好维护。后面发现一种浏览器自动转换的办法，利用这一特性，可以通过js创建一个虚拟的DOM节点，然后把需要转义的字符串使用innerHTML方法放进去，再通过nodeValue方法取出来。
但这种方法需要注意的是：`e.childNodes`是一个数组，它将`_html`分段（每65536字符分一段）存储到`e.childNodes[i]`中，使用`e.childNodes`方法取得反转义后的字符串需要循环`e.childNodes`数组。我看到网上很多方法都是直接取的`e.childNodes[0].nodeValue`，这样在内容过多的时候，是无法把数据取完整的

``` javascript
function htmlDecode(str) {
    const e = document.createElement('div'), _html = '';
    e.innerHTML = str;
    for (let i = 0; i < e.childNodes.length; i++) {
        _html += e.childNodes[i].nodeValue;
    };
    return _html;
};
```

## 未完
