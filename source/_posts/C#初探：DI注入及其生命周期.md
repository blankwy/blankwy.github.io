---
abbrlink: csharp-DI1
categories:
- - 技术学习,编程
date: '2026-01-17T22:39:21.268416+08:00'
tags:
- C#
- 编程
- 学习
title: C#初探：DI注入及其生命周期
updated: '2026-01-17T22:39:25.416+08:00'
---
> 在 `C#.NET` 中，依赖注入（`Dependency Injection`，简称 `DI`） 是一种设计模式，用于实现控制反转（`Inversion of Control，IoC`），以降低代码耦合、提高可测试性和可维护性。

### 简而言之，就是将对象实例的创建和依赖管理，托管给IoC容器，再通过注入将实例传递给需要的流程，实现代码的解耦

# Register 服务注册

常用的四种注册方式

```csharp
// **1** 实现-接口
  Add{LIFETIME}<{SERVICE}, {IMPLEMENTATION}>()
//**2** 实现
  Add{LIFETIME}<{SERVICE},>()
//**3**  对象
  AddSingleton<{SERVICE}>(new {IMPLEMENTATION})
//**4**  工厂模式
  Add{LIFETIME}<{SERVICE}>(sp => new {IMPLEMENTATION})
```

# Inject 服务注入

对于接口IA，具有其实现A，在Services注册后

## Constructor Injection 构造函数注入

可以在类中声明服务类A的成员，并在构造参数中注入

```csharp
public class C{
   private readonly IA _A;

  public C(IA A){
    _A=A;
  }
  
}
```

## Property Injection 属性注入

声明公共属性，再添加[Inject]特性

```csharp
        [Inject]
        public IA _A { get; set; }
```

## Method Injection 方法注入

通过声明方法参数注入，会在方法被调用时生成实例

```csharp
public void D(IA A){
   ...
}
```

## Interface Injection 接口注入

*声明一个接口并注入方法，再实现接口*

*较少使用*

# LifeTime 生命周期

## **Scoped**

即作用域， 最常用的一个，可在**同一作用域**（一般同一个Http请求）内共享**同一实例**，无论是在Controller/Service还是PageModel注入多少次，使用的都是同一个实例，并随着请求的结束而销毁。

常用于DbContext、与用户行为有关服务（如Jwt）

注册方式：

```csharp
services.AddScoped<ExampleService, ExampleService>();
```

## Singleton

单例模式，整个程序共用一个实例，常驻整个进程。

注册方式：

```csharp
services.AddSingleton<ExampleService, ExampleService>();
```

## Transient

暂时模式，每次注入行为单独创建实例，并在任务完成后被回收。

用于轻量化的组件与无状态的处理等（如Identity的EmailSender或简单的逻辑运算服务）

注册方式：

```csharp
services.AddTransient<ExampleService, ExampleService>();
```

---

# 参考文献

1. [在 .NET 中使用依赖项注入](https://learn.microsoft.com/zh-cn/dotnet/core/extensions/dependency-injection-usage)
2. [C#.NET 依赖注入详解](https://segmentfault.com/a/1190000046806379#item-3)
3. [C#面： 依赖注入有哪几种方式？](https://blog.csdn.net/fishandfishand/article/details/140246003)
4. [C#依赖注入3种实现方法](https://blog.csdn.net/blu_e__heart/article/details/135725766)
