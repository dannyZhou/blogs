---
layout: post
title: java 反射 在 web 项目中分发路径的简单使用
date: 2015-09-21
categories: java
tags: java
---

* content
{:toc}

### 序
java 反射可以将很多死的东西变成活的。在做 web 项目的过程中，我一直在想能不能通过 url 来定位 servlet 和 class ，
这个可能是受到了 rails 那个零配置的思想。下面是一个简单的实现。

### 想实现的效果
/b/admin/list 访问到的是 AdminAction 中的 list 方法。

### 路径分发
这里将所有的 try 全部删除了。
{% highlight java %}

public class PathDispatcher extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    
        
        // 找出访问路径
        String path = request.getRequestURI();

        // 根据路径 访问对应的 class
        // 分割 /
        String[] pathCollections = path.split("/");

        // pathCollections[0] is empty
        // pathCollections[1] see web.xml
        int start = 2;

        // 加载 class
        Class<?> action = null;
        String className = null;
        
        // 获取 想调用的类名
        String simpleName = pathCollections[start];
        // 将第一个 字母更改为 大写
        simpleName = simpleName.substring(0, 1).toUpperCase() + simpleName.substring(1);
        // 构造 className
        className = "com.danny.serv.action." + simpleName + "Action";
        // 获取 class
        action = Class.forName(className);
            
        // 获取 action 的 单实例
        Object instance = null;
    
        instance = action.getMethod("getInstance").invoke(action);
        
        // 方法调用
        Method method = action.getMethod(pathCollections[start + 1], new Class[]{HttpServletRequest.class, HttpServletResponse.class});

        method.invoke(instance, request, response);
    }
}
{% endhighlight %}

adminAction 的代码。注意： servlet 我现在写成了单例模式下的，因为这里考虑到 servlet 是单例的。 

{% highlight java %}

public class AdminAction  {

    /**
     * 饿汉式 单例 servlet
     */
    private AdminAction() {}

    private static AdminAction instance = new AdminAction();

    public static AdminAction getInstance() {

        return instance;
    }

    public void list(HttpServletRequest request, HttpServletResponse response) {

    }
}
{% endhighlight %}

注意： 这里的 web.xml 配置

{% highlight xml %}

    <servlet>
        <servlet-name>pathDispatcher</servlet-name>
        <servlet-class>com.danny.serv.PathDispatcher</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>pathDispatcher</servlet-name>
        <url-pattern>/b/*</url-pattern>
    </servlet-mapping>
{% endhighlight %}