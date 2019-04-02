**本文是在Cat Qi的参考原帖的基础之上经本人一题一题练习后编辑而成，非原创，仅润色而已。另外，本文所列题目的解法并非只有一种，本文只是给出比较普通的一种而已，也希望各位园友能够自由发挥。**

# **一、书到用时方恨少："图书-读者-借阅"类题目**

## **1.1 本题目的表结构**

　　本题用到下面三个关系表：

　　CARDS 借书卡。 CNO 卡号，NAME 姓名，CLASS 班级

　　BOOKS 图书。 BNO 书号，BNAME 书名,AUTHOR 作者，PRICE 单价，QUANTITY 库存册数

　　BORROW 借书记录。 CNO 借书卡号，BNO 书号，RDATE 还书日期

　　备注：限定每人每种书只能借一本；库存册数随借书、还书而改变。

## **1.2 基本建表语句**



```
 1 create table CARDS
 2 (
 3     CNO int identity(1,1),
 4     NAME nvarchar(50) not null,
 5     CLASS nvarchar(100) not null,
 6     constraint pk_cards primary key (CNO)
 7 )
 8 
 9 create table BOOKS
10 (
11     BNO int identity(1,1),
12     BNAME nvarchar(255) not null,
13     AUTHOR nvarchar(50) not null,
14     PRICE decimal(8,1) not null,
15     QUANTITY int default 0,
16     constraint pk_books primary key (BNO)
17 )
18 
19 create table BORROW
20 (
21     CNO int not null,
22     BNO int not null,
23     RDATE datetime default GETDATE() not null,
24     constraint pk_borrow primary key (CNO,BNO),
25     constraint fk_borrow_cards foreign key (CNO) references CARDS(CNO),
26     constraint fk_borrow_books foreign key (BNO) references BOOKS(BNO)
27 )
```



## 1.3 插入测试数据



```
 1 insert into CARDS(NAME,CLASS) values('张三','计科一班')
 2 insert into CARDS(NAME,CLASS) values('李四','计科一班')
 3 insert into CARDS(NAME,CLASS) values('王五','计科二班')
 4 insert into CARDS(NAME,CLASS) values('六四','计科二班')
 5 insert into CARDS(NAME,CLASS) values('七七','软工一班')
 6 insert into CARDS(NAME,CLASS) values('粑粑','软工二班')
 7 
 8 insert into BOOKS(BNAME,AUTHOR,PRICE,QUANTITY) values ('水浒','施耐庵',188,3)
 9 insert into BOOKS(BNAME,AUTHOR,PRICE,QUANTITY) values ('计算机网络','谢希仁',49,3)
10 insert into BOOKS(BNAME,AUTHOR,PRICE,QUANTITY) values ('计算方法','严蔚敏',58,3)
11 insert into BOOKS(BNAME,AUTHOR,PRICE,QUANTITY) values ('计算方法习题集','殷人昆',188,3)
12 insert into BOOKS(BNAME,AUTHOR,PRICE,QUANTITY) values ('数据库技术及应用','王珊',38,3)
13 insert into BOOKS(BNAME,AUTHOR,PRICE,QUANTITY) values ('组合数学','周伟',28,3)
14 insert into BOOKS(BNAME,AUTHOR,PRICE,QUANTITY) values ('Redis初探','周旭龙',25,3)
15 
16 insert into BORROW(CNO,BNO) values(1,1)
17 insert into BORROW(CNO,BNO) values(2,1)
18 insert into BORROW(CNO,BNO) values(3,1)
19 
20 insert into BORROW(CNO,BNO) values(4,3)
21 insert into BORROW(CNO,BNO) values(4,6)
22 insert into BORROW(CNO,BNO) values(5,6)
23 insert into BORROW(CNO,BNO) values(7,7)
```



## 1.4 开始实战吧小宇宙

　　（1）写出建立BORROW表的SQL语句，要求定义主码完整性约束和引用完整性约束



```
1 create table BORROW
2 (
3     CNO int not null,
4     BNO int not null,
5     RDATE datetime default GETDATE() not null,
6     constraint pk_borrow primary key (CNO,BNO),
7     constraint fk_borrow_cards foreign key (CNO) references CARDS(CNO),
8     constraint fk_borrow_books foreign key (BNO) references BOOKS(BNO)
9 )
```



　　（2）找出借书超过5本的读者,输出借书卡号及所借图书册数

```
1 select b.CNO,COUNT(b.CNO) as 'BorrowCount' 
2 from BORROW b
3 group by b.CNO
4 having COUNT(b.CNO)>=5
```

