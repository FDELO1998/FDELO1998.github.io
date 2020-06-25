---
title: React笔记1
tags: React
---
### 非受控组件
组件状态不受this.state控制，通过ref直接操作dom的组件。
### 函数组件
函数组件，没有实例，没有state，this,没有生命周期，只有props。函数组件可以捕获render内部的状态，依靠js的闭包实现的。通常搭配hooks，函数组件也可以有state和类生命周期的回调。
### 性能优化
shouldComponentUpdate(SCU);PureComponent和React.memo;immutable.js
首先react父组件有更新，子组件无条件也更新。这里和vue不一样，所以性能优化更加重要。所以使用SCU可以判断返回的props是否一样来决定渲染。 
_.isEqual深度比较数组  但该种方法消费昂贵，建议使用浅层比较

SCU必须配合不可变值进行。
PureComponent和React.memo可用的潜比较，类组件：class A extends React.PureComponent，
函数组件：
```
function Mycomponent(props){}

function areEqual(prevProps,nextProps){}

export default React.memo(Mycomponent,areEqual)
```
immutable.js拥抱不可变值，基于共享数据，不是深拷贝，速度快。但有一定的学习和迁移成本


