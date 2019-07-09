### 1、概念
- `bind()`方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入`bind()`方法的第一个参数作为`this`,传入`bind()`方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数来调用原函数。


***这里返回的绑定函数就是调用bind()的那个函数，只不过this的指向变了***

### 2、特点
- 将所有传入bind()方法中的实参（第一个参数之后的参数）与this一起绑定
- bind()方法所返回的函数的length(形参数量)等于原函数的形参数量减去传入bind()方法中的实参数量(第一个参数以后的所有参数)
- 当bind()所返回的函数作为构造函数的时候，传入bind()的this将会被忽略，实参会全部传入原函数


### 3、模拟实现
```
   Function.prototype.bind2 = function(context) {
                if (typeof this !== "function") {
                    throw new Error(
                        "Function.prototype.bind - what is trying to be bound is not callable"
                    );
                }
                var self = this;
                //因为第一个参数是指定的this,所以只截取第1个之后的参数
                //arr.slice(begin); 即[begin,end]
                var args = Array.prototype.slice.call(arguments, 1);

                var fNOP = function() {};

                var fBound = function() {
                    //这时的argument是指bind返回的函数传入的参数
                    var bindArgs = Array.prototype.slice.call(arguments);
                    //此时返回的绑定函数当作为构造函数时，this指向实例，此时this instance of fBound 结果为true,可以让实例获得来自绑定函数的值
                    //**注意这里的this和外边的this不一样**

                    //当作为普通函数时,this指向window,此时结果为false,将绑定函数的this指向context
                    return self.apply(
                        this instanceof fBound ? this : context,
                        args.concat(bindArgs)
                    );
                };

                fNOP.prototype = this.prototype; //空对象的原型指向绑定函数的原型
                fBound.prototype = new fNOP(); //空对象的实例赋值给fBound.prototype
                return fBound;
            };
```


