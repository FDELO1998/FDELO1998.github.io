---
title: react 生命周期总结
tags: React
---

react目前市面上使用的主要是v16前的生命周期，和v16之后的。所以我们应当都掌握一下。
### React v16.0 前的生命周期
主要分为Initialization,Mounting,Updation,Unmounting。
![早期生命周期](https://techstudyblog.top/2019/09/28/react-life-cycle/react.png)
初始化阶段：主要是构造函数这些方法使用，包括定义的类继承react component基类，准备开始调用render()、生命周期等方法。函数组件不能使用这些方法也是没继承这个原因。
这个阶段，可调用基类的构造方法，父组件props注入子组件，组件中的props只读不可变，state可变。
constructor()做组件的初始化工作，如定义this.state。

挂载阶段：componentWillMount（挂载前）,render（判断props和state，只要值有变化就会引起组件重新render。componentDidMount（挂载后调用一次）三个阶段。

引发组件更新有三种情况：
父组件重新render导致的props重传，无论值是否变化，子组件都会跟着重新渲染。
当然也可以使用shouleComponentUpdate(nextProps,nextState)方法，componentWillReceiveProps去控制是否重新渲染。同时shouleComponentUpdate这个方法在其自身也可以调用来判断state是否有实际改变来判断是不是组件渲染。此阶段生命周期分为componentWillReceiveProps，shouldComponentUpdate，componentWillUpdate，render，componentDidUpdate。

卸载阶段：componentWillUnmount
此方法在组件被卸载前调用，可以在这里执行一些清理工作，比如清楚组件中使用的定时器，清楚componentDidMount中手动创建的DOM元素等，以避免引起内存泄漏。

### React v16.4 后的生命周期
迎来了大动，除了增加的componentDidCatch，主要引入了新的getDerivedStateFromProps，getSnapshotBeforeUpdate。前者是只在由父组件引发的创建和更新中调用。 
![新生命周期](https://upload-images.jianshu.io/upload_images/5287253-19b835e6e7802233.png)
static getDerivedStateFromProps(props, state) 在组件创建时和更新时的render方法之前调用。要加上Static保留字声明为静态，不然会被react忽略。
getSnapshotBeforeUpdate() 被调用于render之后，可以读取但无法使用DOM的时候。
可以看到之前的render之前的生命周期函数全灭。几个will都没了。所以新图![v16.4后的生命周期](https://pic1.zhimg.com/v2-930c5299db442e73dbb1d2f9c92310d4_r.jpg)



[参考鸣谢](https://www.jianshu.com/p/514fe21b9914)  