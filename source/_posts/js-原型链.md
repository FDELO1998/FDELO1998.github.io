---
title: 原型链
date: 2020-04-09
tags: javascript
---

##理解对象

创建自定义对象最简单的方法就是创建一个Object实例，再去添加属性和方法。如
```javascript
var person = new Objext();
person.name = "meg"
person.sayname = function() {
	alert(this.name);
}
当然也可以使用对象字面量语法去写：
var parson = {
	name: "meg",
	sayname: function(){
		alert(this.name);
	}
}
```
属性类型是指描述属性的各种特征。想修改属性特征必须使用Object.defineProperty()方法。
接受三个属性：对象，属性名，描述符对象。描述符对象有四个属性configurable（是否可改变状态）、enumerable、writable（是否可改写） 和 value（值）
```javascript
var person = {}; 
Object.defineProperty(person, "name", { 
 writable: false, 
 configurable: false,
 enumerable: false,
 value: "Nicholas" 
});
```

访问器属性：包含一对儿getter和setter函数，getter负责返回有效值，setter负责传入新值并决定如何处理数据。
```javascript
var book = { 
 _year: 2004, 
 edition: 1 
}; 
Object.defineProperty(book, "year", { 
 get: function(){ 
 return this._year; 
 }, 
 set: function(newValue){ 
 	console.log(newValue);//2005
 if (newValue > 2004) { 
 this._year = newValue; 
 this.edition += newValue - 2004; 
 } 
 } 
}); 
book.year = 2005; 
alert(book.edition);
```
setter接受的参数为getter返回的。
js经历了从工厂模式到构造函数模式。首先我们可以自定义的创建构造函数。比如：

```javascript
function Person(name){
	this.name = name;
    this.sayname = function(){
    	alert(this.name);
    }
}
var person = new Person("meg")
而对应的工厂模式是：
function creatPerson(name) {
	var o = new Object();
	o.name = name;
	o.sayname = function(){
		alert(this.name);
	}
	return o;
}
var person = new creatPerson("meg");
```
不同之处很明显，构造函数模型没有显示创建对象，没有return，直接把属性和给了this对象。
构造函数创建实例经过四个部分。创建一个新对象（person），将构造函数的作用域赋给新对象（this指向person）执行构造函数代码（属性方法赋给person）返回新对象。所以person对象保存了Person的一个实例，这个对象都有一个constructor属性，指向Person。既：
person.constructor == Person;
检测对象类型alert(person1 instanceof Object); //true
alert(person1 instanceof Person); //true
在另一个对象作用域中调用：

```javascript
var o = new Object();
Person.call(o, "meg")
o.sayName(); // meg
```

## 原型模型
我们创建的每个函数都有一个prototype（原型）属性。理解原型模型就要理解原型对象。每个我们创建的新函数都有一个prototype属性，这个属性指向函数的原型对象。这个原型对象除了自身的属性和方法还有个（构造函数）属性constructor，指向prototype所在函数。如图
![原型对象]('../image/原型.png')
从图中可见，Person.prototype指向原型对象，原型对象的constructor指回Person。而且Person的实例person1和person2都包含一个内部属性，指向原型对象，这个内部属性在谷歌等浏览器中定义为_proto_。它们与构造函数都没有直接的关系发现了吗？它们都共享原型对象的属性和方法。所以可以调用person1.sayname()。
还有，实例中如果创建了属性与原型对象的相同，那么会覆盖原型对象的。但原型对象的内容并不会被它改变。
原型对象在共享属性方法方面有绝对的优势，但是对于每个实例需要有属于自己的属性时尤其是引用的属性。那它的共享就会成为缺点。所以接下来又是另外一种方法，就是组合使用构造函数模型和原型模型。

## 组合使用构造函数模式和原型模式
```javascript
function Person(name, age, job){ 
 this.name = name; 
 this.age = age; 
 this.job = job; 
 this.friends = ["Shelby", "Court"]; 
} 
Person.prototype = { 
 constructor : Person, 
 sayName : function(){ 
 alert(this.name); 
 } 
} 
var person1 = new Person("Nicholas", 29, "Software Engineer"); 
var person2 = new Person("Greg", 27, "Doctor"); 
person1.friends.push("Van"); 
alert(person1.friends); //"Shelby,Count,Van" 
alert(person2.friends); //"Shelby,Count" 
alert(person1.friends === person2.friends); //false 
alert(person1.sayName === person2.sayName); //true
```
从这个例子很容易看出组合使用的好处，这样数组是构造模型，修改不会影响另一个实例的使用。

## 继承-原型链
原型链方法主要是用来解决继承问题。基本思想是利用原型让一个引用类型继承另一个引用类型的属性和方法。就是原来原型对象的constructor指向的构造函数本身。那现在我们让这个指针指向另外一个原型对象，也就是成为另外一个类型的实例。此时被指向的原型对象也会有一个内在指针constructor指向它的构造函数。如此层层递进就构成了实例与原型的链条。
![原型2]('../image/原型2.png')

从上图我们能看见后面的实例可以继承前面所有原型的属性和方法。另外不能忘记所有的引用类型都继承了object。所有函数的默认模型都是object的实例，这就是为什么所有的自定义模型都会有tostring() valueof()等默认方法。如下图
![原型链]('../image/原型链.png')

