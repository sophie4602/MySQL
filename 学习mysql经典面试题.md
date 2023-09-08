# 学习SQL

SQL 是最流行的关系型数据库管理系统，在 WEB 应用方面 SQL 是最好的 RDBMS（Relational Database Management System：关系数据库管理系统）应用软件之一。

### 一、RDBMS术语

* **数据库Database**: 数据库是一些关联表的集合。
* **数据表table**: 表是数据的矩阵。在一个数据库中的表看起来像一个简单的电子表格。
* **列column**: 一列(数据元素) 包含了相同类型的数据, 例如邮政编码的数据。
* **行**：一行（元组，或记录）是一组相关的数据，例如一条用户订阅的数据。
* **冗余**：存储两倍数据，冗余降低了性能，但提高了数据的安全性。
* **主键primary key**：主键是唯一的。一个数据表中只能包含一个主键，**是确定一条记录的唯一标识**。不能重复，不能为空null。
* **外键foreign key**：外键是另一个表的主键，可以重复，可以为空。用于关联两个表。
* **复合键**：复合键（组合键）将多个列作为一个索引键，一般用于复合索引。
* **索引index**：使用索引可**快速**访问数据库表中的特定信息。索引是对数据库表中一列或多列的值进行排序的一种结构。类似于书籍的目录。
* **参照完整性reference**: 参照的完整性要求关系中不允许引用不存在的实体。与实体完整性是关系模型必须满足的完整性约束条件，目的是保证数据的一致性。

### 二、SQL数据库
SQL 是一个关系型数据库管理系统，由瑞典 SQL AB 公司开发，目前属于 **Oracle** 公司。SQL 是一种关系数据库管理系统，关系数据库将数据保存在不同的表中，而不是将所有数据放在一个大仓库内，这样就增加了速度并提高了灵活性。

### 三、管理SQL
以下列出了使用SQL数据库过程中常用的命令：
- **USE \*数据库名\*** :
  选择要操作的SQL数据库，使用该命令后所有SQL命令都只针对该数据库。

- **SHOW DATABASES:**

- **SHOW TABLES:**
  显示指定数据库的所有表，使用该命令前需要使用 use 命令来选择要操作的数据库。

- **SHOW COLUMNS FROM \*数据表\*:**
  显示数据表的属性，属性类型，主键信息 ，是否为 NULL，默认值等其他信息。

- **SHOW INDEX FROM \*数据表\*:**
  显示数据表的详细索引信息，包括PRIMARY KEY（主键）。


- **SHOW TABLE STATUS [FROM db_name] [LIKE 'pattern'] \G:**
  该命令将输出SQL数据库管理系统的性能及统计信息。

  ```SQL
  SQL> SHOW TABLE STATUS  FROM student;   # 显示数据库 student 中所有表的信息
  SQL> SHOW TABLE STATUS from student LIKE 's%';     # 表名以student开头的表的信息
  SQL> SHOW TABLE STATUS from student LIKE 's%'\G;   # 加上 \G，查询结果按列打印
  ```

### 四、SQL查询

- select 查询结果，如: `[学号,平均成绩：组函数avg(成绩)]`

- from 从哪张表中查找数据，如:`[涉及到成绩：成绩表sc]`

- where 查询条件，如:`[b.课程号='01' and b.成绩>80]`

- group by 分组，如:`[每个学生的平均：按学号分组]`(oracle,SQL server中出现在select 子句后的非分组函数，必须出现在group by子句后出现),SQL中可以不用

- having 对分组结果指定条件，如:`[平均分大于60分]`

- order by 对查询结果排序，如:`[增序: 成绩 ASC / 降序: 成绩 DESC]`;

- limit 使用limt子句返回topN（对应这个问题返回的成绩前两名），如:`[ limit 2 ==>从0索引开始读取2个]`limit==>从0索引开始 `[0,N-1]`

- **组函数:** 去重 distinct() 统计总数sum() 计算个数count() 平均数avg() 最大值max() 最小数min()

- **多表连接:** 
内连接(省略默认inner) join ...on..as b on a.key ==b.key
左连接left join tableName as b on a.key ==b.key
右连接right join ... on ...
连接union(无重复(过滤去重))
union all(有重复[不过滤去重])

##### 查询执行的顺序
select语句的执行顺序，与书写顺序不一样。下面简单了解一下。
select语句执行顺序：
**from --> on --> join --> where --> group by --> having --> select --> order by --> limit**


### 五、表文件及数据准备

##### 新建数据库；删除数据库；

```sql
create database student;
drop database student;
```
##### 建立数据表

