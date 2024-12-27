---
layout: post
title: "JavaScript 高程设计——JavaScript 简介"
subtitle: ""
author: "yangchw"
catalog: false
date: 2015-01-04 10:06:00
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - javascript
---

JavaScript 诞生于1995年。当时，它的主要目的是处理以前由服务器端语言负责的一些输入验证操作。如今，JavaScript 的用途早已不再局限于简单的表单验证，而是具备了与浏览器窗口及其内容等几乎所有方面交互的能力。JavaScript 已经成为一门功能全面的编程语言，能够处理复杂的计算和交互，拥有了闭包、匿名函数（lamda，拉姆达），甚至元编程等特性。

JavaScript 从一个简单的输入验证器发展成为一门强大的编程语言，完全出乎人们的意料。应该说它既是一门非常简单的语言，又是一门非常复杂的语言。

## JavaScript 简史

Netscape 公司，布兰登·艾奇（Brendan Eich），Navigator 3 / Internet Explorer 3

1997年，以JavaScript 1.1 为蓝本，ECMA（欧洲计算机制造商协会）的 TC39 （39号技术委员会）负责对其标准化，最终发布 ECMA-262。

## JavaScript 实现

一个完整的 JavaScript 实现应该由下列三个不同的部分组成：

*   核心（ECMAScript）
*   文档对象模型（DOM）
*   浏览器对象模型（BOM）

### ECMAScript

由 ECMAScript-262 定义的 ECMAScript 与 Web 浏览器没有依赖关系，后者只是其实现可能的宿主环境之一。其他宿主环境包括 Node 和 Adobe Flash。

ECMAScript 定义了以下内容：语法、类型、语句、关键字、保留字、操作符和对象。

最新版：ECMAScript-262 的第5个版本于2009年12月发布

兼容性：IE 5.5~7 兼容第3版，IE 8+ 兼容第5版，其他主流浏览器一般都兼容第3版。

### DOM

文档对象模型（DOM）是针对 XML 但经过扩展用于 HTML 的应用程序编程接口。DOM 将整个页面映射为一个多层节点结构。

一开始，IE 4 和 Navigator 4 分别支持不同形式的 DHTML ，浏览器互不兼容。此时，负责 Web 通信标准的 W3C （万维网联盟）开始着手规划 DOM。

DOM 级别：DOM 1 级实现了映射文档结构，DOM 2 级扩展了，DOM 3 级进一步扩展了。

浏览器支持：IE 5.5~8 1级，IE9 + 1级、2级、3级，Chrome 1+ 1级、2级，Firefox 1+ 1级、2级、3级。

### BOM

浏览器对象模型支持可以访问和操作浏览器窗口，可以控制浏览器显示的页面以外的部分。BOM 没有相关标准，直到 HTML 5 才将 BOM 正式规范化。例如下面的扩展都属于 BOM ：

*   弹出窗口功能
*   移动、缩放和关闭浏览器窗口的功能
*   提供浏览器详细信息的 navigator 对象
*   提供浏览器所加载页面的详细信息的 location 对象
*   提供用户显示器分辨率详细信息的 screen 对象
*   对 cookie 的支持
*   像 XMLHttpRequest 和 IE 的 ActiveXObject 对象