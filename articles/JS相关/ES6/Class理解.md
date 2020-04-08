### 类
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

#### 类的方法
- 类的所有方法都定义在类的`prototype`属性上面
- 类的方法内部如果含有`this`,它默认指向类的实例。但是，一旦单独使用该方法，很可能报错。

```
class Logger{
    printName(name = 'there'){
        this.print(`Hello ${name}`)
    }
    
    print(text){
        console.log(text);
    }
}

const logger = new Logger();
const {printName} = logger;
printName(); // TypeError: Cannot read property 'print' of undefined
```

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

### 继承
- 子类的构造函数中，只有调用了`super`之后，才可以使用`this`关键字，否则会报错。（子类实例的构建，基于父类实例，只有`super`方法才能调用父类实例）

```
class Point {
    constructor(x,y){
        this.x = x;
        this.y = y;
    }
}

class ColorPoint extends Point {
    constructor(x,y,color){
        this.color = color;//ReferenceError
        super(x,y);
        this.color = color; //正确
    }
}

```

- 父类的静态方法，也会被子类继承
```
class A{
    static hello(){
        console.log('hello world')
    }
}

class B extends A{
    
}

B.hello()  //hello world
```

#### super
- 作为函数</br>
代表父类的构造函数。ES6要求，子类的构造函数必须执行一次`super`函数且只能出现在子类的构造函数中
```
class A {}

class B extends A {
    constructor(){
        super()
    }
}
```
`super`虽然代表了父类的`A`的构造函数，但是返回的是子类`B`的实例，这里的`super()`相当于`A.prototype.constructor.call(this)`

- 作为对象</br>
```
class A {
    p(){
        return 2;
    }
}

class B extends A{
    constructor(){
        super();
        console.log(super.p());//2
    }
}
```
子类`B`当中的`super.p()`,就是将`super`当做一个对象使用。这是`super`在普通方法中，指向的是`A.prototype`,所以`super.p()`就相当于`A.prototype.p()`


- 在子类普通方法中通过`super`调用父类的方法时，方法内部的`this`指向当前的子类实例。
