
### URL
URL是Uniform Resource Locator的简写，统一资源定位符

#### 组成部分
`scheme://host:port/path?query-string=xxx#achor`

- scheme</br>
代表的是访问的协议，一般情况下是http或者https以及ftp等。
- host</br>
主机名，域名，比如`www.baidu.com`
- path</br>
查找路径。比如：`www.jianshu.com/trending/now`,后面的`trending/now`就是path
- query-string</br>
查询字符串，比如:`www.baidu.com/s?wd=python`,后面的`wd=python`就是查询字符串。
- anchor</br>
锚点，后台一般不用管，前端用来做页面定位的

### 网址（url）和域名的关系
网址是打开网站的地址；
比如：http://zhidao.baidu.com/question/263101382.html?push=core&group=1

这个是网址；

**域名是zhidao.baidu.com，
指http://前面到第一个/中间的一段。**

### 1、Http方法

方法 | 含义
---|---
GET | 获取资源
POST | 向服务器端发送数据，传输实体主体
PUT | 传输文件
HEAD | 获取报文首部
DELETE | 删除文件
OPTIONS | 询问支持的方法
TRACE | 追踪路径

### 2、HTTP状态码

类别 | 原因
---|---
1XX | Informational(信息性状态码)
2XX | Success(成功状态码)
3XX | Redirection(重定向)
4XX | Client Error(客户端错误状态码)
5XX | Server Error(服务器错误状态码)

#### 2XX成功
```
200(OK 客户端发过来的数据被正常处理)
204(Not Content 正常响应，没有实体)
206(Partial Content 范围请求，返回部分数据，响应报文中由Content-Range指定实体内容)
```

#### 3XXX重定向
```
301(Moved Permanently) 永久重定向
302(Found) 临时重定向，规范要求，方法名不变，但是都会改变
303(See Other)和302类似，但必须用GET方法
304(Not Modified)状态未改变，配合(If-Modified-Since、If-None-Match)
307(Temporay Redirect)临时重定向，不改变请求方法
```
### 4XXX客户端错误
```
400(Bad Request)请求报文语法错误
401(unauthorized)需要认证
403(Forbidden)服务器拒绝访问对应的资源
404(Not Found)服务器上无法找到资源
```
#### 5XXX服务器端错误
```
500(Internal Server Error)服务器故障
503(Service Unavailable)服务器处于负载或正在停机维护
```

### HTTP之请求头部结构
#### 1、请求行
请求行分为三个部分：请求方法、请求地址和协议版本

#### 2、请求头部
从第二行起为请求头部，HOST将指出请求的目的地。User-Agent，服务器端和客户端脚本都能访问它，它是浏览器类型检测逻辑的重要基础。该信息由你的浏览器来定义，并且在每个请求中自动发送等等。

#### 3、空行
请求头部后面的空行是必须的，即使第四部分的请求数据为空，也必须空行

#### 4、请求体
请求体是post请求方式中的请求参数，以key = value形式进行存储，多个请求参数之间用&连接，如果请求当中请求体，那么在请求头当中的`Content-Length`属性记录的就是该请求体的长度

eg:
```
POST / HTTP1.1
Host:www.wrox.com
User-Agent:Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; .NET CLR 3.0.04506.648; .NET CLR 3.5.21022)
Content-Type:application/x-www-form-urlencoded
Content-Length:40
Connection: Keep-Alive

name=Professional%20Ajax&publisher=Wiley
```

```
 GET /books/?sex=man&name=Professional HTTP/1.1
 Host: www.example.com
 User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6)
 Gecko/20050225 Firefox/1.0.1
 Connection: Keep-Alive
```


#### HTTP的Head头

