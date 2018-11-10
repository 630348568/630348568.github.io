---
title: -常见bug
date: 2018-10-17 11:49:06
author: T.2stt
comments: true
tags:
- [wx小程序]
categories:
- [wx小程序]
---

>记下踩过的坑1.宽度100%超过界面；2.传boolean类型的值

# wxss
## width：100%，界面显示不全
盒模型选择错误,进行盒模型初始化
```bash
在app.wxss
view,text{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
```

# js
## 方法中Boolean参数的传递
关键字(需要在双引号之内)
true：boolean 类型的 true，代表真值。
false： boolean 类型的 false，代表假值。
```bash
<checkbox checked="{{false}}"> </checkbox>
特别注意：不要直接写 checked="false"，其计算结果是一个字符串，转成 boolean 类型后代表真值。
```