# *Mysql基础*
| name | full name                    |
| :--- | :--------------------------- |
| DQL  | Data Query Language          |
| DML  | Data Manipulation Language   |
| DDL  | Data Definition Language     |
| TCL  | Transaction Control Language |

#`进阶1 ：基础语法 `

> ```mysql
>特点：
> 1.  查询列表可以是：表中的字段、常量值、表达式、函数
> 2.  查询的结果是一个虚拟的表格
> ```

```mysql
SELECT a,b,c FROM tablea;
SELECT 查询列表 FROM 表名;
#查询常量值
SELECT 100;
#查询表达式
SELECT 100%98;
#查询函数
SELECT VERSION();

# 起别名
#  1.便于理解。 2.如果查询的字段有重名，使用别名可以区分开
SELECT A AS B, C AS D FROM TABLEE;
SELECT A B, C D FROM TABLEE;
```

*如果表名是关键字，加单引号  ‘name’* 

```mysql
#去重
select DISTINCT a FROM tablea；

# +号 的作用
# 只有一个功能：运算符
# 只要其中一方为null，结果肯定是null
# 查询名和姓连接成一个字段，并显示为newName （拼接）
select IFNULL('aaa', 0) as 'newname';
SELECT CONCAT('firstname','lastname',IFNULL('aaa',0)) as 'newName';



```



## #`进阶2 ：条件查询`

>```mysql
>/*
>语法：
>select 查询列表
>from 表名
>    where 筛选条件
>    */
>    ```

```mysql 
分类：
    1. 按条件表达式筛选
       	条件运算符： > < = != <> >= <=
    2. 按逻辑表达式筛选
        逻辑运算符：
        	&& || ！
        	and or not      not()
    3. 模糊查询
    		like 
    		between and 左右临界值顺序不能颠倒
    		in
    		is null
    		
    		通配符：
    				% 任意多个字符，包括0个字符
    				_ 任意单个字符
    				\— 转义字符
    
 in:
    	where xxx in ('xxx','xxxx','xxxxx');
    	   1. 使用in更加简洁
    	   2. in列表的值类型必须一致或兼容
 
 is null
     where xxx IS NULL;
     where xxx IS NOT NULL;
    		
```

## `数据库的好处`

> 1. 可以持久化数据到本地
> 2. 结构化查询

# `进阶3`

## `排序查询`

```mysql
select  查询列表
from  表
[where筛选条件] 
order by 排序列表 [asc | desc];

1.asc : 升序   desc : 降序   默认为 升序
2.order by 子句中可以支持单个字段、多个字段、表达式、函数、别名
3.order by 子句一般时放在查询语句的最后面，limit子句除外
```

>  案例： 按年薪的高低显示员工的信息和年薪【按表达式排序】（表里没有年薪字段）
>
> ```mysql
> SELECT *, salary * 12 * (1 + IFNULL(awards,0)) 年薪
> FROM info
> order by salary * 12 * (1 + IFNULL(awards,0)) desc；
> order by 年薪 desc；
> ```
>
> 案例：按姓名的长度显示员工的姓名和工资【按函数排序】
>
> ```mysql
> SELECT LENGTH(last_name) 字节长度, last_name, salary
> FROM employees
> ORDER BY LENGTH(last_name) DESC;
> ```
>
> 案例：查询员工信息，先按工资排序，再按员工编号排序【多个字段排序】
>
> ```mysql
> SELECT * 
> FROM employees
> ORDER BY salary ASC, employee_id DESC;
> ```
>
> 

# `进阶4 常见函数`

> 好处：
>
> 1. 隐藏了实现细节
> 2. 提高了代码的重用性

```mysql
select 函数名（实参列表）[from 表]；
```

## `分类`

1. 单行函数：  如 concat、 length、 ifnull等
2. 分组函数。     （做统计使用，又称为统计函数、聚合函数、组函数）

---

### `字符函数`

