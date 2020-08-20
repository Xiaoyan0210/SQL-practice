## 建表

```sql
-- 学生表
-- Student（s_id，s_name，s_birth，s_sex）
-- 学生编号,学生姓名, 出生年月,学生性别
CREATE TABLE `Student`(
`s_id` VARCHAR(20),
`s_name` VARCHAR(20) NOT NULL DEFAULT '',
`s_birth` VARCHAR(20) NOT NULL DEFAULT '',
`s_sex` VARCHAR(10) NOT NULL DEFAULT '',
PRIMARY KEY(`s_id`)
);
-- 课程表
-- Course(c_id,c_name,t_id) 
-- 课程编号, 课程名称, 教师编号
CREATE TABLE `Course`(
`c_id` VARCHAR(20),
`c_name` VARCHAR(20) NOT NULL DEFAULT '',
`t_id` VARCHAR(20) NOT NULL,
PRIMARY KEY(`c_id`)
);
-- 教师表
-- Teacher(t_id,t_name)
-- 教师编号,教师姓名
CREATE TABLE `Teacher`(
`t_id` VARCHAR(20),
`t_name` VARCHAR(20) NOT NULL DEFAULT '',
PRIMARY KEY(`t_id`)
);
-- 成绩表
-- Score(s_id,c_id,s_s_score) 
-- 学生编号,课程编号,分数
CREATE TABLE `Score`(
`s_id` VARCHAR(20),
`c_id` VARCHAR(20),
`s_score` INT(3),
PRIMARY KEY(`s_id`,`c_id`)
)
```

## 插入数据
```sql
-- 学生表数据
insert into Student values('01' , '赵雷' , '1990-01-01' , '男');
insert into Student values('02' , '钱电' , '1990-12-21' , '男');
insert into Student values('03' , '孙风' , '1990-05-20' , '男');
insert into Student values('04' , '李云' , '1990-08-06' , '男');
insert into Student values('05' , '周梅' , '1991-12-01' , '女');
insert into Student values('06' , '吴兰' , '1992-03-01' , '女');
insert into Student values('07' , '郑竹' , '1989-07-01' , '女');
insert into Student values('08' , '王菊' , '1990-01-20' , '女');

-- 插入课程表测试数据
insert into Course values('01' , '语文' , '102');
insert into Course values('02' , '数学' , '101');
insert into Course values('03' , '英语' , '103');
SELECT * FROM Course

-- 教师表测试数据
insert into Teacher values('101' , '教一');
insert into Teacher values('102' , '教二');
insert into Teacher values('103' , '教三');
SELECT * FROM Teacher

-- 成绩表测试数据
insert into Score values('1001' , '01' , 80);
insert into Score values('1001' , '02' , 90);
insert into Score values('1001' , '03' , 99);
insert into Score values('1002' , '01' , 70);
insert into Score values('1002' , '02' , 60);
insert into Score values('1002' , '03' , 80);
insert into Score values('1003' , '01' , 80);
insert into Score values('1003' , '02' , 80);
insert into Score values('1003' , '03' , 80);
insert into Score values('1004' , '01' , 50);
insert into Score values('1004' , '02' , 30);
insert into Score values('1004' , '03' , 20);
insert into Score values('1005' , '01' , 76);
insert into Score values('1005' , '02' , 87);
insert into Score values('1006' , '01' , 31);
insert into Score values('1006' , '03' , 34);
insert into Score values('1007' , '02' , 89);
insert into Score values('1007' , '03' , 98);
SELECT * FROM Score
```

# 练习题目
## 1、查询"01"课程比"02"课程成绩高的学生的信息及课程分数
```sql
SELECT s.*, A.s_score AS score_1,B.s_score AS score_2
FROM student s,(select s_score, s_id from score where c_id = 01) AS A, (select s_score, s_id from score where c_id = 02) AS B
WHERE A.s_score >B.s_score 
AND A.s_id= s.s_id
AND B.s_id = s.s_id
```
## 2、查询"01"课程比"02"课程成绩低的学生的信息及课程分数
```sql
SELECT s.*, A.s_score AS score_1, B.s_score AS score_2
From student s,(select s_score, s_id from score where c_id =01) AS A, (select s_score, s_id from score where c_id=02)as B
where  A.s_score < B.s_score
and a.s_id = s.s_id
and b.s_id = s.s_id
```

## 3、查询平均成绩大于60分的学生的学号和平均成绩
```sql
select s.s_id, new.avger
from (select s_id, avg(s_score) as avger from score group by s_id) as new, student s
where new.s_id = s.s_id
and new.avger > 60
group by s.s_id
```

