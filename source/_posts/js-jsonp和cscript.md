---
title: jsonp实例测试和script
date: 2020-04-11
tags: javascript
---

之前文章总结了跨域的常见方式，现在补充一下jsonp方式的一些操作实例。
首先我们在我们的文件里先定义一个json文件，写个函数。内容如下：
```
api({ "app":1,"slay":2})
```
然后我们在script当中，用script标签去引用这个文件。添加回调参数。
```javascript
<script>
 var api = function (data) {
     console.info(data);
}
</script>
<script src ="./api.json/?&callback=api"></script>
```

控制台打印出来的结果是Object { “app”:1,”slay”:2 }。这里callback函数是核心，因为我们在json文件里封装了这样一个函数，我们想要的值app,slay会跟着请求的回调函数api作为它的data返回回来。也就是说：服务端接口到get请求，返回的是api({ “app”:1,”slay”:2})，这样的话在服务端不就可以把数据通过函数执行传参的方式实现数据传递了吗。然后我们在服务端就可以通过重新编写函数内容去打印参数了吗。另外，为什么script就可以实现跨域呢。

因为对于某个执行请求脚本A的html文件，当它的脚本运行时，A脚本是嵌入在该HTML的script标签里的，所以该脚本判断来源时会认为它们都是同一个域下的脚本。所以不会拦截。这也是a标签，iframe标签都能多多少少跨域的道理。

这里插一个小知识点：浏览器请求的时候怎么判断该资源是请求过的，然后返回304的呢。因为第一次请求后文件被缓存下来，在时效没过时，下一次的响应头http header中会有expire(http 1.0) / max-age(http 1.1)字段，表示你上次请求的时间和内容大小。那么浏览器会直接访问内存读取。