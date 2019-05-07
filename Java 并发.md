## [Java 并发](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91) 

### 1. 线程状态转换

新建、可运行、阻塞、无限期等待、限期等待、死亡

### [2. 使用线程](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e4%ba%8c%e3%80%81%e4%bd%bf%e7%94%a8%e7%ba%bf%e7%a8%8b)

有三种使用线程的方法：

- 实现 Runnable 接口；

- 实现 Callable 接口；

  与 Runnable 相比，Callable 可以有返回值，返回值通过 FutureTask 进行封装。

- 继承 Thread 类。

### [3. 基础线程机制](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e4%b8%89%e3%80%81%e5%9f%ba%e7%a1%80%e7%ba%bf%e7%a8%8b%e6%9c%ba%e5%88%b6)

**Executor**

Executor 管理多个异步任务的执行，而无需程序员显式地管理线程的生命周期。这里的异步是指多个任务的执行互不干扰，不需要进行同步操作。

主要有三种 Executor：

- CachedThreadPool：一个任务创建一个线程；
- FixedThreadPool：所有任务只能使用固定大小的线程；
- SingleThreadExecutor：相当于大小为 1 的 FixedThreadPool。

**Daemon守护线程**

当所有非守护线程结束时，程序也就终止，同时会杀死所有守护线程。使用 setDaemon() 方法将一个线程设置为守护线程。

**Sleep()**

Thread.sleep(millisec) 方法会休眠当前正在执行的线程，millisec 单位为毫秒。

sleep() 可能会抛出 InterruptedException，因为异常不能跨线程传播回 main() 中，因此必须在本地进行处理。线程中抛出的其它异常也同样需要在本地进行处理。

**Yield()**

对静态方法 Thread.yield() 的调用声明了当前线程已经完成了生命周期中最重要的部分，可以切换给其它线程来执行。该方法只是对线程调度器的一个建议，而且也只是建议具有相同优先级的其它线程可以运行。

### [4. 中断](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e5%9b%9b%e3%80%81%e4%b8%ad%e6%96%ad)

**InterruptedException**

通过调用一个线程的 interrupt() 来中断该线程，如果该线程处于阻塞、限期等待或者无限期等待状态，那么就会抛出 InterruptedException，从而提前结束该线程。但是不能中断 I/O 阻塞和 synchronized 锁阻塞。

**interrupted()**

如果一个线程的 run() 方法执行一个无限循环，并且没有执行 sleep() 等会抛出 InterruptedException 的操作，那么调用线程的 interrupt() 方法就无法使线程提前结束。

但是调用 interrupt() 方法会设置线程的中断标记，此时调用 interrupted() 方法会返回 true。因此可以在循环体中使用 interrupted() 方法来判断线程是否处于中断状态，从而提前结束线程。

**Executor 的中断操作**

调用 Executor 的 shutdown() 方法会等待线程都执行完毕之后再关闭，但是如果调用的是 shutdownNow() 方法，则相当于调用每个线程的 interrupt() 方法。

如果只想中断 Executor 中的一个线程，可以通过使用 submit() 方法来提交一个线程，它会返回一个 Future<?> 对象，通过调用该对象的 cancel(true) 方法就可以中断线程。

### [5. 互斥同步](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e4%ba%94%e3%80%81%e4%ba%92%e6%96%a5%e5%90%8c%e6%ad%a5)

Java 提供了两种锁机制来控制多个线程对共享资源的互斥访问，第一个是 JVM 实现的 synchronized，而另一个是 JDK 实现的 ReentrantLock。

**synchronized**

1. 同步一个代码块(锁住this)

   它只作用于同一个对象，如果调用两个对象上的同步代码块，就不会进行同步。

2. 同步一个非静态方法（相当于锁住this）

   它和同步代码块一样，作用于同一个对象。

3. 同步一个类

   作用于整个类，也就是说两个线程调用同一个类的不同对象上的这种同步语句，也会进行同步。

4. 同步一个静态方法

   作用于整个类

