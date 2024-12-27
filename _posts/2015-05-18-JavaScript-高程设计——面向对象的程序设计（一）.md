---
layout: post
title: "JavaScript 高程设计——面向对象的程序设计（一）"
subtitle: ""
author: "yangchw"
catalog: false
date: 2015-05-18 18:29:44
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - javascript
---

## 理解对象

ECMA-262 把对象定义为：“无序属性的集合，其属性可以包含基本值、对象或者函数。”

### 属性类型

ECMA-262 第5版在定义只有内部才用的特性（attribute）时，描述了属性（property）的各种特征。ECMA-262 定义这些特性是为了实现 JavaScript 引擎用的，因此在 JavaScript 中不能直接访问它们。

ECMAScript 中有两种属性：**数据属性**和**访问器属性**。

**1.数据属性**

数据属性包含一个数据值的位置。在这个位置可以读取和写入值。数据属性有4个描述其行为的特性。

*   `[[Configurable]]`：表示能否通过 `delete` 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 `true`。

*   `[[Enumerable]]`：表示能否通过 `for-in` 循环返回属性。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 `true`。

*   `[[Writable]]`：表示能否修改属性的值。像前面例子中那样直接在对象上定义的属性，它们的这个特性默认值为 `true`。

*   `[[Value]]`：包含这个属性的数据值。读取属性值的时候，从这个位置读；写入属性值的时候，把新值保存在这个位置。这个特性的默认值为 `undefined`。

