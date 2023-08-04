# C++ 时间相关操作


<!--more-->

## 时间点

### 输出当前时间

### 休眠到指定时刻

## 时间段

### 计算时间差

#### C语言风格，精度不高

```cpp
// #include <time.h>
#include <ctime>

clock_t st_time = clock();
// 其他程序
clock_t ed_time = clock();

// 以秒为单位
double time_dif = double (ed_time - st_time) / CLOCKS_PER_SEC;
```

#### C语言风格，Linux独占

```cpp
#include <sys/time.h>

// 数据结构定义
// struct timeval {
//     long tv_sec;  // 秒
//     long tv_usec; // 微秒（百万分之一秒）
// };

struct timeval st_time, ed_time;
gettimeofday(&st_time, NULL);
// 其他程序
gettimeofday(&ed_time, NULL);

// 以秒为单位
double time_dif = ed_time.tv_sec - st_time.tv_sec + (ed_time.tv_usec - st_time.tv_usec) / 1000000.0;
```

#### C++ 11风格，精度高，推荐

```cpp
#include <chrono>

auto st_time = std::chrono::steady_clock::now();
// 其他程序
auto ed_time = std::chrono::steady_clock::now();

// 以秒为单位
double time_dif = (ed_time - st_time).count() / 1000000.0;
```

### 休眠一段时间

```cpp
// 休眠5s
sleep(5);
```

```cpp
#include <chrono>
// 休眠5s
std::this_thread::sleep_for(std::chrono::seconds(5));
```
