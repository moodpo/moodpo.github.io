---
layout: post
title: "Window 7 安装 Apache 2.4 和 PHP 5.4 过程"
subtitle: ""
author: "yangchw"
catalog: false
date: 2013-05-28 16:13:00
header-img: "assets/img/home-bg。png"
header-mask: 0.5
tags:
  - win7
  - apache
  - php
---

现在使用UI向导安装AMP(Apache,MySQL和PHP)已经变得非常容易，比如XAMPP，WAMP。但是使用这些集成的包安装你只能获取很少的知识，对于其配置却一无所知，安装完之后也无法按照自己的方式去修改。因此我推荐手动去安装配置它们。

## 一、下载地址

*   Apache 5.4 —— [httpd-2.4.4-win32.zip](http://www.apachelounge.com/download/win32/binaries/httpd-2.4.4-win32.zip)
*   PHP 5.4 ——  [php-5.4.15-Win32-VC9-x86.zip](http://windows.php.net/downloads/releases/php-5.4.15-Win32-VC9-x86.zip)

注意，VC9线程安全版本中已经包含了PHP和Apache connector DLL，因此无需下载此DLL。

## 二、配置

### 1\. Apache

使用任意编辑器打开 apache2.4/conf/httpd.conf 文件开始配置。

#### 1.1 设置 Apache 位置

    ServerRoot &quot;D:/Program Files/apache2.4&quot;
    `</pre>

    #### 1.2 启用使用的模块

    我只去掉了 mod_rewrite 模块的注释。

    #### 1.3 在模块内容下增加以下内容

    <pre>`LoadModule php5_module &quot;D:/Program Files/PHP5.4/php5apache2_4.dll&quot;
    AddHandler application/x-httpd-php .php
    PHPIniDir &quot;D:/Program Files/PHP5.4&quot;
    `</pre>

    #### 1.4 修改服务器管理员邮件地址

    ServerAdmin info@yoursite.com

    #### 1.5 修改文档根目录

    <pre>`DocumentRoot &quot;E:/www&quot;
    &lt;Directory &quot;E:/www&quot;&gt;
    `</pre>

    #### 1.6 找到一下内容替换实际的路径

    <pre>`ScriptAlias /cgi-bin/ &quot;D:/Program Files/apache2.4/cgi-bin/&quot;
    &lt;Directory &quot;D:/Program Files/apache2.4/cgi-bin&quot;&gt;
    `</pre>

    #### 1.7 如果你想启用 `.htaccess` 请修改 `&lt;Directory “D:/www”&gt;` 下内容

    <pre>`AllowOverride All
    `</pre>

    #### 1.8 添加 index.php 到 index 目录中

    <pre>`DirectoryIndex index.html index.php
    `</pre>

    ### 1\. PHP

    #### 1.1 重命名 php.ini-development 为 php.ini

    #### 1.2 修改扩展路径

    <pre>`extension_dir = &quot;D:/Program Files/PHP5.4/ext&quot;
    `</pre>

    #### 1.3 取消以下行的注释

    <pre>`extension=php_curl.dll
    extension=php_mysql.dll
    extension=php_mysqli.dll
    extension=php_pdo_mysql.dll
    extension=php_soap.dll
    `</pre>

    #### 1.4 如果你使用 PHP 的邮件功能请修改下面内容

    <pre>`SMTP = smtp.yoursite.com
    smtp_port = 25
    sendmail_from = youremail@sender.com
    `</pre>

    #### 1.5 最后设置下时区

    <pre>`date.timezone = PRC
    `</pre>

    ## 三、安装

    需要将 Apache 2.4 的服务安装到系统服务中，使用以下命令(需要管理员权限)：

    <pre>`cd D:/Program Files/apache2.4/bin
    httpd -k install

编写一个 index.php 文件，内容为 `&lt;?php phpinfo() ?&gt;`， 启动apache服务，访问以下 [http://localhost/](http://localhost/) 吧。