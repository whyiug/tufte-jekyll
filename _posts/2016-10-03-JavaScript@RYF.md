---
layout: post
title: "JavaScript@RYF"
description: ""
date: 2016-10-03
tags: [JavaScript]
comments: true
share: true
---

## JavaScript简述

### 特性：
* 轻量级的，解释型的，浏览器内置的脚本语言
* 弱类型，动态类型的网页脚本语言。声明变量的时候不需要指定类型
* 基于原型的，多范式的面向对象语言
* 函数式语言，包含大量lambda函数。将函数视作第一等公民，函数为基本数据类型
* JavaScript简单的是，核心内容只有基本语法和函数库。复杂的是，部署环境提供的api众多。
如果部署环境是浏览器，提供额外的api包括，浏览器类，DOM类，web类。
浏览器控制类的接口用来操作浏览器，DOM类的接口用来操作网页的各种元素，Web类的接口用来实现互联网的各种功能。

## 编程风格
* 总用var声明变量，不需要声明类型 

```js
var a; //a:undefined
```
* {}代码块的位置  
'{'建议放在同一行。因此JavaScript引擎解析代码的时候，会在每行后面加上";"

```js
return {
    key:value
};
// 而不是
return
{
    key:value
}
```
* 建议不要省略末尾的分号
* JavaScript最大的语法缺点，可能就是全局变量对于任何一个代码块，都是可读可写。这对代码的模块化和重复使用，非常不利。

## 变量
* 变量提升(hoisting)：  
JavaScript引擎的工作方式是，先解析代码，获取所有被声明的变量，然后再一行一行地运行。

```js
console.log(a);
var a = 1; //错误，但不会报错
// 相当于：
var a;
console.log(a);
a = 1;
```
* 数据类型  

```
             ----原始类型（primitive type）：数值，布尔，字符串
数据类型--- <
             ----合成类型（complex type）：数组，函数，对象  
-----------------两个特殊值： null和 undefined
```
* null和 undefined  
null表示没有， undefined表示未定义。关于 null和 undefined的区别，可以理解成 null表示什么也没有（ nothing）， undefined表示缺少一个值。  

