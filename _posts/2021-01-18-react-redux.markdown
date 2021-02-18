---
layout:     post
title:      "Redux"
subtitle:   " \"redux\""
date:       2021-01-18 12:00:00
author:     "Yandong"
header-img: "img/post-bg-2021.jpg"
tags:
    - react
---

> “Yeah It's on. ”

## redux理解
* redux是一个独立做状态管理的JS库
* 集中式管理(读/写)react应用中多个组件共享的状态
* 如果某个状态需要在任何地方都可以拿到
* ![redux原理图](/redux原理图.png)
* ![blockchain](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/
u=702257389,1274025419&fm=27&gp=0.jpg "区块链")
---

## 核心API
### createStore()
* 创建包含指定reducer的store对象.
```
    import {createStore} from 'redux'
    import reducer from './reducers/counter'
    const strore = createStore(reducer)
```

### store对象
* redux库最核心的管理对象
* 内部维护者 state reducer
* 核心方法 getState()、 dispatch(action)、 subscribe(listener)
* 编码 store.getState()、store.dispatch({type:'INCREMENT',number})、store.subscribe(render)

### applayMiddleware()
* 应用上基于redux的中间件(插件库)
* 编码 
```
    import {createStore, applyMiddleware} from 'redux'
    import thunk from 'redux-thunk'  // redux异步中间件
    const store = createStore(
        reducer,
        applyMiddleware(thunk) // 应用上异步中间件
    )
```

### combineReducers()
* 合并多个reducer函数
```
    export default combineReducers({
        user,
        chatUser,
        chat
    })
```

## redux三个核心概念
### action
* 标识要执行行为的对象
* 属性 type标识属性 data数据属性
```
    const action={
        type:'INCREMENT',
        data:2
    }
```
* Action Creator(创建Action的工厂函数)
`const increment=(number)=>({type:'INCREMENT',data:number})`

### reducer
* 根据老的state和action,产生新的state的纯函数
```
    export default function counter(state = 0, action) {
		  switch (action.type) {
		    case 'INCREMENT':
		      return state + action.data
		    case 'DECREMENT':
		      return state - action.data
		    default:
		      return state
		  }
		}
```

### store
* 将state，action,与reducer联系在一起的对象。
* 如何得到此对象
```
        import {createStore} from 'redux'
		import reducer from './reducers'
		const store = createStore(reducer)
```
* 此对象的功能 
* getState():得到state
* dispatch(action):分发action,触发render调用，产生新的state.
* subscribe(listener):注册监听，当产生新的state时，自动调用。
