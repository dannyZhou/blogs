---
layout: post
title: 私有成员变量，setter，getter
date: 2015-08-30 
categories: java
tags: java
---

* content
{:toc}

### 序
> 形象的说一下私有成员变量，setter， 和 getter。以人的年龄和姓名为例。

### 使用的代码
先来看一下这个简单的代码。
{% highlight java %}

// 这是一个 只有一个属性的 person 类
public class person {

    // 这里面存放了一个 年龄
    private int age;
     
    // get 方法
    public int getAge() {
        return age;
    }
}
{% endhighlight %}

### 解释一下 get
首先呢，age 是私有的，这就说明，只有自己知道，别人是不可能知道的。

而，如果别人想知道怎么办？ `getAge` 的存在就是为了解决这个问题。这个 get 方法就是为了给别人一个知道的方法。
别人可以通过 `getAge` 来获取他的年龄。当然这是最正规的方法。

别人想获取年龄的话， 可以这样获取。
{% highlight java %}
Person zhangSan = new Person();
System.out.println(zhangSan.getAge());  // 输出的是真实的年龄
{% endhighlight %}
这样， 就能获取到他的年龄啦。下面有一个不正规的方法。

### 一个不正规的 get
先来看一下这个简单的代码。
{% highlight java %}

// 这是一个 只有一个属性的 person 类
public class Person {

    // 这里面存放了一个 年龄
    private int age;
     
    // get 方法 当别人访问的时候，他都会告诉别人自己 18 岁了。
    public int getAge() {
        return 18;
    }
}
{% endhighlight %}

可能这个例子更能解释 get 的问题。

现在呢，这个人不想告诉别人自己的真实年龄，*对所有的人都说自己是 18 了*。然后，他就这么弄了。

在 age 中放自己的真实年龄，而且这个只是自己一个人知道，别人都不知道。
当别人问到的时候，就告诉他们我 18 岁了。

别人想获取年龄的话， 可以这样获取。
{% highlight java %}
Person zhangSan = new Person();
System.out.println(zhangSan.getAge());  // 输出的是真实的年龄
{% endhighlight %}
这样，输出的永远是 18 

这样我想就能明白 get 了吧。对吧？那个告诉我自己 18 岁的人  :-D

_**@好啦好啦，开个玩笑@**_

### set 方法
可以这么理解：人的名字
{% highlight java %}

// 这是一个 只有一个属性的 person 类
public class Person {

    // 这里面存放了一个 年龄
    private String name;
     
    // get 方法 
    public String getName() {
        return name;
    }
    public void setName(){
        this.name =name;
    }
}
{% endhighlight %}

刚出生的时候，每个人的名字都是别人给的。 这个时候就用到了 set 方法。

当然，get 和 set 可以同时使用的。

{% highlight java %}
// 创建一个人， （造人）
Person zhangSan = new Person();
// 设置他的名字
zhangSan.setName("zhangSan");
// 打印他的名字
System.out.println(zhangSan.getName());
{% endhighlight %}
这样，就能获取到 他的名字了。