---
layout: post
title: "JavaScript 高程设计——引用类型（一）"
subtitle: ""
author: "yangchw"
catalog: false
date: 2015-05-15 11:29:00
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - javascript
---

引用类型的值（对象）是引用类型的一个实例。在 ECMAScript 中，引用类型是一种数据结构，用于将数据和功能组织在一起。

对象是某个特定引用类型的实例。新对象是使用 `new` 操作符后跟一个构造函数来创建的。构造函数本身就是一个函数，只不过该函数是出于创建新对象的目的而定义的。

    var person = new Object();
    `</pre>

    ## Object 类型

    创建 Object 实例的方式有两种。第一种是使用 `new` 操作符后跟 Object 构造函数，如下所示：

    <pre>`var person = new Object();
    person.name = &quot;Nicholas&quot;;
    person.age = 29;
    `</pre>

    另一种方式是使用对象字面量表示法。对象字面量是对象定义的一种简写形式，目的在于简化创建包含大量属性的对象的过程。

    <pre>`var person = {    
        name : &quot;Nicholas&quot;,    
        age : 29
    };
    `</pre>

    ## Array 类型

    创建数组的基本方式有两种。第一种是使用 Array 构造函数，如下面的代码所示。

    <pre>`var colors = new Array();
    // 创建 length 值20的数组
    var colors = new Array(20); 
    // 码创建了一个包含3个字符串值的数组
    var colors = new Array(&quot;red&quot;, &quot;blue&quot;, &quot;green&quot;);

    // 创建一个包含3项的数组
    var colors = Array(3);
    // 创建一个包含1项，即字符串&quot;Greg&quot;的数组
    var names = Array(&quot;Greg&quot;);

    // 创建一个包含3个字符串的数组
    var colors = [&quot;red&quot;, &quot;blue&quot;, &quot;green&quot;];
    `</pre>

    数组最多可以包含`4 294 967 295`个项，如果想添加的项数超过这个上限值，就会发生异常。

    ### 检测数组

    对于一个网页，或者一个全局作用域而言，使用 `instanceof` 操作符就能得到满意的结果：

    <pre>`if (value instanceof Array){    
        //对数组执行某些操作
    }
    `</pre>

    对于有还有多个框架的页面，需要使用 ECMAScript 5 新增的 `Array.isArray()` 方法：

    <pre>`if (Array.isArray(value)){    
        //对数组执行某些操作
    }
    `</pre>

    如果数组中的某一项的值是 `null` 或者 `undefined`，那么该值在 `join()`、`toLocaleString()`、`toString()` 和 `valueOf()`方法返回的结果中以空字符串表示。

    ### 栈方法

    `push()` 方法可以接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后数组的长度。而 `pop()` 方法则从数组末尾移除最后一项，减少数组的 `length` 值，然后返回移除的项。

    ### 队列方法

    `shift()` 能够移除数组中的第一个项并返回该项，同时将数组长度减1。`unshift()` 与 `shift()` 的用途相反：它能在数组前端添加任意个项并返回新数组的长度。

    ### 重排序方法

    数组中已经存在两个可以直接用来重排序的方法：`reverse()` 和 `sort()`。

    比较函数接收两个参数，如果第一个参数应该位于第二个之前则返回一个负数，如果两个参数相等则返回0，如果第一个参数应该位于第二个之后则返回一个正数。

    <pre>`function compare(value1, value2) {    
        if (value1 &lt; value2) {        
            return 1;     
        } else if (value1 &gt; value2) {        
            return -1;     
        } else {        
            return 0;    
        }
    }

    var values = [0, 1, 5, 10, 15];
    values.sort(compare);

    // 15,10,5,1,0
    alert(values);
    `</pre>

    `reverse()` 和 `sort()` 方法的返回值是经过排序之后的数组。

    ### 操作方法

*   **concat**: `concat()` 方法可以基于当前数组中的所有项创建一个新数组。具体来说，这个方法会先创建当前数组一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组。在没有给 `concat()` 方法传递参数的情况下，它只是复制当前数组并返回副本。如果传递给 `concat()` 方法的是一或多个数组，则该方法会将这些数组中的每一项都添加到结果数组中。如果传递的值不是数组，这些值就会被简单地添加到结果数组的末尾。
*   **slice**: `slice()` 能够基于当前数组中的一或多个项创建一个新数组。`slice()`方法可以接受一或两个参数，即要返回项的起始和结束位置。在只有一个参数的情况下，`slice()`方法返回从该参数指定位置开始到当前数组末尾的所有项。如果有两个参数，该方法返回起始和结束位置之间的项——但不包括结束位置的项。
*   **splice**: `splice()` 的主要用途是向数组的中部插入项，但使用这种方法的方式则有如下3种。
    <pre>`var colors = [&quot;red&quot;, &quot;green&quot;, &quot;blue&quot;];
    // 删除第一项
    var removed = colors.splice(0,1);
    // green,blue
    alert(colors);
    // red，返回的数组中只包含一项
    alert(removed);

    // 从位置1开始插入两项
    removed = colors.splice(1, 0, &quot;yellow&quot;, &quot;orange&quot;);
    // green,yellow,orange,blue
    alert(colors);
    // 返回的是一个空数组
    alert(removed);

    // 插入两项，删除一项
    removed = colors.splice(1, 1, &quot;red&quot;, &quot;purple&quot;);
    // green,red,purple,orange,blue
    alert(colors);
    // yellow，返回的数组中只包含一项
    alert(removed);
    `</pre>

    ### 位置方法

    ECMAScript 5 为数组实例添加了两个位置方法：`indexOf()` 和 `lastIndexOf()` 。这两个方法都接收两个参数：要查找的项和（可选的）表示查找起点位置的索引。其中，`indexOf()` 方法从数组的开头（位置0）开始向后查找，`lastIndexOf()` 方法则从数组的末尾开始向前查找。

    ### 迭代方法

    ECMAScript 5 为数组定义了5个迭代方法。每个方法都接收两个参数：要在每一项上运行的函数和（可选的）运行该函数的作用域对象——影响 `this` 的值。传入这些方法中的函数会接收三个参数：数组项的值、该项在数组中的位置和数组对象本身。

*   **every()**：对数组中的每一项运行给定函数，如果该函数对每一项都返回 `true`，则返回 `true`。
*   **filter()**：对数组中的每一项运行给定函数，返回该函数会返回 `true` 的项组成的数组。
*   **forEach()**：对数组中的每一项运行给定函数。这个方法没有返回值。
*   **map()**：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
*   **some()**：对数组中的每一项运行给定函数，如果该函数对任一项返回 `true`，则返回 `true`。

    ### 缩小方法

    ECMAScript 5 还新增了两个缩小数组的方法：`reduce()` 和 `reduceRight()`。这两个方法都会迭代数组的所有项，然后构建一个最终返回的值。

    这两个方法都接收两个参数：一个在每一项上调用的函数和（可选的）作为缩小基础的初始值。传给 `reduce()` 和 `reduceRight()` 的函数接收4个参数：前一个值、当前值、项的索引和数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在数组的第二项上，因此第一个参数是数组的第一项，第二个参数就是数组的第二项。

    <pre>`var values = [1,2,3,4,5];
    var sum = values.reduce(function(prev, cur, index, array){    
        return prev + cur;
    });

    // 15
    alert(sum);
    `</pre>

    ## Date 类型

    在调用 `Date` 构造函数而不传递参数的情况下，新创建的对象自动获得当前日期和时间。如果想根据特定的日期和时间创建日期对象，必须传入表示该日期的毫秒数。ECMAScript 提供了两个方法：`Date.parse()` 和 `Date.UTC()`。

    <pre>`var now = new Date();

    var someDate = new Date(Date.parse(&quot;May 25, 2004&quot;));
    var someDate = new Date(&quot;May 25, 2004&quot;);

    // GMT时间2000年1月1日午夜零时
    var y2k = new Date(Date.UTC(2000, 0));

    // GMT时间2005年5月5日下午5:55:55
    var allFives = new Date(Date.UTC(2005, 4, 5, 17, 55, 55));

    var y2k = new Date(2000, 0);
    `</pre>

    ECMAScript 5 添加了 `Data.now()` 方法，返回表示调用这个方法时的日期和时间的毫秒数。

    <pre>`// 取得开始时间
    // 在不支持now()的浏览器中使用 var start = +new Date(); 
    var start = Date.now();

    // 调用函数 doSomething();

    // 取得停止时间
    var stop = Date.now(),    
        result = stop – start;

### 日期格式化方法（略）

### 日期/时间组件方法（略）