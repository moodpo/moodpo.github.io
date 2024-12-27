---
layout: post
title: "JavaScript 规范编程笔记：函数（一）"
subtitle: ""
author: "yangchw"
catalog: false
date: 2013-09-14 15:27:00
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - javascript
---

> 一般来说，所谓编程就是将一组需求分解成一组函数与数据结构的技能。

JavaScript中最好的特性就是它对函数的实现，它几乎无所不能。函数包含子组语句，它们是JavaScript的基础模块单元，用于代码复用、信息隐藏和组合调用。函数指定对象的行为。

### 1\. 函数对象

在JavaScrpit中函数也是对象。对象字面量产生的对象连接到Object.prototype上，而函数对象连接到Function.prototype上（该原型对象本身连接到Object.prototype）。每个函数在创建时附有两个附加的隐藏属性：函数的上下文和实现函数行为的代码（JavaScript创建一个函数对象时，会给该对象设置一个“调用”属性）。

每个函数对象在创建时也随带有一个prototype属性。它的值是一个拥有constructor属性且值即为该函数的对象。

因为函数是对象，所以它们可以像任何其他的值一样被使用。函数可以存放在变量、对象和数组中，函数可以被当作参数传递给其他函数，函数也可以再返回函数。而且，因为函数是对象，所以函数可以拥有方法。

