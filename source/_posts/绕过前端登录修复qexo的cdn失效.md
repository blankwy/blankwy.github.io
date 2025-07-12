---
abbrlink: ''
categories:
- - 踩坑日记
date: '2025-07-06T12:31:17.096756+08:00'
tags:
- 踩坑
- 技术分享
- 学习
- JavaScript
- 建站
- hexo
title: 绕过前端登录修复qexo的cdn失效
updated: '2025-07-06T12:31:17.096756+08:00'
---
想写文章的时候打开hexo的后台，发现样式炸了，登录也没反应，f12之后发现js在cdn上的资源无了😨

[![pVKOuZV.png](https://s21.ax1x.com/2025/07/06/pVKOuZV.png)](https://imgse.com/i/pVKOuZV)

后台可以更换为其它cdn，但还是得先登录，绕进死胡同了

于是决定用post请求直接登录绕过前端，再去后台改

---

在html源码中找到登录按钮对应的事件

[![pVKOerq.png](https://s21.ax1x.com/2025/07/06/pVKOerq.png)](https://imgse.com/i/pVKOerq)

定位到submit方法

[![pVKOZMn.png](https://s21.ax1x.com/2025/07/06/pVKOZMn.png)](https://imgse.com/i/pVKOZMn)

记下请求内容与url

---

控制台直接post之后被csrf拦下😓需要加在请求头上

---

最终js代码：

```javascript
const USERNAME = "用户名";
const PASSWORD = "密码";

// 获取CSRF token
const csrfToken = document.querySelector('input[name="csrfmiddlewaretoken"]').value;

// 手动发起登录请求
fetch("/api/auth/", {
  method: "POST",
  headers: {
    "Content-Type": "application/x-www-form-urlencoded",
    "X-CSRFToken": csrfToken
  },
  body: `username=${encodeURIComponent(USERNAME)}&password=${encodeURIComponent(PASSWORD)}`
})
```

在控制台输入后回车，登录成功

[![pVKOmq0.png](https://s21.ax1x.com/2025/07/06/pVKOmq0.png)](https://imgse.com/i/pVKOmq0)

成功登录后台，然而后台js也是崩的，改不了，但注意到里面还有unpkg和jsdelivr的cdn可以选

[![pVKXU6s.png](https://s21.ax1x.com/2025/07/06/pVKXU6s.png)](https://imgse.com/i/pVKXU6s)

来到绑定qexo的MongoDB，将对应cdn地址的数据改为unpkg的url

[![pVKXaXn.md.png](https://s21.ax1x.com/2025/07/06/pVKXaXn.md.png)](https://imgse.com/i/pVKXaXn)

[![pVKX000.jpg](https://s21.ax1x.com/2025/07/06/pVKX000.jpg)](https://imgse.com/i/pVKX000)

再次刷新后台，成功

[![pVKXwmq.jpg](https://s21.ax1x.com/2025/07/06/pVKXwmq.jpg)](https://imgse.com/i/pVKXwmq)
