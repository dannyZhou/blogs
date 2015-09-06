---
layout: post
title: java 内部类
date: 2015-09-06
categories: java
tags: java
---

* content
{:toc}

### 序
最近一个 `Outter.this.field` 我没有看懂，所以写下这篇文章

### 非静态内部类
{% highlight java %}
class A {
    String prop = "outter";
    class B {
        String prop = "inner";
        void test() {
            String prop = "innerMethod";
            
            System.out.println(prop); // innerMethod
            System.out.println(this.prop); // innder 
            System.out.println(B.this.prop); // innder 
            System.out.println(A.this.prop); // outter
        }
    }
}
{% endhighlight %}
在这里需要注意一个问题就是，`test()` 方法中，`this.prop` 和 `B.this.prop` 的输出结果是一样的。

### 非静态内部类方法访问变量的过程
先查找方法中有没有对应的，再查找方法所在的内部类有没有对应的，再查找内部类所在的外部类有没有，如果不存在，就报错。

跨过顺序直接访问的方式见上面。

### 更难的非静态内部类
{% highlight java %}

public class A {
    String prop = "A.prop";
    class B {
        String prop = "B.prop";
        class C {
            String prop = "C.prop";
            void test() {
                String prop = "prop";
                System.out.println(prop); // innerMethod
                System.out.println(this.prop); // innder
                System.out.println(B.this.prop); // outter
                System.out.println(A.this.prop);
            }
        }
        public void test() {
            C c = new C();
            c.test();
        }
    }

    public static void main(String[] args) {
        A clock = new A();
        clock.test();
    }
    public void test() {
        B b = new B();
        b.test();
        b.prop = "";  // 这句话是能够使用的 
    }
}
{% endhighlight %}
自己看代码吧。

### 非静态内部类 注意事项
非静态内部类里面不能有静态方法，静态属性，静态初始化块。

### 静态内部类
静态内部类不能访问外部类的实例成员，只能访问外部类的类成员。

外部类不能直接访问内部类属性，但是可以使用静态内部类类名.field 作为调用方式。也可以静态内部类的实例调用实例成员。

### interface
接口中可以定义内部类，默认使用 public static 修饰。

接口中可以定义内部接口，但是很少这么用。

### 外部类创建非静态内部类
{% highlight java %}
 public class Outter {
     public class Inner {
         String name = "inner";
     }
 }
{% endhighlight %}
创建方式是：
{% highlight java %}
    Outter outter = new Outter();
    Outter.Inner inner = outter.new Inner();
{% endhighlight %}

### 外部类创建静态内部类
{% highlight java %}
 public class Outter {
     public static class Inner {
         String name = "inner";
     }
 }
{% endhighlight %}
创建方式是：
{% highlight java %}       
    Outter.Inner inner = new Outter.Inner();                 
{% endhighlight %}

### 匿名内部类
匿名内部类如果想访问外部类的局部变量，局部变量必须使用 final 修饰。(注：1.7必须使用，但是1.8 没有这个限制)
{% highlight java %}       
interface A {
    void test();
}
public class Main { 
    public static void main(String[] args) {
        final int aString = 1;   // 在1.7必须使用 final 修饰符，但是1.8 没有这个限制
        A a = new A() {
            @Override
            public void test() {
                System.out.println(aString); 
            }
        };
        a.test();
    }
}
{% endhighlight %}
