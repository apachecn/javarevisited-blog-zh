# 流畅的界面模式——减少冗长代码的简单方法

> 原文：<https://medium.com/javarevisited/fluent-interface-pattern-a-simple-way-to-reduce-verbose-code-93df3d58b391?source=collection_archive---------2----------------------->

![](img/cf0cb23c2e519e5cd3c2e396e4f6d27e.png)

有数以千计的模式，你应该知道其中的一些。有关于模式的大课程和书籍。在这篇文章中，我们将谈论流畅的界面模式。是如此简单，使代码更加清晰，所以我忍不住不给你看。

我们经常有代码创建这样的对象:

```
public Person generatePerson() {
	Person person = new Person();
	person.setFirstName("FirstName");
	person.setLastName("LastName");
	person.setAge(15);
	person.setSex("M");
	person.setCity("City");
	person.setHairColor("Black");
	return person;
}
```

**经典映射。**我们正在初始化一个对象，然后我们把一些数据放入 setters，然后我们返回这个对象。这是`Person` 物体的样子:

```
class Person {
  private String firstName;
  private String lastName;
  private int age;
  private String sex;
  private String city;
  private String hairColor; public String getFirstName() {
    return firstName;
  } public void setFirstName(String firstName) {
    this.firstName = firstName;
  } public String getLastName() {
    return lastName;
  } public void setLastName(String lastName) {
    this.lastName = lastName;
  }

  // The rest of the getters/setters are
  // using the same pattern
  // ...
}
```

我们为它们准备了字段、getters 和 setters。经典。

现在，为了实现这个模式，我们对所有 setter 方法做了两个简单的修改。由此可知:

```
public void setHairColor(String hairColor) {
    this.hairColor = hairColor;
}
```

对此:

```
public Person setHairColor(String hairColor) {
    this.hairColor = hairColor;
    return this;
}
```

> 变化很简单:在每个 **set** 之后，我们返回当前对象或者所谓的“ **this** ”。

让我们为所有的设定者做那件事:

```
class Person {
     private String firstName;
     private String lastName;
     private int age;
     private String sex;
     private String city;
     private String hairColor; public String getFirstName() {
	  return firstName;
     }

     public ServiceMappingDTO setFirstName(String firstName) {
	  this.firstName = firstName;
	  return this;
     }

     public String getLastName() {
	  return lastName;
     }

     public ServiceMappingDTO setLastName(String lastName) {
	  this.lastName = lastName;
	  return this;
     }

    // The rest of the getters/setters are
    // using the same pattern
    // ...

}
```

你可能会问，变化是什么？我现在就给你看。

```
public Person generatePerson() {
	return new Person()
		.setFirstName("FirstName")
		.setLastName("LastName")
		.setAge(15)
		.setSex("M")
		.setCity("City")
		.setHairColor("Black");
}
```

看看我们删除了多少冗长的代码。流程是这样的:我们首先创建一个新的 Person 对象——它返回一个新的对象。然后我们说——这个新物体**。setFirstName** 。然后 **setFirstName** 方法设置名字，然后返回同一个对象( **this** )。然后我们可以做**。上面的 setLastName** 等等。我们可以像这样链接这些方法。

快速提示:如果您正在手工编写 getters 和 setters 停止这样做！所有现代的 ide 都有一个简单的方法来生成它们。 [IntelliJ](/javarevisited/7-best-courses-to-learn-intellij-idea-for-beginners-and-experienced-java-programmers-2e9aa9bb0c05) 甚至有一个生成*“流畅界面”设置器的选项。*

**在你离开之前——我们惊人的干净代码课程**

如果你喜欢这篇文章，你会喜欢我们的课程:

*   46 场讲座
*   3 小时的内容
*   做一个真实的项目来测试你的知识
*   27 个可下载资源
*   30 天退款保证

你可以在这里报名参加课程— [干净代码:在 7 天内暴涨你的编程生涯](https://rebrand.ly/spcc-fluent)

[![](img/dc9c4358d48d8c1502f7fa18c29da201.png)](https://rebrand.ly/spcc-fluent)