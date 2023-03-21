# 10 个 Selenium Webdriver 提示和技巧

> 原文：<https://medium.com/javarevisited/10-selenium-webdriver-tips-and-tricks-514f5ab928ec?source=collection_archive---------3----------------------->

![](img/9a0702f940f960d7961310923fb67b4f.png)

很多时候，我们在使用 Selenium Webdriver 时会遇到一些简单的问题。几个简单的步骤就可以提高自动化测试的稳定性和执行速度。我已经列出了用 Java 开发 Selenium 时实时面临的各种用例。还列出了这些用例可以直接使用的代码片段。

**1。抓取网页截图(针对故障/问题)**

有时，需要对网页进行截图。它有助于识别问题和调试测试用例。

屏幕截图使用以下代码片段

```
File scrFile ;
String pathofscreenshot;
scrFile = ((TakesScreenshot)driver).getScreenshotAs(OutputType.FILE);
FileUtils.copyFile(scrFile, new File(pathofscreenshot + "\\screenshot.png"));
```

**2。取部分截图**

有时需要截取屏幕的一部分。例如，我们需要对网页中的弹出窗口或框架进行截图。

要截取部分屏幕截图，请使用以下代码片段

```
String screenShot = System.getProperty("user.dir") + "\\screenShot.png";

File screen = ((TakesScreenshot)driver).getScreenshotAs(OutputType.FILE);

Point p = webelement.getLocation();

int width = webelement.getSize().getWidth();

int height = webelement.getSize().getHeight();

BufferedImage img = ImageIO.read(screen);

BufferedImage dest = img.getSubimage(p.getX(), p.getY(), width,height);

ImageIO.write(dest, "png", screen);

FileUtils.copyFile(screen, new File(pathofscreenshot + "\\screenshot.png"));
```

**3。执行 javascript(。js)**

我们可以执行 javascript 来进行自定义同步、更改一些值、启用/禁用元素或隐藏/显示元素。

使用以下代码通过 selenium webdriver 执行 javascript

```
JavascriptExecutor js = ((JavascriptExecutor) driver);
js.executeScript("alert('hello world');");
```

**4。从列表中选择/获取下拉选项。**

使用选择类来标识下拉元素

```
Select selectValue = new Select(webelement1);
```

按索引选择选项

```
selectValue.selectByIndex(<index in integer>);
```

通过值选择选项

```
selectValue.selectByValue(<string value of dropdown option>);
```

**5。使用拖放功能**

有时需要将元件从一个位置移动到另一个位置。拖放在为 webelement 提供的基本方法中不可用。

为此，selenium 提供了 Action 类

使用以下代码段从元素 webElementFrom 拖动到 WebelementTo

```
Actions action = new Actions(driver);Action dragAndDrop = action.clickAndHold(webElementFrom)
.moveToElement(WebelementTo)
.release(WebelementTo)
.build();dragAndDrop.perform();
```

**6。滚动网页(垂直&水平)**

在 selenium webdriver 中，可以使用 javascriptexecutor 来完成滚动。在 js executor 中调用 javascript 函数 window.scrollBy。

```
((JavascriptExecutor)driver).executeScript("window.scrollBy(0,250);");
```

这将垂直滚动网页 250 像素。

```
((JavascriptExecutor)driver).executeScript("window.scrollBy(250,0);");
```

这将使网页水平滚动 250 像素。

**7。打开一个新标签页**

有时需要在浏览器的同一个窗口中打开一个新标签页，使用下面的代码片段打开一个新标签页

```
driver.findElement(By.cssSelector("body")).sendKeys(Keys.CONTROL +"t");
```

在新选项卡上运行测试后，使用以下代码再次切换到第一个选项卡

```
ArrayList<String> tab_list=new ArrayList<String> (driver.getWindowHandles()); driver.switchTo().window(tab_list.get(0));
```

**8。设置页面加载超时**

有时网页需要一些时间来完全加载。要设置页面加载超时，请使用以下代码片段

```
driver.manage().timeouts().pageLoadTimeout(40, TimeUnit.SECONDS);
```

这将使页面在 40 秒后超时。

**9。刷新网页**

有时候网页中会出现元素没有加载，需要刷新页面来加载所有元素的情况。在大多数情况下，可以看到元素在第一次尝试时没有被加载，但是如果我们再次加载页面，所有的组件都会被加载

使用 selenium 刷新网页的方法有很多。一些方法如下:

```
driver.navigate().refresh();
driver.navigate().to(driver.getCurrentUrl());
```

假设当前应用程序的 url 是 google.co.in

```
driver.get("https://www.google.co.in");
```

我们可以在任何元素上发送 F5 键

```
driver.findElement(By.id("id1")).sendKeys(Keys.F5);
driver.findElement(By.id("id1")).sendKeys("\uE035");
```

这里，\uE035 是 id1 的 ASCII 码

当我们使用 javascript 执行器执行 location.reload 函数时，当前网页被加载

```
$ JavascriptExecutor js; $ js.executeScript("location.reload()");
```

10。在浏览器窗口之间切换

有时点击一个元素，网页会打开一个新的弹出窗口，需要将驱动程序实例切换到新窗口并执行一些操作。

使用下面的代码片段。

```
//Get the current window handle

String currentwindowhandle = driver.getWindowHandle();

//Click to open new windows.
....
....
....

//Switch to new window

for(String newwindowhandle: driver.getWindowHandles()){

driver.switchTo().window(newwindowhandle);

}

//Close new window

driver.close();

//Switch back to first window

driver.switchTo().window(currentwindowhandle);
```

*最初发表于*[T5【https://www.linkedin.com】](https://www.linkedin.com/pulse/10-selenium-webdriver-tips-tricks-ankit-agrawal/)*。*