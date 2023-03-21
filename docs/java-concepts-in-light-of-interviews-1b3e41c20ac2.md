# 访谈中的 Java 概念

> 原文：<https://medium.com/javarevisited/java-concepts-in-light-of-interviews-1b3e41c20ac2?source=collection_archive---------1----------------------->

本文将针对我在采访中问到的那些问题解释 Java 概念。

## Q1。两个相等的对象可以有不同的哈希码吗？

答案是否定的。为了解释这一点，我们必须看看 Java 数据结构的内部工作原理。java 中的每个对象都继承了两个方法。

1.  哈希码()
2.  等于()

“equals()”方法用于比较两个对象。例如，为了比较两个字符串“str1”和“str2”，可以在 Java 中使用 equals，如 str1.equals(str2)。“hashcode()”方法返回的哈希代码只是一个整数。像 HashMap 和 HashSet 这样的数据结构使用哈希代码来标识存储桶。例如，默认情况下，最初在一个 HashSet 中有 16 个桶。所以一个 HashSet 将使用哈希码的模数除以桶的总数(哈希码% 16)来标识桶号。如果一个对象的散列码是 20，那么 HashSet 将把这个对象放在第 4 个桶中(20 % 16 = 4)。如果使用“add”方法将两个具有相同散列码的对象放入 HashSet 中，那么 HashSet 会将它们放入同一个桶中，并在它们之间创建一个链表。类似地,“get”方法也将通过上述过程识别桶，但是为了从链表中检索对象，它将遍历链表并对每个对象调用 equals。如果 equals 方法返回 true，它将返回该对象。

因此，如果两个相等的对象有不同的散列码，那么 HashSet 可能会将它们放在不同的桶中，这将导致 HashSet 的错误行为。

这个问题还有其他的变体。

***两个不相等的对象可以有相同的哈希码吗？*** 答案是肯定的。

***两个相等的对象必须有相同的哈希码吗？*** 答案是肯定的。它是 Java 中的一个虚拟契约，不应该被破坏，否则插入和检索(get)将不能用于 HashSet 和 HashMap。

正如你所看到的，这个问题有各种各样的变体，在回答这个问题之前，你必须考虑 HashSet 或 HashMap 的工作原理。当面试官问这个问题的时候，暂停一下，按照上面解释的逻辑，你会得到你的答案。

## ArrayList 和 LinkedList 有什么区别？

当问及与数据结构相关的问题时，考虑插入、检索(get)和删除操作。

**数组列表中的插入:**在数组列表中，最坏情况下插入复杂度为 O(n)次，一般情况下为 O(1)次。默认情况下，Java 中 ArrayList 的大小是 10。如果 ArrayList 达到了它的最大容量，并希望插入一个额外的第 11 个元素，它将创建一个更大的新 ArrayList，并将原始列表中的所有元素复制到新列表中。该操作的时间复杂度为 O(n)。但是平均来说，插入需要 O(1)时间，因为 ArrayList 保存了一个大小变量，该变量存储了列表中插入项的数量，当需要向 ArrayList 中插入额外的项时，它只需要在索引等于数组大小的单元格处插入。

**在数组列表中检索:**在数组列表中获取一个带索引的对象需要 O(1)时间。

**在数组列表中删除:**从数组列表中删除需要 O(n)时间，因为需要移动元素。

**linked list 中的插入:**linked list 中的插入花费 O(1)时间，因为只需要更新指向下一个元素的指针。

**LinkedList 中的检索:**从 LinkedList 中获取带有索引的对象需要 O(n)时间，因为它必须遍历 linked list 才能到达某个索引。

**LinkedList 中的删除:**linked list 中的删除也需要 O(n)时间，因为它必须遍历 linked list 才能到达某个索引。此外，LinkedList 具有 removeFirst 和 removeLast 方法，这需要 O(1)时间，因为只需要调整第一个和最后一个指针，并且不需要像 ArrayList 那样进行移位操作。

## Q3)什么时候应该使用 ArrayList 或 LinkedList？

如果插入比读取多，那么我们应该使用 LinkedList，否则我们应该使用 ArrayList。

## Q4)Java 中有多少种异常？

主要有两种例外

1.  检查异常
2.  未检查的异常

IOException 或 ClassNotFound 异常是检查异常的示例，因为它们可以在编译时检查。IndexOutOfBoundException 或 NullpointerException 是未检查异常的示例，因为它们只能在运行时检测到。

## Q5)如何在 Java 中对集合进行升序排序？

假设你有一个收藏

```
List<Integer> lst=new ArrayList<Integer>();
lst.add(10);
lst.add(4);
lst.add(6);
Collections.sort(lst);
```

上面的代码将对列表进行排序。现在假设您有一个学生列表，您想根据他们的编号对他们进行排序。