　　这里测试数据里边没有借过5本的，但只要改为2，即可得到一条结果：

![img](https://images0.cnblogs.com/i/381412/201408/041454433811378.jpg)

　　（3）查询借阅了"水浒"一书的读者，输出姓名及班级

```
1 select c.NAME,c.CLASS 
2 from CARDS c,BORROW r,BOOKS b
3 where c.CNO=r.CNO and r.BNO=b.BNO and b.BNAME='水浒'
```

![img](https://images0.cnblogs.com/i/381412/201408/041457466624672.jpg)

　　（4）查询目前为止未还图书，输出借阅者（卡号）、书号及还书日期

```
1 select CNO,BNO,RDATE 
2 from BORROW 
3 where RDATE<GETDATE()
```

![img](https://images0.cnblogs.com/i/381412/201408/041459417099485.jpg)

　　（5）查询书名包括"网络"关键词的图书，输出书号、书名、作者

```
1 select b.BNO,b.BNAME,b.AUTHOR 
2 from BOOKS b
3 where b.BNAME like '%网络%'
```

![img](https://images0.cnblogs.com/i/381412/201408/041501232259433.jpg)

　　（6）查询现有图书中价格最高的图书，输出书名及作者

```
1 select BNAME,AUTHOR 
2 from BOOKS
3 where PRICE=( SELECT MAX(PRICE) from BOOKS )
```

![img](https://images0.cnblogs.com/i/381412/201408/041503038655779.jpg)

　　（7）查询当前借了"计算方法"但没有借"计算方法习题集"的读者，输出其借书卡号，并按卡号降序排序输出



```
1 select r.CNO 
2 from BORROW r,BOOKS b
3 where r.BNO=b.BNO and b.BNAME='计算方法' and not exists
4 (
5     select * from BORROW r1,BOOKS b1 
6     where r1.BNO=b1.BNO and r.CNO=r1.CNO and b1.BNAME='计算方法习题集'
7 )
8 order by r.CNO desc
```



![img](https://images0.cnblogs.com/i/381412/201408/041516068034805.jpg)

　　（8）将"计科一班"班同学所借图书的还期都延长一周



```
1 --解法一
2 update BORROW set RDATE=DATEADD(Day,7,RDATE) 
3 where CNO in ( select CNO from CARDS where CLASS='计科一班' )
4 --解法二
5 update b set b.RDATE=DATEADD(Day,7,RDATE)  
6 from BORROW b,CARDS c
7 where b.CNO=c.CNO and c.CLASS='计科一班'
```



![img](https://images0.cnblogs.com/i/381412/201408/041520101312742.jpg)

　　（9）从BOOKS表中删除当前无人借阅的图书记录

```
1 delete from BOOKS 
2 where BNO not in 
3 (
4     select distinct BNO from BORROW
5 )
```

![img](https://images0.cnblogs.com/i/381412/201408/041524435532275.jpg)

　　这里四本图书被删除，只剩下1,3,6这三本图书了。

　　（10）如果经常按书名查询图书信息，请建立合适的索引

```
1 create index index_books_bname on BOOKS(BNAME)
```

> **PS：关于索引，你必须了解的东东**
>
> ①索引就是**加快检索表中数据**的方法。数据库的索引类似于书籍的索引。在书籍中，索引允许用户不必翻阅完整个书就能迅速地找到所需要的信息。在数据库中，索引也允许数据库程序迅速地找到表中的数据，而**不必扫描整个数据库**。
>
> ②索引的优点：**大大加快数据的检索速度**，这也是创建索引的最主要的原因；
>
> ③索引的缺点：**索引需要占物理空间**，除了数据表占数据空间之外，每一个索引还要占一定的物理空间，如果要建立聚簇索引，那么需要的空间就会更大；当对表中的数据进行增加、删除和修改的时候，**索引也要动态的维护**，降低了数据的维护速度；

　　（11）在BORROW表上建立一个触发器，完成如下功能：

　　　　-- 如果读者借阅的书名是"数据库技术及应用"
　　　　-- 就将该读者的借阅记录保存在BORROW_SAVE表中（注ORROW_SAVE表结构同BORROW表）



1 create trigger Tr_CopyToSave
2 on BORROW
3 for insert,update
4 as
5 if @@ROWCOUNT>=1
6 insert into BORROW_SAVE select i.BNO,i.CNO,i.RDATE
7 from inserted i,BOOKS b
8 where i.BNO=b.BNO and b.BNAME='数据库技术及应用'



> **PS：关于触发器，你必须了解的东东**
>
> ①触发器是一种**特殊类型的存储过程**，对特定事件作出响应。触发器对表进行插入、更新、删除的时候会自动执行的特殊存储过程，一般用在较check约束更加复杂的约束上面。
>
> ②触发器有两个特殊的表：**插入表**（**instered**表）和**删除表**（**deleted**表）。这两张是逻辑表也是虚表。系统**在内存中创建这两张表**，不会存储在数据库中。而且两张表的都是只读的，只能读取数据而不能修改数据。这两张表的结果总是与被改触发器应用的表的结构相同。当触发器完成工作后，这两张表就会被删除。inserted表的数据是插入或是修改后的数据，而deleted表的数据是更新前的或是删除的数据。

　　（12）建立一个视图，显示"计科一班"班学生的借书信息（只要求显示姓名和书名）

```
1 create view V_BorrowInfo_CS0801
2 as
3 select c.NAME,b.BNAME 
4 from CARDS c,BOOKS b,BORROW r
5 where c.CNO=r.CNO and b.BNO=r.BNO and c.CLASS='计科一班'
```

![img](https://images0.cnblogs.com/i/381412/201408/041613012259430.jpg)

> **PS：关于（View）视图，你必须了解的东东**
>
> （1）视图是从一个或几个基本表中根据用户需要而做成的一个**虚表**：①视图是虚表，它在存储时**只存储视图的定义，而没有存储对应的数据**；②视图只在刚刚打开的一瞬间，通过定义从基表中搜集数据,并展现给用户；
>
> （2）视图的优点：①能分割数据，**简化用户观点**。②为数据提供一定的**逻辑独立性**（如果为某一个基表定义一个视图,即使以后基本表的内容的发生改变了也不会影响“视图定义”所得到的数据）；③提供**自动的安全保护**功能（ 视图能像基本表一样授予或撤消访问许可权）

　　（13）查询当前同时借有"计算方法"和"组合数学"两本书的读者，输出其借书卡号，并按卡号升序排序输出



```
1 select b.CNO 
2 from BORROW b
3 where b.BNO in (select BNO from BOOKS where BNAME in ('计算方法','组合数学'))
4 group by b.CNO
5 having COUNT(b.BNO)=2
6 order by b.CNO
```



![img](https://images0.cnblogs.com/i/381412/201408/041623466154067.jpg)

　　（14）假定在建BOOKS表时没有定义主码，写出为BOOKS表追加定义主码的语句

```
alter table BOOKS add primary key (BNO)
```

　　（15）①将CARDS表中的NAME最大列宽增加到100个字符（原为50个字符）

```
alter table CARDS alter column NAME nvarchar(100)
```

　　　　　 ②为CARDS表增加1列DEPTNAME（系名），可变长，最大50个字符

```
alter table CARDS add DEPTNAME nvarchar(50)
```

 

# **二、练习总结**

　　本篇是从[Cat Qi](http://www.cnblogs.com/qixuejia)的原文《SQL面试题（学生表-教师表-课程表-选课表）》中摘抄的，Part 1的链接[点此访问](http://www.cnblogs.com/edisonchou/p/3878135.html)。总体来说，Part 2本篇的题目难度没有Part 1的高，比较适合总结锻炼。最后，感谢Cat Qi总结的文章，让我可以从中实践并得到一点提高。后面，我会继续复习一下有关数据库的基础知识和练习一下数据库的其他方面的笔试面试题，到时如果有机会，还会总结成博客发布到我的园子。

# **参考原帖**

　　（1）Cat Qi，《SQL面试题（学生表-教师表-课程表-选课表）》：http://www.cnblogs.com/qixuejia/p/3637735.html

　　（2）CSDN，《找些不错的SQL面试题》讨论帖，http://bbs.csdn.net/topics/280002741

　　（3）逆心，《SQL Server 触发器》，http://www.cnblogs.com/kissdodog/p/3173421.html

　　（4）JohnSoft工作室，《数据库视图定义及其相关操作》，http://www.cnblogs.com/GISDEV/archive/2008/02/13/1067817.html

 

作者：[周旭龙](http://www.cnblogs.com/edisonchou/)

出处：<http://www.cnblogs.com/edisonchou/>

本文版权归作者和博客园共有，欢迎转载，但未经作者同意必须保留此段声明，且在文章页面明显位置给出原文链接。

# 走向面试之数据库基础：一、你必知必会的SQL语句练习-Part 2@edison chou原文链接:https://www.cnblogs.com/edisonchou/p/3886801.html