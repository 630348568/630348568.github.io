---
title: Vue 源码
date: 2019-07-19 10:23:48
author: T.2stt
tags:
- Vue
- Vue-css
categories:
- [MV*框架,Vue]
---

> `Vue的工作机制` (初始化、编译、响应式、虚拟dom、更新视图) 、 `响应式原理Object.defineProperty`, `实战数据响应式`(依赖收集、依赖追踪)、 编译compile 、 入口文件 、 Dep 、watch 、 监听器 、Vuex原理 、Vue-router原理

## Vue工作机制

{% asset_img base1.png 图片介绍整个流程 %}
### 初始化
- 在 `new Vue()` 之后进行初始化，初始化生命周期 、事件 、props 、methods 、data 、computed 与 watch 等。
- 最重要的是通过 `Object.defineProperty` 设置 `setter` 与 `getter` ， 用来实现响应式以及依赖收集。

### 初始化之后调用 $mount 挂在组件
将接下来写的所有的东西都挂在在我们写的主页 `id=app` 下
```
<div  id = "app"> </div>
```
### compile()编译
#### 对模板 template 渲染
- 对模板 `template` 进行一遍扫描（parse 、 optimize、 generate）生成一些渲染函数，`vDom（虚拟dom）`。
    1. <font color="red">parse:</font> 使用正则解析 `template` 中的 `vue的指令（v-xx）`变量等，形成语法树`AST`；
    2. <font color="red">optimize:</font> 标记一些静态节点，用作后面的性能优化，在`diff`的时候直接略过；
    3. <font color="red">generate：</font>把第一部生成的AST转化为渲染函数`render function`。
- 即我们在数据更新的时候并非真实的去改变dom的数据，而是改变的vDom的数值，在准备更新之前会做一个 `differ` 算法的比较，通过最新的值和之前的值比较，计算出做的最小的更新
- 然后打补丁执行到 `patch()` 做界面的更新
- <font color="red">目的：</font>用 `cpu`（js的计算）换dom操作的时间。浏览器的瓶颈就是在页面的渲染比较慢，vue的 `核心是减少页面渲染的次数和数量`。
```
1. parse: 使用正则解析template中的vue的指令（v-xx）变量等，形成语法树AST；
2. optimize: 标记一些静态节点，用作后面的性能优化，在diff的时候直接略过；
3. generate： 把第一部生成的AST转化为渲染函数render function。
```

#### 依赖收集watch
对界面中绑定的地方，通过依赖收集知道其绑定关系。知道当数据发生变化时知道去更改哪些东西
<font color="red">简化版</font>
{% asset_img base2.png 简化版 %}

## 代码实现以上内容
### Obect.defineProperty
新建一个 `defineProperty.html`
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id="app">
        <p id="name"></p>
    </div>
    <script>
        var obj = {};
        // 要给obj定义一个name，给其挂载两个方法
        Object.defineProperty(obj, 'name', {
            get: function(){
                return document.querySelector('#name').innerHTML;
            },
            set: function(val){ // 设置name的时候会走set函数
                document.querySelector('#name').innerHTML = val;
            }
        });
        obj.name = 'Jerry'; // 改变name值了
    </script>