##### 1.学生表student(SId,Sname,Sage,Ssex)
```SQL
-- SId 学号,Sname 学生姓名,Sage 出生年月,Ssex 学生性别
create table Student(SId varchar(10),Sname varchar(10),Sage datetime,Ssex varchar(10));
insert into Student values('01' , '赵雷' , '1990-01-01' , '男');
insert into Student values('02' , '钱电' , '1990-12-21' , '男');
insert into Student values('03' , '孙风' , '1990-05-20' , '男');
insert into Student values('04' , '李云' , '1990-08-06' , '男');
insert into Student values('05' , '周梅' , '1991-12-01' , '女');
insert into Student values('06' , '吴孙' , '1992-03-01' , '女');
insert into Student values('07' , '郑竹' , '1989-07-01' , '女');
insert into Student values('09' , '张三' , '2017-12-20' , '女');
insert into Student values('10' , '李四' , '2017-12-25' , '女');
insert into Student values('11' , '李四' , '2017-12-30' , '女');
insert into Student values('12' , '赵六' , '2017-01-01' , '女');
insert into Student values('13' , '孙七' , '2018-01-01' , '女');
insert into Student values('14' , '李孙' , '2018-04-15' , '男');
```
##### 2.科目表 Course(CId,Cname,TId)
```SQL
-- CId --课程编号,Cname 课程名称,TId 教师编号
create table Course(CId varchar(10),Cname nvarchar(10),TId varchar(10));
insert into Course values('01' , '语文' , '02');
insert into Course values('02' , '数学' , '01');
insert into Course values('03' , '英语' , '03');
```
##### 3.教师表 Teacher(TId,Tname)
```SQL
-- TId 教师编号,Tname 教师姓名
create table Teacher(TId varchar(10),Tname varchar(10));
insert into Teacher values('01' , '张三');
insert into Teacher values('02' , '李四');
insert into Teacher values('03' , '王五');
```
##### 4.成绩表 SC(SId,CId,score)
```SQL
-- SId 学号,CId 课程编号,score 分数
create table SC(SId varchar(10),CId varchar(10),score decimal(18,1));
insert into SC values('01' , '01' , 80);
insert into SC values('01' , '02' , 90);
insert into SC values('01' , '03' , 99);
insert into SC values('02' , '01' , 70);
insert into SC values('02' , '02' , 60);
insert into SC values('02' , '03' , 80);
insert into SC values('03' , '01' , 80);
insert into SC values('03' , '02' , 80);
insert into SC values('03' , '03' , 80);
insert into SC values('04' , '01' , 50);
insert into SC values('04' , '02' , 30);
insert into SC values('04' , '03' , 20);
insert into SC values('05' , '01' , 76);
insert into SC values('05' , '02' , 87);
insert into SC values('06' , '01' , 31);
insert into SC values('06' , '03' , 34);
insert into SC values('07' , '02' , 89);
insert into SC values('07' , '03' , 98);
```
##### 注意：插入数据时出行错误
```SQL
SQL> insert into student values('01','钱电','1990-12-31','男');
ERROR 1366 (HY000): Incorrect string value: '\xC7\xAE\xB5\xE7' for column 'Sname' at row 1
```
是因为字符集为Latin1导致。

查询字符集的方法：
```SQL
SQL> show create table student;
+---------------------------------------------------------------------------------+
| Table   | Create Table                                                          |
+---------------------------------------------------------------------------------+
| student | CREATE TABLE `student` (
  `SId` varchar(10) DEFAULT NULL,
  `Sname` varchar(10) DEFAULT NULL,
  `Sage` datetime DEFAULT NULL,
  `Ssex` varchar(10) DEFAULT NULL
) ENGINE=MyISAM DEFAULT CHARSET=latin1                                            |
+---------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

需要调整数据库及数据表及字段的编码格式
```SQL
alter database student character set utf8;
alter table student character set utf8;
alter table student change sname sname varchar(10) character set utf8;
alter table student change ssex ssex varchar(10) character set utf8;
```

### 六、查询练习（来自网上SQL经典面试50题，打乱原顺序，按照由简到难的顺序练习）

#### （一）、简单查询（一个表）

##### 1.查询数据表student各列
```SQL
show columns from student;
select * from student;
select sid,sname,sage,ssex from student;
```
##### 2.查询课程编号为 01 且课程成绩在 80 分以上的学生的学号和姓名（原31题简化下，不需要学生详细情况,考察where中的复合条件）
```SQL
select *
from sc
where cid='01' and score>=80
```
---
==部分日期函数==

|用途|函数|案例|
|--|--|--|
|当前日期|current_date|current_date 结果：2023-08-20|
|当前时间|current_time|current_time 结果：11:36:00|
|当前日期和时间|current_timestamp|current_timestamp 结果：2023-08-20　11:36:00|
|获取日期的年份 月份 日期|year() month() day()|year('2023-08-20') 结果：2023|
|日期对应星期几|dayname()|dayanme('2023-08-20 11:36:00') 结果：星期三|

##### 3.查询 1990 年出生的学生名单year()（原24）

```SQL
select * from student where year(sage)=1990';
```

#####  4.查询各学生年龄，只按年份来算now()（原40题）
```SQL
select *,year(now())-year(sage) as age
from student
```
##### 5.按照出生日期来算，当前月日 < 出生年月的月日则，年龄减一（原41题）
**时间差函数timestampdiff()函数不是单纯的计算 年 的相减，月日时间也会算到(貌似是MySQL专有的)**
```SQL
select *,timestampdiff(year,sage,curdate()) as 年龄,year(now())-year(sage) as age
from student
```
##### 6.查询本月过生日的学生month(),curdate()(原44题)
```SQL
select *
from student
where month(sage)=month(curdate())
```
##### 7.查询下月过生日的学生month(),curdate()(原45题)
```SQL
-- 错误解法，没有考虑本月为12月情况，下个月就应该是1月，而不是13月
select *
from student
where month(sage)=month(curdate())+1

-- 考虑到本月如果是12月情况，能用下面条件语句case进行判断
select *
from student
where month(sage)=case when month(curdate())+1=12 then 1 else month(curdate())+1 end
```
##### 8.查询本周过生日的学生，函数weekofyear()(原42题)
```SQL
-- 网上说这种解法有错误，因为每年对应周的日期值并不一定是一样的
-- 但我理解这么计算应该没有错误
select *,weekofyear(sage),weekofyear(curdate())
from student
where weekofyear(sage)=weekofyear(curdate())
```
##### 9.查询下周过生日的学生(原43题)
```SQL
select *
from student
where weekofyear(sage)=weekofyear(curdate())+1
```
---
**LIKE 及通配符**（%是通配符，可通配任意字符，不同位置发挥不同作用）
##### 10.查询姓【孙】的学生信息(原22题) 以及查询名字中含有【孙】字的学生信息
```SQL
select * from student where sname like '孙%';
select * from student where sname like '%孙%';
```
---
**去重**
##### 11.查询不及格课程（原30）
```SQL
-- 方法一
-- 下面写法对不对？为什么不对？
select distinct *
from sc
where score<60
-- 下面的写法对不对？
select distinct cid
from sc
where score<60

