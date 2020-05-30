### 用法示例
```
classNames('foo', 'bar'); => 'foo bar'
classNames('foo',{bar:true}); => 'foo bar'
classNames('foo-bar':false); => ''

//lots of arguments of various types
classNames('foo',{bar:true,duck:false}, 'baz', {quux:true}); => 'foo bar baz quux'

var arr = ['b', {c:true, d:false}]
classNames('a', arr); // => 'a b c'

let buttonType = 'primary';
classNames({[`btn-${buttonTye}`]}:true)
```




[官方文档](https://github.com/JedWatson/classnames)