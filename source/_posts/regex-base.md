---
title: 正则的基本准则
date: 2019-07-11 17:05:56
author: T.2stt
comments: true
tags:
- regex
- javascript
categories:
- [正则,regex]
---

> 正则表达式 `(regular expression)` 描述了一种字符串匹配的模式 `（pattern）`，可以用来检查一个串是否含有某种子串、将匹配的子串替换或者从某个串中取出符合某个条件的子串等。


# 实例化RegExp对象
## 字面量
```
<!-- 实例化 -->
var reg = /\bis\b/g; // \b 是单词边界 \bis\b表示完整的is字符
<!-- 文本替换 用replace(reg, 'IS') 匹配第一个的内容（字符串或者是正则）替换成第二个的-->
'He is a body. This is a dog. Where is she?'.replace(reg, 'IS');
结果：'He IS a body. This IS a dog. Where IS she?' 
如果没有g: 'He IS a body. This is a dog. Where is she?'
```
## 构造函数 new
```
var reg  = new RegExp('\\bis\\b', 'g'); // \\b前一个\是转义的意思；
'He is a body. This is a dog. Where is she?'.replace(reg, 'IS');
```

# 修饰符（正则表达式的属性）
<font color="red">g:</font> global全文搜索，不添加搜索到第一个匹配停止；
<font color="red">i:</font> ignore case 忽略大小写，默认大小写敏感；
```
'He is a body. Is he?'.replace('/\bis\b\/g', '0');
结果： 'He 0 a body. Is he?'
'He is a body. Is he?'.replace('/\bis\b\/gi', '0');
结果： 'He 0 a body. 0 he?'
```
<font color="red">m:</font> mulitiple lines 多行搜索；
```
var mulStr = "@123\n@456\n@789";
mulSrt.replace(/^@\d/g, 'X');
结果：
"X23
@456
@789"
mulSrt.replace(/^@\d/gm, 'X');
结果：
"X23
X56
X89"
```
<font color="red">:</font> lastIndex 是当前表达式匹配内容的最后一个字符的下一个位置（下次搜索的开始位置）；
<font color="red">:</font> source 正则表达式的文本字符串；
```
var reg1 = /\w/;
reg1.global // false
reg1.source // \w
var reg2 = /\w/gim;
reg2.global // true
reg2.source // \w
```
# 字符组成
## 原义文本字符
代表本来含义的字符: a,b,c 就是代表a,b,c;1,2,3代表着1,2,3等

## 元字符
代表特殊含义的字符：`\b`: 匹配字符的边界
### [] - 构建一个简单的类
`[表达式]`：一般情况下正则表达式一个字符对应字符串的一个字符： `ab\t`含义是一个字母a加一个字母b和一个tab表头，但有时我们希望匹配某类字符（泛指符合某些特性的对象）
```
[abc] // 把字符a或b或c归为一类，有其中一个就可以
'a1b2c3d4'.replace(/[abc]/g, 'X'); // X1X2X3d4
```
#### 字符类取反(不属于某类)
使用元字符 `^` 创建`反向类/负向类`
```
[^abc] // 不是字符a或b或c的内容
'a1b2c3d4'.replace(/[^abc]/g, 'X');  // aXbXcXXX
```
### 范围类 
如果使用字符类匹配数字得 `[0123456789]` ,但使用范围类就只用 `[0-9]` , `[a-z]` 连接两个字符表达式：从a到z的任意字符
#### 例子
```
'a1b2d3x4z9'.replace(/[a-z]/g, 'Q');  // Q1Q2Q3Q4Q9
```
#### 大小写字母一样匹配
类是可连写的 `[a-zA-Z]`
```
'a1b2d3x4z9AJDSD'.replace(/[a-zA-Z]/g, 'Q');  // Q1Q2Q3Q4Q9QQQQQ
```
#### 匹配数字和'-'
`-` 在一个开头一个结尾里面，表示一个范围的意思，但不是的话直接在后面加就可以了
```
'2016-09-12'.replace(/[0-9]/g, 'A');  // AAAA-AA-AA
'2016-09-12'.replace(/[0-9-]/g, 'A'); // AAAAAAAAAA
```

