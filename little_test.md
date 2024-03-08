# SQL 基础

## 1. DDL 语句

> 数据库操作

``` mysql
show databases; -- 查看数据库
create database 数据库名; -- 创建数据库
use 数据库名; -- 应用数据库
select database(); -- 查看当前应用的数据库
drop database 数据库名; -- 删除数据库
```

> 表操作

``` mysql
show tables; -- 查看数据库所有的表
create table 表名 (字段 字段类型[, 字段 字段类型]); -- 创建表
desc 表名; -- 查看表中的字段
show create table 表名; -- 查看建表语句
alter table 表名 (
	add -- 添加字段
  modify -- 修改字段类型
  change -- 修改字段名称和类型
  drop -- 删除字段
  rename to -- 修改表名
);
drop table 表名; -- 删除表
```



## 2. DML语句

``` mysql
/* 插入语句 */
insert into emp (id, work_code, name, gender, age, id_card, entry_date, nickname)
values (3, '100003', '小何', 3, 20, '112233445544332211', '2001-09-08', '想要桀骜不驯的小何');

/* 更新语句 */
update emp -- 更新哪张表
set age = 22 -- 更新后的值
where id = 3; -- 条件

/* 删除语句 */
delete
from emp -- 删除哪张表的数据
where id = 3; -- 条件

/* 删除整张表 */
delete from emp;
```



## 3. DQL 语句

### 3.1 基础查询

``` mysql
select 字段名[, 字段名] from 表名; -- 查询指定字段
select * from 表名; -- 查询所有字段
select 字段名 as '别名' from 表名; -- 查询指定字段并起别名
select distinct 字段名 from 表名; -- 查询字段并去重
```

### 3.2 条件查询

![image-20240305140053130](.\assets\image-20240305140053130.png)

``` mysql
-- 查询年龄等于 99
select * from 表名 where age = 99;
-- 查询年龄小于等于 20
select * from 表名 where age <= 20;
-- 查询没有性别的
select * from 表名 where gender is null;
-- 查询有性别的
select * from 表名 where gender is not null;
-- 查询年龄 15 到 20岁
select * from 表名 where age >= 15 and age <= 20;
select * from 表名 where age between 15 and 20;
-- 查询性别为女并且年龄小于20
select * from 表名 where gender = '女' and age < 20;
-- 查询年龄等于18，19，20
select * from 表名 where age in (18, 19, 20);
-- 查询姓名长度为 2 的
select * from 表名 where name like '__';
```

### 3.3 聚合函数

 ``` mysql
 -- 查询表中数据的数量
 select count(id) from 表名;
 -- 查询平均年龄
 select avg(age) from 表名;
 -- 查询最大年龄
 select max(age) from 表名;
 -- 查询最小年龄
 select min(age) from 表名;
 -- 统计年龄之和
 select sum(age) from 表名;
 ```

### 3.4 分组查询

``` mysql
-- 分组统计男性员工和女性员工的数量
select gender, count(*) from 表名 group by gender;
```

### 3.5 排序查询

``` mysql
-- 语法：selcet 字段 from 表名 order by 字段 排序方式;
-- 根据年龄升序排序
select * from 表名 order by age asc;
-- 根据年龄降序排序
select * from 表名 order by age desc;
```

### 3.6 分页查询

``` mysql
-- 查询第 1 页数据，每页 2 条数据
select * from 表名 limit 0, 2;
```



## 4. DCL 语句

> 用户管理

``` mysql
-- 查询用户
use mysql;
select * from user;
-- 创建用户
create user 用户名@主机名 identified by '密码';
-- 修改密码
alter user 用户名@主机名 identified with mysql_native_password by '新密码';
-- 删除用户
drop user 用户名@主机名;
```

> 权限管理

``` mysql
-- 查询权限
show grants for 用户名@主机名;
-- 授予权限
grant 权限列表 on 数据库名.表名 to 用户名@主机名;
-- 撤销权限
revoke 权限列表 on 数据库名.表名 from 用户名@主机名;
```



## 5. 函数

### 5.1 字符串函数

