# Java SE 教程


Java 是全球使用最广泛的编程语言。

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

## 3 面向对象编程

面向对象编程（Object Oriented Programming，OOP），是一种程序设计思想，把类作为程序的基本单元，一个类包含了变量和方法。

```java
public class Circle {
    final static double PI = 3.14159;
    double radius;
    Circle(double radius) {
        this.radius = radius;
    }
    double getPerimeter() {
        return 2 * PI * radius;
    }
    double getArea() {
        return PI * radius * radius;
    }
    public static void main(String[] args) {
        Circle circle = new Circle(2);
        System.out.println("半径为：" + circle.radius);
        System.out.println("周长为：" + circle.getPerimeter());
        System.out.println("面积为：" + circle.getArea());
    }
}
```

### 3.1 类

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

## 4 集合

可变容量的容器。

### 4.1 ArrayList

- 可变数组。
- **不能使用基本数据类型**。
- 打印则直接遍历打印数组值，而不是打印地址。

构造函数

- `ArrayList()`
- `ArrayList(int initialCapacity)`
- `ArrayList(Collection<? extends E> c)`

```java
// 不指定类型，可添加任意类型
ArrayList l1 = new ArrayList();
// 指定类型
ArrayList<Integer> l2 = new ArrayList<>();
```

增

- `void	add(int index, E element)`
- `boolean add(E e)`
- `boolean addAll(int index, Collection<? extends E> c)`
- `boolean addAll(Collection<? extends E> c)`

删

- `E remove(int index)`
- `boolean remove(Object o)`：删除第一个出现的
- `boolean removeAll(Collection<?> c)`
- `void	clear()`

```java
for (int i = 0; i < list.size(); i++) {
    if (list.get(i) == target) {
        list.remove(i);
        i--; // 集合删除元素后，后面元素整体前移一位。
    }
}
```

改

- `E set(int index, E element)`

查

- `E get(int index)`
- `boolean contains(Object o)`

大小

- `int size()`
- `boolean isEmpty()`

#### 4.1.1 遍历

```java
// IDEA 快捷键：list.fori
for (int i = 0; i < list.size(); i++) {
    list.get(i);
}
// IDEA 快捷键：list.forr
for (int i = list.size() - 1; i >= 0; i--) {
    list.get(i);
}
```

## 垃圾回收

垃圾回收，顾名思义就是释放垃圾占用的空间，从而提升程序性能，防止内存泄露。当一个对象不再被需要时，该对象就需要被回收并释放空间。

Java 内存运行时数据区域包括程序计数器、虚拟机栈、本地方法栈、堆等区域。其中，程序计数器、虚拟机栈和本地方法栈都是线程私有的，当线程结束时，这些区域的生命周期也结束了，因此不需要过多考虑回收的问题。而堆是虚拟机管理的内存中最大的一块，堆中的内存的分配和回收是动态的，垃圾回收主要关注的是堆空间。

**调用垃圾回收器的方法**

调用垃圾回收器的方法是`gc`，该方法在`System`类和`Runtime`类中都存在。在`Runtime`类中，方法`gc`是实例方法，方法`System.gc`是调用该方法的一种传统而便捷的方法。在`System`类中，方法`gc`是静态方法，该方法会调用`Runtime`类中的`gc`方法。其实，`java.lang.System.gc`等价于`java.lang.Runtime.getRuntime.gc`的简写，都是调用垃圾回收器。方法`gc`的作用是提示`Java`虚拟机进行垃圾回收，该方法由系统自动调用，不需要人为调用。该方法被调用之后，由`Java`虚拟机决定是立即回收还是延迟回收。

**finalize 方法**

与垃圾回收有关的另一个方法是`finalize`方法。该方法在`Object`类中被定义，在释放对象占用的内存之前会调用该方法。该方法的默认实现不做任何事，如果必要，子类应该重写该方法，一般建议在该方法中释放对象持有的资源。

**判断对象是否可回收**

垃圾回收器在对堆进行回收之前，首先需要确定哪些对象是可回收的。常用的算法有两种，引用计数算法和根搜索算法。

**1.引用计数算法**

引用计数算法给每个对象添加引用计数器，用于记录对象被引用的计数，引用计数为 0 的对象即为可回收的对象。

虽然引用计数算法的实现简单，判定效率也很高，但是引用计数算法无法解决对象之间循环引用的情况。如果多个对象之间存在循环引用，则这些对象的引用计数永远不为 0，无法被回收。因此 Java 语言没有使用引用计数算法。

**2.根搜索算法**

主流的商用程序语言都是使用根搜索算法判断对象是否可回收。根搜索算法的思路是，从若干被称为 GC Roots 的对象开始进行搜索，不能到达的对象即为可回收的对象。

在 Java 中，GC Roots 一般包含下面几种对象：

- 虚拟机栈中引用的对象
- 本地方法栈中的本地方法引用的对象
- 方法区中的类静态属性引用的对象
- 方法区中的常量引用的对象

**3.引用的分类**

引用计数算法和根搜索算法都需要通过判断引用的方式判断对象是否可回收。

在 JDK 1.2 之后，Java 将引用分成四种，按照引用强度从高到低的顺序依次是：

