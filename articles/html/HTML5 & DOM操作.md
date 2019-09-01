### 什么是HTML5
HTML5并不仅仅只是作为HTML标记语言的一个最新版本，更重要的是它**制定了Web应用开发的一系列标准**，成为第一个将Web作为应用开发平台的HTML语言。


### 语义化标签
#### 语义化的作用
- 能够便于开发者阅读和写出更优雅的代码。
- 同时让浏览器或者网络爬虫可以很好地解析，从而更好分析其中的内容。
- 更好地搜索引擎优化。

### H5中常用的新语义标签
-  `<nav>`表示导航
-  `<header>`表示页眉
-  `<footer>`表示页脚
-  `<section>`表示区块
-  `<article>`表示文章。如文章、评论、帖子、博客
-  `<aside>`表示侧边栏如文章的侧栏
-  `<figure>`表示媒介内容分组
-  `<mark>`表示标记
-  `<progress>`表示进度
-  `<time>`表示日期


### H5新增表单类型
- `email`只能输入email格式。自动带有验证功能。
- `tel`手机号码。
- `url`只能输入url格式
- `number`只能输入数字
- `search`搜索框
- `range`滑动条
- `color`拾色器
- `time`时间
- `date`日期
- `--datetime`时间日期
- `month`月份
- `week`星期

### DOM操作
#### 获取元素
- `document.getElementById`：
根据ID查找元素，大小写敏感，如果有多个结果，只返回第一个
- `document.getElementByClassName`：根据类名查找元素，多个类名用空格隔开返回一个HTMLCollection
- `document.getElementByTagName`:根据标签查找元素，*表示查询所有标签，返回一个HTMLCollection。
- `document.getElementByName`:根据元素的name属性查找，返回一个NodeList
- `document.querySelector('selector')` ：通过CSS选择器获取符合条件的第一个元素。
- `document.querySelectorAll('selector')`：通过CSS选择器获取符合条件的所有元素，以类数组形式存在。

#### 节点创建
- `createElement` 创建元素
- `createTextNode` 创建文本节点
- `cloneNode` 克隆一个节点
    - `node.cloneNode(true/fals)`，它接受一个bool参数，用来表示是否复制子元素
- `createDocumentFragment` 本方法用来创建一个DocumentFragment,也就是文档碎片，它表示一种轻量级的文档，主要用来存储临时节点，大量操作DOM时用它可以大大提升性能。


#### 节点修改API
- `appendChild`
    - 语法
      `parent.appendChild(child)`

- `insertBefore`
    - 语法
      `parentNode.insertBefore(newNode,refNode)`
- `removeChild`
用于删除指定的子节点并返回子节点
    - 语法
     `var deleteChild = parent.removeChild(node)`
- `replaceChild`
用于将一个节点替换另一个节点
    - 语法
    `parent.replaceChild(newChild,oldChild)`

#### 节点关系API
- 父关系API
    - `parentNode` 
    每个节点都有一个parentNode属性，它表示元素的父节点。
    - `parentElement`返回元素的父元素节点，与parentNode的区别在于，其父节点必须是一个Element元素，如果不是，则返回null

- 子关系API
    - `children`: 返回一个实时的HTMLCollection,子节点都是Element
    - `childNodes`: 返回一个实时的NodeList,表示元素的子节点列表，注意子节点可能包含文本节点、注释节点
    - `firstChild`:返回一个子节点，不存在返回null，与之相对应的还有一个firstElementChild;
    - `lastChild`:返回最后一个子节点，不存在返回null,与之相对应的还有一个lastElementChild;

#### 类名操作
- Node.classList.add("class")添加class
- Node.classList.remove("class")移除class
- Node.classList.toggle("class")切换class,有则移除，无则添加
- Node.classList.contains("class")检测是否存在class