```mysql
-- 字符串拼接
select concat('Hello', ' world!'); -- Hello world!
-- 转小写
select lower('Hello'); -- hello
-- 转大写
select upper('Hello'); -- HELLO
-- 从左边填充:lpad; 从右边填充: rpad;
select lpad('01', 5, '-'); -- '00001'
-- 去除两边空格
select trim('  Hello world '); -- Hello world
-- 截取字符串
select substring('Hello world', 1, 5); -- Hello
```

### 5.2 数值函数

```mysql
-- 向上取整
select ceil(1.5); -- 2
-- 向下取整
select floor(1.5); -- 1
-- 取模
select mod(8, 3); -- 2
-- 随机数, 0 ~ 1
select rand();
-- 四舍五入
select round(2.545, 2); -- 2.55
```

### 5.3 日期函数

```mysql
-- 当前日期
select curdate();
-- 当前时间
select curtime();
-- 当前日期时间
select now();
-- 获取年、月、日
select year(now());
select month(now());
select day(now());
-- 往后推 70 天(年、月)的日期时间
select date_add(now(), interval 70 day);
-- 查询两个日期之间的差值
select datediff(now(), '2000-11-22 00:00:00');
```

### 5.4 条件函数

```mysql
-- if, 跟三元运算差不多
select if(1 = 1, 'success', 'fail'); -- success
-- ifnull, 第一个值不为 null 返回该值，反之返回第二个
select ifnull(null, 'OK'); -- OK
-- case when else end
select
    name,
    (case gender when 1 then '男' when 2 then '女' else '未知' end) '性别'
from emp;
```



## 6. 约束

### 6.1 字段约束

![image-20240306112510204](.\assets\image-20240306112510204.png)

``` mysql
create table user(
    id int primary key auto_increment comment 'key', -- 主键，自增
    name varchar(12) not null unique comment '姓名', -- 不为 null，值唯一
    age int check (age > 0 and age <= 120) comment '年龄', -- 值 > 0 && <= 120
    status char(1) default '1' comment '状态', -- 默认值 '1'
    gender tinyint comment '性别' -- 无限制
) comment '用户表';
```

### 6.2 外键约束

``` mysql
-- 添加外键
alter table 表名 add constraint 外键名 foreign key (字段名) references 需要关联的表名(主键字段);
-- 删除外键
alter table 表名 drop foreign key 外键名;
```

![image-20240306140324264](.\assets\image-20240306140324264.png)



## 7. 多表查询

 ### 7.1 内连接

``` mysql
-- 隐式内连接
select 字段列表 from 表1, 表2 where 条件 ...;
-- 显式内连接
select 字段列表 from 表1, 表2 on 条件...;
```

### 7.2 外连接

``` mysql
-- 左外连接，完全包含左表
select 字段列表 from 表1 left join 表2 on 条件...;
-- 右外连接，完全包含右表
select 字段列表 from 表1 right join 表2 on 条件...;
```

### 7.3 自连接

``` mysql
-- 自连接可以是内连接也可以是外连接
select 字段列表 from 表A 别名1 join 表A 别名2 on 条件...;
```

### 7.4 联合查询

``` mysql
-- 将两个查询结果合并
select * from emp2 where salary > 15000
union all -- union all 会将所有数据合并，union 则不会
select * from emp2 where age < 50;
```

### 7.5 子查询

``` mysql
-- 标量子查询：根据部门名称，查询员工信息
select * from emp2 where dept_id = (select id from dept where name = '总裁办');
-- 列子查询
select * from emp2
where dept_id in (select id from dept where name = '总裁办' or name = '市场部');
-- 行子查询
select *from emp2
where (salary, manager_id) =(select salary, id from emp2 where name = '张无忌');
-- 表子查询
select e.*, d.* from (select * from emp2 where entry_date > '2000-01-01') e left join dept d on e.dept_id = d.id;
```



## 8. 事务

### 8.1 事务基础

- 事务处理要么全部成功，要么全部失败

``` mysql
-- 查看事务提交方式
select @@autocommit; -- 1: 自动提交; 0: 手动提交
-- 设置事务提交方式
set @@autocommit = 0;
-- 开启事务
start transaction;
-- 提交事务
commit;
-- 回滚事务
rollback;
```

### 8.2 并发事务问题

>- 脏读：一个事务读到另外一个事务还没有提交的数据。
>- 不可重复读： 一个事务先后读取同一条记录，但是两次读取的数据不同。
>- 一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这条数据已经存在，好像出现了**“幻影”**。



