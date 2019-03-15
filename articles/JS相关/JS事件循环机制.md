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

## 2、 事件和回调函数
所谓函调函数(callback)，就是那些会被主线程挂起来的代码。**异步任务必须制定回调函数**，当主线程开始执行异步任务，就是执行对应的回调函数。

`任务队列`是一个先进先出的数据结构，排在前面的事件，优先被主线程读取。主线程的读取过程基本上是自动的，只要执行栈一清空，`任务队列`上第一位的事件就自动进入主线程。但是，由于存在`定时器`功能，主线程首先要检查一下执行时间，某些事件只有到了规定的时间，才能返回主线程。

### Macrotask & Microtask
Macrotask也就是Task和microtask是异步任务的两种分类。在挂起任务时，JS引擎会将所有异步任务按照类别分到这两个队列，首先macrotask的队列（这个队列被叫做task queue）中取出第一个任务，执行完毕后取出microtask队列中的所有任务顺序执行，之后再取macrotask任务，周而复始，直至两个队列的任务取完。

- macro-task: script(整体代码)，setTimeout,setInterval,setImmediate,I/O

- micro-task: process.nextTick,Promise（这里指浏览器实现的原生Promise）,Object.observe,MutationObserver

### 实战分析
```
console.log('script start');  
setTimeout(function()  {  console.log('setTimeout');  },  0);  Promise.resolve().then(function()  {  console.log('promise1');  })
.then(function()  {  console.log('promise2');  }); 
 console.log('script end');
```
运行结果：
script start
script end
promise1
promise2
setTimeout

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

## 总结
`全部代码(script) macrotask -> microtask queue (含有promise.then) -> macrotask(setTimeout) -> 下一个microtask`

## 事件循环机制每次从macrotask队列中取一个任务执行，然后将microtask队列中的所有任务执行完，在去执行macrotask中的下一个任务。


## 事件循环可以分为以下一个过程：`宏任务------->执行栈-------->微任务`






