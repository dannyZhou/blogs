---
layout: post
title: 常见的编码格式 UTF-8 ISO-8859-1
date: 2015-08-30
categories: network
tags: http unicode ascii utf-8 utf-16 utf-32 iso-8859-1
---

### 序
> java 中，包括 firefox 等浏览器中，编码问题可能是经常遇到的一个问题了。
> 今天我们就简单的分析一下常见的格式。如果可能的话，说一些解决办法。

### 参考资料
[Unicode 百度百科](http://baike.baidu.com/view/40801.htm)

[ASCII 百度百科](http://baike.baidu.com/view/15482.htm)

### Unicode
Unicode 是一种在计算机上使用的字符编码。他为每一种语言的每个字符都设定了统一并且唯一的二进制编码。

Unicode为了和拉丁字母相互兼容，其首256字符保留给ISO 8859-1所定义的字符，使既有的西欧语系文字的转换不需特别考量。

Unicode是可以容纳世界上所有文字和符号的字符编码方案。Unicode用数字0-0x10FFFF来映射这些字符，最多可以容纳1114112个字符，或者说有1114112个码位。码位就是可以分配给字符的数字。

UTF-8、UTF-16、UTF-32都是将数字转换到程序数据的编码方案。

### UTF-8 UTF-16 UTF-32
UTF是“UCS Transformation Format”的缩写，可以翻译成Unicode字符集转换格式，即怎样将Unicode定义的数字转换成程序数据。*注：通用字符集（Universal Character Set, UCS）*

例子：将 `汉字` 两个字进行编码
{% highlight C %}
chardata_utf8[]={0xE6,0xB1,0x89,0xE5,0xAD,0x97};//UTF-8编码
char16_tdata_utf16[]={0x6C49,0x5B57};//UTF-16编码
char32_tdata_utf32[]={0x00006C49,0x00005B57};//UTF-32编码
{% endhighlight %}

### ASCII
ASCII（American Standard Code for Information Interchange，美国标准信息交换代码）是基于拉丁字母的一套电脑编码系统，主要用于显示现代英语和其他西欧语言。

标准ASCII码：也叫基础ASCII码，使用7 位二进制数来表示所有的大写和小写字母，数字0 到9、标点符号， 以及在美式英语中使用的特殊控制字符。

扩展ASCII码：后128个称为扩展ASCII码。许多基于x86的系统都支持使用扩展（或“高”）ASCII。扩展ASCII 码允许将每个字符的第8 位用于确定附加的128 个特殊符号字符、外来语字母和图形符号。

![ASCII 标志图](http://www.asciitable.com/index/asciifull.gif)

### ISO-8859-1
ISO-8859-1编码是单字节编码，向下兼容ASCII，其编码范围是0x00-0xFF，0x00-0x7F之间完全和ASCII一致，0x80-0x9F之间是控制字符，0xA0-0xFF之间是文字符号。

![ISO-8859-1 标志图]({{ site.baseurl }}/assets/iso-8859-1.png)
