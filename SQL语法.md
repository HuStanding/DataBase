# 1. 启动数据库

```SQL
mysql -u root -p
```
# 2. 显示和创建数据库
```sql
show databases;
create databse 数据库名;
```

# 3. 显示和创建表格

```sql
show tables;  #查看有哪些表格
create table mytable(id int(10),name char(20));
```

# 4. 插入数据
+ 普通插入
```sql
insert into mytable(id,name) values(1,'Elvan');  #这里的(id,name)也可以省略
```
+ 插入检索出来的数据
```sql
insert into mytable(id,name) select id,name from mytable;
```
+ 将一个表的内容插入一个新表
```sql
create table newtable as select * from mytable;
```

# 5. 修改表
+ 添加列
```sql
alter table mytable add newcol char(20);
```
+ 如果在第一列之前插入的话应该使用
```sql
alter table mytable add column newcol int(15) first;
```

+ 删除列
```sql
alter table mytable drop column newcol;
```
+ 删除表
```sql
drop table mytable;
```
+ 表的重命名
```sql
rename table 原名 to 新名;
alter table 原名 rename 新名;
alter table 原名 rename to 新名;
```

# 6. 删除数据库
```sql
drop database 数据库名;
```

# 7. sql查询命令

+ 基本的查询
```sql
select col1 from mytable where id = 1;
```
+ distinct  
相同的字段只出现一次，作用于所有列，当所有列的值相同的时候才算相同
```sql
select distinct col1,col2 from mytable;
```

+ limit  
限制返回的行数。可以有两个参数，第一个参数为起始行，从 0 开始；第二个参数为返回的总行数。
    + 返回前5行
    ```sql
    select * from mytable limit 5;
    ```
    ```sql
    select * from mytable limit 0,5;
    ```
    + 返回第3~5行
    ```sql
    select * from mytable limit 2,3;
    ```
# 8. 更新

```sql
update mytable set col1 = value1,col2 = value2 where id = 1; 
```
# 9. 删除
+ 删除某一行的数据
```sql
delete from mytable where id = 1;
```
+ 清空表
```sql
truncate table mytable;
```
# 10.排序
```sql
order by asc;  #升序
order by desc;  #降序
select * from mytable order by id desc;
```

+ 对多列进行排序
```sql
select * from mytable order by col1 desc,col2 asc;
```

# 11.过滤
```sql
select * from mytable where col is null;
```

+ where可用的操作符

|   操作符    | 说明         |
| :---------: | :----------- |
|      =      | 等于         |
|    &lt;     | 小于         |
|    &gt;     | 大于         |
| &lt;&gt; != | 不等于       |
| &lt;= !&gt; | 小于等于     |
| &gt;= !&lt; | 大于等于     |
|   BETWEEN   | 在两个值之间 |
|   IS NULL   | 为 NULL 值   |

**AND 和 OR**用于连接多个过滤条件。优先处理 AND，当一个过滤表达式涉及到多个 AND 和 OR 时，可以使用 () 来决定优先级，使得优先级关系更清晰。

**IN** 操作符用于匹配一组值，其后也可以接一个 SELECT 子句，从而匹配子查询得到的一组值。

**NOT** 操作符用于否定一个条件。

# 12. 通配符
通配符为like，一般的有两个：
+ _  匹配一个未指定的字符
+ ? 匹配不定个未指定的字符
+ %  匹配 >=0 个任意字符；
+ []  可以匹配集合内的字符，例如 [ab] 将匹配字符 a 或者 b。用脱字符 ^ 可以对其进行否定，也就是不匹配集合内的字符。

```sql
select * from mytable where col like '[^AB]%'; #匹配不以A和B开头的任意文本
```

# 13. 内置函数
## 统计
+ count ——所有类型
+ sum，avg——数字
+ max，min——数值，字符串，日期
如：

```
select max(salary) as max_salary,min(salary) from employee;
```

## 日期和时间处理

- 日期格式：YYYY-MM-DD
- 时间格式：HH:MM:SS

