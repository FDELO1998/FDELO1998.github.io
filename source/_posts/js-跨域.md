---
title: 跨域资源共享
date: 2020-04-27
tags: javascript
---

首先什么叫跨域：
跨域来源于同源策略，也就是限制一个源加载的文档或脚本如何与来自另一个源的资源进行交互。同源策略是保护隔离潜在恶意文件的关键的安全机制。
定义为：一个脚本向后台申请数据时只能读取同一协议名，同一主机名，同一端口号下的数据。当不满足上述条件时，我们称为违背同源策略，这时候就要进行跨域。

## 实现跨域的方法
主要分为架设代理服务器，JSONP和CORS三种方案实现跨域

### JSONP系列方法
1.后台头部设置 Access-Control-Allow-Origin:来表示允许什么域名去访问，表示所有域名
2.使用SRC+jsonp实现跨域 
```
<script src="http://127.0.0.1/json.php">< /script>
后台会返回回调函数，echo "callback({$json})"; 在script中通过
function callback(data){
alert("请求成功!!");
console.log(data);} 回调成功。
```
3.jquery ajax 实现jsonp
设置datatype属性为jsonp，后台依旧需要返回回调函数，这样ajax在发送请求时会默认get将回调函数名给后台。

### CORS方法
需要浏览器和服务器同时支持，目前所有浏览器都支持，除低版本IE。
实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。
对于CORS分为简单请求和非简单请求。
对于简单请求(get,post,head)，由浏览器直接发送，自动添加Origin字段。如果发送的这个源，服务器判断在不在许可范围，不在的话就返回正常的回应，那浏览器会发现没有Access-Control-Allow-Origin字段就知道出错，通过回调函数捕获错误。这种错误状态码不会识别出来，所以可能会显示200.
如果许可，返回信息会多几个字段
```
Access-Control-Allow-Origin: http://api.bob.com
Access-Control-Allow-Credentials: true
Access-Control-Expose-Headers: FooBar
Content-Type: text/html; charset=utf-8
```

上面这些是默认不发送Cookies和http验证信息的，那如果要发送的话，服务器端要设置Access-Control-Allow-Credentials: true 属性，发送端的ajax要设置 xhr.withCredentials = true;有些浏览器默认这个属性是true，那我们也可以通过设置去关闭。
注意：如果要发送cookies，Access-Control-Allow-Origin就不能设为星号，必须指定明确的、与请求网页一致的域名。 cookies依旧遵照同源策略
对于非简单请求（put,delete,或者Content-Type字段的类型是application/json) 正式通信前会进行预检。浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错。
服务器收到后，检查字段，确认允许跨域，回应(Origin、Access-Control-Request-Method和Access-Control-Request-Headers)。
当预检通过后，浏览器检查了各个字段，才会发送正式的请求。
CORS与JSONP的使用目的相同，但是比JSONP更强大。
JSONP只支持GET请求，CORS支持所有类型的HTTP请求。JSONP的优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。

[阮一峰老师的文章真是666啊](http://www.ruanyifeng.com/blog/2016/04/cors.html)

###架设代理服务器
自然就是用后台语言搭个服务器啦，如nodejs，毕竟服务器之间是没有跨域之说的。