## 4、查询平均成绩小于60分的同学的学生编号和学生姓名和平均成绩
```sql
select s.s_id,s.s_name, avg(sc.s_score) as avger
from student s left join score sc
on s.s_id = sc.s_id
group by s.s_id
having avg(sc.s_score) <60 or avg(sc.s_score) is NULL
```

## 5、查询所有学生的学号、姓名、选课数、总成绩
```sql
SELECT s.s_id,s.s_name, COUNT(distinct sc.c_id) as num, sum(sc.s_score) as sumscore
from student s, score sc
where s.s_id = sc.s_id
group by s.s_id
```

## 6、查询名字有“一”的老师的个数
```sql
select t_name
from teacher
where t_name like '%一%'
```

## 7、查询没学过“教一”老师课的学生的学号、姓名
```sql
SELECT * 
from student
where s_id not in (select sc.s_id from teacher t, course c,score sc where t.t_id = c.t_id and c.c_id = sc.c_id and t.t_name = '教一') 
```

## 8、查询学过“教一”老师所教的所有课的同学的学号、姓名
```sql
SELECT * 
from student
where s_id in (select sc.s_id from teacher t, course c,score sc where t.t_id = c.t_id and c.c_id = sc.c_id and t.t_name = '教一') 
```

## 9、查询学过编号为“01”的课程并且也学过编号为“02”的课程的学生的学号、姓名
```sql
select *
from student
where s_id in (select s_id from score where c_id = 01) 
and s_id in (select s_id from score where c_id = 02)
```

## 10、查询学过编号为"01"但是没有学过编号为"02"的课程的同学的信息
```sql
select * 
from student
where s_id in (select s_id from score where c_id = 01) 
and s_id not in (select s_id from score where c_id = 02)
```

## 11、查询没有学全所有课程的同学的信息
```sql
select s.*
from score sc, student s
where sc.s_id = s.s_id
group by s.s_id
having count(distinct sc.c_id) != (select count(c_id) from course)
```

## 12、查询至少有一门课与学号为"01"的同学所学相同的同学的信息
```sql
select s.*
from score sc, student s
where sc.s_id = s.s_id
and sc.c_id in (select c_id from score where s_id = 1001)
group by s_id
```

## 13、查询和"1001"号的同学学习的课程完全相同的其他同学的信息
```sql
select s.*
from score sc, student s
where sc.s_id = s.s_id
and sc.c_id in (select c_id from score where s_id = 1001)
group by s_id
having count(*) = (select count(*) from score where s_id = 1001)
```

## 14、查询没学过"教三"老师讲授的任一门课程的学生姓名
```sql
SELECT * 
from student
where s_id not in (select sc.s_id from teacher t, course c,score sc where t.t_id = c.t_id and c.c_id = sc.c_id and t.t_name = '教三') 
```

## 15、查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩
```sql
select s.*, new.avgs
from (select s_id, avg(s_score) as avgs from score where s_score < 60 group by s_id having count(c_id) >=2) as new, student s
where new.s_id = s.s_id
group by s.s_id
```

## 16、检索"01"课程分数小于60，按分数降序排列的学生信息
```sql
select s.*, new.s_score
from (select s_id,s_score from score where s_score < 60 and c_id = 01) as new, student s
where new.s_id = s.s_id
order by new.s_score desc 
```

## ※17、按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩 
### 必须要加上s_id = score_s_id才可以
```sql
select score.s_id as score_s_id, 
(select s_score from score where c_id = 01 and s_id = score_s_id) as '语文',
(select s_score from score where c_id = 02 and s_id = score_s_id) as '数学',
(select s_score from score where c_id = 03 and s_id = score_s_id) as '英语', 
avg(score.s_score) as avgsc
from score
group by score.s_id
order by avg(score.s_score) desc
```

## 18、查询各科成绩最高分，最低分和平均分：以如下形式显示：课程ID，课程name，最高分，最低分，平均分，及格率，中等率，优良率，优秀率，及格为>=60，中等为：70-80，优良为：80-90，优秀为：>=90
### 还可用case when
```sql
select c.c_id, c.c_name, 
max(sc.s_score) as '最高分', 
min(sc.s_score) as '最低分', 
avg(sc.s_score) as '平均分', 
(select count(*) from score where s_score >=60 and score.c_id = c.c_id)/(select count(*) from score where score.c_id = c.c_id) as '及格率',
(select count(*) from score where s_score >=70 and s_score <80 and score.c_id = c.c_id)/(select count(*) from score where score.c_id = c.c_id) as '优良率',
(select count(*) from score where s_score >=80 and s_score <90 and score.c_id = c.c_id)/(select count(*) from score where score.c_id = c.c_id) as '优秀率'
from course c, score sc
where c.c_id =  sc.c_id
group by c.c_id
```

