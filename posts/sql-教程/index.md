# SQL 教程


结构化查询语言（Structured Query Language，SQL），读作`S-Q-L`或者`sequel`，是用来与数据库通信的语言。

<!--more-->

> SQL关键字不区分大小写。

## 一、数据库

- **显示数据库**

```sql
SHOW DATABASES;
```

- **选择数据库**

```sql
USE <database>;
```

- **显示创建数据库的 SQL 语句**

```sql
SHOW CREATE DATABASE <database>;
```

- **创建数据库**

```sql
CREATE DATABASE <database>;
CREATE DATABASE IF NOT EXISTS <database>;
```

- **删除数据库**

```sql
DROP DATABASE <database>;
```

<br />

## 二、表

- **显示表**

```sql
SHOW TABLES;
```

- **描述表**

```sql
DESCRIBE <table>;
SHOW COLUMNS FROM <table>;
```

- **显示创建表的 SQL 语句**

```sql
SHOW CREATE TABLE <table>;
```

- **创建表**

```sql
CREATE TABLE <table> (
    <column1> INT NOT NULL AUTO_INCREMENT COMMENT 'column1',
    <column2> CHAR(10) NOT NULL DEFAULT '' COMMENT 'column2',
    <column3> VARCHAR(20) NULL COMMENT 'column3',
    PRIMARY KEY (column1, column2)
);
```

- **删除表**

```sql
DROP TABLE <table>;
```

<br />

## 三、记录

- **添加**

```sql
INSERT INTO <table> (<column1>, <column2>)
VALUES (<values1>, <values2>);
```

- **删除**

```sql
DELETE FROM <table> WHERE <condition>;
```

- **修改**

```sql
UPDATE <table> SET <column1>=<value1>, <column2>=<value2> WHERE <condition>;
```

<br />

### 3.1 查询

- **条件查询**

```sql
SELECT <column1>, <column2> FROM <table> WHERE <condition>;
SELECT * FROM <table> WHERE <condition>;
```

- **去除重复值**

```sql
SELECT DISTINCT <column1>, <column2> from <table>;
```

<br />

## 五、查询

### 5.3 指定结果行数

`start`从0开始。

```sql
select <columns> from <table> limit <size>;
select <columns> from <table> limit <start>, <size>;
select <columns> from <table> limit <size> offset <start>;
```

{{< admonition tip 示例 false >}}
```sql
-- demo
-- +------+----------+
-- | id   | name     |
-- +------+----------+
-- |    1 | zhangsan |
-- |    1 | lisi     |
-- |    2 | zhangsan |
-- |    2 | lisi     |
-- +------+----------+
```

```sql
select id, name from demo limit 2;
-- +------+----------+
-- | id   | name     |
-- +------+----------+
-- |    1 | zhangsan |
-- |    1 | lisi     |
-- +------+----------+
```

```sql
select id, name from demo limit 1, 2;
-- +------+----------+
-- | id   | name     |
-- +------+----------+
-- |    1 | lisi     |
-- |    2 | zhangsan |
-- +------+----------+
```

```sql
select id, name from demo limit 2 offset 1;
-- +------+----------+
-- | id   | name     |
-- +------+----------+
-- |    1 | lisi     |
-- |    2 | zhangsan |
-- +------+----------+
```
{{< /admonition >}}

### 5.4 结果排序

- 默认升序`asc`，`desc`表示降序，只针对当前列。
- 优先排序靠前的列。
- 排序的列不一定要显示。

```sql
select <columns> from <table> order by <column>;
select <column1> from <table> order by <column2> desc;
select <column1>, <column2>, <column3> from <table> order by <column3> desc, <column1>;
```

### 5.5 条件查询

```sql
select <columns> from <table> where <condition>;
```

|操作符|说明|
|:--:|:--:|
|`=`|等于|
|`<>`或`!=`|不等于|
|`<`|小于|
|`<=`|小于等于|
|`>`|大于|
|`>=`|大于等于|
|`between <a> and <b>`|`[a,b]`|
|`is null`|空|

|逻辑操作符|说明|
|:--:|:--:|
|`and`|与|
|`or`|或|
|`not`|非|
|`in (<a>, <b>, <c>)`|等于`a`或`b`或`c`|

#### 5.5.1 通配符

- `%`表示任意字符任意次数（不能匹配NULL）
- `_`表示单个任意字符

```sql
select <columns> from <table> where <column> like <pattern>;
```

#### 5.5.2 正则表达式

- `\\`两个反斜杠转义字符。

```sql
select <columns> from <table> where <column> regexp <pattern>;
```

|字符类|说明|
|:--:|:--:|
|`[:alnum:]`||
|`[:alpha:]`||
|`[:blank:]`||
|`[:cntrl:]`||
|`[:digit:]`||
|`[:graph:]`||
|`[:lower:]`||
|`[:upper:]`||
|`[:print:]`||
|`[:punct:]`||
|`[:space:]`||
|`[:xdigit:]`||

#### 5.5.3 生成新列

- 拼接：

```sql
select concat(<columns>) from <table>;
```

- 计算：

```sql
select <column1> <op> <column2> as <alias> from <table>;
```

{{< admonition tip 示例 false >}}
```sql
-- demo
-- +------+----------+
-- | id   | name     |
-- +------+----------+
-- |    1 | zhangsan |
-- |    1 | lisi     |
-- |    2 | zhangsan |
-- |    2 | lisi     |
-- +------+----------+
```

```sql
select concat(id, ' ', name) as id_name from demo;
-- +------------+
-- | id_name    |
-- +------------+
-- | 1 zhangsan |
-- | 1 lisi     |
-- | 2 zhangsan |
-- | 2 lisi     |
-- +------------+
```

```sql
select id * 2 as id_double from demo;
-- +-----------+
-- | id_double |
-- +-----------+
-- |         2 |
-- |         2 |
-- |         4 |
-- |         4 |
-- +-----------+
```
{{< /admonition >}}

## 六、函数

### 6.1 文本处理

|函数|说明|
|:--:|:--:|
|`left()`|返回串左边的字符|

### 6.2 日期和时间处理

|函数|说明|
|:--:|:--:|
|`adddate()`|增加一个日期（天、周等）|

