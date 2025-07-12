---
abbrlink: ''
categories: []
date: '2025-07-13T00:55:50.178636+08:00'
tags:
- JavaScript
- 编程
- 学习
- 技术分享
title: ES6异步编程笔记
updated: '2025-07-13T00:55:50.178636+08:00'
---
# 计时器

```javascript
setTimeout(()=>{
  代码块
},等待时间)
```

无论等待时间多短，异步任务都不会阻塞接下来的代码执行，即使是0也会在同步任务执行完后再执行（宏任务，执行顺序在同步代码、微任务之后）

[![pVlDibq.png](https://s21.ax1x.com/2025/07/13/pVlDibq.png)](https://imgse.com/i/pVlDibq)

# Promise

本身是个类，实例化传入函数执行异步任务，成功执行调用`resolve()`，失败调用`reject()`，两个方法都可以接受传入各种参数来传递成功/失败信息

成功（即`resolve()`被执行）后会自动执行该Promise对象的`then()`回调，`then()`方法本身返回一个新Promise对象，可以接着往下then，失败则调用catch()回调。

除了`resolve()`和`reject()`外还有个`finally()`，无论成败都会在结束之后执行

[![pVlDAaV.png](https://s21.ax1x.com/2025/07/13/pVlDAaV.png)](https://imgse.com/i/pVlDAaV)

**Async**

先声明要异步的函数，把传入耗时任务的Promise对象作为返回值（如果任务本身是异步任务（`fetch`、`io`等）就直接传入，否则要手动用`setTimeout`等调整队列）

用`async`修饰的函数简化后续操作，在`async`修饰的函数内使用`await Promise对象`来时async函数内余下任务等待promise的任务完成（**但不会阻塞同步代码**）

[![pVlDE5T.png](https://s21.ax1x.com/2025/07/13/pVlDE5T.png)](https://imgse.com/i/pVlDE5T)


# 参考资料

[20分钟学会ES6之异步处理 Promise&Async 前端新手最头疼的技能之一](https://www.bilibili.com/video/BV1cX4y1b7vc/?share_source=copy_web&vd_source=1783b13c7749e59b8870148047aa7c79)

[web前端知识：js中的微任务和宏任务js中什么是微任务和宏任务 在 JavaScript 引擎中，任务分为两种类型：微 - 掘金](https://juejin.cn/post/7214018170835173434)

[使用 promise - JavaScript | MDN Web 中文网](https://web.nodejs.cn/en-us/docs/web/javascript/guide/using_promises/)
