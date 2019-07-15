---
title: JS构造函数/原型对象
date: 2019-03-05 11:57:50
author: T.2stt
comments: true
tags:
- js
categories:
- [js,构造函数]
---
>在典型的 oop 的语言中，如 JAVA ,都存在类的概念，类就是对象的模板，对象就是类的实例。但在 JS 中不存在类的概念，JS 不是基于类，而是通过 `构造函数`(constructor) 和 `原型链`（propotype chains）实现的。但在ES6中引入了类（class）这个概念，作为对象的模板，新的class写法知识让原型对象的写法更加清晰

# 构造函数
## 构造函数的特点
- **a** :构造函数的首字母必须大写，用来区分于普通函数;
- **b** :内部使用的this对象，来指向即将要生成的实例对象；
- **c** :使用New来生成实例对象；
```javascript
function Person(name,age){ 
     this.name = name;    
     this.age = age;   
     this.sayHello = function(){   
         console.log(this.name +"say hello");
    }
}

var boy = new Person("bella",23);    
boy.sayHello(); // bella say hello
```
## 构造函数的缺点
所有的实例对象都可以继承构造器函数中的属性和方法。但是，**同一个对象实例之间，无法共享属性**
<font color="red">【解决思路：】</font>
- **a** : 所有实例都会通过原型链引用到prototype；
- **b** : prototype相当于特定类型所有实例都可以访问到的一个公共容器；
- **c** : 那么我们就将重复的东西放到公共容易就好了
**解释**
```javascript
function Person(name,age){
    this.name = name;
    this.age = age;
    this.sayHello = function(){
        console.log(this.name + "say hello");
    }
}
var girl = new Person("bella",23);
var boy = new Person("alex",23);
console.log(girl.name);  // bella
console.log(boy.name);   // alex
console.log(girl.sayHello === boy.sayHello);  // false
```
{% asset_img 1.png 构造函数 %}
一个构造函数 `Person` 生成了两个对象实例 `girl` 和 `boy` ,并且有两个属性和一个方法。但是 `sayHello` 方法是不一样的，如上图。也就是说当 `New` 一个实例对象的时候，都会去创建一个 `sayHello` 方法，**这就浪费了内存资源**，因为sayHello方法使一样的行为的，完全可以被两个实例对象共享。
所以，缺点就是：**<font color="red">同一个构造函数的对象实例之间无法共享属性和方法。</font>**
为了解决构造函数的这个缺点，**<font color="red">js提供了prototype属相来解决该问题。。</font>**
- `propotype` 属性的作用
js中每个数据类型都是对象，除了null 和 undefined（这个可以参考另一篇将null 和 undefined的博客）,而每个对象都是继承自一个原型对象，只有null除外，它没有自己的原型对象，最终的Object的原型为null
EG:
```javascript
function Person(name,age){
    this.name = name;
    this.age = age;
}
Person.propotype.sayHello = function(){
    console.log(this.name + "say hello");
}
var girl = new Person("bella",23);
var boy = new Person("alex",23);
console.log(girl.name);  // bella
console.log(boy.name);   // alex
console.log(girl.sayHello === boy.sayHello);  // true
```
{% asset_img 2.png propotype在构造函数中的作用 %}
【解释】：由上图可以看出，`propotype是构造函数的属性`，而 `consructor` 则是构造函数的 `prototype` 属性所指向的那个对象，也就是说 **constuctor是原型对象的属性**。
`constructor` 属性是定义在原型对象上面，意味着也可以被实例对象继承

EG:
```javascript
function Person(name,age){
    this.name = name;
    this.age = age;
}
Person.propotype.sayHello = function(){
    console.log(this.name + "say hello");
}
var girl = new Person("bella",23);
console.log(girl.construcotr); // Person()
console.log(girl.construcotr == Person.propotype.construcotr); // true
```
constructor属性的作用
- **a** :分辨原型对象到底是哪个构造函数　
```javascript
function Person(){};
var person1 = new Person();
console.log(person1.construcotr === Person); // true
```
- **b** : 从实例新建另一个实例
```javascript
function Person(){};
var person1 = new Person();
var person2 = new person1.construcotr();
console.log(person2 instanceof Person); // true
``` 
- **c** : 由于constructor属性是一种原型对象和构造函数的关系，所以在修改原型对象的时候，一定 要注意construtor的指向问题，避免instanceof失真，关于这一点，会在继承中讲到。

## 原型prototype
【{% post_link js-proto %}】