-- 方法二
-- 其实分组，也是为了去重
select cid
from sc
where score<60
group by cid
```
#### （二）、统计分组、聚合函数（sum(),avg(),count(),max(),min()） 

1.聚合函数sum()/avg()/count()/max()/min()经常与好基友group by搭配使用

2.在使用group by时，select后面只能放

- 常数（如数字/字符/时间）
- 聚合键（也就是group by后面的列名）
- 聚合函数

因此，在使用group by时，千万不要在select后面放聚合键以外的列名！

3.where函数后面不能直接使用聚合函数！（考虑放在having后面/变成子查询放在where后面）

---
##### 12.查询课程编号为'01'的总成绩
```SQL
select sum(score) as sum_score
from sc
where sid='01';
```
##### 13.查询男生、女生人数（原21）
```SQL
select ssex,count(ssex)
from student
group by ssexselect 
```
##### 14.查询【李】姓老师的数量（原5）
```SQL
select count(tname) 
from teacher
where tname like '李%';
```
##### 15.求每门课程的学生人数(原32)
```SQL
-- 不能使用count（*），因为会计算sid 为null的行，如果有的话（后面有详细解释）
select cid,count(sid) as count_sid
from sc 
group by cid
-- 带上课程名称
select sc.cid,c.cname,count(sid) as count_sid
from sc sc
join course c on sc.cid=c.cid
group by sc.cid,c.cname

-- 网上下面的写法是错误的，因为select list中cname不在group by list中
select sc.cid,cname,count(sid) 人数 
from sc,course c
where sc.cid=c.cid
group by sc.cid;
```
##### 16.查询选了课程'02'的学生人数
```SQL
-- ？奇怪，这个distinct有没有影响？本例没有影响
select count(distinct sid) as scount
from sc
where cid='02';
```
##### 17.查询各科成绩最高和最低的分， 以如下的形式显示：课程号，最高分，最低分（14题简化版）
```SQL
select cid as 课程号 ,max(score) as 最高分,min(score) as 最低分
from sc
group by cid;
```
##### 18.查询每门课程被选修的学生数(原19)
```SQL
select cid,count(sid)
from sc
group by cid;
```
##### 19.统计每门课程的学生选修人数（超过 5 人的课程才统计）（原37）
```SQL
select cid,count(cid)
from sc
group by cid
having count(cid)>5;
-- 注意用count(*)可能会导致cid为null的行也被计算在内，不过考虑到cid不可能有null的，用count（*）应该也不会错
```
##### 20.检索至少选修两门课程的学生学号(原38),注意having这么写也可以
```SQL
-- 标准写法
select sid,count(cid)
from sc
group by sid
having count(cid)>2;
-- 这么写居然也可以
select sid,count(sid) as cc
from sc
group by sid
having cc>=2;
```
##### 21.查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列（原25题）
```SQL
select cid,avg(score) as avg_score
from sc
group by cid
order by avg(score) desc,cid asc;
-- SQL在order by中直接引用上面字段别名也可以
select cid,avg(score) as avg_score
from sc
group by cid
order by avg_score desc,cid asc;
```
##### 22.查询平均成绩大于60分学生的学号和平均成绩（原2）
```SQL
select sid,avg(score)
from sc
group by sid
having avg(score)>60;
```

#### （三）、多表查询

- **INNER JOIN（内连接,或等值连接）：** 获取两个表中字段匹配关系的记录。
- **LEFT JOIN（左连接）：** 获取左表所有记录，即使右表没有对应匹配的记录。
- **RIGHT JOIN（右连接）：** 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。
- **全连接：** SQL没有全连接，用union来替代
图示如下：
![连接图示](join.jpg)

##### 23.查询学生选课情况，有学号，学生姓名，课程名称（普通的3表关联）
```SQL
select s.sid,s.sname,cname
from student s
join sc sc on s.sid=sc.sid
join course c on sc.cid=c.cid;

-- 另一种写法
-- 网上的另一种写法，将表都写入from，并将关联的条件写入where中
-- 全部内连接的数据表，感觉这样写写法简单
-- 但在网上没有查到这么写的优点
select student.sid,sname,cname
from student,sc,course
where student.sid=sc.sid and sc.cid=course.cid;
```
##### 24.查询所有学生的课程及分数情况（存在学生没成绩，没选课的情况）（原28题）
**理解左连接（如果用内连接，就不能显示没有选课的学生清单了）**
```SQL
select *
from student s
left outer join sc sc on s.sid=sc.sid
```
##### 25.查询任何一门课程成绩在 70 分以上的姓名、课程名称和分数(原29题)
```SQL
select s.sname,c.cname,sc.score
from sc sc
join course c on sc.cid=c.cid
join student s on sc.sid=s.sid
where sc.score>70

-- 网上的另一种写法
select student.sname, course.cname,sc.score 
from student,course,sc
where sc.score>70 and student.sid = sc.sid and sc.cid = course.cid;
```
##### 26.查询同名同性学生名单，并统计同名人数(原23)
```SQL
-- 注意，没有要求显示出不同名同性的学生，所有用join,剔除那些没有同名的学生名单
select s.*,tm.tmrs
from student s
join (
	select sname,ssex,count(*) as tmrs
	from student
	group by sname,ssex
	having count(*)>1 )tm on s.sname=tm.sname and s.ssex=tm.ssex
```
##### 27.查询平均成绩大于85分所有学生的学号、姓名和平均成绩（原26题）
```SQL
-- 我总是用join写成子查询情况
select s.*,tm.avg_score
from student s
join (
	select sid,avg(score) as avg_score
	from sc
	group by sid
	having avg_score>=85 )tm on s.sid=tm.sid

-- 其实下面的方法也对，不过ms sql可能不允许在having中直接引用别名
-- 聚合函数前面这么写，居然没有错误，我分析sc.sid与s中是一对一的关系
select s.*,avg(score) as avg_score
from sc 
join student s on sc.sid=s.sid
group by sc.sid
having avg(score)>85;
```
##### 28.查询课程名称为「数学」，且分数低于 60 的学生姓名和分数(原27题)
```SQL
select s.*,c.cname,sc.score
from sc sc
join course c on sc.cid=c.cid
join student s on sc.sid=s.sid
where c.cname='数学' and sc.score<60;

