---
title: 第1-4章
published: 2024-06-15
description: "今天开始学习C++"
tags: ["C++","基础","用户自定义类型","模块化","类"]
category: C++
draft: false
---

# 第一章 基础部分
- 区别于C语言，C++在变量初始化的时候可进行隐式类型转换（会导致C++转为C使出兼容问题），如<br>
```cpp
int a = 7.1;
```
经过测试，C++和C都可以这么写。鉴定为书有点老了。
- 书里推荐使用auto进行泛型编程以缩短类名，疑似有点随意。
- const用于函数接口处告知调用者不用担心值被改变（参数只读）。
- constexpr用于编译时求值，并将其置于只读常量区，如示例，func()必须是constexpr int定义的，且函数必须简单。
```cpp
constexpr int a = func(2);
```
# 第二章 用户自定义类型
- struct和class没有本质区别，struct就是成员默认为public的class。
- C++不记录union具体是什么，需要程序员自己记，或者用一个结构体，将union和类型放到里面。推荐使用variant替换union。

# 第三章 模块化
- 不推荐使用#include，推荐使用module。#include会导致多次编译、语义覆盖，而import module不会。且import无传递性，即不会获得import模块的import模块的访问权。