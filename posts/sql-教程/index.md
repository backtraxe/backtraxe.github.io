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
USE db_name;
```

- **创建数据库**

```sql
CREATE DATABASE db_name;
CREATE DATABASE IF NOT EXISTS db_name;
```

- **删除数据库**

```sql
DROP DATABASE db_name;
```

- **显示创建数据库的 SQL 语句**

```sql
SHOW CREATE DATABASE db_name;
```

<br />

## 二、表

- **显示表**

```sql
SHOW TABLES;
```

- **描述表**

```sql
DESCRIBE tb_name;
SHOW COLUMNS FROM tb_name;
```

- **创建表**

```sql
CREATE TABLE tb_name (
    col_1 type_1 cons_1,
    col_2 type_2 cons_2,
    cons_3,
    cons_4
);

-- 使用旧表创建新表
CREATE TABLE tb_new LIKE tb_old;
```

- **显示创建表的 SQL 语句**

```sql
SHOW CREATE TABLE tb_name;
```

- **删除表**

```sql
DROP TABLE tb_name;
```

- **修改表名**

```sql
ALTER TABLE tb_old RENAME TO tb_new;
```

- **添加列**

```sql
ALTER TABLE tb_name ADD COLUMN col_ type_ cons_;
```

- **删除列**

```sql
ALTER TABLE tb_name DROP COLUMN col_;
```

- **修改列名**

```sql
ALTER TABLE tb_name RENAME col_old TO col_new;
```

- **添加约束**

```sql
ALTER TABLE tb_name ADD cons;

-- 添加主键
ALTER TABLE tb_name ADD PRIMARY KEY (col_);
```

- **删除约束**

```sql
ALTER TABLE tb_name DROP cons;

-- 删除主键
ALTER TABLE tb_name DROP PRIMARY KEY (col_);
```

- **创建索引**

```sql
CREATE INDEX idx_name ON ta_name (col_1, col_2);

-- 唯一索引
CREATE UNIQUE INDEX idx_name ON ta_name (col_1, col_2);
```

- **删除索引**

```sql
DROP INDEX idx_name;
```

<br />

## 三、修改数据

- **添加**

```sql
-- 添加多条记录
INSERT INTO tb_name (col_1, col_2) VALUES (val_1_1, val_1_2), (val_2_1, val_2_2);

-- 从其他表添加
INSERT INTO tb_name_1 (col_1, col_2) SELECT col_1, col_2 FROM tb_name_2;
```

- **删除**

```sql
-- 删除指定记录
DELETE FROM tb_name WHERE cond_;

-- 删除所有记录
DELETE FROM tb_name;
TRUNCATE TABLE tb_name;
```

- **修改**

```sql
UPDATE tb_name SET col_1=val_1, col_2=val_2 WHERE cond_;
```

<br />

## 四、查询数据

### 4.1 单表查询

- **条件查询**

```sql
-- 指定列
SELECT col_1, col_2 FROM tb_name WHERE cond_;

-- 所有列
SELECT * FROM tb_name;
```

- **去重**

```sql
SELECT DISTINCT col_ FROM tb_name;
```

- **分页**：`pos`从 0 开始。

```sql
-- [0, len)
SELECT col_ FROM tb_name LIMIT len_;

-- [pos, pos + len)
SELECT col_ FROM tb_name LIMIT pos_, len_;
SELECT col_ FROM tb_name LIMIT len_ OFFSET pos_;
```

- **排序**

```sql
-- 升序，ASC
SELECT col_ FROM tb_name ORDER BY col_1;

-- 降序
SELECT col_ FROM tb_name ORDER BY col_1 DESC;

-- 复杂排序
SELECT col_ FROM tb_name ORDER BY col_1 DESC col_2 ASC;
```

<br />

### 4.2 函数

- **数量**

```sql
SELECT COUNT(*) FROM tb_name;
SELECT COUNT(1) FROM tb_name;
```

- **求和**

```sql
SELECT SUM(col_) FROM tb_name;
```

- **平均值**

```sql
SELECT AVG(col_) FROM tb_name;
```

- **最大值**

```sql
SELECT MAX(col_) FROM tb_name;
```

- **最小值**

```sql
SELECT MIN(col_) FROM tb_name;
```

<br />

### 4.3 多表查询

```sql
--
SELECT col_ FROM tb_1 INNER JOIN tb_2 ON cond_;

SELECT col_ FROM tb_1 LEFT JOIN tb_2 ON cond_;

SELECT col_ FROM tb_1 RIGHT JOIN tb_2 ON cond_;

SELECT col_ FROM tb_1 INNER JOIN tb_2 ON cond_;
```

<br />

## 五、条件

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

## 六、约束