```js
typeof null // "object"```
```
这并不是说null的数据类型就是对象，而是JavaScript早期部署中的一个约定俗成.
本质上null是一个类似于undefined的特殊值。  
以下三种情况会引起 undefined变量声明但没有赋值。读取对象不存在的属性。运行没有返回语句的函数。
* 5个false  

```js
undefined ,null ,false ,0 ,""
```
* typeof方法  

```js
function f() {}
typeof f // function
typeof undefined // "undefined"
typeof window // "object"
typeof {} // "object"
typeof [] // "object"
typeof null // "object"
if (typeof v === "undefined") {
    // ...
} //正确的判断v是否定义的方法
```

## 数值
* 整数  
JavaScript内部，所有数字都是以 64位浮点数形式储存，即使整数也是如此。所以， 1与 1.0是相等的，而且 1加上 1.0得到的还是一个整数，不会像有些语言那样变成小数。

```js
1 === 1.0 // true
1 + 1.0 // 2
```
* 浮点数运算  
由于浮点数不是精确的值，所以涉及小数的比较和运算要特别小心。

```js
0.1 + 0.2 === 0.3 // false
0.3 / 0.1 // 2.9999999999999996
(0.3 - 0.2) === (0.2 - 0.1) // false
```
* 正零和负零

```js
(1 / +0) === (1 / -0) // false
```
* NaN(Not a Number)

```js
NaN === NaN // false
Boolean(NaN) // false
0 /0 // NaN
isNaN(NaN) // true
// isNaN只对数值有效，如果传入其他值，会被先转成数值。
isNaN("Hello") // true
// 相当于
isNaN(Number("Hello")) // true
isNaN({}) // true
isNaN(["xzy"]) // true
```
isNaN为 true的值，有可能不是 NaN，而是一个字符串。因此，判断 NaN更可靠的方法是，利用 NaN是 JavaScript之中唯一不等于自身的值这个特点，进行判断。

```js
function myIsNaN(value) {
    return value !== value;
}
```
* Infinity: Infinity有正负之分。  

```js
1 / -0 // -Infinity
1 / +0 // Infinity
Infinity === -Infinity // false
```
* 由于数值正向溢出（ overflow）、负向溢出（ underflow）和被 0除， JavaScript都不报错，所以单纯的数学运算几乎没有可能抛出错误。
* isFinite函数返回一个布尔值，检查某个值是否为正常值，而不是 Infinity。
* parseInt()方法  
parseInt方法可以将字符串或小数转化为整数。如果字符串头部有空格，空格会被自动去除。

```js
parseInt("123") // 123
parseInt(1.23) // 1
// 如果字符串包含不能转化为数字的字符，则不再进行转化，返回已经转好的部分。
parseInt("8a23") // 8
// 如果字符串的第一个字符不能转化为数字（正负号除外），返回 NaN。
parseInt("abc") // NaN
parseInt(".3") // NaN
parseInt方法还可以接受第二个参数（ 2到 36之间），表示被解析的值的进制。
parseInt(1000, 2) // 8
parseInt(1000, 6) // 216
parseInt(1000, 8) // 512
// 这意味着，可以用 parseInt方法进行进制的转换。
parseInt("1011", 2) // 11
```
* parseFloat()方法  
用于将一个字符串转为浮点数。

```js
parseFloat("3.14");
parseFloat("314e-2");
parseFloat("0.0314E+2");
parseFloat("3.14more non-digit characters");
parseFloat("FF2") // NaN
```
## 字符串
* 单引号和双引号混用的时候，需要转义
* 在JavaScript内部，字符串可以被视为字符数组。
```js
'abc'[1] // b
'abc'[3] // undefined
'abc'[-1] // undefined
'abc'["x"] // undefined
// 但是字符串内部的单个字符无法改变和增删
var s = 'hello';
delete s[0];
s // "hello"
s[1] = 'a';
s // "hello"
```

## 类和对象

* JavaScript中所有数据都可以看做为对象。对象是无序的数据集合。数组和函数式特殊的对象，原始类型有对应的包装对象。

```js
var o = {
    p: 123, // 对象的键名都是字符串类型，引号可以省略
    m: function() {...
    } //逗号最好不加
}
```
* 创建对象的三种方法

```js
var o1 = {}; // 简洁
var o2 = new Object(); // 意图清晰
var o3 = Object.create(null); // 继承
```
* 操作对象的两个方法

```js
var o = {
    p: "Hello world"
};
o['p'] // "Hello world"
o.p // "Hello world"
o[p] // undefined，这里把p当成了变量，undefined
```
* JavaScript允许后绑定

```js
var o = {
    key1: 1,
    key2: 2
};
o.key3 = 3;
Object.keys(o); // ["key1","key2","key3"]
```
* 删除对象属性的两个方法

```js
delete o.key1
o.key1 = undefined
// delete方法总是返回true，只有一种情况会返回false
var o = {};
delete o.p; // true, 删除了一个不存在的属性，返回true
```
* 引用传递  
对象之间的值传递为引用传递，不同引用名可以指向同一个地址。数组是传址传递。

```js
var o1 = {};
var o2 = o1;
o1.a = 1;
o2.a // 1
//但是
var o1 = [1, 2, 3];
var o2 = o1;
o2 = [4, 5, 6];
o1 // [1, 2, 3]
```
* in 运算符  
in 用来检查一个对象中是否包含某个键名，注意不是键值

```js
var o = {
    p: undefined
};
'p' in o //true
//适用于数组
var a = ["hello", "world"];
0 in a // true
1 in a // true
'2' in ['a', 'b', 'c'] //true
```
* with语句  
不建议使用

```js
o.p1 = 1;
o.p2 = 2;
// 等同于
with(o) {
    p1 = 1;
    p2 = 2;
}
```

## 数组
* JavaScript中的数组不像PHP那样分为数值数组和关联数组，它只有数值数组。键名默认用数字。用方括号声明

```js
var arr = [];
arr[0] = 'a';
arr[1] = 'b';
arr[2] = 'c';
//相当于
var arr = ['a', 'b', 'c'];
```
* 数组和对象的区别在于数组不需要有键名，对象必须要有显示的键名。
* length属性

```js
var arr = ['a', 'b'];
arr.length // 2
arr[100] = 'c';
arr.length // 101，数组的数字键不需要连续。length为最大键+1
arr.length = 2;
arr // ['a', [b]]
// 可以这样清空一个数组
arr.length = 0;
```
* 数组的键值可以为任意数据类型

```js
var arr = [{
        a: 1
    },
    [1, 2, 3],
    function() {
        return true;
    }
];
arr[0] // Object {a: 1}
arr[1] // [1, 2, 3]
arr[2] // function (){return true;}
//多维数组
var a = [
    [1, 2],
    [3, 4]
];
```
* 删除数组中的元素

```js
var a = [1, 2, 3];
a.length // 3
delete a[1];
a // [1, undefined, 3]
a.length // 3
//此时a = [1,,3]
```
* 遍历一个数组

```js
var a = [1, 2, 3, ,5];
for (var i in a){
    console.log(a[i]);
}
// 或者
a.forEach(function(x, i){
    console.log(i + ". " + x);
})
//如果这样的话,会忽略非数字键，而且会遍历空位
for (var i = 0; i < a.length; i++) {} 
```
* 用Array对象来创建数组  
注意一个参数和两个参数的区别

```js
var a = new Array(2); // [undefined *2]
a.length // 2
var a = new Array(1, 2); // [1, 2]
a.length // 2
```

## 函数
* 声明函数有三种方式

```js
function print(){}
//或者
var print = function(){
    // 将函数表达式或者匿名函数赋值给变量
};// 注意逗号
var foo = new Function(
  'return "hello world"'
);
```
* JavaScript中函数和其他数据类型一样，都是第一等公民

```js
function add(x, y) {
  return x + y;
}

