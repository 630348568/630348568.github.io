---
title: wxpromise的元素（试图容器）
date: 2018-05-07 20:26:31
author: T.2stt
comments: true
tags:
- wxprogram-element
categories:
- wxprogram
---

>此章节记载的是 wxpromise的元素与html的差别

## view
| 属性名        | 类型    |默认值        | 说明    |
| --------   | :-----:   | :-----:   | :-----:   |
| hover-class   | String      |无   | 指定按下去的样式类。当  hover-class="none"  时，没有点击态效果      |
| hover-stop-propagation   | Boolean |false   | 指定是否阻止本节点的祖先节点出现点击态    |
| hover-start-time   | Number      |50  | 按住后多久出现点击态 （单位毫秒）    |
| hover-stay-tim   | Number      |400  | 手指松开后点击态保留时间      |

###  项目总结
```bash
	1.属于块级元素（div）;
	2.如果需要使用滚动视图，请使用 scroll-view;
```