---
title: -style一些小笔记
date: 2018-05-07 20:26:10
author: T.2stt
comments: true
tags:
- [wx小程序]
categories:
- [wx小程序]
---

>此章节记载的是项目中的一些wxss的操作记录：界面滚动条的隐藏、

## 界面滚动条的隐藏
```bash
::-webkit-scrollbar {  //app.wxss直接加入
  width: 0;
  height: 0;
  color: transparent;
}
```

---
## 防止滚动穿透
    谭宽的滚动会带动界面的滚动
内容部分100%高度，overflow：auto；

---

## 去除默认样式
### button
```bash
button {
  font-size: 28rpx;
  background-color: transparent;
  border: none;
  padding: 0;
  margin: 0;
  line-height: 1;
}
button::after {
  border: none;
}
```
### input 
```bash
input {
  outline: none;
  border: none;
  list-style: none;
}
```
