# Java 面试


<!--more-->

## 路线图

- [MCA JAVA后端架构师](https://www.processon.com/view/link/61b2313b0e3e74683770741d#map)
- [CS-Notes](http://www.cyc2018.xyz/)
- [JavaGuide](https://javaguide.cn/home.html)
- [Java 全栈知识体系](https://pdai.tech/)
- [进击的java菜鸟](https://fhfirehuo.github.io/Attacking-Java-Rookie/)
- [labuladong 的算法小抄](https://labuladong.github.io/algo/)
- [DB-TUTORIAL](https://dunwu.github.io/db-tutorial/)

## MySQL

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


