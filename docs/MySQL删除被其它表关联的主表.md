# MySQL删除被其它表关联的主表

数据表之间经常存在外键关联的情况，这时如果直接删除父表，会破坏数据表的完整性，也会删除失败。

删除父表有以下两种方法：

- 先删除与它关联的子表，再删除父表；但是这样会同时删除两个表中的数据。
- 将关联表的外键约束取消，再删除父表；适用于需要保留子表的数据，只删除父表的情况。


下面介绍了如何取消关联表的外键约束并删除主表，也就是上面所说的删除父表的第二种方法。
 
在数据库中创建两个关联表。创建表 tb_emp4 的 SQL 语句如下：

```
CREATE TABLE tb_emp4
(
id INT(11) PRIMARY KEY,
name VARCHAR(22),
location VARCHAR (50)
);
```


接下来创建表 tb_emp5，SQL 语句如下：

```
CREATE TABLE tb_emp5
(
id INT(11) PRIMARY KEY,
name VARCHAR(25),
deptId INT(11),
salary FLOAT,
CONSTRAINT fk_emp4_emp5 FOREIGN KEY (deptId) REFERENCES tb_emp4(id)
);
```


使用 SHOW CREATE TABLE 命令查看表 tb_ emp5 的外键约束，SQL 语句和运行结果如下：

```
mysql> SHOW CREATE TABLE tb_emp5\G;
*************************** 1. row ***************************
       Table: tb_emp5
Create Table: CREATE TABLE `tb_emp5` (
  `id` int(11) NOT NULL,
  `name` varchar(25) DEFAULT NULL,
  `deptId` int(11) DEFAULT NULL,
  `salary` float DEFAULT NULL,
  PRIMARY KEY (`id`),
  KEY `fk_emp4_emp5` (`deptId`),
  CONSTRAINT `fk_emp4_emp5` FOREIGN KEY (`deptId`) REFERENCES `tb_emp4` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1
1 row in set (0.00 sec)
```

由运行结果可以看出，tb_emp5 表为子表，具有名称为 fk_emp4_emp5 的外键约束；tb_emp4 为父表，其主键 id 被子表 tb_ emp5 所关联。

删除被数据表 tb_emp5 关联的数据表 tb_emp4，SQL 语句如下：

```
mysql> DROP TABLE tb_emp4;
ERROR 1217 (23000): Cannot delete or update a parent row: a foreign key constraint fails
```

由运行结果可以看出，当主表在存在外键约束时，不能被直接删除。
 
下面解除子表 tb_emp5 的外键约束，SQL语句和运行结果如下：

```
mysql> ALTER TABLE tb_emp5 DROP FOREIGN KEY fk_emp4_emp5;
Query OK, 0 rows affected (0.03 sec)
Records: 0  Duplicates: 0  Warnings: 0
```

语句成功执行后，会取消表 tb_emp4 和表 tb_emp5 之间的关联关系。

解除关联关系后，可以使用 DROP TABLE 语句直接删除父表 tb_emp4，SQL 语句如下：

DROP TABLE tb_emp4;


最后通过 SHOW TABLES 命令查看数据表列表，如下所示：

```
mysql> show tables;
+----------------+
| Tables_in_test |
+----------------+
| tb_emp5        |
| temp           |
+----------------+
2 rows in set (0.00 sec)
```

可以发现，数据库列表中已经不存在名称为 tb_emp4 的表，删除成功。



(转自[MySQL删除被其它表关联的主表 (biancheng.net)](http://c.biancheng.net/view/7200.html))