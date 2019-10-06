### 什么是Node.js?
- Node.js是一个基于Chrome V8引擎的JavaScript运行环境
- Node.js使用了一个事件驱动、非阻塞式I/O的模型，使其轻量又高效


### BFF层 
Backend for Frontend(中间渲染层)


### Node.js的非阻塞I/O
- I/O即Input/Output,一个系统的输入和输出
- 阻塞I/O和非阻塞I/O的区别就在于**系统接收输入再到输出期间，能不能接收其他输入**


### Node.js异步编程 - callback
- 回调函数格式规范
  - error-first callbak
  - Node-style callback
- 第一个参数是error,后面的参数才是结果