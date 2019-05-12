## Oralce_SQL

标签 (SQL)

### 一、基础

主键的值不允许修改，也不允许复用（不能使用已经删除的主键值赋给新数据行的主键）。

SQL（Structured Query Language）,标准 SQL 由 ANSI 标准委员会管理，从而称为 ANSI SQL。各个 DBMS 都有自己的实现，如 PL/SQL、Transact-SQL 等。

SQL 语句不区分大小写，但是数据库表名、列名和值是否区分依赖于具体的 DBMS 以及配置。

SQL支持三种注释：

``` sql
# 注释
SELECT *
FROM mytable; -- 注释
/* 注释1
   注释2 */
```



数据库创建与使用：

```
CREATE DATABASE test;
USE test;
```



### 二、创建表

~~~sql
create table mytable (
id INT NOT NULL,
COL1 INT NOT,
COL2 VARCHAR(32),
COL3 DATE);
~~~



### 三、修改表

添加字段：

~~~sql
alter table mytable add col4 varchar2(32);
~~~



删除字段：

~~~sql
alter table mytable drop column col4;
~~~



删除表：

~~~sql
drop table mytable;
~~~





### 四、插入表

普通插入：

```sql
insert into mytbale(col1,col2) values('1','2');
```



插入检索出来的数据：

```sql
insert into mytable1(col1,col2) select col1,col2 from mytable2;
```



将一张表的内容备份到备份表中：

```sql
create table newtable as select * from mytable;
```



### 五、更新

```sql 
update mytable set col1='1' where id='one';
```



### 六、删除

~~~sql
delete from mytable where id='one';
~~~

**truncate table** 可以清空表，也就是删除表的所有行。直接清空表不用提交动作。

~~~sql
TRUNCATE table mytbale;
~~~



### 七、查询

#### **distinct**

---

 相同值只会出现一次。它作用与所有列，也就是说所有列的值都相同才算相同。

~~~sql
select distinct col1,col1 from mytable;
~~~



#### **limit**  

---

mysql 中的函数：限制返回的行数，可以有俩个参数，第一个参数为起始行，从0 开始；第二个参数为返回的总行数；

返回前5行：

~~~sql
select * from mybtable limit 5;
~~~

~~~sql
select * from mybable limit 0,5;
~~~

返回第3~~5行：

~~~sql
select * from mytable limit 2,5;
~~~



### 八、排序

* ACS 升序（默认）
* DESC 降序

可以按多个列进行排序，并且为每个列指定不同的排序方式；

~~~sql
select * from mytable order by col1 DESC,col2 ASC;
~~~



### 九、过滤

不进行过滤的数据非常大，导致通过网络传输了多余的数据，从而浪费了网络带宽。因此尽量使用 SQL 语句来过滤不必要的数据，而不是传输所有的数据到客户端中然后由客户端进行过滤。

~~~sql
select * from mytable where id is not null;
~~~



下表显示了 where子句可用的操作符；

|  操作符  |     说明     |
| :------: | :----------: |
|    =     |     等于     |
|    <     |     小于     |
|    >     |     大于     |
| <>    != |    不等于    |
|  <= !>   |   小于等于   |
| >=   !<  |   大于等于   |
| between  | 在俩个值之间 |
| is null  |   为null值   |



注：null 与 0 、空字符串都不同。

**and** 和 **or** 用于连接多个过滤条件。优先处理 and，当一个过滤表达式涉及到多个and和or时，可以使用 () 来决定优先级，使得优先级关系更清晰。 



**IN** 操作符用户匹配一组值，其后也可以连接一个select子句，从而匹配子查询得到的一组词。

**NOT** 用于否定一个条件。



### 十、通配符

---

通配符也是用在过滤语句中，但它只能用于文本字段。

* % 匹配 >=0 个任意字符；

* _ 匹配 ==1 个任意字符；

* [   ] 可以匹配集合内的字符，例如[ab] 将匹配字符 a 或者 b 。用脱字符 ^ 可以对其进行否定，也就是不匹配集合内的字符。

   

使用 like 来进行通配符匹配。

~~~sql
select * from mytable where col like '[^AB]%'; --不以 A 和 B 开头的任意文本。
~~~

不要滥用通配符，通配符位于开头处匹配会非常慢。

### 十一、计算字段

在数据库服务器上完成数据的转换和格式化的工作往往比客户端上快得多，并且转换和格式化后的数据量更少的话可以减少网络通信量。

计算字段通常需要使用 **AS** 来取别名，否则输出的时候字段名为计算表达式。

