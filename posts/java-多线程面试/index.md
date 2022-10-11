# Java 多线程面试


<!--more-->

## 线程、程序和进程

- **程序**：含有指令和数据的文件，被存储在磁盘或其他的数据存储设备中，也就是说程序是静态的代码。

- **进程**：程序的一次执行过程，是系统运行程序的基本单位，因此进程是动态的。系统运行一个程序即是一个进程从创建，运行到消亡的过程。简单来说，一个进程就是一个执行中的程序，它在计算机中一个指令接着一个指令地执行着，同时，每个进程还占有某些系统资源如 CPU 时间，内存空间，文件，文件，输入输出设备的使用权等等。换句话说，当程序在执行时，将会被操作系统载入内存中。 线程是进程划分成的更小的运行单位。线程和进程最大的不同在于基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响。从另一角度来说，进程属于操作系统的范畴，主要是同一段时间内，可以同时执行一个以上的程序，而线程则是在同一程序内几乎同时执行一个以上的程序段。

- **线程**：与进程相似，但线程是一个比进程更小的执行单位。一个进程在其执行的过程中可以产生多个线程。与进程不同的是同类的多个线程共享同一块内存空间和一组系统资源，所以系统在产生一个线程，或是在各个线程之间作切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。

## 线程

### Java 线程的 6 个基本状态

Java 线程在运行的生命周期中的指定时刻只可能处于下面 6 种不同状态的其中一个状态：

- **创建（New）**：还未`start()`的线程。
- **可运行（Runnable）**：调用`start()`后的线程，可能正在运行，也可能在排队等待时间片。
- **阻塞（Blocked）**：等待获取`monitor`锁，进入`synchronized`块或方法。
- **等待（Waiting）**：等待被唤醒，在调用`wait()`、`join()`、`LockSupport.park()`后。
- **超时等待（Timed Waiting）**：等待被唤醒，超时自动唤醒，在调用`wait(long)`、`join(long)`、`LockSupport.parkNanos(long)`、`LockSupport.parkUntil(long)`后。
- **终止（Terminated）**：执行结束后。

线程在生命周期中并不是固定处于某一个状态而是随着代码的执行在不同状态之间切换。Java 线程状态变迁如下图所示：

<img src="/imgs/Java多线程/Java线程状态图.jpeg" alt="Java 线程状态图">

### Runnable & Callable

1. 实现`Callable`接口需要重写`call()`方法，实现`Runnable`接口需要重写`run()`方法。
2. `Callable`的任务有返回值，而`Runnable`的任务无返回值。
3. `call()`方法抛出异常，`run()`方法不抛出。
4. 运行`Callable`任务可以拿到一个`FutureTask`对象，表示异步计算的结果。它提供了检查计算是否完成的方法，以等待计算的完成，并检索计算的结果。通过`FutureTask`对象可以了解任务执行情况，可取消任务的执行，还可获取执行结果。

### 创建线程

1. 继承`Thread`类，实现`run()`方法，创建对象。

```java
class Thread1 extends Thread {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}

Thread t1 = new Thread1();
t1.setName("t1");
t1.start();
```

2. 实现`Runnable`接口，实现`run()`方法，传入`Runnable`对象。

```java
class Thread2 implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
}

Thread t2 = new Thread(new Thread2(), "t2");
t2.start();

// 等价于
Thread t22 = new Thread(() -> {
    System.out.println(Thread.currentThread().getName());
}, "t22");
t22.start();
```

3. 实现`Callable`接口，实现`call()`方法，使用`FutureTask`进行包装，支持接收返回值。

```java
class Thread3 implements Callable<String> {
    @Override
    public String call() throws Exception {
        return Thread.currentThread().getName();
    }
}

FutureTask<String> ft = new FutureTask<>(new Thread3());
Thread t3 = new Thread(ft, "t3");
t3.start();
try {
    System.out.println(ft.get());
} catch (Exception e) {
    e.printStackTrace();
}
```

## 线程池

### 线程池的优点

- 线程是稀缺资源，使用线程池可以减少创建和销毁线程的次数，每个工作线程都可以重复使用。
- 可以根据系统的承受能力，调整线程池中工作线程的数量，防止因为消耗过多内存导致服务器崩溃。