> utf 8 下一个汉字默认占3个字节
>
> gbk  下一个汉字默认占2个字节
>
> ```mysql
> 1. length 获取参数值的|字节|个数
> select length(“xxxx”)；
> 
> 2. concat() 拼接函数
> 
> 3.upper、lower
> select upper('join');
> 
> 案例： 将姓变大写，名变小些，然后拼接
> select concat(upper(last_name) , lower(first_name)) 姓名 from employees;
> 
> 4. substr、substring
> 索引从1开始，截取 这个数字及后面的 |字符|
> select substr('abcdefg',5) out_put;   efg
> select substr('abcdefg',1,3) out_put;   abc
> 
> 5. instr
> select INSTR('str','substr')as out_put;  
> 
> 6. trim 去前后空格
> select trim('    abc   ') as out_put;
> 去前后
> select trim('a' from 'aaaaabaaaabaaa') as out_put;  结果为 baaaab 
> 
> 7. lpad
> select LPAD('aaa',5,'*') AS out_put;  **aaa
> 
> 8. rpad
> select LPAD('aaa',5,'*') AS out_put;  aaa**
> 
> 9. replace 替换
> select replace('xxabcyyxx','xx','zz') as out_put;   zzabcyyzz
> ```
>
> 

### `数学函数`

### `日期函数`

```mysql
# now() 返回当前系统日期 + 时间
# curtime() 返回当前时间，不包含日期
# curdate() 返回当前时间，不包含时间 

```

### `其他函数`

```mysql
SELECT VERSION();
SELECT DATABASE();
SELECT USER();
```

### `流程控制函数`

```mysql
1. if 函数： if else的效果
	select if(10>5,'a','b') 备注;

2. case函数的使用 一：switch case 的效果
	case 要判断的字段或表达式
	when 常量1 then 要显示的值1或语句1;
	when 常量1 then 要显示的值1或语句1;
	when 常量1 then 要显示的值1或语句1;
	······
	else 要显示的值n或者语句n;
	end
```



### `分组函数`

功能：用作统计使用，又称为聚合函数或统计函数或组函数

分类：

sum 求和，avg求平均值，max最大值，min最小值，count计算个数



和distinct搭配

select sum(distinct age), sum(age) from people;



效率：

MYISAM存储引擎下， count(*)的效率高。

INNODB存储引擎下，count(*)和count(1)的效率较高，count(字段)效率较低

---

和分组函数一同查询的字段要求是group by后的字段



## `进阶5`

```mysql
分组查询

/*
语法：
		select 分组函数， (每个的意思)列(要求出现在group by的后面)
		from 表
		【where 筛选条件】
		group by
		分组的列表
		【order by 子句】
注意：
		查询列表必须特殊，要求是分组函数和group by后出现的字段
	
*/
```



```mysql
添加分组后的筛选
/*
语法：
		select 分组函数， (每个的意思)列(要求出现在group by的后面)
		from 表
		【where 筛选条件】
		group by
		分组的列表
		having 添加分组后的筛选条件
		【order by 子句】		
*/		
```



特点：

1. 分组查询中的筛选条件分为2类

|            |  数据源  |        位置        | 关键字 |
| :--------: | :------: | :----------------: | :----: |
| 分组前筛选 |  原始表  | group by子句的前面 | where  |
| 分组后筛选 | 筛选后表 | group by子句的后面 | having |

- 分组函数做条件肯定是放在having子句中
- 能用分组前筛选的，优先考虑使用分组前筛选



2. group by子句支持单个字段分组，多个字段分组
3. 也可以添加排序，排序放在整个分组查询的最后

---

## `进阶6 连接查询`

又称：多表查询

笛卡尔乘积现象： 表1 有m行，表2 有 n行， 结果m*n 行

发生原因：没有有效的连接条件

如何避免：添加有效的连接条件



笛卡尔集的错误情况

```mysql
select count(*) from beauty;
输出  12
select count(*) from boys;
输出  4 行

select name,boyname from beauty,boys;
输出48行
```



分类：

​	按功能分类：

  - 按功能分类

    sql 99标准： 

      - 内连接：
        		- 等值连接
        		- 非等值连接
        		- 自连接
      - 外连接：
        		- 左外连接
        		- 右外连接 
        		- 全外连接
    		- 交叉连接

> 可以为表起别名
>
> 如果为表取了别名，则查询的字段就不能用原来的表名去限定。

