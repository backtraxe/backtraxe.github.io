# 设计模式教程


设计模式（Design Pattern）是对软件设计中普遍存在（反复出现）的各种问题，所提出的解决方案。

<!--more-->

## 1.介绍

面向对象五大基本原则：

- **单一职责原则（Single Responsibility Principle，SRP）**：一个类的功能要单一，不能包罗万象。
- **开放封闭原则（Open-Close Principle，OCP）**：一个模块在扩展性方面应该是开放的而在更改性方面应该是封闭的。比如：一个网络模块，原来只实现了服务端功能，而现在要加入客户端功能，那么应当在不用修改服务端功能代码的前提下，就能够增加客户端功能的实现代码，这要求在设计之初，就应当将服务端和客户端分开，公共部分抽象出来。
- **里式替换原则（Liskov Substitution Principle，LSP）**：子类应当可以替换父类并出现在父类能够出现的任何地方。
- **依赖倒置原则（Dependency Inversion Principle，DIP）**：具体依赖抽象，上层依赖下层。假设B是较A低的模块，但B需要使用到A的功能，这个时候，B不应当直接使用A中的具体类： 而应当由B定义一个抽象接口，并由A来实现这个抽象接口，B只使用这个抽象接口：这样就达到了依赖倒置的目的，B也解除了对A的依赖，反过来是A依赖于B定义的抽象接口。通过上层模块难以避免依赖下层模块，假如B也直接依赖A的实现，那么就可能造成循环依赖。一个常见的问题就是编译A模块时需要直接包含到B模块的cpp文件，而编译B时同样要直接包含到A的cpp文件。
- **接口分离原则（Interface Segregation Principle，ISP)**：模块间要通过抽象接口隔离开，而不是通过具体的类强耦合起来。

设计模式的六大原则：

- **开闭原则**：一个软件实体如类、模块和函数应该对修改封闭，对扩展开放。
- **单一职责原则**：一个类只做一件事，一个类应该只有一个引起它修改的原因。
- **里氏替换原则**：子类应该可以完全替换父类。也就是说在使用继承时，只扩展新功能，而不要破坏父类原有的功能。
- **依赖倒置原则**：细节应该依赖于抽象，抽象不应依赖于细节。把抽象层放在程序设计的高层，并保持稳定，程序的细节变化由低层的实现层来完成。
- **迪米特法则（Law Of Demeter，LOD）**：又名“最少知道原则”，一个类不应知道自己操作的类的细节，尽量降低类与类之间的耦合。
- **接口隔离原则**：客户端不应依赖它不需要的接口。如果一个接口在实现时，部分方法由于冗余被客户端空实现，则应该将接口拆分，让实现类只需依赖自己需要的接口方法。

## 2.构建型模式

Creational Patterns

### 2.1 工厂模式

Factory Pattern

构建对象最常用的方式是 new 一个对象。然而每 new 一个对象，相当于调用者多知道了一个类，增加了类与类之间的联系，不利于程序的松耦合。工厂模式便是用于封装对象构建过程的设计模式。

#### 2.1.1 简单工厂模式

Simple Factory Pattern

当我们需要创建一个对象时，不直接 new 这个对象，而是通过提供对象信息给对象工厂，然后由工厂提供创建的对象。这样调用者无需关心对象的创建细节，实现了解耦。

- 用户

```java
class User {
    public void eat() {
        FruitFactory fruitFactory = new FruitFactory();
        Fruit apple = fruitFactory.create("苹果");
        apple.eat();
        Fruit pear = fruitFactory.create("梨");
        pear.eat();
    }
}
```

- 水果工厂

```java
class FruitFactory {
    public Fruit create(String type) {
        switch (type) {
            case "苹果": return new Apple();
            case "梨": return new Pear();
            default: throw new IllegalArgumentException("暂时没有这种水果");
        }
    }
}
```

- 水果

```java
abstract class Fruit {}

class Apple extends Fruit {}

class Pear extends Fruit {}
```

优点：

- 封装了对象创建细节，调用者无需关心对象的创建细节，实现了解耦。

缺点：

- 如果需要生产的产品过多，此模式会导致工厂类过于庞大，承担过多的职责，变成超级类。当苹果生产过程需要修改时，要来修改此工厂。梨子生产过程需要修改时，也要来修改此工厂。也就是说这个类不止一个引起修改的原因，违背了单一职责原则。
- 当要生产新的产品时，必须在工厂类中添加新的分支。而开闭原则告诉我们：类应该对修改封闭。我们希望在添加新功能时，只需增加新的类，而不是修改既有的类，违背了开闭原则。

#### 2.1.2 工厂方法模式

Factory Method Pattern

为了解决简单工厂的缺点，工厂方法模式应运而生，它规定每个产品都有一个专属工厂。

- 用户

```java
class User {
    public void eat() {
        Fruit apple = new AppleFactory().create();
        apple.eat();
        Fruit pear = new PearFactory().create();
        pear.eat();
    }
}
```

- 苹果工厂

```java
class AppleFactory {
    public Fruit create() {
        return new Apple();
    }
}
```

- 梨工厂

```java
class PearFactory {
    public Fruit create() {
        return new Pear();
    }
}
```

优点：

- 封装了对象创建细节。

缺点：

- 将调用者与创建对象解耦的同时，引入了调用者和对象工厂之间的耦合。

