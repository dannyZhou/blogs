---
layout: post
title: sql 语句简单练习题
date: 2015-09-21
categories: sql
tags: sql
---

* content
{:toc}

### 序
> 到今天为止， cocos2dx 项目基本完成了，带队老师的项目也基本完成了。开始自己的学习吧。上来先来点基础 sql 练习题玩玩。
我用的是 mysql 

### 参考资料
[http://blog.sina.com.cn/s/blog_46c5441f01015ubp.html](http://blog.sina.com.cn/s/blog_46c5441f01015ubp.html)

[http://www.2cto.com/database/201402/282086.html](http://www.2cto.com/database/201402/282086.html)


### 准备工作
首先创建一个学习使用的数据库，然后使用下面的代码创建环境。
{% highlight sql %}

CREATE TABLE STUDENT
(SNO VARCHAR(3) NOT NULL, 
SNAME VARCHAR(4) NOT NULL,
SSEX VARCHAR(2) NOT NULL, 
SBIRTHDAY DATETIME,
CLASS VARCHAR(5)) AUTO_INCREMENT=348  ; 

CREATE TABLE COURSE
(CNO VARCHAR(5) NOT NULL, 
CNAME VARCHAR(10) NOT NULL, 
TNO VARCHAR(10) NOT NULL) AUTO_INCREMENT=348 ;

CREATE TABLE SCORE 
(SNO VARCHAR(3) NOT NULL, 
CNO VARCHAR(5) NOT NULL, 
DEGREE NUMERIC(10, 1) NOT NULL) AUTO_INCREMENT=348 ;

CREATE TABLE TEACHER 
(TNO VARCHAR(3) NOT NULL, 
TNAME VARCHAR(4) NOT NULL, TSEX VARCHAR(2) NOT NULL, 
TBIRTHDAY DATETIME NOT NULL, PROF VARCHAR(6), 
DEPART VARCHAR(10) NOT NULL) AUTO_INCREMENT=348 ;

INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (108 ,'曾华' ,'男' ,'1977-09-01',95033);
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (105 ,'匡明' ,'男' ,'1975-10-02',95031);
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (107 ,'王丽' ,'女' ,'1976-01-23',95033);
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (101 ,'李军' ,'男' ,'1976-02-20',95033);
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (109 ,'王芳' ,'女' ,'1975-02-10',95031);
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,CLASS) VALUES (103 ,'陆君' ,'男' ,'1974-06-03',95031);

INSERT INTO COURSE(CNO,CNAME,TNO)VALUES ('3-105' ,'计算机导论',825);
INSERT INTO COURSE(CNO,CNAME,TNO)VALUES ('3-245' ,'操作系统' ,804);
INSERT INTO COURSE(CNO,CNAME,TNO)VALUES ('6-166' ,'数据电路' ,856);
INSERT INTO COURSE(CNO,CNAME,TNO)VALUES ('9-888' ,'高等数学' ,100);

INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (103,'3-245',86);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (105,'3-245',75);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (109,'3-245',68);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (103,'3-105',92);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (105,'3-105',88);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (109,'3-105',76);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (101,'3-105',64);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (107,'3-105',91);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (108,'3-105',78);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (101,'6-166',85);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (107,'6-106',79);
INSERT INTO SCORE(SNO,CNO,DEGREE)VALUES (108,'6-166',81);

INSERT INTO TEACHER(TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART) 
VALUES (804,'李诚','男','1958-12-02','副教授','计算机系');
INSERT INTO TEACHER(TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART) 
VALUES (856,'张旭','男','1969-03-12','讲师','电子工程系');
INSERT INTO TEACHER(TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART)
VALUES (825,'王萍','女','1972-05-05','助教','计算机系');
INSERT INTO TEACHER(TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART) 
VALUES (831,'刘冰','女','1977-08-14','助教','电子工程系');

{% endhighlight %}

### 题目以及解答
1. 查询教师所有的单位即不重复的Depart列。
{% highlight sql %} 
 select dintinct depart from teacher; 
{% endhighlight %}

2. 查询Score表中成绩在60到80之间的所有记录。 (注意： between and 是包含边界值的)
{% highlight sql %} 
 select  * from score where degree BETWEEN 60 and 80; 
{% endhighlight %} 
    
3. 查询Student表中“95031”班或性别为“女”的同学记录。
{% highlight sql %} 
 select * from student where class=95031 or ssex='女' 
{% endhighlight %}
    
4. 以Cno升序、Degree降序查询Score表的所有记录。
{% highlight sql %} 
 select * from score order by sno asc , degree desc; 
{% endhighlight %}
    
5. 查询 score 表中最高分的学生学号和课程号。（有没有更好的办法？）
{% highlight sql %} 
 select  degree ,cno from score  where degree =(select MAX(degree) from score); 
{% endhighlight %}
    
6. 查询‘3-105’号课程的平均分。(注意两个语句的区别)
{% highlight sql %} 
 select AVG(DISTINCT degree) from score where cno = '3-105'; 
 select AVG( degree) from score where cno = '3-105'; 
{% endhighlight %}

12. *查询Score表中至少有5名学生选修的并以3开头的课程的平均分数。(这里 使用了  having )*
{% highlight sql %} 
select avg(degree), cno from score where cno like '3%' 
group  by cno 
having count(sno) >= 5;
{% endhighlight %}

