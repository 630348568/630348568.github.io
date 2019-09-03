---
title: vue的style的scoped属性
date: 2018-02-28 08:14:50
author: T.2stt
tags:
- Vue
- Vue-css
categories:
- [MV*框架,Vue]
---

> 在vue组件中，在style标签上添加scoped属性，以表示它的样式作用于当下的模块，很好的实现了样式私有化的目的，这是一个非常好的机制。但是要慎用,在实际业务中我们往往会对公共组件样式做细微的调整，如果添加了scoped属性，那么样式将会变得不易修改。

## scoped有作用域的 CSS
`scoped`肯定是解决了样式私有化的问题，但同时也引入了新的问题---样式不易修改，而很多时候，我们是需要对公共组件的样式做微调的。所以该属性的使用需要谨慎考虑
### demo
```javascript
<style scoped>
.example {
  color: red;
}
</style>

<template>
  <div class="example">hi</div>
</template>
```
转换结果：（以达到私有的效果）
```javascript
<style>
.example[data-v-f3f3eg9] {
  color: red;
}
</style>

<template>
  <div class="example" data-v-f3f3eg9>hi</div>
</template>
```
### 渲染规则
总结一下`scoped`三条渲染规则
- 给`HTML`的`DOM`节点加一个不重复`data`属性(形如：data-v-2311c06a)来表示他的唯一性
- 在每句`css`选择器的末尾（编译后的生成的css语句）加一个当前组件的`data`属性选择器（如[data-v-2311c06a]）来私有化样式
- 如果组件内部包含有其他组件，只会给其他组件的最外层标签加上当前组件的data属性

## 注意
### 可混用本地和全局样式
我们可以在一个组件中同时使用有作用域和无作用域的样式：
```bash
<style>
/* 全局样式 */
</style>

<style scoped>
/* 本地样式 */
</style>
```
### 子组件的根元素
使用 `scoped` 后，父组件的样式将不会渗透到子组件中。不过一个子组件的根节点会同时受其父组件有作用域的 CSS 和子组件有作用域的 CSS 的影响。这样设计是为了让父组件可以从布局的角度出发，调整其子组件根元素的样式。

### 深度作用选择器
如果你希望 scoped 样式中的一个选择器能够作用得“更深”，例如影响子组件，你可以使用 >>> 操作符：
```bash
<style scoped>
.a >>> .b { /* ... */ }
</style>
```
上述代码将会编译成：
```bash
.a[data-v-f3f3eg9] .b { /* ... */ }
```
有些像 Sass 之类的预处理器无法正确解析 >>>。这种情况下你可以使用 /deep/ 操作符取而代之——这是一个 >>> 的别名，同样可以正常工作。