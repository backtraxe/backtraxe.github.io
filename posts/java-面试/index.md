# Java 面试


<!--more-->

## 路线图

- [MCA JAVA后端架构师](https://www.processon.com/view/link/61b2313b0e3e74683770741d#map)

八股文：

- [CS-Notes](http://www.cyc2018.xyz/)
- [JavaGuide](https://javaguide.cn/home.html)
- [Java 全栈知识体系](https://pdai.tech/)
- [进击的java菜鸟](https://fhfirehuo.github.io/Attacking-Java-Rookie/)

算法：

- [labuladong 的算法小抄](https://labuladong.github.io/algo/)
- [代码随想录](https://programmercarl.com/)

数据库：

- [DB-TUTORIAL](https://dunwu.github.io/db-tutorial/)

## Java 基础

### Math.abs(Integer.MIN_VALUE)

```java
Math.abs(Integer.MIN_VALUE) == Integer.MIN_VALUE;
```

### 使用未初始化的变量

使用未初始化的变量抛出编译异常。

### 1 / 0 和 1 / 0.0 的区别？

- `1 / 0` 抛出 `java.lang.ArithmeticException: / by zero` 异常。
- `1 / 0.0` 返回 `Infinity`。

### 为什么数组起始索引是 0 ？

计算数组元素地址为数组起始地址加索引，将其实索引设为 1 会浪费第一个元素空间或者计算地址时需要减 1，不便于使用。

### Java 方法参数传递为按值传递

Java 将参数值的一个副本从调用端传递到方法。

### 数组也是对象




## 数据库

### 数据库范式

- **1NF(第一范式)**：属性不可再分。
- **2NF(第二范式)**：1NF 的基础之上，消除了非主属性对于码的部分函数依赖。即新增了主键列。
- **3NF(第三范式)**：3NF 在 2NF 的基础之上，消除了非主属性对于码的传递函数依赖。即删除依赖于其他属性的列。

### drop、delete 与 truncate 区别

- drop(丢弃数据): drop table 表名 ，直接将表都删除掉，在删除表的时候使用。
- truncate (清空数据) : truncate table 表名 ，只删除表中的数据，再插入数据的时候自增长 id 又从 1 开始，在清空表中数据的时候使用。
- delete（删除数据） : delete from 表名 where 列名=值，删除某一行的数据，如果不加 where 子句和truncate table 表名作用类似。

> truncate 和 drop 属于 DDL(数据定义语言)语句，**操作立即生效**，原数据不放到 rollback segment 中，不能回滚，操作不触发 trigger。而 delete 语句是 DML (数据库操作语言)语句，这个操作会放到 rollback segement 中，**事务提交之后才生效**。

### 大数据量高并发的数据库优化


## MySQL

{{< admonition tip "折叠" false >}}
### 一、索引

**为什么使用索引**

- 提升查询速度，减少IO。

**索引是什么**

- 记录数据所在磁盘位置的目录。

**索引的存储位置**

- `InnoDB`存储引擎：数据和索引都存放于`*.ibd`
- `MyISAM`存储引擎：数据`*.MYD`，索引`*.MYI`

**索引分类及创建**

- 主键索引：主键自带索引。
- 普通索引：为普通列创建索引。`create index <idx_name> on <table>(<column>);`
- 唯一索引：为唯一列创建索引。`create unique index <idx_unique_name> on <table>(<column>);`
- 联合索引：为多个列创建索引。`create index <idx_name> on <table>(<column1>,<column2>)`
- 全文索引：在不同列或不同表中查询，MyISAM存储引擎支持，但实际生产中使用ElasticSearch、Solr代替。

> 联合索引建议不超过5个列。

**索引的数据结构**
{{< /admonition >}}

### MyISAM 和 InnoDB 的区别

**是否支持行级锁**

- MyISAM 只有表级锁(table-level locking)。
- InnoDB 支持行级锁(row-level locking)和表级锁，默认为行级锁。

**是否支持事务**

- MyISAM 不提供事务支持。
- InnoDB 提供事务支持，具有提交(commit)和回滚(rollback)事务的能力。

**是否支持外键**

- MyISAM 不支持。
- InnoDB 支持。

**是否支持数据库异常崩溃后的安全恢复**

- MyISAM 不支持。
- InnoDB 支持。

**是否支持 MVCC**

- MyISAM 不支持。
- InnoDB 支持。

> MVCC 可以看作是行级锁的一个升级，可以有效减少加锁操作，提高性能。

**索引和数据是否分开存储**

- MyISAM 分开存储，`.MYI`存索引，`.MYD`存数据。
- InnoDB 一起存储，`.ibd`文件。

### 表级锁和行级锁

- 表级锁： MySQL 中锁定**粒度最大**的一种锁，对当前操作的整张表加锁，实现简单，资源消耗也比较少，加锁快，不会出现死锁。其锁定粒度最大，触发锁冲突的概率最高，**并发度最低**，MyISAM 和 InnoDB 引擎都支持表级锁。
- 行级锁： MySQL 中锁定**粒度最小**的一种锁，只针对当前操作的行进行加锁。行级锁能大大减少数据库操作的冲突。其加锁粒度最小，**并发度高，但加锁的开销也最大，加锁慢，会出现死锁**。

### InnoDB 的锁算法

- **Record lock**：**行锁**，单个行记录上的锁。
- **Gap lock**：**间隙锁**，锁定一个范围，不包括记录本身。
- **Next-key lock**：**行锁+间隙锁**，锁定一个范围，包含记录本身。

### 事务

事务是逻辑上的一组操作，要么都执行，要么都不执行。

> InnoDB 使用 redo log(重做日志) 保证事务的持久性，使用 undo log(回滚日志) 来保证事务的原子性。
> InnoDB 通过 锁机制、MVCC 等手段来保证事务的隔离性（ 默认支持的隔离级别是 REPEATABLE-READ ）。
> 保证了事务的持久性、原子性、隔离性之后，一致性才能得到保障。

### 事务的 ACID 特性

- **原子性（Atomicity）**：事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么完全不起作用；
- **一致性（Consistency）**：执行事务前后，数据保持一致，例如转账业务中，无论事务是否成功，转账者和收款人的总额应该是不变的；
- **隔离性（Isolation）**：并发访问数据库时，一个用户的事务不被其他事务所干扰，各并发事务之间数据库是独立的；
- **持久性（Durability）**：一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有任何影响。

### 并发事务带来的问题

- **脏读（Dirty read）**: 当一个事务正在访问数据并且对数据进行了修改，而这种修改**还没有提交**到数据库中，这时另外一个事务也访问了这个数据，然后使用了这个数据。因为这个数据是还没有提交的数据，那么另外一个事务读到的这个数据是“脏数据”，依据“脏数据”所做的操作可能是不正确的。
- **丢失修改（Lost to modify）**: 指在一个事务读取一个数据时，另外一个事务也访问了该数据，那么在第一个事务中修改了这个数据后，**第二个事务也修改了这个数据**。这样第一个事务内的修改结果就被丢失，因此称为丢失修改。 例如：事务 1 读取某表中的数据 A=20，事务 2 也读取 A=20，事务 1 修改 A=A-1，事务 2 也修改 A=A-1，最终结果 A=19，事务 1 的修改被丢失。
- **不可重复读（Unrepeatable read）**: 指在一个事务内多次读同一数据。在这个事务还没有结束时，另一个事务也访问该数据。那么，在第一个事务中的两次读数据之间，由于**第二个事务的修改**导致第一个事务两次读取的数据可能不太一样。这就发生了在一个事务内两次读到的数据是不一样的情况，因此称为不可重复读。
- **幻读（Phantom read）**: 幻读与不可重复读类似。它发生在一个事务（T1）读取了几行数据，接着另一个并发事务（T2）**插入了一些数据**时。在随后的查询中，第一个事务（T1）就会发现多了一些原本不存在的记录，就好像发生了幻觉一样，所以称为幻读。

### 事务隔离级别

- **READ-UNCOMMITTED（读未提交）**： 最低的隔离级别，允许读取尚未提交的数据变更，可能会导致**脏读、幻读或不可重复读**。
- **READ-COMMITTED（读已提交）**： 允许读取并发事务已经提交的数据，可以阻止脏读，但是**幻读或不可重复读**仍有可能发生。
- **REPEATABLE-READ（可重复读）**： 对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但**幻读**仍有可能发生。
- **SERIALIZABLE（串行化）**： 最高的隔离级别，完全服从 ACID 的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，该级别可以防止脏读、不可重复读以及幻读。

> - MySQL InnoDB 存储引擎的默认支持的隔离级别是 REPEATABLE-READ（可重复读）。
> - 避免幻读
>   - 将事务隔离级别调整为 SERIALIZABLE
>   - 在可重复读的事务级别下，给事务操作的这张表添加表锁
>   - 在可重复读的事务级别下，给事务操作的这张表添加 Next-Key Locks
> - 因为隔离级别越低，事务请求的锁越少，所以大部分数据库系统的隔离级别都是 READ-COMMITTED(读取提交内容) ，但是你要知道的是 InnoDB 存储引擎默认使用 REPEATABLE-READ（可重读） 并不会有任何性能损失。
> - InnoDB 存储引擎在**分布式事务**的情况下一般会用到 SERIALIZABLE(串行化) 隔离级别。

### 索引

索引是一种用于快速查询和检索数据的数据结构。常见的索引结构有: B 树， B+树和 Hash。

**优点**

- 使用索引可以大大加快数据的检索速度（大大减少检索的数据量）, 这也是创建索引的最主要的原因。
- 通过创建唯一性索引，可以保证数据库表中每一行数据的唯一性。

**缺点**

- 创建索引和维护索引需要耗费许多时间。当对表中的数据进行增删改的时候，如果数据有索引，那么索引也需要动态的修改，会降低 SQL 执行效率。
- 索引需要使用物理文件存储，也会耗费一定空间。

### 索引数据结构

#### Hash表

为什么 MySQL 没有使用其作为索引的数据结构呢？

1. Hash 冲突问题
2. Hash 索引不支持顺序和范围查询

#### B 树& B+树

B 树也称 B-树,全称为 多路平衡查找树 ，B+ 树是 B 树的一种变体。B 树和 B+树中的 B 是 Balanced （平衡）的意思。

在 MySQL 中，MyISAM 引擎和 InnoDB 引擎都是使用 B+Tree 作为索引结构，但是，两者的实现方式不太一样。

- MyISAM 引擎中，B+Tree 叶节点的 data 域存放的是数据记录的地址。在索引检索的时候，首先按照 B+Tree 搜索算法搜索索引，如果指定的 Key 存在，则取出其 data 域的值，然后以 data 域的值为地址读取相应的数据记录。这被称为**非聚簇索引**。
- InnoDB 引擎中，其数据文件本身就是索引文件。相比 MyISAM，索引文件和数据文件是分离的，其表数据文件本身就是按 B+Tree 组织的一个索引结构，树的叶节点 **data 域保存了完整的数据记录**。这个索引的 key 是数据表的主键，因此 InnoDB 表数据文件本身就是主索引。这被称为**聚簇索引（或聚集索引）**，而其余的索引都作为辅助索引，**辅助索引的 data 域存储相应记录主键的值**而不是地址，这也是和 MyISAM 不同的地方。在根据主索引搜索时，直接找到 key 所在的节点即可取出数据；在根据辅助索引查找时，则需要先取出主键的值，再走一遍主索引。 因此，在设计表的时候，不建议使用过长的字段作为主键，也不建议使用非单调的字段作为主键，这样会造成主索引频繁分裂。

### 索引类型

#### 主键索引(Primary Key)

数据表的主键列使用的就是主键索引。

一张数据表有只能有一个主键，并且主键不能为 null，不能重复。

在 MySQL 的 InnoDB 的表中，当没有显式地指定表的主键时，InnoDB 会自动先检查表中是否有**唯一索引且不允许存在null值**的字段，如果有，则选择该字段为默认的主键，否则 InnoDB 将会自动创建一个 **6Byte 的自增主键**。

#### 二级索引(辅助索引)

二级索引又称为辅助索引，是因为二级索引的叶子节点存储的数据是主键。也就是说，通过二级索引，可以定位主键的位置。

- **唯一索引(Unique Key)** ：唯一索引也是一种约束。唯一索引的属性列不能出现重复的数据，但是允许数据为 NULL，一张表允许创建多个唯一索引。 建立唯一索引的目的大部分时候都是为了该属性列的数据的唯一性，而不是为了查询效率。
- **普通索引(Index)** ：普通索引的唯一作用就是为了快速查询数据，一张表允许创建多个普通索引，并允许数据重复和 NULL。
- **前缀索引(Prefix)** ：前缀索引只适用于字符串类型的数据。前缀索引是对文本的前几个字符创建索引，相比普通索引建立的数据更小， 因为只取前几个字符。
- **全文索引(Full Text)** ：全文索引主要是为了检索大文本数据中的关键字的信息，是目前搜索引擎数据库使用的一种技术。Mysql5.6 之前只有 MYISAM 引擎支持全文索引，5.6 之后 InnoDB 也支持了全文索引。

### 聚簇索引和非聚簇索引

#### 聚簇索引

聚簇索引即索引结构和数据一起存放的索引。主键索引属于聚簇索引。

在 MySQL 中，InnoDB 引擎的表的 .ibd 文件就包含了该表的索引和数据，对于 InnoDB 引擎表来说，该表的索引(B+树)的每个非叶子节点存储索引，叶子节点存储索引和索引对应的数据。

**优点**

- 聚集索引的查询速度非常的快，因为整个 B+ 树本身就是一颗多叉平衡树，叶子节点也都是有序的，定位到索引的节点，就相当于定位到了数据。

**缺点**

- 依赖于有序的数据 ：因为 B+ 树是多路平衡树，如果索引的数据不是有序的，那么就需要在插入时排序，如果数据是整型还好，否则类似于字符串或 UUID 这种又长又难比较的数据，插入或查找的速度肯定比较慢。
- 更新代价大 ： 如果对索引列的数据被修改时，那么对应的索引也将会被修改，而且聚簇索引的叶子节点还存放着数据，修改代价肯定是较大的，所以对于主键索引来说，**主键一般都是不可被修改的**。

#### 非聚簇索引

非聚簇索引即索引结构和数据分开存放的索引。

二级索引属于非聚簇索引。

非聚簇索引的叶子节点并不一定存放数据的指针，因为二级索引的叶子节点就存放的是主键，根据主键再回表查数据。

**优点**

- 更新代价比聚簇索引要小。非聚簇索引的更新代价就没有聚簇索引那么大了，非聚簇索引的叶子节点是不存放数据的。

**缺点**

- 跟聚簇索引一样，非聚簇索引也依赖于有序的数据
- 可能会二次查询(回表) :这应该是非聚簇索引最大的缺点了。 当查到索引对应的指针或主键后，可能还需要根据指针或主键再到数据文件或表中查询。**当查询列为索引列时（覆盖索引），无需回表查询。**

### 覆盖索引

如果一个索引包含（或者说覆盖）所有需要查询的字段的值，我们就称之为“覆盖索引”。我们知道在 InnoDB 存储引擎中，如果不是主键索引，叶子节点存储的是主键+列值。最终还是要“回表”，也就是要通过主键再查找一次。这样就会比较慢覆盖索引就是把要查询出的列和索引是对应的，不做回表操作！

覆盖索引即需要查询的字段正好是索引的字段，那么直接根据该索引，就可以查到数据了， 而无需回表查询。

### 联合索引

使用表中的多个字段创建索引，就是联合索引，也叫组合索引或复合索引。

#### 最左前缀匹配原则

在使用联合索引时，MySQL 会根据联合索引中的字段顺序，从左到右依次到查询条件中去匹配，如果查询条件中存在与联合索引中最左侧字段相匹配的字段，则就会使用该字段过滤一批数据，直至联合索引中全部字段匹配完成，或者在执行过程中遇到范围查询，如 >、<、between 和 以%开头的like查询 等条件，才会停止匹配。

所以，我们在使用联合索引时，可以将区分度高的字段放在最左边，这也可以过滤更多数据。

#### 索引下推

索引下推是 MySQL 5.6 版本中提供的一项索引优化功能，可以在非聚簇索引（MyISAM）遍历过程中，对索引中包含的字段先做判断，过滤掉不符合条件的记录，减少回表次数。

### 索引注意事项

1. 选择合适的字段创建索引：

    - **不为 NULL 的字段** ：索引字段的数据应该尽量不为 NULL，因为对于数据为 NULL 的字段，数据库较难优化。如果字段频繁被查询，但又避免不了为 NULL，建议使用 0,1,true,false 这样语义较为清晰的短值或短字符作为替代。
    - **被频繁查询的字段** ：我们创建索引的字段应该是查询操作非常频繁的字段。
    - **被作为条件查询的字段** ：被作为 WHERE 条件查询的字段，应该被考虑建立索引。
    - **频繁需要排序的字段** ：索引已经排序，这样查询可以利用索引的排序，加快排序查询时间。
    - **被经常频繁用于连接的字段** ：经常用于连接的字段可能是一些外键列，对于外键列并不一定要建立外键，只是说该列涉及到表与表的关系。对于频繁被连接查询的字段，可以考虑建立索引，提高多表连接查询的效率。

2. 被频繁更新的字段应该慎重建立索引。

    - 虽然索引能带来查询上的效率，但是维护索引的成本也是不小的。 如果一个字段不被经常查询，反而被经常修改，那么就更不应该在这种字段上建立索引了。

3. 尽可能的考虑建立联合索引而不是单列索引。

    - 因为索引是需要占用磁盘空间的，可以简单理解为每个索引都对应着一颗 B+树。如果一个表的字段过多，索引过多，那么当这个表的数据达到一个体量后，索引占用的空间也是很多的，且修改索引时，耗费的时间也是较多的。如果是联合索引，多个字段在一个索引上，那么将会节约很大磁盘空间，且修改数据的操作效率也会提升。

4. 注意避免冗余索引。

    - 冗余索引指的是索引的功能相同，能够命中索引(a, b)就肯定能命中索引(a)，那么索引(a)就是冗余索引。如（name,city）和（name）这两个索引就是冗余索引，能够命中前者的查询肯定是能够命中后者的，在大多数情况下，都应该尽量扩展已有的索引而不是创建新索引。

5. 考虑在字符串类型的字段上使用前缀索引代替普通索引。

    - 前缀索引仅限于字符串类型，较普通索引会占用更小的空间，所以可以考虑使用前缀索引带替普通索引。

### MySQL 三大日志(binlog、redo log和undo log)

#### binlog

二进制日志 binlog（归档日志）是逻辑日志，记录内容是语句的原始逻辑，类似于“给 ID=2 这一行的 c 字段加 1”，属于MySQL Server 层。

不管用什么存储引擎，只要发生了表数据更新，都会产生 binlog 日志。

MySQL数据库的数据备份、主备、主主、主从都离不开binlog，需要依靠binlog来同步数据，保证数据一致性。

**记录格式**

- `statement`：记录的内容是SQL语句原文。同步时，`update_time=now()`这里会获取当前系统时间，直接执行会导致与原库的数据不一致。
- `row`：记录具体数据，不可视化。字段和数据全都记录，能保证同步数据的一致性，**通常情况下都是指定为row**，这样可以为数据库的恢复与同步带来更好的可靠性。但是需要更大的容量来记录，比较占用空间，恢复与同步时会更消耗IO资源，影响执行速度。
- `mixed`：MySQL 会判断这条 SQL 语句是否可能引起数据不一致，如果是，就用 row 格式，否则就用 statement 格式。

**写入机制**

binlog 的写入时机也非常简单，事务执行过程中，先把日志写（`write`）到 binlog cache，事务提交的时候，再把 binlog cache 写（`fsync`）到 binlog 文件中。

sync_binlog

- 0：每次提交事务都只 write，由系统自行判断什么时候执行 fsync。机器宕机，page cache里面的 binlog 会丢失。
- 1：每次提交事务都会执行 fsync。
- N：每次提交事务都 write，但累积 N 个事务后才 fsync。机器宕机，会丢失最近 N 个事务的 binlog 日志。

#### redo log

事务日志 redo log（重做日志）是 InnoDB 存储引擎独有的，它让 MySQL 拥有了崩溃恢复能力。

当 MySQL 实例挂了或宕机了，重启时，InnoDB 存储引擎会使用 redo log 恢复数据，保证数据的持久性与完整性。

**缓存 lazy 机制**

MySQL 中数据是以页为单位，你查询一条记录，会从硬盘把一页的数据加载出来，加载出来的数据叫数据页，会放入到 Buffer Pool 中。

后续的查询都是先从 Buffer Pool 中找，没有命中再去硬盘加载，减少硬盘 IO 开销，提升性能。

更新表数据的时候，也是如此，发现 Buffer Pool 里存在要更新的数据，就直接在 Buffer Pool 里更新。

然后会把“在某个数据页上做了什么修改”（lazy 标记）记录到重做日志缓存（redo log buffer）里，接着刷盘到 redo log 文件里。

**刷盘策略**

- 0 ：每次事务提交时不进行刷盘操作
- 1 ：每次事务提交时都将进行刷盘操作（默认值）
- 2 ：每次事务提交时都只把 redo log buffer 内容写入 page cache

> - InnoDB 存储引擎有一个后台线程，每隔 1 秒，就会把 redo log buffer 中的内容写到文件系统缓存（page cache），然后调用 fsync 刷盘。
> - 当 redo log buffer 占用的空间即将达到 innodb_log_buffer_size 一半的时候，后台线程会主动刷盘。

**日志文件组**

硬盘上存储的 redo log 日志文件不只一个，而是以一个日志文件组的形式出现的，每个的 redo 日志文件大小都是一样的。

它采用的是环形数组形式（队列）。

- `write pos`（队尾入队） 是当前记录的位置，一边写一边后移。
- `checkpoint`（队头出队） 是当前要擦除的位置，也是往后推移。

每次刷盘 redo log 记录到日志文件组中，write pos 位置就会后移更新。

每次 MySQL 加载日志文件组恢复数据时，会清空加载过的 redo log 记录，并把 checkpoint 后移更新。

#### 两阶段提交

解决 binlog 和 redo log 之间的逻辑一致问题，InnoDB 存储引擎使用两阶段提交方案。

将 redo log 的写入拆成了两个步骤 prepare 和 commit。

当写入 binlog 时发生异常时，MySQL 根据 redo log 日志恢复数据时，发现 redo log 还处于 prepare 阶段，并且没有对应 binlog 日志，就会回滚该事务。其他情况则提交事务，恢复数据。

#### undo log

undo log（回滚日志），保证事务的原子性。

如果执行过程中遇到异常的话，我们直接利用回滚日志中的信息将数据回滚到修改之前的样子即可。

回滚日志会先于数据持久化到磁盘上。

<br>

## Java 集合框架

<img src="/imgs/Java面试/Java集合框架类图.png">

<br>

### Arraylist 和 Vector 的区别

- ArrayList 是 List 的主要实现类，底层使⽤ Object[] 存储，适⽤于频繁的查找⼯作，线程不安全。

- Vector 是 List 的古⽼实现类，底层使⽤ Object[] 存储，线程安全的。

<br>

### RandomAccess 接⼝

```java
public interface RandomAccess {
}
```

RandomAccess 接⼝用于标识实现这个接⼝的类具有随机访问功能。`ArrayList` 实现了 RandomAccess 接⼝， ⽽ `LinkedList` 没有实现。

在 `binarySearch()` ⽅法中，它要判断传⼊的 list 是否 RandomAccess 的实例，如果是，调⽤ `indexedBinarySearch()` ⽅法，如果不是，那么调⽤ `iteratorBinarySearch()` ⽅法。

```java
public static <T> int binarySearch(List<? extends Comparable<? super T>> list, T key) {
    if (list instanceof RandomAccess || list.size() < BINARYSEARCH_THRESHOLD)
        return Collections.indexedBinarySearch(list, key);
    else
        return Collections.iteratorBinarySearch(list, key);
}
```

<br>

### Comparable 和 Comparator 的区别

- `Comparable` 接口出自 java.lang 包，它有⼀个 `compareTo(Object obj)` 方法⽤来排序。

```java
interface Comparable {
    int compareTo(Object obj);
}
```

- `Comparator` 接口出自 java.util 包（需要导包），它有⼀个 `compare(Object obj1, Object obj2)` 方法⽤来排序。

```java
package java.util;

interface Comparator {
    int compare(Object obj1, Object obj2);
}
```

- `compareTo(Object obj)` 方法和 `compare(Object obj1, Object obj2)` 方法的返回值 **小于 0** 代表升序， **大于 0** 代表降序。

- `Collections.sort()`、`TreeSet`、`TreeMap`、`PriorityQueue`使用自定义类时必须实现 `Comparable` 接口或传入 `Comparator` 接口的实现类。

<br>

### HashMap 和 HashTable 的区别

> HashTable 已被 ConcurrentHashMap 淘汰

1. 是否线程安全
    - HashMap 线程不安全
    - HashTable 线程安全
2. 效率
    - HashMap 比 HashTable 效率高，因为 HashTable 线程安全
3. 对 null 的⽀持
    - HashMap 可以存储 null 的 key 和 value，但 null 作为 key 只能有⼀个，null 作为 value 可以有多个
    - HashTable 不允许 null，否则抛出 NullPointerException
4. 初始⼤⼩和扩充机制
    - HashMap 默认初始⼤⼩为 16，当指定大小时扩充为 2 的幂次方，已使用 75% 空间时进行扩容，每次扩容变为 2 * n
    - HashTable 默认初始⼤⼩为 11，每次扩容变为 2 * n + 1
5. 底层数据结构
    - JDK1.8 以后的 HashMap 在解决哈希冲突时有了较⼤的变化，当链表⻓度⼤于阈值（默认为 8）（将链表转换成红⿊树前会判断，如果当前数组的⻓度⼩于 64，那么会选择先进⾏数组扩容，⽽不是转换为红⿊树）时，将链表转化为红⿊树，以减少搜索时间。
    - Hashtable 没有这样的机制。

<br>

### HashMap 的底层实现

- 变量

```java
package java.util;

public class HashMap<K,V> extends AbstractMap<K,V> implements Map<K,V>, Cloneable {
    static class Node<K,V> implements Map.Entry<K,V> {
        // 数组存放的元素，即链表结点
        final int hash;
        final K key;
        V value;
        Node<K,V> next;
    }

    static final class TreeNode<K,V> extends LinkedHashMap.Entry<K,V> {
        // 红黑树结点
        TreeNode<K,V> parent;
        TreeNode<K,V> left;
        TreeNode<K,V> right;
        TreeNode<K,V> prev;
        boolean red;
    }

    static final int   DEFAULT_INITIAL_CAPACITY = 1 << 4; // 默认初始大小 16
    static final int   MAXIMUM_CAPACITY = 1 << 30;        // 最大容量
    static final float DEFAULT_LOAD_FACTOR = 0.75f;       // 默认扩容阈值
    static final int   TREEIFY_THRESHOLD = 8;             // 链表变为红黑树时的链表结点数量阈值
    static final int   MIN_TREEIFY_CAPACITY = 64;         // 链表变为红黑树时的数组大小阈值
    static final int   UNTREEIFY_THRESHOLD = 6;           // 红黑树变为链表时的结点数量阈值

    transient Node<K,V>[] table;                          // 底层数组
    transient Set<Map.Entry<K,V>> entrySet;               // entry 集合
    transient int size;                                   // 集合大小
    transient int modCount;                               // 集合修改次数，实现 fail-fast
    int threshold;                                        // 数组大小
    final float loadFactor;                               // 数组扩容阈值
}
```

- 构造方法

```java
public HashMap(int initialCapacity, float loadFactor) {
    if (initialCapacity < 0)
        throw new IllegalArgumentException("Illegal initial capacity: " + initialCapacity);
    if (initialCapacity > MAXIMUM_CAPACITY)
        initialCapacity = MAXIMUM_CAPACITY;
    if (loadFactor <= 0 || Float.isNaN(loadFactor))
        throw new IllegalArgumentException("Illegal load factor: " + loadFactor);
    this.loadFactor = loadFactor;
    this.threshold = tableSizeFor(initialCapacity);
}

public HashMap(int initialCapacity) {
    this(initialCapacity, DEFAULT_LOAD_FACTOR);
}

public HashMap() {
    this.loadFactor = DEFAULT_LOAD_FACTOR;
}
```

- `hash()`

```java
// >= jdk 1.8
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}

// < jdk 1.8
static final int hash(Object key) {
    int h = key.hashCode();
    h ^= (h >>> 20) ^ (h >>> 12);
    return h ^ (h >>> 7) ^ (h) >>> 4;
}
```

- `tableSizeFor()`

```java
static final int tableSizeFor(int cap) {
    // 扩充为 2 的幂次方
    int n = -1 >>> Integer.numberOfLeadingZeros(cap - 1);
    return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
}
```

- `get()`、`containsKey()`

```java
public V get(Object key) {
    Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}

public boolean containsKey(Object key) {
    return getNode(hash(key), key) != null;
}

final Node<K,V> getNode(int hash, Object key) {
    Node<K,V> first; // 链表第一个结点 或 红黑树根节点
    int n = table.length;
    // 当 n 是 2 的幂次方时，(n - 1) & hash 等价于 hash % n
    if (table != null && n > 0 && (first = table[(n - 1) & hash]) != null) {
        K k = first.key;
        if (first.hash == hash && (k == key || (key != null && key.equals(k))))
            return first;
        Node<K,V> e = first.next;
        if (e != null) {
            // 红黑树
            if (first instanceof TreeNode)
                return ((TreeNode<K,V>)first).getTreeNode(hash, key);
            // 链表
            do {
                k = e.key;
                if (e.hash == hash && (k == key || (key != null && key.equals(k))))
                    return e;
                e = e.next;
            } while (e != null);
        }
    }
    return null;
}


```

- `put()`

```java
public V put(K key, V value) {
    return putVal(hash(key), key, value, false, true);
}

final V putVal(int hash, K key, V value, boolean onlyIfAbsent, boolean evict) {
    Node<K,V>[] tab = table;
    int n = table.length;
    if (tab == null || n == 0) {
        // 检测是否为空
        n = (tab = resize()).length;
    }
    int i = (n - 1) & hash;
    Node<K,V> p = tab[i];
    if (p == null) {
        tab[i] = newNode(hash, key, value, null);
    } else {
        Node<K,V> e;
        K k = p.key;
        if (p.hash == hash && (k == key || (key != null && key.equals(k)))) {
            e = p;
        } else if (p instanceof TreeNode) {
            e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
        } else {
            for (int binCount = 0; ; ++binCount) {
                // binCount 记录当前链表上已遍历的结点数量
                e = p.next;
                if (e == null) {
                    // 未找到结点，进行添加
                    p.next = newNode(hash, key, value, null);
                    if (binCount >= TREEIFY_THRESHOLD - 1) {
                        // 转为红黑树
                        treeifyBin(tab, hash);
                    }
                    break;
                }
                k = e.key;
                if (e.hash == hash && (k == key || (key != null && key.equals(k)))) {
                    // 找到结点
                    break;
                }
                // 继续下一个
                p = e;
            }
        }
        if (e != null) {
            // key 已存在
            V oldValue = e.value;
            if (!onlyIfAbsent || oldValue == null) {
                // 非 putIfAbsent()
                e.value = value;
            }
            afterNodeAccess(e);
            return oldValue;
        }
    }
    ++modCount;
    if (++size > threshold)
        resize();
    afterNodeInsertion(evict);
    return null;
}
```

- `resize()`：扩容

```java
final Node<K,V>[] resize() {
    Node<K,V>[] oldTab = table;
    int oldCap = (oldTab == null) ? 0 : oldTab.length;
    int oldThr = threshold;
    int newCap = 0;
    int newThr = 0;
    if (oldCap > 0) {
        if (oldCap >= MAXIMUM_CAPACITY) {
            // 已经最大，无法扩容
            threshold = Integer.MAX_VALUE;
            return oldTab;
        } else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                   oldCap >= DEFAULT_INITIAL_CAPACITY) {
            // 扩容为 2 倍
            newThr = oldThr << 1;
        }
    } else if (oldThr > 0) {
        newCap = oldThr;
    } else {
        newCap = DEFAULT_INITIAL_CAPACITY;
        newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
    }
    if (newThr == 0) {
        float ft = (float)newCap * loadFactor;
        newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                    (int)ft : Integer.MAX_VALUE);
    }
    threshold = newThr;
    // 新建数组
    Node<K,V>[] newTab = (Node<K,V>[]) new Node[newCap];
    // 先替换原数组再拷贝对象
    table = newTab;
    if (oldTab != null) {
        for (int j = 0; j < oldCap; ++j) {
            Node<K,V> e = oldTab[j];
            if (e != null) {
                oldTab[j] = null;
                if (e.next == null) {
                    // rehash，重新计算再数组中的位置
                    // 可能留在原位置（e.hash & (oldCap - 1)）
                    // 也可能插入新位置（(e.hash & (oldCap - 1)) + oldCap）
                    newTab[e.hash & (newCap - 1)] = e;
                } else if (e instanceof TreeNode) {
                    // 红黑树分裂
                    ((TreeNode<K,V>) e).split(this, newTab, j, oldCap);
                } else {
                    Node<K,V> loHead = null, loTail = null;
                    Node<K,V> hiHead = null, hiTail = null;
                    Node<K,V> next;
                    do {
                        next = e.next;
                        if ((e.hash & oldCap) == 0) {
                            if (loTail == null)
                                loHead = e;
                            else
                                loTail.next = e;
                            loTail = e;
                        }
                        else {
                            if (hiTail == null)
                                hiHead = e;
                            else
                                hiTail.next = e;
                            hiTail = e;
                        }
                    } while ((e = next) != null);
                    if (loTail != null) {
                        loTail.next = null;
                        newTab[j] = loHead;
                    }
                    if (hiTail != null) {
                        hiTail.next = null;
                        newTab[j + oldCap] = hiHead;
                    }
                }
            }
        }
    }
    return newTab;
}
```

<br>

### 为什么 HashMap 的大小是 2 的幂次方

每个 key 的 hash 值很大，需要通过取模，即 `hash(key) % n` 得到数组索引，当 n 是 2 的幂次方时通过 `(n - 1) & hash(key)` 可以更快速的进行取模。

<br>

### 为什么 HashMap 多线程会导致死循环

JDK 1.8 之前存在这个问题，因为并发下扩容时的 rehash 会造成元素之间会形成⼀个循环链表。

> 当 HashMap 扩容时，HashMap 中的所有元素都需要被重新 hash 一遍，称为 rehash。

```java

```

<br>

### ConcurrentHashMap 和 HashTable 的区别

<br>

### ConcurrentHashMap 的底层实现

<br>

### 快速失败（Fail-fast）和 安全失败（Fail-safe）

快速失败（fail-fast）是 Java 集合的⼀种错误检测机制。在使用迭代器对集合进⾏遍历的时候，
- 多线程下，在操作非安全失败（fail-safe）的集合类时，可能触发 fail-fast 机制，导致抛出 `ConcurrentModificationException` 异常。
- 单线程下，在遍历过程中对集合对象的内容进行修改，会触发 fail-fast 机制，导致抛出 `ConcurrentModificationException` 异常。

采⽤安全失败机制的集合容器，在遍历时不是直接在集合内容上访问的，⽽是先复制原有集合内容，在拷⻉的集合上进⾏遍历。所以，在遍历过程中对原集合所作的修改并不能被迭代器检测到，故不会抛出 `ConcurrentModificationException` 异常。

<br>

### Arrays.asList() 和 List.of()

- `Arrays.asList()` 返回的对象是 `java.util.Arrays` 类中的内部类 `java.util.Arrays$ArrayList`，而不是 `java.util.ArrayList`。`java.util.Arrays$ArrayList` 没有实现 `add()`、`remove()` 和 `clear()` 方法，但实现了 `set()` 方法，因此不能添加和删除元素，只能修改和读取元素。
    - 传入的数组必须是对象数组，不能是基本类型数组。

```java
public class Arrays {
    public static <T> List<T> asList(T... a) {
        return new ArrayList<>(a);
    }

    private static class ArrayList<E> extends AbstractList<E> implements RandomAccess {
        private final E[] a;

        ArrayList(E[] array) {
            a = Objects.requireNonNull(array);
        }

        public int     size() {}
        public E       get(int index) {}
        public E       set(int index, E element) {}
        public int     indexOf(Object o) {}
        public boolean contains(Object o) {}
        public void    sort(Comparator<? super E> c) {}
    }
}

public abstract class AbstractList<E> extends AbstractCollection<E> implements List<E> {
    public void add(int index, E element) {
        throw new UnsupportedOperationException();
    }

    public E remove(int index) {
        throw new UnsupportedOperationException();
    }
}
```

- `List.of()` 返回的是 `AbstractImmutableList` 接口的实现类，该接口中所有修改操作（`add()`、`remove()`、`set()`等）全都会抛出异常。

```java
public interface List<E> extends Collection<E> {
    static <E> List<E> of(E... elements) {
        switch (elements.length) {
            case 0:
                return ImmutableCollections.emptyList();
            case 1:
                return new ImmutableCollections.List12<>(elements[0]);
            case 2:
                return new ImmutableCollections.List12<>(elements[0], elements[1]);
            default:
                return new ImmutableCollections.ListN<>(elements);
        }
    }
}

class ImmutableCollections {
    static UnsupportedOperationException uoe() { return new UnsupportedOperationException(); }

    static abstract class AbstractImmutableList<E> extends AbstractImmutableCollection<E> implements List<E>, RandomAccess {
        @Override public void    add(int index, E element) { throw uoe(); }
        @Override public boolean addAll(int index, Collection<? extends E> c) { throw uoe(); }
        @Override public E       remove(int index) { throw uoe(); }
        @Override public void    replaceAll(UnaryOperator<E> operator) { throw uoe(); }
        @Override public E       set(int index, E element) { throw uoe(); }
        @Override public void    sort(Comparator<? super E> c) { throw uoe(); }
    }

    static final class List12<E> extends AbstractImmutableList<E> implements Serializable {}

    static final class ListN<E> extends AbstractImmutableList<E> implements Serializable {}
}
```

<br>

## Spring

### Spring 特征

- **轻量**
    - 从大小与开销两方面而言 Spring 都是轻量的。完整的 Spring 框架可以在一个大小只有 1 MB 多的 jar 文件里发布，并且 Spring 所需的处理开销也是微不足道的。
    - Spring 是非侵入式的。Spring 应用中的对象不依赖于 Spring 的特定类。
- **控制反转（IoC，Inversion of Control）**
    - Spring 通过一种称作控制反转的技术促进了低耦合。
    - 当应用了 IoC，一个对象依赖的其他对象会通过被动的方式传递进来，而不是这个对象自己创建或者查找依赖对象。
-

## Mybatis



## 计算机网络

### 什么是网络协议？

在计算机网络要做到有条不紊地交换数据，就必须遵守一些事先约定好的规则，比如交换数据的格式、是否需要发送一个应答信息。这些规则被称为网络协议。

### TCP/IP 和 OSI

- TCP/IP：四层。应用层（Application）、传输层（Transport）、网际层（Internet）、网络接口层（Network Access）。
- OSI：七层。应用层（Application）、表示层（Presentation）、会话层（Session）、传输层（Transport）、网络层 （Network）、数据链路层（Data Link）、物理层（Physical）。

#### 为什么要对网络协议分层？

优点：

1. 简化问题难度和复杂度。由于各层之间独立，我们可以分割大问题为小问题。
2. 灵活性好。当其中一层的技术变化时，只要层间接口关系保持不变，其他层不受影响。
3. 易于实现和维护。
4. 促进标准化工作。分开后，每层功能可以相对简单地被描述。

缺点：

1. 功能可能出现在多个层里，产生了额外开销。
2. 开放系统互联基本参考模型（OSI/RM），简称为 OSI，其概念清楚，理论也较完整，但既复杂又不实用。

#### 应用层

应用层（Application Layer）的任务是通过应用进程间的交互来完成特定网络应用。应用层协议定义的是应用进程（进程：主机中正在运行的程序）间的通信和交互的规则。

对于不同的网络应用需要不同的应用层协议。在互联网中应用层协议很多，如域名系统 DNS，支持万维网应用的 HTTP 协议，支持电子邮件的 SMTP 协议等。

**基于 TCP 的协议：**

- HTTP（Hypertext Transfer Protocol，超文本传输协议），主要用于普通浏览。
- HTTPS（HTTP over SSL，安全超文本传输协议），HTTP 协议的安全版本。
- FTP（File Transfer Protocol，文件传输协议），文件传输。
- POP3（Post Office Protocol version 3，邮局协议），收邮件。
- SMTP（Simple Mail Transfer Protocol，简单邮件传输协议），发邮件。
- SSH（Secure Shell，用于替代安全性差的 TELNET），用于加密安全登陆用。
- TELNET（Teletype over the Network，网络电传），通过一个终端（terminal）登陆到网络。

**基于 UDP 的协议：**

- DHCP（Dynamic Host Configuration Protocol，动态主机配置协议），动态配置IP地址。

**基于 TCP 和 UDP 的协议：**

- DNS（Domain Name System，域名系统），根据域名查找 IP。

#### 传输层

传输层（Transport Layer）的主要任务就是负责向两台主机进程之间的通信提供通用的数据传输服务。应用进程利用该服务传送应用层报文。主要使用 TCP 和 UDP 两种协议。

1. TCP（Transmission Control Protocol，传输控制协议）：提供面向连接的，可靠的数据传输服务。
2. UDP（User Datagram Protocol，用户数据报协议）：提供无连接的，尽大努力的数据传输服务（不保证数据传输的可靠性）。

|        | UDP                    | TCP                 |
|:------:|:----------------------:|:-------------------:|
| 是否连接   | 无连接                    | 面向连接                |
| 是否可靠   | 不可靠传输                  | 可靠传输，使用流量控制和拥塞控制    |
| 连接对象个数 | 支持一对一、一对多、多对一和多对多交互通信  | 一对一通信               |
| 传输方式   | 面向报文                   | 面向字节流               |
| 首部开销   | 首部8B                   | 首部20B\-60B          |
| 适用场景   | 适用于实时应用（IP电话、视频会议、直播等） | 适用于要求可靠传输的应用，例如文件传输 |

#### 网际层

网际层（Internet Layer）的任务就是选择合适的网间路由和交换结点，确保计算机通信的数据及时传送。在发送数据时，网络层把传输层产生的**报文段或用户数据报封装成分组和包**进行传送。

在 TCP/IP 体系结构中，由于网际层使用 IP（Internet Protocol，网际协议）协议，因此分组也叫 **IP 数据报**，简称数据报。

互联网是由大量的异构（Heterogeneous）网络通过路由器（Router）相互连接起来的。互联网使用的网络层协议是无连接的 IP 协议和许多路由选择协议，因此互联网的网络层也叫做网际层或 IP 层。

#### 数据链路层

数据链路层（Data Link Layer）通常简称为链路层。两台主机之间的数据传输，总是在一段一段的链路上传送的，这就需要使用专门的链路层的协议。

在两个相邻节点之间传送数据时，数据链路层将网络层交下来的 IP 数据报**组装成帧**，在两个相邻节点间的链路上传送帧。每一帧包括**数据和必要的控制信息（如同步信息，地址信息，差错控制等）**。

在接收数据时，控制信息使接收端能够知道一个帧从哪个比特开始和到哪个比特结束。

{{< figure src="/imgs/Java面试/web应用的通信传输流.png" caption="web 应用的通信传输流" >}}

发送端在上层向下层传输数据时，每经过一层时会添加一个该层所属的首部信息。反之，接收端在下层向上层传输数据时，每经过一层时会把对应该层的首部信息去除，只传输数据。

#### 物理层

在物理层上所传送的数据单位是**比特**。物理层（Physical Layer）的作用是实现相邻计算机节点之间**比特流的透明传送**，尽可能屏蔽掉具体传输介质和物理设备的差异。使其上面的数据链路层不必考虑网络的具体传输介质是什么。

比特流的透明传送：表示经实际电路传送后的比特流没有发生变化，对传送的比特流来说，这个电路好像是看不见的。

### TCP 三次握手和四次挥手

TCP 是一种面向连接的、可靠的、基于字节流的传输层通信协议，在发送数据前，通信双方必须在彼此间建立一条连接。

所谓的“连接”，其实是客户端和服务端保存的一份关于对方的信息，如 ip 地址、端口号等。

TCP 可以看成是一种字节流，它会处理网际层或更低层的丢包、重复以及错误问题。

在连接的建立过程中，双方需要交换一些连接的参数。这些参数可以放在 TCP 头部。

一个 TCP 连接由一个 4 元组构成，分别是两个 ip 地址和两个端口号。

一个 TCP 连接通常分为三个阶段：连接、数据传输、退出（关闭）。

通过三次握手建立一个链接，通过四次挥手来关闭一个连接。

**当一个连接被建立或被关闭时，交换的报文段只包含 TCP 头部，而没有数据。**

#### TCP 报文的头部结构

{{< figure src="/imgs/Java面试/TCP报文的头部结构.png" caption="TCP 报文的头部结构" >}}

- **序号（Sequence Number，seq）**：32 bit，4B，用来标识从 TCP 源端向目的端发送的字节流，发起方发送数据时对此进行标记。
- **确认序号（Acknowledgement Number，ack）**：32 bit，4B，只有 ACK 标志位置为 1 时，确认序号字段才有效，`确认方ack=发送方seq+1`。
- 标志位：1 bit，6 个共 6 bit。
    - **URG（urgent）**：紧急指针（Urgent Pointer）有效。
    - **ACK（acknowledgment）**：确认序号有效。
    - **PSH（push）**：接收方应该尽快将这个报文交给应用层。
    - **RST（reset）**：重置连接。
    - **SYN（synchronize）**：发起一个新连接。
    - **FIN（finish）**：释放一个连接。

#### 建立连接的三次握手

三次握手的本质是**确认通信双方接收和发送数据**的能力。

1. 握手前。我不知道我能否发送和接收数据；对方也不知道他能否发送和接收数据。
2. 第一次握手。我发送数据，对方收到，则对方知道我能发送数据，他自己能接收数据。
3. 第二次握手。对方发送数据，他告诉我收到了我之前发送的数据，我收到，则我知道对方能发送和接收数据，我自己能发送和接收数据。
4. 第三次握手。我发送数据，我告诉对方收到了他之前发送的数据，对方收到，则对方知道我能发送和接收数据，他自己能发送和接收数据。
5. 握手后。双方都确认对方能够发送和接收数据，自己也能够发送和接收数据。连接建立。

{{< figure src="/imgs/Java面试/三次握手.png" caption="TCP 建立连接的三次握手" >}}

1. 第一次握手。客户端向服务端发起连接请求，首先客户端随机生成一个起始序列号 ISN（假设 100），那客户端向服务端发送的报文段 SYN=1，seq=100。
2. 第二次握手。服务端收到客户端发过来的报文后，发现 SYN=1，知道这是一个连接请求，于是将客户端的起始序列号 100 存起来，并且随机生成一个服务端的起始序列号（假设 300）。然后给客户端回复一段报文，回复报文段 SYN=1、ACK=1、seq=300、ack=101（客户端seq + 1）。
3. 第三次握手。客户端收到服务端的回复后发现 ACK=1、ack=101，于是知道服务端已经收到了序列号为 100 的报文；同时发现 SYN=1，知道了服务端同意了这次连接，于是就将服务端的起始序列号 300 保存下来。然后客户端再回复一段报文给服务端，报文 ACK=1、ack=301（服务端seq + 1)、seq=101（第一次握手时发送报文是占据一个序列号的，所以这次 seq 就从 101 开始）。当服务端收到报文后发现 ACK=1、ack=301，就知道客户端已经收到序列号为 300 的报文，就这样客户端和服务端就通过 TCP 建立了连接。

> 不携带数据的 ACK 报文是不占据序列号的，所以后面第一次正式发送数据时 seq 还是 101。

#### 关闭连接的四次挥手

四次挥手的本质是**确认通信双方是否想关闭连接**。

1. 挥手前。双方可以互相发送数据。
2. 第一次挥手。我发送数据，告诉对方我想要关闭连接，对方收到，则对方知道我想关闭连接，且我已经准备好关闭连接。
3. 第二次挥手。对方发送数据，告诉我收到了我之前发送的数据，我收到，但我不知道对方是否想关闭连接。
4. 第三次挥手。对方发送数据，告诉我他也想要关闭连接，我收到，则我知道了对方也想关闭连接，因此我正式关闭连接。
5. 第四次挥手。我发送数据，告诉对方收到了他之前发送的数据，对方收到，则对方知道了我知道他也想关闭连接，因此对方正式关闭连接。
6. 挥手后。连接关闭。

{{< figure src="/imgs/Java面试/四次挥手.png" caption="TCP 关闭连接的四次挥手" >}}

比如客户端初始化的序列号ISA=100，服务端初始化的序列号ISA=300。TCP连接成功后客户端总共发送了1000个字节的数据，服务端在客户端发FIN报文前总共回复了2000个字节的数据。

1. 第一次挥手：当客户端的数据都传输完成后，客户端向服务端发出连接释放报文(当然数据没发完时也可以发送连接释放报文并停止发送数据)，释放连接报文包含FIN标志位
(FIN=1)、序列号seq=1101(100+1+1000，其中的1是建立连接时占的一个序列号)。需要注意的是客户端发出FIN报文段后只是不能发数据了，但是还可以正常收数据；另外
FIN报文段即使不携带数据也要占据一个序列号。
2. 第二次挥手：服务端收到客户端发的FIN报文后给客户端回复确认报文，确认报文包含ACK标志位(ACK=1)、确认号ack=1102(客户端FIN报文序列号1101+1)、序列号
seq=2300(300+2000)。此时服务端处于关闭等待状态，而不是立马给客户端发FIN报文，这个状态还要持续一段时间，因为服务端可能还有数据没发完。
3. 第三次挥手：服务端将最后数据(比如50个字节)发送完毕后就向客户端发出连接释放报文，报文包含FIN和ACK标志位(FIN=1,ACK=1)、确认号和第二次挥手一样ack=1102、
序列号seq=2350(2300+50)。
4. 第三次挥手：服务端将最后数据(比如50个字节)发送完毕后就向客户端发出连接释放报文，报文包含FIN和ACK标志位(FIN=1,ACK=1)、确认号和第二次挥手一样ack=1102、
序列号seq=2350(2300+50)。


#### 为什么 TCP 建立连接是 3 次握手？

因为需要考虑连接时丢包的问题，如果只握手2次，第二次握手时如果服务端发给客户端的确认报文段丢失，此时服务端已经准备好了收发数(可以理解服务端已经连接成功)据，
而客户端一直没收到服务端的确认报文，所以客户端就不知道服务端是否已经准备好了(可以理解为客户端未连接成功)，这种情况下客户端不会给服务端发数据，也会忽略服务端
发过来的数据。
如果是三次握手，即便发生丢包也不会有问题，比如如果第三次握手客户端发的确认ack报文丢失，服务端在一段时间内没有收到确认ack报文的话就会重新进
行第二次握手，也就是服务端会重发SYN报文段，客户端收到重发的报文段后会再次给服务端发送确认ack报文。

#### 为什么 TCP 建立连接是 3 次握手，关闭连接却是 4 次挥手？

因为只有在客户端和服务端都没有数据要发送的时候才能断开TCP。而客户端发出FIN报文时只能保证客户端没有数据发了，服务端还有没有数据发客户端是不知道的。而服务端
收到客户端的FIN报文后只能先回复客户端一个确认报文来告诉客户端我服务端已经收到你的FIN报文了，但我服务端还有一些数据没发完，等这些数据发完了服务端才能给客户
端发FIN报文(所以不能一次性将确认报文和
FIN报文发给客户端，就是这里多出来了一次)。

#### 为什么客户端第四次挥手后要等 2 MSL 才能释放连接？

这里同样是要考虑丢包的问题，如果第四次挥手的报文丢失，服务端没收到确认 ack报文就会重发第三次挥手的报文，这样报文一去一回 长时间就是2MSL，所以需要等这么长时间来确认服务端确实已经收到了。

#### 如果已经建立连接，但是客户端出现故障？

TCP设有一个保活计时器，客户端如果出现故障，服务器不能一直等下去，白白浪费资源。服务器每收到一次客户端的请求后都会重新复位这个计时器，时间通常是设置为2小时，若两小时还没有收到客户端的任何数据，服务器就会发送一个探测报文段，以后每隔75秒钟发送一次。若一连发送10个探测报文仍然没反应，服务器就认为客户端出了故障，接着就关闭连接。

### HTTP 和 HTTPS

#### 常用 HTTP 状态码

HTTP 状态码表示客户端 HTTP 请求的返回结果、标识服务器处理是否正常、表明请求出现的错误等。

- 1XX：Informational（信息性），接受的请求正在处理。
    - 100：Continue（继续）。
    - 101：Switching Protocol（切换协议）。
- 2XX：Success（成功），请求正常处理完毕。
    - 200：OK（成功），表示从客户端发来的请求在服务器端被正确处理。
    - 201：Created（已创建）。
    - 202：Accepted（已创建）。
    - 203：Non-Authoritative Information（未授权信息）。
    - 204：No Content（无内容），表示请求成功，但响应报文不含实体的主体部分。
    - 205：Reset Content（重置内容）。
    - 206：Partial Content（部分内容），进行范围请求成功。
- 3XX：Redirection（重定向），需要进行附加操作以完成请求。（对于 301/302/303 状态码，几乎所有浏览器都会删除报文主体并自动用 GET 方法重新请求）
    - 300：Multiple Choice（多种选择）。
    - 301：Moved Permanently（永久移动），永久性重定向，表示资源已被分配了新的 URL。
    - 302：Found（临时移动），临时性重定向，表示资源临时被分配了新的 URL。
    - 303：See Other（查看其他位置），表示资源存在着另一个 URL，应使用 GET 方法获取资源。
    - 304：Not Modified（未修改），表示服务器允许访问资源，但请求未满足条件的情况（与重定向无关）。
    - 305：Use Proxy（使用代理）。
    - 306：Unused（未使用）。
    - 307：Temporary Redirect（临时重定向），临时性重定向，和 302 类似，但是期望客户端保持请求方法不变向新的地址发出请求。
    - 308：Permanent Redirect（永久重定向）。
- 4XX：Client Error（客户端错误），服务器无法处理请求。
    - 400：Bad Request（错误请求），请求报文存在语法错误。
    - 401：Unauthorized（未授权），表示发送的请求需要有通过 HTTP 认证的认证信息。
    - 402：Payment Required（需要付款）。
    - 403：Forbidden（禁止访问），表示对请求资源的访问被服务器拒绝，可在实体主体部分返回原因描述。
    - 404：Not found（未找到），表示在服务器上没有找到请求的资源。
    - 405：Method Not Allowed（不允许使用该方法）。
    - 406：Not Acceptable（无法接受）。
    - 407：Proxy Authentication Required（要求代理身份验证）。
    - 408：Request Timeout（请求超时）。
    - 409：Conflict（冲突）。
    - 410：Gone（已失效）。
    - 411：Length Required（需要内容长度头）。
    - 412：Precondition Failed（预处理失败）。
    - 413：Request Entity Too Large（请求实体过长）。
    - 414：Request-URI Too Long（请求网址过长）。
    - 415：Unsupported Media Type（媒体类型不支持）。
    - 416：Requested Range Not Satisfiable（请求范围不合要求）。
    - 417：Expectation Failed（预期结果失败）。
- 5XX：Server Error（服务器错误），服务器处理请求出错。
    - 500：Internal Sever Error（内部服务器错误），表示服务器端在执行请求时发生了错误。
    - 501：Not Implemented（未实现），表示服务器不支持当前请求所需要的某个功能。
    - 502：Bad Gateway（网关错误）。
    - 503：Service Unavailable（服务不可用），表明服务器暂时处于超负载或正在停机维护，无法处理请求。
    - 504：Gateway Timeout（网关超时）。
    - 505：HTTP Version Not Supported（HTTP 版本不受支持）。

#### GET 和 POST

说到GET和POST，就不得不提HTTP协议，因为浏览器和服务器的交互是通过HTTP协议执行的，而GET和POST也是HTTP协议中的两种方法。

HTTP全称为Hyper Text Transfer Protocol，中文翻译为超文本传输协议，目的是保证浏览器与服务器之间的通信。HTTP的工作方式是客户端与服务器之间的请求-应答协议。

HTTP协议中定义了浏览器和服务器进行交互的不同方法，基本方法有4种，分别是GET，POST，PUT，DELETE。这四种方法可以理解为，对服务器资源的查，改，增，删。

- GET：从服务器上获取数据，也就是所谓的查，仅仅是获取服务器资源，不进行修改。
- POST：向服务器提交数据，这就涉及到了数据的更新，也就是更改服务器的数据。
- PUT：英文含义是放置，也就是向服务器新添加数据，就是所谓的增。
- DELETE：从字面意思也能看出，这种方式就是删除服务器数据的过程。

GET 和 POST 的区别

1. Get是不安全的，因为在传输过程，数据被放在请求的URL中；Post的所有操作对用户来说都是不可见的。 但是这种做法也不时绝对的，大部分人的做法也是按照上面的说法来的，但是也可以在get请求加上 request body，给 post请求带上 URL 参数。
2. Get请求提交的url中的数据 多只能是2048字节，这个限制是浏览器或者服务器给添加的，http协议并没有对url长度进行限制，目的是为了保证服务器和浏览器能够正常运行，防止有人恶意发送请求。Post请求则没有大小限制。
3. Get限制Form表单的数据集的值必须为ASCII字符；而Post支持整个ISO10646字符集。
4. Get执行效率却比Post方法好。Get是form提交的默认方法。
5. GET产生一个TCP数据包；POST产生两个TCP数据包。对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。

### 浏览器访问网站的详细过程

首先是查找浏览器缓存，浏览器会保存一段时间你之前访问过的一些网址的DNS信息，不同浏览器保存的时常不等。
如果没有找到对应的记录，这个时候浏览器会尝试调用系统缓存来继续查找这个网址的对应DNS信息。
如果还是没找到对应的IP，那么接着会发送一个请求到路由器上，然后路由器在自己的路由器缓存上查找记录，路由器一般也存有DNS信息。
如果还是没有，这个请求就会被发送到ISP（注：Internet Service Provider，互联网服务提供商，就是那些拉网线到你家里的运营商，中国电信中国移动什么的），ISP也会有相
应的ISP DNS服务器，一听中国电信就知道这个DNS服务器的规模肯定不会小，所以基本上都能在这里找得到。题外话：会跑到这里进行查询是因为你没有改动过"网络中
心"的"ipv4"的DNS地址，万恶的电信联通可以改动了这个DNS服务器，换句话说他们可以让你的浏览器跳转到他们设定的页面上，这也就是人尽皆知的DNS和HTTP劫持，ISP们
还美名曰“免费推送服务”。强烈鄙视这种霸王行为。我们也可以自行修改DNS服务器来防止DNS被ISP污染。
如果还是没有的话， 你的ISP的DNS服务器会将请求发向根域名服务器进行搜索。根域名服务器就是面向全球的顶级DNS服务器，共有13台逻辑上的服务器，从A到M命名，真正
的实体服务器则有几百台，分布于全球各大洲。所以这些服务器有真正完整的DNS数据库。如果到了这里还是找不到域名的对应信息，那只能说明一个问题：这个域名本来就不
存在，它没有在网上正式注册过。或者卖域名的把它回收掉了（通常是因为欠费）。
这也就是为什么打开一个新页面会有点慢，因为本地没什么缓存，要这样递归地查询下去。
多说一句，例如"mp3.baidu.com"，域名先是解析出这是个.com的域名，然后跑到管理.com域名的服务器上进行进一步查询，然后是.baidu，最后是mp3，
所以域名结构为：三级域名.二级域名.一级域名。
浏览器终于得到了IP以后，浏览器接着给这个IP的服务器发送了一个http请求，方式为get，例如访问nbut.cn

这个get请求包含了主机（host）、用户代理(User-Agent)，用户代理就是自己的浏览器，它是你的"代理人"，Connection（连接属性）中的keep-alive表示浏览器告诉对方服务
器在传输完现在请求的内容后不要断开连接，不断开的话下次继续连接速度就很快了。其他的顾名思义就行了。还有一个重点是Cookies，Cookies保存了用户的登陆信息，在每
次向服务器发送请求的时候会重复发送给服务器。Corome上的F12与Firefox上的firebug(快捷键shift+F5)均可查看这些信息。
发送完请求接下来就是等待回应了
当然了，服务器收到浏览器的请求以后（其实是WEB服务器接收到了这个请求，WEB服务器有iis、apache等），它会解析这个请求（读请求头），然后生成一个响应头和具体响
应内容。接着服务器会传回来一个响应头和一个响应，响应头告诉了浏览器一些必要的信息，例如重要的Status Code，2开头如200表示一切正常，3开头表示重定向，4开头，
如404，呵呵。响应就是具体的页面编码，就是那个......，浏览器先读了关于这个响应的说明书（响应头），然后开始解析这个响应并在页面上显示出来。在下一次CF的时候（不
是穿越火线，是http://codeforces.com/），由于经常难以承受几千人的同时访问，所以CF页面经常会出现崩溃页面，到时候可以点开火狐的firebug或是Chrome的F12看看状
态，不过这时候一般都急着看题和提交代码，似乎根本就没心情理会这个状态吧-.-。
如果是个静态页面，那么基本上到这一步就没了，但是如今的网站几乎没有静态的了吧，基本全是动态的。所以这时候事情还没完，根据我们的经验，浏览器打开一个网址的时
候会慢慢加载这个页面，一部分一部分的显示，直到完全显示，最后标签栏上的圈圈就不转了。

这是因为，主页（index）页面框架传送过来以后，浏览器还要继续向服务器发送请求，请求的内容是主页里面包含的一些资源，如图片，视频，css样式等等。这些"非静态"的
东西要一点点地请求过来，所以标签栏转啊转，内容刷啊刷，最后全部请求并加载好了就终于好了。

需要说明的是，对于静态的页面内容，浏览器通常会进行缓存，而对于动态的内容，浏览器通常不会进行缓存。缓存的内容通常也不会保存很久，因为难保网站不会被改动。

### ping 网站的详细过程

### ARP 协议和 RARP 协议

### DNS 域名解析的过程

### CDN 原理

### RPC

#### 为什么要有 RPC？

#### 什么是 RPC？

#### RPC 架构组件

## Linux

### 常用命令

#### cd/cp/mv/pwd/clear/echo

#### touch/mkdir/rm/rmdir/ln

#### chmod/chown

#### df/du/ls

#### cat/more/less/head/tail

#### find/which/locate/

#### sed

#### grep

#### ifconfig

#### netstat

#### vmstat

#### ssh

#### free

#### kill

#### ps

#### top

#### tar/unzip/gzip/bzip2

####

## 设计模式

### 设计原则

### 工厂方法模式

### 抽象工厂模式

### 单例模式

### 建造者模式

### 原型模式

### 适配器模式

### 装饰器模式

### 代理模式

## 算法

### 如何给 100 亿个数字排序？

1. 把这个37GB的大文件，平均分成1000个小文件。
2. 拆分完了之后，得到一些几十MB的小文件，那么就可以放进内存里排序了，可以用快速排序，归并排序，堆排序等等。
3. 1000个小文件内部排好序之后，就要把这些内部有序的小文件，合并成一个大的文件，可以用二叉堆来做1000路合并的操作，每个小文件是一路，合并后的大文件仍然有序。
4. 首先遍历1000个文件，每个文件里面取第一个数字，组成 (数字, 文件号) 这样的组合加入到堆里（假设是从小到大排序，用小顶堆），遍历完后堆里有1000个 (数字，文件号) 这样的元素。
5. 然后不断从堆顶拿元素出来，每拿出一个元素，把它的文件号读取出来，然后去对应的文件里，加一个元素进入堆，直到那个文件被读取完。拿出来的元素当然追加到最终结果的文件里。
6. 按照上面的操作，直到堆被取空了，此时最终结果文件里的全部数字就是有序的了。

### 统计 100 亿个 ip 中出现次数最多的前 10 个

## 场景题

### 一千万用户实时排名如何实现？

### 五万人并发抢票如何实现？

### 如何提高服务器并发处理能力？

### 你的项目中使用过缓存机制吗？有没用用户非本地缓存？

### 网站访问慢如何处理？

找原因：

1. 服务器出口带宽不够用。
    - 本身服务器购买的出口带宽比较小。一旦并发量大的话，就会造成分给每个用户的出口带宽就小，访问速度自然就会慢。
    - 跨运营商网络导致带宽缩减。例如，公司网站放在电信的网络上，那么客户这边对接是长城宽带或联通，这也可能导致带宽的缩减。
2. 服务器负载过大，导致响应不过来。
    - 分析系统负载，使用 w 命令或者 uptime 命令查看系统负载。如果负载很高，则使用 top 命令查看 CPU ，MEM 等占用情况，要么是 CPU 繁忙，要么是内存不够。
    - 如果这二者都正常，再去使用 sar 命令分析网卡流量，分析是不是遭到了攻击。一旦分析出问题的原因，采取对应的措施解决，如决定要不要杀死一些进程，或者禁止一些访问等。
3. 数据库瓶颈。
    - 如果慢查询比较多。那么就要开发人员或 DBA 协助进行 SQL 语句的优化。
    - 如果数据库响应慢，考虑可以加一个数据库缓存，如 Redis 等。然后，也可以搭建 MySQL 主从，一台 MySQL 服务器负责写，其他几台从数据库负责读。
4. 网站开发代码没有优化好。
    - 例如 SQL 语句没有优化，导致数据库读写相当耗时。

排查：

1. 首先要确定是用户端还是服务端的问题。当接到用户反馈访问慢，那边自己立即访问网站看看，如果自己这边访问快，基本断定是用户端问题，就需要耐心跟客户解释，协助客户解决问题。不要上来就看服务端的问题。一定要从源头开始，逐步逐步往下。
2. 如果访问也慢，那么可以利用浏览器的调试功能，看看加载那一项数据消耗时间过多，是图片加载慢，还是某些数据加载慢。
3. 针对服务器负载情况。查看服务器硬件(网络、CPU、内存)的消耗情况。如果是购买的云主机，比如阿里云，可以登录阿里云平台提供各方面的监控，比如 CPU、内存、带宽的使用情况。
4. 如果发现硬件资源消耗都不高，那么就需要通过查日志，比如看看 MySQL慢查询的日志，看看是不是某条 SQL 语句查询慢，导致网站访问慢。怎么去解决？

解决：

1. 如果是出口带宽问题，那么久申请加大出口带宽。
2. 如果慢查询比较多，那么就要开发人员或 DBA 协助进行 SQL 语句的优化。
3. 如果数据库响应慢，考虑可以加一个数据库缓存，如 Redis 等等。然后也可以搭建MySQL 主从，一台 MySQL 服务器负责写，其他几台从数据库负责读。
4. 申请购买 CDN 服务，加载用户的访问。
5. 如果访问还比较慢，那就需要从整体架构上进行优化咯。做到专角色专用，多台服务器提供同一个服务。
