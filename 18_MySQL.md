## 数据库简介

### 作用

- 持久的存储数据
- 备份和恢复数据
- 快速的存取数据
- 权限控制

### 类型

#### 关系数据库

- 特点：
  - 以表和表的关联构成的数据结构
- 优点：
  1. 能表达复杂的数据关系
  2. 强大的查询语言，能精确查找想要的数据
- 缺点：
  1. 读写性能比较差，尤其是海量数据的读写
  2. 数据结构比较死板
- 用途：
  - 存储结构复杂的数据，尤其是业务数据
- 代表：
  - Oracle
  - MySQL
  - SQL Server

#### 非关系型数据库

- 特点：
  - 以极其简单的结构存储数据
  - 类型有文档型、键值对
- 优点：
  1. 格式灵活
  2. 海量数据读写效率很高
- 缺点：
  1. 难以表示复杂的数据结构
  2. 对于复杂查询效率不好
- 用途：
  - 存储结构简单的数据
- 代表：
  - MongoDB
  - Redis
  - Membase

#### 面向对象数据库

### 术语

- DB：database 数据库
- DBA：database administrator 数据库管理员
- DBMS：database management system 数据库管理系统
- DBS：database system 数据库系统，DBS 包含 DB、DBA、DBMS



## 数据库设计

### SQL

- Structured Query Language 结构化查询语言
- 大部分关系型数据，拥有着基本一致的SQL语法

#### 分支

- DDL：
  - Data Definition Language 数据定义语言
  - 操作数据库对象：
    - 库
    - 表
    - 视图
    - 存储过程
- DML：
  - Data Manipulation Language 数据操控语言
  - 操作数据库中的记录
- DCL：
  - Data Control Language 数据控制语句
  - 操作用户权限


### 管理库

#### 创建库

#### 切换当前库

#### 删除库

### 管理表

#### 创建表

- 字段：
  - 字段名
  - 字段类型：
    - bit：占1位，0或1，false 或 true
    - int：占32位，整数
    - decimal(M, N)：能精确计算的实数，M 是总的数字位数，N 是小数位数
    - char(n)：固定长度为 n 的字符
    - varchar(n)：长度可变，最大长度为 n 的字符
    - text：大量的字符
    - date：仅日期
    - datetime：日期和时间
    - time：仅时间
  - 是不是null
  - 自增
  - 默认值

#### 修改表

#### 删除表

### 主键和外键

#### 主键

- 根据设计原则，每张表都要有主键
- 主键必须满足的要求：
  - 唯一
  - 不能更改
  - 无业务含义

#### 外键

- 用于产生表关系的列
- 外键列会连接到另一张表（或自己）的主键

### 表关系

#### 一对一

- 一个A对应一个B，一个B对应一个A
- 例如：用户和用户信息
- 把任意一张表的主键同时设置为外键

#### 一对多

- 一个A对应多个B，一个B对应一个A，A和B是一对多，B和A是多对一
- 例如：班级和学生，用户和文章
- 在多一端的表上设置外键，对应到另一张表的主键

#### 多对一

- 一个A对应多个B，一个B对应多个A
- 例如：学生和老师
- 需要新建一张关系表，关系表至少包含两个外键，分别对应到两张表

### 三大设计范式

1. 要求数据库表的每一列都是不可分割的原子数据项
2. 非主键列必须依赖于主键列
3. 非主键列必须直接依赖主键列



## 表记录的增删改

### DML

#### 增 CREATE

```sql
-- 语法
CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    ...
);

-- 范例
CREATE TABLE employees (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    position VARCHAR(50),
    salary DECIMAL(10, 2)
);
```



#### 查 Retrieve

```sql
-- 语法
SELECT column1, column2, ...
FROM table_name
WHERE condition;

-- 范例
SELECT name, position
FROM employees;
WHERE salary > 50000;
```



#### 改 UPDATE

```sql
-- 语法
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;

-- 范例
UPDATE employees
SET position = 'Manager'
WHERE id = 1;
```



#### 删 DELETE

```sql
-- 语法
DELETE FROM table_name
WHERE condition;

-- 范例
DELETE FROM employees
WHERE id = 1;
```

 

#### 修改表结构 ALTER

```sql
-- 基本语法
ALTER TABLE table_name
-- 新增列
ADD column_name datatype constraints;
-- 删除列
DROP COLUMN column_name;
-- 修改列
MODIFY COLUMN column_name new_datatype constraints;

-- 范例
ALTER TABLE employees
ADD birthdate DATE;

ALTER TABLE employees
DROP COLUMN birthdate;

ALTER TABLE employees
MODIFY COLUMN salary FLOAT;
```



#### 删除表 DROP

```sql
-- 语法
DROP TABLE table_name;

-- 范例
DROP TABLE employees;
```



