---
title:  vue2.0 常用指令
date: 2018-02-28 08:14:50
author: T.2stt
comments: true
tags:
- vue
categories:
- vue
---
##### [官网](https://vuefe.cn/v2/api/#v-bind)
##### v-text、v-html
v-text、v-html这两者分为一组很好理解，一个用于绑定文本，一个用于绑定html。上文使用到的 {{ Name }}这种写法就是v-text的的缩写形式，数据的单向绑定
```
<body>
    <div id="app">
        <h1>姓名：<label v-text="Name"></label></h1>
        <h1>姓名：{{ Name }}</h1>
        <div style="font-size:30px;font-weight:bold;" v-html="Age">年龄：</div>
    </div>

    <script src="Content/vue/dist/vue.js"></script>
    <script type="text/javascript">
    //Model
    var data = {
        Name: '小明',
        Age: "<label>20</label>",
        School:'光明小学',
    }

    //ViewModel
    var vue = new Vue({
        el: '#app',
        data: data,
    });
    </script>
</body>

注意v-text和v-html绑定都是单向的，只能从Model到View的绑定，不能实现View到Model的更新。
```
##### v-model
vue限定只能对表单控件进行绑定,数据双向绑定
```
常见的有<input>、<select>、<textarea>等。

-------------------------------------------修饰符
.lazy - 取代 input 监听 change 事件
	<!-- 在触发 "change" 事件后同步，而不是在触发 "input" 事件后更新 -->
	<input v-model.lazy="msg" >
.number - 输入字符串转为数字
	<input v-model.number="age" type="number">
	这通常很有用，因为即使是在 type="number" 时，HTML 中 input 元素也总是返回一个字符串类型的值。
.trim - 输入首尾空格过滤
	<input v-model.trim="msg">
	
-------------------------------------------例子
<body>
    <div id="app">
        <h2>编辑姓名：<input type="text" v-model="Name" /></h2>
        <h2>姓名：{{Name}}</h2>
        <hr />
        <h2>编辑备注：<textarea v-model="Remark"></textarea></h2>
        <h2>备注：{{Remark}}</h2>
        <hr />
        <input type="checkbox" id="basketball" value="篮球" v-model="Hobby">
        <label for="basketball">篮球</label>
        <input type="checkbox" id="football" value="足球" v-model="Hobby">
        <label for="football">足球</label>
        <input type="checkbox" id="running" value="跑步" v-model="Hobby">
        <label for="running">跑步</label>
        <br>
        <h2>学生爱好： {{ Hobby }}</h2>
        <hr />
        <h2>户籍：{{ Huji }}</h2>
        <select style="width:100px;" class="form-control" v-model="Huji">
            <option>湖南</option>
            <option>广东</option>
            <option>北京</option>
        </select>
</div>

    <script src="Content/vue/dist/vue.js"></script>
    <script type="text/javascript">
    //Model
    var data = {
        Name: '小明',
        Age: 18,
        School: '光明小学',
        Hobby: [],
        Remark: '三好学生',
        Huji:""
    }

    //ViewModel
    var vue = new Vue({
        el: '#app',
        data: data,
    });
    </script>
</body>
```
##### v-if、v-else、v-else-if
v-if可以单独使用，
v-else的前面必须要有一个v-if的条件或者v-show指令，
 v-else-if前一兄弟元素必须有 v-if 或 v-else-if
```
<body>
    <div id="app">
        <h1>姓名：<label v-text="Name"></label></h1>
        <h1>是否已婚：<span v-if="IsMarry">是</span></h1>
        <h1>大人or小孩：<span v-if="Age>18">大人</span><span v-else>小屁孩</span></h1>
        <h1>学校：{{ School }}</h1>
    </div>
	<div v-if="type === 'A'">
	  A
	</div>
	<div v-else-if="type === 'B'">
	  B
	</div>
	<div v-else-if="type === 'C'">
	  C
	</div>
	<div v-else>
	  Not A/B/C
	</div>
    <script src="Content/vue/dist/vue.js"></script>
    <script type="text/javascript">
    //Model
    var data = {
        Name: '小明',
        IsMarry: true,
        Age: 20,
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
##### v-show
根据表达式之真假值，切换元素的 display CSS 属性。当条件变化时该指令触发过渡效果。

##### v-bind
对于html标签的text、value等属性，Vue里面提供了v-text、v-model去绑定。但是对于除此之外的其他属性呢，这就要用到接下来要讲的v-bind指令了。
v-bind的作用是绑定除了text、value之外的其他html标签属性，常见的比如class、style、自定义标签的自定义属性等。
（动态地绑定一个或多个特性，或一个组件 prop 到表达式）
它的语法如下：
```
 <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
    <link href="Content/bootstrap/css/bootstrap.css" rel="stylesheet" />
    <style type="text/css">
        .class1 {
            padding:20px;
        }
        .backred {
            background-color:red;
        }
    </style>
