---
title: MySQL常见问题
categories: MySQL常见问题
tags: MySQL
date: 2022/12/10
cover: https://img.sj33.cn/uploads/allimg/201402/7-14022H14522550.png
---

### MySQL的安装与配置



### 设置数据库主键自动递增的位置

```mysql
alter table 表名 AUTO_INCREMENT=N;#(N:下一次添加数据是主键id就是N+1，并依次递增)
```

### 解决因删除某条数据导致自增id断层的问题

```mysql
alter table 表名 drop id;
alter table 表名 add id int(11) primary key auto_increment first;
```

### 排序ORDER BY

```sql
SELECT * FROM users ORDER BY 字段名 #默认是按字段名升序排列

SELECT * FROM users ORDER BY 字段名 DESC #指定为按字段名降序排列

SELECT * FROM users ORDER BY 字段名1 DESC, 字段名2 ASC #先按字段名1降序排列，如果字段名1相同就按字段名2升序排列 
```

### COUNT() 和 AS 的使用

````sql
SELECT COUNT(*) AS 别名 FROM users 条件 #统计出符合条件的数量并给该列起一个别名
````

### 分页查询

```sql

```

### 在表中添加一列数据

```sql
alter table 表名 add 列名 数据类型
```

### 在表中修改某一个字段的数据类型

```mysql
alter table 表名 alter column 字段 数据类型 
```

### 给某个字段添加唯一性属性

```mysql
alter table 表名 add unique(Cname)
```

### 删除一个表

```mysql
drop table 表名 restrict(限制)/cascade(级联)
```

### 索引的使用

> 例题：为学生-课程数据库中的Student、Course和SC三个表建立索引。其中Student表按学号升序建唯一索引，Course表按课程号升序建唯一索引，SC表按学号升序和课程号降序建唯一索引。

```mysql
create unique index Scno on SC(Sno ASC, Cno DESC)
```

### 修改索引名

```mysql
alter index SCno rename to SCSno
```

### 删除索引

```mysql
drop index 索引名
```

### 经典查询例句：

```mysql
select Sname NAME,'Year of Birth:' BIRTH,2014-Sage BIRTHDAY,LOWER(Sdept) DEPARTMENT
from Student
/*select 字段名 别名,字符串常量 别名,表达式 别名,小写字母表示Sdept 别名*/
```

```mysql
select distinct Sno #去重 
from SC
where Grade < 60
```

```mysql
where Sage between 20 and 23; ——————(20 <= Sage <= 23)
where Sage not between 20 and 23; ——————(Sage < 20 或 Sage > 23)
where Sdept in ('CS','MA','IS')——————(Sdept='CS' or Sdept='MA' or Sdept='IS')
where Sdept not in ('CS','MA','IS')
```

**模糊查询：**

```mysql
where Sname like '刘%';——————刘xxxx......
where Sname like '刘_ _';——————刘xx
where Sname like '_阳%';——————x阳xxxx......
where Cname like 'DB\_ Design' escape '\'——————查找Cname=DB_Design,escape '\'指明'\'为转义符
```

```mysql
order by Sdept,Sage DESC; = order by Sdept ASC,Sage DESC;
```

<br/>

```mysql
group by Cno;——————将相同Cno的数据归为一组
```

```mysql
select Cno,Count(Sno)
from SC
group by Cno;
/*
1、处理对象表SC
2、将Cno相同的数据归为一组
3、分别对每一组数据进行处理
4、得到每一个课程的选课人数
*/
```

>where子句中不能用聚集函数作为条件表达式
>
>```mysql
>select Sno,AVG(Grade)
>from SC
>group by Sno
>having AVG(grade) >= 90
>/*
>1、从SC中查出Sno和Grade列
>2、将Sno相同的数据归为一组
>3、将每一组数据处理得到的Sno(学号)和(平均成绩)
>4、将平均成绩 >= 90的数据保留下来
>*/
>```

### 外连接：

> 左外连接left,右外连接right

```mysql
select Student.Sno,Sname,Ssex,Sage,Sdept,Cno,Grade
from Student left outer join SC on(Student.Sno = SC.Sno)
```

<br/>

```mysql
from(select * from student where Ssex="女") as females
where females.Sage > 18
/*
1、查出所有女学生，并将这个查找结果命名为females
2、找出查询结果中Sage>18的数据
*/
```