```mysql
自连接

select e.employee_id , e.last_name , m.employee_id, m.last_name
From employees e , employees m
Where e.'manager_id' = m.'employee_id';
```



---

Sql 99语法

```mysql
/*
语法：
		select 查询列表
		from 表1 别名  【连接类型】
		join 表2 别名
		on 连接条件
		【where 筛选条件】
		【group by 分组】
		【having 筛选条件】
		【order by 排序列表】
		

分类：
	内连接：  inner
	外连接：  左外： left 【outer】
	         右外： right 【outer】
	         全外： full  【outer】
  交叉连接：
           cross
*/


select e.name from people e, people m
where e.age = max(age); 


/*找出年龄最大的同学*/

select name from people where age = (select max(age) from people);
```



```mysql
/*
非等值连接
*/

查询员工薪资的级别（2个表）
select e.name,j.grade_level from employee_salary e, job_grades j where e.salary between j.lowest_sal and j.highest_sal order by j.grade_level ;
```



内连接

```mysql
select 查询列表 
from table1  别名1
inner join table2  别名2
on 连接条件;

查询员工薪资的级别（2个表）
select name,grade_level 
from employee_salary table1  
inner join job_grades table2 
on 
table1.salary between table2.lowest_sal and table2.highest_sal 
order by table2.grade_level desc;

```



外连接

```mysql
/* 
应用场景：用于查询一个表中有，另一个表中没有的记录
1. 外连接的查询结果为主表中的所有记录
    外连接查询结果 = 内连接结果 + 主表中有而从表没有得记录。 

2.左外连接， left join 左边的是主表
  右外连接， right join 右边的是主表

3.左外和右外交换2个表的顺序，可以实现同样的效果


*/
```



### `子查询`

出现在其他语句中的select语句，称为子查询或内查询

外部的查询语句，称为主查询或外查询



```mysql
select xxx from xxx where  a in/ not in (select xx from xx);
select xxx from xxx where  a > any/ some (select xx from xx);
select xxx from xxx where  a > all (select xx from xx);
```



exists后面（相关子查询）

语法：

select  exists（完整的查询语句）



### `分页查询`

应用场景：当前要显示的数据，一页显示不全，需要分页提交sql请求

```mysql
语法：
 	  select 查询列表
		from 表1 别名  【连接类型】
		join 表2 别名
		on 连接条件
		【where 筛选条件】
		【group by 分组】
		【having 筛选条件】
		【order by 排序列表】
		limit offset,size;
		
		offset:要显示条目的启示索引（起始索引从0开始）
		size：要显示的条目个数
```



Union 联合查询

```mysql
select xxx from xxx where xxx
union [ALL | Distinct]
select xxx from xxx where xxx;
```

默认会去掉重复项



# `DML Data Manipulation Language`

数据操作语言：

​	插入 ： insert

​	修改： update

​	删除： delete



插入语法

```mysql
方法1 ：
insert into 表名（列名,...）values(值1,...);

1.插入的值的类型要与列的类型一致或兼容

方法2 ：
insert into 表名
set 列名=值,列名 = 值,...

方法一支持子查询，方法二不支持
```



修改语法

```mysql
1.修改单表的记录

语法
update 表名
set 列 = 新值,列 = 新值,列 = 新值,...
where 筛选条件;

2.修改多表的记录

语法
...
```



删除语法

```mysql
 方法一 delete：
 1. 单表的删除
 delete from 表名 where 筛选条件
 
 2.多表的删除
 ...
 
 方法二 truncate
 语法： truncate table 表名;

```





# `DDL 数据定义语言`

DDL是对库、表 属性的操作。DML是对数据的操作

## `一、库的管理`

创建 ：create

```mysql
create database if not exists books;
```



修改：alter

```mysql
修改库名
rename database books to newtablename; 已经不能用了

修改库的字符集
alter database books character set gbk;
```



删除：drop

```mysql
drop database if exists books;
```





## `二、表的管理`

创建：create

```mysql
create table 表名(
    列名 列的类型【(长度) 列的约束】,
    列名 列的类型【(长度) 列的约束】,
    列名 列的类型【(长度) 列的约束】,
    ...
    列名 列的类型【(长度) 列的约束】
);
```



表的修改：

