---
abbrlink: ''
categories:
- - 技术分享
- - 建站
date: '2024-07-24T22:56:56.066795+08:00'
tags:
- JavaScript
- 踩坑
title: js应在dom加载完成再操作元素
updated: '2024-07-24T22:56:59.179+08:00'
---
省流：放到body下面或者套上window.onload()

# 情景

修改flarum时，在后台自定义页眉html代码包了块script写js，通过`getElementsByClassName(' ')`来获取某指定类的标签的对象，遍历检测是否包含特定文本，再修改其文字内容，然而没有生效。💩 

原代码：

```javascript
<script>
console.log("test001");
var elements = document.getElementsByClassName('TagLabel-name');

// 遍历这些元素
for (var i = 0; i < elements.length; i++) {
  console.log("test002");
  // 获取当前元素的文本内容
  var text = elements[i].textContent || elements[i].innerText;

  // 检查文本内容是否为'Win'或'Linux'
  if (text === 'Win' || text === 'Linux' || text === 'Mac' || text === '安卓') {
   elements[i].textContent=''; 
   console.log("test003");

  }
}
</script>
```

控制台输出结果只有test001，js被执行了，但没有进一步遍历到元素。

通过一顿chatgpt了解到：这段js执行的时候dom还没加载完全，需要get到的元素还不存在。

# 解决

解决方案是：

1. script代码块**放到body下面**
2. 或者加上**window.onload()**来确保dom加载完成。

后者：

```javascript
<script>
window.onload = function() {

………… //要执行的代码

};</script>
```

总结：我是个不会js的麻瓜😬