### 创建线程池

```java
ThreadPoolExecutor​(int corePoolSize,
                   int maximumPoolSize,
                   long keepAliveTime,
                   TimeUnit unit,
                   BlockingQueue<Runnable> workQueue,
                   ThreadFactory threadFactory,
                   RejectedExecutionHandler handler)
```

- `corePoolSize`：线程池核心线程数量
- `maximumPoolSize`：线程池最大线程数量
- `keepAliveTime`：当活跃线程数大于核心线程数时，空闲的多余线程最大存活时间
- `unit`：存活时间的单位
- `workQueue`：存放任务的队列
- `threadFactory`：线程工厂
- `handler`：超出线程范围和队列容量的任务的处理程序

### 线程池原理

提交一个任务到线程池中，线程池的处理流程如下：

1. 判断线程池里的**核心线程**是否都在执行任务。
    - 如果核心线程空闲或者还有核心线程没有被创建，则创建一个新的工作线程来执行任务。
    - 如果核心线程都在执行任务，则进入下个流程。
2. 线程池判断**工作队列**是否已满。
    - 如果工作队列未满，则将新提交的任务存储在这个工作队列里。
    - 如果工作队列已满，则进入下个流程。
3. 判断线程池里的**线程**是否都处于工作状态。
    - 如果存在空闲线程，则创建一个新的工作线程来执行任务。
    - 如果全部繁忙，则交给**饱和策略**来处理这个任务。

<img src="/imgs/Java多线程/Java线程池处理流程.png" alt="Java 线程池处理流程">

### 线程池源码分析

- `ThreadPoolExecutor`的`execute()`方法

```java
public void execute(Runnable command) {
    if (command == null)
        throw new NullPointerException();

    // 如果线程数大于等于基本线程数或者线程创建失败，将任务加入队列
    int c = ctl.get();
    if (workerCountOf(c) < corePoolSize) {
        if (addWorker(command, true))
            return;
        c = ctl.get();
    }

    // 线程池处于运行状态并且加入队列成功
    if (isRunning(c) && workQueue.offer(command)) {
        int recheck = ctl.get();
        if (! isRunning(recheck) && remove(command))
            reject(command);

        // 线程池不处于运行状态或者加入队列失败，则创建线程（创建的是非核心线程）
        else if (workerCountOf(recheck) == 0)
            addWorker(null, false);
    }

    // 创建线程失败，则采取阻塞处理的方式
    else if (!addWorker(command, false))
        reject(command);
}
```

- `ThreadPoolExecutor`的`addWorker()`方法

```java
private boolean addWorker(Runnable firstTask, boolean core) {
    retry:
    for (int c = ctl.get();;) {
        // Check if queue empty only if necessary.
        if (runStateAtLeast(c, SHUTDOWN)
            && (runStateAtLeast(c, STOP)
                || firstTask != null
                || workQueue.isEmpty()))
            return false;

        for (;;) {
            if (workerCountOf(c)
                >= ((core ? corePoolSize : maximumPoolSize) & COUNT_MASK))
                return false;
            if (compareAndIncrementWorkerCount(c))
                break retry;
            c = ctl.get();  // Re-read ctl
            if (runStateAtLeast(c, SHUTDOWN))
                continue retry;
            // else CAS failed due to workerCount change; retry inner loop
        }
    }

    boolean workerStarted = false;
    boolean workerAdded = false;
    Worker w = null;
    try {
        w = new Worker(firstTask);
        final Thread t = w.thread;
        if (t != null) {
            final ReentrantLock mainLock = this.mainLock;
            mainLock.lock();
            try {
                // Recheck while holding lock.
                // Back out on ThreadFactory failure or if
                // shut down before lock acquired.
                int c = ctl.get();

                if (isRunning(c) ||
                    (runStateLessThan(c, STOP) && firstTask == null)) {
                    if (t.getState() != Thread.State.NEW)
                        throw new IllegalThreadStateException();
                    workers.add(w);
                    workerAdded = true;
                    int s = workers.size();
                    if (s > largestPoolSize)
                        largestPoolSize = s;
                }
            } finally {
                mainLock.unlock();
            }
            if (workerAdded) {
                t.start();
                workerStarted = true;
            }
        }
    } finally {
        if (! workerStarted)
            addWorkerFailed(w);
    }
    return workerStarted;
}
```