#### 2.1.3 抽象工厂模式

Abstract Factory Pattern

定义一个工厂接口，提供创建方法，然后由具体工厂去实现该方法。

- 用户

```java
class User {
    public void eat() {
        FruitFactory fruitFactory = new AppleFactory();
        Fruit apple = fruitFactory.create();
        apple.eat();
        fruitFactory = new PearFactory();
        Fruit pear = fruitFactory.create();
        pear.eat();
    }
}
```

- 工厂接口

```java
interface FruitFactory {
    Fruit create();
}
```

- 苹果工厂

```java
class AppleFactory implements FruitFactory {
    @Override
    public Fruit create() {
        return new Apple();
    }
}
```

- 梨工厂

```java
class PearFactory implements FruitFactory {
    @Override
    public Fruit create() {
        return new Pear();
    }
}
```

优点：

- 实现调用者与创建对象的解耦，也实现了调用者与具体对象工厂的解耦。
- 只需少量代码即可无缝切换创建对象的具体工厂类。

缺点：

- 当需要新增功能时，所有实现了工厂接口的具体工厂都必须实现新增的抽象方法，代码修改量巨大。

### 2.2 单例模式

Singleton Pattern

当一个类全局只需要一个实例时，即可使用单例模式。

不能自行创建，可通过指定方法获取该实例。

优点：

- 减少内存开销。
- 避免对资源的多重占用。
- 优化对共享资源的访问。

缺点：

- 单例模式一般没有接口，扩展困难。扩展必须修改代码，违背开闭原则。
- 在并发测试中，单例模式不利于代码调试。在调试过程中，如果单例中的代码没有执行完，也不能模拟生成一个新的对象。
- 单例模式的功能代码通常写在一个类中，如果功能设计不合理，则很容易违背单一职责原则。

用户使用

```java
Singleton instance = Singleton.getInstance();
```

#### 2.2.1 饿汉式

实例在声明时便初始化，即使不使用。

```java
class Singleton {
    private static Singleton instance = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return instance;
    }
}
```

#### 2.2.2 懒汉式：静态变量

实例在第一次获取时才初始化，减少内存占用和减少类初始化时间。

```java
class Singleton {
    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

**为什么使用 synchronized ？**

如果有多个线程同一时间调用 getInstance 方法，instance 变量可能会被实例化多次。为了保证线程安全，我们需要给判空过程加上锁。

**为什么使用两次 if 判断？**

如果外面不做空检查，当多个线程调用 getInstance 时，每次都需要执行 synchronized 同步方法，这样会严重影响程序的执行效率。所以更好的做法是在同步之前，再加上一层检查。

如果里面不做空检查，可能会有两个线程同时通过了外面的空检查，然后在一个线程 new 出实例后，第二个线程进入锁中又 new 出一个实例，导致创建多个实例。

**为什么使用 volatile ？**

JVM 底层为了优化程序运行效率，可能会对我们的代码进行指令重排序，在一些特殊情况下会导致出现空指针，为了防止这个问题，更进一步的优化是给 instance 变量加上 volatile 关键字。

`instance = new Singleton()`实际上执行了三条重要的指令：

1. 分配对象的内存空间。
2. 初始化对象。
3. 将变量 instance 指向刚分配的内存空间。

在这个过程中，如果第二条指令和第三条指令发生了重排序，可能导致 instance 还未初始化时，其他线程提前通过双检锁外层的 null 检查，获取到“不为 null，但还没有执行初始化”的 instance 对象，发生空指针异常。

#### 2.2.3 懒汉式：静态内部类

静态内部类实现。内部类会在使用时才被加载，并且 JVM 会保证内部类只会被加载一次。

```java
class Singleton {
    private static class Inner {
        public static Singleton instance = new Singleton();
    }

    private Singleton() {}

    public Singleton getInstance() {
        return Inner.instance;
    }
}
```

### 2.3 建造者模式

Builder Pattern

建造者模式用于创建过程稳定，但配置多变的对象。

用户不能通过 new 创建对象，只能通过 Builder 构建。对于必须配置的属性，通过 Builder 的构造方法传入，可选的属性通过 Builder 的链式调用方法传入，如果不配置，将使用默认配置。

- 用户

-

```java
class MikeTea {
    private String type;
    private int size;
    private boolean ice;

    private MikeTea() {}

    private MikeTea(Builder builder) {
        type = builder.type;
        size = builder.size;
        ice = builder.ice;
    }

    public static class Builder {
        private String type;
        private int size = 1;
        private boolean ice = true;

        public Builder(String type) {
            this.type = type;
        }

        public Builder size(int size) {
            this.size = size;
            return this;
        }

        public Builder ice(boolean ice) {
            this.ice = ice;
            return this;
        }

        public MikeTea create() {
            return new MikeTea(this);
        }
    }
}
```


### 2.4





## 2.代理模式

Proxy Pattern

### 2.1 定义与特点

**定义：**

**特点：**

### 2.2 优点与缺点

### 2.3 应用场景

### 2.4 代码实现

## 参考

1. [图说设计模式 — Graphic Design Patterns](https://design-patterns.readthedocs.io/zh_CN/latest/index.html)
2. [设计模式 - 菜鸟教程](https://www.runoob.com/design-pattern/design-pattern-tutorial.html)

