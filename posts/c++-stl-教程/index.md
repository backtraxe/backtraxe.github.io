# C++ STL 教程


标准模板库（Standard Template Library，STL）是一组 C++ 模板类，提供常见的数据结构和函数，如列表、堆栈、数组等。它是由容器类、算法和迭代器构成的一个通用库，它的组件是参数化的。

<!--more-->

STL 包含以下四个组件：

1. 算法（Algorithms）：头文件`<algorithm>`定义了一组函数，作用于容器，并为容器中的内容提供各种操作方法。
1. 容器（Containers）：用来管理某一类对象的集合。C++ 提供了各种不同类型的容器，如 deque、list、vector、map、set、bitset 等。
1. 函数（Functions）：
1. 迭代器（Iterators）：遍历容器。

## `<vector>`

`vector`是一个动态数组，需要`#include <vector>`。

- 数组大小动态改变
- 可以进行逻辑操作（是否相等、比较大小）

### `vector`

#### 1.1 初始化

- `vector()` 初始化为空
- `explicit vector(size_type n)` 初始化为 n 个 0
- `vector(size_type n, const value_type& val)` 初始化为 n 个 val
- `vector(InputIterator first, InputIterator last)` 初始化为数组或迭代器 [first, last) 区间内的元素
- `vector(const vector& x)` 复制 vector 中的元素
- `vector(initializer_list<value_type> il)` 复制指定列表中的元素
- `vector& operator=(const vector& x)` 复制 vector 中的元素
- `vector& operator=(initializer_list<value_type> il)` 复制指定列表中的元素

```cpp
vector<int> v1;                              // {}
vector<int> v2 = {1, 2, 3};                  // {1, 2, 3}
vector<int> v3({1, 2, 3});                   // {1, 2, 3}
vector<int> v4 = v3;                         // {1, 2, 3}
vector<int> v5(v3);                          // {1, 2, 3}
vector<int> v6(3);                           // {0, 0, 0}
vector<int> v7(3, 2);                        // {2, 2, 2}
int arr[] = {1, 2, 3};
vector<int> v8(arr, arr + 1);                // {1}
vector<int> v9(v4.begin(), v4.begin() + 2);  // {1, 2}
vector<vector<int>> v10(2, vector<int>(3));  // {{0, 0, 0}, {0, 0, 0}}
```

> 类型任意，长度可以是变量

#### 1.2 添加

- `void push_back(const value_type& val)` 在末尾添加元素
- `void emplace_back(Args&&... args)` 在末尾构造并插入元素
- `iterator emplace(const_iterator position, Args&&... args)` 在指定位置构造并插入元素
- `iterator insert(const_iterator position, const value_type& val)` 在指定位置插入元素
- `iterator insert(const_iterator position, size_type n, const value_type& val)` 在指定位置插入 n 个 val
- `iterator insert(const_iterator position, InputIterator first, InputIterator last)` 在指定位置插入数组或迭代器 [first, last) 区间内的元素
- `iterator insert(const_iterator position, initializer_list<value_type> il)` 在指定位置插入指定列表中的元素

```cpp
vector<pair<string, int>> v;
v.push_back(make_pair("Mike", 1));
v.emplace_back("John", 2);  // 隐式地构造了 pair 并插入末尾
```

#### 1.3 删除

- `void pop_back()` 删除最后一个元素
- `iterator erase(const_iterator position)` 删除指定位置的元素
- `iterator erase(const_iterator first, const_iterator last)` 删除迭代器 [first, last) 区间内的元素
- `void clear() noexcept` 清空

#### 1.4 容量

- `bool empty() const` 判断容器是否为空
- `size_type size() const` 返回元素个数
- `size_type capacity() const noexcept` 返回已分配存储容量的大小
- `void resize(size_type n)` 改变大小，变小截断，变大补 0
- `void resize(size_type n, const value_type& val)` 改变大小，变小截断，变大补 val

#### 1.5 其他操作

- `void assign(size_type n, const value_type& val)` 赋值为 n 个 val
- `void assign(InputIterator first, InputIterator last)` 赋值为数组或迭代器 [first, last) 区间内的元素
- `void assign(initializer_list<value_type> il)` 赋值为指定列表中的元素
- `void swap(vector& x)` 交换两个 vector