### 线程池饱和策略

`RejectedExecutionHandler`

当队列和线程池都满了，说明线程池处于饱和状态，那么必须对新提交的任务采用一种特殊的策略来进行处理。这个策略默认配置是`AbortPolicy`，表示无法处理新的任务而抛出异常。Java 提供了 4 种策略：

1. `AbortPolicy`：直接抛出异常。
2. `CallerRunsPolicy`：只用调用所在的线程运行任务。
3. `DiscardOldestPolicy`：丢弃队列里最早的一个任务，并执行当前任务。
4. `DiscardPolicy`：不处理，直接丢弃。

### Executor 框架的两级调度模型

在 HotSpot VM 的模型中，Java 线程被一对一映射为本地操作系统线程。Java 线程启动时会创建一个本地操作系统线程，当 Java 线程终止时，对应的操作系统线程也被销毁回收，而操作系统会调度所有线程并将它们分配给可用的 CPU。

在上层，Java 程序会将应用分解为多个任务，然后使用应用级的调度器（Executor）将这些任务映射成固定数量的线程；在底层，操作系统内核将这些线程映射到硬件处理器上。

## JUC 基础

通常所说的并发包（JUC）也就是 java.util.concurrent 及其子包，集中了 Java 并发的各种基础工具类，具体主要包括几个方面：

- 提供了 CountDownLatch、CyclicBarrier、Semaphore 等 ， 比 synchronized 更加高级，可以实现更加丰富的多线程操作的同步结构。
- 提供了 ConcurrentHashMap、有序的 ConcunrrentSkipListMap，或者通过类似快照机制实现线程安全的动态数组 CopyOnWriteArrayList 等，各种线程安全的容器。
- 提供了 ArrayBlockingQueue、SynchorousQueue 或针对特定场景的 PriorityBlockingQueue 等，各种并发队列实现。
- 强大的 Executor 框架，可以创建各种不同类型的线程池，调度任务运行等。

## synchronized

### synchronized 原理

`synchronized`是由 JVM 实现的一种实现互斥同步的一种方式，查看被`synchronized`修饰过的程序块编译后的字节码，会发现，被`synchronized`修饰过的程序块，在编译前后被编译器生成了 monitorenter 和 monitorexit 两个字节码指令。

在虚拟机执行到 monitorenter 指令时，首先要尝试获取对象的锁：

- 如果这个对象没有锁定，或者当前线程已经拥有了这个对象的锁，把锁的计数器 +1；当执行 monitorexit 指令时将锁计数器 -1；当计数器为 0 时，锁就被释放了。
- 如果获取对象失败了，那当前线程就要阻塞等待，直到对象锁被另外一个线程释放为止。

`synchronized`通过在对象头设置标记，达到了获取锁和释放锁的目的。

### 确定 synchronized 加锁对象

“锁”的本质是 monitorenter 和 monitorexit 字节码指令的一个 Reference 类型的参数，即要锁定和解锁的对象。

1. 如果指定了锁对象，即修饰类或代码块，如`synchronized(obj)`、`synchronized(this)`，说明加解锁对象为该对象。
2. 如果没有明确指定对象，即修饰方法：
    - 若修饰成员方法，表示此方法对应的对象为锁对象。
    - 若修饰静态方法，表示此方法对应的类对象为锁对象。

> 当一个对象被锁住时，对象里面所有用`synchronized`修饰的方法都将产生阻塞，而对象里非`synchronized`修饰的方法可正常被调用，不受锁影响。

### synchronized 是可重入锁

可重入性是锁的一个基本要求，为了解决自己锁死自己的情况。

如下面的代码，一个类中的同步方法调用另一个同步方法，假如`synchronized`不支持重入，进入`method2`方法时当前线程获得锁，`method2`方法里面执行`method1`时当前线程又要去尝试获取锁，这时如果不支持重入，它就要等锁释放，把自己阻塞，导致自己锁死自己。

