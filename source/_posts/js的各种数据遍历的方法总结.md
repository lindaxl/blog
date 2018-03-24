---
title: js的各种数据遍历的方法总结
date: 2017-03-20 10:31:37
categories: Javascript
tags:
- js遍历
---
JS数组遍历，基本就是for,forin,foreach,forof,map等等一些方法，以下介绍几种本文分析用到的数组遍历方式以及进行性能分析对比
## for循环
1.第一种:普通for循环
````
for(j = 0; j < arr.length; j++) {

}
````
简要说明: 最简单的一种，也是使用频率最高的一种，虽然性能不弱，但仍有优化空间
2.第二种:优化版for循环
````
for(j = 0,len=arr.length; j < len; j++) {

}
````
简要说明: 使用临时变量，将长度缓存起来，避免重复获取数组长度，当数组较大时优化效果才会比较明显。
3.第三种:弱化版for循环
````
for(j = 0; arr[j]!=null; j++) {

}
````
简要说明: 这种方法其实严格上也属于for循环，只不过是没有使用length判断，而使用变量本身判断

实际上，这种方法的性能要远远小于普通for循环

## forEach循环
````
arr.forEach(function(e){  

});
````
简要说明: 数组自带的foreach循环，使用频率较高，实际上性能比普通for循环弱
第五种:foreach变种
````
Array.prototype.forEach.call(arr,function(el){  

});
````
简要说明: 由于foreach是Array型自带的，对于一些非这种类型的，无法直接使用(如``NodeList``)，所以才有了这个变种，使用这个变种可以让类似的数组拥有foreach功能。
实际性能要比普通foreach弱
## for in循环
````
for(j in arr) {

}
````
简要说明: 这个循环很多人爱用，但实际上，经分析测试，在众多的循环遍历方式中

它的效率是最低的
## map遍历
````
arr.map(function(n){  

});
````
简要说明: 这种方式也是用的比较广泛的，虽然用起来比较优雅，但实际效率还比不上foreach
## for of遍历(需要ES6支持)
````
for(let value of arr) {  

});
````
简要说明: 这种方式是es6里面用到的，性能要好于forin，但仍然比不上普通for循环

>1. javascript遍历的常用的遍历方法是for循环和for-in，ES5的时候加上了forEach方法（IE9以下不支持）。

````
js原生遍历
//for循环遍历数组
for(var i=0;i<arrTmp.length;i++){
   console.log(i+": "+arrTmp[i])
}

//for-in遍历对象属性,i指代属性名
for(var i in objTmp){
   console.log(i+": "+objTmp[i])
}

//forEach遍历数组，三个参数依次是数组元素、索引、数组本身
arrTmp.forEach(function(value,index,array){
   console.log(value+","+index+","+array[index])
})
````

>2. for-in循环是为了遍历对象而设计的，事实上for-in也能用来遍历数组，但定义的索引i是字符串类型的。如果数组具有一个可枚举的方法，也会被for-in遍历到，例如：
````
//for-in遍历数组
for(var i in arrTmp){
   console.log(i+": "+arrTmp[i])
}
//for-in会遍历到数组的属性
arrTmp.name="myTest";
for(var i in arrTmp){
   console.log(i+":"+arrTmp[i])
}
//输出 0:value1  1:value2  2:value3  name:myTest
````
3. for循环和for-in能正确响应break、continue和return语句，但forEach不行。
````
//只会输出value1 value2
for(var i=0;i<arrTmp.length;i++){
  console.log(i+": "+arrTmp[i]);
  if(i==1){
      break;
  }
}

//会输出value1 value2 value3
arrTmp.forEach(function(value){
  console.log(value+);
  if(value==1){
      return;
  }
})
````

4. ES6中，新增了for-of遍历方法。它被设计用来遍历各种类数组集合，例如DOM NodeList对象、Map和Set对象，甚至字符串也行。官方的说法是：

