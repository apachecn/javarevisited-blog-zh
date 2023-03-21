# 如何在 Java 中使用线程打印偶数和奇数

> 原文：<https://medium.com/javarevisited/how-to-print-even-and-odd-numbers-using-threads-in-java-java2blog-3188249cb48f?source=collection_archive---------0----------------------->

![](img/b5f9acda2c1f6e8f1b5c386a23c0d57b.png)

澳大利亚八月在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

在这篇文章中，我们将看到如何在 java 中使用[线程](https://java2blog.com/java-thread-example/)打印偶数和奇数。

参见:[如何在 java 中使用 3 个线程打印序列](https://www.java2blog.com/print-sequence-3-threads-java/)

> 你有两条线索。你需要用一个线程打印奇数，用另一个线程打印偶数。您需要以自然顺序打印，直到最大值。
> **例如:**
> 如果 MAX 为 10，则需要打印:
> 
> 因此 1 3 5 7 9 将由奇数线打印
> 2 4 6 8 10 将由偶数线打印。

我们将使用 wait 和 notify 来解决如何在 java 中使用[线程](https://javarevisited.blogspot.com/2018/07/java-multi-threading-interview-questions-answers-from-investment-banks.html)打印偶数和奇数。

*   使用一个名为布尔[奇数](https://java2blog.com/even-odd-program-java/)的变量。如果你想打印奇数，它的值应该是真的，反之亦然。
*   创建两个方法(printOdd 和 printEven)，一个打印奇数，另一个打印偶数。
*   创建两个线程，一个用于奇数线程，一个用于偶数线程。
*   t1 将调用 printEven 方法，t2 将同时调用 printOdd 方法。
*   如果 printEven 方法中布尔奇数为真，t1 将等待。
*   如果 printOdd 方法中的 boolean odd 为 false，t2 将等待。

# 在 java 中使用线程打印偶数和奇数

```
package org.arpit.java2blog;

public class OddEvenPrintMain {

	boolean odd;
	int count = 1;
	int MAX = 20;

	public void printOdd() {
		synchronized (this) {
			while (count < MAX) {
				System.out.println("Checking odd loop");

				while (!odd) {
					try {
						System.out.println("Odd waiting : " + count);
						wait();
						System.out.println("Notified odd :" + count);
					} catch (InterruptedException e) {
						// TODO Auto-generated catch block
						e.printStackTrace();
					}
				}
				System.out.println("Odd Thread :" + count);
				count++;
				odd = false;
				notify();
			}
		}
	}

	public void printEven() {

		try {
			Thread.sleep(1000);
		} catch (InterruptedException e1) {
			e1.printStackTrace();
		}
		synchronized (this) {
			while (count < MAX) {
				System.out.println("Checking even loop");

				while (odd) {
					try {
						System.out.println("Even waiting: " + count);
						wait();
						System.out.println("Notified even:" + count);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				}
				System.out.println("Even thread :" + count);
				count++;
				odd = true;
				notify();

			}
		}
	}

	public static void main(String[] args) {

		OddEvenPrintMain oep = new OddEvenPrintMain();
		oep.odd = true;
		Thread t1 = new Thread(new Runnable() {

			@Override
			public void run() {
				oep.printEven();

			}
		});
		Thread t2 = new Thread(new Runnable() {

			@Override
			public void run() {
				oep.printOdd();

			}
		});

		t1.start();
		t2.start();

		try {
			t1.join();
			t2.join();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}

	}
}
```

当您运行上面的程序时，您将得到下面的输出:

> 检查奇循环
> 奇线程:1
> 检查奇循环
> 奇等待:2
> 检查偶循环
> 偶线程:2
> 检查偶循环
> 偶等待:3
> 通知奇:3
> 奇线程:3
> 检查奇循环
> 奇等待:4
> 通知偶:4
> 偶线程:4
> 检查偶循环
> 偶等待:5
> 通知奇:5
> 奇线程:5
> 通知偶:6
> 偶线程:6
> 检查偶循环
> 偶等待:7
> 通知奇:7
> 奇线程:7
> 检查奇循环
> 奇等待:8
> 通知偶:8
> 检查偶循环
> 偶等待:9
> 通知奇:9
> 奇线程:9
> 检查奇循环
> 奇等待:10【T33 奇:11
> 奇线程:11
> 检查奇循环
> 奇等待:12
> 通知偶:12
> 偶线程:12
> 检查偶循环
> 偶等待:13
> 通知奇:13
> 奇线程:13
> 检查奇循环
> 奇等待:14
> 通知偶:14
> 偶线程:14
> 检查偶循环
> 偶 偶数:16
> 偶数线程:16
> 检查偶数循环
> 偶数等待:17
> 通知奇数:17
> 奇数线程:17
> 检查奇数循环
> 奇数等待:18
> 通知偶数:18
> 偶数线程:18
> 检查偶数循环
> 偶数等待:19
> 通知奇数:19
> 奇数线程:

如果你观察输出，你应该能够理解上述程序。

我来试着解释一下前几行:
**检查奇循环:** t2 检查 printOdd 方法中的 while 条件
**奇线程:1 :** t2 打印计数，加 1 并使 odd =false
**检查奇循环:**检查 printOdd 方法中的 while 条件
**奇等待:2:** 既然 odd =false 现在， t2 将等待并释放锁
**检查偶数循环:** t1 检查 printEven 方法中的 while 条件
**偶数线程:2 :** t1 打印计数，递增 1 并使 odd =true
**检查偶数循环:** t1 检查 printEven 方法中的 while 条件
**偶数等待:3:** 既然 odd =true，t1 将等待并释放锁【T94

所有其他步骤都会随之而来。

# 解决方案 2:使用余数

你可以在这里使用余数[的概念。](http://www.java67.com/2014/11/modulo-or-remainder-operator-in-java.html)

*   如果数字%2==1，Odd 将打印该数字并递增，否则将进入等待状态。
*   如果数字%2==0，Even 将打印该数字并递增，否则将进入等待状态。

让我们借助例子来检查一下。

创建一个名为“OddEvenRunnable”的类，并实现 [Runnable](https://www.java2blog.com/java-runnable-example/) 接口。

```
public class OddEvenRunnable implements Runnable{

	public int PRINT_NUMBERS_UPTO=10;
	static int  number=1;
	int remainder;
	static Object lock=new Object();

	OddEvenRunnable(int remainder)
	{
		this.remainder=remainder;
	}

	@Override
	public void run() {
		while (number < PRINT_NUMBERS_UPTO) {
			synchronized (lock) {
				while (number % 2 != remainder) { // wait for numbers other than remainder
					try {
						lock.wait();
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
				}
				System.out.println(Thread.currentThread().getName() + " " + number);
				number++;
				lock.notifyAll();
			}
		}
	}
}
```

创建名为“PrintOddEvenMain”的主类

```
public class PrintOddEvenMain {
	public static void main(String[] args) {

		OddEvenRunnable oddRunnable=new OddEvenRunnable(1);
		OddEvenRunnable evenRunnable=new OddEvenRunnable(0);

		Thread t1=new Thread(oddRunnable,"Odd");
		Thread t2=new Thread(evenRunnable,"Even");

		t1.start();
		t2.start();

	}
}
```

当你运行上面的程序时，你会得到下面的输出

> 奇数 1
> 偶数 2
> 奇数 3
> 偶数 4
> 奇数 5
> 偶数 6
> 奇数 7
> 偶数 8
> 奇数 9
> 偶数 10

这都是关于在 java 中使用线程打印偶数和奇数。解释不太清楚的请评论。

您可能还喜欢:

*   [Java 多线程面试问题](https://java2blog.com/java-multithreading-interview-questions-and-answers/)。
*   [Java 面试问题](https://java2blog.com/java-interview-questions/)
*   [Java 面试题 5 年经验](https://java2blog.com/java-interview-questions-for-5-years-experience/)
*   [Java 集合面试问题](https://java2blog.com/java-collections-interview-questions/)
*   [Java 线程示例](https://java2blog.com/java-thread-example/)
*   [字符串编码面试问题](https://hackernoon.com/20-string-coding-interview-questions-for-programmers-6b6735b6d31c)

*原载于 2019 年 5 月 29 日*[*https://java2blog.com*](https://java2blog.com/print-even-odd-numbers-threads-java/)*。*

**进一步学习**
[10 本算法书每个程序员都应该读的](http://www.java67.com/2015/09/top-10-algorithm-books-every-programmer-read-learn.html)
[Java 开发人员的前 5 本数据结构和算法书](http://javarevisited.blogspot.sg/2016/05/5-free-data-structure-and-algorithm-books-in-java.html#axzz4uXETWjmV)
[从 0 到 1:数据结构&Java 中的算法](https://click.linksynergy.com/fs-bin/click?id=JVFxdTr9V80&subid=0&offerid=323058.1&type=10&tmpid=14538&RD_PARM1=https%3A%2F%2Fwww.udemy.com%2Ffrom-0-to-1-data-structures%2F)
[数据结构和算法分析—求职面试](https://click.linksynergy.com/fs-bin/click?id=JVFxdTr9V80&subid=0&offerid=323058.1&type=10&tmpid=14538&RD_PARM1=https%3A%2F%2Fwww.udemy.com%2Fdata-structure-and-algorithms-analysis%2F)
[50+程序员的数据结构和编码问题](https://dev.to/javinpaul/50-data-structure-and-algorithms-problems-from-coding-interviews-4lh2)