---
layout: post
title: "JavaScript 规范编程笔记：继承"
subtitle: ""
author: "yangchw"
catalog: false
date: 2013-09-28 11:15:00
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - javascript
---

JavaScript是一门弱类型的语言，从不需要类型转换。对象的起源是无关紧要的。对于一个对象来说重要的事它能做什么，而不是它从哪里来。JavaScript提供了一套更为丰富的代码重用模式。它可以模拟那些基于类的模式，同时它也可以支持其他更具表现力的模式。在JavaScript中可能的继承模式有很多。下面将介绍几种最为直接的模式。

在基于类的语言中，对象是类的实例，并且类可以从另一个类继承。JavaScript是一门基于原型的语言，这意味着对象直接从其他对象继承。

### 1\. 伪类

当一个函数对象被创建时，Function构造器产生的函数对象会运行类似这样的一些代码：

    this.prototype = {constructor: this};
    `</pre>

    该prototype对象是存放继承特征的地方。因为JavaScript语言没有提供一种方法去确定哪个函数是打算用来作构造器的，所以每个函数都会得到一个prototype对象。

    当采用构造器调用模式，即使用new前缀去调用一个函数时，这将修改函数执行的方式。如果new运算符是一个方法而不是一个运算符，它将可能会像这样执行：

    <pre>`Function.method('new', function(){

        // 创建一个新对象，它继承自构造器函数的原型对象
        var that = Object.beget(this.prototype);

        // 调用构造器函数，绑定 this 到新对象上
        var other = this.apply(that, arguments);

        // 如果它的返回值不是一个对象，就返回该新对象
        return (typeof other === 'object' &amp;&amp; other) || that;
    });
    `</pre>

    以下是一个示例：

    <pre>`var Mammal = function(name){
        this.name = name;
    };

    Mammal.prototype.get_name = function(){
        return this.name;
    };

    Mammal.prototype.says = function(){
        return this.saying || '';
    };

    var myMammal = new Mammal('Herb the Mammal');
    var name = myMammal.get_name();
    `</pre>

    以下我们可以构造另一个伪类来继承Mammal，通过定义它的构造函数并替换它的prototype为一个Mammal的实例来实现：

    <pre>`var Cat = function(name){
        this.name = name;
        this.saying = 'meow';
    };

    Cat.prototype = new Mammal();

    Cat.prototype.purr = function(n){
        var i, s = '';
        for(i = 0; i &lt; n; i += 1){
            if(s){
                s += '-';
            }
            s += 'r';
        }
        return s;
    };

    Cat.prototype.get_name = function(){
        return this.says() + ' ' + this.name + ' ' + this.says();
    };  

    var myCat = new Cat('Henrietta');
    var says = myCat.says();
    var purr = myCat.purr(5);
    var name = myCat.get_name();

    document.writeln('says : ' + says + '; purr : ' + purr + '; name : ' + name);
    // says : meow; purr : r-r-r-r-r; name : meow Henrietta meow
    `</pre>

    伪类模式本意是想向面向对象靠拢，但是它看起来格格不入。我们甚至可以自行定义一个inherits方法来隐藏内部的细节：

    <pre>`Function.method('inherits', function(Parent){
        this.prototype = new Parent();
        return this;
    });
    `</pre>

    伪类形式可以给不熟悉JavaScript的程序员提供便利，但是它也隐藏了该语言的真实本质。借鉴类的表示法可能误导程序员去编写过于深入复杂的层次结构。许多复杂的类层次结构产生的原因就是静态类型检查的约束。JavaScript完全摆脱了那些约束。在基于类的语言中，类的继承是代码重用的唯一方式。显然，JavaScript有着更多且更好的选择。

    ### 2\. 对象说明符

    <pre>`var myObject = maker({
        first: f,
        last: l,
        state: s,
        city: c
    });
    `</pre>

    当与JSON一起工作时，这还可以有一个间接的好处。JSON文本只能描述数据，但有时数据表示的是一个对象，如果构造器取得一个对象说明符，可以容易的做到，因为我们可以简单的传递该JSON对象给构造器，而它将返回一个构造完全的对象。

    ### 3\. 原型

    基于原型的继承相比于基于类的继承在概念上更为简单：一个新对象可以继承一个旧对象的属性。通过构造一个有用的对象开始，接着可以构造更多和那个对象类似的对象。可以完全避免把一个应用拆解成一系列嵌套抽象类的分类过程。

    <pre>`var myMa = {
        name : 'Herb the Mammal',
        get_name : function(){
            return this.name;
        },
        says : function(){
            return this.saying || '';
        }
    }; 

    // 差异化继承
    var myCatMa = Object.beget(myMa);

    myCatMa.name = 'Henrietta';
    myCatMa.saying = 'meow';
    myCatMa.purr = function(n){
        var i, s = '';
        for(i = 0; i &lt; n; i += 1){
            if(s){
                s += '-';
            }
            s += 'r';
        }
        return s;
    };
    myCatMa.get_name = function(){
        return this.says() + ' ' + this.name + ' ' + this.says();
    };

    document.writeln(myCatMa.get_name());
    `</pre>

    ### 4\. 函数化

    上述几种继承方式的一个弱点是我们没法保护隐私，对象的所有属性都是可见的。幸运的是，我们有一个更好的选择，那就是模块模式的应用。以下是构造一个产生对象的函数的步骤：

1.  创建一个新对象，可以使用多种方式；
2.  选择性的定义私有实例变量和方法；
3.  给这个新对象扩充方法，这些方法将拥有特权去访问参数，以及新定义的变量；
4.  返回新对象。

    以下是上述步骤的伪码模板：

    <pre>`var constructor = function (spec, my){
        var that, ……；// 其他的私有实例变量
        my = my || {};

        // 把共享的变量和函数添加到my中

        that = // 一个新对象

        // 添加给 that 的特权方法

        return that;
    };
    `</pre>

    spec对象包含构造器需要构造一个新实例的所有信息。spec的内容可能会被复制到私有变量中，或者被其他函数改变，或者方法可以在需要的时候访问spec的信息。my对象是一个为继承链中的构造器提供共享的容器。my 对象可以选择性的使用。

    让我们将这个步骤应用到mammal例子中，此处不需要my，所以我们先抛开它，但将使用一个spec对象：

    <pre>`var mammal = function(spec){
        var that = {};
        that.get_name = function(){
            return spec.name;
        };

        that.says = function(){
            return spec.saying || '';
        };

        return that;
    };

    var myMammal = mammal({name: 'Herb'});

    document.writeln(myMammal.get_name());

    var cat = function(spec){
        spec.saying = spec.saying || 'meow';
        var that = mammal(spec);
        that.purr = function(n){
            var i, s = '';
            for(i = 0; i &lt; n; i += 1){
                if(s){
                    s += '-';
                }
                s += 'r';
            }
            return s;
        };
        that.get_name = function(){
            return that.says() + ' ' + spec.name + ' ' + that.says();
        };

        return that;
    };

    var myCats = cat({name: 'Cate',saying: 'Ni Ma B'});

    document.writeln(myCats.get_name());
    `</pre>

    函数化模式还给我们提供了一个处理父类方法的方法：

    <pre>`Object.method('superior',function(name){
        var that = this,
            method = that[name];
        return function(){
            return method.apply(that, arguments);
        };
    });
    `</pre>

    以下是在coolcat上的实验，它将返回父对象cat中的方法执行结果：

    <pre>`var coolcat = function(spec){
        var that = cat(spec),
            super_get_name = that.superior('get_name');
        that.get_name = function(n){
            return 'like ' + super_get_name() + ' bady';
        };
        return that;
    };

    var myCoolCat = coolcat({name: 'Bix'});
    // like meow Bix meow baby

函数化模式有很大的灵活性。它不仅不像伪类模式那样需要很多功夫，还让我们得到更好的封装和信息隐藏，以及访问父类方法的能力。

如果使用函数化的样式创建一个对象，并且该对象的所有方法都不使用this 或 that，那么该对象就是持久性的。一个持久性的对象就是一个简单功能函数的集合。