---

title: 学习promise第一天
date: 2018-03-12 14:50:28
categorise: Javascript
tags:
  - Javascript
  - Promise
---

我们先看一个最简单的Promise例子：生成一个0-2之间的随机数，如果小于1，则等待一段时间后返回成功，否则返回失败
````
    function test(resolve, reject) {
        var timeOut = Math.random() * 2;
        setTimeout(function () {
            if (timeOut < 1) {
                resolve('200 OK');
            }
            else {
                reject('timeout in ' + timeOut + ' seconds.');
            }
        }, 1000);
    }
    ````
这个``test()``函数有两个参数，这两个参数都是函数，如果执行成功，我们将调用  ``resolve('200 OK')``函数，如果执行失败，我们将调用函数``reject('timeout in ' + timeOut + ' seconds.');``。可以看出，``test()``函数只关心自身的逻辑，并不关心具体的``resolve``和``reject``将如何处理结果。
有了执行函数，我们就可以用一个Promise对象来执行它，并在将来某个时刻获得成功或失败的结果：
````
    var p1 = new Promise(test);
    var p2 = p1.then(function (result) {
      console.log(result)
        console.log('成功：' + result);
    });

    var p3 = p2.catch(function (reason) {
      console.log(reason)
        console.log('失败：' + reason);
    });
    test()
    ````
变量p1是一个Promise对象，它负责执行``test``函数。由于``test``函数在内部是异步执行的，当``test``函数执行成功时，我们告诉Promise对象：
````
    // 如果成功，执行这个函数：
    p1.then(function (result) {
      console.log('成功：' + result);
    });
    ````
当test函数执行失败时，我们告诉Promise对象：
````
    p2.catch(function (reason) {
      console.log('失败：' + reason);
    });
    ````
Promise对象可以串联起来，所以上述代码可以简化为：
````
    new Promise(test).then(function (result) {
        console.log('成功：' + result);
    }).catch(function (reason) {
        console.log('失败：' + reason);
    });
    ````
````
// 0.5秒后返回input*input的计算结果:
function multiply(input) {
    return new Promise(function (resolve, reject) {
        log('calculating ' + input + ' x ' + input + '...');
        setTimeout(resolve, 500, input * input);//定时器启动的时候，第三个参数是作为第一个func()的参数传进去的
    });
}

// 0.5秒后返回input+input的计算结果:
function add(input) {
    return new Promise(function (resolve, reject) {
        log('calculating ' + input + ' + ' + input + '...');
        setTimeout(resolve, 500, input + input);
    });
}

var p = new Promise(function (resolve, reject) {
    log('start new Promise...');
    resolve(123);
});

p.then(multiply)
 .then(add)
 .then(multiply)
 .then(add)
 .then(function (result) {
    log('Got value: ' + result);
});

    定时器启动的时候，第三个参数是作为第一个func()的参数传进去的
````
## 定时器启动的时候，第三个参数是作为第一个func()的参数传进去的
<!-- more -->
[promise廖雪峰](https://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/0014345008539155e93fc16046d4bb7854943814c4f9dc2000?_blank)
[大白话讲解promise](https://www.cnblogs.com/lvdabao/p/es6-promise-1.html)
