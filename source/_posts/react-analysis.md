---
title: 我对react理解 + 温习
date: 2019-02-25 16:26:00
author: T.2stt
comments: true
tags:
- react
categories:
- react
---
>React的新理解 + 注意事项 + 语法

## JSX
[官网](http://caibaojian.com/react/jsx.html)就是为了把 HTML 模板直接嵌入到 JS 代码里面，这样就做到了模板和组件关联

### 使用 JSX
[官网](http://caibaojian.com/react/jsx-in-depth.html) 
利用 JSX 编写 DOM 结构，可以用原生的 HTML 标签（小写的字符串），也可以直接像普通标签一样引用 React 组件（大写开头的变量）。
<font color="red">JSX 中 HTML 不一样的地方 </font>
class 写成 className，for 写成 htmlFor

#### 使用 JavaScript 表达式
[官网](http://caibaojian.com/react/jsx-in-depth.html) 
属性值使用表达式，只要用 {} 替换 "":

#### 注释
沿用 JavaScript，唯一要注意的是在一个组件的子元素位置使用注释要用 {} 包起来。

#### HTML 转义
&copy; (©) 最后显示到 DOM 中不会正确显示，因为 React 自动把 &copy; 中的特殊字符转义了
解决办法：
```bash
直接使用 UTF-8 字符 ©
使用对应字符的 Unicode 编码，查询编码
使用数组组装 <div>{['cc ', <span>&copy;</span>, ' 2015']}</div>
直接插入原始的 HTML
```
---
### 属性扩散（ ... ）
用于有时需要给组件设置多个属性，不想一个个写下这些属性，或者不知道这些属性的名称
```bash
var props = {};
props.foo = x;
props.bar = y;
var component = <Component {...props} />;
props 对象的属性会被设置成 Component 的属性。
属性也可以被覆盖：
var props = { foo: 'default' };
var component = <Component {...props} foo={'override'} />;
console.log(component.props.foo); // 'override'
```
---------

## React 组件
组件有两个核心概念：
- props: 就是组件的属性，由外部通过 JSX 属性传入设置，一旦初始设置完成，就可以认为 this.props 是不可更改的。
- state:state 是组件的当前状态，可以把组件简单看成一个“状态机”，根据状态 state 呈现不同的 UI 展示。一旦状态（数据）更改，组件就会自动调用 render 重新渲染 UI，这个更改的动作会通过 this.setState 方法来触发。

### 组件生命周期
一个组件类由 extends Component 创建，并且提供一个 render 方法以及其他可选的生命周期函数、组件相关的事件或方法来定义。
#### getInitialState
初始化 this.state 的值，只在组件装载之前调用一次。
```bash
class Counter extends Component {
  constructor(props) {
    super(props);
    this.state = { count: props.initialCount };
  }

  render() {
    // ...
  }
}
```
#### getDefaultProps
Counter.defaultProps = { initialCount: 0 };

#### render(必须要有)
组装生成这个组件的 HTML 结构（使用原生 HTML 标签或者子组件），也可以返回 null 或者 false，这时候 ReactDOM.findDOMNode(this) 会返回 null。

#### 生命周期函数
- 装载组件触发
<font color="red">componentWillMount</font>：只会在装载之前调用一次，render 之前调用，你可以在这个方法里面调用 setState 改变状态，并且不会导致额外调用一次 render；
<font color="red">componentDidMount</font>：只会在装载完成之后调用一次，在 render 之后调用，从这里开始可以通过 ReactDOM.findDOMNode(this) 获取到组件的 DOM 节点。
- 更新组件触发（不会在首次 render 组件的周期调用）
<font color="red">componentWillReceiveProps</font>：啊发顺丰
<font color="red">shouldComponentUpdate</font>：阿斯顿发多少
<font color="red">componentWillUpdate</font>：大师傅
<font color="red">componentDidUpdate</font>：打发第三方
- 卸载组件触发
<font color="red">componentWillUnmount</font>：按时发生

### 事件处理
？
### DOM 操作
ReactDOM.render:它会返回对组件的引用也就是组件实例（对于无状态状态组件来说返回 null）
#### findDOMNode()
当组件加载到页面上之后（mounted），你都可以通过 react-dom 提供的 findDOMNode() 方法拿到组件对应的 DOM 元素。不能用在无状态组件上。
```bash
import { findDOMNode } from 'react-dom';

// Inside Component class
componentDidMound() {
  const el = findDOMNode(this);
}
```
#### Refs(相当于ID)
通过在要引用的 DOM 元素上面设置一个 ref 属性指定一个名称，然后通过 this.refs.name 来访问对应的 DOM 元素。
如果 ref 是设置在原生 HTML 元素上，它拿到的就是 DOM 元素，如果设置在自定义组件上，它拿到的就是组件实例，这时候就需要通过 findDOMNode 来拿到组件的 DOM 元素。如果想要拿无状态组件的 DOM 元素的时候，就需要用一个状态组件封装一层，然后通过 ref 和 findDOMNode 去获取。
- 例如：有一种情况是必须直接操作 DOM 来实现的，你希望一个 input 元素在你清空它的值时 focus，你没法仅仅靠 state 来实现这个功能。

#### 总结
- 你可以使用 ref 到的组件定义的任何公共方法，比如 this.refs.myTypeahead.reset()
- Refs 是访问到组件内部 DOM 节点唯一可靠的方法
- Refs 会自动销毁对子组件的引用（当子组件删除时）

#### 注意事项
- 不要在 render 或者 render 之前访问 refs
- 不要滥用 refs，比如只是用它来按照传统的方式操作界面 UI：找到 DOM -> 更新 DOM

### 组合组件
在 React 组件中要包含其他组件作为子组件，只需要把组件当作一个 DOM 元素引入就可以了。

#### 循环插入子元素
如果组件中包含通过循环插入的子元素，为了保证重新渲染 UI 的时候能够正确显示这些子元素，每个元素都需要通过一个特殊的 key 属性指定一个唯一值。
？

### 组件间通信
#### 父子组件间通信
- 父->子 通过 props 属性传递，在父组件给子组件设置 props，然后子组件就可以通过 props 访问到父组件的数据／方法，这样就搭建起了父子组件间通信的桥梁
- 父组件访问子组件 用 refs

#### 非父子组件间的通信
- 公共参数
- Redux

## Data Flow
- 官方的 Flux
- 更优雅的 Redux
### Flux
Flux 是 Facebook 使用的一套前端应用的架构模式。
- the dispatcher:处理动作分发，维护 Store 之间的依赖关系
- the stores:数据和逻辑部分
- the views:React 组件，这一层可以看作 controller-views，作为视图同时响应用户交互
- the actions:提供给 dispatcher 传递数据给 store
```bash
数据流向（单向）
Action -> Dispatcher -> Store -> View
```
### Redux
Redux(oldState) => newState

#### 三个基本原则
- 整个应用只有唯一一个可信数据源，也就是只有一个 Store
- State 只能通过触发 Action 来更改
- State 的更改必须写成纯函数，也就是每次更改总是返回一个新的 State，在 Redux 里这种函数称为 Reducer

#### Actions
就是一个单纯的包含 { type, payload } 的对象,Action 需要通过 store.dispatch() 方法来发送。
- type 是一个常量用来标示动作类型
- payload 是这个动作携带的数据。
一般来说，会使用函数（Action Creators）来生成 action，这样会有更大的灵活性，Action Creators 是一个 pure function，它最后会返回一个 action 对象：
```bash
function addTodo(text) {
  return {
    type: 'ADD_TODO',
    text
  }
}
所以现在要触发一个动作只要调用 dispatch: dispatch(addTodo(text))
```
#### Reducers
用来处理 Action 触发的对状态树的更改。所以一个 reducer 函数会接受 oldState 和 action 两个参数，返回一个新的 state：(oldState, action) => newState。一个简单的 reducer 可能类似这样：
```javascript
const initialState = {
  a: 'a',
  b: 'b'
};

function someApp(state = initialState, action) {
  switch (action.type) {
    case 'CHANGE_A':
      return { ...state, a: 'Modified a' };
    case 'CHANGE_B':
      return { ...state, b: action.payload };
    default:
      return state
  }
}
// 运用了扩展符号：...state确保不会更改到 oldState 而是返回一个 newState
// 对于不需要处理的 action，直接返回 oldState
```
##### combineReducers
Redux 提供了一个工具函数 combineReducers 来简化这种 reducer 合并：

#### Store
Store 的作用就是连接 Action 和 Reducer，Store 的作用有这么几个：
- Hold 住整个应用的 State 状态树
- 提供一个 getState() 方法获取 State
- 提供一个 dispatch() 方法发送 action 更改 State
- 提供一个 subscribe() 方法注册回调函数监听 State 的更改
创建一个 Store
```javascript
import { createStore } from 'redux';
import someApp from './reducers';
let store = createStore(someApp);

// 你也可以额外指定一个初始 State（initialState），这对于服务端渲染很有用
// let store = createStore(someApp, window.STATE_FROM_SERVER);
let unsubscribe = store.subscribe(() => console.log(store.getState()));

// Dispatch
store.dispatch({ type: 'CHANGE_A' });
store.dispatch({ type: 'CHANGE_B', payload: 'Modified b' });

// Stop listening to state updates
unsubscribe();
```

#### Redux 基础总结
单向数据流
```bash
store.dispatch(action) -> reducer(state, action) -> store.getState()
```

- 调用 store.dispatch(action)
Action 是一个包含 { type, payload } 的对象，它描述了“发生了什么”，比如：
```javascript
{ type: 'LIKE_ARTICLE', articleID: 42 }
{ type: 'FETCH_USER_SUCCESS', response: { id: 3, name: 'Mary' } }
{ type: 'ADD_TODO', text: 'Read the Redux docs.' }
// 可以在任何地方调用 store.dispatch(action)，比如组件内部，Ajax 回调函数里面等等。
```

- Action 会触发给 Store 指定的 root reducer
root reducer 会返回一个完整的状态树，State 对象上的各个字段值可以由各自的 
reducer 函数处理并返回新的值。
```
1. reducer 函数接受 (state, action) 两个参数
2. reducer 函数判断 action.type 然后处理对应的 action.payload 数据来更新并返回一个新的 state
```
- Store 会保存 root reducer 返回的状态树
新的 State 会替代旧的 State，然后所有 store.subscribe(listener) 注册的回调函数会被调用，在回调函数里面可以通过 store.getState() 拿到新的 State。

#### 在 React 应用中使用 Redux
Redux 也是需要注册一个回调函数 store.subscribe(listener) 来获取 State 的更新，然后我们要在 listener 里面调用 setState() 来更新 React 组件。
react-redux 只有两个 API
##### 元素Provider
Provider 作为一个容器组件，用来接受 Store，并且让 Store 对子组件可用，用法如下：
```javascript
import { render } from 'react-dom';
import { Provider } from 'react-redux';
import App from './app';
render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('root')
);
// 这时候 <Provider> 里面的子组件 <App /> 才可以使用 connect 方法关联 store。
```
##### Connect
connect([mapStateToProps], mapDispatchToProps], [mergeProps], [options])，它最多接受4个参数，都是可选的，并且这个方法调用会返回另一个函数，这个返回的函数来接受一个组件类作为参数，最后才返回一个和 Redux store 关联起来的新组件，类似这样：
```javascript
class App extends Component { ... }

export default connect()(App);
```
这样就可以在 App 这个组件里面通过 props 拿到 Store 的 dispatch 方法，但是注意现在的 App 没有监听 Store 的状态更改，如果要监听 Store 的状态更改，必须要指定 mapStateToProps 参数。
先来看它的参数：
```javascript
- [mapStateToProps(state, [ownProps]): stateProps]: 
第一个可选参数是一个函数，只有指定了这个参数，这个关联（connected）组件才会监听 Redux Store 的更新，
每次更新都会调用 mapStateToProps 这个函数，返回一个字面量对象将会合并到组件的 props 属性。 
ownProps 是可选的第二个参数，它是传递给组件的 props，当组件获取到新的 props 时，
ownProps 都会拿到这个值并且执行 mapStateToProps 这个函数。

- [mapDispatchProps(dispatch, [ownProps]): dispatchProps]: 
这个函数用来指定如何传递 dispatch 给组件，在这个函数里面直接 dispatch action creator，
返回一个字面量对象将会合并到组件的 props 属性，这样关联组件可以直接通过 props 调用到 action，
Redux 提供了一个 bindActionCreators() 辅助函数来简化这种写法。
 如果省略这个参数，默认直接把 dispatch 作为 props 传入。ownProps 作用同上。
```
[demo](http://caibaojian.com/react/usage-with-react.html)
这个 connected 的组件必须放到 <Provider> 的容器里面，当 State 更改的时候就会自动调用 mapStateToProps 和 mapDispatchProps 从而更新组件的 props。 组件内部也可以通过 props 调用到 action，如果没有省略了 mapDispatchProps，组件要触发 action 就必须手动 dispatch，类似这样：this.props.dispatch(someActionCreator('arg'))。