// 将函数赋值给一个变量
var operator = add;

// 将函数作为参数和返回值
function a(op){
  return op;
}
a(add)(1, 1)
// 2
```
* 函数提升

```js
function f() {
  console.log(1);
}
f() // 2,这里注意

function f() {
  console.log(2);
}
f() // 2
```
* 函数是参数的数量

```js
function f(a, b) {
    return a;
}
f(1, 2, 3) // 1
f(1) // 1
f() // undefined
```
* name属性和length属性

```js
function f(a, b){}
f.length // 2
f.name // f
```
* 函数参数的默认值

```js
function f(a) {
    a = a || 1;
    return a;
}
// 函数本意是，如果传参则返回参数，否则返回1
f('') // 1
f(0) // 1
//更加精确的写法
function f(a) {
    (a !== undefined && a != null) ? (a = a) : (a = 1);
    return a;
}
f('') // ""
f(0) // 0
```
* 函数参数的传递  
函数参数的传递为值传递。但是，当参数为对象引用时，改变对象的属性，可以改变函数体外的对象的属性.
(复合类型变量的属性值是传址传递。)

```js
// 修改复合类型的参数值
var o = [1, 2, 3];

function f(o) {
    o = [2, 3, 4];
}
f(o);
o // [1, 2, 3]
// 修改对象的属性值，数组也一样
var o = {
    p: 1
};

function f(obj) {
    obj.p = 2;
}
f(o);
o.p // 2
```
* JavaScript允许函数的参数列表不定长度，因此它提供了一个机制。在函数体内部，存在 argument对象。（只能在函数体内使用）

```js
var f = function(one) {
    console.log(arguments[0]);
    console.log(arguments[1]);
    console.log(arguments[2]);
    return arguments.length;
}
f(1, 2, 3) // 1 2 3 3
var f = function(a, b) {
    arguments[0] = 3;
    arguments[1] = 2;
    return a + b;
}
f(1, 1) // 5
// 将 argument对象转换为数组：
var args = Array.prototype.slice.call(arguments);
// 或者
var args = [];
for (var i = 0; i < arguments.length; i++) {
    args.push(arguments[i]);
}
```
* 闭包函数  
闭包不仅可以读取函数内部变量，还可以使得内部变量记住上一次调用时的运算结果。
立即调用的函数表达式：（ Immediately - Invoked Function Expression），简称 IIFE。(function() { /*code*/ })()或者(function() { /*code*/ }())

```js
//解释器会将函数当做表达式来处理，所以以下写法是正确的。(建议都加括号)
var i = function() {}();
true && function() {}()；
0，
function() {}();
! function() {}();~

