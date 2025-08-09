---
abbrlink: ''
categories:
- - 踩坑日记
date: '2025-08-09T20:45:19.114006+08:00'
tags:
- nodejs
- 学习
- 技术分享
- 踩坑
- JavaScript
- TypeScript
title: npm无法加载文件，在此系统上禁止运行脚本。
updated: '2025-08-09T20:45:50.565+08:00'
---
更新完node之后跑了下npm，发现整个npm命令都用不了了：

> npm : 无法加载文件 D:\node\node_global\npm.ps1，因为在此系统上禁止运行脚本。

大概是系统策略限制？

运行：`get-ExecutionPolicy`

得到结果：*`Restricted`*，也就是当前策略禁止运行脚本。

**改变策略：`Set-ExecutionPolicy RemoteSigned`**

验证：`Get-ExecutionPolicy`

得到结果：`RemoteSigned`，更改生效

再次运行`npm -v`，正常返回版本号，问题解决