#### 清空表 TRUNCATE

```sql
-- 语法
TRUNCATE TABLE table_name;

-- 范例
TRUNCATE TABLE employees;
```



## 单表基本查询

### SELECT

- 别名
- *
- case
- distinct

### FROM

### WHERE

- =
- in
- is、is not
- \> \< \>= \<=
- between
- like
- and
- or

### ORDER BY

- asc
- desc

### LIMIT

- n,m：跳过 n 条数据，取出 m 条数据

### 运行顺序

1. from
2. where
3. select
4. order by
5. limit



## 联表查询

### 笛卡尔积

```sql
-- 查询 team 表中足球队的对阵表
SELECT
	t1.NAME 主场,
	t2.NAME 客场 
FROM
	team AS t1,
	team AS t2 
WHERE
	t1.id != t2.id;
```



### 左连接（左外连接）LEFT JOIN

### 右连接（右外连接）RIGHT JOIN

### 内连接 INNER JOIN

```sql
-- 显示出所有员工的姓名、性别（使用男或女显示）、入职时间、薪水、所属部门（显示部门名称）、所属公司（显示公司名称）
SELECT
	e.`name` 员工姓名,
CASE
		ismale 
		WHEN 1 THEN
		'男' ELSE '女' 
	END 性别,
	e.joinDate 入职时间,
	e.salary 薪水,
	d.`name` 部门名称,
	c.`name` 公司名称 
FROM
	employee e
	INNER JOIN department d ON e.deptId = d.id
	INNER JOIN company c ON d.companyId = c.id

-- 查询腾讯和蚂蚁金服的所有员工姓名、性别、入职时间、部门名、公司名
...
WHERE
	c.`name` IN (
	'腾讯科技',
	'蚂蚁金服')

-- 查询腾讯教学部的所有员工姓名、性别、入职时间、部门名、公司名
...
WHERE
	c.`name` LIKE '%渡一%' 
	AND d.`name` = '教学部';

-- 列出所有公司员工居住的地址（需去重）
SELECT DISTINCT
	location 
FROM
	employee;
```



## 函数和分组

### 函数

#### 内置函数

##### 数学

- ABS(x)：绝对值
- CEILING(x)：向上取整 alias -> CEIL
- FLOOR(x)：向下取整
- MOD(x,y)：x / y 的模
- PI()：PI 值
- RAND()：０到１内的随机值
- ROUND(x,y)：x 四舍五入到保留 y 位小数的值
- TRUNCATE(x,y)：x 截短到 y 位小数

##### 聚合

- AVG(col)：返回指定列的平均值

- COUNT(col)：返回指定列中非 NULL 值的个数

- MIN(col)：返回指定列的最小值

- MAX(col)：返回指定列的最大值

- SUM(col)：返回指定列的所有值之和

  ```sql
  SELECT
  	COUNT( id ) AS 员工数量,
  	AVG( salary ) AS 平均薪资,
  	SUM( salary ) AS 总薪资,
  	MIN( salary ) AS 最小薪资 
  FROM
  	employee;
  ```

  

##### 字符

- CONCAT(s1,s2...,sn)：将 s1,s2...,sn 连接成字符串
- CONCAT_WS(sep,s1,s2...,sn)：将 s1,s2...,sn 连接成字符串，并用 sep 字符间隔
- TRIM(str)：去除字符串 str 首尾空格
- LTRIM(str)：去除字符串 str 头部空格
- RTRIM(str)：去除字符串 str 尾部空格

##### 日期

- CURTIME() / CURRENT_TIME()：当前的时间
- TIMESTAMPDIFF(part,  date1,date2)：返回 date1 到 date2 之间相隔的 part 值，part 是时间单位，取值如下
  - MICROSECOND
  - SECOND
  - MINUTE
  - HOUR
  - DAY
  - WEEK
  - MONTH
  - QUARTER
  - YEAR

#### 自定义函数

### 分组

1. from
2. join ... on ...
3. where
4. group by
5. select
6. having
7. order by
8. limit

分组后，只能查询分组的列和聚合列

```sql
-- 查询员工分布的居住地，以及每个居住地有多少名员工
SELECT
	location,
	count( id ) AS empnumber 
FROM
	employee 
GROUP BY
	location 
HAVING
	empnumber >= 40
	
-- 查询所有薪水在10000以上的员工的分布的居住地，然后仅得到聚集地大于30的结果
SELECT
	location,
	count( id ) AS empnumber 
FROM
	employee 
WHERE
	salary >= 10000 
GROUP BY
	location 
HAVING
	count( id )>= 30
```



## 练习

