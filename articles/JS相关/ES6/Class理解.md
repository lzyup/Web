##
> 简单对比
#### ES5
```
function Point(x,y){
    this.x = x;
    this.y = y;
}
```

#### ES6
```
class Point{
    constructor(x,y){
        this.x = x;
        this.y = y;
    }
    
    toString(){
        return '(' + this.x + ', ' + this.y + ')'
    }
}
```
ES6的类可以看做是构造函数的另外一种写法。
`typeof Point //function` </br>
`Point === Point.prototype.constructor //true`</br>
**类的所有方法都定义在类的`prototype`属性上面**


#### constructor方法
`constructor`方法是类的默认方法，通过`new`命令生成对象实例时，自动调用该方法。


#### 静态方法
类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上`static`关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这称为"静态方法"。


- 静态方法包含`this`关键字，这个`this`指的是类，而不是实例。
```
class Foo {
    static bar(){
        this.baz();
    }
    
    static baz(){
        console.log('hello')
    }
    
    baz(){
        console.log('world')
    }
}

Foo.bar() //hello

```

- 父类的静态方法，可以被子类继承
```
class Foo {
    static classMethod(){
        return 'hello'
    }
}

class Bar extends Foo {
}

Bar.classMethod() //'hello'
```

#### 静态属性
静态属性指的是Class本身的属性
```
class Foo {
   static myStaticProp = 42;
   
   constructor(){
       console.log(Foo.myStaticProp);//42
   }
}

```