### 预定义类
大写表示取反

|   元字符  | 
| --------   |
| <font color="red"> · 等价于[^\r\n]:</font>  任意字符--除了回车符和换行符的所有字符|
| <font color="red"> \d 等价于[0-9]</font> 数字字符|
| <font color="red"> \D 等价于[^0-9]</font> 非数字字符|
| <font color="red"> \s 等价于 [ \f\n\r\t\v]</font> 匹配任何空白字符，包括空格、制表符、换页符等等。注意 Unicode 正则表达式会匹配全角空格符。|
| <font color="red"> \S 等价于 [^ \f\n\r\t\v]</font> 匹配任何非空白字符。|
| <font color="red"> \w 等价于[a-zA-Z_0-9]</font> 单词字符（字母、数字下划线）|
| <font color="red"> \W 等价于[^a-zA-Z_0-9]</font> 非单词字符|

### 普通字符
普通字符包括没有显式指定为元字符的所有可打印和不可打印字符。这包括所有大写和小写字母、所有数字、所有标点符号和一些其他符号。

#### 非打印字符
|   元字符  | 
| --------   |
| <font color="red">\cx</font> 匹配由x指明的控制字符。 `\cM` 匹配一个 `Control-M` 或`回车符`。x 的值必须为 `A-Z` 或 `a-z` 之一。否则，将 `c` 视为一个原义的 `'c'` 字符。|
| <font color="red">\f</font> 匹配一个换页符。 等价于 `\x0c` 和 `\cL`。|
| <font color="red">\n</font> 匹配一个换行符。 等价于 `\x0a` 和 `\cJ`。|
| <font color="red">\r</font> 匹配一个回车符。 等价于 `\x0d` 和 `\cM`。|
| <font color="red">\t</font> 匹配一个制表符。 等价于 `\x09` 和 `\cI`。|
| <font color="red">\v</font> 匹配一个垂直制表符。 等价于 `\x0b` 和 `\cK`。|


### 特殊字符
一些有特殊含义的字符，如上面说的 `runoo*b` 中的 `*`，简单的说就是表示任何字符串的意思。如果要查找字符串中的 `*` 符号，则需要对 `*` 进行转义，即在其前加一个 `\: runo\*ob` 匹配 `runo*ob`。

|   元字符  |  描述  |
| --------   | :-----: |
| <font color="red">( )</font>标记一个子表达式的开始和结束位置。（分组）| 子表达式可以获取供以后使用。要匹配这些字符，请使用 `\(` 和 `\)`。 |
| <font color="red">\</font>将下一个字符标记为或特殊字符、或原义字符、或向后引用、或八进制转义符。| 例如， `'n'` 匹配字符 `'n'`。`'\n'` 匹配换行符。序列 `'\\'` 匹配 `"\"`，而 `'\('` 则匹配 `"("`。|
| <font color="red">一竖</font>指明两项之间的一个选择。(或者的意思)| 要匹配 `一竖`，请使用 `\一竖`。|
(一竖表示`|`由于文档不识别， 所以分开)


### 定位符(边界)
定位符用来描述字符串或单词的边界，`^` 和 `$` 分别指字符串的开始与结束，`\b` 描述单词的前或后边界，`\B` 表示非单词边界。

