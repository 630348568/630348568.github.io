---
title: git-hexo-next 主题的更改
date: 2018-02-12 18:32:00
author: T.stt
tags:
- git
categories:
- tools---git-hexo
---

>根据自己的需求去将hexo-next的皮肤外观以及需求变得符合自己，创造一个全新不一样的你

## 背景配置

### 整体的背景配置
``` bash
	博客根目录 themes\next\source\css\_schemes\Pisces\_layout.styl这个文件的第65行background:进行更改
```
### 按钮背景
``` bash
	博客根目录 themes\next\source\css\_common\components\post\post-button.styl 第七行修 background: ;
```
### 站点概况背景
``` bash
	博客根目录 themes\next\source\css\_schemes\Pisces\_sidebar.styl
```
### 菜单栏背景
``` bash
	next\source\css_schemes\Pisces\_layout.styl 文件里.header-inner 这个选择器下的background 就是背景色
```