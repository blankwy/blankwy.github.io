---
abbrlink: ''
categories:
 - [技术学习,编程]
date: '2025-10-05T01:00:38.941597+08:00'
tags:
- 编程
- 学习
- Go
title: 通过Air实现Go的热重载
updated: '2025-10-05T01:00:57.578+08:00'
---
花一周学了Go之后就上手写后端了，然而gin不带热重载，饱受ctrl+c ，go run 折磨，才发现了Air。

[image.png](https://postimg.cc/xNyT5rWL)

# 安装

`go install github.com/air-verse/air@latest`

通过`air -v`获取版本成功，安装完成

# 生成配置文件

`air init`

生成了`.air.toml`文件，可自己修改配置

# 启动

执行`air`启动热重载

# 测试

修改代码后立即自动build了一遍，即刻生效，不用一遍遍run了🙂 


[官方Github仓库](https://github.com/air-verse/air)
