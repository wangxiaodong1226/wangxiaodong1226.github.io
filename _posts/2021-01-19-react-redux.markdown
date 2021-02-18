---
layout:     post
title:      "React-redux"
subtitle:   " \"react-redux\""
date:       2021-01-19 12:00:00
author:     "Yandong"
header-img: "img/post-bg-2021.jpg"
tags:
    - react
---

> “Yeah It's on. ”

## 理解
* 一个react插件库
* 专门用来简化react应用中使用redux
---

## React-Redux 将所有组件分为两大类
### UI组件
* 主负责UI的呈现，不带有任何业务逻辑
* 通过props接收数据(一般数据和函数)
* 不使用任何Redux的API
* 一般保存在components文件夹下

### 容器组件
* 负责管理数据和业务逻辑，不负责UI的呈现
* 使用Redux的API
* 一般保存在container文件夹下

## 相关API
### Provider
* 让所有组件都可以得到state数据
```
    <Provider store={store}>
        <App/>
    </Provider>
```

### connect()
* 用于包装UI组件生成容器组件
```
    import { connect } from 'react-redux'
     connect(
        mapStateToprops,
        mapDispatchToProps
    )(Counter)
```

### mapStateToprops()
* 将外部的数据(即state对象)转换为UI组件的标签属性
```
   const mapStateToprops = function (state) {
   return {
     value: state
   }
  } 
```

### mapDispatchToprops()
* 将分发action的函数转换为UI组件的标签属性
* components文件夹下
![java-javascript](/img/react/react-redux1.png)

* containers文件夹下
![java-javascript](/img/react/react-redux2.png)
