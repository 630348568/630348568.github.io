---
title: webpack的loader
date: 2019-06-24 18:59:50
author: T.2stt
comments: true
tags:
- [webpack]
categories:
- [webpack打包工具]
---

>`babel`

## 初始化
1. 重新整理演示目录；
2. 修改配置文件; 入口文件等基本的更改
3. 常用的 loader `https://www.webpackjs.com/loaders/`
4. `module` 的配置介绍： `https://www.webpackjs.com/configuration/module/#rule-test`(解释哪个参数拿来干啥的)


## loader的方式
1. require的方式，指定的文件加上指定的处理
```
require('./loader!./dir/file.txt');// 在感叹号前
```

2. 在命令里打包时写
```
webpack hello.js -o  hello.bundle.js --module-bind 'css=style-loader!css-loader'
```
3. 配置文件中配置`Module`

```
module.exports={
    entry: './src/app.js',
    output: {
        path: __dirname + '/dist',
        filename: 'js/[name]-bundle.js'
    },
    module: {
        loaders: { // 
            test: ...,
            loader: ...,
            query: {
                presets: ['latest']
            }
        }
    },
    plugins: [
        new htmlWebpackPlugin({
            filename: 'index.html',
            template: 'index.html',
            title: 'this is a.html',
            inject: 'body'
        })
    ]
}
```

