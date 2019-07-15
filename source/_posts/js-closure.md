---
title: JS的闭包
date: 2019-02-27 12:24:45
author: T.2stt
comments: true
tags:
- js
- 闭包
categories:
- [js,ms]
---
>有权限访问另一个函数作用域中的变量的函数。(定义在一个函数内部的函数)闭包就是 `连接函数内部和函数外部` 的桥梁
`闭包` 是由 **函数以及创建该函数的词法环境组合而成** 。`这个环境` 包含了这个 **闭包创建时所能访问的所有局部变量**。
常应用于 `模块化` 与 `柯里化`
## 用例子介绍闭包
### 闭包创建时所能访问的所有局部变量
```javascript
function makeFunc() {
    var name = "Mozilla";
    function displayName() {
        alert(name);
    }
    return displayName;
}

var myFunc = makeFunc();
myFunc();
// myFunc 是执行 makeFunc 时创建的 displayName 函数实例的引用，而 displayName 实例仍可访问其词法作用域中的变量name
```
### 共享相同的函数定义，但是保存了不同的词法环境
```javascript
function makeAdder(x) {
  return function(y) {
    return x + y;
  };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);

console.log(add5(2));  // 7
console.log(add10(2)); // 12
// add5 和 add10 都是闭包。它们共享相同的函数定义，但是保存了不同的词法环境。在 add5 的环境中，x 为 5。而在 add10 中，x 则为 10。
```
### 在循环中创建闭包：一个常见错误
```html
<p id="help">Helpful notes will appear here</p>
<p>E-mail: <input type="text" id="email" name="email"></p>
<p>Name: <input type="text" id="name" name="name"></p>
<p>Age: <input type="text" id="age" name="age"></p>
```
```javascript
// let 关键字 之前，在循环中有一个常见的闭包创建问题。

function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = function() {
      showHelp(item.help);
    }
  }
}

setupHelp();
```
**无论焦点在哪个input上，显示的都是关于年龄的信息。**
<font color="red">【原因:】</font> 是赋值给 onfocus 的是闭包。
- 这些闭包是由他们的函数定义和在 setupHelp 作用域中捕获的环境所组成的。
- 这三个闭包在循环中被创建，但他们共享了同一个词法作用域，在这个作用域中存在一个变量item。
- 当onfocus的回调执行时，item.help的值被决定。由于循环在事件触发之前早已执行完毕，变量对象item（被三个闭包所共享）已经指向了helpText的最后一项。

解决这个问题的一种方案是使用更多的闭包：特别是使用前面所述的 `函数工厂`
```javascript
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}
function makeHelpCallback(help) {
  return function() {
    showHelp(help);
  };
}
function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];
  for (var i = 0; i < helpText.length; i++) {
    var item = helpText[i];
    document.getElementById(item.id).onfocus = makeHelpCallback(item.help);
  }
}
setupHelp();
```
这段代码可以如我们所期望的那样工作。所有的回调不再共享同一个环境， makeHelpCallback 函数为每一个回调创建一个新的词法环境。在这些环境中，help 指向 helpText 数组中对应的字符串。
另一种方法使用了 `匿名闭包`：
```javscript
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}
function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];
  for (var i = 0; i < helpText.length; i++) {
    (function() {
       var item = helpText[i];
       document.getElementById(item.id).onfocus = function() {
         showHelp(item.help);
       }
    })(); // 马上把当前循环项的item与事件回调相关联起来
  }
}
setupHelp();
```
避免使用过多的闭包，可以用let关键词
```javascript
function showHelp(help) {
  document.getElementById('help').innerHTML = help;
}

function setupHelp() {
  var helpText = [
      {'id': 'email', 'help': 'Your e-mail address'},
      {'id': 'name', 'help': 'Your full name'},
      {'id': 'age', 'help': 'Your age (you must be over 16)'}
    ];

  for (var i = 0; i < helpText.length; i++) {
    let item = helpText[i];
    document.getElementById(item.id).onfocus = function() {
      showHelp(item.help);
    }
  }
}

setupHelp();
// 这个例子使用let而不是var，因此每个闭包都绑定了块作用域的变量，这意味着不再需要额外的闭包。
```
## 闭包有3个特性
1. 作为一个函数变量的一个引用，当函数返回时，其处于激活状态；
2. 一个闭包就是当一个函数返回时，没有释放资源的栈区, `内存消耗方面` 对脚本性能具有负面影响；
3. 闭包在` 处理速度` 具有负面影响;
但是你只需要知道应用的两种情况即可：函数作为返回值，函数作为参数传递。

