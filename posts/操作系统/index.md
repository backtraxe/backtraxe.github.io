# 操作系统


操作系统（Operating System, OS）是硬件和其它软件沟通的桥梁。

<!--more-->

## 第1章 计算机系统概述

### 1.1 操作系统的基本概念

#### 1.1.1 操作系统的概念

- 控制和管理整个计算机系统的硬件与软件资源。
- 合理地组织、调度计算机的工作与资源的分配。
- 为用户和其他软件提供方便的接口与环境。
- 计算机系统中最基本的系统软件。

#### 1.1.2 操作系统的特征

1. **并发（Concurrence）**：两个或多个事件在**同一时间间隔**内发生。
    - 并发是同一时间间隔，**并行（Parallel）是同一时间**。
    - 单处理机通过分时实现并发，微观上交替执行。
2. **共享（Sharing）**：系统中的资源可供内存中多个并发执行的进程共同使用。
    - **互斥共享**：一段时间内只允许一个进程访问该资源。如：打印机、磁带机。
    - **同时访问**：一段时间内允许多个进程“同时”访问该资源，宏观同时访问，微观交替访问。如：磁盘设备。
3. **虚拟（Virtual）**：把一个物理上的实体变为若干逻辑上的对应物。
    - 按物理实体分类：
        - **虚拟处理器**：利用多道程序设计技术把一个物理上的 CPU 虚拟为多个逻辑上的 CPU。
        - **虚拟存储器**：将一台机器的物理存储器变为虚拟存储器，以便从逻辑上扩充存储器的容量。
        - **虚拟设备**：将一台物理 I/O 设备虚拟为多台逻辑上的 I/O 设备，并允许每个用户占用一台逻辑上的 I/O 设备，使原来仅允许在一段时间内由一个用户访问的设备（即临界资源）变为在一段时间内允许多个用户同时访问的共享设备。
    - 按时空分类：
        - **时分复用**：虚拟处理器。
        - **空分复用**：虚拟存储器。
4. **异步（Asynchronism）**：多道程序环境允许多个程序并发执行，但由于资源有限，进程的执行并不是一贯到底的，而是走走停停的，它以不可预知的速度向前推进。

#### 1.1.3 操作系统两个最基本的特征

1. **并发和共享**，两者之间互为存在的条件。
2. 资源共享是以程序的并发为条件的，若系统不允许程序并发执行，则自然不存在资源共享问题。
3. 若系统不能对资源共享实施有效的管理，则必将影响到程序的并发执行，甚至根本无法并发执行。

#### 1.1.4 操作系统的功能

1. **处理机管理**：
    - 处理机的分配和运行都以进程（或线程）为基本单位，因而对处理机的管理可归结为对进程的管理。
    - **进程管理**主要包括进程控制、进程同步、进程通信、死锁处理、处理机调度等。
2. **存储器管理**：
    - 给多道程序的运行提供良好的环境，方便用户使用及提高内存的利用率。
    - 主要包括内存分配与回收、地址映射、内存保护与共享和内存扩充等功能。
3. **文件管理**：
    - 计算机中的信息都是以文件的形式存在的，操作系统中负责文件管理的部分称为**文件系统**。
    - 主要包括文件存储空间的管理、目录管理及文件读写管理和保护等。
4. **设备管理**：
    - 完成用户的 I/O 请求，方便用户使用及提高设备的利用率。
    - 主要包括缓冲管理、设备分配、设备处理和虚拟设备等功能。

#### 1.1.5 操作系统提供的用户接口

1. **命令接口**：用户利用命令接口来组织和控制作业的执行。
    - **联机命令接口/交互式命令接口**：适用于分时或实时系统的接口。它由一组键盘操作命令组成。用户通过控制台或终端输入操作命令，向系统提出各种服务要求。用户每输入一条命令，控制权就转给操作系统的命令解释程序，然后由命令解释程序解释并执行输入的命令，完成指定的功能之后，控制权转回控制台或终端，此时用户又可输入下一条命令。
    - **脱机命令接口/批处理命令接口**：适用于批处理系统，它由一组作业控制命令组成。脱机用户不能直接干预作业的运行，而应事先用相应的作业控制命令写成一份作业操作说明书，连同作业一起提交给系统。系统调度到该作业时，由系统中的命令解释程序逐条解释执行作业说明书上的命令，从而间接地控制作业的运行。
2. **程序接口**：编程人员可以使用程序接口来请求操作系统服务。
    - 程序接口由一组系统调用（也称广义指令）组成，用户通过在程序中使用这些系统调用来请求操作系统为其提供服务，如使用各种外部设备、申请分配和回收内存及其他各种要求。
    - 当前最为流行的是图形用户界面（GUI），即图形接口。图形接口不是操作系统的一部分，但图形接口所调用的系统调用命令是操作系统的一部分。

### 1.2 操作系统发展历程

#### 1.2.1 手工操作阶段（无操作系统）

缺点：

- 用户独占全机，虽然不会出现因资源已被其他用户占用而等待的现象，但资源利用率低。
- CPU 等待手工操作，CPU 的利用不充分。

#### 1.2.2 批处理阶段（操作系统开始出现）

**单道批处理系统**

1. **自动性**：作业自动执行。
2. **顺序性**：作业按顺序执行。
3. **单道性**：内存中仅有一个作业在执行。

优点：减少了人工参与。

缺点：当程序进行 I/O 请求时，CPU 便处于等待状态，CPU 的利用率低。

**多道批处理系统**：

1. **多道性**：内存中同时存放多个作业。
2. **宏观并行**：总体上看内存中每个作业都在运行。
3. **微观串行**：每个作业轮流占用 CPU，交替执行，同一时刻只有一个作业在运行。

需解决 CPU 、存储器和外部设备的分配问题。

- 优点：资源利用率高，系统吞吐量大。
- 缺点：用户响应不及时，无交互性。

#### 1.2.3 分时操作系统

**分时技术**
- 把处理器的运行时间分成很短的时间片，按**时间片轮流**把处理器分配给各个联机作业使用。
- 若某个作业在分配给它的时间片内不能完成，则该作业暂时停止运行，把处理器让给其他作业使用，等待下次获取到 CPU 时间片时再继续运行。
- 由于计算机速度很快，时间片很小，作业运行轮转得也很快，因此用户感觉就像是自己独占一台计算机。

