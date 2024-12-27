---
layout: post
title: "JavaScript 规范编程笔记：对象"
subtitle: ""
author: "yangchw"
catalog: false
date: 2013-09-06 22:52:00
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - javascript
---

![对象](http://moodpo.com/usr/uploads/2014/01/3961998075.jpg)

没有对象我可以试着 new 一个

在其中我可以重载甚至覆盖任何一种方法

但是我却无法重载对你的思念

也许命中注定

你在我的世界里永远的烙上了静态的属性

而我不慎调用了爱你这个方法

当我义无返顾的把自己作为参数传进这个方法时

我才发现爱上你是一个死循环

JavaScript的简单类型包括数字、字符串、布尔值（true和false）、null值和undefined值，其他所有的值都是对象。在JavaScript中，数组是对象，函数是对象，正则表达式是对象，当然，对象自然也是对象。对象是属性的容器，其中每个属性都拥有名字和值。属性的名字可以是包括空字符串在内的任意字符串。属性值可以是除undefined值之外的任何值。

JavaScript包括一个原型链特性，允许对象继承另一个对象的属性。

### 1\. 对象字面量

对象字面量提供了一种非常方便的创建新对象值得表示法。一个对象字面量就是包裹在一对花括号中的零或多个“名/值”对。

    var empty_object = {};

    var stooge = {
        &quot;first-name&quot;: &quot;Jerome&quot;,
        &quot;last-name&quot;: &quot;Howard&quot;   
    };
    `</pre>

    对象是可嵌套的。

    <pre>`var flight = {
        airline: &quot;Oceanic&quot;,
        number: 815,
        departure: {
            IATA: &quot;SYD&quot;,
            time: &quot;2004-09-22&quot;,
            city: &quot;Sydney&quot;
        },
        arrival: {
            IATA: &quot;LAX&quot;,
            time: &quot;2004-09-23&quot;,
            city: &quot;Los Angeles&quot;
        }
    };
    `</pre>

    ### 2\. 检索

    要检索对象中包含的值，可以采用在`[]`后缀中括住一个字符串表达式的方式。如果字符串表达式是一个常数，也可以使用`.`表示法代替。

    <!-- more -->
    <pre>`stooge[&quot;first-name&quot;]   // &quot;Joe&quot;
    flight.departure.IATA  // &quot;SYD&quot;
    `</pre>

    如果尝试检索一个并不存在的成员元素的值，将返回一个`undefined`值。

    <pre>`stooge[&quot;middle-name&quot;]   // undefined
    flight.status           // undefined
    `</pre>

    尝试检索一个`undefined`值将会导致TypeError异常。这可以通过`&amp;&amp;`运算符来避免错误。

    <pre>`flight.equipment    // undefined
    flight.equipment.model // throw &quot;TypeError&quot;
    flight.equipment &amp;&amp; flight.equipment.model  // undefined
    `</pre>

    ### 3\. 引用

    对象通过引用来传递，它们永远不会被拷贝：

    <pre>`var x = stooge;
    x.nickname = 'Curly';
    var nick = stoogee.nickname;   // 'Curly'

    var a = {}, b = {}, c = {};  // a、b和c每个都引用一个不同的空对象
    a = b = c = {};  // a、b和c都应用同一个空对象
    `</pre>

    ### 4\. 原型

    每个对象都连接到一个原型对象，并且它可以从中继承属性。所有通过对象字面量创建的对象都连接到 Object.prototype 这个JavaScript中标准的对象。

    当你创建一个新对象时，你可以选择某个对象作为它的原型。如下我们将给Object增加一个beget方法，这个方法将创建一个使用原对象作为其原型的新对象。

    <pre>`if(typeof Object.beget !== 'function'){
        Object.beget = function(o){
            var F = function(){};
            F.prototype = o;
            return new F();
        };
    }

    var another_stooge = Object.beget(stooge);
    `</pre>

    如果我们尝试去获取对象的某个属性值，且该对象没有此属性名，那么JavaScript会试着从原型对象中获取属性值。如果哪个原型对象也没有该属性，那么再从它的原型中寻找，依次类推，直到该过程最后到达终点Object.prototype.如果想要的属性完全不存在于原型链中，那么结果就是undefined值。这个过程称为**委托**。

    原型关系是一种动态的关系，如果我们添加一个新的属性到原型中，该属性会立即对所有基于该原型创建的对象可见。

    <pre>`stooge.profession = 'actor';
    another_stooge.profession   // 'actor'
    `</pre>

    ### 5\. 反射

    检查对象并确定对象有什么属性是很容易的事情，只要试着去检索该属性并验证取得的值。typeof操作符对确定属性的类型很有帮助：

    <pre>`typeof flight.number    // 'number'
    typeof flight.status    // 'string'
    typeof flight.arrival   // 'object'
    typeof flight.manifest  // 'undefined'
    `</pre>

    但是注意原型链中德任何属性也会产生一个值：

    <pre>`typeof flight.toString  // 'function'
    typeof flight.constructor // 'function'
    `</pre>

    可以使用 hasOwnProperty 方法剔除这些不需要的属性：

    <pre>`flight.hasOwnProperty('number')       // true
    flight.hasOwnProperty('constructor')  // false
    `</pre>

    ### 6\. 枚举

    for in 语句可用来遍历一个对象中德所有属性名。并且遍历过程将会列出所有的属性——包括函数和你可能不关心的原型中德属性——所以有必要过滤掉那些不想要的值。

    <pre>`var name;
    for (name in another_stooge) {
        if(typeof another_stooge[name] !== 'function') {
            document.writeln(name + ': ' + another_stooge[name]);
        }
    }
    `</pre>

    属性名出现的顺序是不确定的，如果想要确保属性以特定的顺序出现，最好的方法就是完全避免使用 for in 语句，而是创建一个数组，在其中以正确的顺序包含属性名，通过使用 for 而不是 for in,可以得到我们想要的属性。

    ### 7\. 删除

    delete 运算符可以用来删除对象的属性。它将会移除对象中确定包含的属性。它不会触及原型中德任何对象。删除对象的属性可能会让来自原型中的属性浮现出来：

    <pre>`another_stooge.nickname // 'Moe'
    // 删除 another_stooge 的 nickname 属性，从而暴露出原型的 nickname 属性
    delete another_stooge.nickname;
    another_stooge.nickname  // 'Curly'
    `</pre>

    ### 8\. 减少全局变量污染

    最小化使用全局变量的一个方法是在你的应用中只创建唯一一个全局变量：

    <pre>`var MYAPP = {};
    `</pre>

    然后将此变量作为你的应用的容器：

    <pre>`MYAPP.stooge = {
        &quot;first-name&quot;: &quot;Joe&quot;,
        &quot;last-name&quot;: &quot;Howard&quot;   
    };

    MYAPP.flight = {
        ....
    }
    