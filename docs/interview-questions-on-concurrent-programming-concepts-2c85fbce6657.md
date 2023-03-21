# 关于并发编程概念的面试问题

> 原文：<https://medium.com/javarevisited/interview-questions-on-concurrent-programming-concepts-2c85fbce6657?source=collection_archive---------1----------------------->

## 面试经验和指南—第五部分

## 软件工程并发编程概念的面试准备

*大家好*😊*，今天我将分享一些常见的并发编程相关的面试问题，作为我的软件工程师面试准备系列的一部分。这是该系列的第五部分。如果错过了前几期，可以在这里阅读第一部分*[](/geekculture/interview-preparation-kid-for-software-engineer-1380f6fcbae9)**[*OOP&Java*](/javarevisited/interview-questions-on-object-oriented-programming-and-java-41b027d93ddb)*[*数据库*](https://faun.pub/interview-questions-on-database-concepts-d480defce050)*[*数据结构*](/geekculture/interview-questions-on-data-structures-417761216620) *。不再拖延，让我们进入今天的主题并发编程。*****

**[![](img/4353c5fc4a3923252f43a78a44ecfed2.png)](https://www.java67.com/2022/03/top-8-free-and-paid-java-multithreading.html)

托马斯·索贝克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片** 

1.  ****解释以下与并发编程相关的基本术语****

*   **正在执行的程序通常被称为进程。**
*   ****线程:**线程是进程的最小部分(轻量级进程)，一个进程由多个线程组成。**
*   ****竞态条件:**竞态条件是一种令人不快的情况，当一个设备或系统试图同时执行两个或多个操作，但由于该设备或系统的性质，这些活动必须按正确的顺序执行。竞态条件通常与计算机科学和编程联系在一起。当两个计算机程序进程(线程)试图同时访问同一资源时，就会发生这种情况，从而导致系统问题。**
*   ****同步:**同步是指调节多个线程对共享资源的访问的能力。多线程概念下多个线程试图同时访问共享资源，导致不可预知的效果。线程间的可靠通信需要同步。**
*   ****互斥:**互斥(mutex)对象是一个阻止几个用户同时访问一个共享资源的软件对象。对于关键部分，即进程或线程访问公共资源的代码块，这种思想被用于并发编程。**
*   ****饥饿:**指一个线程由于缺乏对共享资源的频繁访问而无法取得进展的情况。当“贪婪”线程使共享资源长时间不可用时，就会发生这种情况。考虑一个具有同步方法的对象，该方法经常需要很长时间才能返回。当一个线程重复调用这个函数时，其他需要频繁同步访问同一个对象的线程会被频繁阻塞。**

**[![](img/9920a489d1a1c52b5b6afe014eb7f1a8.png)](https://javarevisited.blogspot.com/2018/06/top-5-java-multithreading-and-concurrency-courses-experienced-programmers.html)

照片由[哈桑·帕夏](https://unsplash.com/@hpzworkz?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄** 

*   **[**死锁**](/hackernoon/how-to-avoid-a-deadlock-while-writing-concurrent-programs-java-example-988bb07db25f) **:** 死锁是两个或多个线程无限期地等待对方的场景。当几个线程需要相同的锁，但以不同的顺序接收时，就会出现死锁。**
*   ****活锁:**一个线程会频繁地对另一个线程的动作做出反应。如果另一个线程的活动同样是对另一个线程的动作的反应，则可能发生活锁。活锁线程与死锁线程一样，无法向前移动。另一方面，线程没有被阻塞；他们只是太专注于回应彼此，而无法继续工作。这就像两个人试图在走廊里擦肩而过:阿桑移到他的右边让马赫拉通过，而马赫拉移到他的左边让阿桑通过。马赫拉走到他的右边，而阿桑移到他的左边，因为他们继续阻止对方。他们仍在阻碍彼此的进步。**

**![](img/b4226cb6f00cd703a8e587ec839c25f5.png)**

**照片由[日常基础](https://unsplash.com/@zanardi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄**

*   **[**锁**](https://javarevisited.blogspot.com/2013/03/reentrantlock-example-in-java-synchronized-difference-vs-lock.html) :它是一种同步块的机制。锁可以在不同的方法中调用 lock()和 unlock()。**

****2。易变变量是什么意思？****

*   ****Volatile** 是 Java 中的变量类型。它用来告诉 JVM，访问变量的线程必须总是将自己的变量私有副本与内存中的主副本合并。**
*   **访问一个**可变**变量会同步内存中该变量的所有缓存副本。它保证了线程间变量变化的可见性。**

****3。什么是并发编程中的监视器？****

*   **在并发编程中，监视器是一个由多个线程安全使用的对象或模块。**
*   **运行时线程同步。**
*   **监视器与特定的数据和功能相关联。**
*   **锁为实现监视器提供了必要的支持。**
*   **监视器是一组一次只能由一个线程运行的代码。如果另一个线程试图与当前线程同时访问，它将被停止，直到当前线程释放监视器。**

****4。比较同步方法和同步块****

*   **限制访问对象的方法称为同步方法。获取方法的对象或类的锁后，线程执行同步方法。同步语句和同步方法是相似的。只有当线程获得 synchronized 语句中提到的对象或类的锁时，它才能运行。**

****5。线程转储是什么意思？****

*   **线程转储是 Java 进程中所有线程状态的快照。每个线程的状态都有一个堆栈跟踪，显示线程堆栈的内容。**
*   **对于诊断问题非常有用，因为它显示了线程的活动。**

****6。Java 编程中线程是如何实现的？****

*   **java 中的线程实现有两种方式。**
*   ****扩展线程类**:Java 中不允许多重继承，因此扩展线程类的类不能扩展任何其他类。如果用户希望覆盖 Thread 类中的其他方法，他们必须扩展它。每个线程产生一个与其相关的不同对象。因为每个线程创建一个单独的对象，所以需要更多的内存。**
*   ****实现 Runnable 接口**:如果一个类定义了一个实现 Runnable 接口的线程，它就有可能扩展另一个类。如果您只想自定义 run 函数，实现 Runnable 是一种更好的方法。相同的项目由几个线程共享。因为许多线程共享同一个对象，所以减少了内存的使用。**

**7.**为线程安全的 Singleton 设计模式编写 Java 代码，并解释代码的每一行？****

*   **它是一种用于创建对象的创造性设计模式。这种设计模式的主要目标是在程序的生命周期中只维护类的一个实例。**
*   ****第 2 行:** volatile 是指这个变量的值永远不会被线程本地缓存，所有的读写都会直接行进到主存。该变量被访问，就好像它被包含在一个自身同步的[同步块](https://www.java67.com/2013/01/difference-between-synchronized-block-vs-method-java-example.html)中一样。它基本上说明了在读取一个变量时，永远不要在自己的本地内存中查找它；相反，你应该到主存中查找。**
*   ****第 4 行:**它的目的是防止另一个类调用它的构造函数，产生一个 singleton 实例。我们确保没有其他类可以通过声明它的私有构造函数来调用它**
*   ****第 7 行到第 10 行:**在一个特定的应用程序中，同步进程是非常昂贵的操作。因此，程序员应该只在绝对必要的时候使用它。因此，执行双重检查以进行优化。仅当实例为空时，才使用 Synchronized。**

**8.**关于读者-作者问题的解释？****

**![](img/d413e7f69c14b12b0c3032fe15f95123.png)**

**照片由[法比奥拉·佩尼亚巴](https://unsplash.com/@fabspotato?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄**

*   **读取器-写入器问题涉及由许多程序使用的共享对象，例如文件。这些进程中的一些是读取器，从某种意义上说，它们只是想从对象中读取数据，而另一些是写入器，从某种意义上说，它们想将数据写入对象。**
*   **读取器-写入器问题用于维护同步并确保对象数据不被破坏。例如，如果两个读者同时访问对象，就没有困难。但是，如果两个编写器或一个读取器和编写器同时访问项目，可能会出现问题。**
*   **为了解决这个问题，作者应该拥有对项目的独占访问权，这意味着当他或她正在处理项目时，其他作者或读者不应该能够访问它。另一方面，多个读者可以同时使用该项目。信号量或读写锁可以用来做到这一点。**

**9.**什么是死锁处理中的鸵鸟算法？****

**![](img/8b535d346da87b8118bd8603d2f0cfbf.png)**

**由[萨阿德汗](https://unsplash.com/@sakhan88?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片**

*   **鸵鸟算法是一种计算机科学方法，用于忽略可能出现的问题，其基础是假设这些问题极其罕见，“将你的头埋在沙子里，假装没有问题。”这种观点认为，放任问题发展比试图阻止它更具成本效益。**
*   **它有时用于死锁处理。**

**10.**任务延迟和吞吐量是什么意思？****

*   ****延迟**:提交请求后，完成工作所需的时间。**
*   ****吞吐量**:在指定时间内完成的工作总数。**

**11.**什么是多线程？****

*   **同时操作许多线程的过程称为多线程。基本的优点是所有的线程都有相同的地址空间，线程保持轻量级，线程到线程的通信是廉价的。**

**12.**为什么线程行为如此不可预测？****

*   **因为线程执行依赖于**线程调度器**，我们可以说线程行为是不可预测的。了解每个线程调度器在各种平台(如 Windows、Unix 等)上都有不同的实现非常重要。**

**13.**什么是线程池？****

*   **ThreadPool 是一个线程池，它重用预定数量的线程来完成任务。例如:Redis 中的 Jedis 线程池。这里看[这里看](/geekculture/the-pooling-of-connections-in-redis-e8188335bf64)。**

**14.[**在 Java 中，notify 和 notifyAll 有什么区别？**](https://javarevisited.blogspot.com/2012/10/difference-between-notify-and-notifyall-java-example.html#axzz6dHZ7oEpK)**

**[![](img/12c680dfdd1d8db41bd3db65921259c9.png)](https://javarevisited.blogspot.com/2012/10/difference-between-notify-and-notifyall-java-example.html#axzz6dHZ7oEpK)

照片由 [Kinga Cichewicz](https://unsplash.com/@all_who_wander?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄** 

*   **因为 [Notify()函数](https://javarevisited.blogspot.com/2017/02/10-java-wait-notify-locking-and-synchronization-Interview-Questions-Answers.html)不允许你选择一个特定的线程，所以只有当一个线程。而 notifyAll()向所有线程发送通知。这也给了他们竞争锁的机会。它还保证至少有一个线程会继续。**

**15.[**thread . start()和 Thread.run()方法的主要区别是什么？**](https://javarevisited.blogspot.com/2012/03/difference-between-start-and-run-method.html#axzz6vPUwyVzv)**

*   **线程类的 **start()** 函数(native method)负责启动线程。一个线程的 **run()** 函数。所以如果我们直接调用 Thread。 **run()** 方法与 **run()** 函数使用相同的线程。这样一来，启动一个新线程的目标就永远无法实现。**

**我相信你已经理解了上面讨论的与面试并发编程相关的所有问题。如果您有任何问题或任何澄清，不要犹豫，通过回复部分与我联系。感谢你花宝贵的时间阅读这篇博客，我相信这会激励你继续阅读其他关于面对面试的博客。面试准备的前几部分可以在[这里](/@sthenusan)找到。这些对你准备面试会很有帮助。**

***喜欢这篇文章吗？成为* [*中等会员*](https://sthenusan.medium.com/membership) *继续学习没有任何限制。如果你使用上面的链接，我会收到你的一部分会员费，不需要你额外付费。提前感谢。***

**![](img/c20766cf7105311ca13cd46e6ae0fd2d.png)**