## display几种属性
- inline

inline是内联对象，比如`<a/>`、`<span/>`标签等，可以“堆在一起”显示，宽高由内容决定，不能设置。


- block

block是块对象，比如`<div/>、<p/>`标签等，要占一行，但是宽高可以定义。

- inline-block

可以将对象呈递为内联对象，而内容作为块对象呈递。

**`inline-bolock`块之间的距离会被保持父层字体的1/3大小的空间**

- 解决`inine-block`之间的距离
 **在父布局设置`font-size：0`**