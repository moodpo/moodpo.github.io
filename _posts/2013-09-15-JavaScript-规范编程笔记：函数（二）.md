---
layout: post
title: "JavaScript 规范编程笔记：函数（二）"
subtitle: ""
author: "yangchw"
catalog: false
date: 2013-09-15 11:26:00
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - javascript
---

### 1\. 给类型增添方法

JavaScript 允许给语言的基本类型增加方法。以下是各种例子：

    // 通过给 Function.prototype 增加方法使得该方法对所有函数可用
    Function.prototype.method = function(name, func){
        this.prototype[name] = func;
        return this;    
    };

    // 给数字类型添加一个提取数字中的整数部分的方法
    Number.method('integer', function(){
        return Math[this &lt; 0 ? 'ceiling' : 'floor'](this);
    });

    document.writeln((-10 / 3).integer());  // -3

    // 给字符串添加一个 trim 方法
    String.method('trim', function(){
        return this.replace(/^\s+|\s+$/g, '');
    });

    document.writeln('&quot;' + &quot; neat &quot;.trim() + '&quot;');
    `</pre>

    基本类型的原型是公共的结构，所以在类库混用时需要判断，只在确定没有该方法时才添加它。

    <pre>`// 有条件的增加一个方法
    Function.prototype.method = function(name, func){
        if(!this.prototype[name]){
            this.prototype[name] = func;
        }
        return this;
    };
    `</pre>

    ### 2\. 变量作用域

    JavaScript只有函数作用域，而不支持会计作用域。那意味着定义在函数中德参数和变量在函数外部是不可见的，而且在一个函数中的任何位置定义的变量在该函数中的任何地方都可见。

    ### 3\. 闭包

    作用域的好处是内部函数可以访问定义它们的外部函数的参数和变量。一个更有趣的情形是内部函数拥有比它的额外部函数更长的生命周期。

    <pre>`// 此函数定义了一个value变量，该变量对increment和getValue方法总是可用的，
    // 但函数的作用域使得它对其他的程序来说是不可见的

    // 该函数将返回一个包含两个方法的对象。
    var myObject = function(){
        var value = 0;
        return {
            increment: function(inc) {
                value += typeof inc === 'number' ? inc : 1;
            },
            getValue: function(){
                return value;
            }
        };
    }();
    `</pre>

    下面是一个更有意义的函数。

    <pre>`// 创建一个名为quo的构造函数
    // 它构造出带有get_status方法和status私有属性的一个对象。
    var quo = function(status){
        return {
            get_status: function(){
                return status;
            }
        };
    };

    var myQuo = quo('amazed');
    document.writeln(myQuo.get_status());
    `</pre>
    <!-- more -->
    当我们调用quo时，它返回包含get_status方法的一个新对象。该对象的一个引用保存在myQuo中。即使quo已经返回了，但get_status方法仍然享有访问quo对象的status属性的特权。get_status方法并不是访问该参数的一个拷贝，它访问的就是该参数本身。这使可能的，因为函数可以访问它被创建时所处的上下文环境，这被称为**闭包**。
    <pre>`// 另一个更有用的例子
    var fade = function(node){
        var level = 1;
        var step = function(){
            var hex = level.toString(16);
            node.style.backgroundColor = '#FFFF' + hex + hex;
            if(level &lt; 15){
                level += 1;
                setTimeout(step, 100);
            }
        };

        setTimeout(step, 100);
    }

    fade(document.body);
    `</pre>

    理解内部函数能访问外部函数的实际变量而无需复制是很重要的：

    <pre>`// 糟糕的例子

    // 当点击一个节点时，按照预想应该弹出一个对话框显示节点的序号
    // 但它总是会显示节点的数目
    var add_the_handlers = function(nodes){
        var i;
        for (i = 0; i &lt; nodes.length; i += 1) {
            nodes[i].onclick = function(){
                alert(i);
            }
        };
    };
    `</pre>

    add_the_handlers 函数目的是给每个事件处理器一个唯一值(i)。它未能达到目的是因为事件处理器函数绑定了变量`i`，而不是函数在构造时的变量`i`的值。

    <pre>`// 更好的例子
    var add_the_handlers_bb = function(nodes){
        var i;
        for (i = 0; i &lt; nodes.length; i += 1) {
            nodes[i].onclick = function(b){
                return function(){
                    alert(b);
                };
            }(i);
        };
    };
    `</pre>

    现在，我们定义了一个函数并立即传递i进去执行，而不是把一个函数赋值给onclick。那个函数将返回一个事件处理器函数。这个事件处理器函数绑定的是传递进去的i的值，而不是定义在add_the_handlers函数里的i的值。那个被返回的函数将被赋值给onclick。

    ### 4\. 模块

    我们可以使用函数和闭包来构造模块。模块是一个提供接口却隐藏状态与实现的函数或对象。

    以下是一个模块的例子，它的任务是寻找字符串中的HTML字符实体并替换为它们对应的字符。它保存字符实体的名字和它们对应的字符放入一个闭包中。

    <pre>`String.method('deentityify', function(){
        // 字符实体表，它映射字符实体的名字到对应的字符。
        var entity = {
            quot: '&quot;',
            lt: '&lt;',
            gt: '&gt;'
        };

        return function(){
            // 这才是 deentityify方法
            return this.replace(/&amp;([^&amp;;]+);/g, 
                    function(a,b){
                        var r = entity[b];
                        return typeof r === 'string' ? r : a;
                    }
                );
        };

    }());

    document.writeln('&amp;lt;&amp;quot;&amp;gt;'.deentityify()); // &lt;&quot;&gt;
    `</pre>

    请注意最后一行，我们用()运算符立即调用我们刚刚构造出来的函数。这个调用所创建并返回的函数才是deentityify方法。在这个例子中，只有deentityify方法有权访问字符实体表这个数据对象。

    模块模式的一般形式是：一个定义了私有变量和函数的函数；利用闭包创建可以访问私有变量和函数的特权函数；最后返回这个特权函数，或者把它们保存到一个可访问到的地方。

    由此，我们可以使用模块模式产生安全的对象，类似JavaBean的形式：

    <pre>`var serial_maker = function(){

        var prefix = '';
        var seq = 0;
        return {
            set_prefix: function(p){
                prefix = String(p);
            },
            set_seq: function(s){
                seq = s;
            },
            gensym: function(){
                var result = prefix + seq;
                seq += 1;
                return result;
            }
        };
    };

    var seqer = serial_maker();
    seqer.set_prefix('Q');
    seqer.set_seq(1000);
    var unique = seqer.gensym();
    