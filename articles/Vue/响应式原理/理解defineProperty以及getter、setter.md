### 认识defineProperty
为对象定义一个属性，或是修改已有属性的值，并设置该属性的描述符。该方法返回修改后的对象。

### JS对象的两类属性
对于JavaScript来说，属性并非只是简单的名称和值，JavaScript用一组特征(attribute)来描述属性(property)。

#### 数据属性
- value:属性的值
- writable:决定属性能否被赋值
- enumerable:决定for in能否枚举该属性
- configurable:决定该属性能否被删除或者改变特征值。


#### 属性访问器(getter/setter)
所谓getter与setter其实是两个概念，并没有这样的属性
- getter:函数或undefined,在取属性值时被调用
- setter:函数或undefined,在设置属性值时被调用
- enumerable:决定for in能否枚举该属性
- configurable:决定该属性能否被删除或者改变特征值。


### Object.defineProperty的缺点
- 无法监控到数组下标的变化，导致直接通过数组的下标给数组设置值，不能实时响应。所以vue才设置了7个变异数组（push、pop、shift、splice、sort、rerverse）的hack方法解决问题
- 只能劫持对象的属性，因此我们需要对每个对象的每个属性进行遍历。如果能直接劫持一个对象，就不需要递归+遍历了。所以vue3.0会使用Proxy来替代Object.defineProperty

