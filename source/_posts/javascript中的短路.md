---
title: javascript中的短路
date: 2018-03-13 17:18:15
categories:
- 前端
tags:
- Javascript
- 短路
---
# 短路原理

即通过最短路径达到目的（不需要把所有条件都执行和判断）。
javascript中的 &&（逻辑与） 和 || （逻辑或）都遵循短路原理。
````
var a=1,b=1,c=2;  
console.log(a==b&&c);   //2  
console.log(a==0&&c);   //false  
````
在逻辑与（&&）中，表达式中有条件为false的表达式，返回第一个条件为false的表达式的值,就是短路。没有则返回最后哦一个表达式的值。
````
console.log(a==b||c);     //true  
console.log(a==0||c);     //2;
````
在逻辑或（||）中，表达式中有条件为true的表达式，返回第一个条件为true的表达式的值，就是短路。没有则返回最后一个表达式的值
