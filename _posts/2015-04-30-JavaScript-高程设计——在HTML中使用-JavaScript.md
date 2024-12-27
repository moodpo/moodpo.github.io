---
layout: post
title: "JavaScript 高程设计——在HTML中使用 JavaScript"
subtitle: ""
author: "yangchw"
catalog: false
date: 2015-04-30 10:48:00
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - javascript
---

## &lt;script&gt; 元素

HTML 4.01 为 `&lt;script&gt;` 定义了以下6个属性：

*   `async`：可选。表示应该立即下载脚本，但不应妨碍页面中的其他操作。只对外部脚本文件有效；
*   `charset`：可选。表示通过 `src` 属性指定的代码的字符集。很少有人用这个属性；
*   `defer`：可选。表示脚本可以延迟到文档完全被解析和显示之后再执行；
*   `language`：已废弃；
*   `src`：可选。表示包含要执行代码的外部文件；
*   `type`：可选。表示编写代码使用的脚本语言的内容类型；目前 `type` 属性的值依旧还是 `text/javascript`。不过这个属性并不是必须的，因为没有指定时默认值也是 `text/javascript`；

在使用 `&lt;script&gt;` 元素嵌入 `JavaScript` 代码时，不要在代码中的任何地方出现 `&quot;&lt;/script&gt;&quot;`字符串。按照接卸嵌入式代码的规则，当浏览器遇到字符串 `&quot;&lt;/script&gt;&quot;` 时，就会认为是结束的 `&lt;/script&gt;`标签。而通过转义字符 `“\”` 可解决这个问题：`&quot;&lt;\/script&gt;&quot;`。例如：

    &lt;script&gt;
        function sayScript(){
            alert(&quot;&lt;\/script&gt;&quot;);
        }
    &lt;/script&gt;
    `</pre>

    在引入外部文件时，只能像下面这样写：

    <pre>`&lt;script type=&quot;application/javascript&quot; src=&quot;example.js&quot;&gt;&lt;/script&gt;
    `</pre>

    而不能使用如下形式：

    <pre>`&lt;script type=&quot;application/javascript&quot; src=&quot;example.js&quot;/&gt;
    `</pre>

    引入了外部文件的 `&lt;script&gt;` 元素不能再嵌入代码，嵌入的代码将会被忽略，只外部文件起作用。

    无论如何包含代码，只要不存在 `defer` 和 `async` 属性，浏览器都会按照 `&lt;script&gt;` 元素在页面中出现的先后顺序对它们依次进行解析。

    ## 标签的位置

    现代 Web 应用程序一般都把全部的 JavaScript 引用放在 <body> 元素中页面内容的后面。如下：

    <pre>`&lt;!DOCTYPE html&gt;
    &lt;html lang=&quot;zh-CN&quot;&gt;
    &lt;head&gt;
        &lt;meta charset=&quot;utf-8&quot;&gt;
        &lt;meta http-equiv=&quot;X-UA-Compatible&quot; content=&quot;IE=edge&quot;&gt;
        &lt;meta name=&quot;viewport&quot; content=&quot;width=device-width, initial-scale=1&quot;&gt;
        &lt;title&gt;&lt;/title&gt;
    &lt;/head&gt;
    &lt;body&gt;

    &lt;script src=&quot;example.js&quot;&gt;&lt;/script&gt;
    &lt;/body&gt;
    &lt;/html&gt;
    `</pre>

    ### 延迟脚本

    使用 `defer` 属性，相当于告诉浏览器立即下载，但延迟执行。IE4/Firefox3.5/Safari5/Chrome 是最早支持此属性的浏览器，其他浏览器会忽略此属性，因此把延迟脚本放在页面底部仍然是最佳选择。使用例子：

    <pre>`&lt;script defer src=&quot;example.js&quot;&gt;&lt;/script&gt;
    `</pre>

    ### 异步脚本

    HTML 5 为 `&lt;script&gt;` 元素定义了 `async` 属性。这个属性与 `defer` 属性类似，都用来改变处理脚本的行为；但与 `defer` 属性不同的是，标记为 `async` 脚本并不保证按照指定它们的先后顺序执行。指定 `async` 属性的目的不是让页面等待两个脚本下载和执行，从而异步加载页面其他内容。因此，建议异步脚本不要在加载期间修改 `DOM`。异步脚本一定会在页面的 `load` 事件前执行。

    <pre>`&lt;script async src=&quot;example.js&quot;&gt;&lt;/script&gt;
    `</pre>

    ## 文档模式

    IE 5.5 引入了文档模式的概念，而这个概念是通过使用文档类型（doctype）切换实现的。最初的两种文档模式是：混杂模式（quirks mode）和标准模式（standards mode）。这两种模式主要影响 CSS 内容的呈现，但在某些情况下也会影响 JavaScript 的解释执行。在此之后，IE 又提出一种所谓的准标准模式（almost standards mode）。

    如果文档开始处没有发现文档类型声明，则所有的浏览器都会默认开启混杂模式；对于标准模式可以通过使用下面任何一种文档类型来开启：

    <pre>`&lt;!-- HTML 4.0.1 严格型 html:4s --&gt;
    &lt;!DOCTYPE HTML PUBLIC &quot;-//W3C//DTD HTML 4.01//EN&quot; &quot;http://www.w3.org/TR/html4/strict.dtd&quot;&gt;
    `</pre>
    <pre>`&lt;!-- HTML 5 !--&gt;
    &lt;!DOCTYPE html&gt;
    `</pre>

    而对于准标准模式，则可以使用过渡型（transitional）或框架集型（frameset）文档类型来触发：

    <pre>`&lt;!-- HTML 4.0.1 过渡型 html:4t --&gt;
    &lt;!DOCTYPE HTML PUBLIC &quot;-//W3C//DTD HTML 4.01 Transitional//EN&quot; 
        &quot;http://www.w3.org/TR/html4/loose.dtd&quot;&gt;
    `</pre>
    <pre>`&lt;!-- HTML 4.0.1 框架集型 --&gt;
    &lt;!DOCTYPE HTML PUBLIC &quot;-//W3C//DTD HTML 4.01 Frameset//EN&quot; 
        &quot;http://www.w3.org/TR/html4/frameset.dtd&quot;&gt;

## &lt;noscript&gt; 元素

在 `&lt;body&gt;` 中使用此元素。