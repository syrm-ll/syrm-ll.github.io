# 语法格式

## DDL：数据定义语言

### create

```sql
-- 创建数据库
create database "databaseName";
-- 创建数据表
create table "tableName"(
	"colum" 	dataType [primary key] [identity] [not null] [check boolean ]
    ...
    [
        constraint "cName" 
     	[foreign key ("colName") references "tableName"("colName")]
        [primary key ("colName",...)]
        [default | unique | ckeck ] ...
    ]
);

-- 创建视图
create view "viewName" as select ...;

-- 创建触发器
create trigger "triggerName" on "tableName"
after [insert] | [update] | [delete]
as
begin 
-- 触发操作
end

-- 创建索引
create [unique ] index indexName
on "tableName"(columnName);
```

###  drop

```sql
-- 和create类似， 略
```



### alter

```sql
-- 如需在表中添加列，请使用下面的语法:
ALTER TABLE table_name
ADD column_name datatype

-- 如需删除表中的列，请使用下面的语法（请注意，某些数据库系统不允许这种在数据库表中删除列的方式）：
ALTER TABLE table_name
DROP COLUMN column_name

-- 要改变表中列的数据类型，请使用下面的语法：
-- SQL Server / MS Access：
ALTER TABLE table_name
ALTER COLUMN column_name datatype
-- My SQL / Oracle：
ALTER TABLE table_name

--MODIFY COLUMN column_name datatype
--Oracle 10G 之后版本:
ALTER TABLE table_name
MODIFY column_name datatype;
```



## DML：数据操作语言

### insert

```sql
insert into "tableName" (column, ...) values (value, ...);
```

### delete

```sql
delete from "tableName" where boolean;
```

>   注意： 如果where被省略，则会删除整张表所有记录。
>
>   ​					为了安全，很多服务器禁止不带where的delete语句执行，如果确认需要执行可以使用：
>
>   ```sql
>   delete from "tableName" where 1 = 1;
>   --和
>   truncate table "tableName";
>   ```
>
>   >   【关于truncate】： 
>   >
>   >   truncate table 表名 速度快,而且效率高,因为:
>   >
>   >   　　truncate table 在功能上与不带 WHERE 子句的 DELETE 语句相同：二者均删除表中的全部行。但 truncate table 比 DELETE 速度快，且使用的系统和事务日志资源少。
>   >
>   >   　　DELETE 语句每次删除一行，并在事务日志中为所删除的每行记录一项。truncate table 通过释放存储表数据所用的数据页来删除数据，并且只在事务日志中记录页的释放。
>   >
>   >   　　truncate table 删除表中的所有行，但表结构及其列、约束、索引等保持不变。新行标识所用的计数值重置为该列的种子。
>   >   如果想保留标识计数值，请改用 DELETE。
>   >   如果要删除表定义及其数据，请使用 DROP table 语句。
>   >
>   >   　　对于由 FOREIGN KEY 约束引用的表，不能使用 truncate table，而应使用不带 WHERE 子句的 DELETE 语句。由于 truncate table 不记录在日志中，所以它不能激活触发器。
>   >
>   >   　　truncate table 不能用于参与了索引视图的表。
>   >
>   >   　　对用truncate table删除数据的表上增加数据时，要使用UPDATE STATISTICS来维护索引信息。
>   >
>   >   　　如果有ROLLBACK语句，DELETE操作将被撤销，但truncate不会撤销。

### update

```sql
update "tableName" 
set "colum" = value [, colum = value]...
where true | false;
```

>   注意： where 如果被省略，则所有记录都会被更新。

### select

```sql
select [top num] [distinct] columns list | *
from "tableName" ,...
[where true | false ]
[group by "colum"]
[having true | false ]
[order by "colum" [desc]];
```



## DCL：数据控制语言





# 特殊

## where： and, or,  like, not like, in, between and

## 数据完整性： 约束 触发器

## 索引

## 存储过程

## 批处理

## 系统函数





> 以上为历史数据

# ---------------------------

# SQL

# 一, 基础

SQL概要, 创建表,算数 比较 逻辑 运算符  略

# 二, 基础操作

## 查询: select

### 聚合和排序

#### 聚合(函数)

常用聚合函数:

```sql
-- 计数, 入参为 * 统计所有行数, 入参为列,统计所有非空记录(忽略NULL)
count()
-- 求和
sum()
-- 平均
avg()
-- 取最大
max()
-- 取最小
min()
```



