---
layout:     post
title:      "React Hooks study"
subtitle:   " \"useState useMemo useReducer useContext useEffect useRef\""
date:       2021-01-17 12:00:00
author:     "Yandong"
header-img: "img/post-bg-2021.jpg"
tags:
    - react
---

> “Yeah It's on. ”

## 什么是Hooks

函数组件比较简单提倡使用，但是有时候需要state或其他一些功能时，只能使用类组件，因为函数组件没有实例，没有生命周期，只有类组件有。<br>
新增特性Hooks可以让在不编写class情况下使用state及其他特性 。凡是use开头的React API 都是Hooks。

---
## Hooks解决问题
***1.类组件的不足***
* 状态逻辑难复用：组件之间复用**状态逻辑很难**，要用到**render props渲染属性** 或 **HOC高阶组件**。
* 趋于复杂很难维护：类组件都是对状态访问和处理，组件难以拆分更小的组件。
* 还有this指向问题 必须要绑定this。

***2.Hooks优势***
* 优化类组件3大问题
* 在不修改组件结构下复用状态逻辑(自定义Hooks)
* 而且没有像类组件生命周期函数中如ajax请求、定时器等数据向视图转换的逻辑，不容易出bug,useEffect在全部渲染完毕才执行。

---
## useState& useMemo& useCallback
* 声明useState给函数组件内部添加状态
* 返回值：状态和改变状态的方法,状态的初始值是0 类似于类组件this.state
* useState(0)初始状态值 不同于类组件this.state必须是一个对象，useState可以是值或对象。
* React在更新重复渲染的时候，保留了状态(闭包-函数变量保存在内存里)

```
    import React, { useState } from 'react';
    function Example(){
        const [ count , setCount ] = useState(0);
        return (
            <div>
                <p>You clicked {count} times</p>
                <button onClick={()=>{setCount(count+1)}}>click me</button>
            </div>
        )
    }
    export default Example;
```
`const [ count , setCount ] = useState(0);`
ES6语法数组解构
`let _useState = useState(0)`
`let count = _useState[0]`
`let setCount = _useState[1]`
**useState**函数接收参数是状态的初始值,它返回一个数组，数组的0位是当前的状态值，第1位是改变状态值的方法函数。
`<p>You clicked {count} times</p>`读取值
`<button onClick={()=>{setCount(count+1)}}>click me</button>`
React自动帮我们记忆组件上一次状态值,接收新状态值，重新渲染组件。
---

* useMemo优化React Hooks程序性能
解决React Hooks产生无用渲染的性能问题，使用function形式声明组件，失去了shouldComponentUpdate生命周期，即我们没有办法通过组件更新前条件决定组件是否更新。而且在函数组件里，不再区分mount和update两个状态，意味着函数组件每一次调用都会执行内部的所有逻辑，带来非常大的损耗。
```
    function ChildComponent({name,children}){
    function changeXiaohong(name){
        console.log('她来了，她来了。小红向我们走来了')
        return name+',小红向我们走来了'
    }

    const actionXiaohong = useMemo(()=>changeXiaohong(name),[name]) 
    return (
        <>
            <div>{actionXiaohong}</div>
            <div>{children}</div>
        </>
    )
}
```
使用useMemo，然后给她传递第二个参数，参数匹配成功，才会执行.