**分时系统**
1. **同时性**：多个用户通过终端同时共享一台计算机。
2. **交互性**：用户通过终端与计算机进行交互。
3. **独立性**：用户之间互不影响。
4. **及时性**：系统响应及时，用户等待时间短。

#### 1.2.4 实时操作系统

任务具有优先级，可以抢占 CPU 时间片优先执行。
- **硬实时系统**：在规定时间内必须执行。如：飞行器控制系统。
- **软实时系统**：尽量在规定时间内执行。如：订票系统。

#### 1.2.5 网络操作系统和分布式计算机系统

- **网络操作系统**：网络中多个计算机结合，实现资源的共享和计算机之间的通信。
- **分布式计算机系统**：多台计算机组成的系统。每台计算机地位平等、资源共享、并行工作、协同完成。
    - **分布性**
    - **并行性**

#### 1.2.6 个人计算机操作系统

目前最广泛使用。

- Windows
- Linux
- macOS
- Android
- iOS

### 1.3 操作系统运行环境

#### 1.3.1 处理器运行模式

**计算机程序分类**
- **内核程序**：执行**特权指令**的程序。特权指令如：I/O 指令、置中断指令、存取用于内存保护的寄存器、送程序状态字到程序状态字寄存器等的指令。
- **用户程序**：不能执行特权指令，防止破坏系统。

**CPU 运行模式**
- **核心态/内核态/管态**：可以执行特权指令。
    - 如时钟管理、中断处理、设备驱动等。
    - 如进程管理、存储器管理、设备管理等。
- **用户态/目态**：不能执行特权指令。

**内核功能**

1. **时钟管理**
    - 计时。提供用户时间。
    - 进程切换。时间片轮转。
2. **中断机制**
    - 键盘鼠标的输入、进程管理和调度、系统功能调用、设备驱动、文件访问等都需要中断。
    - 在内核态中进行中断现场的保存与恢复，转移控制权到相关处理程序，减少中断等待时间，提高 CPU 利用率。
3. **原语**：关闭中断，运行程序，打开中断。
    1. 处于操作系统的最底层。
    2. 原子性。作为一个整体，要么全都完成，要么全都不完成。
    3. 运行快速，调用频繁。
4. **系统控制的数据结构及处理**

<!-- ### 1.4

### 1.5

### 1.6 虚拟机

#### 1.6.1 虚拟机概念

虚拟机是一台逻辑计算机，是指利用特殊的虚拟化技术，通过隐藏特定计算平台的实际物理特性，为用户提供抽象的、统一的、模拟的计算环境。

#### 1.6.2 第一类虚拟机管理程序

- 像一个操作系统，是**唯一**一个运行在**最高特权级**的程序。
- 在裸机上运行并且具备多道程序功能。
- 向上层提供若干台虚拟机，这些虚拟机是裸机硬件的精确复制品。
- 每台虚拟机都与裸机相同，所以在不同的虚拟机上可以运行任何不同的操作系统。

虚拟机作为用户态的一一个进程运行，不允许执行敏感指令。然而，虚拟机上的操作系统认为
自已运行在内核态(实际上不是)，称为虚拟内核态。虚拟机中的用户进程认为自己运行在用户.
态(实际上确实是)。当虚拟机操作系统执行了- -条CPU处于内核态才允许执行的指令时，会陷
入虚拟机管理程序。在支持虚拟化的CPU.上,虚拟机管理程序检查这条指令是由虚拟机中的操作
系统执行的还是由用户程序执行的。如果是前者，虚拟机管理程序将安排这条指令功能的正确执
行。否则，虚拟机管理程序将模拟真实硬件面对用户态执行敏感指令时的行为。
在过去不支持虚拟化的CPU上，真实硬件不会直接执行虚拟机中的敏感指令，这些敏感指
令被转为对虚拟机管理程序的调用，由虚拟机管理程序模拟这些指令的功能。

{{< figure src="/imgs/操作系统/第一类虚拟机管理程序.png" caption="第一类虚拟机管理程序" >}}

#### 1.6.3 第二类虚拟机管理程序

- 依赖于操作系统，像一个普通的进程。如：VMware Workstation。
- 伪装成具有 CPU 和各种设备的完整计算机。
- 运行在底层硬件上的操作系统称为**宿主操作系统**，提供的虚拟机称为**客户操作系统**。
- 云主机一般通过这种虚拟机提供。

{{< figure src="/imgs/操作系统/第二类虚拟机管理程序.png" caption="第二类虚拟机管理程序" >}} -->

## 第2章 进程管理

### 2.1 进程与线程

#### 2.1.1 操作系统之进程的定义、特征、组成、组织

{{< figure src="/imgs/操作系统/281.png" >}}
{{< figure src="/imgs/操作系统/282.png" >}}
{{< figure src="/imgs/操作系统/283.png" >}}
{{< figure src="/imgs/操作系统/284.png" >}}
{{< figure src="/imgs/操作系统/285.png" >}}
{{< figure src="/imgs/操作系统/286.png" >}}
{{< figure src="/imgs/操作系统/287.png" >}}
{{< figure src="/imgs/操作系统/288.png" >}}
{{< figure src="/imgs/操作系统/289.png" >}}

#### 2.1.2 操作系统之进程的状态及转换

{{< figure src="/imgs/操作系统/290.png" >}}
{{< figure src="/imgs/操作系统/291.png" >}}
{{< figure src="/imgs/操作系统/292.png" >}}
{{< figure src="/imgs/操作系统/293.png" >}}
{{< figure src="/imgs/操作系统/294.png" >}}
{{< figure src="/imgs/操作系统/295.png" >}}
{{< figure src="/imgs/操作系统/296.png" >}}
{{< figure src="/imgs/操作系统/297.png" >}}

#### 2.1.3 操作系统之原语实现对进程的控制

