# 异常中的最终阻塞(JAVA)

> 原文：<https://medium.com/javarevisited/finally-block-in-exception-java-187cc87e69ce?source=collection_archive---------4----------------------->

为了确保程序的所有状态都被执行，我们通过编写一个 [try-and-catch 块来处理异常。](https://javarevisited.blogspot.com/2013/03/0-exception-handling-best-practices-in-Java-Programming.html)

在某些情况下，catch 块之后的语句不会被执行:

1.当 try 或 catch 块中有 return 语句时

2.当 try 块中发生异常并且匹配的 catch 块不可用时。

3.当我们硬推代码从 try 块 [System.exit(0)执行时；](https://javarevisited.blogspot.com/2014/11/dont-use-systemexit-on-java-web-application.html)

当你想在 catch 块之后执行语句时，你必须把这些语句放在 finally 块中。

运行时系统总是执行 [finally 块](https://javarevisited.blogspot.com/2012/11/difference-between-final-finally-and-finalize-java.html)中的语句，而不管 try 块中发生了什么

```
public class TestFinallyBlock2{    
      public static void main(String args[]){   

      try {    

        System.out.println("Inside try block");  

        //below code throws divide by zero exception  
       int data=25/0;    
       System.out.println(data);    
      }   

      //handles the Arithmetic Exception / Divide by zero exception  
      catch(ArithmeticException e){  
        System.out.println("Exception handled");  
        System.out.println(e);  
      }   

      //executes regardless of exception occured or not   
      finally {  
        System.out.println("finally block is always executed");  
      }    

      System.out.println("rest of the code...");    
      }    
    } 
```

# **System.exit(0)**

`System.exit(0)`是`java.lang.System`类中的一个方法，用于终止当前的 Java 虚拟机。它采用一个整数参数，表示虚拟机的退出状态。`0`状态表示虚拟机正常终止，而非零状态表示出现错误。

下面是一个如何在 Java 程序中使用`System.exit(0)`的例子:

```
public class Main {
  public static void main(String[] args) {
    // some code here
    System.exit(0);
  }
}
```

当调用`System.exit(0)`方法时，Java 虚拟机将立即终止，并且不会执行`main`方法中的任何剩余代码。

需要注意的是，调用`System.exit(0)`也会导致虚拟机中运行的任何非守护线程突然停止。这可能会导致意外行为，应谨慎使用。

一般来说，建议只在万不得已的情况下使用`System.exit(0)`，因为某种原因需要终止虚拟机的时候。在大多数情况下，最好让虚拟机在`main`方法执行完毕后自然终止。

![](img/a7d8c1ca5201216f8856b10e902b2fba.png)![](img/2265d2aed53e42edf985e9e23b94fe05.png)