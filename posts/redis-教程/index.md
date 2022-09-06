# Redis 教程


<!--more-->

## 安装

```bash
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

sudo apt-get update
sudo apt-get install redis

sudo service redis-server start

redis-cli
```

## 基础

### 数据类型

- Key
    - `string`：字符串。任意值都可以作为 key，包括字符串、整数、图片、视频、空字符串等。最大 512M。
- Value
    1. `string`：字符串。任意值都可以作为 key，包括字符串、整数、图片、视频、空字符串等。最大 512M。
    2. `list`：双向链表。

### 字符串操作命令

- `set/get/getset/mset/mget`：存取命令。

```bash
# 赋值
set <KEY> <VALUE>

# 取值，空返回 nil
get <KEY>

# 空才赋值
set <KEY> <VALUE> nx

# 非空才赋值
set <KEY> <VALUE> xx

# 赋值并返回原值
getset <KEY> <VALUE>

# 同时赋值多个 key
mset <KEY1> <VALUE1> <KEY2> <VALUE2> ...

# 同时取值多个 key
mget <KEY1> <KEY2> ...
```

- `incr/incrby/decr/decrby`：加减命令，原子操作。

```bash
# 自增 1
incr <KEY>

# 加 N
incrby <KEY> <N>

# 自减 1
decr <KEY>

# 减 N
decrby <KEY> <N>
```

- `exists/del/type`：判断 key。

```bash
# key 存在返回 1，否则返回 0
exists <KEY>

# 删除 key，成功返回 1，失败返回 0
del <KEY>

# 返回 value 类型，不存在返回 none
type <KEY>
```

- `expire/ttl/pexpire/pttl/persist`：key 自动失效。

```bash
# 设置/更新存活时间，单位秒，精度毫秒
expire <KEY> <TIME>

# 赋值，同时设置/更新存活时间，单位秒，精度毫秒
set <KEY> <VALUE> ex <TIME>

# 返回剩余存活时间，单位秒，已失效返回 -2，永久保存返回 -1
ttl <KEY>

# 设置/更新存活时间，单位毫秒
pexpire <KEY> <TIME>

# 赋值，同时设置/更新存活时间，单位毫秒
set <KEY> <VALUE> px <TIME>

# 返回剩余存活时间，单位毫秒，已失效返回 -2，永久保存返回 -1
pttl <KEY>

# 去除存活时间，永久保存，成功返回 1，失败返回 0
persist <KEY>
```

### 双向链表操作命令

- `lpush/rpush`：添加。

```bash
# 左侧添加
lpush <LIST> <VALUE1> <VALUE2> ...

# 右侧添加
rpush <LIST> <VALUE1> <VALUE2> ...
```

- `lpop/rpop`：删除。

```bash
# 左侧删除，空返回 nil
lpop <LIST>

# 右侧删除，空返回 nil
rpop <LIST>
```

- `lrange`：遍历。

```bash
# 返回列表 [start, end] 范围内的元素，start 和 end 可以取负数，-1 表示最后一个元素
# 空返回 empty array
lrange <LIST> <START> <END>

# 返回整个列表的值，空返回 empty array
lrange <LIST> 0 -1
```

- `ltrim`：截取。

```bash
# 截取列表 [start, end] 范围内的元素
ltrim <LIST> <START> <END>
```


## 参考

1. [Redis data types tutorial | Redis](https://redis.io/docs/data-types/tutorial/)

