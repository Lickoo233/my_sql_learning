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
> CREATE TABLE （<字段名> <数据类型> , ... [CONSTRAINT <约束名>] PRIMARY KEY [字段名1,字段名2,...]）（如果不是联合主键的话就只有一个字段名）
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