### 2\. 函数字面量

    // 创建一个名为 add 的变量，并用来把两个数字相加的函数赋值给它
    var add = function (a, b){
        return a + b;   
    };
    `</pre>

    函数字面量可以出现在任何允许表达式出现的地方。函数也可以被定义在其他函数中。一个内部函数自然可以访问自己的参数和变量，同时它也能方便的访问它被嵌套在其中的那个函数的参数与变量。通过函数字面量创建的函数对象包含一个连到外部上下文的连接。这被称为**闭包**。

    ### 3\. 调用

    在JavaScript中一共有四种调用模式：方法调用模式、函数调用模式、构造器调用模式和apply调用模式。调用函数时，除了声明时定义的形式参数，每个函数接收两个附加的参数：this和arguments。参数this在面向对象变成中非常重要，它的值取决于调用的模式。调用运算符是跟在任何产生一个函数值的表达式之后的一对圆括号。圆括号中包含了参数（如果有的话），当实际参数的个数过多时，超出的参数值将被忽略，如果实际参数值过少，缺失的值将会被替代为undefined。

    **方法调用模式**

    当一个函数被保存为对象的一个属性时，我们称它为一个方法。当一个方法被调用时，this被绑定到该对象。

    <pre>`// 常见 myObject,它有一个value属性和一个increment方法
    // increment方法接受一个可选的参数。如果参数不是数字，那么默认使用数字1。
    var myObject = {
        value: 0,
        increment: function(inc){
            this.value += typeof inc === 'number' ? inc : 1;
        }
    };

    myObject.increment();
    document.writeln(myObject.value); // 1

    myObject.increment(2);
    document.writeln(myObject.value); // 3
    `</pre>

    this到对象的绑定发生在调用的时候，这个“超级”迟绑定使得函数可以对this高度复用。通过this可取得它们所属对象的上下文的方法称为**公共方法**。

    <!-- more -->

    **函数调用模式**

    当一个函数并非一个对象的属性时，那么它被当作一个函数来调用：

    <pre>`var sum = add(3, 4);   // sum 为 7
    `</pre>

    当函数以此模式调用时，this被绑定到全局对象。这是一个设计错误，这个错误的后果是方法不能利用内部函数来帮助它工作。解决方法是：如果该方法定义一个变量并给它赋值为this，那么内部函数就可以通过哪个变量访问到this。按照约定，我给这个变量命名为that：

    <pre>`// 给myObject增加一个double方法。
    myObject.double = function(){
        var that = this; // this指向全局变量的解决方法

        var helper = function(){
            that.value = add(that.value, that.value);
        };

        helper(); // 以函数的形式调用helper。
    };

    // 以方法的形式调用double。
    myObject.double();
    document.writeln(myObject.value); // 6
    `</pre>

    **构造器调用模式**

    尽管原型继承有着强大的表现力，但它并不被广泛理解。JavaScript本身对其原型的本质也缺乏信心，所以它提供了一套和基于类的语言类似的对象构建语法。

    如果在一个函数前面带上new来调用，那么将创建一个隐藏连接到该函数的prototype成员的新对象，同时this将会被绑定到那个新对象上。

    <pre>`// 创建一个名为Quo的构造器函数，它构造一个带有status属性的对象。
    var Quo = function(string){
        this.status = string;
    };

    // 给Quo的所有实例提供一个名为get_status的公共方法。
    Quo.prototype.get_status = function(){
        return this.status;
    };

    // 构造一个Quo实例
    var myQuo = new Quo(&quot;confused&quot;);

    document.writeln(myQuo.get_status()); // 令人困惑
    `</pre>

    结合new前缀调用的函数被称为**构造器函数**。按照约定，它们保存在以大写格式命名的变量里。如果调用构造器函数时没有在前面加上new，可能会发生非常糟糕的事情，既没有编译时警告，也没有运行时警告，因此大写约定非常重要。不推荐使用这种形式的构造器函数。

    **Apply 调用模式**

    因为JavaScript是一门函数式的面向对象编程语言，所以函数可以拥有方法。apply方法让我们创建一个参数数组并用其去调用函数。它允许我们选择this的值。apply方法接收两个参数，第一个将被绑定给this的值，第二个就是一个参数数组。

    <pre>`var array = [3, 4];
    var sum = add.apply(null, array); // 7

    var statusObject = {
        status: 'A-OK'
    };

    // get_status 没有参数，statusObject指定了将被绑定给this的值
    var status = Quo.prototype.get_status.apply(statusObject);
    document.writeln(status); // A-OK
    `</pre>

    ### 4\. 参数

    当函数被调用时，会得到一个“免费”奉送的参数，那就是 arguments 数组。通过它函数可以访问所有它被调用时传递给它的参数列表，包括那些没有被分配给函数声明时定义的形式参数的多余参数。这使得编写一个无须指定参数个数的函数成为可能：

    <pre>`// 构造一个将很多个值想加的函数
    var sum = function(){
        var i, sum = 0;
        for(i = 0; i &lt; arguments.length; i += 1){
            sum += arguments[i];
        }   
        return sum;
    };

    document.writeln(sum(4, 8, 15, 16, 23, 42));  // 108
    `</pre>

    因为语言的一个设计错误，arguments并不是一个真正的数组，arguments拥有一个length属性，但它缺少所有数组方法。

    ### 5\. 返回

    return 语句可用来使函数提前返回。当return被执行时，函数立即返回而不再执行余下的语句。一个函数总是会返回一个值。如果没有指定返回值，则返回undefined。

    ### 6\. 异常

    JavaScrpit提供了一套异常处理机制。throw语句中断函数的执行，它应该抛出一个exception对象，该对象包含可识别异常类型的name属性和一个描述性的message属性。你也可以添加其他的属性。

    <pre>`var add_throw = function(a,b){
        if(typeof a !== 'number' || typeof b !== 'number'){
            throw {
                name: 'TypeError',
                message: 'add needs numbers'
            };
        }
        return a + b;
    };
    `</pre>

    该exception对象将被传递到一个try语句的catch从句：

    <pre>`var try_it = function(){
        try {
            add_throw('sever');
        } catch (e) {
            document.writeln(e.name + ': ' + e.message);
        }
    };

    try_it();

一个try语句只会有一个将捕获所有异常的catch代码块。如果你的处理手段取决于异常的类型，那么异常处理器必须检查异常对象的name属性以确定异常的类型。