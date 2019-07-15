---
title: JS 作用域（链） / 上下文
date: 2019-03-06 08:10:52
author: T.2stt
comments: true
tags:
- js
- 作用域
categories:
- [js,ms]
---
> 作用域: 包括 **静态作用域（词法作用域）** 和 **动态作用域** 
作用域链： [[scope]] 属性 **指变量的可访问性**。函数被定义时就被创建了，**包含函数定义时作用域中所有可访问的数据**。
上下文：this的取值指向。是用来 **指定代码某些特定部分中this的值** 。
执行期上下文： **指的是作用域**。对同一函数调用多次，会导致创建多个执行期上下文。一旦函数执行完成，执行期上下文将被销毁；


# 作用域
包括: **静态作用域(此法作用域)** 和 **动态作用域**，javascript采用的是静态作用域。
JS 中作用域是指可访问变量，对象，函数的集合，也就是调用它们能生效的代码区块。在 JS 中没有块级作用域，只有全局作用域和函数作用域，可以模仿块级作用域。
- 词法作用: **变量的作用域是在定义时决定而不是执行时决定**，也就是说词法作用域取决于源码，通过静态分析就能确定，因此词法作用域也叫做静态作用域。
- 动态作用域： **是在运行时根据程序的流程信息来动态确定的**，而不是在写代码时进行静态确定的。 动态作用域并不关心函数和作用域是如何声明以及在何处声明的，只关心它们在何处调用。
## 全局作用域
在代码中任何地方都能访问到的对象拥有全局作用域。全局作用域的变量是全局对象的属性，不论在什么函数中都可以直接访问，而不需要通过全局对象，但加上全局对象，指的是作用域可以提供搜索效率。
```javascript
 var a = 10
 function f1(){
    var b = c = 20;
    console.log(a);     // 10
    console.log(c);     // 20
    function f2() {
        console.log(b); // 20
    }
    f2();
}
f1();
console.log(a);     // 10
console.log(c);     // 20
console.log(b);     // error
// var b = c = 20  是指 var b = c; c = 20
// 在f1函数中c没使用var声明为全局变量，b为局部变量，绑定在f1函数下，外部访问不到。
```
- **a** 、没有用var声明的变量（除去函数的参数）都具有全局作用域，成为全局变量，所以声明局部变量必须要用var。
- **b** 、window的所有属性都具有全局作用域
- **c** 、最外层函数体外声明的变量也具有全局作用域
## 局部作用域
**局部变量的优先级高于全局变量。**
- **a** 、函数体内用var声明的变量具有局部作用域，成为局部变量
- **b** 、函数的参数也具有局部作用域
```javascript
var a=3; // 全局变量  
function fn(b){ // 局部变量  
 c=2; // 全局变量  
 var d=5; // 局部变量  
 function subFn(){  
    var e=d; // 父函数的局部变量对子函数可见  
    for(var i=0;i<3;i++){  
      console.write(i);  
    }  
    alert(i);// 3, 在for循环内声明，循环外function内仍然可见，没有块作用域  
 }  
}  
alert(c); // 在function内声明但不带var修饰，仍然是全局变量
``` 
## 模仿块级作用域
没有块级作用域之前，但是有if()，for()等块语句，在块语句内部定义的变量会保留在它们已经存在的作用域内
```javascript
if(true) {
    var word = 'hello';
    console.log(word);  // hello
}
console.log(word);      // hello
//   if()语句存在全局作用域下，所以内部定义的变量存在于全局作用域中，无论在哪都可以访问。
```
```javascript
function add(num) {
    if(num > 10) {
        var num = 10;
        console.log(num);   // 10
    }
    console.log(num);       // 10
}
add(11);
console.log(num);   // Uncaught ReferenceError: num is not defined
// 此时if()在add函数中，内部定义的变量存在于add函数的作用域中，只有在函数和块语句中才可以访问到，外部无法访问。
```
**使用自执行的匿名函数包裹块语句构建块作用域，也叫私有作用域**
```javascript
function add(num) {
    for(var i = 0; i < num; i++) {
    console.log(i);		//0,1,2,3,4,5,6,7,8,9
    }
    console.log(i);		//10
}
add(10);
// 改为
 function add(num) {
    (function () {
        for(var i = 0; i < num; i++) {
            console.log(i);	//0,1,2,3,4,5,6,7,8,9
        }
    })()
    console.log(i);		// Uncaught ReferenceError: i is not defined
}
add(10)
// 此时变量i只能在for()循环中访问到，在add函数和外部都无法访问，并且在匿名函数中定义的任何变量都会在执行结束时被销毁，所以变量i只能在for()循环中使用。
```
## 块级作用域
使用 `let` 和 `const` 关键字声明的变量，会在形成块级作用域。
```javascript
if (true) {
   // 'if' 条件语句块不会创建一个新的作用域
   // name 在全局作用域中，因为通过 'var' 关键字定义
   var name = 'Hammad';
   // likes 在局部（本地）作用域中，因为通过 'let' 关键字定义
   let likes = 'Coding';
   // skills 在局部（本地）作用域中，因为通过 'const' 关键字定义
   const skills = 'JavaScript and PHP';
}
console.log(name); // logs 'Hammad'
console.log(likes); // Uncaught ReferenceError: likes is not defined
console.log(skills); // Uncaught ReferenceError: skills is not defined
```
------
# 上下文(context)
`上下文(context)` 是用来 **指定代码某些特定部分中this的值** 。 `作用域(scope)` 是指 **变量的可访问性**。上下文(context)是指this在同一作用域内的值。我们也可以使用call()、apply()、bind()、箭头函数等改变上下文。在浏览器中在全局作用域(scope)中上下文中始终是Window对象。在Node.js中在全局作用域(scope)中上下文中始终是Global 对象。
```javascript
var name = "windowsName";
function a() {
    var name = "Cherry";
    console.log(this.name);  // windowsName
    console.log("inner:" + this);// inner: Window
}
a();
console.log("outer:" + this) // outer: Window
```
上下文始终坚持一个原理：**this 永远指向最后调用它的那个对象**，上例中调用a函数的是window，所以a函数中的this指向window对象。关于this以及改变this的指向，可以参考this、apply、call、bind

