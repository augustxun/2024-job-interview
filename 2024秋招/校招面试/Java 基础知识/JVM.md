## 一、JVM基础知识

### 1. Java栈和堆的区别？

在 Java 中，通常所说的**栈**指的是**虚拟机栈**，也叫**线程栈**。虚拟机栈是 Java 虚拟机内存模型的一部分，主要用于存储每个线程在执行方法时的栈帧。每个栈帧包含局部变量表、操作数栈、动态链接、方法返回地址等信息。



堆主要用于**存储实例对象、数组**，由 JVM 在堆内存上动态分配内存空间，多个线程可以共享数据。在堆中分配的内存，由 Java虚拟机的自动垃圾回收器来管理。

### 2. 对象头里的内容有什么？

1. Mark Word：
   存储与对象自身状态相关的信息：

1. 对象的哈希码
2. GC标记：在垃圾回收过程中使用
3. 锁状态标志
4. 线程持有的锁记录

1. Class Pointer类型指针：
   指向对象对应的类元数据的指针

------

## 二、内存模型

### 1. (中高频) 说说JVM内存模型？

*线程私有的*

1. 程序计数器：记录当前线程执行的**字节码指令的位置** 1. 字节码解释器通过程序计数器去执行指令；2.多线程情况下，上下文切换时可以恢复到上次发生切换的位置；
2. 虚拟机栈：栈由一个个栈帧组成，每一次调用Java方法都会有一个对应的栈帧被压入栈中，每一个方法调用结束后，都会有一个栈帧被弹出。
3. 本地方法栈：功能和虚拟机栈类似，但是为Native方法服务

*线程共享的*

1. 堆：为对象实例和数组分配内存，字符串常量池也在堆上	
2. 方法区：存储类信息、字段信息、方法信息、常量、静态变量等等。运行时常量池属于方法区的一部分，存储编译期生成的各种字面量和符号引用。
3. 直接内存：不是运行时数据区的一部分，主要由NIO库使用

### 2. 新生代和老年代的区别？什么时候会进入老年代？

Java的堆内存分为新生代、老年代和永久代

大部分新创建的对象都会首先在新生代分配内存。

老年代用于存储生命周期较长的对象，这些对象从新生代晋升而来，或者大对象直接在老年代分配内存。



对象会在以下情况进入老年代：

