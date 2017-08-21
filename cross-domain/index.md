# 跨域

> 跨域是由浏览器的同源策略引起的，是指页面请求的url地址，必须与浏览器上的url地址处于同域上（即域名，端口，协议相同）。这是为了防止某域名下的接口被其他域名下的网页非法调用，是浏览器对JavaScript施加的安全限制。

以下是常用的几种跨域通信解决方案:

## document.domain + iframe (只有在主域相同的时候才能使用该方法)
这两个域名必须属于同一个基础域名!而且所用的协议，端口都要一致，否则无法利用document.domain进行跨域

```
// http://testA.test.com/pageA
document.domain = 'test.com';
var cIframe = document.createElement('iframe');
cIframe.src = 'http://testB.test.com/pageB';
cIframe.display = none;
document.body.appendChild(cIframe);
cIframe.onload = function () {
    var data = cIframe.contentDocument || cIframe.contentWindow.document;
    // 在此可以在pageA中操作pageB的数据
    cIframe.onload = null;
};
// http://testB.test.com/pageB
document.domain = 'test.com';

```

## window.name 进行跨域
一个窗口(window)的生命周期内,窗口载入的所有的页面都是共享一个window.name的，每个页面对window.name都有读写的权限，window.name是持久存在一个窗口载入过的所有页面中的，并不会因新页面的载入而进行重置

```
(function () {
    window.dataRequest = {
        doc: document,
        cfg: {
            proxyUrl: 'proxy.html'
        }
    };
    dataRequest.send = function (url, callback) {
        var self = this,
            doc = self.doc;
        if (!url || typeof url !== 'string') return;
        url += (url.indexOf('?') > 0 ? '&' : '?') + 'windowname=true';
        // 
        var frame = doc.createElement('iframe'), state = 0, self = this;
        doc.body.appendChild(frame);
        frame.style.display = 'none';
        var clear = function () {
            try {
                frame.contentWindow.document.write('');
                frame.contentWindow.close();
                doc.body.removeChild(frame);
            } catch (e) { }
        };
        var getData = function () {
            try {
                var frameData = frame.contentWindow.name;
            } catch (e) { }
            clear();
            if (callback && typeof callback === 'function') {
                callback(frameData);
            }
        };
        //
        frame.onload = function () {
            if (state === 1) {
                getData();
            } else if (state === 0) {
                state = 1;
                frame.contentWindow.location = self.cfg.proxyUrl;
            }
        };
        frame.src = sUrl;
    }
})();

// 请求 http://test.com/getdata?windowname=true 
// 返回 <html><script> window.name="Hello"; </script></html>
dataRequest.send("http://test.com/getdata?windowname=true",function(data){})

```

## postMessage 实现
html5引入的message的API可以更方便、有效、安全的解决这些难题。postMessage()方法允许来自不同源的脚本采用异步方式进行有限的通信，可以实现跨文本档、多窗口、跨域消息传递。postMessage(data,origin)还用来解决： 1.页面和其打开的新窗口的数据传递 2.多窗口之间消息传递 3.页面与嵌套的iframe消息传递 4.上面三个问题的跨域数据传递
```
//  http://test.com/
<iframe src="http://testiframe.com/getData"></iframe>
// 
window.frames[0].postMessage('getcolor', 'http://testiframe.com');
window.addEventListener('message', function (event) {
    // 通过origin属性判断消息来源地址
    if (event.origin == 'http://testiframe.com') {
        // event.data  传递过来的数据
        // event.source 传递过来的window对象的引用，但由于同源策略，这里event.source不可以访问window对象
    }
}, false);

// http://testiframe.com/getData
window.addEventListener('message', function (event) {
    if (event.source == window.parent) {
        window.parent.postMessage(/* data */, '*');
    }
}, false);
```

