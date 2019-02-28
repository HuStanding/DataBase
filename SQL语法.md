### mysql基本操作

+ 启动数据库

```SQL
mysql -u root -p
```
+ 显示和创建数据库
```sql
show databases;
create databse 数据库名;
```

+ 显示和创建表格

```sql
show tables;  #查看有哪些表格
create table 表格名(id int(10),name char(20));
```

+ 插入数据

```
insert into 表格名(id,name) values(1,'Elvan');  #这里的(id,name)也可以省略
```

+ 删除数据库
```
drop database 数据库名;
```

+ 添加主键

```
primary key(列1，列2);
```

### sql查询命令

基本的查询命令为
```
select 列名 from 表名 where 条件
```
当不止一个条件的时候，可以使用and或or、between连接

+ 选择在或不在情况下的结果

```
in/not in;
```

+ 通配符
通配符为like，一般的有两个：
1. _  匹配一个未指定的字符
2. ? 匹配不定个未指定的字符

+ 排序

```
order by asc;  #升序
order by desc;  #降序
select * from employee order by salary desc;
```

+ 聚类
```
group by
select 类别, sum(数量) as 数量之和 from A group by 类别;  #表示以类别分类的各类数量之和
```
+ 内置函数的计算
1. count ——所有类型
2. sum，avg——数字
3. max，min——数值，字符串，日期
如：

```
select max(salary) as max_salary,min(salary) from employee;
```
+ 子查询（使用括号）

```
select * from shop where price=(select max(price) from shop);
select * from shop order by price desc limit 1;
```

+ 表的重命名
```
rename table 原名 to 新名;
alter table 原名 rename 新名;
alter table 原名 rename to 新名;
```
+ 表的删除

```
drop table 表名;
```

+ 增加一列

```
alter table 表名 add column 列名 数据类型 约束;
```
如果在第一列之前插入的话应该使用
```
alter table 表名 add column 列名 数据类型 约束 first;
```

+ 删除一列
```
alter table 表名 drop 列名;
```

+ 重命名一列
```
alter table 表名 change 原列 新列 数据类型 约束;
```

+ 改变数据类型
```
alter table 表名 modify 列名 新数据类型;
```

+ 修改表中某个值
```
update 表名 set 列1 = 值1,列2 = 值2 where 条件; 
```

+ 删除最后一行记录
```
delete from 表名 where 条件;
```

+ 创建视图
```
create view 视图名(列1,列2) as select 列1,列2 from 表名字;
```

+ 导入导出数据
```
load data infile '路径和文件名' into table 表格名;
select 列1,列2 into outfile '路径和文件名' from 表格名;
```

+ 备份和恢复
```
mysqldump -u root mysql-shiyan > bak.sql;
mysql -u root test < bak.sql;
```

+ 验证表
```
describe 表格名;
```
+ 事务  
事务一般以begin关键字开始，commit关键字结束，表示在这之间的任何一条申sql语句失败，那么rollback命令就会把数据库恢复到begin开始之前的状态，如：  
```
begin:
    insert into salesinfo set customerid=14;
    update inventory set quantity=11 where item='book';
commint;
```