{{< figure src="/imgs/操作系统/298.png" >}}
{{< figure src="/imgs/操作系统/299.png" >}}
{{< figure src="/imgs/操作系统/300.png" >}}
{{< figure src="/imgs/操作系统/301.png" >}}
{{< figure src="/imgs/操作系统/302.png" >}}
{{< figure src="/imgs/操作系统/303.png" >}}
{{< figure src="/imgs/操作系统/304.png" >}}
{{< figure src="/imgs/操作系统/305.png" >}}
{{< figure src="/imgs/操作系统/306.png" >}}
{{< figure src="/imgs/操作系统/307.png" >}}

#### 2.1.4 进程之间的通信

{{< figure src="/imgs/操作系统/308.png" >}}
{{< figure src="/imgs/操作系统/309.png" >}}
{{< figure src="/imgs/操作系统/310.png" >}}
{{< figure src="/imgs/操作系统/311.png" >}}
{{< figure src="/imgs/操作系统/312.png" >}}

#### 2.1.5 操作系统之线程概念与多线程模型

{{< figure src="/imgs/操作系统/313.png" >}}
{{< figure src="/imgs/操作系统/314.png" >}}
{{< figure src="/imgs/操作系统/315.png" >}}
{{< figure src="/imgs/操作系统/316.png" >}}
{{< figure src="/imgs/操作系统/317.png" >}}
{{< figure src="/imgs/操作系统/318.png" >}}
{{< figure src="/imgs/操作系统/319.png" >}}
{{< figure src="/imgs/操作系统/320.png" >}}
{{< figure src="/imgs/操作系统/321.png" >}}
{{< figure src="/imgs/操作系统/322.png" >}}
{{< figure src="/imgs/操作系统/323.png" >}}
{{< figure src="/imgs/操作系统/324.png" >}}

### 2.2 处理机的调度

#### 2.2.1 处理机调度的概念及层次

{{< figure src="/imgs/操作系统/325.png" >}}
{{< figure src="/imgs/操作系统/326.png" >}}
{{< figure src="/imgs/操作系统/327.png" >}}
{{< figure src="/imgs/操作系统/328.png" >}}
{{< figure src="/imgs/操作系统/329.png" >}}
{{< figure src="/imgs/操作系统/330.png" >}}
{{< figure src="/imgs/操作系统/331.png" >}}

#### 2.2.2 进程调度的时机、切换与过程、方式

{{< figure src="/imgs/操作系统/332.png" >}}
{{< figure src="/imgs/操作系统/333.png" >}}
{{< figure src="/imgs/操作系统/334.png" >}}
{{< figure src="/imgs/操作系统/335.png" >}}
{{< figure src="/imgs/操作系统/336.png" >}}
{{< figure src="/imgs/操作系统/337.png" >}}
{{< figure src="/imgs/操作系统/338.png" >}}

#### 2.2.3 调度算法的评价指标

{{< figure src="/imgs/操作系统/339.png" >}}
{{< figure src="/imgs/操作系统/340.png" >}}
{{< figure src="/imgs/操作系统/341.png" >}}
{{< figure src="/imgs/操作系统/342.png" >}}
{{< figure src="/imgs/操作系统/343.png" >}}
{{< figure src="/imgs/操作系统/344.png" >}}
{{< figure src="/imgs/操作系统/345.png" >}}

#### 2.2.4 作业/进程调度算法

{{< figure src="/imgs/操作系统/346.png" >}}
{{< figure src="/imgs/操作系统/347.png" >}}
{{< figure src="/imgs/操作系统/348.png" >}}
{{< figure src="/imgs/操作系统/349.png" >}}
{{< figure src="/imgs/操作系统/350.png" >}}
{{< figure src="/imgs/操作系统/351.png" >}}
{{< figure src="/imgs/操作系统/352.png" >}}
{{< figure src="/imgs/操作系统/353.png" >}}
{{< figure src="/imgs/操作系统/354.png" >}}
{{< figure src="/imgs/操作系统/355.png" >}}
{{< figure src="/imgs/操作系统/356.png" >}}
{{< figure src="/imgs/操作系统/357.png" >}}

#### 2.2.5 作业/进程调度算法（高级）

{{< figure src="/imgs/操作系统/358.png" >}}
{{< figure src="/imgs/操作系统/359.png" >}}
{{< figure src="/imgs/操作系统/360.png" >}}
{{< figure src="/imgs/操作系统/361.png" >}}
{{< figure src="/imgs/操作系统/362.png" >}}
{{< figure src="/imgs/操作系统/363.png" >}}
{{< figure src="/imgs/操作系统/364.png" >}}
{{< figure src="/imgs/操作系统/365.png" >}}
{{< figure src="/imgs/操作系统/366.png" >}}
{{< figure src="/imgs/操作系统/367.png" >}}
{{< figure src="/imgs/操作系统/368.png" >}}
{{< figure src="/imgs/操作系统/369.png" >}}
{{< figure src="/imgs/操作系统/370.png" >}}
{{< figure src="/imgs/操作系统/371.png" >}}
{{< figure src="/imgs/操作系统/372.png" >}}
{{< figure src="/imgs/操作系统/373.png" >}}
{{< figure src="/imgs/操作系统/374.png" >}}
{{< figure src="/imgs/操作系统/375.png" >}}
{{< figure src="/imgs/操作系统/376.png" >}}
{{< figure src="/imgs/操作系统/377.png" >}}

### 2.3 进程的同步与互斥

#### 2.3.1 进程的同步与互斥

{{< figure src="/imgs/操作系统/378.png" >}}
{{< figure src="/imgs/操作系统/379.png" >}}
{{< figure src="/imgs/操作系统/380.png" >}}

#### 2.3.2 实现临界区进程互斥的软件实现方法

{{< figure src="/imgs/操作系统/381.png" >}}
{{< figure src="/imgs/操作系统/382.png" >}}
{{< figure src="/imgs/操作系统/383.png" >}}
{{< figure src="/imgs/操作系统/384.png" >}}
{{< figure src="/imgs/操作系统/385.png" >}}
{{< figure src="/imgs/操作系统/386.png" >}}
{{< figure src="/imgs/操作系统/387.png" >}}

#### 2.3.3 实现临界区进程互斥的硬件实现方法

