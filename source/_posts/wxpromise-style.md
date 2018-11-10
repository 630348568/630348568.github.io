---
title: -style主体的与css区别介绍
date: 2018-05-07 20:26:20
author: T.2stt
comments: true
tags:
- [wx小程序]
categories:
- [wx小程序]
---

>此章节记载的是 wxpromise的style与css的差别(rpx、样式导入、内联样式、选择器)

## 尺寸单位rpx
```bash
rpx：可以根据屏幕宽度进行自适应。规定屏幕宽为750rpx。
如在 iPhone6 上，屏幕宽度为375px，共有750个物理像素，则750rpx = 375px = 750物理像素，1rpx = 0.5px = 1物理像素。
【建议】： 开发微信小程序时设计师可以用 iPhone6 作为视觉稿的标准。
【注意】： 在较小的屏幕上不可避免的会有一些毛刺，请在开发时尽量避免这种情况。
```
---
## 样式导入
```bash
    /** common.wxss **/
    .small-p {
    padding:5px;
    }

    /** app.wxss **/
    @import "common.wxss";
    .middle-p {
    padding:15px;
    }
```
---
## 内联样式
框架组件上支持使用 style、class 属性来控制组件的样式。
- style：静态的样式统一写到 class 中。style 接收动态的样式，在运行时会进行解析，请尽量避免将静态的样式写进 style 中，以免影响渲染速度。
```bash
    <view style="color:{{color}};" />
```
- class：用于指定样式规则，其属性值是样式规则中类选择器名(样式类名)的集合，样式类名不需要带上.，样式类名之间用空格分隔。
```bash
    <view class="normal_view" />
```
---
## 选择器
| 选择器        | 样例    |样例描述|
| ---- | :-----:   | :-----: |
| .class   | .intro      |选择所有拥有 class="intro" 的组件   |
| #id   | #firstname |选择拥有 id="firstname" 的组件   |
| element   | view      |选择所有 view 组件|
| element, element   | view, checkbox      |选择所有文档的 view 组件和所有的 checkbox 组件|
| ::after   | view::after      | 在 view 组件后边插入内容  |
| ::before  | view::before     | 在 view 组件前边插入内容      |