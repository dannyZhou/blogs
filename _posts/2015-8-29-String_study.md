---
layout: post
title: JAVA String 类学习遇到的问题以及源码解析
date: 2015-08-30 
categories: java
tags: java
---

* content
{:toc}

### 序
> 今天开始自己抄写 java 中 src.zip 中的 String.java 。不抄不要紧，一吵吓一跳。很多的问题现在发现不是这样的.......

### String 的内部存储
String 源代码上来就是这么一行, 这一行直接表示了它的底层实现。
{% highlight java %}
    private final char value[];
{% endhighlight %}

### String 构造函数
1. 首先, 空的构造函数就下我一跳
{% highlight java %}
   public String() {
        this.value = "".value;
    }
{% endhighlight %}
这说明，`"" != new String()` 的返回值是 true。

2. String 其他的构造函数都不是太难，仔细读的话，应该都能理解。

###