~~~sql
select cou1*col2 AS alias from mytable;
~~~

**concat()** 用于连接两个字段，使用 **trim()** 可以去除首尾空格。

~~~sql
select concat(trim(col1),trim(col2)) as concat_col from mytable;
~~~



### 十二、函数

####  汇总

----

|  行数   |   说明   |
| :-----: | :------: |
|  AVG()  |  平均值  |
| COUNT() | 统计数量 |
|  MAX()  |  最大值  |
|  MIN()  |  最小值  |
|  SUM()  |  值之和  |

AVG() 会忽略 NULL 行。

使用 DISTINCT 可以让汇总函数值胡总不同的值。

~~~sql
select AVG(distinct col1) as avg_col from mytable;
~~~

#### **文本处理**

----

|   函数    |      说明      |
| :-------: | :------------: |
|  left()   |   左边的字符   |
|  right()  |   右边的字符   |
|  lower()  | 转换为小写字符 |
|  upper()  | 转换为大写字符 |
|  ltrim()  | 去除左边的空格 |
|  rtrim()  | 去除右边的空格 |
| length()  |      长度      |
| soundex() |  转换为语音值  |

其中， **SOUNDEX()** 可以将一个字符串转换为描述其语音表示的字母数字模式。

~~~sql
select * from mytable where soundex(col) = soundex('apple');
~~~



#### **日期和时间处理**

---

* 日期格式：YYYY-MM-DD

* 时间格式：HH:MM:SS

  

|      函数      |              说明              |
| :------------: | :----------------------------: |
|   ADDDATE()    |    增加一个日期（天，周等）    |
|   ADDTIME()    |    增加一个时间（时，分等）    |
|   CURDATE()    |          返回当前日期          |
|   CURTIME()    |          返回当前时间          |
|     DATE()     |     返回日期时间的日期部分     |
|   DATEDIFF()   |        计算两个日期只差        |
|   DATE_ADD()   |     高度灵活的日期运算函数     |
| DATE_FORMATE() |  返回一个格式话的日期或时间串  |
|     DAY()      |     返回一个日期的天数部分     |
|  DAYOFWEEK()   | 对于一个日期，返回对应的星期几 |
|     HOUR()     |     返回一个时间的小时部分     |
|    MINUTE()    |     返回一个时间的分钟部分     |
|    MONTH()     |     返回一个日期的月份部分     |
|     NOW()      |       返回当前日期和时间       |
|    SECOND()    |      返回一个时间的秒部分      |
|     TIME()     |   返回一个日期时间的时间部分   |
|     YEAR()     |     返回一个日期的年份部分     |

oracle 获取当前时间

~~~sql
select sysdate from dual;
select to_char(sysdate,'yyyy-MM-dd') from dual;
~~~

mysql 获取当前时间

~~~sql
select NOW();
~~~



#### 数值处理

|  函数  |  说明  |
| :----: | :----: |
| SIN()  |  正弦  |
| COS()  |  余弦  |
| TAN()  |  正切  |
| ABS()  | 绝对值 |
| SQRT() | 平方根 |
| MOD()  |  余数  |
| EXP()  |  指数  |
|  PI()  | 圆周率 |
| RAND() | 随机数 |



### 十三、分组

---

分组就是把具有相同的数据值的行放在同一组中。

可以对同一分组数据使用汇总函数进行处理，例如求分组数据的平均值等。

指定的分组字段除了能按该字段进行分组，也会自动按该字段进行排序。

~~~sql
select col,count(*) from mytable group by col;
~~~

**group by** 自动按分组字段进行排序，**order by** 也可以按汇总字段来进行排序。

~~~sql
select col,count(*) as num from mytable group col order by num;
~~~

**where** 过滤行，**HAVING** 过滤分组，行过滤应当先于分组过滤。

~~~sql
select col,count(*) as num from mytbable where col>2 group by col HAVING num >2;
~~~

分组规则：

* GROUP BY 子句出现在where 子句之后，ORDER BY 子句之前；
* 除了汇总字段之外，select 语句中的每一个字段都必须在 GROUP BY 子句中给出。
* NULL 的行会单独分为一组；
* 大多数 SQL 实现不支持 GRUOP BY 列具有可变长度的数据类型。

### 十四、子查询

**子查询中只能返回一个字段的数据**

可以将子查询的结果作为 WHERE 语句的过滤条件；

~~~sql
select * from mytable col in (select col2 from mytable2);
~~~

下面的语句可以检索出客户的订单数量，子查询语句会对第一个查询检索出的每个客户执行一次：

