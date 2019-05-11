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





