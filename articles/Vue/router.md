### 路由对象属性（`$route`）
#### `$route.path`

字符串，对应当前路由的路径，总是解析为绝对路径，如`"/foo/bar"`

#### `$route.params`

一个key/value对象,包含了动态片段和全匹配片段，如果没有路由参数，就是一个空对象。例如

| 模式                          | 匹配路径            | $route.params                     |
| ----------------------------- | ------------------- | --------------------------------- |
| /user/:username               | /user/evan          | `{username:'evan'}`               |
| /user/:username/post/:post_id | /user/evan/post/123 | `{username:'evan',post_id:'123'}` |



#### `$route.query`

一个key/value对象，表示URL查询参数。例如，对于路径`/foo?user=1`,则有`$route.query.user ==  1`,如果没有查询参数，则是个空对象。


### `$route、$router、$router.optionns.routes`的区别
- `$route`<br>
路由对象信息，表示的是当前页面的路由信息

- `$router`<br>
指的是router的实例对象
    - 编程式导航
   ```
    // 命名的路由
    router.push({ name: 'user', params: { userId: '123' }})

    // 带查询参数，变成 /register?plan=private
    router.push({ path: 'register', query: { plan: 'private' }})
    ```
    
    - 如果提供了path,params会被忽略，取而代之的是需要提供路由的name或者完整的带有参数的path
    ```
    const userId = '123'
    router.push({ name: 'user', params: { userId }}) // -> /user/123
    router.push({ path: `/user/${userId}` }) // -> /user/123
    // 这里的 params 不生效
    router.push({ path: '/user', params: { userId }}) // -> /user
    ```
- `$router.optionns.routes` <br>    
   包含整个路由实例的所有路由信息
### 嵌套路由


### 路由导航守卫
#### 全局守卫
1. `router.beforeEach`全局前置守卫进入路由之前
2. `router.beforeResolve`全局解析守卫(2.5.0+)在`beforeRouteEnter`调用之后调用
3. `router.afterEach`全局后置钩子进入路由之后

#### to,from,next三个参数
- `to:Route`:即将要进入的路由对象
- `from:Route`:当前导航正要离开的路由
- `next:Function`:一定要调用该方法来**resolve**这个钩子。执行效果依赖`next`方法的调用参数。也就是说这个方法***必须调用，否则不能进入路由**（页面空白）。
    - `next()`:进入该路由。
    - `next(false)`:取消进入路由，url地址重置为from路由地址（也就是将要离开的路由地址）。
    - `next('/')或者next({path:'/'})`:跳转到新路由，当前导航被终端，重新开始一个新导航。
    

#### 常见死循环错误
例如：
```
router.beforeEach((to,from,next)=>{
    if(登录){
        next()
    }else{
        next({name:"login"})
    }
})
```

这里看似逻辑没问题，但是当我们跳转`login`之后，因为此时还是未登录状态，所以会一直跳转到`login`然后死循环，页面一直空白的，所以：我们需要把判断条件稍微改一下。
```
if(登录 || to.name === "login"){next()}
```
