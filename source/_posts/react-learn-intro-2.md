---
title: react项目中的统计
date:  2018-07-30 15:41:54
author: T.2stt
comments: true
tags:
- React
categories:
- [MV*框架,React]
---
> 人类只有不断地总结经验,才会有所 发现,有所发明,有所创造,有所前进。在学习中也一样，需要不断的总结，踩坑。
此文记载react的项目中踩的坑，以及经验之谈，只有不断的去尝试才能更加的得心应手。包括onClick

## 事件生命
1.只要是通过onXXX事件调用的都必须是handle开头进行方法生命
```bash
onClick={ () => { this.handleNextSameClick(); }}
```
2.对于父组件传给子组件的方法需要传递参数this，方法有两种
```bash
在父组件
1. 在构造函数中：this.handleClick = this.handleClick.bind(this);  在调用中使用 onClick={this.handleClick}
2. 在使用处直接传参用：handleFouceSearchFn={this.handleFouceSearchFn.bind(this)}
```