</body>
</html>
<!-- 界面上显示了 Jerry -->
```
### 实现数据响应式
- 新建一个 `kvue.js`; 数据的劫持

```
// 希望将来这个类的用法 new KVue({data: {...}})
class KVue {
    constructor(options){ // options构造函数中需要接收的对象（用户传入的）
        this.$options = options; // 缓存一下options 其他类需要用到

        // 数据的响应化
        this.$data = options.data;
        // 对齐遍历设置set get
        this.observe(this.$data);
    }
    observe(value){
        if (!value || typeof value !== 'object'){
            return ;
        }
        // 遍历该对象
        Object.keys(value).forEach( key => {
            // 定义响应化
            this.defineReactive(value, key, value[key]);
        })
    }
    // 数据响应化
    defineReactive(obj, key, val){
        this.observe(val); // 递归-解决数据的嵌套问题，如果没有这句话则只打印出-test属性更新了： hellow, Kaikebal,没有foo的变化
        // test属性更新了： hellow, Kaikebal
        // bar属性更新了： oh my bar
        Object.defineProperty(obj, key, {
            get(){ // get: function(){} 等效
                return val;
            },
            set(newVal){
                if (newVal === val) return ;
                val = newVal;
                console.log(`${key}属性更新了： ${val}`);
            }
        })
    }
}
```
引用
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <script src="kvue.js"></script>
    <script>
        const app = new KVue({
            data: {
                test: 'I am test',
                foo: {
                    bar: 'bar'
                }
            }
        })
        app.$data.test = "hellow, Kaikebal";
        app.$data.foo.bar = "oh my bar";
    </script>
</body>
</html>
```
<font color="red">vue数据绑定的原理</font>利用了 `Object.defineProperty` 这个属性，把 `data` 里面的放的数据每一个属性都定义了 `get和set`，当数据变化的时候我们通知那些需要更新的地方去更新

### 依赖收集
当数据更新的时候我们需要去做界面的更新，以上的KVue只做了 
```
console.log(`${key}属性更新了： ${val}`);
```
#### 例子解析
```
new Vue({
    template: 
    `<div>
        <span>{{name1}}</span>
        <span>{{name2}}</span>
        <span>{{name1}}</span>
    </div>
    `,
    data: {
        name1: 'name1',
        name2: 'name2',
        name3: 'name3'
    },
    created(){
        this.name1 = '开课吧';
        this.name3 = 'Jerry';

    }
})
```
<font color="red">修改1和2会发生什么事情</font>
- name1 被修改，视图更新，且要更新两处；
- name2 被修改，视图更新；
- name3 没用到，不需要更新；
name1 通过依赖收集的方式，发现界面中有两个依赖，当name发生变化的时候我去通知。若name3发生了变化不做任何通知，因为没做任何依赖。
实现以上操作，需要扫描视图收集依赖，知道视图中到底哪些地方对数据有依赖，这样当数据变化时就能有的放矢了。

#### 实现展示
dep 和 watch 的功能实现
在之前的`Kvue.js`继续
```
class KVue {
    constructor(options){ // options构造函数中需要接收的对象（用户传入的）
        this.$options = options; // 缓存一下options 其他类需要用到

        // 数据的响应化
        this.$data = options.data;
        // 对齐遍历设置set get
        this.observe(this.$data);

        // 模拟watcher创建
        new Watcher();
        this.$data.test; // 激活了get
        new Watcher();
        this.$data.foo.bar; // 激活了get
    }
    observe(value){
        if (!value || typeof value !== 'object'){
            return ;
        }
        // 遍历该对象
        Object.keys(value).forEach( key => {
            // 定义响应化
            this.defineReactive(value, key, value[key]);
        })
    }
    // 数据响应化
    defineReactive(obj, key, val){
        this.observe(val); // 递归-解决数据的嵌套问题，如果没有这句话则只打印出-test属性更新了： hellow, Kaikebal,没有foo的变化
        // test属性更新了： hellow, Kaikebal
        // bar属性更新了： oh my bar

        // 实现依赖
        const dep = new Dep();

        Object.defineProperty(obj, key, {
            get(){ // get: function(){} 等效
                Dep.target && dep.addDep(Dep.target); // 测试watcher,往dep中加入依赖
                return val;
            },
            set(newVal){
                if (newVal === val) return ;
                val = newVal;
                // console.log(`${key}属性更新了： ${val}`);
                dep.notify(); // 做通知操作 
            }
        })
    }
}
// Dep： 用来管理 watcher 的，观察者的集合
class Dep{
    constructor(){
        this.deps = []; // 存放的是若干依赖(watcher) 一个watcher对应一个属性
    }

    addDep(dep) { // 收集依赖
        this.deps.push(dep);
    }

    notify(){ // 通知所有依赖watcher去更新
        this.deps.forEach(dep => {
            dep.update()
        });
    }
}

// watcher
class Watcher {
    constructor(){
        // 将当前watcher实例指定到Dep静态属性target
        Dep.target = this;
    }

    update(){
        console.log('属性更新了');
    }
}
```
### 编译Compile
找到浏览器不认识的，进行遍历处理，转换一下
界面的更改
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <div id = "app">
        <!-- 插值绑定 -->
        <p>{{name}}</p>
        <!-- 指令解析 -->
        <p k-text="name"></p>
        <p>{{age}}</p>
        <p>
            {{doubleAge}}
        </p>
        <!-- 双向绑定 -->
        <input type="text" k-model="name" />
        <!-- 事件处理 -->
        <button @click="changeName">呵呵</button>
        <!-- html内容解析 -->
        <div k-html="html"></div>
    </div>
    <script src="compile.js"></script>
    <script src="kvue.js"></script>
    <script>
        const kaikeba = new KVue({
            el: '#app',
            data: {
                name: 'I am test',
                age: 12,
                html: '<button>这是一个按钮</button>'
            },
            created(){
                console.log('开始啦');
                setTimeout(() => {
                    this.name = '我是测试';
                }, 1500);
            },
            methods: {
                changeName(){
                    this.name = '哈喽， 开课吧';
                    this.age = 1;
                }
            }
        })
    </script>
