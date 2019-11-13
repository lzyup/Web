### clientHeight
 这个属性是只读属性，对于没有定义CSS或者内联布局盒子的元素为0,否则，它是元素内部的高度
 （单位像素），包含内边距，但不包括水平滚动、边框和外边距。（`height + padding`）
 
 
 `clientHeight`可以通过CSS`height` + CSS `padding`-水平滚动条高度（如果存在）来计算
 
 
 ### offsetHeight
 包括padding、border、水平滚动条，但不包括margin的元素的高度。对于inline的元素这个属性一直是0,单位是px（`height + padding + border`）
 
 
 ### scrollHeight
获取元素**整个滚动区域**的宽、高。包括width和padding,不包括border和margin
 
 
 ### scrollTop
 获取垂直滚动条滚动的距离
 
 
 ### offsetTop
当前元素相对于其**定位父元素**的垂直偏移量。

### getBoundingClientRect().top
用于获得页面中某个元素的左，上，右和下分别相对浏览器视窗的位置

### IE盒模型
- W3C标准盒模型</br>盒子的高度是由盒子的内容区仅由width,height决定的，不包含边框，内外边距
- IE盒子模型</br>
盒子的宽高不仅包含了元素的宽高，而且包含了元素的边框以及内边距

.item{
box-sizing:border-box;	IE盒模型
box-sizing:content-box;
}
 
 
 [参考链接](https://juejin.im/entry/59c1fd23f265da06594316a9)