> 聚合函数中还可以使用 `DISTINCT` 去重:
>
> ```sql
> select count(distinct colums )
> 
> select sum(distinct colums)
> ```



#### 分组: group by

语法:

```sql
SELECT <列名1>, <列名2>, <列名3>, ……
 FROM <表名>
 GROUP BY <列名1>, <列名2>, <列名3>, ……
HAVING <分组结果对应的条件>
```

示例:

```sql
select type, avg(sale_price)
from product
group by type
having avg(sale_price) > 2000;
```



> 筛选条件优先放在`where` 中, 有较好性能.



#### 排序

语法:

```sql
-- ASC 升序, DESC  降序. 默认 ASC 省略
select * from TableName 
order by colum ASC | DESC 
```



> **▲使用 HAVING 子句时 SELECT 语句的顺序 FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY**
>
> SELECT 子 句的执行顺序在 GROUP BY 子句之后，ORDER BY 子句之前。因此，在执 行 GROUP BY 子句时，SELECT 语句中定义的别名无法被识别
>
> 对于 在 SELECT 子句之后执行的 ORDER BY 子句来说，就没有这样的问题了。



**order by 子句中可以使用列名(不存在select 中的表列也行), 列别名, 列ID(不推荐)**



## 插入: insert

### 语法

```sql
insert into 表名 (col1, col2, ...) values (val1, val2, ...);
```

**注意:**

- 当列名列表和表中列完全相符(数量和顺序)时, 可以省略.
- values 和 value 相等, 推荐使用values

```sql
-- 显示插入null
... values (null, null)

-- 显式插入默认值.
... values (default ...)
```

> **如果没有列清单中指定列名, 此次插入使用默认值, 如果没有设置默认值, 自动插入`NULL`, 如果设置了 `NOT NULL` 约束, 报错.**



### 多行插入

```sql
-- 仅适用于  DB2、SQL、SQL Server、PostgreSQL 和 MySQL
insert into 表名 (col1, col2, ...) values
(val1, val2, ...),
......;

-- oracle
INSERT ALL INTO ProductIns VALUES ('0002', '打孔器', 
'办公用品', 500, 320, '2009-09-11')
 INTO ProductIns VALUES ('0003', '运动T恤', 
'衣服', 4000, 2800, NULL)
 INTO ProductIns VALUES ('0004', '菜刀', 
'厨房用具', 3000, 2800, '2009-09-20')
SELECT * FROM DUAL;
```

### 查询插入: insert ... select

```sql
-- 这个select 语句只要最后返回的列数量和数据类型和 A表相同即可.
-- 表A必须存在, 否则报错.
insert into 表A (列清单)
select 列清单 from 表B ;
```







## 更新: update

### 语法

```sql
-- 通用写法
update 表名
	set 列名 = 值,
		列名 = 值,
		...
	[where boolean ]

-- 个别DBMS支持: PostgreSQL和 DB2
update 表名
	set (列1, 列2 ...) = (值1, 值2 ...)
	[where boolean ]
```





## 删除: delete

### 语法

```sql
delete from 表名
[where boolean ]
```

这里, where 子句只能使用简单的表达式, 不能使用 `group by ` `having` 和 子查询.



> 标准 SQL 中用来从表中删除数据的只有 DELETE 语句。但是，很多数据库产 品中还存在另外一种被称为 TRUNCATE 的语句。这些产品主要包括 Oracle、SQL Server、PostgreSQL、MySQL 和 DB2。 
>
> TRUNCATE 是舍弃的意思，具体的使用方法:
>
> ```sql
> truncate <表名>;
> ```
>
>  DELETE 不同的是，TRUNCATE 只能删除表中的全部数据，而不能通过 WHERE 子句指定条件来删除部分数据。也正是因为它不能具体地控制删除对象， 所以其处理速度比 DELETE 要快得多。实际上，DELETE 语句在 DML 语句中也 属于处理时间比较长的，因此需要删除全部数据行时，使用 TRUNCATE 可以缩短 执行时间。 但是，产品不同需要注意的地方也不尽相同。例如在 Oracle 中，把 TRUNCATE 定义为 DDL，而不是 DMLA。使用 TRUNCATE 时，请大家仔细阅读使用手册， 多加注意。便利的工具往往还是会存在一些不足之处的。
>
> **因 此，Oracle中 的TRUNCATE 不能使用 ROLLBACK。执 行 TRUNCATE的同时会默认执行 COMMIT操作。**
>
> **附: mysql 8+ 也无法回滚 ╮(╯▽╰)╭**



