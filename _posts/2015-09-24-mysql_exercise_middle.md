---
layout: post
title: sql 语句中级练习题
date: 2015-09-24
categories: sql
tags: sql
---

* content
{:toc}

### 序
中级 sql 语句练习

### 参考资料
[http://blog.sina.com.cn/s/blog_43eb83b90102dzk3.html](http://blog.sina.com.cn/s/blog_43eb83b90102dzk3.html)

### 准备工作
Student(Sid,Sname,Sage,Ssex) 学生表

Course(Cid,Cname,Tid) 课程表

SC(Sid,Cid,score) 成绩表

Teacher(Tid,Tname) 教师表

### 题目1
查询“某1”课程比“某2”课程成绩高的所有学生的学号;
{% highlight  sql %}
 -- 选择 sid 
select a.sid from
        -- 从两个 已经选出的 虚拟表中 进行筛选
		(select sid, score from sc where cid =1) a, (select sid, score from sc where cid = 3) b 
		--  条件： 两个之间的 sid 相等 保证是一个人的信息， 并且 a 的成绩 大于 b 的成绩
where  a.sid = b.sid and a.score > b.score ;
{% endhighlight %}

### 题目2
查询平均成绩大于60分的同学的学号和平均成绩;
{% highlight  sql %}
 -- 选出 sid 和  平均成绩
SELECT sid,avg(score) 
    -- 从 这个表中搜索
    FROM sc  
    -- 分组， 从 sid 中 分组
    GROUP BY sid 
    -- 限制 平均分数 大于 60 
    having avg(score) >60;
{% endhighlight %}
注意： group by 后面不能接 where

### 题目3
查询所有同学的学号、姓名、选课数、总成绩
{% highlight  sql %} 
SELECT Student.sid,Student.Sname,count(SC.cid),sum(score)
    FROM Student left Outer JOIN SC on 
    Student.sid=SC.sid 
    GROUP BY Student.sid,Sname
{% endhighlight %} 

### 题目4
查询没学过“叶平”老师课的同学的学号、姓名;
{% highlight  sql %}
 -- 炫出来 id 姓名
SELECT Student.sid,Student.Sname
    FROM Student 
    -- 这是指的 不在 下面这个 select 中 
    WHERE sid not in 
        -- 去重 sid  这里 可以不去重
        (SELECT distinct( SC.sid) 
            FROM SC,Course,Teacher
             -- 三个表关联 （ 可以 从下往上 理解）
            WHERE  SC.cid=Course.cid 
            AND Teacher.id=Course.tid
            AND Teacher.Tname='叶平');
{% endhighlight %}

### 题目 5
查询学过“？”并且也学过编号“？”课程的同学的学号、姓名;
{% highlight  sql %}
    -- 选出来学生姓名 和 id 
select a.SID,a.SNAME from
    -- 选出信息
    (select student.SNAME,student.SID
        -- 三个表关联
        from student,course,sc 
        where cname='c++'and sc.sid=student.sid and sc.cid=course.cid
        -- 将这个结果放在 a 中
        ) a,
    -- 和 上面一个 select 是一样的。 
    -- 选出 信息
    (select student.SNAME,student.SID
        -- 三个表关联
        from student,course,sc 
        where cname='english'and sc.sid=student.sid and sc.cid=course.cid
        -- 将这个结果放在 b 中
        ) b 
   -- 选出 两个表中都存在的信息
where a.sid=b.sid;
{% endhighlight %}

### 题目6
查询学过“叶平”老师所教的所有课的同学的学号、姓名;（这里进行所有课数量的比对， 如果说两个 数量相同， 则返回 信息）
{% highlight  sql %}
 -- 选出信息
SELECT sid, sname
    from student 
    where 
    -- sid 在 下面的信息中
    sid in ( 
        -- 获取信息 
        select sid 
        from sc , course teacher
         -- 三张表链接 
        where sc.cid = course.cid 
            and teacher.tid = course.tid 
            and teacher.tname = "zhangsan" 
        --   统计 sid 
        GROUP BY sid
         -- 限制： sc.cid 总数 与 一个教师真正教学的人数相同 
        having count(sc.cid) = ( 
            select  count(cid) 
                from course, teacher 
                where teacher.tid = course.tid and tname = "zhangsan"
            ) 
{% endhighlight %}

### 题目7
查询课程编号“”的成绩比课程编号“”课程低的所有同学的学号、姓名;
{% highlight  sql %} 
 -- 选择 sid sname 
select sid, sname
    -- 从 下面表中， 这个表将会 返回一个 虚拟的表 , 里面 有 四个 字段. 
    from ( 
    #
    select student.sid, 
        student.sname, 
        score, 
        -- 下面这里是 返回 第二科 的成绩 放在 score2 中 
        ( select 
            score 
           from sc sc_2 
           where sc_2.sid = student.sid and sc_2.cid = 2 
        )  score2 
    from student, sc 
    where student.sid = sc.sid and cid = 1 
    ) s_2 -- 重命名为 s_2 不过可能没有用到.
where score2 <  score;
{% endhighlight %}

### 题目 7 
查询所有课程成绩小于分的同学的学号、姓名;
{% highlight  sql %} 
 -- 选出 id name
SELECT sid,Sname 
    FROM (
        -- 选择, 信息, 
        SELECT Student.sid,Student.Sname,score ,
            -- 这个 select 确保返回一个信息 是第二个 成绩
                (SELECT score 
                    FROM SC SC_2 
                    WHERE SC_2.sid=Student.sid AND SC_2.cid=1
                ) score2 
           FROM Student,SC
        -- 
    WHERE Student.sid=SC.sid AND cid=1
    ) S_2 
    -- 两个数据进行比较 
    WHERE score2 < score;
{% endhighlight %}


### 经典 sql 编程问题
连续范围问题

#### 数据准备
{% highlight  sql %} 
    create table t (a int unsigned not null primary key);
    insert into t values(1);
    insert into t values(2);
    insert into t values(3);
    insert into t values(4);
    insert into t values(5);
    insert into t values(16);
    insert into t values(17);
    insert into t values(18);
{% endhighlight %}

#### 预想结果

| start_range |  end_range  |
| ----------- | ----------- |
| 1           | 3           |
| 16          | 17          | 

#### 第一步看这个 sql 语句结果集
{% highlight  sql %}  
    ##  select 
    SELECT  
        -- 这个是 t 中的 a 
        a, 
        -- 这个 是将 @a ++ 然后 赋值给 rowNumber
        @a := @a +1 as rowNumber
    from 
        -- 从 t 表 中选取
        t, 
        -- 将 @a 设置为 0 
        (select @a:=0) as abs;
{% endhighlight %}

|  a  | rowNumber |
| --- | --- |
| 1  | 1   |
| 2  |  2  |
| 3  |  3  |
| 4  |  4  |
| 5  |  5  |
| 16 |  6 |
| 17 |  7 |
| 18 |  8 |

#### 如果还没有看出来....
{% highlight  sql %}  
select a , rn , a-rn 
    from (
        SELECT a, @a := @a +1 as rowNumber
        from  t, (select @a:=0) as abs;
        ) as b;
{% endhighlight %}

|  a  |  rn  |  a-rn  |
| --- | ---- | ------ |
| 1  | 1   |    0    |
| 2  |  2  |    0    |
| 3  |  3  |    0    |
| 4  |  4  |    0    |
| 5  |  5  |    0    |
| 16 |  6 |     10    |
| 17 |  7 |     10    |
| 18 |  8 |     10    |

#### 最终sql
{% highlight  sql %}
  -- 选择出来 最大的和 最小的 并且更改列名
select MIN(a) as start_range, MAX(a) as end_range 
  from (
      -- 见上 
    select a , rn , a-rn as diff 
        from (
            SELECT a, @a := @a +1 as rowNumber
            from  t, (select @a:=0) as abs;
            ) as b
         -- 按照 diff 进行合并
        group by diff
      ) as cde;
{% endhighlight %}
