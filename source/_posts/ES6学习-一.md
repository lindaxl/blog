---
title: ES6学习(一)
date: 2017-10-19 13:04:47
categories: Javascript
tags:
  - ES6
---
# 一： let和const
## 1.在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）
“暂时性死区”也意味着typeof不再是一个百分之百安全的操作。
````
typeof x; // ReferenceError
let x;
````
上面代码中，变量x使用let命令声明，所以在声明之前，都属于x的“死区”，只要用到该变量就会报错。因此，typeof运行时就会抛出一个ReferenceError。

作为比较，如果一个变量根本没有被声明，使用typeof反而不会报错。
````
typeof undeclared_variable // "undefined"
````
不允许重复声明
let不允许在相同作用域内，重复声明同一个变量。

## 2.ES6中的函数(下面3条规则只对实现ES6的浏览器有效，其他环境的实现不用考虑，还是将块级作用域的函数声明当作let处理。)
1.允许在块级作用域内声明函数。
2.函数声明类似于var，即会提升到全局作用域或函数作用域的头部。
3.同时，函数声明还会提升到所在的块级作用域的头部。
````
// 浏览器的 ES6 环境
function f() { console.log('I am outside!'); }

(function () {
  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }
  f();
}());
// Uncaught TypeError: f is not a function

相当于是
function f() { console.log('I am outside!'); }
(function () {
  var  f= undefined;
  if (false) {
    function f() { console.log('I am inside!'); }
  }
  f();
}());
// Uncaught TypeError: f is not a function
````
考虑到环境导致的行为差异太大，应该避免在块级作用域内声明函数。如果确实需要，也应该写成函数表达式，而不是函数声明语句。
<!-- more -->
# 二： 变量的解构赋值
## 1.数组的解构赋值
1.ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）
如果等号的右边不是数组（或者严格地说，不是可遍历的结构，参见《Iterator》一章），那么将会报错。
````
// 报错
let [foo] = 1;
let [foo] = false;
let [foo] = NaN;
let [foo] = undefined;
let [foo] = null;
let [foo] = {};
````
对于 Set 结构，也可以使用数组的解构赋值。
````
let [x, y, z] = new Set(['a', 'b', 'c']);
x // "a"
````
2.默认值
````
let [foo = true] = [];
foo // true
let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b'
````
在ES6 内部使用严格相等运算符（===），判断一个位置是否有值。所以，只有当一个数组成员严格等于undefined，默认值才会生效。
````
let [x = 1] = [undefined];
x // 1

let [x = 1] = [null];
x // null
上面代码中，如果一个数组成员是null，默认值就不会生效，因为null不严格等于undefined。
````
如果默认值是一个表达式，那么这个表达式是惰性求值的，即只有在用到的时候，才会求值。
````
function f() {
  console.log('aaa');
}

let [x = f()] = [1];//console.log(x)==1
上面代码中，因为x能取到值，所以函数f根本不会执行。上面的代码其实等价于下面的代码。
````
## 2.对象的解构赋值
1.对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值
````
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined
````
## 3.字符串的解构赋值
````
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
````
类似数组的对象都有一个length属性，因此还可以对这个属性解构赋值。
````
let {length : len} = 'hello';
len // 5
````
## 4.数字和布尔值的解构赋值
解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。
````
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true
````
上面代码中，数值和布尔值的包装对象都有toString属性，因此变量s都能取到值。

解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。
````
let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError
````
## 5.函数参数的解构赋值
````
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
````
