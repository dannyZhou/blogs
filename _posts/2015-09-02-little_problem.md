---
layout: post
title: java 小错误汇总（持续更新）
date: 2015-09-02 
categories: java
tags: java
---

* content
{:toc}

### 序
> 今天在写作业得时候，在一个 static 的方法中无意敲出了 `this.` 结果 IDE 直接毫不留情的把我拒绝了。
> 然而，就这样一个小的错误不值得占用一个博客，所以，开设一个小错误汇总。

### static 方法中不能有 this
this 是指代这个对象，而静态方法面向的是类，所以， static 中不能使用 this 啦。

### Switch 中的代码是一个代码块
{% highlight java %}
    switch( number ) {
        case 1:
            String tmpString = new String();
            break;
        case 2:
            String tmpString = new String();
{% endhighlight %}
注意: 这里 连续 定义了两次 tmpString 但是这两个是在一个代码块中的，所以 IDE 报错。
而如果将第二个前面 的 String 删了， 可以使用。
  
### interface 接口的一个小的注意事项
{% highlight java %} 
class CA {
}
class CB extends CA{
}
interface IA {
    CA returnCA(); 
}

class ImplIA implements  IA{

    @Override
    public CB returnCA() {  // 这里返回的 不是 CA 而是 CB
        return new CB();
    } 
}
{% endhighlight %}

这里说明，interface 方法中的返回值 在编写实现类的时候， 可以更改为 他的子类。但是不能是其父类。 