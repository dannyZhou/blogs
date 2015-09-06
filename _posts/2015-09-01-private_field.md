---
layout: post
title: 私有变量的一个使用问题
date: 2015-09-01
categories: java
tags: java
---

* content
{:toc}

### 序
> 最近打算看一下 jdk 的源代码，先拿 `String.java` 文件开刀。结果上来就懵了。

### 参考资料
java jdk src.zip 源代码

### 参考源码
> 这个代码是复原代码，并不是 `String.java` 的代码。 详见 [参考资料](#参考资料)
{% highlight java %}
public class PrivateFields {

    // 设置一个 age
    private int age;

    public static void main(String[] args) {

        // 实例化一个对象
        PrivateFields fields = new PrivateFields();
        // 直接设置
        fields.age = 10;
    }
}
{% endhighlight %}

### 注意问题
private 的字段，在自己的 class 内部方法中可以直接调用，即使是创建了一个新的对象，调用对象中的字段。