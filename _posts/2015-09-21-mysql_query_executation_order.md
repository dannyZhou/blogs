---
layout: post
title: sql 中 query 的执行顺序
date: 2015-09-21
categories: sql
tags: sql
---

* content
{:toc}

### 序
今天在做 sql 的练习题（详见本站其他博客）的时候，有一个疑问在我脑海中浮现出来，就是 select 中 order by group by having 
书写顺序和执行顺序。

### 参考资料
[http://www.cnblogs.com/rollenholt/p/3776923.html](http://www.cnblogs.com/rollenholt/p/3776923.html)

[http://www.w3school.com.cn/sql/sql_having.asp](http://www.w3school.com.cn/sql/sql_having.asp)

### mysql 执行顺序（一种解释，出自第一个参考资料）
MySQL的语句一共分为11步，如下表所标注的那样，最先执行的总是FROM操作，最后执行的是LIMIT操作。
其中每一个操作都会产生一张虚拟的表，这个虚拟的表作为一个处理的输入，只是这些虚拟的表对用户来说是透明的，
但是只有最后一个虚拟的表才会被作为结果返回。如果没有在语句中指定某一个子句，那么将会跳过相应的步骤。
{% highlight sql %}
(8) select (9) distinct <select_list>
(1) from <left_table>
(3) <join_type> JOIN <right_type>
(2)         ON <join_condition>
(4) WHERE <where_condition>
(5) GROUP BY <group_by_list>
(6) WITH {CUBE|ROLLUP}
(7) HAVING <having_condition>
(10) ORDER BY <order_by_list>
(11) LIMIT <limit_number>
{% endhighlight %}

### 上面顺序的详细介绍
1.   FORM: 对FROM的左边的表和右边的表计算笛卡尔积。产生虚表VT1
2.   ON: 对虚表VT1进行ON筛选，只有那些符合 &lt; join-condition &gt; 的行才会被记录在虚表VT2中。
3.   JOIN： 如果指定了OUTER JOIN（比如left join、 right join），那么保留表中未匹配的行就会作为外部行添加到虚拟表VT2中，产生虚拟表VT3, rug from子句中包含两个以上的表的话，那么就会对上一个join连接产生的结果VT3和下一个表重复执行步骤1~3这三个步骤，一直到处理完所有的表为止。
4.   WHERE： 对虚拟表VT3进行WHERE条件过滤。只有符合 &lt; where-condition &gt; 的记录才会被插入到虚拟表VT4中。
5.   GROUP BY: 根据group by子句中的列，对VT4中的记录进行分组操作，产生VT5.
6.   CUBE | ROLLUP: 对表VT5进行cube或者rollup操作，产生表VT6.
7.   HAVING： 对虚拟表VT6应用having过滤，只有符合 &lt; having-condition &gt; 的记录才会被 插入到虚拟表VT7中。
8.   SELECT： 执行select操作，选择指定的列，插入到虚拟表VT8中。
9.   DISTINCT： 对VT8中的记录进行去重。产生虚拟表VT9.
10.   ORDER BY: 将虚拟表VT9中的记录按照 &lt; order_by_list &gt; 进行排序操作，产生虚拟表VT10.
11.   LIMIT：取出指定行的记录，产生虚拟表VT11, 并将结果返回。

### 上面解释的疑问
我疑惑的是，如果ORDER BY是在SELECT之后执行，那么我 ORDER BY 非 SELECT 的字段，已经无法解释。
比如：
{% highlight sql %}
select student_name from tb_student order by student_no；
{% endhighlight %}
所以，我认为是先执行order by才执行的select。

> 自己的见解： SELECT 是最后执行的一个代码。

### having 解释
在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与合计函数一起使用。实际项目使用详见另外两篇文章。