**ReentrantLock（Lock）**

ReentrantLock 是 java.util.concurrent（J.U.C）包中的锁。

**比较**

1. 锁的实现

   synchronized 是 JVM 实现的，而 ReentrantLock 是 JDK 实现的。

2. 性能

   新版本 Java 对 synchronized 进行了很多优化，例如自旋锁等，synchronized 与 ReentrantLock 大致相同。

3. 等待可中断

   当持有锁的线程长期不释放锁的时候，正在等待的线程可以选择放弃等待，改为处理其他事情。

   ReentrantLock 可中断，而 synchronized 不行。

4. 公平锁

   公平锁是指多个线程在等待同一个锁时，必须按照申请锁的时间顺序来依次获得锁。

   synchronized 中的锁是非公平的，ReentrantLock 默认情况下也是非公平的，但是也可以是公平的。

5. 锁绑定多个条件

   一个 ReentrantLock 可以同时绑定多个 Condition 对象。

**使用选择**

除非需要使用 ReentrantLock 的高级功能，否则优先使用 synchronized。这是因为 synchronized 是 JVM 实现的一种锁机制，JVM 原生地支持它，而 ReentrantLock 不是所有的 JDK 版本都支持。并且使用 synchronized 不用担心没有释放锁而导致死锁问题，因为 JVM 会确保锁的释放。

### [6. 线程之间的协作](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e5%85%ad%e3%80%81%e7%ba%bf%e7%a8%8b%e4%b9%8b%e9%97%b4%e7%9a%84%e5%8d%8f%e4%bd%9c)

当多个线程可以一起工作去解决某个问题时，如果某些部分必须在其它部分之前完成，那么就需要对线程进行协调。

**join()**

在线程中调用另一个线程的 join() 方法，会将当前线程挂起，而不是忙等待，直到目标线程结束。

**wait() notify() notifyAll()**

调用 wait() 使得线程等待某个条件满足，线程在等待时会被挂起，当其他线程的运行使得这个条件满足时，其它线程会调用 notify() 或者 notifyAll() 来唤醒挂起的线程。

它们都属于 Object 的一部分，而不属于 Thread。

只能用在同步方法或者同步控制块中使用，否则会在运行时抛出 IllegalMonitorStateException。

使用 wait() 挂起期间，线程会释放锁。这是因为，如果没有释放锁，那么其它线程就无法进入对象的同步方法或者同步控制块中，那么就无法执行 notify() 或者 notifyAll() 来唤醒挂起的线程，造成死锁。

**wait() 和 sleep() 的区别**

- wait() 是 Object 的方法，而 sleep() 是 Thread 的静态方法；
- wait() 会释放锁，sleep() 不会。

**await() signal() signalAll()**

java.util.concurrent 类库中提供了 Condition 类来实现线程之间的协调，可以在 Condition 上调用 await() 方法使线程等待，其它线程调用 signal() 或 signalAll() 方法唤醒等待的线程。

相比于 wait() 这种等待方式，await() 可以指定等待的条件，因此更加灵活。

使用 Lock 来获取一个 Condition 对象。

### [7. J.U.C - AQS](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e4%b8%83%e3%80%81juc-aqs)

java.util.concurrent（J.U.C）大大提高了并发性能，AQS 被认为是 J.U.C 的核心。

**CountDownLatch**

用来控制一个线程等待多个线程。

维护了一个计数器 cnt，每次调用 countDown() 方法会让计数器的值减 1，减到 0 的时候，那些因为调用 await() 方法而在等待的线程就会被唤醒。

**CyclicBarrier**

用来控制多个线程互相等待，只有当多个线程都到达时，这些线程才会继续执行。

和 CountdownLatch 相似，都是通过维护计数器来实现的。线程执行 await() 方法之后计数器会减 1，并进行等待，直到计数器为 0，所有调用 await() 方法而在等待的线程才能继续执行。

CyclicBarrier 和 CountdownLatch 的一个区别是，CyclicBarrier 的计数器通过调用 reset() 方法可以循环使用，所以它才叫做循环屏障。

