---
title: react初步学习
date: 2018-05-22 21:21:45
author: T.2stt
comments: true
tags:
- react
categories:
- react
---
>react的初步介绍了以下相关信息：
react的特点、react如何安装、react的JSX语法、react的组件、react的state以及react的props

## React 特点
1.声明式设计 ———— React采用声明范式，可以轻松描述应用。
2.高效 ———— React通过对DOM的模拟，最大限度地减少与DOM的交互。
3.灵活 ———— React可以与已知的库或框架很好地配合。
4.JSX  ————  JSX 是 JavaScript 语法的扩展。React 开发不一定使用 JSX ，但我们建议使用它。
5.组件  ————  通过 React 构建组件，使得代码更加容易得到复用，能够很好的应用在大项目的开发中。
6.单向响应的数据流  ————  React 实现了单向响应的数据流，从而减少了重复代码，这也是它为什么比传统数据绑定更简单。<font color="red">（类似于wxpromise的setData）</font>

## 实例
```bash
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Hello React!</title>
    <script src="https://cdn.bootcss.com/react/15.4.2/react.min.js"></script>
    <script src="https://cdn.bootcss.com/react/15.4.2/react-dom.min.js"></script>
    <script src="https://cdn.bootcss.com/babel-standalone/6.22.1/babel.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      ReactDOM.render(
        <h1>Hello, world!</h1>,
        document.getElementById('example')
      );
    </script>
  </body>
</html>
```
### 实例解析：
```bash
react.min.js - React 的核心库
react-dom.min.js - 提供与 DOM 相关的功能
babel.min.js - Babel 可以将 ES6 代码转为 ES5 代码，这样我们就能在目前不支持 ES6 浏览器上执行 React 代码。Babel 内嵌了对 JSX 的支持。通过将 Babel 和 babel-sublime 包（package）一同使用可以让源码的语法渲染上升到一个全新的水平。
以下代码是将一个 h1 标题，插入 id="example" 节点中:
ReactDOM.render(
    <h1>Hello, world!</h1>,
    document.getElementById('example')
);
```
## React 安装
### 1.直接使用 BootCDN 的 React CDN 库
```bash
<script src="https://cdn.bootcss.com/react/15.4.2/react.min.js"></script>
<script src="https://cdn.bootcss.com/react/15.4.2/react-dom.min.js"></script>
<script src="https://cdn.bootcss.com/babel-standalone/6.22.1/babel.min.js"></script>
使用实例
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Hello React!</title>
    <script src="https://cdn.bootcss.com/react/15.4.2/react.min.js"></script>
    <script src="https://cdn.bootcss.com/react/15.4.2/react-dom.min.js"></script>
    <script src="https://cdn.bootcss.com/babel-standalone/6.22.1/babel.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      ReactDOM.render(
        <h1>Hello, world!</h1>,
        document.getElementById('example')
      );
    </script>
  </body>
</html>
```
#### 注意
	如果我们需要使用 JSX，则 <script> 标签的 type 属性需要设置为 text/babel。
### 2.通过 npm 使用 React
	create-react-app 是来自于 Facebook，通过该命令我们无需配置就能快速构建 React 开发环境。
	create-react-app 自动创建的项目是基于 Webpack + ES6 。
```bash
$ cnpm install -g create-react-app
$ create-react-app my-app
$ cd my-app/
$ npm start

项目的目录结构如下：
my-app/
  README.md
  node_modules/
  package.json
  .gitignore
  public/
    favicon.ico
    index.html
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg

在浏览器中打开 http://localhost:3000/ 

```
#### 尝试修改 src/App.js 文件代码：
```bash
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';
 
class App extends Component {
  render() {
    return (
      <div className="App">
        <div className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h2>欢迎来到菜鸟教程</h2>
        </div>
        <p className="App-intro">
          你可以在 <code>src/App.js</code> 文件中修改。
        </p>
      </div>
    );
  }
}
 
export default App;
```

## React JSX
	React 使用 JSX 来替代常规的 JavaScript。JSX 是一个看起来很像 XML 的 JavaScript 语法扩展。
	我们不需要一定使用 JSX，但它有以下优点：
		1.JSX 执行更快，因为它在编译为 JavaScript 代码后进行了优化。
		2.它是类型安全的，在编译过程中就能发现错误。
		3.使用 JSX 编写模板更加简单快速。

### 使用 JSX
```bash
JSX 看起来类似 HTML ，我们可以看下实例:
ReactDOM.render(
    <h1>Hello, world!</h1>,
    document.getElementById('example')
);
```
### 如何嵌入多层元素
	我们可以在以上代码中嵌套多个 HTML 标签，需要使用一个 div 元素包裹它，
	实例中的 p 元素添加了自定义属性 data-myattribute，添加自定义属性需要使用 data- 前缀。