#### 1.6 遍历

- `reference operator[](size_type n)` 返回位置为 n 的元素的引用，越界发生未知错误（不推荐）
- `reference at(size_type n)` 返回位置为 n 的元素的引用，越界抛出 out_of_range 异常（推荐）
- `reference front()` 返回第一个元素的引用
- `reference back()` 返回最后一个元素的引用

```cpp
for (auto it = v.begin(); it != v.end(); it++)   {*it;}  // 正向遍历，有迭代器
for (auto it = v.rbegin(); it != v.rend(); it++) {*it;}  // 反向遍历，有迭代器
for (int& e : v) {e;}                                    // 正向遍历，无索引
for (int e : v)  {e;}                                    // 正向遍历，无索引，不改变原数据
for (int i = 0; i < v.size(); i++)      {v.at(i);}       // 正向遍历，有索引
for (int i = v.size() - 1; i >= 0; i--) {v.at(i);}       // 反向遍历，有索引
```

### `vector<bool>`

基本操作同 [vector](#vector-1)。

特殊操作：

- `void flip() noexcept` 所有位都翻转
- `static void swap(reference ref1, reference ref2) noexcept` 交换两个位置的值

```cpp
vector<bool> mask;
mask.push_back(true);
mask.push_back(false);        // {1 0}
mask.flip();                  // {0 1}
mask.swap(mask[0], mask[1]);  // {1 0}
```

## `<stack>`

`stack`是一个栈，需要`#include <stack>`。

- 后进先出（LIFO）

### 2.1 初始化

- 默认底层容器是 [deque](#deque)
- 可以显式设置底层容器为 [vector](#vector-1)

```cpp
stack<int> st1;
stack<int> st2(st1);
stack<int> st3({1, 2, 3});       // st3.top() == 3
deque<int> dq(2, 3);
stack<int> st4(dq);              // 默认底层容器是 deque
vector<int> v({1, 2, 3});
stack<int, vector<int>> st5(v);  // 设置底层容器为 vector
```

### 2.2 操作

- `void push(const value_type& val)` 栈顶添加元素
- `void emplace(Args&&... args)` 栈顶添加元素
- `void pop()` 栈顶弹出元素
- `reference& top()` 返回栈顶元素
- `bool empty() const` 判断栈是否为空
- `size_type size() const` 返回元素个数
- `void swap(stack& x) noexcept` 交换两个栈

## `<list>`

`list`是一个双向链表，需要`#include <list>`。

- 无法按索引访问元素
- 插入删除元素效率高

### 3.1 基本操作

基本操作同 [vector](#vector-1)。

### 3.2 特殊操作

添加：

- `void push_front(const value_type& val)` 在开头插入元素
- `void emplace_front(Args&&... args)` 在开头构造并插入元素
- `void splice(const_iterator position, list& x)` 将 x 中的元素转移到指定位置
- `void splice(const_iterator position, list& x, const_iterator i)` 将 x 中的位置为 i 元素转移到指定位置
- `void splice(const_iterator position, list& x, const_iterator first, const_iterator last)` 将 x 中的 [first, last) 区间内的元素转移到指定位置
- `void merge(list& x)`
- `void merge(list& x, Compare comp)`

删除：

- `void pop_front()` 删除第一个元素
- `void remove(const value_type& val)` 删除值为 val 的所有元素
- `void remove_if(Predicate pred)` 删除满足自定义一元函数的元素
- `void unique()` 删除连续重复元素，只保留一个
- `void unique(BinaryPredicate binary_pred)` 删除满足自定义二元函数的元素

其他：

- `void sort()` 按升序排序
- `void sort(Compare comp)` 按自定义二元函数排序
- `void reverse() noexcept` 逆序

```cpp
list<int> l = {1, 1, 1, 2, 1, 2};
l.unique();  // {1, 2, 1, 2}
```

## \<queue>

`queue`是一个单向队列容器，需要`#include <list>`。

### queue

- 先进先出（FIFO）
- 队尾添加，队首删除

#### 4.1.1 初始化

```cpp
queue<int> q1;                            // 空 queue
queue<int> q2(5, 2);                      // 大小为 5 的 queue，值均为 2
queue<int> q3(q2);                        // 复制 queue
int arr[] = {1, 2, 3};
queue<int> q4(arr, arr + 1);              // 复制数组 [first, last) 区间内的元素
vector<int> v = {1, 2, 3};
queue<int> q5(v.begin(), v.begin() + 1);  // 复制迭代器 [first, last) 区间内的元素
queue<int> q6(v);                         // 复制 vector
```

#### 4.1.2 操作

- `void push(const value_type& val)` 队尾添加元素
- `void emplace(Args&&... args)` 队尾添加元素
- `void pop()` 删除队首元素
- `const_reference& front() const` 返回队首元素
- `const_reference& back() const` 返回队尾元素
- `size_type size() const` 返回大小
- `bool empty() const` 是否为空
- `void swap(queue& x) noexcept` 交换

### priority_queue

- 优先队列（堆）
- 默认最大优先队列（最大堆）
- 自动调整顺序使队首（堆顶）元素最大

#### 4.2.1 初始化

```cpp
priority_queue<int> pq1;                             // 空 priority_queue
priority_queue<int> pq2(5, 2);                       // 大小为 5 的 priority_queue，值均为 2
priority_queue<int> pq3(pq2);                        // 复制 priority_queue
int arr[] = {1, 2, 3};
priority_queue<int> pq4(arr, arr + 1);               // 复制数组 [first, last) 区间内的元素
vector<int> v = {1, 2, 3};
priority_queue<int> pq5(v.begin(), v.begin() + 1);   // 复制迭代器 [first, last) 区间内的元素
priority_queue<int> pq6(v);                          // 复制 vector
priority_queue<int, vector<int>, less<int>> pq7;     // 最大优先队列（最大堆）
priority_queue<int, vector<int>, greater<int>> pq8;  // 最小优先队列（最小堆）
```

#### 4.2.2 操作

- `void push(const value_type& val)` 添加元素
- `void emplace(Args&&... args)` 添加元素
- `void pop()` 删除队首（堆顶）元素
- `const_reference top() const` 返回队首（堆顶）元素
- `size_type size() const` 返回大小
- `bool empty() const` 是否为空
- `void swap(priority_queue& x) noexcept` 交换

## \<deque>

`deque`是一个双端队列容器，需要`#include <deque>`。

### 5.1 初始化

```cpp
deque<int> dq1;                                // 空 deque
deque<int> dq2(5, 2);                          // 大小为 5 的 deque，值均为 2
deque<int> dq3(dq2);                           // 复制 deque
deque<int> dq4(dq2.begin(), dq2.begin() + 1);  // 复制迭代器 [first, last) 区间内的元素
int arr[] = {1, 2, 3};
deque<int> dq5(arr, arr + 1);                  // 复制数组 [first, last) 区间内的元素
vector<int> v = {1, 2, 3};
deque<int> dq6(v.begin(), v.begin() + 1);      // 复制迭代器 [first, last) 区间内的元素
deque<int> dq7 = dq6;
deque<int> dq8 = {1, 2, 3};
```

### 5.2 修改

- `void push_back(const value_type& val)` 队尾添加元素
- `void push_front(const value_type& val)` 队首添加元素
- `void emplace_back(Args&&... args)` 队尾添加元素
- `void emplace_front(Args&&... args)` 队首添加元素
- `iterator emplace(const_iterator position, Args&&... args)` 迭代器指定位置前面添加元素
- `iterator insert(const_iterator position, const value_type& val)` 迭代器指定位置前面添加元素
- `iterator insert(const_iterator position, size_type n, const value_type& val)` 迭代器指定位置前面添加 n 个相同元素
- `iterator insert(const_iterator position, InputIterator first, InputIterator last)` 迭代器指定位置前面添加 [first, last) 区间内元素
- `iterator insert(const_iterator position, initializer_list<value_type> il)`
- `void pop_back()` 删除队尾
- `void pop_front()` 删除队首
- `iterator erase(iterator position)` 删除迭代器指向元素
- `iterator erase(const_iterator first, const_iterator last)` 删除 [first, last) 区间内元素
- `void clear() noexcept` 清空

### 5.3 遍历

```cpp
deque<int> dq;
for (auto it = dq.begin(); it != dq.end(); it++) {*it;}
for (auto it = dq.rbegin(); it != dq.rend(); it++) {*it;}
for (int e : dq) {e;}
for (int& e : dq) {e;}
```

### 5.4 操作

- `size_type size() const noexcept` 返回大小
- `void resize(size_type n)` 调整大小为 n，调大补 0，调小末尾截断
- `void resize(size_type n, const value_type& val)` 调整大小为 n，调大补 val，调小末尾截断
- `bool empty() const noexcept` 判断是否为空
- `reference operator[](size_type n)` 访问指定位置元素，越界报错
- `reference at(size_type n)` 访问指定位置元素，越界抛出 out_of_range 异常
- `const_reference back() const` 返回队尾元素
- `const_reference front() const` 返回队首元素

## `<map>`

`map`是一个有序键值对容器，每个元素由关键字（key）和该关键字对应的值（value）组合而成。需要`#include <map>`。

### map

- `key`唯一且无法修改
- 默认按`key`升序排列
- 底层二叉搜索树实现，速度比`unordered_map`慢

#### 6.1 初始化

```cpp
map<char, int> m1;
map<char, int> m2(m1);
map<char, int> m3(m1.begin(), m1.end());
map<char, int, less<char>> m4;     // 按 key 升序排列，相当于 map<char, int>
map<char, int, greater<char>> m5;  // 按 key 降序排列
```

#### 6.2 添加

```cpp
map<char, int> m1, m2;
m1['a'] = 1;
m1.insert(make_pair('b', 2));
m1.insert(pair<char, int>('c', 3));
m1.emplace('d', 4);
m2.insert(m1.begin(), m1.find('c'));
```

#### 6.3 删除

```cpp
map<char, int> m;
m['a'] = 1;
m['b'] = 2;
m['c'] = 3;
m.erase(m.find('c'));
m.erase('a');
m.erase(m.begin(), m.end());
m.clear();
```

#### 6.4 遍历

- `mapped_type& operator[](const key_type& k)`
- `mapped_type& at(const key_type& k)`

```cpp
for (auto it = m.begin();  it != m.end();  it++) { it->first; it->second; }
for (auto it = m.rbegin(); it != m.rend(); it++) { it->first; it->second; }
for (auto &p : m) { p.first; p.second; }
for (auto &[k, v] : m) { k; v; }
for (auto &[_, v] : m) { k; v; }
for (auto &[k, _] : m) { k; v; }
```

#### 6.5 其他操作

- `size_type size() const noexcept` 返回元素个数
- `bool empty() const noexcept` 判断是否为空
- `void swap(map& x)` 交换两个 map
- `iterator find(const key_type& k)` 查找 key 值为 k 的元素，未找到返回 map::end()
- `size_type count(const key_type& k) const` 返回 key 值为 k 的元素的数量，由于 key 唯一，则存在返回 1，不存在返回 0
- `iterator lower_bound(const key_type& k)` 返回指向第一个 key 大于等于 k 的元素的迭代器
- `iterator upper_bound(const key_type& k)` 返回指向第一个 key 大于 k 的元素的迭代器
- `pair<iterator, iterator> equal_range(const key_type& k)` 返回指向 key 等于 k 的所有元素的范围的边界元素的迭代器 [first, second)

```cpp
map<char, int> m;
m['a'] = 0;
m.find('b');   // m.end()
m.count('a');  // 1
m.count('b');  // 0
```

#### 6.6 排序

> `map`没有随机迭代器，只有顺序迭代器，所以不能用`sort`

##### 6.6.1 按 key 排序

**key 升序，value 随机**

默认情况，`map<int, char>`，相当于`map<int, char, less<int>>`。

当 key 为自定义类时：

```cpp
typedef struct {  // 自定义类
    int one, two;
} Grade;

struct Cmp {      // 自定义比较类
    bool operator()(const Grade& a, const Grade& b) const {
        if (a.one != b.one) return a.one < b.one;
        return a.two < b.two;
    }
};

map<Grade, int, Cmp> m;
```

```cpp
typedef struct {  // 自定义类
    int one, two;
} Grade;

struct Cmp {      // 自定义比较类
    bool operator()(const Grade& a, const Grade& b) const {
        if (a.one != b.one) return a.one < b.one;
        return a.two < b.two;
    }
};

map<Grade, int, Cmp> m;
```

**key 降序，value 随机**

`map<int, char, greater<int>>`

##### 6.6.2 按 value 排序

**key 降序，value 随机**

**key 降序，value 随机**

```cpp

```

```cpp

```

```cpp

```

### multimap

- `key`允许重复
- 默认按`key`升序排列
- 底层二叉搜索树实现，速度比`unordered_multimap`慢

基本使用方法同 [map](#map-1)。

## \<unordered_map>

### unordered_map

- `key`唯一且不能修改，可以添加或删除
- 无序
- 速度比`map`快

### unordered_multimap

## \<set>

`set`是一个有序集合容器。需要`#include <set>`。

### set

- 元素唯一
- 元素默认升序
- 底层二叉排序树实现，速度比`unordered_set`慢

#### 初始化

```cpp
set<int> s1;                              // {}
set<int> s2 = {1, 2, 3};                  // { 1 2 3 }
set<int> s3 = s2;                         // { 1 2 3 }
set<int> s4({1, 2, 3});                   // { 1 2 3 }
int arr[] = {1, 2, 3};
set<int> s5(arr, arr + 3);                // { 1 2 3 }
set<int> s6(arr, arr + 1);                // { 1 }
set<int> s7(s4);                          // { 1 2 3 }
set<int> s8(s4.begin(), s4.end());        // { 1 2 3 }
set<int> s9(s4.begin(), s4.begin() + 1);  // { 1 }

struct CompClass {
    bool operator() (const int& left, const int& right) const {
        return left < right;
    }
};
set<int, CompClass> s10;                  // { 1 2 3 }
```

#### 修改

- `pair<iterator, bool> emplace(Args&&... args)` 添加一个元素
- `pair<iterator, bool> insert(value_type&& val)` 添加一个元素
- `void insert(InputIterator first, InputIterator last)` 添加 [first, last) 范围内的元素
- `void insert(initializer_list<value_type> il)` 添加另一个容器的所有元素
- `iterator erase(const_iterator position)` 删除指定位置元素
- `size_type erase(const value_type& val)` 删除指定元素
- `iterator erase(const_iterator first, const_iterator last)` 删除 [first, last) 范围内的元素
- `void swap(set& x)` 交换两个 set
- `void clear() noexcept` 清空

#### 容量

- `bool empty() const noexcept` 判断是否为空
- `size_type size() const noexcept` 当前元素个数

#### 遍历

```cpp
for (auto it = s.begin(); it != s.end(); it++) {*it;}
for (auto it = s.rbegin(); it != s.rend(); it++) {*it;}
for (int e : s) {e;}
for (int& e : s) {e;}
```

#### 操作

- `iterator find(const value_type& val)` 查找指定元素，成功返回迭代器，失败返回 end()
- `size_type count(const value_type& val) const` 返回指定元素的个数
- `iterator lower_bound(const value_type& val)` 下界，查找**第1个大于等于**指定元素的位置，成功返回迭代器，失败返回 end()
- `iterator upper_bound(const value_type& val)` 上界，查找**最后一个小于等于**指定元素的位置，成功返回迭代器，失败返回 end()
- `pair<iterator, iterator> equal_range(const value_type& val)` 返回 set 中与指定元素相等的一个范围 [first, second)

### multiset

- 允许重复元素
- 元素默认升序
- 速度比`unordered_set`慢
- 使用方法同`set`

## \<unordered_set>

`unordered_set`是一个无序集合容器。需要`#include <unordered_set>`。

### unordered_set

- 元素唯一
- 无序
- 底层哈希表实现，速度比`set`快
- 使用方法同`set`

### unordered_multiset

- 允许重复元素
- 无序
- 速度比`multiset`快
- 使用方法同`set`

## \<bitset>

`bitset`模拟一个 bool 数组，每个元素只能是 0 或 1.

### 初始化

```cpp
bitset<4> b1;           // 0000
bitset<4> b2("100");    // 0100, b2[0] == 0
bitset<4> b3("1100");   // 1100
bitset<4> b4("11100");  // 1110
bitset<4> b5(b2);       // 0100
string s = "1010";
bitset<4> b6(s);        // 1010
```

### 位运算

```cpp
bitset<4> a("1001"), b("1010");
a & b;   // 1000 AND
a | b;   // 1011 OR
a ^ b;   // 0011 XOR
~a;      // 0110 NOT
a << 1;  // 0010 SHL
a >> 1;  // 0100 SHR
a == b;  // false
a != b;  // true
```

### 操作

- `reference operator[](size_t pos)` 访问指定位置，**0 是最右边一位，即最低位**
- `size_t count() const noexcept` 返回 1 的 个数
- `size_t size() const noexcept` 返回长度
- `bool test(size_t pos) const` 判断指定位置是否为 1
- `bool any() const noexcept` 判断是否存在某一位是 1
- `bool none() const noexcept` 判断是否全是 0
- `bool all() const noexcept` 判断是否全是 1
- `bitset& set() noexcept` 全部置为 1
- `bitset& set(size_t pos, bool val = true)` 指定位置置为 1
- `bitset& reset() noexcept` 全部置为 0
- `bitset& reset(size_t pos)` 指定位置置为 0
- `bitset& flip() noexcept` 翻转
- `bitset& flip(size_t pos)` 翻转指定位置
- `string to_string() const` 返回该二进制数的字符串
- `unsigned long to_ulong() const` 返回该 2 进制数对应的整数，类型 unsigned long
- `unsigned long long to_ullong() const` 返回该 2 进制数对应的整数，类型 unsigned long long

## \<algorithm>

### sort

#### 数组排序

```cpp
bool cmp(int a, int b) { return a > b; }  // 自定义降序比较函数

int arr[] = {2, 3, 1};
sort(arr, arr + 3);             // {1, 2, 3}
sort(arr, arr + 3, cmp);        // {3, 2, 1}

vector<int> v(arr, arr + 3);
sort(v.begin(), v.end());       // {1, 2, 3}
sort(v.begin(), v.end(), cmp);  // {3, 2, 1}
```

#### 类（结构体）排序

```cpp
class Stu {  // 自定义类
public:
    int no;
    int score;
};

bool cmpClass(Stu& a, Stu& b) {  // 自定义类的降序比较函数
    return a.score > b.score;
}

Stu stu[] = {1, 90, 2, 100, 3, 80};  // {{1, 90}, {2, 100}, {3, 80}}
sort(stu, stu + 3, cmpClass);        // {{2, 100}, {1, 90}, {3, 80}}
```

### reverse

### lower_bound

### upper_bound

### search

## `<tuple>`

`tuple`将不同类型的许多元素打包成一个对象，便于访问，（就像定义了一个只有属性的类，并且属性只定义了类型，未定义名字）。需要`#include <tuple>`。

- 元素类型任意
- 元素数量任意

```cpp
tuple<int, string> t1;
tuple<int, string> t2{t1};
tuple<int, string> t3(t2);
tuple<int, string> t4{1, "one"};

get<0>(t4);       // 1
get<1>(t4);       // one
get<int>(t4);     // 1
get<string>(t4);  // one

make_tuple(2, string("two"));

```

## `<numeric>`

该头文件包括了一组对数组进行某些操作的算法。

### `accumulate`

- `T accumulate(InputIterator first, InputIterator last, T init)`：默认求和
- `T accumulate(InputIterator first, InputIterator last, T init,                  BinaryOperation binary_op)`：自定义函数

```cpp
int res = 0;
int arr[3] = {1, 2, 3};
vector<int> vec(arr, arr + 3);
accumulate(arr, arr + 3, res);                // 求和，6
accumulate(vec.begin(), vec.end(), res);      // 求和，6
accumulate(arr, arr + 3, res, minus<int>());  // 累减，-6
accumulate(arr, arr + 3, res, [z](int x, int y) { return x + 2 * y; });

```

## 参考

1. [Standard C++ Library Reference - cplusplus.com](https://www.cplusplus.com/reference/)