Header | 解释 | 示例
---|---|---
Accept| 指定客户端能够接收的内容类型| Accept:text/plain,text/html
Accept-Charset | 浏览器可以接受的字符串编码集|Accept-Charset:iso-8859-5
Accept-Encoding|指定浏览器可以支持的web服务器返回内容压缩编码类型|Accept-Encoding:compress,gzip
Accept-Language | 浏览器可接受的语言 | Accept-Language:en,zh
Accept-Ranges|可以请求网页实体的一个或者多个子范围字段|Accept-Range:bytes
Authorization|Http授权的授权证书|Authorization:BasicQWxhZGRpbjpvcGVuIHNlc2FtZQ==
Cache-Control|指定请求和响应遵循的缓存机制|Cache-Control:no-cache
Connection|表示是否需要持久连接。（HTTP1.1默认进行持久连接）|Connection:close
Cookie|Http请求发送时，会把保存在该请求域名下的所有cookie值一起发送给Web服务器|Cookie:$Version=1:Skin=new;
Content-Length|请求的内容长度|Content-Length:348
Content-Type|请求的实体对应的MIME信息|Content-Type:application/x-www-form-urlencode
Date|请求发送的日期和时间|Date:Tue,15 Nov 2010 08:12:31 GMT
Expect|请求的特定的服务器行为|Expect:100-continue
From|发出请求的用户的Email|From:user@email.com
Host|**指定请求的服务器的域名和端口号**|Host:www.zcmhi.com
If-Match|只有请求内容与实体相匹配才有效|If-Match:“737060cd8c284d8af7ad3082f209582d”
If-Modified-Since|如果请求的部分在指定时间之后被修改则请求成功，未被修改则返回304|If-Modified-Since:Sat,29 Oct 2010 19:43:31 GMT
If-Range|如果实体未改变，服务器发送客户端丢失的部分，否则发送整个实体。参数也为Etag|If-Range:“737060cd8c284d8af7ad3082f209582d”
If-Unmodified-Since|只在实体在指定时间之后未被修改才请求成功|If-Unmodified-Since:Sat,29 Oct 2010 19:43:31 GMT
Max-Forwards|限制信息通过代理和网关传送的时间|Max-Forwards:10
Pragma|用来包含实现特定的指令|Pragma:no-cache
Proxy-Authorization|连接到代理的授权证书|Proxy-Authorization:Basic QWxhZGRpbjpvcGVuIHNlc2FtZQ==
Range|只请求实体的一部分，指定范围|Range:bytes=500-999
Referer|先前网页的地址，当前请求网页紧随其后,即来路|Referer:http:
TE|客户端愿意接受的传输编码，并通知服务器接受接受尾加头信息|TE:trailers,deflate;q=0.5
Upgrade|向服务器指定某种传输协议以便服务器进行转换（如果支持）|Upgrade:HTTP/2.0,SHTTP/1.3,IRC/6.9,RTA/x11
User-Agent|User-Agent的内容包含发出请求的用户信息|User-Agent:Mozilla/5.0(Linux;X11)
Via|通知中间网关或代理服务器地址，通信协议|Via:1.0 fred,1.1 nowhere.com (Apache/1.1)
Warning|关于消息实体的警告信息|Warn:199 Miscellaneous warning

### HTTP之响应头部结构
#### 1、状态行
由HTTP协议版本号，状态码，状态消息三部分组成

#### 2、消息报头
用来说明客户端要使用的一些附加信息

#### 3、空行
消息报头后面的空行是必须的

#### 4、响应正文
服务器返回给客户端的文本信息

eg:输入`curl -I www.baidu.com`
```
HTTP/1.1 200 OK
Accept-Ranges: bytes
Cache-Control: private, no-cache, no-store, proxy-revalidate, no-transform
Connection: keep-alive
Content-Length: 277
Content-Type: text/html
Date: Thu, 19 Dec 2019 10:48:12 GMT
Etag: "575e1f72-115"
Last-Modified: Mon, 13 Jun 2016 02:50:26 GMT
Pragma: no-cache
Server: bfe/1.0.8.18
```

eg:输入`curl -I geekbang.org`
```
HTTP/1.1 301 Moved Permanently
Date: Thu, 19 Dec 2019 09:40:46 GMT
Content-Type: text/html
Content-Length: 178
Connection: keep-alive
Location: https://www.geekbang.org/
Strict-Transport-Security: max-age=15768000
```

#### 常见响应头

Header | 解释 | 示例
---|---| ---
Date | 原始服务器消息发出的时间 | Date: Tue, 15 Nov 2010 08:12:31 GMT
Last-Modified | 请求资源的最后修改时间 | Last-Modified: Tue, 15 Nov 2010 12:45:26 GMT
ETag | 请求变量的实体标签的当前值 | ETag: “737060cd8c284d8af7ad3082f209582d”
Location| 重定向到另一个 URL，如输入浏览器就输入 baidu.com 回车，会自动跳转到www.baidu.com 就是通过这个响应头控制的|Location:http://www.zcmhi.com/archives/94.html


[参考链接1](https://note.youdao.com/)




