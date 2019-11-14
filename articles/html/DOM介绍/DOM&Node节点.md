### DOM

#### 什么是DOM
Document Object Model,文档对象模型。DOM为文档提供了结构化表示，并定义了如何通过脚本来访问文档结构。目的其实就是为了能让js操作html元素而制定的一个规范。</br>
DOM就是由节点组成

### 节点
Node:构成HTML网页的最基本单元。网页中的每一个部分都可以称为是一个节点，比如：html标签、属性、文本、注释、整个文档等都是一个节点</br>
虽然是节点，但是实际上他们的具体类型是不同的。常见的节点类型分为四类：
- 文档节点（文档）</br>
    整个HTML文档。整个HTML文档就是一个文档节点
- 元素节点（标签）</br>
    HTML标签
- 属性节点（属性）</br>
    元素的属性,比如:id,name之类
- 文本节点（文本）</br>
    HTML标签中的文本内容（**包括标签之间的空格、换行**</br>

节点的类型不同，属性和方法也都不尽相同。所有的节点都是Object.

#### 节点的属性

##### innerHTML和innerText的区别
- `value`:标签的value属性
- `innerHTML`:双闭合标签里面的内容（识别标签）
- `innerText`:双闭合标签里面的内容（不识别标签）

#### nodeType属性
- nodeType == 1 </br>
    元素节点（元素就是标签）
- nodeTpe == 2 </br>
    属性节点
- nodeType == 3</br>
   文本节点

#### nodeValue
- 对于元素节点是`null`
- 对于文本节点是`文本值`
- 对于属性节点是`属性值`

#### nodeName
- 对于元素节点是`标签名（返回的名称是大写）`
- 对于文本节点是`#text`
- 对于属性节点是`属性名（返回的名称是大写）`


### DOM操作
#### 获取元素
- `document.getElementById`：
根据ID查找元素，大小写敏感，如果有多个结果，只返回第一个
- `document.getElementsByClassName`：根据类名查找元素，多个类名用空格隔开返回一个HTMLCollection
- `document.getElementsByTagName`:根据标签查找元素，*表示查询所有标签，返回一个HTMLCollection。
- `document.getElementsByName`:根据元素的name属性查找，返回一个NodeList
- `document.querySelector('selector')` ：通过CSS选择器获取符合条件的第一个元素。
- `document.querySelectorAll('selector')`：通过CSS选择器获取符合条件的所有元素，返回一个NodeList

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


### 文档加载
浏览器在加载一个页面时，是按照自上而下的顺序加载的，读取到一行就运行一行。如果将script标签写到页面的上边，在代码执行时，页面没有加载DOM对象会导致无法获取到DOM对象

`onload事件`</br>
onload事件会在整个页面加载完成之后才触发，为window绑定一个onload事件，该事件对应的响应函数将会在页面加载完成之后执行，这样可以确保我们的代码执行时所有的DOM对象已经加载完毕了。


### 小试牛刀
#### 遍历dom
```
 function traversal(node) {
            //对node的处理
            if (node && node.nodeType === 1) {
                console.log(node.tagName);
            }
            let childNodes = node.childNodes,
            for (let i = 0; i < childNodes.length; i++) {
                let item = childNodes[i];
                if (item.nodeType === 1) {
                    traversal(item);
                }
            }
        }
```


