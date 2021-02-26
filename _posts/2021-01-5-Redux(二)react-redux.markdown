---
layout:     post
title:      "Redux(二)react-redux"
subtitle:   " \"React-redux\""
date:       2021-01-05 12:00:00
author:     "Yandong"
header-img: "img/post-bg-2021.jpg"
tags:
    - react
---

> 上一节学习了redux本身基本使用过程，这一节学习redux结合React使用。先按照简单的方式，自己将redux结合到react中，并且结合高阶函数、Context一步一步实现connect和Provider的功能。紧接讲解react-redux库的使用，帮助我们实现react-redux的连接。
---

## 一、react结合react代码
### redux融入redux代码
> 目前redux在react中使用是最大的，所以我们将之前编写的redux代码，融入到raect当中去。
![java-javascript](/img/react/react-redux/react-redux1.png)
* home.js代码实现
![java-javascript](/img/react/react-redux/react-redux2.png)
* Profile.js代码实现
![java-javascript](/img/react/react-redux/react-redux3.png)
上面代码非常简单
* `在componentDidMount中定义数据的变化，当数据发生变化时重新设置counter.`
* `在发生点击事件时，调用store的dispatch来派发对应的action`

---
### 自定义connect函数
如何实现react组件和redux结合起来呢？
* 我们发现每个使用的地方都有一些重复的代码
* **比如监听store数据改变的代码，都需要在componentDidMount中完成**
* **比如派发事件，都需要先拿到store,在调用其dispatch**

> 将这些公共的内容提取出来，自定义connect函数
* connect函数接收两个参数
* 参数一 `存放component希望使用到的state属性`
* 参数二 `存放component希望使用到的dispatch动作`

> connect函数有一个返回值，是一个高阶组件
* **在constructor中的state保存一下我们需要获取的状态**；
* **在componnetDidMount中订阅store中数据的变化，并且执行setState操作**。
* **在componentWillUnmount中取消订阅**
* **在render函数中返回传入的WrappedComponent,并且将所有的状态映射到props中**。
* **这个高阶组件接收一个组件作为参数，返回一个class组件**。
![java-javascript](/img/react/react-redux/react-redux4.png)

* `mapStateToProps:将state映射到一个对象中，对象包含我们需要的属性。`
* `mapDispatchToProps:将dispatch映射到对象里，对象包含组件可能操作的函数`
![java-javascript](/img/react/react-redux/react-redux5.png)
**有了connect函数，我们之后只需要关心从state和dispatch中映射自己需要的状态和行为即可**

---
### store中context处理
如何不依赖导入的store ,不直接用store.getState()呢
* `提供一个Provider，来自我们创建的Context，让用户将store传入到value即可`。
创建一个context.js文件
![java-javascript](/img/react/react-redux/react-redux6.png)
* 修改connect函数代码
* **将class组件名称的contextType赋值**
* **在组件内用到store的地方，统一用this.context代替**
![java-javascript](/img/react/react-redux/react-redux7.png)
* 在入口的index.js中，使用Provider并且提供store即可.
![java-javascript](/img/react/react-redux/react-redux8.png)

---
## react-redux使用
> 虽然我们之前已经实现了connect、Provider这些帮助我们完成连接redux、react的辅助工具，但是实际上redux官方帮助我们提供了 react-redux 的库，可以直接在项目中使用，并且实现的逻辑会更加的严谨和高效.
* 安装react-redux
`yarn add react-redux`
* 将之前使用的connect函数，换成react-redux中的connect函数
![java-javascript](/img/react/react-redux/react-redux9.png)
* 使用Provider
*** 将之前自己创建的Context的Provider，换成react-redux的Provider组件**
* **这里传入的是store属性，而不是value属性**
![java-javascript](/img/react/react-redux/react-redux10.png)

---
## react-redux源码
* Provider源码 使用了useMemo返回一个contextValue的对象
* 使用useMemo的原因是为了进行性能的优化
* 在依赖的store不改变的情况下，不会进行重新计算，返回一个新的对象

* 下面Context的Provider中就会将其赋值给value属性
![java-javascript](/img/react/react-redux/react-redux11.png)
* ReactReduxContext来自另外一个文件
![java-javascript](/img/react/react-redux/react-redux12.png)
* connect函数的依赖比较复杂：调用createConnect来返回一个connect函数
![java-javascript](/img/react/react-redux/react-redux13.png)
* createConnect函数的调用
![java-javascript](/img/react/react-redux/react-redux14.png)
* **connect函数最终调用的是connectHOC**：
* connectHOC其实是connectAdvanced的函数
* connectAdvanced函数最终返回的是wrapWithConnect函数
![java-javascript](/img/react/react-redux/react-redux15.png)
* wrapWithConnect函数
![java-javascript](/img/react/react-redux/react-redux16.png)
* wrapWithConnect最终的返回值
![java-javascript](/img/react/react-redux/react-redux17.png)
