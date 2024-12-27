---
layout: post
title: "使用 JSDoc 3 自动生成 JavaScript API 文档"
subtitle: ""
author: "yangchw"
catalog: false
date: 2014-01-28 22:14:00
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - javascript
---

> 软件并不只是包括可以在计算机上运行的电脑程序，与这些电脑程序相关的文档，一般也被认为是软件的一部分。简单的说软件就是程序加文档的集合体。

随着项目的升级进化，前端产生的 JavaScript 代码越来越多越复杂；同时，也有越来越多的 JavaScript 问题出现，其中 JavaScript 文档的编写和维护成为一个亟待解决的难题。

许多现代编程语言都有自己的集成化文档生成工具，像 Java 有 JavaDoc ，.NET 有 NDoc，PHP 有 PHPDoc，这些自动化文档工具可以根据代码中的注释自动生成代码文档。本文将介绍的 JSDoc 也是这样的工具。

## 简介

JSDoc 3 是 [JsDoc Toolkit](https://code.google.com/p/jsdoc-toolkit/) 的升级版本，JsDoc Toolkit 是一个自动化文档工具，它是发布在 Google code 上的一个开源项目，和其他语言的文档工具一样，它可以自动从 JavaScript 代码中提取注释生成格式化文档。从 2010年7月27日 开始，JsDoc Toolkit 2 已经停止开发了，因此如果要使用 JSDoc 的话，还是推荐采用 JSDoc 3。现在 JSDoc 3 代码托管在 GitHub 上，项目地址：[https://github.com/jsdoc3/jsdoc](https://github.com/jsdoc3/jsdoc)

## 安装

JSDoc 3 在本地安装依赖于 Nodejs，因此首先需要下载 Nodejs 进行安装：

    http://nodejs.org/
    http://nodejs.org/dist/v0.10.25/x64/node-v0.10.25-x64.msi
    `</pre>

    安装完后，使用以下命令安装 JSDoc 3 即可（**注**：必须使用超级管理员身份安装）：

    <pre>`# stable
    npm install jsdoc@&quot;&lt;=3.3.0&quot; -g
    # dev
    npm install git+https://github.com/jsdoc3/jsdoc.git -g

    # test
    C:\Users\moodpo&gt;jsdoc --help
    JSDoc 3.3.0-alpha3 (Wed, 25 Dec 2013 17:07:35 GMT)
    ......
    `</pre>

    ## 使用

    JSDoc 3 生成 API 文档完全依赖于你的代码注释，因此了解 JSDoc 3 的注释规则是最重要的。首先，注释必须要以 `/**` 开头，如果使用 `/*` 将不被识别；其次，每个注释块都应该包含唯一的主标签，和零个或多个副标签。

    ### 1\. JSDoc 3 标签规范

    <table class="table table-bordered table-striped table-condensed">
      <tbody>
      <tr>
        <td>标签</td>
        <td>描述</td>
      </tr>
      <tr>
        <td>@abstract/@virtual</td>
        <td>This member must be implemented (or overridden) by the inheritor.</td>
      </tr>
      <tr>
        <td>@access</td>
        <td>Specify the access level of this member - private, public, or protected.</td>
      </tr>
      <tr>
        <td>@alias</td>
        <td>Treat a member as if it had a different name.</td>
      </tr>
      <tr>
        <td>@augments/@extends</td>
        <td>标明一个函数继承另一个函数，如 A 继承 B 则可以在 A 的注释中加 `@augments B`</td>
      </tr>
      <tr>
        <td>@author</td>
        <td>Identify the author of an item.</td>
      </tr>
      <tr>
        <td>@borrows</td>
        <td>参考，如 A 和 B 两个函数意义相同，则可以在 B 的注释中加 `@borrows A as B`，而不需重复添加注释</td>
      </tr>
      <tr>
        <td>@callback/@typedef</td>
        <td>标明此方法是一个回调函数</td>
      </tr>
      <tr>
        <td>@classdesc</td>
        <td>对一个类的描述</td>
      </tr>
      <tr>
        <td>@constant/@const</td>
        <td>常量标识</td>
      </tr>
      <tr>
        <td>@constructor/@class</td>
        <td>标明是构造器函数，可使用 `new` 关键字实例化</td>
      </tr>
      <tr>
        <td>@constructs</td>
        <td>当使用对象字面量形式定义类时，可使用此标签标明其构造函数</td>
      </tr>
      <tr>
        <td>@copyright</td>
        <td>描述版权信息</td>
      </tr>
      <tr>
        <td>@default</td>
        <td>默认值</td>
      </tr>
      <tr>
        <td>@deprecated</td>
        <td>弃用</td>
      </tr>
      <tr>
        <td>@desc/@description</td>
        <td>如果在注释开始描述可省略此标签</td>
      </tr>
      <tr>
        <td>@enum</td>
        <td>一个类中属性的类型相同时，使用此标签标明</td>
      </tr>
      <tr>
        <td>@event</td>
        <td>标明一个可触发的事件函数，一个典型的事件是由对象定义的一组属性来表示。</td>
      </tr>
      <tr>
        <td>@example</td>
        <td>示例，代码可自动高亮</td>
      </tr>
      <tr>
        <td>@exports</td>
        <td>标识此对象将会被导出到外部调用</td>
      </tr>
      <tr>
        <td>@external/@host</td>
        <td>标识此类、命名空间或模块来自外部</td>
      </tr>
      <tr>
        <td>@file</td>
        <td>描述文件功能</td>
      </tr>
      <tr>
        <td>@fires/@emits</td>
        <td>标明当一个方法被调用时将出发一个特殊类型的事件</td>
      </tr>
      <tr>
        <td>@global</td>
        <td>全局变量标识</td>
      </tr>
      <tr>
        <td>@ignore</td>
        <td>忽略此注释块</td>
      </tr>
      <tr>
        <td>@inner</td>
        <td>标明此代码是父类的内部变量</td>
      </tr>
      <tr>
        <td>@instance</td>
        <td>标明此代码是父类的实例</td>
      </tr>
      <tr>
        <td>@kind</td>
        <td>标明代码在其文档中的类型，如Class、Modual等</td>
      </tr>
      <tr>
        <td>@license</td>
        <td>采用的开源协议版本</td>
      </tr>
      <tr>
        <td>@link</td>
        <td>内联标签，创建一个链接，如 `{@link http://github.com Github}` </td>
      </tr>
      <tr>
        <td>@member/@var</td>
        <td>记录一个基本数据类型的成员变量</td>
      </tr>
      <tr>
        <td>@memberof</td>
        <td>指定一个变量所属的父类</td>
      </tr>
      <tr>
        <td>@method/@function/@func</td>
        <td>标记一个方法或函数</td>
      </tr>
      <tr>
        <td>@mixes</td>
        <td>标记一个对象混合了另一个对象的所有成员</td>
      </tr>
      <tr>
        <td>@mixin</td>
        <td>一个 `mixin` 提供了旨在将方法添加到对象上的功能</td>
      </tr>
      <tr>
        <td>@module</td>
        <td>标明当前文件模块，在这个文件中的所有成员将被默认为属于此模块，除非另外标明</td>
      </tr>
      <tr>
        <td>@name</td>
        <td>指定一段代码的名称，强制 JSDoc 使用此名称，而不是代码里的名称</td>
      </tr>
      <tr>
        <td>@namespace</td>
        <td>指定一个变量为命名空间变量</td>
      </tr>
      <tr>
        <td>@param</td>
        <td>标记方法参数及参数类型</td>
      </tr>
      <tr>
        <td>@private/@protected/@public</td>
        <td>标明变量访问等级</td>
      </tr>
      <tr>
        <td>@property</td>
        <td>标明一个对象的属性</td>
      </tr>
      <tr>
        <td>@readonly</td>
        <td>只读</td>
      </tr>
      <tr>
        <td>@requires</td>
        <td>标明运行代码所需模块</td>
      </tr>
      <tr>
        <td>@returns/@return</td>
        <td>标明返回值、类型及描述</td>
      </tr>
      <tr>
        <td>@see</td>
        <td>链接到一个参考位置</td>
      </tr>
      <tr>
        <td>@since</td>
        <td>描述此功能来自哪一版本</td>
      </tr>
      <tr>
        <td>@static</td>
        <td>描述一个不需实例即可使用的变量</td>
      </tr>
      <tr>
        <td>@summary</td>
        <td>对描述信息的短的概述</td>
      </tr>
      <tr>
        <td>@this</td>
        <td>说明 `this` 关键字所代表的意义</td>
      </tr>
      <tr>
        <td>@throws</td>
        <td>描述方法将会出现的错误和异常</td>
      </tr>
      <tr>
        <td>@todo</td>
        <td>描述函数的功能或任务</td>
      </tr>
      <tr>
        <td>@tutorial</td>
        <td>插入一个指向向导教程的链接</td>
      </tr>
      <tr>
        <td>@type</td>
        <td>描述代码变量的类型</td>
      </tr>
      <tr>
        <td>@version</td>
        <td>版本信息</td>
      </tr>
    </tbody></table>

    更多内容请查看[官方文档](http://usejsdoc.org/#JSDoc3_Tag_Dictionary)。

    ### 2\. 使用 conf.json 文件

    安装完 JSDoc 3 后默认使用的配置文件为 `conf.json.EXAMPLE`，内容如下：

    <pre>`{
        &quot;tags&quot;: {
            &quot;allowUnknownTags&quot;: true
        },
        &quot;source&quot;: {
            &quot;includePattern&quot;: &quot;.+\\.js(doc)?$&quot;,
            &quot;excludePattern&quot;: &quot;(^|\\/|\\\\)_&quot;
        },
        &quot;plugins&quot;: [],
        &quot;templates&quot;: {
            &quot;cleverLinks&quot;: false,
            &quot;monospaceLinks&quot;: false
        }
    }
    `</pre>

    我们可以定义自己的 `conf.json` 文件，使用此文件可以指定 JSDoc 输出目录、命令行操作参数、使用的插件、输出模版以及读取的文件等，以下是完整的文件内容：

    <pre>`// 使用时请删除所有注释
    {
        &quot;tags&quot;: {
            &quot;allowUnknownTags&quot;: true //允许输出位置标签
        },
        &quot;source&quot;: { // 指定读取的源文件
            &quot;include&quot;: [], // 包含文件列表
            &quot;exclude&quot;: [], // 不包含文件列表
            &quot;includePattern&quot;: &quot;.+\\.js(doc)?$&quot;, // 正则方式
            &quot;excludePattern&quot;: &quot;(^|\\/|\\\\)_&quot;
        },
        &quot;plugins&quot;: [], // 启用插件列表，可在 JSDoc 目录下找到所有的插件
        &quot;templates&quot;: {
            &quot;cleverLinks&quot;: false,
            &quot;monospaceLinks&quot;: false
        },
        &quot;opts&quot;: { // 命令行操作参数配置，详细内容请查看 http://usejsdoc.org/about-commandline.html
            &quot;template&quot;: &quot;templates/default&quot;,  // same as -t templates/default
            &quot;encoding&quot;: &quot;utf8&quot;,               // same as -e utf8
            &quot;destination&quot;: &quot;./out/&quot;,          // same as -d ./out/
            &quot;recurse&quot;: true,                  // same as -r
            &quot;tutorials&quot;: &quot;path/to/tutorials&quot;, // same as -u path/to/tutorials, default &quot;&quot; (no tutorials)
            &quot;query&quot;: &quot;value&quot;,                 // same as -q value, default &quot;&quot; (no query)
            &quot;private&quot;: true,                  // same as -p
            &quot;lenient&quot;: true,                  // same as -l
            // these can also be included, though you probably wouldn't bother
            // putting these in conf.json rather than the command line as they cause
            // JSDoc not to produce documentation. 
            &quot;version&quot;: true,                  // same as --version or -v
            &quot;explain&quot;: true,                  // same as -X
            &quot;test&quot;: true,                     // same as -T
            &quot;help&quot;: true,                     // same as --help or -h
            &quot;verbose&quot;: true,                  // same as --verbose, only relevant to tests.
            &quot;match&quot;: &quot;value&quot;,                 // same as --match value, only relevant to tests.
            &quot;nocolor&quot;: true                   // same as --nocolor, only relevant to tests
        }
    }
    