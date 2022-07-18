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



