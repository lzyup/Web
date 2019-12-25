### 简介
`window.history`是一个只读属性，用来获取`History`对象的引用，`History`对象提供了操作浏览器会话历史（浏览器地址栏中访问的页面，以及当前页面中通过框架加载的页面）的接口。**同时，从`HTML5`开始，提供了对history栈中内容的操作**

### pushState
**pushState**:用来新增历史记录

`pushState()`需要三个参数：一个状态对象，一个标题（目前被忽略），和（可选的）一个URL.
- **状态对象**</br>
状态对象state是一个JavaScript对象，通过pushState()创建新的历史记录条目。无论什么时候用户导航到新的状态，popState事件就会被触发，且该事件的state属性包含该历史记录条目状态对象的副本。

- 标题</br>
Firefox目前忽略这个参数，但未来可能会用到。在此处传一个空字符串应该可以安全的防范未来这个方法的更改。或者，你可以为跳转的state传递一个短标题。

- URL</br>
改参数定义了新的历史URL记录。注意，调用`pushState()`后浏览器并不会立即加载这个URL,但可能会在稍后某些情况下加载这些这个URL,比如在用户重新打开浏览器时。新URL不必须为绝对路径。如果新URL是相对路径，那么它将被作为相对于当前URL处理。新URL必须与当前URL同源，否则`pushState()`会抛出一个异常。改参数是可选的，缺省为当前URL。

### replaceState
`history.replaceState()`的使用与`history.pushState()`非常相似，区别在于`replaceState()`是修改了当前的历史记录项而不是新建一个。注意这并不会阻止其在全局浏览器历史记录中创建一个新的历史记录项。

### popstate事件
触发历史记录改变的事件，比如用户前进、后退等操作会触发popstate事件。进而获取到通过pushState和replaceState传递的值

```
//监听state事件
window.addEventListener('popstate',function(event){
    console.log(event.state);
})
```

### history.back回退
回退到上一个history记录

### history.forward前进
前进一个history记录

### history.go去往第n条记录
去往第几个history记录

### history.length长度
获取浏览history记录的长度

### history.state当前地址
获取当前的记录

