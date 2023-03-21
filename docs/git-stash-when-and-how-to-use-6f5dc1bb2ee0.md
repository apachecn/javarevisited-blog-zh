# git Stash——何时以及如何使用

> 原文：<https://medium.com/javarevisited/git-stash-when-and-how-to-use-6f5dc1bb2ee0?source=collection_archive---------0----------------------->

## GIT 贮藏

## 详细研究 git stash 派上用场的一些常见用例。这是一个非常有用的特性，应该放在您的 git 知识库中。

[![](img/88899a64c8152538e302a2cc644bb480.png)](https://www.java67.com/2019/04/top-5-courses-to-learn-git-and-github.html)

图片属于 atlassian.com 的

Git Stash 是我经常使用的最喜欢的 Git 特性之一，它将工作目录的当前状态存储在一个堆栈中，您可以随时在任何分支上重新应用这个堆栈。它给你一个干净的工作目录，指向你的工作分支中最近的提交。

> 通常，当你一直在做项目的一部分时，事情处于一种混乱的状态，你想换个分支做点别的事情。问题是，你不想做半途而废的工作。这个问题的答案是`git stash`命令。
> —[git 藏匿的正式文件](https://git-scm.com/docs/git-stash)

# 那么，何时以及如何使用 git stash 呢？

现在我们已经定义了 git stash 的作用，让我们看看一些常见的使用情况，git stash 在这些情况下非常有用。

## 在分支之间或任务之间切换

你正在处理一项任务，这时另一项任务出现了，需要你的注意力，你不得不停止当前的工作。到目前为止，您对工作目录所做的所有修改对于新任务来说都是多余的，所以您不想提交它们。

在指向您的工作目录的终端中，运行命令

```
> git stash
```

这将为您的工作副本创建一个快照，并建立一个干净的工作目录，指向您当前分支的最近提交。从这里，你可以开始写下一个故事。

当您完成了这个新任务，将修改提交并推送到远程存储库，并希望返回继续处理之前的任务时，您需要-

```
> git stash pop
```

这将把前一个任务中尚未完成的修改恢复到您的工作目录中，允许您继续工作。您不必回到您执行的分支，并且可以`pop`在更新的分支或者任何您想要用作您的变更的基础的分支上存储。

就像这样，您可以在不同的任务之间转换，而不必先撤消再重做更改。

## 获取最新的远程更改

另一个用例是，如果您一直在处理一个文件，并且想要获取其他人对同一个文件所做的修改。如果你同事的输入修改在文件的不同行上，一个`git pull`会添加修改。然而，如果在同一行上有您可能已经更改的输入修改，那么`git pull`将不会直接工作。

[Git](/javarevisited/11-best-online-places-to-learn-git-for-beginners-in-2021-6dc2b7c6ef48) 会投诉`error: Your local changes to the following files would be overwritten by merge:`。别担心，`git stash`来救援了！

在终点站，做-

```
> git stash> git pull> git stash pop** *Resolve the conflicts* **
```

如果你执行了一个`git pull`并且由于冲突而失败了，那么`git stash`你的工作目录。`git pull`现在允许你毫无问题地从远程引入修改。

一旦你同事的修改在你的工作区，一个`git stash pop`将恢复你的修改到你的工作目录。当然，`git stash pop`不会因为存在冲突而简单地放置您的更改，它会提示您手动解决 git 无法自动解决的冲突。

## 保留个性化或无关的修改以备后用

我使用`git stash`的一个用例是保存我的个性化更改，特别是需要用本地环境值修改的配置文件，以便在本地运行应用程序。

为了清楚起见，让我们将一个文件`application.properties`可视化，我们将它个性化为引用我们的本地数据库来连接应用程序，同时添加与任务相关的新属性。

我们希望提交新添加的更改，而不提交我们的个性化更改。我们也不想为了提交新的变更而放弃我们的个性化变更。再一次，`git stash`！

这里我们要做的不是将所有的更改都存储在所有的文件中，而是只存储那些包含我们个性化更改的文件。对于这些个性化更改的任何未来使用，我们可以从存储中恢复它们。

```
> git add application.properties> git stash -k -m "My app properties" -- application.properties*** Edit file to remove your personalized changed ***> git commit -m "Added new properties"> git stash apply
```

我们在这里做的是首先用`git add`暂存文件。然后用`-k`(T3 的简写)选项`git stash`保存文件，同时在我们的工作目录中保留更改，因为我们想要提交新添加的修改。在提交新的更改之前，我们需要编辑文件以删除我们的个性化更改。`-m`为隐藏的条目提供了一个有意义的名称，通过这个名称，我们可以在任何时候使用我们的个性化更改时再次引用它。在这种情况下，我们用`git stash apply`代替`git stash pop`。当两者都将隐藏的更改恢复到工作目录时，`pop`从 stash stack 中删除条目，而`apply`保留 stash 条目，只要我们决定将它保留在那里，我们就可以在将来引用它。

## 在多次提交下拆分更改

您可能同时在同一个文件中处理两个或多个任务，但是您希望对每个任务执行单独的提交。假设你正在做`Tasks A`和`Task B` -

```
> git stash push -k*** Edit files, remove changes of Task B, keep changes of Task A. ***> git add <files>> git commit -m "Task A" > git stash pop> git commit -m "Task B"
```

最初，您隐藏所有的修改，同时使用`-k`选项保持工作目录中的更改不变，如前面的用例中所述。然后编辑两个任务之间的共享文件，删除对`Task B`的修改。将保留对`Task A`的修改。在根据`Task A`提交修改之前，对其进行测试。在提交了`Task A`的变更后，您将恢复隐藏的条目——它们包括`Task B`和`Task A`的变更。但是，由于工作目录已经包含了`Task A`的变更，`git stash pop`将自动合并它们。如果您在提交`Task A`之前对公共文件进行了更多的修改，将会产生冲突，您必须手动解决。工作目录中未提交的更改来自`Task B`。您应该在根据`Task B`提交之前测试它们。

对于用例#3 和#4，您可能希望将您的更改拆分成一个小的补丁，其中只包含您要提交的任务的修改，而不是将所有更改隐藏到已修改的文件中，然后在提交之前编辑它们以删除不需要的更改。包含您要提交的更改的修补程序将被存放，而您不想提交的其余修补程序将保留在未存放部分。这是从一个大的变更中生成几个提交的一种更加微妙而优雅的方式。对此有两种方法。

1.  最简单的方法是使用`git add --patch`命令。然而，这可能很难理解。你可以参考这个视频 [Git 教程——添加和编辑提交(Hunks)](https://www.youtube.com/watch?v=_AH3bzMUsbg) 来看看实现这个的实用方法。
2.  如果你使用 IntelliJ IDEA 作为你的 IDE，你可以创建一个叫做 **Shelve** 的东西，它相当于上面步骤中的`git stash`或`git add --patch`。使用 [IntelliJ IDEA](/javarevisited/7-best-courses-to-learn-intellij-idea-for-beginners-and-experienced-java-programmers-2e9aa9bb0c05) 的交互式编辑器，这是掌握和完成的更简单的方法。您可以阅读 IntelliJ 文档[搁置更改](https://www.jetbrains.com/help/idea/2021.3/shelving-and-unshelving-changes.html)来了解更多，我将在另一篇文章中演示。

它包装了 git stash 用例。如果你已经读到这里，我希望它对你有所帮助。

在我的下一篇文章中，作为这方面的延续，我将介绍`git stash`命令的变体和许多选项，在使用 git stash 时，您可以使用它们来提高生产率。

让我知道你是如何使用 git stash 的，如果你遇到了它派上用场的任何其他用例。

## **附加参考文献**

[git 藏匿的官方文件](https://git-scm.com/docs/git-stash)