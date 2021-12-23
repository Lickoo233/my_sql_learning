# MYSQL命令总结

- 创建表

> CREATE TABLE <表名> ([表定义选项]) [表选项] [分区选项];
>
> 其中，[表定义选项]的格式为：
> <列名1> <类型1> [,…] <列名n> <类型n>

- 修改表

> ALTER TABLE <表名> [修改选项]
>
> 修改选项的语法格式如下：
> { ADD COLUMN <列名> <类型>
> | CHANGE COLUMN <旧列名> <新列名> <新列类型>
> | ALTER COLUMN <列名> { SET DEFAULT <默认值> | DROP DEFAULT }
> | MODIFY COLUMN <列名> <类型>
> | DROP COLUMN <列名>
> | RENAME TO <新表名>
> | CHARACTER SET <字符集名>
> | COLLATE <校对规则名> }

- 修改字段名称

> ALTER TABLE <表名> CHANGE <旧字段名> <新字段名> <新数据类型>；
>
> （经测试好像效果和上一点的alter table <表名> change column一样？）
>
> 其中：
> 旧字段名：指修改前的字段名；
> 新字段名：指修改后的字段名；
> 新数据类型：指修改后的数据类型，如果不需要修改字段的数据类型，可以将新数据类型设置成与原来一样，但数据类型不能为空。

- 修改字段数据类型

> ALTER TABLE <表名> MODIFY <字段名> <数据类型>
>
> 其中：
> 表名：指要修改数据类型的字段所在表的名称；
> 字段名：指需要修改的字段；
> 数据类型：指修改后字段的新数据类型。

- 删除字段

> ALTER TABLE <表名> DROP <字段名>；
>
> 其中，“字段名”指需要从表中删除的字段的名称。

- 删除数据表

> DROP TABLE [IF EXISTS] 表名1 [ ,表名2, 表名3 ...]
>
> 对语法格式的说明如下：
> 表名1, 表名2, 表名3 ...表示要被删除的数据表的名称。DROP TABLE 可以同时删除多个表，只要将表名依次写在后面，相互之间用逗号隔开即可。
> IF EXISTS 用于在删除数据表之前判断该表是否存在。如果不加 IF EXISTS，当数据表不存在时 MySQL 将提示错误，中断 SQL 语句的执行；加上 IF EXISTS 后，当数据表不存在时 SQL 语句可以顺利执行，但是会发出警告（warning）。

- 删除关联表

  > http://c.biancheng.net/view/7200.html

- 展示表结构

> DESCRIBE <表名>;
>
> 或简写成：
> DESC <表名>;

- 以SQL语句的形式展现表结构

> SHOW CREATE TABLE <表名>;
>
> 在 SHOW CREATE TABLE 语句的结尾处（分号前面）添加\g或者\G参数可以改变展示形式。

- 末尾添加字段

> ALTER TABLE <表名> ADD <新字段名><数据类型>[约束条件];
>
> 对语法格式的说明如下：                                       
> <表名> 为数据表的名字；
> <新字段名> 为所要添加的字段的名字；
> <数据类型> 为所要添加的字段能存储数据的数据类型；
> [约束条件] 是可选的，用来对添加的字段进行约束。

- 在开头添加字段

> ALTER TABLE <表名> ADD <新字段名> <数据类型> [约束条件] FIRST;
>
> FIRST 关键字一般放在语句的末尾。

- 在中间位置添加字段

> ALTER TABLE <表名> ADD <新字段名> <数据类型> [约束条件] AFTER <已经存在的字段名>;

- 创建表时添加主键/联合主键

> CREATE TABLE （<字段名> <数据类型> PRIMARY KEY [默认值], ...）;
>
> 例：mysql> CREATE TABLE tb_emp3
>     -> (
>     -> id INT(11) PRIMARY KEY,
>     -> name VARCHAR(25),
>     -> deptId INT(11),
>     -> salary FLOAT
>     -> );
>
> CREATE TABLE （<字段名> <数据类型> , ... [CONSTRAINT <约束名>] PRIMARY KEY [字段名1,字段名2,...]）;（如果不是联合主键的话就只有一个字段名）
>
> 例：mysql> CREATE TABLE tb_emp4
>     -> (
>     -> id INT(11),
>     -> name VARCHAR(25),
>     -> deptId INT(11),
>     -> salary FLOAT,
>     -> PRIMARY KEY(id)
>     -> );

