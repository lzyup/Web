### 水平居中
 - inline元素用`text-align:center`即可，如下：
 ```
 .container{
     text-align:center;
 }
 ```
 
 - blcok元素可使用`margin:auto`,PC时代的很多网站都是这么搞的
 ```
 .container{
     text-align:center;
 }
 .item{
     width:100px;
     margin:auto
 }
 ```
 
 - 绝对定位元素可结合`left`和`margin`实现，但是必须知道宽度
 ```
 .container{
     position:relative;
     width:500px;
 }
 .item{
     width:300px;
     height:100px;
     position:absolute;
     left:50%;
     margin:-150px;
 }
 ```
 
 ### 垂直居中
 - inline元素可设置`line-height`的值等于`height`值，如单行文字垂直居中：
 ```
 .container{
     height:50px;
     line-height:50px;
 }
 ```
 - 绝对定位元素，可结合`left`和`margin`实现，但是必须知道尺寸
```
.container{
    position:relative;
    height:200px;
}
.item{
    width:80px;
    height:40px;
    position:absolute;
    left:50%;
    top:50%;
    margin-top:-20px;
    margin-left:-40px;
}
```

- 绝对定位可结合`transform`实现居中

```
.container{
    position:relative;
    height:200px;
}
.item{
    width:80px;
    height:40px;
    position:absolute;
    left:50%;
    top:50%;
    transform:translate(-50%,-50%);
    background:blue
}
```

- 绝对定位结合`margin:auto`,不需要提前知道尺寸，兼容性好
```
.container{
    position:relative;
    height:300px;
}
.item{
    width:100px;
    height:50px;
    position:absolute;
    left:0;
    top:0;
    right:0;
    bottom:0;
    margin:auto;
}
```
 
 