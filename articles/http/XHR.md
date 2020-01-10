### XMLHttpRequest
`XMLHttpRequest`是一个API,它为客户端提供了在客户端和服务器之间传输数据的功能。它提供了一个通过URL来获取数据的简单方式，并且不会使整个页面刷新。这使得网页只更新一部分页面而不会打扰到用户。

### XMLHttpRequest1使用
```
    var xhr = new XMLHttpRequest();//创建xhr对象
    xhr.open(method,url)
    xhr.onreadystatechange = function(){...}
    xhr.setRequestHeader(...,...);
    xhr.send(optionalEncodeData)
```

### XMLHttpRequest属性
- onreadystatechange:Function</br>
    当readyState属性改变时会调用它
- readyState</br>
    返回一个XMLHttpRequest代理当前所处的状态。

值 | 状态 | 描述
---|---|---
0 | UNSEND(未打开) | 表示已创建XHR对象，open()方法还未被调用
1 | OPEN(未发送)|open()方法已被成功调用，send()方法还未被调用
2 | HEADER_RECEIVED(已获取响应头)|send()方法已被调用，响应头和响应状态已经返回
3|LOADING(正在下载响应体)|响应体下载中，responseText中已经获取了部分数据
4|DONE(请求完成)|整个请求过程已经完毕

- status</br>
返回XMLHttpRequest响应中的状态码


### XMLHttpRequest方法
- `open()`</br>
XMLHttpRequest.open()方法初始化一个请求。
- `send()`</br>
发送HTTP请求。如果是异步请求（默认为异步请求），则此方法会在请求发送后立即返回；如果是同步请求，则此方法直到响应到达后才会返回。XMLHttpRequest.send()方法可以接受一个可选的参数，其作为**请求主体**；如果请求方法是GET或者HEAD,则应该请求主体设置为null。




### XMLHttpRequest2级的改进

- 可以设置HTTP请求的超时时间
- 可以使用FormData对象管理表单数据
- 可以上传文件
- 可以请求不用域名下的数据（跨域请求）
- 可以获取服务器端的二进制数据
- 可以获取数据传输的进度信息

### XMLHttpRequest2级新增事件
- `loadstart`</br>
在接收响应数据的第一个字节时触发
- `progress`</br>
在接收响应期间持续不断地触发。
- `error`</br>
在请求发生错误时触发
- `abort`</br>
在因为调用`abort()`方法而终止连接时触发
- `load`</br>
在接收到完整的响应数据时触发
- `loadend`</br>
在通信完成或者触发error、abort或load事件后触发
- timeout</br>
设置超时时间
- `ontimeout`</br>
当超时后执行的回调

eg:
```
    function GetWebData(URL){
        //1、新建XMLHttpRequest请求对象
        let xhr = new XMLHttpRequest();

        xhr.onreadystatechange = function () {
            switch (xhr.readyState) {
                case 0:
                    console.log('请求未初始化');
                    break;
                case 1:
                    console.log("OPEN");
                    break;
                case 2:
                    console.log("HEADERS_RECEIVED");
                    break;
                case 3:
                    console.log("LOADING");
                    break;
                case 4:
                    if(this.status == 200 || this.status == 304){
                        console.log(this.responseText);
                    }
                    console.log("DONE");
                default:
                    break;
            }     
        }

        xhr.ontimeout = function (e) {
            console.log("ontimeout");
        }

        xhr.onerror = function (e) {
            console.log("onerror");
        }

        xhr.open("Get",URL,true);//创建一个Get请求，采用异步

        //配置参数
        xhr.timeout = 3000; //设置xhr请求的超时时间
        xhr.responseType = "text";//设置响应返回的数据格式
        xhr.setRequestHeader("X_TEST","time.geekbang");

        //发送请求
        xhr.send();

    }
```

eg2:
```
export default (type= 'GET' , url = '',data={},async=true)=>{
    return new Promise ((resolve,reject)=>{
        type = type.toUpperCase();

        let requestObj;
        if(window.XMLHttpRequest){
            requestObj = new XMLHttpRequest();
        }else{
            requestObj = new ActiveXObject;
        }

        if(type == 'GET'){
            let dataStr = '';//数据拼接字符串
            Object.keys(data).forEach(key=>{
                dataStr += key + '=' + data[key] + '&';
            })
            dataStr = dataStr.substr(0,dataStr.lastIndexOf('&'));
            url = url + '?' + dataStr;
            requestObj.open(type,url,async);
            requestObj.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
            requestObj.send();
        }else if(type == 'POST'){
            requestObj.open(type,url,async);
            requestObj.setRequestHeader('Content-type',"application/json");
            requestObj.send(JSON.stringify(data));
        }else{
            reject('error type')
        }
        requestObject.onreadystatechange = () => {
            if (requestObject.readyState == 4) {
                if (requestObject.status == 200) { }
                let obj = requestObj.response;
                if (typeof obj !== 'object') {
                    obj = JSON.parse(obj);
                }
                resolve(obj);

            } else {
                reject(requestObj)
            }
        }
    })
}
```

[XHR1和XHR2详解](https://juejin.im/post/58e4a174ac502e006c1e18f4)
    