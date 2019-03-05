前几天做了好几套笔试题，发现自己对Promise这部分的内容还是不太理解，所以整理下

简介
---
Promise是一个对象，用来传递异步操作的消息。

状态
---
* pending
* Resolve（Fulfilled）
* Rejected

特点
---
* 对象的状态不受外界影响，只有异步操作可以决定当前是哪一种状态
* 一旦状态改变就不会再变，任何时候都可以得到这个结果，两种状态改变：pending→Resolved，pending→Rejected

基本用法
--
先new一个实例 （摘自阮一峰老师的《ES 6标准入门》）
```
    Promis构造函数接受一个函数作为参数，这个函数有两个参数，分别是resolve()和reject()。

    resovle()函数是将Promise对象从pending变成fulfilled，在异步操作完成时执行，将异步结果，作为参数传递出去。

    reject()函数是将Promise对象从pending变成rejected，在异步执行失败时执行，将报错信息，作为参数传递出去。

    // 简单的一个promise实例
    const promise = new Promise((resolve, reject) => {
        // some code 

      if(/* 异步执行成功 */) {
        resolve(value);
      } else {
        reject(error);
      }
    })
```
then方法
---
Promise 有个.then()方法，then 方法中的回调在微任务队列中执行，支持传入两个参数，一个是成功的回调，一个是失败的回调，在 Promise 中调用了 resolve 方法，就会在 then 中执行成功的回调，调用了 reject 方法，就会在 then 中执行失败的回调，成功的回调和失败的回调只能执行一个，resolve 和 reject 方法调用时传入的参数会传递给 then 方法中对应的回调函数。
```
   // 执行 resolve  
    var promise = new Promise((resolve, reject) => {
      console.log(1);
      resolve(3);
    })

    console.log(2);

    promise.then((data)=>{
      console.log(data);
    }, (err)=>{
      console.log(err);
    })
    
    // 1
    // 2
    // 3
```
敲重点，then方法中的回调是异步的

解释该程序运行结果：Promise中的函数是立即执行的，输出1，然后执行resolve（），传到then，而then是异步的，所以进入微队列中等待下一轮执行（具体执行参照js事件机制），继续往下走，输出2，后面没有东西了，再去执行微队列中的任务，resolve表示成功的状态，所以输出3

```
   // 执行 reject  
    var promise = new Promise((resolve, reject) => {
        console.log(1);
        reject();
    })

    promise.then(() => {
      console.log(2);
    }, () => {
      console.log(3);
    })

    // 1
    // 3
``` 
catch方法
---
catch方法是.then(null,rejection)的别名，用于指定发生错误时的回调函数。

将上面执行reject的代码改下：
```
  // 执行 reject  
    var promise = new Promise((resolve, reject) => {
        console.log(1);
        reject();
    })

    promise.then(() => {
      console.log(2);
    })
    .catch(() => {
      console.log(3);
    })
    
    // 1
    // 3
```
运行效果一样，一般来说，我们尽量使用catch方法来指定错误处理的回调函数，因为使用then的话，Promise对象抛出的错误不会传递到外层代码，即导致运行后没有任何输出，也发现不了错误。

链式调用
---
由于promise每次调用then方法就会返回一个新的promise对象，如果该then方法中执行的回调函数有返回值，那么这个返回值就会作为下一个promise实例的then方法回调的参数，如果 then 方法的返回值是一个 Promise 实例，那就返回一个新的 Promise 实例，将 then 返回的 Promise 实例执行后的结果作为返回 Promise 实例回调的参数。

来看个例子
```
   var promise = new Promise((resolve, reject) => {
      resolve('hello');
    });
   promise.then(data => {
      console.log('第一次: ', data);
      return Promise.resolve(1); 
    })
    .then(data => {
      console.log('第二次: ', data);
      return Promise.reject(2); 
    })
    .catch(err => {
      console.log('失败: ', err);
    });
    
    // 第一次：hello
    // 第二次：2
    // 失败：2
```

由于Promise可以链式调用，就避免了回调地狱的发生

就写到这吧，以后再来更，错误之处，请大家指正

参照博客
---
https://github.com/PDKSophia/blog.io
