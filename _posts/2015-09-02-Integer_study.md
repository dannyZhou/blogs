---
layout: post
title: JAVA Integer 类学习遇到的问题以及源码解析
date: 2015-09-02
categories: java
tags: java
---

* content
{:toc}

### 序
> 前一段时间励志要写 src.zip，但是上来的一个 String.java 就把我弄晕了。代码写了一半，想换换口味，弄弄
> Integer.java 吧。

<A name="seeAlso"></A>
### 参考资料
java jdk src.zip 源代码

[why-is-the-size-constant-only-native-for-integer-and-long](http://stackoverflow.com/questions/28770822/why-is-the-size-constant-only-native-for-integer-and-long) 

### Integer 的最大值和最小值
我们知道，int 在 java 中存放是 32 位，所以 java 中， int 的范围应该是 -2^31 - 2^31-1 
所以， 就有了这两行代码
{% highlight java %}
    @Native public static final int   MIN_VALUE = 0x80000000;
    @Native public static final int   MAX_VALUE = 0x7fffffff;
{% endhighlight %}
采用 16 进制，表示的 -2^31 和 2^31-1。至于为什么会加上 `@Native` 请参考 [参见资料](#seeAlso) (反正我不懂....) 

### 由 Integer.java 引出来的一个问题
{% highlight java %}
Class clazz = int.class
{% endhighlight %}

### toString 问题
在代码中，采用了 `new String()`，所以，
`Integer.toString(3) == Integer.toString(3)` 返回的是 _*false*_

但是！！`(Integer.MIN_VALUE)== Integer.toString(Integer.MIN_VALUE))` 返回 true。 具体参见代码实现。

### 奇葩数学公式
看的时候看到这样一行代码: 
{% highlight java %} 
        // really: r = i - (q * 100);
            r = i - ((q << 6) + (q << 5) + (q << 2));
{% endhighlight %}
进行实验，果然如此！

### parseInt 等 parse 系列空格
看源代码可以发现，`Long.parseLong(" 3 ")` 将会抛出异常，所以，保险的办法是手动将前后空格去掉。

### int Integer 重载
{% highlight java %} 
    public static void main(String[] args) {

        intTest(3);
        
        int a = 3;
        intTest(a);

        Integer b = 3;
        intTest(b);
        
        intTest(null);
    }

    public static void intTest(int test) {
        System.out.println(" int");
    }

    public static void intTest(Integer test) {
        System.out.println("integer");
    }
{% endhighlight %}
输出结果是: `int int Integer Integer` 请对应以下吧。
*注意*一点是：null 会调用 integer。

当然， 这里都是优先选择，如果没有，将会尝试着进行拆包或者装包，再进行匹配。