### 1、JSX
JSX是一个JavaScript的语法扩展。JSX可以很好地描述UI应该呈现出它应有交互的本质形式。JSX可能会使人联想到模版语言，但它具有JavaScript的全部功能。

#### JSX特定属性
- 你可以通过使用引号，来将属性值指定为字符串字面量。
```
const element = <div tabIndex="0"></div>
```
- 也可以使用大括号，在属性值中插入一个**JavaScript表达式**
```
const element = <img src={user.avatarUrl}></img>
```
### 2、元素渲染
React元素是创建开销极小的普通对象。`React DOM`会负责更新DOM来与React元素保持一致。

### 3、组件
- 通过JS函数创建（无状态组件）
- 通过class创建（有状态组件）

#### 函数组件和class组件的区别
1、如果一个组件仅仅是为了展示数据，那么此时就可以使用函数组件

2、如果一个组件中有一定业务逻辑，需要操作数据，那么就需要使用class创建组件，因为，此时需要使用state

#### 函数组件的注意点
- 函数名称必须为大写字母开头，React通过这个特点来判断是不是一个组件
- 函数必须要返回值，返回值可以是:JSX对象或者null
- 返回的JSX,必须是一个根元素


### 4、生命周期
#### 4.1 创建阶段(Mounting)
##### constructor()
- 作用：1获取props2初始化state
- 说明：通过`constructor()`的参数`props`获取
```
class Greeting extends React.Component{
    constructor(props){
        //获取props
        super(props)
        //初始化state
        this.state = {
            count:props.initCount
        }
    }
}
```

##### componentWillMount()
- 说明:组件被挂载到页面之前调用，其在render()之前被调用，因此在这里同步地设置状态不会触发重渲染。
- 注意：无法获取页面中的DOM对象
- 注意：可以调用`setState()`方法来改变状态值
- 用途：发送ajax请求获取数据

##### render()
- 作用：渲染组件到页面中，无法获取页面中的DOM对象
- 注意：**不要在render方法中调用setState方法，否则会递归渲染**

```
render(){
    console.warn(document.getElementById('btn'))//null
    
    return(
        <div>
            <button id="btn" onClick={this.handleAdd}>打豆豆</button>
            {
                this.state.count == 4
                ?null
                :<CounterChild initCount={this.state.count}></CounterChild>
            }
        </div>
    )
}
```

##### componentDidMount()
- 组件已经挂载到页面中
- 可以进行DOM操作
- 可以发送请求获取数据
- 可以通过`setState()`修改状态值
- 注意：这里修改状态值会重新渲染


#### 4.2 运行阶段（Updating）
- 该阶段的函数执行多次
- 每当组件的`props`或者`state`改变的时候，都会触发运行阶段的函数
##### componentWillReceiveProps()
- 组件接收到新的`props`前触发这个方法
- 当前组件的`props`值
- 可以通过`this.props`获取到上一次的值
- 如果你需要响应属性的改变，可以通过对比`this.props`和`this.setState()`处理状态方法
- 注意:修改`state`不会触发该方法

```
componentWillReceiveProps(nextProps){
    
}
```
##### shouldComponentUpdate()
- 根据这个方法的返回值决定是否重新渲染组件，返回true重新渲染，否则不渲染
- 通过某个条件渲染组件，降低组件渲染频率，提升组件性能
- 如果返回值为`false`,那么，后续`render()`方法不会被调用
- 这个方法必须返回布尔值
-场景： 根据随机数决定是否渲染组件
```
//第一个参数：最新属性对象
//第二个参数：最新状态对象
shouldComponentUpdate(nextProps,nextState){
    return nextStaet.count % 2 === 0
}
```

##### componentWillUpdate()
- 作用：组件将要更新
- 参数：最新的属性和状态对象
- 注意：不能修改状态否则会循环渲染
```
componentWillUpdate(nextPorps,nextState){
    
}
```

##### render()渲染
- 作用：重新渲染组件
- 注意：这个函数能够执行多次，只要组件的属性或状态改变了，这个方法就会重新执行

##### componentDidUpdate()
- 作用：组件已经被更新
- 参数：旧的属性和状态对象
```
componentDidUpdate(prevProps,prevState){
    
}
```

#### 4.3 卸载阶段（Unmounting）
- 组件销毁阶段：组价卸载期间，函数比价单一，只有一个函数，这个函数也有一个显著的特点：组件只能执行一次
- 只要组件不在被渲染到页面中，那么这个方法就会被调用

##### componentWillUnmount()
- 作用：在卸载组件的时候，执行清理工作，比如
    - 清除定时器
    - 清除`ComponentDidMount`创建的DOM对象




### 5、prop
`this.props`对象的属性与组件的属性一一对应，但是有一个例外，就是`this.props.children`属性。它表示组件的所有子节点

##### propTypes
组件类的`propTypes`属性，用来验证组件实例的熟悉感是否符合要求


### 6、State
- `state`是私有的，并且完全受控于当前组件
- 修改`state`必须使用`setState()`方法
- 构造函数是唯一可以给`this.state`赋值的地方

```
constructor(props){
    super(props)
    this.state = {
        count:props.initCount
    }
}

componentWillMount(){
    //方式一
    this.setState({
        count:this.state.count + 1
    })
    
    this.setState({
        count:this.state.count + 1
    },function(){
        //由于setState()是异步操作，所以，如果想立即获取修改后的state
        //需要在回调函数中获取
        
    })
    
    //方式二：
    this.setState(function(prevState,props){
        return{
            counter:prevState.counter + props.increament
        }
    })
    
}


```

### 7、 Fragments
React中的一个常见模式是一个组件返回多个元素。Fragments允许你将子列表分组，而无需向DOM添加额外节点。
```
render{
    return (
        <React.Fragment>
            <ChildA/>
            <ChildB/>
            <ChildC/>
        </React.Fragment>
    )
}
```

### 8、Refs
#### 应用场景
- 管理焦点，文本选择或媒体播放
- 触发强制动画
- 集成第三方DOM库

#### 使用方法
- Refs是使用`React.createRef()`创建的
- 当ref被传递给`render`中的元素时，对该节点的引用可以在ref的`current`属性中被访问
- 当`ref`属性用于自定义class组件时，`ref`对象接收组件的挂载实例作为其`current`属性
- **不能再函数组件上使用ref属性**，因为他们没有实例


```
class CustomTextInput extend React.Component{
    constructor(props){
      super(props);
      //创建一个ref来存储textInput的DOM元素
      this.textInput = React.createRef();
      this.fousTextInput = this.focusTextInput.bind(this);
    }
    
    focusTextInput(){
        //直接使用原生API使text输入框获得焦点
        //注意：我们通过"current"来访问DOM节点
        this.textInput.current.focus()
    }
    
    render(){
        //告诉React我们想把<input>ref关联到构造器创建的'textInput'上
        return(
            <div>
                <input type="text"
                        ref={this.textInput}/>
                <input
                    type="button"
                    value="Focus the text input"
                    onClick={this.foucusTextInput}
                />
            </div>
        )
    }
}
```
### 9、事件处理
