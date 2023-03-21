# 用 Builder 和 Factory 在 Java 中创建对象的方法

> 原文：<https://medium.com/javarevisited/ways-to-create-objects-em-java-679e4f3c2160?source=collection_archive---------1----------------------->

![](img/effb333bda3a215222f4e730243414e5.png)

几乎每次我开始写程序的时候，我都试图把定义领域作为要做的第一步。举例来说，这对我理解 api 的含义很有帮助。她是做什么的？她在说什么？为什么会存在？

然后我尝试定义如何创建对象。我也注意到了“如何”这个词，思考了这样一些问题:

*   任何人都可以随时给一个新的对象吗？
*   任何人都可以随时设置属性吗？
*   任何人都可以用空属性随心所欲地创建吗？

在某堂课上，我听到了一个关于汽车的类比，直到今天我还带着它，它是这样的:我制造了一辆汽车，我要交付它，我希望收到它的人伸出手，打开发动机，而不知道他们在做什么，改变什么或做任何其他事情，除了开那辆车？

也就是说，我将向您展示创建对象的三种方法及其摆脱实例化和属性集的优势。

我将假设我正在为一个宠物商店系统编程，所以我将考虑在那里处理的小动物。

通常我们在这里会有这个:

```
package com.example.demo.domain;public class Dog {
    public String name;
    public Integer age;
    public String race;
    public String sex;
    public Double weight;
}
```

该对象将按如下方式使用:

```
public class Tests {      
    public static void main(String[] args) {         
        Dog dog = new Dog();         
        dog.raca = "pitbull";
        dog.age = 5;         
        dog.weight = 10.0;         
        dog.sex = "M";     
    }  
}
```

不要使用这种方式，它有许多漏洞和差距。

我不会评论这种形式的使用，我会谈论其他方式以及为什么他们会更有趣。

## 最常见的方式:

首先，我通常不公开任何东西，我喜欢定义一个构造函数，所以它看起来像这样:

```
package com.example.demo.english.domain;import java.time.LocalDate;
import java.time.Period;public class Dog {
    private String name;
    private LocalDate birthDate;
    private String breed;
    private String sex;
    private Double weight; public Dog(String name, LocalDate birthDate, String breed, String sex, Double weight){
        this.name = name;
        this.birthDate = birthDate;
        this.breed = breed;
        this.sex = sex;
        this.weight = weight;
    } public String getName() {
        return name;
    } public Integer getAge() {
        return Period.*between*(this.birthDate, LocalDate.*now*()).getYears();
    } public String getBreed() {
        return breed;
    } public String getSex() {
        return sex;
    } public Double getWeight() {
        return weight;
    } public void setWeight(Double weight) {
        this.weight = weight;
    }
}
```

现在，属性的使用是通过 getters 和 setters 方法完成的，对象的创建是通过其构造函数传递参数来完成的，如下所示:

```
public static void main(String[] args) {
    Dog dog = new Dog("Rex", LocalDate.*of*(2015, 10, 10), "pitbull", "M", 10.0);
    dog.getName();
}
```

## 使用工厂

这相对简单，我们需要一个类，它的唯一功能是提供对象的创建:

```
import java.time.LocalDate;public class DogFactory { public Dog create(String name, LocalDate birthDate, String breed, String sex, Double weight){
        return new Dog(name, birthDate, breed, sex, weight);
    }}
```

然后关闭 Dog 类，不允许实例化:

```
import java.time.LocalDate;
import java.time.Period;public class Dog {
    private String name;
    private LocalDate birthDate;
    private String breed;
    private String sex;
    private Double weight; public Dog(String name, LocalDate birthDate, String breed, String sex, Double weight){
        this.name = name;
        this.birthDate = birthDate;
        this.breed = breed;
        this.sex = sex;
        this.weight = weight;
    } public String getName() {
        return name;
    } public Integer getAge() {
        return Period.*between*(this.birthDate, LocalDate.*now*()).getYears();
    } public String getBreed() {
        return breed;
    } public String getSex() {
        return sex;
    } public Double getWeight() {
        return weight;
    } public void setWeight(Double weight) {
        this.weight = weight;
    }
}
```

用法是这样的:

```
import com.example.demo.english.factory.Dog;
import com.example.demo.english.factory.DogFactory;import java.time.LocalDate;public class FactoryTests { public static void main(String[] args) { DogFactory dogFactory = new DogFactory();
        Dog dog = dogFactory.create("Rex", LocalDate.*of*(2015, 10, 10), "pitbull", "M", 10.0);
    }}
```

就这样，我们使用工厂模式。如果有人试图在其他包中实例化，他们不会成功。现在让我们来看看构建器。

## 使用构建器

在这里，事情变得有点复杂，但是对于有很多属性的对象来说，这是值得的。这是因为通过构造函数传递太多参数会让我们在创建对象时感到困惑。

我首先创建了 DogBuilder 类:

```
import java.time.LocalDate;public class DogBuilder { private Dog dog; public DogBuilder() {
        this.dog = new Dog();
    } public static DogBuilder builder() {
        return new DogBuilder();
    } public DogBuilder addName(String nome) {
        this.dog.setName(nome);
        return this;
    } public DogBuilder addBirthDate(LocalDate birthDate) {
        this.dog.setBirthDate(birthDate);
        return this;
    } public DogBuilder addBreed(String raca) {
        this.dog.setRaca(raca);
        return this;
    } public DogBuilder addWeight(Double peso) {
        this.dog.setWeight(peso);
        return this;
    } public DogBuilder addSex(String sexo) {
        this.dog.setSex(sexo);
        return this;
    } public Dog get() {
        return this.dog;
    }}
```

之后，我保护了 dog 类的方法，这样它们就不会被误用:

```
import java.time.LocalDate;
import java.time.Period;public class Dog {
    private String name;
    private LocalDate birthDate;
    private String breed;
    private String sex;
    private Double weight; protected Dog(){ } protected void setName(String name) {
        this.name = name;
    } public void setBirthDate(LocalDate birthDate) {
        this.birthDate = birthDate;
    } protected void setRaca(String raca) {
        this.breed = raca;
    } protected void setSex(String sex) {
        this.sex = sex;
    } public String getName() {
        return name;
    } public Integer getAge() {
        return Period.*between*(this.birthDate, LocalDate.*now*()).getYears();
    } public String getBreed() {
        return breed;
    } public String getSex() {
        return sex;
    } public Double getWeight() {
        return weight;
    } public void setWeight(Double weight) {
        this.weight = weight;
    }}
```

请注意，只有 setWeight(…)是公共的。其余的都是受保护的，那是因为这是唯一可以随时间更新的东西。
使用构建器是这样的:

```
import com.example.demo.english.builder.DogBuilder;
import com.example.demo.english.builder.Dog;import java.time.LocalDate;public class BuilderTests { public static void main(String[] args) {
        Dog dog = DogBuilder.*builder*()
                .addName("Rex")
                .addBirthDate(LocalDate.*of*(2015, 10, 10))
                .addWeight(10.0)
                .addSex("M")
                .addBreed("Pitbull")
                .get(); dog.getName();
    }}
```

很酷吧？

![](img/2b833c37d58a6f78790df90e46f3a482.png)

现在，只需选择使代码可读性更好的形式，并努力去做。

这段代码在这里:[https://github.com/mmarcosab/tres-formas-criar-objetos](https://github.com/mmarcosab/tres-formas-criar-objetos)

正如我们在巴西所说的:我们在一起。