1. 修改列名

   ```mysql
   alter table 表名 change column 原列名 新列名 类型;
   ```

2. 修改列的类型或约束

   ```mysql
   alter table 表名 modify column 列名 新类型;
   ```

3. 添加新列

     ```mysql
   alter table 表名 add column 新列名 类型;
   ```

4. 删除列

   ```mysql
   alter table 表名 drop column 列名;
   ```

5. 修改表名

   ```mysql
   alter table 表名 rename to 新表名
   ```



归纳：

```mysql
alter table 表名 add|modify|change|drop column 列名 【列类型 约束】; 
```



表的删除

```mysql
drop table 表名;
drop table if exists 表名;
```



表的复制

1.复制表的结构

```mysql
create table copy表 like 原表;
```

2.复制表的结构 + 数据

```mysql
create table copy表 
select * from 原表;
```

3.只复制部分数据

```mysql
create table copy3
select id, au_name
from author
where nation='中国';
```

4.只复制某些字段

```mysql
create table copy4
select id, au_name
from author
where 0;
```



### `数据类型`

常见的数据类型 

```mysql
数值型：
		整型
		小数：
				定点数  ： DEC(M,D)     DECIMAL(M,D) 精确度要求高
				浮点数  ： float 4字节 8B、  double  8字节  16B

字符型：
		较短的文本 ：char（固定长度） 默认长度为1、varchar（可变长度）必须要写长度
		较长的文本 ：text, blob（较长的二进制数据）

日期型：
    date      4字节      1000-01-01             9999-12-31
    datetime  8字节      1000-01-01 00:00:00    9999-12-31 00:00:00   
    timestamp 4字节
    time      3字节
    year      1字节
    
    timestamp和实际时区有关,时区改变，值改变
```



### `常见约束`

分类：  六大约束

```mysql
NOT NULL : 非空
DEFAULT : 默认，用于保证该字段有默认值
PRIMARY KEY : 主键， 用于保证该字段的值具有唯一性，并且非空
UNIQUE : 唯一，用于保证该字段的值具有唯一性，可以为空
CHECK : 检查约束，【mysql中不支持】
FOREIGN KEY : 外键，用于限制两个表的关系，用于保证该字段的值必须来自于主表的关联列的值
              在从表添加外键约束，用于引用主表中某列的值
```

约束的添加分类：

   - 列级约束：
     				- 六大约束语法上都支持，但外键约束没有效果
   - 表级约束：
     				- 除了非空，默认。其他都支持



添加列级约束

```mysql
DROP TABLE IF EXISTS major;
CREATE TABLE major(
id INT primary key auto_increment,
marjorName VARCHAR(20)
);


DROP TABLE IF EXISTS stuinfo；
CREATE TABLE stuinfo(
id INT primary key auto_increment,
stuNmae VARCHAR(20) NOT NULL,
gender CHAR(1) CHECK(gender='male' OR gender = 'female'),
seat INT UNIQUE,
age INT DEFAULT 18,
majorId INT REFERENCES major(id)
);
```

添加表级约束

```mysql
语法：
    在各个字段的最下面
    【constraint 约束名】 约束类型(字段名)
#表级约束
DROP TABLE IF EXISTS studentInfo;
CREATE TABLE iF NOT EXISTS studentInfo(
id INT,
stuNmae VARCHAR(20) ,
gender CHAR(1) ,
seat INT UNIQUE,
age INT ,
majorId INT,
CONSTRAINT pk primary key(id),
CONSTRAINT uq UNIQUE(seat),
constraint ck CHECK(gender='male' OR gender = 'female'),
CONSTRAINT fk_studentInfo_major FOREIGN KEY(majorId) REFERENCES major(id)
);
```



PRIMARY KEY 和 UNIQUE的区别

|      | 保证唯一性 | 是否允许非空 | 一个表中可以有多少个 | 是否可以组合   |
| :--: | :--------: | :----------: | :------------------: | -------------- |
| 主键 |     是     |      no      |      至多有1个       | 可以，但不推荐 |
| 唯一 |     是     |     yes      |      可以有多个      | 可以，但不推荐 |

组合主键，必须都一样，才报错



外键： 	

