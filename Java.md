# Java

## 一、 基础

### 1. 重载和重写的区别

重载：发生在一个类中，方法的名字相同，参数的数量 or 类型 or 顺序不一样，方法的返回值和访问修饰符可以不一样，发生在编译期间

重写：发生在父子类中，方法的名字和参数列表都一样，发生在运行期间。返回值的类型范围小于等于父类、抛出异常的类型范围小于等于父类、访问修饰符范围大于等于父类。如果父类方法的访问修饰符为private，则子类不能重写该方法

### 2. String 为什么是不可变的？String 和 StringBuffer、StringBuilder 的区别是什么？ 

**可变性**

String 类中使用 final 关键字字符数组保存字符串，`private final char value[]` ，所以 String对象是不可变的 

而StringBuildr和StringBuffer的字符数组没有用final关键字修饰，`char[] value` 

**线程安全性**

String和StringBuffer是线程安全的，StringBuilder线程不安全

**性能**

每次对 String 类型进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的 String 对象。
StringBuffer 每次都会对 StringBuffer 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用
StringBuilder相比使用 StringBuffer 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。 

**对于三者使用的总结：**

1. 操作少量的数据 = String

2. 单线程操作字符串缓冲区下操作大量数据 = StringBuilder

3. 多线程操作字符串缓冲区下操作大量数据 = StringBuffer 

   String 为什么是不可变的？String 和 StringBuffer、StringBuilder 的区别是什么？ 

### 3. 自动装箱与拆箱 

1. == 与 equals 

2. final 关键字

   变量：

   类：

   方法：

### 4. Object类的常见方法 

   ```java
   public final native Class<?> getClass()//native方法，用于返回当前运行时对象的Class对象，使用了
   final关键字修饰，故不允许子类重写。
   public native int hashCode() //native方法，用于返回对象的哈希码，主要使用在哈希表中，比如JDK中的
   HashMap。
   public boolean equals(Object obj)//用于比较2个对象的内存地址是否相等，String类对该方法进行了重写用
   户比较字符串的值是否相等。
   protected native Object clone() throws CloneNotSupportedException//naitive方法，用于创建并返
   回当前对象的一份拷贝。一般情况下，对于任何对象 x，表达式 x.clone() != x 为true，
   x.clone().getClass() == x.getClass() 为true。Object本身没有实现Cloneable接口，所以不重写clone
   方法并且进行调用的话会发生CloneNotSupportedException异常。
   public String toString()//返回类的名字@实例的哈希码的16进制的字符串。建议Object所有的子类都重写这个
   方法。
   public final native void notify()//native方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监
   视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
   public final native void notifyAll()//native方法，并且不能重写。跟notify一样，唯一的区别就是会唤
   醒在此对象监视器上等待的所有线程，而不是一个线程。
   public final native void wait(long timeout) throws InterruptedException//native方法，并且不
   能重写。暂停线程的执行。注意：sleep方法没有释放锁，而wait方法释放了锁 。timeout是等待时间。
   public final void wait(long timeout, int nanos) throws InterruptedException//多了nanos参
   数，这个参数表示额外时间（以毫微秒为单位，范围是 0-999999）。 所以超时的时间还需要加上nanos毫秒。
   public final void wait() throws InterruptedException//跟之前的2个wait方法一样，只不过该方法一直
   等待，没有超时时间这个概念
   protected void finalize() throws Throwable { }//实例被垃圾回收器回收的时候触发的操作 
   ```
### 5. 异常处理 

   **异常类层次结构图** 

   ![1555410924597](C:\Users\CHT\AppData\Roaming\Typora\typora-user-images\1555410924597.png)

   **Throwable类常用方法**

   + public string getMessage()：返回异常发生时的详细信息

   + public string toString()：返回异常发生时的简要描述

   + public string getLocalizedMessage()：返回异常对象的本地化信息。使用Throwable的子类覆盖这个方法，可
     以声称本地化信息。如果子类没有覆盖该方法，则该方法返回的信息与getMessage（）返回的结果相同

   + public void printStackTrace()：在控制台上打印Throwable对象封装的异常信息

   **异常处理总结**

   + try 块：用于捕获异常。其后可接零个或多个catch块，如果没有catch块，则必须跟一个finally块。

   + catch 块：用于处理try捕获到的异常。 
   + finally 块：无论是否捕获或处理异常，finally块里的语句都会被执行。当在try块或catch块中遇到return语句
     时，finally语句块将在方法返回之前被执行。 

   **在以下4种特殊情况下，finally块不会被执行：**

   1. 在finally语句块中发生了异常。
   2. 在前面的代码中用了System.exit()退出程序。
   3. 程序所在的线程死亡。
   4. 关闭CPU。 

### 6. 获取用键盘输入常用的的两种方法 

   1. 通过 Scanner 

   2. 通过 BufferedReader 

      ```java
      BufferedReader input = new BufferedReader(new InputStreamReader(System.in));
      String s = input.readLine(); 
      ```