## useEffect代替常用生命周期函数
```
   import React, { useState , useEffect } from 'react';
    function Example(){
        const [ count , setCount ] = useState(0);
        //---关键代码---------start-------
        useEffect(()=>{
            console.log(`useEffect=>You clicked ${count} times`)
        })
        //---关键代码---------end-------

        return (
            <div>
                <p>You clicked {count} times</p>
                <button onClick={()=>{setCount(count+1)}}>click me</button>
            </div>
        )
    }
    export default Example; 
```
第一次组件渲染和每次组件更新都会执行这个函数<br>
声明一个状态变量count,将它的初始值设为0，给useEffecthook传一个**匿名函数**，告诉react这个组件有一个副作用<br>
(副作用：和函数业务主逻辑关联不大，特定时间或事件中执行的动作，比如Ajax请求后端数据，添加登录监听和取消登录，手动修改DOM等等)
当React要渲染组件时，它会记住用到的副作用，然后执行一次。<br>
### useEffect注意
* 首次渲染和之后渲染都会调用useEffect函数
* useEffect不会阻碍浏览器更新视图，函数异步执行。而componentDidMount和componentDidUpdate同步执行。
### useEffect第二个参数
```
    function Index() {
    useEffect(()=>{
        console.log('useEffect=>老弟你来了！Index页面')
        return ()=>{
            console.log('老弟，你走了!Index页面')
        }
    },[])
    return <h2>JSPang.com</h2>;
}
```
数组，数组里可以写入很多状态对应的变量，当状态值发生变化时，我们才进行解绑。但是当传入空数组[]时，当组件被销毁时才进行解绑，实现componentWillUnmount生命周期函数。

## useContext让父子组件传值更简单
在使用类组件时，父子组件传值通过组件属性和props进行的,现在使用函数组件声明组件，**没有构造函数也没有props接收**，父子组件传值React Hooks准备了useContext.帮助我们**跨越组件层级直接传递变量，实现共享**.<br>
### 注意
useContext和redux作用不同的，一个是解决组件之间值传递的问题，一个是应用统一管理状态的问题，但通过和useReducer配合使用，可以实现类似Redux作用。**Context的作用就是对它所包含的组件树提供全局共享数据的一种技术。**
> createContext函数创建context
```
    import React, { useState , createContext } from 'react';
    //===关键代码
    const CountContext = createContext()

    function Example4(){
        const [ count , setCount ] = useState(0);

        return (
            <div>
                <p>You clicked {count} times</p>
                <button onClick={()=>{setCount(count+1)}}>click me</button>
                {/*======关键代码 */}
                <CountContext.Provider value={count}>
                    <Counter/>
                </CountContext.Provider>
            </div>
        )
    }
    export default Example4;
```

* 这段代码相当于把count变量允许跨层级实现传递和使用了(也就实现了上下文)，当父组件的count变量发生变化时，子组件也会发生变化。

 useContext接收上下文变量

```
    import React, { useState , createContext , useContext } from 'react';
    function Counter(){
        const count = useContext(CountContext)  //一句话就可以得到count
        return (<h2>{count}</h2>)
    }
```

---

## useReducer增强我们的reducer,实现类似Redux功能
* Reducer就是一个函数，就收两个参数，一个是状态，一个是用来控制业务逻辑的判断函数。
```
    function countReducer(state, action) {
    switch(action.type) {
        case 'add':
            return state + 1;
        case 'sub':
            return state - 1;
        default: 
            return state;
    }
}
```
* useReducer计数器加减双向操作
```
    import React, { useReducer } from 'react';

    function ReducerDemo(){
        const [ count , dispatch ] =useReducer((state,action)=>{
            switch(action){
                case 'add':
                    return state+1
                case 'sub':
                    return state-1
                default:
                    return state
            }
        },0)
        return (
        <div>
            <h2>现在的分数是{count}</h2>
            <button onClick={()=>dispatch('add')}>Increment</button>
            <button onClick={()=>dispatch('sub')}>Decrement</button>
        </div>
        )

    }

    export default ReducerDemo
```

---
## useRef获取DOM元素和保存变量
* 用useRef获取React JSX中的DOM元素，获取后你就可以控制DOM的任何东西了
* 用useRef来保存变量
```
    import React, { useRef} from 'react';
    function Example8(){
        const inputEl = useRef(null)
        const onButtonClick=()=>{ 
            inputEl.current.value="Hello ,JSPang"
            console.log(inputEl) //输出获取到的DOM节点
        }
        return (
            <>
                {/*保存input的ref到inputEl */}
                <input ref={inputEl} type="text"/>
                <button onClick = {onButtonClick}>在input上展示文字</button>
            </>
        )
    }
    export default Example8
```