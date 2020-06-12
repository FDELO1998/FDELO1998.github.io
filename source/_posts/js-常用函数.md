---
title: js常见实用函数总结
date: 2020-04-13
tags: javascript
---

concat(): 可以把当前Array和另外一个Array连接起来，返回新的Array。，并且可以将Array自动拆开,但只能拆开一层，全部添加到新的Array里。如：
```javascript
var arr = ['A', 'B', 'C'];
var added = arr.concat(1, 4, [2, 3]);
 console.log(added); // ['A', 'B', 'C', 1, 4, 2, 3]
 var added = arr.concat(1, [4, [2, 3]]);
 console.log(added); // ['A','B','C',1,4,[2,3]]
 ```
 join()方法，用某符号连接所有元素
 ```javascript
 var arr = ['A', 'B', 'C', 1, 2, 3];
arr.join('-'); // 'A-B-C-1-2-3'
```
slice():
截取数组，从某一索引到某一索引。默认不加参数截取全部。
var … in … 循环：
```javascript
var o = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in o) {
    if (o.hasOwnProperty(key)) {
        console.log(key); // 'name', 'age', 'city'
    }
}
```
var in 循环的索引下标是字符串。不是数字，要注意。
reduce(): 可将每个元素执行一个算数运算：如下执行加法。返回最终值10

```
[0, 1, 2, 3, 4].reduce((accumulator, currentValue, currentIndex, array) => { return accumulator + currentValue; }, 10 )
```
apply() bind() call():
这三个函数都有修改this指向的功能。其中apply是需要数组做参数。apply和call用法相近。都是可以直接。apply和call的魅力在于可以动态指派别人的方法给自己。
比如：
```javascript
function A() {
		this.mas = "123";
		this.get = function() {
			return this.mas;
		}
	}
	var a = new A();
	console.log(a.get()); //123
	function B() {
		this.set = function(mss){
			this.mas = mss;
		}
	}
	var b = new B();
	b.set.call(a, '111'); 这里相当于把b的set方法借给了a用，无中生有，原理上是方法执行时上下文对象变了。等于a执行时对象调用b的set方法，但却与b没关系。
	console.log(a.get()); // 111
```

call, apply方法区别是,从第二个参数起,call方法参数将依次传递给借用的方法作参数,而apply直接将这些参数放到一个数组中再传递,最后借用方法的参数列表是一样的。
那么bind函数就是：
```javascript
const module = {
  x: 42,
  getX: function() {
    return this.x;
  }}
const unboundGetX = module.getX;
console.log(unboundGetX()); // The function gets invoked at the global scope
// expected output: undefined
const boundGetX = unboundGetX.bind(module);
console.log(boundGetX());
```
bind返回的是函数，其传参方式与Call一样。就像上面的例子如果使用bind需要 b.set.bind(a,’111’)();

js七种数据类型：Sting Object null undefined Array Boolean Number
js五种基本类型：String Boolean Number null undefined
typeof六种返回格式：’string’ ‘number’ ‘object’ ‘function’ ‘boolean’ ‘undefined’