### 7. 接口和抽象类的区别是什么 

  1. 接口的方法默认是 public，所有方法在接口中不能有实现(Java 8 开始接口方法可以有默认实现），抽象类可以有非抽象的方法
  2. 接口中的实例变量默认是 final 类型的，而抽象类中则不一定
  3. 一个类可以实现多个接口，但最多只能实现一个抽象类
  4. 一个类实现接口的话要实现接口的所有方法，而抽象类不一定
  5. 接口不能用 new 实例化，但可以声明，但是必须引用一个实现该接口的对象 从设计层面来说，抽象类是对类的抽象，是一种模板设计，接口是行为的抽象，是一种行为的规范。 

### 8. 已掌握

1. 基本数据类型

+ 8种基本数据类型及大小：byte/8，short/16，int/32，long/64，float/32，double/64，char/16，boolean/不确定

  boolean 只有两个值：true、false，可以使用 1 bit 来存储，但是具体大小没有明确规定。JVM 会在编译时期将 boolean 类型的数据转换为 int，使用 1 来表示 true，0 表示 false。JVM 支持 boolean 数组，但是是通过读写 byte 数组来实现的。

+ 基本数据类型的自动装箱和拆箱

  编译器会在自动装箱过程调用 valueOf() 方法，因此多个值相同且值在缓存池范围内的 Integer 实例使用自动装箱来创建，那么就会引用相同的对象。

+ 缓存池

  new Integer(123) 与 Integer.valueOf(123) 的区别在于：

  - new Integer(123) 每次都会新建一个对象；
  - Integer.valueOf(123) 会使用缓存池中的对象，多次调用会取得同一个对象的引用。

  整数（Byte、Short、Integer、Long）的缓存池大小为-128~127

  Boolean的true和false

  Character的缓存池大小为\u0000到\u007F，0~127

2. String

   + String不可变

     value 数组被声明为 final，这意味着 value 数组初始化之后就不能再引用其它数组。并且 String 内部没有改变 value 数组的方法，因此可以保证 String 不可变。

   + 不可变的好处

     **可以缓存hash值，String Pool 的需要**。JDK1.6字符串常量池在方法区中，JDK1.7及以上在堆中

   + **安全性**

     String 经常作为参数，String 不可变性可以保证参数不可变。例如在作为网络连接参数的情况下如果 String 是可变的，那么在网络连接过程中，String 被改变，改变 String 对象的那一方以为现在连接的是其它主机，而实际情况却不一定是。

   + 线程安全

     String 不可变性天生具备线程安全，可以在多个线程中安全地使用。

   + String, StringBuffer and StringBuilder对比

   + [String Pool](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=string-pool)

     字符串常量池保存所有字符串字面量，在编译期间就会确定。

     在 Java 7 之前，String Pool 被放在运行时常量池中，它属于永久代。而在 Java 7，String Pool 被移到堆中。这是因为永久代的空间有限，在大量使用字符串的场景下会导致 OutOfMemoryError 错误。

     如果是采用 "bbb" 这种字面量的形式创建字符串，会自动地将字符串放入 String Pool 中。

   + intern()方法

     当一个字符串调用 intern() 方法时，如果 String Pool 中已经存在一个字符串和该字符串值相等（使用 equals() 方法进行确定），那么就会返回 String Pool 中字符串的引用；否则，就会在 String Pool 中添加一个新的字符串，并返回这个新字符串的引用。

   + [new String("abc")](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=new-stringquotabcquot)

     使用这种方式一共会创建两个字符串对象（前提是 String Pool 中还没有 "abc" 字符串对象）。

     - "abc" 属于字符串字面量，因此编译时期会在 String Pool 中创建一个字符串对象，指向这个 "abc" 字符串字面量；
     - 而使用 new 的方式会在堆中创建一个字符串对象。

     以下是 String 构造函数的源码，可以看到，在将一个字符串对象作为另一个字符串对象的构造函数参数时，并不会完全复制 value 数组内容，而是都会指向同一个 value 数组。

     ```java
public String(String original) {
         this.value = original.value;
         this.hash = original.hash;
     }
     ```
   
3. 运算

   + [参数传递](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=%e5%8f%82%e6%95%b0%e4%bc%a0%e9%80%92)

     **Java 的参数是以值传递的形式传入方法中，而不是引用传递**。实参复制了一份给形参，基本数据类型复制的是值，引用数据类型复制的是地址，方法返回时，实参的值或者地址均不会改变。

     在将一个参数传入一个方法时，本质上是将对象的地址以值的方式传递到形参中。因此在方法中使指针引用其它对象，那么这两个指针此时指向的是完全不同的对象，在一方改变其所指向对象的内容时对另一方没有影响。

   + 类型转换

     Java 不能隐式执行向下转型，因为这会使得精度降低。

   + [switch](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=switch)

     从 Java 7 开始，可以在 switch 条件判断语句中使用 String 对象。switch 不支持 long。

4. 继承

   + [访问权限](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=%e8%ae%bf%e9%97%ae%e6%9d%83%e9%99%90)

   + [抽象类与接口](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=%e6%8a%bd%e8%b1%a1%e7%b1%bb%e4%b8%8e%e6%8e%a5%e5%8f%a3)

     在很多情况下，接口优先于抽象类。因为接口没有抽象类严格的类层次结构要求，可以灵活地为一个类添加行为。并且从 Java 8 开始，接口也可以有默认的方法实现，使得修改接口的成本也变的很低。

     从设计层面上看，抽象类提供了一种 IS-A 关系，那么就必须满足里式替换原则，即子类对象必须能够替换掉所有父类对象。而接口更像是一种 LIKE-A 关系，它只是提供一种方法实现契约，并不要求接口和实现接口的类具有 IS-A 关系。

   + [super](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=super)

     访问父类的构造函数：可以使用 super() 函数访问父类的构造函数，从而委托父类完成一些初始化的工作。

     访问父类的成员：如果子类重写了父类的某个方法，可以通过使用 super 关键字来引用父类的方法实现。

   + [重写与重载](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=%e9%87%8d%e5%86%99%e4%b8%8e%e9%87%8d%e8%bd%bd)

     重写（Override）：要注意父子类的访问权限、抛出异常的包含关系、返回类型的包含关系。子类重写方法使用 @Override 注解，从而让编译器自动检查是否满足限制条件。

     重载（Overload）：存在于同一个类中，指一个方法与已经存在的方法名称上相同，但是参数类型、个数、顺序至少有一个不同。应该注意的是，返回值不同，其它都相同不算是重载（非法，编译不通过）。

     *涉及到重写时，方法调用的优先级为：（看不太懂）*

     - this.show(O)
     - super.show(O)
     - this.show((super)O)
     - super.show((super)O)

5. [Object 通用方法](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=%e4%ba%94%e3%80%81object-%e9%80%9a%e7%94%a8%e6%96%b9%e6%b3%95)

   + equals()

     实现

     1. 检查是否为同一个对象的引用，如果是直接返回 true；

     2. 检查是否是同一个类型，如果不是，直接返回 false；

     3. 将 Object 对象进行转型；

     4. 判断每个关键域是否相等。

   + hashCode()

     在覆盖 equals() 方法时应当总是覆盖 hashCode() 方法，保证等价的两个对象散列值也相等。

   + toString()

     默认返回 类名称@4554617c 这种形式，其中 @ 后面的数值为散列码的无符号十六进制表示

   + clone()

     clone() 是 Object 的 protected 方法，它不是 public，一个类不显式去重写 clone()，其它类就不能直接去调用该类实例的 clone() 方法。

     应该注意的是，clone() 方法并不是 Cloneable 接口的方法，而是 Object 的一个 protected 方法。Cloneable 接口只是规定，如果一个类没有实现 Cloneable 接口又调用了 clone() 方法，就会抛出 CloneNotSupportedException。

     **浅拷贝**

     拷贝对象和原始对象的引用类型引用同一个对象。

     **深拷贝**

     拷贝对象和原始对象的引用类型引用不同对象，他们的值一致。

     **clone() 的替代方案**

     使用 clone() 方法来拷贝一个对象即复杂又有风险，它会抛出异常，并且还需要类型转换。Effective Java 书上讲到，最好不要去使用 clone()，可以使用拷贝构造函数（构造函数的参数就是本类型对象）或者拷贝工厂来拷贝一个对象。

6. [关键字](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=%e5%85%ad%e3%80%81%e5%85%b3%e9%94%ae%e5%ad%97)

   + final：修饰变量，方法，类时不同

   + static：变量、方法、语句块、内部类、静态导包、初始化顺序

     静态导包

     在使用静态变量和方法时不用再指明 ClassName，从而简化代码，但可读性大大降低。

     存在继承的情况下，初始化顺序为：

     - 父类（静态变量、静态语句块）
     - 子类（静态变量、静态语句块）
     - 父类（实例变量、普通语句块）
     - 父类（构造函数）
     - 子类（实例变量、普通语句块）
     - 子类（构造函数）

7. [反射](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=%e4%b8%83%e3%80%81%e5%8f%8d%e5%b0%84)

   Class 和 java.lang.reflect 一起对反射提供了支持，java.lang.reflect 类库主要包含了以下三个类：

   - Field ：可以使用 get() 和 set() 方法读取和修改 Field 对象关联的字段；
   - Method ：可以使用 invoke() 方法调用与 Method 对象关联的方法；
   - Constructor ：可以用 Constructor 创建新的对象。

   反射的优点：可扩展性、类浏览器和可视化开发环境、调试器和测试工具

   反射的缺点：性能开销、安全限制、内部暴露

8. [异常](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=%e5%85%ab%e3%80%81%e5%bc%82%e5%b8%b8)

   - 受检异常 ：需要用 try...catch... 语句捕获并进行处理，并且可以从异常中恢复；
   - 非受检异常(RuntimeException) ：是程序运行时错误，例如除 0 会引发 Arithmetic Exception，此时程序崩溃并且无法恢复。

9. [泛型](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=%e4%b9%9d%e3%80%81%e6%b3%9b%e5%9e%8b)

10. [注解](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=%e5%8d%81%e3%80%81%e6%b3%a8%e8%a7%a3)

    Java 注解是附加在代码中的一些元信息，用于一些工具在编译、运行时进行解析和使用，起到说明、配置的功能。

11. [特性](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=%e5%8d%81%e4%b8%80%e3%80%81%e7%89%b9%e6%80%a7)

    [Java 各版本的新特性](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=java-%e5%90%84%e7%89%88%e6%9c%ac%e7%9a%84%e6%96%b0%e7%89%b9%e6%80%a7)

    [Java 与 C++ 的区别](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=java-%e4%b8%8e-c-%e7%9a%84%e5%8c%ba%e5%88%ab)

    - Java 是纯粹的面向对象语言，所有的对象都继承自 java.lang.Object，C++ 为了兼容 C 即支持面向对象也支持面向过程。
    - Java 通过虚拟机从而实现跨平台特性，但是 C++ 依赖于特定的平台。
    - Java 没有指针，它的引用可以理解为安全指针，而 C++ 具有和 C 一样的指针。
    - Java 支持自动垃圾回收，而 C++ 需要手动回收。
    - Java 不支持多重继承，只能通过实现多个接口来达到相同目的，而 C++ 支持多重继承。
    - Java 不支持操作符重载，虽然可以对两个 String 对象执行加法运算，但是这是语言内置支持的操作，不属于操作符重载，而 C++ 可以。
    - Java 的 goto 是保留字，但是不可用，C++ 可以使用 goto。
    - Java 不支持条件编译，C++ 通过 #ifdef #ifndef 等预处理命令从而实现条件编译。

    [JRE or JDK](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%9F%BA%E7%A1%80?id=jre-or-jdk)

    - JRE is the JVM program, Java application need to run on JRE.
    - JDK is a superset of JRE, JRE + tools for developing java programs. e.g, it provides the compiler "javac"

### 9. 没熟悉

1. 基本数据类型直接将值存在栈上，而引用数据类型将引用的地址存在栈上，实际引用的值存在堆上。

2. 缓存池

   缓存池在JDK1.6之后放在堆中，之前放在方法区。
   整型常量池大小为-128~127。字符型的常量池大小为0~127。
   与基本数据类型用"=="比较时，比较的是值的大小。两个引用数据类型"=="比较时，比较的是内存位置。
   用new新建的对象会在堆中的随机位置。用valueOf优先从缓存池获取。
   String的intern方法，会返回此字符串常量池中的引用。如果常量池中没有，会在常量池中创建新的字符串，再返回其引用。

3. BIO、NIO、AIO

   BIO：同步阻塞IO，传统IO，使用简单方便，并发处理能力低
   NIO：同步非阻塞IO，客户端和服务端通过通道（channel）通信实现了多路复用。select
   AIO：异步非阻塞IO，基于时间和回调机制
   
4. try-catch-finally 中，如果 catch 中 return 了，finally 还会执行吗？

   会执行，在return之前执行。不过不会改变返回值，除非finally中也有返回语句。

5. 常见的异常类

   - NullPointerException：当应用程序试图访问空对象时，则抛出该异常。
   - SQLException：提供关于数据库访问错误或其他错误信息的异常。
   - IndexOutOfBoundsException：指示某排序索引（例如对数组、字符串或向量的排序）超出范围时抛出。 
   - NumberFormatException：当应用程序试图将字符串转换成一种数值类型，但该字符串不能转换为适当格式时，抛出该异常。
   - FileNotFoundException：当试图打开指定路径名表示的文件失败时，抛出此异常。
   - IOException：当发生某种I/O异常时，抛出此异常。此类是失败或中断的I/O操作生成的异常的通用类。
   - ClassCastException：当试图将对象强制转换为不是实例的子类时，抛出该异常。

## 二、 集合框架

![](http://ww1.sinaimg.cn/mw690/005DF9Qily1g2cjacd57sj30k00ehwha.jpg)

### 1. Arraylist 与 LinkedList 异同 

|                    | ArrayList  | LinkedList   |
| ------------------ | ---------- | ------------ |
| 是否线程安全       | 否         | 否           |
| 底层数据结构       | 数组       | 双向链表     |
| 插入删除时间复杂度 | O(n)       | O(1)         |
| 查询时间复杂度     | O(1)       | O(n)         |
|                    | 预留的空间 | 前驱后继引用 |

   **补充内容:RandomAccess接口** 

   ```java
   public interface RandomAccess {} 
   ```

   查看源码我们发现实际上 RandomAccess 接口中什么都没有定义。所以，在我看来 RandomAccess 接口不过是一个
   标识罢了。标识什么？ 标识实现这个接口的类具有随机访问功能。 

   **下面再总结一下 list 的遍历方式选择：** 

   + 实现了RandomAccess接口的list，优先选择普通for循环 ，其次foreach,
   + 未实现RandomAccess接口的list， 优先选择iterator遍历（foreach遍历底层也是通过iterator实现的），大
     size的数据，千万不要使用普通for循环 。每次都需要O(n)的时间复杂度指定位置的数据。

### 2. ArrayList 与 Vector 区别 

   Vector类的所有方法都是同步的。可以由两个线程安全地访问一个Vector对象、但是一个线程访问Vector的话代码要
   在同步操作上耗费大量的时间。
   Arraylist不是同步的，所以在不需要保证线程安全时时建议使用Arraylist。 

### 3. HashMap的底层实现 

   **JDK1.8之前**，HashMap的底层是**数组和链表**结合在一起使用的**链表散列**。

   通过 **(n - 1) & hash** 判断当前元素存放的位置（这里的 n 指的是数组的长度），如果当前位置存在元素的话，就判断该元素与要存入的元素的 hash 值以及 key 是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。**数组长度需要为2的n次方，这样可以用按位与运算代替取余运算**。 

   **JDK1.8之后**  ，当链表长度大于阈值（默认为8）时，会将链表转化为红黑树，以减少搜索时间O(logn)。 

### 4. HashMap 和 Hashtable 的区别 

|                              | HashMap                                       | Hashtable              |
| ---------------------------- | --------------------------------------------- | ---------------------- |
| 线程是否安全                 | 不安全                                        | 安全                   |
| 效率                         | 低                                            | 高                     |
| 对null key和null value的支持 | null键只能有一个，值可以为null                | 键和值都不可以为null   |
| 初始容量和扩充方法           | 初始为16，扩充后为2n                          | 初始为11，扩充后为2n+1 |
| 底层数据结构                 | JDK1.8之后，链表长度大于8，链表会转换为红黑树 | 没有这样的机制         |

   HashMap多线程操作时可能会导致死循环问题（在扩容复制时）。

### 5. HashSet 和 HashMap 区别 

   HashSet借由HashMap来实现，HashSet的值就是HashMap的键，由HashMap的键唯一保证HashSet的值唯一。

   HashSet 的源码非常非常少，因为除了 clone() 方法、writeObject()方法、readObject()方法是 HashSet 自己不得不实现之外，其他方法都是直接调用 HashMap 中的方法。

### 6. ConcurrentHashMap和Hashtable

   + **底层数据结构**：JDK1.7的 ConcurrentHashMap 底层采用 **分段的数组+链表** 实现，JDK1.8 采用的数据结构跟
     HashMap1.8的结构一样，数组+链表/红黑二叉树。 

   + **实现线程安全的方式（重要）**： ① **在JDK1.7的时候，ConcurrentHashMap（分段锁）** 对整个桶数组进行了分割分段(Segment)，每一把锁只锁容器其中一部分数据，多线程访问容器里不同数据段的数据，就不会存在锁竞争，提高并发访问率。 **到了 JDK1.8 的时候已经摒弃了Segment的概念，而是直接用 Node 数组+链表+红黑树的数据结构来实现，并发控制使用 synchronized 和 CAS 来操作。（JDK1.6以后 对 synchronized锁做了很多优化） 整个看起来就像是优化过且线程安全的 HashMap**，虽然在JDK1.8中还能看到 Segment 的数据结构，但是已经简化了属性，只是为了兼容旧版本；② **Hashtable(同一把锁)** :使用 synchronized 来保证线程安全，效率非常低下。当一个线程访问同步方法时，其他线程也访问同步方法，可能会进入阻塞或轮询状态，如使用put 添加元素，另一个线程不能使用 put 添加元素，也不能使用 get，竞争会越来越激烈效率越低。 

     JDK1.8的ConcurrentHashMap（TreeBin: 红黑二叉树节点 Node: 链表节点）： 

     ![1555414278694](C:\Users\CHT\AppData\Roaming\Typora\typora-user-images\1555414278694.png)

### 7. ConcurrentHashMap线程安全的具体实现方式/底层具体实现 

   **JDK1.7**
   首先将数据分为一段一段的存储，然后给每一段数据配一把锁，当一个线程占用锁访问其中一个段数据时，其他段的
   数据也能被其他线程访问。
   **ConcurrentHashMap 是由 Segment 数组结构和 HashEntry 数组结构组成。**
   Segment 实现了 ReentrantLock,所以 Segment 是一种可重入锁，扮演锁的角色。HashEntry 用于存储键值对数据。 

   ```java
   static class Segment<K,V> extends ReentrantLock implements Serializable {
   } 
   ```

   一个 ConcurrentHashMap 里包含一个 Segment 数组。Segment 的结构和HashMap类似，是一种数组和链表结构，一个 Segment 包含一个 HashEntry 数组，每个 HashEntry 是一个链表结构的元素，每个 Segment 守护着一个
   HashEntry数组里的元素，当对 HashEntry 数组的数据进行修改时，必须首先获得对应的 Segment的锁。 

   **JDK1.8**

   ConcurrentHashMap取消了Segment分段锁，采用CAS和synchronized来保证并发安全。**数据结构跟HashMap1.8的结构类似，数组+链表/红黑二叉树。**
   **synchronized只锁定当前链表或红黑二叉树的首节点，这样只要hash不冲突，就不会产生并发，效率又提升N倍**

### 8. 集合框架底层数据结构总结

   **Collection**

      1. List
      + **ArrayList**：Object数组，线程不安全
      + **Vector**：Object数组，线程安全，速度慢
      + **LinkedList**：双向链表（JDK1.6之前为循环链表，JDK1.7取消了循环）
      2. Map
      + **HashMap**： JDK1.8之前HashMap由数组+链表组成的，数组是HashMap的主体，链表则是主要为了解决哈希冲突而存在的（“拉链法”解决冲突）。JDK1.8以后在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为8）时，将链表转化为红黑树，以减少搜索时间
      + **LinkedHashMap**：LinkedHashMap 继承自 HashMap，所以它的底层仍然是基于拉链式散列结构即由数组和
        链表或红黑树组成。另外，LinkedHashMap 在上面结构的基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。同时通过对链表进行相应的操作，实现了访问顺序相关逻辑。 
      3. Set
      + **HashSet（无序，唯一）**:基于 HashMap 实现的，底层采用 HashMap 来保存元素
      + **LinkedHashSet**：LinkedHashSet 继承与 HashSet，并且其内部是通过 LinkedHashMap 来实现的。有点类似于我们之前说的LinkedHashMap 其内部是基于 HashMap 实现一样，不过还是有一点点区别的。
      + **TreeSet（有序，唯一）**：红黑树(自平衡的排序二叉树)

### 9. [容器 Cyc2018](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8) 

一、概述

容器主要包括 Collection 和 Map 两种，Collection 存储着对象的集合，而 Map 存储着键值对（两个对象）的映射表。

[Collection](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=collection)

1. [Set](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_1-set)
   - TreeSet：基于红黑树实现，支持有序性操作，例如根据一个范围查找元素的操作。但是查找效率不如 HashSet，HashSet 查找的时间复杂度为 O(1)，TreeSet 则为 O(logN)。
   - HashSet：基于哈希表实现，支持快速查找，但不支持有序性操作。并且失去了元素的插入顺序信息，也就是说使用 Iterator 遍历 HashSet 得到的结果是不确定的。
   - LinkedHashSet：具有 HashSet 的查找效率，且内部使用双向链表维护元素的插入顺序。
2. [List](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_2-list)
   - ArrayList：基于动态数组实现，支持随机访问。
   - Vector：和 ArrayList 类似，但它是线程安全的。
   - LinkedList：基于双向链表实现，只能顺序访问，但是可以快速地在链表中间插入和删除元素。不仅如此，LinkedList 还可以用作栈、队列和双向队列。
3. [Queue](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_3-queue)
   - LinkedList：可以用它来实现双向队列。
   - PriorityQueue：基于堆结构实现，可以用它来实现优先队列。

[Map](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=map)

- TreeMap：基于红黑树实现。
- HashMap：基于哈希表实现。
- HashTable：和 HashMap 类似，但它是线程安全的，这意味着同一时刻多个线程可以同时写入 HashTable 并且不会导致数据不一致。它是遗留类，不应该去使用它。现在可以使用 ConcurrentHashMap 来支持线程安全，并且 ConcurrentHashMap 的效率会更高，因为 ConcurrentHashMap 引入了分段锁。
- LinkedHashMap：使用双向链表来维护元素的顺序，顺序为插入顺序或者最近最少使用（LRU）顺序。

二、[容器中的设计模式](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=%e4%ba%8c%e3%80%81%e5%ae%b9%e5%99%a8%e4%b8%ad%e7%9a%84%e8%ae%be%e8%ae%a1%e6%a8%a1%e5%bc%8f)

[迭代器模式](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=%e8%bf%ad%e4%bb%a3%e5%99%a8%e6%a8%a1%e5%bc%8f)

Collection 继承了 Iterable 接口，其中的 iterator() 方法能够产生一个 Iterator 对象，通过这个对象就可以迭代遍历 Collection 中的元素。从 JDK 1.5 之后可以使用 foreach 方法来遍历实现了 Iterable 接口的聚合对象。

[适配器模式](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=%e9%80%82%e9%85%8d%e5%99%a8%e6%a8%a1%e5%bc%8f)

java.util.Arrays#asList() 可以把数组类型转换为 List 类型。

应该注意的是 asList() 的参数为泛型的变长参数，不能使用基本类型数组作为参数，只能使用相应的包装类型数组。

[三、源码分析](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=%e4%b8%89%e3%80%81%e6%ba%90%e7%a0%81%e5%88%86%e6%9e%90)

[ArrayList](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=arraylist)

+ Fail-Fast

  modCount 用来记录 ArrayList 结构发生变化的次数。结构发生变化是指添加或者删除至少一个元素的所有操作，或者是调整内部数组的大小，仅仅只是设置元素的值不算结构发生变化。

  在进行序列化或者迭代等操作时，需要比较操作前后 modCount 是否改变，如果改变了需要抛出 ConcurrentModificationException。

+ 序列化

  ArrayList 基于数组实现，并且具有动态扩容特性，因此保存元素的数组不一定都会被使用，那么就没必要全部进行序列化。

  保存元素的数组 elementData 使用 transient 修饰，该关键字声明数组默认不会被序列化。

  ArrayList 实现了 writeObject() 和 readObject() 来控制只序列化数组中有元素填充那部分内容。

[Vector](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=vector)

它的实现与 ArrayList 类似，但是使用了 synchronized 进行同步。

+ 替代方案

  可以使用 `Collections.synchronizedList();` 得到一个线程安全的 ArrayList。

  ```java
  List<String> list = new ArrayList<>();
  List<String> synList = Collections.synchronizedList(list);
  ```

  也可以使用 concurrent 并发包下的 CopyOnWriteArrayList 类。

  ```java
  List<String> list = new CopyOnWriteArrayList<>();
  ```

  [CopyOnWriteArrayList](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=copyonwritearraylist)

  **读写分离**

  写操作在一个复制的数组上进行，读操作还是在原始数组中进行，读写分离，互不影响。

  写操作需要加锁，防止并发写入时导致写入数据丢失。

  写操作结束之后需要把原始数组指向新的复制数组。

  **适用场景**

  CopyOnWriteArrayList 在写操作的同时允许读操作，大大提高了读操作的性能，因此很适合读多写少的应用场景。

  但是 CopyOnWriteArrayList 有其缺陷：

  - 内存占用：在写操作时需要复制一个新的数组，使得内存占用为原来的两倍左右；
  - 数据不一致：读操作不能读取实时性的数据，因为部分写操作的数据还未同步到读数组中。

  所以 CopyOnWriteArrayList 不适合内存敏感以及对实时性要求很高的场景。

[LinkedList](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=linkedlist)

基于双向链表实现，使用 Node 存储链表节点信息。

```java
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;
}Copy to clipboardErrorCopied
```

每个链表存储了 first 和 last 指针：

```java
transient Node<E> first;
transient Node<E> last;
```

[HashMap](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=hashmap)（JDK1.7）

1. [存储结构](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_1-%e5%ad%98%e5%82%a8%e7%bb%93%e6%9e%84)

   内部包含了一个 Entry 类型的数组 table。

   ```java
   transient Entry[] table;Copy to clipboardErrorCopied
   ```

   Entry 存储着键值对。它包含了四个字段，从 next 字段我们可以看出 Entry 是一个链表。即数组中的每个位置被当成一个桶，一个桶存放一个链表。HashMap 使用拉链法来解决冲突，同一个链表中存放哈希值相同的 Entry。

2. [拉链法的工作原理](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_2-%e6%8b%89%e9%93%be%e6%b3%95%e7%9a%84%e5%b7%a5%e4%bd%9c%e5%8e%9f%e7%90%86)

   应该注意到链表的插入是以头插法方式进行的，不是插在链表最后，而是插入在链表头部。

   查找需要分成两步进行：

   - 计算键值对所在的桶；
   - 在链表上顺序查找，时间复杂度显然和链表的长度成正比。

3. [put 操作](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_3-put-%e6%93%8d%e4%bd%9c)

   HashMap 允许插入键为 null 的键值对。但是因为无法调用 null 的 hashCode() 方法，也就无法确定该键值对的桶下标，只能通过强制指定一个桶下标来存放。HashMap 使用第 0 个桶存放键为 null 的键值对。

4. [确定桶下标](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_4-%e7%a1%ae%e5%ae%9a%e6%a1%b6%e4%b8%8b%e6%a0%87)

   根据hash值得到对应桶的位置时，需对数组长度进行取余操作，由于数组长度为2的n次方，这里使用hash&(length-1)的位运算代替的取余操作，为了得到更高的效率。

5. [扩容-基本原理](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_5-%e6%89%a9%e5%ae%b9-%e5%9f%ba%e6%9c%ac%e5%8e%9f%e7%90%86)

   | 参数       | 含义                                                         |
   | ---------- | ------------------------------------------------------------ |
   | capacity   | table 的容量大小，默认为 16。需要注意的是 capacity 必须保证为 2 的 n 次方。扩容时加倍。 |
   | size       | 键值对数量。                                                 |
   | threshold  | size 的临界值，当 size 大于等于 threshold 就必须进行扩容操作。 |
   | loadFactor | 装载因子，table 能够使用的比例，threshold = capacity * loadFactor。 |

6. [扩容-重新计算桶下标](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_6-%e6%89%a9%e5%ae%b9-%e9%87%8d%e6%96%b0%e8%ae%a1%e7%ae%97%e6%a1%b6%e4%b8%8b%e6%a0%87)

   假设原数组长度 capacity 为 16，扩容之后 new capacity 为 32：

   ```html
   capacity     : 00010000
   new capacity : 00100000Copy to clipboardErrorCopied
   ```

   对于一个 Key，

   - 它的哈希值如果在第 5 位上为 0，那么取模得到的结果和之前一样；
   - 如果为 1，那么得到的结果为原来的结果 +16。

7. [计算数组容量](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_7-%e8%ae%a1%e7%ae%97%e6%95%b0%e7%bb%84%e5%ae%b9%e9%87%8f) ？

   HashMap 构造函数允许用户传入的容量不是 2 的 n 次方，因为它可以自动地将传入的容量转换为 2 的 n 次方。

   先考虑如何求一个数的掩码，对于 10010000，它的掩码为 11111111，可以使用以下方法得到：

   ```
   mask |= mask >> 1    11011000
   mask |= mask >> 2    11111110
   mask |= mask >> 4    11111111Copy to clipboardErrorCopied
   ```

   mask+1 是大于原始数字的最小的 2 的 n 次方。

8. [链表转红黑树](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_8-%e9%93%be%e8%a1%a8%e8%bd%ac%e7%ba%a2%e9%bb%91%e6%a0%91)

   从 JDK 1.8 开始，一个桶存储的链表长度大于 8 时会将链表转换为红黑树。

9. ### [与 HashTable 的比较](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_9-%e4%b8%8e-hashtable-%e7%9a%84%e6%af%94%e8%be%83)

   - HashTable 使用 synchronized 来进行同步。
   - HashMap 可以插入键为 null 的 Entry。
   - HashMap 的迭代器是 fail-fast 迭代器。
   - HashMap 不能保证随着时间的推移 Map 中的元素次序是不变的。

[ConcurrentHashMap](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=concurrenthashmap)（JDK1.7）

1. [存储结构](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_1-%e5%ad%98%e5%82%a8%e7%bb%93%e6%9e%84-1)

   ConcurrentHashMap 和 HashMap 实现上类似，最主要的差别是 ConcurrentHashMap 采用了分段锁（Segment），每个分段锁维护着几个桶（HashEntry），多个线程可以同时访问不同分段锁上的桶，从而使其并发度更高（并发度就是 Segment 的个数）。

   Segment 继承自 ReentrantLock。

   默认的并发级别为 16，也就是说默认创建 16 个 Segment。

2. [size 操作](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_2-size-%e6%93%8d%e4%bd%9c)

   每个 Segment 维护了一个 count 变量来统计该 Segment 中的键值对个数。

   在执行 size 操作时，需要遍历所有 Segment 然后把 count 累计起来。

   ConcurrentHashMap 在执行 size 操作时先尝试不加锁，如果连续两次不加锁操作得到的结果一致，那么可以认为这个结果是正确的。

   尝试次数使用 RETRIES_BEFORE_LOCK 定义，该值为 2，retries 初始值为 -1，因此尝试次数为 3。

   如果尝试的次数超过 3 次，就需要对每个 Segment 加锁。

3. [JDK 1.8 的改动](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=_3-jdk-18-%e7%9a%84%e6%94%b9%e5%8a%a8)

   JDK 1.7 使用分段锁机制来实现并发更新操作，核心类为 Segment，它继承自重入锁 ReentrantLock，并发度与 Segment 数量相等。

   JDK 1.8 使用了 CAS 操作来支持更高的并发度，在 CAS 操作失败时使用内置锁 synchronized。基础数据结构为Node数组，只有操作数据的哈希值相等的时候才需要加锁。

   并且 JDK 1.8 的实现也在链表过长时会转换为红黑树。

[LinkedHashMap](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=linkedhashmap)

1. [存储结构](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=%e5%ad%98%e5%82%a8%e7%bb%93%e6%9e%84)

   继承自 HashMap，因此具有和 HashMap 一样的快速查找特性。

   内部维护了一个双向链表，用来维护插入顺序或者 LRU 顺序。

   accessOrder 决定了顺序，默认为 false，此时维护的是插入顺序。

   LinkedHashMap 最重要的是以下用于维护顺序的函数，它们会在 put、get 等方法中调用。

2. [afterNodeAccess()](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=afternodeaccess)

   当一个节点被访问时，如果 accessOrder 为 true，则会将该节点移到链表尾部。也就是说指定为 LRU 顺序之后，在每次访问一个节点时，会将这个节点移到链表尾部，保证链表尾部是最近访问的节点，那么链表首部就是最近最久未使用的节点。

3. [afterNodeInsertion()](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=afternodeinsertion)

   当一个节点被访问时，如果 accessOrder 为 true，则会将该节点移到链表尾部。也就是说指定为 LRU 顺序之后，在每次访问一个节点时，会将这个节点移到链表尾部，保证链表尾部是最近访问的节点，那么链表首部就是最近最久未使用的节点。

4. [afterNodeInsertion()](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=afternodeinsertion)

   在 put 等操作之后执行，当 removeEldestEntry() 方法返回 true 时会移除最晚的节点，也就是链表首部节点 first。

   evict 只有在构建 Map 的时候才为 false，在这里为 true。

   removeEldestEntry() 默认为 false，如果需要让它为 true，需要继承 LinkedHashMap 并且覆盖这个方法的实现，这在实现 LRU 的缓存中特别有用，通过移除最近最久未使用的节点，从而保证缓存空间足够，并且缓存的数据都是热点数据。

5. [LRU 缓存](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=lru-%e7%bc%93%e5%ad%98)

   以下是使用 LinkedHashMap 实现的一个 LRU 缓存：

   - 设定最大缓存空间 MAX_ENTRIES 为 3；
   - 使用 LinkedHashMap 的构造函数将 accessOrder 设置为 true，开启 LRU 顺序；
   - 覆盖 removeEldestEntry() 方法实现，在节点多于 MAX_ENTRIES 就会将最近最久未使用的数据移除。

[WeakHashMap](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=weakhashmap)

1. [存储结构](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=%e5%ad%98%e5%82%a8%e7%bb%93%e6%9e%84-1)

   WeakHashMap 的 Entry 继承自 WeakReference，被 WeakReference 关联的对象在下一次垃圾回收时会被回收。

   WeakHashMap 主要用来实现缓存，通过使用 WeakHashMap 来引用缓存对象，由 JVM 对这部分缓存进行回收。

2. [ConcurrentCache](https://cyc2018.github.io/CS-Notes/#/notes/Java%20%E5%AE%B9%E5%99%A8?id=concurrentcache)

   Tomcat 中的 ConcurrentCache 使用了 WeakHashMap 来实现缓存功能。

   ConcurrentCache 采取的是分代缓存：

   - 经常使用的对象放入 eden 中，eden 使用 ConcurrentHashMap 实现，不用担心会被回收（伊甸园）；
   - 不常用的对象放入 longterm，longterm 使用 WeakHashMap 实现，这些老对象会被垃圾收集器回收。
   - 当调用 get() 方法时，会先从 eden 区获取，如果没有找到的话再到 longterm 获取，当从 longterm 获取到就把对象放入 eden 中，从而保证经常被访问的节点不容易被回收。
   - 当调用 put() 方法时，如果 eden 的大小超过了 size，那么就将 eden 中的所有对象都放入 longterm 中，利用虚拟机回收掉一部分不经常使用的对象。

### 10. 不熟悉

1. 在java编程语言中，**最基本的结构就是两种，一个是数组，另外一个是模拟指针（引用）**，所有的数据结构都可以用这两个基本结构来构造的，HashMap也不例外。

2. 数组和List相互转换

   - List转换成为数组：调用ArrayList的toArray方法。
   - 数组转换成为List：调用Arrays的asList方法。

3. poll和remove区别

   获取元素失败时，poll会返回空，remove会抛出异常

4. util包里线程安全的集合类

   - vector：就比arraylist多了个同步化机制（线程安全），因为效率较低，现在已经不太建议使用。在web应用中，特别是前台页面，往往效率（页面响应速度）是优先考虑的。
   - statck：堆栈类，先进后出。在Vector基础上实现的。
   - hashtable：就比hashmap多了个线程安全。
   - enumeration：枚举，相当于迭代器。

5. Iterator 和 ListIterator 有什么区别？

   + Iterator可用来遍历Set和List集合，但是ListIterator只能用来遍历List。 
+ Iterator对集合只能是前向遍历，ListIterator既可以前向也可以后向。 
   + ListIterator实现了Iterator接口，并包含其他的功能，比如：增加元素，替换元素，获取前一个和后一个元素的索引，等等。

6. 动态代理

   + 当想要给实现了某个接口的类中的方法，加一些额外的处理。比如说加日志，加事务等。可以给这个类创建一个代理，故名思议就是创建一个新的类，这个类不仅包含原来类方法的功能，而且还在原来的基础上添加了额外处理的新类。这个代理类并不是定义好的，是动态生成的。具有解耦意义，灵活，扩展性强。

   + 应用场景：Spring的AOP，加日志，加性能统计

   + 实现方式

     + 静态代理

       由程序员创建或特定工具自动生成源代码，也就是在编译时就已经将接口，被代理类，代理类等确定下来。在程序运行之前，代理类的.class文件就已经生成。

     + 动态代理

       代理类在程序运行时创建的代理方式被成为动态代理。 我们上面静态代理的例子中，代理类(studentProxy)是自己定义好的，在程序运行之前就已经编译完成。然而动态代理，代理类并不是在Java代码中定义的，而是在运行时根据我们在Java代码中的“指示”动态生成的。相比于静态代理， 动态代理的优势在于可以很方便的对代理类的函数进行统一的处理，而不用修改每个代理类中的方法。

7. 对象克隆

   有两种方式：

   + 实现Cloneable接口并重写Object类中的clone()方法；
   + 实现Serializable接口，通过对象的序列化和反序列化实现克隆，可以实现真正的深度克隆

8. 深拷贝和浅拷贝

   浅拷贝只是拷贝对象的引用。深拷贝是将对象的值也复制过来，是一个全新的对象。


## 三、[Java I/O](https://cyc2018.github.io/CS-Notes/#/notes/Java%20IO)

### 1. [概览](https://cyc2018.github.io/CS-Notes/#/notes/Java%20IO?id=%e4%b8%80%e3%80%81%e6%a6%82%e8%a7%88)

Java 的 I/O 大概可以分成以下几类：

- 磁盘操作：File
- 字节操作：InputStream 和 OutputStream
- 字符操作：Reader 和 Writer
- 对象操作：Serializable
- 网络操作：Socket
- 新的输入/输出：NIO

### 2. [磁盘操作](https://cyc2018.github.io/CS-Notes/#/notes/Java%20IO?id=%e4%ba%8c%e3%80%81%e7%a3%81%e7%9b%98%e6%93%8d%e4%bd%9c)

File 类可以用于表示文件和目录的信息，但是它不表示文件的内容。

### 3. [字节操作](https://cyc2018.github.io/CS-Notes/#/notes/Java%20IO?id=%e4%b8%89%e3%80%81%e5%ad%97%e8%8a%82%e6%93%8d%e4%bd%9c)

**实现文件复制**

**装饰者模式**

Java I/O 使用了装饰者模式来实现。以 InputStream 为例，

- InputStream 是抽象组件；

- FileInputStream 是 InputStream 的子类，属于具体组件，提供了字节流的输入操作；

- FilterInputStream 属于抽象装饰者，装饰者用于装饰组件，为组件提供额外的功能。例如 BufferedInputStream 为 FileInputStream 提供缓存的功能。

  ![](http://wx2.sinaimg.cn/large/005DF9Qily1g272vukjc6j310s0c40vn.jpg)

### 4. [字符操作](https://cyc2018.github.io/CS-Notes/#/notes/Java%20IO?id=%e5%9b%9b%e3%80%81%e5%ad%97%e7%ac%a6%e6%93%8d%e4%bd%9c)

**编码与解码**

编码就是把字符转换为字节，而解码是把字节重新组合成字符。

如果编码和解码过程使用不同的编码方式那么就出现了乱码。

- GBK 编码中，中文字符占 2 个字节，英文字符占 1 个字节；
- UTF-8 编码中，中文字符占 3 个字节，英文字符占 1 个字节；
- UTF-16be 编码中，中文字符和英文字符都占 2 个字节。

UTF-16be 中的 be 指的是 Big Endian，也就是大端。相应地也有 UTF-16le，le 指的是 Little Endian，也就是小端。

Java 的内存编码使用双字节编码 UTF-16be，这不是指 Java 只支持这一种编码方式，而是说 char 这种类型使用 UTF-16be 进行编码。char 类型占 16 位，也就是两个字节，Java 使用这种双字节编码是为了让一个中文或者一个英文都能使用一个 char 来存储。

**String 的编码方式**

String 可以看成一个字符序列，可以指定一个编码方式将它编码为字节序列，也可以指定一个编码方式将一个字节序列解码为 String。

```java
String str1 = "中文";
byte[] bytes = str1.getBytes("UTF-8");
String str2 = new String(bytes, "UTF-8");
System.out.println(str2);Copy to clipboardErrorCopied
```

在调用无参数 getBytes() 方法时，默认的编码方式不是 UTF-16be。双字节编码的好处是可以使用一个 char 存储中文和英文，而将 String 转为 bytes[] 字节数组就不再需要这个好处，因此也就不再需要双字节编码。getBytes() 的默认编码方式与平台有关，一般为 UTF-8。

```java
byte[] bytes = str1.getBytes();
```

**Reader 与 Writer**

不管是磁盘还是网络传输，最小的存储单元都是字节，而不是字符。但是在程序中操作的通常是字符形式的数据，因此需要提供对字符进行操作的方法。

- InputStreamReader 实现从字节流解码成字符流；
- OutputStreamWriter 实现字符流编码成为字节流。

### 5. [对象操作](https://cyc2018.github.io/CS-Notes/#/notes/Java%20IO?id=%e4%ba%94%e3%80%81%e5%af%b9%e8%b1%a1%e6%93%8d%e4%bd%9c)

**序列化**

序列化就是将一个对象转换成字节序列，方便存储和传输。

- 序列化：ObjectOutputStream.writeObject()
- 反序列化：ObjectInputStream.readObject()

不会对静态变量进行序列化，因为序列化只是保存对象的状态，静态变量属于类的状态。

**Serializable**

序列化的类需要实现 Serializable 接口，它只是一个标准，没有任何方法需要实现，但是如果不去实现它的话而进行序列化，会抛出异常。

**transient**

transient 关键字可以使一些属性不会被序列化。

ArrayList 中存储数据的数组 elementData 是用 transient 修饰的，因为这个数组是动态扩展的，并不是所有的空间都被使用，因此就不需要所有的内容都被序列化。通过重写序列化和反序列化方法，使得可以只序列化数组中有内容的那部分数据。

### 6. [网络操作](https://cyc2018.github.io/CS-Notes/#/notes/Java%20IO?id=%e5%85%ad%e3%80%81%e7%bd%91%e7%bb%9c%e6%93%8d%e4%bd%9c)

Java 中的网络支持：

- InetAddress：用于表示网络上的硬件资源，即 IP 地址；

  没有公有的构造函数，只能通过静态方法来创建实例。

- URL：统一资源定位符；

  可以直接从 URL 中读取字节流数据。

- Sockets：使用 TCP 协议实现网络通信；

  - ServerSocket：服务器端类
  - Socket：客户端类
  - 服务器和客户端通过 InputStream 和 OutputStream 进行输入输出。

- Datagram：使用 UDP 协议实现网络通信。

  - DatagramSocket：通信类
  - DatagramPacket：数据包类

### 7. [NIO](https://cyc2018.github.io/CS-Notes/#/notes/Java%20IO?id=%e4%b8%83%e3%80%81nio)

新的输入/输出 (NIO) 库是在 JDK 1.4 中引入的，弥补了原来的 I/O 的不足，提供了高速的、面向块的 I/O。

**流与块**

I/O 与 NIO 最重要的区别是数据打包和传输的方式，I/O 以流的方式处理数据，而 NIO 以块的方式处理数据。

**通道与缓冲区**

+ 通道

  通道 Channel 是对原 I/O 包中的流的模拟，可以通过它读取和写入数据。

  通道与流的不同之处在于，流只能在一个方向上移动(一个流必须是 InputStream 或者 OutputStream 的子类)，而通道是双向的，可以用于读、写或者同时用于读写。

  通道包括以下类型：

  + FileChannel：从文件中读写数据；
  + DatagramChannel：通过 UDP 读写网络中数据；
  + SocketChannel：通过 TCP 读写网络中数据；
  + ServerSocketChannel：可以监听新进来的 TCP 连接，对每一个新进来的连接都会创建一个 SocketChannel。

+ 缓冲区

  发送给一个通道的所有数据都必须首先放到缓冲区中，同样地，从通道中读取的任何数据都要先读到缓冲区中。也就是说，不会直接对通道进行读写数据，而是要先经过缓冲区。

  缓冲区实质上是一个数组，但它不仅仅是一个数组。缓冲区提供了对数据的结构化访问，而且还可以跟踪系统的读/写进程。

**缓冲区状态变量**

- capacity：最大容量；
- position：当前已经读写的字节数；
- limit：还可以读写的字节数。

**文件 NIO 实例**

**选择器**

NIO 常常被叫做非阻塞 IO，主要是因为 NIO 在网络通信中的非阻塞特性被广泛使用。

NIO 实现了 IO 多路复用中的 Reactor 模型，一个线程 Thread 使用一个选择器 Selector 通过轮询的方式去监听多个通道 Channel 上的事件，从而让一个线程就可以处理多个事件。

通过配置监听的通道 Channel 为非阻塞，那么当 Channel 上的 IO 事件还未到达时，就不会进入阻塞状态一直等待，而是继续轮询其它 Channel，找到 IO 事件已经到达的 Channel 执行。

因为创建和切换线程的开销很大，因此使用一个线程来处理多个事件而不是一个线程处理一个事件，对于 IO 密集型的应用具有很好地性能。

应该注意的是，只有套接字 Channel 才能配置为非阻塞，而 FileChannel 不能，为 FileChannel 配置非阻塞也没有意义。

**套接字 NIO 实例**

**内存映射文件**

**对比**

NIO 与普通 I/O 的区别主要有以下两点：

- NIO 是非阻塞的；
- NIO 面向块，I/O 面向流。