```sql
-- 查询腾讯每个部门的员工数量
SELECT
	d.`name`,
	COUNT( e.id ) AS number 
FROM
	company AS c
	INNER JOIN department AS d ON c.id = d.companyId
	INNER JOIN employee AS e ON d.id = e.deptId 
WHERE
	c.`name` LIKE '%腾讯%' 
GROUP BY
	d.id,
	d.`name`;
	
-- 查询每个公司的员工数量
SELECT
	c.`name`,
	COUNT( e.id ) AS number 
FROM
	company AS c
	INNER JOIN department AS d ON c.id = d.companyId
	INNER JOIN employee AS e ON d.id = e.deptId 
GROUP BY
	c.id,
	c.`name`

-- 查询所有公司5年内入职的居住在万家湾的女员工数量
SELECT
	c.`name`,
CASE
		
		WHEN r.number IS NULL THEN
		0 ELSE r.number 
	END AS number 
FROM
	company c
	LEFT JOIN (
	SELECT
		c.id,
		c.`name`,
		COUNT( e.id ) AS number 
	FROM
		company AS c
		INNER JOIN department AS d ON c.id = d.companyId
		INNER JOIN employee AS e ON d.id = e.deptId 
	WHERE
		TIMESTAMPDIFF(
			YEAR,
			e.joinDate,
		CURDATE())<= 5 
		AND e.location LIKE '%万家湾%' 
	GROUP BY
		c.id,
	c.`name` 
	) AS r ON c.id = r.id

-- 查询腾讯所有员工分布在哪些居住地，每个居住地的数量
SELECT
	e.location,
	count( e.id ) AS empnumber 
FROM
	company AS c
	INNER JOIN department AS d ON c.id = d.companyId
	INNER JOIN employee AS e ON d.id = e.deptId 
WHERE
	c.`name` LIKE '%腾讯%' 
GROUP BY
	e.location

-- 查询员工人数大于200的公司信息
SELECT
	* 
FROM
	company 
WHERE
	id IN (
	SELECT
		c.id 
	FROM
		company AS c
		INNER JOIN department AS d ON c.id = d.companyId
		INNER JOIN employee AS e ON d.id = e.deptId 
	GROUP BY
		c.id,
		c.`name` 
	HAVING
	count( e.id )>= 200 
	)

-- 查询腾讯公司里比其平均工资高的员工
SELECT
	e.* 
FROM
	company AS c
	INNER JOIN department AS d ON c.id = d.companyId
	INNER JOIN employee AS e ON d.id = e.deptId 
WHERE
	c.`name` LIKE '%腾讯%' 
	AND e.salary >(-- 查询平均薪资
	SELECT
		avg( e.salary ) 
	FROM
		company AS c
		INNER JOIN department AS d ON c.id = d.companyId
		INNER JOIN employee AS e ON d.id = e.deptId 
	WHERE
	c.`name` LIKE '%腾讯%' 
	)

-- 查询腾讯所有名字为两个字和三个字的员工对应人数
SELECT
	CHAR_LENGTH( e.`name` ) AS 姓名长度,
	COUNT( E.ID ) AS 员工数量 
FROM
	company AS c
	INNER JOIN department AS d ON c.id = d.companyId
	INNER JOIN employee AS e ON d.id = e.deptId 
WHERE
	c.`name` LIKE '%腾讯%' 
GROUP BY
	CHAR_LENGTH( e.`name` ) 
HAVING
	姓名长度 IN (
	2,
	3)

-- 查询每个公司每个月的总支出薪水，并按照从低到高排序
SELECT
	c.`name`,
	SUM( e.salary ) AS sumofsalary 
FROM
	company AS c
	INNER JOIN department AS d ON c.id = d.companyId
	INNER JOIN employee AS e ON d.id = e.deptId 
GROUP BY
	c.id,
	c.`name` 
ORDER BY
	sumofsalary
```



## 视图

MySQL 的视图（View）是一种虚拟表，其内容是根据 SQL 查询结果生成的。视图并不存储数据本身，而是存储用于生成数据的查询语句。当查询视图时，MySQL 会根据视图定义的查询语句动态生成数据。

视图的主要用途包括：

1. **简化复杂查询**：视图可以将复杂的查询封装起来，用户只需查询视图即可获得结果，而无需重复编写复杂的SQL语句。

2. **数据安全性**：视图可以隐藏表中的某些列或行，从而控制用户对敏感数据的访问。

3. **数据一致性**：视图可以确保不同用户看到的数据是一致的，即使底层数据发生了变化。

4. **抽象层次**：视图可以为底层数据提供一个抽象层，使得应用程序与数据库的结构解耦。

   ```sql
   -- 新增
   CREATE VIEW view_name AS
   SELECT column1, column2, ...
   FROM table_name
   WHERE condition;
   
   -- 删除
   DROP VIEW view_name;
   ```
