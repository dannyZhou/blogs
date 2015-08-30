---
layout: post
title: "TCP/IP 协议学习"
date: 2015-08-29
categories: network
tags: tcp ip http network telnet
---

### 序
> 计算机网络是大学计算机学科中的一门，而对于 java 程序员来讲，
> 我们只需要接触五层协议中网络层以上的几层就好：网络层、传输层、应用层
>  主要学习： TCP/IP 、 HTTP 等。

### 参考资料
[HTTP权威指南](http://book.douban.com/subject/10746113/)

[TCP/IP协议族(第2版)](http://book.douban.com/subject/1141215/) 
[(现在已经存在第四版)](http://book.douban.com/subject/5386194/)

### HTTP
http 特点： 不是二进制，而是直接的文本格式。
用 java 中的知识来讲，用最简单的 `socket` 发送文本就能实现一个简单的浏览器。
当然，系统中的 `telnet` 也能做到。

#### `telnet` 获取百度页面的代码
{% highlight bash shell scripts %}
telnet baidu.com 80 # 连接 baidu.com 80 端口
Trying 220.181.57.217...
Connected to baidu.com.
Escape character is '^]'.

# 这是我输入的信息， 只是简单的获取 /， 注意： Host 为空。 
    # 不加入 Host 将会返回 400 Bed Request （这个和 http1.0 不同） 
    # 可以将 1.1 更改为 1.0 查看效果
GET / HTTP/1.1
Host:

    # 注意： 这里 应该是 连续敲入两个回车
    # 下面的 都是返回信息 请自己尝试 详见 ---参考资料---

{% endhighlight %}

### HTTP 1.1 和 HTTP 1.0 
HTTP 1.1 是 HTTP 1.0 的升级版。
一个很明显的区别就是，HTTP 1.0 不需要 Host: 信息发送。 

### ATM 
网络中的 ATM 并不是 ATM 机，而是 [异步传输模式（ATM Asynchronous Transfer Mode）](http://baike.baidu.com/subview/26/5395796.htm)