{{< figure src="/imgs/操作系统/388.png" >}}
{{< figure src="/imgs/操作系统/389.png" >}}
{{< figure src="/imgs/操作系统/390.png" >}}
{{< figure src="/imgs/操作系统/391.png" >}}

#### 2.3.4 信号量机制

{{< figure src="/imgs/操作系统/392.png" >}}
{{< figure src="/imgs/操作系统/393.png" >}}
{{< figure src="/imgs/操作系统/394.png" >}}
{{< figure src="/imgs/操作系统/395.png" >}}
{{< figure src="/imgs/操作系统/396.png" >}}
{{< figure src="/imgs/操作系统/397.png" >}}
{{< figure src="/imgs/操作系统/398.png" >}}
{{< figure src="/imgs/操作系统/399.png" >}}
{{< figure src="/imgs/操作系统/400.png" >}}
{{< figure src="/imgs/操作系统/401.png" >}}
{{< figure src="/imgs/操作系统/402.png" >}}
{{< figure src="/imgs/操作系统/403.png" >}}

#### 2.3.5 信号量机制实现进程的互斥、同步与前驱关系

{{< figure src="/imgs/操作系统/404.png" >}}
{{< figure src="/imgs/操作系统/405.png" >}}
{{< figure src="/imgs/操作系统/406.png" >}}
{{< figure src="/imgs/操作系统/407.png" >}}
{{< figure src="/imgs/操作系统/408.png" >}}

####  2.3.6 进程同步与互斥经典问题

{{< figure src="/imgs/操作系统/409.png" >}}
{{< figure src="/imgs/操作系统/410.png" >}}
{{< figure src="/imgs/操作系统/411.png" >}}
{{< figure src="/imgs/操作系统/412.png" >}}
{{< figure src="/imgs/操作系统/413.png" >}}
{{< figure src="/imgs/操作系统/414.png" >}}
{{< figure src="/imgs/操作系统/415.png" >}}
{{< figure src="/imgs/操作系统/416.png" >}}
{{< figure src="/imgs/操作系统/417.png" >}}
{{< figure src="/imgs/操作系统/418.png" >}}
{{< figure src="/imgs/操作系统/419.png" >}}
{{< figure src="/imgs/操作系统/420.png" >}}
{{< figure src="/imgs/操作系统/421.png" >}}
{{< figure src="/imgs/操作系统/422.png" >}}
{{< figure src="/imgs/操作系统/423.png" >}}
{{< figure src="/imgs/操作系统/424.png" >}}
{{< figure src="/imgs/操作系统/425.png" >}}
{{< figure src="/imgs/操作系统/426.png" >}}
{{< figure src="/imgs/操作系统/427.png" >}}
{{< figure src="/imgs/操作系统/428.png" >}}
{{< figure src="/imgs/操作系统/429.png" >}}
{{< figure src="/imgs/操作系统/430.png" >}}
{{< figure src="/imgs/操作系统/431.png" >}}
{{< figure src="/imgs/操作系统/432.png" >}}
{{< figure src="/imgs/操作系统/433.png" >}}
{{< figure src="/imgs/操作系统/434.png" >}}
{{< figure src="/imgs/操作系统/435.png" >}}
{{< figure src="/imgs/操作系统/436.png" >}}

#### 2.3.7 管程和java中实现管程的机制

{{< figure src="/imgs/操作系统/437.png" >}}
{{< figure src="/imgs/操作系统/438.png" >}}
{{< figure src="/imgs/操作系统/439.png" >}}
{{< figure src="/imgs/操作系统/440.png" >}}
{{< figure src="/imgs/操作系统/441.png" >}}
{{< figure src="/imgs/操作系统/442.png" >}}
{{< figure src="/imgs/操作系统/443.png" >}}
{{< figure src="/imgs/操作系统/444.png" >}}

### 2.4 死锁

#### 2.4.1 死锁详解

{{< figure src="/imgs/操作系统/445.png" >}}
{{< figure src="/imgs/操作系统/446.png" >}}
{{< figure src="/imgs/操作系统/447.png" >}}
{{< figure src="/imgs/操作系统/448.png" >}}
{{< figure src="/imgs/操作系统/449.png" >}}
{{< figure src="/imgs/操作系统/450.png" >}}
{{< figure src="/imgs/操作系统/451.png" >}}
{{< figure src="/imgs/操作系统/452.png" >}}
{{< figure src="/imgs/操作系统/453.png" >}}
{{< figure src="/imgs/操作系统/454.png" >}}
{{< figure src="/imgs/操作系统/455.png" >}}
{{< figure src="/imgs/操作系统/456.png" >}}
{{< figure src="/imgs/操作系统/457.png" >}}
{{< figure src="/imgs/操作系统/458.png" >}}
{{< figure src="/imgs/操作系统/459.png" >}}
{{< figure src="/imgs/操作系统/460.png" >}}
{{< figure src="/imgs/操作系统/461.png" >}}
{{< figure src="/imgs/操作系统/462.png" >}}
{{< figure src="/imgs/操作系统/463.png" >}}
{{< figure src="/imgs/操作系统/464.png" >}}
{{< figure src="/imgs/操作系统/465.png" >}}
{{< figure src="/imgs/操作系统/466.png" >}}
{{< figure src="/imgs/操作系统/467.png" >}}
{{< figure src="/imgs/操作系统/468.png" >}}
{{< figure src="/imgs/操作系统/469.png" >}}
{{< figure src="/imgs/操作系统/470.png" >}}
{{< figure src="/imgs/操作系统/471.png" >}}
{{< figure src="/imgs/操作系统/472.png" >}}
{{< figure src="/imgs/操作系统/473.png" >}}
{{< figure src="/imgs/操作系统/474.png" >}}
{{< figure src="/imgs/操作系统/475.png" >}}
{{< figure src="/imgs/操作系统/476.png" >}}
{{< figure src="/imgs/操作系统/477.png" >}}
{{< figure src="/imgs/操作系统/478.png" >}}
{{< figure src="/imgs/操作系统/479.png" >}}
{{< figure src="/imgs/操作系统/480.png" >}}
{{< figure src="/imgs/操作系统/481.png" >}}
{{< figure src="/imgs/操作系统/482.png" >}}
{{< figure src="/imgs/操作系统/483.png" >}}