|     函 数     |             说 明              |
| :-----------: | :----------------------------: |
|   AddDate()   |    增加一个日期（天、周等）    |
|   AddTime()   |    增加一个时间（时、分等）    |
|   CurDate()   |          返回当前日期          |
|   CurTime()   |          返回当前时间          |
|    Date()     |     返回日期时间的日期部分     |
|  DateDiff()   |        计算两个日期之差        |
|  Date_Add()   |     高度灵活的日期运算函数     |
| Date_Format() |  返回一个格式化的日期或时间串  |
|     Day()     |     返回一个日期的天数部分     |
|  DayOfWeek()  | 对于一个日期，返回对应的星期几 |
|    Hour()     |     返回一个时间的小时部分     |
|   Minute()    |     返回一个时间的分钟部分     |
|    Month()    |     返回一个日期的月份部分     |
|     Now()     |       返回当前日期和时间       |
|   Second()    |      返回一个时间的秒部分      |
|    Time()     |   返回一个日期时间的时间部分   |
|    Year()     |     返回一个日期的年份部分     |

```sql
mysql> SELECT NOW();
```

```sql
2019-02-28 17:10:11
```

## 数值处理
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

# 14. 分组

分组就是把具有相同的数据值的行放在同一组中。

可以对同一分组数据使用汇总函数进行处理，例如求分组数据的平均值等。

指定的分组字段除了能按该字段进行分组，也会自动按该字段进行排序。

```sql
select col, count(*) as num
from mytable
group by col;
```

GROUP BY 自动按分组字段进行排序，ORDER BY 也可以按汇总字段来进行排序。

```sql
select col, count(*) as num
from mytable
group by col
order by num;
```

WHERE 过滤行，HAVING 过滤分组，行过滤应当先于分组过滤。

```sql
select col, count(*) as num
from mytable where col > 2
group by col having num >= 2;
```

分组规定：

- **GROUP BY 子句出现在 WHERE 子句之后，ORDER BY 子句之前**；
- 除了汇总字段外，SELECT 语句中的每一字段都必须在 GROUP BY 子句中给出；
- NULL 的行会单独分为一组；
- 大多数 SQL 实现不支持 GROUP BY列具有可变长度的数据类型。



# 15. 子查询（使用括号）
子查询中只能返回一个字段的数据。

可以将子查询的结果作为 WHRER 语句的过滤条件：

```sql
select * from shop where price=(select max(price) from shop);
```
下面的语句可以检索出客户的订单数量，子查询语句会对第一个查询检索出的每个客户执行一次：

```sql
select cust_name, (select COUNT(*)
                   from Orders
                   where Orders.cust_id = Customers.cust_id)
                   as orders_num
from Customers
order by cust_name;
```

# 16. 连接
连接用于连接多个表，使用 join 关键字，并且条件语句使用 **on** 而不是 where。

连接可以替换子查询，并且比子查询的效率一般会更快。

可以用 as 给列名、计算字段和表名取别名，给表名取别名是为了简化 SQL 语句以及连接相同表。

## 内连接

内连接又称等值连接，使用 inner join 关键字。

```sql
select A.value, B.value
from tablea as A inner join tableb as B
on A.key = B.key;
```

可以不明确使用 INNER JOIN，而使用普通查询并在 WHERE 中将两个表中要连接的列用等值方法连接起来。

```sql
select A.value, B.value
from tablea as A , tableb as B
on A.key = B.key;
```

在没有条件语句的情况下返回笛卡尔积。

## 自连接

自连接可以看成内连接的一种，只是连接的表是自身而已。

一张员工表，包含员工姓名和员工所属部门，要找出与 Jim 处在同一部门的所有员工姓名。

+ 子查询版本

```sql
select name
from employee
where department = (
      select department
      from employee
      where name = "Jim");
```

+ 自连接版本

```sql
select e1.name
from employee as e1 inner join employee as e2
on e1.department = e2.department
      and e2.name = "Jim";
```

## 自然连接

自然连接是把同名列通过等值测试连接起来的，同名列可以有多个。

内连接和自然连接的区别：**内连接提供连接的列，而自然连接自动连接所有同名列。**

```sql
select A.value, B.value
from tablea as A natual join tableb as B;
```

## 外连接

外连接保留了没有关联的那些行。分为左外连接，右外连接以及全外连接，左外连接就是保留左表没有关联的行。

检索所有顾客的订单信息，包括还没有订单信息的顾客。

```sql
select Customers, Orders.order_id
from Customers left join Orders
on Customers.cust_id = Orders.cust_id;
```

