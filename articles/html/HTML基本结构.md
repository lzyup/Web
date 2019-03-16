## 一、HTML文档
```
<!DOCTYPE html>                         <!--文档类型数据-->
<html lang="zh-cn">                     <!--表示HTML文档开始-->
<head>                                  <!--包含文档元数据开始-->
    <meta charset="utf-8">              <!--声明字符编码-->
    <title>文档结构</title>             <!--设置文档标题-->
</head>                                 <!--包含文档元数据结束-->

<body>                                  <!--表示HTML文档的内容-->

<a href="http://www.baidu.com">百度</a>   <!--一个超链接元素（标签）-->

</body>                                 <!--表示HTML文档的内容结束   -->

</html>                                 <!--表示HTML文档结束-->
--------------------- 
```

## 二、文档结构解析
### 1、Doctype
文档类型说明（Document Type Declaration,也称Doctype）。它主要告诉浏览器所查看的文件类型。

### 2、html元素
首先，元素就是标签的意思，html元素即html标签。html元素视文档开始和结尾的元素。它是一个双标签，头尾呼应，包含内容。这个元素有一个属性和值：lang="zh-cn"，表示文档采用语言为：简体中文。
`<html lang="zh-cn">    //如果视英文则为en`
### 3、head元素
用来包含元数据内容，元数据包括:<link>、<meta>、<noscript>、<script>、<style>、<title>。这些内容用来浏览器提供信息，比如link提供CSS信息，script提供JavaScript信息，title提供页面标题等。

`<head>...</head>`//这些信息在页面不可见

### 4、meta元素
这个元素来提供关于文档的信息，起始结构有一个个属性为:charset="utf8"。表示告诉浏览器页面采用什么编码，一般来说我们就用utf8。当然，文件保存的时候也是utf8，而浏览器也设置utf8即可正确显示中文。
`<meta charset="urf-8"> //除了设置编码，还有别的`

### 5、title元素
设置浏览器左上角的标题
`<title>基本结构</title>`

### 6、body元素
用来包含文档内容的元素，也就是浏览器可见区域。所有的可见内容，都应该在这个元素内部进行添加。
`<body>...</body>`

### 7、a元素
一个超链接
`<a href="http://www.baidu.com">`百度</a>