## 第3章 内存管理

### 3.1 内存管理的概念

#### 3.1.1 什么是内存？

{{< figure src="/imgs/操作系统/226.png" >}}

**什么是内存？有何作用？**

{{< figure src="/imgs/操作系统/227.png" >}}

- 存储单元

{{< figure src="/imgs/操作系统/228.png" >}}

- 几个常用数量单位&内存地址

{{< figure src="/imgs/操作系统/229.png" >}}

**进程运行的基本原理**

- 指令的工作原理—操作码+若干参数（可能包含地址参数）

{{< figure src="/imgs/操作系统/230.png" >}}

{{< figure src="/imgs/操作系统/231.png" >}}

{{< figure src="/imgs/操作系统/232.png" >}}

{{< figure src="/imgs/操作系统/233.png" >}}

- 逻辑地址（相对地址）vs物理地址（绝对地址）

{{< figure src="/imgs/操作系统/234.png" >}}

- 从写程序到程序运行—编译、链接、装入

{{< figure src="/imgs/操作系统/235.png" >}}

- 装入模块装入内存

{{< figure src="/imgs/操作系统/236.png" >}}

{{< figure src="/imgs/操作系统/237.png" >}}

- 装入的三种方式

绝对装入

{{< figure src="/imgs/操作系统/238.png" >}}

静态重定位

{{< figure src="/imgs/操作系统/239.png" >}}

动态重定位

{{< figure src="/imgs/操作系统/240.png" >}}

{{< figure src="/imgs/操作系统/241.png" >}}

{{< figure src="/imgs/操作系统/242.png" >}}

- 链接的三种方式

静态链接

{{< figure src="/imgs/操作系统/243.png" >}}

装入时动态链接

{{< figure src="/imgs/操作系统/244.png" >}}

运行时动态链接

{{< figure src="/imgs/操作系统/245.png" >}}

#### 3.1.2 内存管理管些什么？

{{< figure src="/imgs/操作系统/246.png" >}}

**内存空间的分配与回收**

{{< figure src="/imgs/操作系统/247.png" >}}

**内存空间的扩展（实现虚拟性）**

{{< figure src="/imgs/操作系统/248.png" >}}

**地址转换**

{{< figure src="/imgs/操作系统/249.png" >}}

- 三种方式

{{< figure src="/imgs/操作系统/250.png" >}}

**内存保护**

{{< figure src="/imgs/操作系统/251.png" >}}

- 两种方式

{{< figure src="/imgs/操作系统/252.png" >}}

{{< figure src="/imgs/操作系统/253.png" >}}

#### 3.1.3 覆盖技术与交换技术的思想

{{< figure src="/imgs/操作系统/254.png" >}}

{{< figure src="/imgs/操作系统/255.png" >}}

**覆盖技术**

{{< figure src="/imgs/操作系统/256.png" >}}

{{< figure src="/imgs/操作系统/257.png" >}}

**交换技术**

{{< figure src="/imgs/操作系统/258.png" >}}

{{< figure src="/imgs/操作系统/259.png" >}}

{{< figure src="/imgs/操作系统/260.png" >}}

#### 3.1.4 内存的分配与回收

{{< figure src="/imgs/操作系统/261.png" >}}
{{< figure src="/imgs/操作系统/262.png" >}}
{{< figure src="/imgs/操作系统/263.png" >}}
{{< figure src="/imgs/操作系统/264.png" >}}
{{< figure src="/imgs/操作系统/265.png" >}}
{{< figure src="/imgs/操作系统/266.png" >}}
{{< figure src="/imgs/操作系统/267.png" >}}
{{< figure src="/imgs/操作系统/268.png" >}}
{{< figure src="/imgs/操作系统/269.png" >}}
{{< figure src="/imgs/操作系统/270.png" >}}
{{< figure src="/imgs/操作系统/271.png" >}}
{{< figure src="/imgs/操作系统/272.png" >}}
{{< figure src="/imgs/操作系统/273.png" >}}
{{< figure src="/imgs/操作系统/274.png" >}}
{{< figure src="/imgs/操作系统/275.png" >}}
{{< figure src="/imgs/操作系统/276.png" >}}
{{< figure src="/imgs/操作系统/277.png" >}}
{{< figure src="/imgs/操作系统/278.png" >}}
{{< figure src="/imgs/操作系统/279.png" >}}
{{< figure src="/imgs/操作系统/280.png" >}}

### 3.2 虚拟内存管理

#### 3.2.1 虚拟内存的基本概念

{{< figure src="/imgs/操作系统/173.png" >}}

{{< figure src="/imgs/操作系统/174.png" >}}

{{< figure src="/imgs/操作系统/175.png" >}}

**传统存储管理的特征、缺点**

{{< figure src="/imgs/操作系统/176.png" >}}

**局部性原理**

{{< figure src="/imgs/操作系统/177.png" >}}

**虚拟内存的定义和特征**

{{< figure src="/imgs/操作系统/178.png" >}}

{{< figure src="/imgs/操作系统/179.png" >}}

**如何实现虚拟内存技术**

{{< figure src="/imgs/操作系统/180.png" >}}

#### 3.2.2 请求分页管理方式

{{< figure src="/imgs/操作系统/181.png" >}}

**概念**

{{< figure src="/imgs/操作系统/182.png" >}}

**页表机制—请求页表与基本页表的区别**

{{< figure src="/imgs/操作系统/183.png" >}}

**缺页中断机构**

{{< figure src="/imgs/操作系统/184.png" >}}

{{< figure src="/imgs/操作系统/185.png" >}}

{{< figure src="/imgs/操作系统/186.png" >}}

{{< figure src="/imgs/操作系统/187.png" >}}

{{< figure src="/imgs/操作系统/188.png" >}}

**地址变换机构**

{{< figure src="/imgs/操作系统/189.png" >}}

{{< figure src="/imgs/操作系统/190.png" >}}

{{< figure src="/imgs/操作系统/191.png" >}}

