---
abbrlink: ''
categories:
- - 技术学习/编程
date: '2025-07-11T14:37:05.540240+08:00'
tags:
- JavaScript
- 技术分享
- 学习
title: ES6核心特性速通笔记
updated: '2025-07-13T01:17:04.858+08:00'
---
> **碎碎念**
>
> 高考完决定改学后端，经过一番纠结后最终选择了nodejs，凭借java基础和一点点js基础，过了几天文档和一些速通教程，通关ES6，以此笔记作结

# **模版字符串**

`"a,"+var1` ⇔ `` `a,${var1}` ``

---

# **解构赋值**

## **1.数组中**

**`const [a,b,c]=[1,2,3]`**

等同于

```javascript
const arr=[1,2,3]
const a=arr[0]
const b=arr[1]
const c=arr[2]
```

**便于操作组内某一元素**

*（补充：可以跳过不要的元素，变量名留空即可。如：**const [a, ,c]=[1,2,3]**中a、c赋值了1、3而留空保留括号的的2没有被赋值到新的变量****）

## **2.类中**

按顺序将内部属性赋值给左侧变量名，也可以用**原属性名称:新名称**的形式自定义名称或顺序

```javascript
const {username,url:link} = {
	username: "Binarydev",
	url: "https://binarydev.top"
}
console.log(`Username is ${username} , with the link: ${link}`) ;
//原本对象中url被赋值到了名为link的新变量中，username为进行该操作故不变
```

## **3.函数参数中**

**可用于获取剩余参数**

```javascript
function fn(a,...other){
   console.log(a,other)}
```

若传入`fn(1,2,3,4)`则会打印1,2,3,4，其中1被赋值给a，2,3,4被赋值给**数组**other

**此写法可替代[下文的arguments](#1.arguments "下文的arguments")，得到的是真数组**

---

# 扩展运算符

#### · `...var2`自动展开var2中所有项，可以用于合并或复制数组

（*补充：为什么不直接var3=var2来复制？

A直接赋值为**浅拷贝**，新变量指向指针而非重新创造对象，若var2在复制之后发生改变，var3也会一起变，本质还是同一对象而无法达到**拷贝**目的）*

（补充：此法仅为一维深拷贝，若第二层指向相同变量则当任一第二层改变时，另一个也会跟着变，推荐使用**lodash 的 cloneDeep 函数**来进行全局深拷贝）

#### · 如果加在**赋值行为左侧**，则为展开**剩余参数**，用来一键赋值剩下的到`...var3` 将剩余参数合并到变量`var3`中，此时var3是包含所有剩余参数的**数组**

参考上面的解构赋值

#### · 如果加在**字符串前**，再加方括号，可将字符串转为数组，每个字符对应一个项

`const a=[..."cs:go"]`
此时a为一个数组`["c","s",":","g","o"]`

---

# 数组方法

## 1.arguments

**获取函数的所有参数（即使没有定义）**

```javascript
function fn(){ 
	console.log(Array.from(arguments))
}

```

**此时无论fn()传入什么参数都会被添加进arguments**

属于**伪数组**

## **2.伪数组转数组**

`Array.from(伪数组) `

返回值为**数组**

## 3.数组的遍历

```javascript
数组.forEach(function(item){
	console.log(item)
})
```

forEach方法传入的回调函数的参数item对应值为当前遍历所在循环中对应的数组项

---

# **对象方法**

`Object.assign()`

用于对象的**浅拷贝、合并**

`Object.assign({},objA,objB)`返回一个**新对象**，该对象含有objA和objB的

`Object.assign(obj0,objA,objB)`**直接修改**obj0，将objA、objB的属性并入

当并入的两个对象含有同名属性时，后并入的将覆盖前并入的

---

# **类**

（java玩家警撅）

```javascript
class 类名{

	//构造方法

	constructor(构造参数){
		this.var1=var1
		//以此类推为类参数赋值
	}
	//其它自定义方法(参考函数，省了function关键字)
	method1(){
		//方法体（可用this.xxx调用类参数)
	}
}

//获取类对象
const c1=new 类名(构造参数) //将自动执行构造函数并返回一个对象
c1.method1() //调用自定义方法
```

---

# 类的继承

声明类时用`extends`继承父类，派生出子类

super传入父类构造方法中的参数，可从子类的构造参数中获取，会顺带生成this，故必须在调用this前用上`super()`

（补充：在子类非构造方法中也可以用`super.方法名()`调用父类原型方法并加以修改，但此时this仍然会指向子类）

```javascript
class B extends A{
	constructor(name,link,age){
		super(name,link,age)
		//this.name=name //父类已有的属性可省略绑定
		//this.link=link //父类已有的属性可省略绑定
		this.age=age
	}
}
```

---

# 箭头函数

~~（类似java的lambda表达式）~~
单表达式：
**`const var1= (参数) => 表达式`**

最终var1将被赋值为表达式的返回值，参数为右边表达式可调用的传入参数
当只有一个参数时，括号可省略

⚠️箭头函数不可用arguments

多语句：

```javascript
const var2=（参数) => { 
		语句
		return 返回值;
	}
```

同单表达式，但语句**需要用{}包裹并手动return返回值**

---

参考资料：

1. [20分钟学会ES6之核心语法 前端开发速成必看 干货满满无废话](https://www.bilibili.com/video/BV1zm4y1y7R9)
2. [数组的扩展 - ECMAScript 6入门](https://es6.ruanyifeng.com/#docs/array#Array-from)
3. [javascript 数组以及对象的深拷贝（复制数组或复制对象）的方法\_js深拷贝数组-CSDN博客](https://blog.csdn.net/fungleo/article/details/54931379)
4. [super - JavaScript | MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/super)
5. [JavaScript中的回调函数看这篇就够了 - 疯狂的技术宅 - SegmentFault 思否](https://segmentfault.com/a/1190000038869766)