</body>
</html>
```
解析以上内容，新建一个`compile.js` 文件
```
//  用法 new compile(el, Vm); el-#app， vm-vue的实例
class Compile{
    constructor(el, vm){
        this.$el = document.querySelector(el); // 要遍历的宿主节点 （#app）

        this.$vm = vm; // 其他方法要用

        // 编译
        if(this.$el) { // 如果存在就去编译 把app下的元素先拿出来
            // 转换内部内容为片段Fragment（虚拟的东西，不在界面中挂在的）
            this.$fragment = this.node2Fragment(this.$el);
            // 执行编译
            this.compile(this.$fragment);
            // 将编译完的html结果追加至$el
            this.$el.appendChild(this.$fragment);
        }
    }
    // 将宿主元素中代码片段拿出来遍历，这样做比较高效
    node2Fragment(el) {
        const frag = document.createDocumentFragment();
        // 将el中所有的子元素 **搬家** 至frag中
        let child;
        while(child = el.firstChild) { // 拿出el的首个元素赋值到el上 
            frag.appendChild(child); // 移动操作-会把el存放的内容逐渐拿出来，放在frag中，el会逐渐变少
        }
        return frag;
    }

    // 编译过程
    // 获取dom-》遍历子元素->如果是编译文本（直接编译）；获取{{}}格式变量，以及每个dom的属性，截获k-和@开头的设置响应式 
    // 如果是节点-》遍历属性=》k-开头（k-text处理textContent，k-html处理innerHtml，k-model监听input）
    // at 开头-》绑定click
    compile(el) {
        const childNodes = el.childNodes;
        Array.from(childNodes).forEach(node=>{ // 类数组中转出一个数组
            // 类型判断
            if(this.isElement(node)){
                // 元素
                // console.log('编译元素' + node.nodeName);
            } else if (this.isInterpolation(node)){
                //  文本
                // console.log('编译文本' + node.textContent);
                this.compileText(node);
            }
            // 递归子节点
            if (node.childNodes &&  node.childNodes.length>0){
                this.compile(node);
            }
        })
    }

    isElement(node){
        return node.nodeType === 1;
    }