{{< figure src="/imgs/操作系统/192.png" >}}

#### 3.2.3 页面置换算法

{{< figure src="/imgs/操作系统/193.png" >}}

{{< figure src="/imgs/操作系统/194.png" >}}

**最佳置换算法—OPT**

{{< figure src="/imgs/操作系统/195.png" >}}

{{< figure src="/imgs/操作系统/196.png" >}}

{{< figure src="/imgs/操作系统/197.png" >}}

**先进先出置换算法—FIFO**

{{< figure src="/imgs/操作系统/198.png" >}}

{{< figure src="/imgs/操作系统/199.png" >}}

**最近最久未使用置换算法—LRU**

{{< figure src="/imgs/操作系统/200.png" >}}

{{< figure src="/imgs/操作系统/201.png" >}}

{{< figure src="/imgs/操作系统/202.png" >}}

**时钟置换算法—CLOCK**

{{< figure src="/imgs/操作系统/203.png" >}}

{{< figure src="/imgs/操作系统/204.png" >}}

{{< figure src="/imgs/操作系统/205.png" >}}

**改造型时钟置换算法**

{{< figure src="/imgs/操作系统/206.png" >}}

{{< figure src="/imgs/操作系统/207.png" >}}

{{< figure src="/imgs/操作系统/208.png" >}}

{{< figure src="/imgs/操作系统/209.png" >}}

{{< figure src="/imgs/操作系统/210.png" >}}

{{< figure src="/imgs/操作系统/211.png" >}}

{{< figure src="/imgs/操作系统/212.png" >}}

{{< figure src="/imgs/操作系统/213.png" >}}

#### 3.2.4 页面分配策略

{{< figure src="/imgs/操作系统/214.png" >}}
{{< figure src="/imgs/操作系统/215.png" >}}

**驻留集**

{{< figure src="/imgs/操作系统/216.png" >}}

**页面分配、置换策略**

{{< figure src="/imgs/操作系统/217.png" >}}

- 固定分配局部置换、可变分配局部置换、可变分配全局置换

{{< figure src="/imgs/操作系统/218.png" >}}

{{< figure src="/imgs/操作系统/219.png" >}}

**何时调入页面？**

{{< figure src="/imgs/操作系统/220.png" >}}

**从何处调页？**

{{< figure src="/imgs/操作系统/221.png" >}}

{{< figure src="/imgs/操作系统/222.png" >}}

{{< figure src="/imgs/操作系统/223.png" >}}

**抖动（颠簸）现象**

{{< figure src="/imgs/操作系统/224.png" >}}

**工作集**

{{< figure src="/imgs/操作系统/225.png" >}}

## 第4章 文件管理

### 4.1 文件系统

#### 4.1.1 文件管理概念和功能

{{< figure src="/imgs/操作系统/81.png" >}}

{{< figure src="/imgs/操作系统/82.png" >}}

**文件的属性**

{{< figure src="/imgs/操作系统/83.png" >}}

**文件内部的数据如何组织？**

{{< figure src="/imgs/操作系统/84.png" >}}

{{< figure src="/imgs/操作系统/85.png" >}}

**文件之间如何组织？**

{{< figure src="/imgs/操作系统/86.png" >}}

**操作系统应该向上提供哪些功能？**

{{< figure src="/imgs/操作系统/87.png" >}}

{{< figure src="/imgs/操作系统/88.png" >}}

**从上往下看，文件应该如何存放在外存？**

{{< figure src="/imgs/操作系统/89.png" >}}

{{< figure src="/imgs/操作系统/90.png" >}}

**其他需要由操作系统实现的文件管理功能**

{{< figure src="/imgs/操作系统/91.png" >}}

#### 4.1.2 文件逻辑结构

{{< figure src="/imgs/操作系统/92.png" >}}

{{< figure src="/imgs/操作系统/93.png" >}}

**无结构文件**

{{< figure src="/imgs/操作系统/94.png" >}}

**有结构文件**

{{< figure src="/imgs/操作系统/95.png" >}}

{{< figure src="/imgs/操作系统/96.png" >}}

{{< figure src="/imgs/操作系统/97.png" >}}

- 有结构文件的逻辑结构

{{< figure src="/imgs/操作系统/98.png" >}}

- 顺序文件

{{< figure src="/imgs/操作系统/99.png" >}}

{{< figure src="/imgs/操作系统/100.png" >}}

- 索引文件

{{< figure src="/imgs/操作系统/101.png" >}}

- 索引顺序文件

{{< figure src="/imgs/操作系统/102.png" >}}

{{< figure src="/imgs/操作系统/103.png" >}}

- 多级索引顺序文件

{{< figure src="/imgs/操作系统/104.png" >}}

#### 4.1.3 文件目录结构

{{< figure src="/imgs/操作系统/105.png" >}}

{{< figure src="/imgs/操作系统/106.png" >}}

**文件控制块**

{{< figure src="/imgs/操作系统/107.png" >}}

{{< figure src="/imgs/操作系统/108.png" >}}

- 对目录的操作

{{< figure src="/imgs/操作系统/109.png" >}}

**单级目录结构**

{{< figure src="/imgs/操作系统/110.png" >}}

**两级目录结构**

{{< figure src="/imgs/操作系统/111.png" >}}

**多级目录结构(树形目录结构)**

{{< figure src="/imgs/操作系统/112.png" >}}

{{< figure src="/imgs/操作系统/113.png" >}}

{{< figure src="/imgs/操作系统/114.png" >}}

**无环图目录结构**

{{< figure src="/imgs/操作系统/115.png" >}}

{{< figure src="/imgs/操作系统/116.png" >}}

**索引节点(FCB的改进)瘦身**

{{< figure src="/imgs/操作系统/117.png" >}}

{{< figure src="/imgs/操作系统/118.png" >}}

#### 4.1.4 OS之文件的物理结构

{{< figure src="/imgs/操作系统/119.png" >}}

{{< figure src="/imgs/操作系统/120.png" >}}

**文件块、磁盘块**

{{< figure src="/imgs/操作系统/121.png" >}}

{{< figure src="/imgs/操作系统/122.png" >}}