## ※19、按各科成绩进行排序，并显示排名
### 使用rank variable
### set @xx := num 定义
```sql
set @rank:=0;
select sc.s_id, sc.s_score, @rank:=@rank+1
from score sc
order by sc.s_score desc
```

## ※20、查询学生的总成绩并进行排名
```sql
set @rank:=0;
select r.s_id, r.summ, @rank:=@rank+1
from (select sc.s_id, sum(sc.s_score) as summ
from score sc
group by sc.s_id
order by sum(sc.s_score) desc) as r  #先计算学生总成绩并以倒序排序后再使用Rank，否则会出现先rank再order by的情况
```

## 21、查询不同老师所教不同课程平均分从高到低显示
```sql
select avg(sc.s_score) as avgs, sc.c_id, c.t_id
from score sc, course c
where sc.c_id = c.c_id
group by c.t_id
order by avgs desc
```

## 22、查询所有课程的成绩第2名到第3名的学生信息及该课程成绩
```sql
select s.*, r.rank1, r.c_id, r.s_score
FROM (select sc.s_id, sc.s_score,sc.c_id, @rank1:=@rank1+1 as rank1
from score sc, (select @rank1:=0) k
where c_id = 01
order by sc.c_id, sc.s_score desc) r, student s
where r.s_id = s.s_id 
and rank1 in (2,3)
union
select s.*, r.rank2, r.c_id, r.s_score
FROM (select sc.s_id, sc.s_score,sc.c_id, @rank2:=@rank2+1 as rank2
from score sc, (select @rank2 := 0) h
where c_id = 02
order by sc.c_id, sc.s_score desc) r, student s
where r.s_id = s.s_id 
and rank2 in (2,3)
union
select s.*, r.rank3, r.c_id, r.s_score
FROM (select sc.s_id, sc.s_score,sc.c_id, @rank3:=@rank3+1 as rank3
from score sc, (select @rank3 := 0) h
where c_id = 03
order by sc.c_id, sc.s_score desc) r, student s
where r.s_id = s.s_id 
and rank3 in (2,3)
```

## 23、统计各科成绩各分数段人数：课程编号,课程名称,[100-85],[85-70],[70-60],[0-60]及所占百分比
```sql
select score.c_id, c.c_name, 
(sum(case when score.s_score >=85 then 1 else 0 end))/(sum(case when score.s_score then 1 else 0 end)) as '85-100',
(sum(case when score.s_score < 85 and score.s_score >=70 then 1 else 0 end))/(sum(case when score.s_score then 1 else 0 end)) as '70-85',
(sum(case when score.s_score < 70 and score.s_score >=60 then 1 else 0 end))/(sum(case when score.s_score then 1 else 0 end)) as '60-70',
(sum(case when score.s_score < 60 then 1 else 0 end))/(sum(case when score.s_score then 1 else 0 end)) as '0-60'
from score, course c
where score.c_id = c.c_id
group by c.c_id
```

## 24、查询学生平均成绩及其名次
```sql
set @rank:=0;
select new.s_id, new.s_name, new.s_sex,new.s_birth,new.avgsc, (@rank:=@rank+1)as rank1
from (select s.*, sc.c_id,avg(sc.s_score)as avgsc from score sc, student s where sc.s_id = s.s_id group by sc.s_id order by avg(sc.s_score) desc ) as new
```

## 25、查询各科成绩前三名的记录
```sql
(select sc.s_score, sc.c_id, s.* from score sc, student s where sc.s_id = s.s_id and c_id =01 order by s_score desc limit 3)
union all
(select sc.s_score, sc.c_id, s.* from score sc, student s where sc.s_id = s.s_id and c_id =02 order by s_score desc limit 3)
union all
(select sc.s_score, sc.c_id, s.* from score sc, student s where sc.s_id = s.s_id and c_id =03 order by s_score desc limit 3)
```

## 26、查询每门课程被选修的学生数
```sql
select count(c_id), c_id
from score
group by c_id
```

## 27、查询出只有两门课程的全部学生的学号和姓名
```sql
select *
from student
where s_id in (select s_id from score group by s_id having count(c_id)=2)
```

## 28、查询男生、女生人数
```sql
select s_sex, count(s_id)
from student
group by s_sex
```

## 29、查询名字中含有"风"字的学生信息
```sql
select *
from student
where s_name like '%风%'
```

## 30、查询同名同性学生名单，并统计同名人数
```sql
select a.s_id, a.s_name
from student a, student b
where a.s_id != b.s_id 
and a.s_name = b.s_name
```