- 要求在从表设置外键关系
- 从表的外键列的类型和主表的关联列的类型要求一致或兼容，名称无要求
- 主表的关联列必须是一个key（一般是主键或唯一）
- 插入数据时，先插入主表的数据，再插入从表的数据
- 删除数据时，先删除从表，再删除主表



### `标识列`

又称为自增长列，系统提供默认的序列值，从 1 开始

variables ：   auto_increment_increment   :  1  步长

​						auto_increment_offset  :   1         起始值  （不支持设置） 可以手动在某个数据处修改值，后面的会受影响



####特点：

- 标识列不一定要和主键搭配，但要求是一个 key
- 一个表只能有一个标识列
- 标识列的类型只能是数值型
- 标识列可以通过 set auto_increment_increment = 3; 设置步长

---

# `TCL`

Transaction Control Language  事务控制语言

## 存储引擎：

概念： 在mysql中的数据用各种不同的存储技术 存储在文件（或内存）中。

事务的ACID属性：

1. 原子性（Automicity）
2. 一致性（Consistency）
3. 隔离性（Isolation）
4. 持久性（Durability）



步骤1: 开启事务

​				Set autocommit = 0;

​				start transaction;       可选的

步骤2:编写事务中的sql语句（select insert update delete）

​				语句1;

​				语句2;

​				...

步骤3: 结束事务

​				commit;     提交事务

​				rollback;    回滚事务 



---

- 脏读： 对于两个事务T1, T2. T1读取了已经被T2更新但还没有提交的字段之后， 若T2 回滚，T1读取的内容就是临时且无效的。（跟幻读比，针对的是更新）
- 不可重复读：对于两个事务T1, T2. T1读取了一个字段，然后T2更新了该字段之后，T1再次读取同一个字段，值就不同了。
- 幻读：对于两个事务T1, T2. T1读取了一个字段，然后T2在该表中*插入*了一些新的行之后，如果T1再次读取同一个表，就会多出几行 （和脏读相比，针对的是 插入和删除）



### 数据库事务的隔离性

|            隔离级别            |                          保证唯一性                          |
| :----------------------------: | :----------------------------------------------------------: |
| READ UNCOMMITTED(读未提交数据) |                              是                              |
|    READ COMMITTED(读已提交)    |                              是                              |
|   REPEATABLE READ(可重复读)    | 确保事务可以多次从一个字段中读取相同的值。在这个事务持续期间，禁止其他事务对这个字段进行更新。可以避免脏读和不可重复读，但幻读的问题仍然存在。 |
|      SERIALIZABLE(串行化)      | 确保事务可以从一个表中读取相同的行。在这个事务持续期间，禁止其他事务对该表执行插入，更新和删除操作。所有并发问题都可以避免，但性能低下。 |

SERIALIZABLE(串行化) ： 阻塞操作，等待释放锁（等待其他事务commit）

默认的隔离级别是：   REPEATABLE-READ  可重复读

```mysql
/* 查看当前的隔离级别*/
select @@tx_isolation;

设置当前mysql的隔离级别
set transcation isolation level read committed;

设置全局的隔离级别
set global transcation isolation level read committed;
```



savepoint 节点名，设置保存点，rollback到保存点 

只能搭配 rollback

 ```mysql
set autocommit=0;
start transcation;
delete from table1 where id = 1;
savepoint a;设置保存点
delete from table1 where id = 2;
rollback to a;回滚到保存点
 ```

### 视图

含义：虚拟表 ，包装了结果集

一种虚拟的表，行和列的数据来自定义视图的查询中使用的表，并且是在使用视图时，动态生成的，只保存了sql逻辑，不保存查询结果。



### 变量

- 系统变量：
  - 全局变量
  - 会话变量
- 自定义变量
  - 用户变量
  - 局部变量



1. 查看所有的系统变量

```mysql
show global | 【session】 variables;
```

2. 查看满足条件的部分系统变量

```mysql
show global | 【session】 variables like '%char%';
```

3. 查看置顶的某个系统变量的值

```mysql
select @@global |【session】 .系统变量名
```

4. 为某个系统变量赋值

```mysql
set global | 【session】 系统变量名 = 值;
```

