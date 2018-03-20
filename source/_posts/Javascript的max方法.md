---
title: Javascript的max方法
date: 2018-03-19 10:45:20
categories: Javascript
tags:
- Javascript
- Math.max()
---
var a = [1,4,5,2,9];
求最大值
console.log(Math.max.apply(null,a))    //9 这是使用apply的方法
console.log(Math.max(...a))            //9 这是ES6语法
## Math对象包含max()方法，用于确认一组数值中的最大值。该方法接收任意多个数值参数，不接受数组参数。
要找到数组中的最值，可以使用apply()方法，D表示将Math.max()方法的执行环境切换到null上，apply()方法接收两个参数，第二个参数是一个数组。