```java
class SynchronizedTest {
    public synchronized void method1() {}

    public synchronized void method2() {
        method1();
    }
}
```

对`synchronized`来说，可重入性是显而易见的，在执行 monitorenter 指令时，如果这个对象没有锁定，或者当前线程已经拥有了这个对象的锁（而不是已拥有了锁则不能继续获取），就把锁的计数器 +1，通过这种方式实现了可重入性。

###  Java 原生锁的优化

在 Java 6 之前，Monitor 的实现完全依赖底层操作系统的互斥锁来实现。由于 Java 的线程与操作系统的原生线程有一一对应的关系，如果要将一个线程进行阻塞或唤醒都需要操作系统的协助，需要从用户态切换到内核态来执行，这种切换代价十分昂贵，很耗处理器时间。

现代 JDK 中做了大量的优化。一种优化是使用自旋锁，即在把线程进行阻塞操作之前先让线程自旋等待一段时间（尝试获取锁多次），可能在等待期间其他线程已经释放了锁，这时就无需再让线程执行阻塞操作，避免了用户态到内核态的切换。

现代 JDK 中还提供了三种不同的 Monitor 实现，也就是三种不同的锁：

- 偏向锁（Biased Locking）
- 轻量级锁
- 重量级锁

当 JVM 检测到不同的竞争状况时，会自动切换到适合的锁实现，这就是锁的升级、降级。

当没有竞争出现时，默认会使用偏向锁。JVM 会利用 CAS 操作，在对象头的 Mark Word 部分设置线程 ID，以表示这个对象偏向于当前线程，所以并不涉及真正的互斥锁，因为在很多应用场景中，大部分对象生命周期中最多会被一个线程锁定，使用偏向锁可以降低无竞争开销。

如果有另一线程试图锁定某个被偏斜过的对象，JVM 就撤销偏向锁，升级到轻量级锁。轻量级锁依赖 CAS 操作 Mark Word 来试图获取锁，如果重试成功，就使用普通的轻量级锁；否则，进一步升级为重量级锁。

### synchronized 是非公平锁

非公平主要表现在获取锁的行为上，并非是按照申请锁的时间前后给等待线程分配锁的，每当锁被释放后，任何一个线程都有机会竞争到锁，这样做的目的是为了提高执行性能，缺点是可能会产生线程饥饿现象。

### 锁消除和锁粗化

**锁消除**：指 JVM 即时编译器在运行时，对一些代码上要求同步，但被检测到不可能存在共享数据竞争的锁进行消除（这些锁很多不是程序员自己加入的）。主要根据逃逸分析。

**锁粗化**：原则上，同步块的作用范围要尽量小。但是如果一系列的连续操作都对同一个对象反复加锁和解锁，甚至加锁操作在循环体内，频繁地进行互斥同步操作也会导致不必要的性能损耗。锁粗化就是增大锁的作用域。

### synchronized 是悲观锁

`synchronized`显然是一个悲观锁，因为它的并发策略是悲观的：不管是否会产生竞争，任何的数据操作都必须要加锁、用户态核心态转换、维护锁计数器和检查是否有被阻塞的线程需要被唤醒等操作。

随着硬件指令集的发展，我们可以使用基于冲突检测的乐观并发策略。先进行操作，如果没有其他线程征用数据，那操作就成功了；如果共享数据有征用，产生了冲突，那就再进行其他的补偿措施。这种乐观的并发策略的许多实现不需要线程挂起，所以被称为非阻塞同步。

乐观锁的核心算法是 CAS（Compare and Swap，比较并交换），它涉及到三个操作数：内存值、预期值、新值。当且仅当预期值和内存值相等时才将内存值修改为新值。这样处理的逻辑是，首先检查某块内存的值是否跟之前我读取时的一样，如不一样则表示期间此内存值已经被别的线程更改过，舍弃本次操作，否则说明期间没有其他线程对此内存值操作，可以把新值设置给此块内存。

CAS 具有原子性，它的原子性由 CPU 硬件指令实现保证，即使用 JNI 调用`native`方法调用由 C++ 编写的硬件级别指令，JDK 中提供了`Unsafe`类执行这些操作。

