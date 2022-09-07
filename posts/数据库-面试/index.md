# 数据库 面试


<!--more-->

## 数据库

### 数据库范式

- **1NF(第一范式)**：属性不可再分。
- **2NF(第二范式)**：1NF 的基础之上，消除了非主属性对于码的部分函数依赖。即新增了主键列。
- **3NF(第三范式)**：3NF 在 2NF 的基础之上，消除了非主属性对于码的传递函数依赖。即删除依赖于其他属性的列。

## SQL

### SQL 语句分类

- DDL: Data Definition Language。创建和修改数据库中数据库对象的结构。一般用户通常不使用这些命令，他们应该通过应用程序访问数据库。
    - CREATE: This command is used to create the database or its objects (like table, index, function, views, store procedure, and triggers).
    - DROP: This command is used to delete objects from the database.
    - ALTER: This is used to alter the structure of the database.
    - TRUNCATE: This is used to remove all records from a table, including all spaces allocated for the records are removed.
    - COMMENT: This is used to add comments to the data dictionary.
    - RENAME: This is used to rename an object existing in the database.
- DQL: Data Query Language
    - SELECT: It is used to retrieve data from the database.
- DML: Data Manipulation Language
    - INSERT: It is used to insert data into a table.
    - UPDATE: It is used to update existing data within a table.
    - DELETE: It is used to delete records from a database table.
    - LOCK: Table control concurrency.
    - CALL: Call a PL/SQL or JAVA subprogram.
    - EXPLAIN PLAN: It describes the access path to data.
- DCL: Data Control Language
    - GRANT: This command gives users access privileges to the database.
    - REVOKE: This command withdraws the user’s access privileges given by using the GRANT command.
- TCL: Transaction Control Language.
    - COMMIT: Commits a Transaction.
    - ROLLBACK: Rollbacks a transaction in case of any error occurs.
    - SAVEPOINT: Sets a savepoint within a transaction.
    - SET TRANSACTION: Specify characteristics for the transaction.

### drop、delete 与 truncate 区别

- drop(丢弃数据): drop table 表名 ，直接将表都删除掉，在删除表的时候使用。
- truncate (清空数据) : truncate table 表名 ，只删除表中的数据，再插入数据的时候自增长 id 又从 1 开始，在清空表中数据的时候使用。
- delete（删除数据） : delete from 表名 where 列名=值，删除某一行的数据，如果不加 where 子句和truncate table 表名作用类似。

> truncate 和 drop 属于 DDL(数据定义语言)语句，**操作立即生效**，原数据不放到 rollback segment 中，不能回滚，操作不触发 trigger。而 delete 语句是 DML (数据库操作语言)语句，这个操作会放到 rollback segement 中，**事务提交之后才生效**。


