### `context`是什么
`context`提供了一种通过组件树传递数据的方法，无需每个级别手动传递props属性即全局共享数据

### 何时使用
以主题色`theme`为例
#### 不使用`context`时
```
class App extends React.Component {
    render(){
        return <Toolbar theme="dark" />
    }
}

function Toolbar(props){
    return (
        <div>
            <ThemeButton theme={props.theme} />
        </div>
    )
}

class ThemeButton extends React.Component {
    render(){
        return <Button theme={this.props.theme}/>
    }
}
```

#### 使用`context`时
```
//为当前theme创建一个context（'light'为默认值）
const ThemeContext = React.createContext('light');
class App extends React.component {
    render(){
        //使用一个Provider来将当前的theme传递给以下组件树
        //无论多深，任何组件都能读取这个值
        return (
            <ThemeContext.Provider value="dark">
                <Toolbar />
            </ThemeContext.Provider>
        )
    }
}

function ToolBar(){
    return (
        <div>
            <ThemeButton />
        </div>
    )
}

class ThemeButton extends React.Component{
    //挂载在class上的`contextType`属性会被重赋值为一个由React.createContext()创建的Context对象。
    //这样就能使用this.context来消费最近Context上的那个值
    static contextType = ThemeContext;
    render(){
        return <Button theme={this.context} />
    }
}
```