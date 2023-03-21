# 重构 007 —提取类

> 原文：<https://medium.com/javarevisited/refactoring-007-extract-class-5b0b5960d0e7?source=collection_archive---------1----------------------->

## 行为在整个系统中重复。但是我们遗漏了一个概念

[![](img/5cff62f5eb715be3a7d27b3fe29b7471.png)](https://javarevisited.blogspot.com/2020/12/top-5-course-to-improve-coding-skills.html)

> *TL；博士:把该在一起的放在一起*

# 解决的问题

*   代码复制
*   缺失的抽象
*   低内聚力

# 相关代码气味

[](https://mcsee.medium.com/code-smell-124-divergent-change-48825bcc74d0) [## 代码气味 124 —不同的变化

### 你在课堂上改变了一些东西。你在同一个类中改变一些不相关的东西

mcsee.medium.com](https://mcsee.medium.com/code-smell-124-divergent-change-48825bcc74d0) [](https://levelup.gitconnected.com/code-smell-143-data-clumps-86cfe2f36a19) [## 代码气味 143 —数据块

### 有些物体总是在一起。我们为什么要分开他们？

levelup.gitconnected.com](https://levelup.gitconnected.com/code-smell-143-data-clumps-86cfe2f36a19) 

# 步伐

1.  提取耦合到一个新概念中的方法(偶尔还有属性)
2.  使用新概念

# 示例代码

## 以前

```
final class Person { private String name; // Below cohesive properties
      private String homeAreaCode;
      private String homeNumber; public String name() {
          return name;
      } // Below cohesive behaviour
      public String telephoneNumber() {
          return ("(" + homeAreaCode + ") " + homeNumber);
      }
      String areaCode() {
          return homeAreaCode;
      }
      String officeNumber() {
          return officeNumber;
      } 
 }
```

## 在...之后

```
// 1\. Extract the methods (and accidentally the properties) coupled into a new concept      
   public class TelephoneNumber { private String number;
      private String areaCode; public String telephoneNumber() {
          return ("(" + areaCode + ") " + _number);
      }
      public String areaCode() {
          return areaCode;
      }
      public String number() {
          return number;
      }
   }final class Person { private String name; // 2\. Use the new concept
      private TelephoneNumber officeTelephone = new TelephoneNumber(); public String name() {
          return name;
      }
      public String telephoneNumber(){
          return officeTelephone.getTelephoneNumber();
      } }
```

# 类型

[X]自动

像 [IntelliJIDEA](/javarevisited/7-best-courses-to-learn-intellij-idea-for-beginners-and-experienced-java-programmers-2e9aa9bb0c05) 和 [Eclipse](/javarevisited/top-10-courses-to-learn-eclipse-junit-and-mockito-for-java-developers-4de1e8d62b96) 这样的大多数 ide 都实现了这种安全重构。

# 为什么代码更好？

逻辑代码和它的规则在同一个地方

# 标签

*   班级

# 请参见

[Refactoring.com](https://refactoring.com/catalog/extractClass.html)

[重构大师](https://refactoring.guru/extract-class)

[重构课程](/javarevisited/7-best-courses-to-learn-refactoring-and-clean-coding-in-java-47bea3c67006)

# 信用

来自 [Pixabay](https://pixabay.com/es/) 上 [drpepperscott230](https://pixabay.com/es/users/drpepperscott230-1212529/) 的图像

本文是重构系列的一部分。