for...of语句在可迭代对象(包括 Array, Map, Set, String, TypedArray，arguments 对象等等)上创建一个迭代循环，对每个不同属性的属性值,调用一个自定义的有执行语句的迭代挂钩
````
// for-of遍历数组，不带索引，i即为数组元素
for(let i of arrTmp){
    console.log(i)
}
//输出 "value1" "value2" "value3"

// for-of遍历Map对象
let iterable = new Map([["a", 1], ["b", 2], ["c", 3]]);
for (let [key, value] of iterable) {
  console.log(value);
}
//输出 1 2 3

// for-of遍历字符串
let iterable = "china中国";
for (let value of iterable) {
  console.log(value);
}
//输出 "c" "h" "i" "n" "a" "中" "国"
````
5. 上面的方法，注重点都是数组的元素或者对象的属性值。如果单纯的想获取对象的属性名，js有原生的Object.keys()方法（低版本IE不兼容）,返回一个由对象的可枚举属性名组成的数组：
````
/****Object.keys()返回键名数组****/
//数组类型
let arr = ["a", "b", "c"];
console.log(Object.keys(arr));
// (3) ['0', '1', '2']

// 类数组对象
let anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj));
// (3) ['2', '7', '100']

//一般对象
let xyz = {z: "zzz", x: "xxx", y: "yyy"};
console.log(Object.keys(xyz));
// (3) ["z", "x", "y"]
````
javascript原生遍历方法的建议用法：
用for循环遍历数组
用for-in遍历对象
用for-of遍历类数组对象（ES6）
用Object.keys()获取对象属性名的集合

>for … of循环和for … in循环有何区别
for … in循环，它遍历的实际上是对象的属性名称。
一个Array数组实际上也是一个对象，它的每个元素的索引被视为一个属性。
当我们手动给Array对象添加了额外的属性后，for … in循环将带来意想不到的意外效果：
for in 遍历数组时会为把数组索引作为键值　如：数组0、1、2、3、4、5、…的键；当我们这样写：
````
var arr=[1,2,3,4,5,6];
arr.value='val';
//在使用for in 遍历时
for(var i in arr){
    console.log(i+'   '+arr[i]);//这时的i为键值，不为数组索引
}
//输出
0   1
1   2
2   3
3   4
4   5
5   6
value   val
接着执行，这时出现问题：

arr;//输出[1, 2, 3, 4, 5, 6]

//使用 console.log(arr)时，这样
console.log(arr);//输出[1, 2, 3, 4, 5, 6, value: "val"]

//alert(arr)时
alert(arr);//输出[1, 2, 3, 4, 5, 6]
````
而当我们使用for of 时：
````
var arr=[1,2,3,4,5,6];
arr.value='val';
//在使用for in 遍历时
for(var v of arr){
    console.log(v);//v为数组的项
}
//输出
1
2
3
4
5
6
直接遍历出值，杜绝使用for in 时，下标索引的影响
````

>jQuery的$.each
    jQuery的遍历方法通常被用来遍历DOM元素，用于数组和对象的是$.each()方法，它接受三个参数，分别指代数组索引/元素/数组本身(跟forEach相比,第1个和第2个参数正好是相反的，不要记错了。)：
/****$.each()遍历对象和数组****/
````
$.each(arrTmp,function(index,value,array){
    console.log(index+": "+value)
});
$.each(objTmp,function(key,value){
    console.log(key+": "+value)
});
````
>map方法
这里的map不是“地图”的意思，而是指“映射”。[].map(); 基本用法跟forEach方法类似：
array.map(callback,[ thisObject]);
callback的参数也类似：
````
[].map(function(value, index, array) {
  // ...
});
````
map方法的作用不难理解，“映射”嘛，也就是原数组被“映射”成对应新数组。下面这个例子是数值项求平方：
````
var data=[1,3,4]
var Squares=data.map(function(val,index,arr){
  console.log(arr[index]==val);  // ==> true
  return val*val           
})
console.log(Squares);        // ==> [1, 9, 16]
````
