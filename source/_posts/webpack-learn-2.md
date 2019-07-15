---
title: webpack的配置详解，如何搭建webpack基本配置
date: 2019-06-23 18:59:50
author: T.2stt
comments: true
tags:
- [webpack]
categories:
- [webpack打包工具]
---

>`webpack`的基本配置，以及配置的基本功能

## 初始化新项目
1. 新建文件夹->`npm init`->`npm install webpack --save-dev`->创建文件夹`src、dist`->`index.html`中放入`bundle.js`;
2. src中有`script/style`文件夹;
3. 新建`webpack.config.js`(https://webpack.js.org/configuration/#options);
4. 直接使用执行`webpack`自己会去寻找`webpack.config.js`;

## 编写webpack.config.js
```
module.exports={
    entry: './src/script/main.js', // 入口文件
    output: {
        path: './dist/js', // 放在哪个目录下（会报错输出路径不对 将path: './dist/js',改成path: '__dirname/dist/js', // __dirname则是获得当前文件所在目录的完整路径名）
        filename: 'bundle.js' // 打包以后的文件名
    }
}
```
## --config的使用
把`webpack.config.js`改成`webpack.dev.config.js`->运行`webpack`不执行->`webpack --config webpack.dev.config.js`

## webpack在命令行中使用的一些参数配合npm脚本做到
`pacage.json`中的`scripts`中定义脚本
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "webpack": "webpack --config webpack.config.js --progress --display-moudle --colors --display-reason"
  },
  运行npm run webpack
```
## 打包入口entry
3种输入方式
1. 简单的`string`
```
module.exports={
    entry: './src/script/main.js', // 入口文件
    output: {
        path: '__dirname/dist/js',
        filename: 'bundle.js'
    }
}
```
2. 数组`arry`
```
module.exports={
    entry: ['./src/script/main.js', './src/script/a.js'], // 入口文件(新建a文件)
    output: {
        path: '__dirname/dist/js',
        filename: 'bundle.js'
    }
}
执行 npm run webpack
```
3. 对象
```
module.exports={
    entry: {
        main: './src/script/main.js',
        a: './src/script/a.js'
    }, // 入口文件(新建a文件)
    output: {
        path: '__dirname/dist/js',
        filename: 'bundle.js'
    }
}
执行 npm run webpack 会报错 打在同一个js filename: 'bundle.js'改成filename: '[name].js'分别打包出一个文件 main.js 和 a.js
filename: '[name]-[chunkhash].js'
```
## output
### filename
如上3对象的输入写一个bundle.js则会相互覆盖报错，则需要使用占位符去解决
1. [name] --- `entry`以对象输入的`key`值`main、a` `filename: '[name].js`
2. [hash] --- 打包的`hash`每次打包的时候命令界面会生成一段信息的第一行的`hash`值`filename: '[name]-[hash].js`
3. [chunkhash] --- 每一个`chunck`自己的`hash`自己的版本号，修改后的文件的`chunkhash`会变，没改的不变,`filename: '[chunkhash].js`对于线上更改后的文件才打包很有用的哦
### publicPath
开发环境以及上线环境的文件路径是不一样的， 占位符（设置之后）在html引用的js会以其为头

## html-webpack-plugin
解决手动载入`bundle.js`进入`index.html`的问题,特别是针对`hash`值的路劲
1. 安装
```
cnpm install html-webpack-plugin --save-dev
如果用npm的话下载不了
```

