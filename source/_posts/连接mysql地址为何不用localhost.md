---
abbrlink: ''
categories:
- - 踩坑
- - php
- - sql
date: '2023-10-02T12:02:33.533126+08:00'
tags:
- 技术分享
- 学习
title: 连接mysql地址为何不用localhost
updated: '2025-04-06T12:10:42.672+08:00'
---
在连接mysql时，数据库地址选项看似填`localhost`和`127.0.0.1`都可以，实则会造成别的问题
在部分系统（尤其是win！）中，连接localhost需要对dns先解析（可能因此导致错误），系统还需要判断ipv4/ipv6（拖慢查询速度）
因此，用localhost不仅可能导致很多地方出现sql连接错误，整站速度也会史诗级削弱

————😋 ———————
在安装群友开源的程序时出现了sql错误，经群友点拨才意识到这个问题
在升级flarum时再次遇到sql错误，通过把localhost改为127.0.0.1完美解决

[![r-BAAdm-Ua-JXWAQyhi-AAGc-Hg-ZUho-E170.jpg](https://i.postimg.cc/65BpPbms/r-BAAdm-Ua-JXWAQyhi-AAGc-Hg-ZUho-E170.jpg)](https://postimg.cc/py1vhCvk)

[![r-BAAdm-Ua-JXa-AOp-LLAACr-X3oix-Wg970.jpg](https://i.postimg.cc/T342pdYG/r-BAAdm-Ua-JXa-AOp-LLAACr-X3oix-Wg970.jpg)](https://postimg.cc/sM5CLRh0)

[![r-BAAdm-Ua-JXe-Ab-Je-PAACnv9pz37o973.jpg](https://i.postimg.cc/5ybbZjp5/r-BAAdm-Ua-JXe-Ab-Je-PAACnv9pz37o973.jpg)](https://postimg.cc/1nCkFmk4)