### 乐观锁的优点和缺点

优点：

- 避免了悲观锁独占对象的现象，提高了并发性能。

缺点：

1. 乐观锁只能保证一个共享变量的原子操作。如果有多个共享变量，乐观锁将变得力不从心，但互斥锁能轻易解决，不管对象数量多少及对象颗粒度大小。
2. 长时间自旋可能导致开销大。假如 CAS 长时间不成功而一直自旋，会给 CPU 带来很大的开销。
3. ABA 问题。CAS 的核心思想是通过比对内存值与预期值是否一致而判断内存值是否被改过，但这个判断逻辑不严谨，假如内存值原来是 A，后来被改为 B，最后又被改成了 A，则 CAS 认为此内存值并没有发生改变，但实际上是有被其他线程改过的，这种情况对依赖过程值的情景的运算结果影响很大。解决的思路是引入版本号，每次变量更新都把版本号加一。

## volatile

### volatile 作用

- `volatile`在指令之间插入**内存屏障**+**缓存一致性协议**，保证按照特定顺序执行和某些变量的**可见性**。
- `volatile`通过内存屏障通知 CPU 和编译器**阻止指令重排优化**来维持**有序性**。

### Java 内存屏障

- `LoadLoad`屏障：对于这样的语句`Load1; LoadLoad; Load2`，在`Load2`及后续读取操作要读取的数据被访问前，保证`Load1`要读取的数据被读取完毕。
- `StoreStore`屏障：对于这样的语句`Store1; StoreStore; Store2`，在`Store2`及后续写入操作执行前，保证`Store1`的写入操作对其它处理器可见。
- `LoadStore`屏障：对于这样的语句`Load1; LoadStore; Store2`，在`Store2`及后续写入操作被刷出前，保证`Load1`要读取的数据被读取完毕。
- `StoreLoad`屏障：对于这样的语句`Store1; StoreLoad; Load2`，在`Load2`及后续所有读取操作执行前，保证`Store1`的写入对所有处理器可见。它的开销是四种屏障中最大的。在大多数处理器的实现中，这个屏障是个万能屏障，兼具其它三种内存屏障的功能。

### volatile 语义的内存屏障

- 在每个`volatile`写操作前插入`StoreStore`屏障，在写操作后插入`StoreLoad`屏障。
- 在每个`volatile`读操作前插入`LoadLoad`屏障，在读操作后插入`LoadStore`屏障。
- 由于内存屏障的作用，避免了`volatile`变量和其它指令重排序、线程之间实现了通信，使得`volatile`表现出了锁的特性。

## 可重入锁 ReentrantLock

### synchronized 和 ReentrantLock 简单对比

锁的实现原理基本是为了达到一个目的：让所有的线程都能看到某种标记。

`synchronized`通过在对象头中设置标记实现了这一目的，是一种 JVM 原生的锁实现方式，而`ReentrantLock`以及所有的基于`Lock`接口的实现类，都是通过用一个`volatile`修饰的`int`类型变量，并保证每个线程都能拥有对该变量的可见性和修改的原子性，其本质基于 AQS 框架。

### AQS 框架

AQS（Abstract Queued Synchronizer）是一个用来构建锁和同步器的框架，是一个抽象类，各种`Lock`包中的锁（常用的有`ReentrantLock`、`ReadWriteLock`），以及其他如`Semaphore`、`CountDownLatch`，甚至是早期的`FutureTask`等，都是基于 AQS 来构建的。

AQS 定义了一个`volatile int state`变量，表示同步状态，当线程调用`lock()`方法时，

- 如果`state==0`，说明没有任何线程占有共享资源的锁，可以获得锁并将`state=1`；
- 如果`state==1`，则说明有线程目前正在使用共享变量，其他线程必须加入同步队列进行等待。

AQS 通过内部类 Node 构成的一个**双向链表**结构的同步队列，来完成线程获取锁的排队工作，当有线程获取锁失败后，就被添加到队列末尾。

