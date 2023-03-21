# 面向初学者的立面设计模式

> 原文：<https://medium.com/javarevisited/facade-design-pattern-for-beginners-7dde0612d069?source=collection_archive---------1----------------------->

![](img/f46889811b97a7eae32e71cba53d9b27.png)

[王运明](https://unsplash.com/@ymwang?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/dog-leash?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍照

立面设计模式是结构设计模式的子类型之一。它用于向客户展示最小的实现，也是以客户能够理解的简单方式。

Facade 提供了一个客户端可以用来访问系统的接口。业务逻辑的复杂性可能隐藏在接口的背后。

**让我们直接进入主题，看看如何实现外观设计模式:**

考虑一个需求，我们必须生成不同的报告，这些报告的数据存储在不同的 ORM 上，比如 MYSQL 和 DB2。因此，在生成报告时，我们必须首先建立一个连接，并从数据库中获取数据。一旦数据被填充，我们就可以将数据发送给生成报告的方法。

在这里，我们将创建一个接口，并定义可以在所有地方使用的公共方法。

```
public interface Reports {
    public void getDataForReports(String reportType);
    public void generateReport();
}
```

我们将考虑两种类型的报告:客户报告和订单报告。CustomerReport 类可以这样定义。

```
public class CustomerReport implements Reports {

    @Override
    public void getDataForReports(String reportType) {
        *// BusinessLogic for getting the data for Customer Report* }

    @Override
    public void generateReport() {
        *// Business Logic for generating report;* }
}
```

订单报告应该这样定义。

```
public class OrderReports implements Reports{

    @Override
    public void getDataForReports(String reportType) {
        *// BusinessLogic for getting the data for Customer Report* }

    @Override
    public void generateReport() {
        *// Business Logic for generating report;* }
}
```

接下来是 ReportsGenerator 类，它将充当门面。这个类将隐藏数据获取和生成报告的实际实现。

```
public class ReportsGenerator {
    public static String generateReport(String reportType) {
        switch(reportType) {
            case "Customer":
                Reports customerReport = new CustomerReport();
                customerReport.getDataForReports(reportType);
                customerReport.generateReport();
                return reportType + "generated successfully!";

            case "OrderReport":
                Reports orderReport = new OrderReports();
                orderReport.getDataForReports(reportType);
                orderReport.generateReport();
                return reportType + "generated successfully!";
        }
        return "Invalid Report Type, Please select the correct one!";
    }
}
```

现在让我们设计 ApiHandler 类，它将依次调用 ReportsGenerator 类的`generateReport()`方法。

```
public class ApiHandler {
    public static void main(String[] args) {
        System.*out*.println(ReportsGenerator.*generateReport*("Customer"));
        System.*out*.println(ReportsGenerator.*generateReport*("Order"));
    }
}
```

## 什么时候使用外观设计模式？

当我们有一个复杂的业务逻辑，并且我们必须只向客户公开简化的 API 时，应该使用 Facade。

## 立面设计模式的优势:

*   我们可以将复杂的系统逻辑从客户端分离出来。
*   包装复杂的系统逻辑是使用外观设计模式实现的。

## 立面设计模式的缺点:

*   从长远来看，Facade 类可能会成为超级类。
*   由于 Facade，增加了额外的抽象层次。