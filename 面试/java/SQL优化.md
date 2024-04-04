Oracle_SQL优化:

1.选择最有效率的表名顺序

* ORACLE解析器按照从右到左的顺序处置FROM子句中的表名，因此FROM子句中写在最后的表(基础表 drivetab)将被最先处理。
* 当ORACLE处置多个表时，会运用排序及合并的方式连接它首先，扫描第一个表(FROM子句中最后的那个表)并对记录进行派序，然后扫描第二个表(FROM子句中最后第二个表)最后将所有从第二个表中检索出的记录与第一个表中合适记录进行合并。
* 只在基于规则的优化器中有效。
举例：EMP为基础表
SELECT * FROM LOCATION L,CATEGORY C, EMP E WHERE E.EMP_NO BETWEEN 1000 AND 2000 AND E.CAT_NO=C.CAT_NO AND E.LOCN=L.LOCN
优于
SELECT * FROM EMP E, LOCATION L, CATEGORY C WHERE E.CAT_NO=C.CAT_NO AND E.LOCN=L.LOCN AND E.EMP_NO BETWEEN1000 AND 2000

2.ORACLE采用自下而上的顺序解析WHERE子句,根据这个原理,表之间的连接必须写在其他WHERE条件之前, 那些可以过滤掉最大数量记录的条件必须写在WHERE子句的末尾

 例如:

 (低效,执行时间156.3秒)

SELECT …

FROM EMP E

WHERE  SAL > 50000

AND    JOB = ‘MANAGER’

AND    25 < (SELECT COUNT(*) FROM EMP

             WHERE MGR=E.EMPNO);

 (高效,执行时间10.6秒)

SELECT …

FROM EMP E

WHERE 25 < (SELECT COUNT(*) FROM EMP

             WHERE MGR=E.EMPNO)

AND    SAL > 50000

AND    JOB = ‘MANAGER’;

3.减少访问数据库的次数

当执行每条SQL语句时,ORACLE内部执行了许多工作： 解析SQL语句 >估算索引的利用率 >绑定变量 >读数据块等等由此可见,减少访问数据库的次数 ,就能实际上减少ORACLE工作量。

4.使用Truncat而非Delete

Delete表中记录的时候，Oracle会在Rollback段中保存删除信息以备恢复。Truncat删除表中记录的时候不保管删除信息，不能恢复。因此Truncat删除记录比Delete快，而且占用资源少。
删除表中记录的时候，如果不需要恢复的情况之下应该尽量使用Truncat而不是DeleteTruncat仅适用于删除全表的记录。

5.计算记录条数

Select count* from table name; Select count1 from table name; Select maxrownum from table name;
一般认为，没有索引的情况之下，第一种方式最快。如果有索引列，使用索引列当然最快。
		 
6.用Where子句替换Having子句

防止使用HAVING子句，HAVING只会在检索出所有记录之后才对结果集进行过滤。这个处置需要排序、总计等操作。如果能通过WHERE子句限制记录的数目，就能减少这方面的开销。

7.减少对表的查询操作

含有子查询的SQL语句中，要注意减少对表的查询操作。例如:
// 低效
SELECTTA B_NAME FROM TABLES WHERE TAB_NAME = (SELECT TAB_NAME FROM TAB_COLUMNS WHERE VERSION = 604 AND DB_VER = SELECT DB_VER FROM TAB_COLUMNS WHERE VERSION=604)
// 高效
SELECTTA B_NAME FROM TABLES WHERE TAB_NAME DB_VER = (SELECT TAB_NAME DB_VER FROM TAB_COLUMNS WHERE VERSION=604)

8.使用表的别名（Alias）

 当在SQL语句中连接多个表时,请使用表的别名并把别名前缀于每个Column上.这样一来,就可以减少解析的时间并减少那些由Column歧义引起的语法过失。

  Column歧义指的由于SQL中不同的表具有相同的Column名,当SQL语句中出现这个Column时,SQL解析器无法判断这个Column归属。
 
9.用EXISTS与IN

 许多基于基础表的查询中，为了满足一个条件 往往需要对另一个表进行联接。这种情况下，使用EXISTS或NOTEXISTS通常将提高查询的效率。
 
 -- 财务部或销售部的员工

select * from emp e where e.deptno in 
(select deptno from dept d where d.dname ='ACCOUNTING' or d.dname = 'SALES')

select * from emp e where exists 
(select 1 from dept d where (d.dname ='ACCOUNTING' or d.dname = 'SALES') and d.deptno = e.deptno)


-- 不是财务部或销售部的员工
select * from emp e where e.deptno not in 
(select deptno from dept d where d.dname ='ACCOUNTING' or d.dname = 'SALES')

select * from emp e where not exists 
(select 1 from dept d where (d.dname ='ACCOUNTING' or d.dname = 'SALES') and d.deptno = e.deptno)

在某些情况下，使用EXISTS子查询可能比使用IN子句更高效，但这并不是绝对的。查询的性能取决于多个因素，包括数据量、索引、统计信息和查询条件等。

一般而言，EXISTS子查询通常在以下情况下具备较高的效率：

