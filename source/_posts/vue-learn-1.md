---
title: vue2.0 基本语法1
date: 2018-02-23 18:16:06
author: T.2stt
comments: true
tags:
- Vue
categories:
- [MV*框架,Vue]
---
>vuejs轻量级，MVVM 结构。以下是其基本语法，包括：简单的demo，以及API的描述（配置，公共API），模板渲染的前端框架（数据渲染，组件化/模块化其他功能：路由，ajax，数据流）等的介绍

## 最简单的例子
### demo
``` bash 
<body>
    <div id="app">
        <h1>姓名：{{ Name }}</h1>
        <h1>年龄：{{ Age }}</h1>
        <h1>学校：{{ School }}</h1>
    </div>

    <script src="Content/vue/dist/vue.js"></script>
    <script type="text/javascript">
    //Model
    var data = {
        Name: '小明',
        Age: 18,
        School:'光明小学',
    }
    //ViewModel
    var vue = new Vue({
        el: '#app',
        data: data,
    });
    </script>
</body>
```
### 解释
``` bash
1.Model就是data变量
2.ViewModel就是这里的new Vue()得到的对象
  2.1 el----表示绑定的Dom元素，此例子中表示的是父级的Dom元素
  2.2 data----需要绑定的数据Model
3.展示----{{变量}}--》{{ Name }}，值得一提的是{{变量}}这种写法仅仅只能实现单向绑定
```
## API
### 全局配置 ---- Vue.config
#### Vue.config 是一个对象，包含 Vue 的全局配置。可以在启动应用之前，预先修改下列属性：
属性 | 作用  | 用法
---|---|---
<font color=#0099ff size=3 face="黑体">silent</font> | 是否禁止所有 Vue 的日志(log)与警告(warning) | Vue.config.silent = true
<font color=#0099ff size=3 face="黑体">optionMergeStrategies</font> | 用于定义选项(options)的自定义合并策略,合并策略函数分别接收的参数：定义在父实例上的选项(options)作为第一个参数，定义在子实例上的选项(options)作为第二个参数，Vue 实例上下文作为第三个参数传入。 | Vue.config.optionMergeStrategies._my_option = function (parent, child, vm) {<br /> return child + 1<br />}<br />const Profile = Vue.extend({<br />_my_option: 1<br />})<br />// Profile.options._my_option = 2
<font color=#0099ff size=3 face="黑体">devtools</font> | 是否允许 vue-devtools 审查应用程序。在开发版本的默认值是 true，在生产版本的默认值是 false。可以将生产版本的值设为 true，以启用审查 | Vue.config.devtools = true// 务必在加载 Vue 之后，立即同步设置以下内容
<font color=#0099ff size=3 face="黑体">errorHandler</font> | 设置一个处理函数，用于在组件渲染函数调用和 watcher 期间捕获错误。这个处理函数被调用时，传入 error 对象和 Vue 实例。 | Vue.config.errorHandler = function (err, vm, info) {<br/>// 处理错误,`info` 是 Vue 特有的错误信息，例如，错误是在哪个生命周期钩子函数中发现的<br/>}
<font color=#0099ff size=3 face="黑体">warnHandler</font> | 为运行时(runtime)下的 Vue 警告设置一个自定义处理函数。注意，这只会在开发环境下生效，在生产环境下设置会被忽略。 | Vue.config.warnHandler = function (msg, vm, trace) {<br/>// `trace` 是组件层级结构的追踪信息<br/>}
<font color=#0099ff size=3 face="黑体">ignoredElements</font> | 帮助 Vue 忽略不受 Vue 管理的自定义元素（例如，使用 Web Components API 的那些自定义元素）否则，Vue 会错误地认为你是忘记注册全局组件或将组件名称拼写错误，然后抛出一条Unknown custom element（意思是：未知的自定义元素）的警告 | Vue.config.ignoredElements = ['my-custom-web-component','another-web-component',// 使用`正则表达式(RegExp)`忽略所有以 "ion-" 开头的元素, // 仅在 2.5+ 版本中支持表达式用法,/^ion-/]
<font color=#0099ff size=3 face="黑体">keyCodes</font> | 设置 v-on 自定义按键别名。 |js: Vue.config.keyCodes = {<br/>v: 86,<br/>f1: 112,<br/>// 无法识别驼峰式命名(camelCase)<br/>mediaPlayPause: 179,<br/>// 可以识别的是双引号括起来的串联式命名(kebab-case)<br/>"media-play-pause": 179,up: [38, 87]} <br/>html:<<br/>input type="text" @keyup.media-play-pause="method"/<br/>>
<font color=#0099ff size=3 face="黑体">performance</font> | 设置为 true，以在浏览器开发工具 timeline 中，启用组件初始化(init)、编译(compile)、渲染(render)和修补(patch)的性能追踪。只能在开发模式和支持 performance.mark API 的浏览器中运行。| Vue.config.performance = true
<font color=#0099ff size=3 face="黑体">productionTip</font> | 设置为 false，以禁止在 Vue 启动时的生产提示。 | Vue.config.productionTip = false
### 全局API

#### Vue.extend
Vue.extend( options ) :使用 Vue 的基础构造函数，创建一个“子类(subclass)”。参数是一个包含组件选项的对象。

#### Vue.nextTick
Vue.nextTick( [callback, context] )：在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。

#### Vue.set
Vue.set( target, key, value ):设置对象的属性。如果对象是响应式的，确保属性被创建后也是响应式的，同时触发视图更新。这个方法主要用于避开 Vue 不能检测属性被添加的限制。注意对象不能是 Vue 实例，或者 Vue 实例的根数据对象。

#### Vue.delete
Vue.delete( target, key )：删除对象的属性。如果对象是响应式的，确保删除能触发更新视图。这个方法主要用于避开 Vue 不能检测到属性被删除的限制，但是你应该很少会使用它。

#### Vue.directive
Vue.directive( id, [definition] )：注册或获取全局指令

#### Vue.filter
Vue.filter( id, [definition] ):注册或获取全局过滤器。

#### Vue.component
Vue.component( id, [definition] ):注册或获取全局组件。注册还会自动使用给定的id设置组件的名称

#### Vue.use
Vue.use( plugin ):安装 Vue.js 插件。如果插件是一个对象，必须提供 install 方法。如果插件是一个函数，它会被作为 install 方法。install 方法将被作为 Vue 的参数调用。
当 install 方法被同一个插件多次调用，插件将只会被安装一次。

#### Vue.mixin
Vue.mixin( mixin ):全局注册一个混合，影响注册之后所有创建的每个 Vue 实例。插件作者可以使用混合，向组件注入自定义的行为。不推荐在应用代码中使用。

#### Vue.compile
Vue.compile( template ):在render函数中编译模板字符串。只在完整构建时有效

#### Vue.version
提供字符串形式的 Vue 安装版本号。这对社区的插件和组件来说非常有用，你可以根据不同的版本号采取不同的策略。

### 选择

## 模板渲染
``` bash
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>

var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // a computed getter
    reversedMessage: function () {
      // `this` points to the vm instance
      return this.message.split('').reverse().join('')
    }
  }
})
结果：
Original message: "Hello"
Computed reversed message: "olleH"
```

## 数据的双向绑定

## 组件
创建一个组件包括如下三个步骤
### 定义

### 注册

### 创建根实例

## 路由

## ajax
## 数据流