- 在修改表时添加主键约束

> ALTER TABLE <数据表名> ADD PRIMARY KEY(<字段名>);

- 删除主键

> ALTER TABLE <数据表名> DROP PRIMARY KEY;

- 创建表时设置主键自增长

> CREATE TABLE （<字段名> <数据类型> AUTO_INCREASEMENT , ...);
>
> 例：mysql> CREATE TABLE tb_student(
>     -> id INT(4) PRIMARY KEY AUTO_INCREMENT,
>     -> name VARCHAR(25) NOT NULL
>     -> );

- 给已存在的主键设置自增

> ALTER TABLE <表名> MODIFY <字段名> <字段类型> AUTO_INCREMENT;

- 创建表时给主键的自增设置初始值

> 例：
>
> mysql> CREATE TABLE tb_student2 (
>     -> id INT NOT NULL AUTO_INCREMENT,
>     -> name VARCHAR(20) NOT NULL,
>     -> PRIMARY KEY(ID)
>     -> )AUTO_INCREMENT=100;

- 给已有的自增值设置初始值

> ALTER TABLE <表名> AUTO_INCREMENT=值;

**Q：自增字段值不连续：http://c.biancheng.net/view/7624.html**

- 在创建表时设置外键约束

> CREATE TABLE （<字段名> <数据类型>, ... [CONSTRAINT <外键名>] FOREIGN KEY 字段名 [，字段名2，…]
> REFERENCES <主表名> 主键列1 [，主键列2，…]);
>
> 例：mysql> CREATE TABLE tb_dept1
>     -> (
>     -> id INT(11) PRIMARY KEY,
>     -> name VARCHAR(22) NOT NULL,
>     -> location VARCHAR(50)
>     -> );
> Query OK, 0 rows affected (0.37 sec)
>
> mysql> CREATE TABLE tb_emp6
>     -> (
>     -> id INT(11) PRIMARY KEY,
>     -> name VARCHAR(25),
>     -> deptId INT(11),
>     -> salary FLOAT,
>     -> CONSTRAINT fk_emp_dept1
>     -> FOREIGN KEY(deptId) REFERENCES tb_dept1(id)
>     -> );
> Query OK, 0 rows affected (0.37 sec)

- 在修改表时设置外键约束

> ALTER TABLE <数据表名> ADD CONSTRAINT <外键名>
> FOREIGN KEY(<列名>) REFERENCES <主表名> (<列名>);

- 删除外键约束

> ALTER TABLE <表名> DROP FOREIGN KEY <外键约束名>;

- 在创建表时设置唯一约束

> CREATE TABLE （<字段名> <数据类型> UNIQUE , ...);
>
> 例：mysql> CREATE TABLE tb_dept2
>     -> (
>     -> id INT(11) PRIMARY KEY,
>     -> name VARCHAR(22) UNIQUE,
>     -> location VARCHAR(50)
>     -> );

- 在修改表时设置唯一约束

> ALTER TABLE <数据表名> ADD CONSTRAINT <唯一约束名> UNIQUE(<列名>);

- 删除唯一约束

> ALTER TABLE <表名> DROP INDEX <唯一约束名>;

- 创建表时设置检查约束

> CREATE TABLE （<字段名> <数据类型> , ... CHECK(<条件>) );
>
> 例：mysql> CREATE TABLE tb_emp7
>     -> (
>     -> id INT(11) PRIMARY KEY,
>     -> name VARCHAR(25),
>     -> deptId INT(11),
>     -> salary FLOAT,
>     -> CHECK(salary>0 AND salary<100),
>     -> FOREIGN KEY(deptId) REFERENCES tb_dept1(id)
>     -> );

- 修改表时添加检查约束

> ALTER TABLE tb_emp7 ADD CONSTRAINT <检查约束名> CHECK(<检查约束>)
>
> 例：mysql> ALTER TABLE tb_emp7
>     -> ADD CONSTRAINT check_id
>     -> CHECK(id>0);

- 删除检查约束名

