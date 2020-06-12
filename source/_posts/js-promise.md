---
title: Promise
date: 2020-04-08
tags: javascript
---

Promise是目前异步解决方式非常重要的一种。promise是一个对象，可以获取异步操作的消息。
Promise可以将异步的操作以同步流程表达，避免的层层嵌套会带来的回调地狱等问题，且其提供统一接口，更加方便控制异步。
缺点：无法取消。不设置回调函数，错误不会反应到外部。
### 基础用法

promise对象是个构造函数，用来生成promise实例。例如我们现在定义一个最基础的promise实例：
```javascript
const promise = new Promise(function(resolve,reject){
	resolve(value);
	reject(error);
	})
```
我们可以看到promise构造函数接受一个函数为参数，该函数的有两个参数分别是resolve,reject。这两个参数其实是两个函数，由js引擎提供。resolve的作用是将promise对象状态从未完成到成功，reject就是到失败了。两个函数都可以将异步操作的结果传递出去，即成为then回调的传入参数。如下所示:
```
promise.then(function(value){},function(error){})
```
写一个简单的使用例子：

```javascript
const promise = new Promise(function(resolve,reject){
	resolve('ok');
	reject('no');
	});
	promise.then((value) => {
		console.log(value);
	},(error) => {
		console.log(error);
	})
```
这里我们在promise中使用了两个函数并定义了成功或失败的参数，该程序运行结果是ok，因为resolve在前面，先执行把promise的状态从未完成到成功。promise的状态一旦修改是无法改变的，所以reject不会执行，如果把它位置放前面，那么返回结果就是no了。
如果在resolve后面再加上打印的语句，如：
```
resolve('ok');
console.log('123');
```
那么执行结果会是 123 ok。这里需要知道promise这两个函数是控制异步调用的，所以会在所有同步代码执行完之后执行。但是promise新建时立即执行，then在所有外部同步任务执行完才执行。这里看一个[阮一峰](https://es6.ruanyifeng.com/#docs/promise)老师的例子:
```javascript
const getJSON = function(url) {
  const promise = new Promise(function(resolve, reject){ //创建实例
    const handler = function() { //定义函数
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) { //当执行成功，就将response作为成功状态的回调函数的参数传出去。然后在第一个then中的value就会是它，这段代码对应值是下面的json
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
    const client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();
  });
  return promise;
};
getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json); //上面的this.response
}, function(error) {
  console.error('出错了', error); // 如果失败，传入的内容
});
```
注意按理说，promise调用了resolve或reject就该停止了，但是像上面console还是会执行的，所以如果不想后续内容继续，最好使用return resolve这样更严谨。

### Promise.prototype.then()
promise是具有then方法，then是定义在原型对象的prototype上的。之前说过它的参数就是状态回调函数，两个参数分别是两个函数，对应resolved和rejected。
```javascript
getJSON("/post/1.json").then(function(post) {
  return getJSON(post.commentURL);
}).then(function (comments) {
  console.log("resolved: ", comments);
}, function (err){
  console.log("rejected: ", err);
});
```
所以这里post来自getJSON的成功回调，comments来自上一个then的return，所以说每个then也是一个promise。

### Promise.prototype.catch()
弄清了前面，这个就比较好理解了。Catch专门用来指定发生错误时的回调。

```javascript
getJSON('/posts.json').then(function(posts) {
  // ...
}).catch(function(error) {
  // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});
```
另外，上面所有代码要是使用箭头函数将会更加整洁

```javascript
const promise = new Promise(function(resolve,reject){
	resolve('ok');
	reject('no');
	});
	promise.then(value => return value+"123")
	.then(result => console.log(result))
	// ok123
```
value = ok ; result = value+”123”

完结，撒花。