-- 网上的写法，也没有错
select a.sname,b.cid,b.score 
from student a
inner join sc b on a.sid=b.sid
where cid=(select cid from course where cname='数学')and b.score<60;
```
##### 29.检索" 01 "课程分数小于 60，按分数降序排列的学生信息（原12题）
```SQL
select s.*,sc.score
from sc
join student s on sc.sid=s.sid
where cid='01' and score<60
order by sc.score desc
```
#### （四）、复杂查询

##### 30.查询所有学生的学号、姓名、选课总数、所有课程的总成绩（没有成绩显示null）、平均分（原4）
```SQL
select s.sid,s.sname,count(sc.cid) as count_cid,sum(sc.score) as sum_score,avg(sc.score) as avg_score
from student as s
left join sc as sc on s.sid=sc.sid
group by s.sid;
```
###### 上面的，只查询有成绩的学生信息
```SQL
select s.sid,s.sname,count(sc.cid) as count_cid,sum(sc.score) as sum_score,avg(sc.score) as avg_score
from student as s
join sc as sc on s.sid=sc.sid
group by s.sid;
```
###### 上面的条件再选出平均分大于70分的（增加查询条件）
```SQL
-- 这个用left join 可不可以？可以，保留了没有成绩的学生，但是，没有成绩的学生，不满足平均分>70的条件被筛出了
-- 我喜欢用left join，感觉数据不会被漏掉
select s.sid,s.sname,count(sc.cid) as count_cid,sum(sc.score) as sum_score,avg(sc.score) as avg_score
from student as s
join sc as sc on s.sid=sc.sid
group by s.sid
having avg(sc.score)>70;
```
###### 上面结果再排序，按照平均分从高到底排序（再增加排序）
```SQL
select s.sid,s.sname,count(sc.cid) as count_cid,sum(sc.score) as sum_score,avg(sc.score) as avg_score
from student as s
join sc as sc on s.sid=sc.sid
group by s.sid
having avg(sc.score)>70
order by avg(sc.score) desc;
```
##### 31.查询在 SC 表存在成绩的学生信息（如何去重）（原3）
```SQL
select b.* 
from sc a
left join student b on a.sid=b.sid
group by a.sid;

--另一种写法
select distinct b.* 
from sc a
left join student b on a.sid=b.sid;
```
##### 32.查询学过「张三」老师授课的**同学的信息**（原6）
```SQL
select distinct s.* 
from sc sc
join course c on sc.cid=c.cid
join teacher t on c.tid=t.tid
join student s on sc.sid=s.sid
where t.tname='张三';

-- 网上的写法如下
select student.*
from teacher,course,student,sc
where teacher.tname='张三' and teacher.tid=course.tid and course.cid=sc.cid and sc.sid=student.sid
```
##### 33.查询没学过"张三"老师讲授的任一门课程的学生姓名（原题10）
```SQL
-- 我是一步步写，观察结果后，写出两个子查询的方法，易于理解但估计查询效率低
select s.*
from student s
where sid not in(
	select sid 
	from sc
	where cid  in (
		select cid
		from teacher t
		join course c on t.tid=c.tid
		where tname='张三') ) ;

 -- 网上的写法，也容易理解，张三教授任意一门课程，选学这些课的学生，排除这些学生即可
 select *
from student
where sid not in(
  select distinct sc.sid
  from sc
  join course c on sc.cid=c.cid
  join teacher t  on c.tid=t.tid
  where t.tname='张三');
```
##### 34.查询选修了全部课程的学生信息(原39)
```SQL
-- 选修了全部课程，需要得知一共多少课程，如果选修的课程数与多少课一样就可以了
-- select 语句后的s.*很独特，但居然也可以
select s.*,count(cid)
from sc sc
join student s on sc.sid=s.sid
group by sid
having count(cid) = (select count(*) from course)
-- 网上很独特的写法，用到子查询，结果是一样的
select * from student a
where (select count(*) from sc b where a.sid=b.sid)
    =(select count(*) from course)
```
##### 35.查询没有学全所有课程的**同学的信息**(我都犯晕了，这个怎么判断？23.9.1)（原7）
##### 这个主要根据每个学生选课的数量进行判断，如果选课数量小于开课的数量，就说明没有学全所有课程
```SQL
--- ？这么写怎么就是对的？
select a.* 
from student a 
left join sc b on a.sid=b.sid 
group by a.sid 
having count(b.cid)<(select count(cid) from course);

-- 下面这种写法居然是对的！！
-- 后来考虑，可能因为student表里的数据都是唯一的，所以就可以省略
select s.*,count(sc.cid)
from student s
left join sc on s.sid=sc.sid
group by s.sid
```
##### 36.查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩（原题11）
```SQL
-- 哦，下面这个写法没有平均成绩，这个写法就是不对的
select s.*,avg_sc.avg_score
from student s 
where sid in(
	select sid
	from sc 
	where score<60
	group by sid
	having count(sid)>=2);

-- 如果加上平均成绩，下面的写法
select s.*,avg_sc.avg_score
from student s 
join (select sid,avg(score) as avg_score
  from sc
  group by sid) avg_sc on s.sid=avg_sc.sid
where s.sid in(
	select sid
	from sc 
	where score<60
	group by sid
	having count(sid)>=2);

-- -------------------
-- 另一种写法，应该效率更高些
--这个平均成绩是哪个平均成绩？因为考虑到第二个汇总，只是算成绩《60分的学生，但是这个学生的成绩不一定都小于60，平均成绩不能从这里算，我就单独计算了一下平均成绩
select s.*,avg_sc.avg_score
from student s
join (
    select sid
	from sc 
	where score<60
	group by sid
	having count(sid)>=2) sc on s.sid=sc.sid
join (select sid,avg(score) as avg_score
	from sc
	group by sid) avg_sc on s.sid=avg_sc.sid;

-- 网上的例子，我觉得不严谨，只计算了分数《60分的成绩的平均分，当然目前结果都是一致的
select a.sid,sname,avg(score) as avg_score
from sc a 
left join student b on a.sid=b.sid
where a.score<60
group by a.sid 
having count(cid)>=2;  
```
##### 37.按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩(原13题)
```SQL
select px.*,scc.score
from(
	select s.*,avg(sc.score) as avg_score
	from sc sc
	join student s on sc.sid=s.sid
	group by sc.sid
	order by avg_score desc ) px
