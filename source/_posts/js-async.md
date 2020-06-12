---
title: 详细理解async和await
date: 2020-05-16
tags: javascript
---

async首先从定义上，是异步的简写，而await可以认为是async wait的简写。所以可以很好的理解为async使用声明一个function是异步的，await用来等待一个异步方法执行完成。 语法规定就是await只能在async中间使用。
async起的作用就是将函数（函数语句，函数表达式，lambda表达式）返回成promise对象。如果直接return 会把直接量通过promise.resolve封装成promise对象。
以下对于async的写法皆是正确的
```javascript
function getSomething() {
    return "something";
}

async function testAsync() {
    return Promise.resolve("hello async");
}

async function test() {
    const v1 = await getSomething();
    const v2 = await testAsync();
    console.log(v1, v2);
}
test();
// 返回something hello async
```
如果有错误代码，如：
```javascript
async function doIt(f) {
		return cee;
	//await doIt(pay);
}
```
返回的就是promise的rejected报错
proto: Promise
[[PromiseStatus]]: “rejected”
[[PromiseValue]]: ReferenceError: cee is not defined at doIt (file:///F:/test.html:45:3) at window.onload (file:///F:/test.html:63:13)

单一的 Promise 链并不能发现 async/await 的优势，但是，如果需要处理由多个 Promise 组成的 then 链的时候，优势就能体现出来了。
```javascript
function takeLongTime(n) {
    return new Promise(resolve => {
        setTimeout(() => resolve(n + 200), n);
    });
}

function step1(n) {
    console.log(`step1 with ${n}`);
    return takeLongTime(n);
}

function step2(n) {
    console.log(`step2 with ${n}`);
    return takeLongTime(n);
}

function step3(n) {
    console.log(`step3 with ${n}`);
    return takeLongTime(n);
}
function doIt() {
    console.time("doIt");
    const time1 = 300;
    step1(time1)
        .then(time2 => step2(time2))
        .then(time3 => step3(time3))
        .then(result => {
            console.log(`result is ${result}`);
            console.timeEnd("doIt");
        });
}

doIt();
async function doIt() {
    console.time("doIt");
    const time1 = 300;
    const time2 = await step1(time1);
    const time3 = await step2(time2);
    const result = await step3(time3);
    console.log(`result is ${result}`);
    console.timeEnd("doIt");
}

doIt();
```
async和await的好处就在于：1，可以快速定义异步函数，不用在函数内部再new promise（定义也可）。2，将异步调用代码，写起来像同步调用，精简和清晰了代码。也更方便理解了。在参数传递上，也比用promise更加方便。
```javascript
async function doIt() {
    console.time("doIt");
    const time1 = 300;
    const time2 = await step1(time1);
    const time3 = await step2(time1, time2);
    const result = await step3(time1, time2, time3);
    console.log(`result is ${result}`);
    console.timeEnd("doIt");
}
```
同时async只会阻塞函数内部同步代码，外部代码正常异步执行。
```
function getSomething() {
	console.log("我是最先执行的");
	var a = 0;
    return a++;
}
async function test() {
    const v1 = await getSomething();
    console.log(v1);
}
test();
console.log('我是先执行的');

//我是最先执行的
//我是先执行的
//0
```
因为 await 内部实现了 generator ，generator 会保留堆栈中东西。
若是添加promise

```javascript
function getSomething() {
	console.log("我是最先执行的");
	var a = 0;
    return a++;
}
async function test() {
    const v1 = await getSomething();
    console.log(v1);
}
test();
console.log('我是先执行的');

new Promise((resolve, reject) => {
  console.log('async2 end')
  resolve(Promise.resolve());
}).then(() => {
  console.log('async1 end')
})

我是最先执行的
我是先执行的
async2 end
0
async1 end
```