    isInterpolation(node){ // 插值元素
        return node.nodeType === 3 && /\{\{(.*)\}\}/.test(node.textContent);
    }
    // 以上步骤展示的是{{}} 表单等
    // 编译插值文本 变成了 I am test 12 ...
    compileText(node){
        // console.error(RegExp.$1); // 获取 isInterpolation分组()的值
        // node.textContent = this.$vm[RegExp.$1]; // 但是数据发生变化了就无效了, KVue 中出发created可以看出无效果
        // 所以改为以下的
        this.update(node, this.$vm, RegExp.$1, 'text'); // 执行textUpdater
    }
    
    // 所以需要更新函数
    update(node, vm, exp, dir) { // 更新节点，vue的实例，更新的表达式，n指令（更新的是文本还是事件？）
        const updaterFn = this[dir + 'Updater']; // dir 可以是text，html，model...
        // 初始化
        updaterFn && updaterFn(node, vm[exp]);
        // 依赖收集
        new Watcher(vm, exp, function(value){ // watcher 更改
            updaterFn && updaterFn(node, value);
        })
    }

    // this[dir + 'Updater']=》 this['textUpdater']
    textUpdater(node, value){
        node.textContent = value;
    }
    
}
```
`kvue.js` 的更改
```
class KVue {
    constructor(options){ // options构造函数中需要接收的对象（用户传入的）
        this.$options = options; // 缓存一下options 其他类需要用到

        // 数据的响应化
        this.$data = options.data;
        // 对齐遍历设置set get
        this.observe(this.$data);

        // 模拟watcher创建
        // new Watcher();
        // this.$data.test; // 激活了get
        // new Watcher();
        // this.$data.foo.bar; // 激活了get

        new Compile(options.el, this);
        if(options.created) {
            options.created.call(this);
        }
    }
    observe(value){
        if (!value || typeof value !== 'object'){
            return ;
        }
        // 遍历该对象
        Object.keys(value).forEach( key => {
            // 定义响应化
            this.defineReactive(value, key, value[key]);
            // 代理data中的属性到vue的实例上
            this.proxyData(key);
        })
    }
    // 数据响应化
    defineReactive(obj, key, val){
        this.observe(val); // 递归-解决数据的嵌套问题，如果没有这句话则只打印出-test属性更新了： hellow, Kaikebal,没有foo的变化
        // test属性更新了： hellow, Kaikebal
        // bar属性更新了： oh my bar

        // 实现依赖
        const dep = new Dep();

        Object.defineProperty(obj, key, {
            get(){ // get: function(){} 等效
                Dep.target && dep.addDep(Dep.target); // 测试watcher,往dep中加入依赖
                return val;
            },
            set(newVal){
                if (newVal === val) return ;
                val = newVal;
                // console.log(`${key}属性更新了： ${val}`);
                dep.notify(); // 做通知操作 
            }
        })
    }

    proxyData(key){
        Object.defineProperty(this, key, {// this 表示vue的实例
            get(){
                return this.$data[key];
            },
            set(newVal){
                this.$data[key] = newVal; // 触发 defineReactive的data的setter
            }
        })
    }
}
// Dep： 用来管理 watcher 的，观察者的集合
class Dep{
    constructor(){
        this.deps = []; // 存放的是若干依赖(watcher) 一个watcher对应一个属性
    }

    addDep(dep) { // 收集依赖
        this.deps.push(dep);
    }

    notify(){ // 通知所有依赖watcher去更新
        this.deps.forEach(dep => {
            dep.update()
        });
    }
}

// watcher
class Watcher {
    constructor(vm, key, cb){
        this.vm = vm;
        this.key = key;
        this.cb = cb;
        // 将当前watcher实例指定到Dep静态属性target
        Dep.target = this;
        this.vm[this.key]; // 触发getter 添加依赖
        Dep.target = null; // 置空，避免重复添加
    }

    update(){
        // console.log('属性更新了');
        this.cb.call(this.vm, this.vm[this.key]); // proxyData方法使访问vm.$data 只需要 vm就可以了
    }
}
```
实现结果
```
name 在1.5秒后有 I am test 变成了 我的名字
```