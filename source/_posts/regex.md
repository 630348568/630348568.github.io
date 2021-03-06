---
title: 常用正则整理
date: 2016-09-05 12:12:24
author: T.2stt
tags:
- Regex
- javascript
categories:
- [正则Regex]
---

> 正则的魅力在于使用很简洁的方式解决一些比较复杂的方式，使代码变得更优雅，也使实现的过程变得更简单透明。本文搜集整理一些常用正则，记录以便查阅

```javascript
var regex = window.regex || (function (document, $) {
        var _reg = {};
        /* 'pwd':/^[\@A-Za-z0-9\!\#\$\%\^\&\*\.\~]{6,16}$/,//密码 */
        //验证数字
        $.extend(_reg, {
            'num': /^\d+$/, //数字
            'znum': /^[1-9](\d+)?$/, //  大于0的数字
            'float': /^[-]{0,1}(\d+)[\.]+(\d+)$/, //浮点数
            'money': /^\d{1,12}(?:\.\d{1,3})?$/, // money
            'idCard': /^\d{15}$|^\d{18}$|^\d{17}[xX]$/, //身份证
            'idCardStrict':/^(\d{6})([1-2])(\d{3})((?:0[1-9])|(?:1[0-2]))((?:0[0-9])|(?:[1-2][0-9])|(?:3[0-1]))(\d{3})(\d{1})$/,
            'qq': /^[1-9]\d{4,15}$/, //QQ
            'pwd': /^[\@A-Za-z0-9]{6,16}$/, //密码
            'areacode': /^(0[1,2]{1}\d{1})$|^(0[3-9]{1}\d{2})$/, //区号
            'tel': /^\d{7,8}$/, // 固话格式
            'mobile': /^((\+86)|(\(\+86\)))?-?(13|14|15|18|17)[0-9]{9}$/, //验证手机号码
            'telephone': /^(((\+)?86)|(\(\+86\)))?-?((((0)?[1,2]{1}\d{1})?-?\d{8})|((0[3-9]{1}\d{2})?-?\d{7,12}))-?(\d{1,8})?$/, //验证固定电话
            'phone': /^((\+86)|(\(\+86\)))?-?(13|14|15|18|17)[0-9]{9}$|^((\+86)|(\(\+86\)))?-?(((0[1,2]{1}\d{1})?-?\d{8})|((0[3-9]{1}\d{2})?-?\d{7,8}))$/, //手机号码和固定电话
            'zipcode': /^\d{6}$/ //验证邮编
        });
        //验证字符串
        $.extend(_reg, {
            'email': /^\w{1,16}([-+.']\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/, //邮箱
            'chinese': /^[\u4E00-\u9FA5]+$/, //仅汉字
            'char': /^[A-Za-z]+$/, //仅仅是字母
            'charn': /^[A-Za-z0-9]+$/, //数字加字母
            'nospecial': /^[\u4E00-\u9FA5A-Za-z0-9]+$/, // 不包含特殊字符
            'url': /^((http|https|ftp):\/\/)?(\w(\:\w)?@)?([0-9a-z_-]+\.)*?([a-z0-9-]+\.[a-z]{2,6}(\.[a-z]{2})?(\:[0-9]{2,6})?)((\/[^?#<>\/\\*":]*)+(\?[^#]*)?(#.*)?)?$/,
            'loginName': /^(13|14|15|18|17)[0-9]{9}$|^\w+([-+.']\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/, // 用户名
            'userName': /^[\u4e00-\u9fa5]{2`,4}$|[a-zA-Z]{4,20}$/, //真实姓名
            'nickName': /^([a-zA-Z\u4e00-\u9fa5]{1}[a-zA-Z0-9_\u4e00-\u9fa5]{3,19})$/ //昵称
        });
        return _reg;
    })(document, window.jQuery);
window.regex = regex;
```
