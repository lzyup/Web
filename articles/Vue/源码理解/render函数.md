### render
该渲染函数接收一个`createElement`方法作为第一个参数用来创建`VNode`。

### 示例
```
<div id="app">
    {{message}}
</div>
```
相当于
```
    render:function(createElement){
        return createElement('div',{
            attrs:{
                id:'app'
            }
        },this.message)
    }
```

### createElement


### 组件化
```
import Vue from 'vue'
import App from './App.vue'

var app = new Vue({
    el:'#app',
    //这里的h是createElement方法
    render:h=>h(App)
})
```
