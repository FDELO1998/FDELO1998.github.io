---
title: 手写bind apply call 函数
tags: javascript
---

首先简单说一下bind apply call在传参和返回值上的区别。bind 和 call第一个参数为要指向的对象，如果不传this指向window。后面的参数bind可以挨个往里面传,而call是要一次性传入。
apply后面的参数要以数组方式传入。
返回，apply和call都是直接返回改变了this的结果，而bind则返回一个永久改变this指向的函数。具体实现
### bind
``` javascript
Function.prototype.bindx = function() {
	//将参数拆解为数组
	const args = Array.prototype.slice.call(arguments)// arguments是object
	//获取this
	const t = args.shift()// 拿到数组第一个值
	// 
	const self = this// 修改this的指向
	return function(){
		return self.apply(t,args)// 这里也可以不使用原生的方法。看下面的例子
	 }
}
```
### call
``` javascript
Function.prototype.callx = function() {
	if (typeof this != 'function'){
		throw new TypeError('no function')
	}
	let obj = []
	const a =  arguments[0]
for(var i=1;i<arguments.length;i++){ // 这里没有使用原生方法，主要就是想记得如何处理不同的arguments参数，一般就是普通对象和数组模式
	obj.push(arguments[i])
}
	a.that = this // 这里的this指向的是调用本方法applyx的对象，就是A1
	let result = a.that(...obj)// 所以这里相当于在用A1()这个方法
	delete a.that // 这里that只是我们临时用来存this的，不删会改变原来的对象结构。
	return result
}
```
### apply
```javascript
Function.prototype.applyx = function() {
	if(typeof this != 'function'){
		throw new TypeError('no function')
	}
	let [center, arg] = arguments
    center.that = this
	let result = center.that(...arg); 
	//这里也可以用 this.call(center, ...arg);
	delete center.that;// 这里that只是我们临时用来存this的，不删会改变原来的对象结构。
	return result;
}
```
因为中间如果可以使用原生方法，那么可以有个更简洁的写法
```javascript
Function.prototype.applyx = function() {
	if(typeof this != 'function'){
		throw new TypeError('no function')
	}
	let [center, arg] = arguments
	return this.call(center,...arg);
}
```
测试的例子为
```javascript
function A1(a,b,c){
	console.log('this', this); // 这里的this指windows
	console.log(a,b,c);
	return 'this is A1';
}
// console.log(A1(2,3,4));
//  const fn1 = A1(2,3,4);
const fn2 = A1.applyx({x:100},[10,20,30]);
const fn2 = A1.callx({x:100},10,20,30);
const fn2 = A1.bindx({x:100},10,20,30);
console.log(fn2);
```