**连续分配**

{{< figure src="/imgs/操作系统/123.png" >}}

{{< figure src="/imgs/操作系统/124.png" >}}

{{< figure src="/imgs/操作系统/125.png" >}}

{{< figure src="/imgs/操作系统/126.png" >}}

{{< figure src="/imgs/操作系统/127.png" >}}

**链接分配**

{{< figure src="/imgs/操作系统/128.png" >}}

- 隐式链接

{{< figure src="/imgs/操作系统/129.png" >}}

{{< figure src="/imgs/操作系统/130.png" >}}

- 显式链接

{{< figure src="/imgs/操作系统/131.png" >}}

{{< figure src="/imgs/操作系统/132.png" >}}

- 链接分配总结

{{< figure src="/imgs/操作系统/133.png" >}}

**索引分配**

{{< figure src="/imgs/操作系统/134.png" >}}

{{< figure src="/imgs/操作系统/135.png" >}}

{{< figure src="/imgs/操作系统/136.png" >}}

- 链接方案

{{< figure src="/imgs/操作系统/137.png" >}}

- 多层索引

{{< figure src="/imgs/操作系统/138.png" >}}

{{< figure src="/imgs/操作系统/139.png" >}}

- 混合索引

{{< figure src="/imgs/操作系统/140.png" >}}

- 索引分配总结

{{< figure src="/imgs/操作系统/141.png" >}}

**文件物理结构分配总结**

{{< figure src="/imgs/操作系统/142.png" >}}

### 4.2 磁盘组织与管理

#### 4.2.1 磁盘的结构

{{< figure src="/imgs/操作系统/143.png" >}}

**磁盘、磁道、扇区**

{{< figure src="/imgs/操作系统/144.png" >}}

**如何在磁盘中读/写数据**

{{< figure src="/imgs/操作系统/145.png" >}}

**盘面、柱面**

{{< figure src="/imgs/操作系统/146.png" >}}

**磁盘的分类**

- 磁头是否可移动

{{< figure src="/imgs/操作系统/147.png" >}}

- 盘片是否可更换

{{< figure src="/imgs/操作系统/148.png" >}}

#### 4.2.2 磁盘调度算法

{{< figure src="/imgs/操作系统/149.png" >}}

{{< figure src="/imgs/操作系统/150.png" >}}

**一次磁盘读/写操作需要的时间**

{{< figure src="/imgs/操作系统/151.png" >}}

{{< figure src="/imgs/操作系统/152.png" >}}

{{< figure src="/imgs/操作系统/153.png" >}}

**先来先服务(FCFS)**

{{< figure src="/imgs/操作系统/154.png" >}}

**最短寻找时间优先算法（SSTF）**

{{< figure src="/imgs/操作系统/155.png" >}}

**扫描算法（SCAN）**

{{< figure src="/imgs/操作系统/156.png" >}}

**LOOK算法**

{{< figure src="/imgs/操作系统/157.png" >}}

**循环扫描算法（S-SCAN）**

{{< figure src="/imgs/操作系统/158.png" >}}

**C-LOOK算法**

{{< figure src="/imgs/操作系统/159.png" >}}

#### 4.2.3 减少磁盘延迟时间的方法

{{< figure src="/imgs/操作系统/160.png" >}}

**一次磁盘读/写操作需要的时间**

{{< figure src="/imgs/操作系统/161.png" >}}

**交替编号**

{{< figure src="/imgs/操作系统/162.png" >}}

**磁盘地址结构的设计**

{{< figure src="/imgs/操作系统/163.png" >}}

{{< figure src="/imgs/操作系统/164.png" >}}

{{< figure src="/imgs/操作系统/165.png" >}}

**错位命名**

{{< figure src="/imgs/操作系统/166.png" >}}

{{< figure src="/imgs/操作系统/167.png" >}}

#### 4.2.4 磁盘管理

{{< figure src="/imgs/操作系统/168.png" >}}

**磁盘初始化**

{{< figure src="/imgs/操作系统/169.png" >}}

**引导块**

{{< figure src="/imgs/操作系统/170.png" >}}

{{< figure src="/imgs/操作系统/171.png" >}}

**坏块的管理**

{{< figure src="/imgs/操作系统/172.png" >}}

## 第5章 I/O 管理

### 5.1 I/O 管理概述

#### 5.1.1 I/O 设备

{{< figure src="/imgs/操作系统/42.png" >}}

**什么是 I/O 设备？**

{{< figure src="/imgs/操作系统/43.png" >}}

{{< figure src="/imgs/操作系统/44.png" >}}

**I/O 设备的分类**

- 使用特性分类

{{< figure src="/imgs/操作系统/45.png" >}}

- 传输速率分类

{{< figure src="/imgs/操作系统/46.png" >}}

- 信息交换单位分类

{{< figure src="/imgs/操作系统/47.png" >}}

#### 5.1.2 控制 I/O 设备的 I/O 控制器

{{< figure src="/imgs/操作系统/48.png" >}}

**I/O 设备的组成**

{{< figure src="/imgs/操作系统/49.png" >}}

- 机械部件

{{< figure src="/imgs/操作系统/50.png" >}}

- 电子部件：I/O 控制器的功能

{{< figure src="/imgs/操作系统/51.png" >}}

**I/O 控制器的组成**

{{< figure src="/imgs/操作系统/52.png" >}}

{{< figure src="/imgs/操作系统/53.png" >}}

**I/O 控制器的两种寄存器编址方式**

- 内存映像
- 独立编址

{{< figure src="/imgs/操作系统/54.png" >}}

#### 5.1.3 控制 I/O 设备的方式

{{< figure src="/imgs/操作系统/55.png" >}}

- 程序直接控制

{{< figure src="/imgs/操作系统/56.png" >}}

{{< figure src="/imgs/操作系统/57.png" >}}

{{< figure src="/imgs/操作系统/58.png" >}}

- 中断驱动

{{< figure src="/imgs/操作系统/59.png" >}}

{{< figure src="/imgs/操作系统/60.png" >}}

- DMA

{{< figure src="/imgs/操作系统/61.png" >}}

{{< figure src="/imgs/操作系统/62.png" >}}