CyclicBarrier 有两个构造函数，其中 parties 指示计数器的初始值，barrierAction 在所有线程都到达屏障的时候会执行一次。

**Semaphore**

Semaphore 类似于操作系统中的信号量，可以控制对互斥资源的访问线程数。

### 8. [J.U.C - 其它组件](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e5%85%ab%e3%80%81juc-%e5%85%b6%e5%ae%83%e7%bb%84%e4%bb%b6)

**FutureTask**

在介绍 Callable 时我们知道它可以有返回值，返回值通过 Future 进行封装。FutureTask 实现了 RunnableFuture 接口，该接口继承自 Runnable 和 Future 接口，这使得 FutureTask 既可以当做一个任务执行，也可以有返回值。

FutureTask 可用于异步获取执行结果或取消执行任务的场景。当一个计算任务需要执行很长时间，那么就可以用 FutureTask 来封装这个任务，主线程在完成自己的任务之后再去获取结果。

**BlockingQueue**

java.util.concurrent.BlockingQueue 接口有以下阻塞队列的实现：

- **FIFO 队列** ：LinkedBlockingQueue、ArrayBlockingQueue（固定长度）
- **优先级队列** ：PriorityBlockingQueue

提供了阻塞的 take() 和 put() 方法：如果队列为空 take() 将阻塞，直到队列中有内容；如果队列为满 put() 将阻塞，直到队列有空闲位置。使用 BlockingQueue 可以实现生产者消费者问题。

**ForkJoin**

主要用于并行计算中，和 MapReduce 原理类似，都是把大的计算任务拆分成多个小任务并行计算。

### 9. [线程不安全示例](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e4%b9%9d%e3%80%81%e7%ba%bf%e7%a8%8b%e4%b8%8d%e5%ae%89%e5%85%a8%e7%a4%ba%e4%be%8b)

如果多个线程对同一个共享数据进行访问而不采取同步操作的话，那么操作的结果是不一致的。

以下代码演示了 1000 个线程同时对 cnt 执行自增操作，操作结束之后它的值有可能小于 1000。

### 10. [Java 内存模型](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e5%8d%81%e3%80%81java-%e5%86%85%e5%ad%98%e6%a8%a1%e5%9e%8b)

Java 内存模型试图屏蔽各种硬件和操作系统的内存访问差异，以实现让 Java 程序在各种平台下都能达到一致的内存访问效果。

**主内存与工作内存**

处理器上的寄存器的读写的速度比内存快几个数量级，为了解决这种速度矛盾，在它们之间加入了高速缓存。

加入高速缓存带来了一个新的问题：缓存一致性。如果多个缓存共享同一块主内存区域，那么多个缓存的数据可能会不一致，需要一些协议来解决这个问题。

所有的变量都存储在主内存中，每个线程还有自己的工作内存，工作内存存储在高速缓存或者寄存器中，保存了该线程使用的变量的主内存副本拷贝。

线程只能直接操作工作内存中的变量，不同线程之间的变量值传递需要通过主内存来完成。

**内存间交互操作**