left outer join sc scc on px.sid=scc.sid
```
##### 38.成绩不重复，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩（原33）
```Sql
-- 强调成绩不重复，是什么意思呢？
-- 使用了这样简单的写法，注意各个表的字段别弄错了
select s.*,c.cname,sc.score
from course c,teacher t,sc ,student s
where c.tid=t.tid and tname='张三' and c.cid=sc.cid and sc.sid=s.sid
order by score desc
limit 1;
-- 也可以用下面的写法，是我习惯的写法，但确实比较麻烦
select sc.sid,s.sname,s.ssex,s.sage,sc.cid,c.cname,sc.score
from sc sc
join student s on sc.sid=s.sid
join course c on sc.cid=c.cid
join teacher t on c.tid=t.tid
where t.tname='张三'
order by sc.score desc
limit 1
```
##### 39.成绩有重复的情况下，查询选修「张三」老师所授课程的学生中，成绩最高的学生信息及其成绩（原34题）
* **为了验证结果，先修改课程情况**
```SQL
-- 同一学科设置同样成绩
UPDATE sc 
SET score=90
where sid = "07" and cid ="02";
-- 不同学科设置同样成绩
UPDATE sc 
SET score=90
where sid = '02' and cid ='03';
```
**出现错误**
>Error Code: 1175. You are using safe update mode and you tried to update a table without a WHERE that uses a KEY column.  To disable safe mode, toggle the option in Preferences -> SQL Editor and reconnect.

* 查找原因
>这是因为MySql运行在safe-updates模式下，该模式会导致非主键条件下无法执行update或者delete命令。
>可以通过以下SQL进行状态查询：
```SQL
show variables like 'SQL_SAFE_UPDATES';
```
* 解决方法(执行下面的sql语句，关闭sql_safe_updates)
```SQL
SET SQL_SAFE_UPDATES = 0;
-- 或
SET SQL_SAFE_UPDATES = false;
```
* 打开的方法
```SQL
SET SQL_SAFE_UPDATES = 1;
SET SQL_SAFE_UPDATES = true;
```
```SQL
-- 强调【成绩有重复的】，比如成绩最高分的人有两个人，需要都显示出来，而上面的limit  1 限制只能有1条记录，答案就是错误的
-- 我当时只考虑排序的方法，就是本文最后使用排序的练习
-- 方法一
-- 先用网友判断方法：如果成绩有重复，那么获得该科成绩最高分，然后再去查找这个与该科成绩最高分的相同的学生信息
-- 我喜欢用left join语句
select s.*,cj.*
from sc sc
join student s on sc.sid=s.sid
join(
	select sc.cid,c.cname,max(score) as max_score
	from sc sc,course c,teacher t
	where sc.cid=c.cid and c.tid=t.tid and tname='张三'
	group by sc.cid,c.cname)cj on sc.cid=cj.cid and sc.score=cj.max_score;
-- 方法二
-- 以下是网友写的，感觉有漏洞，如果老师不止教一门课呢
-- 这个查询的写法，老师教的所有课中，取分数最高的成绩，不管是哪门课，学生学的课程中，也不管是哪门课，只要成绩对上了就可以？
-- 再细看，外围的连接中，还是限制了老师的名字，剔除了不相关的课程，答案是对的
select student.*, sc.score, sc.cid 
from student, teacher, course,sc 
where teacher.tid = course.tid and sc.sid = student.sid and sc.cid = course.cid and teacher.tname = "张三"
	and sc.score = (
		select max(sc.score) 
		from sc,student, teacher, course
		where teacher.tid = course.tid
		and sc.sid = student.sid
		and sc.cid = course.cid
		and teacher.tname = "张三"
	);	
-- 方法三
-- 使用mysql的排序的方法，具体用法见后面
select s.*,c.cname,sc.score
from 
(select sid,cid,score,rank() over (partition by cid order by score desc) ranks from sc)sc
join student s on sc.sid=s.sid
join course c on sc.cid=c.cid
join teacher t on c.tid=t.tid
where t.tname='张三' and ranks=1;
-- 下面的写法更简单些
select s.*,c.cname,sc.score
from 
(select sid,cid,score,rank() over (partition by cid order by score desc) ranks from sc)sc
,student s,course c,teacher t
where sc.sid=s.sid and sc.cid=c.cid and c.tid=t.tid and t.tname='张三' and ranks=1;
```
-- 
**SQL CASE 函数定义和用法**

CASE 语句遍历条件并在满足第一个条件时返回一个值（如 IF-THEN-ELSE 语句）。 因此，一旦条件为真，它将停止读取并返回结果。

如果没有条件为真，它将返回 ELSE 子句中的值。

如果没有ELSE部分且没有条件为真，则返回NULL。
##### 语法
```SQL
CASE
  WHEN *condition1* THEN *result1*
  WHEN *condition2* THEN *result2*
  WHEN *conditionN* THEN *resultN*
  ELSE *result*
END;
```
##### 40.分数段按这个[100-85],[85-70],[70-60],[<60]评分标准显示每个分数所在的评分标准(自编练习)
```SQL
-- 就是相当于一个列，不过按照我们的要求进行调整了数据显示的方式
select sid,cid,score,
	case when score between 85 and 100 then '优秀' 
		when score between 70 and 84 then '良好' 
    	when score between 60 and 69 then '及格'
		else  '不及格'
	end as 评分标准
from sc;
```
##### 41.查询出每门课程的及格人数和不及格人数()
```SQL
select cname
	,sum(case when score>=60 then 1 else 0 end) as 及格人数
	,sum(case when score<60 then 1 else 0 end) as 不及格人数
from sc sc
left join course c on sc.cid=c.cid
group by cname;
```
##### 42.算出[100-85],[85-70],[70-60],[<60]来统计各科成绩，分别统计：各分数段人数，课程号和课程名称（练习between）（原14简化）
```SQL

select sc.cid,c.cname
	,sum(case when score between 85 and 100 then 1 else 0 end) as scroe_1
	,sum(case when score between 70 and 84 then 1 else 0 end)  as scroe_2
	,sum(case when score between 60 and 69 then 1 else 0 end)  as scroe_3
	,sum(case when score<60 then 1 else 0 end) as scroe_4
