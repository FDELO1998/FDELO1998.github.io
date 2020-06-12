---
title: hexo主题搭建博客主题文件不生效
date: 2020-04-06
tags: ES6
---
看了网上很多人介绍的symbol，还是不能很好的理解这个类型，现在通过一些示例去总结一下。
首先，symbol类型的经典代码
```javascript
const color = Symbol('green');
	const colorre = Symbol('red');
	function isSafe(trafficLight) {
		if(trafficLight === color) return false;
		if(trafficLight === colorre) return true;
		throw new Error(`invalid trafficLight: ${trafficLight}`);
	}
```
首先，当我们打印color和colorre时，结果为
```javascript
Symbol(green) Symbol(red)
```
可见symbol的展示类型。再打印函数
```javascript
console.log(isSafe(color),isSafe(colorre));
```
得出
```javascript
false true
```
说明类型判断正确。那怎么体现了symbol的作用呢，symbol不仅可以解决属性名的冲突，也可以保证每个属性都独一无二。比如既然color和colorre都可以被函数识别，那我定义一个相同的，会是什么结果呢
```javascript
const color2 = Symbol('green');
```
再打印isSafe(color2),得到的结果是
```javascript
TypeError: "can't convert symbol to string"
```
说明即使定义的内容相同，对于symbol类型来说也都是不同的，除非是color2 = color,才能得到正确的结果


