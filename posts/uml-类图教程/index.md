# UML 类图教程


类图是 UML 中面向对象系统建模中最常用和最重要的图，是定义其它图的基础。类图主要是用来显示系统中的类、接口以及它们之间的静态结构和关系的一种静态模型。

<!--more-->

## 1.类图表示

{{< mermaid >}}classDiagram
    class Person {
        -name : String
        -age : int
        +getName() String
        +setName(name : String) void
        +getAge() int
        +setAge(age : int) void
        +work() void
    }
{{< /mermaid>}}

一个类的 UML 图表示为一个矩形框，分为三层：

- **类名**
    - 粗体居中
    - 若为抽象类，类名和抽象方法用**斜体**表示
    - 若为接口，类名上加`<<interface>>`，一般无属性
- **属性**
    - 可见性 + 属性名 + 类型
- **方法**
    - 可见性 + 方法名 + ( + 参数名 + 参数类型 + ) + 返回类型

可见性：

- `-`表示`private`
- `#`表示`protected`
- 空表示`package/default`
- `+`表示`public`

## 2.关系表示

### 2.1 泛化（Generalization）

{{< mermaid >}}classDiagram
    Person <|-- Student
{{< /mermaid >}}

关系：继承非抽象类

表示：子类指向父类的**实线空心三角箭头**

### 2.2 实现（Realize）

{{< mermaid >}}classDiagram
    class Vehicle
    <<interface>> Vehicle
    Vehicle <|.. Car
{{< /mermaid >}}

关系：继承抽象类

表示：子类指向父类的**虚线空心三角箭头**

### 2.3 聚合（Aggregation）

{{< mermaid >}}classDiagram
    Car o-- Wheel
{{< /mermaid >}}

关系：成员对象是整体对象的属性，部分可独立存在，且可属于多个整体

表示：部分指向整体的**实线空心菱形箭头**

### 2.4 组合（Composition）

{{< mermaid >}}classDiagram
    Face *-- Eye
{{< /mermaid >}}

关系：成员对象是整体对象的属性，整体与部分密不可分

表示：部分指向整体的**实线实心菱形箭头**

### 2.5 关联（Association）

{{< mermaid >}}classDiagram
    Class <-- Student
{{< /mermaid >}}

关系：成员对象是整体对象的属性，一般表示一种平等关系

表示：部分指向整体的**实线箭头**

### 2.6 依赖（Dependency）

{{< mermaid >}}classDiagram
    Car <.. Driver
{{< /mermaid >}}

关系：依赖对象一般作为参数传入另一个对象

表示：对象指向依赖对象的**虚线箭头**

## 参考

1. [看懂UML类图和时序图 — Graphic Design Patterns](https://design-patterns.readthedocs.io/zh_CN/latest/read_uml.html)
1. [30分钟学会UML类图 - 知乎](https://zhuanlan.zhihu.com/p/109655171)
1. [Class diagrams - Mermaid](https://mermaid-js.github.io/mermaid/#/classDiagram)

