## [sql面试题（学生表_课程表_成绩表_教师表）](http://www.cnblogs.com/a-dou/articles/5489772.html)

原帖链接:http://bbs.csdn.net/topics/280002741

表架构

Student(S#,Sname,Sage,Ssex) 学生表 
Course(C#,Cname,T#) 课程表 
SC(S#,C#,score) 成绩表 
Teacher(T#,Tname) 教师表

建表语句 





```
 1 CREATE TABLE student 
 2   ( 
 3      s#    INT, 
 4      sname nvarchar(32), 
 5      sage  INT, 
 6      ssex  nvarchar(8) 
 7   ) 
 8 
 9 CREATE TABLE course 
10   ( 
11      c#    INT, 
12      cname nvarchar(32), 
13      t#    INT 
14   ) 
15 
16 CREATE TABLE sc 
17   ( 
18      s#    INT, 
19      c#    INT, 
20      score INT 
21   ) 
22 
23 CREATE TABLE teacher 
24   ( 
25      t#    INT, 
26      tname nvarchar(16) 
27   ) 
```





插入测试数据语句 





```
 1 insert into Student select 1,N'刘一',18,N'男' union all
 2  select 2,N'钱二',19,N'女' union all
 3  select 3,N'张三',17,N'男' union all
 4  select 4,N'李四',18,N'女' union all
 5  select 5,N'王五',17,N'男' union all
 6  select 6,N'赵六',19,N'女' 
 7  
 8  insert into Teacher select 1,N'叶平' union all
 9  select 2,N'贺高' union all
10  select 3,N'杨艳' union all
11  select 4,N'周磊'
12  
13  insert into Course select 1,N'语文',1 union all
14  select 2,N'数学',2 union all
15  select 3,N'英语',3 union all
16  select 4,N'物理',4
17  
18  insert into SC 
19  select 1,1,56 union all 
20  select 1,2,78 union all 
21  select 1,3,67 union all 
22  select 1,4,58 union all 
23  select 2,1,79 union all 
24  select 2,2,81 union all 
25  select 2,3,92 union all 
26  select 2,4,68 union all 
27  select 3,1,91 union all 
28  select 3,2,47 union all 
29  select 3,3,88 union all 
30  select 3,4,56 union all 
31  select 4,2,88 union all 
32  select 4,3,90 union all 
33  select 4,4,93 union all 
34  select 5,1,46 union all 
35  select 5,3,78 union all 
36  select 5,4,53 union all 
37  select 6,1,35 union all 
38  select 6,2,68 union all 
39  select 6,4,71
```





问题





```sql
--问题： 
--1、查询“001”课程比“002”课程成绩高的所有学生的学号； 
  select a.S# from (select s#,score from SC where C#='001') a,(select s#,score 
  from SC where C#='002') b 
  where a.score>b.score and a.s#=b.s#; 

--2、查询平均成绩大于60分的同学的学号和平均成绩； 
    select S#,avg(score) 
    from sc 
    group by S# having avg(score) >60; 

--3、查询所有同学的学号、姓名、选课数、总成绩； 
  select Student.S#,Student.Sname,count(SC.C#),sum(score) 
  from Student left Outer join SC on Student.S#=SC.S# 
  group by Student.S#,Sname 

--4、查询姓“李”的老师的个数； 
  select count(distinct(Tname)) 
  from Teacher 
  where Tname like '李%'; 

--5、查询没学过“叶平”老师课的同学的学号、姓名； 
    select Student.S#,Student.Sname 
    from Student  
    where S# not in (select distinct( SC.S#) from SC,Course,Teacher where  SC.C#=Course.C# and Teacher.T#=Course.T# and Teacher.Tname='叶平'); 

--6、查询学过“001”并且也学过编号“002”课程的同学的学号、姓名； 
  select Student.S#,Student.Sname 
  from Student,SC 
  where Student.S#=SC.S# and SC.C#='001'
  and exists( Select * from SC as SC_2 where SC_2.S#=SC.S# and SC_2.C#='002'); 

--7、查询学过“叶平”老师所教的所有课的同学的学号、姓名； 
  select S#,Sname 
  from Student 
  where S# in 
  (
      select S# 
      from SC ,Course ,Teacher 
      where SC.C#=Course.C# and Teacher.T#=Course.T# and Teacher.Tname='叶平' 
      group by S# 
      having count(SC.C#)=(select count(C#) from Course,Teacher  where Teacher.T#=Course.T# and Tname='叶平')
  ); 

--8、查询课程编号“002”的成绩比课程编号“001”课程低的所有同学的学号、姓名； 
  Select S#,Sname from (select Student.S#,Student.Sname,score ,(select score from SC SC_2 where SC_2.S#=Student.S# and SC_2.C#='002') score2 
  from Student,SC where Student.S#=SC.S# and C#='001') S_2 where score2 <score; 

--9、查询所有课程成绩小于60分的同学的学号、姓名； 
  select S#,Sname 
  from Student 
  where S# not in (select S.S# from Student AS S,SC where S.S#=SC.S# and score>60); 

--10、查询没有学全所有课的同学的学号、姓名； 
    select Student.S#,Student.Sname 
    from Student,SC 
    where Student.S#=SC.S# group by  Student.S#,Student.Sname having count(C#) <(select count(C#) from Course); 
--11、查询至少有一门课与学号为“1001”的同学所学相同的同学的学号和姓名； 
    select distinct S#,Sname from Student,SC where Student.S#=SC.S# and SC.C# in (select C# from SC where S#='1001'); 
--12、查询至少学过学号为“001”同学所有一门课的其他同学学号和姓名； 
    select distinct SC.S#,Sname 
    from Student,SC 
    where Student.S#=SC.S# and C# in (select C# from SC where S#='001'); 
--13、把“SC”表中“叶平”老师教的课的成绩都更改为此课程的平均成绩； 
    update SC set score=(select avg(SC_2.score) 
    from SC SC_2 
    where SC_2.C#=SC.C# ) from Course,Teacher where Course.C#=SC.C# and Course.T#=Teacher.T# and Teacher.Tname='叶平'); 
--14、查询和“1002”号的同学学习的课程完全相同的其他同学学号和姓名； 
    select S# from SC where C# in (select C# from SC where S#='1002') 
    group by S# having count(*)=(select count(*) from SC where S#='1002'); 
--15、删除学习“叶平”老师课的SC表记录； 
    Delect SC 
    from course ,Teacher  
    where Course.C#=SC.C# and Course.T#= Teacher.T# and Tname='叶平'; 
--16、向SC表中插入一些记录，这些记录要求符合以下条件：没有上过编号“003”课程的同学学号、2、号课的平均成绩； 
    Insert SC select S#,'002',(Select avg(score) 
    from SC where C#='002') from Student where S# not in (Select S# from SC where C#='002'); 
--17、按平均成绩从高到低显示所有学生的“数据库”、“企业管理”、“英语”三门的课程成绩，按如下形式显示： 学生ID,,数据库,企业管理,英语,有效课程数,有效平均分 
    SELECT S# as 学生ID 
        ,(SELECT score FROM SC WHERE SC.S#=t.S# AND C#='004') AS 数据库 
        ,(SELECT score FROM SC WHERE SC.S#=t.S# AND C#='001') AS 企业管理 
        ,(SELECT score FROM SC WHERE SC.S#=t.S# AND C#='006') AS 英语 
        ,COUNT(*) AS 有效课程数, AVG(t.score) AS 平均成绩 
    FROM SC AS t 
    GROUP BY S# 
    ORDER BY avg(t.score)  
--18、查询各科成绩最高和最低的分：以如下形式显示：课程ID，最高分，最低分 
    SELECT L.C# As 课程ID,L.score AS 最高分,R.score AS 最低分 
    FROM SC L ,SC AS R 
    WHERE L.C# = R.C# and 
        L.score = (SELECT MAX(IL.score) 
                      FROM SC AS IL,Student AS IM 
                      WHERE L.C# = IL.C# and IM.S#=IL.S# 
                      GROUP BY IL.C#) 
        AND 
        R.Score = (SELECT MIN(IR.score) 
                      FROM SC AS IR 
                      WHERE R.C# = IR.C# 
                  GROUP BY IR.C# 
                    ); 
--自己写的:select c# ,max(score)as 最高分 ,min(score) as 最低分 from dbo.sc  group by c#
--19、按各科平均成绩从低到高和及格率的百分数从高到低顺序 
    SELECT t.C# AS 课程号,max(course.Cname)AS 课程名,isnull(AVG(score),0) AS 平均成绩 
        ,100 * SUM(CASE WHEN  isnull(score,0)>=60 THEN 1 ELSE 0 END)/COUNT(*) AS 及格百分数 
    FROM SC T,Course 
    where t.C#=course.C# 
    GROUP BY t.C# 
    ORDER BY 100 * SUM(CASE WHEN  isnull(score,0)>=60 THEN 1 ELSE 0 END)/COUNT(*) DESC 
--20、查询如下课程平均成绩和及格率的百分数(用"1行"显示): 企业管理（001），马克思（002），OO&UML （003），数据库（004） 
    SELECT SUM(CASE WHEN C# ='001' THEN score ELSE 0 END)/SUM(CASE C# WHEN '001' THEN 1 ELSE 0 END) AS 企业管理平均分 
        ,100 * SUM(CASE WHEN C# = '001' AND score >= 60 THEN 1 ELSE 0 END)/SUM(CASE WHEN C# = '001' THEN 1 ELSE 0 END) AS 企业管理及格百分数 
        ,SUM(CASE WHEN C# = '002' THEN score ELSE 0 END)/SUM(CASE C# WHEN '002' THEN 1 ELSE 0 END) AS 马克思平均分 
        ,100 * SUM(CASE WHEN C# = '002' AND score >= 60 THEN 1 ELSE 0 END)/SUM(CASE WHEN C# = '002' THEN 1 ELSE 0 END) AS 马克思及格百分数 
        ,SUM(CASE WHEN C# = '003' THEN score ELSE 0 END)/SUM(CASE C# WHEN '003' THEN 1 ELSE 0 END) AS UML平均分 
        ,100 * SUM(CASE WHEN C# = '003' AND score >= 60 THEN 1 ELSE 0 END)/SUM(CASE WHEN C# = '003' THEN 1 ELSE 0 END) AS UML及格百分数 
        ,SUM(CASE WHEN C# = '004' THEN score ELSE 0 END)/SUM(CASE C# WHEN '004' THEN 1 ELSE 0 END) AS 数据库平均分 
        ,100 * SUM(CASE WHEN C# = '004' AND score >= 60 THEN 1 ELSE 0 END)/SUM(CASE WHEN C# = '004' THEN 1 ELSE 0 END) AS 数据库及格百分数 
  FROM SC 
```









```mssql
  1 --21、查询不同老师所教不同课程平均分从高到低显示 
  2   SELECT max(Z.T#) AS 教师ID,MAX(Z.Tname) AS 教师姓名,C.C# AS 课程ＩＤ,MAX(C.Cname) AS 课程名称,AVG(Score) AS 平均成绩 
  3     FROM SC AS T,Course AS C ,Teacher AS Z 
  4     where T.C#=C.C# and C.T#=Z.T# 
  5   GROUP BY C.C# 
  6   ORDER BY AVG(Score) DESC 
  7 
  8 --22、查询如下课程成绩第 3 名到第 6 名的学生成绩单：企业管理（001），马克思（002），UML （003），数据库（004） 
  9     [学生ID],[学生姓名],企业管理,马克思,UML,数据库,平均成绩 
 10     SELECT  DISTINCT top 3 
 11       SC.S# As 学生学号, 
 12         Student.Sname AS 学生姓名 , 
 13       T1.score AS 企业管理, 
 14       T2.score AS 马克思, 
 15       T3.score AS UML, 
 16       T4.score AS 数据库, 
 17       ISNULL(T1.score,0) + ISNULL(T2.score,0) + ISNULL(T3.score,0) + ISNULL(T4.score,0) as 总分 
 18       FROM Student,SC  LEFT JOIN SC AS T1 
 19                       ON SC.S# = T1.S# AND T1.C# = '001' 
 20             LEFT JOIN SC AS T2 
 21                       ON SC.S# = T2.S# AND T2.C# = '002' 
 22             LEFT JOIN SC AS T3 
 23                       ON SC.S# = T3.S# AND T3.C# = '003' 
 24             LEFT JOIN SC AS T4 
 25                       ON SC.S# = T4.S# AND T4.C# = '004' 
 26       WHERE student.S#=SC.S# and 
 27       ISNULL(T1.score,0) + ISNULL(T2.score,0) + ISNULL(T3.score,0) + ISNULL(T4.score,0) 
 28       NOT IN 
 29       (SELECT 
 30             DISTINCT 
 31             TOP 15 WITH TIES 
 32             ISNULL(T1.score,0) + ISNULL(T2.score,0) + ISNULL(T3.score,0) + ISNULL(T4.score,0) 
 33       FROM sc 
 34             LEFT JOIN sc AS T1 
 35                       ON sc.S# = T1.S# AND T1.C# = 'k1' 
 36             LEFT JOIN sc AS T2 
 37                       ON sc.S# = T2.S# AND T2.C# = 'k2' 
 38             LEFT JOIN sc AS T3 
 39                       ON sc.S# = T3.S# AND T3.C# = 'k3' 
 40             LEFT JOIN sc AS T4 
 41                       ON sc.S# = T4.S# AND T4.C# = 'k4' 
 42       ORDER BY ISNULL(T1.score,0) + ISNULL(T2.score,0) + ISNULL(T3.score,0) + ISNULL(T4.score,0) DESC); 
 43 
 44 --23、统计列印各科成绩,各分数段人数:课程ID,课程名称,[100-85],[85-70],[70-60],[ <60] 
 45     SELECT SC.C# as 课程ID, Cname as 课程名称 
 46         ,SUM(CASE WHEN score BETWEEN 85 AND 100 THEN 1 ELSE 0 END) AS [100 - 85] 
 47         ,SUM(CASE WHEN score BETWEEN 70 AND 85 THEN 1 ELSE 0 END) AS [85 - 70] 
 48         ,SUM(CASE WHEN score BETWEEN 60 AND 70 THEN 1 ELSE 0 END) AS [70 - 60] 
 49         ,SUM(CASE WHEN score < 60 THEN 1 ELSE 0 END) AS [60 -] 
 50     FROM SC,Course 
 51     where SC.C#=Course.C# 
 52     GROUP BY SC.C#,Cname; 
 53 
 54 --24、查询学生平均成绩及其名次 
 55       SELECT 1+(SELECT COUNT( distinct 平均成绩) 
 56               FROM (SELECT S#,AVG(score) AS 平均成绩 
 57                       FROM SC 
 58                   GROUP BY S# 
 59                   ) AS T1 
 60             WHERE 平均成绩 > T2.平均成绩) as 名次, 
 61       S# as 学生学号,平均成绩 
 62     FROM (SELECT S#,AVG(score) 平均成绩 
 63             FROM SC 
 64         GROUP BY S# 
 65         ) AS T2 
 66     ORDER BY 平均成绩 desc; 
 67   
 68 --25、查询各科成绩前三名的记录:(不考虑成绩并列情况) 
 69       SELECT t1.S# as 学生ID,t1.C# as 课程ID,Score as 分数 
 70       FROM SC t1 
 71       WHERE score IN (SELECT TOP 3 score 
 72               FROM SC 
 73               WHERE t1.C#= C# 
 74             ORDER BY score DESC 
 75               ) 
 76       ORDER BY t1.C#; 
 77 --26、查询每门课程被选修的学生数 
 78   select c#,count(S#) from sc group by C#; 
 79 --27、查询出只选修了一门课程的全部学生的学号和姓名 
 80   select SC.S#,Student.Sname,count(C#) AS 选课数 
 81   from SC ,Student 
 82   where SC.S#=Student.S# group by SC.S# ,Student.Sname having count(C#)=1; 
 83 --28、查询男生、女生人数 
 84     Select count(Ssex) as 男生人数 from Student group by Ssex having Ssex='男'; 
 85     Select count(Ssex) as 女生人数 from Student group by Ssex having Ssex='女'； 
 86 --29、查询姓“张”的学生名单 
 87     SELECT Sname FROM Student WHERE Sname like '张%'; 
 88 --30、查询同名同性学生名单，并统计同名人数 
 89   select Sname,count(*) from Student group by Sname having  count(*)>1;; 
 90 --31、1981年出生的学生名单(注：Student表中Sage列的类型是datetime) 
 91     select Sname,  CONVERT(char (11),DATEPART(year,Sage)) as age 
 92     from student 
 93     where  CONVERT(char(11),DATEPART(year,Sage))='1981'; 
 94 --32、查询每门课程的平均成绩，结果按平均成绩升序排列，平均成绩相同时，按课程号降序排列 
 95     Select C#,Avg(score) from SC group by C# order by Avg(score),C# DESC ; 
 96 --33、查询平均成绩大于85的所有学生的学号、姓名和平均成绩 
 97     select Sname,SC.S# ,avg(score) 
 98     from Student,SC 
 99     where Student.S#=SC.S# group by SC.S#,Sname having    avg(score)>85; 
100 --34、查询课程名称为“数据库”，且分数低于60的学生姓名和分数 
101     Select Sname,isnull(score,0) 
102     from Student,SC,Course 
103     where SC.S#=Student.S# and SC.C#=Course.C# and  Course.Cname='数据库'and score <60; 
104 --35、查询所有学生的选课情况； 
105     SELECT SC.S#,SC.C#,Sname,Cname 
106     FROM SC,Student,Course 
107     where SC.S#=Student.S# and SC.C#=Course.C# ; 
108 --36、查询任何一门课程成绩在70分以上的姓名、课程名称和分数； 
109     SELECT  distinct student.S#,student.Sname,SC.C#,SC.score 
110     FROM student,Sc 
111     WHERE SC.score>=70 AND SC.S#=student.S#; 
112 --37、查询不及格的课程，并按课程号从大到小排列 
113     select c# from sc where scor e <60 order by C# ; 
114 --38、查询课程编号为003且课程成绩在80分以上的学生的学号和姓名； 
115     select SC.S#,Student.Sname from SC,Student where SC.S#=Student.S# and Score>80 and C#='003'; 
116 --39、求选了课程的学生人数 
117     select count(*) from sc; 
118 --40、查询选修“叶平”老师所授课程的学生中，成绩最高的学生姓名及其成绩 
119     select Student.Sname,score 
120     from Student,SC,Course C,Teacher 
121     where Student.S#=SC.S#         and SC.C#=C.C# and C.T#=Teacher.T#         and Teacher.Tname='叶平'         and SC.score=(select max(score)from SC where C#=C.C# ); 
122 --41、查询各个课程及相应的选修人数 
123     select count(*) from sc group by C#; 
124 --42、查询不同课程成绩相同的学生的学号、课程号、学生成绩 
125   select distinct  A.S#,B.score       from SC A  ,SC B       where A.Score=B.Score and A.C# <>B.C# ; 
126 --43、查询每门功成绩最好的前两名 
127     SELECT t1.S# as 学生ID,t1.C# as 课程ID,Score as 分数 
128       FROM SC t1 
129       WHERE score IN (SELECT TOP 2 score 
130               FROM SC 
131               WHERE t1.C#= C# 
132             ORDER BY score DESC 
133               ) 
134       ORDER BY t1.C#; 
135 --44、统计每门课程的学生选修人数（超过10人的课程才统计）。要求输出课程号和选修人数，查询结果按人数降序排列，查询结果按人数降序排列，若人数相同，按课程号升序排列  
136     select  C# as 课程号,count(*) as 人数 
137     from  sc  
138     group  by  C# 
139     order  by  count(*) desc,c#  
140 --45、检索至少选修两门课程的学生学号 
141     select  S#  
142     from  sc  
143     group  by  s# 
144     having  count(*)  >  =  2 
145 --46、查询全部学生都选修的课程的课程号和课程名 
146     select  C#,Cname  
147     from Course  
148     where C# in (select  c#  from  sc group  by  c#)  
149 --47、查询没学过“叶平”老师讲授的任一门课程的学生姓名 
150     select Sname from Student         where S# not in         (           select S#            from Course,Teacher,SC            where Course.T#=Teacher.T#            and SC.C#=course.C# and Tname='叶平'        ); 
151 --48、查询两门以上不及格课程的同学的学号及其平均成绩 
152     select S#,avg(isnull(score,0)) from SC         where S# in         (            select S# from SC where score <60 group by S# having count(*)>2        )        group by S#; 
153 --49、检索“004”课程分数小于60，按分数降序排列的同学学号 
154     select S# from SC where C#='004'and score <60 order by score desc; 
155 --50、删除“002”同学的“001”课程的成绩 
156 delete from Sc where S#='001'and C#='001'; 
```





问题描述：

本题用到下面三个关系表：

CARD 借书卡。 CNO 卡号，NAME 姓名，CLASS 班级

BOOKS 图书。 BNO 书号，BNAME 书名,AUTHOR 作者，PRICE 单价，QUANTITY 库存册数

BORROW 借书记录。 CNO 借书卡号，BNO 书号，RDATE 还书日期

备注：限定每人每种书只能借一本；库存册数随借书、还书而改变。

要求实现如下15个处理：





```
  1 --1. 写出建立BORROW表的SQL语句，要求定义主码完整性约束和引用完整性约束
  2 --实现代码：
  3 CREATE TABLE BORROW(
  4     CNO int FOREIGN KEY REFERENCES CARD(CNO),
  5     BNO int FOREIGN KEY REFERENCES BOOKS(BNO),
  6     RDATE datetime,
  7     PRIMARY KEY(CNO,BNO)) 
  8 
  9 --2. 找出借书超过5本的读者,输出借书卡号及所借图书册数
 10 --实现代码：
 11 SELECT CNO,借图书册数=COUNT(*)
 12 FROM BORROW
 13 GROUP BY CNO
 14 HAVING COUNT(*)>5
 15 
 16 --3. 查询借阅了"水浒"一书的读者，输出姓名及班级
 17 --实现代码：
 18 SELECT * FROM CARD c
 19 WHERE EXISTS(
 20     SELECT * FROM BORROW a,BOOKS b 
 21     WHERE a.BNO=b.BNO
 22         AND b.BNAME=N'水浒'
 23         AND a.CNO=c.CNO) 
 24 
 25 --4. 查询过期未还图书，输出借阅者（卡号）、书号及还书日期
 26 --实现代码：
 27 SELECT * FROM BORROW 
 28 WHERE RDATE<GETDATE() 
 29 
 30 --5. 查询书名包括"网络"关键词的图书，输出书号、书名、作者
 31 --实现代码：
 32 SELECT BNO,BNAME,AUTHOR FROM BOOKS
 33 WHERE BNAME LIKE N'%网络%' 
 34 
 35 --6. 查询现有图书中价格最高的图书，输出书名及作者
 36 --实现代码：
 37 SELECT BNO,BNAME,AUTHOR FROM BOOKS
 38 WHERE PRICE=(
 39     SELECT MAX(PRICE) FROM BOOKS) 
 40 
 41 --7. 查询当前借了"计算方法"但没有借"计算方法习题集"的读者，输出其借书卡号，并按卡号降序排序输出
 42 --实现代码：
 43 SELECT a.CNO
 44 FROM BORROW a,BOOKS b
 45 WHERE a.BNO=b.BNO AND b.BNAME=N'计算方法'
 46     AND NOT EXISTS(
 47         SELECT * FROM BORROW aa,BOOKS bb
 48         WHERE aa.BNO=bb.BNO
 49             AND bb.BNAME=N'计算方法习题集'
 50             AND aa.CNO=a.CNO)
 51 ORDER BY a.CNO DESC 
 52 
 53 --8. 将"C01"班同学所借图书的还期都延长一周
 54 --实现代码：
 55 UPDATE b SET RDATE=DATEADD(Day,7,b.RDATE)
 56 FROM CARD a,BORROW b
 57 WHERE a.CNO=b.CNO
 58     AND a.CLASS=N'C01' 
 59 
 60 --9. 从BOOKS表中删除当前无人借阅的图书记录
 61 --实现代码：
 62 DELETE A FROM BOOKS a
 63 WHERE NOT EXISTS(
 64     SELECT * FROM BORROW
 65     WHERE BNO=a.BNO) 
 66 
 67 --10. 如果经常按书名查询图书信息，请建立合适的索引
 68 --实现代码：
 69 CREATE CLUSTERED INDEX IDX_BOOKS_BNAME ON BOOKS(BNAME)
 70 
 71 --11. 在BORROW表上建立一个触发器，完成如下功能：如果读者借阅的书名是"数据库技术及应用"，就将该读者的借阅记录保存在BORROW_SAVE表中（注ORROW_SAVE表结构同BORROW表）
 72 --实现代码：
 73 CREATE TRIGGER TR_SAVE ON BORROW
 74 FOR INSERT,UPDATE
 75 AS
 76 IF @@ROWCOUNT>0
 77 INSERT BORROW_SAVE SELECT i.*
 78 FROM INSERTED i,BOOKS b
 79 WHERE i.BNO=b.BNO
 80     AND b.BNAME=N'数据库技术及应用' 
 81 
 82 --12. 建立一个视图，显示"力01"班学生的借书信息（只要求显示姓名和书名）
 83 --实现代码：
 84 CREATE VIEW V_VIEW
 85 AS
 86 SELECT a.NAME,b.BNAME
 87 FROM BORROW ab,CARD a,BOOKS b
 88 WHERE ab.CNO=a.CNO
 89     AND ab.BNO=b.BNO
 90     AND a.CLASS=N'力01'
 91 
 92 --13. 查询当前同时借有"计算方法"和"组合数学"两本书的读者，输出其借书卡号，并按卡号升序排序输出
 93 --实现代码：
 94 SELECT a.CNO
 95 FROM BORROW a,BOOKS b
 96 WHERE a.BNO=b.BNO
 97     AND b.BNAME IN(N'计算方法',N'组合数学')
 98 GROUP BY a.CNO
 99 HAVING COUNT(*)=2
100 ORDER BY a.CNO DESC 
101 
102 --14. 假定在建BOOKS表时没有定义主码，写出为BOOKS表追加定义主码的语句
103 --实现代码：
104 ALTER TABLE BOOKS ADD PRIMARY KEY(BNO) 
105 
106 --15.1 将NAME最大列宽增加到10个字符（假定原为6个字符）
107 --实现代码：
108 ALTER TABLE CARD ALTER COLUMN NAME varchar(10) 
109 
110 --15.2 为该表增加1列NAME（系名），可变长，最大20个字符
111 --实现代码：
112 ALTER TABLE CARD ADD 系名 varchar(20)
```





问题描述： 为管理岗位业务培训信息，建立3个表:

S (S#,SN,SD,SA) S#,SN,SD,SA 分别代表学号、学员姓名、所属单位、学员年龄

C (C#,CN ) C#,CN 分别代表课程编号、课程名称

SC ( S#,C#,G ) S#,C#,G 分别代表学号、所选修的课程编号、学习成绩

要求实现如下5个处理：



```
 1 --1. 使用标准SQL嵌套语句查询选修课程名称为’税收基础’的学员学号和姓名 
 2 --实现代码：
 3 SELECT SN,SD FROM S
 4 WHERE [S#] IN(
 5     SELECT [S#] FROM C,SC
 6     WHERE C.[C#]=SC.[C#]
 7         AND CN=N'税收基础')
 8 
 9 
10 --2. 使用标准SQL嵌套语句查询选修课程编号为’C2’的学员姓名和所属单位
11 --实现代码：
12 SELECT S.SN,S.SD FROM S,SC
13 WHERE S.[S#]=SC.[S#]
14     AND SC.[C#]='C2'
15 
16 --3. 使用标准SQL嵌套语句查询不选修课程编号为’C5’的学员姓名和所属单位
17 --实现代码：
18 SELECT SN,SD FROM S
19 WHERE [S#] NOT IN(
20     SELECT [S#] FROM SC 
21     WHERE [C#]='C5')
22 
23 --4. 使用标准SQL嵌套语句查询选修全部课程的学员姓名和所属单位
24 --实现代码：
25 SELECT SN,SD FROM S
26 WHERE [S#] IN(
27     SELECT [S#] FROM SC 
28         RIGHT JOIN C ON SC.[C#]=C.[C#]
29     GROUP BY [S#]
30     HAVING COUNT(*)=COUNT(DISTINCT [S#]))
31 
32 --5. 查询选修了课程的学员人数
33 --实现代码：
34 SELECT 学员人数=COUNT(DISTINCT [S#]) FROM SC
35 
36 --6. 查询选修课程超过5门的学员学号和所属单位
37 --实现代码：
38 SELECT SN,SD FROM S
39 WHERE [S#] IN(
40     SELECT [S#] FROM SC 
41     GROUP BY [S#]
42     HAVING COUNT(DISTINCT [C#])>5)
```

# 遇到的面试题-sql@xuyatao原文链接:https://www.cnblogs.com/xuyatao/p/6732324.html