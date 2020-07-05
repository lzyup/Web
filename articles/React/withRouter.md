
## 作用
withRouter是react-route的一个高阶组件，可获取history

render时会把match,location和history传入props

### `history`
- `action`</br>
可得知路由栈的操作是`POP`、`PUSH`、`REPLACE`
### `location`</br>
    - `pathname`当前路由的绝对路径
    - `search`查询参数，访问`/user?userId=123`=>可以拿到`?userId=123`
    - `hash`:`#the-hash`
    - `state`:类似vue的路由元信息`meta`,在这里的存储的信息不会明文显示在浏览器上

### `match`</br>
    - `params`当`<Route to='/user/:id' component={user}>`,访问`/user/123`在`user`组件中。`this.props.match.params.id`可以获取id
    - `isExact`,当前路由是不是完全匹配
    
## 使用场景
路由组件是可以直接获取这些属性（`history`、`location`,`match`）,而非路由组件就必须通过`withRouter`修饰后才能获取这些属性。
例如：
```
<Route path='/' component={App} />

```
App组件就可以直接获取路由中这些属性，但是，如果App组件汇总有一个子组件`Foo`，那么`Foo`就不能直接获取路由中的属性了，必须通过`withRouter`修饰后才能获取到

