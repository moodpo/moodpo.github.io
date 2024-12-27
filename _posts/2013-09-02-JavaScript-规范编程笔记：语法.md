---
layout: post
title: "JavaScript 规范编程笔记：语法"
subtitle: ""
author: "yangchw"
catalog: false
date: 2013-09-02 19:47:00
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - javascript
---

![JavaScript 语法](http://moodpo.com/usr/uploads/2014/01/1263846946.jpg)

本节将简要介绍JavaScript的语法，并简要地概述其语言结构。

### 1\. 注释

JavaScript提供了两种注释形式，一种是用`/* */`包围的块状注释，另一种是以`//`为开头的行注释。注释应该被充分用来提高程序的可读性。必须注意的是，注释一定要精确的描述代码。没有用的注释比没有注释更糟糕。

其中，块注释对于被注释的代码块来说是不安全的额。例如下面的代码将导致一个语法错误：

    /*
        var rm_a = /a*/.match(s);
    */
    `</pre>

    因此应避免使用`/* */`注释，而用`//`注释代替它。

    ### 2\. 标识符

    JavaScript保留字列表：

    <table><tbody><tr valign="top"><td>break</td><td>delete</td><td>function</td><td>return</td><td>typeof</td></tr><tr valign="top"><td>case</td><td>do</td><td>if</td><td>switch</td><td>var</td></tr><tr valign="top"><td>catch</td><td>else</td><td>in</td><td>this</td><td>void</td></tr><tr valign="top"><td>continue</td><td>false</td><td>instanceof</td><td>throw</td><td>while</td></tr><tr valign="top"><td>debugger</td><td>finally</td><td>new</td><td>true</td><td>with</td></tr><tr valign="top"><td>default</td><td>for</td><td>null</td><td>try</td><td>&nbsp;</td></tr></tbody></table>

    JavaScript未来保留字（这些单词虽然现在没有用到JavaScript中，但将来有可能用到）：

    <table><tbody><tr valign="top"><td>abstract</td><td>double</td><td>goto</td><td>native</td><td>static</td></tr><tr valign="top"><td>boolean</td><td>enum</td><td>implements</td><td>package</td><td>super</td></tr><tr valign="top"><td>byte</td><td>export</td><td>import</td><td>private</td><td>synchronized</td></tr><tr valign="top"><td>char</td><td>extends</td><td>int</td><td>protected</td><td>throws</td></tr><tr valign="top"><td>class</td><td>final</td><td>interface</td><td>public</td><td>transient</td></tr><tr valign="top"><td>const</td><td>float</td><td>long</td><td>short</td><td>volatile</td></tr></tbody></table>

    注意，其中不包括`undefined`、`NaN`和`Infinity`等这些本应被保留的字。

    ### 3\. 数字

    JavaScript中只有一个单一的数字类型，它在内部被表示为64位的浮点数，和Java的`double`一样。因此`1`和`1.0`是相同的值。

    指数表示法，如`100`可表示为`1e2`。负数可以用前缀运算符`-`来构成。值`NaN`是一个数值，它表示一个不能产生正常结果的运算结果。`NaN`不等于任何值，包括它自己。你可以使用函数`isNaN(number)`检测`NaN`。值`Infinity`表示所有大于1.79769313486231570e+308的值。JavaScript提供了一个对象`Math`,它包括一套作用于数字的方法。

    ### 4\. 字符串

    字符串字面量使用单引号或双引号包围，它可能包含0个或多个字符。\（反斜杠）是转义符。JavaScript在被创建的时候，Unicode是一个16位的字符集，因此JavaScript中德所有字符都是16位的。

    字符串有一个`length`属性，例如`&quot;seven&quot;.length`是5。字符串使用`+`运算符进行连接，此外JavaScript还为字符串提供了一些方法。

    ### 5\. 语句

    一个编译单元包含一组可执行的语句。在web浏览器中，每个`&lt;script&gt;`标签都提供一个被编译且立即执行的编译单元。因为缺少链接器，JavaScript把它们一起抛入一个公共的全局名字空间中。

    代码块是包在一对花括号的一组语句。不像许多其他的语言，JavaScript中德代码块不会创建一个新的作用域，因此变量应该被定义在函数的顶端，而不是代码块中。

    当`var`语句被用在函数的内部时，它定义了这个函数的私有变量。

    下面的值被当作假(false):

*   false
*   null
*   undefined
*   空字符串 ''
*   数字 0
*   数字 NaN

    其他所有的值都被当作真，包括`true`、字符串`&quot;false&quot;`，以及所有的对象。

    for in 语句会枚举一个对象的所有属性名（或键名），在每次循环中，对象的另一个属性名字符串被赋值给for和in之间的变量。通常你需要通过检测`object.hasOwnProperty(variable)`来确定这个属性名就是该对象的成员，还是从其原型链里找到的。

    <pre>`for(myvar in obj){
        if(obj.hasOwnProperty(myvar)){
            ...
        }
    }

### 6\. 表达式

最简单的表达式是字面量值（比如字符串或数字）、变量、内置的值（true、false、null、undefined、NaN和Infinity）、以new前导的调用表达式、以delete前导的属性存取表达式、包在圆括号中德表达式、以一个前缀运算符作为前导的表达式，或者表达式后面跟着：

*   一个插入运算符与另一个表达式；
*   三元运算符?后面跟着另一个表达式，然后接一个:，再然后接第三个表达式；
*   一个函数调用；
*   一个属性存取表达式。

函数调用引发函数的执行，函数调用运算符是跟随在函数名后面的一对圆括号。圆括号中可能包含将会传递给这个函数的参数。一个属性存取表达式用于指定一个对象或数组的属性或元素。

### 7\. 字面量

对象字面量是一种方便指定新对象的表示法。属性名可以是标示符或字符串。这些名字被当作字面量名而不是变量名来对待，所以对象的属性名在编译时才能知道。属性的值就是表达式。

### 8\. 函数

函数字面量定义了函数值，它可以有一个可选的名字，用于递归的调用自己。它可以指定一个参数列表，这些参数将作为变量由调用时传递的实际参数（arguments）初始化。函数的主体包括变量定义的语句。