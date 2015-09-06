---
layout: post
title: JAVA String 类学习遇到的问题以及源码解析
date: 2015-08-29
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

`包级别权限公用一个数组为了速度。这个构造函数一直让 share = true，一个单独的构造方法是必要的，
因为我们已经有了一个构造方法是用来拷贝一个char[]`

2. String 其他的构造方法都不是太难，仔细读的话，应该都能理解。

### 构造方法引出的一个问题
构造方法里面有这么一行 `"".value`；参见 [私有属性访问]({% post_url 2015-09-01-private_field %})

### String 中一种 for 循环的使用与平常使用的区别
先看测试代码， 看看这两个 for 循环有什么区别， 然后看看他们的返回值的不同
{% highlight java %} 
    public void testFor() {

        for (int i=0;i++<5;) {
            System.out.println(i); // 这个 for 循环输出是 1 2 3 4 5
        }
        System.out.println();
        for (int i=0;i<5;i++) {
            System.out.println(i);  // 这个 for 循环输出是 0 1 2 3 4
        }
    }
{% endhighlight %}
 
### String 中 `charAt()` 和 `codePointAt()` 方法
先看 `charAt()` 的代码
{% highlight java %} 
    public char charAt(int index) {
        // 删除了边界检查， 只剩下关键代码
        return value[index];
    }
{% endhighlight %}
`charAt()` 的实现相当简单，直接返回数组对应位置

再来看 `codePointAt()`，返回 int 类型，在 index 处字符的代码点值 返回的应该是所在 unicode 的值
{% highlight java %}
    public int codePointAt(int index) {
        // 删除边界检查， 保留核心代码
        return Character.codePointAtImpl(value, index, value.length);
    }
{% endhighlight %}
