Promise之我见
===
## 1、Promise的含义
- Promise是异步编程的一种解决方案，用来解决函数的嵌套，也就是传说中的回调地狱
- Promise对象具有状态，分为两种状态变化
	- 从`pending`到`fulfilled`
	- 从`pending`到`rejected`
这两种状态的变化任一个发生就定型了不会再发生改变。

## 2、Promise的基本用法
```
let p = new Promise((resolve, reject) => {
  // 做一些事情
  // 然后在某些条件下resolve，或者reject
  if (/* 条件随便写^_^ */) {
    resolve()
  } else {
    reject()
  }
})

p.then(() => {
    // 如果p的状态被resolve了，就进入这里
}, () => {
    // 如果p的状态被reject
})
```
这里解释一下，Promise构造函数接受一个函数作为参数，resolve和reject是这个函数的两个参数。
- `resolve`函数的作用就是将Promise的状态从`pending`到`fulfilled`
- `reject`函数的作用就是将Promise的状态 从`pending`到`rejected`
- `then`方法是Promise状态发生改变的回调函数，该方法的第一个参数是`resolve`处理后状态发生变化的回调函数，第二参数是`reject`处理后状态发生变化的回调函数，需要强调的是 **`then`方法返回的是一个新的`Promise`实例，不是原来那个`Promise`，所以可以采用链式的写法来避免回调地狱。**

## 3、Promise链式调用
- 案例分析一
```
下面分别是三个Promise
function testPromise1(){
	let p = new Promise(function(resolve,reject){
	setTimeout(function(){
		console.log("第一个Promise");
		resolve("数据111");
},1000)
});
return p;
}

function testPromise2(){
	let p = new Promise(function(resolve,reject){
	setTimeout(function(){
		console.log("第二个Promise");
		resolve("数据222");
},1000)
});
return p;
}

function testPromise3(){
	let p = new Promise(function(resolve,reject){
	setTimeout(function(){
		console.log("第三个Promise");
		resolve("数据333");
},1000)
});
return p;
}


接着链式调用
testPromise1()
	.then(function(data){
		console.log(data);
		return testPromise2();
})
.then(function(data){
	console.log(data);
	return testPromise3();
})
.then(function(data){
	console.log(data);
})

输出的结果为：
	第一个Promise
	数据111
	第二个Promise
	数据222
	第三个Promise
	数据333
```
不难发现，在每个then方法中都返回了一个新的`Promise`,通过调用新的`Promise`的`then`方法从而实现链式调用。

- 案例分析二
```
Promise.resolve()
  .then(() => {
    return new Error('error!!!')
  })
  .then((res) => {
    console.log('then: ', res)
  })
  .catch((err) => {
    console.log('catch: ', err)
  })

输出的结果为
``
then: Error: error!!!
    at Promise.resolve.then (...)
    at ...
```
- 这里指的关注的第一点是，`then`方法中`return`一个`error`对象并不会抛出错误，所以不会被后续的.catch捕获，需要改成下面中的一种
	- `return Promise.reject(new Error('error!!!'))`
	- `throw new Error('error!!!')`
- 在`then`方法中返回任意一个非`promise`的值都会被包裹为`Promise`对象，即`return new Error('error!!!')`等价于`return Promise.resolve(new Error('error!!!'))`

## 4、 Promise.then
- `Promise`实例具有`then`方法。
- `then`方法有两个参数，第一个参数是`resolve`状态的回到函数，第二参数（可选）是`reject`状态的回调函数，看到这里有没有觉得这个`then`方法的参数很眼熟，对，你猜的没错，这不是跟`Promise`的构造函数的参数一样嘛，所以就有下面这一点。
- `then`方法返回的是一个新的`Promise`实例（不是原来那个`Promise`实例了）。
## 5 、Promise.catch
- `Promise.catch`方法是`.then(null,rejection)`的别名，用于指定发生错误时的回调函数。
-

```javascript
// bad
promise
  .then(function(data) {
    // success
  }, function(err) {
    // error
  });

// good
promise
  .then(function(data) { //cb
    // success
  })
  .catch(function(err) {
    // error
  });
```
一般来说，第二种写法是好于第一种写法，理由是第二种写法可以捕获前面`then`方法执行中的错误，也更接近同步的写法（`try/catch`）。因此，建议总是使用`catch`方法，而不使用`then`方法的第二个参数。用`Promise`链式调用的时候也只用在最后加一个`catch`用来捕获错误即可。


## 6 、Promise.all的使用
`Promise.all`方法接受一个数组作为参数，包装成一个新的`Promise`实例。
例如
```
Promise
.all([testPromise1(),testPromise2(),testPromise3()])
.then(function(results){
})
```
- 这里只有`testPromise1()` 、`testPromise2()`和`testPromise3()`三个Promise的状态都从`pending`到`fulfilled`那么`Promise.all`的状态才会从`pending`到`fulfilled`
- 这里只要`testPromise1()` 、`testPromise2()`和`testPromise3()`中有一个状态变为`reject`那么`Promise.all`就会变成`reject`状态
- 适用的场景主要是，可能你一个网络请求需要的一些参数依赖于不用的异步任务获取，等这些异步任务都完成，你拿到全部参数才能发这个网络请求。

## 7、 Promise.race的用法
- 跟`Promise.all`的区别在于，接受的数组参数里面，只要有一个`Promise`状态发生的变化，就能在`Promise.all`的`then`方法里面执行回调函数。