使用 ECMAScript 5 的 `Object.defineProperty()` 方法修改属性默认的特性。这个方法接收三个参数：属性所在的对象、属性的名字和一个描述符对象。其中，描述符（descrip-tor）对象的属性必须是：`configurable`、`enumerable`、`writable` 和 `value`。

    var person = {};
    Object.defineProperty(person, &quot;name&quot;, {    
        writable: false,
        value: &quot;Nicholas&quot;
    });

    //&quot;Nicholas&quot;
    alert(person.name);
    person.name = &quot;Greg&quot;;
    //&quot;Nicholas&quot;
    alert(person.name);
    `</pre>

    **2\. 访问器属性**

    访问器属性不包含数据值；它们包含一对儿 `getter` 和 `setter` 函数。在读取访问器属性时，会调用 `getter` 函数，这个函数负责返回有效的值；在写入访问器属性时，会调用 `setter` 函数并传入新值，这个函数负责决定如何处理数据。访问器属性有如下4个特性。

*   `[[Configurable]]`：表示能否通过 `delete` 删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为数据属性。对于直接在对象上定义的属性，这个特性的默认值为 `true`。
*   `[[Enumerable]]`：表示能否通过 `for-in` 循环返回属性。对于直接在对象上定义的属性，这个特性的默认值为 `true`。
*   `[[Get]]`：在读取属性时调用的函数。默认值为 `undefined`。
*   `[[Set]]`：在写入属性时调用的函数。默认值为 `undefined`。

    ## 创建对象

    ### 工厂模式

    <pre>`function createPerson(name, age, job){
        var o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function(){
            alert(this.name);
        };    
        return o;
    }

    var person1 = createPerson(&quot;Nicholas&quot;, 29, &quot;Software Engineer&quot;);
    var person2 = createPerson(&quot;Greg&quot;, 27, &quot;Doctor&quot;);

    person1.sayName();   //&quot;Nicholas&quot;
    person2.sayName();   //&quot;Greg&quot;
    `</pre>

    工厂模式虽然解决了创建多个相似对象的问题，但却没有解决对象识别的问题（即怎样知道一个对象的类型）。

    ### 构造函数模式

    <pre>`function Person(name, age, job){
        this.name = name;
        this.age = age;
        this.job = job;
        this.sayName = function(){
            alert(this.name);
        };    
    }

    var person1 = new Person(&quot;Nicholas&quot;, 29, &quot;Software Engineer&quot;);
    var person2 = new Person(&quot;Greg&quot;, 27, &quot;Doctor&quot;);

    person1.sayName();   //&quot;Nicholas&quot;
    person2.sayName();   //&quot;Greg&quot;

    alert(person1 instanceof Object);  //true
    alert(person1 instanceof Person);  //true
    alert(person2 instanceof Object);  //true
    alert(person2 instanceof Person);  //true

    alert(person1.constructor == Person);  //true
    alert(person2.constructor == Person);  //true

    alert(person1.sayName == person2.sayName);  //false 
    // 以这种方式创建函数，会导致不同的作用域链和标识符解析，但创建 Function 新实例的机制仍然是相同的
    `</pre>

    任何函数，只要通过new操作符来调用，那它就可以作为构造函数；而任何函数，如果不通过 `new` 操作符来调用，那它跟普通函数也不会有什么两样。

    ### 原型模式

    使用原型对象的好处是可以让所有对象实例共享它所包含的属性和方法。

    <pre>`function Person(){
    }

    Person.prototype.name = &quot;Nicholas&quot;;
    Person.prototype.age = 29;
    Person.prototype.job = &quot;Software Engineer&quot;;
    Person.prototype.sayName = function(){
        alert(this.name);
    };

    var person1 = new Person();
    person1.sayName();   //&quot;Nicholas&quot;

    var person2 = new Person();
    person2.sayName();   //&quot;Nicholas&quot;

    alert(person1.sayName == person2.sayName);  //true

    // // 内部都有一个指向Person.prototype的指针，因此都返回了true
    alert(Person.prototype.isPrototypeOf(person1));  //true
    alert(Person.prototype.isPrototypeOf(person2));  //true

    //only works if Object.getPrototypeOf() is available
    if (Object.getPrototypeOf){
        alert(Object.getPrototypeOf(person1) == Person.prototype);  //true
        alert(Object.getPrototypeOf(person1).name);  //&quot;Nicholas&quot;
    }
    `</pre>

    `prototype` 属性指向函数的原型对象。在默认情况下，所有原型对象都会自动获得一个 `constructor`（构造函数）属性，这个属性包含一个指向 `prototype` 属性所在函数的指针。

    通过使用 `hasOwnProperty()` 方法，什么时候访问的是实例属性，什么时候访问的是原型属性就一清二楚了。

    要取得对象上所有可枚举的实例属性，可以使用 EC-MAScript 5 的 `Object.keys()` 方法。这个方法接收一个对象作为参数，返回一个包含所有可枚举属性的字符串数组。

    如果你想要得到所有实例属性，无论它是否可枚举，都可以使用 `Object.getOwnPropertyNames()` 方法。

    <pre>`var keys = Object.keys(Person.prototype);
    //&quot;name,age,job,sayName&quot;
    alert(keys);

    var keys1 = Object.getOwnPropertyNames(Person.prototype); 
    //&quot;constructor,name,age,job,sayName&quot; 
    alert(keys1);
    `</pre>

    **更简单的原型语法**

    <pre>`function Person(){
    }

    Person.prototype = {
        constructor : Person, 
        name : &quot;Nicholas&quot;,
        age : 29,
        job: &quot;Software Engineer&quot;,
        sayName : function () {
            alert(this.name);
        }
    };

    //重设构造函数，只适用于ECMAScript 5兼容的浏览器 
    Object.defineProperty(Person.prototype, &quot;constructor&quot;, {     
        enumerable: false,     
        value: Person 
    });  

    var friend = new Person();

    alert(friend instanceof Object);  //true
    alert(friend instanceof Person);  //true
    alert(friend.constructor == Person);  //false
    alert(friend.constructor == Object);  //true

**原型对象的问题**

原型模式的最大问题是由其共享的本性所导致的。对于包含引用类型值的属性来说，会导致创建的对象共享同一个属性，其中一个修改其他的实例也会被修改。