{{< figure src="/imgs/操作系统/63.png" >}}

- 通道控制

{{< figure src="/imgs/操作系统/64.png" >}}

{{< figure src="/imgs/操作系统/65.png" >}}

**四种方式比较**

{{< figure src="/imgs/操作系统/66.png" >}}

#### 5.1.4 I/O 软件的层次结构

{{< figure src="/imgs/操作系统/67.png" >}}

- 用户层软件

{{< figure src="/imgs/操作系统/68.png" >}}

- 设备独立性软件

{{< figure src="/imgs/操作系统/69.png" >}}

{{< figure src="/imgs/操作系统/70.png" >}}

{{< figure src="/imgs/操作系统/71.png" >}}

逻辑设备表（LUT）

{{< figure src="/imgs/操作系统/72.png" >}}

**为什么不同的设备需要不同的驱动程序？**

{{< figure src="/imgs/操作系统/73.png" >}}

{{< figure src="/imgs/操作系统/74.png" >}}

{{< figure src="/imgs/操作系统/75.png" >}}

{{< figure src="/imgs/操作系统/76.png" >}}

- 设备驱动程序

{{< figure src="/imgs/操作系统/77.png" >}}

- 中断处理程序

{{< figure src="/imgs/操作系统/78.png" >}}

{{< figure src="/imgs/操作系统/79.png" >}}

**总结**

{{< figure src="/imgs/操作系统/80.png" >}}

### 5.2 I/O 核心子系统

#### 5.2.1 内核的 I/O 核心子系统及功能

{{< figure src="/imgs/操作系统/IO核心子系统.png" caption="I/O核心子系统" >}}

{{< figure src="/imgs/操作系统/假脱机技术.png" caption="假脱机技术" >}}

{{< figure src="/imgs/操作系统/IO调度.png" caption="I/O调度" >}}

{{< figure src="/imgs/操作系统/设备保护.png" caption="设备保护" >}}

#### 5.2.2 I/O 设备假脱机技术（SPOOLing）

{{< figure src="/imgs/操作系统/1.png" caption="假脱机技术" >}}

**脱机技术**
- 手工操作阶段：主机直接从 I/O 设备获得数据，由于设备速度慢，主机速度很快。人机速度矛盾明显，主机要浪费很多时间来等待设备。
- 批处理阶段引入了脱机输入/输出技术(用磁带完成)：引入脱机技术后，缓解了CPU与慢速I/0设备的速度矛盾。另方面，即使CPU在忙碌，也可以提前将数据输入到磁带:即使慢速的输出设备正在忙碌，也可以提前将数据输出到磁带。

**假脱机技术**

- 输入井和输出井

{{< figure src="/imgs/操作系统/2.png" >}}

- 输入进程与输出进程

{{< figure src="/imgs/操作系统/3.png" >}}

- 输入输出缓冲区

{{< figure src="/imgs/操作系统/4.png" >}}

**共享打印机原理分析**

{{< figure src="/imgs/操作系统/5.png" >}}

{{< figure src="/imgs/操作系统/6.png" >}}

{{< figure src="/imgs/操作系统/7.png" >}}

#### 5.2.3 I/O 设备的分配与回收

{{< figure src="/imgs/操作系统/8.png" >}}

**设备分配时应该考虑的因素**

- 设备的固有属性

{{< figure src="/imgs/操作系统/9.png" >}}

- 设备的分配算法

{{< figure src="/imgs/操作系统/10.png" >}}

- 设备分配中的安全性

{{< figure src="/imgs/操作系统/11.png" >}}

**静态分配与动态分配**
- 静态分配：进程运行前为其分配全部所需资源，运行结束后归还资源.破坏了“请求和保持”条件，不会发生死锁
- 动态分配：进程运行过程中动态申请设备资源

**设备、控制器、通道之间的关系**

{{< figure src="/imgs/操作系统/12.png" >}}

**设备分配管理中的数据结构**

- 设备控制表（DCT）

{{< figure src="/imgs/操作系统/13.png" >}}

- 控制器控制表（COCT）

{{< figure src="/imgs/操作系统/14.png" >}}

- 通道控制表（CHCT）

{{< figure src="/imgs/操作系统/15.png" >}}

- 系统设备表（SDT）

{{< figure src="/imgs/操作系统/16.png" >}}

**设备分配的步骤**

{{< figure src="/imgs/操作系统/17.png" >}}

{{< figure src="/imgs/操作系统/18.png" >}}

{{< figure src="/imgs/操作系统/19.png" >}}

{{< figure src="/imgs/操作系统/20.png" >}}

**设备分配的改进步骤**

{{< figure src="/imgs/操作系统/21.png" >}}

{{< figure src="/imgs/操作系统/22.png" >}}

{{< figure src="/imgs/操作系统/23.png" >}}

#### 5.2.4 缓冲区管理

{{< figure src="/imgs/操作系统/24.png" >}}

**缓冲区**

{{< figure src="/imgs/操作系统/25.png" >}}

{{< figure src="/imgs/操作系统/26.png" >}}

**单缓冲区**

{{< figure src="/imgs/操作系统/27.png" >}}

{{< figure src="/imgs/操作系统/28.png" >}}

{{< figure src="/imgs/操作系统/29.png" >}}

{{< figure src="/imgs/操作系统/30.png" >}}

{{< figure src="/imgs/操作系统/31.png" >}}

**双缓冲区**

{{< figure src="/imgs/操作系统/32.png" >}}

{{< figure src="/imgs/操作系统/33.png" >}}

**单缓冲和双缓冲通信时的区别**

{{< figure src="/imgs/操作系统/34.png" >}}

{{< figure src="/imgs/操作系统/35.png" >}}

**循环缓冲区**

{{< figure src="/imgs/操作系统/36.png" >}}

**缓冲池**

{{< figure src="/imgs/操作系统/37.png" >}}

{{< figure src="/imgs/操作系统/38.png" >}}

{{< figure src="/imgs/操作系统/39.png" >}}

{{< figure src="/imgs/操作系统/40.png" >}}

{{< figure src="/imgs/操作系统/41.png" >}}

