---
title: webpack初步学习
date: 2019-06-21 16:31:17
author: T.2stt
comments: true
tags:
- [webpack]
categories:
- [webpack打包工具]
---
>`WebPack` 可以看做是静态模块打包器：它做的事情是，分析你的项目结构，找到`JavaScript`模块以及其它的一些浏览器不能直接运行的拓展语言（`Scss，TypeScript`等），并将其转换和打包为合适的格式供浏览器使用。学习地址`https://www.imooc.com/learn/802`

## 自主学习--基础入门
1. 新建目录 `webpack`;
2. `npm init`;
3. `npm install webpack --save-dev`;
4. 建立一个hello.js
```
function hello (str){
    alert(str);
}
```
5. 打包`webpack hello.js  hello.bundle.js` 保存webpacak-cli没安装  但是安装后还是这样,则需要全局安装`npm i webpack-cli -g`;
6. 继续打包，会报错找不到`hello.bundle.js`则需要更改打包代码，因为webpack是新版本的4.XX `webpack hello.js  -o hello.bundle.js`，成功输出`hello.bundle.js`;

## 进阶理解
1. 新建`world.js`文件;
```
function world (){
    return {
        
    }
}
```
2. `hello.js` 引用 `world.js`;
```
require ('./world.js');

function hello (str){
    alert(str);
}
```
3. 继续打包 `webpack hello.js -o hello.bundle.js`，成功输出`hello.bundle.js` 查看`hello.bundle.js`发现它自己会去找寻world.js;

## 加入css
1. 新建`style.css`文件;
```
html, body{
    padding: 0;
    margin: 0;
}
```
2. `hello.js` 引用 `world.js`;
```
require ('./world.js');
require ('./style.css');
function hello (str){
    alert(str);
}
```
3. 继续打包 `webpack hello.js -o  hello.bundle.js`，报错没有`cssload`;
4. 添加`cssload/styleload` `npm install css-loader style-loader --save-dev`;
5. 还是报错没有处理文件;
6. `require ('style-loader!css-loader!./style.css');`// 引用css之前需要css-loader处理
或者在命令行中弄`webpack hello.js -o  hello.bundle.js --module-bind 'css=style-loader!css-loader'`
7. `webpack hello.js -o  hello.bundle.js --watch` 文件更改就会编译；
8. `webpack hello.js -o  hello.bundle.js --progress` 打包过程的进度条；
9. `webpack hello.js -o  hello.bundle.js --progress --display-modules` 展示所有引用模块  以及使用什么loader处理的；
10. `--display-reasons` 展示为什么打包这个模块的原因；