## plugin 的使用
### template属性
```
plugins: [
    new htmlWebpackPlugin() // 初始化插件
]
// 但是这个只是在dist中生成了个index.html与之前的index.html没有任何的联系
```
所以需要重新传值
```
module.exports={
    entry: {
        main: './src/script/main.js',
        a: './src/script/a.js'
    }, // 入口文件(新建a文件)
    output: {
        path: '__dirname/dist/js',
        filename: '[name]-[chunkhash].js'
    },
    plugins: [
        new htmlWebpackPlugin({
            template: 'index.html' // 代表着根目录的index.html
        }) // 初始化插件
    ]
}
// 但是这个只是在dist的js中生成了个index.html是原来的index.html+新的js dist的js里的，更改根目录下的index.html后dist里的js的index.html的也会同步更改（例如更改title）
```
希望index.html和js分开
```
module.exports={
    entry: {
        main: './src/script/main.js',
        a: './src/script/a.js'
    },
    output: {
        path: __dirname + '/dist', // 打包的东西放在dist下
        filename: 'js/[name]-[chunkhash].js' // 把js放在js目录下
    },
    plugins: [
        new htmlWebpackPlugin({
            template: 'index.html'
        }) 
    ]
}
```
### filename/inject属性
```
module.exports={
    entry: {
        main: './src/script/main.js',
        a: './src/script/a.js'
    },
    output: {
        path: __dirname + '/dist',
        filename: 'js/[name]-[chunkhash].js'
    },
    plugins: [
        new htmlWebpackPlugin({
            filename: 'index-[hash].html', // 打包后的名字加上hash
            template: 'index.html',
            inject: 'head' // 脚本是放在head还是body标签里
        }) 
    ]
}
```
### 参数中传参，模板中引用，title为例子
```
module.exports={
    entry: {
        main: './src/script/main.js',
        a: './src/script/a.js'
    },
    output: {
        path: __dirname + '/dist',
        filename: 'js/[name]-[chunkhash].js'
    },
    plugins: [
        new htmlWebpackPlugin({
            filename: 'index-[hash].html',
            template: 'index.html',
            title: 'webpack is good', // 在根目录的html中更改成变量
            data: new Date(), // index.html 的body写<%= htmlWebpackPlugin.options.data %>
        }) 
    ]
}

在html中<title><%= htmlWebpackPlugin.options.title %></title>
如果想看一下htmlWebpackPlugin里有啥值，就需要用循环不要=号了,分别展现出来（npm中都有用法）
<% for (var key in htmlWebpackPlugin) {%>
    <% key %>
<% } %>
<% for (var key in htmlWebpackPlugin.files) {%>
    <%= key %> : <%= JSON.stringfy(htmlWebpackPlugin.files[key]) %>
<% } %>
```
如果要把一部分js放在heade一部分放在body
```
inject: false
在index.html的head上加入
<script  type='text/javascript' src="<%= htmlWebpackPlugin.files.chunks.main.entry %>" >
在body中加入
<script  type='text/javascript' src="<%= htmlWebpackPlugin.files.chunks.a.entry %>" >
```
### minify 压缩
```
module.exports={
    entry: {
        main: './src/script/main.js',
        a: './src/script/a.js'
    },
    output: {
        path: __dirname + '/dist',
        filename: 'js/[name]-[chunkhash].js'
    },
    plugins: [
        new htmlWebpackPlugin({
            template: 'index.html',
            minify: {
                removeComments: true, // 删除注释。。。
            }
        }) 
    ]
}
```

### 处理多页面的应用
```
<!-- 假设有a,b,c三个界面 -->
module.exports={
    entry: {
        main: './src/script/main.js',
        a: './src/script/a.js',
        b: './src/script/b.js',
        c: './src/script/c.js'
    },
    output: {
        path: __dirname + '/dist',
        filename: 'js/[name]-[chunkhash].js'
    },
    plugins: [
        new htmlWebpackPlugin({
            filename: 'a.html',
            template: 'index.html',
            title: 'this is a.html',
            <!-- chunks: ['main', 'a'], // html里包含的chunk -->
            excludeChunks:  ['a', 'c'], // 除了哪些外
            inject: 'body',
            minify: {
                removeComments: true
            }
        }),
        new htmlWebpackPlugin({
            filename: 'b.html',
            template: 'index.html',
            title: 'this is b.html',
            chunks: ['main', 'b'], // html里包含的chunk
            inject: 'body',
            minify: {
                removeComments: true
            }
        }),
        new htmlWebpackPlugin({
            filename: 'c.html',
            template: 'index.html',
            title: 'this is c.html',
            chunks: ['main', 'c'], // html里包含的chunk
            inject: 'body',
            minify: {
                removeComments: true
            }
        }),
        new htmlWebpackPlugin({
            ...// 根据不同的页面指定不同的模板
        })
    ]
}
```
### 把js的代码引入html界面
3.3最后一点