----

# 作用域链(Scope Chain)
Js 中每个函数都都表示为一个函数对象（函数实例），函数对象有一个仅供 Js 引擎使用的[[scope]] 属性。通过语法分析和预解析，将[[scope]] 属性指向函数定义时作用域中的所有对象集合。这个集合被称为函数的  `作用域链`（scope chain），**包含函数定义时作用域中所有可访问的数据。**
```javascript
function add(num1, num2) {
  var sum = num1 + num2;
  return sum;
}
```
当定义 add 函数后，其作用域链就创建了。函数所在的全局作用域的全局对象被放置到 add 函数作用域链（[[scope]] 属性）中。我们可以从下图中看到作用域链的第一个对象保存的是全局对象，全局对象中保存了诸如 this , window , document 以及全局对象中的 add 函数，也就是他自己。这也就是我们可以在全局作用域下的函数中访问 window(this)，访问全局变量，访问函数自身的原因。全局上下文中的变量对象(Variable object，VO)就是全局对象。
{% asset_img 1.png 图解作用域链 %}

-----
# 执行期上下文(Execution Context)
执行具体的某个函数时，JS 引擎在执行每个函数实例时，都会创建一个执行期上下文（Execution Context）和激活对象（active Object）（它们属于宿主对象，与函数实例执行的生命周期保持一致，也就是函数执行完成，这些对象也就被销毁了，闭包例外。）
```javascript
// 假设我们运行以下代码：
var total = add(5, 10);
```
执行该函数创建一个内部对象，称为 Execution Context（执行期上下文）。执行期上下文定义了一个函数正在执行时的作用域环境。特别注意，执行期上下文和我们平常说的上下文不同，**执行期上下文指的是作用域**。平常说的上下文是this的取值指向。执行期上下文和函数创建时的作用域链对象 [[scope]] 区分，这是两个不同的作用域链对象。分开的原因很简单，**函数定义时的作用域链对象 [[scope]] 是固定的，而 执行期上下文 会根据不同的运行时环境变化。** 而且该函数每执行一次，都会创建单独的 执行期上下文，**因此对同一函数调用多次，会导致创建多个执行期上下文。一旦函数执行完成，执行期上下文将被销毁。**
  执行期上下文对象有自己的作用域链，当创建执期行上下文时，其作用域链将使用执行函数[[scope]]属性所包含的对象（即，函数定义时的作用域链对象）进行初始化。这些值按照它们在函数中出现的顺序复制到执行期上下文作用域链中。
  无论有多少个函数上下文，但是全局上下文只有一个。执行期上下文有创建和代码执行的两个阶段。