customers 表：

| cust_id | cust_name |
| :---: | :---: |
| 1 | a |
| 2 | b |
| 3 | c |

orders 表：

| order_id | cust_id |
| :---: | :---: |
|1    | 1 |
|2    | 1 |
|3    | 3 |
|4    | 3 |

结果：

| cust_id | cust_name | order_id |
| :---: | :---: | :---: |
| 1 | a | 1 |
| 1 | a | 2 |
| 3 | c | 3 |
| 3 | c | 4 |
| 2 | b | Null |


# 17.组合查询

使用  **UNION**  来组合两个查询，如果第一个查询返回 M 行，第二个查询返回 N 行，那么组合查询的结果一般为 M+N 行。

每个查询必须包含相同的列、表达式和聚集函数。

默认会去除相同行，如果需要保留相同行，使用 UNION ALL。

只能包含一个 ORDER BY 子句，并且必须位于语句的最后。

```sql
select col
from mytable
where col = 1
union
select col
from mytable
where col =2;
```

# 18. 视图

视图是虚拟的表，本身不包含数据，也就不能对其进行索引操作。

对视图的操作和对普通表的操作一样。

视图具有如下好处：

- 简化复杂的 SQL 操作，比如复杂的连接；
- 只使用实际表的一部分数据；
- 通过只给用户访问视图的权限，保证数据的安全性；
- 更改数据格式和表示。

```sql
create view myview as
select Concat(col1, col2) as concat_col, col3*col4 as compute_col
from mytable
where col5 = val;
```

# 19. 游标

在存储过程中使用游标可以对一个结果集进行移动遍历。

游标主要用于交互式应用，其中用户需要对数据集中的任意行进行浏览和修改。

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

# 20. 触发器

触发器会在某个表执行以下语句时而自动执行：DELETE、INSERT、UPDATE。

触发器必须指定在语句执行之前还是之后自动执行，之前执行使用 BEFORE 关键字，之后执行使用 AFTER 关键字。BEFORE 用于数据验证和净化，AFTER 用于审计跟踪，将修改记录到另外一张表中。

INSERT 触发器包含一个名为 NEW 的虚拟表。

```sql
CREATE TRIGGER mytrigger AFTER INSERT ON mytable
FOR EACH ROW SELECT NEW.col into @result;

SELECT @result; -- 获取结果
```

DELETE 触发器包含一个名为 OLD 的虚拟表，并且是只读的。

UPDATE 触发器包含一个名为 NEW 和一个名为 OLD 的虚拟表，其中 NEW 是可以被修改的，而 OLD 是只读的。

MySQL 不允许在触发器中使用 CALL 语句，也就是不能调用存储过程。

# 21. 事务管理

基本术语：

- 事务（transaction）指一组 SQL 语句；
- 回退（rollback）指撤销指定 SQL 语句的过程；
- 提交（commit）指将未存储的 SQL 语句结果写入数据库表；
- 保留点（savepoint）指事务处理中设置的临时占位符（placeholder），你可以对它发布回退（与回退整个事务处理不同）。

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

# 22. 字符集

基本术语：

- 字符集为字母和符号的集合；
- 编码为某个字符集成员的内部表示；
- 校对字符指定如何比较，主要用于排序和分组。

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

# 23.权限管理

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
DROP USER myuser;
```

**查看权限** 

```sql
SHOW GRANTS FOR myuser;
```

**授予权限** 

账户用 username@host 的形式定义，username@% 使用的是默认主机名。

```sql
GRANT SELECT, INSERT ON mydatabase.* TO myuser;
```

**删除权限** 

GRANT 和 REVOKE 可在几个层次上控制访问权限：

- 整个服务器，使用 GRANT ALL 和 REVOKE ALL；
- 整个数据库，使用 ON database.\*；
- 特定的表，使用 ON database.table；
- 特定的列；
- 特定的存储过程。

```sql
REVOKE SELECT, INSERT ON mydatabase.* FROM myuser;
```

**更改密码** 

必须使用 Password() 函数

```sql
SET PASSWROD FOR myuser = Password('new_password');
```

# 参考资料

- [CyC2018/CS-Notes/docs/notes/SQL.md](https://github.com/CyC2018/CS-Notes/blob/master/docs/notes/SQL.md)