function() {}();
new function() {}();
//通常只对使用一次的匿名函数使用IIFE
```
* eval

```js
var a = 1;
eval("a=2");
a // 2
```
* eval的危险性
* Function构造匿名函数

```js
var a = 1;
Function("a=2")()
a // 2
```
## 运算符
* 转化为数值

```js
+true // 1
+[] // 0
+{} // NaN
-(-x) // 相当于Number(x)
```
* 全等运算符

```js
1 === "1" // false
true === "true" // false
undefined === undefined // true
NaN === NaN // false
null === null // true
```
* 复合类型比较的时候，比较的是，它们是否指向了同一地址。

```js
({}) === {} // false
[] === [] // false
(function (){}) === function (){} // false
//undefined和null与其他类型的值比较时，结果都为false，它们互相比较时结果为true。
false == null // false
0 == null // false
undefined == null // true
```
* 建议不适用相等，一直使用全等符号

```js
'' == '0'           // false
0 == ''             // true
0 == '0'            // true 
false == 'false'    // false
false == '0'        // true 
false == undefined  // false
false == null       // false
null == undefined   // true
' \t\r\n ' == 0     // true
// !!x相当于Boolean(x)
```
* &&且运算符 （短路原则）  
A && B  
如果A成立，则计算B，并返回B结果；如果A不成立，则返回A结果。

```js
var x = 1;
(1-1) && (x+=1) // 0
x // 1
```
* || 运算符
* 位运算符

## 数据类型转换
* JavaScript是动态类型的脚本语言，变量没有显式类型，但是数据和运算是有类型的。大部分情况下数据的类型可以自动转化，也可以强制类型转化。
* 转化为数值： Number()

```js
Number("324") // 324
Number("324abc") //NaN
Number("") //0
Number(false) // 0
Number("undefined") // NaN
Number(null) // 0
//对象转化为数值
Number({valueOf:function (){return 2;}}) // 2 
Number({toString:function(){return 3;}}) // 3 
Number({valueOf:function (){return 2;},toString:function(){return 3;}}) // 2 先valueof后tostring
Number({a:1}) // NaN
```
* 转化为字符串

```js
String(123) // "123"
String("abc") // "abc"
String(true) // "true", 这里要注意不是1
String(undefined) // "undefined"
String(null) // "null"
//对象转化为字符串
String({toString:function(){return 3;}}) // "3" 
String({valueOf:function (){return 2;}}) // "[object Object]" 
String({valueOf:function (){return 2;},toString:function(){return 3;}}) // "3" 相反
String({a:1}) // "[object Object]"
```
* 转化为布尔型

```js
if (!undefined && !null && !0 && !NaN && !''){
    console.log('true');
} // true
```
## 错误处理机制
* try...catch结构

```js
try {
    throw new Error('出错了!');
} catch (e) {
    console.log(e.name + ": " + e.message);  // Error: 出错了！
    console.log(e.stack);  // 不是标准属性，但是浏览器支持
}
```
## Array类
* Array类是JavaScript的内置类，同时也是一个构造函数，可以用来生成数组。

```js
var a1 = new Array(); // []
var a2 = new Array(1); //[undefined *1 ]
var a3 = new Array('abc'); // ['abc']
var a4 = new Array([1]); //[array[1]]
var a5 = new Array(1,2,3) //[1,2,3]
//最好用方括号 [] 生成数组
```
* Array对象的一些静态方法 valueOf toString push pop

```js
var arr = [1, 2, 3];
typeof arr // object
Array.isArray(arr) // true
//Array对象的方法
var a = [1, 2, 3, [4, 5, 6]]
a.valueOf() // [1, 2, 3, [4, 5, 6]]
a.toString() // '1,2,3,4,5,6'
```
* push和pop操作

```js
var a = new Array();
a.push(1) // 1
a.push("a") // 2
a // [1,"a"]
var a = [1,2,3];
var b = [4,5,6];
Array.prototype.push.apply(a, b);
// 等同于
a.push(4,5,6)
a // [1, 2, 3, 4, 5, 6]
// pop方法用于删除数组的最后一个元素，并返回该元素。
var a = ['a', 'b', 'c'];
a.pop() // 'c'
a // ['a', 'b']
// 对空数组使用pop方法，不会报错，而是返回undefined。 
[].pop() // undefined
```
* join方法

```js
var a = [1, 2, 3, 4];
a.join(); // "1,2,3,4"
a.join(''); // "1234"
a.join('|'); //"1|2|3|4"
```
* concat操作

```js
[1, 2, 3].concat(4, 5, 6); //[1, 2, 3, 4, 5, 6]
```
* shift和unshift操作

```js
// shift删除第一个元素并返回
var arr = ['a', 'b', 'c'];
arr.shift(); // a
// unshift在头部添加一个元素，并返回新数组长度
var arr=['a','b','c'];
arr.unshift('x'); // 4 
```
* reverse方法
* slice方法用来获取元素。参数为区间，左闭右开

```js
var a = ['a', 'b', 'c'];
a.slice(1, 2) // ["b"]
a.slice(1) // ["b", "c"]
a.slice(0) // ["a", "b", "c"]
a.slice(-2) // ["b", "c"]
a.slice(4) // []
// slice另一个重要的用法，是将类似数组的对象转换为真正的数组

