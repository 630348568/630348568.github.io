---
title: vscode的插件使用介绍
date: 2018-05-06 19:51:28
author: T.2stt
comments: true
tags:
- [工具-vscode]
categories:
- [工具,开发工具,vscode]
---

>此章节记载的是 vscode 插件的使用介绍。针对于前端，小程序，css，html，nodejs，ts，es6，es7等使用

## (常)关于HTML
### HTML Snippets
	 回车或tap生成标签
### HTML CSS Support 
	CSS的智能补全
## 图标、代码样式插件

## (常)关于Css
### stylelint
	CSS/SCSS/LESS检查工具 
	（首选项-设置-用户设置配置： 
		"stylelint.enable": true, 
		"css.validate": false, 
		"scss.validate": false 
		）
### HTML CSS Support 
	CSS的智能补全
	
### Less/Sass 编译
#### Easy Sass 
	scss/sass文件保存可自动生成并同步编译成同名css文件
#### Easy WXLESS
	微信小程序WXSS文件专用，保存可自动生成并同步编译成同名css文件


## 图标、代码样式插件，语法高亮
### vscode-icon
	让vscode资源目录加上图标、必备
### 语法高亮
#### "files.associations": { 
	"*.vue": "vue", 
	"*.wpy": "vue", 
	"*.wxml": "html", 
	"*.wxss": "css" 
	}
#### beautify
	javascript, JSON, CSS, Sass, HTML 代码高亮
#### Beautify css/sass/scss/less
	代码高亮
	
## Vue
### vetur
	Vue语法高亮
### vue
	 Vue语法高亮（首选项-设置-用户设置配置： 
		"emmet.syntaxProfiles": { 
		"vue-html": "html", 
		"vue": "html"
		}）
### Vue 2 Snippets 
	基于最新的 Vue 官方语法高亮文件添加了语法高亮，并且依据 Vue 2 的 API 添加了代码补全
### Vue Peek
	查找vue组件文件
### VueHelper 
	vue代码提示插件，包括了vue2所有api，vue-router2和vuex2的代码提示
	
## 小程序
### vscode wxml
	识别wxml
### 小程序组件
	识别wxss
	+vscode weapp api以及WeApp Snippets插件（但是避免wxml或者wxss之一不能用必须加）


	


