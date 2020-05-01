 一般React技术栈的项目主要用到几个相关的库，这里总结一下。

### React
- 界面都是由一个个组件拼接而成的组件
- JSX语法书写组件
- 组件的更新依赖于`setState`方法

### Redux
面对复杂系统，一个组件内部状态的改变可能引起其它某个组件甚至多个组件的改变，需要用Redux将state管理起来，方便维护。

#### 核心概念
- Action
- Reducer
- Store

#### 工作流（数据流）
通过调用`store.dispatch`派发一个`action`，从而触发`reducer`，继而改变整个状态树(state tree)。整个
流程是单向数据流。

#### 注意点
在`Reducer`中需要小心，不要修改`State`，使用比较hack的方法是`Object.assign()`返回一个副本，也可以结合`Immutable`来处理。
- `Redux`的`combineReducers`方法是浅比较它调用的`reducer`的引用是否发生变化
- `react-redux`的`connect`方法生成的组件（容器组件）也是通过浅比较根state的引用变化与`mapStateToProps`函数的返回值，来判断是否更新组件。
    - 浅比较</br>
       只检查两个不同变量是否为同一个对象引用
    - 深比较</br>
        必须检查两个对象所有属性的值是否相等

    
### react-redux
本来React和Redux本身是没有什么关系的，通过`react-redux`将二者联系起来

#### 核心API
- Provider</br>
提供`store`给`connect`使用

- `connect([mapStateToProps],[mapDispatchToProps],[megeProps],[options])`</br>
连接React组件与Redux store</br>
连接操作不会改变原来的组件类</br>
反而返回一个新的已与`Redux store`连接的组件类



#### `connect([mapStateToProps],[mapDispatchToProps],[megeProps],[options])`

- mapStateToProps</br>
建立state对象到组件props对象的映射关系

- mapDispatchToProps</br>
将dispatch(actionCreator)映射到props对象

这样就不用手动调用`setState`更新组件状态了，`react-redux`帮助我们更新组件且性能更优

### `Middleware`:
- 主要用来包装`store`的`dispatch`方法（即自定义了一个dispatch）
- 每个`middleware`接收`store`的`dispatch`和`getState`返回的是一个函数
- 这个函数的参数是下一个`middleware`的`dispatch`,称为`next`
- 中间件完整的函数是:</br>
 `store => next => action`
- 通过`applyMiddle`方法，将这些中间件(middleware)串联起来，原始的`dispatch`方法会最后执行。
- 中间件的整体工作流程是属于洋葱模型