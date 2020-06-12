---
title: react 学习
date: 2020-04-28
tags: React
---

react函数式组件 react的组件利用类似函数定义的方式去实现的如：
```javascript
function Clock(props){
  return (
     <div>
       <h1>现在的时间是{props.date.toLocaleTimeString()}</h1>
     </div>
  )
}
```
这里同时传入了参数，用于父子组件参数的传递。再利用函数式的调用如：
```javascript
function run(){
  ReactDOM.render(
    <Clock date = {new Date()} />, // 组件渲染，传参
    document.querySelector('#root')
  )0
}
setInterval(run,1000)
```
