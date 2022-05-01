# 设计模式教程


设计模式（Design Pattern）是对软件设计中普遍存在（反复出现）的各种问题，所提出的解决方案。

<!--more-->

## 1.单例模式

Singleton Pattern

### 1.1 定义与特点

**定义：**

一个类只有一个实例。

**特点：**

- 单例类只有一个实例对象。
- 单例对象必须由单例类自行创建。
- 单例类对外提供一个访问该单例的全局访问点。

### 1.2 优点与缺点

**优点：**

- 减少内存开销。
- 避免对资源的多重占用。
- 优化对共享资源的访问。

**缺点：**

- 单例模式一般没有接口，扩展困难。扩展必须修改代码，违背开闭原则。
- 在并发测试中，单例模式不利于代码调试。在调试过程中，如果单例中的代码没有执行完，也不能模拟生成一个新的对象。
- 单例模式的功能代码通常写在一个类中，如果功能设计不合理，则很容易违背单一职责原则。

### 1.3 应用场景

- 需要频繁创建的类，使用单例可以降低系统的内存压力，减少 GC。
- 某类只要求生成一个对象的时候，如一个班中的班长、每个人的身份证号等。
- 某类创建实例时占用资源较多，或实例化耗时较长，且经常使用。
- 某类需要频繁实例化，而创建的对象又频繁被销毁的时候，如多线程的线程池、网络连接池等。
- 频繁访问数据库或文件的对象。
- 对于一些控制硬件级别的操作，或者从系统上来讲应当是单一控制逻辑的操作，如果有多个实例，则系统会完全乱套。
- 当对象需要被共享的场合。由于单例模式只允许创建一个对象，共享该对象可以节省内存，并加快对象访问速度。如 Web 中的配置对象、数据库的连接池等。

### 1.4 代码实现

#### 1.4.1 懒汉式

```java
class LazySingleton {
    // 保证 instance 在所有线程中同步
    private static volatile LazySingleton instance;

    // private 避免类在外部被实例化
    private LazySingleton() {}

    public LazySingleton getInstance() {
        if (instance == null) {
            synchronized (LazySingleton.class) {
                if (instance == null) {
                    instance = new LazySingleton();
                }
            }
        }
        return instance;
    }
}
```

#### 1.4.2 饿汉式

```java
class HungrySingleton {
    private static final HungrySingleton instance = new HungrySingleton();

    private HungrySingleton() {}

    public HungrySingleton getInstance() {
        return instance;
    }
}
```

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
1. [设计模式 - 菜鸟教程](https://www.runoob.com/design-pattern/design-pattern-tutorial.html)

