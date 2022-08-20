# 设计模式教程


设计模式（Design Pattern）是对软件设计中普遍存在（反复出现）的各种问题，所提出的解决方案。

<!--more-->

## 1.介绍

面向对象的特点是:

- 可维护
- 可复用
- 可扩展
- 灵活性好

设计模式的六大原则：

- **开闭原则（Open-Closed Principle，OCP）**：一个软件实体如类、模块和函数应该对修改封闭，对扩展开放。
- **单一职责原则（Single Responsibility Principle，SRP）**：一个类只做一件事，一个类应该只有一个引起它修改的原因。
- **里氏替换原则（Liskov Substitution Principle，LSP）**：子类应该可以完全替换父类。也就是说在使用继承时，只扩展新功能，而不要破坏父类原有的功能。
- **依赖倒置原则（Dependence Inversion Principle，DIP）**：细节应该依赖于抽象，抽象不应依赖于细节。把抽象层放在程序设计的高层，并保持稳定，程序的细节变化由低层的实现层来完成。
- **迪米特法则（Law of Demeter，LoD）**：又名“最少知道原则”，一个类不应知道自己操作的类的细节，尽量降低类与类之间的耦合。
- **接口隔离原则（Interface Segregation Principle，ISP）**：客户端不应依赖它不需要的接口。如果一个接口在实现时，部分方法由于冗余被客户端空实现，则应该将接口拆分，让实现类只需依赖自己需要的接口方法。

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

使用建造者模式的好处是不用担心忘了指定某个配置，保证了构建过程是稳定的。

- 用户

```java
class User {
    public void buyMilkTea() {
        MilkTea milkTea = new MilkTea().Builder("原味奶茶").build();
        MilkTea mongoMilkTea = new MilkTea.Builder("杨枝甘露").size(2).ice(true).build();
    }
}
```

- 对象

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

### 2.4 原型模式

Prototype Pattern

用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象。

简单来说就是深拷贝复制对象。

- 用户

```java
class User {
    public void buyMilkTea() throws CloneNotSupportedException {
        // 两杯一样的
        MilkTea milkTea1 = new MilkTea("蜂蜜柚子茶", true);
        MilkTea milkTea2 = milkTea1.clone();
    }
}
```

- 对象

```java
class MilkTea implements Cloneable {
    private String type;
    private boolean ice;

    public MilkTea(String type, boolean ice) {
        this.type = type;
        this.ice = ice;
    }

    @Override
    public MilkTea clone() throws CloneNotSupportedException {
        return (MilkTea) super.clone();
    }
}
```

## 3.结构型模式

Structural Patterns

### 3.1 适配器模式

Adapter Pattern

将一个类的接口转换成用户希望的另一个接口，使得原本由于接口不兼容而不能一起工作的那些类能一起工作。

适配器模式适用于有相关性但不兼容的结构，源接口通过一个中间件转换后才可以适用于目标接口，这个转换过程就是适配，这个中间件就称之为适配器。

只有在遇到接口无法修改时才应该考虑适配器模式。如果接口可以修改，那么将接口改为一致的方式会让程序结构更加良好。

- Lightning

```java
class Lightning {
    public void transmit() {
        System.out.println("正在使用 Lightning 传输数据");
    }
}
```

- USB-C

```java
class USBC {
    public void transmit() {
        System.out.println("正在使用 USB-C 传输数据");
    }
}
```

- 电脑

```java
class PC {
    // 电脑只有 USB-C 接口
    public void connect(USBC plug) {
        plug.transmit();
    }
}
```

- Lightning 转 USB-C 适配器

```java
class LightningToUSBCAdapter extends USBC {
    private Lightning plug;

    public LightningToUSBCAdapter(Lightning plug) {
        this.plug = plug;
    }

    @Override
    public void transmit() {
        plug.transmit();
    }
}
```

- 用户

```java
class User {
    public void transmitData() {
        PC pc = new PC();
        pc.connect(new USBC());
        pc.connect(new LightningToUSBCAdapter(new Lightning()));
    }
}
```

### 3.2 桥接模式

Bridge Pattern

将抽象部分与它的实现部分分离，使它们都可以独立地变化。它是一种对象结构型模式，又称为柄体模式或接口模式。

- 图形

```java
interface Shape {
    void draw();
}
```

- 矩形

```java
class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("画矩形");
    }
}
```

- 三角形

```java
class Triangle implements Shape {
    @Override
    public void draw() {
        System.out.println("画三角形");
    }
}
```

- 圆形

```java
class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("画圆");
    }
}
```

### 3.3 组合模式

Composite Pattern

组合模式：又叫部分整体模式，是用于把一组相似的对象当作一个单一的对象。组合模式依据树形结构来组合对象，用来表示部分以及整体层次。这种类型的设计模式属于结构型模式，它创建了对象组的树形结构。

组合模式用于整体与部分的结构，当整体与部分有相似的结构，在操作时可以被一致对待时，就可以使用组合模式。

