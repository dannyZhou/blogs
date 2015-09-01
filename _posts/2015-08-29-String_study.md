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

### 参考资料
java jdk src.zip 源代码

### String 的内部存储
String 源代码上来就是这么一行, 这一行直接表示了它的底层实现。
{% highlight java %}
    private final char value[];
{% endhighlight %}

### String 构造方法
1. 首先, 空的构造方法就下我一跳
{% highlight java %}
   public String() {
        this.value = "".value;
    }
{% endhighlight %}
这说明，`"" != new String()` 的返回值是 true。

2. String 的一个 default 构造方法。先上代码：
{% highlight java %} 
    String(char[] value, boolean share) {
        // assert share : "unshared not supported";
        this.value = value;
    }
{% endhighlight %}
这一个的注释是这么写的：

` Package private constructor which shares value array for speed.
 this constructor is always expected to be called with share==true.
 a separate constructor is needed because we already have a public
 String(char[]) constructor that makes a copy of the given char[]. `
 
翻译过来以后（My English is poor!）： 

``

2. String 其他的构造方法都不是太难，仔细读的话，应该都能理解。

### 构造方法引出的一个问题
构造方法里面有这么一行 `"".value`；参见 私有属性访问。
（这里怎么加一个自己到 md 的链接？）
 