</head>
<body>
    <div id="app">
        <h1>姓名：<label v-text="Name"></label></h1>
        <h1>是否红领巾：<span class="class1" v-bind:class="{backred:IsBack}"><label v-if="IsBack">是</label></span></h1>
        <h1>学校星级：<span v-bind:style="{color:SchoolLevel}">aa</span></h1>
    </div>
    
    <!-- 绑定一个属性 -->
	<img v-bind:src="imageSrc">
	<!-- 缩写 -->
	<img :src="imageSrc">
	<!-- 内联字符串拼接 -->
	<img :src="'/path/to/images/' + fileName">
	
	<!-- class 绑定 -->
	<div :class="{ red: isRed }"></div>
	<div :class="[classA, classB]"></div>
	<div :class="[classA, { classB: isB, classC: isC }]">
	
	<!-- style 绑定 -->
	<div :style="{ fontSize: size + 'px' }"></div>
	<div :style="[styleObjectA, styleObjectB]"></div>
	
	<!-- 绑定一个有属性的对象 -->
	<div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>
	
	<!-- 通过 prop 修饰符绑定 DOM 属性 -->
	<div v-bind:text-content.prop="text"></div>
	
	<!-- prop 绑定. “prop” 必须在 my-component 中声明。 -->
	<my-component :prop="someThing"></my-component>
	
	<!-- 通过 $props 将父组件的 props 一起传给子组件 -->
	<child-component v-bind="$props"></child-component>
	
	<!-- XLink -->
	<svg><a :xlink:special="foo"></a></svg>

    <script src="Content/vue/dist/vue.js"></script>
    <script type="text/javascript">
    //Model
    var data = {
        Name: '小明',
        Age: 18,
        School: '光明小学',
        SchoolLevel: 'red',
        IsBack:true
    }

    //ViewModel
    var vue = new Vue({
        el: '#app',
        data: data,
    });
    </script>
</body>
```
##### v-on
在Vue里面，v-on指令用来绑定标签的事件，其语法和v-bind基本类似。
缩写： @
```
<div id="app">
        <h1>姓名：<label v-text="Name"></label></h1>
        <h1>年龄：{{ Age }}</h1>
        <button class="btn btn-primary" v-on:click="Age++;if(Name=='小明')Name='吉姆格林';else Name='小明';">
        	年龄递增
        </button>
    </div>
	<!-- 方法处理器 -->
	<button v-on:click="doThis"></button>
	
	<!-- 对象语法 (2.4.0+) -->
	<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
	
	<!-- 内联语句 -->
	<button v-on:click="doThat('hello', $event)"></button>
	
	<!-- 缩写 -->
	<button @click="doThis"></button>
	
	<!-- 停止冒泡 -->
	<button @click.stop="doThis"></button>
	
	<!-- 阻止默认行为 -->
	<button @click.prevent="doThis"></button>
	
	<!-- 阻止默认行为，没有表达式 -->
	<form @submit.prevent></form>
	
	<!--  串联修饰符 -->
	<button @click.stop.prevent="doThis"></button>
	
	<!-- 键修饰符，键别名 -->
	<input @keyup.enter="onEnter">
	
	<!-- 键修饰符，键代码 -->
	<input @keyup.13="onEnter">
	
	<!-- 点击回调只会触发一次 -->
	<button v-on:click.once="doThis"></button>
	
    <script src="Content/vue/dist/vue.js"></script>
    <script type="text/javascript">
    //Model
    var data = {
        Name: '小明',
        Age: 18,
    }
    //ViewModel
    var vue = new Vue({
        el: '#app',
        data: data,
    });
    </script>
```

##### v-for
```bash
	<div v-for="item in items">
	  {{ item.text }}
	</div>
	另外也可以为数组索引指定别名（或者用于对象的键）：
	<div v-for="(item, index) in items"></div>
	<div v-for="(val, key) in object"></div>
	<div v-for="(val, key, index) in object"></div>
	迫使其重新排序的元素,你需要提供一个 key 的特殊属性:类似于ng1中的track-by
	<div v-for="item in items" :key="item.id">
	  {{ item.text }}
	</div>
```

##### v-pre
``` bash
	<span v-pre>{{ this will not be compiled }}</span
```

##### v-cloak
```bash
css
	[v-cloak] {
	  display: none;
	}
html
	<div v-cloak>
	  {{ message }}
	</div>
```

##### v-once
只渲染元素和组件一次