## 使用babel-loader 转换es6代码
1. `babel` 的官网-> `设置`->`Build systems`->`webpack`；
2.  安装`babel` `npm install --save-dev babel-loader @babel/core`；
```
此时报错没安装上 cnpm install --save-dev babel-loader @babel/core
```
3. 使用
```
<!-- 基本用法 -->
module: {
  rules: [
    { test: /\.js$/, exclude: /node_modules/, loader: "babel-loader" }
  ]
}
```
4. `babel`的插件
`babel`的插件---作用： 每年都在更新，支持各个浏览器，用于设定指定的`prest`转换我们需要的一个基准特性；
[地址]!(https://babeljs.io/docs/en/babel-preset-latest/)
安装： `cnpm install --save-dev babel-preset-latest`
使用：
```
<!-- 1. 项目根目录下建.babelrc -->
{
  "presets": ["latest"]
}
<!-- 2. package.json中设置一个键值对 -->
"babel": {
    "presets": ['latest']
}
<!-- 3. 打包的时候命令行中 -->
babel script.js --presets latest
<!--  4. 配置文件中 -->
 module: {
        test: /\.js$/,
        loader: 'babel' ,
        query: { // load 所带的参数,也可以加载loader: 'bable?latest'
            presets: ['latest']
        }
    },
```
### 打包慢问题
`webpack` 官方文档配置文件
设定属性`include`, `exclude`
```
    const path = require('path');
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: path.resolve(__dirname, 'node_modules'), // 直接用'./node_modules/'相对路劲不得行，需要引入node的API `path`，const path = require('path'); 然后转换为绝对路径
                include: path.resolve(__dirname, 'app/src/'),
                use: [
                    {
                        loader: 'babel-loader',
                        options: {
                            presets: [
                                '@babel/preset-env'
                            ]
                        }
                    }
                ],
            }   
        ]   
    },
```

### preset-env
`babel-preset-env` 是非常重要且常用的一个插件预设。
为了让开发者能够尽早用上新的JS特性，babel团队开发了 `babel-preset-latest`,随着时间的推移，`babel-preset-latest` 包含的插件越来越多，这带来了如下问题：
1. 加载的插件越来越多，编译速度会越来越慢；
2. 随着用户浏览器的升级，ECMA规范的支持逐步完善，编译至低版本规范的必要性在减少（比如ES6 -> ES5），多余的转换不单降低执行效率，还浪费带宽。

因为上述问题的存在，babel官方推出了 `babel-preset-env` 插件。它可以根据开发者的配置，按需加载插件。默认配置的情况下，它跟 babel-preset-latest 是等同的，会加载从es2015开始的所有preset。

兼容性处理
1. `node` 如果我们的代码是打算跑在node@8.9.3版本上
```
<!-- 修改下 .babelrc，加上配置参数"target"，它表示我们需要支持哪些平台+哪些版本。这里声明我们要支持的是node版本为8.9.3。 -->
{
  "presets": [
    ["env", {
      "targets": {
        "node": "8.9.3"
      }      
    }]
  ]
}
```
2. 针对浏览器版本的配置
```
<!-- 如果只需要支持 IE11，那么可以这样配置。 -->
{
  "presets": [
    ["env", {
      "targets": {
        "browsers": "ie 11"
      }      
    }]
  ]
}
<!-- 如果只需要支持支持 Edge 16，那么可以这样配置 -->
{
  "presets": [
    ["env", {
      "targets": {
        "browsers": "edge 16"
      }      
    }]
  ]
}
```
3. 支持特定版本范围的浏览器
```
{
  "presets": [
    ["env", {
      "targets": {
        "browsers": [ "ie >= 8", "chrome >= 62" ]
      }      
    }]
  ]
}


$ `npm bin`/browserslist "ie >= 8, chrome >= 62"
chrome 66
chrome 65
chrome 64
chrome 63
chrome 62
ie 11
ie 10
ie 9
ie 8

```

## 样式
1. `src` 目录下新建 `common.css`
2. 加入 `app.js`: `import './css/common.css';`
3. 配置 loader
```
    {
        test: /\.css$/,
        loaders: [
        //   devMode ? 'style-loader' : MiniCssExtractPlugin.loader,
            'style-loader',
            'css-loader'
        ]
    },
    css会被打包在index.html界面中
```
### css-loader/styles-loader
- `style-loader` 能够在需要载入的html中创建一个`<style></style>`标签，标签里的内容就是CSS内容。将css打包成css文件，并将css文件通过<link>标签进行引入，当app.js中import两个css文件时，执行打包时会在dist目录下生成两个css文件，然后html页面也会引入两个css文件。 

- `css-loader` 是允许在js中import一个css文件，`会将css文件当成一个模块引入到js文件中`。在见茶元素的时候回发现className多了很多数字等，还有就是模块化css的设定；
#### 自动添加前缀 postcss-loader
1. 安装 `npm install postcss-loader --save-dev`；
2. 在 `npm` 中看 `postcss` 的官方文档；
3. 安装 `autoprefixer` `npm install autoprefixer --save-dev`
4. 使用 `postcss-loader`
```
    {
        test: /\.css$/,
        loaders: [
        //   devMode ? 'style-loader' : MiniCssExtractPlugin.loader,
            'style-loader',
            'css-loader', 
            {
                loader: 'postcss-loader',
                options: {
                    plugins: [
                    require('autoprefixer')({ 
                        // 这里写兼容没用在webpack中国需要在package.json 配置
                        browsers: ['last 100 versions'] // 或者是browserslist
                    }),
                    // postcssPxtorem
                    ]
                }
            },// 编译方向是从右到左 即先    postcss->css=>style
        ]
    },

<!-- 新的webpack4 则需要在packpage.json中配置 -->
"browserslist": [
    "> 1%",
    "last 100 versions",
    "not ie <= 8",
    "defaults",
    "> 1%",
    "iOS 7",
    "last 3 iOS versions"
  ],
```

#### css的@import引入文件问题
1. `@import` 引入的文件；
2. 在 `css` 中加入 `flex.css`;
3. 引入 `@import './flex.css'`;
4. 发现 `flex.css` 被引入到了界面中，但是没加前缀；
```
<!-- 使用css-loader的一个参数 importLoaders 解决这个问题
用于配置「css-loader 作用于 @import 的资源之前」有多少个 loader -->
{
    test: /\.css$/,
    loaders: [
      'style-loader',
      {
        loader: 'css-loader',
        options: {
          importLoaders: 1 //（其前面只有postcss-loader所以为1） 0 => 无 loader(默认); 1 => postcss-loader; 2 => postcss-loader, sass-loader
        },
      },
      {
          loader: 'postcss-loader',
          options: {
              plugins: [
              require('autoprefixer')
              ]
          }
      },// 编译方向是从右到左 即先    postcss->css=>style
  ]
},
```
### less-loader预编译
1. 安装 `npm install less-loader --save-dev`;
2. `webpack` 的 `loaders` 中的 `postcss-loader` 编译sass/less处理，再加前缀；
3. 打包的时候可能报错没有 `less` 需要安装less；
4. 打包后index.html中多了一个style
```
{
  test: /\.less$/,
  loaders: [
    'style-loader',
    'css-loader',
  {
      loader: 'postcss-loader',
      options: { // 必须要有操作才可以用postcss
        plugins: [
          require('autoprefixer'),
          // postcssPxtorem
        ]
      }
    },
    'less-loader'
  ],
},
```
## 处理模板文件
1. 在`layer.js` 引入模板 `import tpl from './layer.html';`；
2. 在 `app.js` 中
```
const App = function (){
    var dom = document.getElementById('app');
    var layer = new Layer();
    dom.innerHTML = layer.tpl;
}
```
3. 在 `index.html` 的 `body` 中加入
```
  <div id="app"></div>
```
4. 配置规则
```
{
  test: /\.html$/,
  loaders: [
      'html-loader'
  ],
}
```
5. 打包
6. 打开打包后的html；
7. 新建一个layer的`ej` 的语法的东西；
`layer.ejs 或者是 layer.tpl`
```
<div class="layer">
    <div> this is <%= name %> layer</div>
    <% for (var i =0; i < arr.length; i++) { %>
        <%= arr[i] %>
    }
    <% } %>
</div>
```
则需要 `ejs` 的 `loader`

8. 安装 `ejs`, 写配置
```
{
  test: /\.ejs$/,// 或者是tpl
  loaders: [
      'ejs-loader'
  ],
}
```
9. 因为ejs传入的是一个函数，所以在`app.js`传入参数
```
dom.innerHTML = layer.tpl({
    name: 'john',
    arr: ['apple', 'xiaomi', 'oppo']
})
```
## 处理图片
### css中
1. 新建文件夹 `assets`;
2. 放入图片；
3. 在项目中引用 `layer,less`中一个背景；
4. 安装loader ；
```
npm install file-loader --save-dev
```
5. 配置中添加一个图片的配置
```
{
  test: /\.(png|jpg|jpeg|gif|eot|TTF|ttf|woff|woff2|svg|svgz|otf)(\?.+)?$/i, // i 表示不区分大小写
  loaders: [
      'file-loader'
  ],
}
```

### 引入img元素
1. 在index.html中引入；
```
 <img src="./assets/timg.jpg" />
```
也会按照css的没问题

### 组件中的img引入
1. 在 `layer.ejs`中加入img；
2. 打包后发现图片加载失败，没找到图片；
3. 引入 `cdn` 地址或者是 `require` 地址；
```
<img src="${require(../../../assets/timg.jpg)}" />
```

### 改变图片输出方式
更改配置
```
{
  test: /\.(png|jpg|jpeg|gif|eot|TTF|ttf|woff|woff2|svg|svgz|otf)(\?.+)?$/i,
  loaders: [
      {
        loader: 'file-loader',
        options: {
          name: 'images/[name]_[hash:base64:5].[ext]',
        }
      }
  ],
}
```

### url-loader
可以处理文件或者图片，但其可以限制大小，大于大小限制以http的形式，小于限制的准尉base64编码
http请求图片会有缓存；而base64会使其打包文件变大

```
{
  test: /\.(png|jpg|jpeg|gif|eot|TTF|ttf|woff|woff2|svg|svgz|otf)(\?.+)?$/i,
  loaders: [
      {
        loader: 'url-loader',
        options: {
          limit: 1024*2,
          name: 'images/[name]_[hash:base64:5].[ext]',
        }
      }
  ],
}
```

### image-webpack-loader
压缩图片的作用
`https://www.npmjs.com/package/image-webpack-loader`
```
npm install image-webpack-loader --save-dev
```