---
abbrlink: '63534'
categories: []
date: '2024-03-08T23:05:19+08:00'
tags: []
title: hexo每次远程部署会变动文章更新日期解决方案
updated: '2024-07-24T23:35:32.388+08:00'
---
# 踩坑

配置了`update_option: mtime`和`order_by: -update`，实现文章按最后修改日期排序，但每次部署完发现都会有个hello world不合时宜地出现在最上面😓 打开发现大部分文章的修改日期都变成了同一个——部署的日期，而自己加了updated参数的不受影响，疑似git本身对文件修改日期的判定机制导致。

于是乎，给每个文章都添加了update参数，例如

```markdown
---
title: Hello World
date: 2023-01-01 00:00:00
updated: 2023-02-01 00:00:00
---
```

勉强解决，但每次都手动添加太麻烦了。

# 完美解决方案

## 修改格式模板

在文章模板中添加updated参数到Front-matter（修改/scaffolds/post.md），如下

```markdown
---
title: {{ title }}
date: {{ date }}
tags:
updated: {{ date }}
---
```

## 在自动部署脚本加上对时间的矫正

加上一行`git ls-files -z | while read -d '' path; do touch -d "\$(git log -1 --format="@%ct" "\$path")" "\$path"; done`来矫正修改日期即可。

已加入[本站自动部署workflow | 无愚の日记 (binarydev.top)](https://blog.binarydev.top/posts/2024/04/13/27508/))，可参考。




参考文章：

[hexo自动更新文章修改时间 - yyyz - 博客园 (cnblogs.com)](https://www.cnblogs.com/yyyzyyyz/p/15792199.html)

[Hexo GitHub Action CI配置(解决文章修改时间问题) | Calvin Cui (unparalleled-calvin.github.io)](https://unparalleled-calvin.github.io/Blog/2022/10/14/Hexo-GitHub-Action-CI%E9%85%8D%E7%BD%AE(%E8%A7%A3%E5%86%B3%E6%96%87%E7%AB%A0%E4%BF%AE%E6%94%B9%E6%97%B6%E9%97%B4%E9%97%AE%E9%A2%98)/)