## 动态创建script (JSONP 实现的方式 利用script标签可跨域的特点，在跨域脚本中可以直接回调当前脚本的函数)
JSONP是指JSON Padding，JSONP是一种非官方跨域数据交换协议，由于script的src属性可以跨域请求，所以JSONP利用的浏览器的这个“漏洞”,实现跨域请求。但因为src只能实现GET请求，限定JOSNP也只能实现GET请求。
```
var JSONP = {
    now: function () {
        return (new Date()).getTime();
    },
    rand: function () {
        return Math.random().toString().substr(2);
    },
    removeElem: function (elem) {
        var parent = elem.parentNode;
        if (parent && parent.nodeType !== 11) {
            parent.removeChild(elem);
        }
    },
    parseData: function (data) {
        var ret = "";
        if (typeof data === "string") {
            ret = data;
        }else if (typeof data === "object") {
            for (var key in data) {
                ret += "&" + key + "=" + encodeURIComponent(data[key]);
            }
        }
        ret += "&_time=" + this.now();
        ret = ret.substr(1);
        return ret;
    },
    getJSON: function (url, data, callback) {
        var name;
        if(!url || !data) return;
        if(typeof data == "function") {callback = data; data = null}
        if(data){
            url = url + (url.indexOf("?") === -1 ? "?" : "&") + this.parseData(data);
        }
        var match = /callback=(\w+)/.exec(url);
        if (match && match[1]) {
            name = match[1];
        } else {
            name = "jsonp_" + this.now() + '_' + this.rand();// 如果未定义函数名的话随机成一个函数名
            url = url.replace("callback=?", "callback=" + name);
            url = url.replace("callback=%3F", "callback=" + name);// 处理?被encode的情况
        }
        var script = document.createElement("script");
        script.src = url;
        script.id = "id_" + name;
        window[name] = function (json) {
            window[name] = null;// 执行这个函数后，要销毁这个函数
            var elem = document.getElementById("id_" + name);
            JSONP.removeElem(elem); 
            callback && callback(json);
        };
        var head = document.getElementsByTagName("head");
        if (head && head[0]) {
            head[0].appendChild(script);
        }
    }
};
//
JSONP.getJSON("http://test.com/getdata",function(data){})

```



## CORS(跨域资源共享)是由W3C制定的跨站资源分享标准，是最终的http跨域解决方案.
浏览器将CORS请求分成两类,简单请求（simple request）和非简单请求（not-so-simple request）
只要同时满足以下两大条件，就属于简单请求。
*请求方法是以下三种方法之一：HEAD GET POST
*HTTP的头信息不超出以下几种字段：Accept Accept-Language Content-Language Last-Event-ID Content-Type:(pplication/x-www-form-urlencoded multipart/form-data text/plain)
凡是不同时满足上面两个条件，就属于非简单请求。
1. 简单请求
对于简单请求，浏览器直接发出CORS请求。具体来说，就是在头信息之中，增加一个Origin字段。
```
Origin: http://api.bob.com
```
这个回应的头信息没有包含Access-Control-Allow-Origin字段,浏览器抛出一个错误，被XMLHttpRequest的onerror回调函数捕获。如果Origin指定的域名在许可范围内，服务器返回的响应，会多出几个头信息字段。
```
Access-Control-Allow-Origin: http://api.bob.com  //该字段是必须的。它的值要么是请求时Origin字段的值，要么是一个*，表示接受任意域名的请求
Access-Control-Allow-Credentials: true //该字段可选。它的值是一个布尔值，表示是否允许发送Cookie。默认false
Access-Control-Expose-Headers: FooBar //CORS请求时，XMLHttpRequest对象的getResponseHeader()方法只能拿到6个基本字段：Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma。如果想拿到其他字段，就必须在Access-Control-Expose-Headers里面指定
```
>发者必须在AJAX请求中打开withCredentials属性
```
var xhr = new XMLHttpRequest();
xhr.withCredentials = true;
```
2. 非简单请求
非简单请求是那种对服务器有特殊要求的请求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json。
非简单请求的CORS请求，会在正式通信之前，增加一次HTTP查询请求，称为"预检"请求（preflight）。浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错
预请求发送
```
OPTIONS /cors HTTP/1.1 //"预检"请求用的请求方法是OPTIONS
Origin: http://api.bob.com
Access-Control-Request-Method: PUT //该字段是必须的，用来列出浏览器的CORS请求会用到哪些HTTP方法，上例是PUT
Access-Control-Request-Headers: X-Custom-Header //该字段是一个逗号分隔的字符串，指定浏览器CORS请求会额外发送的头信息字段，上例是X-Custom-Header。
```
预请求返回
```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://api.bob.com
Access-Control-Allow-Methods: GET, POST, PUT //该字段必需，它的值是逗号分隔的一个字符串，表明服务器支持的所有跨域请求的方法。
Access-Control-Allow-Headers: X-Custom-Header //如果浏览器请求包括Access-Control-Request-Headers字段，则Access-Control-Allow-Headers字段是必需的
Access-Control-Allow-Credentials: true //该字段与简单请求时的含义相同。
Access-Control-Max-Age: 1728000 //该字段可选，用来指定本次预检请求的有效期，单位为秒
```
一旦服务器通过了"预检"请求，以后每次浏览器正常的CORS请求，就都跟简单请求一样，会有一个Origin头信息字段。服务器的回应，也都会有一个Access-Control-Allow-Origin头信息字段。

## web sockets 从协议上跨过同源策略 
在js创建了web socket之后，会有一个HTTP请求发送到浏览器以发起连接。取得服务器响应后，建立的连接会使用HTTP升级从HTTP协议交换为web sockt协议。当然这种通信只有在支持web socket协议的服务器上才能正常工作。
```
var socket = new WebSocket('ws://www.test.com'); //http->ws; https->wss
socket.send('hello WebSocket');
socket.onmessage = function(event){
    var data = event.data;
}
```
