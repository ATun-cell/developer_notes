主要定义了对于一个共享变量，当另一个线程对这个共享变量执行写操作后这个线程对这个共享变量的可见性
# 从CPU缓存模型说起
****
CPU缓存存在内存缓存不一致的问题 为了解决这个问题可以通过指定缓存一致协议（比如MESI协议）CPU缓存与主内存交互的时候需要遵守的原则和规范

# 指令重排序
****
系统在执行代码的时候会进行指令重排序，不一定按照尼代码的顺序依次执行
- 编译器优化重排
- 指令并行重排

Java原代码会经历*编译器优化重排* ，*指令并行重排* ，*内存系统重排* 过程才变成操作系统可执行的指令序列
指令重排序可以保证串行语义一致但是没有义务保证多线程间语义也一致


# JMM
****
为什么需要JMM，时Java提供的内存模型，Java的跨系统特性依赖于提供内存模型来屏蔽不同操作系统不同内存模型的差异
并且对Java来说，JMM是定义的并发编程相关的一组规范，主要目的是为了简化多线程编程。可以直接使用并发相关的关键字和类（比如volatile、synchronized、各种lock）

## 如何抽象线程和主内存之间的关系
- *主内存*  所有线程创建的实例对象及其内部信息都存放在主内存中
- *本地内存*  每个线程都有一个私有的本地内存，存储了该线程用以读写共享变量的副本。每个线程只能操作自己本地内存中的变量，无法直接访问其他线程的本地内存，只能通过主内存进行通信。本地内存并不存在只是一个概念
JMM为共享变量提供了可见性的保障

## happens-before原则是什么
![[Pasted image 20250623161636.png]]
![[Pasted image 20250623161818.png]]

# 并发编程的三个特性
****
*原子性*  
*可见性* 将变量声明为volatile就意味这个变量是共享的，每次使用都到主存中进行读取
*有序性* volatile关键字可以禁止指令进行重排序优化 通过插入内存屏障的方式