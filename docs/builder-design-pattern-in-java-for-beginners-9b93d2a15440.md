# 面向初学者的 Java 构建器设计模式

> 原文：<https://medium.com/javarevisited/builder-design-pattern-in-java-for-beginners-9b93d2a15440?source=collection_archive---------2----------------------->

[![](img/611a1f08cf88a092cc6f1e57121903a0.png)](https://medium.com/javarevisited/7-best-online-courses-to-learn-object-oriented-design-pattern-in-java-749b6399af59)

照片由[古伊列梅·崔林](https://unsplash.com/@guiccunha?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/builder?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

生成器设计模式是一种用于创建复杂对象的创造性设计模式。当从超过四个参数的构造函数中创建对象时，我们更容易出错。即使一个参数被切换或丢失，也要花费相当多的时间来找出问题所在。

```
**public class** Customer {
    **public** String **name**;
    **public** String **firstName**;
    **public** String **lastName**;
    **public int age**;
    **public** String **address**;
    **public** String **city**;
    **public** String **mobileNumber**;
    **public** String **pincode**;

    **public** Customer(String name, 
                    String firstName, 
                    String lastName, 
                    **int** age, 
                    String address, 
                    String city, 
                    String mobileNumber,
                    String pincode) {
        **this**.**name** = name;
        **this**.**firstName** = firstName;
        **this**.**lastName** = lastName;
        **this**.**age** = age;
        **this**.**address** = address;
        **this**.**city** = city;
        **this**.**mobileNumber** = mobileNumber;
        **this**.**pincode** = pincode;
    }
}
```

考虑上面的例子，[对象创建](https://javarevisited.blogspot.com/2012/12/what-is-object-in-java-or-oops-example.html#axzz6ikhp68Le)需要八个参数，如果你错过了其中一个，那么就要准备好花时间去找出哪里出错了。现在，如果客户没有明确提供`firstName` 和`lastName` 怎么办？我们必须再创建一个构造函数，它不接受这两个参数。

```
**public** Customer(String name, **int** age, String address, String city, String mobileNumber, String pincode) {
    **this**.**name** = name;
    **this**.**age** = age;
    **this**.**address** = address;
    **this**.**city** = city;
    **this**.**mobileNumber** = mobileNumber;
    **this**.**pincode** = pincode;
}
```

如果在不久的将来增加了一个参数，我们将不得不增加一个构造函数或者编辑之前的构造函数，并且在使用过构造函数的地方增加参数。进入[构建器设计模式](http://javarevisited.blogspot.sg/2012/06/builder-design-pattern-in-java-example.html)我们的救世主。它不仅有助于轻松地创建对象，而且有助于我们长期维护代码。让我们看看这是如何实现的。

```
**public class** Customer {
    **public** String **name**;
    **public** String **firstName**;
    **public** String **lastName**;
    **public int age**;
    **public** String **address**;
    **public** String **city**;
    **public** String **mobileNumber**;
    **public** String **pincode**;

    **public** String getName() {
        **return name**;
    }

    **public** String getFirstName() {
        **return firstName**;
    }

    **public** String getLastName() {
        **return lastName**;
    }

    **public int** getAge() {
        **return age**;
    }

    **public** String getAddress() {
        **return address**;
    }

    **public** String getCity() {
        **return city**;
    }

    **public** String getMobileNumber() {
        **return mobileNumber**;
    }

    **public** String getPincode() {
        **return pincode**;
    }

    @Override
    **public** String toString() {
        **return "Customer{"** +
                **"name='"** + **name** + **'\''** +
                **", firstName='"** + **firstName** + **'\''** +
                **", lastName='"** + **lastName** + **'\''** +
                **", age="** + **age** +
                **", address='"** + **address** + **'\''** +
                **", city='"** + **city** + **'\''** +
                **", mobileNumber='"** + **mobileNumber** + **'\''** +
                **", pincode='"** + **pincode** + **'\''** +
                **'}'**;
    }

    **private** Customer(CustomerBuilder customerBuilder) {
        **this**.**name** = customerBuilder.**name**;
        **this**.**firstName** = customerBuilder.**firstName**;
        **this**.**lastName** = customerBuilder.**lastName**;
        **this**.**pincode** = customerBuilder.**pincode**;
        **this**.**address** = customerBuilder.**address**;
        **this**.**mobileNumber** = customerBuilder.**mobileNumber**;
        **this**.**age** = customerBuilder.**age**;
        **this**.**city** = customerBuilder.**city**;
    }

    **public static class** CustomerBuilder {
        **private** String **name**;
        **private** String **firstName**;
        **private** String **lastName**;
        **private int age**;
        **private** String **address**;
        **private** String **city**;
        **private** String **mobileNumber**;
        **private** String **pincode**;

        **public** CustomerBuilder() {
        }

        **public** CustomerBuilder setName(String name) {
            **this**.**name** = name;
            **return this**;
        }

        **public** CustomerBuilder setFirstName(String firstName) {
            **this**.**firstName** = firstName;
            **return this**;
        }

        **public** CustomerBuilder setLastName(String lastName) {
            **this**.**lastName** = lastName;
            **return this**;
        }

        **public** CustomerBuilder setAge(**int** age) {
            **this**.**age** = age;
            **return this**;
        }

        **public** CustomerBuilder setAddress(String address) {
            **this**.**address** = address;
            **return this**;
        }

        **public** CustomerBuilder setCity(String city) {
            **this**.**city** = city;
            **return this**;
        }

        **public** CustomerBuilder setMobileNumber(String mobileNumber) {
            **this**.**mobileNumber** = mobileNumber;
            **return this**;
        }

        **public** CustomerBuilder setPincode(String pincode) {
            **this**.**pincode** = pincode;
            **return this**;
        }

        **public** Customer build() {
            **return new** Customer(**this**);
        }
    }
}
```

需要注意的事项:

1.  有一个静态内部类可以返回对象。
2.  静态类有 setters，也有一个 build()方法，该方法将返回新的 Customer 对象。
3.  外部类只有 getters，它帮助我们获取设置的字段。

**让我们看看如何实例化客户对象**

```
**public class** BuilderClassImplementation {
    **public static void** main(String[] args) {
        Customer customer = **new** Customer.CustomerBuilder()
                .setName(**"Omkar Kulkarni"**)
                .setFirstName(**"Omkar"**)
                .setLastName(**"Kulkarni"**)
                .setMobileNumber(**"8551223****"**)
                .build();
    }
}
```

**优点:**

1.  消除了向构造函数传递如此多的参数。
2.  我们不必在构造函数中传递 null。
3.  代码为 ***可维护*** 。
4.  一旦创建了对象，我们就可以强制*不变性。*

***缺点:***

1.  *内部和外部类中的代码是重复的。*
2.  *实例也可能在没有参数的情况下创建。*

*可能会有这样的情况，我们需要一些强制性的参数，但是在上面的实现中，对象可能是无参数创建的。*

*因此，在这种情况下，我们需要对构建器类的实现进行一些调整。我们传递对象创建可能需要的强制字段，而不是有一个空的构造函数。*

*下面是构造函数的实现。*

```
***public** CustomerBuilder(String name, **int** age, String city) {
    **this**.**name** = name;
    **this**.**age** = age;
    **this**.**city** = city;
}*
```

*对象实例化也会因此而改变。*

```
*Customer customer = **new** Customer
        .CustomerBuilder(**"Omkar"**, 29, **"Pune"**)
        .setMobileNumber(**"987*******"**)
        .build();*
```

## ***结论:***

*构建器设计模式应该使用创造性的设计模式来创建复杂的对象。代码变得可维护，不容易出错。*