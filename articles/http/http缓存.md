## 什么是http缓存？
 指的是浏览器在本地磁盘对用户最近请求过的文档进行存储，当访问者再次访问同一页面时，浏览器就可以直接从本地磁盘加载文档。
 ### 优点
 - 减少了冗余的数据传输
 - 减少了服务器的负担，大大提升了网站的性能
 - 加快了客户端加载网页的速度

## 缓存相关的header
- Expires  `响应头，代表该资源的过期时间`
- Cache-Control `请求/响应头，缓存控制字段，精确控制缓存策略`

    主要是利用该字段的`max-age`值来判断，譬如Cache-Control:max-age=3600,代表资源的有效期是3600秒，还有几个常用字段是
    
    （1）no-cache:不使用本地缓存。需要使用缓存协商，先与服务器确认返回的响应头是否被更改，如果之前的响应头中存在ETag,那么请求的时候会与服务器端验证，如果资源为被更改，则可以避免重新下载。
    （2）no-store:直接禁止浏览器缓存数据，每次用户请求该资源，都会向服务器发送一个请求，每次都会下载完整的资源。
    （3）public:可以被所有的用户缓存，包括终端用户和CDN等中间代理服务器
    （4）private:只能被终端用户的浏览器缓存，不允许CDN等中间代理服务器对其缓存
    
    **另外，`Cache-Control`与`Expires`可以在服务器端配置同时启用，同时启用的时候`Cache-Control优先级高`**
    
- If-Modified-Since `请求头，资源最近修改时间，由浏览器告诉服务器` 
- Last-Modified `响应头，资源最近修改时间，由服务器告诉浏览器，如果命中缓存，则返回304，并且不会返回资源内容，也不会返回Last_Modified`
- ETag `响应头，资源标识，由服务器告诉浏览器,如果命中缓存，由于ETag重新生成，响应头中还是会把这个ETag返回，即使这个ETag跟之前没有变化`
- If-None-Match `请求头，缓存资源标识，由浏览器告诉服务器`

## 为什么要用`ETag`
- `If-Modified-Since`是秒级别的
- 如果服务器上文件被修改了，但是实际上内容根本没发生改变（仅仅改变修改的时间），也会因为`Last-Modified`时间匹配不上而重新返回数据
- 某些服务器不能精确的得到文件的最后修改时间


## 缓存分类
 http缓存分为两类：`协商缓存`和`强缓存`。
 
 浏览器在第一次请求发生后，再次请求时：
 
 1、浏览器会先获取该资源缓存的header信息，根据其中的`expires`和`cache-control`判断是否命中`强缓存`，若命中则直接从缓存中获取资源，包括缓存的header信息，本次请求不会与服务器进行通信。
 
 2、如果没有命中强缓存，浏览器会发送请求到服务器，改请求会携带第一次请求返回的有关缓存的header字段信息（Last-Modified/IF-Modified-Since、Etag/IF-None-Match）,由服务器根据请求中的相关header信息来对比结果是否命中协商缓存，若命中，则服务器返回新的响应header信息更新缓存中的对应header信息，但是并不返回资源内容，它会告知浏览器可以直接从缓存中获取；否则返回最新的资源内容。
 
 
 ### 强缓存
 强缓存是利用http的返回头中的`Expires`或者`Cache-Control`两个字段来控制的，用来表示资源的缓存时间（对应状态码200）

### 协商缓存
协商缓存就是由服务器来确定缓存资源是否可用，所以客户端与服务器端要通过某种标识来进行通信，从而让服务器判断请求资源是否可以缓存访问，这主要涉及到下面两组header字段，这两组搭档都是成对出现的，即第一次请求的响应头带上某个字段（`Last-Modified`或者`Etag`）,则后续请求则会带上对应的请求字段（`If-Modified-Since`或者`If-None-Match`），若响应头没有Last-Modified或者Etag字段，则请求头也不会有对应的字段（对应状态码304）


### 情景分析
- 浏览器请求a.js
- 服务器返回a.js,同时告诉浏览器的过期绝对时间（Expires）和相对时间（Cache-Control:max-age=10）,以及a.js上次修改时间Last-Modified,以及a.js的Etag.
- 10s内浏览器再次请求a.js,不在请求服务器，直接使用本地缓存
- 11s时，浏览器再次请求a.js，请求服务器，带上上次修改时间If-Modified-Since和上次的Etag值If-None-Match.
- 服务器收到浏览器的If-Modified-Since和If-None-Match,发现了  If-None-Match,则比较If-None-Match和a.js的Etag值，忽略If-Modified和If-Modified-Since的比较。
- a.js文件内容没有变化，则Etag和If-None-Match一致，服务器告诉浏览器继续使用本地缓存（304）
 






[参考链接1](http://caibaojian.com/browser-cache.html)

 [参考链接2](https://juejin.im/post/5b3c87386fb9a04f9a5cb037)
 