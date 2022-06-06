# Java 多线程详解


<!--more-->

## 线程、程序和进程

- **程序**：含有指令和数据的文件，被存储在磁盘或其他的数据存储设备中，也就是说程序是静态的代码。

- **进程**：程序的一次执行过程，是系统运行程序的基本单位，因此进程是动态的。系统运行一个程序即是一个进程从创建，运行到消亡的过程。简单来说，一个进程就是一个执行中的程序，它在计算机中一个指令接着一个指令地执行着，同时，每个进程还占有某些系统资源如 CPU 时间，内存空间，文件，文件，输入输出设备的使用权等等。换句话说，当程序在执行时，将会被操作系统载入内存中。 线程是进程划分成的更小的运行单位。线程和进程最大的不同在于基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响。从另一角度来说，进程属于操作系统的范畴，主要是同一段时间内，可以同时执行一个以上的程序，而线程则是在同一程序内几乎同时执行一个以上的程序段。

- **线程**：与进程相似，但线程是一个比进程更小的执行单位。一个进程在其执行的过程中可以产生多个线程。与进程不同的是同类的多个线程共享同一块内存空间和一组系统资源，所以系统在产生一个线程，或是在各个线程之间作切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。

## 线程的基本状态

Java 线程在运行的生命周期中的指定时刻只可能处于下面 6 种不同状态的其中一个状态：

- **初始（New）**

- **运行（Runnable）**

- **等待（Waiting）**

- **超时等待（Timed Waiting）**

- **阻塞（Blocked）**

- **终止（Terminated）**

线程在生命周期中并不是固定处于某一个状态而是随着代码的执行在不同状态之间切换。Java 线程状态变迁如下图所示：

<img src="/imgs/Java多线程/Java线程状态图.jpeg" alt="Java 线程状态图">

## 线程池的优点

- 线程是稀缺资源，使用线程池可以减少创建和销毁线程的次数，每个工作线程都可以重复使用。

- 可以根据系统的承受能力，调整线程池中工作线程的数量，防止因为消耗过多内存导致服务器崩溃。

## 创建线程池

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

## 线程池原理

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

## 线程池源码分析

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

## 线程池饱和策略

`RejectedExecutionHandler`

当队列和线程池都满了，说明线程池处于饱和状态，那么必须对新提交的任务采用一种特殊的策略来进行处理。这个策略默认配置是`AbortPolicy`，表示无法处理新的任务而抛出异常。Java 提供了 4 种策略：

1. `AbortPolicy`：直接抛出异常

2. `CallerRunsPolicy`：只用调用所在的线程运行任务

3. `DiscardOldestPolicy`：丢弃队列里最早的一个任务，并执行当前任务

4. `DiscardPolicy`：不处理，丢弃

## Executor框架的两级调度模型

在 HotSpot VM 的模型中，Java 线程被一对一映射为本地操作系统线程。Java 线程启动时会创建一个本地操作系统线程，当 Java 线程终止时，对应的操作系统线程也被销毁回收，而操作系统会调度所有线程并将它们分配给可用的 CPU。

在上层，Java 程序会将应用分解为多个任务，然后使用应用级的调度器（Executor）将这些任务映射成固定数量的线程；在底层，操作系统内核将这些线程映射到硬件处理器上。