```
class Student{
 private int rollNumber; public Student(int rollNumber){
   this.rollNumber=rollNumber;
 } public int getRollNumber(){
  return this.rollNumber;
 } public static void main(String[] args){
  List<Student> lstStudent=new ArrayList<Student>();
  lstStudent.add(new Student(10));
  lstStudent.add(new Student(3));
  lstStudent.add(new Student(6));
  //How to sort this list according to Student Rollnumber.
 }}
```

有以下方法可以解决这个问题:

***方法一:*** Class Student 实现 comparable 接口，然后可以用 Collections.sort(lstStudent)对列表进行排序。

```
class Student implements Comparable<Student>{
private int rollNumber;
public Student(int rollNumber){
 this.rollNumber=rollNumber;
}public int getRollNumber(){
 return rollNumber;
}//If smaller then return a negative integer, equal then 0 and if //greater then a positive integer
@Override
 public int compareTo(Student arg1) {
  return this.getRollNumber() — arg1.getRollNumber();
 }public static void main(String[] args){
  List<Student> lstStudent=new ArrayList<Student>();
  lstStudent.add(new Student(10));
  lstStudent.add(new Student(3));
  lstStudent.add(new Student(6));
  Collections.sort(lstStudent);
 }}
```

***方法二:*** 可以使用匿名类来给出 Collections API 的 sort 方法中 Comparator 接口的实现。通过匿名类，我们可以提供一个接口的实现，抽象类或者在使用时用一个表达式扩展一个类。

```
public static void main(String[] args){
  List<Student> lstStudent=new ArrayList<Student>();
  lstStudent.add(new Student(10));
  lstStudent.add(new Student(3));
  lstStudent.add(new Student(6));
  Collections.sort(lstStudent, new Comparator(){
    @Override
    public int compare(Student arg0, Student arg1) {
     return arg0.getRollNumber() — arg1.getRollNumber():
    }
  })
}
```

***方法三:*** 在集合 API 的 sort 方法中提供一个 lambda 表达式。写起来极其简单。我会推荐记住它，因为它在日常编码问题中经常使用。

```
public static void main(String[] args){
  List<Student> lstStudent=new ArrayList<Student>();
  lstStudent.add(new Student(10));
  lstStudent.add(new Student(3));
  lstStudent.add(new Student(6));
  Collections.sort(lstStudent,(x,y)->x.getRollNumber() -           y.getRollNumber())
}
```

如您所见，该集合仅使用一行代码进行排序。由于比较器接口是一个函数接口，所以 lambda 表达式可以用来提供它的实现。函数接口就是只有一个抽象方法的接口。

**Lambda 表达式解释:** (x，y)->x . getrollnumber()—y . getrollnumber()

x 和 y 是比较函数的两个参数，这个比较函数的实现是 x . getrollnumber()-y . getrollnumber()。

## Q6)说出这个首要难题的结果:

```
class Engine{
 public Engine(){
  getCarInitials();
 }
 public String getCarInitials(){}
}class Car extends Engine{
 String carNumber;
 public Car(){
   carNumber=”ABC567”;
 } @Override
 public String getCarInitials(){
  return carNumber.substring(0,3);
 } public static void main(String[] args){
  Car car=new Car();
  System.out.println(“Car Initials are:”+car.getCarInitials());
 }
}
```

这个程序会在第行抛出一个 Nullpointer 异常(Car car=new Car())。因为我们知道，在设置 carNumber 之前，首先会在 Car()构造函数内部默认调用父构造函数 Engine()。调用引擎内部的构造函数“getCarInitials()”，该构造函数实际上在子类中被覆盖。因为它被覆盖了，所以 Java 将调用被覆盖的方法，并尝试获取空“carNumber”字符串的子字符串。这将导致 Nullpointer 异常。

## Q7:这个超载难题的结果会是什么？解释静态绑定和动态绑定的区别。

```
class Shape{}
class Circle extends Shape{}
class Building{
 public void setShape(Shape shape){
  System.out.println(“Parameter is shape”);
 } public void setShape(Circle circle){
  System.out.println(“Parameter is circle”);
 } public static void main(String[] args){
  Building building=new Building();
  Shape shape=new Circle();
  building.setShape(shape);
 }
}
```

这个程序将打印“参数是形状”。Java 对重载函数使用静态或编译时绑定。所以这里的参数类型将在编译时检查。另一方面，动态或运行时绑定用于被覆盖的函数。与 Java 相反，Groovy 语言对重载函数使用动态绑定。

## 参考资料:

[Java 列表:LinkedList vs ArrayList](https://manifesto.co.uk/java-lists-arraylist-vs-linkedlist/)

[哈希表中的加载因子](https://www.javatpoint.com/load-factor-in-hashmap)

[Java 中的检查和未检查异常及示例](https://beginnersbook.com/2013/04/java-checked-unchecked-exceptions-with-examples/)

[Java 中的匿名类](https://www.baeldung.com/java-anonymous-classes)

[Java 功能接口](http://tutorials.jenkov.com/java-functional-programming/functional-interfaces.html#:~:text=A%20functional%20interface%20in%20Java,to%20the%20single%20unimplemented%20method.)