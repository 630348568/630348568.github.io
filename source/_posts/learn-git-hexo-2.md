---
title: git-hexo-next 主题的更改
date: 2018-02-12 18:32:00
author: T.stt
tags:
- git-hexo
categories:
- [工具,hexo]
---

>根据自己的需求去将hexo-next的皮肤外观以及需求变得符合自己，创造一个全新不一样的你
-背景的更改
-页面展示尺寸的更改

---
# 背景配置

## 整体的背景配置
``` bash
	博客根目录 themes\next\source\css\_schemes\Pisces\_layout.styl这个文件的第65行background:进行更改
```
## 按钮背景
``` bash
	博客根目录 themes\next\source\css\_common\components\post\post-button.styl 第七行修 background: ;
```
## 站点概况背景
``` bash
	博客根目录 themes\next\source\css\_schemes\Pisces\_sidebar.styl
```
## 菜单栏背景
``` bash
	next\source\css_schemes\Pisces\_layout.styl 文件里.header-inner 这个选择器下的background 就是背景色
```

---
# 界面尺寸的更改
## 右边内容部分
``` bash
	next\source\css_schemes\Pisces\_layout.styl 文件里
	.content-wrap 这个选择器下的把width: $content-desktop;改成width: calc(100% - 260px);
	- 即左边宽度加上margin值
```

---
# [Hexo+Next个人博客主题优化](https://www.jianshu.com/p/efbeddc5eb19)