# 事务

## 事务的特性: ACID

> 原子性（Atomicity）、一致性（Consistency）、隔离性 （Isolation）和持久性（Durability）**四种特性**

## 语法

由开始语句和结束语句包围的DML语句,组成一个事务.

```sql
-- 开始语句: 没有统一的标准
-- SQL Server、PostgreSQL
begin transaction
-- MySQL
start transaction
-- Oracle、DB2
无, 即不需要显示声明

-- 结束语句
commit | rollback
```



**自动处理的事务**

不使用指令而悄悄开始事务的情况下，应该如何区分各个事务呢？通常 会有如下两种情况。 

- 每条SQL语句就是一个事务（自动提交模式） 

- 直到用户执行COMMIT或者ROLLBACK为止算作一个事务



# 三, 复杂查询

## 视图

### 语法

```sql
-- 创建
-- select 中不能使用 order by 排序
create view 视图名(列清单)
as 
<select .....>;

-- 删除
drop view 视图名称;

-- 在PostgreSQL 删除关联视图
drop view 视图名称 cascade;
```

> **多重 视图会降低SQL查询性能, 尽量避免使用**



### 视图更新

标准SQL规定, 满足一下条件,可以更新视图:

- ① SELECT 子句中未使用 DISTINCT 
- ② FROM 子句中只有一张表 
- ③ 未使用 GROUP BY 子句 
- ④ 未使用 HAVING 子句 



## 子查询

略



执行顺序: 子查询 -> 外层查询

## 关联查询

自我关联查询存疑:

TODO:

```sql
select type, name, sale_price
from product p1
where sale_price > (
    select avg(p1.sale_price) as sale
    from product p1, product p2
    where p1.type = p2.type
    );
-- 上面的子查询只会返回一个结果. 不能添加group by (演示案例的PTSQL没有测试)
-- 返回的结果不是总数的平均, 也不是group by 分类平均, 不知道是啥 绝望
```

> SQL基础教程, 第二版,5-3 关联子查询案例存疑



# 四, 函数,谓词,case表达式

## 函数

### 算数函数

加减乘除运算符

```sql
-- 绝对值
ABS()
-- 取余数
MOD(被除数, 除数)
-- 四舍五入
ROUND(对象, 保留小数位数)
```

### 字符串函数

```sql
-- 字符串拼接,(SQL server, MySql 不能用)
str1 || str2 || str3 ....
-- 求字符串长度, SQL Server 使用 Len(str), mysql 使用: char_length()
length(str)
-- 转小写
lower()
-- 转大写
upper()
-- 字符串替换
replact(对象字符串, 替换前, 替换后)

-- 字符串截取, 不同DBMS实现不同, 可能使用byte计算长度, 并且只有  PostgreSQL 和 MySQL 支持
substring(对象字符串 from 开始位置 for 截取长度 )
```

### 日期函数

```sql
-- 获取当前日期 mysql
current_date
-- 获取当前时间 mysql
current_time
-- 获取当前日期时间, sqlserver, ptsql, mysql
current_stamp
```

上面函数无法在 SQL Server 中执行。此外，Oracle 和 DB2 中的语法略 有不同。

```sql
-- SQL Server
-- 使用CAST（后述）函数将CURRENT_TIMESTAMP转换为日期类型
SELECT CAST(CURRENT_TIMESTAMP AS DATE) AS CUR_DATE;
SELECT CAST(CURRENT_TIMESTAMP AS TIME) AS CUR_TIME;

-- oracle  指定临时表（DUAL）
SELECT CURRENT_DATE FROM dual;
SELECT CURRENT_TIMESTAMP FROM dual;
-- SELECT CURRENT_TIMESTAMP FROM dual;

-- DB2 CURRENT和TIME之间使用了半角空格，指定临时表SYSIBM.SYSDUMMY1
SELECT CURRENT DATE FROM SYSIBM.SYSDUMMY1;
SELECT CURRENT TIME FROM SYSIBM.SYSDUMMY1;
SELECT CURRENT TIMESTAMP FROM SYSIBM.SYSDUMMY1;
```