当子查询返回的结果集很大时，EXISTS相对来说更高效。因为EXISTS只关注是否存在匹配的行，而不关心具体的数据内容，当外层查询的结果远大于子查询时，则in的查询效果更高。
当子查询中使用了适当的索引时，EXISTS可以利用索引来快速定位匹配的行，从而提高查询性能。
当需要判断一个表是否存在相关记录时，EXISTS更合适。EXISTS可以在找到第一个匹配的记录后就停止查询，而IN会继续查找整个结果集。
然而，对于小型数据集或具有适当索引支持的情况，IN子句也可能是高效的。

10.使用表连接替换EXISTS
 通常来说 采用表连接的方式比EXISTS更有效率
 // 低效
 SELECT ENA ME FROM EMP E WHERE EXISTS SELECT X FROM DEPT WHERE DEPT_NO=E.DEPT_NO AND DEPT_CAT=A
 // 高效
 SELECT ENA ME FROM DEP TDEMPE WHEREE.DEPT_NO=D.DEPT_NO AND DEPT_CAT=A
 
11.使用>=优于>

当进行范围查询时，“>=”操作符可能比“>”操作符更高效。例如，如果您要查询年龄大于等于30岁的人，使用“>=”操作符可以返回所有年龄大于等于30岁的人，而使用“>”操作符需要额外查询一次，以获取年龄等于30岁的人。

12.防止在索引列上使用NOT
 //低效
 SELECT FROM DEPT WHERE NOT DEPT_CODE = 0
 //高效
 SELECT FROM DEPT WHERE DEPT_CODE>0

13.使用UNION替换OR

通常情况下，使用UNION替换WHERE子句中的OR可能会提高查询的性能和可读性。这种方法可以将原本复杂的OR条件拆分成多个简单的查询，每个查询只包含一个条件，从而更好地利用索引和减少查询语句的复杂度。

当使用OR条件时，数据库需要同时考虑多个条件，这可能会导致优化器难以选择最佳的执行计划。而通过使用UNION，可以将不同的条件拆分到不同的查询中，使得优化器更容易选择适当的执行计划。

另外，使用UNION可以使得查询语句更易于理解和维护，因为每个子查询只包含一个条件，减少了逻辑复杂度。

然而，需要注意的是，使用UNION也可能增加数据库的负担，因为它需要执行多个查询并对结果进行合并。在一些情况下，优化器可能会选择使用OR条件来执行更高效的连接操作，而不是使用UNION。

 // 高效
 SELECT LOC_ID,LOC_DESC,REGION FROM LOCATION WHERE LOC_ID=10 UNION SELECT LOC_ID,LOC_DESC,REGION FROM LOCATION WHERE REGION = MELBOURNE
 // 低效
 SELECT LOC_ID,LOC_DESC,REGION FROM LOCATION WHERE LOC_ID=10 OR REGION=MELBOURNE
 
14.用in替代Or
 // 低效
 SELECT * FROM LOCATION WHERE LOC_ID = 10 OR LOC_ID =20 OR LOC_ID = 30
 // 高效
 SELECT * FROM LOCATION WHERE LOC_ID IN (10,20,30)
 
15.防止在索引列上使用is null和is not null

 //低效，索引失效
 SELECT * FROM DEPARTMENT WHERE DEPT_CODE IS NOT NULL;
 //索引有效
 SELECT * FROM DEPARTMENT WHERE DEPT_CODE >=0;
 
16.优化GROUP BY: 

 提高GROUP BY 语句的效率,可以通过将不需要的记录在GROUP BY之前过滤掉。

下面两个查询返回相同结果但明显第二个效率更高。 

低效: 

SELECT JOB,AVG(AGE) FROM TEMP
GROUP BY JOB HAVING JOB = 'STUDENT' OR JOB = 'MANAGER';
高效: 

SELECT JOB,AVG(AGE) FROM EMP
WHERE JOB = 'STUDENT' OR JOB = 'MANAGER' GROUP BY JOB;

17、避免在索引列上使用计算： 

 WHERE子句中，如果索引列是函数的一部分，优化器将不使用索引而使用全表扫描。 

低效： 

SELECT … FROM TEMP WHERE SAL * 12 > 25000;
高效: 

SELECT … FROM TEMP WHERE SAL > 25000/12;

18、避免改变索引列的类型: 

当比较不同数据类型的数据时, ORACLE自动对列进行简单的类型转换。 

假设 USER_ID 是一个数值类型的索引列。 

SELECT … FROM USER_TAB WHERE USER_ID = '123';
实际上,经过ORACLE类型转换, 语句转化为: 

SELECT … FROM USER_TAB WHERE USER_ID = TO_NUMBER('123'); 
幸运的是,类型转换没有发生在索引列上,索引的用途没有被改变。

现在,假设USER_TYPE是一个字符类型的索引列。

SELECT …  FROM USER_TAB WHERE USER_TYPE = 123 
这个语句被ORACLE转换为: 

SELECT …  FROM USER_TAB WHERE TO_NUMBER(USER_TYPE)=123;
因为内部发生的类型转换, 这个索引将不会被用到! 为了避免ORACLE对你的SQL进行隐式的类型转换, 最好把类型转换用显式表现出来。

注：当字符和数值比较时, ORACLE会优先转换数值类型到字符类型。

SELECT …  FROM USER_TAB WHERE TO_NUMBER(USER_TYPE)=123;