## 第一阶段：创建阶段
当一个函数被调用但是其代码还没有被执行的时。在创建阶段主要做的三件事情是：
```javascript
创建变量（激活）对象（VO == AO） 
创建作用域链
设置上下文(context)的值（ this ）
```
**激活对象(Activation Object，AO)**
当一个函数被调用但是其代码还没有被执行的时，在执行其上下文中创建一个名为 Activation Object（激活对象）的新对象。这个激活对象保存了函数中的所有形参，实参，局部变量，this 指针等函数执行时函数内部的数据情况。然后将这个激活对象推送到执行其上下文作用域链的顶部。
- **1)**. 函数参数(若未传入，初始化该参数值为undefined) 
- **2)**. 函数声明(若发生命名冲突，会覆盖) 
- **3)**. 变量声明(初始化变量值为undefined，若发生命名冲突，会忽略。)
add函数被调用，但是还未执行时的VO(变量对象)==AO(激活对象)是：
```javascript
AO(add) = { 
  arguments: {
    0: 5,
    1: 10
    length: 2
  }, 
  num1: 5,  
  num2: 10,  
  sum: undefined  
}; 
```
激活对象是一个可变对象，里面的数据随着函数执行时的数据的变化而变化，当函数执行结束之后，执行期上下文将被销毁。也就会销毁Execution Context的作用域链，激活对象也同样被销毁。但如果存在闭包，激活对象就会以另外一种方式存在，这也是闭包产生的真正原因，具体的我们稍后讨论。下图显示了执行上下文及其作用域链：
{% asset_img 2.png 执行图 %}
从左往右看，第一部分是函数执行时创建的执行期上下文，它有自己的作用域链，第二部分是作用域链中的对象，索引为1的对象是从[[scope]]作用域链中复制过来的，索引为0的对象是在函数执行时创建的激活对象，第三部分是作用域链中的对象的内容Activation Object(激活对象)和Global Object(全局对象)。

  函数在执行时，每遇到一个变量，都会去执行期上下文的作用域链的顶部，执行函数的激活对象开始向下搜索，如果在第一个作用域链（即，Activation Object 激活对象）中找到了，那么就返回这个变量。如果没有找到，那么继续向下查找，直到找到为止。如果在整个执行期上下文中都没有找到这个变量，在这种情况下，该变量被认为是未定义的。这也就是为什么函数可以访问全局变量，当局部变量和全局变量同名时，会使用局部变量而不使用全局变量，以及 JavaScript 中各种看似怪异的、有趣的作用域问题的答案。

