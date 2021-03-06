---
title:  一句话木马上传常见的几种方法
date: 2016-01-09 23:55:45
categories: 信息安全
tags:
- webshell
- 信息安全
- 黑客
- 入侵
---

**1，利用00截断，brupsuite上传**

 利用00截断就是利用程序员在写程序时对文件的上传路径过滤不严格，产生0X00上传截断漏洞。

假设文件的上传路径为http://xx.xx.xx.xx/upfiles/lubr.php.jpg ,通过抓包截断将lubr.php后面的“.”换成“0X00”。在上传的时候，当文件系统读到”0X00″时，会认为文件已经结束，从而将lubr.php.jpg 的内容写到lubr.php中，从而达到攻击的目的。

**2，构造服务器端扩展名检测上传**

当浏览器将文件提交到服务器端的时候，服务器端会根据设定的黑名单对浏览器提交上来的文件扩展名进行检测，如果上传的文件扩展名不符合黑名单的限制，则不予上传，否则上传成功。

<!--MORE-->
本例讲解，将一句话木马的文件名lubr.php改成lubr.php.abc。首先，服务器验证文件扩展名的时候，验证的是.abc，只要改扩展名符合服务器端黑名单规则，即可上传。另外，当在浏览器端访问该文件时，Apache如果解析不了.abc扩展名，会向前寻找可解析的扩展名，即”.php”。一句话木马可以被解析，即可通过中国菜刀连接。

**3，绕过Content-Type检测文件类型上传**

 当浏览器在上传文件到服务器端的时候，服务器对上传的文件Content-Type类型进行检测，如果是白名单允许的，则可以正常上传，否则上传失效。绕过Content-Type文件类型检测，就是用Burpsuite截取并修改数据包中文件的Content-Type类型，使其符合白名单的规则，达到上传的目的。

**4，构造图片木马，绕过文件内容检测上传Shell**

一般文件内容验证使用getimeagesize()函数检测，会判断文件是否一个有效的文件图片，如果是，则允许上传，否则的话不允许上传。

制作图片木马： copy 1.jpg/b+2.php/a 3.jpg