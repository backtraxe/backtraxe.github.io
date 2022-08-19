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

## 四、查询

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

### 4.2

### 4.3 多表查询

```sql
-- 返回 tb_1 和 tb_2 的交集
SELECT col_ FROM tb_1 INNER JOIN tb_2 ON cond_;
-- 返回 tb_1 减去 tb_1 和 tb_2 的交集
SELECT col_ FROM tb_1 LEFT JOIN tb_2 ON cond_;
-- 返回 tb_2 减去 tb_1 和 tb_2 的交集
SELECT col_ FROM tb_1 RIGHT JOIN tb_2 ON cond_;
-- 返回 tb_1 和 tb_2 的笛卡尔积
SELECT col_ FROM tb_1 CROSS JOIN tb_2 ON cond_;
```

```sql
select * from test1;
-- +------+------+
-- | r1   | r2   |
-- +------+------+
-- |    1 |    1 |
-- |    1 |    2 |
-- +------+------+
select * from test2;
-- +------+------+
-- | r1   | r2   |
-- +------+------+
-- |    1 |    2 |
-- |    2 |    2 |
-- +------+------+
select * from test1, test2;
-- +------+------+------+------+
-- | r1   | r2   | r1   | r2   |
-- +------+------+------+------+
-- |    1 |    2 |    1 |    2 |
-- |    1 |    1 |    1 |    2 |
-- |    1 |    2 |    2 |    2 |
-- |    1 |    1 |    2 |    2 |
-- +------+------+------+------+
select * from test1, test2 where test1.r1 = test2.r1;
-- +------+------+------+------+
-- | r1   | r2   | r1   | r2   |
-- +------+------+------+------+
-- |    1 |    2 |    1 |    2 |
-- |    1 |    1 |    1 |    2 |
-- +------+------+------+------+
select * from test1 inner join test2 on test1.r1 = test2.r1;
-- +------+------+------+------+
-- | r1   | r2   | r1   | r2   |
-- +------+------+------+------+
-- |    1 |    2 |    1 |    2 |
-- |    1 |    1 |    1 |    2 |
-- +------+------+------+------+

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

## 函数

逻辑语句：

- `IF(condition, true_value, false_value)`：条件语句 condition 为真返回 true_value 否则返回 false_value。
- `CASE col_name WHEN case1 THEN value1 WHEN case2 THEN value2 ELSE value3 END`：switch 语句。
- `IFNULL(query, null_value)`：若查询 query 返回 null，则返回 null_value，否则返回 query 的值。

数值：

- `ROUND(value)`：四舍五入。
- `FLOOR(value)`：去尾法。
- `CEIL(value)`：进一法。同`CEILING`。
- `FORMAT(value, n)`：保留 n 位小数。

统计量：

- `COUNT(col_name) | COUNT(1) | COUNT(*)`：行数。
- `SUM(col_name)`：求和。
- `AVG(col_name)`：平均值。
- `MAX(col_name)`：最大值。
- `MIN(col_name)`：最小值。

字符串：

- `LENGTH(str)`：返回 str 长度。
- `CHAR_LENGTH(str)`：返回非 ASCII 码字符串长度。比如'你好'返回 2。
- `LEFT(str, n)`：返回 str 长度为 n 的前缀子串。
- `RIGHT(str, n)`：返回 str 长度为 n 的后缀子串。
- `SUBSTRING(str, start)`：返回 str 从第 start 个字符开始直到末尾的子串。同`SUBSTR`。
- `SUBSTRING(str, start, len)`：返回 str 从第 start 个字符开始长度为 len 的子串。同`SUBSTR`。
- `CONCAT(s1, s2, ...)`：返回拼接后的字符串，不限制参数数量。
- `CONCAT_WS(c, s1, s2, ...)`：用分隔符 c 将字符串拼接起来。
- `LOWER(str)`：转为小写。
- `UPPER(str)`：转为大写。
- `TRIM(str)`：去掉两侧空白字符。
- `REVERSE(str)`：翻转字符串。

日期：

- `DATEDIFF(date1, date2)`：返回 date1 减去 date2 的天数。

## 参考

- [MySQL :: MySQL 8.0 Reference Manual :: 12 Functions and Operators](https://dev.mysql.com/doc/refman/8.0/en/functions.html)
