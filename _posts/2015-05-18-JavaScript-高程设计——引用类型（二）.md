---
layout: post
title: "JavaScript 高程设计——引用类型（二）"
subtitle: ""
author: "yangchw"
catalog: false
date: 2015-05-18 16:36:22
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - javascript
---

# RegExp 类型

ECMAScript 通过 RegExp 类型来支持正则表达式。使用下面类似 Perl 的语法，就可以创建一个正则表达式。

    var expression = / pattern / flags ;
    `</pre>

    正则表达式的匹配模式支持下列3个标志（flags）：`g` ——全局，`i` ——不区分大小写，`m` ——匹配多行。

    <pre>`// 匹配字符串中所有&quot;at&quot;的实例
    var pattern1 = /at/g;
    // 匹配第一个&quot;bat&quot;或&quot;cat&quot;，不区分大小写
    var pattern2 = /[bc]at/i;
    // 匹配所有以&quot;at&quot;结尾的3个字符的组合，不区分大小写
    var pattern3 = /.at/gi;
    // 与pattern1相同，只不过是使用构造函数创建的
    var pattern4 = new RegExp(&quot;[bc]at&quot;, &quot;i&quot;);
    `</pre>

    由于 RegExp 构造函数的模式参数是字符串，所以在某些情况下要对字符进行双重转义。所有元字符都必须双重转义，那些已经转义过的字符也是如此。

    使用正则表达式字面量和使用 RegExp 构造函数创建的正则表达式不一样。在 ECMAScript 3 中，正则表达式字面量始终会共享同一个 RegExp 实例，而使用构造函数创建的每一个新 RegExp 实例都是一个新实例。

    ECMAScript 5 明确规定，使用正则表达式字面量必须像直接调用 RegExp 构造函数一样，每次都创建新的 RegExp 实例。

    ### RegExp 实例属性

    RegExp 的每个实例都具有下列属性，通过这些属性可以取得有关模式的各种信息。

*   `global`：布尔值，表示是否设置了g标志。
*   `ignoreCase`：布尔值，表示是否设置了i标志。
*   `lastIndex`：整数，表示开始搜索下一个匹配项的字符位置，从0算起。
*   `multiline`：布尔值，表示是否设置了m标志。
*   `source`：正则表达式的字符串表示，按照字面量形式而非传入构造函数中的字符串模式返回。

    ### RegExp 实例方法

    RegExp 对象的主要方法是 `exec()`，该方法是专门为捕获组而设计的。`exec()` 接受一个参数，即要应用模式的字符串，然后返回包含第一个匹配项信息的数组；或者在没有匹配项的情况下返回 `null`。返回的数组虽然是 `Array` 的实例，但包含两个额外的属性：`index` 和 `input` 。其中，`index` 表示匹配项在字符串中的位置，而 `input` 表示应用正则表达式的字符串。在数组中，第一项是与整个模式匹配的字符串，其他项是与模式中的捕获组匹配的字符串。

    正则表达式的第二个方法是 `test()`，它接受一个字符串参数。在模式与该参数匹配的情况下返回 `true`；否则，返回 `false`。

    尽管 ECMAScript 中的正则表达式功能还是比较完备的，但仍然缺少某些语言（特别是 Perl）所支持的高级正则表达式特性。

    ## Function 类型

    每个函数都是 Function 类型的实例，而且都与其他引用类型一样具有属性和方法。由于函数是对象，因此函数名实际上也是一个指向函数对象的指针，不会与某个函数绑定。

    在函数内部，有两个特殊的对象：`arguments` 和 `this`。其中，`arguments` 是一个类数组对象，包含着传入函数中的所有参数。虽然 `arguments` 的主要用途是保存函数参数，但这个对象还有一个名叫 `callee` 的属性，该属性是一个指针，指向拥有这个 `arguments` 对象的函数。

    <pre>`arguments.callee
    `</pre>

    `this` 引用的是函数据以执行的环境对象——或者也可以说是 `this` 值。

    ECMAScript 5 中 `caller` 属性中保存着调用当前函数的函数的引用，如果是在全局作用域中调用当前函数，它的值为 `null`。

    当函数在严格模式下运行时，访问 `arguments.callee` 会导致错误。ECMAScript 5 还定义了 `arguments.caller` 属性，但在严格模式下访问它也会导致错误，而在非严格模式下这个属性始终是 `undefined`。严格模式还有一个限制：不能为函数的 `caller` 属性赋值，否则会导致错误。

    ECMAScript 中的函数包含两个属性：length和proto-type。其中，length属性表示函数希望接收的命名参数的个数。对于 ECMAScript 中的引用类型而言，`prototype` 是保存它们所有实例方法的真正所在。

    在 ECMAScript 5 中，`prototype` 属性是不可枚举的，因此使用 `for-in` 无法发现。

    每个函数都包含两个非继承而来的方法：`apply()` 和 `call()` 。这两个方法的用途都是在特定的作用域中调用函数，实际上等于设置函数体内 `this` 对象的值。

    <pre>`function callSum1(num1, num2){    
        // 传入arguments对象    
        return sum.apply(this, arguments);
    }

    function callSum2(num1, num2){    
        // 传入数组    
        return sum.apply(this, [num1, num2]);
    }

`call()` 方法与 `apply()` 方法的作用相同，它们的区别仅在于接收参数的方式不同。对于 `call()` 方法而言，第一个参数是 `this` 值没有变化，变化的是其余参数都直接传递给函数。

事实上，传递参数并非 `apply()` 和 `call()` 真正的用武之地；它们真正强大的地方是能够扩充函数赖以运行的作用域。

ECMAScript 5 还定义了一个方法：`bind()`。这个方法会创建一个函数的实例，其 `this` 值会被绑定到传给 `bind()` 函数的值。

## 基本包装类型

ECMAScript 提供了3个特殊的引用类型：Boolean、Number 和 String。

## 单体内置对象

ECMA-262 还定义了两个单体内置对象：Global 和 Math。Global 对象的 `encodeURI()` 和 `encodeURIComponent()` 方法可以对 URI（Uniform Resource Identifiers，通用资源标识符）进行编码，以便发送给浏览器。

`Math.random()` 方法返回介于0和1之间一个随机数，不包括0和1。