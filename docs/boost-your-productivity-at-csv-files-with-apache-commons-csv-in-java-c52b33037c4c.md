# 使用 Java 中的 Apache.commons.CSV 提高 CSV 文件的产量

> 原文：<https://medium.com/javarevisited/boost-your-productivity-at-csv-files-with-apache-commons-csv-in-java-c52b33037c4c?source=collection_archive---------1----------------------->

![](img/aebeff20416f0ecbe987910a281b18de.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[克里斯托佛罗拉](https://unsplash.com/@krisroller?utm_source=medium&utm_medium=referral)拍摄的照片

# 1.介绍

CSV(逗号分隔值)文件广泛用于在应用程序之间交换数据。然而，对 CSV 文件的操作可能会很棘手且耗时。

Apache 软件基金会给了我们[的 T5。Commons.CSV](https://commons.apache.org/proper/commons-csv/summary.html) 库，通过 CSV 文件的读/写操作使我们的生活变得更加轻松。因此它附带了 Apache 许可证 2.0。

CSV 文件有多种格式，在下面的例子中我们将使用经典的 [**RFC4180**](https://tools.ietf.org/html/rfc4180) 格式。

**RFC4180** 格式规格根据 [**维基百科**](https://en.wikipedia.org/wiki/Comma-separated_values) :

> [纯文本](https://en.wikipedia.org/wiki/Plain_text)是否使用了字符集，如 [ASCII](https://en.wikipedia.org/wiki/ASCII) 、各种 [Unicode](https://en.wikipedia.org/wiki/Unicode) 字符集(如 [UTF-8](https://en.wikipedia.org/wiki/UTF-8) )、 [EBCDIC](https://en.wikipedia.org/wiki/EBCDIC) 或 [Shift JIS](https://en.wikipedia.org/wiki/Shift_JIS) ，
> 
> 由[记录](https://en.wikipedia.org/wiki/Record_(computer_science))组成(通常每行一条记录)，
> 
> 记录被分成由[分隔符](https://en.wikipedia.org/wiki/Delimiter)分隔的[字段](https://en.wikipedia.org/wiki/Field_(computer_science))(通常是单个保留字符，如逗号、分号或制表符；有时定界符可以包括可选的空格)，
> 
> 其中每个记录都有相同的字段序列。

</javarevisited/what-java-programmers-should-learn-in-2020-648050533c83>  

# 2.用 Apache Commons CSV 读取 CSV 文件

创建一个名为 **ParseCSV** 的新 maven 项目，并将下面的代码片段粘贴到您的 **ParseCSV/pom.xml** 文件中:

```
<dependency><groupId>org.apache.commons</groupId><artifactId>commons-csv</artifactId><version>1.8</version></dependency>
```

上述依赖项将为我们的示例下载必要的库。

下载一个包含国家的示例 [**CSV**](https://gist.github.com/petranb2/5f2f877df3b15042151cf80ba49bba4a) 文件，并将其粘贴到您的项目目录***parse CSV/countries . CSV***。

将以下代码添加到您的 main 方法中:

```
try {Reader csvData = new FileReader("countries.csv");CSVParser parser = CSVFormat.RFC4180.withFirstRecordAsHeader().parse(csvData);for (CSVRecord csvRecord : parser) {System.out.println(csvRecord.getRecordNumber()+":"+csvRecord.get("name"));}} catch (FileNotFoundException e) {e.printStackTrace();} catch (IOException e) {e.printStackTrace();}
```

让我们解释一下上面的代码片段:

首先，我们创建一个 **Reader** 对象来读取 **countries.csv** 文件。

```
Reader csvData = new FileReader("countries.csv");
```

我们使用 **CSVFormat** 静态对象创建一个 **CSVParser** 解析 CSV 文件，格式为 **RFC4180** ，头为**第一条记录**。

```
CSVParser parser = CSVFormat.RFC4180.withFirstRecordAsHeader().parse(csvData);
```

最后，迭代**解析器**对象从文件中获取数据。

```
for (CSVRecord csvRecord : parser) {System.out.println(csvRecord.getRecordNumber()+":"+csvRecord.get("name"));}
```

[**阿帕奇。Commons.CSV**](https://commons.apache.org/proper/commons-csv/summary.html) 提供了几种方法来读取 CSV 文件中的字段，在本例中，我们使用标题名来访问数据，但也可以使用索引号来访问数据。

</javarevisited/what-next-for-senior-developers-in-tech-project-manager-technical-architect-or-a-devops-engineer-b532a80c9ba1>  

# 3.用 Apache Commons CSV 编写 CSV 文件

用 [**Apache 写 **CSV** 文件。Commons.CSV**](https://commons.apache.org/proper/commons-csv/summary.html) 一样简单。

复制并粘贴以下代码片段:

```
try {FileWriter fileWriter = new FileWriter("./mountains.csv", true);try (CSVPrinter csvPrinter = new CSVPrinter(fileWriter, CSVFormat.RFC4180)) {//HEADER recordcsvPrinter.printRecord("mountain","meters");//DATA recordscsvPrinter.printRecord("Mount Everest","8.848");csvPrinter.printRecord("K2","8.611");csvPrinter.printRecord("Kangchenjunga","8.586");csvPrinter.printRecord("olumpus","2.918");csvPrinter.flush();}} catch (IOException e) {e.printStackTrace();}
```

让我们解释一下上面的代码片段:

第一步创建一个**文件写入器**，以便写入 mountains.csv 文件

```
FileWriter fileWriter = new FileWriter("./mountains.csv", true);
```

接下来，使用 **CSVPrinter** 类生成一个 csvWriter 并开始编写

```
CSVPrinter csvPrinter= new CSVPrinter(fileWriter, CSVFormat.RFC4180)
```

终于到了开始写数据的时候了

```
//HEADER recordcsvPrinter.printRecord("mountain","meters");//DATA recordscsvPrinter.printRecord("Mount Everest","8.848");csvPrinter.printRecord("K2","8.611");csvPrinter.printRecord("Kangchenjunga","8.586");csvPrinter.printRecord("olumpus","2.918");csvPrinter.flush();
```

# 4.结论

[**阿帕奇。Commons.CSV**](https://commons.apache.org/proper/commons-csv/summary.html) 为 CSV 文件的读/写操作提供了一个简单的接口。

然而，Apache Commons CSV 是最好的库之一，具有定期更新和丰富的文档。

# 您可能还喜欢:

</analytics-vidhya/how-to-build-a-rest-api-with-node-js-and-oracle-18c-xe-f57bbbdd9b09>  </javascript-in-plain-english/learn-to-use-regular-expressions-like-a-ninja-in-node-js-20cfb6806f26> 