```sql
-- 截取日期, ptsql, mysql
extract(单位 from 日期类型数据)
-- 例:
SELECT CURRENT_TIMESTAMP,
       EXTRACT(YEAR FROM CURRENT_TIMESTAMP) AS year,
       EXTRACT(MONTH FROM CURRENT_TIMESTAMP) AS month,
       EXTRACT(DAY FROM CURRENT_TIMESTAMP) AS day,
       EXTRACT(HOUR FROM CURRENT_TIMESTAMP) AS hour,
       EXTRACT(MINUTE FROM CURRENT_TIMESTAMP) AS minute,
       EXTRACT(SECOND FROM CURRENT_TIMESTAMP) AS second;            
```

同样,sqlserver, oracle, db2 需要特殊写法:

```sql
-- SQL Server
SELECT CURRENT_TIMESTAMP,
 DATEPART(YEAR , CURRENT_TIMESTAMP) AS year,
 DATEPART(MONTH , CURRENT_TIMESTAMP) AS month,
 DATEPART(DAY , CURRENT_TIMESTAMP) AS day,
 DATEPART(HOUR , CURRENT_TIMESTAMP) AS hour,
 DATEPART(MINUTE , CURRENT_TIMESTAMP) AS minute,
 DATEPART(SECOND , CURRENT_TIMESTAMP) AS second;
 
 -- 在FROM子句中指定临时表（DUAL）
SELECT CURRENT_TIMESTAMP,
 EXTRACT(YEAR FROM CURRENT_TIMESTAMP) AS year,
 EXTRACT(MONTH FROM CURRENT_TIMESTAMP) AS month,
 EXTRACT(DAY FROM CURRENT_TIMESTAMP) AS day,
 EXTRACT(HOUR FROM CURRENT_TIMESTAMP) AS hour,
 EXTRACT(MINUTE FROM CURRENT_TIMESTAMP) AS minute,
 EXTRACT(SECOND FROM CURRENT_TIMESTAMP) AS second
FROM DUAL;
DB2
-- CURRENT和TIME之间使用了半角空格，指定临时表SYSIBM.SYSDUMMY1 
SELECT CURRENT TIMESTAMP,
 EXTRACT(YEAR FROM CURRENT TIMESTAMP) AS year,
 EXTRACT(MONTH FROM CURRENT TIMESTAMP) AS month,
 EXTRACT(DAY FROM CURRENT TIMESTAMP) AS day,
 EXTRACT(HOUR FROM CURRENT TIMESTAMP) AS hour,
 EXTRACT(MINUTE FROM CURRENT TIMESTAMP) AS minute,
 EXTRACT(SECOND FROM CURRENT TIMESTAMP) AS second
 FROM SYSIBM.SYSDUMMY1;
```

### 转换函数

cast 数据类型转换

```sql
CAST（转换前的值 AS 想要转换的数据类型）

-- SQL Server PostgreSQL
SELECT CAST('0001' AS INTEGER) AS int_col;
-- MySQL
SELECT CAST('0001' AS SIGNED INTEGER) AS int_col;
-- Oracle
SELECT CAST('0001' AS INTEGER) AS int_col FROM DUAL;
-- DB2
SELECT CAST('0001' AS INTEGER) AS int_col FROM SYSIBM.SYSDUMMY1;
```

## 谓词

like

between

is null

is not null

in / or / not in 

exist / not exist



## case 表达式

语法:

```sql
-- 执行顺序, 依次执行when 后面的求值表达式, 为真返回 then 的值, 然后中止.
-- 搜索型case表达式
case 
	when <求值表达式> then <表达式>
	when <求值表达式> then <表达式>
	...
	else <表达式>
end	

-- 简单case表达式
case 列
	when 值 then <表达式>
	when 值 then <表达式>
	...
end
```

特定case表达式:

oracle 的 decode

mysql 的 if 



# 五, 集合运算

## 表联合

**注意事项:**

1. 作为运算对象的集合的列数, 列类型,必须一致.
2. `order by` 只能在最后使用一次.

> mysql 确认可以使用order by, 但是没啥效果  ╮(╯▽╰)╭

union: 并集

union / union all

intersect: 交集

except: 差集

## 表连接: join

**内连接: inner join**



**外连接: outer join**



**交叉连接: cross join**

不写 on 条件时, 结果集是 `笛卡尔积` 即两张表记录的乘积

> **交叉联结没有应用到实际业务之中的原因有两个。一是其结果没有实 用价值，二是由于其结果行数太多，需要花费大量的运算时间和高性能设 备的支持。**



# 窗口函数



略







# -----------------------------------

# SQL 进阶



