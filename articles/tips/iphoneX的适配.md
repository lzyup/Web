### 第一步:设置网页在可视窗口的布局
新增`viewport-fit`属性，使得页面内容完全覆盖整个窗口：
```
<meta name="viewport" content="width=device-width, viewport-fit=cover">
```
***注意只有设置了`viewport-fit=cover`,才能使用`env()`***


### 第二步:页面主题内容限定在安全区域内
这一步根据实际页面场景选择，不过不设置这个值，可能存在小黑条遮挡底部内容的情况
适配方式譬如：
```
body {
padding-bottom: constant(safe-area-inset-bottom); //兼容 iOS < 11.2
padding-bottom: env(safe-area-inset-bottom); //兼容iOS >=11.2
}
```

- `safe-area-inset-left`
安全区域距离左边边界距离

- `safe-area-inset-right`
安全区域距离右边边界距离

- `safe-area-inset-top`
安全区域距离顶部边界距离

- `safe-area-inset-bottom`
安全区域距离底部边界距离
