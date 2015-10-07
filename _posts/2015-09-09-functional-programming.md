---
layout: post
title: 函数式编程 以及 lambda 初级
date: 2015-09-09
categories: java
tags: java
---

* content
{:toc}

### 序
> Java 8 中有一个新的特性是支持了 lambda 表达式，而我在翻译 getting-started-with-google-guava.pdf 的第三章也开始讲
函数式编程，所以，还是先学习函数式编程为好。

### 参考资料
[函数式编程 百度百科](http://baike.baidu.com/view/1711147.htm)



### 函数式编程， lambda 表达式
函数编程语言最重要的基础是 λ 演算（lambda calculus）。而且λ演算的函数可以接受函数当作输入（参数）和输出（返回值）。

上面这句话同时表明了 函数式编程 和 λ 演算的关系。

“Lambda 表达式”(lambda expression)是一个匿名函数，Lambda表达式基于数学中的 λ 演算 得名，直接对应于其中的lambda抽象
(lambda abstraction)，是一个匿名函数，即没有函数名的函数。Lambda表达式可以表示闭包（注意和数学传统意义上的不同）。

上面这句话同时表明了 “Lambda 表达式” 和 λ 演算 的关系。

### 函数式接口
函数式接口是 java 8 对一类特殊类型的接口的称呼。这类接口只定义了唯一抽象方法的接口。（除了隐含的 Object 对象的公共方法）
因此最开始也叫做 SAM 类型的接口。（Single Abstract Method）

函数式接口一般用 @FunctionalInterface 标注出来，（也可以不标注）

JDK 中已有的一些接口本身就是函数式接口，如 `Runnable`。JDK 8 中又添加了 `java.util.function` 包，提供了常用的函数式接口。

### lambda 在 java 8 中的实现
任意只包含一个抽象方法的接口，我们都可以用来做成lambda表达式。为了让你定义的接口满足要求，你应当在接口前加上@FunctionalInterface 
标注。编译器会注意到这个标注，如果你的接口中定义了第二个抽象方法的话，编译器会抛出异常。
{% highlight java %}
public class Main {
    public static void main(String[] args) {
        FunctionInterface functionInterface = (int x, int y) -> {
            return x + y;
        };
        System.out.println(functionInterface.add(3, 4));
    }
}
// 创建一个 函数式接口 这个接口中只有一个 方法， 那就是 `add` 方法 
@FunctionalInterface
interface FunctionInterface {
    public int add(int x, int y);
}
{% endhighlight %}
这里可以将代码进行进一步简化
{% highlight java %}
        FunctionInterface functionInterface = (x, y) -> {
            return x + y;
        };
{% endhighlight %}
当然， 可以进行进一步简化
{% highlight java %}
        FunctionInterface functionInterface = (x, y) -> x + y;
 {% endhighlight %}
我不知道还有没有更简单的形式。。。 暂时先这样吧。 

由此可见呢？
lambda 表达式 有 三部分 组成。 参数列表， 箭头 ->  以及一个表达式，或者语句块

下面这个 是一个 没有 参数，没有返回值的 lambda 表达式
{% highlight java %}
() -> { System.out.println(" hello , lambda ") } 
{% endhighlight %}

当然，如果说， 只有一个参数并且 java 可以推断出其类型，那么参数列表括号也能省略
{% highlight java %}
c -> { return c.size(); }
{% endhighlight %}

### lambda java 一个小小的深入
函数式接口中可以额外定义多个抽象方法，但这些抽象的方法必须和 Object 的 public 方法一样。
接口最终有确定的类实现，而类的最终父类是 Object，因此函数式接口可以定义 Object 的 public 方法。
{% highlight java %}
@FunctionalInterface
public interface ObjectMethodFunctionalInterface {
    void count (int i);
    String toString();
    int hashCode();
    boolean equals(Object obj);
{% endhighlight %}

注意这里 不能添加 `clone` 方法，因为 `Object` 中的 `clone` 方法是 protected 类型。

接口可以包含一个或者多个 default 方法 和 静态方法。这是 java 8 的另一个新特性，这个将会在后面的博客讨论。（××××××××）