## 闭包的5种写法和用法
### 只是给函数添加一些属性
```javascript
function Circle(r) {
    this.r = r;
}
Circle.PI = 3.14159;
Circle.prototype.area = function() {
    return Circle.PI * this.r * this.r;
}
var c = new Circle(1.0);
alert(c.area());
```
### 声明一个变量，将一个函数当作值赋给变量。
```javascript
var Circle = function() {
    var obj = new Object();
    obj.PI = 3.14159;
    obj.area = function( r ) {
        return this.PI * r * r;
    }
    return obj;
 } 
 var c = new Circle();
 alert( c.area( 1.0 ) );
```

### new 一个对象，然后给对象添加属性和方法。
```javascript
var Circle = new Object();
Circle.PI = 3.14159; 
Circle.Area = function( r ) {
       return this.PI * r * r;
}
alert( Circle.Area( 1.0 ) );
```

### 使用较多，也最为方便。var obj = {}就是声明一个空的对象
```javascript
var Circle={  
    "PI":3.14159,  
"area":function(r){  
           return this.PI * r * r;  
         }  
 };  
 alert( Circle.area(1.0) );
```

### 第5种
```javascript
var Circle = new Function("this.PI = 3.14159;this.area = function( r ) {return r*r*this.PI;}");  
alert( (new Circle()).area(1.0) );  
```

## javascript闭包的用途
它的最大用处有两个:
一个是前面提到的可以读取函数内部的变量，
就是让这些变量的值始终保持在内存中
### 匿名自执行函数
我们知道所有的变量，如果不加上var关键字，则默认的会添加到全局对象的属性上去，这样的临时变量加入全局对象有很多坏处，比如：别的函数可能误用这些变量；造成全局对象过于庞大，影响访问速度(因为变量的取值是需要从原型链上遍历的)。
即有的函数只需要执行一次，其内部变量无需维护，比如UI的初始化，那么我们可以使用闭包：
```javascript
var data= {    
    table : [],    
    tree : {}    
};    
     
(function(dm){    
    for(var i = 0; i < dm.table.rows; i++){    
       var row = dm.table.rows[i];    
       for(var j = 0; j < row.cells; i++){    
           drawCell(i, j);    
       }    
    }    
})(data);
// 我们创建了一个匿名的函数，并立即执行它，由于外部无法引用它内部的变量，因此在函数执行完后会立刻释放资源，关键是不污染全局对象。
```
### 结果缓存
开发中会碰到很多情况，设想有一个处理过程很耗时的函数对象，每次调用都会花费很长时间，
那么就需要将计算出来的值存储起来，当调用这个函数的时候，首先在缓存中查找，如果找不到，则进行计算，然后更新缓存并返回值，如果找到了，直接返回查找到的值即可。闭包正是可以做到这一点，因为它不会释放外部的引用，从而函数内部的值可以得以保留。
```javascript
var CachedSearchBox = (function(){    
    var cache = {},    
       count = [];    
    return {    
       attachSearchBox : function(dsid){    
           if(dsid in cache){//如果结果在缓存中    
              return cache[dsid];//直接返回缓存中的对象    
           }    
           var fsb = new uikit.webctrl.SearchBox(dsid);//新建    
           cache[dsid] = fsb;//更新缓存    
           if(count.length > 100){//保正缓存的大小<=100    
              delete cache[count.shift()];    
           }    
           return fsb;          
       },    
     
       clearSearchBox : function(dsid){    
           if(dsid in cache){    
              cache[dsid].clearSelection();      
           }    
       }    
    };    
})();    
     
CachedSearchBox.attachSearchBox("input");
// 这样我们在第二次调用的时候，就会从缓存中读取到该对象。
```
### 封装
```javascript
var person = function(){    
    //变量作用域为函数内部，外部无法访问    
    var name = "default";
    return {    
       getName : function(){    
           return name;    
       },    
       setName : function(newName){    
           name = newName;    
       }    
    }    
}();    
     
print(person.name); 
print(person.getName());    
person.setName("abruzzi");    
print(person.getName());    
   
得到结果如下：  
   
undefined  //直接访问，结果为undefined   
default  
abruzzi
```
### 实现类和继承
```javascript
function Person(){   
    var name = "default";       
    return {    
       getName : function(){    
           return name;    
       },    
       setName : function(newName){    
           name = newName;    
       }    
    }    
};   

    var p = new Person();
    p.setName("Tom");
    alert(p.getName());

    var Jack = function(){};
    //继承自Person
    Jack.prototype = new Person();
    //添加私有方法
    Jack.prototype.Say = function(){
        alert("Hello,my name is Jack");
    };
    var j = new Jack();
    j.setName("Jack");
    j.Say();
    alert(j.getName());
    // 我们定义了Person，它就像一个类，我们new一个Person对象，访问它的方法。下面我们定义了Jack，继承Person，并添加自己的方法。
```
## 常见的陷阱
```javascript
function createFunctions(){
    var result = new Array();
    for (var i=0; i < 10; i++){
      result[i] = function(){
        return i;
      };
    }
    return result;
  }
  var funcs = createFunctions();
  for (var i=0; i < funcs.length; i++){
    console.log(funcs[i]());
  }
// 以为输出 0~9 ，万万没想到输出10个10？
函数带()才是执行函数！ 单纯的一句 var f = function() { alert('Hi'); }; 是不会弹窗的，后面接一句 f(); 才会执行函数内部的代码。上面代码翻译一下就是：
var result = new Array(), i;
result[0] = function(){ return i; }; //没执行函数，函数内部不变，不能将函数内的i替换！
result[1] = function(){ return i; }; //没执行函数，函数内部不变，不能将函数内的i替换！
...
result[9] = function(){ return i; }; //没执行函数，函数内部不变，不能将函数内的i替换！
i = 10;
funcs = result;
result = null;
 
console.log(i); // funcs[0]()就是执行 return i 语句，就是返回10
console.log(i); // funcs[1]()就是执行 return i 语句，就是返回10
...
console.log(i); // funcs[9]()就是执行 return i 语句，就是返回10
// 闭包就是一个函数引用另外一个函数的变量，因为变量被引用着所以不会被回收，因此可以用来封装一个私有变量。这是优点也是缺点，不必要的闭包只会徒增内存消耗！
```
## 使用闭包的注意点
1）由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。
2）闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