Array.prototype.slice.call({ 0: 'a', 1: 'b', length: 2 }) // ['a', 'b'] 
Array.prototype.slice.call(document.querySelectorAll("div"));
Array.prototype.slice.call(arguments);
```
* splice用来删除元素，并返回删除的元素。参数为起始位置和数目

```js
//两个参数
var a= ['a','b','c','d','e','f'];
a.splice(4,2); // ['e','f']
a // ['a','b','c','d']
// 更多参数
var a = ["a","b","c","d","e","f"];
a.splice(4,2,1,2) // ["e", "f"]
a // ["a", "b", "c", "d", 1, 2]
var a = [1,1,1];
a.splice(1,0,2) // [] 
a // [1, 2, 1, 1]
// 三个参数 ，单纯的插入
var a = [1,1,1];
a.splice(1,0,2) // [] 
a // [1, 2, 1, 1]
// 一个参数，拆分
var a = [1,2,3,4];
a.splice(2) // [3, 4] 
a // [1, 2]
```
* sort方法对数组进行--字典序--排序

```js
var a = [10111, 1101, 111];
a.sort() // [10111, 1101, 111]
// 接受一个函数类型的参数
function f(a, b){
    return a - b;
}
a.sort(f); // [111, 1101, 10111]
// 用闭包函数
[
    { name: "张三", age: 30 },
    { name: "李四", age: 24 },
    { name: "王五", age: 28  }
].sort(function(o1, o2){
    return o1.age = o2.age;
})
```
* map方法对数组中的每一个成员都执行一次函数，注意不改变 元素组的值。接受一个函数作为参数。

```js
[1, 2, 3].map(function(elem, index, arr){
    return elem * elem;
}) // [1, 4, 9]
a // [1, 2, 3]
```
* forEach方法对所有元素依次执行一个函数，它与map的区别在于不返回新数组，而是对原数组的成员执行某种操作，甚至可能改变原数组的值。

```js
[1, 2, 3].forEach(function(elem, index, arr){
    console.log("array[" + index + "] = " + elem);
});
// array[0] = 1
// array[1] = 2
// array[2] = 3
// map和forEach都可以接受第二个参数，用来绑定this关键字
var out = [];
[1, 2, 3].map(function(elem, index, arr){
    this.push(elem * elem);
}, out);
out // [1, 4, 9]
```
* filter方法过滤并返回新数组

```js
[1, 2, 3, 4, 5].filter(function(elem, index, arr){
    return index % 2 === 0;
}); // [1, 3, 5]
```
* some和every断言
* indexOf和lastIndexOf方法

```js
var a = ['a','b','c'];
a.indexOf('b') // 1 
a.indexOf('y') // -1
['a','b','c'].indexOf('a', 1) // -1
var a = [2, 5, 9, 2];
a.lastIndexOf(2) // 3
```
* 数组方法的链式使用

```js
var users = [{name:"tom", email:"tom@example.com"},
             {name:"peter", email:"peter@example.com"}];