- **透明方式**：在 Component 中声明所有管理子对象的方法，包括 add 、remove 等，这样继承自 Component 的子类都具备了 add、remove 方法。对于外界来说叶节点和枝节点是透明的，它们具备完全一致的接口。
- **安全方式**：在 Component 中不声明 add 和 remove 等管理子对象的方法，这样叶节点就无需实现它，只需在枝节点中实现管理子对象的方法即可。

安全方式和透明方式各有好处，在使用组合模式时，需要根据实际情况决定。但大多数使用组合模式的场景都是采用的透明方式，虽然它有点不安全，但是客户端无需做任何判断来区分是叶子结点还是枝节点。

### 3.4 装饰模式

Decorator Pattern

动态地给一个对象增加一些额外的职责，就增加对象功能来说，装饰模式比生成子类实现更为灵活。其别名也可以称为包装器，与适配器模式的别名相同，但它们适用于不同的场合。根据翻译的不同，装饰模式也有人称之为“油漆工模式”。

- 增强一个类原有的功能。
- 为一个类添加新的功能。
- 不会改变原有的类功能。

没有修改原有的功能，只是扩展了新的功能，这种模式在装饰模式中称之为半透明装饰模式。

这个装饰类对客户端来说是可见的、不透明的。而被装饰者可以是实现了指定接口的任意对象，所以被装饰者对客户端是不可见的、透明的。由于一半透明，一半不透明，所以称之为半透明装饰模式。

半透明装饰模式中，我们无法多次装饰。

Java I/O 的设计框架便是使用的装饰者模式。

### 3.5 外观模式

Facade Pattern

外部与一个子系统的通信必须通过一个统一的外观对象进行，为子系统中的一组接口提供一个一致的界面，外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。外观模式又称为门面模式。

外观模式就是这么简单，它使得两种不同的类不用直接交互，而是通过一个中间件——也就是外观类——间接交互。外观类中只需要暴露简洁的接口，隐藏内部的细节，所以说白了就是封装的思想。

### 3.6 享元模式

Flyweight Pattern

运用共享技术有效地支持大量细粒度对象的复用。系统只使用少量的对象，而这些对象都很相似，状态变化很小，可以实现对象的多次复用。由于享元模式要求能够共享的对象必须是细粒度对象，因此它又称为轻量级模式。

享元模式的主要目的是实现对象的共享，即共享池，当系统中对象多的时候可以减少内存的开销，通常与工厂模式一起使用。FlyWeightFactory 负责创建和管理享元单元，当一个客户端请求时，工厂需要检查当前对象池中是否有符合条件的对象，如果有，就返回已经存在的对象，如果没有，则创建一个新对象，FlyWeight是超类。

### 3.7 代理模式

Proxy Pattern

代理模式：给某一个对象提供一个代理，并由代理对象控制对原对象的引用。

- 静态代理：包装方法

```java
interface Shape {
    void draw(String color);
    void erase();
}

class Circle implements Shape {
    @Override
    public void draw(String color) {
        System.out.println("画" + color + "圆");
    }

    @Override
    public void erase() {
        System.out.println("擦除图形");
    }
}

class CircleProxy implements Shape {
    private final Circle circle;

    public CircleProxy(Circle circle) {
        this.circle = circle;
    }

    @Override
    public void draw(String color) {
        System.out.println("准备作画");
        circle.draw(color);
        System.out.println("结束作画");
    }

    @Override
    public void erase() {
        System.out.println("准备擦除");
        circle.erase();
        System.out.println("擦除完毕");
    }
}

class User {
    public void drawing() {
        Circle circle = new Circle();
        circle.draw("红色");
        circle.erase();
        CircleProxy proxy = new CircleProxy(circle);
        proxy.draw("红色");
        proxy.erase();
    }
}
```

- 动态代理：反射

```java
interface Shape {
    void draw(String color);
    void erase();
}

class Circle implements Shape {
    @Override
    public void draw(String color) {
        System.out.println("画" + color + "圆");
    }

    @Override
    public void erase() {
        System.out.println("擦除图形");
    }
}

class CircleProxy implements InvocationHandler {
    private Circle circle;

    public Shape getInstance(Circle circle) {
        this.circle = circle;
        return (Shape) Proxy.newProxyInstance(circle.getClass().getClassLoader(), circle.getClass().getInterfaces(), this);
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Object result = null;
        if (method.getName().equals("draw")) {
            System.out.println("开始作画");
            method.invoke(circle, args);
            System.out.println("结束作画");
        } else if (method.getName().equals("erase")) {
            System.out.println("开始擦除");
            method.invoke(circle, args);
            System.out.println("擦除完毕");
        }
        return result;
    }
}

class User {
    public void drawing() {
        Circle circle = new Circle();
        circle.draw("红色");
        circle.erase();
        Shape proxy = new CircleProxy().getInstance(circle);
        proxy.draw("红色");
        proxy.erase();
    }
}
```

## 4.行为型模式

Behavioral Patterns



## 参考

1. [图说设计模式 — Graphic Design Patterns](https://design-patterns.readthedocs.io/zh_CN/latest/index.html)
2. [设计模式 - 菜鸟教程](https://www.runoob.com/design-pattern/design-pattern-tutorial.html)

