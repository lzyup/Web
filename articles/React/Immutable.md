`ImmutableJS`最大的两个特点：
- `immutable data structures`（持久性数据结构）

    保证了数据一旦创建就不能修改，使用旧数据创建新数据时，旧数据不会改变
- `structural sharing`(结构共享)
    
    是指没有改变的数据共有一个引用，这样既减少了深拷贝的性能消耗，也减少了内存


```
import { Map } from 'immutable'
const person = Map();


const personWithAge = person.set('age', 20)

console.log(person.toJS()) // {}
console.log(personWithAge.toJS()) //{ age:20}
```
也就是说，在Immutabejs的数据结构中，当你想更改某个对象属性时，你得到的永远是一个新的对象，而原对象永远也不会发生更改。

### Immutable常用类型
1、`List` 类似JS的Array
2、`Map`类似JS的Object


### Immutable常用API
- `fromJS(value,convert)`</br>
简介：value是要转变的数据，converter是要做的操作。第二个参数可不填，默认情况会将数组准换为List类型，将对象转换为Map类型，其余不做操作。

- `toJS()`</br>
将一个Immutable数据转换为JS类型的数据


- `merge`</br>
浅合并,新数据与旧数据对比，旧数据中不存在的属性直接添加，旧数据已存在的属性用新数据覆盖


[参考链接](https://github.com/camsong/blog/issues/3)
