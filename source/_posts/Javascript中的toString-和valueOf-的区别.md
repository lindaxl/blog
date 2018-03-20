---
title: Javascript中的toString()和valueOf()的区别
date: 2018-03-20 08:28:56
categories: Javascript
tags:
- Javascript
- toString()
- valueOf()
---

# toString：返回对象的字符串表示形式。
语法：objectname.toString([radix])
objectname要为其搜索字符串表示形式的对象。  
radix可选，为将数字值转换为字符串指定一个基数。  此值仅用于数字。
toString 方法是一个所有内置的 JavaScript 对象的成员。
它的行为取决于对象的类型：
<img src="https://gss0.baidu.com/7Po3dSag_xI4khGko9WTAnF6hhy/zhidao/wh%3D600%2C800/sign=b8fe745c7ec6a7efb973a020cdca8369/6a600c338744ebf845a3b5bcd1f9d72a6159a767.jpg"/>

# valueOf：返回指定对象的基元值。
语法：object.valueOf( )
object 引用是任何内部 JavaScript 对象，将通过不同的方式为每个内部 JavaScript 对象定义 valueOf 方法。Math 和 Error 对象都没有 valueOf 方法。
<img src="https://gss0.baidu.com/7Po3dSag_xI4khGko9WTAnF6hhy/zhidao/wh%3D600%2C800/sign=ba1d04a298ef76c6d087f32dad26d1c2/f7246b600c338744b438f9ff590fd9f9d62aa0b0.jpg"/>

## 总结：toString主要是把对象转换为字符串，而valueOf主要把对象转换成一个基本数据的值这就是他们之间最基本的区别。