from sc sc
left join course c on sc.cid=c.cid
group by sc.cid,c.cname;
```
##### 43.查询各科成绩最高分、最低分和平均分： 以如下形式显示：课程 ID，课程 name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率 及格为>=60，中等为：70-80，优良为：80-90，优秀为：>=90 要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列（原14题）
```SQL
select sc.cid,c.cname,max(score) as max_score,min(score) as min_score,avg(score) as avg_score
	,count(*) as 选修人数
	,sum(case when score between 90 and 100 then 1 else 0 end )/count(*) as 优秀率
	,sum(case when score between 80 and 89 then 1 else 0 end )/count(*) as 良好率
	,sum(case when score between 70 and 79 then 1 else 0 end )/count(*) as 中等率
	,sum(case when score between 60 and 69 then 1 else 0 end )/count(*) as 及格率
from sc
join course c on sc.cid=c.cid
group by sc.cid,c.cname
order by count(*) desc,sc.cid 
```
##### 行列转换
##### 44.求每个人各科成绩
```SQL
-- 方法一，好理解的，也是我常用的，如果学生没有该课成绩，显示为null，我用函数ifnull()将其改为0
-- 顺便熟悉一下ifnull（），isnull（）的用法
-- 缺点：科目是固定的，如果科目是变化的，下面方法就不对
select s.sid,sname
	,ifnull(cid_01,0) as cid_01
    ,if(isnull(cid_02),0,cid_02) as cid_02
    ,case when isnull(cid_03) then 0 else cid_03 end as cid_03
from student s 
left join (select sid,score as cid_01 from sc where cid='01') c01 on s.sid=c01.sid
left join (select sid,score as cid_02 from sc where cid='02') c02 on s.sid=c02.sid
left join (select sid,score as cid_03 from sc where cid='03') c03 on s.sid=c03.sid;

-- 还有一种写法，这种写法对吗？
-- 不对！！无法获得每个人的各科成绩，只能获得三科都有成绩的学生明细(因为条件中的and语句，甚至剔除了某一科有成绩而其它科没有成绩的情况)
select a.sid,a.score as '01',b.score '02',c.score '03'
from student s
inner join sc as a on s.sid=a.sid
inner join sc as b on s.sid=b.sid
inner join sc as c on s.sid=c.sid
where a.cid='01' and b.cid='02' and c.cid='03';

-- 这种写法对吗？
-- 不对！！上面的语句，内连接改成left join能获得所有成绩吗？（不能，还是条件限制）
select a.sid,a.score as '01',b.score '02',c.score '03'
from student s
left join sc as a on s.sid=a.sid
left join sc as b on s.sid=b.sid
left join sc as c on s.sid=c.sid
where a.cid='01' and b.cid='02' and c.cid='03';

-- 这时可用另一种写法，同上面类似，但是放student的地方变了,也可以
-- 这种写法只能获得同时存在01，02，03课程的情况
select a.sid,s.Sname,a.score as '01',b.score as '02',c.score as '03'
from sc as a 
inner join sc as b on a.sid=b.sid
inner join sc as c on a.sid=c.sid
inner join student as s on c.sid = s.sid 
where a.cid='01' and b.cid='02' and c.cid='03';


-- 方法二、case语句
-- 只是有成绩的学生
select sc.sid,s.sname,
    (case when cid='01' then score else 0 end) as cid_01,
    (case when cid='02' then score else 0 end) as cid_02,
    (case when cid='03' then score else 0 end) as cid_03
from sc sc
join student s on sc.sid=s.sid;
-- 换种写法，结果一样
select s.sid,s.sname,
    (case when cid='01' then score else 0 end) as cid_01,
    (case when cid='02' then score else 0 end) as cid_02,
    (case when cid='03' then score else 0 end) as cid_03
from student s
join sc sc on s.sid=sc.sid;
-- 包括所有学生的语句（包括没有成绩的学生left join）
select s.sid,s.sname,
    (case when cid='01' then score else 0 end) as cid_01,
    (case when cid='02' then score else 0 end) as cid_02,
    (case when cid='03' then score else 0 end) as cid_03
from student s
left join sc sc on s.sid=sc.sid;

-- 问题，上面的每个学生都显示了3行，与要求不符合！
-- 使用汇总语句（sum和max效果一样）这种仅限于一个人一科只有一个成绩，下面是指有成绩的学生
select sc.sid,s.sname,
    max(case when cid='01' then score else 0 end) as cid_01,
    max(case when cid='02' then score else 0 end) as cid_02,
    max(case when cid='03' then score else 0 end) as cid_03
from sc sc
join student s on sc.sid=s.sid
group by sc.sid,s.sname;
-- 下面包括没有成绩的学生
select s.sid,s.sname,
    max(case when cid='01' then score else 0 end) as cid_01,
    max(case when cid='02' then score else 0 end) as cid_02,
    max(case when cid='03' then score else 0 end) as cid_03
from student s
left join sc on s.sid=sc.sid
group by s.sid,s.sname;

-- 方法三、if语句，同case类似
-- 只是有成绩的
select sid,
	sum(if(cid='01',score,0)) as cid_01,
	sum(if(cid='02',score,0)) as cid_02,
	sum(if(cid='03',score,0)) as cid_03
from sc
group by sid;
--所有人
select s.sid,
	sum(if(cid='01',score,0)) as cid_01,
	sum(if(cid='02',score,0)) as cid_02,
	sum(if(cid='03',score,0)) as cid_03
from student s
left join sc on s.sid=sc.sid
group by sid;

-- 方法四、函数group_concat(),很奇妙的函数，行列转换的函数。（这个与要求的不太一样，成绩都在一列中）
select sc.sid,group_concat(`cname`,':',score)
from sc sc
left outer join course c on sc.cid=c.cid
group by sc.sid;
```
##### 45.查询同时存在" 01 "、" 02 "课程的情况,就可以用到上面的写法（原1.1）
```SQL
-- 我习惯的写法
select s.sid,sname,cid_01,cid_02
from student s 
left join (select sid,score as cid_01 from sc where cid='01') c01 on s.sid=c01.sid
left join (select sid,score as cid_02 from sc where cid='02') c02 on s.sid=c02.sid
-- left join (select sid,score as cid_03 from sc where cid='03') c03 on s.sid=c03.sid
where c01.sid is not null and c02.sid is not null;

