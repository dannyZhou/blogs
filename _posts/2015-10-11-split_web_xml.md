---
layout: post
title: web.xml 拆分成多个文件
date: 2015-10-11
categories: java
tags: java tomcat
---

* content
{:toc}

### 序
今天发现一个问题，纯正 servlet 项目，将会使得 web.xml 变得很大。所以想办法将其进行拆分。

### 参考资料
[关于web.xml中引用其它xml片段，然后运行在tomcat7.0.62上出现的问题](http://blog.csdn.net/elong490/article/details/46379725)

### 第一步
在 web.xml 中添加 如下 代码第一行，还有内容
{% highlight xml %}
<!DOCTYPE web-app
        [<!ENTITY test SYSTEM  "test.xml">]>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
&test;
</web-app>
{% endhighlight %}

### 第二步 
建立 test.xml （注意：这里没有根元素） 
{% highlight xml %}
<servlet>
    <servlet-name>orderList</servlet-name>
    <servlet-class>com.hp.shopping.servlet.admin.order.OrderListServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>orderList</servlet-name>
    <url-pattern>/b/order/list</url-pattern>
</servlet-mapping>
{% endhighlight %}

### 报错了
java.io.FileNotFoundException: Could not resolve XML resource [null] with public ID [null], system ID [rua.xml] and base URI [jndi:/localhost/oms/WEB-INF/web.xml] to a known, local entity.

### 解决办法
tomcat 7.0.52开始的版本才会出这个问题，是因为安全的考虑tomcat 7.0.52开始的版本把xmlBlockExterna属性默认为true，要解决这个问题，两种方法：1、把tomcat版本换成7.0.52之前的版本。2、把xmlBlockExterna设成false。
 
下面是原版解释：
 
As per discussion with Tomcat developers, xmlBlockExternal=”true” attribute of Tomcat’s Context (context.xml) was set true by default starting from 7.0.52. With xmlBlockExternal=”false”generated/djn-settings.conf can be included】
 
- 修改 tomcat 目录下 conf/context.xml

{% highlight xml %}
    <Context  xmlBlockExternal="false">
{% endhighlight %}