```bash
ReactDOM.render(
    <div>
    <h1>菜鸟教程</h1>
    <h2>欢迎学习 React</h2>
    <p data-myattribute = "somevalue">这是一个很不错的 JavaScript 库!</p>
    </div>
    ,
    document.getElementById('example')
);
```
### 独立文件
```bash
React JSX 代码可以放在一个独立文件上，例如我们创建一个 helloworld_react.js 文件，代码如下：
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('example')
);
然后在 HTML 文件中引入该 JS 文件：
<body>
  <div id="example"></div>
<script type="text/babel" src="helloworld_react.js"></script>
</body>
```
### JavaScript 表达式
```bash
JSX 中使用 JavaScript 表达式。表达式写在花括号 {} 中。实例如下：
ReactDOM.render(
    <div>
      <h1>{1+1}</h1>
    </div>,
    document.getElementById('example')
);
在 JSX 中不能使用 if else 语句，但可以使用 conditional (三元运算) 表达式来替代。
以下实例中如果变量 i 等于 1 浏览器将输出 true, 如果修改 i 的值，则会输出 false.
<body>
<div id="example"></div>
<script type="text/babel">
  var i = 1;
  ReactDOM.render(
  	<div>
  	  <h1>{i == 1 ? 'True!' : 'False'}</h1>
   </div>,
  	document.getElementById('example')
  );
</script>
</body>
```
### 样式
```bash
React 推荐使用内联样式。我们可以使用 camelCase 语法来设置内联样式. React 会在指定元素数字后自动添加 px 。
以下实例演示了为 h1 元素添加 myStyle 内联样式：
var myStyle = {
     fontSize: 100,
     color: '#FF0000'
};
ReactDOM.render(
  	<h1 style = {myStyle}>菜鸟教程</h1>,
  	document.getElementById('example')
);
```
### 注释
```bash
注释需要写在花括号中，实例如下：
ReactDOM.render(
    <div>
    <h1>菜鸟教程</h1>
    {/*注释...*/}
     </div>,
    document.getElementById('example')
);
```
### 关于React注释的问题
	关于React注释的问题：
	1、在标签内部的注释需要花括号
	2、在标签外的的注释不能使用花括号
```bash
ReactDOM.render(
    <div>
    <h1>菜鸟教程</h1>
    {/*注释...*/}
     </div>,
    document.getElementById('example')
);
```

### 数组
```bash
JSX 允许在模板中插入数组，数组会自动展开所有成员：
var arr = [
  <h1>菜鸟教程</h1>,
  <h2>学的不仅是技术，更是梦想！</h2>,
];
ReactDOM.render(
  <div>{arr}</div>,
  document.getElementById('example')
);
```

### HTML 标签 vs. React 组件
```bash
React 可以渲染 HTML 标签 (strings) 或 React 组件 (classes)。

要渲染 HTML 标签，只需在 JSX 里使用小写字母的标签名。
var myDivElement = <div className="foo" />;
ReactDOM.render(myDivElement, document.getElementById('example'));

要渲染 React 组件，只需创建一个大写字母开头的本地变量。
var MyComponent = React.createClass({/*...*/});
var myElement = <MyComponent someProperty={true} />;
ReactDOM.render(myElement, document.getElementById('example'));
React 的 JSX 使用大、小写的约定来区分本地组件的类和 HTML 标签。
```
### 注意:
	由于 JSX 就是 JavaScript，一些标识符像 class 和 for 不建议作为 XML 属性名。
	作为替代，React DOM 使用 className 和 htmlFor 来做对应的属性。

## React 组件
	封装一个输出 "Hello World！" 的组件，组件名为 HelloMessage：
```bash
var HelloMessage = React.createClass({
  render: function() {
    return <h1>Hello World！</h1>;
  }
});
 
ReactDOM.render(
  <HelloMessage />,
  document.getElementById('example')
);
```
### 实例解析：
```bash
React.createClass 方法用于生成一个组件类 HelloMessage。
<HelloMessage /> 实例组件类并输出信息。
```
### 注意:
	1.原生 HTML 元素名以小写字母开头，而自定义的 React 类名以大写字母开头，比如 HelloMessage 不能写成 helloMessage。
	2.组件类只能包含一个顶层标签，否则也会报错。
### 向组件传递参数，使用 this.props 对象
```bash
var HelloMessage = React.createClass({
  render: function() {
    return <h1>Hello {this.props.name}</h1>;
  }
});
 
ReactDOM.render(
  <HelloMessage name="Runoob" />,
  document.getElementById('example')
);

```
<font color="red">
注意:在添加属性时， class 属性需要写成 className ，for 属性需要写成 htmlFor ，
这是因为 class 和 for 是 JavaScript 的保留字。
</font>

### 复合组件
	我们可以通过创建多个组件来合成一个组件，即把组件的不同功能点进行分离。