-- 网上的写法，写法完全没有错误,完全可以这么写，就是相当于两个表内连接
select *
from 
	(select * from sc where sc.cid='01') as t1,
    (select * from sc where sc.cid='02') as t2
where t1.sid=t2.sid

-- 或者这么写
select a.sid,a.score as '01',b.score '02'
from student s
inner join sc as a on s.sid=a.sid
inner join sc as b on s.sid=b.sid
where a.cid='01' and b.cid='02';

-- 下面这么写也对，但很少这么写，太麻烦
select sc.sid,s.sname,
    max(case when cid='01' then score else 0 end) as cid_01,
    max(case when cid='02' then score else 0 end) as cid_02,
    max(case when cid='03' then score else 0 end) as cid_03
from sc sc
join student s on sc.sid=s.sid
group by sc.sid,s.sname
having cid_01<>0 and cid_02<>0;
```
##### 46.查询存在" 01 "课程但可能不存在" 02 "课程的情况(不存在时显示为 null)（原1.2）
```SQL
select a.sid,a.score as score_01,b.score as score_02 
from 
	(select SId ,score from sc where sc.CId='01')as a 
left join 
	(select SId ,score from sc where sc.CId='02')as b on a.SId=b.SId;

-- 另一种写法，用到right join，此时注意表的位置变化
select a.sid,a.score as score_02,b.score as score_01
from 
	(select SId ,score from sc where sc.CId='02')as a 
right join 
	(select SId ,score from sc where sc.CId='01')as b on a.SId=b.SId;
```

#####  47.查询不存在" 01 "课程但存在" 02 "课程的情况，与上面类似，主要是左右连接的区别(原1.3)
```SQL
select b.sid,a.score as score_01,b.score as score_02 
from 
	(select SId ,score from sc where sc.CId='01')as a 
right join 
	(select SId ,score from sc where sc.CId='02')as b on a.SId=b.SId
where a.sid is null;

-- 方法二，我不太喜欢这么写，但这么写是对的（in语句）
select * 
from sc a 
where sid not in(select sid from sc where cid='01')
and cid='02';

-- 网页中指出下面的写法，对吗？
-- 错误写法：这种方法查询出来的结果是不存在“01”且不止存在“02”课程的情况
-- 不是很理解这种写法，也说不明白为啥错？？23.8.31
select * 
from sc a 
inner join sc bon a.sid=b.sid and a.cid='02'
where b.cid not in ('01','02');
```

##### 48.查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩（原35题）
```SQL
-- 题目有歧义，我理解，一个同学，不同课程成绩相同，这个就是子查询中的分类汇总
select sc.sid,sc.cid,sc.score
from sc sc
join (
	select sid,score,count(*)
	from sc
	group by sid,score
	having count(score)>1 ) same_score on sc.sid=same_score.sid and sc.score=same_score.score;
-- 网上部分做法是错误,下面的group by错误，调整为order by才可
select * 
from sc a
inner join sc b on a.sid=b.sid
where a.cid!=b.cid and a.score=b.score
group by a.sid ,a.cid
-- 如下写法才是正确，比如绕脑子
select distinct a.*
from sc a
inner join sc b on a.sid=b.sid
where a.cid!=b.cid and a.score=b.score
order by a.sid,a.cid
```

##### 49.查询" 01 "课程比" 02 "课程成绩高的学生的信息及课程分数（原1题）
```SQL
-- 下面是我喜欢的写法，左连接
-- 我喜欢以所有学生为主表，连接一堆其它表，再筛选出满足条件的行
select s.sid
	,sc1.score as c01
    ,sc2.score as c02
from student s
left join (select sid,score from sc where cid='01') sc1 on s.sid=sc1.sid
left join (select sid,score from sc where cid='02') sc2 on s.sid=sc2.sid
where sc1.score>sc2.score

--网上的写法，只用到2个表，确实比我的写法效率高些
--这个的缺点是，如果该学生没有选02课，是否需要认定其为0呢，也算满足题目条件的话，就无法显示出来了
select sc.sid
  ,sc.score c01
  ,sc2.score c02
from sc sc
join (select sid,score from sc where cid='02') sc2 on sc.sid=sc2.sid
where sc.cid='01' and sc.score>sc2.score;
```
##### 50.查询至少有一门课与学号为" 01 "的同学所学相同的同学的信息（in函数）（原题8）
```SQL
-- 这个01学生选课是几行，怎么让其它同学的学课与这几行一致呢？我的想法是行列转换，把这几行转为列再合并列，最后成为一个值
-- 例子用到的是in()函数
-- 我以为in()函数只针对一个值对应的情况呢？
-- 我理解错了，这个指明至少，表明只有一个能对上就可以了，可以用in()函数

select distinct s.*
from student s
left join sc sc on s.sid=sc.sid
where sc.cid in (select cid from sc where sid='01') and s.sid<>'01'
```
##### 51.查询和" 01 "号的同学学习的课程 完全相同的其他同学的信息（重要）（原题9）
##### 这个才是我上面理解的那些。我使用行列转换的方法，利用下面学到的SQL的函数group_concat()连接字符串的方法
```SQL
-- 我的方法
-- 注意函数group_concat中用`（撇号，而不是单引号，来取该字段的值）
select sid,group_concat(`cid`) as course_all
from sc 
group by sid
having group_concat(`cid`)=(select group_concat(`cid`) from sc where sid='01' group by sid) and sid<>'01';

--网上的用法，他们都参考B站的方法

--思路：
--01号同学学习了01,02,03课程
--1.选出所学的课不在（01,02,03）的同学
--2.剩下的同学肯定选了01，02，03中的某几门，再判断所学课程数是否等于3
--复制下来，运行后发现多一行，没有琢磨
select * 
from student
where sid in (select sid from sc
		where sid!='01'
		group by sid 
        having count(distinct cid)=(select count(distinct cid) from sc where sid='01')
		)
		and 
		sid not in (
			select sid from sc where cid not in (
				select cid from sc where sid='01')
)
```
##### 排名
##### 52.按各科成绩进行排序，并显示排名， Score 重复时保留名次空缺（原15题）
##### 53.查询学生的总成绩，并进行排名，总分重复时保留名次空缺（原16题，没做）
```SQL
--- 排名不知道该咋弄！！
select cid,score,sid
from sc
order by cid,score desc,sid

-- 网上实现方法，很巧妙，我根本想不到
-- 
select a.*, count(b.score)+1 as pm
from sc a 
left join sc as b on a.score<b.score and a.cid = b.cid
group by a.cid, a.sid,a.score
order by a.cid, pm;


--法二：SQL8.0及以上版本使用窗口函数进行排名。
select *, rank() over(partition by sc.cid order by sc.score desc)排名
from sc;
```

