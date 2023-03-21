# 春季数据预测

> 原文：<https://medium.com/javarevisited/spring-data-projections-6f4e56023795?source=collection_archive---------1----------------------->

Spring 数据预测快速介绍！

![](img/2839a96d0c18d19692d6851ad7f7bfc3.png)

伊恩·杜利在 Unsplash[上的照片](https://unsplash.com/s/photos/neat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Spring Data 支持存储库中的定制查询，开发人员只需要按照一套方式为他们正在寻找的功能编写存储库方法。至少对于大多数类型的搜索来说，只需在接口中定义方法就可以了。开发人员选择要搜索的功能和字段，创建方法并传入所需的参数，方法就会做它应该做的事情。

这非常简单明了。但是假设您不希望(无论出于什么原因)将整个实体或对象作为查询的一部分返回。假设您只需要原始实体的字段子集:Spring data projections to the rescue！

Spring 支持不同类型的投影:基于接口、基于类和动态投影。出于本文的考虑，我将坚持使用基于接口的投影。Spring 数据文档中有关于[其他类型](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#projections.dtos)的精彩信息！

在我们开始之前，我使用了以下版本的工具和语言:

*   科特林 : 1.4.21
*   等级 : 6.7.1
*   [Spring Boot](/javarevisited/top-10-courses-to-learn-spring-boot-in-2020-best-of-lot-6ffce88a1b6e?source=---------39------------------) : 2.4.1
*   JDK: 15 人

关于这一点，让我们一步一步地讨论如何使用 spring 数据预测，使用书籍作为我们想要管理的资源:

## 步骤 1:定义实体

```
import javax.persistence.Entity
import javax.persistence.GeneratedValue
import javax.persistence.Id

@Entity
class BookEntity(
    val name: String,
    val author: String,
    val isSeries: Boolean,
    val yearPublished: Int,
    val publisherName: String,
    val genre: String
) {

    @Id
    @GeneratedValue
    var id: Long = 0

    override fun equals(other: Any?): Boolean {
        if (this === other) return true
        if (*javaClass* != other?.*javaClass*) return false

        other as BookEntity

        if (name != other.name) return false
        if (author != other.author) return false
        if (isSeries != other.isSeries) return false
        if (yearPublished != other.yearPublished) return false
        if (publisherName != other.publisherName) return false
        if (genre != other.genre) return false

        return true
    }

    override fun hashCode(): Int {
        var result = name?.hashCode() ?: 0
        result = 31 * result + (author?.hashCode() ?: 0)
        result = 31 * result + isSeries.hashCode()
        result = 31 * result + (yearPublished ?: 0)
        result = 31 * result + (publisherName?.hashCode() ?: 0)
        result = 31 * result + (genre?.hashCode() ?: 0)
        return result
    }

    override fun toString(): String {
        return "BookEntity(" +
                "name='$name', " +
                "author='$author', " +
                "isSeries=$isSeries, " +
                "yearPublished=$yearPublished, " +
                "publisherName='$publisherName', " +
                "genre='$genre', " +
                "id=$id" +
                ")"
    }
}
```

我们定义了一个非常简单的类来管理书籍。我们让 ID 字段自动生成(这样用户就不必为它设置一个值),所有其他字段都是必需的！

## 步骤 2:定义存储库

```
import org.springframework.data.repository.CrudRepository

interface BookRepository : CrudRepository<BookEntity, Long> {

    fun findByAuthor(author: String): List<BookEntity>
}
```

接下来，我们定义一个简单的存储库来管理书籍。为了简单起见，我们假设不需要任何分页或排序功能。所以我们只是从 [Spring Data 的](https://www.java67.com/2021/01/spring-data-jpa-interview-questions-answers-java.html) `CrudRepository`延伸。

## 步骤 3:使用存储库保存和检索数据

```
import org.springframework.beans.factory.annotation.Autowired
import org.springframework.boot.CommandLineRunner
import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.boot.runApplication

@SpringBootApplication
class Application : CommandLineRunner {

   @Autowired
   private lateinit var bookRepository: BookRepository

   override fun run(vararg args: String?) {
      bookRepository.saveAll(
         *listOf*(
            BookEntity(
               name = "Harry Potter and the Chamber of Secrets",
               author = "J K Rowling",
               isSeries = true,
               yearPublished = 1998,
               publisherName = "Bloomsbury Publishing",
               genre = "Fantasy"
            ),
            BookEntity(
               name = "Harry Potter and the Goblet of Fire",
               author = "J K Rowling",
               isSeries = true,
               yearPublished = 2000,
               publisherName = "Bloomsbury Publishing",
               genre = "Fantasy"
            ),
            BookEntity(
               name = "Harry Potter and the Deathly Hallows",
               author = "J K Rowling",
               isSeries = true,
               yearPublished = 2007,
               publisherName = "Bloomsbury Publishing",
               genre = "Fantasy"
            ),
            BookEntity(
               name = "Lord of the Rings: The Fellowship of the Ring",
               author = "J.R.R. Tolkien",
               isSeries = true,
               yearPublished = 1954,
               publisherName = "Allen & Unwin",
               genre = "Adventure"
            ),
            BookEntity(
               name = "The Lion, the Witch, and the Wardrobe",
               author = "C. S. Lewis",
               isSeries = true,
               yearPublished = 1950,
               publisherName = "Geoffrey Bles",
               genre = "Children's Fantasy"
            ),
            BookEntity(
               name = "Gone Girl",
               author = "Gillian Flynnp",
               isSeries = false,
               yearPublished = 2012,
               publisherName = "Crown Publishing Group",
               genre = "Thriller"
            )
         )
      )

      bookRepository.findByAuthor("J K Rowling")
         .*forEach* **{** book **->** *println*(book) **}** }
}

fun main(args: Array<String>) {
   *runApplication*<Application>(*args)
}
```

我们的数据库目前没有数据。所以我们首先加载一些数据。我们添加了一些[书籍](/hackernoon/top-5-spring-boot-and-spring-cloud-books-for-java-developers-75df155dcedc?source=---------23------------------)，然后作者使用我们在存储库中创建的自定义查询获取它们。

如果我们此时运行此应用程序，我们将在日志中看到以下内容:

```
BookEntity(name='Harry Potter and the Chamber of Secrets', author='J K Rowling', isSeries=true, yearPublished=1998, publisherName='Bloomsbury Publishing', genre='Fantasy', id=1)BookEntity(name='Harry Potter and the Goblet of Fire', author='J K Rowling', isSeries=true, yearPublished=2000, publisherName='Bloomsbury Publishing', genre='Fantasy', id=2)BookEntity(name='Harry Potter and the Deathly Hallows', author='J K Rowling', isSeries=true, yearPublished=2007, publisherName='Bloomsbury Publishing', genre='Fantasy', id=3) 
```

## 步骤 4:引入投影！

假设我们只想获得某个作者写的书名，而不是获得关于书籍的所有信息。因此，我们用一个方法创建了一个接口，该方法以`get`开头，后跟我们要检索的字段的名称(在我们的例子中是`name`)。

```
import org.springframework.beans.factory.annotation.Value

interface BookName {

    fun getName(): String
}
```

## 步骤 5:更新存储库

```
import org.springframework.data.repository.CrudRepository

interface BookRepository : CrudRepository<BookEntity, Long> {

    fun findByAuthor(author: String): List<BookName>
}
```

我们更新了`findByAuthor`方法的返回类型，以返回一个`BookName`对`BookEntity`的列表。作为最后一步，我们还更新了打印语句以打印投影方法

```
bookRepository.findByAuthor("J K Rowling")
   .*forEach* **{** bookName **->** *println*(bookName.getName()) **}**
```

## 步骤 6:行动中的预测

现在，当我们运行我们的应用程序时，我们应该会在控制台中看到以下内容:

```
Harry Potter and the Chamber of Secrets
Harry Potter and the Goblet of Fire
Harry Potter and the Deathly Hallows
```

## 第七步:另一种投影方法

如果我们想将`BookEntity`中的多个字段合并成投影界面中的一个字段，我们可以将`getName()`方法更新为:

```
import org.springframework.beans.factory.annotation.Value

interface BookName {

    @Value("#{target.name + ', ' + target.yearPublished}")
    fun getName(): String
}
```

当我们运行应用程序时，我们应该会在日志中看到以下内容:

```
Harry Potter and the Chamber of Secrets, 1998
Harry Potter and the Goblet of Fire, 2000
Harry Potter and the Deathly Hallows, 2007
```

春季数据预测就是这么简单方便！如需进一步阅读，请参考 Spring Data JPA 关于[预测](https://docs.spring.io/spring-data/jpa/docs/1.10.1.RELEASE/reference/html/#projections)的文档！