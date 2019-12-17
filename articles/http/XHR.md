### XMLHttpRequest
`XMLHttpRequest`是一个API,它为客户端提供了在客户端和服务器之间传输数据的功能。它提供了一个通过URL来获取数据的简单方式，并且不会使整个页面刷新。这使得网页只更新一部分页面而不会打扰到用户

### XMLHttpRequest使用
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
    用于表示请求的五种状态

值 | 状态 | 描述
---|---|---
0 | UNSEND(未打开) | 表示已创建XHR对象，open()方法还未被调用
1 | OPEN(未发送)|open()方法已被成功调用，send()方法还未被调用
2 | HEADER_RECEIVED(已获取响应头)|send()方法已被调用，响应头和响应状态已经返回
3|LOADING(正在下载响应体)|响应体下载中，responseText中已经获取了部分数据
4|DONE(请求完成)|整个请求过程已经完毕

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


    