~~~sql
select cust_name,(select count(*) from Orders where Orders.cust_id=Customers.cust_id) as oders_num from Customers order by cust_name;
~~~



### 十五、连接

---

连接用于连接多个表，使用 JOIN 关键字，并且条件语句使用 ON 而不是where；

连接可以替换子查询，并且比子查询查询效率一般会更快。

可以使用 AS 给列名，计算字段和表名取别名，给表名取别名是为了简化SQL以及连接相同的表。

#### **内连接**

---

内连接又称等值连接，使用 INNER JION 关键字。

~~~sql
select A.value,B.value from tablea as A INNER JOIN tableb AS B ON A.Key=B.Key;
~~~

也可以

~~~sql
select A.value,B.value from tablea as A,tableb as B where A.key=B.key;
~~~

在没有条件语句的情况下返回笛卡尔积。

#### **自连接**

---

自连接可以看成是内连接的一种，只是连接的表是自身而已。

一张员工表，包含员工名称和员工部门，要找去与JIM所处同一个部门的所有员工姓名；

子查询版本：

~~~sql
select name from employee where department = (select department from employee where name='jim');
~~~

自连接版本

~~~sql
select e1.name from employee e1 inner join employee e2 on e1.department=e2.department and e2.name='jim';
~~~

#### **自然连接**

---

自然连接是把同名列通过等值测试连接起来的，同名列可以有多个。

内连接和自然连接的区别：内连接提供连接的列，而自然连接自动连接多有同名的列。

~~~sql
select A.value,B.value from tablea AS A natural join tableb AS B;
~~~

#### **外连接**

---

外连接保留了没有关联的那些行。分为左外连接，右外连接以及全外连接，左外连接就是保留左表没有关联的行。

检索所有顾客的订单信息，包括还没有订单信息的顾客。

```sql
SELECT Customers.cust_id, Orders.order_num
FROM Customers LEFT OUTER JOIN Orders
ON Customers.cust_id = Orders.cust_id;
```

customers 表：

| cust_id | cust_name |
| ------- | --------- |
| 1       | a         |
| 2       | b         |
| 3       | c         |

orders 表：

| order_id | cust_id |
| -------- | ------- |
| 1        | 1       |
| 2        | 1       |
| 3        | 3       |
| 4        | 3       |

结果：

| cust_id | cust_name | order_id |
| ------- | --------- | -------- |
| 1       | a         | 1        |
| 1       | a         | 2        |
| 3       | c         | 3        |
| 3       | c         | 4        |
| 2       | b         | Null     |



### 十六、组合查询

使用 UNION 来组会来个查询，如果第一个查询返回 M 行，第二个查询返回 N 行，那么组合查询的结果一般为 M+N 行。

每个查询必须包含相同的列，表达式，聚合函数。

默认去除相同行，如果需要保留相同行，使用 UNION ALL。

只能包含一个 Order By 子句，并且必须是位于语句的最后。

~~~sql
select col from mytable where col=1 
union 
select col from mytable where col=2
~~~



### 十七、视图

视图是虚拟的表，本身不包含数据，也就不能对其索引操作。

对视图的操作和对普通表的操作一样。

视图具有如下好处：

* 简化复杂的SQL操作，比如复杂的连接；
* 只使用实际表的一部分数据；
* 通过只给用户访问视图的权限，保证数据的安全性；
* 更改数据格式和表示；

```sql
create view myview as 
select Concat(col1,col2) as concat_col,col3*col4 as compute_col from mytable where  col5=val;
```



### 十八、存储过程

---

存储过程可以看成是对一系列 SQL 操作的批处理；

使用存储过程的好处：

* 代码封装，保证了一定的安全性；
* 代码复用；
* 由于是预先编译，因此具有很高的性能；

命令行中创建存储过程需要自定义分隔符，因为命令窗口是；为结束符。而存储过程中也包含了分号，因此会错误把这部分分号当成结束符，造成语法错误；

包含 in 、out、和 inout 三种参数。

给变量赋值都需要用 select  into 语句。

每次只能给一个变量赋值，不支持集合操作。

```sql
delimiter //
create procedure myprocedure (out ret into )
	begin
		declare y int;
		select sum(col1)
		from mytable
		int y;
		select y*y into ret;
	end //
delimiter ;
```

```sql
call myprocedure(@ret);
select @ret;
```



### 十九、游标

在存储过程中使用游标可以对一个结果集进行移动遍历。

游标主要用于交互是应用，其中用户需要对数据集中的任意行进行浏览和修改。

使用游标的四个步骤：