users
.map(function (user){ return user.email; })
.filter(function (email) { return /^t/.test(email); })
.forEach(alert);// 弹出tom@example.com
```

## JavaScript包装对象
* JavaScript可以把所有数据都看做是对象，所以数值，字符串，布尔型都有自己的包装对象，分别为Number,Boolean,String.

```js
typeof "abc" // 'string'
typeof new String("abc") // 'object'
"abc" instanceof Object // false
new String("abc") instanceof Object // true
```
* 包装对象使得原始类型的数据也可以用对象的方法。例如valueOf和toString，字符串还有length属性

```js
new String("abc").valueOf() // "abc"
new Boolean("true").toString() // "true"
// 字符串默认自动转化成字符串对象
"abc".length // 3
var s1 = "some text";
s1.substring(2); // "me text"
```

### JavaScript Boolean对象
* 尽量不用下面的第一种声明，而是用第二种

```js
var b1 = new Boolean(false);
b1; // false 
var b2 = true;
b1 && b2 // true ,b1对象会自动转换为布尔型
```
* 转化为类型Boolean

```js
Boolean([]) // true
Boolean({}) // true
Boolean(function(){}) // true
Boolean(/foo/) // true
!![] // true
!!{} // true
```
### JavaScript Number对象
* 数值在运算时可以自动转化为Number包装对象
* toString方法用来转化进制

```js
10).toString() // "10"
(10).toString(2) // "1010"
(10).toString(8) // "12"
(10).toString(16) // "a"
// 建议一致加上括号
```
* toFixed用来指定小数的位数

```js
(10).toFixed(2); // 10.00
(10.005).toFixed(2); //10.01
```

### JavaScript Math对象
* Math虽然是内置类，但是不能用来生成对象
* 记住一些常量
* 记住一些方法

```js
Math.floor()
Math.ceil()
Math.pow(a,b)
Math.sqrt()
Math.abs()
Math.max()
Math.min()
Math.round()
Math.random()
// 返回给定范围内的随机数
function getRandomArbitrary(min, max) {
  return Math.random() * (max - min) + min;
}
 // 返回给定范围内的随机整数
function getRandomInt(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
 }