> ALTER TABLE <数据表名> DROP CONSTRAINT <检查约束名>;

- 在创建表时设置默认约束

> CREATE TABLE （<字段名> <数据类型> DEFAULT <默认值> , ...);
>
> 例：mysql> CREATE TABLE tb_dept3
>     -> (
>     -> id INT(11) PRIMARY KEY,
>     -> name VARCHAR(22),
>     -> location VARCHAR(50) DEFAULT 'Beijing'
>     -> );

- 修改表时添加默认约束

> ALTER TABLE <数据表名> CHANGE COLUMN <字段名> <数据类型> DEFAULT <默认值>;

- 删除默认约束

> ALTER TABLE <数据表名> CHANGE COLUMN <字段名> <字段名> <数据类型> DEFAULT NULL;

- 在创建表时设置非空约束

> CREATE TABLE （<字段名> <数据类型> NOT NULL , ...);
>
> 例：mysql> CREATE TABLE tb_dept4
>     -> (
>     -> id INT(11) PRIMARY KEY,
>     -> name VARCHAR(22) NOT NULL,
>     -> location VARCHAR(50)
>     -> );

- 在修改表时添加非空约束

> ALTER TABLE <数据表名> CHANGE COLUMN <字段名> <字段名> <数据类型> NOT NULL;

- 删除非空约束

> ALTER TABLE <数据表名> CHANGE COLUMN <字段名> <字段名> <数据类型> NULL;

- 查看表中的约束

> SHOW CREATE TABLE <数据表名>;

---

- 查询表操作

