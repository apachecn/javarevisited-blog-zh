# Hashcode 等于调试，性能

> 原文：<https://medium.com/javarevisited/hashcode-and-equals-debugging-performance-a4763d02f0ce?source=collection_archive---------1----------------------->

![](img/6a1251c0f6f54406ae5ec020fbe3f17b.png)

几周前，我在 Reddit 上看到了这个故事，讨论了在地图中使用 URL 类作为关键字的问题。这归结为 [java.net.URL](http://java.net.URL) 中 hashcode()方法的实现非常慢，这使得这个类在这种情况下无法使用。不幸的是，这是 Java API 规范的一部分，在不破坏向后兼容性的情况下不再能够修复。

我们能做的是理解[等于](https://www.java67.com/2012/11/difference-between-operator-and-equals-method-in.html)和[哈希码](https://javarevisited.blogspot.com/2013/08/10-equals-and-hashcode-interview.html#axzz7D1K8JL8x)的问题。以后怎么才能避免这样的问题？

# URLs Hashcode 和 Equals 有什么问题？

为了理解这一点，让我们来看看 hashcode 和 equals 的 JavaDoc:

> *比较此 URL 与另一个对象是否相等。*
> 
> *如果给定的对象不是 URL，那么这个方法立即返回 false。*
> 
> *两个 URL 对象如果有相同的协议，* ***引用等价主机*** *，在主机上有相同的端口号，以及相同的文件和文件的片段，则是相等的。*
> 
> ***如果两台主机的名称都可以解析成相同的 IP 地址，那么这两台主机就被认为是等价的；*** *否则如果任一主机名无法解析，则主机名必须相等，不考虑大小写；或者两个主机名都等于空。*
> 
> *因为主机比较需要名称解析，所以该操作是阻塞操作。*

因为主机比较需要名称解析，所以此操作是一个阻塞操作。"

这可能不清楚。让我们用一个简单的代码块来阐明它:

```
System.out.println(new URL("http://localhost/").equals(new URL("http://127.0.0.1/")));
System.out.println(new URL("http://localhost/").hashCode() == new URL("http://127.0.0.1/").hashCode());
```

将打印出:

```
true
true
```

对于 localhost 来说，这可能很简单，但是如果我们比较域并且字符串不相同(通常不相同)，我们就需要进行 DNS 查找。我们需要做的只是一个 hashcode()调用！

# 快速解决方法

对于这种情况，一个快速的解决方法是避免使用 URL。Sun 将该类深深嵌入到原始 JVM 代码中，但是我们可以使用 URI 来实现大多数目的。

例如，如果我们将上面的 hashcode 和 equal 调用改为使用 URI 而不是 URL，我们将得到以下结果:

```
System.out.println(new URI("http://localhost/").equals(new URI("http://127.0.0.1/")));
System.out.println(new URI("http://localhost/").hashCode() == new URI("http://127.0.0.1/").hashCode());
```

这两个语句都将为假。虽然对于某些用例来说这可能会有问题，但这在性能上是一个巨大的差异。

# 一个更大的陷阱

如果我们只使用字符串作为映射键，那就没问题了。在我们使用这些方法的每一个地方，这种错误都会袭击我们:

*   设置
*   地图
*   储存；储备
*   业务逻辑

但是会越来越深。当用我们自己的 hashcode 和 equals 逻辑编写我们自己的类时，我们经常会成为糟糕代码的牺牲品。hashcode 方法或过于简单的版本上的一个小的性能损失会导致很难跟踪的大的性能损失。

例如，因为 hashcode 方法慢或不正确而花费更长时间的流操作可能代表长期问题。

# 最好的 Hashcode 实现

要理解最好的 hashcode 和 equals 方法，我们首先需要理解一些平庸的代码。现在我不会展示可怕的或旧的代码。这是很好的代码，但不是最好的:

```
public int hashCode() {
    return Objects.hash(id, core, setting, values, sets);
}
```

这段代码乍一看似乎还不错，但真的是这样吗？以下是理想的代码:

```
public int hashCode() {
    return id;
}
```

这是快速的，100%唯一和正确的。实际上没有理由做任何超出这一范围的事情。id 有一个例外，它是一个对象。在这种情况下，我们可能希望用 Objects.hashCode(id)代替，这也适用于 null，等等。

# Hashcode 不等于

显然，这是你在编写 hashcode 实现时需要记住的最重要的事情之一。该方法必须快速执行，并且对于假情况必须与 equals 一致。这对于 true 的情况是不正确的。

澄清一下，hashcode 必须始终遵守这条法则:

```
assert(obj1.hashCode() != obj2.hashCode() && !obj1.equals(obj2));
```

这意味着如果 hashcode 结果不同，那么对象也必须不同，并且必须从 equals 返回 false。但反过来却不是这样:

```
if(obj1.hashCode() == obj2.hashCode()) {
    if(obj1.equals(obj2)) {
       // this can be false...
    }
}
```

这里的价值在于性能。hashcode 方法的执行速度应该比 equals 快得多。它应该让我们快速跳过潜在的昂贵的 equals 计算和索引元素。

# JPA 的特殊情况

JPA 开发人员通常只使用 hashcode 的硬编码值，或者使用 Class 对象来生成 hashCode()。这似乎很奇怪，直到你想到这一点。

如果让数据库为您生成 ID，您将保存一个对象，它将不再等于源对象…一个解决方案是使用`@NaturalId`注释和数据类型。但是这需要改变数据模型。不幸的是，对于实体类没有合适的解决方法。

事实上，我认为 JPA 开发人员在使用 Lombok 时遇到的很多问题都是因为它为您生成了 hashcode 和 equals 方法。这些可能会有问题。

# 这是一个关于调试的博客吗？

很抱歉这么长时间，但确实是这样。所以我需要所有的序言，从更一般的调试角度来讨论这个问题。请注意，对于其他使用类似公共接口范例的语言来说也是如此。

这篇博客从一个性能问题开始，我想从调试的角度来讨论这个问题。在许多分析器中，hashcode 方法的开销几乎不会被注意到。但是因为它被如此频繁地引用，并且有着广泛的影响，所以你最终可能会感受到它的影响，并把责任推给其他人。

膝跳反应将是实现一个“dummy”[hashcode 方法](https://javarevisited.blogspot.com/2011/02/how-to-write-equals-method-in-java.html)，并看到由此产生的性能差异。只是返回一个硬编码的数字，而不是一个有效的数字。

这在某些情况下是有价值的，甚至可以解决上面提到的 hashcode 方法表现不佳的问题。但是，它对地图没有帮助。如果 hashcode 将返回相同的值，那么将它用作 map 中的一个键实际上会使 hashcode 所能提供的所有性能优势失效。

我们如何知道一个 hashcode 方法是好的呢？

嗯……我们可以用调试器来解决这个问题。只需检查一下您的地图，看看对象在不同存储桶之间的分布，就能感受到 hashcode 方法的真实价值。

如果你有一个关于提交的代码验证过程，我强烈建议你在 hashcode 方法的复杂程度上定义一个规则。这应该设置得非常低，以防止缓慢的代码渗入。

但问题是筑巢。例如，想想我们之前讨论过的代码:

```
public int hashCode() {
    return Objects.hash(id, core, setting, values, sets);
}
```

它又短又简单。然而，这段代码的性能可以无处不在。该方法将调用所有内部对象的 hashcode 方法。这些方法在性能方面要差得多。我们需要对此保持警惕。即使对于 JDK 类，如 URL，正如我们前面所讨论的，这是有问题的。

# TL；速度三角形定位法(dead reckoning)

我们经常自动生成 hashcode 和 equals 方法。ide 通常在这方面做得很好；它们为我们提供了选择想要比较的字段的选项。不幸的是，他们随后将这两组字段应用于 hashcode 和 equals。

有时候，这并不重要。通常我们不会“看到”重要的地方，因为这些方法太小了，不足以影响分析器。但是它们有着广泛的影响，我们应该为之进行优化。

调试让我们可以检查映射并查看存储桶分布，这样我们就可以了解 hashcode 方法的执行情况，以及我们是否应该调整它以从映射和类似的 API 中获得更一致的结果。