## 第二阶段：代码执行
在代码执行阶段，会顺序执行代码，根据代码，修改变量对象的值，并最终执行代码。当代码执行完后，这时候的 AO 是：
```javascript
AO(add) = { 
  arguments: {
    0: 5,
    1: 10
    length: 2
  }, 
  num1: 5,  
  num2: 10,  
  sum: 15
}; 
```
# 闭包（Closure）
闭包 是 JS 最强大的特性之一，它允许函数访问局部作用域之外的数据。闭包在日常编码工作中很常见。但是，它会对性能造成影响。了解闭包我们使用以下示例代码：
```javascript
function assignEvents(){
    var id = "xdi9592";
    document.getElementById("save-btn").onclick = function(event) {
        saveDocument(id);
    };
}
```
assignEvents 函数为DOM元素分配一个事件处理程序。这个处理函数就是一个闭包。为了使该闭包访问 `id` 变量，必须创建一个特定的作用域链。
**闭包的形成过程：**
  assignEvents `函数创建并且词法解析后`，函数对象 assignEvents 的 [[scope]] 属性被初始化，作用域链形成，作用域链中包含了全局对象的所有属性和方法（注意，此时因为 assignEvents 函数还未被执行，所以闭包函数并没有被解析）。
  assignEvents `开始执行时`，创建 Execution Context（执行期上下文），在执行期上下文的作用域链中创建 Activation Object(激活对象)，并将 Activation Object(激活对象) 推送到作用域链顶部，在其中保存了函数执行时所有可访问函数内部的数据。激活对象包含 id 变量。
  当执行到闭包时，JS 引擎发现了闭包函数的存在，按照通常的手法，将闭包函数解析，为闭包函数对象创建 [[scope]] 属性，初始化作用域链。特别注意的是，这个时候，闭包函数对象的作用域链中有两个对象，一个是 assignEvents 函数执行时的 Activation Object(激活对象) ，还有一个是全局对象，如下图：
{% asset_img 3.png 执行图 %}
我们看到图中闭包函数对象的作用域链和 assignEvents 函数的执行期上下文的作用域链是相同的。为什么相同呢？我们来分析一下，闭包函数是在 assignEvents 函数执行的过程中被定义并且解析的，而函数执行时的作用域是 Activation Object(激活对象) ，闭包函数被解析的时候它的作用域正是 assignEvents 作用域链中的第一个作用域对象 Activation Object(激活对象) ，当然，由于作用域链的关系，全局对象作用域也被引入到闭包函数的作用域链中。
  在词法分析的时候闭包函数的 [[scope]] 属性 就已经在作用域链中保存了对 assignEvents 函数的 Activation Object(激活对象) 的引用，所以当 assignEvents 函数执行完毕之后，闭包函数虽然还没有开始执行，但依然可以访问 assignEvents 的局部数据，并不是因为闭包函数要访问 assignEvents 的局部变量id，所以当 assignEvents 函数执行完毕之后依然保持了对局部变量id的引用。而是不管是否存在变量引用，都会保存对 assignEvents 的 Activation Object(激活对象)作用域对象的引用。因为在词法分析时，闭包函数没有执行，函数内部根本就不知道是否要对 assignEvents 的局部变量进行访问和操作，所以只能先把 assignEvents 的 Activation Object(激活对象) 作用域对象保存起来，当闭包函数执行时，如果需要访问 assignEvents 的局部变量，那么再去作用域链中查找。
  也正是因为这种引用，造成了一个副作用。通常，当执行期上下文被销毁时，函数的激活对象也就被销毁了。当有闭包引用时，激活对象就不会被销毁，因为他仍然被引用。这意味着闭包比非隔离的函数需要更多的内存。
  闭包函数执行时创建了自己的 Execution Context（执行期上下文），其作用域链使用了 [[scope]] 属性，其引用了 assignEvents 函数的 Activation Object(激活对象) 和 全局对象。然后为闭包本身创建一个新的 Activation Object(激活对象)。 所以在闭包函数的执行期上下文的作用域链中保存了自己的 Activation Object(激活对象)，外层函数 assignEvents Execution Context（执行期上下文）的 Activation Object(激活对象)，以及 Global Object(全局对象)，如图：
{% asset_img 4.png 执行图 %}