> SELECT [DISTINCT]
> {* | <字段列名>}
> [
> FROM <表 1>, <表 2>…
> [WHERE <表达式>
> [GROUP BY <group by definition>
> [HAVING <expression> [{<operator> <expression>}…]]
> [ORDER BY <order by definition>]
> [LIMIT[<offset>,] <row count>]
> ]
>
> 例：
>
> mysql> SELECT * FROM tb_students_info;
>
> SELECT id,name,dept_id,age,sex,height,login_date FROM tb_students_info;
>
> mysql> SELECT * FROM tb_students_info LIMIT 3,5;
>
> mysql> SELECT * FROM tb_students_info LIMIT 4;
>
> mysql> SELECT * FROM tb_students_info LIMIT 5 OFFSET 3;
>
> mysql> SELECT name,height FROM tb_students_info ORDER BY height,name;
>
> mysql> SELECT name,height FROM tb_student_info ORDER BY height DESC,name ASC;

- 指定别名

> <表名> [AS] <别名>
>
> <字段名> [AS] <别名>
>
> 例：
>
> mysql> SELECT name AS student_name, age AS student_age FROM tb_students_info;

- 限制查询结果数

> LIMIT 记录数
>
> LIMIT 初始位置，记录数
>
> LIMIT 记录数 OFFSET 初始位置
>
> 例：
>
> mysql>SELECT * FROM tb_students_info LIMIT 5 OFFSET 3;

- 对查询结果排序

> ORDER BY <字段名1>[ASC|DESC]，<字段名2>[ASC|DESC]，...  （ASC为默认值）
>
> 例：
>
> mysql> SELECT name,height FROM tb_student_info ORDER BY height DESC,name ASC;

- 条件查询

> WHERE 查询条件
>
> 查询条件可以是：
>
> - 带比较运算符和逻辑运算符的查询条件
> - 带 BETWEEN AND 关键字的查询条件
> - 带 IS NULL 关键字的查询条件
> - 带 IN 关键字的查询条件
> - 带 LIKE 关键字的查询条件
>
> 例：
>
> mysql> SELECT name,age,height FROM tb_students_info 
>     -> WHERE age>21 XOR height>=175;
>
> mysql> SELECT name FROM tb_students_info
>     -> WHERE name NOT LIKE BINARY 'T%';
>
> mysql> SELECT name FROM tb_students_info
>     -> WHERE name LIKE '____y';
>
> mysql> SELECT NAME FROM test.`tb_students_info` WHERE NAME LIKE '%\%';
>
> 
>
> mysql> SELECT name,age FROM tb_students_info 
>     -> WHERE age NOT BETWEEN 20 AND 23;
>
> mysql> SELECT name,login_date FROM tb_students_info
>     -> WHERE login_date BETWEEN '2015-10-01' AND '2016-05-01';
>
> 
>
> mysql> SELECT 'name','login_date' FROM tb_students_info 
>     -> WHERE login_date IS NOT NULL;

- 分组查询

> GROUP BY <字段名>
>
> 聚合函数包括 COUNT()，SUM()，AVG()，MAX() 和 MIN()。
>
> 例：
>
> mysql> SELECT `sex`, <u>GROUP_CONCAT(name)</u> FROM tb_students_info 
>     -> GROUP BY sex;
>
> mysql> SELECT sex,COUNT(sex) FROM tb_students_info 
>     -> GROUP BY sex;
>
> mysql> SELECT sex,GROUP_CONCAT(name) FROM tb_students_info 
>     ->GROUP BY sex WITH ROLLUP;
>
> WITH POLLUP 关键字用来在所有记录的最后加上一条记录，这条记录是上面所有记录的总和，即统计记录数量。

- 过滤分组

> [MySQL HAVING：过滤分组 (biancheng.net)](http://c.biancheng.net/view/7416.html)

- 交叉链接

> SELECT <字段名> FROM <表1> CROSS JOIN <表2> [WHERE子句]
>
> 或SELECT <字段名> FROM <表1>, <表2> [WHERE子句] 
>
> 例：
>
> mysql> SELECT * FROM tb_course CROSS JOIN tb_students_info 
>     -> WHERE tb_students_info.course_id = tb_course.id;

- 内连接

> SELECT <字段名> FROM <表1> INNER JOIN <表2> [ON子句]
>
> 内连接中可以省略 INNER 关键字，只用关键字 JOIN。
>
> 例：
>
> mysql> SELECT s.name,c.course_name FROM tb_students_info s INNER JOIN tb_course c 
>     -> ON s.course_id = c.id;
>
> 注意：当对多个表进行查询时，要在 SELECT 语句后面指定字段是来源于哪一张表。因此，在多表查询时，SELECT 语句后面的写法是`表名.列名`。另外，如果表名非常长的话，也可以给表设置别名，这样就可以直接在 SELECT 语句后面写上`表的别名.列名`。

- 外连接

> SELECT <字段名> FROM <表1> LEFT OUTER JOIN <表2> <ON子句>
>
> mysql> SELECT s.name,c.course_name FROM tb_students_info s LEFT OUTER JOIN tb_course c 
>     -> ON s.'course_id'=c.'id';
>
> SELECT <字段名> FROM <表1> RIGHT OUTER JOIN <表2> <ON子句>
>
> 例：
>
> mysql> SELECT s.name,c.course_name FROM tb_students_info s RIGHT OUTER JOIN tb_course c 
>     -> ON s.'course_id'=c.'id';
>
> 多个表左/右连接时，在 ON 子句后连续使用 LEFT/RIGHT OUTER JOIN 或 LEFT/RIGHT JOIN 即可。

- 子查询

> WHERE <表达式> <操作符> (子查询)
>
> 1）IN | NOT IN
>
> 2）EXISTS | NOT EXISTS
>
> 例：
>
> mysql> SELECT name FROM tb_students_info 
>     -> WHERE course_id IN (SELECT id FROM tb_course WHERE course_name = 'Java');
>
> mysql> SELECT name FROM tb_students_info
>     -> WHERE course_id = (SELECT id FROM tb_course WHERE course_name = 'Python');
>
> mysql> SELECT name FROM tb_students_info
>     -> WHERE course_id <> (SELECT id FROM tb_course WHERE course_name = 'Python');
>
> mysql> SELECT * FROM tb_students_info
>     -> WHERE EXISTS(SELECT course_name FROM tb_course WHERE id=1);
>
> mysql> SELECT * FROM tb_students_info
>     -> WHERE age>24 AND EXISTS(SELECT course_name FROM tb_course WHERE id=1);
>
> 一般来说，表连接（内连接和外连接等）都可以用子查询替换，但反过来却不一定，有的子查询不能用表连接来替换。子查询比较灵活、方便、形式多样，适合作为查询的筛选条件，而表连接更适合于查看连接表的数据。

...

---

  