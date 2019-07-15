---
title: 关于 JS 获取元素尺寸位置等问题
date: 2019-03-01 15:16:54
author: T.2stt
comments: true
tags:
- offsetWidth/clientWidth
categories:
- [js]
---
>Web 前端之 `offsetWidth` ，`clientWidth` ，`scrollWidth` ，`offsetLeft` ，`clientLeft` ，`scrollLeft`
## **Width 和 **Height
### offsetWidth 和 offsetHeight
**IE 的盒模型少margin** 
```javascript
offset* = width / height + padding + border
js：var offset* = document.documentElement.offset* || document.body.offset*t;//兼容浏览器
jq：$('body')[0].offset*;
```
### innerWidth 和 innerHeight
**整个屏幕宽 offset + 滚动条**
```javascript
inner*() = width / height + padding + border + 横向滚动条  // 整个屏幕宽
$(window).inner*()
```
### clientWidth 和 clientHeight
**offset - border** 
```javascript
client* = $(window).width/height() = width / height + padding
js：var client* = document.documentElement.client* || document.body.client*;//兼容浏览器
jq：$('body')[0].client*;
```
### style.width / height
**盒模型的宽高**
{% asset_img 1.png css的盒模型 %}

### 注意
```bash
`style.*` 返回的是`字符串`，如28px，`offset*`返回的是 `数值` 28；
`style.*` 与 `scroll*` 是可读写的属性，`client*`与`offset*`是只读属性
`style.*` 的值需要事先定义，否则取到的值为空。而且必须要定义在html里(内联样式)，如果定义在css里，`style.*`的值仍然为空，但 `offset` 则仍能取到。
```
### scrollWidth / scrollHeigh
scroll* = client* + 溢出内容的尺寸

## **Top 和 **Left
### offsetParent
HTMLElement.offsetParent 是一个只读属性，返回一个指向最近的（closest，指包含层级上的最近）包含该元素的定位元素。如果没有定位的元素，则 offsetParent 为最近的 table, table cell 或根元素（标准模式下为 html；quirks 模式下为 body）。当元素的 style.display 设置为 "none" 时，offsetParent 返回 null。
```javascript
var offsetParent = element.offsetParent;
```
{% asset_img offsetParent.jpg 图形解说 %}
### offsetTop / offsetLeft
```javascript
// 父元素定位
offsetTop = 元素外缘即offset模型（`offsetWidth / Height`）到父元素内壁即client模型(`clientWidth / Height`)的距离 
// 父元素未定位
则分别是到 body 的距离
```
### clientTop / clientLeft
```javascript
clientTop = boder.top
clientLeft = boder.left
```
### scrollTop / scrollLeft
**顶部 / 左边 被隐藏的那部分=滚动条离头的距离）**
```javascript
// 返回值是一个整数，单位是像素。可读写的
scrollTop = 元素滚动条被向下拉动的距离。
scrollLeft = 元素被滚动条向左拉动的距离。
```

## 区分客户区坐标、页面坐标、屏幕坐标
**不管是onclick，还是omousemove，onmouseover等**
### clientX / Y(常用--滑动)     
鼠标相对于 `浏览器`（这里说的是浏览器的有效区域）左上角 `x/y` 轴的坐标；  不随滚动条滚动而改变；
### pageX / Y       
鼠标相对于 `浏览器` 左上角 `x/y` 轴的坐标； 随滚动条滚动而改变；
### screenX / Y     
鼠标相对于 `显示器屏幕` 左上角`x/y` 轴的坐标；  
### offsetX / Y 
鼠标相对于 `事件源` 左上角 `x/y` 轴的坐标
{% asset_img 2.jpg 图形解说 %}

## 图形分解解说
{% asset_img offsetHeight.png offsetWidth / offsetHeight %}
{% asset_img offsetParent.jpg  offsetParent %}
{% asset_img clientWH.png clientWidth / clientHeight %}
{% asset_img clientLT.png clientLeft / clientTop %}
{% asset_img outerWH.png outerWidth / clientHeight %}
{% asset_img scollHeight.png scollHeight / scollWidth %}
{% asset_img scrollerTL.png scrollTop / scollLeft %}
{% asset_img scrollTop.png scrollTop %}

## 图形整体解说
{% asset_img 1.png 图形解说 %}