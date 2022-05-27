# Java SE 教程


<!--more-->

## 0 名词解释

- JVM：Java Virtual Machine，Java 虚拟机。
- JRE：Java Runtime Environment，Java 运行环境，包含 JVM 和 Java 核心类库。
- JDK：Java Development Kit，Java 开发工具包，包含 JRE，编译工具和运行工具。

## 1 准备

### 1.1 JDK 下载

- [Oracle JDK](https://www.oracle.com/java/technologies/javase-downloads.html)
- [Oracle OpenJDK](https://jdk.java.net/)

> - Windows 可通过 Chocolatey 安装。
> - MacOS 可通过 Homebrew 安装。

### 1.2 环境变量配置

Windows：

1. `设置`→`系统`→`关于`→`高级系统设置`→`环境变量`。
2. 在下方的`系统变量`中找到`Path`，点击`编辑`。
3. 点击`新建`，将JDK的路径下的bin目录粘贴进去。（示例：`C:\Users\backs\Downloads\jdk-11.0.10\bin\`）
4. 连续点击`确定`，保存退出。
5. 打开cmd，输入`java -version`，若有如下所示输出即为配置成功。

```
java version "11.0.10" 2021-01-19 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.10+8-LTS-162)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.10+8-LTS-162, mixed mode)
```

6. 在任意位置创建 `test.java`，写入以下内容。

```java
public class test {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

7. 在**该文件夹**下打开命令行，输入以下内容，输出应该为 `Hello World!`。

```bash
javac test.java
java test
```

### 1.3 IDE Download

- [IntelliJ IDEA](https://www.jetbrains.com/idea/download/): 最好用，`Community`版免费，`Ultimate`版收费，学生可白嫖，推荐。

```text
psvm               public static void main(String[] args) {}
sout               System.out.println();
arr.fori           for (int i = 0; i < arr.length; i++) {}
arr.forr           for (int i = arr.length - 1; i >= 0; i--) {}
alt + 1            开/关左侧目录结构
alt + 4            开/关底部控制台
ctrl + alt + L     格式化代码
ctrl + /           单行注释
ctrl + shift + /   多行注释
shift + alt + ↑    上移
shift + alt + ↓    下移
ctrl + x           剪切当前行
ctrl + d           下方复制当前行
shift + enter      下方新建空行
ctrl + alt + enter 上方新建空行
ctrl + alt + v     自动定义变量来接收当前值
ctrl + alt + m     将选中代码生成独立方法
```

- [Eclipse](https://www.eclipse.org/downloads/): 开源，免费。

## 2 基础语法

### 2.1 注释

```java
// 单行注释

/*
多行注释
*/

/**
文档注释
*/
```

### 2.2 常量

- 字符串常量：`"helloworld"`，`"程序员"`
- 整数常量：`12`，`-3`
- 浮点数常量：`1.2`，`-3.4`
- 字符常量：`'a'`，`'\n'`，`'我'`
- 布尔常量：`true`，`false`
- 空常量：`null`

### 2.3 数据类型

<table>
  <tr>
    <th>数据类型</th>
    <th>关键字</th>
    <th>内存占用（字节）</th>
    <th>取值范围</th>
  </tr>
  <tr align="center">
    <td rowspan=4>整数</td>
    <td><code>byte</code></td>
    <td>1</td>
    <td>-128~127</td>
  </tr>
  <tr align="center">
    <td><code>short</code></td>
    <td>2</td>
    <td>-32,768~32,767</td>
  </tr>
  <tr align="center">
    <td><code>int</code></td>
    <td>4</td>
    <td>-2,147,483,648~2,147,483,647</td>
  </tr>
  <tr align="center">
    <td><code>long</code></td>
    <td>8</td>
    <td>$$-2^{63} \sim 2^{63} - 1$$</td>
  </tr>
  <tr align="center">
    <td rowspan=2>浮点数</td>
    <td><code>float</code></td>
    <td>4</td>
    <td>小数点后6位</td>
  </tr>
  <tr align="center">
    <td><code>double</code></td>
    <td>8</td>
    <td>小数点后15位</td>
  </tr>
  <tr align="center">
    <td>字符</td>
    <td><code>char</code></td>
    <td>2</td>
    <td>0~65,535</td>
  </tr>
  <tr align="center">
    <td>布尔</td>
    <td><code>boolean</code></td>
    <td>1</td>
    <td><code>true</code>,<code>false</code></td>
  </tr>
</table>

{{< mermaid >}}graph TB;
    A(数据类型) --> B(基本数据类型)
    A --> C(引用数据类型)
    B --> D(数值型)
    B --> E(非数值型)
    C --> F(类)
    C --> G(接口)
    C --> H(数组)
    D --> I(整数)
    D --> J(浮点数)
    D --> K(字符)
    E --> L(布尔)
{{< /mermaid >}}

### 2.4 变量定义和使用

```java
float a = 3.14F;
// float b = 3.14f;
long c = 100L;
// long d = 100l;
```

### 2.5 读取输入

```java
import java.util.Scanner;

Scanner sc = new Scanner(System.in);
int a = sc.nextInt();
String s1 = sc.nextLine(); // 遇到换行结束，接收换行符，并丢弃。
String s2 = sc.next();     // 遇到空格或换行结束，不接收。
```

### 2.6 标识符定义

必须：

- 由数字、字母、下划线、美元符号组成
- 不能以数字开头
- 不能是关键字
- 区分大小写

建议：

- 小驼峰命名法：`backTraxe`，常用于定义**方法**、**变量**。
- 大驼峰命名法：`BackTraxe`，常用于定义**类**。

### 2.7 类型转换

1. 隐式转换

{{< mermaid >}}graph LR;
    A(byte) --> B(short)
    B --> D(int)
    C(char) --> D
    D --> E(long)
    E --> F(float)
    F --> G(double)
{{< /mermaid >}}

- 占用空间小的数据类型会先转换为占用空间大的数据类型，然后进行运算。
- `byte`、`short`和`char`三种数据类型在运算时会转换为`int`，然后进行运算。

```java
byte a = 1, b = 2;
// 错误
byte c = a + b;
// 正确
int d = a + b;
```

2. 显式（强制）转换

- 有可能产生精度损失。

```java
byte a = 1, b = 2;
byte e = (byte) (a + b);
```

### 2.8 运算符

1. **单目算术运算符**：自增`++`、自减`--`

只能作用于变量。

```java
int a = 1;
int b = a++; // a = 2, b = 1
int c = ++a; // a = 3, c = 3
int d = a--; // a = 2, d = 3
int e = --a; // a = 1, e = 1
```

2. **双目算术运算符**：加`+`、减`-`、乘`*`、除`/`、取余`%`

- 加`+`：出现字符串则为连接运算符，否则为算术运算。从左到右执行。

```java
System.out.println(1 + 2);              // 3
System.out.println(2 + 1);              // 3
System.out.println(1 + 'a');            // 98
System.out.println('a' + 2);            // 99
System.out.println(1 + "abc");          // 1abc
System.out.println("abc" + 2);          // abc2
System.out.println(true + "abc");       // trueabc
System.out.println("abc" + true);       // abctrue
System.out.println("abc" + 1 + 2);      // abc12
System.out.println(1 + 2 + "abc");      // 3abc
System.out.println(1 + "" + 2 + "abc"); // 12abc
```

- 除`/`：整数除整数得整数。

```java
System.out.println(10 / 3);   // 3
System.out.println(10 / 3.0); // 3.3333333333333335
System.out.println(10.0 / 3); // 3.3333333333333335
```

3. **赋值运算符**：赋值`=`、加后赋值`+=`、减后赋值`-=`、乘后赋值`*=`、除后赋值`/=`、取余后赋值`%=`

`a 操作符= b`等价于`a = (a的类型) (a 操作符 (b))`，隐含了强制类型转换，从右往左运算。

```java
int a = 10;
a /= 2 + 3;  // 2 等价于 a = a / (2 + 3)

short b = 1;
b = b + 1;   // 报错
b += 1;      // 2 等价于 b = (short) (b + 1)
```

4. **比较运算符**：相等`==`、不等`!=`、大于`>`、大于等于`>=`、小于`<`、小于等于`<=`

```java
// 基本数据类型比较值
int a = 1, b = 1;
System.out.println(a == b);        // true
// 引用数据类型比较地址
String s1 = "abc";
String s2 = new String(new char[]{'a', 'b', 'c'});
System.out.println(s1 == s2);      // false
System.out.println(s1.equals(s2)); // true
```

5. **逻辑运算符**：与`&,&&`、或`|,||`、非`!`、异或`^`

```java
int a = 1, b = 1;

// 非短路与 &，每个条件都进行判断
a > 2 & b++ > 0;   // false, a = 1, b = 2
// 短路与 &&，当某个条件为 false 时，后面不再进行判断
a > 2 && b++ > 0;  // false, a = 1, b = 1

// 非短路或 |，每个条件都进行判断
a > 0 | b++ > 0;   // true, a = 1, b = 2
// 短路或 ||，当某个条件为 true 时，后面不再进行判断
a > 0 || b++ > 0;  // true, a = 1, b = 1
```

6. **位运算符**：与`&`、或`|`、非`~`、异或`^`、左移`<<`、右移`>>`、无符号右移`>>>`

```java
a ^ a  == 0
a ^ 0  == a
a << 1 == a * 2
```

7. **三目运算符**：`a ? b : c`

```java
// 等价于
if (a) {
    b;
} else {
    c;
}
```

### 2.9 分支语句

```java
if (A) {
    B;
}

if (A) {
    B;
} else {
    C;
}

if (A) {
    B;
} else if (C) {
    D;
} else {
    E;
}
```

```java
// case 表达式不能重复，且不能为变量。
String rank = "First";
switch (rank) {
    case "First":
        System.out.println("第一");
        break;
    case "Second":
    case "Third":
        System.out.println("前三");
        break;
    default:
        System.out.println("再接再厉");
        break;
}
```

### 2.10 循环语句

```java
for (初始化语句; 继续循环条件判断语句; 每轮循环后执行语句) {
    循环体;
}
```

```java
while (继续循环条件判断语句) {
    循环体;
}
```

```java
// 至少执行一次
do {
    循环体;
} while (继续循环条件判断语句);
```

**跳转控制语句**

- `continue`：中断本次循环，直接开始下一次循环。
- `break`：退出循环。

```java
// 中断多重循环
outer:
for (int i = 0; i < 10; i++) {
    for (int j = 0; j < 10; j++) {
        if (i == 5 && j == 7) {
            System.out.println(i + "\n" + j);
            break outer;
        }
    }
}
```

### 2.11 格式化输出

```java
System.out.print();
System.out.printf();
System.out.println();
```

### 2.12 随机数生成

```java
import java.util.Random;

Random r = new Random();
int a = r.nextInt(10); // [0, 10)
```

### 2.13 数组

```java
// 动态初始化。不指定元素，指定长度。默认初始化为 0
int[] arr1 = new int[3];
int[][] arr2 = new int[2][3];
// 静态初始化。指定元素，不指定长度。
int[] arr3 = new int[]{1, 2, 3};
int[][] arr4 = new int[][]{{1, 2}, {3, 4}};
int[] arr5 = {1, 2, 3};
int[][] arr6 = {{1, 2}, {3, 4}};

// 长度
arr1.length;    // 3
arr2[0].length; // 3
```

### 2.14 方法

- 方法不能嵌套定义
- 方法重载（overload）：方法名相同，参数数量或者类型不同。**与返回值类型无关**。

```java
// 返回匿名数组
return new int[]{1, 2};
```

### 2.15 进制

- 二进制：以`0b, 0B`开头
- 八进制：以`0`开头
- 十六进制：以`0x, 0X`开头

```java
System.out.println(10);   // 10
System.out.println(0b10); // 2
System.out.println(010);  // 8
System.out.println(0x10); // 16
```

```java
// 10 进制转 x 进制
public static String ten2x(int num, int x) {
    StringBuilder str = new StringBuilder();
    while (0 != num) {
        str.append((char) (num % x + '0'));
        num /= x;
    }
    str.reverse();
    return str.toString();
}
```

```java
// x 进制转 10 进制
public static int x2ten(String str, int x) {
    int res = 0;
    for (int i = 0; i < str.length(); i++) {
        res = res * x + str.charAt(i) - '0';
    }
    return res;
}
```

### 2.16 字符串

**不可修改**，当字符串拼接时，系统自动转为`StringBuilder`进行拼接，然后转为字符串。

```java
String s = "abc";
s.length();  // 3
```

#### 2.16.1 字符串构造

```java
String s1 = "abc";
String s2 = new String(new char[]{'a', 'b', 'c'});
String s3 = new String("abc");
```

#### 2.16.2 字符串比较

```java
String s1 = "abc";
String s2 = new String("abc");
String s3 = "Abc";
// 比较地址
System.out.println(s1 == s2);                // false
// 比较值
System.out.println(s1.equals(s2));           // true
// 不区分大小写
System.out.println(s1.equalsIgnoreCase(s3)); // true
```

#### 2.16.3 字符串遍历

```java
String s = "abc";
// 1
for (int i = 0; i < s.length(); i++) {
    s.charAt(i);
}
// 2
char[] chars = s.toCharArray();
for (int i = 0; i < chars.length; i++) {
    chars[i];
}
```

#### 2.16.4 子串

- `String substring(int beginIndex)`
- `String substring(int beginIndex, int endIndex)`: [beginIndex, endIndex)
- `Char[] subSequence(int beginIndex, int endIndex)`: [beginIndex, endIndex)

```java
String s = "abcde";
s.substring(1);    // bcde
s.substring(2, 4); // cd
```

#### 2.16.5 其他操作

格式化

- `static String format(String format)`
- `String strip()`
- `String stripLeading()`
- `String stripTrailing()`
- `String toUpperCase()`
- `String toLowerCase()`

查找

- `boolean startsWith(String prefix)`
- `boolean endsWith(String suffix)`
- `boolean matches(String regex)`
- `int indexOf(int ch)`
- `int indexOf(int ch, int fromIndex)`
- `int indexOf(String str)`
- `int indexOf(String str, int fromIndex)`
- `int lastIndexOf(int ch)`
- `int lastIndexOf(int ch, int fromIndex)`
- `int lastIndexOf(String str)`
- `int lastIndexOf(String str, int fromIndex)`

替换

- `String replace(char oldChar, char newChar)`
- `String replace(CharSequence target, CharSequence replacement)`
- `String replaceAll(String regex, String replacement)`
- `String replaceFirst(String regex, String replacement)`

拆分/合并

- `String[] split(String regex)`
- `static String join(CharSequence delimiter, CharSequence... elements)`
- `static String join(CharSequence delimiter, Iterable<? extends CharSequence> elements)`

```java
String s = "Java-is-cool";
s.split("-");                                    // {"Java", "is", "cool"}
String.join("-", "Java", "is", "cool");          // Java-is-cool
String.join("-", List.of("Java", "is", "cool")); // Java-is-cool
```

#### 2.16.6 字符串常量池

- 当使用双引号创建字符串对象的时候，系统会在常量池中检查是否已存在该字符串，若不存在，则创建，若存在，则直接使用。
- 当系统发现字符串拼接时，会自动创建`StringBuilder`对象完成字符串拼接，然后转为`String`。

```java
String a = "abc";
String b = new String("abc");
String c = "abc";
String d = "ab" + "c";
String e = "a" + "b" + "c";
String f = new String("ab") + "c";
System.out.println(a == b); // false 两个对象
System.out.println(a == c); // true  常量池
System.out.println(a == d); // true  编译期间的常量优化机制
System.out.println(a == e); // true  编译期间的常量优化机制
System.out.println(a == f); // false 两个对象
System.out.println(b == f); // false 两个对象
```

### 2.17 可变字符串

#### 2.17.1 StringBuilder

插入

- `StringBuilder append(X x)`
- `StringBuilder insert(int offset, X x)`

修改

- `void	setCharAt(int index, char ch)`
- `StringBuilder replace(int start, int end, String str)`
- `StringBuilder delete(int start, int end)`
- `StringBuilder deleteCharAt(int index)`
- `StringBuilder reverse()`

索引查找

- `char	charAt(int index)`

值查找

- `int indexOf(String str)`
- `int indexOf(String str, int fromIndex)`
- `int lastIndexOf(String str)`
- `int lastIndexOf(String str, int fromIndex)`

比较

- `int compareTo(StringBuilder another)`

子串

- `CharSequence	subSequence(int start, int end)`
- `String substring(int start)`
- `String substring(int start, int end)`

转换

- `StringBuilder(String str)`
- `String toString()`

### 2.18 复杂度分析

程序耗时

```java
long start = System.currentTimeMillis();
// 执行代码
long end = System.currentTimeMillis();
System.out.println(end - start);
```

### 2.19 static

静态修饰符，可修饰变量和方法。

    - 静态变量
        1. 被所有实例化的对象共享。
        2. 随类的加载而加载。
        3. 不需要创建对象即可调用（使用类名）。
    - 静态方法
        1. 只能访问静态变量或静态方法。
        2. 不能使用this。
        3. 不需要创建对象即可调用（使用类名）。

```java
public class Student {
    public static int age = 18;
    public static int getAge() {
        return age;
    }
}
Student.age;      // 18
Student.getAge(); // 18
Student stu1 = new Student();
stu1.age;         // 18
stu1.age = 20;
Student stu2 = new Student();
stu2.age;         // 20
Student.age;      // 20
```

## 3 面向对象编程

面向对象编程（Object Oriented Programming，OOP），是一种程序设计思想，把类作为程序的基本单元，一个类包含了变量和方法。

可以提高代码的**维护性、可读性、复用性**。

### 3.1 为什么要面向对象

#### 3.1.1 分类思想

例如：学生信息管理系统。

- Student类：标准学生类，封装学生信息（学号、姓名、性别、年级等）。
- StudentDao类：DAO，Data Access Object，访问存储数据的数组或集合。
- StudentService类：业务逻辑处理。例如添加学生、查询学生。
- StudentController类：用户交互相关。例如处理用户输入、给予用户反馈信息。

#### 3.1.2 分包思想

- 本质文件夹。
- 多级包使用`.`分割，一般用逆序网址（去掉www），如 io.github.backtraxe。
- 全小写字母。
- 必须在文件开头（注释不算）。
- 不同包下类的访问：1.先导包。2.包名+类名（重名类使用）。

```java
package io.github.backtraxe;
// 1. 导包
import io.github.backsided.Student;
// 2. 包名+类名
io.github.backsided.Student stu = new io.github.backsided.Student();
```

### 怎样面向对象

### 3.1 类

执行顺序：

1. 静态代码块
2. 构造代码块
3. 构造方法

#### 3.1.1 构造方法

1. 名称与类名相同。
2. 无返回值。
3. 实现类时自动调用。
4. 若无自定义构造方法，则系统提供空构造方法。
5. 若自定义构造方法，则系统不再提供。

```java
public class Circle {
    public Circle() {
        // 系统默认提供的构造函数
    }
}
```

#### 3.1.2 构造代码块

```java
public class Circle {
    {
        // 每次创建对象时在构造方法之前执行。
    }
}
```

#### 3.1.3 静态代码块

```java
public class Circle {
    static {
        // 类加载时执行一次。
    }
}
```

#### 3.1.4 内部类

- 内部类可以使用外部类中所有成员和方法（包括私有）。

```java
class Outer {
    class Inner {
        // 成员内部类
    }
}

Outer.Inner oi = new Outer().new Inner();
```

```java
class Outer {
    static class Inner {
        // 静态内部类
    }
}

Outer.Inner oi = new Outer.Inner();
```

```java
class Outer {
    void method() {
        class Inner {
            // 局部内部类，外界无法访问。
        }
    }
}
```

```java
class Outer {
    Inner inner = new Inner() {
        // 匿名内部类
        @Override
        public void method() {}
    };

    inner.method();
}
```

### 3.2 封装

隐藏实现细节，仅对外暴露公共的访问方式。可以提高代码的安全性和复用性。

针对`private`修饰的成员变量，如果需要被其他类使用，需要提供`getXxx()`和`setXxx()`方法。Idea 可自动生成。

```java
public class Student {
    private String name; // 类外无法访问
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public static void main(String[] args) {
        Student stu = new Student();
        stu.setName("张三");
        System.out.println(stu.getName());
    }
}
```

### 3.3 继承

### 3.4 多态

- 子类继承父类，或者子类实现接口。
- 子类进行方法重写。
- 父类引用指向子类对象，调用父类中定义的方法，子类完成动作。


### 3.5 接口

- 无构造方法。
- 所有变量都是常量`public static final`。
- 所有方法都是抽象方法`public abstract`(JDK 7)。
    - `public default`方法，可以有方法体（JDK 8）。若子类实现的多个接口存在相同的`default`方法，则子类必须重写该方法。
    - `public static`方法，可以有方法体（JDK 8）。该方法只能通过接口名调用，不能通过子类调用。
    - `private`方法，可以有方法体（JDK 9）。仅允许接口内使用。
- 子类`implements`接口，实现接口中所有抽象方法。
- 子类可实现多接口。

```java
public interface Shape {
    double area();
    double perimeter();
}
```

## 字符串

- String 转 byte 数组

```java
// 使用平台的默认字符集
byte[] getBytes()
// 指定字符集。UTF-8（中文3字节）、GBK（中文2字节）等
byte[] getBytes​(String charsetName)
byte[] getBytes​(Charset charset)
```

- byte 数组转 String

```java
String​(byte[] bytes)
String​(byte[] bytes, String charsetName)
String​(byte[] bytes, Charset charset)
String​(byte[] bytes, int offset, int length)
String​(byte[] bytes, int offset, int length, String charsetName)
String​(byte[] bytes, int offset, int length, Charset charset)
```

## 常用库

### Math

```java
static double E
static double PI

static T abs​(T a)
static T max​(T a, T b)
static T min​(T a, T b)

static double ceil​(double a)  // 向上取整
static double floor​(double a) // 向下取整

static double exp​(double a)
static double pow​(double a, double b)

static double log​(double a)
static double log10​(double a)

static double sin​(double a)
static double cos​(double a)
static double tan​(double a)

static double random() // [0.0, 1.0)
```

### System

```java
static PrintStream err
static InputStream in
static PrintStream out

// System.setIn​(new InputStream("input"));
static void setIn​(InputStream in)
// System.setOut(new PrintStream("output"));
static void setOut​(PrintStream out)

// System.exit(0);
static void	exit​(int status)
static void	gc()

static long currentTimeMillis() // 毫秒

static void arraycopy​(Object src, int srcPos, Object dest, int destPos, int length)
```

### Object

```java
protected Object clone()

boolean equals​(Object obj) {
    return (this == obj);
}

// Deprecated
protected void	finalize()

Class<?> getClass()

int hashCode()

void notify()
void notifyAll()

String toString() {
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}

void wait()
void wait​(long timeoutMillis)
void wait​(long timeoutMillis, int nanos)
```

### Class<T>

```java
String getName()

boolean isArray()
boolean	isInstance​(Object obj)
boolean	isInterface()

Constructor<T> getDeclaredConstructor​(Class<?>... parameterTypes)
Constructor<?>[] getDeclaredConstructors()

Field getDeclaredField​(String name)
Field[] getDeclaredFields()

Method getDeclaredMethod​(String name, Class<?>... parameterTypes)
Method[] getDeclaredMethods()

T newInstance()
```

### BigDecimal

```java
import java.math.BigDecimal;

BigDecimal​(char[] in)
BigDecimal​(char[] in, int offset, int len)
BigDecimal​(double val)
BigDecimal​(int val)
BigDecimal​(long val)
BigDecimal​(String val) // 推荐
BigDecimal​(BigInteger val)

BigDecimal abs()

BigDecimal max​(BigDecimal val)
BigDecimal min​(BigDecimal val)
BigDecimal pow​(int n)

int	compareTo​(BigDecimal val)
boolean	equals​(Object x)

BigDecimal add​(BigDecimal augend)
BigDecimal subtract​(BigDecimal subtrahend)
BigDecimal multiply​(BigDecimal multiplicand)
BigDecimal divide​(BigDecimal divisor)
// RoundingMode.UP      进一法
// RoundingMode.DOWN    去尾法
// RoundingMode.HALF_UP 四舍五入
BigDecimal divide​(BigDecimal divisor, int scale, RoundingMode roundingMode)
BigDecimal remainder​(BigDecimal divisor)
BigDecimal[] divideAndRemainder​(BigDecimal divisor)

int	intValue()
long longValue()
double doubleValue()
BigInteger toBigInteger()
String toString()

static BigDecimal valueOf​(double val)
static BigDecimal valueOf​(long val)
```

### Integer

```java
static int BYTES           // 4
static int MAX_VALUE       // 0x7fffffff 2147483647
static int MIN_VALUE       // 0x80000000 -2147483648
static int SIZE            // 32
static Class<Integer> TYPE // int

static int compare​(int x, int y)
static int compareUnsigned​(int x, int y)
int	compareTo​(Integer anotherInteger)

static int bitCount​(int i)      // 返回1的数量
static int highestOneBit​(int i) // 保留最高位1
static int lowestOneBit​(int i)  // 保留最低位1

static int numberOfLeadingZeros​(int i)  // 前缀0数量
static int numberOfTrailingZeros​(int i) // 后缀0数量

static int reverse​(int i)
static int reverseBytes​(int i)

static int parseInt​(CharSequence s, int beginIndex, int endIndex, int radix)
static int parseInt​(String s)
static int parseInt​(String s, int radix)

static String toString​(int i)
static String toString​(int i, int radix)
```

### Date & SimpleDateFormat

```java
import java.util.Date;
import java.text.SimpleDateFormat;

// Date 类中的方法
Date()
Date​(long date)
Date​(String s)

long getTime()
void setTime​(long time)

int	compareTo​(Date anotherDate)

// Date 转 String
Date date1 = new Date();
SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 E HH:mm:ss");
sdf.format(date1); // 2022年05月27日 周五 13:57:41

// String 转 Date
String str = "2022年05月27日 周五 13:57:41";
Date date2 = sdf.parse(str);
```

### Calendar

```java
import java.util.Calendar;

Date getTime()
void setTime​(Date date)

long getTimeInMillis()
void setTimeInMillis​(long millis)

void add​(int field, int amount)
int get​(int field)
void set​(int field, int value)

void set​(int year, int month, int date)
void set​(int year, int month, int date, int hourOfDay, int minute)
void set​(int year, int month, int date, int hourOfDay, int minute, int second)

void setFirstDayOfWeek​(int value)

Calendar calendar = Calendar.getInstance();
calendar.get(Calendar.YEAR);                  // 年
calendar.get(Calendar.MONTH) + 1;             // 月
calendar.get(Calendar.DAY_OF_MONTH);          // 日
(calendar.get(Calendar.DAY_OF_WEEK) - 1) % 7; // 周
calendar.get(Calendar.HOUR_OF_DAY);           // 时
calendar.get(Calendar.MINUTE);                // 分
calendar.get(Calendar.SECOND);                // 秒
```

### DateTimeFormatter & LocalDateTime

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy年MM月dd日 E HH:mm:ss");
LocalDateTime time = LocalDateTime.parse(str, formatter);

// LocalDateTime 类中的方法
String format​(DateTimeFormatter formatter)
int getYear()
int getMonthValue()      // [1, 12]
Month getMonth()         // May
int getDayOfMonth()      // [1, 31]
int getDayOfYear()       // [1, 366]
DayOfWeek getDayOfWeek() // FRIDAY
int getHour()            // [0, 23]
int getMinute()
int getSecond()
int getNano()

static LocalDateTime of​(int year, int month, int dayOfMonth, int hour, int minute)
static LocalDateTime of​(int year, int month, int dayOfMonth, int hour, int minute, int second)

static LocalDateTime parse​(CharSequence text)
static LocalDateTime parse​(CharSequence text, DateTimeFormatter formatter)

LocalDateTime plusYears​(long years)
LocalDateTime plusMonths​(long months)
LocalDateTime plusDays​(long days)
LocalDateTime plusHours​(long hours)
LocalDateTime plusMinutes​(long minutes)
LocalDateTime plusSeconds​(long seconds)
LocalDateTime plusWeeks​(long weeks)

LocalDateTime minusYears​(long years)
LocalDateTime minusMonths​(long months)
LocalDateTime minusDays​(long days)
LocalDateTime minusHours​(long hours)
LocalDateTime minusMinutes​(long minutes)
LocalDateTime minusSeconds​(long seconds)
LocalDateTime minusWeeks​(long weeks)

LocalDateTime withYear​(int year)
LocalDateTime withMonth​(int month)
LocalDateTime withDayOfMonth​(int dayOfMonth)
LocalDateTime withHour​(int hour)
LocalDateTime withMinute​(int minute)
LocalDateTime withSecond​(int second)
LocalDateTime withDayOfYear​(int dayOfYear)

LocalDate toLocalDate()
LocalTime toLocalTime()
```

### Period &

```java
LocalDate startDate = LocalDate.of(2022, 2, 1);
LocalDate endDate = LocalDate.of(2025, 11, 18);
Period period = Period.between(startDate, endDate);
period.getYears();
period.getMonths();
period.getDays();

LocalDateTime startTime = LocalDateTime.of(2022, 2, 4, 5, 18, 23);
LocalDateTime endTime = LocalDateTime.of(2030, 1, 30, 17, 24, 12);
Duration duration = Duration.between(startTime, endTime);
duration.toSeconds();
```

## 文件

### 构造方法

```java
File​(File parent, String child)
File​(String pathname)
File​(String parent, String child)
```

### 判断路径

```java
// 是否目录
boolean isDirectory()
// 是否文件
boolean isFile()
// 是否存在
boolean exists()
// 是否绝对路径
boolean isAbsolute()
```

### 获取路径

```java
// 文件名加后缀
String getName()
// 父目录
String getParent()
new File("demo").getParent();        // null
new File("folder/demo").getParent(); // folder
// 返回传入的路径
String getPath()
// 绝对路径
String getAbsolutePath()
// 获取路径下所有路径（包括隐藏）
String[] list()    // 只返回文件名
File[] listFiles() // 文件名加路径（非绝对路径）
```

### 创建文件或文件夹

```java
// 创建单个文件
boolean createNewFile() throws IOException
// 创建单个文件夹
boolean mkdir()
// 递归创建文件夹
boolean mkdirs()
```

### 删除文件或文件夹

```java
// 删除空目录或文件
boolean delete()
```

## I/O

### 分类

**流向**

- 输入流
- 输出流

**类型**

- 字节流：所有文件。
- 字符流：文本文件。

### 文件字节流

#### FileInputStream​

- 构造方法

```java
FileInputStream​(String name) throws FileNotFoundException
FileInputStream​(File file) throws FileNotFoundException
```

- 读

```java
int read() throws IOException // 读1个字节
int read​(byte[] b) throws IOException
int read​(byte[] b, int off, int len) throws IOException
// 读取整个文件
while ((b = fis.read()) != -1) {
    read();
}

long skip​(long n) throws IOException
```

- 关闭

```java
void close() throws IOException
```

#### FileOutputStream​

- 构造方法

```java
FileOutputStream​(String name) throws FileNotFoundException
FileOutputStream​(String name, boolean append) throws FileNotFoundException
FileOutputStream​(File file) throws FileNotFoundException
FileOutputStream​(File file, boolean append) throws FileNotFoundException
```

- 写

```java
void write​(int b) throws IOException // 写入对应字符
void write​(byte[] b) throws IOException
void write​(byte[] b, int off, int len) throws IOException
// 写入换行
fos.write("\r\n".getBytes()); // windows
```

- 关闭

```java
void close() throws IOException
```

#### 实战

- 文件复制

```java
try (FileInputStream fis = new FileInputStream("input");
     FileOutputStream fos = new FileOutputStream("output");) {
    byte[] buffer = new byte[1024];
    int len;
    while ((len = fis.read(buffer)) != -1) {
        fos.write(buffer, 0, len);
    }
}
```

### 缓冲字节流

#### BufferedInputStream

- 构造方法

```java
// DEFAULT_BUFFER_SIZE = 8192
BufferedInputStream​(InputStream in)
BufferedInputStream​(InputStream in, int size)
```

- 读

```java
int read() throws IOException // 读1个字节
int read​(byte[] b) throws IOException
int read​(byte[] b, int off, int len) throws IOException
// 读取整个文件
while ((b = fis.read()) != -1) {
    read();
}

void reset() throws IOException
long skip​(long n) throws IOException
```

- 关闭

```java
void close() throws IOException
```

#### BufferedOutputStream

- 构造方法

```java
// DEFAULT_BUFFER_SIZE = 8192
BufferedOutputStream(OutputStream out)
BufferedOutputStream(OutputStream out, int size)
```

- 写

```java
void write​(int b) throws IOException // 写入对应字符
void write​(byte[] b) throws IOException
void write​(byte[] b, int off, int len) throws IOException
// 写入换行
fos.write("\r\n".getBytes()); // windows

void flush() throws IOException
```

- 关闭

```java
void close() throws IOException
```

### 对象操作流

#### ObjectInputStream

```java
ObjectInputStream​(InputStream in)
Object readObject()
```

#### ObjectOutputStream

- 需要对象实现`java.io.Serializable`接口
- `private static final long serialVersionUID = 1L;`

```java
ObjectOutputStream​(OutputStream out)
void writeObject​(Object obj)
```

### 字符流

#### Reader

```java
int	read()
int	read​(char[] cbuf)
int read​(char[] cbuf, int off, int len)
int	read​(CharBuffer target)

void mark​(int readAheadLimit)
boolean markSupported()

long transferTo​(Writer out)

long skip​(long n)
void reset()

void close()
```

#### Writer

```java
Writer append​(char c)
Writer append​(CharSequence csq)
Writer append​(CharSequence csq, int start, int end)

void write​(int c)
void write​(char[] cbuf)
void write​(char[] cbuf, int off, int len)
void write​(String str)
void write​(String str, int off, int len)

void flush()

void close()
```

### 文件字符流

#### FileReader

- 构造方法

```java
FileReader​(String fileName)
FileReader​(String fileName, Charset charset)
FileReader​(File file)
FileReader​(File file, Charset charset)
```

#### FileWriter

- 构造方法

```java
FileWriter​(String fileName)
FileWriter​(String fileName, boolean append)
FileWriter​(String fileName, Charset charset)
FileWriter​(String fileName, Charset charset, boolean append)
FileWriter​(File file)
FileWriter​(File file, boolean append)
FileWriter​(File file, Charset charset)
FileWriter​(File file, Charset charset, boolean append)
```

### 缓冲字符流

#### BufferedReader

```java
BufferedReader​(Reader in)
BufferedReader​(Reader in, int sz)

int read()
int read​(char[] cbuf, int off, int len)
String readLine()

Stream<String> lines()

void reset()
long skip​(long n)

void close()
```

#### BufferedWriter

```java
BufferedWriter​(Writer out)
BufferedWriter​(Writer out, int sz)

void write​(int c)
void write​(char[] cbuf)
void write​(char[] cbuf, int off, int len)
void write​(String s)
void write​(String s, int off, int len)
void newLine()

void flush()

void close()
```

#### 实战

- 文件复制

```java
try (BufferedReader br = new BufferedReader(new FileReader("input"));
     BufferedWriter bw = new BufferedWriter(new FileWriter("output"));) {
    String line;
    while ((line = br.readLine()) != null) {
        bw.write(line);
    }
}
```

### 转换流

#### InputStreamReader

字节流 -> 字符流

```java
InputStreamReader​(InputStream in)
InputStreamReader​(InputStream in, String charsetName)
InputStreamReader​(InputStream in, Charset cs)
InputStreamReader​(InputStream in, CharsetDecoder dec)

String	getEncoding()
int	read()
int	read​(char[] cbuf, int offset, int length)
```

#### OutputStreamWriter

字符流 -> 字节流

```java
OutputStreamWriter​(OutputStream out)
OutputStreamWriter​(OutputStream out, String charsetName)
OutputStreamWriter​(OutputStream out, Charset cs)
OutputStreamWriter​(OutputStream out, CharsetEncoder enc)

String getEncoding()

void write​(int c)
void write​(char[] cbuf)
void write​(char[] cbuf, int off, int len)
void write​(String str)
void write​(String str, int off, int len)

void flush()
void close()
```

## Properties

`key`和`value`都是`String`的`Hashtable`

- 构造方法

```java
Properties()
Properties​(int initialCapacity)
Properties​(Properties defaults)
```

- 添加/修改

```java
Object setProperty​(String key, String value)
Object put(String key, String value)
```

- 删除

```java
V remove(Object key)
```

- 查询

```java
Set<String>	stringPropertyNames()
Set<Object> keySet()

Set<Map.Entry<Object, Object>> entrySet()

boolean containsKey(Object key)

String getProperty​(String key)
String getProperty​(String key, String defaultValue)

Object get(Object key)
Object getOrDefault(Object key, Object defaultValue)
```

- 序列化

```java
void load​(InputStream inStream)
void load​(Reader reader)
void loadFromXML​(InputStream in)

void store​(OutputStream out, String comments)
void store​(Writer writer, String comments)
void storeToXML​(OutputStream os, String comment)
void storeToXML​(OutputStream os, String comment, String encoding)
void storeToXML​(OutputStream os, String comment, Charset charset)
```

```properties
# properties
key1=value1
key2=value2
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
    <entry key="key1">value1</entry>
    <entry key="key2">value2</entry>
</properties>
```

## Lambda 表达式

- 方法传入参数为接口。
- 接口有且只有一个抽象方法。
- 不产生`.class`文件。

```java
// 无参数
() -> {
    // return
}

// 一个参数
a -> {
    // return
}

// 多个参数
(a, b) -> {
    // return
}
```

## 异常

### 分类

- java.lang.Throwable
    - java.lang.Error：严重错误
    - java.lang.Exception
        - java.lang.RuntimeException：运行时异常
        - 编译时异常：编译器能够检查出的异常，需要进行异常处理

### 异常处理

```java
try {

} catch (Exception e) {

} finally {
    // 一定会执行，就算没有出现异常
    // 即使 try 程序块中有 return 语句，也是在执行了 finally 语句块后再返回
}
```

```java
// 不处理，向上继续抛出
void func() throws Exception {}
```

```java
void func() {
    // 主动抛出异常
    throw new Exception();
}
```

### 自定义异常

```java
class DIYException extends Exception {
    public DIYException() {
        super();
    }

    public DIYException(String message) {
        super(message);
    }
}
```