1. 声明游标，这个过程没有实际检索出数据；
2. 打开游标；
3. 取出数据；
4. 关闭游标；

```sql
delimiter //
create procedure myprocedure(out ret int)
    begin
        declare done boolean default 0;

        declare mycursor cursor for
        select col1 from mytable;
        # 定义了一个 continue handler，当 sqlstate '02000' 这个条件出现时，会执行 set done = 1
        declare continue handler for sqlstate '02000' set done = 1;

        open mycursor;

        repeat
            fetch mycursor into ret;
            select ret;
        until done end repeat;

        close mycursor;
    end //
 delimiter ;
```



### 二十、触发器

---

触发器会在某个表执行以下语句时而自动执行：delete 、insert 、update；

触发器必须指定在语句执行之前还是之后自动执行，之前执行使用 BEFORE 关键字，之后执行使用 AFTER 关键字。BEFORE 用于数据验证和净化，AFTER 用于审计跟踪，将修改记录到另外一张表中。

insert 触发器包含一个名为 new 的虚拟表。

```sql
CREATE TRIGGER mytrigger AFTER INSERT ON mytable
FOR EACH ROW SELECT NEW.col into @result;

SELECT @result; -- 获取结果
```

DELETE 触发器包含一个名为 OLD 的虚拟表，并且是只读的。

UPDATE 触发器包含一个名为 NEW 和一个名为 OLD 的虚拟表，其中 NEW 是可以被修改的，而 OLD 是只读的。

MySQL 不允许在触发器中使用 CALL 语句，也就是不能调用存储过程



### 二十一、事务管理

基本术语：

* 事物（transaction）指一组SQL语句；
* 退回（roolback）指撤销指定SQL语句的过程；
* 提交（commit）指将未存储的SQL语句结果写入数据库表中；
* 保留点（sqvepoint）指事务处理中设置的临时占位符（placeholder），你可以对他发布回退（与回退整个事务处理不同）。

不能回退 SELECT 语句，回退 SELECT 语句也没意义；也不能回退 CREATE 和 DROP 语句。

MySQL 的事务提交默认是隐式提交，每执行一条语句就把这条语句当成一个事务然后进行提交。当出现 START TRANSACTION 语句时，会关闭隐式提交；当 COMMIT 或 ROLLBACK 语句执行后，事务会自动关闭，重新恢复隐式提交。

通过设置 autocommit 为 0 可以取消自动提交；autocommit 标记是针对每个连接而不是针对服务器的。

如果没有设置保留点，ROLLBACK 会回退到 START TRANSACTION 语句处；如果设置了保留点，并且在 ROLLBACK 中指定该保留点，则会回退到该保留点。

```sql
START TRANSACTION
// ...
SAVEPOINT delete1
// ...
ROLLBACK TO delete1
// ...
COMMIT
```



### 二十二、字符集

---

基本术语：

* 字符集为字母和符号的集合；
* 编码为某个字符集成员的内部表示；
* 校对字符指定如何比较，主要用于排序和分组；

除了给表指定字符集和校对外，也可以给列指定：

```sql
CREATE TABLE mytable
(col VARCHAR(10) CHARACTER SET latin COLLATE latin1_general_ci )
DEFAULT CHARACTER SET hebrew COLLATE hebrew_general_ci;
```

可以在排序、分组时指定校对：

```sql
SELECT *
FROM mytable
ORDER BY col COLLATE latin1_general_ci;
```

### 二十三、权限管理

---

MySQL 的账户信息保存在 mysql 这个数据库中。

```sql
USE mysql;
SELECT user FROM user;
```

**创建账户**

新创建的账户没有任何权限。

```sql
CREATE USER myuser IDENTIFIED BY 'mypassword';
```

**修改账户名**

```sql
RENAME myuser TO newuser;
```

**删除账户**

```sql
DROP user myuser;
```

**查看权限**

```sql
show grants for myuser;
```

**授予权限**

账户用 username@host 的形式定义，username@% 使用的是默认主机名。

```sql
GRANT SELECT, INSERT ON mydatabase.* TO myuser;
```

**删除权限**

GRANT 和 REVOKE 可在几个层次上控制访问权限；

* 整个服务器，使用 GRANT ALL 和 REVOKE ALL；
* 整个数据库，使用 ON database.*；
* 特定的表，使用 ON database.table；
* 特定的列；
* 特定的存储过程。

```sql
REVOKE SELECT, INSERT ON mydatabase.* FROM myuser;
```

**更改密码**

必须使用 Password() 函数

```sql
SET PASSWROD FOR myuser = Password('new_password');
```

