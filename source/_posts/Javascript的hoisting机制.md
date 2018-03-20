---
title: Javascript的hoisting机制
date: 2018-03-19 09:43:24
categories: Javascript
tags:
- Javascript
- hoisting机制
---
````
var foo = {n:1};
(function(foo){
  console.log(foo.n);
  foo.n = 3;
  var foo = {n: 2};
  console.log(foo.n);
})(foo);
console.log(foo.n)//最后结果是1,2，3

等同于
var foo = {n:1};
(function(foo){               //形参foo同实参foo一样指向同一片内存空间，这个空间里的n的值是1
  var foo ;                   //优先级低于实参，无效
  console.log(foo.n);         //输出为1
  foo.n =3;                   //形参于实参foo指向的内存空间里的n值被改为3
  foo = {n:2};                //形参foo指向了新的内存空间，里面的值为2
  console.log(foo.n);         //输出新的内存空间的n的值为2
})(foo)
console.log(foo.n)            //实参foo的指向还是原来的内存空间，里面的值为3.
````

````
参数是引用参数
var foo={n:1};
(function (foo) {
    console.log(foo.n);
    foo.n=3;
    var foo={n:2};
    console.log(foo.n);
})(foo);
console.log(foo.n);
结果: 1 2 3

参数是传值参数
var foo=1;
(function (foo) {
    console.log(foo);
    foo=3;
    var foo=2;
    console.log(foo);
})(foo);
console.log(foo);
结果:1 2 1
````
