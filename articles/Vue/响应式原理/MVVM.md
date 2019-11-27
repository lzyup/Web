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

### Dep
`Dep`是一个Class,它定义了一些属性和方法，这里需要特别注意的是它有一个静态属性`target`,这是一个全局唯一`Watcher`,这是一个非常巧妙的设计，**因为在同一时间只能有一个全局的Watcher**


### 实现Compile
compile主要做的事情是解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图。

因为遍历解析的过程有多次操作dom节点，为提高性能和效率，会先将vue实例根节点的`el`转换成文档碎片`fragment`进行解析编译操作，解析完成，再将`fragment`添加回原来的真实dom节点中。

```
function Compile(el){
    this.$el = this.isElementNode(el)?el:documennt.querySelector(el);
    if(this.$el){
        this.$fragment = this.node2Fragment(this.$el);
        this.init();
        this.$el.appendChild(this.$fragment);
    }
}

Compile.prototype = {
    init:function(){this.compileElement(this.$fragment);},
    node2Fragment:function(el){
        var fragment = document.createDocumentFragment(),child;
        //将原生节点拷贝到fragment
        while(child = el.firstChild){
            fragment.appendChild(child)
        }
        return fragment
    }
}
```

compileElement方法将遍历所有节点及其子节点，进行扫描解析编译，调用对应的指令渲染函数进行数据渲染，并调用对应的指令更新函数进行绑定

```
Compile.prototype = {
    //...省略
    compileElement:function(el){
        var childNodes = el.childNodes, me = this;
        [].slice.call(childNodes).forEach(function(node){
            var text = node.textContent;
            var reg = /\{\{(.*)\}\}/;//表达式文本
            // 按元素节点方式编译
            if(me.isElementNode(node)){
                me.compile(node);
            }else if(me.isTextNode(node) && reg.test(text)){
                me.compileText(node,RegExp.$1);
            }
            //遍历编译子节点
            if(node.childNodes && node.childNodes.length){
                me.compileElement(node);
            }
            
        })
    },
    
    compile:function(node){
        var nodeAttrs = node.attributes, me = this;
        [].slice.call(nodeAttrs).forEach(function(attr){
            //规定：指令以 v-xxx 命令
            // 如<span v-text="content"></span>中指令为v-text
            var attrName = attr.name; //v-text
        if(me.isDirective(attrName)){
            var exp = attr.value;//content
            var dir = attrName.substring(2);//text
            if(me.isEventDirective(dir)){
                //事件指令，如 v-on:click
                compileUtil.eventHandler(node,me.$vm,exp,dir);
            }else{
                // 普通指令
                compileUtil[dir] && compileUtil[dir](node,me.$vm,exp);
            }
            
        }
        })
    }
};

//指令处理集合
var compileUtil = {
    text:function(node,vm,exp){
        this.bind(node,vm,exp,'text');
    },
    //...省略
    bind:function(node,vm,exp,dir){
        var updateFn = update[dir + 'Updater'];
        //第一次初始化视图
        updateFn && updateFn(node,vm[exp]);
        // 实例化订阅者，此操作会在对应的属性消息订阅器中添加了该订阅者Watcher
        new Watcher(vm,exp,function(value,oldValue){
            //一旦属性值有变化，会收到通知执行此更新函数，更新视图
            updaterFn && updateFn(node,value,oldValue);
        });
    }
}

//更新函数
var updater = {
    textUpdater: function(node,value){
        node.textConntent = typeof value === 'undefined'?'':value;
    }
    //...省略
}
```

这里通过递归遍历保证了每个节点及子节点都会解析编译到，包括了{{}}表达式声明的文本节点。监听数据、绑定更新函数的处理是在`compileUtil.bind()`这个方法中，通过`new Watcher()`添加回调来接收数据变化的通知。

### 实现Watcher
Watcher订阅者作为Observer和Compile之间通信的桥梁，主要做的事情是
- 在自身实例化时往属性订阅器(dep)里面添加自己
- 自身必须有一个update()方法
- 待属性变动dep.notice()通知时，能调用自身的update()方法，并触发Compile中绑定的回调

```
function Watcher(vm,exp,cb){
    this.cb = cb;
    this.vm = vm;
    this.exp = exp;
    //此处为了触发属性的getter,从而在dep添加自己，结合Observer更容易理解
    this.value = this.get();
}

Watcher.prototype = {
    update:function(){
        this.run(); //属性值变化收到通知
    },
    run:function(){
        var value = this.get();//取到最新值
        var oldValue = this.value;
        if(value !== oldValue){
            this.value = value;
            this.cb.call(this.vm,value,oldVal);//执行Compile中绑定的回调，更新视图
        }
    },
    get:function(){
        Dep.target = this;//将当前订阅者指向自己
        var value = this.vm[exp]; //触发getter,添加自己到属性订阅器中
        Dep.target = null;//添加完毕，重置
        return value;
    }
};
//这里再次列出Observer和Dep,方便理解
Object.defineProperty(data,key,{
    get:function(){
        //由于需要在闭包内添加watcher,所以可以在Dep定义一个全局的target属性，暂存watcher,添加完移除
        Dep.target && dep.addDep(Dep.target);
        return val;
    }
    // ...省略
});
Dep.prototype = {
    notify:function(){
        this.subs.forEach(function(sub){
            sub.update();//调用订阅者的update方法，通知变化
        })
    }
}
```

实例化`Watcher`的时候，调用`get()`方法，通过`Dep.target = watcherInstance`标记订阅者是当前Watche实例，强行触发属性定义的`getter`方法，`getter`方法执行的时候，就会在属性的订阅器`dep`添加当前watcher实例，从而属性值有变化的时候，watcherinstance就能收到更新通知。

#### 补充实例化Watcher的场景
- Vue实例化过程中的Watch选项
- Vue实例化过程中的Computed计算属性选项
- Vue原型上有挂载$watch方法：Vue.prototype.$watch,可以直接通过实例调用this.$watch方法
- Vue生成了render函数，更新视图时

### 实现MVVM
MVVM作为数据绑定的入口，整合Observer、Compile和Watcher三者,通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的沟通桥梁。
```
function MVVM(options) {
    this.$options = options || {};
    var data = this._data = this.$options.data;
    var me = this;

    // 数据代理
    // 实现 vm.xxx -> vm._data.xxx
    Object.keys(data).forEach(function(key) {
        me._proxyData(key);
    });

    this._initComputed();

    observe(data, this);

    this.$compile = new Compile(options.el || document.body, this)
}
```
1、实例化MVVM的时候，会先调用observe就是Observer类的方法，从而对数据劫持。
**每个对象值的getter都持有一个`dep`,这里的dep是观察者管理器Dep类中有一个subs数组是用来收集watcher**

2、实例化Compile,传入模板,可以认为在模板上用到data选项（也就是被observe劫持的数据）里面的标签就是一个watcher实例

3、实例化watcher的同时，会触发劫持属性的get方法从而添加自己到订阅中去。**即触发了`dep.depend()方法`**


4、然后改变劫持属性的时候，会发送通知给watcher触发update方法更新