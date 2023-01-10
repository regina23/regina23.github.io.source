---
title: "Effective C++ 阅读记录"
author: "Regina"
date: 2022-12-14T15:36:12+08:00
lastmod: 2022-12-14T15:36:12+08:00
draft: false
toc:
  enable: true
categories: [""]
tags: [""]
---

## Item 1: 将 C++ 视为 federation of languages（语言联合体）

将C++看作亲族语言的联合体：

- C：C++是基于C的，比如blocks（模块）、statements（语句）、preprocessor（预处理器）、built-in data types（内建数据类型）、arrays（数组）、pointers（指针）等等。
- Object-Oriented C++：面向对象的规则，比如classes（类）（包括构造函数和析构函数）、encapsulation（封装）、inheritance（继承）、polymorphism（多态）、virtual functions（虚函数）、 dynamic binding（动态绑定）等等。
- Template C++：泛型编程，template metaprogramming (TMP) （模板元编程）。
- STL：模版库，包括containers（容器）、iterators（迭代器）等等。

> 使用 built-in（内建）（也就是说，C-like（类 C 的））类型时，pass-by-value（传值）通常比 pass-by-reference（传引用）更高效，但是当你从 C++ 的 C 部分转到 Object-Oriented C++（面向对象 C++），user-defined constructors（用户自定义构造函数）和 destructors（析构函数）意味着，通常情况下，更好的做法是 pass-by-reference-to-const（传引用给 const）。在 Template C++ 中工作时，这一点更加重要，因为，在这种情况下，你甚至不知道你的操作涉及到的 object（对象）的类型。然而，当你进入 STL，你知道 iterators（迭代器）和 function objects（函数对象）以 C 的 pointers（指针）为原型，对于 STL 中的 iterators（迭代器）和 function objects（函数对象），古老的 C 中的 pass-by-value（传值）规则又重新生效。
>
> 

## Item 2: 用const、enum和inline取代#define

#define 在preprocessor（预处理器）中处理，const、enum和inline在compiler（编译器）中处理，所以也可以说用compiler（编译器）取代preprocessor（预处理器）。

**原因：**

例如定义了 ```#define ASPECT_RATIO 1.653```

预处理器处理了`ASPECT_RATIO`这个变量，编译的时候`ASPECT_RATIO`可能就不会被加入符号表。在编译报错或者调试的时候信息可能会会用 `1.653`代替了`ASPECT_RATIO`，导致增加了排查难度。

**解决方案：**

使用常量const代替宏定义#define。```const double AspectRatio = 1.653;```

因为语言层面的常量const会被编译器识别并加入符号表；并且对于这个例子的浮点数来说，使用常量const比使用宏定义#define能产生更小的代码。
