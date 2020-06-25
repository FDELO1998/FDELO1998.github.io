---
title: event-loop
tags: javascript
---

### event loop事件循环

是异步回调的实现原理（机制）,js代码的执行一行一行来，遇错停止，先同步后异步。

事件循环过程：四个模块（call stack调用栈 ，Web APIs浏览器定义的API，callback Queue回调队列, event loop
一般情况：同步代码推入调用栈，执行完清空。
有定时任务执行settimeout，放入调用栈，然后在web apis中创建定时任务，同时传入函数。
调用栈清空，等待定时后把函数放入回调队列。这中间后面的同步代码不受定时的阻拦，直接进入调用栈执行。 
调用栈为空后执行eventloop去拿回调队列中的函数送进调用栈。然后执行函数体内的内容。

复杂情况：考虑dom渲染和宏微任务

事实上，在调用栈空闲时会先尝试dom渲染，所以如果有渲染会先执行。如果碰到promise函数，会先放到micro task queue中，不会放到web apis中（因为他不是w3c的规范，是es规范）和宏任务队列分开的，所以微任务都是ES6规定的，宏任务都是浏览器规定的。所以完整的顺序是：
1.执行栈清空
2.执行当前的微任务 micro task queue
3.尝试DOM渲染
4.触发事件循环机制，执行回调队列中的任务

### 微任务宏任务
微任务：promise async/await 
宏任务：setTimeout setInterval  DOM事件 Ajax
微任务更早