```
### JavaScript String对象
* charAt方法

```js
var s = new String("abc");
s.charAt(1) // "b"
s.charAt(s.length-1) // "c"
// 这个方法完全可以用数组下标替代
"abc"[1] // "b"
```
* concat方法用来连接两个字符串，原来字符串不变

```js
var a = "abc";
var b = "def";
a.concat(b); // "abcdef"
a // "abc"
```
* slice, substring, sbustr方法  
前两者参数为区间。substr第二个参数是长度
* indexOf和lastIndexOf
```js
"hello world".indexOf("o", 6) // 7 
"hello world".lastIndexOf("o", 6) // 4 ,向前匹配
```
* trim, toLowerCase, toUpperCase

```js
"  hello world  ".trim() // "hello world"
```
* #match search split replace等等

### JavaScript Date对象
* Date() 和 new Date() 都能创建当前时间的对象
* new Date(year, month, day [, hour, minute, second, millisecond]);month=2意味着3月份
* new Date(),接收毫秒时间戳，相等于unix时间戳的1000倍
* 两个日期相减

```js
var then = new Date(2013,2,1);
var now = new Date(2013,3,1);
now - then // 2678400000
```
* Date对象的方法返回的都是毫秒时间戳

```js
Date.now()
Date.parse(...)
Date.UTC(...)
```
* toString方法（F12敲一遍）

```js
var today = new Date();
today.toString();
today.toDateString();
today.toTimeString();
today.toLocaleDateString()
today.toLocaleTimeString()
```
* valueOf相当于getTime

```js
var today = new Date();
today.valueOf() // 1362790014817 
today.getTime() // 1362790014817
```
* 常用来获取耗费时间

```js
var start = new Date();
// do something();
var end = new Date();
var elapsed = end.getTime() = start.getTime();
```
* Date对象的几个常用方法

```js
var date1 = new Date ( "January 6, 2013" );
date1.getDate() // 6
date1.getMonth() // 0
date1.getFullYear() // 2013
date1.getDay() // 0 (周日)
```

### JavaScript RegExp对象
* 生成对象的两种方法

```js
var regex = /xyx/[igm];
var regex = new RegExp('xyz','[igm]');
// 建议使用第一种写法
```
* 正则对象的一些属性

```js
var r = /abc/igm;
r.ignoreCase // true
r.global // true
r.multiline // true
r.lastIndex // 0
r.source // "abc"
```
* 正则对象的test方法

```js
var regex = /x/g;
var str = '_x_x';
regex.lastIndex // 0
regex.test(str) // true
regex.lastIndex // 2
regex.test(str) // true
regex.lastIndex // 4
regex.test(str) // false
```
* exec方法返回一个字符串中所有匹配正则模式的结果。
var regex = /a(b+)a/;
regex.exec("_abbba_aba_")
// [ 'abbba'
//    , 'bbb'
//    , index: 1
//    , input: '_abbba_aba_'
// ] 
regex.lastIndex // 0
* 正则用于字符串的查找替换等

### JavaScript Json对象
> son指JavaScript Object Notation  
> 是一种用于数据交换的文本格式。相对于传统的XML格式有两个主要优点：  
> 书写简单，占用空间少；符合JavaScript原生语法，直接直接被引擎处理。  

* json格式就是表示一系列值的方法。
* 规则：
数组或对象的每个成员的值，可以是简单值，也可以是复合值。  
简单值分为四种：字符串、数值（必须以十进制表示）、布尔值和null。  
复合值分为两种：符合JSON格式的对象和符合JSON格式的数组。  
数组或对象最后一个成员的后面，不能加逗号。  
数组或对象之中的字符串必须使用双引号，不能使用单引号。  
对象的成员名称必须使用双引号。
* JSON对象为ECMAScript5新增内容

```js
JSON.stringify():将对象转化为json字符串
JSON.parse(): 将json字符串转化为对象
e.g.
JSON.stringify("abc"); // ""abc"" 
JSON.stringify({ name: "张三" }); // {"name":"张三"}
var o = JSON.parse('{"name":"张三"}');
o.name // 张三
```

## JavaScript DOM简述
* DOM的基本思想是把结构化的文档解析成一系列节点，所有这些节点将组成树状结构。这些节点和树状结构都有规范的的操作接口，使得编程语言可以方便的操作文档。浏览器原生提供一个Node对象，所有类型的节点都有Node对象派生出来。
* Node对象的三个属性  
--- nodeName nodeType nodeValue
* HTML文档中常用的节点

```
类型          nodeName        nodeType    nodeValue
DOCUMENT_NODE   #document           9           null    
ELEMENT_NODE    大写的HTML元素名  1           null
ATTRIBUTE_NODE  等同于Attr.name        2           null
TEXT_NODE       #text               3           {text}
```

```js
// 判断一个节点的类型
document.querySelector('a').nodeType === 1 //true
document.querySelector('a').nodeType === ELEMENT_NODE //true
// 注意：nodeValue只能用来返回文本节点的文本内容，其他的都返回null
```
* Node对象的属性---childNodes 和 children

```js
childNodes:返回所有子节点
children:返回所有html元素类型的子节点。
```
* Node对象的一些方法

```js
appendChild()
cloneNode()
compareDocumentPosition()
contains()
hasChildNodes()
insertBefore()
isEqualNode()
removeChild()
replaceChild()
```

## 浏览器对象
* 浏览器有渲染引擎和JavaScript解释器（JavaScript引擎）
* 浏览器解析网页的步骤。会出现JavaScript代码下载阻塞，称作“阻塞效应”。解决方法有：

```
1,script代码最好放在html文档的尾部。
2,onload, defer, async等
```
* setTimeout和setInterval方法, clearTimeout(id)和clearInterval

```js
var timeId = setTimeout(func | code, delay)
setInterval(func | code, delay)
// 确保执行完间隔时间代码
var i = 1;
var timer = setTimeout(function() {
  alert(i++);
  timer = setTimeout(arguments.callee, 2000);
}, 2000);
```
* window对象
JavaScript在浏览器环境中运行，顶层对象就是window对象。所有浏览器环境的全局变量都是window对象的属性。可以简单的理解为，window就是指当前的浏览器窗口。

```js
window.name = "Hello World!" // 刷新窗口值不变
alert(message);
prompt(message, '');
confirm(message);
```
* history对象

```js
history.back();
history.forward();
history.go();history.go(0);history.go(-1);history.go(1);
```
* 控制台的console对象

```js
console.log() // %d %s %i %f
// 覆盖console的各个方法
['log', 'info', 'warn', 'error'].forEach(function(method) {
  console[method] = console[method].bind(
    console,
    new Date().toISOString()
  );
});
console.table();
console.dir(document.body)
console.assert(true === false, "判断条件不成立")
```
* 计时器

```js
console.time("Array initialize");
var array= new Array(1000000);
for (var i = array.length - 1; i >= 0; i--) {
    array[i] = new Object();
};
console.timeEnd("Array initialize");
// 性能测试器：
console.profile();console.profileEnd();
```
* debugger语句

```js
for(var i = 0;i < 5;i++){
  console.log(i);
  if (i === 2) debugger;
}
```