|   元字符  | 
| --------   |
| <font color="red">^</font> 以XXX开始。 如果设置了 RegExp 对象的 Multiline 属性，`^` 还会与 `\n` 或 `\r` 之后的位置匹配。|
| <font color="red">$</font> 以XXX结束。 如果设置了 RegExp 对象的 Multiline 属性，`$` 还会与 `\n` 或 `\r` 之前的位置匹配。|
| <font color="red">\b</font> 单词边界。 即字与空格间的位置。|
| <font color="red">\B</font> 非单词边界 - |
```
<!-- \b, \B -->
'This is a body'.replace(/is/g, '0'); // Th0 0 a body
'This is a body'.replace(/\bis\b/g, '0'); // This 0 a body
'This is a body'.replace(/\Bis\b/g, '0'); // Th0 is a body
<!-- ^在类以外表示已xx开头 -->
'@123@abc@'.replace(/@./g, 'Q'); // 把@加任意字符换成Q=>Q23Qbc@
'@123@abc@'.replace(/^@./g, 'Q'); // 把以(@加任意字符)开头的换成Q=>Q23@abc@
<!-- $ -->
'@123@abc@'.replace(/.@$/g, 'Q'); // @123@abQ
```

### 限定符（量词）
用来指定正则表达式的一个给定组件必须要出现多少次才能满足匹配。有 `*` 或 `+` 或 `?` 或 `{n}` 或 `{n,}` 或 `{n,m}` 共6种。

|   元字符  |  例子  |
| --------   | :-----: |
| <font color="red">* </font> = `{0,}` 出现 `0` 次或 `n` 次。{任意性} | 例如，`zo*` 能匹配 `"z"` 以及 `"zoo"`|
| <font color="red">+</font> = `{1,}` 出现 `1` 次或 `n` 次（至少出现1次）| 例如，`'zo+'` 能匹配 `"zo"` 以及 `"zoo"`，但不能匹配 `"z"`|
| <font color="red">?</font>  = `{0,1}` 出现 `0` 次或 `1` 次（最多出现1次）| 例如，`"do(es)?"` 可以匹配 `"do"` 、 `"does"` 中的 `"does"` 、 `"doxy"` 中的 `"do"`|
| <font color="red">{n}</font> `n` 是一个非负整数。出现 `n` 次。| 例如，`'o{2}'` 不能匹配 `"Bob`" 中的 `'o'`，但是能匹配 `"food"` 中的两个 `o`。|
| <font color="red">{n,}</font> `n` 是一个非负整数。至少出现`n` 次。 `'o{1,}'` 等价于 `'o+'`。`'o{0,}'` 则等价于 `'o*'`| 例如，`'o{2,}'` 不能匹配 `"Bob"` 中的 `'o'`，但能匹配 `"foooood"` 中的所有 `o`|
| <font color="red">{n,m}</font> `m` 和 `n` 均为非负整数，其中`n <= m`。出现 `n` 到 `m` 次。| 例如，`"o{1,3}"` 将匹配 `"fooooood"` 中的前三个 `o`。`'o{0,1}'` 等价于 `'o?'`。请注意在逗号和两个数之间不能有空格。|
```
<!--匹配一个联系出现20次数字的字符串  -->
\d\d\d\d\d... = \d{20}

\d{20}\w\d?\w+\d*\d{3}\w{3,5}\d{3,}
```

### 贪婪模式(默认)
`\d{3,6}` 对于 `12345678` 匹配几位数？（3？4？5？6）=》尽量多的匹配
```
'12345678'.replace(/\d{3,6}/g, 'X'); // X78
```

### 非贪婪模式（少匹配）
匹配成功了就不匹配了，即匹配到3，在两次后加上`？`即可
```
'12345678'.replace(/\d{3,6}?/g, 'X'); // "XX78" 匹配到123-》X 停止  456=》X 剩余的无法匹配了
```

### 分组（）
匹配字符Byron连续出现3次的场景 =》 以为以`Byron{3}`=>实际是Byron中的n循环了3次。所以要分组，确定量词那里执行
```
(Byron){3} // 即可
<!-- 匹配字母数字连续出现三次 -->
'a1b2c3d4'.replace(/([a-z]\d){3}/g,'X'); // Xd4
```

