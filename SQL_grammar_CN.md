
```
CREATE TABLE tbl_name ( column_definition ..., PRIMARY KEY(..) ) [table_options]
```
<a href='http://code.google.com/p/flashdb/wiki/SQL_CreateTable_CN'>创建表</a>： FlashDB的表必须指定一个或多个字段作为主键。
<br />

```
SELECT ... FROM ... [WHERE ...] [GROUP BY ...] [ORDER BY ...]
```
<a href='http://code.google.com/p/flashdb/wiki/SQL_Select_CN'>查询检索</a>： FlashDB的SELECT语法与ANSI SQL基本类似，但有一些特殊之处。
<br />

```
INSERT INTO tbl_name (col_name,...) VALUES (value,...)
REPLACE INTO tbl_name (col_name,...) VALUES (value,...)
INSERT IGNORE INTO tbl_name (col_name,...) VALUES (value,...)
```
插入单条数据。FlashDB插入数据时必须指定列名。三种写法在插入数据的主键重复时的处理不同:
  * INSERT INTO - 主键重复时抛异常
  * REPLACE INTO - 后写的数据覆盖之前的数据
  * INSERT IGNORE INTO - 后写的数据被忽略，即保留之前的数据不变
<br />

```
UPDATE tbl_name SET col_name1=value1 [, col_name2=value2 ...] WHERE key_col_name1=value1 [ AND key_col_name2=value2 ...]
```
修改单条数据。FlashDB不能修改主键字段的值, WHERE条件仅支持主键字段等于某值(即一次只能修改一行数据)
<br />

```
DELETE FROM tbl_name WHERE key_col_name1=value1 [ AND key_col_name2=value2 ...]
```
删除单条数据。FlashDB删除数据的WHERE条件仅支持主键字段等于某值(即一次只能删除一行数据)
<br />

<a href='Hidden comment: 
```
INSERT INTO tbl_name (col_name,...) BY ...
REPLACE INTO tbl_name (col_name,...) BY ...
INSERT IGNORE INTO tbl_name (col_name,...) BY ...
UPDATE tbl_name SETFIELDS (col_name,...) BY ...
DELETE FROM tbl_name BY ...
```
<a href="http://code.google.com/p/flashdb/wiki/SQL_transfer_CN">批量数据迁移

Unknown end tag for &lt;/a&gt;

. FlashDB的扩展功能，用于支持批量数据导入和整理计算。
<br/>
'></a>

```
FLUSH TABLE tbl_name
```
提交表的写缓存： FlashDB的特性，未FLUSH前，所有写操作对SELECT不可见。
<br />
```
DROP TABLE [IF EXISTS] tbl_name
```
删除表.
<br />

```
TRUNCATE TABLE tbl_name
```
清空表的数据.
<br />

```
OPTIMIZE TABLE tbl_name
```
优化表： 表多次写入后，会产生数据碎片，优化有助于提高检索性能。
<br />

```
LOAD TABLE DATA tbl_name FROM data_source_table
```
同步表的数据。数据来源表有三种方式：
  * LOCAL other\_tbl\_name - 从本机上的另一张表 other\_tbl\_name 同步
  * SHARDING MASTER - 从配置项 sharding\_master 指定服务器上的同名表同步
  * REPLICATION MASTER - 从配置项 replication\_master 指定服务器上的同名表同步
<br />

```
RUNON SHARDING|REPLICATION [MASTER] SLAVES [NOPARALLEL] sql_line
```
在多台服务器上执行同一句sql\_line。选项意义：
  * SHARDING|REPLICATION - 对应配置项中的 sharding\_slaves 或 replication\_slaves 中指定的多台服务器
  * MASTER - 表示在主服务器(本机)上也执行 sql\_line
  * NOPARALLEL - 表示每台服务器依次运行。未指定时默认为同时并行。
<br />

```
SYSCALL bean_id.method_name(...)
```
系统调用： 调用服务器spring配置中bean\_id相应对象的method\_name方法。