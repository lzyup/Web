### Position属性
- `static`<br>
默认值。没有定位，元素出现在**正常**流中（忽略top,bottom,left,right或者z-index声明）

- `relative`<br>
相对定位。生成相对定位的元素，通过top,bottom,left,right的设置相对于**正常（原先本身**位置进行定位。
>
     定位过程
        （1） 首先按照默认方式`（static）`生成一个元素（并且元素像浮层一样浮动了起来）<br>
        （2） 然后相对于以前的位置移动，移动的方向和幅度由left,right,top,bottom属性确定，但是**偏移前的位置保留不动**
    

- `absolute`<br>
绝对定位，生成绝对定位的元素，相对于**static定位以外**的第一个**父元素**进行定位。元素的位置通过left,top,right,bottom属性进行规定。可通过z-index进行层次分级。

- `fixed`<br>
固定定位，生成绝对定位的元素，相对于**浏览器窗口**进行定位。元素的位置left,top,right,bottom属性进行规定


其中`relative`的元素脱离了正常的文本流，但是**其在文本流中的位置依然存在**

### relative和absolute的主要区别

- `relative定位的层总是相对于其最近的父元素，无论其父元素是何种定位方式`
- 而absolute是相对于其最近的定义为absolute或relative的父层，如果父层没有找到，就是body


### 什么是文档流
将窗体自上而下分成一行行，并在每行中按从左至右的顺序排放元素，即为文档流

**只有三种情况会是使得元素脱离文档流，分别是：relative,absolute,fixed**

[链接](https://www.cnblogs.com/theWayToAce/p/5264436.html)