## 介绍闭包的用法
【函数取值注意this以调用处，变量要去创建这个函数的作用域取值，而不是“父作用域”】
### 函数作为返回值
{% asset_img close1.png bar函数作为返回值，赋值给f1变量。执行f1(15)时，用到了fn作用域下的max变量的值。 %}

### 函数作为参数被传递
{% asset_img close2.png 如上代码中，fn函数作为一个参数被传递进入另一个函数，赋值给f参数。执行f(15)时，max变量的取值是10，而不是100。要去创建这个函数的作用域取值，而不是“父作用域”。讲到闭包，除了结合着作用域之外，还需要结合着执行上下文栈来说一下。 %}
一般当一个函数被调用完成之后，其执行上下文环境将被销毁，其中的变量也会被同时销毁。
但是有些情况下，函数调用完成之后，其执行上下文环境不会接着被销毁。这就是需要理解闭包的核心内容。
咱们可以拿本文的第一段代码（稍作修改）来分析一下。
{% asset_img close3.png 修改后 %}
{% asset_img close5.png 第一步，代码执行前生成全局上下文环境，并在执行时对其中的变量进行赋值。此时全局上下文环境是活动状态。 %}
{% asset_img close6.png 第二步，执行第17行代码时，调用fn()，产生fn()执行上下文环境，压栈，并设置为活动状态。 %}
{% asset_img close7.png 第三步，执行完第17行，fn()调用完成。 %}
按理说应该销毁掉fn()的执行上下文环境，但是这里不能这么做。注意，重点来了：因为执行fn()时，返回的是一个函数。函数的特别之处在于可以创建一个独立的作用域。而正巧合的是，返回的这个函数体中，还有一个自由变量max要引用fn作用域下的fn()上下文环境中的max。因此，这个max不能被销毁，销毁了之后bar函数中的max就找不到值了。
因此，这里的fn()上下文环境不能被销毁，还依然存在与执行上下文栈中。
{% asset_img close8.png 即，执行到第18行时，全局上下文环境将变为活动状态，但是fn()上下文环境依然会在执行上下文栈中。另外，执行完第18行，全局上下文环境中的max被赋值为100。 %}
{% asset_img close9.png 第四步，执行到第20行，执行f1(15)，即执行bar(15)，创建bar(15)上下文环境，并将其设置为活动状态。 %}
执行bar(15)时，max是自由变量，需要向创建bar函数的作用域中查找，找到了max的值为10。这个过程在作用域链一节已经讲过。
这里的重点就在于，创建bar函数是在执行fn()时创建的。fn()早就执行结束了，但是fn()执行上下文环境还存在与栈中，因此bar(15)时，max可以查找到。如果fn()上下文环境销毁了，那么max就找不到了。
使用闭包会增加内容开销，现在很明显了吧！
{% asset_img close10.png 第五步，执行完20行就是上下文环境的销毁过程，这里就不再赘述了。 %}
【参数的作用域问题】
要到创建这个函数的那个作用域中取值——是“创建”，而不是“调用”，切记切记