![img](https://cdn.nlark.com/yuque/0/2024/png/42782944/1725074684107-03ef95cd-ea2e-4b3b-bc6b-f815a3bfab22.png)

### 3. 说说内存泄露问题？

首先，内存泄露的含义指的是程序中不再使用的对象不能被垃圾回收器回收，从而导致内存浪费。

1. **static 字段：**静态字段的生命周期和类的生命周期相同，会存在于应用程序运行生命周期之内，无法清除这些对象会内存泄露。静态字段引用了大量对象，静态集合类
2. **未关闭的资源：**创建连接或者打开流时，JVM都会为这些资源分配内存。如果没有关闭连接，会导致持续占有内存。
3. **使用 ThreadLocal：**

### 4. (中高频) Java 栈帧中存了什么？

1. 局部变量表：存储方法的局部变量，包括方法形参和方法内部定义的变量；
2. 操作数栈：在方法执行的过程中存储**中间计算结果和操作数**；
3. 动态链接：在方法调用时将编译得到的符号引用解析为实际内存地址，以便执行；
4. 方法的返回地址：当一个方法调用另一个方法时，调用指令的下一条指令地址会被存储在**栈帧**中，方法执行完毕后，JVM 会使用这个返回地址继续执行剩余的指令；
5. 其他信息：异常处理表，调试信息等。

### 5. **Jvm 内存分布，有垃圾回收的是哪些地方？**

垃圾回收主要发生在堆和方法区（或元空间）中。

在堆中，垃圾回收器会自动回收不再被引用的对象，释放内存空间。在方法区（或元空间）中，垃圾回收主要针对无用的类和常量进行回收。

需要注意的是，不同的垃圾回收器有不同的工作方式和策略，如串行回收器、并行回收器、并发回收器等。它们会根据具体的配置和场景来决定何时进行垃圾回收以及如何回收。

### 6. Jvm 堆内存缓慢增长如何定位哪行代码出问题？



### 7. Jvm 内存模型？自己一般开多大的堆内存？

### 

### 8. 在 Jvm 里如何确认对象的内存大小？

------

## 三、类加载过程

### 1. (中高频) 什么是类加载？类加载过程？

Java类加载过程是指将类字节码文件加载到JVM中，并转换为可执行的Java类的一系列步骤。

1. 加载
   首先类加载器通过全类名找到.class 文件，将文件内容读取到内存中形成二进制字节流，接着将字节流所代表的静态存储结构转换为方法区的运行时数据结构，最后在堆内存中生成一个代表该类 Class 对象，作为方法区数据的访问入口。
2. 链接

1. 验证阶段：确保字节流文件中包含的信息符合 JVM 规范，包括文件格式验证，元数据验证，字节码验证，符号引用验证。
2. 准备阶段：为类的静态变量分配内存并设置初始值（final修饰在准备阶段就会被赋予初始值）
3. 解析阶段：将运行时常量池中的符号引用转化为直接引用（这些符号引用在编译期间无法被解析成具体的内存地址，不同环境下实际物理位置不同）

1. 初始化**
   **将类的静态变量初始化为默认值并执行类的静态代码块

### 2. (中高频) 类加载器加载的先后顺序？

1. 启动类加载器（Bootstrap ClassLoader）：负责加载Java核心类库，如 java.lang 包中的类，它是由 C/C++ 实现的，是最顶层的类加载器。
2. 扩展类加载器（Extension ClassLoader）：负责加载 Java 的扩展库，位于 jre/lib/ext 目录下的类，比如加密库，网络通信库，图形处理库
3. 应用程序类加载器（Application ClassLoader）：也称为系统类加载器，负责加载用户类路径（ClassPath）上指定的类库。

1. 自定义类加载器

**加载规则**：自底向上查找判断类是否被加载，然后再自顶向下尝试加载类

### 3. (中高频) 什么是双亲委派机制？作用？什么时候失效

类加载过程遵循**双亲委派模型**，即类加载请求先委托给父类加载器处理，如果父类加载器无法加载，再由当前加载器尝试加载。
这一机制确保了核心类库的优先加载，避免了同一个类的重复加载和类型安全问题。



补充：

重复加载问题：如果父类加载器已经加载过这个类，它就会直接返回已加载的Class对象，而不会重新加载类。

类型安全问题：即使两个类的字节码完全相同，但如果它们由不同的类加载器加载，它们在JVM中被认为是不同的类，属于不同的命名空间。两个类实例在互相比较时，即使它们看起来相同，也会被认为是不同的类型，导致ClassCastException等运行时异常。

------

## 四、垃圾回收机制

### 1. 为什么需要GC？

### 

### 2. 谈谈你对Minor GC还有Full GC的理解？MinorGC会发生stop the world现象吗？

**MinorGC** 主要针对清理Java堆中的新生代（Eden区和两个Survivor区）。新创建的对象会被分配到Eden区，**Eden区满了会触发Minor GC**。Minor GC过程中，Eden区中存活的对象会被移动到其中一个Survivor区，而经过一定次数的GC后仍然存活的对象会被移到另一个Survivor区，同时将这些对象的年龄加一。达到一定年龄阈值的对象会被晋升到老年代。

**Minor GC** 会触发一次短暂的STW事件，为保证新对象的分配和存活对象的复制不受影响，所有线程都会暂停，直到GC完成。

------

**Full GC** 会清理整个堆内存，包括新生代、老年代。

Full GC触发条件包含以下几个方面：

- 当老年代空间不足以容纳新创建的对象时，会触发**Full GC**
- 当**Minor GC**无法为对象分配空间或晋升到老年代时
- 对象在永久代中达到GC条件，会触发Full GC

**Full GC**会导致更长时间的STW事件，因为一般需要扫描整个堆内存进行整理，所以一般会对性能有影响。

### 3. MinorGC还有Full GC分别在什么时候发生？





### 4. 如何判断对象是否死亡？

*引用计数法*

给对象添加一个引用计数器，有一个地方引用，计数器加1；每当引用失效，计数器就减1。

*可达性分析算法*

把一部分对象作为**GC Roots**，将GC Roots作为起点，从这些节点开始向下搜索，节点走过的所有路径称为引用链。当一个对象到GC Roots没有任何引用链相连的话，则证明此对象是不可用的，需要被回收。

### 5. (中高频) 引用类型？

- 

1. *强引用*
   最常用的引用类型，当一个对象被强引用引用时，垃圾回收器永远不会回收它。
   默认情况创建对象就是强引用。
2. *弱引用*
   用于描述非必需对象，弱引用的对象只能生存到下一次垃圾回收发生之前。当垃圾回收器运行时，无论内存是否充足，都会回收被弱引用关联的对象。
   适用于实现规范化映射和解决内存泄露问题
3. *软引用*
   用于描述一些还有用但并非必需的对象，对于只有软引用的对象，在内存不足时会被回收。
   适用于实现内存敏感的缓存
4. *虚引用
   *最弱的一种引用类型，虚引用的存在不会决定对象的生命周期。唯一的作用是在这个对象被回收时收到一个系统通知。
   主要用于跟踪对象被垃圾回收的活动

### 6. 什么对象被垃圾回收？

1. 如果一个对象没有任何引用指向它。 
2. 如果一个对象只被弱引用或软引用所引用。

### 7. 哪些对象可以作为GC Roots呢？堆上的对象可以作为GC Roots吗？

1. 虚拟机栈(栈帧中的局部变量表)中引用的对象
2. 本地方法栈(Native 方法)中引用的对象
3. 方法区中类静态属性引用的对象
4. 方法区中常量引用的对象
5. 所有被同步锁持有的对象
6. JNI（Java Native Interface）引用的对象

不能，GC Roots是一组特定的根对象，作为起始点来遍历和标记活跃的对象，从而确定哪些对象死亡。堆中的普通对象一般被GC Roots直接或间接引用的，而不是作为GC Roots存在的。



### 8. (高频) 有哪些垃圾回收算法？

*1.标记-清除算法*

分为标记和清除两个阶段：首先标记出**需要回收的对象**，在标记完成后统一回收掉垃圾。

效率问题：标记、清除的效率都不高。

空间问题：标记清除后会产生大量不连续的内存碎片，程序需要分配较大对象时无法找到足够的连续内存

![img](https://cdn.nlark.com/yuque/0/2024/png/42782944/1719955333042-f0b42ea7-f9f4-404f-9557-c41e8cae8fc7.png)

*2.复制算法*

将内存空间分为大小相同的两块，每次只使用其中一半。使用完成后将存活对象复制到另外一块中去，然后将使用的空间一次回收掉，这样就保证了内存空间的连续性。

问题：

1. 因为缩小到原来一半，可用内存变小；
2. 不适合老年代，如果存活对象数量多，复制过程消耗系统性能

![img](https://cdn.nlark.com/yuque/0/2024/png/42782944/1719955373784-6dfe4b1a-1057-4f4f-9bfb-9bffc71cea53.png)

*3.标记-整理算法*

标记过程仍然与“标记-清除”算法一样，但后续步骤不是直接对可回收对象回收，而是让所有存活的对象向一端移动，然后直接清理掉端边界以外的内存。

标记整理算法一方面在标记-清除算法上做了升级，解决了内存碎片的问题，也规避了复制算法只能利用一半内存区域的弊端，但内存变动更频繁，需要整理所有存活对象的引用地址，在效率上比复制算法差很多。

![img](https://cdn.nlark.com/yuque/0/2024/png/42782944/1719707435850-2b9e3738-cef4-4ed1-89dd-50ea3e9647d4.png)

*4.分代收集算法*

Java堆分为新生代和老年代，根据各个年代特点选择合适的垃圾收集算法。

比如在新生代中，每次回收都会有大量对象死去，一般选择复制算法，优点是每次只需要复制少量对象就可以完成回收。
而老年代的对象存活几率是比较高的，而且没有额外的空间对它进行分配，所以我们必须选择“标记-清除”或“标记-整理”算法进行垃圾回收。

### 9. (高频) 垃圾回收器？



### 10. 说说CMS垃圾回收器？和g1区别？

### 11. G1垃圾回收的过程？

G1是一款面向服务器的垃圾收集器，主要针对配备多颗处理器及大容量内存的机器。

### 



### 12. 如何确定哪个对象占用堆内存大？





### 13. 对Key的弱引用被垃圾回收是否会造成内存泄漏







### 14. 并发收集阶段再次触发full gc怎么处理？