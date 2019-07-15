---
title: JS 原型 / 原型链 / 构造函数
date: 2019-03-02 17:53:02
author: T.2stt
comments: true
tags:
- prototype 原型（链）
categories:
- [js,ms]
---
>从对象的生成入手,对象其实是通过函数创建的。函数原型 `prototype`= `constructor` + `__prto__` 是为了优化同一个构造函数的对象实例之间无法共享属性和方法的缺点，将公共用的东西挂在在原型上，共同使用。而`__proto__` 则是实例化的对象的属性。原型链就是不断通过`__proto__` 去访问需要的资源
# 对象都是通过函数创建的
```javascript
//var obj = { a: 10, b: 20 };
//var arr = [5, 'x', true];
// 以上的只是语法糖
var obj = new Object();
obj.a = 10;
obj.b = 20;

var arr = new Array();
arr[0] = 5;
arr[1] = 'x';
arr[2] = true;
```
# 函数对象
- 所有 `引用类型（函数，数组，对象）` 都拥有 **__proto__** 属性（隐式原型）
- 所有 `函数` 拥有 **prototype** 属性（显式原型）（<font color="red">仅限函数</font>）
- 原型对象：拥有 `prototype` 属性的对象，在定义函数时就被创建
- `实例` 的 **隐式原型** 指向它 `构造函数` 的 **显式原型** 

# 构造函数
【{% post_link js-constructor %}】
```javascript
//创建构造函数
function Word(words){
    this.words = words;
}
Word.prototype = {
    alert(){
        alert(this.words);
    }
}
//创建实例
var w = new Word("hello world");
w.print = function(){
    console.log(this.words);
    console.log(this);  //Person对象
}
w.print();  //hello world
w.alert();  //hello world
// print()方法是w实例本身具有的方法，所以w.print()打印hello world；
// alert()不属于w实例的方法，属于构造函数的方法，w.alert()也会打印hello world，因为实例继承构造函数的方法。
```
`实例w` 的 **隐式原型** 指向它 `构造函数` 的 **显式原型** ，指向的意思是恒等于: `w.__proto__ === Word.prototype`
`w` 本身没有 `alert()` 方法，所以会去 `Word()` 的 `显式原型中调用alert()`，即 **实例继承构造函数的方法**。

# 原型和原型链
```javascript
Function.prototype.a = "a";
Object.prototype.b = "b";
function Person(){}
console.log(Person);    // function Person()
let p = new Person();
console.log(p);         // Person {} 对象
console.log(p.a);       // undefined
console.log(p.b);       // b
```
`p` 是 `Person()` 的实例，是一个 `Person` 对象，它拥有一个属性值 `__proto__` ，并且 `__proto__` 是一个对象，包含两个属性值 `constructor` 和 `__proto__`
```javascript
// p是Person()的实例，是一个Person对象，它拥有一个属性值__proto__，并且__proto__是一个对象，包含两个属性值constructor和__proto__
console.log(p.__proto__.constructor);   // function Person(){}
console.log(p.__proto__.__proto__);     // 对象{}，拥有很多属性值
// 我们会发现p.__proto__.constructor返回的结果为构造函数本身，p.__proto__.__proto__有很多参数
```
{% asset_img 1.jpg 图形解释原型链 %}
{% asset_img 5.png 原型链 %}
- **原型链** 指的就是图中的 `proto` 这一条指针链！原型链的顶层就是Object.prototype，而这个对象的是没有原型对象的。
## 函数都有prototype属性，即原型
这个 `prototype` 的属性值是一个对象（属性的集合），默认的只有一个叫做 `constructor` 的属性，指向这个函数本身。
{% asset_img 1.png 图形表示 %}
<font color="red">【优点】</font>所有的对象实例都可以共享它包含的属性和方法。这一点可以在构造函数里就可以看出来，因为构造函数在函数里面就定义了对象的实例信息，而原型对象可以在任何地方定义属性和方法。
在自己自定义的方法的prototype中新增自己的属性
```javascript
function Fn() { }
Fn.prototype.name = '王福朋';
Fn.prototype.getYear = function () {
    return 1988;
};
```
{% asset_img 2.png 实质在constructor里面，构造函数里 %}
这样子有何用呢？其实jQ的元素属性绑定原理也是这样
```javascript
var $div = $('div');
$div.attr('myName', '王福朋');
```
以上代码中，$('div')返回的是一个对象，对象——被函数创建的。假设创建这一对象的函数是 myjQuery。它其实是这样实现的。
```javascript
myjQuery.prototype.attr = function () {
    //……
};
$('div') = new myjQuery();
```
如果用咱们自己的代码来演示，就是这样
```javascript
function Fn() { }
Fn.prototype.name = '王福朋';
Fn.prototype.getYear = function () {
    return 1988;
};
var fn = new Fn();
console.log(fn.name);
console.log(fn.getYear());
// 即，Fn是一个函数，fn对象是从Fn函数new出来的，这样fn对象就可以调用Fn.prototype中的属性。
```

