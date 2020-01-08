### flex语法
```
 flex: [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ];
 
 flex:auto;
 
 flex:none;
```
#### 语法解析
CSS语法中的特殊符号的含义绝大多数就是正则表达式中的含义，例如单管道符`|`,方括号`[]`,问号`?`,个数范围花括号`{}`等。

首先是单管道符`|`。表示排他。也就是这个符号前后的属性值都支持的，且不能同时出现。因此下面语法是支持的
```
 flex: [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ];
 
 flex:auto;
 
 flex:none;
```

接下来，`[...]`这一部分。其中方括号`[]`表示范围。也就是支持的属性值在这个范围内。我们先把方括号`[]`内其他特殊字符去除，可以得到下面的语法
```
flex: auto;
flex: none;

flex: [ <'flex-grow'> <'flex-shrink'> <'flex-basis'> ]
```
这就是说，flex属性支持空格分隔的3个值，因此，下面的语法都是支持的
```
flex: auto;
flex: none;
/* 3个值 */
flex: 1 1 100px;
```

再看方括号`[]`内的其他字符，例如问号`?`,表示0个或1个。也就是`flex-shrink`属性可有可无。因此，flex属性值也可以是2个值。因此，下面的语法都是支持的
```
flex: auto;
flex: none;
/* 2个值 */
flex: 1 100px;
/* 3个值 */
flex: 1 1 100px;
```

最后看双管道符号`||`,是或者的意思。表示前后可以分开独立合法使用。也就是`flex:flex-grow flex-shrink?`和`flex-basis`都是合法的。于是我们又多了2种合法的写法
```
flex:auto;
flex:none;
/**1个值，flex-grow**/
flex:1;
/**1个值,flex-basis**/
flex:100px
/**2个值，flex-grow和flex-basis**/
flex:1 100px
/**2个值，flex-grow和flex-shrink**/
flex:1 1;
/**3个值**/
flex:1 1 100px
```

### flex-grow
指定了容器剩余空间**多余**时候的分配规则。默认值是`0`,多余空间不分配</br>

如果所有项目的`flex-grow`属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的`flex-grow`属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

### flex-shrink
制定了容器剩余空间不足的时候的分配规则，默认值是`1`,空间不足要分配。

如果所有的项目`flex-shrink`属性为1，当空间不足时，都将等比例缩小。如果一个项目的`flex-shrink`属性为0，其他项目都为1，则空间不足时，前者不缩小。

### flex-basis
指定了固定的分配数量

**浏览器根据这个属性，计算主轴是否有多余空间。它默认值为`auto`，即项目的本来大小**


它可以设为跟`width`或`height`属性一样的值，则项目将占据固定空间


### flex的几个简写
- flex:1</br>
`flex: 1 1 0`
- flex:auto</br>
`flex:1 1 auto`
- flex:initial</br>
`flex:0 1 auto`
- flex:none
`flex:0 0 auto`


[link1](https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)

[link2](http://www.ayqy.net/blog/flexbox%E5%B8%83%E5%B1%80%E6%8C%87%E5%8D%97/)

[link3](https://www.zhangxinxu.com/wordpress/2019/12/css-flex-deep/)