## 31、查询1990年出生的学生名单
```sql
select *
from student
where s_birth like '1990%'
-- 
SELECT * FROM student
WHERE YEAR(s_birth)=1990
```

## 32、查询每门课程的平均成绩，结果按平均成绩降序排列，平均成绩相同时，按课程编号升序排列
```sql
select c_id, avg(s_score)
from score
group by c_id
order by avg(s_score) desc,c_id 
```

## 33、查询平均成绩大于等于85的所有学生的学号、姓名和平均成绩
```sql
select s.*, avg(sc.s_score)
from student s, score sc
where s.s_id = sc.s_id
group by s.s_id
having avg(sc.s_score) > 85
```

## 34、查询课程名称为"数学"，且分数低于60的学生姓名和分数
```sql
select s.s_name, sc.s_score
from course c, student s, score sc
where c.c_id = sc.c_id
and s.s_id = sc.s_id
and c.c_name = '数学'
and sc.s_score < 60
```

## 35、查询所有学生的课程及分数情况；
```sql
select s.s_id, s.s_name,
(select s_score from score where s.s_id = score.s_id and c_id = 01) as '语文',
(select s_score from score where s.s_id = score.s_id and c_id = 02) as '数学',
(select s_score from score where s.s_id = score.s_id and c_id = 03) as '英语'
from student s
group by s.s_id
```

## 36、查询任何一门课程成绩在70分以上的姓名、课程名称和分数；
```sql
select s.s_name, c.c_name, sc.s_score
from score sc, student s, course c
where s.s_id = sc.s_id 
and sc.c_id = c.c_id
and sc.s_score > 70
```

## 37、查询不及格的课程
```sql
select sc.c_id, c.c_name, sc.s_score
from score sc, course c
where sc.c_id = c.c_id
AND sc.s_score < 60
```

## 38、查询课程编号为01且课程成绩在80分以上的学生的学号和姓名
```sql
SELECT s.s_id, s.s_name, sc.s_score, sc.c_id
from score sc, student s
where sc.s_id = s.s_id
and sc.c_id = 01 
and sc.s_score >= 80
```

## 39、求每门课程的学生人数
```sql
select c_id, count(s_id)
from score
group by c_id
```

## 40、查询选修"张三"老师所授课程的学生中，成绩最高的学生信息及其成绩
```sql
select s.*, max(sc.s_score)
from score sc, student s, teacher t, course c
where sc.s_id = s.s_id
and sc.c_id = c.c_id
and t.t_id = c.t_id
and t.t_name = '教一'
```

## 41、查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩
```sql
select a.s_score, a.c_id, b.c_id, a.s_id, b.s_id
from (select s_score, c_id, s_id from score) a, (select s_score, c_id, s_id from score) b
where a.s_id !=b.s_id
and a.s_score = b.s_score
```

## 42、查询每门功成绩最好的前两名
```sql
(select s_id, s_score, c_id from score where c_id = 01 order by s_score desc limit 2)
union
(select s_id, s_score, c_id from score where c_id = 02 order by s_score desc limit 2)
union
(select s_id, s_score, c_id from score where c_id = 03 order by s_score desc limit 2)
```

## 43、统计每门课程的学生选修人数（超过5人的课程才统计）。要求输出课程号和选修人数，查询结果按人数降序排列，若人数相同，按课程号升序排列
```sql
SELECT c_id, count(s_id) as number
from score
group by c_id
having COUNT(s_id) > 5
order by count(s_id) desc, c_id
```

## 44、检索至少选修两门课程的学生学号
```sql
select s_id, count(c_id)
from score
group by s_id
having count(c_id) >= 2
```

## 45、查询选修了全部课程的学生信息
```sql
select sc.s_id, count(sc.c_id),s.*
from score sc, student s
where sc.s_id = s.s_id
group by sc.s_id
having count(sc.c_id) = (select count(*) from course)
```

## ※46、查询各学生的年龄 
### 用datediff()
```sql
SELECT s_id, s_birth, DATEDIFF('2020-8-15', s_birth)/365 
FROM student
```

## ※47、查询本周过生日的学生
```sql
SELECT s_name,s_birth 
from student
where week(s_birth) = WEEK(date(NOW()))
```

## ※48、查询下周过生日的学生
```sql
SELECT s_name,s_birth 
from student
WHERE WEEK(DATE_FORMAT(NOW(),'%Y%m%d')+1) = WEEK(s_birth)
```

## 49、查询本月过生日的学生
```sql
SELECT * 
FROM student 
WHERE MONTH(s_birth) = MONTH(date(NOW()))%12
```

## 50、查询下月过生日的学生
```sql
SELECT * 
FROM student 
WHERE MONTH(s_birth) = MONTH(date(NOW()))%12+1
```
