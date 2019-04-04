上节课我们通过子查询，完成了查询的最高分学生的需求，今天我们来学习子查询的分类，以及通过子查询来完成工作中经常遇到一些个性化需求。

### 子查询概念：

```
一个SELECT语句嵌套在另一个SELECT语句中，子查询也叫做内部查询，而包含子查询的语句又称为外部查询或主查询，子查询自身可以包含一个或多个子查询，一个查询语句中可以嵌套任意数量的子查询
```

### 子查询可分类：

```
非相关子查询：独立于外部查询，子查询只执行一次，执行完将结果传递给外部查询相关子查询：依赖于外部查询的数据，外部查询每执行一次，子查询就执行一次
```

### 面试题：

```
还是这道数据库面试题柠檬班第30期学生要毕业了，他们的成绩存放在下表中，写出以下的SQL语句
```

![img](http://quniu.ymxless.com/image/1554367051149.jpg)

### 题目一：查询最新的一条记录

```
Order by 方式：
```

降序排列然后得到我们最新的一条记录，这是我们常写的一种方式

```
SELECT * FROM tb_lemon_grade ORDER BY id DESC LIMIT 1;
子查询方式：
```

但查询最新一条记录，也可以这么去思考：查看id值最大（id是自动增长的，最新表示id值最大）的记录，所以可以这么去写查询

```
SELECT * FROM tb_lemon_grade WHERE id = ( SELECT max(id) FROM tb_lemon_grade);
```

其中子查询SELECT max(id) FROM tb_lemon_grade查询的是记录表中最大的一个id，在整个查询中，只会查询一遍，这种就是非相关子查询，执行完毕后，会将值传递给外部查询。

###  题目二：查询Linux成绩高于平均分的所有同学

```
子查询方式：
SELECT * FROM tb_lemon_grade WHERE Linux > ( SELECT avg(Linux) FROM tb_lemon_grade );
```

上面子查询SELECT avg(Linux) FROM tb_lemon_grade也是非相关子查询，语句只会执行一遍

```
关联查询方式：
```

这个题目我们可以使用两个表的关联查询得到结果

```
SELECT t1.* FROM tb_lemon_grade t1,(SELECT avg(Linux) avgLinux FROM tb_lemon_grade) t2 
where t1.Linux>t2.avgLinux;
```

###  题目三：查询每个班级Linux成绩高于本班Linux平均分的所有同学：

```
子查询方式
SELECT * FROM tb_lemon_grade t1 
WHERE t1.Linux >( 
	SELECT avg(t2.Linux) FROM tb_lemon_grade t2 
	WHERE t1.class_name = t2.class_name 
);
```

我们来分析下这个题目，查询每个班级Linux成绩高于本班Linux平均分的所有同学，而每个班的Linux平均分不同，所以我们采用相关子查询，语句中的这个子查询依赖于外部的查询（ 子查询中的t1.class_name = t2.class_name就是外部的表），外部查询每执行一次，子查询就执行一次。

```
分组的方式写子查询：
SELECT * FROM tb_lemon_grade t1 
WHERE t1.Linux > ( 
	SELECT avg(t2.Linux) FROM tb_lemon_grade t2 
	GROUP BY t2.class_name 
	HAVING t1.class_name = t2.class_name );
关联查询方式：
```

通过分组查询出每个班的最高分，再与原表进行等值连接查询，得到最后结果。

```
SELECT * FROM tb_lemon_grade t1, ( 
	SELECT avg(Linux) avgLinux, class_name 
	FROM tb_lemon_grade 
	GROUP BY class_name 
) t2
 WHERE t1.class_name = t2.class_name
 AND t1.Linux>t2.avgLinux;
```

- 【数据库】SQL经典面试题 - 数据库查询 - 子查询应用二@华妹陀原文链接:https://www.cnblogs.com/liulinghua90/p/7505697.html