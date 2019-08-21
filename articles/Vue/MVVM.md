### 思路
- 实现一个数据监听器`Observer`,能够对数据对象的所有的属性进行监听，如有变动可拿到最新值并通知订阅者。
- 实现一个指令解析器`Compile`，对每个元素节点的指令进行扫描和解析，根据指令模板替换数据，以及绑定相应的更新函数。
- 实现一个Watcher，作为连接`Observer`和`Compile`的桥梁，能够订阅并收到每个属性变动的通知，执行指令绑定的相应回调函数，从而更新视图。


### 实现Observer
利用`Object.defineProperty()`来监听属性变动那么将需要observer的数据对象进行递归遍历，包括子属性对象的属性，都加上`setter`和`getter`这样的话，给这个对象的某个赋值，就会触发`setter`，那么就能监听到了数据变化。代码如下：
```
var data = {name:'kindeng'};
observer(data);
data.name = 'dmp';//监听到值变化了 kindeng ---> dmq

function observer(data){
    if(!data || typeof data !== 'object'){
        return;
    }
    Object.keys(data).forEach(functioin(key){
        defineReactive(data,key,data[key]);
    })
};

function defineReactive(data,key,val){
    observe(val); //监听子属性
    Object.defineProperty(data,key,{
        enumerable:true,//可枚举
        configurable:false,//不能再define
        get:function(){
            return val;
        },
        set:function(newVal){
            console.log('监听到值变化了',val,'---->',newValue);
            val = newVal;
        }
    })
}
```
这样我们已经可以监听每个数据的变化了，那么监听到变化之后就是怎么通知订阅者了，所以接下来需要实现一个消息订阅器，很简单，维护一个数组，用来收集订阅者，数据变动触发notify,再调用订阅者的update方法，代码改善之后如下：
```
//...省略
function defineReactive(data,key,val){
    var dep = new Dep();
    observal(val);//监听子属性
    
    Object.defineProperty(data,key,{
        // ...省略
        set:function(newVal){
            if(val === newVal) return;
            console.log('监听到值变化了',val,'---->',newVal);
            val = newVal;
            dep.notify();//通知所有订阅者
        }
    })
}

function Dep(){
    this.subs = [];
}
Dep.prototype = {
    addSub:function(sub){
        this.subs.push(sub);
    },
    notify:function(){
        this.subs.forEach(function(dub){
            sub.update();
        })
    }
}
```

之前我们已经明确了`Watcher`是订阅者，而且`var dep = new Dep()`;是在`defineReactive`方法内部定义的，所以想通过`dep`添加订阅者，就必须要在闭包内操作，所以我们可以在`getter`里面动手脚：
```
//Observer.js
//...省略
Object.defineProperty(data,key,{
    get:function(){
        //由于需要在闭包内添加Watcher，所以通过Dep定义一个全局target属性，暂存watcher,添加完移除
        Dep.target && dep.addDep(Dep.target);
        return val;
    }
    // ...省略
});

// Watcher.js
Watcher.prototype = {
    get:function(key){
        Dep.target = this;
        this.value = data[key];//这里会触发属性的getter,从而添加订阅者
        Dep.target = null;
    }
}
```