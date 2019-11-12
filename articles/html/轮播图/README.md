### `style.left`

left规定元素的左边缘。该属性定义了元素左外边距边界与其包含左边界之间的偏移。**如果position属性为值static,那么设置的left属性不会产生任何效果**

### `offsetLeft`
当前元素相对于其**定位元素**的水平偏移量

### `style.left`和`offsetLeft`区别
- `offsetLeft`可以返回无定位父元素的偏移量。如果父元素中都没有定位，则body为准
- `offsetTop`返回的是数字，而是style.top返回的是字符串，而且还带单位：px
- `offsetLeft`是只读，而style.left和style.top可读写
- 如果没有给HTML元素指定过top样式，则style.left返回的是空字符串


### 移动动画关键
`setInterval`定时器动态改变`style.left`


### 轮播图思路
赋值第一张图片放到ul的最后，然后当图片重新切换到第1张的时候
实际是切换到ul的最后一张，即第一张的copy，再次从最后一张切换到第二张的时候瞬间切换到第一张图片(style.left = 0)，然后滑动到第二张图