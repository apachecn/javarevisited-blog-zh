# Java 8 流 API 及示例

> 原文：<https://medium.com/javarevisited/java-8-stream-api-with-examples-fc7b083e9ebb?source=collection_archive---------1----------------------->

[![](img/223b80bf341735469e0d0dd781b46024.png)](https://javarevisited.blogspot.com/2020/04/top-5-courses-to-learn-java-collections-and-streams.html)

在 [Java 8](/hackernoon/top-5-java-8-courses-to-learn-online-2db57d9dfb8d) 中引入的**流 API** 用于处理对象集合。流是支持各种方法的对象序列，这些方法可以通过流水线产生所需的结果。

Java 流的特点是

*   流不是一个[数据结构](/javarevisited/top-10-free-data-structure-and-algorithms-courses-for-beginners-best-of-lot-ad807cc55f7a)，而是从集合、数组或 I/O 通道获取输入。
*   [Streams](/javarevisited/7-best-java-tutorials-and-books-to-learn-lambda-expression-and-stream-api-and-other-features-3083e6038e14) 不改变原始的数据结构，它们只按照流水线方法提供结果。

在本文中，我们将通过一些例子来探索如何使用[流 API](https://www.java67.com/2018/03/java-8-stream-find-first-and-filter-example.html) 。

我们需要一个对象列表来使用流 API，我们将为我们的示例制作一个雇员列表。

## 创建员工类别

```
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import lombok.ToString;[@Data](http://twitter.com/Data)
[@NoArgsConstructor](http://twitter.com/NoArgsConstructor)
[@AllArgsConstructor](http://twitter.com/AllArgsConstructor)
[@ToString](http://twitter.com/ToString)
public class Employee {

 private String empId;
 private String firstName;
 private String lastName;
 private String email;
 private String gender;
 private String newJoiner;
 private int salary;
 private int rating;

}
```

使用[***public static void Main(String[]args){ }***函数](https://www.java67.com/2014/02/can-you-run-java-program-without-main-method.html)和返回雇员列表的静态方法创建一个主类

```
import java.util.Arrays;
import java.util.List;public class Main {
    public static void main(String[] args) {

        List<Employee> empList = getEmpList();}public static List<Employee> getEmpList(){
        return Arrays.asList(
                new Employee("59-385-1088","Zacharias","Schwerin","[zchwerin@gmail.com](mailto:zchwerin@gmail.com)","Male","True",101146,0),
                new Employee("73-274-6476","Kyle","Frudd","[kfrudd1@ovh.net](mailto:kfrudd1@ovh.net)","Male","FALSE",29310,2),
                new Employee("85-939-9584","Axe","Gumb","[agumb2@twitter.com](mailto:agumb2@twitter.com)","Female","FALSE",62291,4),
                new Employee("08-180-8292","Robinet","Batterham","[rbatterham3@last.fm](mailto:rbatterham3@last.fm)","Male","FALSE",142439,4),
                new Employee("21-825-2623","Ulick","Burrel","[uburrel4@google.ru](mailto:uburrel4@google.ru)","Male","FALSE",128764,5),
                new Employee("66-708-5539","Tailor","Ridding","Ridding","Female","FALSE",152924,4),
                new Employee("81-697-2363","Joete","Braybrooke","[jbraybrooke6@prnewswire.com](mailto:jbraybrooke6@prnewswire.com)","Male","TRUE",128907,0),
                new Employee("63-019-1110","Elroy","Baverstock","[ebaverstock7@ehow.com](mailto:ebaverstock7@ehow.com)","Male","TRUE",2510,0)
        );
    }
}
```

我们有一个员工列表，包含员工 Id、名字、姓氏、电子邮件、性别、新加入者、工资和评级。

# 过滤方法

我们将[过滤](https://javarevisited.blogspot.com/2018/05/java-8-filter-map-collect-stream-example.html)性别为**女**的员工列表

```
public class Main {
 public static void main(String[] args) {

 List<Employee> empList = getEmpList();
 empList.stream().filter(e -> e.getGender().equals("Female")).forEach(e -> System.out.println(e));
  }
```

*论执行:*

[![](img/0781486a491ebbfb78e34646c43d4fa2.png)](https://javarevisited.blogspot.com/2014/02/10-example-of-lambda-expressions-in-java8.html)

我们将[过滤【newJoiner 为**真**的员工列表](/javarevisited/how-to-use-streams-map-filter-and-collect-methods-in-java-1e13609a318b)

```
public class Main {
    public static void main(String[] args) {

        List<Employee> empList = getEmpList();
        empList.stream().filter(e -> e.getNewJoiner().equals("True")).forEach(e -> System.out.println(e));}
```

*执行上:*

![](img/c5957a8e3b1613ffa08ebe7bca061239.png)

# 排序方法

我们将根据**评级** asc 对员工名单进行[排序。](https://javarevisited.blogspot.com/2012/01/how-to-sort-arraylist-in-java-example.html)

```
public class Main {
    public static void main(String[] args) {

        List<Employee> empList = getEmpList();
        empList.stream()
        .sorted(Comparator.comparing(Employee::getRating))
        .forEach(e -> System.out.println(e));}
```

*执行时:*

[![](img/459677a5b10466a096624658f2af3bf7.png)](https://www.java67.com/2021/09/3-ways-to-sort-list-in-java-8-and-11.html)

我们将根据**对** desc 的评价对员工名单进行排序。

```
public class Main {
    public static void main(String[] args) {

        List<Employee> empList = getEmpList();
        empList.stream()
        .sorted(Comparator.comparing(Employee::getRating).reversed())
        .forEach(e -> System.out.println(e));}
```

*执行时:*

![](img/46b8a90f66530d3d0dcee3cd1cf4cbda.png)

我们将[根据**评级**和**薪资**](https://javarevisited.blogspot.com/2021/09/comparator-comparing-thenComparing-example-java-.html) 对员工列表进行排序

```
public class Main {
    public static void main(String[] args) {

        List<Employee> empList = getEmpList();

        empList.stream()
        .sorted(Comparator.comparing(Employee::getSalary))
        .sorted(Comparator.comparing(Employee::getRating))
        .forEach(e -> System.out.println(e));}
```

*执行上:*

[![](img/b861392cc9fa0c5df55c0246bc629091.png)](https://www.java67.com/2021/09/java-comparator-multiple-fields-example.html)

# 匹配方法

我们会看到所有工资超过 1000 的员工

```
 public static void main(String[] args) {

      List<Employee> empList = getEmpList();
      boolean isSalary = empList.stream()
                  .allMatch(e -> e.getSalary() > 1000);
          System.out.println(isSalary);}
```

*执行时:*

![](img/c544f1ce4ebfbfd9736e299549cd7906.png)

# 最大函数

我们将检索具有最高工资的员工

```
public class Main {
    public static void main(String[] args) {

      List<Employee> empList = getEmpList();
      empList.stream()
         .max(Comparator.comparing(Employee::getSalary))
         .ifPresent(System.out::println);}
```

*执行上:*

![](img/b5cbae8bcd8db8350dd231ebcc55dba3.png)

我们将检索评分最高的员工

```
public class Main {
    public static void main(String[] args) {

      List<Employee> empList = getEmpList();
      empList.stream()
         .max(Comparator.comparing(Employee::getRating))
         .ifPresent(System.out::println);}
```

*执行上:*

![](img/c5b43712bf5853f29ef40091781de7da.png)

# 最小函数

我们将检索最低工资的员工

```
 public class Main {
    public static void main(String[] args) {

      List<Employee> empList = getEmpList();
      empList.stream()
         .min(Comparator.comparing(Employee::getSalary))
         .ifPresent(System.out::println);}
```

*执行时:*

![](img/4cb5e433d6667965ebbb3f93e410f458.png)

我们将检索评分最低的员工

```
public class Main {
    public static void main(String[] args) {

      List<Employee> empList = getEmpList();
      empList.stream()
         .min(Comparator.comparing(Employee::getRating))
         .ifPresent(System.out::println);}
```

*执行上:*

![](img/bd1e5cbd35fe491ff4e6b062ca499438.png)

# GroupBy 函数

我们将按性别对所有员工进行分组

```
public class Main {
    public static void main(String[] args) {

      List<Employee> empList = getEmpList();
      Map<String, List<Employee>> employeesBygender = empList.stream()
                  .collect(Collectors.groupingBy(Employee::getGender));employeesBygender.forEach(((g,e)->{
              System.out.println(g);
              e.forEach(System.out::println);
          }));}
```

*执行时:*

[![](img/902a3a261b4e863a45beb6af2900bce9.png)](https://www.java67.com/2019/06/top-5-sorting-examples-of-comparator-and-comparable-in-java.html)

# 结论

我们已经看了几个简单的例子，如果你有兴趣学习更多关于 Java 8 流的知识，我推荐你查看官方的流 Javadoc 包文档。

希望这篇教程对你有帮助，并且你喜欢阅读它。

快乐编码..