```javascript
function Person(){}
Person.prototype.name = 'bangbang';
Person.prototype.age = 18;
Person.prototype.job = 'programmer';
Person.prototype.dream = function(){
    console.log('Change yourself');
}

var person1 = new Person();
person1.dream();    //Change yourself

var person2 = new Person();
person2.dream();
// 判断两个实例继承的方法和属性是否全等
console.log(person1.dream === person2.dream);
console.log(person1.age === person2.age);
```
【优点】：由以上的代码可以看出它比构造函数方法的好处就是，person1和person2访问的都是同一个dream()函数，即他们的属性和方法都是共享的。
## 组合使用构造函数模式和原型模式优点最多： 
```javascript
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.love = ['qiqi','lili'];
}
Person.prototype = {
    constructor : Person,
    dream : function(){
        console.log(this.love[0]);
    }
}
var person1 = new Person('bangbang',18,'programmer');
var person2 = new Person('xiaoya',18,'teacher');
console.log(person1.love);
console.log(person2.love)
console.log(person1.love === person2.love);  //false
//给person1的love属性添加元素
person1.love.push('niuniu');
console.log(person1.love)  //["qiqi", "lili", "niuniu"]
console.log(person2.love)  //["qiqi", "lili"]
console.log(person1.love === person2.love);  //false
console.log(person1.dream === person2.dream);//true
```
构造函数模式用于定义实例属性，而原型模式用于定义方法和共享的属性。每个实例都会有自己的一份实例属性的副本，但同时又共享着对方发的引用，最大限度的节省了内存。

## 每个对象都有一个__proto__，可成为隐式原型
每个对象都有一个__proto__属性，指向创建该对象的函数的prototype。
【prototype与__proto__的差别介绍 prototype与__proto__.note】
## prototype 和 __proto__ 区别
{% asset_img 3.png prototype 和__proto__ 区别 %}
{% asset_img 4.png __proto__的指向 %}
## prototype 与 __proto__使用
```javascript
function Person() {}
 
// (1).Fun.prototype.*
Person.prototype.a = function() {
    console.log("a");
}
 
// (2).Fun.prototype.__proto__.*
Person.prototype.__proto__.b = function() {
    console.log("b");
}
 
// (3).Fun.prototype
Person.prototype = object;
 
// (4).Fun.prototype.__proto__
Person.prototype.__proto__ = object;
 
// (5).obj.__proto__
var person = new Person();
person.__proto__ = object;

```
【解析】
- __proto__是每个对象都有的一个属性，而prototype（除了一些内建函数）是函数才会有的属性
function Person() { } 是函数，var person 则是对象
- 解释例子
```javascript
var p = {
    c: function() {
        console.log('c');
    }
}
function Person() {}
Person.prototype.a = function() {
    console.log("a");
}
Person.prototype.__proto__ = p;
Person.prototype.__proto__.b = function() {
    console.log("b");
}
var person = new Person();
person.a(); // 自对象方法 a
person.b(); // 继承对象方法 b
person.c(); // 继承对象方法 c
```
定义了p对象带有c方法的，一个Person函数。重点，Person.prototype可以看做是一个自身构造，暂称为自对象；而Person.prototype.__proto__则是一个继承构造，暂称为继承对象。自对象和继承对象的属性和方法的并集组成了Person函数的属性和方法。简单公式就是：自对象（属性、方法）∪ 继承对象（属性、 方法）=new 函数（属性、方法）。因此，上例中new Person( )新建出来的var person 对象同时包含了自对象和继承对象的属性和方法，能成功执行a()、b()、c()。当自对象与继承对象间有相同属性或方法名称时，自对象优先级较高。

- Object.getPrototypeOf( )
```javascript
function POP() {}
POP.prototype.__proto__ = Object.getPrototypeOf(person);
var pop = new POP();
pop.a(); //a
pop.b(); //b
pop.c(); //c
若此时有一个函数POP想要继承Person函数所有的属性和方法呢？这时请使用Object.getPrototypeOf( )。
```

# instanceof
对于值类型，你可以通过 `typeof` 判断，`string / number / boolean` 都很清楚，但是 `typeof` 在判断到引用类型的时候，返回值只有 `object / function` ，你不知道它到底是一个object对象，还是数组，还是new Number等等。这个时候就需要用到instanceof。
`instanceof` 表示的就是一种继承关系，或者原型链的结构。
{% post_link js-data-type %}