![](https://gitee.com/CyC2018/CS-Notes/raw/master/docs/pics/b6a7e8af-91bf-44b2-8874-ccc6d9d52afc.jpg)

- read：把一个变量的值从主内存传输到工作内存中
- load：在 read 之后执行，把 read 得到的值放入工作内存的变量副本中
- use：把工作内存中一个变量的值传递给执行引擎
- assign：把一个从执行引擎接收到的值赋给工作内存的变量
- store：把工作内存的一个变量的值传送到主内存中
- write：在 store 之后执行，把 store 得到的值放入主内存的变量中
- lock：作用于主内存的变量
- unlock

**内存模型三大特性**

1. 原子性

   volatile修饰的变量操作不能保证原子性

2. 可见性

   主要有三种实现可见性的方式：

   - volatile
   - synchronized，对一个变量执行 unlock 操作之前，必须把变量值同步回主内存。
   - final，被 final 关键字修饰的字段在构造器中一旦初始化完成，并且没有发生 this 逃逸（其它线程通过 this 引用访问到初始化了一半的对象），那么其它线程就能看见 final 字段的值。

3. 有序性

   有序性是指：在本线程内观察，所有操作都是有序的。在一个线程观察另一个线程，所有操作都是无序的，无序是因为发生了指令重排序。在 Java 内存模型中，允许编译器和处理器对指令进行重排序，重排序过程不会影响到单线程程序的执行，却会影响到多线程并发执行的正确性。

   volatile 关键字通过添加内存屏障的方式来禁止指令重排，即重排序时不能把后面的指令放到内存屏障之前。

   也可以通过 synchronized 来保证有序性，它保证每个时刻只有一个线程执行同步代码，相当于是让线程顺序执行同步代码。

**先行发生原则HappenBefore**

1. 单一线程原则

   在一个线程内，在程序前面的操作先行发生于后面的操作。

2. 管程锁定原则

   一个 unlock 操作先行发生于后面对同一个锁的 lock 操作。

3. volatile变量原则

   对一个 volatile 变量的写操作先行发生于后面对这个变量的读操作。

4. 线程启动原则

   Thread 对象的 start() 方法调用先行发生于此线程的每一个动作。

5. 线程加入原则

   Thread 对象的结束先行发生于 join() 方法返回。

6. 线程中断原则

   对线程 interrupt() 方法的调用先行发生于被中断线程的代码检测到中断事件的发生，可以通过 interrupted() 方法检测到是否有中断发生。

7. 对象终结原则

   一个对象的初始化完成（构造函数执行结束）先行发生于它的 finalize() 方法的开始。

8. 传递性

   如果操作 A 先行发生于操作 B，操作 B 先行发生于操作 C，那么操作 A 先行发生于操作 C。

### 11. [线程安全](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e5%8d%81%e4%b8%80%e3%80%81%e7%ba%bf%e7%a8%8b%e5%ae%89%e5%85%a8)

多个线程不管以何种方式访问某个类，并且在主调代码中不需要进行同步，都能表现正确的行为。线程安全有以下几种实现方式：

**不可变**

不可变的类型：

- final 关键字修饰的基本数据类型
- String
- 枚举类型
- Number 部分子类，如 Long 和 Double 等数值包装类型，BigInteger 和 BigDecimal 等大数据类型。但同为 Number 的原子类 AtomicInteger 和 AtomicLong 则是可变的。
- 对于集合类型，可以使用 Collections.unmodifiableXXX() 方法来获取一个不可变的集合。

**互斥同步(悲观锁)**

synchronized 和 ReentrantLock。

**非阻塞同步(乐观锁)**

- CAS
- 各种原子类，如AtomicInteger
- ABA问题：versionId、J.U.C 包提供了一个带有标记的原子引用类 AtomicStampedReference等

**无同步方案**

要保证线程安全，并不是一定就要进行同步。如果一个方法本来就不涉及共享数据，那它自然就无须任何同步措施去保证正确性。

- 栈封闭

  多个线程访问同一个方法的局部变量时，不会出现线程安全问题，因为局部变量存储在虚拟机栈中，属于线程私有的。

- 线程本地存储（Thread Local Storage）

  如果一段代码中所需要的数据必须与其他代码共享，那就看看这些共享数据的代码是否能保证在同一个线程中执行。如果能保证，我们就可以把共享数据的可见范围限制在同一个线程之内，这样，无须同步也能保证线程之间不出现数据争用的问题。

  可以使用 java.lang.ThreadLocal 类来实现线程本地存储功能。

  ThreadLocal 从理论上讲并不是用来解决多线程并发问题的，因为根本不存在多线程竞争。

  在一些场景 (尤其是使用线程池) 下，由于 ThreadLocal.ThreadLocalMap 的底层数据结构导致 ThreadLocal 有内存泄漏的情况，应该尽可能在每次使用 ThreadLocal 后手动调用 remove()，以避免出现 ThreadLocal 经典的内存泄漏甚至是造成自身业务混乱的风险。

- 可重入代码（Reentrant Code）

  这种代码也叫做纯代码（Pure Code），可以在代码执行的任何时刻中断它，转而去执行另外一段代码（包括递归调用它本身），而在控制权返回后，原来的程序不会出现任何错误。

  可重入代码有一些共同的特征，例如不依赖存储在堆上的数据和公用的系统资源、用到的状态量都由参数中传入、不调用非可重入的方法等。

### 12. [锁优化](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e5%8d%81%e4%ba%8c%e3%80%81%e9%94%81%e4%bc%98%e5%8c%96)

锁的四种状态：无锁状态、偏向锁状态、轻量级锁状态、重量级锁状态

- 自旋锁

  自旋锁的思想是让一个线程在请求一个共享数据的锁时执行忙循环（自旋）一段时间，如果在这段时间内能获得锁，就可以避免进入阻塞状态。

  自旋锁虽然能避免进入阻塞状态从而减少开销，但是它需要进行忙循环操作占用 CPU 时间，它只适用于共享数据的锁定状态很短的场景。

- 锁消除

  锁消除是指对于被检测出不可能存在竞争的共享数据的锁进行消除。

- 锁粗化

  如果一系列的连续操作都对同一个对象反复加锁和解锁，频繁的加锁操作就会导致性能损耗。

  上一节的示例代码中连续的 append() 方法就属于这类情况。如果虚拟机探测到由这样的一串零碎的操作都对同一个对象加锁，将会把加锁的范围扩展（粗化）到整个操作序列的外部。对于上一节的示例代码就是扩展到第一个 append() 操作之前直至最后一个 append() 操作之后，这样只需要加锁一次就可以了。

- 偏向锁

  偏向锁的思想是偏向于让第一个获取锁对象的线程，这个线程在之后获取该锁就不再需要进行同步操作，甚至连 CAS 操作也不再需要。

- 轻量级锁

  轻量级锁是相对于传统的重量级锁而言，它使用 CAS 操作来避免重量级锁使用互斥量的开销。对于绝大部分的锁，在整个同步周期内都是不存在竞争的，因此也就不需要都使用互斥量进行同步，可以先采用 CAS 操作进行同步，如果 CAS 失败了再改用互斥量进行同步。

  如果 CAS 操作失败了，虚拟机首先会检查对象的 Mark Word 是否指向当前线程的虚拟机栈，如果是的话说明当前线程已经拥有了这个锁对象，那就可以直接进入同步块继续执行，否则说明这个锁对象已经被其他线程线程抢占了。如果有两条以上的线程争用同一个锁，那轻量级锁就不再有效，要膨胀为重量级锁。

### 13. [多线程开发良好的实践](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%B9%B6%E5%8F%91?id=%e5%8d%81%e4%b8%89%e3%80%81%e5%a4%9a%e7%ba%bf%e7%a8%8b%e5%bc%80%e5%8f%91%e8%89%af%e5%a5%bd%e7%9a%84%e5%ae%9e%e8%b7%b5)

- 给线程起个有意义的名字，这样可以方便找 Bug。
- 缩小同步范围，从而减少锁争用。例如对于 synchronized，应该尽量使用同步块而不是同步方法。
- 多用同步工具少用 wait() 和 notify()。首先，CountDownLatch, CyclicBarrier, Semaphore 和 Exchanger 这些同步类简化了编码操作，而用 wait() 和 notify() 很难实现复杂控制流；其次，这些同步类是由最好的企业编写和维护，在后续的 JDK 中还会不断优化和完善。
- 使用 BlockingQueue 实现生产者消费者问题。
- 多用并发集合少用同步集合，例如应该使用 ConcurrentHashMap 而不是 Hashtable。
- 使用本地变量和不可变类来保证线程安全。
- 使用线程池而不是直接创建线程，这是因为创建线程代价很高，线程池可以有效地利用有限的线程来启动任务。

### 14. 不熟悉

1. Executors线程池的几种类型（容易产生OOM错误，排队队列用的是LinkedBlockedPool）

   - CachedThreadPool：一个任务创建一个线程；

     ```java
     new ThreadPoolExecutor(0, Integer.MAX_VALUE, 60L, TimeUnit.SECONDS,new SynchronousQueue<Runnable>());
     ```

   - FixedThreadPool：所有任务只能使用固定大小的线程；

     ```java
     new ThreadPoolExecutor(nThreads, nThreads,0L, TimeUnit.MILLISECONDS,new LinkedBlockingQueue<Runnable>());
     ```

   - SingleThreadExecutor：相当于大小为 1 的 FixedThreadPool。

     ```java
     new ThreadPoolExecutor(1, 1,0L, TimeUnit.MILLISECONDS,new LinkedBlockingQueue<Runnable>())
     ```

   - ScheduledThreadPool：固定长度的线程池，可以以延迟或定时的方式来执行任务

     和CachedThreadPool一样，多了定时的功能

2. ThreadPoolExecutor创建线程池，参数

   ```java
   // Java线程池的完整构造函数
   public ThreadPoolExecutor(
   int corePoolSize, // 线程池长期维持的线程数，即使线程处于Idle状态，也不会回收。
   int maximumPoolSize, // 线程数的上限
   long keepAliveTime, TimeUnit unit, // 超过corePoolSize的线程的idle时长，
                                        // 超过这个时间，多余的线程会被回收。
   BlockingQueue<Runnable> workQueue, // 任务的排队队列
   ThreadFactory threadFactory, // 新线程的产生方式
   RejectedExecutionHandler handler) // 拒绝策略
   ```

3. 三种提交任务的方式

   |               提交方式               |                是否关心返回结果                 |
   | :----------------------------------: | :---------------------------------------------: |
   | `Future<T> submit(Callable<T> task)` |                       是                        |
   |   `void execute(Runnable command)`   |                       否                        |
   |  `Future<?> submit(Runnable task)`   | 否，虽然返回Future，但是其get()方法总是返回null |

4. 拒绝任务的行为

   ![](http://wx3.sinaimg.cn/large/005DF9Qily1g2g1h85q9dj30x6086aal.jpg)

   |      拒绝策略       |                        拒绝行为                        |
   | :-----------------: | :----------------------------------------------------: |
   |     AbortPolicy     |             抛出RejectedExecutionException             |
   |    DiscardPolicy    |                  什么也不做，直接忽略                  |
   | DiscardOldestPolicy | 丢弃执行队列中最老的任务，尝试为当前提交的任务腾出位置 |
   |  CallerRunsPolicy   |              直接由提交任务者执行这个任务              |

5. 锁升级

   ![](http://ww1.sinaimg.cn/mw690/005DF9Qily1g2ckqiiqtlj30nq0d53yu.jpg)

6. 死锁

   - 互斥条件：进程对所分配到的资源不允许其他进程进行访问，若其他进程访问该资源，只能等待，直至占有该资源的进程使用完成后释放该资源
   - 请求和保持资源条件：进程获得一定的资源之后，又对其他资源发出请求，但是该资源可能被其他进程占有，此事请求阻塞，但又对自己获得的资源保持不放
   - 不可剥夺条件：是指进程已获得的资源，在未完成使用之前，不可被剥夺，只能在使用完后自己释放
   - 环路等待条件：是指进程发生死锁后，若干进程之间形成一种头尾相接的循环等待资源关系

   预防死锁：破坏上面四个条件

7. ThreadLocal

   - 概念，作用：线程本地存储，可能用来避免并发问题。保证变量只能有一个线程访问。
   - 原理：每个Thread对象都有唯一 一个ThreadLocalMap。存储了ThreadLocal对象和值的映射。一个ThreadLocalMap可以有多个ThreadLocal对象与值的映射。