##### 54.查询各科成绩前三名的记录（原18题）
##### 55.查询每门功成绩最好的前两名(原36，类似18)

##### 方法一、考察子查询（使用最多的方法）
```SQL
-- 这个我以前会分成3部分（共3个学科所以分3部分）查询，然后再捏合到一起union
-- 想简单了，这个每个部分需要order by，取前3行（这里用limit），再union，但是sql不认这样的写法！！！
-- 得换个思路了
-- 方法一、使用子查询
-- https://zhuanlan.zhihu.com/p/106407619
-- https://blog.csdn.net/and52696686/article/details/107591245

-- 先理解子查询，不设置条件，下面这个语句便于理解
select s1.*,(select count(1) from sc s2 where s1.cid=s2.cid and s1.score<s2.score) as count_score
from sc s1
order by s1.cid,s1.score desc
```
理解图示如下：
![连接图示](子查询.jpg)
```SQL
-- 如果需要满足条件为前3个，需要放入where语句，如下所示
select s1.*,(select count(1) from sc s2 where s1.cid=s2.cid and s1.score<s2.score) as count_score
from sc s1
where (select count(1) from sc s2 where s1.cid=s2.cid and s1.score<s2.score)<3
order by s1.cid,s1.score desc
```
```SQL
-- 如果分数一样成绩并列怎么办？
-- 在上面例子基础上，使用distinct关键字对表s2中存在多名同学分数相同情况下**去重**，从而达到并列排名的目的。
select s1.sid,s1.cid,s1.score,count(1)
from sc s1
left join (select distinct cid,score from sc) s2 on s1.cid=s2.cid and s1.score<s2.score
group by s1.sid,s1.cid,s1.score
having count(1)<3
order by s1.cid ,s1.score desc
```
##### 突然冒出来一个COUNT(1),是使用mssql从来没有用过的
-- https://www.cnblogs.com/hider/p/11726690.html
这里总结得很清楚
###### `COUNT()`函数的用法，主要用于统计表行数。主要用法有：`COUNT(*)`、`COUNT(字段)`和`COUNT(常量)（如COUNT(1))`。
- `COUNT(字段)`，表示，对全表进行扫描，返回该字段非NULL的行数。结果是一个BIGINT,如果查询结果没有命中任何记录，返回0
- `COUNT(*)`、`COUNT(常量)`表示的是，对全表进行扫描，统计符合条件记录的行数，包含值为NULL的记录。

因为`COUNT(*)`是SQL92定义的标准统计行数的语法，所以SQL对他进行了很多优化，MyISAM中会直接把表的总行数单独记录下来供`COUNT(*)`查询，而InnoDB则会在扫表的时候选择最小的索引来降低成本。当然，这些优化的前提都是没有进行where和group的条件查询。(MyISAM及InnoDB是SQL的数据库引擎，我们安装的SQL WorkBench的引擎是InnoDB。)

在InnoDB中COUNT(*)和COUNT(1)实现上没有区别，而且效率一样（都包括NULL的行），但是COUNT(字段)需要进行字段的非NULL判断，所以效率会低一些。

因为COUNT(*)是SQL92定义的标准统计行数的语法，并且效率高，所以请直接使用COUNT(*)查询表的行数！

先了解这么多。

##### 方法二、使用加行号的方式查询（SQL无法实现，MSSql能实现）
```SQL
-- https://blog.csdn.net/weixin_46270651/article/details/107133566
-- 注意：SQL语句中【:=】表示赋值，【=】表示比较，【@】表示变量
-- 方法1
set @rownum:=0;  
select sc.*,@rownum:=@rownum+1 as rownum
from sc sc
where cid='01'
order by score desc;

-- 方法2,声明变量@rownum并赋初始值0放在from里，结果与上面的方法一样
-- 用limit取前3个（SQL没有top n语法）
select sc.*,@rownum:=@rownum+1 as rownum
from sc sc,(select @rownum:=0) a
where cid='01'
order by score desc
limit 3;

-- 如果取三科成绩前3名，仍是无法使用union，会出现错误提示，只能用join，变成行列列转换的格式了，不是题目要求了

```
不过，微软的sql能实现
```mssql
-- 语法
-- https://learn.microsoft.com/zh-cn/sql/t-sql/functions/row-number-transact-sql?view=sql-server-ver16
-- 表示行数的函数为ROW_NUMBER()，下面的函数在ms sql 2008测试通过
-- 后来发现SQL中也有此函数，但是没有top3函数，还是不能这么写
select top 3 ROW_NUMBER() OVER(ORDER BY score DESC) AS Row#,cid,score
from sc
where cid='01'
union all
select top 3 ROW_NUMBER() OVER(ORDER BY score DESC) AS Row#,cid,score
from sc
where cid='02'
order by cid,score desc
```
##### 方法三、使用SQL8.0以上的函数
##### rank()函数考虑并列，如果并列第1有2个，那么下一个顺序就为3
##### dense_rank()函数(dense稠密的意思)
```SQL
select *
from
(select sid,cid,score,rank() over (partition by cid order by score desc) ranks from sc)s
where ranks<4;

-- 把上面的函数修改一下即可
select *
from
(select sid,cid,score,dense_rank() over (partition by cid order by score desc) ranks from sc)s
where ranks<4;

```


