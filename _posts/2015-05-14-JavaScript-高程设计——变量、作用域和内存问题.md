---
layout: post
title: "JavaScript 高程设计——变量、作用域和内存问题"
subtitle: ""
author: "yangchw"
catalog: false
date: 2015-05-14 14:21:00
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - javascript
---


## 基本类型和引用类型的值

5种基本数据类型：Undefined、Null、Boolean、Number和String是按值访问的，因为可以操作保存在变量中的实际的值。

引用类型的值是保存在内存中的对象。与其他语言不同，JavaScript 不允许直接访问内存中的位置，也就是说不能直接操作对象的内存空间。在操作对象时，实际上是在操作对象的引用而不是实际的对象。为此，引用类型的值是按引用访问的。

基本类型值的传递如同基本类型变量的复制一样，而引用类型值的传递，则如同引用类型变量的复制一样。参数只能按值传递。

虽然在检测基本数据类型时 `typeof`是非常得力的助手，但在检测引用类型的值时，这个操作符的用处不大。为此，ECMAScript 提供了 `instanceof` 操作符，其语法如下所示：

    result = variable instanceof constructor

使用 `typeof` 操作符检测函数时，该操作符会返回 `&quot;function&quot;`。在 IE 和 Firefox 中，对正则表达式应用 `typeof` 会返回 `&quot;object&quot;`，在 Safari 和 Chrome 中返回 `&quot;function&quot;`。

## 执行环境及作用域

每个执行环境都有一个与之关联的变量对象（variable object），环境中定义的所有变量和函数都保存在这个对象中。

如果这个环境是函数，则将其活动对象（activation object）作为变量对象。活动对象在最开始时只包含一个变量，即 `arguments` 对象（这个对象在全局环境中是不存在的）。

`try-catch` 语句的 `catch` 块和 `with` 语句可以延长作用域链。

## 垃圾收集

垃圾收集器必须跟踪哪个变量有用哪个变量没用，对于不再有用的变量打上标记，以备将来收回其占用的内存。用于标识无用变量的策略可能会因实现而异，但具体到浏览器中的实现，则通常有两个策略——标记清除和引用计数。

JavaScript 中最常用的垃圾收集方式是标记清除（mark-and-sweep）。当变量进入环境（例如，在函数中声明一个变量）时，就将这个变量标记为“进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到它们。而当变量离开环境时，则将其标记为“离开环境”。

到2008年为止，IE、Firefox、Opera、Chrome 和 Safari 的 JavaScript 实现使用的都是标记清除式的垃圾收集策略（或类似的策略），只不过垃圾收集的时间间隔互有不同。

另一种不太常见的垃圾收集策略叫做引用计数（referencecounting）。**循环引用**指的是对象A中包含一个指向对象B的指针，而对象B中也包含一个指向对象A的引用。

因此，确保占用最少的内存可以让页面获得更好的性能。而优化内存占用的最佳方式，就是为执行中的代码只保存必要的数据。一旦数据不再有用，最好通过将其值设置为 `null` 来释放其引用——这个做法叫做解除引用（dereferencing）。