#### 或 | 
使用 `|` 实现或的表达效果
```
Byron | Casper // 把其分成两部分
<!-- 一部分分成 -->
Byr(on|Ca)sper
'ByronCasper'.replace(/Byron|Casper/g, 'X'); // XX
'ByronsperByrCasper'.replace(/Byr(on|Ca)sper/g, 'X'); // XX
```

#### 反向引用（变量()->$）
`$`表示捕获每个分组
把日期 `2014-12-12`-> `2014/12/12`(日期是动态变化的)
```
'2014-12-13'.replace(/\d{4}-\d{2}-\d{2}/g, '13/12/2131'); // 不知道数字是多少
结果： 13/12/2131 不是 2014/12/12
<!-- 所以需要一个变量$X -->
<!-- 没分组 -->
'2014-12-13'.replace(/\d{4}-\d{2}-\d{2}/g, '$1'); // $1
'2014-12-13'.replace(/(\d{4})-(\d{2})-(\d{2})/g, '$1');  // 2014
'2014-12-13'.replace(/(\d{4})-(\d{2})-(\d{2})/g, '$2');  // 12
'2014-12-13'.replace(/(\d{4})-(\d{2})-(\d{2})/g, '$3');  // 13
'2014-12-13'.replace(/(\d{4})-(\d{2})-(\d{2})/g, '$2/$3/$1');  // 月日年 12/13/2014
```
#### 忽略分组
不希望捕获某些分组，只需要在分组内加上 `？：` 就可以
```
(?:Byron).(ok) // Byron这个分组没得了  只有Ok这个分组
```

### 前瞻
- 正则表达式从文本头部向尾部开始解析，文本尾部方向，称为“前”；
- 前瞻 `就是在正则表达式匹配到规则的时候`，`向前检查` <font color="red">是否符合断言(开发者预测这个结果可能是什么，是对的就过去，否则就跳出)</font>后顾/后瞻方向与前瞻相反；
- 之前的规则只是匹配某个字符串，现在要看是否符合断言部分。例如匹配张三，现在需要匹配到张三同时需要往前看其父亲还得是张二；
- js 不支持后顾；
- 符合特定断言称为 `肯定/正向` 匹配不符合特定断言称为 `否定/负向` 匹配；

#### 正向前瞻-负向前瞻
`正向前瞻`: exp(?=assert) // exp正则， (?=assert)断言部分 `\w(?=\d)`
`负向前瞻`: exp(?!assert)
```
'a2*3'.replace(/\w(?=\d)/g, 'X'); // 匹配的是单词字符，断言---但是是后面含有数字的 X2*3（a2单词字符后面是数字 *不是单词字符 3后面不是数字）
'a2*34v8'.replace(/\w(?=\d)/g, 'X'); // X2*X4X8（a2单词字符后面是数字 *不是单词字符 34、v8后面是数字 ）
'a2*34vv'.replace(/\w(?=\d)/g, 'X'); // X2*X4vv
'a2*34vv'.replace(/\w(?!\d)/g, 'X'); // 匹配的是单词字符，断言---后面不是数字的 aX*3XXX
```

## test 和 exec 方法
### test
- 用于测试字符串参数中是否存在正则表达式模式的字符串；
- 存在返回`true`, 否则返回 `false`;
```
var reg1 = /\w/;
var reg2 = /\w/g;
reg1.test('a'); // true
reg1.test('$'); // false
reg1.test('a'); // true
reg1.test('$'); // false
<!-- 以上是正常的 -->
但是加上g
reg2.test('ab'); // true
reg2.test('ab'); // true
reg2.test('ab'); // false
reg2.test('ab'); // true
reg2.test('ab'); // true
reg2.test('ab'); // false
其实是lastIndex在作怪，第一次执行后会记住
while(reg2.test('ab')){
    console.log(reg2.lastIndex); // 1 2 第一次1，第二次是2，
}
```