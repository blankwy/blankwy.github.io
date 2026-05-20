---
abbrlink: ''
categories:
 - [技术学习,硬件]
date: '2026-05-20T17:36:52.548605+08:00'
tags:
- stm32
- 单片机
- 学习
- 踩坑
- 技术分享
title: 中断回调用HAL_Delay延迟导致卡死
updated: '2026-05-20T18:24:04.197+08:00'
---
写按键驱动的时候封装了一层函数，接收按键枚举和监听事件函数指针，传递给外部中断回调统一处理，习惯性地在监听函数写了`HAL_Delay(200)`，然后就坏菜了

[![image.png](https://i.postimg.cc/rwMp1H33/image.png)](https://postimg.cc/RNp99gML)

编译下载，按下Confirm，程序卡死

重新回顾了一下NVIC与HAL_Delay的机制，才注意到

[![image.png](https://i.postimg.cc/g2cqjgz5/image.png)](https://postimg.cc/8j3rnBrB)

HAL_Delay依赖HAL_GetTick获取SysTick计数值来实现延时的时间判断，而HAL_GetTick的值又需要SysTick中断来更新。在优先级更高的中断的回调中调用HAL_Delay时，优先级相对较低的SysTick中断无法触发，uwTick的值得不到更新，HAL_Delay所比较的时间一直不动，便一直执行阻塞，造成cpu卡死。

---

删除中断中出现的HAL_Delay()，编译下载，正常运行。