- Node 类是对要访问同步代码的线程的封装，用变量`volatile int waitStatus`表示其状态（五种不同状态：`CONDITION`是否被阻塞，`SIGNAL`是否等待唤醒，`CANCELLED`是否已经被取消、`PROPAGATE`需要传播信号、`0`不属于以上四种状态）。
- Node 类有两个常量，`static final Node SHARED = new Node()`和`static final Node EXCLUSIVE = null`，分别代表共享模式和独占模式。所谓共享模式是一个锁允许多个线程同时操作（信号量`Semaphore`就基于 AQS 的共享模式实现），独占模式是同一个时间段只能有一个线程对共享资源进行操作，多余的请求线程需要排队等待（如`ReentranLock`）。

AQS 通过内部类 ConditionObject 构建等待队列（可有多个），当 Condition 调用`wait()`方法后，线程将会加入等待队列中，而当 Condition 调用`signal()`方法后，线程将从等待队列移到同步队列中进行锁竞争。

AQS 和 Condition 各自维护了不同的队列，在使用 Lock 和 Condition 的时候，其实就是两个队列的互相移动。

### synchronized 和 ReentrantLock 详细对比

ReentrantLock 是 Lock 的实现类，是一个互斥的同步锁。

从功能角度，ReentrantLock 比 synchronized 的同步操作更精细（因为可以像普通对象一样使用），甚至实现 synchronized 没有的高级功能，如：

- 等待可中断：当持有锁的线程长期不释放锁的时候，正在等待的线程可以选择放弃等待，对处理执行时间非常长的同步块很有用。
- 带超时的获取锁尝试：在指定的时间范围内获取锁，如果时间到了仍然无法获取则返回。
- 可以判断是否有线程在排队等待获取锁。
- 可以响应中断请求：与 synchronized 不同，当获取到锁的线程被中断时，能够响应中断，中断异常将会被抛出，同时锁会被释放。
- 可以实现公平锁。

从锁释放角度，synchronized 在 JVM 层面上实现，不但可以通过一些监控工具监控 synchronized 的锁定，而且在代码执行出现异常时，JVM 会自动释放锁；但是使用 Lock 则不行，Lock 是通过代码实现的，要保证锁一定会被释放，就必须将 unLock() 放到 finally 中。

从性能角度，synchronized 早期实现比较低效，对比 ReentrantLock，大多数场景性能都相差较大。但是在 Java 6 中对其进行了非常多的改进，在竞争不激烈时，synchronized 的性能要优于 ReetrantLock；在高竞争情况下，synchronized 的性能会下降几十倍，但是 ReetrantLock 的性能能维持常态。

### ReentrantLock 是可重入锁

ReentrantLock 内部自定义了同步器 Sync（Sync 既实现了 AQS，又实现了 AOS，而 AOS 提供了一种互斥锁持有的方式），其实就是加锁的时候通过 CAS 算法，将线程对象放到一个双向链表中，每次获取锁的时候，看下当前维护的那个线程 ID 和当前请求的线程 ID 是否一样，一样就可重入了。

### ReadWriteLock 和 StampedLock 对比

虽然 ReentrantLock 和 synchronized 简单实用，但是行为上有一定局限性，要么不占，要么独占。实际应用场景中，有时候不需要大量竞争的写操作，而是以并发读取为主，为了进一步优化并发操作的粒度，Java 提供了读写锁。

读写锁基于的原理是多个读操作不需要互斥，如果读锁试图锁定时，写锁是被某个线程持有，读锁将无法获得，而只好等待对方操作结束，这样就可以自动保证不会读取到有争议的数据。

ReadWriteLock 代表了一对锁，下面是一个基于读写锁实现的数据结构，当数据量较大，并发读多、并发写少的时候，能够比纯同步版本凸显出优势：

```java
import java.util.Map;
import java.util.TreeMap;
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class RWSample {
    private final Map<String, String> m = new TreeMap<>();
    private final ReentrantReadWriteLock rwl = new ReentrantReadWriteLock();
    private final Lock r = rwl.readLock();
    private final Lock w = rwl.writeLock();

    public String get(String key) {
        r.lock();
        System.out.println("读锁锁定");
        try {
            return m.get(key);
        } finally {
            r.unlock();
        }
    }

    public String put(String key, String value) {
        w.lock();
        System.out.println("写锁锁定");
        try {
            return m.put(key, value);
        } finally {
            w.unlock();
        }
    }
}
```

