# Go 语言教程


<!--more-->

## 一般结构

```go
// 包
package main

// 导包
import (
   "fmt"
)

// 常量
const c = "C"

// 变量
var v int = 5

// 自定义类型
type T struct {

}

// init() 函数，最先执行
func init() {

}

// main() 函数，其次执行
func main() {
   var a int
   Func1()
   // ...
   fmt.Println(a)
}

// 自定义可导出（公开）函数
func Func1() {
   //...
}

// 自定义不可导出（私有）函数
func func2() {

}
```

## 规范

- 虽然不需要分号作为语句的结束，但实际上这一过程是由编译器自动完成
- 不存在隐式类型转换，所有类型转换必须显式说明，如`a := int(b)`
- 标识符的命名规则遵循骆驼命名法
- `\`作为多行连接符

### 注释

```go
// 单行注释
/*
多行注释
*/
```

- 可通过`godoc`来导出注释，显示文档说明。
- 每一个包应该有相关注释。
- 在`package`语句之前的块注释将被默认认为是这个包的文档说明，称为**包注释**。
- 一个包可以有多个文件，只需要在其中一个文件中添加包注释。
- 所有全局的类型、常量、变量、函数和被导出的对象都应有注释。若出现在函数前面，称为**文档注释**，例如函数`Abc()`，则注释为`// Abc ...`。

### 可见性

- `public`：标识符以大写字母开头，如`Name`。
- `private`：标识符以小写字母开头，如`id`。

### 包

```go
import (
    fm "fmt"      // 别名
   "os"
   "./local_pkg" // 本地包
)
```
规范：

- 导入的包未使用报错。

### 函数

```go
func funcName(p1 int, p2 int) (add int, sub int) {
    return p1 + p2, p1 - p2
}

// 函数很短，也可以放在同一行
func Sum(a, b int) int { return a + b }
```

规范：



`main()`函数：

- `main()`函数是程序第一个执行的函数（如果有`init()`函数则会先执行`init()`）
- `main`包必须包含`main()`函数
- `main()`函数既没有参数，也没有返回类型

### 类型

- 基本类型
  - `int`、`float`、`bool`、`string`
- 复杂类型
  - `struct`、`array`、`slice`、`map`、`channel`、`interface`
  - 空值：`nil`
- 类型别名
  - `type si map[string]int`类似`#define si map<string, int>`或`typedef map<string, int> si;`

```go
type (
    IZ int
    FZ float64
    STR string
)
```

#### 常量

```go
const Pi float = 3.14159265358979323846

const (
	Unknown = 0
	Female = 1
	Male = 2
)
```

- 类型包括：`bool`、`int`、`float`、`complex`、`string`
- 类型可省略
- 任何精度，不会溢出

```go
// 赋值一个常量时，之后没赋值的常量都会应用上一行的赋值表达式
const (
	a = iota  // a = 0
	b         // b = 1
	c         // c = 2
	d = 5     // d = 5   
	e         // e = 5
)
```

```go
const (
	Open = 1 << iota  // 0001
	Close             // 0010
	Pending           // 0100
)
```

- 每遇到一次`const`关键字，`iota`就重置为`0`

#### 变量

```go
var a, b int = 1, 2
var (
    c int
    d bool
    e string
)
```

- 变量声明后，自动赋零值
  - `int`为`0`，`float`为`0.0`，`bool`为`false`，`string`为`""`，指针为`nil`。
- 内层代码块中可使用与外部代码相同名称的变量，此时外部的同名变量将会暂时隐藏（内层不改变外部变量值）