1. **强引用**：在程序代码中普遍存在的引用。垃圾回收器永远不会回收被强引用关联的对象。
2. **软引用**：还有用但并非必需的对象。只有在系统将要发生内存溢出异常时，被软引用关联的对象才会被回收。在 JDK 1.2 之后，提供了`SoftReference`类实现软引用。
3. **弱引用**：非必需的对象，其强度低于软引用。被弱引用关联的对象只能存活到下一次垃圾回收发生之前，当垃圾回收器工作时，被弱引用关联的对象一定会被回收。在 JDK 1.2 之后，提供了`WeakReference`类实现弱引用。
4. **虚引用**：最弱的引用关系。一个对象是否有虚引用的存在，完全不会对其生存时间构成影响，也无法通过虚引用取得一个对象实例。为一个对象设置虚引用关联的唯一目的就是能在这个对象被回收时收到一个系统通知。在 JDK 1.2 之后，提供了`PhantomReference`类实现虚引用。

**垃圾回收算法**

**1.标记—清除算法**

标记—清除算法是最基础的垃圾回收算法，后续的垃圾收集算法都是基于标记—清除算法进行改进而得到的。标记—清除算法分为“标记”和“清除”两个阶段，首先标记出所有需要回收的对象，在标记完成后统一回收所有被标记的对象。

标记—清除算法有两个主要缺点。一是效率问题，标记和清除的效率都不高，二是空间问题，标记清除之后会产生大量不连续的内存碎片，导致程序在之后的运行过程中无法为较大对象找到足够的连续内存。

**2.复制算法**

复制算法的将可用内存分成大小相等的两块，每次只使用其中的一块，当用完一块内存时，将还存活着的对象复制到另外一块内存，然后把已使用过的内存空间一次清理掉。

复制算法解决了效率问题。由于每次都是对整个半区进行内存回收，因此在内存分配时不需要考虑内存碎片等复杂情况，只要移动堆顶指针，按顺序分配内存即可。复制算法的优点是实现简单，运行高效，缺点是将内存缩小为了原来的一半，以及在对象存活率较高时复制操作的次数较多，导致效率降低。

**3.标记—整理算法**

标记—整理算法是根据老年代的特点提出的。标记过程与标记—清除算法一样，但后续步骤不是直接回收被标记的对象，而是让所有存活的对象都向一端移动，然后清除边界以外的内存。

**4.分代收集算法**

分代收集算法根据对象的存活周期不同将内存划分为多个区域，对每个区域选用不同的垃圾回收算法。

一般把 Java 堆分为新生代和老年代。在新生代中，大多数对象的生命周期都很短，因此选用复制算法。在老年代中，对象存活率高，因此选用标记—清除算法或标记—整理算法。

**分配内存与回收策略**

Java 堆可以分成新生代和老年代，新生代又可以细分成`Eden`区、`From Survivor`区、`To Survivor`区等。对于不同的对象，有相应的内存分配规则。

**1.Minor GC 和 Full GC**

`Minor GC`指发生在新生代的垃圾回收操作。因为大多数对象的生命周期都很短，因此`Minor GC`会频繁执行，一般回收速度也比较快。

`Full GC`也称`Major GC`，指发生在老年代的垃圾回收操作。出现了`Full GC`，经常会伴随至少依次的`Minor GC`。老年代对象的存活时间长，因此`Full GC`很少执行，而且执行速度会比`Minor GC`慢很多。

**2.对象优先在 Eden 区分配**

大多数情况下，对象在新生代`Eden`区分配，当`Eden`区空间不够时，发起`Minor GC`。

**3.大对象直接进入老年代**

大对象是指需要连续内存空间的对象，最典型的大对象是那种很长的字符串以及数组。大对象对于虚拟机的内存分配而言是坏消息，经常出现大对象会导致内存还有不少空间时就提前触发垃圾回收以获取足够的连续空间分配给大对象。

将大对象直接在老年代中分配的目的是避免在`Eden`区和`Survivor`区之间出现大量内存复制。

**4.长期存活的对象进入老年代**

虚拟机采用分代收集的思想管理内存，因此需要识别每个对象应该放在新生代还是老年代。虚拟机给每个对象定义了年龄计数器，对象在`Eden`区出生之后，如果经过第一次`Minor GC`之后仍然存活，将进入`Survivor`区，同时对象年龄变为 1，对象在`Survivor`区每经过一次`Minor GC`且存活，年龄就增加 1，增加到一定阈值时则进入老年代（阈值默认为 15）。

**5.动态对象年龄判定**

为了能更好地适应不同程序的内存状况，虚拟机并不总是要求对象的年龄必须达到阈值才能进入老年代。如果在`Survivor`区中相同年龄的所有对象的空间总和大于`Survivor`区空间的一半，则年龄大于或等于该年龄的对象直接进入老年代。

**6.空间分配担保**

在发生`Minor GC`之前，虚拟机会先检查老年代最大可用的连续空间是否大于新生代所有对象的空间总和，如果这个条件成立，那么`Minor GC`可以确保是安全的。

只要老年代的连续空间大于新生代对象总大小或者历次晋升的平均大小，就会进行`Minor GC`，否则将进行`Full GC`。