```bash
以下实例我们实现了输出网站名字和网址的组件：
var WebSite = React.createClass({
  render: function() {
    return (
      <div>
        <Name name={this.props.name} />
        <Link site={this.props.site} />
      </div>
    );
  }
});
 
var Name = React.createClass({
  render: function() {
    return (
      <h1>{this.props.name}</h1>
    );
  }
});
 
var Link = React.createClass({
  render: function() {
    return (
      <a href={this.props.site}>
        {this.props.site}
      </a>
    );
  }
});
 
ReactDOM.render(
  <WebSite name="菜鸟教程" site=" http://www.runoob.com" />,
  document.getElementById('example')
);
```

## React State(状态)
	React 把组件看成是一个状态机（State Machines）。React只需更新组件的 state，然后根据新的 state 重新渲染用户界面（不要操作 DOM）。
	以下实例中创建了 LikeButton 组件，getInitialState 方法用于定义初始状态，也就是一个对象，这个对象可以通过 this.state 属性读取。
	当用户点击组件，导致状态变化，this.setState 方法就修改状态值，每次修改以后，自动调用 this.render 方法，再次渲染组件。
```bash
<script type="text/babel">
	//组件类(首字母大写且只能包含一个顶层标签)
    var LikeButton = React.createClass({
        getInitialState: function() {/*这个方法名称是已经定义好的，不能改名称。用于定义初始状态*/
        	return {liked: false};
        },
        handleClick: function(event) {
        	/*state的值发生变化，自动调用this.render方法渲染*/
            this.setState({liked: !this.state.liked});
        },
        render: function() {
            var text = this.state.liked ? '喜欢' : '不喜欢';
            //但是如果有元素内嵌，需要多行展示，则需要 return()，否则报错。
            return (
                <p onClick={this.handleClick}>
            	你<b>{text}</b>我。点我切换状态。
            </p>
          );
        }
    });
    ReactDOM.render(
    	<LikeButton />,
    	document.getElementById('example')
    );
</script>
```
## React Props
	state 和 props 主要的区别在于 props 是不可变的，而 state 可以根据与用户交互来改变。
	这就是为什么有些容器组件需要定义 state 来更新和修改数据。 而子组件只能通过 props 来传递数据。

### 使用 Props
```bash
var HelloMessage = React.createClass({
    render: function() {
      return <h1>Hello {this.props.name}</h1>;
    }
  });

  ReactDOM.render(
    <HelloMessage name="Runoob" />,
    document.getElementById('example')
  );
```
### 默认 Props
	可以通过 getDefaultProps() 方法为 props 设置默认值，实例如下：
```bash
var HelloMessage = React.createClass({
  getDefaultProps: function() {
    return {
      name: 'Runoob'
    };
  },
  render: function() {
    return <h1>Hello {this.props.name}</h1>;
  }
});
 
ReactDOM.render(
  <HelloMessage />,
  document.getElementById('example')
);

```
### State 和 Props
	以下实例演示了如何在应用中组合使用 state 和 props 。我们可以在父组件中设置 state， 
	并通过在子组件上使用 props 将其传递到子组件上。在 render 函数中, 我们设置 name 和 site 来获取父组件传递过来的数据。
```bash
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>菜鸟教程 React 实例</title>
    <script src="https://cdn.bootcss.com/react/15.4.2/react.min.js"></script>
	<script src="https://cdn.bootcss.com/react/15.4.2/react-dom.min.js"></script>
	<script src="https://cdn.bootcss.com/babel-standalone/6.22.1/babel.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
    var WebSite = React.createClass({
      getInitialState: function() {
        return {
          name: "菜鸟教程",
          site: "http://www.runoob.com"
        };
      },
      render: function() {
        return (
          <div>
            <Name name={this.state.name} />
            <Link site={this.state.site} />
          </div>
        );
      }
    });
    var Name = React.createClass({
      render: function() {
        return (
          <h1>{this.props.name}</h1>
        );
      }
    });
    var Link = React.createClass({
      render: function() {
        return (
          <a href={this.props.site}>
            {this.props.site}
          </a>
        );
      }
    });
    ReactDOM.render(
      <WebSite />,
      document.getElementById('example')
    );
    </script>
  </body>
</html>
```
### Props 验证
	Props 验证使用 propTypes，它可以保证我们的应用组件被正确使用，React.PropTypes提供很多验证器 (validator) 来验证传入数据是否有效。
	当向 props 传入无效数据时，JavaScript 控制台会抛出警告。
#### 单个
```bash
以下实例创建一个 Mytitle 组件，属性 title 是必须的且是字符串，非字符串类型会自动转换为字符串 ：
var title = "菜鸟教程";
// var title = 123;
var MyTitle = React.createClass({
  propTypes: {
    title: React.PropTypes.string.isRequired,
  },
 
  render: function() {
     return <h1> {this.props.title} </h1>;
   }
});
ReactDOM.render(
    <MyTitle title={title} />,
    document.getElementById('example')
);
```
#### 多个
```bash

```