读写锁看起来比 synchronized 的粒度似乎细一些，但在实际应用中，其表现也并不尽如人意，主要还是因为相对比较大的开销。所以，JDK 在后期引入了 StampedLock，在提供类似读写锁的同时，还支持优化读模式。优化读基于假设，大多数情况下读操作并不会和写操作冲突，其逻辑是先试着修改，然后通过`validate`方法确认是否进入了写模式，如果没有进入，就成功避免了开销；如果进入，则尝试获取读锁。

```java
import java.util.Map;
import java.util.TreeMap;
import java.util.concurrent.locks.StampedLock;

public class Solution {
    private final Map<String, String> m = new TreeMap<>();
    private final StampedLock sl = new StampedLock();

    public String get(String key) {
        long stamp = sl.tryOptimisticRead();
        String value = m.get(key);
        if (!sl.validate(stamp)) {
            stamp = sl.readLock();
            System.out.println("读锁锁定");
            try {
                value = m.get(key);
            } finally {
                sl.unlockRead(stamp);
            }
        }
        return value;
    }

    public String put(String key, String value) {
        long stamp = sl.writeLock();
        System.out.println("写锁锁定");
        try {
            return m.put(key, value);
        } finally {
            sl.unlockWrite(stamp);
        }
    }
}
```

## AQS

## ThreadLocal 和 InheritableThreadLocal

使资源不再共享，每个线程拥有一份拷贝的资源，实现了线程间隔离。

**原理：**

- 每个线程内部有`threadLocals`和`inheritableThreadLocals`两个属性，这是两个`Map`。其中`threadLocals`实现了线程间隔离，`inheritableThreadLocals`则可以将父线程中`threadLocals`的内容赋值给子线程，实现了父子线程数据传递。
- 当添加`ThreadLocal`属性时，将`ThreadLocal`对象作为 key，添加到了当前线程的`threadLocals`中。
- 当获取`ThreadLocal`属性时，实际上是从当前线程的`threadLocals`中获取。

```java
public class ThreadLocal {
    public T get() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null) {
                T result = (T) e.value;
                return result;
            }
        }
        return setInitialValue(); // null
    }

    public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            map.set(this, value);
        } else {
            createMap(t, value);
        }
    }

    public void remove() {
        ThreadLocalMap m = getMap(Thread.currentThread());
        if (m != null) {
            m.remove(this);
        }
    }

    ThreadLocalMap getMap(Thread t) {
        return t.threadLocals;
    }

    void createMap(Thread t, T firstValue) {
        t.threadLocals = new ThreadLocalMap(this, firstValue);
    }

    static ThreadLocalMap createInheritedMap(ThreadLocalMap parentMap) {
        return new ThreadLocalMap(parentMap); // 复制一份
    }

    static class ThreadLocalMap {
        static class Entry extends WeakReference<ThreadLocal<?>> {
            Object value;

            Entry(ThreadLocal<?> k, Object v) {
                super(k);
                value = v;
            }
        }

        private static final int INITIAL_CAPACITY = 16;
        private Entry[] table; // 开放寻址法解决哈希冲突
    }
}

public class InheritableThreadLocal<T> extends ThreadLocal<T> {
    protected T childValue(T parentValue) {
        return parentValue;
    }

    ThreadLocalMap getMap(Thread t) {
       return t.inheritableThreadLocals;
    }

    void createMap(Thread t, T firstValue) {
        t.inheritableThreadLocals = new ThreadLocalMap(this, firstValue);
    }
}

public class Thread implements Runnable {
    private Thread(..., boolean inheritThreadLocals) { // true
        Thread parent = currentThread();
        if (inheritThreadLocals && parent.inheritableThreadLocals != null)
            this.inheritableThreadLocals = ThreadLocal.createInheritedMap(parent.inheritableThreadLocals);
    }

    ThreadLocal.ThreadLocalMap threadLocals = null;
    ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;
}
```


