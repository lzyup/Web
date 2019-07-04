### 1、new操作做了哪些事情

- 以构造器的`prototype`属性（注意与私有字段`[[prototype]]`的区分）为原型，创建新对象
- 将`this`和调用参数传给构造器，执行；（将`this`绑定到新创建的对象上）
- 如果构造器返回的是对象，则返回，否则返回第一步创建的对象


### 2、模拟实现
```
 create() {
            //创建一个空对象
            var obj = new Object();
            // 获得构造函数，arguments中取出第一个参数
            var Con = Array.prototype.shift.call(arguments);
            // 链接到原型，obj可以访问到构造函数原型中的属性
            obj.__proto__ = Con.prototype;
            //绑定 this 实现继承，obj可以访问到构造函数中的属性
            var ret = Con.apply(obj, arguments);
            // 优先返回构造函数返回的对象
            return ret instanceof Object ? ret :obj
        },
```