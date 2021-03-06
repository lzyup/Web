JS事件循环机制
===

## 1、同步任务和异步任务
### 同步任务

同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务

### 异步任务
异步任务指的是，不进入主线程、而进入”任务队列（task queue）“的任务，只有”任务队列“通知主线程，某个异步任务可以执行了，改任务才会进入主线程执行

### 异步任务的运行机制如下
 - 所有同步任务都在主线程上执行，形成了一个执行栈
- 主线程外，还存在一个`任务队列`。只要异步任务有了运行结果，就在`任务队列`之中放置一个事件
- 一旦`执行栈`中的所有同步任务执行完毕，系统就会读取`任务队列`，看看里面有哪些事件。哪些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
- 主线程不断重复上面的第三步

## 2、事件循环 event-loop

主线程从“任务队列”中读取事件，这个过程是循环不断的，所以整个的这种运行机制称为`Event Loop（事件循环）`。

此机制具体如下：
- 执行一个宏任务（栈中没有就从事件队列中获取）
- 执行过程中如果遇到微任务，就将它添加到微任务的任务队列中
- 宏任务执行完毕后，立即执行当前微任务队列中的所有微任务（依次执行）
- 当前宏任务执行完毕，开始检查渲染，然后GUI线程接管渲染
- 渲染完毕后，JS线程继续接管，开始下一个宏任务

## 3、 事件和回调函数
所谓函调函数(callback)，就是那些会被主线程挂起来的代码。**异步任务必须制定回调函数**，当主线程开始执行异步任务，就是执行对应的回调函数。

`任务队列`是一个先进先出的数据结构，排在前面的事件，优先被主线程读取。主线程的读取过程基本上是自动的，只要执行栈一清空，`任务队列`上第一位的事件就自动进入主线程。但是，由于存在`定时器`功能，主线程首先要检查一下执行时间，某些事件只有到了规定的时间，才能返回主线程。

## 4、 Macrotask & Microtask
Macrotask:`可以理解是每次执行栈执行的代码就是一个宏任务（包括每从事件队列中获取一个事件回调并放到执行栈中执行）`

Microtask:`可以理解是在当前task执行结束后立即执行的任务。也就是说，在当前task任务后，下一个task之前，在渲染之前。`
- macro-task: script(整体代码)，setTimeout,setInterval,setImmediate,I/O

- micro-task: process.nextTick,Promise（这里指浏览器实现的原生Promise）,Object.observe,MutationObserver

在挂起任务时，JS引擎会将所有异步任务按照类别分到这两个队列，首先macrotask的队列（这个队列被叫做task queue）中取出第一个任务，执行完毕后取出microtask队列中的所有任务顺序执行，之后再取macrotask任务，周而复始，直至两个队列的任务取完。




### 实战分析
## eg1：
```
console.log('script start');  
setTimeout(function()  { 
console.log('setTimeout');  },  0);
Promise.resolve().then(function() {
console.log('promise1');  })
.then(function(){
console.log('promise2');  }); 
 console.log('script end');
```
运行结果：

- script start
- script end
- promise1
- promise2
- setTimeout

其中`then`函数分发到了`microtask`队列，setTimeout函数分发到了`macrotask`队列，二者都属于异步任务。
执行流程如下
- 这段代码作为宏任务，进入主线程
- 先遇到`console.log('script start')`，立即执行
- 接下来分别遇到`setTimeout`和`then`方法
- 分别将`setTimeout`的回调函数压入`Macrotask`队列，`then`方法的回调压入`Microtask`队列
- 然后` console.log('script end')`被立即执行
- 接着取出`Microtask`队列中的任务执行输出`console.log('promise1')`
- 然后又将`then`方法的回调压入`microtask`队列中，并执行`console.log('promise1')`
- 接着执行`Macrotask`队列中的`console.log('setTimeout')`

## eg2:
```
async function async1() {
    console.log('async1 start');
    await async2();
    console.log('async1 end');
}
async function async2() {
	console.log('async2');
}

console.log('script start');

setTimeout(function() {
    console.log('setTimeout');
}, 0)

async1();

new Promise(function(resolve) {
    console.log('promise1');
    resolve();
}).then(function() {
    console.log('promise2');
});
console.log('script end');
```
## 注：
`async`函数完全可以看做多个异步操作，包装成一个Promise对象，而`await`命令就是内部`then`的语法糖。

**正常情况下，`await`命令后面是一个`Promise`对象，返回该对象的结果。如果不是`Promise`对象，就直接返回对应的值**

`await`后面的代码是`microtask`

`async`函数返回一个`Promise`对象

运行结果：

- script start
- async1 start
- async2
- promise1
- script end
- async1 end
- promise2
- setTimeout



## 总结
`全部代码(script) macrotask -> microtask queue (含有promise.then) -> macrotask(setTimeout) -> 下一个microtask`

***要做到心中有队列，有先进先出的概念***
![image](https://user-gold-cdn.xitu.io/2019/3/22/169a4038c4e156f0?imageslim)





