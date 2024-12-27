---
layout: post
title: "JavaScript 高程设计——基本概念"
subtitle: ""
author: "yangchw"
catalog: false
date: 2015-05-11 12:35:59
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - javascript
---

## 语法

## 严格模式

    &quot;use strict&quot;;
    `</pre>

    ## 关键字和保留字

    关键字

    <pre>`break   case    catch   continue    default delete  
    do  else    finally for function    if  in  instanceof  
    new return  switch  this    throw   try typeof  
    var void    while   with    debugger*
    `</pre>

    保留字

    <pre>`abstract    boolean byte    char    class   const   
    debugger    double  enum    export  extends final   
    float   goto    implements  import  int interface   
    long    native  package private protected   public  
    short   static  super   synchronized    throws  
    transient   volatile    let yeid
    `</pre>

    ## 数据类型

    ECMAScript 中有5种简单数据类型（也称为基本数据类型）：Undefined、Null、Boolean、Number和String。

    还有一种复杂数据类型——Object。

    鉴于 ECMAScript 是松散类型的，因此需要有一种手段来检测给定变量的数据类型—— `typeof` 就是负责提供这方面信息的操作符。

    <pre>`null == undefined
    // true
    `</pre>

    永远不要测试某个特定的浮点数值。

    ECMAScript 能够表示的最小值保存在 `Number.MIN_VALUE` 中——在大多数浏览器中，这个值是`5e-324`；能表示的最大数值保存在 `Number.MAX_VALUE` 中。超出此范围使用 `Infinity`、`-Infinity` 值。使用 `isFinite()` 函数判断是否在此范围内。

    `NaN` 本身有两个非同寻常的特点。首先，任何涉及 `NaN` 的操作都会返回 `NaN`，这个特点在多步计算中有可能导致问题。其次，`NaN` 与任何值都不相等，包括 `NaN` 本身。使用 `isNaN()` 函数。

    `parseInt()/parseFloat()`，`parseFloat()`，十六进制格式的字符串则始终会被转换为0。

    ECMAScript 中的字符串是不可变的，也就是说，字符串一旦被创建，它们的值就不能改变。

    `Object` 的每个实例都具有下列属性和方法：`Constructor`、`hasOwnProperty(propertyName)`、`isPrototypeOf(object)`、`propertyIsEnumerable(propertyName)`、`toLocaleString()`、`toString()`、`valueOf()`。

    ECMA-262 不负责定义宿主对象，因此宿主对象可能会也可能不会继承 `Object`。

    ## 操作符

    逗号操作符还可以用于赋值。在用于赋值时，逗号操作符总会返回表达式中的最后一项。

    <pre>`// num的值为0
    var num = (5, 1, 4, 8, 0);
    `</pre>

    ## 语句

    `for-in` 语句是一种精准的迭代语句，可以用来枚举对象的属性。以下是`for-in` 语句的语法：

    <pre>`for (property in expression) statement

    for (var propName in window) {     
        document.write(propName);
    }
    `</pre>

    `with` 语句的作用是将代码的作用域设置到一个特定的对象中。`with` 语句的语法如下：

    <pre>`with (expression) statement;

    with(location){    
        var qs = search.substring(1);    
        var hostName = hostname;    
        var url = href;
    }

可以在 `switch` 语句中使用任何数据类型（在很多其他语言中只能使用数值），无论是字符串，还是对象都没有问题。其次，每个 `case` 的值不一定是常量，可以是变量，甚至是表达式。

`switch` 语句在比较值时使用的是全等操作符，因此不会发生类型转换。

## 函数

关于 `arguments` 的行为，还有一点比较有意思。那就是它的值永远与对应命名参数的值保持同步。不过，这并不是说读取这两个值会访问相同的内存空间；它们的内存空间是独立的，但它们的值会同步。但这种影响是单向的：修改`arguments[0]`的值会自动同步到命名参数中；而修改命名参数不会改变 `arguments` 中对应的值。另外还要记住，如果只传入了一个参数，那么为 `arguments[1]` 设置的值不会反应到命名参数中。这是因为 `arguments` 对象的长度是由传入的参数个数决定的，不是由定义函数时的命名参数的个数决定的。

ECMAScript 中的所有参数传递的都是值，不可能通过引用传递参数。